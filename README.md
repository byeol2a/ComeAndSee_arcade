# 🎮 응원 아케이드 (10.00초 챌린지 & 탭탭 챌린지)

학교 행사용 아케이드 게임. Supabase 실시간 공용 랭킹 연동.

## 📦 배포 방법 (GitHub Pages)

1. 이 저장소에 파일 업로드
   - `index.html` → 저장소 최상단
   - `.github/workflows/backup.yml` → 자동 백업용 (폰에서는 Add file > Create new file 에서
     파일명을 `.github/workflows/backup.yml` 로 입력하고 내용 붙여넣기)
2. Settings > Pages > Branch 를 `main` 으로 선택 > Save
3. 1~2분 뒤 `https://아이디.github.io/저장소명/` 으로 접속

## 🎯 게임 요약

| 게임 | 규칙 | 제한 |
|---|---|---|
| ⏱ 10.00초 챌린지 | 5초 뒤 시계가 숨음. 정확히 10.00초에 STOP | 1인 하루 5회 |
| 🔥 탭탭 챌린지 | 10초 연타. 초당 14타 돌파 시 FEVER ×2 | 1인 하루 5회 |

- 입장: 학교("○○중학교/○○고등학교") + 학년 + 이름 + 비밀번호 4자리
- 랭킹: 게임별 × 일간/주간 × 개인/학교 (학교는 누적점수)
- 하루 5회 제한은 서버에서 강제 (기기·브라우저 우회 불가)

## 🔐 관리자 기능

- 숨김 메뉴: 랭킹 화면의 "RANKING" 글자를 2초 안에 5번 연타 → PIN `2026`
  → CSV 저장 / 이 기기 기록 삭제
- 비밀번호 분실 처리: Supabase > Table Editor > `delete_requests` 확인 후
  `players` 테이블에서 해당 학생 행 삭제 → 학생이 새 비밀번호로 재등록 (기록 유지됨)
- 자동 백업: 매일 KST 00:05에 `backups/` 폴더로 CSV 자동 저장
  (Actions 탭 > daily-csv-backup > Run workflow 로 수동 실행 가능)

## 🧹 초기화 쿼리 (Supabase > SQL Editor)

```sql
-- 기록만 초기화 (계정 유지)
truncate table public.records restart identity;

-- 완전 초기화 (기록 + 계정 + 초기화요청)
truncate table public.records restart identity;
truncate table public.players restart identity;
truncate table public.delete_requests restart identity;
```

## ⚠️ 운영 참고

- 입장(로그인)은 인터넷 필수 — 행사장 네트워크 백업(핫스팟) 권장
- 행사 시작 전 리허설 기록은 초기화 쿼리로 정리 후 시작
