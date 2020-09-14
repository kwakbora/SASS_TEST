# SASS 컴파일 하는 방법
Sass(SCSS)는 웹에서 직접 동작할 수 없습니다.
어디까지나 최종에는 표준 CSS로 동작해야 하며, 우리는 전처리기로 작성 후 CSS로 컴파일해야 합니다.
다양한 방법으로 컴파일이 가능하지만 자바스크립트 개발 환경(Node.js)에서 추천하는 몇가지 방법을 소개합니다.



## SassMeister
간단한 Sass 코드는 컴파일러를 설치하는게 부담될 수 있습니다.
그럴 경우 [Sassmeister](https://www.sassmeister.com/)를 사용할 수 있습니다.

페이지 접속 후 바로 Sass나 SCSS 문법으로 코딩하면 CSS로 실시간 변환됩니다.



## node-sass
[node-sass](https://github.com/sass/node-sass)는 Node.js 컴파일러인 [Libsass](https://sass-lang.com/libsass)에 바인딩한 라이브러리 입니다.
NPM으로 전역 설치하여 사용합니다.

```bash
$ npm install -g node-sass
```

컴파일하려는 파일의 경로와 컴파일된 파일이 저장될 경로를 설정합니다.
`[]`는 선택사항입니다.

```bash
$ node-sass [옵션] <입력파일경로> [출력파일경로]
```

```bash
$ node-sass scss/main.scss public/main.css
```

여러 출력 경로를 설정할 수 있습니다.

```bash
$ node-sass scss/main.scss public/main.css dist/style.css
```

옵션을 적용할 수도 있습니다.
옵션으로 `--watch` 혹은 `-w`를 입력하면, 런타임 중 파일을 감시하여 저장 시 자동으로 변경 사항을 컴파일합니다.

```bash
$ node-sass --watch scss/main.scss public/main.css
```

기타 옵션은 [node-sass CLI](https://github.com/sass/node-sass#command-line-interface)에서 확인할 수 있습니다.



## Gulp
빌드 자동화 도구(JavaScript Task Runner)인 [Gulp](https://gulpjs.com/)에서는 gulpfile.js을 만들어 아래와 같이 설정할 수 있습니다. 먼저 gulp 명령을 사용하기 위해서는 전역 설치가 필요합니다.

```bash
$ npm install -g gulp
```

Gulp와 함께 Sass 컴파일러인 [gulp-sass](https://github.com/dlmanning/gulp-sass)를 개발 의존성(devDependency) 모드로 설치합니다.
gulp-sass는 위에서 살펴본 node-sass를 Gulp에서 사용할 수 있도록 만들어진 플러그인입니다.

```bash
$ npm install --save-dev gulp gulp-sass
```

```javascript
// gulpfile.js
var gulp = require('gulp')
var sass = require('gulp-sass')

// 일반 컴파일
gulp.task('sass', function () {
  return gulp.src('./src/scss/*.scss')  // 입력 경로
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./dist/css'));  // 출력 경로
});

// 런타임 중 파일 감시
gulp.task('sass:watch', function () {
  gulp.watch('./src/scss/*.scss', ['sass']);  // 입력 경로와 파일 변경 감지 시 실행할 Actions(Task Name)
});
```

환경을 설정했으니 컴파일합니다.

```bash
$ gulp sass
```

런타임 중 파일 감시 모드로 실행할 수도 있습니다.

```bash
$ gulp sass:watch
```



## Parcel

웹 애플리케이션 번들러 [Parcel](https://parceljs.org/)은 굉장히 단순하게 컴파일할 수 있습니다.

우선 Parcel를 전역으로 설치합니다. (node.js 가 깔려있다는 전제)

```bash
$ npm install -g parcel-bundler
```

프로젝트에 Sass 컴파일러(node-sass)를 설치합니다.

```bash
$ npm install --save-dev node-sass
```

이제 HTML에 `<link>`로 Sass 파일만 연결하면 됩니다.
다른 설정은 필요하지 않습니다.

```html
<link rel="stylesheet" href="scss/main.scss">
```

```bash
$ parcel index.html
# 혹은
$ parcel build index.html
```

`dist/`에서 컴파일된 Sass 파일을 볼 수 있고,
별도의 포트 번호를 설정하지 않았다면 `http://localhost:1234`에 접속하여 적용 상태를 확인할 수 있습니다.
