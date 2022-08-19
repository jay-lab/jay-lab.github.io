
 
### 한줄 코드블럭 표시 색상 변경(background)
_variables.scss >  
`$code-background-color: #253238 !default; // hwb(85 55% 9%) !default;`
### 한줄 코드 표시 색상 변경(font color)
_page.scss > 
`:not(pre) > code` 쪽에 `color : #edffff;` 추가
---
<br>

### 왼쪽 사이드바 마우스를 갖다 대었을 때 원래의 밝은 색상으로 표시하고 마우스가 사이드바 위에 없을 때 다시 흐릿하게하는 기능 제거
opacity: 0.75; 이 부분 주석처리

### footer 컬러
_footer.scss > .page__footer > background-color: #044343; 이 부분을 아래로 수정
_footer.scss > .page__footer > background: linear-gradient(312deg, #000000 90%, #404b42 90%);

## 포스트 제목 컬러 - 1
```css
a {
  &:focus {
    @extend %tab-focus;
  }

  // &:visited {
  //   color: $link-color-visited;
  // }

  &:hover {
    color: #2f9558; //rgb(35, 173, 227); // $link-color-hover;
    outline: 1;
  }
}

.archive__item_hover {
  box-shadow: inset 0 0 0 0 #008585;
  color: #008585;
  // margin: 0 -.25rem;
  // padding: 0 .25rem;
  transition: color .3s ease-in-out, box-shadow .3s ease-in-out;
  &:focus {
    @extend %tab-focus;
  }

  // &:visited {
  //   color: $link-color-visited;
  // }

  &:hover {
    box-shadow: inset 400px 0 0 0 #008585;
    color: white;
  }
}
```

## 포스트 제목 컬러 - 2
_includes/archive-single.html > 
```html
<a class="archive__item_hover"
```

## footer github 아이콘 주황색으로 변경
_config.yml > footer > icon > fa-github2 추가

_sass/minimal-mistakes/_utilities.scss > .fa-github 클래스css 밑에 아래 추가
.fa-github2 {
  color : #fb9c3b;
}

## header 백그라운드 이미지 적용
_layouts/default.html > 
<style>
  .masthead {
    background-image: linear-gradient( rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3) ), url('{{ site.baseurl }}/assets/images/header_background_img.jpg');
    background-size: 100%;
  }
</style>

## header 텍스트 색상 변경
_sass/minimal-mistakes/_masthead.scss > 
.greedy-nav > a태그 color 적용 #ffffff 및 text-shadow: 2px 2px 2px black;

## blog title & sub title CSS 라인업 효과
```css
/* lineup class and keyframes */
.lineUp {
  animation: 1.8s anim-lineUp ease-out;
}
@keyframes anim-lineUp {
  0% {
    opacity: 0;
    transform: translateY(80%);
  }
  20% {
    opacity: 0;
  }
  50% {
    opacity: 1;
    transform: translateY(0%);
  }
  100% {
    opacity: 1;
    transform: translateY(0%);
  }
}
```