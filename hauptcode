#include <Adafruit_GFX.h>
#include <Adafruit_ILI9341.h>
#include <SPI.h>

#define TFT_CS   5
#define TFT_RST  4
#define TFT_DC   6

Adafruit_ILI9341 tft = Adafruit_ILI9341(TFT_CS, TFT_DC, TFT_RST);

#define ILI9341_BROWN 0xA145

#define BTN_UP     22
#define BTN_DOWN   23
#define BTN_LEFT   24
#define BTN_RIGHT  25
#define BTN_A      26
#define BTN_B      27

const char* gamesAll[] = {"Snake", "Pong", "Pac-Man", "DonkeyKong"};
int gameCountAll = 4;

void playIntro() {
  tft.fillScreen(ILI9341_BLACK);
  for (int i = 0; i < 10; i++) {
    int px = i*12 + 10;
    tft.fillCircle(px, 120, 3, ILI9341_WHITE);
  }
  for (int pac = 0; pac < 10; pac++) {
    for (int mouth = 0; mouth < 2; mouth++) {
      tft.fillScreen(ILI9341_BLACK);
      for (int i = pac+1; i < 10; i++) {
        int px = i*12 + 10;
        tft.fillCircle(px, 120, 3, ILI9341_WHITE);
      }
      int x = pac*12 + 10;
      if (mouth == 0) {
        tft.fillCircle(x, 120, 8, ILI9341_YELLOW);
      } else {
        tft.fillCircle(x, 120, 8, ILI9341_YELLOW);
        tft.fillTriangle(x, 120, x+8, 115, x+8, 125, ILI9341_BLACK);
      }
      delay(120);
    }
  }
  tft.fillCircle(140, 120, 8, ILI9341_RED);
  delay(500);

  tft.fillScreen(ILI9341_BLACK);
  int ballX = 100, ballY = 120, dx = 3, dy = 2;
  int paddle1 = 100, paddle2 = 100;
  for (int frame = 0; frame < 30; frame++) {
    tft.fillScreen(ILI9341_BLACK);
    tft.fillRect(5, paddle1, 5, 40, ILI9341_WHITE);
    tft.fillRect(230, paddle2, 5, 40, ILI9341_WHITE);
    tft.fillCircle(ballX, ballY, 6, ILI9341_GREEN);
    ballX += dx; ballY += dy;
    if (ballY < 5 || ballY > 230) dy = -dy;
    if (ballX < 10 || ballX > 220) dx = -dx;
    delay(70);
  }

  int snakeX[12], snakeY[12];
  for (int i = 0; i < 12; i++) {
    snakeX[i] = 20 + i*10;
    snakeY[i] = 150;
  }
  for (int step = 0; step < 12; step++) {
    tft.fillScreen(ILI9341_BLACK);
    for (int i = 0; i <= step; i++) {
      tft.fillRect(snakeX[i], snakeY[i], 9, 9, ILI9341_GREEN);
    }
    delay(150);
  }

  tft.fillScreen(ILI9341_BLACK);
  tft.drawLine(0, 200, 240, 190, ILI9341_WHITE);
  tft.fillRect(0, 40, 30, 30, ILI9341_RED);
  int marioX = 100, marioY = 180;
  tft.fillRect(marioX, marioY, 10, 10, ILI9341_YELLOW);

  for (int f = 30; f < 200; f+=6) {
    tft.fillScreen(ILI9341_BLACK);
    tft.drawLine(0, 200, 240, 190, ILI9341_WHITE);
    tft.fillRect(0, 40, 30, 30, ILI9341_RED);
    tft.fillRect(marioX, marioY, 10, 10, ILI9341_YELLOW);
    tft.fillCircle(f, 180, 8, ILI9341_BROWN);
    delay(100);
  }
  tft.fillScreen(ILI9341_BLACK);
  tft.setTextColor(ILI9341_RED);
  tft.setTextSize(2);
  tft.setCursor(70, 110);
  tft.println("GAME OVER");
  delay(1500);

  tft.fillScreen(ILI9341_BLACK);
  tft.setTextColor(ILI9341_YELLOW);
  tft.setTextSize(2);
  tft.setCursor(40, 80);
  tft.println("RETRO");
  tft.setCursor(50, 110);
  tft.println("SPIELEBOX");
  delay(2500);
}

