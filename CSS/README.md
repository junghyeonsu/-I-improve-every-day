# CSS

## 🤔 will-change 속성에 대해서 아시나요?

```css
/* 키워드 값 */
will-change: auto;
will-change: scroll-position;
will-change: contents;
will-change: transform;        /* Example of <custom-ident> */
will-change: opacity;          /* Example of <custom-ident> */
will-change: left, top;        /* Example of two <animateable-feature> */

/* 전역 값 */
will-change: inherit;
will-change: initial;
will-change: unset;
```

`will-change` 속성은 브라우저한테 "이 스타일 속성이 변화될 것 같다" 라는 암시를 주는 것 입니다. 실제로 해당 속성이 변하기 전에 브라우저에게 미리 실행하도록 설정해놓고 페이지의 반응성을 높일 수 있습니다.

`will-change`는 CSS 속성이지만 CSS 스타일시트를 통해서 넣는 것을 추천하지 않습니다.

`will-chagne` 속성은 브라우저에게 계속 긴장을 하도록 유도하는 것이기 때문에, 과도하거나 불필요한 `will-change` 사용은 오히려 성능을 낮출 수 있습니다.

그래서 우리가 정말로 필요할 때만 사용하고, 그렇지 않은 경우에는 없애주어야 합니다.

```js
const el = document.getElementById('element');

// 마우스가 들어갈 때 브라우저에게 긴장감을 주고,
// 바깥으로 나가거나, 애니메이션이 끝났다면 긴장감을 풀어줘야 합니다.
el.addEventListener('mouseenter', hintBrowser);
el.addEventListener('animationEnd', removeHint);

// 마우스가 들어갔을 때 변화될 속성들을 미리 제공해줍니다.
function hintBrowser() {
  this.style.willChange = 'transform, opacity';
}

// 사용하지 않으면 기본값으로 되돌려 브라우저가 쓸데없는 성능 하락이 없도록 합니다.
function removeHint() {
  this.style.willChange = 'auto';
}
```

### `will-change`는 마법이 아닙니다. 

`will-change` 속성은 해당 속성을 사용하는 요소를 GPU(그래픽 카드)를 사용하도록 합니다. GPU를 사용하는 레이어로 바꾸는 작업은 꽤나 많은 리소스가 들고, `will-change`가 지원하는 속성들이 대부분 리플로우를 일으키는 요소들입니다. (width, height, left, top, bottom, ...etc)
그래서 막무가내로 사용하는 건 오히려 성능을 악화시킬 수 있습니다.

### 브라우저에게 미리 알려줍시다.

```js
.element:hover {
	will-change: transform;
	transition: transform 2s;
	transform: rotate(30deg) scale(1.5);
}
```

위의 방식은 아무런 효과가 없습니다. 해당 요소가 발생하기 직전에 가르쳐주는 것은 효과가 거의, 또는 전혀 없습니다. (새 레이어로 변경하는 비용 때문에 성능이 오히려 나빠질 수도 있습니다.)

그래서 좋은 방법은 어떤 요소를 클릭했을 때의 이벤트에 대해서는 `hover`로 `will-change` 속성을 미리 줄 수 있습니다. (그리고 마우스가 나갔을 때도 `will-change`을 다시 없애주어야 합니다.)

### 그래서 언제쓰면 좋은데?

MDN에서 설명하는 가장 좋은 예시는 슬라이드가 여러 개로 이루어진 프레젠테이션 홈페이지 같은 경우에는 `transform` 속성을 통해서 페이지를 넘기는 애니메이션이 많기 때문에 해당 상황에서는 포함시키면 적절하다고 합니다.

```css
.slide {
  will-change: transform;
}
```

위와 같이 브라우저에게 `transform` 속성이 사용될거니까 미리 준비하고 있어! 라고 말을 해놓고, 유저가 슬라이드 애니메이션을 동작시키면 자연스러운 애니메이션을 보여주도록 할 수 있습니다.

```css
.element-stuck-to-the-mouse-cursor {
	will-change: left, top;
}
```

혹은 위와 같이 커스텀 마우스 커서를 만들어서 마우스 커서를 계속 따라다녀야 하는 상황이라면 `will-change` 속성을 통해서 마우스 커서 요소에 대해서 GPU를 사용하도록 하고, 계속해서 최적화를 진행시킬 수 있습니다.

> 애플리케이션에서 유저와의 상호 작용에 대해서 신속하게 대처해야 하는 속성 하나 혹은 두 개 정도가 적당합니다.

### 참고

- [MDN | will-change](https://developer.mozilla.org/ko/docs/Web/CSS/will-change)
- [will-changeCSS 속성 에 대해 알아야 할 모든 것](https://dev.opera.com/articles/css-will-change-property/)
- [CSS 애니메이션 성능 개선 방법(reflow 최소화, will-change 사용)](https://wit.nts-corp.com/2017/06/05/4571)
