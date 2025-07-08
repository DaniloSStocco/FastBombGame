# FastBombGame
Segundo Projeto da disciplina de Eletrônica, 1° semestre no BCC

## Localização de cada elemento do circuito 
### -- OUTPUT --
ledVerde1 - pino 13 <br>
ledVerde2 - pino 12 <br>
ledVerde3 - pino 11<br>
ledAzul1 - pino 2 <br>
ledAzul2 - pino 3 <br> 
ledAzul3 - pino 4 <br>
ledVermelha1 - pino 6 <br>
ledVermelha2 - pino 8 <br>
buzzer - pino 7 <br>

### -- INPUTS --
botaoVerde - pino 10 <br>
botaoAzul - pino 5

### Código do Arduino
```
const int ledAzul[]  = {2, 3, 4};
const int ledVerde[] = {13, 12, 11};
const int ledVermelho[] = {6, 8};

const int botaoAzul = 5;
const int botaoVerde = 10;

const int buzzer = 7;

int pontosAzul = 0;
int pontosVerde = 0;

void setup() {
  for (int i = 0; i < 3; i++) {
    pinMode(ledAzul[i], OUTPUT);
    pinMode(ledVerde[i], OUTPUT);
  }

  pinMode(ledVermelho[0], OUTPUT);
  pinMode(ledVermelho[1], OUTPUT);

  pinMode(botaoAzul, INPUT_PULLUP);
  pinMode(botaoVerde, INPUT_PULLUP);

  pinMode(buzzer, OUTPUT);  

  Serial.begin(9600);
}

void loop() {
  if (pontosAzul >= 3) {
    anunciarVitoria("AZUL", ledAzul);
    while (true);
  } else if (pontosVerde >= 3) {
    anunciarVitoria("VERDE", ledVerde);
    while (true);
  }

  delay(random(2000, 5000));

  bool bomba = random(0, 3) == 0;  // 33% de chance de bomba

 
  digitalWrite(ledVermelho[0], HIGH);
  if (bomba) digitalWrite(ledVermelho[1], HIGH);

  unsigned long inicio = millis();
  bool clicou = false;

  while (millis() - inicio < 2000) {
    if (digitalRead(botaoAzul) == LOW) {
      clicou = true;
      if (bomba) {
        pontosAzul = max(0, pontosAzul - 1);
        tocarBuzzer();
      } else {
        pontosAzul++;
      }
      break;
    }
    if (digitalRead(botaoVerde) == LOW) {
      clicou = true;
      if (bomba) {
        pontosVerde = max(0, pontosVerde - 1);
        tocarBuzzer();
      } else {
        pontosVerde++;
      }
      break;
    }
  }

 
  digitalWrite(ledVermelho[0], LOW);
  digitalWrite(ledVermelho[1], LOW);

  if (clicou) {
    Serial.print("Pontos - Azul: ");
    Serial.print(pontosAzul);
    Serial.print(" | Verde: ");
    Serial.println(pontosVerde);
  }

  mostrarPontos();
  delay(1000);
}

void mostrarPontos() {
  for (int i = 0; i < 3; i++) {
    digitalWrite(ledAzul[i], i < pontosAzul ? HIGH : LOW);
    digitalWrite(ledVerde[i], i < pontosVerde ? HIGH : LOW);
  }
}

void anunciarVitoria(String jogador, const int leds[]) {

  for (int i = 0; i < 6; i++) {
    for (int j = 0; j < 3; j++) {
      digitalWrite(leds[j], HIGH);
    }
    delay(300);
    for (int j = 0; j < 3; j++) {
      digitalWrite(leds[j], LOW);
    }
    delay(300);
  }
}

void tocarBuzzer() {
  tone(buzzer, 1000);  
  delay(500);          
  noTone(buzzer);  
}
```

## Imagem do circuito no Tinkercad
![alt text](https://github.com/DaniloSStocco/FastBombGame/blob/main/imagens/tinkercadProj2.jpg "Imagem do circuito no tinkercad")

## Imagens do circuito
![alt text](https://github.com/DaniloSStocco/FastBombGame/blob/main/imagens/proj2foto1.png "Foto do circuito")
![alt text](https://github.com/DaniloSStocco/FastBombGame/blob/main/imagens/proj2foto2.png "Foto do circuito")
![alt text](https://github.com/DaniloSStocco/FastBombGame/blob/main/imagens/proj2foto3.png "Foto do circuito")
![alt text](https://github.com/DaniloSStocco/FastBombGame/blob/main/imagens/proj2foto4.png "Foto do circuito")

## Link do vídeo
https://youtube.com/shorts/59rSZbRxSzw?feature=shared

## Alunos
- Danilo Salmen Stocco
- Diogo Salmen Stocco
- Felipe Carlos Savoia Glücksman
- Rafael Senger