int menu() {
  int choice = 0;

  while (true) {
    tft.fillScreen(ILI9341_BLACK);
    tft.setTextSize(2);

    for (int i = 0; i < gameCountAll; i++) {
      if (i == choice) tft.setTextColor(ILI9341_YELLOW);
      else tft.setTextColor(ILI9341_WHITE);
      tft.setCursor(40, 40 + i * 30);
      tft.println(gamesAll[i]);
    }

    delay(200);

    if (digitalRead(BTN_UP) == LOW) { choice--; if (choice < 0) choice = gameCountAll-1; }
    if (digitalRead(BTN_DOWN) == LOW) { choice++; if (choice >= gameCountAll) choice = 0; }
    if (digitalRead(BTN_A) == LOW) return choice;
  }
}

void playSnake() {
  int x = 120, y = 160;
  int dx = 10, dy = 0;
  int foodX = random(0, 24) * 10, foodY = random(0, 32) * 10;
  int length = 5;
  int tailX[200], tailY[200];

  while (true) {
    tft.fillScreen(ILI9341_BLACK);

    if (digitalRead(BTN_UP) == LOW && dy==0) { dx = 0; dy = -10; }
    if (digitalRead(BTN_DOWN) == LOW && dy==0) { dx = 0; dy = 10; }
    if (digitalRead(BTN_LEFT) == LOW && dx==0) { dx = -10; dy = 0; }
    if (digitalRead(BTN_RIGHT) == LOW && dx==0) { dx = 10; dy = 0; }

    for (int i = length; i > 0; i--) {
      tailX[i] = tailX[i-1];
      tailY[i] = tailY[i-1];
    }
    tailX[0] = x; tailY[0] = y;

    x += dx; y += dy;

    if (x < 0 || x >= 240 || y < 0 || y >= 320) return;
    for (int i = 0; i < length; i++) {
      if (x == tailX[i] && y == tailY[i]) return;
    }

    if (x == foodX && y == foodY) {
      length++;
      foodX = random(0, 24) * 10;
      foodY = random(0, 32) * 10;
    }

    for (int i = 0; i < length; i++) {
      tft.fillRect(tailX[i], tailY[i], 9, 9, ILI9341_GREEN);
    }
    tft.fillRect(x, y, 9, 9, ILI9341_GREEN);
    tft.fillRect(foodX, foodY, 9, 9, ILI9341_RED);

    delay(120);
  }
}

void playPong() {
  int ballX = 120, ballY = 160, dx = 2, dy = 2;
  int paddle1 = 140, paddle2 = 140;

  while (true) {
    tft.fillScreen(ILI9341_BLACK);

    if (digitalRead(BTN_UP) == LOW) paddle1 -= 4;
    if (digitalRead(BTN_DOWN) == LOW) paddle1 += 4;

    if (ballY > paddle2+20) paddle2 += 3;
    if (ballY < paddle2+20) paddle2 -= 3;

    tft.fillRect(5, paddle1, 5, 40, ILI9341_WHITE);
    tft.fillRect(230, paddle2, 5, 40, ILI9341_WHITE);
    tft.fillCircle(ballX, ballY, 6, ILI9341_GREEN);

    ballX += dx; ballY += dy;
    if (ballY < 0 || ballY > 320) dy = -dy;

    if (ballX < 10 && ballY > paddle1 && ballY < paddle1+40) dx = -dx;
    if (ballX > 228 && ballY > paddle2 && ballY < paddle2+40) dx = -dx;

    if (ballX < 0 || ballX > 240) return;

    delay(30);
  }
}

