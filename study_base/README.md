# SASS(SCSS) 문법정리

Sass와 SCSS의 기능은 동일하니, 편의를 위해 SCSS 문법으로 위주로 설명하고
Sass와 SCSS의 차이점이 있다면 나눠 설명한다.



## 주석(comment)

CSS 주석은 /* ... */ 인데,

SASS(SCSS)는 javascript 처럼 두 가지 스타일의 주석을 사용한다.

```scss
//컴파일되지 않는 주석(굳이 쓰지 않음)
/* 컴파일 되는 주석*/
```


SASS의 경우 컴파일되는 여러 줄 주석을 사용할 때 각 줄 앞에 * 붙여야 하고, 중요한 것은 * 의 라인을 맞춰줘야 한다.

#### SASS

```scss
/* 컴파일 되는
 * 여러 줄
 * 주석 */

//Error
/* 컴파일 안되는
* 여러 줄
   * 주석 */
```


## 중첩(Nesting)

상위 선택자의 반복을 피하고 좀 더 편리하게 복잡한 구조를 작성할 수 있습니다.

#### SCSS

```scss
.section {
  width: 100%;
  .list {
    padding: 20px;
    li {
      float: left;
    }
  }
}
```

compiled to :

```scss
.section {
  width: 100%;
}
.section .list {
  padding: 20px;
}
.section .list li {
  float: left;
}
```



## Ampersand (상위 선택자 참조)

중첩 안에서 `&` 키워드는 상위(부모) 선택자를 참조하여 치환합니다.

#### SCSS

```scss
.btn {
  position: absolute;
  &.active {
    color: red;
  }
}

.list {
  li {
    &:last-child {
      margin-right: 0;
    }
  }
}
```

compiled to :

```scss
.btn {
  position: absolute;
}
.btn.active {
  color: red;
}
.list li:last-child {
  margin-right: 0;
}
```



## @at-root (중첩 벗어나기)

중첩에서 벗어나고 싶을 때 `@at-root` 키워드를 사용합니다.
중첩 안에서 생성하되 중첩 밖에서 사용해야 경우에 유용합니다.

#### SCSS

```$scss
.list {
  $w: 100px;
  $h: 50px;
  li {
    width: $w;
    height: $h;
  }
  @at-root .box {
    width: $w;
    height: $h;
  }
}
```

$w 변수를 사용하고 싶은데 .list 에 포함되어 있는 class 가 아닐때, 유효성 범위내에서 변수를 사용할 수 있다.

compiled to :

```scss
.list li {
  width: 100px;
  height: 50px;
}
.box {
  width: 100px;
  height: 50px;
}
```



### 중첩된 속성

`font-`, `margin-` 등과 같이 동일한 네임 스페이스를 가지는 속성들을 다음과 같이 사용할 수 있습니다.

#### SCSS

```$scss
.box {
  font: {
    weight: bold;
    size: 10px;
    family: sans-serif;
  };
  margin: {
    top: 10px;
    left: 20px;
  };
  padding: {
    bottom: 40px;
    right: 30px;
  };
}
```

compiled to :

```scss
.box {
  font-weight: bold;
  font-size: 10px;
  font-family: sans-serif;
  margin-top: 10px;
  margin-left: 20px;
  padding-bottom: 40px;
  padding-right: 30px;
}
```



## 변수(Variables)

반복적으로 사용되는 값을 변수로 지정할 수 있습니다. 변수 이름 앞에는 항상 `$`를 붙입니다.

$변수이름: 속성값;

#### SCSS

```$scss
$color-primary: #e96900;
$url-images: "/assets/images/";
$w: 200px;

.box {
  width: $w;
  margin-left: $w;
  background: $color-primary url($url-images + "bg.jpg");
}
```

compiled to :

```scss
.box {
  width: 200px;
  margin-left: 200px;
  background: #e96900 url("/assets/images/bg.jpg");
}
```



### 변수 유효범위(Variable Scope)

변수는 사용 가능한 유효범위가 있습니다. 선언된 블록(`{}`) 내에서만 유효범위를 가집니다.

변수 `$color`는 `.box1`의 블록 안에서 설정되었기 때문에, 블록 밖의 `.box2`에서는 사용할 수 없습니다.



### 변수 재 할당(Variable Reassignment)

다음과 같이 변수에 변수를 할당할 수 있습니다.

#### SCSS

```$scss
$red: #FF0000;
$blue: #0000FF;

$color-primary: $blue;
$color-danger: $red;

.box {
  color: $color-primary;
  background: $color-danger;
}
```

compiled to :

```scss
.box {
  color: #0000FF;
  background: #FF0000;
}
```



### !global (전역 설정)

`!global` 플래그를 사용하면 변수의 유효범위를 전역(Global)로 설정할 수 있습니다.

#### SCSS

```$scss
.box1 {
  $color: #111 !global;
  background: $color;
}
.box2 {
  background: $color;
}
```

compiled to :

```scss
.box1 {
  background: #111;
}
.box2 {
  background: #111;
}
```

대신 기존에 사용하던 같은 이름의 변수가 있을 경우 값이 덮어져 사용될 수 있습니다.



SCSS는 각 줄에 * 이 없어도 문제되지 않고 기존 CSS 와 호환이 쉽다.

#### SCSS

```scss
/*
컴파일 되는
여러 줄
주석
*/
```

