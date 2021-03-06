## 4장 변수와 스코프, 메모리
- 변수의 원시 값과 참조 값
- 실행 컨텍스트의 이해
- 가비지 컬렉션의 이해


### 4.1 원시 값과 참조 값
ECMAScript의 변수는 원시 값과 차조 값 두 가지 타입의 데이터를 저장할 수 있다.
`원시 값`은 단순한 데이터이며 `참조 값`은 여러 값으로 구성되는 객체를 가리킨다.

참조 값은 메모리에 저장된 객체를 의미하며,
여타 언어와는 달리 자바스크립트는 메모리 위치에 직접 접근하는 것을 허용하지 않으므로 객체의 메모리 공간을 직접 조작하는 일은 불가능하다.
객체를 조작할 때는 사실 객체 자체가 아니라 해당 객체에 대한 `참조`를 조작하는 것이다.

> 이런 이유로 객체를 가리키는 값은 `참조로 접근한다`고 한다.

++참고로 다른 언어들은 대두수 문자열을 객체로 표현됨므로 참조타입으로 간주한다. (자바스크립트는 문자열은 원시타입으로 간주)++


#### 4.1.1 동적 프로퍼티 

참조 값을 다룰 때는 언제든 프로퍼티와 메소드를 추가하거나 바꾸고 삭제 할 수 있다.

```javascript
var person = new Object();
person.name = "Nicholas";
console.log(person.name);    //"Nicholas"
```

위 예제에서는 객체를 생성한 후 person이란 변수에 저장했다.
다음에는 name이란 프로퍼티를 추가하고 문자열 값을 할당했다.
이 시점부터 객체가 파괴되거나 프로퍼티를 명시적으로 제거하기 전까지는 해당 프로퍼티에 접근할 수 있다.

#### 4.1.2 값 복사

원시 값과 차조 값은 저장되는 방식 외에도 변수에서 다른 변수로 값을 복사할 때도 다르게 동작한다.
원시 값을 다른 변수로 복사할 때는 현재 저장된 값을 새로 생성한 다음 새로운 변수에 복사한다.

```javascript
var num1 = 5;
var num2 = num1;
```

1. 여기에서 num1에는 값 5가 저장되어 있다.
2. num2를 num1로 초기화하면 num2에도 값 5가 저장된다.
3. 이 값은 복사된 것이믐로 num1에 저장된 값과는 완전히 분리되어있다.

> 즉 num2값을 변경하더라도 num1과는 아무 상관이 없다.

반면,
참조 값을 변수에서 다른 변수로 복사하면 원래 변수에 들어있던 값이 다른 변수에 복사되기는 마찬가지다.
차이는 그 값이 **객체 자체가 아니라 `힙(heap)`에 저장된 객체를 가리키는 포인터라는 점이다.**
조작이 끝나면 두 변수는 정확히 같은 객체를 가리킨다.
따라서 다음 예제처럼 한쪽을 조작하면 다른 쪽에도 반영된다.

```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name);	//Nicholas
```


#### 4.1.3 매개변수 전달.
ECMAScript의 함수 매개변수는 모두 값으로 전달된다. 함수 외부에 있는 값은 함수 내부의 매개변수에 복사되는데, 이는 변수 사이에서 값을 복사하는 것과 마찬가지다

값이 원시 값이라면 변수 사이에서 원시 값을 복사하는 것과 마찬가지이며 참조 값일 때도 변수 사이에서 참조 값을 복사하는 것과 마찬가지다.

> 개발자들이 자주 혼란스러워 하는 부분이므로 주의하자

매개변수를 값 형태로 넘기면 해당 값은 지역변수에 복사된다.
즉 이르 붙은 매개변수로 복사되며 ECMAScript에서는 arguments 객체의 한 자리를 차지한다.
매개변수를 참조형태로 넘기면 메모리 상의 값의 위치가 지역 변수에 저장되므로 지역 변수를 변경하면 함수 바깥에도 해당 변경 내용이 반영된다.
다음예제를 보자

```javascript
function addTen(num) {
	num += 10;
	return num;
}

var count = 20
var result = addTen(count);
console.log(count);    //20
console.log(result);   //10
```

함수 addTen()은 매개변수 num을 넘겨받는데 이 변수는 기본적으로는 지역변수다.
이 코드를 실행하면 변수 count가 매개변수로 전달된다.
변수의 값은 20이었는데 이 값이 매개변수 num에 10을 더해 값을 바꾸지만 함수 외부에 있는 변수 count는 바뀌지 않는다.
매개변수 num과 변수 count는 이제 상관없는 값이다. 
만약 num을 참조로 전달했다면 함수 내부의 변화를 반영해 count의 값도 30으로 바뀌었을 것이다.

