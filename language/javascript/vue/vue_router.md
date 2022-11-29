# Vue Router

## Routing

- 네트워크에서 경로를 선택하는 프로세스
- 웹 서비스에서의 라우팅
  - 유저가 방문한 URL에 대해 적절한 결과를 응답하는 것
  - 즉 예를 들어 /articles/index/ 에 접근하면 articles의 index에 대한 결과를 보여줌

### Routing in SSR

- Server가 모든 라우팅을 통제
- URL로 요청이 들어오면 응답으로 완성된 HTML을 제공
- 결론적으로, Routing(URL)에 대한 결정권을 서버가 가짐

### Routing in SPA / CSR

- 서버는 하나의 HTML만을 제공
- 이후에 모든 동작은 하나의 HTML 문서 위에서 Javascript 코드를 활용
  - DOM을 그리는 데 필요한 추가적인 데이터가 있다면 axios와 같은 AJAX 요청을 보낼 수 있는 도구를 사용하여 데이터를 가져오고 처리
- 즉, 하나의 URL만 가질 수 있음

### Why routing?

- 동작에 따라 URL이 반드시 바뀌어야 할 필요는 없다. 하지만 유저의 사용성 관점에서느 필요하다.
- Routing이 없다면,
  - 유저가 URL을 통한 페이지의 변화를 감지할 수 없음
  - 페이지가 무엇을 렌더링 중인지에 대한 상태를 알 수 없음
    - 새로고침시 처음 페이지로 돌아감
    - 링크를 공유할 시 처음 페이지만 공유 가능
  - 브라우저의 뒤로 가기 기능을 사용할 수 없음

## Vue Router

- Vue의 공식 라우터
- SPA 상에서 라우팅을 쉽게 개발할 수 있는 기능을 제공
- Routes에 컴포넌트를 매핑한 후, 어떤 URL에서 렌더링할 지 알려줌
  - 즉, SPA를 MPA처럼 URL을 이동하면서 사용 가능
  - SPA의 단점 중 하나인 ‘URL이 변경되지 않는다’를 해결

### Vue Router 시작하기

- Vue project에서 `vue add router` 명령을 통해 설치한다.
  - 기존에 프로젝트를 진행하고 있던 도중에 router를 추가하게 되면 App.vue를 덮어쓰므로 필요한 경우 명령을 실행하기 전에 파일을 백업해두어야 함
- history 사용 여부 → y

### History mode

- 브라우저의 History API를 활용한 방식
  - 새로고침 없이 URL 이동 기록을 남길 수 있음
