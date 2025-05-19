# Проект: Игра Snake на HTML, CSS и JavaScript

## Исследование предметной области

**Цель:** Изучить механику классической игры Snake и реализовать её с использованием базовых технологий фронтенда.

**Источники:**
- [Туториал от freeCodeCamp](https://www.freecodecamp.org/news/think-like-a-programmer-how-to-build-snake-using-only-javascript-html-and-css-7b1479c3339e/)
- Документация по JavaScript (MDN)

## ⚙️ Стек технологий
- HTML
- CSS
- JavaScript

---

## Техническое руководство

### Шаг 1: Подготовка HTML-структуры

```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Snake Game</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>Snake</h1>
  <canvas id="gameBoard" width="400" height="400"></canvas>
  <p>Очки: <span id="score">0</span></p>
  <script src="script.js"></script>
</body>
</html>
```

### Шаг 2: Стилизация поля (CSS)

```css
body {
  text-align: center;
  font-family: Arial;
}

canvas {
  background-color: #000;
  display: block;
  margin: 0 auto;
  border: 2px solid #0f0;
}
```

### Шаг 3: Логика игры (JavaScript)

```js
const canvas = document.getElementById("gameBoard");
const ctx = canvas.getContext("2d");
const scale = 20;
const rows = canvas.height / scale;
const columns = canvas.width / scale;

let snake;
let fruit;

function Snake() {
  this.x = 0;
  this.y = 0;
  this.xSpeed = scale;
  this.ySpeed = 0;
  this.body = [];

  this.draw = function () {
    ctx.fillStyle = "#0f0";
    for (let i = 0; i < this.body.length; i++) {
      ctx.fillRect(this.body[i].x, this.body[i].y, scale, scale);
    }
    ctx.fillRect(this.x, this.y, scale, scale);
  };

  this.update = function () {
    for (let i = this.body.length - 1; i > 0; i--) {
      this.body[i] = { ...this.body[i - 1] };
    }

    if (this.body.length) this.body[0] = { x: this.x, y: this.y };

    this.x += this.xSpeed;
    this.y += this.ySpeed;

    // Столкновение со стеной
    if (this.x >= canvas.width) this.x = 0;
    if (this.y >= canvas.height) this.y = 0;
    if (this.x < 0) this.x = canvas.width - scale;
    if (this.y < 0) this.y = canvas.height - scale;
  };

  this.changeDirection = function (direction) {
    switch (direction) {
      case "ArrowUp":
        if (this.ySpeed === 0) {
          this.xSpeed = 0;
          this.ySpeed = -scale;
        }
        break;
      case "ArrowDown":
        if (this.ySpeed === 0) {
          this.xSpeed = 0;
          this.ySpeed = scale;
        }
        break;
      case "ArrowLeft":
        if (this.xSpeed === 0) {
          this.xSpeed = -scale;
          this.ySpeed = 0;
        }
        break;
      case "ArrowRight":
        if (this.xSpeed === 0) {
          this.xSpeed = scale;
          this.ySpeed = 0;
        }
        break;
    }
  };

  this.eat = function (fruit) {
    if (this.x === fruit.x && this.y === fruit.y) {
      this.body.push({});
      return true;
    }
    return false;
  };
}

function Fruit() {
  this.x = Math.floor(Math.random() * columns) * scale;
  this.y = Math.floor(Math.random() * rows) * scale;

  this.draw = function () {
    ctx.fillStyle = "red";
    ctx.fillRect(this.x, this.y, scale, scale);
  };

  this.pickLocation = function () {
    this.x = Math.floor(Math.random() * columns) * scale;
    this.y = Math.floor(Math.random() * rows) * scale;
  };
}

function setup() {
  snake = new Snake();
  fruit = new Fruit();

  window.setInterval(() => {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    fruit.draw();
    snake.update();
    snake.draw();

    if (snake.eat(fruit)) {
      fruit.pickLocation();
      document.getElementById("score").textContent = snake.body.length;
    }
  }, 150);
}

window.addEventListener("keydown", e => {
  snake.changeDirection(e.key);
});

setup();
```

## Модификация проекта
Добавлено:
???
