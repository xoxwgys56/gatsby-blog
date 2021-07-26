---
layout: post
title: 'form tag using vanilaJS'
image: 'img/javascript-default.jpeg'
author: [Dan]
date: 2021-07-26
tags: ['javascript', 'vanila']
---

# `Form` tag using VanilaJS


## Prevent `form` redirect

`form` 태그를 통해 submit하게 되면 submit의 응답 결과를 받는 페이지로 넘어가게 된다.  
이를 방지하기 위해 `e.e.preventDefault()`를 쓸 수 있다. 예시는 아래와 같다.

```jsx
// react 예시이다.
const get_input_value = (e) => {
        e.preventDefault();
        value = e.target.text_input.value;

        console.log('get value', value)
    }

return (
    <form action="" method="post"  onSubmit={get_input_value} >
        <input name="text_input" type="text"/>
    </form>
)
```

나의 경우는 `onInput`보다 `onSubmit`이 더 적절한 상황이었다. 필요한 이벤트리스너에 등록해 사용하면 되겠다.  
또한 코드에서 나온 것처럼 받고자 하는 변수의 이름을 아래와 같이 정하면 편하게 값을 받아올 수 있다. (`id`를 이용해 엘레먼트를 찾은 뒤, 값을 받아오는 방법도 있다. 경우에 따라 더 적절한 방법을 쓰기 권한다.)  
같은 내용의 반복이기 때문에 다른 코드를 인용하겠다.

```html
<form onsubmit="submitForm(event)">
  <!-- searchTerm 이라는 변수로 받고자 했다.  -->
  <input name="searchTerm"/>
  <button>Submit</button>
</form>
```

```js
// 아래처럼 변수로 받을 수 있겠다.
const term = event.target.searchTerm.value;
```

`onSubmit`에 들어가는 함수는 매개변수를 할당해주어되고, 할당해주지 않아도 된다.  

> 다만 태그 내에서 사용할 경우 `event`라는 고정된 변수이름을 사용해야 되는 것으로 기억하는데, 확실하진 않다.

---

## References

- [event prevent default](https://stackoverflow.com/questions/39809943/react-preventing-form-submission)
- [get value from form input](https://stackoverflow.com/questions/37487826/send-form-data-to-javascript-on-submit/57047920)