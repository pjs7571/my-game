# 롤 스킬 피하기 게임

Firebase 랭킹 + Firebase 자유게시판 + 모바일 조작 + 이스터에그 버전입니다.

## 추가 기능

### 이스터에그
게임 시작 후 약 10초 동안 가만히 있으면 1000점을 받습니다.

### Firebase 자유게시판
- 화면 왼쪽 위 `자유게시판` 버튼으로 열기
- 게시글 작성 가능
- Firebase Firestore의 `boardPosts` 컬렉션에 저장
- Firebase 연결 실패 시 임시로 localStorage 로컬 게시판 사용

### 관리자 기능
- 관리자 비밀번호: `987654321`
- 관리자 로그인 후 게시글 수정/삭제 가능
- `index.html`에서 아래 부분을 찾아 비밀번호 변경 가능

```js
const ADMIN_PASSWORD = "987654321";
```

주의: 이 방식은 HTML 안에 관리자 비밀번호가 들어가는 방식이라 완전한 보안은 아닙니다.  
학교 과제나 친구끼리 쓰는 용도에는 괜찮지만, 실제 서비스처럼 만들려면 Firebase Authentication과 서버 검증이 필요합니다.

## Firebase Firestore Rules

Firebase 콘솔의 Firestore `규칙` 탭에 아래 코드를 붙여넣고 게시하세요.

```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /rankings/{docId} {
      allow read: if true;

      allow create: if
        request.resource.data.name is string &&
        request.resource.data.name.size() <= 12 &&
        request.resource.data.score is number &&
        request.resource.data.score >= 0 &&
        request.resource.data.score <= 9999999 &&
        request.resource.data.money is number;

      allow update, delete: if false;
    }

    match /boardPosts/{docId} {
      allow read: if true;

      allow create: if
        request.resource.data.name is string &&
        request.resource.data.name.size() <= 12 &&
        request.resource.data.title is string &&
        request.resource.data.title.size() <= 40 &&
        request.resource.data.content is string &&
        request.resource.data.content.size() <= 300;

      allow update, delete: if true;
    }
  }
}
```

## PC 조작

- WASD / 방향키: 이동
- Space: 대시
- Q: 모래시계
- E: 벨트
- F: 유체화
- ESC: 일시정지

## 모바일 조작

- 왼쪽 조이스틱: 이동
- 오른쪽 큰 버튼: 대시
- Q/E/F 버튼: 스킬 사용
