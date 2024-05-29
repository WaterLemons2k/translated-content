---
title: Event：preventDefault() 方法
slug: Web/API/Event/preventDefault
---

{{APIRef("DOM")}}

{{domxref("Event")}} 接口的 **`preventDefault()`** 方法，告诉{{Glossary("user agent", "用户代理")}}：如果此事件没有被显式处理，它默认的动作也不应该照常执行。

此事件还是继续传播，除非碰到事件监听器调用 {{domxref("Event.stopPropagation", "stopPropagation()")}} 或 {{domxref("Event.stopImmediatePropagation", "stopImmediatePropagation()")}}，才停止传播。

如后文所述，对于不可取消的事件（例如通过 {{domxref("EventTarget.dispatchEvent", "EventTarget.dispatchEvent()")}} 分派的、没有指定 `cancelable: true` 的事件），调用 **`preventDefault()`** 是没有任何效果的。

### 语法

```js-nolint
event.preventDefault()
```

## 示例

### 阻止默认的点击事件执行

切换复选框是点击复选框的默认行为。下面这个例子说明了怎样阻止默认行为的发生：

#### JavaScript

```js
const checkbox = document.querySelector("#id-checkbox");

checkbox.addEventListener("click", checkboxClick, false);

function checkboxClick(event) {
  let warn = "preventDefault() won't let you check this!<br>";
  document.getElementById("output-box").innerHTML += warn;
  event.preventDefault();
}
```

#### HTML

```html
<p>Please click on the checkbox control.</p>

<form>
  <label for="id-checkbox">Checkbox:</label>
  <input type="checkbox" id="id-checkbox" />
</form>

<div id="output-box"></div>
```

#### 结果

{{EmbedLiveSample("阻止默认的点击事件执行")}}

### 在编辑域中阻止按键

下面的这个例子说明了如何使用 `preventDefault()` 在文本编辑域中阻止无效的文本输入。如今，你通常可以使用[原生的 HTML 表单验证](/zh-CN/docs/Learn/Forms/Form_validation)来代替。

#### HTML

此 HTML 表单用于捕获用户输入。由于我们只对按键输入感兴趣，因此我们禁用 `autocomplete` 功能来防止浏览器使用缓存的值填充输入字段。

```html
<div class="container">
  <p>Please enter your name using lowercase letters only.</p>

  <form>
    <input type="text" id="my-textbox" autocomplete="off" />
  </form>
</div>
```

#### CSS

用一些 CSS 来制作当用户按下无效按键时绘制的警告框：

```css
.warning {
  border: 2px solid #f39389;
  border-radius: 2px;
  padding: 10px;
  position: absolute;
  background-color: #fbd8d4;
  color: #3b3c40;
}
```

#### JavaScript

这里是相关的 JavaScript 代码。首先，监听 [`keydown`](/zh-CN/docs/Web/API/Element/keypress_event) 事件：

```js
const myTextbox = document.getElementById("my-textbox");
myTextbox.addEventListener("keydown", checkName, false);
```

`checkName()` 方法可以查看按下的键并决定是否允许：

```js
function checkName(evt) {
  const key = evt.key;
  const lowerCaseAlphabet = "abcdefghijklmnopqrstuvwxyz";
  if (!lowerCaseAlphabet.includes(key)) {
    evt.preventDefault();
    displayWarning(
      "Please use lowercase letters only.\n" + `Key pressed: ${key}\n`
    );
  }
}
```

`displayWarning()` 方法显示问题通知。这种方法并不优雅，但在这个例子中可行：

```js
let warningTimeout;
const warningBox = document.createElement("div");
warningBox.className = "warning";

function displayWarning(msg) {
  warningBox.innerHTML = msg;

  if (document.body.contains(warningBox)) {
    clearTimeout(warningTimeout);
  } else {
    // insert warningBox after myTextbox
    myTextbox.parentNode.insertBefore(warningBox, myTextbox.nextSibling);
  }

  warningTimeout = setTimeout(() => {
    warningBox.parentNode.removeChild(warningBox);
    warningTimeout = -1;
  }, 2000);
}
```

#### 结果

{{ EmbedLiveSample('在编辑域中阻止按键', 600, 200) }}

## 备注

在事件流的任何阶段调用 `preventDefault()` 都会取消事件，这意味着任何通常被该实现触发并作为结果的默认行为都不会发生。

你可以使用 {{domxref("Event.cancelable")}} 来检查该事件是否支持取消。为一个不支持 cancelable 的事件调用 `preventDefault()` 将没有效果。

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}
