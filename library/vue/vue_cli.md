## Vue CLI Quick Start

- 설치
  `$ npm install -g @vue/cli`
- 프로젝트 생성
  `$ vue create {project name}`
  - interactive shell에서 Vue 버전 선택 (Vue2)
  - vscode terminal이 interactive shell 지원하므로 vscode 에서 진행한다.
- 서버 켜기
  `$ cd {project name}`
  `$ npm run serve`

## Vue CLI 프로젝트 구조

### node_modeules - Babel

### node_modules - Webpack

- “static module bundler”
- 모듈 간의 의존성 문제를 해결하기 위한 도구
- 프로젝트에 필요한 모든 모듈을 매핑하고 내부적으로 종속성 그래프를 빌드함

### Module

- 개발하는 애플리케이션의 크기가 커지고 복잡해지면 파일 하나에 모든 기능을 담기가 어려워짐
- 따라서 자연스럽게 파일을 여러 개로 분리하여 관리를 하게 되었고, 이때 분리된 파일 각각이 모듈(module)이다. 즉 js 파일 하나하나가 모듈임
- 모듈은 대개 기능 단위로 분리하며, 클래스 하나 혹은 특정한 목적을 가진 복수의 함수로 구성된 라이브러리 하나로 구성됨
- 여러 모듈 시스템
  - ESM(ECMA Script Module), AMD, CommonJS, UMD
- 모듈의 수가 많아지고 라이브러리 혹은 모듈 간의 의존성(연결성)이 깊어지면서 특정한 곳에서 발생한 문제가 어떤 모듈 간의 문제인지 파악하기 어려움 → Webpack은 이 모듈 간의 의존성 문제를 해결하기 위해 등장

### Bundler

- 모듈 의존성 문제를 해결해주는 작업이 Bundling
- 이러한 일을 해 주는 도구가 Bundler이고, Webpack은 다양한 Bundler 중 하나
- 모듈들을 하나로 묶어주고 묶인 파일은 하나(혹은 여러 개)로 만들어짐
- Bundling된 결과물은 개별 모듈의 실생 순서에 영향을 받지 않고 동작하게 됨
- snowpack, parcel, rollup.js 등 다양한 모듈 번들러가 존재

js runtime services → Deno 참고

node.js 개발자인 라이언 달이 만든 런타임 환경

### package-lock.json

- node_modules에 설치되는 모듈과 관련된 모든 의존성을 설정 및 관리
- 협업 및 배포 환경에서 정확히 동일한 종속성을 설치하도록 보장하는 표현
- 사용할 패키지의 버전을 고정
- 개발 과정 간의 의존성 패키지 충돌 방지

### public/index.html

- Vue 앱의 뼈대가 되는 html 파일
- Vue 앱과 연결될 요소가 있음

### src

- src/assets
  - 정적 파일을 저장하는 디렉토리
- src/components
  - 하위 컴포넌트들이 위치
- src/App.vue
  - 최상위 컴포넌트
  - public/index.html 과 연결됨
- src/main.js
  - Webpack이 빌드를 시작할 때 가장 먼저 불러오는 entry point
  - public/index.html과 src/App.vue를 연결시키는 작업이 이루어지는 곳
  - Vue 전역에서 활용할 모듈을 등록할 수 있는 파일

### Component

- UI를 독립적이고 재사용 가능한 조각들로 나눈 것
  - 즉, 기능별로 분화한 코드 조각
- CS에서는 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어 구성 요소를 의미
- 하나의 app을 구성할 때 중첩된 컴포넌트들의 tree로 구성하는 것이 보편적임
- 컴포넌트는 유지보수를 쉽게 만들어 줄 뿐만 아니라 재사용성의 측면에서도 매우 강력한 기능을 제공
- Component based architecture 특징
  - 관리가 용이 (유지보수 비용 감소)
  - 재사용성
  - 확장 가능
  - 캡슐화
  - 독립적

## SFC(Single File Component)

- Vue에서 말하는 component는 이름이 있는 재사용 가능한 Vue instance를 말한다.
- 하나의 .vue 파일이 하나의 Vue instance이고, 하나의 컴포넌트이다. 즉 single file component.
- Vue instance에서는 HTML, CSS, Javascript 코드를 한 번에 관리. 즉 Vue instance를 기능 단위로 작성하는 것이 핵심이다.

## Vue component

### Vue component 구조

- 템플릿(HTML)
  - HTML의 body 부분
  - 눈으로 보여지는 요소 작성
  - 다른 컴포넌트를 HTML 요소처럼 추가 가능
- 스크립트(Javascript)
  - Javascript 코드가 작성되는 곳
  - 컴포넌트 정보, 데이터, 메서드 등 vue 인스턴스를 구성하는 대부분이 작성됨
- 스타일(CSS)
  - CSS가 작성되며 컴포넌트의 스타일을 담당
- 컴포넌트들이 tree 구조를 이루어 하나의 페이지를 만듦
- root에 해당하는 최상단의 component가 App.vue
- 이 App.vue를 index.html과 연결하여 index.html 파일 하나만을 렌더링(SPA)

## Vue component 실습

### 컴포넌트 생성

1. src/components/ 안에 생성
2. script에 이름 등록
   - `name: {component name}`
