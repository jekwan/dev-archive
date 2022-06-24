# CRA 없이 리액트 앱 시작하기

`create-react-app(이하 CRA)`은 매우 편리합니다. 테스트나 번들링 같은 설정을 알아서 해주고, 눈에 보이지 않게 숨겨주기 때문에 `react` 개발에만 집중할 수 있습니다.

하지만 저는 어플리케이션의 밑 바닥부터 꼭대기까지 모든 부분에 대한 완전한 통제를 목표로 하기에 `CRA`없이 리액트 앱을 시작하는 방법을 공부하며 정리하고자 합니다.

# 1. React

`React`사용을 위해 우선 아래 두 가지의 패키지를 설치해야 합니다.

```
npm install react react-dom
```

**`react`**: 이 패키지는 리액트의 핵심기능인 `React Component`에 대한 기능을 제공합니다.  
**`react-dom`** : 이 패키지는 리액트로 `DOM` 작업을 하기 위한 기능을 제공합니다. 웹 페이지를 만들 예정이라면 이 패키지를 통해 리액트에서 DOM에 접근할 수 있습니다.

이로써, `React`를 사용하기 위한 패키지는 모두 설치되었습니다.

# 2. Webpack

앞으로 만들 리액트 컴포넌트들과 여러 Asset들을 번들링하기 위한 도구로 `Webpack`을 사용하도록 하겠습니다.

