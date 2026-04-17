# JWAY Paperclip → Next.js 마이그레이션 계획

## 🎯 마이그레이션 동기

### 현재 한계
- **단일 HTML 파일** (2,900+ 줄) — 유지보수 어려움
- **localStorage 의존** — 여러 기기에서 공유 불가, 데이터 손실 위험
- **클라이언트에서 API 키 노출** — 배포 시 보안 이슈
- **브라우저 Rate Limit** — 서버 프록시 없이 분산 불가
- **단일 사용자** — 멀티 유저/팀 협업 불가

### Next.js로 가면 얻는 것
- ✅ 컴포넌트 분리 → 재사용/테스트 가능
- ✅ API 라우트 → 서버에서 Claude API 호출 (키 보호)
- ✅ 데이터베이스 → 영속적 저장, 백업
- ✅ 인증 시스템 → 멀티 유저, 팀 관리
- ✅ 실시간 업데이트 → WebSocket/SSE로 협업 진행 상황 스트리밍
- ✅ 배포 용이 → Vercel 원클릭 배포

---

## 📋 Phase 1: 기초 인프라 (1주)

### 1.1 프로젝트 세팅
```bash
npx create-next-app@latest jway-paperclip-next --typescript --tailwind --app
cd jway-paperclip-next
npm install @anthropic-ai/sdk zustand @prisma/client prisma
npm install -D @types/node
```

### 1.2 디렉토리 구조
```
jway-paperclip-next/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── signup/page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx                # SidebarNav
│   │   ├── page.tsx                  # Dashboard
│   │   ├── agents/page.tsx
│   │   ├── issues/page.tsx
│   │   ├── heartbeat/page.tsx
│   │   ├── conversations/page.tsx
│   │   ├── goals/page.tsx
│   │   └── costs/page.tsx
│   └── api/
│       ├── heartbeat/route.ts        # POST: 에이전트 실행
│       ├── collaboration/route.ts    # POST: 협업 루프 (SSE 스트림)
│       ├── agents/route.ts
│       └── issues/route.ts
├── components/
│   ├── AgentCard.tsx
│   ├── KanbanBoard.tsx
│   ├── ConversationThread.tsx
│   ├── HeartbeatControls.tsx
│   └── ...
├── lib/
│   ├── claude.ts                     # API 호출 래퍼
│   ├── harness.ts                    # validateActions, parseAgentResponse
│   ├── collaboration.ts              # runCollaborationHeartbeat
│   ├── prompts.ts                    # buildAgentPrompt
│   └── db.ts                         # Prisma client
├── store/
│   ├── useAgentStore.ts              # Zustand 스토어
│   └── useIssueStore.ts
└── prisma/
    └── schema.prisma
```

### 1.3 데이터베이스 스키마 (Prisma)
```prisma
model User {
  id          String    @id @default(cuid())
  email       String    @unique
  companies   Company[]
  createdAt   DateTime  @default(now())
}

model Company {
  id          String    @id @default(cuid())
  userId      String
  name        String
  agents      Agent[]
  issues      Issue[]
  goals       Goal[]
  // ...
}

model Agent {
  id                 String    @id @default(cuid())
  companyId          String
  name               String
  role               String
  model              String
  advisorEnabled     Boolean   @default(false)
  webSearchEnabled   Boolean   @default(false)
  reportsToId        String?
  reportsTo          Agent?    @relation("AgentHierarchy", fields: [reportsToId], references: [id])
  subordinates       Agent[]   @relation("AgentHierarchy")
  issues             Issue[]
  // ...
}

model Issue {
  id            String      @id @default(cuid())
  identifier    String
  title         String
  description   String?
  status        IssueStatus @default(TODO)
  comments      Comment[]
  goalId        String?
  goal          Goal?       @relation(fields: [goalId], references: [id])
  // ...
}

model Comment {
  id        String   @id @default(cuid())
  issueId   String
  agentId   String
  content   String   @db.Text
  type      String   @default("comment")
  createdAt DateTime @default(now())
}

model HeartbeatRun {
  id           String   @id @default(cuid())
  agentId      String?
  mode         String   // "single" | "collaboration"
  status       String
  costCents    Int
  inputTokens  Int?
  outputTokens Int?
  summary      String?  @db.Text
  createdAt    DateTime @default(now())
}
```

---

## 📋 Phase 2: 핵심 로직 포팅 (1~2주)

### 2.1 라이브러리 분리 (`lib/`)
현재 index.html의 함수들을 TypeScript 모듈로 분리:

| 원본 함수 | 이동 위치 |
|-----------|-----------|
| `callClaudeAPI` | `lib/claude.ts` |
| `calcCostCents`, `extractAdvisorUsage` | `lib/claude.ts` |
| `buildAgentPrompt`, `getTeamContext` | `lib/prompts.ts` |
| `parseAgentResponse`, `validateActions` | `lib/harness.ts` |
| `executeHeartbeat` | `app/api/heartbeat/route.ts` |
| `runCollaborationHeartbeat` | `app/api/collaboration/route.ts` (SSE) |
| `recalcGoalProgress` | `lib/goals.ts` |

### 2.2 API 라우트 (서버사이드)
```typescript
// app/api/heartbeat/route.ts
import { NextRequest } from 'next/server';
import Anthropic from '@anthropic-ai/sdk';

export async function POST(req: NextRequest) {
  const { agentId } = await req.json();
  const apiKey = process.env.ANTHROPIC_API_KEY!; // 서버에서만!
  
  // 1. DB에서 agent, issues, goals 로드
  // 2. buildAgentPrompt
  // 3. Anthropic SDK 호출 (tools: advisor + web_search)
  // 4. 하네스 검증 + 액션 적용
  // 5. DB 업데이트
  // 6. 결과 반환
}
```