- 우리에게 익숙한 URL 구조로 사용 가능
  - 예시. [http://local](http://local)host:8080/index
- History mode를 사용하지 않으면 Default 값인 hash mode로 설정됨 (’#’을 통해 URL을 구분하는 방식)
  - 예시. [http://localhost:8080#index](http://localhost:8080#index)

### <router-link><router-link>

- a 태그와 비슷한 기능 → URL을 이동시킴
  - routes에 등록된 컴포넌트와 매핑됨
  - 히스토리 모드에서 router-link는 클릭 이벤트를 차단하여 a 태그와 달리 브라우저가 페이지를 다시 로드하지 않도록 함
- 목표 경로는 ‘to’ 속성으로 지정됨
- 기능에 맞게 HTML에서 a 태그로 rendering되지만, 필요에 따라 다른 태그로 바꿀 수 있음

### <router-view />

- 주어진 URL에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트
- 실제 component가 DOM에 부착되어 보이는 자리를 의미
- <router-link>를 클릭하면 <router-view>에 routes에 매핑된 컴포넌트를 렌더링

### src/router/index.js

- routes 인스턴스

### src/Views

- <router-view>에 들어갈 component 작성
- 기존에 컴포넌트를 작성하던 곳은 component 폴더 뿐이었지만 이제 두 폴더로 나뉘어짐
- 각 폴더 안의 .vue 파일들이 기능적으로 다른 것은 아님
- 폴더별 컴포넌트 배치 (규약은 아님)
  - views/
    - routes에 매핑되는 컴포넌트, 즉 <router-view>의 위치에 렌더링되는 컴포넌트를 모아두는 디렉토리
    - 다른 컴포넌트와 구분하기 위해 View로 끝나도록 만드는 것을 권장
    - 예시. App 컴포넌트 내부의 AboutView & HomeView 컴포넌트
  - components/
    - routes에 매핑된 컴포넌트의 하위 컴포넌트를 모아두는 폴더
    - 예시. HomeView 컴포넌트 내부의 HelloWorld 컴포넌트

## Vue Router 실습

- 주소를 이동하는 두 가지 방법
  1. 선언적 방식 네비게이션
  2. 프로그래밍 방식 네비게이션

### 선언적 방식 네비게이션

- router-link의 ‘to’ 속성으로 주소 전달
  - routes에 등록된 주소와 매핑된 컴포넌트로 이동
  - routes에 등록된 이름(name)과 매핑된 컴포넌트로 이동(named routes) → 객체({})로 전달
- 동적인 값을 사용하기 때문에 v-bind를 사용해야 정상적으로 작동 (`:to`)

### 프로그래밍 방식 네비게이션

- Vue 인스턴스 내부에서 라우터 인스턴스에 `$routes` 로 접근할 수 있음
- 다른 URL로 이동하려면 `this.$router.push` 를 사용
  - history stack에 이동할 URL을 넣는(push) 방식
  - history stack에 기록이 남기 때문에 사용자가 브라우저의 뒤로 가기 버튼을 클릭하면 이전 URL로 이동할 수 있음
- 결국 `<router-link :to="...">` 를 클릭하는 것과 `$router.push(...)` 를 호출하는 것은 같은 동작

### Dynamic Route Matching

- 동적 인자 전달 (URL의 특정 값을 변수처럼 사용할 수 있음)
  - params를 이용하여 인자 전달 가능
  - 예시. `<router-link :to="{ name: 'home' } params: { userName: 'harry' }}"`
- route를 추가할 때 동적 인자를 명시 (`:dynamic-variable-name` )
  - 예시. `{ path: '/hello/:userName', ...}`
- `$route.params` 로 변수에 접근 가능
  - 다만 HTML에서 직접 사용하기보다는 data에 넣어서 사용하는 것을 권장

### lazy-loading

- route에 컴포넌트를 등록하는 또다른 방법
- route의 component 키에 값을 함수로 등록
  - 기존 방식: `{..., component: HomeView}`
  - lazy-loading: `{…, component: () => import('@/views/HomeView')}`
- 모든 파일을 한 번에 로드하려고 하면 모든 걸 다 읽는 시간이 매우 오래 걸림
- 미리 로드를 하지 않고 특정 라우트에 방문할 때 매핑된 컴포넌트의 코드를 로드하는 방식을 활용할 수 있음
  - 모든 파일을 한 번에 로드하지 않아도 되기 때문에 최초에 로드하는 시간이 빨라짐
  - 당장 사용하지 않을 컴포넌트는 먼저 로드하지 않는 것이 핵심

## Navigation Guard

- Vue router를 통해 특정 URL에 접근할 때 (1) 다른 URL로 redirect를 하거나 (2) 해당 URL로의 접근을 막는 방법
  - 예시. 사용자의 인증 정보가 없으면 특정 페이지에 접근하지 못 하게 함
  - [https://v3.router.vuejs.org/guide/advanced/navigation-guards.html](https://v3.router.vuejs.org/guide/advanced/navigation-guards.html)
- 네비게이션 가드의 종류
  - 전역 가드: 애플리케이션 전역에서 동작
  - 라우터 가드: 특정 URL에서만 동작
  - 컴포넌트 가드: 라우터 컴포넌트 안에 정의

### 전역 가드

- 다른 URL 주소로 이동할 때 항상 실행
- router/index.js 에 `router.beforeEach()` 를 사용하여 설정
- 콜백 함수의 값으로 다음 3개의 인자를 받음
  - to: 이동할 URL 정보가 담긴 Route 객체
  - from: 현재 URL 정보가 담긴 Route 객체
  - next: 지정한 URL로 이동하기 위해 호출하는 함수
    - 콜백 함수 내부에서 반드시 한 번만 호출되어야 함
    - 기본적으로 to에 해당하는 URL로 이동
- URL이 변경되어 화면이 전환되기 전 router.beforeEach 가 호출됨
  - 화면이 전환되지 않고 대기 상태가 됨
- 변경된 URL로 라우팅하기 위해서는 next를 호출해줘야 함
  - next가 호출되기 전까지 화면이 전환되지 않음
- Global Before Guard 실습

  ```jsx
  // router/index.js

  router.beforeEach((to, from, next) => {
    console.log("to", to);
    console.log("from", from);
    console.log("next", next);
  });
  ```

  - next가 호출되지 않으면 화면이 전환되지 않음

  ```jsx
  // router/index.js

  router.beforeEach((to, from, next) => {
    console.log("to", to);
    console.log("from", from);
    console.log("next", next);
    next();
  });
  ```

  - next가 호출되어야 화면이 전환됨

### 라우터 가드

- 전체 route가 아닌 특정 route에 대해서만 가드를 설정하고 싶을 때 사용
- 개별 route에 `beforeEnter()` 를 사용하여 설정
  - route에 진입했을 때 실행됨
  - 라우터를 등록한 위치에 추가
  - 단, 매개변수, 쿼리, 해시 값이 변경될 때는 실행되지 않고 다른 경로에서 탐색할 때만 실행됨
  - 콜백 함수는 to, from, next를 인자로 받음

### 컴포넌트 가드

- 특정 컴포넌트 내에서 가드를 지정하고 싶을 때 사용
- 개별 컴포넌트에 `beforeRouteUpdate()` 를 사용하여 설정
  - 해당 컴포넌트를 렌더링하는 경로가 변경될 때 실행
  - 일반적인 경우 Dynamic Route Matching을 통해 설정한 params가 변경되었을 때, URL은 변하지만 페이지는 변하지 않는다. 이는 컴포넌트가 재사용되기 때문이다. 기존 컴포넌트를 지우고 새로 만드는 것보다 효율적이지만, lifecycle hook이 호출되지 않고 따라서 $route.params에 있는 데이터를 새로 가져오지 않는다.
  - 콜백 함수는 to, from, next를 인자로 받음

## 404 Not Found

- 사용자가 요청한 리소스가 존재하지 않을 때 응답
- 요청한 리소스가 존재하지 않을 때 404 페이지로 이동하도록 하려면

### 요청한 리소스가 존재하지 않는 경우

- 모든 경로에 대해 404 page로 redirect 시키기

  - 기존에 명시한 경로가 아닌 모든 경로가 404 page로 redirect 됨
  - 이때, routes 최하단부에 작성해야 함 (URL path는 코드를 위에서부터 아래로 보면서 매치되는 가장 첫 번째 라우트로 이동하게 된다.)

  ```jsx
  // router/index.js

  import NotFound404 from "@/views/NotFound404";

  const routes = [
    ...{
      path: "/404",
      name: "NotFound404",
      component: NotFound404,
    },
    {
      path: "*",
      redirect: "/404",
    },
  ];
  ```

### 형식은 유효하지만 특정 리소스를 찾을 수 없는 경우

- 예를 들어 서버에 article/1/ 로 요청을 보냈지만 1번 게시글이 삭제된 상태일 경우

  - 이때는 path: ‘\*’를 통해 404 page로 redirect되는 것이 아니라 기존에 명시한 articles/:id/ 에 대한 components가 렌더링 됨
  - 하지만 데이터가 존재하지 않기 때문에 정상적으로 렌더링 되지 않음
  - 따라서 데이터가 없음을 명시하거나 404 page로 이동해야 함

  ```javascript
  <template>
    <div>
      <p v-if="!imgSrc">{{ msg }}</p>
      <img v-if="imgSrc" :src="imgSrc" alt="img">
    </div>
  </template>

  <script>
    axios(...)
      .then(...)
      .catch((error) => {

  	    // 데이터가 없음을 명시하거나
  	    this.msg = `${this.$route.params.breed} not found`;

    		// 404 page로 이동한다.
  	    this.$router.push('/404');

    		console.error(error);
      });

  export default {
    data() {
  	  return {
  		  imgSrc: null,
  		  msg: "loading..."
  	  },
    },
    ...
  };
  </script>

  ```
