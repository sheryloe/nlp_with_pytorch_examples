# Donggri Ledger

로컬 PC에서 실행하는 개인용 가계부/자산 관리 웹앱입니다.
실행하면 브라우저에서 바로 쓰는 구조이며, 데이터는 내 PC(SQLite)에 저장됩니다.

## 한눈에 보기

- 용도: 개인 가계부 + 자산/예산/투자 관리
- 실행 방식: `py launcher.py` 후 브라우저 접속
- 데이터 저장: `%LOCALAPPDATA%\\donggri-ledger\\data\\ledger.db`
- API 키: 별도 발급/설정 필요 없음
- 네트워크: 기본 가계부 기능은 로컬에서 동작, 투자 시세 새로고침은 인터넷 필요

## 1. 설치와 실행 (Windows)

### 1.1 준비

- Python 3.12 이상 설치
- PowerShell에서 프로젝트 루트(`donggri-ledger`)로 이동

### 1.2 패키지 설치

```powershell
py -m pip install -r requirements.txt
```

### 1.3 앱 실행

```powershell
py launcher.py
```

### 1.4 접속 주소

- PC 접속: `http://127.0.0.1:8000/ui/`
- 상태 확인: `http://127.0.0.1:8000/health`

`/health` 응답의 `db_path`로 실제 사용 중인 DB 경로를 바로 확인할 수 있습니다.

## 2. 처음 사용하는 순서 (3 Step)

### Step 1. 자산 먼저 등록

`자산` 탭에서 현금/은행/카드/투자 계좌를 먼저 등록하세요.
거래 입력 시 이 자산 목록을 기준으로 잔액이 자동 반영됩니다.

### Step 2. 카테고리/예산/고정지출 세팅

- `설정` 탭: 수입/지출 카테고리 추가/정리
- `예산` 탭: 카테고리별 월 예산 등록
- `고정지출` 탭: 월세/통신비/구독료 같은 반복 지출 등록

### Step 3. 거래 입력 후 리포트 확인

- `가계부` 탭에서 수입/지출/투자 거래 입력
- `달력`, `리포트`, `검색` 탭으로 월간 흐름 확인
- `투자` 탭에서 수익률(ROI) 관리

## 3. 백업/복원

- 상단 `백업` 버튼: 현재 데이터를 JSON 파일로 저장
- 상단 `복원` 버튼: 저장한 JSON 파일 다시 불러오기

정기 백업(예: 주 1회)을 권장합니다.

## 4. 데이터 위치와 경로 정책

기본 DB 파일:

- `%LOCALAPPDATA%\\donggri-ledger\\data\\ledger.db`

개발 실행과 EXE 실행 모두 같은 기본 경로를 사용합니다.
고급 사용자는 환경변수로 경로를 바꿀 수 있습니다.

- `DONGGRI_LEDGER_DATA_DIR`

## 5. API 관련 안내 (중요)

이 프로젝트의 `/api/*` 엔드포인트는 프론트(`web/index.html`)가 내부적으로 호출하는 용도입니다.
개인 사용 기준으로는 API를 따로 호출하거나 관리할 필요가 없습니다.

- 별도 외부 API 키를 넣어야 시작되는 구조가 아닙니다.
- 투자 시세 새로고침만 Yahoo Finance 공개 엔드포인트를 사용합니다.

## 6. EXE 빌드 (필요할 때만)

### 6.1 표준 빌드

```powershell
py -m PyInstaller -y donggri-ledger.spec
```

산출물:

- `dist\\donggri-ledger\\donggri-ledger.exe`

### 6.2 dist 잠금 우회 빌드

기존 EXE 프로세스가 남아 `dist` 폴더 잠금이 생기면 아래처럼 우회 빌드합니다.

```powershell
py -m PyInstaller -y --distpath dist_hotfix --workpath build_hotfix donggri-ledger.spec
```

산출물:

- `dist_hotfix\\donggri-ledger\\donggri-ledger.exe`

## 7. 트러블슈팅

- UI가 이전 상태로 보이면 `Ctrl+F5`로 강력 새로고침
- 삭제/수정이 꼬이면 브라우저 콘솔 에러와 API `detail` 메시지 확인
- 자산 삭제가 막히면 연결된 거래/고정지출/투자 데이터가 있는 상태일 수 있음
- 서버가 안 뜨면 `http://127.0.0.1:8000/health` 먼저 확인
