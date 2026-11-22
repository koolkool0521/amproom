# AMP ROOM — 락밴드 작곡 & 협업 스튜디오 (Vercel 배포 패키지)

## 1. Vercel 배포 방법 (둘 중 편한 방법 선택)

### 방법 A: GitHub 연동 (추천 — 이후 수정 시 자동 배포)
1. GitHub에 새 저장소(Repository)를 만들고 이 폴더의 파일(`index.html`, `vercel.json`)을 업로드합니다.
2. Vercel(https://vercel.com/new)에 로그인 후 해당 저장소를 Import 합니다.
3. Framework Preset은 `Other` 그대로 두고 **Deploy**를 클릭하면 공유용 링크(`https://프로젝트명.vercel.app`)가 생성됩니다.

### 방법 B: 터미널에서 바로 배포 (Vercel CLI)
이 폴더에서 터미널을 열고 아래 명령어를 실행합니다.
```bash
npx vercel --prod
```

## 2. Firebase 실시간 동기화 & 음원 공유 설정 (필수)
1. Firebase 콘솔(https://console.firebase.google.com/)에서 `invite-c47f6` 프로젝트를 엽니다.
2. **Build > Firestore Database** 생성(테스트 모드 또는 아래 규칙 적용):
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /band_songs/{document=**} {
      allow read, write: if true;
    }
  }
}
```
3. **Build > Storage** 시작 후 규칙 탭에서 아래 규칙 게시(음원 파일 업로드 공유용):
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /band_audio/{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```
4. 배포된 Vercel 주소에 접속해 상단 상태 표시가 **Firebase 실시간 동기화(초록불)**로 바뀌는지 확인하면 완료입니다!
