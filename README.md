# Donggri Ledger

`Donggri Ledger`는 카드 결제 예정 금액, 예산, 자산, 투자 흐름을 한 화면에서 관리하는 개인 재무 서비스입니다.
현재는 `Supabase + Vercel + GitHub` 조합으로 서비스형 구조를 갖추는 방향으로 정리하고 있습니다.

- 앱: `https://donggri-gagyeobu.vercel.app/`
- GitHub Pages: `https://sheryloe.github.io/donggri_gagyeobu/`
- 저장소: `https://github.com/sheryloe/donggri_gagyeobu`
- 위키: `https://github.com/sheryloe/donggri_gagyeobu/wiki`

## 서비스 개요

- 자산 요약, 카드 결제 예정, 예산, 고정지출, 투자 포트폴리오를 함께 관리합니다.
- Supabase Auth와 RLS를 사용해 사용자별 데이터를 분리합니다.
- 백업/복원, 문서, GitHub Pages를 포함한 운영형 서비스를 지향합니다.

## 핵심 기능

- 계정 가입, 로그인, 보안 질문 기반 복구
- 수입/지출/투자 거래 기록
- 카드별 결제 예정 및 정산 흐름 관리
- 예산 및 고정지출 관리
- 주식/코인/ETF 투자 추적
- JSON 백업/복원

## 현재 기술 구성

- Frontend: JavaScript
- Deploy: Vercel
- Auth/DB: Supabase Auth + Postgres
- Security: RLS
- Automation: Supabase Edge Functions

## 최근 빌드 포인트

- 해외 주식/ETF를 포함한 다중 통화 투자 구조 반영
- 검색 기반 투자 추가 흐름 개선
- GitHub Pages와 wiki 연동 정리

## 실행 힌트

프로젝트 루트 기준으로 패키지 설치 후 프런트엔드를 실행하고, 필요한 경우 Python 의존성도 함께 준비합니다.

```bash
npm install
```

```bash
pip install -r requirements.txt
```

## 다음 단계

- 카드/예산/시세 실패 알림 센터 추가
- RLS 검증과 Edge Function 테스트 자동화
- 가족/공동 사용 모드 검토
