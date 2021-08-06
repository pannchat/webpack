# webpack
웹팩이 뭔데
웹팩은 최신 프론트엔드 프레임워크에서 가장 많이사용되는 모던 Javascript 애플리케이션을 위한 정적 모듈 번들러입니다. 

![image](https://user-images.githubusercontent.com/49192400/128522528-910ac163-84d0-4178-942b-dbca5a99efe6.png)

### 모듈 번들러란?

모듈 번들러는 웹 애플리케이션을 구성하는 웹 구성 자원들(HTML,CSS,JS,Images 등)을 모두 각각의 모듈로 보고 이들의 의존성을 묶고 조합해 합쳐진 하나의 결과물(static한 자원)을 만드는 도구입니다.

즉 js, sass, jpg, png, css 같은 많은 자원들을 하나의 파일로 병합하고 압축해주는 동작을 모듈 번들링이라고 합니다.

> **모듈이란?** 
모듈은 단지 파일 하나에 불과 합니다. 모듈은 대개 클래스 하나 혹은 특정한 목적을 가진 복수의 함수로 구성된 라이브러리 하나로 구성됩니다.
각 모듈은 전체 프로그램보다 영향 범위가 좁아 검증 및 디버깅 테스트가 간단합니다.

웹팩에 대해 알아보기전에 모듈에 대해 알아봅니다.

## 모듈의 표준

처음 자바스크립트에는 모듈을 가져오거나 내보내는 방법이 없어 하나에 모든 기능이 들어가야 했습니다. CJS, AMD, ESM 등등 이 등장한 후에는 모듈로 개발, 배포가 가능해졌습니다.

모듈을 사용하는 방법은 모듈이 어떤 표준을 따르냐에 따라 달라지게 됩니다. 모듈시스템은 여러가지가 있지만 몇개만 알아봅니다.

1. CJS(CommonJS)

    → 서버 사이드 자바스크립트 런타임 환경인 Node.js는 모듈 시스템의 사실상 표준인 CommonJS를 채택했고 독자적인 진화를 거쳐 CommonJS와 100%동일 하진 않지만 기본적으로 CommonJS를 방식을 따르고 있습니다.

2. AMD(Asynchronous Module Definition)

    → CommonJS는 모든 파일이 로컬디스크에 있어 필요할 때 바로 불러올 수 있는 상황을 전제로 하기때문에 동기적인 동작이 가능한 서버사이드 자바스크립트 환경을 전제로합니다. 이는 브라우저에서 필요한 모듈이 모두 다운로드 될 때까지 아무것도 할 수 없는 상태가 되어 치명적인 단점이 될 수 있습니다.

    AMD 그룹은 비동기 상황에서 자바스크립트 모듈을 사용하기위해 CommonJS와 함께 논의하다 합의점을 이루지 못하고 독립한 그룹입니다. AMD는 필요한 모듈을 네트워크를 이용해 내려받아야 하는 브라우저 환경에서도 모듈을 사용할 수 있도록 표준을 만드는 것을 목표로하고 있습니다

3. ESM( ECMAScript 2015~ )

    → ESM은 ECMAScript에서 지원하는 자바스크립트 공식 모듈 시스템입니다. 아직 브라우저에서  import와 export를 지원하지 않아 번들러를 함께 사용해야합니다.


### 모듈 내보내기

1. 내보낼 값들을 객체 안에 넣고 객체를 통해 한번에 내보내는 방법
2. 내보낼 값들을 개별적으로 키워드를 지정해서 키워드가 정의되어있는 값들만 내보내는 형태


### 모듈 가져오기

모듈도 하나의 객체로 취급됩니다.

1. 내보낼 값을 객체에 넣고 객체를 통해 한번에 내보내기
2. 내보낼 값들에 개별적으로 키워드를 지정해서 키워드가 정의되어있는 값들만 내보냄.


### CommonJS의 가져오기와 내보내기

CommonJS 에서 모듈을 가져올 때는 require라는 키워드를 사용합니다.

`require(모듈의 경로);` 형태로 사용됩니다.

모듈을 내보낼 때는 module이라는 객체를 사용합니다. CommonJS를 따르는 환경에서는 전역으로 접근할 수 있는 module이라는 객체가 제공됩니다.

`module.exports = {...}`

내보낼 값들을 모두 한 객체에 할당하는 방식

`module.exports = 값`

객체가 아닌 하나의 상수값이나 함수를 바로 할당할 수도 있고

`module.exorts.키_이름 = 값`

직접 키를 지정하고 키마다 각각의 값을 할당할 수도 있습니다.

`exports.키_이름 = 값`

이것은 module.exports의 축약형이라고 볼 수 있습니다.


### Basic Setup

먼저 예제를 실습할 디렉터리를 생성합니다. 그리고 npm을 초기화합니다.

```jsx
$ npm init -y
```

```jsx
$ npm install webpack webpack-cli --save-dev
```

그리고 webpack을 로컬로 설치하고 `wepback-cli` 라는 (Command Line Interface에서 webpack을 실행할 때 사용되는 도구) 를 설치해줍니다.

`index.js` 을 하나 생성해서 아래 예제를 실습.

```jsx
//index.js
const PI = 3.14;
const getCircleArea = r => r * r * PI;
const result = getCircleArea(2);

console.log(result);
```

간단한 수학공식이 정의되어 있는 파일이 있습니다. 코드가 길지 않고 복잡하지 않을 경우 한 파일로 관리할 수 있겠지만 복잡하고 긴 코드라면 모듈화 시켜서 관리하는 것이 좋습니다.

PI, getCircleArea 를 `mathUtil.js` 로 분리해봅니다. 

```jsx
//mathUtil.js
const PI = 3.14;
const getCircleArea = r => r * r * PI;

// 1. 내보낼 값을 한 객체에 할당하여 내보내기
module.exports = {
    PI,
    getCircleArea,
}

```

```jsx
//index.js
const {getCircleArea} = require('./mathUtil');

const result = getCircleArea(2);

console.log(result);
```

`require(모듈의 경로);` 에서 .js 는 생략 가능합니다.

그리고 object destructuring 으로 getCircleArea만 추출할 수 있습니다.

```jsx
// 2. 직접 키를 지정하고 키마다 각각의 값을 할당
exports.PI = PI;
exports.getCircleArea = getCircleArea;
```

위의 방법을 통해서도 내보내기가 가능합니다.


## ESM Keyword
---

Nodejs 환경에서는 CommonJS 표준을 따르기 때문에 모듈을 다른 방식으로 접근할 수 있게 도와주는 도구가 필요합니다.

```
$ npm install esm
```

esm을 설치해준다. esm 모듈이 설치가 되었으면 명령문을 실행할 때

```
$ node -r esm index.js
```

-r 이라는 플래그는 node 명령어를 실행할 때 실행하는 파일의 다른 모듈의 표준도 해석할 수 있게 실행해줍니다.



### 내보내기

`export` 혹은 `export default` 를 사용합니다.

> export, export default의 차이?

모듈은 크게 두 종류로 나눌 수 있습니다.

여러 개의 함수가 있는 라이브러리 형태의 모듈과, 개체 하나만 선언되어있는 모듈

`export default` 를 사용하면 해당 모듈엔 개체가 하나만 있다는 사실을 명확히 나타낼 수 있습니다

우선 `export` 부터 살펴볼까요?

1. `export` 로 내보내기

```jsx
//index.js
const PI = 3.14;
const getCircleArea = r => r * r * PI;

export {
    PI,
    getCircleArea
}

```

이 모듈에는 PI와 getCircleArea라는 변수와 함수 표현식이 있습니다. export를 사용해서 내보내 봅니다. 내보낼 목록을 만들어 객체 형태로 내보냅니다.

1. `export default`

```jsx
//index.js
const PI = 3.14;
const getCircleArea = r => r * r * PI;

export default{
    PI,
    getCircleArea
}
//export default getCircleArea;

```

`export default` 를 사용하면 하나의 클래스나 함수, 객체를 내보내고 불러올 때도 마찬가지로 중괄호 없이 모듈을 통째로 가져오게 됩니다.

export default는 파일당 최대 하나만 사용 가능합니다. 따라서 내보낼 개체에는 이름이 없어도 상관없습니다.


### 가져오기

`import 모듈_이름 from 모듈_위치`

가져오기는 기본적으로 import 모듈 이름과 from 모듈의 파일 위치 형태로 사용합니다.

1. `export`

```jsx
//mathUtil.js
import {getCircleArea} from './mathUtil';
//import {PI, getCircleArea} from './mathUtil';
const result = getCircleArea(2);

console.log(result);

```

`export`로 내보내진 객체는 목록을 만들어 개별적으로 가져오기가 가능합니다.

1. `export default`

```jsx
//mathUtil.js
import mathUtil from './mathUtil';
const result = mathUtil.getCircleArea(2);

console.log(result);

```

export default로 내보낸 모듈은 {}를 사용하지 않고 이름을 사용합니다. 그리고 해당 객체의 메서드에 직접 접근하여 사용합니다.

## 모듈의 종류

모듈의 종류는 총 3가지로 분류할 수 있습니다.

1. Built-in Core Module ( ex. Node.js module )
2. Community-base Module ( ex. NPM )
3. Local Module ( 특정 프로젝트에 정의된 모듈 )

### Built-in module

Node.js에 built-in 되어있는 core module은 웹 브라우저에서 사용되는 javascript 보다 더 많은 기능을 제공합니다. 운영 체제 정보 접근이나 클라이언트가 요청한 주소에 대한 정보를 가져오는 등 여러 기능의 모듈을 제공합니다.

OS모듈, path 모듈

### Community-base Module

NPM은 Node.js 개발자들이 만든 다양한 모듈을 공유하고 사용할 수 있게 해주는 패키지 저장소, 커뮤니티입니다. Node.js에서 제공하는 내장 모듈도 있고, 사용자들이 직접 만든 유저 모듈들도 있습니다.

NPM은 `Website`를 통해 패키지와 관련 문서를 찾거나 공유할 수 있고 `CLI` 를 통해 패키지에 대한 업데이트나, 설치, 제거 등의 관리를 할 수 있습니다. `npm registry` 에는 자바스크립트 소프트웨어와 메타정보들이 들어있고 현재 100만 개 이상의 패키지가 존재합니다.

### Local Module

우리가 이전 예제에서 만든 mathUtil.js 모듈과 같이 로컬 환경에서 만든 모듈을 의미합니다.

## 모듈을 사용할땐

모듈을 사용하면 코드의 재사용성이 증가하고, 코드의 관리가 편해집니다. 그리고 코드를 모듈화 할때는 모듈화하는 기준이 명확해야합니다.




## Bundle이 중요한 이유?

[Bundle이란?](https://pannchat.tistory.com/entry/Webpack-%EC%9D%B4-%EB%AD%94%EB%8D%B01) 

번들은 웹팩에 대해 이해할 때 알아봤습니다. 이번에는 번들을 왜 사용하는지, 이게 왜 중요한지에 대해 알아봅니다.

1. 각 모듈을 요청해서 자원을 얻어와야 했던 것들을 묶어서 요청/응답 을 받기 때문에 파입 접근 횟수가 줄어들고 모듈 로드를 위해 검색하는 시간,  network cost가 줄어듭니다.
2. **Webpack 4** 이상의 버전부터는 development, production 모드를 지원합니다. production 모드에서는 로드 시간을 줄이기 위한 번들 최소화, 가벼운 소스맵 및 최적화에 초점을 맞추고 번들링을 진행합니다. 즉 코드를 `압축`해주고 `난독화`, `최적화(tree shaking)` 작업을 지원합니다.

    `tree shaking`: 사용되지 않는 코드를 제거

## Webpack이 뭔데!!

![image](https://user-images.githubusercontent.com/49192400/128523174-c6f295ad-cf56-4a89-ac56-b1f4fd27209c.png)

### Webpack 기본구조

![image](https://user-images.githubusercontent.com/49192400/128523196-973f9fc5-cfb8-42bd-8cba-47383b458910.png)

위의 예제처럼 index.js는 bar.js에 의존성을 갖습니다. 이런것들 처럼 의존성을 갖는 파일들을 안전하게 유지시키면서 하나의 파일로 만드는 번들링 과정을 거칩니다.

## Webpack의 4가지 주요 속성

- Entry
- Output
- Loader
- Plugin

## Entry

`entry` 속성은 웹팩에서 웹 자원을 변환하기 위한 최초 진입점입니다. 쉽게 말해서 복잡한 참조관계를 가진 모듈들 중에서 어떤 모듈에 시작점을 두고 해석해야하는지 웹팩에 알려줍니다. webpack은 entry point가 의존하는 다른 모듈과 라이브러리를 찾아내고 디펜던시 그래프를 생성합니다.

![image](https://user-images.githubusercontent.com/49192400/128523432-f047b59d-6db8-40ce-be04-fd90897911c1.png)
이미지 출처 : fastcampus webpack 강의

이 그림에서 참조관계에서 가장 상위에있는 모듈 A를 `entry point`로 지정하면 webpack이 모듈 A를 시작점으로 모듈 A가 갖고있는 다른 모듈들과의 의존관계를 해석해서 bundle파일을 만듭니다.

```jsx
// webpack.config.js
module.exports = {
  entry: './src/index.js'
}
```

기본값으로 `./src/index.js` 이지만, webpack 설정에서 entry 속성을 설정하여 다른, 혹은 여러 엔트리 포인트를 지정할 수 있습니다.

```jsx
entry: {
  login: './src/LoginView.js',
  main: './src/MainView.js'
}
```

entry 속성을 정의하는 방법은 여러가지가 있습니다. 아래 링크를 참조해보세요.

[엔트리 포인트 자세히 보기](https://webpack.kr/concepts/entry-points/)

### Output

Webpack이 생성하는 번들 파일에 대한 정보를 설정합니다. `output` 속성은 생성된 번들을 내보낼 위치와 파일의 이름을 지정하는 방법을 webpack에 알려줍니다. 기본적으로 출력파일은 `./dist/main.js` 로, 생성된 기타 파일의 경우 `./dist` 폴더로 설정됩니다.

```jsx
// webpack.config.js
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
```

객체형태로 옵션들을 추가해야합니다.

```jsx
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
};
```

`path` 속성은 해당 파일의 경로를 의미하고 `filename` 속성은 웹팩으로 빌드한 파일의 이름을 의미합니다. `output` 프로퍼티는 최소한의 객체로 값을 설정해야하며, 최소한 `output.filename` 은 지정돼야 합니다.

`output` 옵션을 설정하여 컴파일된 파일을 디스크에 쓰는 방법을 webpack에 알려주는데 `entry point`는 여러개가 될 수 있지만 하나의 `output` 설정만 지정 가능합니다.

[이전 예제](https://www.notion.so/Webpack-57378a3cae044e19964108b337c544d1) 에서 만들었던 2개의 파일로 테스트 해봅니다.

`npm init -y` 명령어를 통해 `package.json` 하나가 생성 되었을 것 입니다.

이 json파일에는 name이라는 property에 이 프로젝트의 이름과 version 등등 다양한 속성이 있습니다.

`webpack` 과 `webpack-cli` 의 설치가 되었다면 `node_modules` 와 `package.json` 의 property에 `"devDependencies"` 가 추가된 것을 확인할 수 있습니다. 확인을 마치고

현재 디렉터리 아래에 `index.js` 와 `mathUtil.js` 를 `src` 디렉터리를 하나 생성해서 옮겨줍니다.

```jsx
$ npx webpack
```

명령어를 통해 webpack 실행시켜줍니다. `npx`는 node_modules아래에 있는 .bin 디렉터리의 실행파일을 직접 접근하지 않고도 쉽게 사용할 수 있게해줍니다.

실행이 정상적으로 완료되었다면 dist 디렉터리 아래에 main.js가 생성됩니다. 앞서 학습했던 entry와 output 설정 없이 어떻게 실행되었을까요?

webpack이 4 버전으로 올라가면서 development 모드와 production 모드가 생겼습니다. 이런 모드가 생기면서 설정파일 없이도 빌드가 가능하게 되었는데 이를 0CJS (Zero configuration Javascript) 라고 합니다. 일반적으로 자주 사용되는 웹팩 설정들을 기본적으로 적용시켜주는 편리함을 제공합니다.

번들링되어 나온 `main.js` 파일을 보면 기본적으로 코드 난독화, 압축, 최적화 된다는 것을 알 수 있습니다.

```jsx
$ node ./dist/main.js
```

`main.js` 를 실행해서 잘 작동하는지 확인해보세요.

webpack 4 부터는 zero config를 제공하긴하지만 대부분 프로젝트에서 더 복잡한 설정이 필요합니다. 따라서 설정 파일을 통해 터미널에서 많은 명령어를 통해 수동으로 입력하는 것보다 효율적으로 관리할 수 있습니다.

```jsx
//webpack.config.js
const path = require('path');

module.exports ={
    entry: './src/index.js',
    output:{
        path : path.resolve(__dirname, './dist'),
        filename: 'bundle.js'
    },
};
```

`webpack.config.js` 파일을 만들어 줍니다. entry 는 /src/index.js를 사용할 것이고 dist라는 디렉터리에 bundle.js 라는 파일로 output 할 것을 의미합니다.

`__dirname` 은 해당 파일이 있는 위치의 절대경로를 나타냅니다. 그리고 `path.resolve()` 는 인자로 넘어온 경로들을 조합해서 유효한 파일 경로로 만들어주는 Node.js API입니다.

webpack은 `webpack.config.js` 이 있으면 기본으로 이것을 선택합니다. `--config` 옵션을 사용하면 다른 이름의 config 파일도 사용할 수 있습니다.

`npx webpack --config webpack.config.js` 처럼요.

다시한번 

```jsx
$ npx webpack
```

을 실행해보고 dist 디렉터리아래에 생성된 bundle 파일을 확인해보세요.

---

### Package.json

package.json은 크게 2가지로 나눠집니다.

1. 어플리케이션 내부에 직접 포함되는 모듈
    - 예를 들어 Jquery같은 모듈이 있습니다. DOM선택을 쉽게하기 위해 Jquery등을 NPM을 통해 설치하면 이 모듈은 애플리케이션 내부에서 직접 동작하는 모듈입니다.
2. 개발 과정에 필요한 모듈
    - 개발 효율을 높이거나 코드 컨벤션을 검사하는 등등의 모듈, 즉 테스트 관련 모듈이나 트랜스 파일러 관련 모듈등이 있습니다.

1. dependencies

- 애플리케이션 내부에 직접 포함되는 모듈 정보를 담습니다. 의존성을 규정하는 것은 패키지 이름과 해당 패키지의 범위를 지정한 객체를 통해 이뤄집니다.
- npm 을 통해 설치하는 모듈이 dependencies에 기록되어야할 때는 옵션으로 `--save` 를 사용합니다.
1. devDependencies
    - 테스트 관련 모듈이나 트랜스 파일러 관련 모듈 등 개발 과정에 필요한 모듈등을 나타냅니다
    - npm 을 통해 설치하는 모듈이 devDependencies에 기록되어야할 때는 옵션으로 `--save-dev` 를 사용합니다.

다른 항목들에 대해서는 [https://programmingsummaries.tistory.com/385](https://programmingsummaries.tistory.com/385) 에서 참고

우리 프로젝트로 돌아와 package.json 파일을 열어 `devDependencies` 에 이전에 설치한 `webpack` 과 `webpack-cli` 가 잘 있는지 확인해 봅니다.

그리고 프로젝트 디렉터리중 `node_modules` 를 과감히 지워버립니다.

```jsx
$ npm install
```

그리고 명령어로 `npm install` 만 쳐주면 `node_modules` 가 다시 생성되면서 내부에 `webpack`과 `webpack-cli`가 정상적으로 다시 설치되는 것을 확인할 수 있습니다.

## Loaders

webpack은 기본적으로 javascript와 json파일만 이해합니다. Loader를 사용하면  webpack이 다른 유형의 파일들을 처리하거나 그들을 유효한 모듈로 변환하여 애플리케이션에 사용하거나 dependency graph에 추가합니다. 파일을 `import` 하거나 'load' 할 때 전처리 할 수 있습니다.  즉 webpack이 dependency graph를 완성시키는 과정에서 의존관계를 갖는 다양한 타입의 모듈들을 입력받아 처리하는 역할을 수행합니다.

[Loader에 대한 자세한 설명](https://webpack.kr/concepts/loaders/)

![image](https://user-images.githubusercontent.com/49192400/128522528-910ac163-84d0-4178-942b-dbca5a99efe6.png)

앞서 설명했듯, webpack은 javascript 파일과 json 파일만 인식하기 때문에 다른 타입의 모듈들은 개별적으로 loader를 준비해서 webpack에 연결시켜줘야 합니다.

```jsx
//webpack.config.js

const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [test,use],
  },
};
```

Loader를 설정할 때는 `module`이라는 key를 webpack설정파일에 추가하는 것으로 시작합니다.

이어서  지원해야하는 모듈 타입들을 위해 필요한 loader들을 설정하는 `rules`라는 key를 가진 속성을 정의합니다. rules의 배열안에 `loader` 에 대한 정보를 넣을 때는 위의 configuration 예제처럼 `loader`내에 설정된 기본동작이 적용되도록 `loader`의 이름을 문자열로 넣는 방법과

```jsx
//webpack.config.js

const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};
```

`loader`가 어떻게 동작할지 개발자가 자세하게 설정할 수 있는 객체형태로 넣는 방법이 있습니다. 위의 설정에서는 `test`와 `use` 라는 두 가지 필수 속성을 가진 하나의 모듈을 위해 정의되어있습니다. 이 속성은 webpack 컴파일러에게 다음과 같이 말합니다.

> webpack 컴파일러야 `require() / import` 문 내에서 '.txt' 파일로 확인되는 경로를 발견하면 번들에 추가하기전에 `raw-loader`를 사용해서 변환해줘

---

여기까지 왔다면 지금까지 새로운 프로젝트폴더를 만들어서 

`$ npm init -y` 로 package.json 파일을 만들고 `webpack`과 `webpack-cli` 를  package.json 의 `devDependencies` 에 추가되게 설치해봅니다.

index 페이지에 Hello Webpack 을 띄워줄 것입니다.

```
새프로젝트폴더이름
  |- package.json
+ |- index.html
+ |- /src
+   |- index.js
```

 `index.html` 과 `/src/index.js` 를 하나 만들어 줍니다.

```jsx
//index.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Getting Started</title>
  </head>
  <body>
    <script src="./src/index.js"></script>
  </body>
</html>
```

```jsx
//src > index.js
function component(){
    const element = document.createElement('div');
    element.innerHTML = 'Hello Webpack';
    return element;
}

document.body.appendChild(component());
```

이제 mode가 none인 webpack의 configuration 파일을 만들어봅시다.

`webpack.config.js`

를 생성해줍니다. `entry`와 `output` 의 `filename`, `path` 를 상기하며 만들어봅니다.

```jsx
const path = require('path');

module.exports={
    entry: './src/index.js',
    output: {
        filename : 'bundle.js',
        path : path.resolve(__dirname, 'dist'),
    },
}
```

이제 `$ npx webpack` 을 통해 번들링해주면 dist 디렉터리 아래에 bundle.js 가 정상적으로 생성될 것입니다.

mode를 지정하지 않아 `warning`이 뜨지만 잠깐 무시해주세요.

그리고 `/dist/index.html` 을 생성해보세요.

```jsx
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>Getting Started</title>
    </head>
    <body>
        <script src="./bundle.js"></script>
    </body>
</html>
```

`./src/index.js` 말고 dist 디렉터리에 새로 생성된 `bundle.js`를 연결해주고 `/dist/index.html`을 켜서 똑같이 잘 동작하는지 확인해 줍니다.

이제 Javascript, JSON이 아닌 이미지나 css같은 애셋들을 통합하고 어떻게 처리되는지 살펴봅니다.

이제 loader를 통해 css를 모듈로 읽어올 것입니다. 사용할 loader는 `style-loader` 와 `css-loader` 입니다. `css-loader`는 css를 모듈로 다루기위해 사용되는 loader이고 `style-loader`는 `css-loader`를 통해 가져온 css내용을 style태그로 생성해서 head태그 안에 삽입해줍니다.

두 모듈을 설치하고 `devDependencies` 에 추가해줍니다.

```jsx
$ npm install style-loader css-loader --save-dev
```

설치를 완료했으면 `package.json` 의 `devDependencies` 에 잘 추가되었는지 확인해주세요.

```jsx
//webpack.config.js
const path = require('path');

module.exports={
    entry: './src/index.js',
    output: {
        filename : 'bundle.js',
        path : path.resolve(__dirname, 'dist'),
    },
    module:{
        rules:[
            {
                test:/\.css$/i,
                use:['style-loader', 'css-loader'],
            }
        ]
    }
}
```

webpack.config.js 도 수정해야합니다. 모듈 로더는 체인으로 연결할 수 있습니다. 체인의 각 로더는 리소스에 변형을 적용합니다. 그리고 체인은 역순으로 실행되기 때문에 `style-loader` 가 먼저오고 그 뒤에 `css-loader` 가 따라옵니다.

역순이기 때문에 `css-loader`가 리소스에 변형을 적용하고 → `style-loader` 에 그 값을 전달합니다.

`test` 에서는 어떤 파일들이 loader의 대상이 되는지 정규표현식의 패턴매칭으로 사용할 수 있습니다.

우선 Normalize.css 를 설치하는 방법입니다. css파일도 npm에 공개되어 있다면 설치가 가능합니다. Normalize.css는 HTML 요소의 기본 스타일을 브라우저 간 일관성을 유지하도록 돕는 CSS파일 입니다. user agent style을 표준화 시키는 것에는 2가지가 있습니다.

우선 많은 html tag 들이 기본적으로 가지고 있는 스타일이 있다는 것을 알고 계셔야합니다.

1. `Reset.css` 

    `Reset.css`는 브라우저간의 차이를 최대한 없애서, 스타일이 0인 상태에서 디자인을 만들어 나가기 위해 생겨난 것입니다.

    ex) 모든 margin 값 0으로 리셋

    [https://cdnjs.com/libraries/meyer-reset](https://cdnjs.com/libraries/meyer-reset)

2. `Normalize.css`

    `Normalize.css` 는 브라우저 간 User Agent Style의 오차를 줄이고 버그를 줄이는 방향으로 재지정 되어있습니다.

    ex) h1 tag의 font-size:28px 로 통일

```jsx
$npm install normalize.css --css
```

```jsx
//src > index.js
import 'normalize.css';
function component(){
    const element = document.createElement('div');
```

설치가 완료되었으면 index.js 에서 import 시켜줍니다.

```css
/* src > index.css */
div{
    color:red;
}
```

이어서 createElement로 만든 div 내에 있는 text에 color 값이 적용될 수 있도록 css 파일을 만들어 불러와 봅시다. src 디렉터리 아래에 index.css를 생성해줍니다.

```jsx
//src > index.js
import 'normalize.css';
import './index.css';
function component(){
```

마찬가지로 만든 index.css를 import 시켜줍니다.

그리고 번들링된 결과물을 살펴봅시다.

```jsx
$ npx webpack
or
$ npm run build
```

직접 dist 디렉터리에 접근해서 `index.html` 파일을 열어도 되고 vscode를 사용하시는 분이면 `[live server](https://boheeee.tistory.com/20)`를 사용하셔도 됩니다.

dist > index.html 은 [여기](https://www.notion.so/Webpack-57378a3cae044e19964108b337c544d1) (이전 예제) 에서 만들었습니다.

> 만약 npm run build 에서 에러가 난다면

`package.json` 에서 

```jsx
  | "scripts": {
  |    "test": "echo \"Error: no test specified\" && exit 1",
+ |    "build": "webpack"
  |  },
```

build 부분을 추가해주세요.

![image](https://user-images.githubusercontent.com/49192400/128523548-7846a274-0f1e-4350-af27-e37943e303c4.png)

결과물의 소스를 보면 index.css와 normalize.css 가 head태그 사이에 잘 적용된 것을 볼 수 있습니다.

`loader`의 자세한 내용들은 google 검색에서 github로 배포되어있는 내용의 README 파일을 읽어보면 됩니다.

css module 이라는 설정을 해볼 것입니다. `[css-loader`의 저장소](https://github.com/webpack-contrib/css-loader) 에서 README의  보시면 [Options](https://github.com/webpack-contrib/css-loader#options) 에서 `modules` 를 찾을 수 있습니다. 여기서 CSS Modules 를 설정에 대한 정보를 확인할 수 있습니다. `css-loader`에서 `options` 키를 추가하고 그 안에 `modules`라는 키를 true로 지정해주면 됩니다.

```jsx
//webpack.config.js
const path = require('path');
					.
					.
					.
                test:/\.css$/i,
                use:[
                    'style-loader',
                    {
                        loader : 'css-loader',
                        options:{
                            modules:true
                        }  
                    }
                ],
            }
        ]
    }
} 
```

```css
/* src > index.css */
div{
    color:red;
}

.hellowebpack{
    color:blue;
}
```

```jsx
//src > index.js
import 'normalize.css';
import styles from './index.css';
function component(){
    const element = document.createElement('div');
    element.innerHTML = 'Hello Webpack';

    console.log(styles);
    element.classList = styles.hellowebpack;
    return element;
}

document.body.appendChild(component());
```

css는 선택자의 이름이 전역환경으로 적용되기 때문에 어플리케이션의 규모가 커졌을 때 선택자 이름에 충돌이 없도록 신경을 써줘야합니다. `css modules`라는 설정을 해주면 css파일을 module로 불러오고 class 이름을 javascript에 불러와서 사용할 수 있어 css 파일별로 classname이 아도 겹치지 않는 장점이 있습니다. 

브라우저에서 console.log를 살펴보면

`{hellowebpack: "ITvqO_MZTGan44lkQuGC"}` 객체가 보이고

div의 class에 이 hellowebpack라는 key를 가진 value가 적용되어있는 것이 보일 것입니다.

즉 이 `css module`을 사용하면 css파일별로 class이름이 겹쳐서 생기는 문제를 방지해 줍니다.

이번에는 `style-loader` 에 대한 option 설정을 해봅시다.

`style-loader` 는 처리하는 css 파일 별로 style 태그를 만드는데 하나의 style 태그에서 한번에 읽어올 수 있도록 option을 적용 해볼 것입니다.

[option](https://webpack.js.org/loaders/style-loader/) 에서 injectType 이라는 키에 여러 값들중 `singletonStyleTag` 을 적용 시켜줍니다.

> `injectType` 이란? style이 DOM에 삽입되는 방식을 설정합니다.

```jsx
//webpack.config.js
	use:[
                    {   
                        loader : 'style-loader',
                        options:{
                            injectType : 'singletonStyleTag'
                        }
                    },
                    {
                        loader : 'css-loader',
                        options:{
                            modules:true
                        }  
                    }
                ],
            }
        ]
```

![image](https://user-images.githubusercontent.com/49192400/128523611-554a9570-6228-45d5-9eac-fd205c74640d.png)

`singletonStyleTag` 가 적용되기 전의 모습입니다.

각 style 태그에는 normalize.css 와 index.css 가 있습니다.

이제 번들링한 후의 모습을 살펴보면

![image](https://user-images.githubusercontent.com/49192400/128523646-2b7f2494-6902-40b1-8f62-9547f302e749.png)

하나의 style태그로 합쳐진 모습을 확인하실 수 있습니다.

---

## plugin

플러그인은 웹팩이 동작하는 자체적인 과정에 개입할 수 있어 웹팩이 동작하는 과정 전반적으로 광범위한 작업을 수행할 수 있습니다. 번들파일에 변화를 주거나 최적화하고, 애셋을 관리하고 또 환경변수 주입등과 같은 작업을 수행할 수 있습니다.

plugin은 `require()`을 통해 플러그인을 요청하고 `plugins` 라는 key를 시작으로 설정할 수 있습니다. 이번에는 [HtmlWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/) 을 사용해 볼 예정입니다. 이 플러그인은 번들러를 위한 html 파일을 자동으로 만들어주고 설정해줍니다. 

플러그인은 `loader` 와 다르게 webpack package 내부에 있는 plugin과 외부 저장소에서 관리되는 plugin으로 나뉩니다. 이번에 사용하는 `HtmlWebpackPlugin` 은 외부저장소에 있는 plugin이기 때문에 설치를 먼저 진행해줍니다.

### `HtmlWebpackPlugin`설치

```jsx
$ npm install --save-dev html-webpack-plugin
```

이 명령어는 

```jsx
$ npm i -D html-webpack-plugin
```

으로 축약해서 사용할 수 있습니다. (install → i, —save-dev → -D )

다음으로는 설치한 plugin 모듈을 불러와 적용해보도록 합니다.

```jsx
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports={
    entry: './src/index.js',
    output: {
        filename : 'bundle.js',
        path : path.resolve(__dirname, 'dist'),
    },
    module:{
        rules:[
            {
                test:/\.css$/i,
                use:[
                    {   
                        loader : 'style-loader',
                        options:{
                            injectType : 'singletonStyleTag'
                        }
                    },
                    {
                        loader : 'css-loader',
                        options:{
                            modules:true
                        }  
                    }
                ],
            }
        ]
    },
		// 아래 추가
    plugins : [
        new HtmlWebpackPlugin({
            template: './template.html'
        })
    ],
    mode:'none'
	// --------------------
}
```

plugin 의 key들 중에 template이라는 key를 사용했습니다.

template은 자동으로 생성되는 html 문서가 특정 파일을 기준으로 생성되게 지정해주는 역할을 합니다.

우리는 이전에 만든 `index.html` 을 열어 

```jsx
//index.html -> template.html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>Getting Started</title>
		</head>
    <body>
    </body>
</html>
```

기존에 작성했던 내용중 <script> 태그를 지울 것입니다.

그리고 `index.html` 의 파일 이름을 `template.html`로 변경해줍니다.

그럼 실제로 html파일을 잘 생성해주는지 확인해봅시다.

```jsx
$ npx webpack
```

`dist/index.html` 이 생겼는지 확인해봅니다.

만약 이전에 진행했던 실습 때문에 dist > index.html 파일이 있다면 열어서 바뀐부분이 있는지 확인해보거나 dist > index.html 을 지우고 다시 번들링 해보세요.

생성된 html 파일은 webpack 설정 파일에서 `output` 에 번들파일이 어디에 위치해있고, 파일 이름을 무엇으로 설정했는지를 이용해서 번들된 리소스를 불러올 수 있도록 srcript 태그나 link 태그를 추가해줍니다.

> webpack 에는 많은 플러그인 인터페이스가 있습니다. [https://webpack.kr/plugins/](https://webpack.kr/plugins/) 을 참고하세요.

---

## template engine을 webpack과 함께 사용해보기

### Webpack이 바라보는 Module

1. js
2. sass
3. hbs
4. jpg, png

hbs는 handlebars 의 약자로 이 확장자는 템플릿 엔진인handlebars가 사용하는 template 파일을 말합니다.

문서 내에 특정 데이터를 노출시켜야 할 때 직접 DOM을 파싱하고 데이터가 들어갈 공간을 찾아서 넣어주는 작업을 반복해야합니다. 이런 일을 많이해야할 때 템플릿 엔진을 사용합니다.

템플릿 엔진은 Model Template View 로 나눠져있습니다.

Model은 문서에 노출시킬 데이터를 뜻하고 , Template 은 일반적인 문서와 비슷한데 모델이 갖고 있는 데이터가 어디에 어떻게 표현될 지 문서내에 작성이 되어있습니다. Model과 Template를 컴파일해서 완성된 문서인 View가 결과로 나옵니다.

`handlebars` 를 사용하기 위해서는 `handlebars`를 통해 컴파일되도록 실행시켜주는 `handlebars` 모듈이 필요합니다. 이를 설치해줍니다.

```jsx
$ npm i handlebars -D
```

`handlebars`는 hbs 확장자를 읽어와 컴파일 해줍니다. hbs를 읽어오기 위해서는 `loader`를 설정해줘야합니다. 

`handlebars-loader` 는 handlebars를 HTML로 컴파일 합니다. ([참고](https://webpack.kr/loaders/#templating))

이 loader도 설치해줍니다.

```jsx
$ npm i handlebars-loader -D
```

`handlebars` 와 `handlebars-loader`를 설치했으니 webpack 설정파일 ( webpack.config.js )에 `css-loader`와 마찬가지로 test와 use키를 사용해서 설정을 해줍시다.

```jsx
//webpack.config.js
...
...
module:{
        rules:[
            {
                test:/\.css$/i,
                use:[
                    {   
                        loader : 'style-loader',
                        options:{
                            injectType : 'singletonStyleTag'
                        }
                    },
                    {
                        loader : 'css-loader',
                        options:{
                            modules:true
                        }  
                    }
                ],
            }, // 아래 추가
						{
                test: /\.hbs$/,
                use: ['handlebars-loader']
            }
        ]
    },
...
...
```

추가를 완료했으면 `temlate.html`파일을 `template.hbs` 로 확장자명을 변경해줍니다.

`hbs` 로 바꾸는 이유는 `html-webpack-plugin` 의 설정내용을 html 문서에 주입시킬 수 있는 형태가 되었기 때문입니다.

```jsx
//webpack.config.js
...
...
plugins : [
        new HtmlWebpackPlugin({
            template: './template.hbs'
        })
    ],
...
...
```

`html-webpack-plugin` 설정에서도 `template` 경로를 바꿔주도록 합니다.

그리고 title에 대한 내용을 적용하기 위해 title이라는 key를 추가하고 Webpack이라는 글자를 넣어볼 것입니다.

```jsx
//webpack.config.js
...
...
plugins : [
        new HtmlWebpackPlugin({
						title: 'Webpack',
            template: './template.hbs'
        })
    ],
...
...
```

# [아직 작성중..]

참고 문서

- [https://joshua1988.github.io/webpack-guide/motivation/why-webpack.html#파일-단위의-자바스크립트-모듈-관리](https://joshua1988.github.io/webpack-guide/motivation/why-webpack.html#%ED%8C%8C%EC%9D%BC-%EB%8B%A8%EC%9C%84%EC%9D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AA%A8%EB%93%88-%EA%B4%80%EB%A6%AC)
- [https://webpack.kr/](https://webpack.kr/)
- [https://ko.javascript.info/import-export#ref-4122](https://ko.javascript.info/import-export#ref-4122)
- [https://humanwater.tistory.com/2](https://humanwater.tistory.com/2)
- [https://sapjil.net/resetcss-normalizecss/](https://sapjil.net/resetcss-normalizecss/)
