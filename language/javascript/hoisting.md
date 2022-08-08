# Hoisting
자바스크립트의 호이스팅은 자바스크립트 엔진이 스크립트를 실행하는 방식에 따라 발생되는 현상이다. <br>
자바스크립트 엔진은 런타임 이전 실행컨텍스트를 생성하는데, 이때 전역 렉시컬 환경에 전역에서 선언된 식별자를 프로퍼티로 등록한다. 이 과정에서 런타임에서 선언되기 이전에 먼저 식별자를 통해 값에 접근할 수 있게 되는, 마치 식별자가 스크립트 최상단으로 끌어올려진 것처럼 동작하는 현상이 호이스팅이다.

<br>

## 변수 호이스팅
### var 키워드 사용
var 키워드를 사용하여 선언한 변수는 변수 선언과 동시에 undefined로 초기화된다. 즉 전역 렉시컬 환경 객체에 변수명(식별자명)이 undefined를 값으로 등록된다. 그래서 스크립트 내의 변수 선언문 이전에 해당 식별자를 통해 접근하면 undefined가 나오고, 런타임에 해당 식별자에 값이 할당된다.(따라서 엄밀히 말하면 재할당이다.)

### let 키워드 사용
let 키워드를 사용하여 선언한 변수는 변수 선언은 이루어지지만 초기화는 이루어지지 않는다. 이는 let 키워드로 선언한 변수가 선언적 렉시컬 환경 객체에 값 없이 등록되기 때문이며, 이 구간을 TDZ(Temporary Dead Zone)이라고 한다. 해당 변수는 런타임에 값이 undefined로 초기화된 후 개발자가 지정한 값이 할당된다. <br> 
스크립트 내의 변수 선언문 이전에 해당 식별자를 통해 접근하면 에러가 발생하고 이 때문에 호이스팅이 되지 않는 것처럼 보이지만 그렇지 않다.

### const 키워드 사용
const 키워드를 사용하여 선언한 변수 또한 let 키워드를 사용하여 선언한 변수와 마찬가지로 선언적 렉시컬 환경 객체에 값 없이 등록되며, 런타임에 개발자가 지정한 값으로 초기화된다. (const 키워드 사용시 재할당이 불가능)

<br>

## 함수 호이스팅
### 함수 선언문 사용
함수 선언문으로 선언한 함수는 자바스크립트 엔진이 암묵적으로 함수명과 동일한 이름의 식별자를 생성하여 해당 함수를 값으로 하여 전역 렉시컬 환경 객체에 등록한다. 그래서 스크립트 내의 함수 선언문 이전에 해당 식별자(함수 이름과 동일)를 통해 함수를 호출하면 동작하게 된다. 

### 함수 표현식 사용
함수 표현식을 사용할 경우 해당 함수를 할당하는 변수 식별자에 대한 변수 호이스팅이 이루어지고 함수 표현식은 런타임에 평가되어 해당 변수 식별자에 할당된다. 