# 🚀 배포 체크리스트

## 현재 상태
✅ **프로토타입 완성**
- 나의 공구 DNA 퀴즈 (7 질문, 6 유형)
- Firebase 데이터 수집 코드 통합
- GitHub Pages 배포 준비 완료

---

## 🎬 배포 단계별 실행 사항

### Phase 1: Firebase 설정 (5분)
- [ ] FIREBASE_SETUP.md 읽고 따라하기
- [ ] Firebase 프로젝트 생성
- [ ] API Key 복사
- [ ] `dna-quiz-public/index.html` 에 설정값 입력
- [ ] Firestore 데이터베이스 생성
- [ ] 보안 규칙 설정 & 발행

### Phase 2: GitHub 배포 (5분)
- [ ] GitHub 새 레포지토리 생성 (`dna-quiz`)
- [ ] git remote 추가
- [ ] git push origin main
- [ ] GitHub Settings → Pages → 설정
  - Branch: `main`
  - Folder: `/dna-quiz-public`
- [ ] 배포 완료 대기 (1-2분)

### Phase 3: 테스트 (5분)
- [ ] 로컬에서 작동 확인
  ```bash
  cd dna-quiz-public
  python3 -m http.server 8000
  ```
- [ ] GitHub Pages URL 접속 확인
- [ ] 퀴즈 완료 후 결과 화면 도달
- [ ] Firebase Console → Firestore에서 데이터 저장 확인

---

## 📁 최종 파일 구조

```
.
├── dna-quiz-public/
│   ├── index.html           ✅ 메인 앱 (Firebase 통합)
│   └── README.md            ✅ 사용자 가이드
├── FIREBASE_SETUP.md        ✅ Firebase 설정 가이드
├── DEPLOYMENT_CHECKLIST.md  ✅ 배포 체크리스트 (이 파일)
└── .gitignore              ✅ git 제외 설정
```

---

## 🔗 최종 URL

배포 후 접속:
```
https://YOUR_USERNAME.github.io/dna-quiz/
```

예: `https://yuri-AI.github.io/dna-quiz/`

---

## 📊 데이터 수집 대시보드

### 실시간 모니터링
```
Firebase Console
→ Firestore Database
→ quiz_results 컬렉션
```

### 수집 데이터 형식
```json
{
  "type": "C",                              // 결과 타입 (A-F)
  "answers": [0, 1, 2, 0, 1, 2, 0],        // 각 질문 선택지 인덱스
  "timestamp": "2026-05-28T10:30:45.123Z", // ISO 시간
  "userAgent": "Mozilla/5.0..."            // 브라우저 정보
}
```

### CSV 내보내기
1. Firebase Console → Firestore
2. `quiz_results` 우클릭 → **내보내기**
3. Google Cloud Storage 또는 Google Drive에 저장

---

## ⚙️ 커스터마이징 옵션

### 1. 질문 변경
`dna-quiz-public/index.html` 내 `QS` 배열 수정

### 2. 유형 추가/변경
`TYPES` 객체 수정 (현재 A-F, 6개)

### 3. 채점 로직 변경
`SCORE_MAP` 배열 수정 (7×3 매트릭스)

### 4. 스타일 커스터마이징
CSS `:root` 변수 수정
```css
:root {
  --red:        #FE0955;   /* 주색상 */
  --red-soft:   #FF6694;   /* 보조색 */
  --dark:       #17192A;   /* 텍스트 */
}
```

---

## 🆘 트러블슈팅

| 증상 | 원인 | 해결 |
|------|------|------|
| "Firebase initialized 안 뜸" | API Key 없음 | FIREBASE_SETUP.md 확인 |
| 데이터 미저장 | Firestore 규칙 미발행 | 규칙 발행 클릭 |
| GitHub Pages 404 | 배포 미완료 | Settings > Pages 확인 |
| 퀴즈 로드 안 됨 | 파일 경로 오류 | `/dna-quiz-public/index.html` 확인 |

---

## 📈 성공 기준

✅ **완성**
- [x] 프로토타입 완성 (HTML/CSS/JS)
- [x] Firebase 코드 통합
- [x] GitHub 커밋 완료
- [ ] Firebase 설정값 입력
- [ ] GitHub Pages 배포
- [ ] 실제 URL 접속 확인

---

## 🎯 다음 단계 (나중에)

1. **마케팅**
   - OK캐쉬백 공구 리스트 배너에 노출
   - SNS 공유 유도

2. **분석**
   - 유형별 참여율 분석
   - 타입별 구매 패턴 분석
   - NPS 수집

3. **최적화**
   - A/B 테스트 (다양한 배너 문구)
   - 모바일 최적화 점검
   - 성능 모니터링

4. **통합**
   - 공구 추천 알고리즘에 유형 반영
   - 마지막 1명 챌린지와 자동 연동
   - 개인화 혜택 제공

---

**준비됐어요! 위 체크리스트를 따라 배포하면 됩니다. 🚀**
