### Advanced JavaScript Concepts for Game Development with p5.js

#### **1. Object-Oriented Programming (OOP)**

You can use **classes** to represent game objects like players, enemies, or projectiles. This keeps your code modular and reusable.

**Example: Player Class**

```javascript
class Player {
  constructor(x, y, size) {
    this.x = x;
    this.y = y;
    this.size = size;
    this.speed = 5;
  }

  display() {
    fill(0, 255, 0);
    ellipse(this.x, this.y, this.size);
  }

  move() {
    if (keyIsDown(LEFT_ARROW)) this.x -= this.speed;
    if (keyIsDown(RIGHT_ARROW)) this.x += this.speed;
    if (keyIsDown(UP_ARROW)) this.y -= this.speed;
    if (keyIsDown(DOWN_ARROW)) this.y += this.speed;
  }
}

let player;

function setup() {
  createCanvas(800, 600);
  player = new Player(width / 2, height / 2, 50);
}

function draw() {
  background(200);
  player.move();
  player.display();
}
```

---

#### **2. Collision Detection**

Collision detection is critical for interactions like checking if a player hits an enemy or collects an item.

**Example: Circle-Rectangle Collision**

```javascript
function isCollidingCircleRect(cx, cy, radius, rx, ry, rw, rh) {
  let closestX = constrain(cx, rx, rx + rw);
  let closestY = constrain(cy, ry, ry + rh);

  let distX = cx - closestX;
  let distY = cy - closestY;

  return (distX * distX + distY * distY) <= (radius * radius);
}

// Example usage:
let playerX = 300, playerY = 300, playerSize = 50;
let rectX = 400, rectY = 400, rectW = 100, rectH = 50;

function draw() {
  background(200);

  // Player
  fill(0, 255, 0);
  ellipse(playerX, playerY, playerSize);

  // Rectangle
  fill(255, 0, 0);
  rect(rectX, rectY, rectW, rectH);

  // Collision check
  if (isCollidingCircleRect(playerX, playerY, playerSize / 2, rectX, rectY, rectW, rectH)) {
    fill(255, 255, 0);
    text("Collision Detected!", 10, 30);
  }
}
```

---

#### **3. Particle Systems**

Use particle systems to create effects like explosions, smoke, or sparks.

**Example: Particle System for an Explosion**

```javascript
class Particle {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.vx = random(-2, 2);
    this.vy = random(-2, 2);
    this.alpha = 255;
  }

  update() {
    this.x += this.vx;
    this.y += this.vy;
    this.alpha -= 5; // Fade out
  }

  display() {
    noStroke();
    fill(255, this.alpha);
    ellipse(this.x, this.y, 10);
  }

  isDead() {
    return this.alpha <= 0;
  }
}

let particles = [];

function setup() {
  createCanvas(800, 600);
}

function draw() {
  background(0);

  // Update and display particles
  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].update();
    particles[i].display();

    if (particles[i].isDead()) {
      particles.splice(i, 1);
    }
  }
}

function mousePressed() {
  for (let i = 0; i < 50; i++) {
    particles.push(new Particle(mouseX, mouseY));
  }
}
```

---

#### **4. Game States**

Control game flow with states (e.g., menu, play, game over).

**Example: Game State Management**

```javascript
let gameState = "menu";

function setup() {
  createCanvas(800, 600);
}

function draw() {
  background(200);

  if (gameState === "menu") {
    displayMenu();
  } else if (gameState === "play") {
    playGame();
  } else if (gameState === "gameOver") {
    displayGameOver();
  }
}

function displayMenu() {
  textSize(32);
  text("Press ENTER to Start", width / 2 - 150, height / 2);
}

function playGame() {
  ellipse(width / 2, height / 2, 50);
  textSize(16);
  text("Press ESC to End Game", 10, 20);
}

function displayGameOver() {
  textSize(32);
  text("Game Over", width / 2 - 100, height / 2);
}

function keyPressed() {
  if (keyCode === ENTER) gameState = "play";
  if (keyCode === ESCAPE) gameState = "gameOver";
}
```

---

#### **5. Sound Effects and Music**

Add sound effects to enhance the gaming experience using the **p5.sound** library.

**Example: Sound Effects**

```javascript
let sound;

function preload() {
  sound = loadSound("jump.mp3");
}

function setup() {
  createCanvas(800, 600);
}

function draw() {
  background(200);
  text("Press SPACE for sound", 10, 30);
}

function keyPressed() {
  if (key === " ") {
    sound.play();
  }
}
```

---

#### **6. Physics: Gravity and Velocity**

Simulate basic physics like gravity and velocity for smooth motion.

**Example: Gravity Simulation**

```javascript
class Ball {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.vy = 0;
    this.gravity = 0.5;
  }

  update() {
    this.vy += this.gravity;
    this.y += this.vy;

    // Bounce off the ground
    if (this.y > height - 25) {
      this.y = height - 25;
      this.vy *= -0.8; // Lose energy on bounce
    }
  }

  display() {
    fill(255, 0, 0);
    ellipse(this.x, this.y, 50);
  }
}

let ball;

function setup() {
  createCanvas(800, 600);
  ball = new Ball(width / 2, 100);
}

function draw() {
  background(200);
  ball.update();
  ball.display();
}
```

---

#### **7. Asynchronous Loading**

Preload assets (images, sounds) to ensure they load before the game starts.

**Example: Preloading Images**

```javascript
let playerImg;

function preload() {
  playerImg = loadImage("player.png");
}

function setup() {
  createCanvas(800, 600);
}

function draw() {
  background(200);
  image(playerImg, width / 2 - 50, height / 2 - 50, 100, 100);
}
```

