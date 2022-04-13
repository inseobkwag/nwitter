#602277101 곽인섭

4월 13일 6주차 
=============
1.Firebase 인증 설정

2.Email,Google,Github 로그인 설정

3.로그인 폼 구조 생성

4.로그인 폼 상태 업데이트 가능하게 만듦

4-1 콘솔로그를 통해 intput에서 입력을 시도하고있는지 확인 
```
{
    console.log(event.target.name)
}
```
5.onSubmit 함수에서 로그인과 회원가입 분기시키기

4월 6일 5주차 
=============
1.상태를 이용한 react 훅스 사용

2.삼항 연산자를 이용하여 컴포넌트 반환

3.버전 다운 그레이드
`$ npm i react=router-dom@5.2.0`

4.useState함수 위치를 이동

5.jsconfig.json파일을 이용해 절대경로 적용
```
{
    "compilerOptions":{
        "baseUrl":"src"
    },
    "include":["src"]
}
```
6.컴포넌트의 import문 수정

7.firebase.js파일 이름 수정

8.파이어베이스 인증 모듈 사용


4주차 
=============
1.firebase 프로젝트 생성

2.firebase 애플리케이션 등록

3.firebase SDK 설치

4.`$ npm install firebase`

5.firebase 동작 확인

6.index.js 파일 생성

7.`$ npm run start`

8.firebase key숨겨야 하기 때문에
...env파일 생성

9..env파일을 gitignore 추가

10.페이지는 routes폴더에, 구성요소는 components폴더에 저장

```
const 컴포넌트 = () => <span>컴포넌트</span>;

export default 컴포넌트;
```


11.react-router-dom 설치

`$ npm install react-router-dom`

12.router.js파일 생성

13.파일 아래 Auth.js, EditProfile.js, Home.js, Profile.js 작성
