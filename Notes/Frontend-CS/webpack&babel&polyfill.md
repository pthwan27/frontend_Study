# Babel(바벨)

**Babel is a JavaScript compiler**

구형 브라우저나 환경에서 ES6이상의 기능을 사용하기 위해서 이전 버전의 자바스크립트 버전으로 변환해주는 도구이다.

## Babel 사용법

### 예시 화살표 함수 transpiling

```javascript
const fn = () => '바벨';
```

- `@babel/core`: 바벨의 핵심적인 기능
- `@babel/cli`: 터미널로 바벨을 사용
- `@babel/plugin-transform-arrow-functions`: 화살표 함수를 transform하는 플러그인

`./node_modues/.bin/babel [변환할 파일] --out-dir [변환될 위치] --plugins=@babel/plugin-transform-arrow-function`를 실행해주면

```javascript
var fn = function fn() {
  return '바벨';
};
```

위와 같이 변환이 된다. 즉, 바벨은 **코드를 재작성해주는 트랜스파일러 프로그램**이다.

하지만 매번 Plugin을 설치해야하고 매번 명령어 옵션에 Plugin을 추가해야 하는 것이 번거로울 수 있다. 이를 해소하기 위해 바벨은 config 파일(babel.config.json 혹은 .babelrc.json)을 제공한다. 이 설정 파일 내에서 Preset을 포함한 여러 플러그인들을 한 번에 지정할 수 있어, 사용자는 명령어마다 모든 플러그인을 지정할 필요가 없게 된다.

- 바벨 config 파일: 바벨의 설정을 지정하는 파일. preset과 plugin들을 지정할 수 있다.
- Preset: 바벨에서 사용할 플러그인들의 모음. 즉, 여러 plugin들의 집합으로 한 번에 여러 플러그인을 적용하기 쉽게 해준다.

```json
// babel.config.json 예시
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "forefpx": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ],
  "plugins": ["some-other-plugin"]
}
```

<br/><br/><br/>

# Polyfill(폴리필)

**구형 브라우저에서 자체적으로 지원하지 않는 최신 기능들을 지원하고자 가져오는 코드 (뭉치)** 이다

바벨은 `Preset`과 `Plugin`을 통해서 코드를 트랜스파일하는데, 트랜스파일한다고 해서 모든 새로운 JavaScript 기능을 지원해주는 것은 아니다. 예를 들면, `Promise`와 같은 빌트인 객체나 `Array.prototype.includes`와 같은 인스턴스 메서드는 바벨의 기본 트랜스파일링만으로는 변환되지 않는다. 이런 경우, 폴리필이 필요하다.

폴리필은 런타입에서 타겟 환경에 빌트인이나 메서드가 존재하지 않으면 이를 확인하고 최신 환경을 맞춰주기 위해서 코드들을 삽입한다.

## Polyfill 사용법

`@babel/polyfill`은 이전에 바벨에서 폴리필을 적용하기 위해 사용되었지만 Babel 7.4.0부터는 권장되지 않는다. 이제 대신에 `core-js`를 직접적으로 사용하고, 바벨 설정에서 `useBuiltIns` 옵션을 활용하여 필요한 폴리필만을 자동으로 import하도록 설정한다.