이런 사실은 숫자 같은 원시 값에서는 분명히 드러나지만 객체에서는 그닥 명확하지 않다. 예를 들어 다음의 코드를 보자

```javascript
function setName(obj){
	obj.name="Nichols";
}
var person = new Object();
setName(person);
alert(person.name); // "Nicholas"
```

이 코드에서는 객체를 만들어 변수 person에 저장했다.
이 객체를 setName()함수에 넘기면 함수는 해당 객체를 obj에 복사한다.
함수 내부에서는 obj와 person이 모두 같은 객체를 가리킨다.
결과적으로 obj는 함수에 값 형태로 전달되었지만 참조를 통해 객체에 접근한다.
함수 내부에서 obj객체에 name 프로퍼티를 추가하면 하수 외부의 객체에도 반영되는데 obj가 가리키는 것은 힙에 존재하는 전역 객체이기 때문이다. 

> 흔히 많은 개발자들이 함수 안에서 객체를 조작했을때 외부에도 반영됨을 보고 매개변수가 참조 형태로 전달되었다고 오해한다.
객체가 값으로 전달됨을 명확히 하기 위해 이전 코드를 다음과 같이 수정해보자

```javascript
function setName(obj){
	obj.name="Nicholas";
	obj = new Object();
	obj.name = "Greg";
}

var person = new Object();
setName(person);
alert(person.name);
```

이 예제가 이전과 다른 점은 setName()에 코드를 두 줄 추가해서 obj를 새로운 객체로 재정의했다는 점인데.
만약 person이 참조로 전달됐다면 person이 가리키는 객체는 자동으로 name 프로퍼티가 "Greg"인 새 객체로 바뀌었을 것이다.
하지만 person.name에 다시 접근하면 그 값은 "Nicholas"다.

함수에 값을 전달했기 때문에 함수 내부에서 매개변수의 값이 바뀌었음에도 불구하고 원래 객체에 대한 참조를 그대로 유지한 것이다.
**함수 내부에서 obj를 덮어쓰면 obj는 지역객체를 가리키는 포인터가 되며 이 지역객체는 함수가 실행을 마치는 즉시 파괴된다.**

#### 4.1.4 타입판별
이전 장에서 소개한 typeof연산자는 변수가 원시타입인지 파악하기에 최상이다.
만약 값이 객체이거나 null이면 typeof는 `object`를 반환한다.

이처럼 `typeof`는 원시 값에 대해서는 잘 동작하지만 참조 값에 대해서는 별 쓸모가 없다.
이럴때 사용하는 것이 instanceof 연산자는 이런 상황에 도움이 되며 다음 문법에 따라 사용한다.

`'result' = 'variable' instanceof 'constructor'`

instanceof 연산자는 변수가 주어진 참조 타입의 인스턴스일 때 (프로토타입 체인으로 판단) true를 반환한다.

```javascript
alert(person instanceof Object);	//person 함수가 Object의 인스턴스인가?
alert(colors instanceof Array);	//colors 변수가 Array의 인스턴스인가?
alert(pattern instanceof RegExp);	//pattern 변수가 RegExp의 인스턴스인가?
```

### 4.2 실행 컨텍스트와 스코프

`실행 컨텍스트(execution context)`는 짧게 '컨텍스트'라고 부르며 자바스크립트에서는 비할 바 없이 중요한 개념이다.
변수나 함수의 실행 컨텍스트는 다른 데이터에 접근할 수 있는지, 어떻게 행동하는지를 규정한다.
각 실행 컨텍스트에는 변수 객체(variable object)가 연결되어 있으며 해당 컨텍스트에서 정의된 모든 변수와 함수는 이 객체에 존재한다.
이 객체를 코드에서 접근할 수는 없지만 이면에서 데이터를 다룰 때 이 객체를 이용한다.

**가장 바깥쪽에 존재하는 실행 컨텍스트는 전역 컨텍스트다.**
ECMAScript를 구현한 환경에 따라 이 컨텍스트를 부르는 이름이 다르다.
웹브라우져는 이 컨텍스트를 `window`라 부르므로 전역 변수와 함수는 모두 window 객체의 프로퍼티 및 메소드로 생성된다.

- 실행 컨텍스트는 포함된 코드가 모두 실행될 때 파괴된다.
- 해당 컨텍스트 내부에서 정의된 변수와 함수도 함께 파괴된다.
- 전역 컨텍스트는 애플리케이션이 종료될때, 예를 들어 웹페이지에서 나가거나 브라우져를 닫을 때 까지 계속 유지된다.
- 함수를 호출하면 독자적인 실행 컨텍스트가 생성된다.
- 코드 실행이 함수로 들어갈 때마다 함수의 컨텍스트가 컨텍스트 스택에 쌓인다.
- 함수 실행이 끝나면 해당 컨텍스트를 스택에서 꺼내고 컨트롤을 이전 컨텍스트에 반환한다.

