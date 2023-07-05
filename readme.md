# 前端游戏小项目

跟着CSDN上的前端大神写前端小游戏，目的是为了练习自己的写代码能力！

## day1: 猜数字游戏

### 功能介绍

要求我们猜出一个1-100之间的整数，用户可以在输入框输入自己猜测的数字点击提交，会根据我们的猜测给出提示，太大了/太小了，我们可以根据提示继续进行猜测，答对后提示我们答对了并且出现在玩一次的按钮，点击再玩一次重新开始；当然我们可以自己规定规则，大家可以自己修改变成更棒的小游戏！

### 页面搭建

#### DOM构建

```html

<div class="container">
    <h1>猜数字游戏</h1>
    <div class="input-group">
        <label for="guess">请猜一个1~100的整数：</label>
        <input type="text" id="guess">
        <button id="submit">提交</button>
    </div>
    <div class="result"></div>
    <div class="message"></div>
    <button id="play-again"
            class="play-again"
            style="display: none;">再玩一次
    </button>
</div>
```

#### 样式设置

```css
* {
    font-family: Arial, sans-serif;
    box-sizing: border-box;
}

.container {
    margin: 50px auto;
    max-width: 600px;
    text-align: center;
    background-color: #d1d1d1;
    padding: 30px;
    border-radius: 10px;
}

h1 {
    font-size: 32px;
}

.input-group {
    margin-bottom: 20px;
}

label {
    display: block;
    margin-bottom: 5px;
}

input[type='text'] {
    font-size: 18px;
    padding: 5px 10px;
}

button {
    font-size: 18px;
    padding: 5px 10px;
    background-color: #007bff;
    color: #ffffff;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #0062cc;
}

.result {
    font-size: 24px;
    margin-bottom: 20px;
}

.message {
    font-size: 18px;
    margin-bottom: 20px;
}

.play-again {
    font-size: 18px;
    padding: 5px 10px;
    background-color: #007bff;
    color: #ffffff;
    border: none;
    cursor: pointer;
    margin: 0 auto;
}

.play-again:hover {
    background-color: #0062cc;
}
```

#### 逻辑部分

首先，随机生成一个1~100的随机数作为正确答案；然后使用选择器来获取页面中的元素。

```js
  // 随机生成数
let answer = Math.floor(Math.random() * 100) + 1;
//获取页面元素
let input = document.getElementById("guess");
let submit_btn = document.getElementById("submit");
let result = document.querySelector(".result");
let message = document.querySelector(".message");
let play_again_btn = document.getElementById("play-again");
```

下面添加事件监听器来处理用虎的输入和提交事件。当用户点击提交按钮时，先获取用户输入框中的内容。首先，先验证用户输入的内容是否合法；如果不合法，则在提示信息中显示错误并结束函数执行

```js
// 处理提交事件
submit_btn.addEventListener("click", function () {
  // 获取输入的内容
  let guess = input.value;
  let guess_num = parseInt(guess);
  // 验证输入的内容
  if (isNaN(guess) || guess_num < 1 || guess_num > 100) {
    result.textContent = "";
    message.textContent = "请输入1~100以内的整数！";
    return;
  }
});
```

如果内容合法，则需要与正确答案进行比较。如果相等，则显示猜对了，并显示再玩一次按钮，同时禁用提交按钮；如果用户输入的数字小于正确答案，则提示太小了；如果用户输入的数字大于正确答案，则提示太大了。

```js
// 处理提交事件
submit_btn.addEventListener("click", function () {
  // 获取输入的内容
  let guess = input.value;
  let guess_num = parseInt(guess);
  // 验证输入的内容
  if (isNaN(guess) || guess_num < 1 || guess_num > 100) {
    result.textContent = "";
    message.textContent = "请输入1~100以内的整数！";
    return;
  }

  // 比较用户输入的数字和答案
  if (guess_num === answer) {
    result.textContent = "恭喜你，答对了！";
    message.textContent = "";
    play_again_btn.style.display = "block";
    submit_btn.disabled = true;
  } else if (guess_num < answer) {
    message.textContent = "太小了，请继续！";
    result.textContent = "";
  } else {
    message.textContent = "太大了，请继续！";
    result.textContent = "";
  }
});
```

当猜到答案之后，会显示再玩一次按钮。对再玩一次按钮进行事件监听。 首先，会重新生成一个随机整数，并清空用户输入的数字、提示信息和结果信息，同时隐藏再玩一次按钮，启用提交按钮。

```js
// 处理再玩一次事件
play_again_btn.addEventListener("click", function () {
  // 重新生成随机数
  answer = Math.floor(Math.random() * 100) + 1;

  // 清空输入框和提示信息
  input.value = "";
  message.textContent = "";
  result.textContent = ""

  // 隐藏再玩一次按钮，启动提交按钮
  play_again_btn.style.display = 'none';
  submit_btn.disabled = false;
});
```

