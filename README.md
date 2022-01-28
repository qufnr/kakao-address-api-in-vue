# Vue.JS에서 Daum 주소 API를 사용하는 방법

> 해당 프로젝트는 Vue 2.x 버전에서 테스트를 진행하였습니다. 3.x 버전에서의 작동 여부는 확인하지 않았습니다.

## 1. Javascript CDN 설정
`public/index.html`에 다음 Script 호출 HTML 태그를 추가합니다.
```html
<script src="http://dmaps.daum.net/map_js_init/postcode.v2.js"></script>
```

## 2. Vue 인스턴스에서 daum.Postcode() 호출하기
먼저 호출을 하기 전에, `template` 부분에서 주소 목록 폼을 표시할 위치 태그를 작성해야합니다.
`ref` 속성으로 주소 폼을 표시하고자 하는 위치를 작성합니다.
```html
<template>
  <div ref="address"></div>
</template>
```
해당 태그를 작성 후 Vue 인스턴스의 `mounted()`나 실행하고자하는 `methods`에 다음 Javascript 코드를 작성합니다. 
다음은 예시입니다.
```javascript
mounted() {
  this.$nextTick(() => {
    new window.daum.Postcode({}).embed(this.$refs.address)
  })
}
```
`$refs` 로 설정한 위치의 `ref` 속성을 갖고 있는 태그 위치에 접근하여 주소 폼을 그려줍니다.

## 3. daum.Postcode() 에서 결과 값 받기
`daum.Postcode()` 생성자에는 `oncomplete()` 속성이 있습니다. 해당 속성으로 주소 폼에서 입력하고 검색된 결과를 Object 형식으로 반환받을 수 있습니다.
소스는 다음과 같습니다.
```javascript
data() {
  return {
    myAddress: null
  }
},

mounted() {
  this.$nextTick(() => {
    new window.daum.Postcode({
      oncomplete: data => {
        this.myAddress = data.zonecode + ' ' + data.roadAddress
      }
    }).embed(this.$refs.address)
  })
}
```
이렇게 작성된 코드로 실행 후 주소를 검색하고 결과를 보내면, `우편번호 주소명` 형식으로 `myAddress` 상태변수에 저장하게 됩니다.

## 4. oncomplete() 에서 반환되는 값
`oncomplete()` 에서 반환되는 결과 값들은 `zonecode`, `roadAddress` 뿐만이 아니라, 여러가지 결과 값들이 존재합니다. 다음 페이지에서 반환 값을 확인하세요. [Daum 주소 API 속성](https://postcode.map.daum.net/guide#attributes "Daum 주소 API 속성")

## 5. daum.Postcode() 생성자의 속성
`daum.Postcode()`에서 가장 중요한 생성자는 `oncomplete()` 이지만, 개발자 마음에 맞게 개발하기 위해 더 많은 속성들이 존재합니다. 다음 페이지에서 속성을 확인하세요. [Daum 주소 API 속성](https://postcode.map.daum.net/guide#attributes "Daum 주소 API 속성")
