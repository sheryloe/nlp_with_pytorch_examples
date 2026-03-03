# SESSION LOG

## Usage
- 다음 세션 시작 시 이 파일부터 읽는다.
- 최근 항목이 맨 아래에 추가된다.
- 경로, 커밋, 운영 상태를 빠르게 재개할 수 있도록 유지한다.

---

## 2026-03-03 22:10 (Asia/Seoul)

### User Requests
- 프로그램 종료/삭제 실패/DB 반영 실패 원인 점검
- 자산 삭제 안 되는 문제 해결
- `Transaction not found` 반복 문제 해결
- README 작성(빌드 방법 + 기능 명세), 커밋, Notion 업데이트
- 다음 세션용 대화 로그 유지

### Changes Applied
- DB 경로 단일화 및 레거시 복사 로직 반영
  - `app/database.py`
- 런처 포트 통일(8000)
  - `launcher.py`
- health에 `db_path` 노출
  - `app/main.py`
- API 에러 메시지(detail) 프론트 토스트 노출
  - `web/index.html`
- 거래 조회 limit 불일치(2000 vs 1000) 수정
  - `web/index.html`
- 자산 삭제 개선
  - 연결 데이터 존재 시 상세 메시지(건수+예시 거래)
  - 강제삭제 `force=true` 지원
  - 파일: `app/crud.py`, `app/routers/assets.py`, `web/index.html`
- 404(`Transaction not found`, `Asset not found`) 시 UI 자동 재동기화
  - `web/index.html`
- README 작성 및 `.gitignore` 추가
  - `README.md`, `.gitignore`

### Data / Runtime State
- 서버 기준 DB 경로:
  - `C:\Users\wlflq\AppData\Local\donggri-ledger\data\ledger.db`
- DB 초기화 수행(assets/transactions/fixed_expenses/investments/budgets 비움)
- categories 기본값 유지
- 점검 시점 기준 `/api/assets`, `/api/transactions` 모두 빈 배열 확인

### Git
- Repository initialized at: `d:\Gaebal\donggri-ledger`
- Latest commit:
  - `d1365993e08fc41d9673cf963c4308e640304709`
  - `docs: add README spec and persist ledger sync/delete fixes`

### Notion
- 새 페이지:
  - https://www.notion.so/31861b8ed53c81c9bc94ec2c2c2e2b29
  - 제목: `donggri-ledger 업데이트 로그 (2026-03-03)`
- Daily Status 페이지 코멘트 추가 완료

### Open Issues
- 기존 `dist\donggri-ledger\donggri-ledger.exe` 프로세스가 남아 있으면 파일 잠금으로 재빌드 실패 가능
- 잠금 시 `dist_hotfix` 출력 경로로 빌드해 우회 가능(README 참고)

### Next Start Checklist
1. `http://127.0.0.1:8000/health` 확인 (`db_path` 확인)
2. UI 강력 새로고침(`Ctrl+F5`) 후 테스트
3. 삭제/동기화 이슈 재현 시 브라우저 콘솔 에러 + API 응답 detail 확인
