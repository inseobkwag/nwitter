#602277101 곽인섭
참고:github.com/easysIT/Nwitter

5월 18일 11주차
=============
1.트윗 목록 출력해보기
  * map함수는 배열을 순회하기위한 ES6함수이다.
  ```
  {nweets.map((nweet)=>(
                    <div key={nweet.id}>
                        <h4>{nweet.text}</h4>
                        </div>
  ```
2.누가쓴 트윗인지 알아보자
* authService.onAuthStateChanged함수를 사용해 정보를 받아온다.
* useState를 추가해 usreObj를 만들고 AppRouter컴포넌트에 userObj를 보내준다.<br>이제 필요한 컴포넌트로 userObj정보를 뿌려준다.
* 프롭스는 여러 컴포넌트를 거치지 않도록 하는게 유지보수에 용이하다.
* Home컴포넌트에서 프롭스를받고 userObj.id(고유값 Uid)를 저장하는 로직을 추가
* 파이어스토어에 잘저장되고 확인되는지 확인해본다.

3.실시간 DB로 트윗목록 보여주기
- getNweets함수 삭제
- onSnapshot함수 적용
```
dbService.collection("nweets").onSnapshot((Snapshot)=>{
  const newArray=Snapshot.docs.map((document)=>({
    id:document.id,
     ...document.data(),
  }));
  setNweets(newArray);
       });
```

4.트윗 삭제 기능 만들기
* 트윗 컴포넌트 분리
* 수정,삭제버튼추가
* 내가쓴 트윗 나만 삭제,수정되도록 만듦
* firestore uid바꾸기
* 버튼에 삭제기능 추가



5월 11일 10주차
=============
1.클라우드 Firestore에서 컬렉션이 잘 가져오는지 확인
  - 컬렉션에서 각 항목을 통해 조회나 수정 삭제가 가능한지 확인

2.리액트에서 db를 사용해보기
* fbase에 firestore import시켜주기

3.Firestore에서 데이터 저장하기
  ```
  await dbService.collection("nweets").add({
            text:nweet,
            createdAt:Date.now(),
        });
        setNweet("");
    };

  ```

4.Firestore에서 문서를 읽어오는지 확인<br>
  
   ``` 
  const getNweets=async()=>{
        const dbNweets=await dbService.collection("nweets").get();
        dbNweets.forEach((document)=> console.log(document.data()));
    };
  ```
  * forEach 함수를 사용해 여러 개의 문서 스냅샷을 순회

5.받은 데이터로 게시물 목록 만들어보기

6.트윗 아이디 저장
* 문서에는 id속성이 있는데 스냅샷을 순회하면서 document.id를 얻자
  ```
  const nweetObject = {...document.data(), id:document.id};
            setNweets((prev)=>[nweetObject, ...prev])
        });
  ```

5월 4일 9주차
=============
1.cache 삭제하는 법 (필요할시 수행)<br>
`$ npm cache clean --force`

2.컴포넌트 안에 Navigation.js 파일을 생성

3.Switch를 이용해 isLoggedIn이 true인 경우에만 Navigation이 보이도록 출력<BR>
`{isLoggedIn && <Navigation />}`
  
4.루트 안에 Profile.js 파일을 생성
  - Profile.js 파일에서 로그아웃 버튼 생성

5.Home.js 내용 작성

6.Redirect로 로그아웃 후 주소 이동

7.useHistory를 이용하여 로그아웃을 처리하는 자바스크립트 코드로 주소 이동
```
  import { useHistory } from 'react-router-dom';
  ...
  const histoy = useHistory();
  const onLogOutClick = () => {
    authService.signOut();
    histoy.push("/");
  };

```
8.파이어베이스 데이터 베이스 생성하기<br>(위치는 가장 가까운 곳으로 해야 빠름)

9.파이어베이스 데이터베이스는 NoSQL기반 데이터베이스이다.

4월 27일 8주차
=============
1.파이어 베이스로 고르인과 회원가입 처리하기

2.Auth.js async
```
const onSubmit = async (event) => {
  event.preventDefault();
  try {
    let data;
    if (newAccount) {
      // create newAccount
      data = await authService.createUserWithEmailAndPassword(email, password);
    } else {
      // log in
      data = await authService.signInWithEmailAndPassword(email, password);
    }
    console.log(data);
  } catch (error) {
    console.log(error);
  }
};
```
3.App.js useEffect 사용하기
```
function App() {
  const [init, setInit] = userState(false);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  useEffect(() => {
    authService.onAuthStateChanged((user) => {
      if (user) {
        setIsLoggedIn(user);
      } else {
        setIsLoggedIn(false);
      }
      setInit(true);
    });
  }, []);
  ```
  4.로그인,회원가입 토글 버튼 적용

  5.소셜 로그인 버튼에서 name 속성 사용

4월 13일 6주차 
=============
1.Firebase 버전 다운 그레이드
`$ npm i firebase@8.8.0`

2.Firebase 인증 설정

3.Email,Google,Github 로그인 설정

4.로그인 폼 구조 생성

5.로그인 폼 상태 업데이트 가능하게 만듦

5-1 콘솔로그를 통해 intput에서 입력을 시도하고있는지 확인 
```
{
    console.log(event.target.name)
}
```
6.onSubmit 함수에서 로그인과 회원가입 분기시키기

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
