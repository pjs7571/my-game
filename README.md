# 롤 스킬 피하기 게임

Firebase 랭킹 + 모바일 조작 + 이스터에그 + 자유게시판 버전입니다.

## 추가 기능

### 이스터에그
게임 시작 후 약 10초 동안 가만히 있으면 1000점을 받습니다.

### 자유게시판
- 화면 왼쪽 위 `자유게시판` 버튼으로 열기
- 글 작성 가능
- Firebase 연결 시 온라인 게시판으로 작동
- 연결 실패 시 localStorage 로컬 게시판으로 작동

### 관리자 기능
- 기본 관리자 비밀번호: `1234`
- 관리자 로그인 후 게시글 수정/삭제 가능
- `index.html`에서 `ADMIN_PASSWORD = "1234"` 부분을 찾아 원하는 비밀번호로 바꿀 수 있습니다.

주의: HTML 안에 들어가는 비밀번호라 완전한 보안은 아닙니다. 학교 과제/친구끼리 쓰는 용도에 적합합니다.

## Firebase Firestore Rules 예시

게시판 수정/삭제까지 앱에서 쓰려면 아래 규칙처럼 `boardPosts`에 update/delete 권한도 필요합니다.
다만 이 방식은 클라이언트 비밀번호 방식이라 강한 보안은 아닙니다.

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