![img.png](img.png)

## day2:打字通游戏

### 功能介绍

给出一个文本域，用户可以在里面输入内容，当点击开始按钮后，启动倒计时，提示部分变成我们需要打的字。当计时结束后，上面的提示会变成我们的分数；用户可以点击开始，重新开始一轮打字游戏。

### 页面搭建

#### DOM结构

```html

<div class="box">
    <div class="container">
        你准备好了吗？
    </div>
    <textarea name="" id="" cols="30" rows="10" placeholder="开始输入..." style="resize: none"></textarea>
    <div class="operate">
        <button>开始</button>
        <div id="timer">60</div>
    </div>
</div>
```

#### 样式代码

```css
* {
    font-family: Arial sans-serif;
    margin: 0;
    padding: 0;
    -moz-user-select: none;
    /*火狐*/
    -webkit-user-select: none;
    /*webkit浏览器*/
    -ms-user-select: none;
    /*IE10*/
    -khtml-user-select: none;
    /*早期浏览器*/
    -o-user-select: none;
    user-select: none;
}

.box {
    width: 50%;
    min-width: 300px;
    background-color: #ac8c3e;
    /*text-align: center;*/
    margin: 40px auto;
    box-sizing: border-box;
    padding: 20px;
    border-radius: 30px;
    box-shadow: 0 0 30px 9px #939393;
}

.container {
    margin: 0 auto;
    text-align: center;
    padding: 20px;
}

textarea {
    width: 100%;
    height: 200px;
    margin: 20px 0;
    font-size: 20px;
    border: none;
}

.operate {
    width: 20%;
    margin: 0 auto;
    text-align: center;
}

.operate button {
    width: 90px;
    font-size: 24px;
    padding: 10px 20px;
    background-color: #007bff;
    color: #ffffff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

#timer {
    font-size: 48px;
    margin: 20px;
}
```

### 功能实现

#### 获取页面dom元素

使用document.querySelector获取要用到的元素

```js
  const text = "Believe in yourself and all that you are. Know that there is something inside you that is greater than any obstacle. This quote by Christian D. Larson reminds us that we all have the power within us to overcome any obstacle we may face. When we have confidence in ourselves and our abilities, we can achieve great things. So, let's trust ourselves, believe in our dreams, and work hard to make them a reality.";
let container = document.querySelector(".container");
let input_ = document.querySelector("textarea");
let button = document.querySelector(".operate button");
let timer = document.querySelector("#timer");
input_.value = "";
```

#### 给开始按钮绑定监听事件

```js
button.addEventListener("click", function () {
  // 设置倒计时
  timer.textContent = "60";

  // 情况输入框和输出文本区域
  input_.value = "";
  container.textContent = "";

  // 开启游戏
  startGame();
});
```

#### 启动游戏

因为游戏的过程中需要计时，需要声明一个变量来保存计时器，方便后面游戏结束来销毁定时器。 然后，游戏开始后，开始按钮需要被禁用；范文被展示在container中；计时器开始倒计时。

```js
// 定时器
let countDown;

// 启动游戏
function startGame() {
  // 开始按钮禁用
  button.disabled = true;

  // 显示文本
  container.textContent = text;

  // 启动倒计时
  countDown = setInterval(() => {
    const remainingTime = parseInt(timer.textContent) - 1;
    if (remainingTime === 0) {
      // 倒计时结束，结束游戏
      endGame();
    }
    timer.textContent = remainingTime + "";
  }, 1000);
}
```

#### 结束游戏

倒计时结束后，游戏停止。 开始按钮需要启用；container范文部分需要展示最后的分数；销毁计时器。

```js
// 结束游戏
function endGame() {
  // 停止倒计时
  clearInterval(countDown);

  // 计算得分
  const score = calculateScore();
  container.textContent = `你的得分是${score}分`;
  // 显示开始按钮和计时器
  button.style.display = "block";
  timer.style.display = "block";
  button.disabled = false;
}
```

#### 计算最后得分

首先将范文和用户输入的内容两端的空格去掉，然后将他们分割成两个数组。遍历用户输入的内容，如果当前项和范文中的单词相同，则记1。 最后，返回的分数是正确的单词数/总共的单词数 * 100。

```js
// 计算得分
function calculateScore() {
  const userText = input_.value.trim().split(" ");
  const correctText = text.trim().split(" ");
  let score = 0;

  for (let i = 0; i < userText.length; i++) {
    console.log(userText[i], correctText[i]);
    if (userText[i] === correctText[i]) {
      score++;
    }
  }
  return (score / correctText.length * 100).toFixed(2);
}
```
![img_1.png](img_1.png)
