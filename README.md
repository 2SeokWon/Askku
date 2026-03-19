# ASKku - 성균관대학교 AI 캠퍼스 도우미

성균관대학교 학생을 위한 AI 기반 캠퍼스 정보 통합 서비스입니다.
RAG(Retrieval-Augmented Generation) 기술을 활용해 학교 공지사항, 학사일정, 건물 정보 등을 학습하고, 학생의 개인 시간표/일정을 반영한 맞춤형 답변을 제공합니다.

> 2025년 2학기 팀 프로젝트 (Team 1)

---

## 주요 기능

### AI 챗봇
- RAG 기반 질의응답 (학교 공지사항, 학사일정, 건물 정보 등)
- **실시간 스트리밍 응답** (SSE 방식, 타이핑 효과)
- **일정 추출** — AI 답변에서 일정 추출 버튼을 눌러 날짜/일정을 캘린더에 추가
- **번역** — AI 응답을 영어로 번역
- **출처 표시** — 답변에 사용된 참고 문서 및 링크 확인
- **북마크** — 중요한 Q&A를 저장하고 사이드바에서 관리
- 사용자 시간표 및 캘린더를 AI 컨텍스트로 반영

### 공지사항
- 성균관대, 소프트웨어학과, 소프트웨어융합대학, 기숙사 공지 통합 조회
- 페이지네이션 지원

### 마이페이지
- 시간표 관리 (강의 추가/삭제)
- 월별 캘린더 뷰 (개인/학사/강의 일정 필터링)
- 일정 추가/수정/삭제
- 개인정보 및 챗봇 설정 수정

### 인증
- 이메일/비밀번호 회원가입 및 로그인
- JWT 기반 세션 관리
- Protected Route로 인증된 사용자만 접근

---

## 기술 스택

| 영역 | 기술 |
|------|------|
| Frontend | React 18, TypeScript, Vite, TailwindCSS |
| Backend | Node.js, Express 5, Sequelize, MySQL |
| AI / RAG | Python, FastAPI, LangChain, ChromaDB, OpenAI API |
| 인증 | JWT, bcrypt |
| 마크다운 렌더링 | react-markdown, remark-gfm, rehype-sanitize |
| XSS 방어 | DOMPurify |

---

## 시스템 아키텍처

```
React (port 5173)
      │
      ▼
Node.js API (port 4000)  ──── MySQL DB
      │
      ▼
FastAPI RAG Server (port 8001)
      │
      ▼
ChromaDB (벡터 DB) + OpenAI API
```

---

## 프로젝트 구조

```
Askku/
├── web/              # React 프론트엔드
│   └── src/
│       ├── pages/    # 페이지 컴포넌트 (Chat, Home, MyPage, Auth)
│       ├── components/  # 재사용 컴포넌트
│       ├── services/ # API 통신 및 비즈니스 로직
│       ├── types/    # TypeScript 타입 정의
│       └── contexts/ # React Context (UserContext)
├── backend/          # Node.js 백엔드 + Python RAG
│   └── src/
│       ├── routes/   # API 라우트
│       ├── controllers/
│       ├── models/   # Sequelize 모델
│       └── rag/      # Python RAG 엔진 (FastAPI + LangChain)
```

---

## 로컬 실행 방법

### 사전 요구사항
- Node.js >= 22.10.0
- Python >= 3.10
- MySQL 8+
- OpenAI API Key

### 1. 환경변수 설정

`backend/.env` 파일 생성:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=askku
JWT_SECRET=your_jwt_secret
OPENAI_API_KEY=your_openai_api_key
```

### 2. 데이터베이스 설정

```bash
mysql -u root -p -e "CREATE DATABASE askku;"
```

### 3. 백엔드 실행

```bash
cd backend
npm install
npm run dev
```

### 4. Python RAG 서버 실행

```bash
cd backend/src/rag
pip install -r requirements.txt
python rag_api.py
```

### 5. 지식 베이스 구축 (최초 1회)

```bash
cd backend/src/rag
python ingest.py
```

### 6. 프론트엔드 실행

```bash
cd web
npm install
npm run dev
```

브라우저에서 `http://localhost:5173` 접속

---

## 나의 기여 (Frontend)

### 챗봇 UI 및 기능
- `Fetch API + ReadableStream`을 활용한 SSE 스트리밍 응답 처리 구현
- AI 답변에서 일정 추출 버튼으로 날짜/일정 파싱 후 캘린더 저장 플로우 구현 (`ScheduleSelectionModal`)
- AI 답변 북마크 기능 — DB 저장, 세션 동기화, 사이드바 관리 (`BookmarkSidebar`)
- AI 응답 번역 기능 (스트리밍 방식 동일 적용)
- RAG 출처 문서 표시 (참고 링크 포함)
- `react-markdown` + DOMPurify를 활용한 마크다운 렌더링 및 XSS 방어

### 서비스 레이어
- 채팅 세션 관리 (localStorage 기반, 대화 히스토리 10개 유지)
- `chatService.ts` — 스트리밍 파싱(SSE), 북마크 CRUD, 일정 추출 API 연동
- 사용자 시간표/캘린더를 AI 요청 컨텍스트로 자동 포함

### 기타
- 인증 플로우 및 `ProtectedRoute` 구현
- 월별 캘린더 뷰, 시간표 뷰 컴포넌트
- 공지사항 조회 및 페이지네이션
- React Context 기반 전역 유저 상태 관리

---

## 팀 구성

| 역할 | 담당 |
|------|------|
| Frontend | 이동환, 이석원 |
| Backend | 강철석, 김태완 |
| AI / RAG | 김건, 오은서 |
