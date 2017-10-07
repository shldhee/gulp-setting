# gulp

> 자동화하는 빌드 도구

## 설치하기

``` js
node, npm 설치 후
> npm init
> npm install -g gulp
```

* -g 는 global의 약자로 전역설치 어느 위치에서든 사용이 가능하게 한다.

``` js
const gulp = require('gulp');
  // 걸프 의존성을 여기 씁니다.

gulp.task('default', function() {
  // 걸프 작업을 여기 씁니다.
});
```

``` js
// 성공적으로 설치됐는지 확인
> gulp
[19:17:21] Using gulpfile ~/Desktop/dev/12_LearningJS/gulpfile.js
[19:17:21] Starting 'default'...
[19:17:21] Finished 'default' after 61 ms
```

### babel 설치

`babel`은 ES5를 ES6로 바꾸는 트랜스컴파일러로 시작했고, 프로젝트가 성장하면서 ES6와 리액트, ES7 등 여러 가지를 지원하는 범용 트랜스컴파일러가 됐습니다.

바벨 버전 6부터는 ES5를 ES6로 변환하려면 ES6 변환 프리셋을 설치하고 바벨이 해당 프리셋을 사용하게끔 설정해야 합니다.
(설치해보니 이것도 끝났고 babel-preset-env를 설치하라고 함)
링크 : [babel-env](http://babeljs.io/env)

``` js
//> npm install --save-dev babel-preset-es2015
> npm install --save-dev babel-preset-env
```

### 프로젝트 구조

root
.babelrc
gulpfile.js
package.json
/node_modules
-----/es6   # 노드 소스
-----/dist
-----/public # 브라우저 소스
------------/es6
------------/dist

서버쪽(node), 클라이언트(브라우저) 코드를 모두 포함하는 프로젝트가 많으므로 양쪽 다 만들었습니다.
브라우저에 보내는 자바스크립트는 원래 공개된(public) 것이고, 이런 식으로 저장하는 프로젝트가 많습니다.
(???????)

### 바벨을 걸프와 함께 사용하기

* ES6을 ES6로 변환하기(ES6, public/ES6에 있는 코드를 ES5 코드로 변환해서 dist와 public/dist에 저장)

``` js
npm install --save-dev gulp-babel
```

*gulpfile.js*

``` js
const gulp = require('gulp');
const babel= require('gulp-babel');

gulp.task('default', function() {
  // 노드 소스
  gulp.src("es6/**/*.js")
    .pipe(babel())
    .pipe(gulp.dest("dist"));
  // 브라우저 소스
  gulp.src("public/es6/**/*.js")
    .pipe(babel())
    .pipe(gulp.dest("public/dist"));
});
```

* 걸프는 파이프라인 개념으로 작업하니다.
* 변환할 파일 선택(`src("es6/**/*.js")`) es6폴더 이하 모든 js 확장자 선택
* 그 다음 바빌에 파이프로 연결합니다. 바벨은 ES6 코드를 ES5 코드로 변형합니다.
* 마지막으로 컴파일된 ES5 코드를 dist 폴더에 저장합니다.
* 걸프는 소스 파일 이름과 폴더 구조를 그대로 유지합니다.
  - es6/a.js 파일은 dist/a.js로 컴파일 되고 es6/a/b/c.js파일은 dist/a.b.c.js로 컴파일되는 식입니다.
* 같은 과정을 public 폴더에도 합니다.


### test.js를 이용한 예제

*es6/test.js, public/es6/test.js*

``` js
const a = 10;
const c = a => a*a;
c(a);
```

위와 같이 폴더와 내용을 작성 후 `gulp` 실행

``` js
> gulp
```

실행 후 결과는 아래와 같다.

*dist/test.js, public/dist/test.js*

``` js
"use strict";

var a = 10;
var c = function c(a) {
  return a * a;
};
c(a);
```
