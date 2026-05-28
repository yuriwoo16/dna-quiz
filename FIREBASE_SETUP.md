# Firebase 설정 가이드 (5분)

## 📋 준비물
- Google 계정
- GitHub 계정 (GitHub Pages 배포용)

---

## 1️⃣ Firebase 프로젝트 생성 (2분)

### 1.1 Firebase Console 접속
```
https://console.firebase.google.com
```

### 1.2 새 프로젝트 생성
- **프로젝트명**: `dna-quiz`
- **Google Analytics**: ❌ 비활성화
- **지역**: Asia Southeast 1 (싱가포르) 권장

### 1.3 프로젝트 설정 복사
1. **프로젝트 설정** (⚙️) → **앱 추가** → Web (</> 아이콘)
2. 다음과 같은 설정이 표시됨:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDx...",
  authDomain: "dna-quiz-xxxxx.firebaseapp.com",
  projectId: "dna-quiz-xxxxx",
  storageBucket: "dna-quiz-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef..."
};
```

---

## 2️⃣ HTML 파일에 설정 입력 (1분)

### 파일: `dna-quiz-public/index.html`

찾기 (Ctrl+F): `FIREBASE_CONFIG`

```javascript
// 이 부분을 위에서 복사한 값으로 변경
const FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY",          // ← 변경
  authDomain: "YOUR_AUTH_DOMAIN",  // ← 변경
  projectId: "YOUR_PROJECT_ID",    // ← 변경
  storageBucket: "YOUR_STORAGE_BUCKET",  // ← 변경
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",  // ← 변경
  appId: "YOUR_APP_ID"             // ← 변경
};
```

---

## 3️⃣ Firestore 데이터베이스 생성 (1분)

### 3.1 Firebase Console에서
1. **Firestore Database** → **데이터베이스 만들기**
2. **위치**: `asia-southeast1`
3. **시작 모드**: `테스트 모드로 시작` ✅
4. **활성화** 클릭

### 3.2 보안 규칙 설정 (중요!)
1. **Firestore Database** → **Rules** 탭
2. 기본 규칙을 다음으로 교체:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /quiz_results/{document=**} {
      allow read, write: if request.time < timestamp.date(2026, 12, 31);
    }
  }
}
```

3. **발행** 클릭

> ⚠️ 이 규칙은 2026년 12월 31일까지만 유효합니다. 이후 자동 차단됨.

---

## 4️⃣ GitHub에 푸시 (1분)

```bash
cd /Users/1004735/yuri_AI/OCB_AI

# 원격 저장소 추가
git remote add origin https://github.com/YOUR_USERNAME/dna-quiz.git

# 커밋 및 푸시
git push -u origin main
```

---

## 5️⃣ GitHub Pages 배포 (1분)

### 5.1 GitHub 저장소 설정
1. GitHub → 저장소 이동
2. **Settings** → **Pages**
3. **Build and deployment**
   - Source: `Deploy from a branch`
   - Branch: `main`
   - Folder: `/dna-quiz-public`
4. **Save**

### 5.2 배포 완료
약 1-2분 후 접속 가능:
```
https://YOUR_USERNAME.github.io/dna-quiz/
```

---

## ✅ 테스트

### 로컬에서 먼저 테스트
```bash
cd dna-quiz-public
python3 -m http.server 8000
# http://localhost:8000 접속
```

### 실제 배포 확인
1. URL 접속
2. 퀴즈 완료 후 결과 화면 도달
3. Firebase Console → **Firestore Database** → `quiz_results` 컬렉션에서 데이터 확인

---

## 📊 수집 데이터 확인

### Firestore에서 직접 보기
```
Firebase Console → Firestore Database → quiz_results 컬렉션
```

각 문서:
```json
{
  "type": "C",
  "answers": [0, 1, 2, 0, 1, 2, 0],
  "timestamp": "2026-05-28T10:30:45.123Z",
  "userAgent": "Mozilla/5.0..."
}
```

### CSV로 내보내기
1. **Firestore** → `quiz_results` 우클릭 → **내보내기**
2. **Google Cloud Storage**로 내보내기
3. 또는 Google Sheets에 자동 연동

---

## 🚨 주의사항

### 1. Firebase 한도
- **무료 플랜**: 하루 최대 50,000건 쓰기 가능 ✅
- 초과 시 추가 요금 발생

### 2. 보안 규칙
- 테스트 모드는 **읽기/쓰기 제한 없음**
- 프로덕션 전에 반드시 규칙 강화

### 3. GitHub Pages
- 정적 파일만 배포 가능 (서버 X)
- Firebase는 클라이언트에서 직접 호출

---

## 🆘 문제 해결

### "Firebase initialized" 메시지가 안 떰
- 브라우저 개발자 도구 (F12) → **Console** 확인
- API Key가 올바른지 재확인
- Firestore 보안 규칙 확인

### 데이터가 저장되지 않음
1. Firebase Console → Firestore → **Rules** 확인
2. 규칙 발행 상태 확인 (초록색 ✓)
3. 콘솔 에러 메시지 확인

### GitHub Pages에 404 뜸
1. **Settings** → **Pages** → 배포 상태 확인
2. 올바른 브랜치 & 폴더 설정 확인
3. 파일명 확인 (`index.html`)

---

## 📞 피드백
이 가이드에서 막히는 부분이 있으면 알려주세요!
