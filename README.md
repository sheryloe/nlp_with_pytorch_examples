# Donggri Ledger

로컬 PC에서 실행하는 개인 가계부/자산 관리 웹앱입니다.
FastAPI 백엔드 + 정적 HTML/JS 프론트엔드 + SQLite(DB) 구조로 동작합니다.

## 1. 기술 스택

- Python 3.12
- FastAPI
- SQLAlchemy
- Uvicorn
- SQLite
- 단일 페이지 UI (`web/index.html`)

## 2. 로컬 실행

### 2.1 의존성 설치

```powershell
py -m pip install -r requirements.txt
```

### 2.2 서버 실행

```powershell
py launcher.py
```

접속 주소:

- PC: `http://127.0.0.1:8000/ui/`
- 헬스체크: `http://127.0.0.1:8000/health`

`/health` 응답에는 현재 서버가 사용하는 DB 경로(`db_path`)가 포함됩니다.

## 3. DB 경로/정책

기본 DB 파일:

- `%LOCALAPPDATA%\\donggri-ledger\\data\\ledger.db`

개발/EXE 실행 모두 기본 경로를 동일하게 사용합니다.
필요 시 환경변수로 경로를 강제 지정할 수 있습니다.

- `DONGGRI_LEDGER_DATA_DIR`

## 4. EXE 빌드 방법

### 4.1 표준 빌드

```powershell
py -m PyInstaller -y donggri-ledger.spec
```

산출물:

- `dist\\donggri-ledger\\donggri-ledger.exe`

### 4.2 기존 EXE가 실행 중이라 dist 잠김일 때

```powershell
py -m PyInstaller -y --distpath dist_hotfix --workpath build_hotfix donggri-ledger.spec
```

산출물:

- `dist_hotfix\\donggri-ledger\\donggri-ledger.exe`

## 5. 기능 명세

### 5.1 공통

- 월 단위 가계부 집계/조회
- 자산(은행/현금/카드/투자계좌) 관리
- 카테고리 관리(수입/지출/투자/고정)
- 로컬 네트워크(동일 Wi-Fi)에서 모바일 접속 가능

### 5.2 자산

- 자산 생성/조회/수정/삭제
- 자산 삭제 시 연결 데이터(거래/고정지출/투자) 존재하면 삭제 차단
- `force=true`로 연결 데이터까지 포함해 강제 삭제 가능

### 5.3 거래(수입/지출/투자)

- 거래 생성/목록 조회/수정/삭제
- 자산 잔액 자동 반영
  - 수입: 잔액 증가
  - 지출/투자: 잔액 감소
- 거래 수정/삭제 시 잔액 역반영 처리

### 5.4 고정지출

- 고정지출 생성/조회/삭제
- 기간(`start_month`/`end_month`) 기반 월별 표시

### 5.5 예산

- 카테고리별 예산 등록/조회/삭제
- 월별 지출 대비 사용률 표시

### 5.6 투자

- 투자 자산 등록/수정/삭제
- 수익률(ROI) 계산
- 실시간 가격 새로고침 API 제공 (`/api/investments/refresh-prices`)

### 5.7 카테고리

- 카테고리 생성/조회/삭제
- 타입별 필터 조회 지원

### 5.8 UI/동기화

- API 실패 사유(`detail`)를 토스트로 표시
- 이미 삭제된 항목 요청(404) 시 목록 자동 동기화
- 자산 삭제 실패 시 연결 데이터 안내 후 강제삭제 흐름 제공

## 6. API 요약

- `GET/POST/PATCH/DELETE /api/assets`
- `GET/POST/PATCH/DELETE /api/transactions`
- `GET/POST/DELETE /api/fixed`
- `GET/POST/DELETE /api/budgets`
- `GET/POST/DELETE /api/categories`
- `GET/POST/PATCH/DELETE /api/investments`
- `POST /api/investments/refresh-prices`

## 7. 운영 메모

- 서버 실행 중 `dist` 폴더가 잠겨 빌드 실패할 수 있습니다. 이 경우 기존 프로세스를 종료하거나 `dist_hotfix` 빌드를 사용하세요.
- 데이터 초기화가 필요하면 `assets/transactions/fixed_expenses/investments/budgets` 테이블을 비우고, `categories`는 유지하는 방식으로 복구 가능합니다.