3. template에 요소(HTML element) 추가
   - 주의. template 안에는 반드시 하나의 요소만 추가 가능 (비어 있어도 안 됨, 해당 요소 안에 추가 요소를 작성해야 함)

### 컴포넌트 등록

1. import
   - `import {instance name} from {location}`
   - instance name은 instance 생성 시 작성한 name
   - @는 src의 shortcut
   - .vue 생략 가능
2. components에 인스턴스 이름 등록
   - 상위 컴포넌트의 components에 가져올 컴포넌트를 작성한다.
3. template의 적절한 위치에 컴포넌트 추가
   - template 내의 최상위 요소 안에 닫는 태그만 있는 요소처럼 사용

## Pass Props & Emit Events

### Data in components

- pass props
  - 부모 → 자식으로의 데이터의 흐름
- emit events
  - 자식 → 부모로의 데이터의 흐름

### Pass Props

- 요소의 속성(property)을 사용하여 데이터 전달
- props는 부모(상위) 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성
- 정적인 데이터를 전달하는 경우 static props라고 명시하기도 함
- 요소에 속성을 작성하듯 사용 가능하며, `prop-data-name="value"` 의 형태로 데이터를 전달(kebab-case 사용)
  - 자식(하위) 컴포넌트에서 받을 때는 camelCase 사용
- 자식(하위) 컴포넌트는 props 옵션을 사용하여 수신하는 props를 명시적으로 선언해야 함
- 전달받은 props를 type과 함께 명시
- 컴포넌트를 문서화할 뿐 아니라, 잘못된 타입을 전달하는 경우 브라우저의 콘솔에서 사용자에게 경고
- Dynamic props
  - 변수를 props로 전달할 수 있음
  - v-bind directive를 사용해 데이터를 동적으로 바인딩
  - 부모 컴포넌트의 데이터가 업데이트되면 자식 컴포넌트로 전달되는 데이터 또한 업데이트 됨
- 컴포넌트의 data 함수
  - 각 Vue 인스턴스는 같은 data 객체를 공유한다. 따라서 dynamic props를 넘길 때는 data 키에 함수를 바인딩해서 새로운 data 객체를 반환(return)하여 사용해야 한다. (안 그러면 독립적으로 props를 관리할 수 없게 된다.)
  - [https://v2.vuejs.org/v2/guide/components.html#data-Must-Be-a-Function](https://v2.vuejs.org/v2/guide/components.html#data-Must-Be-a-Function)

### 단방향 데이터 흐름

- 모든 props는 부모에서 자식으로 즉 아래로 단방향 바인딩을 형성
- 부모 속성이 업데이트되면 자식으로 흐르지만 반대 방향은 아님
  - 부모 컴포넌트가 업데이트될 때마다 자식 컴포넌트의 모든 props가 최신 값으로 새로고침됨
- 하위 컴포넌트에서 props를 변경하려고 시도하면 Vue 콘솔에서 경고가 출력됨

### Emit Events

- 자식 컴포넌트에서 부모 컴포넌트로 데이터를 전달하고자 할 때는 이벤트를 발생시킨다.
  - 데이터를 이벤트 리스너의 콜백함수의 인자로 전달
  - 상위 컴포넌트는 해당 이벤트를 통해 데이터를 받음
- $emit 메서드
  - $emit(’event-name’) 형식으로 사용하며, event-name이라는 이벤트를 발생시키고(트리거) 부모 컴포넌트에 event-name이라는 이벤트가 발생했다는 것을 알림
  - 이벤트를 발생(emit)시킬 때 인자로 데이터를 전달 가능하고, 이렇게 전달한 데이터는 이벤트와 연결된 부모 컴포넌트의 핸들러 함수의 인자로 사용 가능
- emit event 흐름
  1. 자식 컴포넌트에 있는 버튼 클릭 이벤트를 청취하여 연결된 핸들러 함수 호출
     - 굳이 버튼이 아니어도 된다. 이는 예시임.
  2. 호출된 함수에서 $emit을 통해 이벤트 발생시킴. 이때 이벤트에 데이터를 함께 전달 가능
     - 두 번째 인자부터는 데이터
  3. 상위 컴포넌트는 자식 컴포넌트가 발생시킨 이벤트를 청취하여 연결된 핸들러 함수 호출, 함수의 인자로 전달된 데이터가 포함되어 있음
  4. 상위 컴포넌트에서 호출된 핸들러 함수에서 인자로 받은 데이터를 사용 가능
- emit with dynamic data
  - pass props와 마찬가지로 동적인 데이터도 전달 가능
- emit event with dynamic data 흐름
  1. 자식 컴포넌트에 있는 keyup.enter 이벤트를 청취하여 연결된 핸들러 함수 호출
     - 이벤트 종류(keyup)는 예시임
  2. 호출된 함수에서 $emit을 통해 이벤트 발생시킴. 이때 이벤트에 v-model로 바인딩된 입력받은 데이터를 함께 전달
  3. 상위 컴포넌트는 자식 컴포넌트가 발생시킨 이벤트를 청취하여 연결된 핸들러 함수 호출, 함수의 인자로 전달된 데이터가 포함되어 있음
  4. 상위 컴포넌트에서 호출된 핸들러 함수에서 인자로 받은 데이터를 사용 가능
