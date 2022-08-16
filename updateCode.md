
 
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