```json
// babel.config.json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "entry",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

`core-js`는 최슨 Javascript기능을 오래된 브라우저에서 사용 가능하게 해주는 폴리필 라이브러리이다. 이를 사용하면 인스턴스 메서드 동작이 전부 잘 동작하지만 `core-js` 단독 사용하는 것을 권장하지 않는다. **전역 스코프가 오염**될 수 있는 단점이 있기 때문이다. `core-js`같은 경우에는 전역에 존재하지 않는 메서드들이나 빌트인들을 바로 주입해서 사용하기 때문에 전역 스코프가 오염될 수 있다. 이 경우 이름 충돌이 발생할 수 있고, 외부에서 사용하는 라이브러리를 가져왔을 경우에 전역 스코프를 오염시키면 예상할 수 없는 오류가 생길 수 있다.

이 문제를 해결하기 위해 `@babel/plugin-transform-runtime`을 함께 사용해주는 것이 좋다.

<br/><br/><br/>

# Webpack(웹팩)

## Webpack이란?

웹팩은 오픈 소스 자바스크립트 **모듈 번들러**로써 **여러개로 나누어져 있는 파일들을 하나의 자바스크립트 코드로 압축하고 최적화**하는 라이브러리이다.

웹페이지를 구성하는 데 필요한 파일들간의 관계를 웹팩에서 인식을 하고 최종적으로 웹페이지에 올라가기 직전에 최적화된 파일셋을 뽑아내는 역할을 한다.

### 모듈 번들러

웹 애플리케이션을 구성하는 자원(HTML, CSS, JS, Image 등)을 모두 각각의 모듈로 보고, 이를 조합해서 병합된 하나의 결과물을 만드는 도구

## Webpack 등장 이유

javascript에서의 모듈의 한계 때문에 등장하였다. javascript에서는 script태그를 분리하여 모듈처럼 사용하고 있었는데 이렇게 이용하게 된다면 전역을 공유하기 때문에 같은 변수 이름에 대해서 문제가 발생하게 된다.

## Webpack 과정

<img src="../../images/Frontend-CS/webpack&babel&polyfill/what-is-webpack.png">

웹팩은 모듈간의 관계들을 정립하는 의존성 그래프를 갖게 되어 모듈 관리를 수월하게 하고 이 자원들을 최적화해서 웹페이지의 성능을 끌어올리는 역할을 한다.

웹 팩은 기본적으로 JavaScript 및 JSON 파일만 해석이 가능하다. CSS, SVG, TypeScript, Babel 등 다양한 포맷의 파일을 처리하기 위해서는 loader를 통해 앱에서 사용할 수 있는 모듈로 변환할 수 있다.

```json
// webpack.config.js
module.exports = {
  // 모듈 설정
  module: {
    // 로더 규칙 설정
    rules: [
      // 긱 로더에 대한 설정
      {
        test: /파일_포멧_정규식/, // 변환하고자 하는 파일의 포맷
        use: ['로더'] // 사용할 로더
      },
      // 추가적인 로더 설정...
    ]
  }
}
```

<br/><br/><br/>

# 정리

1. 바벨 (Babel):

   - JavaScript 소스 코드 변환 도구.
   - ES6 이상의 최신 문법을 구형 브라우저에서 동작 가능한 ES5 코드로 변환.
   - 주로 "컴파일 타임"에 동작.

2. 폴리필 (Polyfill):

   - 브라우저에서 기본적으로 제공하지 않는 객체나 메서드를 JavaScript로 구현한 코드 조각.
   - 예: Promise, Array.prototype.includes 등이 구형 브라우저에서 지원되지 않을 때 이를 위한 폴리필을 제공하여 해당 기능을 사용할 수 있게 해줌.
   - 주로 "런타임"에 동작하여 브라우저에 누락된 기능을 추가.

3. 웹팩 (Webpack):
   - 모듈 번들러 (Module Bundler)로써, 프로젝트의 여러 파일과 의존성을 하나 또는 여러 개의 번들 파일로 묶어줌.
   - 개발자가 만든 여러 JS, CSS, 이미지 파일들을 웹에서 효과적으로 로드할 수 있는 형태로 번들링.
   - 개발 환경과 프로덕션 환경에 따라 다른 최적화와 플러그인 설정을 제공할 수 있어, 성능 향상과 리소스 관리에 도움을 줌.
   - 주로 "빌드 타임"에 동작하여, 웹 리소스를 최적화하고 번들링.

# 참고

https://www.youtube.com/watch?v=o-5K5Sc7L1k&t=318s

https://www.youtube.com/watch?v=U6BUs_n8WWk

https://velog.io/@hozzijeong/WebPack%EA%B3%BC-Babel-%EA%B7%B8%EB%A6%AC%EA%B3%A0-Polyfill%EC%97%90-%EB%8C%80%ED%95%B4

https://manhyuk.github.io/webpack/
