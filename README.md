# Donggri Ledger

브라우저에서 바로 쓰는 개인 가계부 서비스입니다. 현재 구조는 `Vercel + Supabase + GitHub` 기준으로 정리되어 있고, 아이디/비밀번호 로그인, 보안질문 기반 비밀번호 재설정, 카드 결제예정 관리, 투자 포트폴리오, 백업/복원까지 한 화면 흐름으로 이어집니다.

- Repository: https://github.com/sheryloe/donggri_gagyeobu
- GitHub Pages: https://sheryloe.github.io/donggri_gagyeobu/
- Build Story Hub: https://www.notion.so/32461b8ed53c8008bbfffe194db4bf5e?source=copy_link

## Current Product Snapshot

- Frontend: static web app deployed with Vercel
- Auth / DB: Supabase Auth + Postgres + RLS
- Automation: Supabase Edge Functions
- Docs / SEO landing: GitHub Pages
- User model: small private service with a 50-user signup cap

## Current Features

### 1. Account Access

- 아이디 + 비밀번호 로그인
- 보안질문 3개 기반 비밀번호 재설정
- 회원가입 50명 제한
- 사용자별 데이터 분리와 RLS 적용

### 2. Ledger Core

- 수입 / 지출 / 투자 거래 입력
- 월별 합계, 전월이월, 현금기준 잔액 요약
- 거래 검색
- 달력 뷰
- 카테고리 직접 관리

### 3. Asset and Card Flow

- 은행 / 현금 / 카드 / 투자계좌 자산 구분
- 카드별 결제일과 결제통장 설정
- 카드 사용 시 실제 출금일 기준 결제예정 금액 추적
- 자산 삭제 시 연결 데이터 검사와 강제 삭제 처리

### 4. Fixed Expenses and Budgets

- 고정지출 등록
- 카테고리별 예산 설정
- 미지출 고정비와 예산 사용률 확인

### 5. Investments

- 주식 / 코인 / ETF / 펀드 관리
- 실시간 시세 새로고침
- 펀드 현재가 수동 입력
- 투자 원금, 평가금, 손익, 수익률 집계

### 6. Operations

- JSON 백업 / 복원
- GitHub 기반 버전 관리
- Vercel 정적 배포
- GitHub Pages용 소개 페이지 분리

## Why This Architecture

이 프로젝트는 원래 로컬 FastAPI + SQLite 구조였지만, 외부 접속과 유지보수 부담 때문에 현재는 서비스형 구조로 정리했습니다.

- `GitHub`
  - 코드 이력 관리
  - README / 문서 / 협업 기준점
- `Vercel`
  - 프론트엔드 정적 배포
  - 빠른 재배포
- `Supabase`
  - 사용자 인증
  - 사용자별 데이터 저장
  - RLS 기반 접근 제어
  - Edge Function 기반 보조 로직

이 조합으로 바꾸면서 “집 PC를 켜둬야 하는 로컬 서버”가 아니라 “배포된 웹 서비스처럼 쓰는 개인 가계부”로 방향을 바꿨습니다.

## Recommended Next Features

아래는 지금 코드와 운영 방식 기준으로 추천하는 다음 우선순위입니다.

### Priority 1

- 반복 수입 자동 생성
- 계좌 간 이체 전용 거래 타입
- 월마감 스냅샷과 순자산 추이 저장

### Priority 2

- 카드 결제일 / 고정지출 / 예산 초과 알림 센터
- 투자 시세 갱신 이력과 수익률 추세 차트
- 월별 리포트 다운로드 PDF

### Priority 3

- CSV / Excel 가져오기
- 관리자용 가입 현황 대시보드
- README / GitHub Pages / 가입 인원 수 자동 동기화

### Priority 4

- 가족 / 커플용 공유 계정 모델
- 태그 기반 거래 분류
- 목표저축 / sinking fund 관리

## Screens and Tabs

현재 앱은 아래 화면들로 구성됩니다.

- `가계부`
- `달력`
- `고정지출`
- `예산`
- `리포트`
- `투자`
- `검색`
- `자산`
- `설정`

## Local Build for Web Output

이 저장소는 정적 웹 빌드 결과를 `dist/`에 만듭니다.

```bash
npm install
npm run build
```

검증:

```bash
node --check web/app.js
```

## Environment Variables

Vercel에는 아래 값이 필요합니다.

```text
SUPABASE_URL
SUPABASE_ANON_KEY
APP_NAME
```

주의:

- `SUPABASE_SERVICE_ROLE_KEY`는 브라우저 환경변수에 넣지 않습니다.
- 회원가입 / 비밀번호 찾기용 Edge Function은 로그인 전 호출이므로 JWT 설정을 따로 확인해야 합니다.

## Repository Structure

- `web/`
  - 실제 앱 UI와 프론트 로직
- `supabase/`
  - schema, RLS, Edge Function
- `docs/`
  - GitHub Pages 랜딩, SEO 파일, 스크린샷
- `scripts/`
  - 정적 빌드 스크립트
- `docs/SESSION_LOG.md`
  - 작업 세션 로그

## Build Story Outline

프로젝트 빌드 과정은 아래 5단계로 정리할 수 있습니다.

1. 로컬 FastAPI + SQLite MVP
2. 카드 결제예정 / 투자 / 운영 기능 확장
3. GitHub Pages 랜딩과 SEO 정리
4. Supabase + Vercel 서비스형 전환
5. 회원가입 / 보안질문 / 실시간 시세 안정화

## Notes

- 현재 README는 “지금 구현된 기능”과 “다음 추천 기능” 기준으로 계속 업데이트됩니다.
- GitHub Pages는 소개 / SEO 랜딩 역할, 실제 앱은 Vercel 배포 역할로 분리해 운영하는 방향입니다.
