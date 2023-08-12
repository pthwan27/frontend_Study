# CSS Position

## Position ?

CSS에서 `position` 속성은 HTML 문서의 요소가 배치되는 방식을 결정하는 것입니다.

주로 top, left, bottom, rignt 속성을 함께 사용하여 배치할 최종 위치를 결정합니다.

### **position : static**

`position`을 별도로 지정해주지 않는다면 기본값으로 `static`이 적용됩니다.

`position`이 `static`인 요소들은 HTML 문서 상에 원래 있어야 하는 위치에 배치됩니다.

**따라서 `top, left, bottom, right` 속성 값이 무시됩니다.**

```html
<main>
  <div>position: static;</div>
  <div>position: static;</div>
  <div>position: static;</div>
</main>
```

![static](../../images/etc/CSS%20Position/static.png)

### **position : relative**

`position`속성을 `relative`로 설정하면 요소를 원래 위치에서 벗어나게 배치할 수 있씁니다.

**원래 위치를 기준!!!
상대적으로 배치해주는 것입니다!**

`top`, `bottom`, `left`, `right` 속성을 이용해서, 요소가 원래 위치에 있을 때의 위치로부터 얼마나 떨어지게 할지 지정할 수 있습니다.

```css
div:nth-of-type(2) {
  position: relative;
  top: 28px;
  left: 48px;
  background: cyan;
  opacity: 0.8;
}
```

예를 들어 위의 `div` 중 두번째 `div`의 `css`를 다음과 같이 작성하면 다음과 같이 바뀌게 됩니다.

![relative](../../../images/Language/CSS/CSS%20Position/relative.png)

### **position : absolute**

`absolute`는 `relative`와 전혀 관련이 없습니다!

`absolute`는 배치 기준이 **자신이 아닌 가장 가까운 상위 요소**입니다!

이 때 주의해야하는 것은 **상위 요소**가 `static`이 아니여야한다는 것입니다!

만약 상위 요소 중 `static`이 아닌 요소가 없다면 `<body>`가 배치 기준이 됩니다!

```css
main {
  position: relative;
  width: 300px;
  height: 400px;
  background: tomato;
}
```

위의 예제에서 `<main>`을 relative로 바꾸고

```css
div:nth-of-type(2) {
  position: absolute;
  bottom: 8px;
  right: 16px;
  background: cyan;
  opacity: 0.8;
}
```

두 번째 `<div>`를 `absolute`로 바꾼 뒤 다음과 같이 작성한다면 다음과 같이 바뀌게 됩니다.

![absolut](../../../images/Language/CSS/CSS%20Position/absolute.png)

`position: absolute`인 요소는 HTML 문서 상에서 독립되어 앞뒤에 나온 요소와 더 이상 상호작용을 하지 않게 된다는 것입니다.
따라서 위에서 보이는 것처럼, 첫 번째 요소 아래에 바로 세 번째 요소가 배치됩니다.

### **position : fixed**

우리가 화면에서 드래그를 했을 때 특정 부분에 고정되어 있는 UI가 `fixed`입니다.

`fixed`는 배치 기준이 부모 요소가 아닌 브라우저 전체화면인 `viewport` 입니다.

`fixed`요소에서 `top`, `left`, `bottom`, `right` 속성은 각각 브라우저 상단, 좌측, 하단, 우측으로 부터 해당 요소가 얼마나 떨어져있는지를 결정합니다.

```css
div:nth-of-type(2) {
  position: fixed;
  bottom: 8px;
  right: 16px;
  background: cyan;
  opacity: 0.8;
}
```

위의 예제에서 2번째 `<div>`를 바꾸게 되면 다음과 같이 바뀌게 됩니다.

![fixed](.../../../images/Language/CSS/CSS%20Position/fixed.png)

`position: fixed`인 요소도 `position: absolute`인 요소와 마찬가지로 HTML 문서 상에서 독립되어 앞뒤에 나온 요소와 더 이상 상호작용 하지 않습니다.

### **position : sticky**

`sticky`는 브라우저 화면을 스크롤할 때 효과가 나타납니다.

위의 예제에서 `main`을 다음과 같이 바꾼 뒤

```css
main {
  width: 300px;
  height: 120px;
  overflow: auto;
  background: tomato;
}
```

두번째 `<div>`를 `sticky`로 변경하고 `top : 0`으로 설정해주면 스크롤이 내려갔을 때 두번째 `<div>`의 `top: 0`이 되는 위치에 오면 그 자리에 고정되어있습니다!

```css
div:nth-of-type(2) {
  position: sticky;
  top: 0;
  background: cyan;
  opacity: 0.8;
}
```

![sticky1](../../../images/Language/CSS/CSS%20Position/sticky1.png)

![sticky2](../../../images/Language/CSS/CSS%20Position/sticky2.png)