### 2.3 협업 SSE 스트리밍
```typescript
// app/api/collaboration/route.ts
export async function POST(req: NextRequest) {
  const encoder = new TextEncoder();
  const stream = new ReadableStream({
    async start(controller) {
      const send = (event: string, data: any) => {
        controller.enqueue(encoder.encode(
          `event: ${event}\ndata: ${JSON.stringify(data)}\n\n`
        ));
      };
      
      // 매니저 → 실무자 순서로 실행
      for (const agent of executionOrder) {
        send('progress', { agent: agent.name, phase: 'starting' });
        const result = await executeHeartbeat(agent);
        send('progress', { agent: agent.name, phase: 'done', result });
      }
      send('complete', { summary });
      controller.close();
    },
  });
  return new Response(stream, {
    headers: { 'Content-Type': 'text/event-stream' },
  });
}
```

### 2.4 클라이언트에서 스트림 수신
```typescript
// components/CollaborationRunner.tsx
const startCollab = async () => {
  const res = await fetch('/api/collaboration', { method: 'POST' });
  const reader = res.body!.getReader();
  const decoder = new TextDecoder();
  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    const events = decoder.decode(value).split('\n\n');
    // 각 이벤트 파싱해서 UI 업데이트
  }
};
```

---

## 📋 Phase 3: UI 컴포넌트 분리 (1주)

현재 단일 파일의 각 Page 함수를 Next.js 페이지로 이동하면서 컴포넌트 분리:

```tsx
// app/(dashboard)/agents/page.tsx
import { AgentGrid } from '@/components/AgentGrid';
import { AgentForm } from '@/components/AgentForm';

export default async function AgentsPage() {
  const agents = await prisma.agent.findMany();
  return (
    <div>
      <AgentForm />
      <AgentGrid agents={agents} />
    </div>
  );
}
```

### 상태 관리 (Zustand)
```typescript
// store/useAgentStore.ts
import { create } from 'zustand';

export const useAgentStore = create((set) => ({
  agents: [],
  setAgents: (agents) => set({ agents }),
  updateAgent: (id, data) => set((state) => ({
    agents: state.agents.map(a => a.id === id ? {...a, ...data} : a)
  })),
}));
```

---

## 📋 Phase 4: 인증 + 배포 (3~5일)

### 4.1 인증
- **NextAuth.js** 또는 **Clerk** 통합
- OAuth (Google, GitHub)
- 사용자별 데이터 격리

### 4.2 배포 (Vercel)
```bash
vercel deploy
```

**환경 변수:**
```
ANTHROPIC_API_KEY=sk-ant-...
DATABASE_URL=postgresql://...
NEXTAUTH_SECRET=...
```

### 4.3 데이터베이스 (PostgreSQL)
- **Vercel Postgres** 또는 **Supabase** (무료 티어 충분)
- Prisma migrate로 스키마 배포

---

## 📋 Phase 5: 추가 기능 (선택, 2~3주)

### 5.1 팀 기능
- 사용자가 여러 명일 때 한 회사에 속하도록
- 역할 기반 권한 (Owner, Admin, Member)

### 5.2 실시간 협업
- Pusher/Ably 통합
- 다른 사용자의 이슈 변경 실시간 반영

### 5.3 Webhook
- Slack/Discord 연동 (협업 완료 시 알림)
- Notion DB 동기화

### 5.4 모바일 앱
- React Native + Expo로 재활용
- 주요 페이지 공유

---

## 🗓️ 전체 타임라인

| 주차 | 작업 |
|------|------|
| 1주차 | Phase 1 (세팅 + DB 스키마) |
| 2~3주차 | Phase 2 (로직 포팅) |
| 4주차 | Phase 3 (UI 분리) |
| 5주차 | Phase 4 (인증 + 배포) |
| 6~8주차 | Phase 5 (선택 기능) |

**예상 총 기간: 5~8주**

---

## ⚠️ 마이그레이션 시 주의사항

### 데이터 마이그레이션
1. 현재 localStorage 데이터를 JSON으로 export (설정 페이지의 기존 기능)
2. Next.js에서 JSON → DB import 스크립트 작성
3. 기존 사용자에게 import 경로 안내

### 호환성 유지
- 단일 HTML 버전은 `legacy/` 브랜치로 남겨두기
- Next.js 버전이 안정화될 때까지 양쪽 유지

### 비용
- **Vercel Hobby**: 무료 (개인/소규모)
- **Vercel Postgres**: 무료 티어 256MB
- **Anthropic API**: 사용량 기반 (현재와 동일)

---

## 🚀 시작 신호

준비되면 다음 명령으로 시작:

```bash
cd /c/Users/gogon/Desktop/ryu_ws
npx create-next-app@latest jway-paperclip-next --typescript --tailwind --app
```

이 플랜은 **선택적**입니다. 현재 단일 HTML 버전도 소규모 개인 사용에는 충분합니다. Next.js로 가는 건 **다음 중 하나라도 필요할 때**:
- 여러 사람이 같이 쓸 때
- 공개 서비스로 만들 때
- 모바일 접근이 필요할 때
- 데이터가 중요해서 서버 백업 필요할 때
