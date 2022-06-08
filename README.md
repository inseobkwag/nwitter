#602277101 곽인섭

참고:github.com/easysIT/Nwitter
react api연동 오류날때 참고할만한 사이트:netlify.app

6월 8일 13주차
=============
1.사진 저장 기능 만들기
* firebse storage 임포트
* 고유 식별자를 만들어주는 UUID 라이브러리 설치
```
npm install uuid
```
* UUID 임포트
* 스토리지에 사진 저장<br>
  제공함수 putString사용<br>
  불러오기<br>
  response.ref.getDownloadURL 함수를 사용
* 사진을 포함한 결과를 출력하고 코드를 다듬는다
* 트윗 삭제시 사진을 스토리지에서 삭제<br>
refFromURL함수를 사용하면 attachmentUrl만으로도 스토리지에서 삭제가능
- alt=읽은값 그대로 
- 임시의 아이디=uuid

2.내가 쓴 트윗만 보기
* 파일 정리하기
* 트윗 필터링 기능 구현
* 정렬 쿼리 사용하기
* 필터링 기능 잘 동작하는지 확인



요즘은 var 잘 안씀, 변수:let 상수:const





5월 25일 12주차
=============
1.바로 전 주차에서 했던 삭제기능과 유사하게 수정기능도 만들어보자
* 수정기능을 위한 useState를 추가
* editing에 따라 토글 상태를 관리
* 입력란에 onChange작업
```
const onChange = (event) => {
    const {
      target: { value },
    } = event;
    setNewNweet(value);
  };
```

* 파이어스토어에 새 입력값 반영
```
const onSubmit = async (event) => {
    event.preventDefault();
    await dbService.doc(`nweets/${nweetObj.id}`).update({ text: newNweet });
    setEditing(false);
  };
```

2.사진 미리보기 기능 만들기
* 파일 첨부 양식 만들기(설정한것만 선택가능)
`accept="image/*`를 활용<br>text,png,heic,jpg등 다양하게 활용가능

* 첨부파일 정보 출력
* 웹 브라우저에 사진 출력
`FileReader`를 이용, `readAsDataURL`함수를 이용해 파일정보를 인자로 받아 파일위치를 URL로 반환해줌
* 사진 미리보기 구현
`const [attachment, setAttachment] = useState("");`
```
 const {
        currentTarget: { result },
      } = finishedEvent;
      setAttachment(result);
    };
```
* attachment가 있는경우 img 엘리먼트를 출력
`{attachment && <img src={attachment} width="50px" height="50px" />}`
* 파일 선택 취소 버튼 생성
onClearAttachment함수를 이용
`const onClearAttachment = () => setAttachment("");`
`<button onClick={onClearAttachment}>Clear</button>`

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
