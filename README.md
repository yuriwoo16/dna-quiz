# 나의 공구 DNA — 퀴즈 프로토타입

OK캐쉬백 쇼핑 공구 심리 유형 진단 서비스

## 🚀 배포

### GitHub Pages로 배포하기

1. **저장소 생성** (이미 git init됨)
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/dna-quiz.git
   git branch -M main
   git push -u origin main
   ```

2. **GitHub Settings → Pages**
   - Source: `Deploy from a branch`
   - Branch: `main` / Folder: `/dna-quiz-public`
   - 저장

3. **배포 완료**
   - 접속: `https://YOUR_USERNAME.github.io/dna-quiz/`

---

## 🔥 Firebase 데이터 수집 설정

### 1단계: Firebase 프로젝트 생성

1. https://console.firebase.google.com → 새 프로젝트
2. 프로젝트명: `dna-quiz` → 계속
3. Google Analytics: 비활성화 → 만들기

### 2단계: 프로젝트 설정 복사

1. **프로젝트 설정** → **앱 추가** → Web
2. 앱 닉네임: `dna-quiz-web`
3. **Firebase SDK 복사** (예시):
   ```javascript
   const FIREBASE_CONFIG = {
     apiKey: "AIzaSyDx...",
     authDomain: "dna-quiz-xxxxx.firebaseapp.com",
     projectId: "dna-quiz-xxxxx",
     storageBucket: "dna-quiz-xxxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef..."
   };
   ```

4. `dna-quiz-public/index.html`의 `FIREBASE_CONFIG` 값 변경
   ```html
   const FIREBASE_CONFIG = {
     apiKey: "YOUR_API_KEY",  // 여기 수정
     ...
   };
   ```

### 3단계: Firestore 데이터베이스 생성

1. Firebase Console → **Firestore Database** → Create Database
2. **위치**: `asia-southeast1` (싱가포르)
3. **보안 규칙**: `테스트 모드로 시작`

### 4단계: 보안 규칙 설정

Firebase Console → **Firestore** → **Rules** 탭에서:

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

발행 → 완료

---

## 📊 수집된 데이터 확인

### Firestore에서 직접 보기

1. Firebase Console → **Firestore Database**
2. `quiz_results` 컬렉션 → 문서 클릭

### CSV로 다운로드

1. Firebase Console → **Firestore Database**
2. `quiz_results` 우클릭 → **Export** → Google Cloud Storage 선택
3. 또는 Google Apps Script로 자동화

---

## 📁 파일 구조

```
dna-quiz-public/
├── index.html          # 메인 앱 (Firebase 통합)
└── README.md           # 설정 가이드
```

---

## 🔄 로컬 테스트

```bash
cd dna-quiz-public
python3 -m http.server 8080
```

브라우저: `http://localhost:8080`

> **주의**: Firebase 초기화 전까지 데이터 저장이 작동하지 않습니다.

---

## 📈 데이터 구조

Firestore에 저장되는 각 응답:

```javascript
{
  "type": "C",                                    // 결과 유형 (A~F)
  "answers": [0, 1, 2, 0, 1, 2, 0],             // 7개 질문 답변 인덱스
  "timestamp": "2026-05-28T10:30:45.123Z",      // ISO 타임스탬프
  "userAgent": "Mozilla/5.0..."                 // 브라우저 정보 (100자 제한)
}
```

---

## 🎯 6가지 유형

| 유형 | 이름 | 특징 |
|------|------|------|
| A | 🔍 의심 많은 단골 | 검색 후 결국 여기서 사는 사람 |
| B | 🛡️ 인원 안심형 | 인원 수 봐야 안심하는 사람 |
| C | ⏱️ 타이머 인간 | 마감 알림이 결정 버튼인 사람 |
| D | 🎫 공구 핑계러 | 원래 갖고 싶던 거를 사는 사람 |
| E | 🧩 1명 완성러 | 마지막 자리를 채워야 하는 사람 |
| F | 👁️ 눈팅 전문가 | 봐만 두고 못 사는 사람 |

---

## 🔗 관련 링크

- [마지막 1명 챌린지](../specs/game/last-one-challenge/)
- [설계 문서](../specs/game/last-one-challenge/GDD.md)
- [디자인 시스템](../specs/game/last-one-challenge/design/design-system.md)