void playPacMan() {
  int pacX = 40, pacY = 160;
  int dx = 0, dy = 0;

  while (true) {
    tft.fillScreen(ILI9341_BLACK);

    if (digitalRead(BTN_UP) == LOW) { dx=0; dy=-3; }
    if (digitalRead(BTN_DOWN) == LOW) { dx=0; dy=3; }
    if (digitalRead(BTN_LEFT) == LOW) { dx=-3; dy=0; }
    if (digitalRead(BTN_RIGHT) == LOW) { dx=3; dy=0; }

    pacX += dx; pacY += dy;

    tft.fillCircle(pacX, pacY, 12, ILI9341_YELLOW);
    tft.fillCircle(180, 160, 12, ILI9341_RED);

    int d2 = (pacX-180)*(pacX-180) + (pacY-160)*(pacY-160);
    if (d2 < 20*20) return;

    delay(60);
  }
}

void playDonkeyKong() {
  bool gameOver = false;
  int marioX = 20, marioY = 280;
  int jump = 0;

  struct Barrel { int x, y, dx; };
  Barrel barrels[10];
  int barrelCount = 0;

  while (!gameOver) {
    tft.fillScreen(ILI9341_BLACK);

    for (int i = 0; i < 4; i++) {
      int y = 320 - i*80;
      tft.drawLine(0, y, 240, y-20, ILI9341_WHITE);
    }

    tft.fillRect(0, 10, 30, 30, ILI9341_RED);

    if (digitalRead(BTN_LEFT) == LOW) marioX -= 3;
    if (digitalRead(BTN_RIGHT) == LOW) marioX += 3;
    if (digitalRead(BTN_A) == LOW && jump == 0) jump = 15;
    if (jump > 0) { marioY -= 4; jump--; }
    else if (marioY < 280) { marioY += 2; }

    tft.fillRect(marioX, marioY, 12, 12, ILI9341_YELLOW);

    if (random(0, 12) == 0 && barrelCount < 10) {
      barrels[barrelCount].x = 30;
      barrels[barrelCount].y = 40;
      barrels[barrelCount].dx = random(2, 5);
      barrelCount++;
    }

    for (int i = 0; i < barrelCount; i++) {
      barrels[i].x += barrels[i].dx;
      barrels[i].y += 2;
      if (barrels[i].x > 240) barrels[i].x = 30;

      tft.fillCircle(barrels[i].x, barrels[i].y, 8, ILI9341_BROWN);

      if (barrels[i].x < marioX+12 && barrels[i].x+8 > marioX &&
          barrels[i].y < marioY+12 && barrels[i].y+8 > marioY) {
        gameOver = true;
      }
    }
    delay(80);
  }

  tft.fillScreen(ILI9341_BLACK);
  tft.setTextColor(ILI9341_RED);
  tft.setTextSize(2);
  tft.setCursor(60, 150);
  tft.println("GAME OVER!");
  delay(2000);
}

void setup() {
  tft.begin();
  tft.setRotation(1);
  tft.fillScreen(ILI9341_BLACK);

  pinMode(BTN_LEFT, INPUT_PULLUP);
  pinMode(BTN_RIGHT, INPUT_PULLUP);
  pinMode(BTN_UP, INPUT_PULLUP);
  pinMode(BTN_DOWN, INPUT_PULLUP);
  pinMode(BTN_A, INPUT_PULLUP);
  pinMode(BTN_B, INPUT_PULLUP);

  randomSeed(analogRead(A0));

  playIntro();
}

void loop() {
  int g = menu();

  if (g == 0) playSnake();
  else if (g == 1) playPong();
  else if (g == 2) playPacMan();
  else if (g == 3) playDonkeyKong();
}
// =============================================================
void setup() {
  tft.begin();
  tft.setRotation(1);
  tft.fillScreen(ILI9341_BLACK);

  pinMode(BTN_LEFT, INPUT_PULLUP);
  pinMode(BTN_RIGHT, INPUT_PULLUP);
  pinMode(BTN_UP, INPUT_PULLUP);
  pinMode(BTN_DOWN, INPUT_PULLUP);
  pinMode(BTN_A, INPUT_PULLUP);
  pinMode(BTN_B, INPUT_PULLUP);

  randomSeed(analogRead(A0));

  playIntro();
}

void loop() {
  int g = menu();

  if (g == 0) playSnake();
  else if (g == 1) playPong();
  else if (g == 2) playPacMan();
  else if (g == 3) playDonkeyKong();
}
