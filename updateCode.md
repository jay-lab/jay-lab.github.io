
 
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
_footer.scss > .page__footer > background-color: #044343;
_footer.scss > .page__footer > color: #ffffff; 

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