> 컨텍스트에서 코드를 실행하면 변수 객체에 `스코프 체인(scope chain)`이 만들어 진다.

이 스코프 체인의 목적은 실행 컨텍스트가 접근할 수 있는 모든 변수와 함수에 순서를 정의하는 것이다.

1. 스코프 체인의 앞쪽은 항상 코드가 실행되는 컨텍스트의 변수 객체다.
2. 변수객체 다음 순서는 해당 컨텍스트를 포함하는 컨텍스트(부모컨텍스트)이다.
3. 그 다음에는 다시 붐모의 부모 컨텍스트로 순서가 진행된다..

위와 같은 순서로 계속 진행하여 전역 컨텍스트에 도달할 때 까지 계속한다.
즉 전역 컨텍스트의 변수객체는 항상 스코프 체인의 마지막에 존재한다.

```javascript
var color = "blue";

function changeColor(){
	var anotherColor = "red";
	
	function swapColors(){
		var tempColor = anotherColor;
		anotherColor = color;
		color = tempColor;
		// color, anotherColor, tempColor 모두 접근 가능
	}
	// color, anotherColor 접근가능 , tempColor 는 불가능
	swapColors();
}
//color 만 접근가능
changeColor();
```

위 예제에는 실행 컨텍스트가 세 개 있다. 전역 컨텍스트, changeColor()의 로컬컨텍스트, swapColors()의 로컬 컨텍스트 세 개다

1. 전역컨텍스트에는 color변수와 changeColor()함수가 포함
2. changeColor()의 로컬 컨텍스트에는 anotherColor 변수와 swapColors()만 있지만 전역 컨텍스트의 color변수에도 접근가능하다
3. .....

각 컨텍스트는 스코프 체인을 따라 상위 컨텍스트에서 변수나 함수를 검색할 수 있지만 스코프 체인을 따라 내려가며 검색할 수는 없다.

#### 4.2.1 스코프 체인 확장
실행 컨텍스트에는 전역 컨텍스트와 함수 컨텍스트 두 가지 타입만 있지만(eval()을 호출할 때 생성되는 세 번째 타입이 있긴 하다.)
스코프 체인을 확장할 수 있는 방법도 있다.

특정 문장은 스코프 체인 앞부분에 임시로 변수 객체를 만들며 변수 객체는 코드 실행이 끝나면 사라진다.
이렇게 임시 객체가 생성되는 경우는 다음 두 가지다.
- try ~catch 문의 catch 블록
- with ㅁ문

두 문장은 모두 스코프 체인 앞에 변수 객체를 추가한다.

#### 4.2.2 자바스크립트에는 블록 레벨 스코프가 없다.
자바스크립트에는 블록레벨 스코프가 없는데 이는 종종 혼란을 일으킨다.

보통 for 문같은 경우는 다음과 같이 사용하는데

```javascript
for( var i=0; i<10; i++){
	doSomething(i);
}

alert(i); //10
```
블록 레벨 스코프를 지원하는 언어에서는 for 문의 초기화 부분에서 선언한 변수가 오직 for 문의 컨텍스트 안에서만 존재한다.
자바스크립트에서는 for 문에서 생성한 i 변수가 루프 실행이 끝난 후에도 존재한다.

**변수 선언 **
`var`를 사용해 선언한 변수는 자동으로 가장 가까운 컨텍스트에 추가된다.
함수 내부에서는 함수의 로컬 컨텍스트가 가장 가까운 컨텍스트이며 `with 문` 내부에서는 함수 컨텍스트가 가장 가까운 컨텍스트이다.

즉 `var`을 선언하지 않으면 전역 컨텍스트에 생성이 된다는 뜻

**식별자 검색**
컨텍스트 안에서 식별자를 참조하려 하면 먼저 검색부터 해야 한다.
검색은 스코프 체인 앞에서 시작하며 주어진 이름으로 식별자를 찾는다.

1. 로컬 컨텍스트에서 식별자 이름을 찾으면 검색을 멈추고 변수를 설정한다.
2. 만약 찾지 못한다면 스코프 체인을 따라 검색을 계속한다.
3. 전역 컨텍스트의 변수 객체에 도달할 때까지 이 과정을 반복한다.
4. 전역 컨텍스트의 변수 객체에서도 식별자를 차지 못하면 정의 되지 않은것으로 판단한다.


다음의 코드를 통해 식별자 검색이 어떻게 이루어지는지 생각해보자

```javascript
var color = "blue";
function getColor(){
	return color;
}
alert(getColor());	// blue
```

