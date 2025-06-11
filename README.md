let campo = [];
let cidade = [];
let recursos = [];

function setup() {
  createCanvas(800, 400);
  
  // Criando elementos do campo
  for (let i = 0; i < 5; i++) {
    campo.push(new Elemento(random(50, 200), random(200, 350), random(['🌾', '💧', '⚡️'])));
  }
  
  // Criando elementos da cidade
  for (let i = 0; i < 5; i++) {
    cidade.push(new Elemento(random(600, 750), random(50, 200), random(['🏢', '🏭', '🚗'])));
  }
}

function draw() {
  background(220);
  
  // Desenha o campo e a cidade
  fill(100, 200, 100);
  rect(0, 200, width / 2, 200); // Campo
  fill(150);
  rect(width / 2, 0, width / 2, 400); // Cidade
  
  // Exibe os elementos
  for (let e of campo) e.show();
  for (let e of cidade) e.show();
  
  // Atualiza e exibe recursos
  for (let r of recursos) {
    r.update();
    r.show();
  }
  
  // Remove recursos que chegaram ao destino
  recursos = recursos.filter(r => !r.chegou);
}

// Classe para representar elementos do campo e cidade
class Elemento {
  constructor(x, y, emoji) {
    this.x = x;
    this.y = y;
    this.emoji = emoji;
  }
  
  show() {
    textSize(32);
    text(this.emoji, this.x, this.y);
  }
}

// Classe para representar recursos trocados
class Recurso {
  constructor(x, y, destinoX, destinoY, tipo) {
    this.x = x;
    this.y = y;
    this.destinoX = destinoX;
    this.destinoY = destinoY;
    this.tipo = tipo;
    this.vel = random(1, 3);
    this.chegou = false;
  }
  
  update() {
    this.x = lerp(this.x, this.destinoX, 0.02);
    this.y = lerp(this.y, this.destinoY, 0.02);
    
    if (dist(this.x, this.y, this.destinoX, this.destinoY) < 5) {
      this.chegou = true;
    }
  }
  
  show() {
    textSize(24);
    text(this.tipo, this.x, this.y);
  }
}

// Criar recursos ao clicar
function mousePressed() {
  if (mouseX < width / 2) {
    // Campo envia recurso para a cidade
    let tipo = random(['🍎', '💧', '⚡️']);
    recursos.push(new Recurso(mouseX, mouseY, random(600, 750), random(50, 200), tipo));
  } else {
    // Cidade envia tecnologia para o campo
    let tipo = random(['⚙️', '🏭', '🚶‍♂️']);
    recursos.push(new Recurso(mouseX, mouseY, random(50, 200), random(200, 350), tipo));
  }
}
