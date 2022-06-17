
## Vue.js 의 단방향 데이터 흐름

v-modal은 양방향 데이터 바인딩 방법으로 한개의 컴포넌트 내에서 입력값을 데이터에 바인딩하기에는 좋지만, **부모자식 혹은 동일 레벨의 컴포넌트**와 데이터 바인딩 하기에는 어렵다. 

따라서 다른 컴포넌트와 데이터의 흐름을 좀 더 쉽게 추적하기 위해서는 단뱡향 바인딩이 권장된다. 이를 위한 단방향 데이터 흐름은 pros와 emit을 사용하여 구현할 수 있다.

---

### props
- 상위 컴포넌트 > 하위 컴포넌트
- props는 camel 표기법을 사용하면 안된다. 대신 kebab 표기법 사용가능
- 상위 컴포넌트에서 kebab, 하위 컴포너트에서 camel
```
1. camelCase: 첫글자를 소문자로 하고, 이어지는 글자를 대문자로(canelCase)
2. PasCalCase: 첫글자와 이어지는 글자 모두 대문자(PasCal)
3. snake_case: 모두 대문자 or 소문자, 언더바로 글자를 이어줌(snake_case)
4. kebab-case: 모두 소문자, 대시로 글자를 이어줌(kebab-case)
```

#### 1. 부모가 전달할 값을 prop을 작성: 부모의 jsp, js
```html
// 개념
<Vue.component :전달할prop명="값"></Vue.component>

// 예제
<data-modal :value="popupVal" :name="popupName" :service-List="popupServiceList"></data-modal>

```
``` js
data() {
    return {
        popupVal: "2",
        popupName: "hi",
        popupServiceList: {'service1', 'service2', 'service3'}
    }
}
```

#### 2. 자식이 그대로 사용: 자식의 jsp, js
``` js
data() {
    return {
    }
},
props: [
    'value',
    'name',
    'serviceList'
],
createdd: function () {
    console.log("value 값:" + this.vaule);
    console.log("name 값:" + this.name);
    console.log("serviceList 값:" + this.serviceList);
}
```

---

### emit
- 하위 컴포넌트 > 상위 컴포넌트
- 다른 컴포넌트에게 **이벤트**를 전달하기 위해서 사용

#### 1. 자식이 전달할 값을 emit에 담기: 자식의 JS

``` js
var serviceList = {'S1','S2'};
this.$emit('add-service', servicesList);
```

#### 2. 자식에게 받을 데이터를 처리할 Event 작성: 부모의 JS

``` js
receiveServicesList: function(data) {
    this.serviceList = data;
}
```
#### 3. 전달받을 JSP에 사용하기: 부모의 JSP

```html
// 개념
<Vue.component v-if="" @자식의emit이름="부모의 js 함수명"></Vue.component>

// 예제
<data-modal v-if="showModal" @add-service="receiveServicesList"></data-modal>

``` 

---