먼저 getColor()의 변수 객체에서 color라는 식별자 이름을 검색하고, getColor()의 변수 객체에서 찾지 못하면 다음(전역 컨텍스트) 변수 객체에서 color 식별자를 검색한다.

검색과정을 보면 지역변수를 참조할 때는 다음 변수를 검색하지 않도록 자동으로 검색을 멈춤을 알 수 있는데
달리 말해 다음과 같이 식별자가 로컬 컨텍스트에 정의되어 있으면 부모 컨텍스트에 같은 이름의 식별자가 있다 해도 참조 할 수 엇ㅂ다


```javascript
var color = "blue";
function getColor(){
	var color = "red"
	return color;
}
alert(getColor());	// red
```

가령 위와같이 getColor() 안에 지역변수 color를 설정해놓으면 로컬컨텍스트에서 검색을 마치고 전역 변수로는 접근하지 않는다.
위 소스에서 만약 전역 컨텍스ㅏ트의 color변수를 이용하려면 `window.color` 라고 명시해야 한다.

> 변수 검색에도 비용이 들어가게 되는데 지역변수는 스코프 체인을 따라 올라가며 검색할 피룡가 없으므로 전역 변수보다 빨리 검색된다.
> 자바스크립트 엔진에서 식별자 검색을 최적화하고 있으므로 미래에는 이런 차이가 무시할만한 정도로 좁혀질 수도 있다.


### 4.3 가비지 콜렉션
자바스크립트는 실행환경에서 코드 실행중에 메모리를 관리하는데 이런의미에서 가비지 콜렉션 언어라고 불러도 된다.

자바스크립트는 필요한 메모리를 자동으로 할당하고 더 이상 사용하지 않는 메모리는 자동으로 회수하므로 개발자가 직접 메모리를 관리하지 않아도 된다.

그 개념은 아주 단순한데, 
> 더 이상 사용하지 않을 변수를 찾아내서 해당 변수가 차지하고 있던 메모리를 회수하는 것이다.

이 프로세스는 주기적으로 실행되는데 코드 실행 중에 특정 시점에서 메모리를 회수하도록 지정할 수도 있다.

**함수의 지역 변수를 통해 생각해보자**

1. 함수를 실행하면 변수가 생성된다.
2. 해당 시점에서 스택에 변수의 값을 저장할 메모리가 할당된다.(힙에도 할당될 수 있다.
3. 변수는 함수안에서만 존재하므로 함수가 종료되면 변수는 더 이상 필요하지 않고 , 따라서 해당 변수가 차지하고 있던 메모리를 회수해 다른 용도로 쓸 수 있다.

위와 같은 상황이라면 분면 해당 변수가 더 이상 필요 없지만 이렇게 분명하지 않은 경우도 많다.
> 즉 분명하지 않은 상황에서 `가비지 콜렉터`는 어떤 변수가 더 이상 사용되지 않는지, 사용될 가능성이 있는 변수는 무엇인지 추적해야 메모리 회수 대상을 정할 수 있다.
더 이상 사용하지 않는 변수를 식별하는 기준은 브라우저마다 다르지만 보통 두 가지 방법을 흔히 채용한다.

#### 4.3.1 표시하고 지우기

자바스크립트에서 가장 널리 쓰이는 가비지 컬렉션 방법은 `표시하고 지우기(mark-and-sweep)`라 불린다.

> 변수가 특정 컨텍스트 안에서 사용할 것으로 사용할 것으로 정의되면(예를 들어 함수 안에서 변수를 정의하면) 그 변수는 그 컨텍스트 안에 있는 것으로 표시된다.
> 컨텍스트 안에 존재하는 변수의 메모리는 해제해서는 안되는데, 해당 컨텍스트가 실행 중인 한 사용될 가능성이 있기 때문이다.
> 변수가 컨텍스트 밖으로 나가면 컨텍스트 밖에 있는 것으로 표시된다.

변수에 표시하는 방법은 다양한데,
변수가 컨텍스트 안에 있을 때 특정 비트를 `on` 상태로 표시할 수도 있고, "컨텍스트 내부"와 "컨텍스트 외부"를 나타내는 변수 목록을 따로 두어서 변수의 움직임을 추적할 수도 있지만, 이를 어떻게 구현 했느냐는 중요하지 않은 탁상공론일 뿐이다.

가비지 컬렉터가 작동하면 메모리에 저장된 변수 전체에 표시를 남긴다.
그 다음 컨텍스트에 있는 변수와, 컨텍스트에 있는 변수가 참조하는 변수에서 표시를 지운다. 이 과정을 거친 다음에도 표시가 남아 있는 변수는 컨텍스트에 있는 변수와 무관하므로 삭제해도 안전하다. 