> [Why webpack](https://webpack.js.org/concepts/why-webpack/)  
> `Webpack`은 스코프 문제도 해결해주는 등 다양한 장점이 있다고 소개하고 있습니다.

아래와 같이 3가지의 패키지를 설치합니다.

```
npm install --save-dev webpack webpack-cli webpack-dev-server
```

**`webpack`** : 웹팩의 코어 패키지입니다. 모듈 번들러의 기능을 제공합니다.  
**`webpack-cli`** : 웹팩 3버전까지는 CLI기능이 **`webpack`** 패키지에 포함되어 있었으나, 4버전 이후부터 웹팩의 코어 기능과 CLI기능이 다른 두 패키지로 나뉘게 되었습니다. 웹팩의 빌드 기능등을 CLI로 사용하기위해 이 패키지를 사용합니다.  
**`webpack-dev-server`** : 웹팩을 사용하여 개발할 때 사용할 수 있는 공식 툴로 간단한 CLI 명령어를 통해 빠르게 개발용 서버를 띄워 애플리케이션을 테스트할 수 있습니다. 실제 번들 파일을 생성하는게 아니라 메모리 위에 저장하여 사용하기 때문에 빠르게 빌드됩니다. `live reload`기능도 더해지기 때문에 `개발 - 빌드 - 테스트`의 리듬을 빠르게 가져갈 수 있게 도움을 주는 툴입니다.

# 3. Babel

리액트는 `es6` 사양의 `class`나 `import`기능을 활용하기때문에 최신의 자바스크립트를 지원하지 않는 브라우저에서 정상적으로 동작하기 위해서는 낮은 버전의 자바스크립트로 변환해 줄 필요가 있습니다. 이를 위해 `Babel`을 사용합니다

```
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader
```

**`@babel/core`** : 바벨의 코어 패키지입니다. 자바스크립트 코드를 변환하는 핵심적인 기능을 제공합니다.  
`@babel/cli` : 바벨을 CLI상에서 사용하기 위해 필요한 패키지 입니다. 하지만 저는 직접 바벨 명령어를 호출하지 않고 웹팩의 빌드 과정에서 사용할 것 이기 때문에 이 패키지는 사용하지 않습니다.  
**`@babel/preset-env`** : 바벨 그 자체로는 아무런 변환을 수행하지 않습니다. 바벨의 자바스크립트 코드 변환은 플러그인을 적용함으로써 이루어집니다. 그리고 이 플러그인들을 묶어놓은 것이 preset입니다. `@babel/preset-env`는 바벨이 제공하는 공식 프리셋 중 하나로 최신 자바스크립트 코드를 원하는 수준의 브라우저에서 돌아가게끔 변환해주는 기능을 제공합니다. `browserlist`를 설정하면 현재 자바스크립트 코드를 어느정도 수준까지 변환할 지 최적화할 수 있습니다.  
**`@babel/preset-react`** : 마찬가지로 바벨이 제공하는 공식 프리셋 중 하나로 JSX문법을 변환할 때 필요한 기능을 제공합니다.  
**`babel-loader`** : 바벨을 웹팩의 빌드과정에서 수행하기위해 필요한 패키지입니다. 이 패키지를 사용하도록 웹팩 설정파일에 명시하면 웹팩의 빌드 과정에 바벨이 참여하여 자바스크립트 코드를 변환할 수 있습니다.

# 4. Webpack 설정

여기까지 필요한 핵심 패키지는 모두 설치되었습니다.  
별다른 설정을 하지 않으면 웹팩은 `src/index.js`을 엔트리 포인트로 하여 모듈을 스캔한 후 번들링한 결과를 `dist/main.js`로 내보냅니다.

그러나 명심해야 하는 부분은 웹팩은 기본적으로 자바스크립트 애플리케이션에 대한 번들러라는 점 입니다. 리액트의 경우 JSX문법을 사용하기 마련이고 TypeScript를 적용하기도 합니다. 웹팩이 이를 제대로 번들링하기 위해서 추가적인 설정이 필요합니다.

프로젝트의 루트에 `webpack.config.js`파일을 생성하면 webpack은 자동으로 이 설정파일을 사용합니다. 대부분의 경우 이런 설정파일을 만들어 자바스크립트 이외의 확장된 기능들을 번들에 포함해야 합니다.

JSX를 사용하는 리액트를 위한 기본적인 웹팩 설정은 아래와 같습니다.

```javascript
// webpack.config.js

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.join(__dirname, "/dist"),
    filename: "index_bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
  ],
};
```

먼저 하나의 패키지를 더 설치합니다.

```
npm install --save-dev html-webpack-plugin
```

**`html-webpack-plugin`** : 웹팩에서 사용하는 플러그인 패키지입니다. 웹팩은 빌드의 결과로 하나의 자바스크립트 파일을 내보내는데, 이 자바스크립트를 불러오는 `index.html`파일을 알아서 생성해주는 편의 기능을 제공합니다. 따라서 이 플러그인을 사용하도록 설정하면 빌드 결과로 하나의 html파일을 추가로 내보냅니다.

그리고 `CommonJS`방식으로 객체 하나를 export합니다.  
export되는 객체에는 몇몇 속성을 지정해줘야 하는데, 위에서는 `entry`, `output`, `module`, `plugin`을 지정해줬습니다.

**`entry`** : 웹팩이 빌드할 때 사용할 엔트리포인트를 지정합니다. 웹팩은 여기에 명시된 파일들을 기반으로 디펜던시 그래프를 만들어 필요한 모듈들을 번들링 합니다.  
**`output`** : 번들된 결과물에 대한 경로와 파일명을 지정합니다.  
**`module`** : 웹팩은 자바스크립트와 JSON파일만 이해할 수 있습니다. 따라서 그 이외의 문법이나 파일을 처리하기 위해서는 `Loader`가 필요합니다. 위에서 추가한 `babel-loader`패키지가 바로 웹팩에서 사용할 `Loader`의 일종입니다. 이 외에도 css파일을 위한 `css-loader`등 다양한 패키지를 추가할 수 있습니다.  
**`plugin`** : `Loader`가 웹팩이 빌드하기 위한 파일을 해석하는 데 도움을 준다면 `Plugin`은 웹팩의 결과물에 대한 최적화나 추가 처리를 하는 데 사용됩니다.

# 5. 실행 및 빌드

이제 설치된 패키지들과 설정을 기반으로 리액트 개발을 시작할 수 있습니다. 개발중에는 개발 편의성을 위해 `webpack-dev-server`를 사용합니다.

```
webpack-dev-server --mode development --open --hot
```

위 명령어를 통해 `webpack-dev-server`를 개발모드로 실행할 수 있습니다.

```
webpack --mode production
```

위 명령어를 통해 웹팩 빌드를 수행할 수 있습니다.
