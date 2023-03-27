#include <iostream>
#include <sstream>
#include <fstream>>
#include <SFML/Graphics.hpp>
#include <SFML/Audio.hpp>
#include <SFML/System.hpp>
#include <SFML/Window.hpp>
#include <SFML/Network.hpp>
#include <cstdlib>
using namespace std;
using namespace sf;

Time x; //total pause time
bool f = 0; // just a boolan to limit its addition

//level completion
bool level1Completed = false;
bool level2Completed = false;
//ifstream Save("save.txt");
//ofstream Load("save.txt");

const int blocksnumY = 30;
const int blocksnumX = 30;

void right(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator);
void left(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator);
void backward(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator);
void forward(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator);
void animation(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator, int& animationIndicator);
void Enemyanimation(Texture& enemyd, Sprite& enemy, int& EnemyFrameIndicator, int& EnemyAnimationIndicator);
void EnemyMotion_right_left1(Texture& enemyTexture, Sprite enemySprite[3], int& EnemyFrameIndicator, int& EnemyAnimationIndicator);
void EnemyMotion_up_down(Texture& enemyTexture, Sprite enemySprite[3], int& EnemyFrameIndicator, int& EnemyAnimationIndicator);
void bombfun(int& t1, int& t2, int& t3, int& index, Sprite bomb[3], bool isbom[3], Sprite& bmanSprite2);
void bmanlose(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator, int& animationIndicator, bool& bmanld, Texture& bmanl);
void bmanlose2(Texture& bmanTexture2, Sprite& bmanSprite, int& FrameIndicator, int& animationIndicator, bool& bmanld, Texture& bmanl);
void Ecollisonrl(Sprite S[3], Sprite& b);
void Ecollisonud(Sprite S[3], Sprite& b);
void collison(Sprite& S, Sprite& b);
//void save(ifstream& Load, bool& level1Completed, bool& level2Completed);
//void load(ofstream& Load, bool& level1Completed, bool& level2Completed);

bool cleft, cright, cup, cdown;
Clock enemySprite_clock;

const int windowWidth = 1850;
const int windowHeight = 1050;
int windowIndex = 0;

int Map[15][40] = {};



const int stepSize = 5;  //distance of movement of single step 
const int bmanWidth = 57; //height of the player character in the sprite sheet
const int bmanHeight = 75; //width of the player character in the sprite sheet

const int enemyWidth = 55; //width of the enemy in the spritesheet
const int enemyHeight = 56; //height of the enemy in the spritesheet
int enemyStepSizex[3] = { 2,2,2 };
int enemyStepSizey[3] = { 2,2,2 };
int enemyStepSizex2[3] = { 2,2,2 };
int enemyStepSizey2[3] = { 2,2,2 };


//basic idea of the structs we want to use 
struct player {
	int health = 3;
	int score = 0;
	int num_of_bombs = 25;
	bool powerup_obtained = false;
} player1, player2;

// player1.score += 2000;
struct enemy {
	
};

//this one the TA told me about and I want to edit my code into it later
struct map {

};

//this function is figuring out how to create the map using grid and cells logic
void map1();


Clock respawn;
bool ismoving = true;

int main()
{

	bool bmanld2 = false;
	bool bmanld = false;
	int count1 = 0;
	Clock bomb_clock;
	Clock fire_clock;
	Clock fire_clock1;
	Clock enemyclock;
	//Clock respawn2;
	int count = 0;
	bool bom = false;
	int bmanpositionx;
	int bmanpositiony;


	map1();

	//saving progress?


	//Window
	RenderWindow window(VideoMode(windowWidth, windowHeight), "Bomberman", Style::Titlebar | Style::Close);
	window.setFramerateLimit(30);


	View Camera(sf::FloatRect(0, 0, windowWidth, windowHeight));

	//Fonts
	Font pixelEmulator;
	pixelEmulator.loadFromFile("textures/mainMenu/PixelEmulator.ttf");
	Font BmanFont;
	BmanFont.loadFromFile("textures/mainMenu/BOMBERMAN.ttf");

	//Main Menu 
	Texture titleScreenImage;
	titleScreenImage.loadFromFile("textures/mainMenu/title_screen.png");
	if (!titleScreenImage.loadFromFile("textures/mainMenu/title_screen.png")) {
		return EXIT_FAILURE;
	}
	Sprite titleScreen;
	titleScreen.setTexture(titleScreenImage);
	titleScreen.setScale(Vector2f(1.1, 1.1));
	Text play("Play", BmanFont);
	play.setPosition(100, 700);
	play.setCharacterSize(100);
	play.setOutlineColor(Color(30, 35, 128));
	play.setOutlineThickness(10);
	Text exit("Exit", BmanFont);
	exit.setPosition(100, 850);
	exit.setCharacterSize(50);
	exit.setOutlineColor(Color(30, 35, 128));
	exit.setOutlineThickness(7);
	// play submenu: index: - 1
	Text Campaign("Single Player Mode", BmanFont);
	Campaign.setPosition(100, 700);
	Campaign.setCharacterSize(50);
	Campaign.setOutlineColor(Color::Black);
	Campaign.setOutlineThickness(5);
	Text Vsmode("VS Mode", BmanFont);
	Vsmode.setPosition(100, 800);
	Vsmode.setCharacterSize(50);
	Vsmode.setOutlineColor(Color::Black);
	Vsmode.setOutlineThickness(5);
	// Levels submenu: index - 2;
	Text level1("Level 1", BmanFont);
	level1.setPosition(100, 700);
	level1.setCharacterSize(50);
	level1.setOutlineColor(Color::Black);
	level1.setOutlineThickness(5);
	Text level2("Level 2", BmanFont);
	level2.setPosition(100, 800);
	level2.setCharacterSize(50);
	level2.setOutlineColor(Color::Black);
	level2.setOutlineThickness(5);
	//Vs mode maps submenu: index - 3;
	Text MapOne("Map 1", BmanFont);
	MapOne.setPosition(100, 700);
	MapOne.setCharacterSize(50);
	MapOne.setOutlineColor(Color::Black);
	MapOne.setOutlineThickness(5);
	Text MapTwo("Map 2", BmanFont);
	MapTwo.setPosition(100, 800);
	MapTwo.setCharacterSize(50);
	MapTwo.setOutlineColor(Color::Black);
	MapTwo.setOutlineThickness(5);
	
	//Pause menu
	bool pauseOn = false;
	Text Pause("Pause", pixelEmulator);
	Pause.setPosition(750, 250);
	Pause.setCharacterSize(100);
	Pause.setOutlineColor(Color::Black);
	Pause.setOutlineThickness(8);
	Text Continue("Continue", pixelEmulator);
	Continue.setPosition(780, 450);
	Continue.setCharacterSize(50);
	Continue.setOutlineColor(Color::Black);
	Continue.setOutlineThickness(5);
	Text Quit("Quit", pixelEmulator);
	Quit.setPosition(850, 550);
	Quit.setCharacterSize(50);
	Quit.setOutlineColor(Color::Black);
	Quit.setOutlineThickness(5);
	//pause menu clock
	Clock pausetimer;
	Time timepaused;

	//Clock
	Clock clock;
	Time timee;
	stringstream ss;
	int s = 0;
	int m = 0;
	Text gametimer(" ", pixelEmulator);
	gametimer.setPosition(850, 25);
	gametimer.setCharacterSize(50);

	//Score
	Text score(" ", pixelEmulator);
	score.setPosition(1300, 25);
	score.setCharacterSize(50);

	//Health
	Text lives(" ", pixelEmulator);
	lives.setPosition(280, 50);
	lives.setCharacterSize(30);
	lives.setOutlineColor(Color::Black);
	lives.setOutlineThickness(3);

	Text lives2(" ", pixelEmulator);
	lives2.setPosition(1320, 50);
	lives2.setCharacterSize(30);
	lives2.setOutlineColor(Color::Black);
	lives2.setOutlineThickness(3);

	//Game Over
	bool lose = false;
	Text GameOver("Game Over", pixelEmulator);
	GameOver.setPosition(550, 350);
	GameOver.setCharacterSize(100);
	GameOver.setOutlineColor(Color::Black);
	GameOver.setOutlineThickness(8);
	Text instructions("Press Esc to Exit to Main Menu", pixelEmulator);
	instructions.setPosition(650, 600);
	instructions.setCharacterSize(20);
	instructions.setOutlineColor(Color::Black);
	instructions.setOutlineThickness(3);
	RectangleShape BgScreen;
	BgScreen.setSize(Vector2f(windowWidth, windowHeight));
	BgScreen.setFillColor(Color(255, 170, 0));

	//vs mode win
	Text Winner(" ", pixelEmulator);
	Winner.setPosition(350, 350);
	Winner.setCharacterSize(100);
	Winner.setOutlineColor(Color::Black);
	Winner.setOutlineThickness(5);

	//Victory
	bool win = false;
	Text Victory("!Level Completed!", pixelEmulator);
	Victory.setPosition(300, 350);
	Victory.setCharacterSize(100);
	Victory.setOutlineColor(Color::Black);
	Victory.setOutlineThickness(8);



	//bomberman texture & sprite
	Texture bmanTexture;
	//Texture bman_texture;
	bmanTexture.loadFromFile("textures/Entities/BombermanSpriteSheet.png");
	//bman_texture.loadFromFile("textures/Entities/BombermanSpriteSheet.png");
	if (!bmanTexture.loadFromFile("textures/Entities/BombermanSpriteSheet.png")) {
		return EXIT_FAILURE;
	}
	Sprite bmanSprite;
	bmanSprite.setTexture(bmanTexture);
	bmanSprite.setScale(.70, .70);
	bmanSprite.setPosition(132, 855);
	bmanSprite.setTextureRect(IntRect(0, 0, bmanWidth, bmanHeight));

	//second player
	Texture bmanTexture2;
	bmanTexture2.loadFromFile("textures/Entities/BombermanSpriteSheet2.png");
	if (!bmanTexture2.loadFromFile("textures/Entities/BombermanSpriteSheet2.png")) {
		return EXIT_FAILURE;
	}
	Sprite bmanSprite2;
	bmanSprite2.setTexture(bmanTexture2);
	bmanSprite2.setScale(.70, .70);
	bmanSprite2.setPosition(1630, 900);
	bmanSprite2.setTextureRect(IntRect(0, 0, bmanWidth, bmanHeight));



	Texture bmanl;
	bmanl.loadFromFile("textures/Entities/bmanl.png");
	int bmanlanim = 0;

	//enemy texture & sprite
	int EnemyAnimationIndicator = 0; //Indicates the frame in animation for the enemy
	int EnemyFrameIndicator = 0; // Indicates starting frame for the selected animation for the enemy
	Sprite enemy[3];
	Texture Enemy;
	Enemy.loadFromFile("textures/Entities/enemies - Copy.png");
	if (!Enemy.loadFromFile("textures/Entities/enemies - Copy.png")) {
		return EXIT_FAILURE;
	}
	for (size_t i = 0; i < 3; ++i) {
		enemy[i].setTexture(Enemy);
		enemy[i].setTextureRect(IntRect(0, 0, enemyWidth, enemyHeight));
		enemy[i].setScale(0.8, 0.8);
	}
	Texture enemyd;
	enemyd.loadFromFile("textures/Entities/enemiesd - Copy.png");
	Texture enemyd2;
	enemyd.loadFromFile("textures/Entities/enemiesd - Copy.png");
	enemy[2].setPosition(195, 280);
	enemy[1].setPosition(195, 740);
	enemy[0].setPosition(195, 510);


	//enemy2 
	int EnemyAnimationIndicator2 = 0; //Indicates the frame in animation for the enemy
	int EnemyFrameIndicator2 = 0; // Indicates starting frame for the selected animation for the enemy
	Sprite enemy2[3];
	Texture Enemy2;
	Enemy2.loadFromFile("textures/Entities/enemies2.png");
	if (!Enemy2.loadFromFile("textures/Entities/enemies2.png")) {
		return EXIT_FAILURE;
	}
	for (size_t i = 0; i < 3; ++i) {
		enemy2[i].setTexture(Enemy2);
		enemy2[i].setTextureRect(IntRect(0, 0, enemyWidth - 2, enemyHeight));
		enemy2[i].setScale(0.8, 0.8);
	}
	enemy2[2].setPosition(913, 280);
	enemy2[1].setPosition(310, 280);
	enemy2[0].setPosition(1508, 280);

	int enemy_num = 6;

	//Block
	Texture blockt;
	blockt.loadFromFile("textures/Entities/blocks1.png");
	if (!blockt.loadFromFile("textures/Entities/blocks1.png")) {
		return EXIT_FAILURE;
	}
	Texture blockt2;
	blockt2.loadFromFile("textures/Entities/blocks2.png");
	if (!blockt2.loadFromFile("textures/Entities/blocks2.png")) {
		return EXIT_FAILURE;
	}
	Sprite block(blockt);
	int blocksizeX = block.getGlobalBounds().width / 8;
	int blocksizeY = block.getGlobalBounds().height;
	block.setTextureRect(IntRect(0, 0, blocksizeX, blocksizeY));
	block.setScale(1.2, 1.2);
	block.setPosition(0, 100);


	srand(time(0));
	for (int i = 0; i < blocksnumY; i++)
		for (int j = 0; j < blocksnumX; j++)
			Map[i][j] = 0;
	map1();
	Sprite blocks[blocksnumY][blocksnumX];
	for (int i = 0; i < blocksnumY; i++)
	{
		for (int j = 0; j < blocksnumX; j++) {
			if (Map[i][j]) {
				blocks[i][j] = block;
				blocks[i][j].move(block.getGlobalBounds().width * i, block.getGlobalBounds().height * j);
				if (Map[i][j] == 1)
					block.setTextureRect(IntRect(0, 0, blocksizeX, blocksizeY));
				if (Map[i][j] == 2)
					blocks[i][j].setTextureRect(IntRect(blocksizeX, 0, blocksizeX, blocksizeY));
			}
		}
	}

	// Green Background
	RectangleShape frame(Vector2f(block.getGlobalBounds().width * 29, block.getGlobalBounds().height * 15));
	frame.setFillColor(Color(0, 102, 10));
	frame.setPosition(block.getPosition().x + block.getGlobalBounds().width, block.getPosition().y + block.getGlobalBounds().height);




	//GUI textures:
	RectangleShape gameHeader;
	gameHeader.setSize(Vector2f(windowWidth, 100));
	gameHeader.setFillColor(Color(255, 170, 0));
	gameHeader.setOutlineColor(Color::Black);
	gameHeader.setOutlineThickness(3);

	RectangleShape TimerBox;
	TimerBox.setSize(Vector2f(120, 50));
	TimerBox.setFillColor(Color::Black);
	TimerBox.setOutlineColor(Color::Red);
	TimerBox.setOutlineThickness(3);
	TimerBox.setPosition(850, 35);

	RectangleShape ScoreBox;
	ScoreBox.setSize(Vector2f(350, 50));
	ScoreBox.setFillColor(Color::Black);
	ScoreBox.setOutlineColor(Color::Red);
	ScoreBox.setOutlineThickness(3);
	ScoreBox.setPosition(1300, 35);

	//lives UI
	Texture bmanIconTexture;
	bmanIconTexture.loadFromFile("textures/download textures/Bomberman.png");
	if (!bmanIconTexture.loadFromFile("textures/download textures/Bomberman.png")) {
		return EXIT_FAILURE;
	}
	Sprite bmanIcon;
	bmanIcon.setTexture(bmanIconTexture);
	bmanIcon.setScale(1, 1);
	bmanIcon.setPosition(250, 30);
	bmanIcon.setTextureRect(IntRect(0, 0, bmanWidth, bmanHeight - 30));

	Texture bmanIconBlackTexture;
	bmanIconBlackTexture.loadFromFile("textures/download textures/Bomberman2.png");
	if (!bmanIconBlackTexture.loadFromFile("textures/download textures/Bomberman2.png")) {
		return EXIT_FAILURE;
	}
	Sprite bmanIcon2;
	bmanIcon2.setTexture(bmanIconBlackTexture);
	bmanIcon2.setScale(1, 1);
	bmanIcon2.setPosition(1300, 30);
	bmanIcon2.setTextureRect(IntRect(0, 0, bmanWidth, bmanHeight - 30));




	int animationIndicator = 0; //Indicates the frame in animation 
	int FrameIndicator = 0; // Indicates starting frame for the selected animation
	int animationIndicator2 = 0;
	int FrameIndicator2 = 0;


	//bomb texture
	int index = 0;
	int t1 = 0, t2 = 0, t3 = 0;
	Sprite bomb[3];
	bool isbom[3] = {};
	Texture Bomb;
	Bomb.loadFromFile("textures/Entities/bombs.png");
	if (!Bomb.loadFromFile("textures/Entities/bombs.png")) {
		return EXIT_FAILURE;
	}
	int bomb_animation_indicator = 0;
	for (size_t i = 0; i < 3; ++i) {
		bomb[i].setTexture(Bomb);
		bomb[i].setPosition(-2000, 2000);
		bomb[i].setTextureRect(IntRect(0, 0, 50, 50));
		bomb[i].setScale(1, 1);
		isbom[i] = 0;
	}

	Sprite fire[3];
	Texture Fire;
	Fire.loadFromFile("textures/Entities/fire2.png");
	int fireanim = 0;
	for (size_t i = 0; i < 3; i++) {
		fire[i].setTexture(Fire);
		fire[i].setTextureRect(IntRect(0, 0, 148, 756));
		fire[i].setScale(0.3, 0.3);
		fire[i].setPosition(-2000, -2000);
	}

	Sprite fire1[3];
	Texture Fire1;
	Fire1.loadFromFile("textures/Entities/fire1.png");
	int fireanim1 = 0;
	for (size_t i = 0; i < 3; i++) {
		fire1[i].setTexture(Fire1);
		fire1[i].setTextureRect(IntRect(0, 0, 756, 148));
		fire1[i].setScale(0.3, 0.3);
		fire1[i].setPosition(-2000, -2000);
	}

	//soundeffects
	SoundBuffer Bombsound;
	Bombsound.loadFromFile("Battle Mode Sounds (41).wav");
	Sound bombsound;
	bombsound.setBuffer(Bombsound);

	SoundBuffer Bmandead;
	Bmandead.loadFromFile("music/Sounds/Player/Player Dead.wav");
	Sound bmandead;
	bmandead.setBuffer(Bmandead);

	SoundBuffer Enemydead;
	Enemydead.loadFromFile("music/Sounds/Enemies/Dead/Monster Sounds (70).wav");
	Sound enemydead;
	enemydead.setBuffer(Enemydead);

	SoundBuffer Click;
	Click.loadFromFile("Button Press.wav");
	Sound click;
	click.setBuffer(Click);

	SoundBuffer Music;
	Music.loadFromFile("1-06. Planet Timbertree (World 2).flac");
	Sound music;
	music.setBuffer(Music);
	music.play();
	music.setLoop(true);

	SoundBuffer Ready;
	Ready.loadFromFile("music/Sounds/Player/V_RED_004_E.wav");
	Sound ready;
	ready.setBuffer(Ready);

	SoundBuffer Ready2;
	Ready2.loadFromFile("music/Sounds/Player/V_RED_003_E.wav");
	Sound ready2;
	ready2.setBuffer(Ready2);

	SoundBuffer Pass;
	Pass.loadFromFile("mixkit-game-level-completed-2059.wav");
	Sound pass;
	pass.setBuffer(Pass);

	SoundBuffer Gameover;
	Gameover.loadFromFile("mixkit-wrong-answer-fail-notification-946.wav");
	Sound gameover;
	gameover.setBuffer(Gameover);



	int select = 1; // for menu selection

	//Game loop
	while (window.isOpen()) {
		//music.play();
		//Event polling
		Event event;
		while (window.pollEvent(event))
		{
			if (event.type == Event::Closed) {
				window.close();
			}
		}

		if (windowIndex == 0 && pauseOn == false) {

			if (Keyboard::isKeyPressed(Keyboard::S)) {
				click.play();
				select = 1;
				exit.setOutlineColor(Color(255, 170, 0));
				play.setOutlineColor(Color(30, 35, 128));
			}
			if (Keyboard::isKeyPressed(Keyboard::W)) {
				click.play();
				select = 0;
				play.setOutlineColor(Color(255, 170, 0));
				exit.setOutlineColor(Color(30, 35, 128));
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter)) {
				if (select == 0) {
					windowIndex = -1;
				}
				else if (select == 1) {
					window.close();
				}
			}

		}
		//reset the game 
		if (windowIndex < 0) {
			x = seconds(0.0f);
			clock.restart();
			lose = false;
			win = false;
			bmanSprite.setPosition(132, 855);
			bmanSprite2.setPosition(1630, 900);
			player1 = {};
			player2 = {};
			for (int j = 0; j < 3; ++j) {
				enemy[j].setScale(0.8, 0.8);
				enemy2[j].setScale(0.8, 0.8);
			}
			enemy[2].setPosition(block.getPosition().x + block.getGlobalBounds().width * 6, block.getPosition().y + block.getGlobalBounds().height * 2+583);
			enemy[1].setPosition(block.getPosition().x + block.getGlobalBounds().width * 6, block.getPosition().y + block.getGlobalBounds().height * 4);
			enemy[0].setPosition(block.getPosition().x + block.getGlobalBounds().width * 6, block.getPosition().y + block.getGlobalBounds().height * 6+127);
			enemy2[2].setPosition(block.getPosition().x + block.getGlobalBounds().width * 8+846, block.getPosition().y + block.getGlobalBounds().height * 6);
			enemy2[1].setPosition(block.getPosition().x + block.getGlobalBounds().width * 10-223, block.getPosition().y + block.getGlobalBounds().height * 6);
			enemy2[0].setPosition(block.getPosition().x + block.getGlobalBounds().width * 12+135, block.getPosition().y + block.getGlobalBounds().height * 6);


		}

		if (windowIndex == -1) {
			if (Keyboard::isKeyPressed(Keyboard::S)) {
				click.play();
				select = 3;
				Vsmode.setOutlineColor(Color(255, 170, 0));
				Campaign.setOutlineColor(Color::Black);
			}
			if (Keyboard::isKeyPressed(Keyboard::W)) {
				click.play();
				select = 2;
				Campaign.setOutlineColor(Color(255, 170, 0));
				Vsmode.setOutlineColor(Color::Black);
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter)) {
				if (select == 2) {
					windowIndex = -2;
				}
				else if (select == 3) {
					//windowIndex = 22;
					windowIndex = -3;
				}
			}
		}

		if (windowIndex == -2) {
			if (Keyboard::isKeyPressed(Keyboard::S)) {
				click.play();
				level1.setOutlineColor(Color::Black);
				if (level1Completed) {
					select = 5;
					level2.setOutlineColor(Color(255, 170, 0));
				}
				else {
					select = NULL;
					level2.setOutlineColor(Color(181, 181, 181));
				}
			}
			if (Keyboard::isKeyPressed(Keyboard::W)) {
				click.play();
				select = 4;
				level1.setOutlineColor(Color(255, 170, 0));
				level2.setOutlineColor(Color::Black);
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter)) {
				if (select == 4) {
					ready.play();
					windowIndex = 1;
				}
				else if (select == 5) {
					ready2.play();
					windowIndex = 2;
				}
			}
		}

		if (windowIndex == -3) {
			if (Keyboard::isKeyPressed(Keyboard::S)) {
				click.play();
				select = 5;
				MapOne.setOutlineColor(Color::Black);
				MapTwo.setOutlineColor(Color(255, 170, 0));
			}
			if (Keyboard::isKeyPressed(Keyboard::W)) {
				click.play();
				select = 4;
				MapOne.setOutlineColor(Color(255, 170, 0));
				MapTwo.setOutlineColor(Color::Black);
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter)) {
				if (select == 4) {
					windowIndex = 21;
				}
				else if (select == 5) {
					windowIndex = 22;
				}
			}
		}

		if (pauseOn) {
			music.pause();
			if (Keyboard::isKeyPressed(Keyboard::S)) {
				click.play();
				select = 7;
				Quit.setOutlineColor(Color(255, 170, 0));
				Continue.setOutlineColor(Color::Black);
			}
			if (Keyboard::isKeyPressed(Keyboard::W)) {
				click.play();
				select = 6;
				Continue.setOutlineColor(Color(255, 170, 0));
				Quit.setOutlineColor(Color::Black);
			}
			if (Keyboard::isKeyPressed(Keyboard::Enter)) {
				pauseOn = false;
				music.play();
				if (select == 7) {
					windowIndex = 0;
				}
			}
			//cout << clock.getElapsedTime().asSeconds() - pausetimer.getElapsedTime().asSeconds() << endl;
		}


		if (windowIndex > 0 && !pauseOn) {
			if (Keyboard::isKeyPressed(Keyboard::Escape)) {
				if (lose || win) {
					windowIndex = 0;
				}
				else {
					pauseOn = true;
				}
			}
			//collison
			cleft = cright = cup = cdown = true;
			for (int i = 0; i < blocksnumY; i++) {
				for (int j = 0; j < blocksnumX; j++) {
					collison(bmanSprite, blocks[i][j]);
					collison(bmanSprite2, blocks[i][j]);
				}
			}
			//move
			if (Keyboard::isKeyPressed(Keyboard::S) && cdown && ismoving) {
				forward(bmanTexture, bmanSprite, FrameIndicator);
				animation(bmanTexture, bmanSprite, FrameIndicator, animationIndicator);

			}
			else if (Keyboard::isKeyPressed(Keyboard::D) && cright && ismoving) {
				right(bmanTexture, bmanSprite, FrameIndicator);
				animation(bmanTexture, bmanSprite, FrameIndicator, animationIndicator);

			}
			else if (Keyboard::isKeyPressed(Keyboard::A) && cleft && ismoving) {
				left(bmanTexture, bmanSprite, FrameIndicator);
				animation(bmanTexture, bmanSprite, FrameIndicator, animationIndicator);

			}
			else if (Keyboard::isKeyPressed(Keyboard::W) && cup && ismoving) {
				backward(bmanTexture, bmanSprite, FrameIndicator);
				animation(bmanTexture, bmanSprite, FrameIndicator, animationIndicator);

			}
			//for 2nd player
			if (windowIndex >= 20) {
				if (Keyboard::isKeyPressed(Keyboard::Down) && cdown && ismoving) {
					forward(bmanTexture2, bmanSprite2, FrameIndicator2);
					animation(bmanTexture2, bmanSprite2, FrameIndicator2, animationIndicator2);

				}
				else if (Keyboard::isKeyPressed(Keyboard::Up) && cup && ismoving) {
					backward(bmanTexture2, bmanSprite2, FrameIndicator2);
					animation(bmanTexture2, bmanSprite2, FrameIndicator2, animationIndicator2);

				}
				else if (Keyboard::isKeyPressed(Keyboard::Right) && cright && ismoving) {
					right(bmanTexture2, bmanSprite2, FrameIndicator2);
					animation(bmanTexture2, bmanSprite2, FrameIndicator2, animationIndicator2);

				}
				else if (Keyboard::isKeyPressed(Keyboard::Left) && cleft && ismoving) {
					left(bmanTexture2, bmanSprite2, FrameIndicator2);
					animation(bmanTexture2, bmanSprite2, FrameIndicator2, animationIndicator2);

				}
				if (Keyboard::isKeyPressed(Keyboard::L) && count == 0 && ismoving) {
					bombfun(t1, t2, t3, index, bomb, isbom, bmanSprite2);
				}
			}
			//Bomb
			if (Keyboard::isKeyPressed(Keyboard::E) && count == 0 && ismoving) {
				bombfun(t1, t2, t3, index, bomb, isbom, bmanSprite);
			}
			if (isbom[0] == true) {
				t1++;
			}
			if (isbom[1] == true) {
				t2++;
			}
			if (isbom[2] == true) {
				t3++;
			}
			if (t1 > 60) {
				bombsound.play();
				fireanim = 0;
				fireanim1 = 0;
				fire[0].setPosition(bomb[0].getPosition().x, bomb[0].getPosition().y - 100);
				fire1[0].setPosition(bomb[0].getPosition().x - 92, bomb[0].getPosition().y - 10);
				bomb[0].setPosition(-200, -200);
				isbom[0] = false;
				t1 = 0;
			}
			if (t2 > 60) {
				bombsound.play();
				fireanim = 0;
				fireanim1 = 0;
				fire[1].setPosition(bomb[1].getPosition().x, bomb[1].getPosition().y - 100);
				fire1[1].setPosition(bomb[1].getPosition().x - 92, bomb[1].getPosition().y - 10);
				bomb[1].setPosition(-200, -200);
				isbom[1] = false;
				t2 = 0;
			}
			if (t3 > 60) {
				bombsound.play();
				fireanim = 0;
				fireanim1 = 0;
				fire[2].setPosition(bomb[2].getPosition().x, bomb[2].getPosition().y - 100);
				fire1[2].setPosition(bomb[2].getPosition().x - 92, bomb[2].getPosition().y - 10);
				bomb[2].setPosition(-200, -200);
				isbom[2] = false;
				t3 = 0;
			}
			for (int i = 0; i < 3; i++) {
				bomb[i].setTextureRect(IntRect(bomb_animation_indicator * 50, 0, 50, 50));
			}
			if (bomb_clock.getElapsedTime().asSeconds() > 0.08) {
				bomb_animation_indicator++;
				bomb_clock.restart();
				bomb_animation_indicator++;
				bomb_animation_indicator = bomb_animation_indicator % 3;
			}
			if (fire_clock.getElapsedTime().asSeconds() > 0.1) {

				fire[0].setTextureRect(IntRect(fireanim * 148, 0, 148, 756));
				fire1[0].setTextureRect(IntRect(0, fireanim * 148, 756, 148));
				fire[1].setTextureRect(IntRect(fireanim * 148, 0, 148, 756));
				fire1[1].setTextureRect(IntRect(0, fireanim * 148, 756, 148));
				fire[2].setTextureRect(IntRect(fireanim * 148, 0, 148, 756));
				fire1[2].setTextureRect(IntRect(0, fireanim * 148, 756, 148));
				if (fireanim > 7) {
					fire[0].setPosition(-2000, -2000);
					fire1[0].setPosition(-2000, -2000);
					fire[1].setPosition(-2000, -2000);
					fire1[1].setPosition(-2000, -2000);
					fire[2].setPosition(-2000, -2000);
					fire1[2].setPosition(-2000, -2000);
					// fireanim = 0;
				}
				fireanim++;
				fire_clock.restart();

			}

			for (int i = 0; i < 3; i++) {
				if ((fire[i].getGlobalBounds().intersects(bmanSprite.getGlobalBounds()) || fire1[i].getGlobalBounds().intersects(bmanSprite.getGlobalBounds())) && (bmanld == false)) {
					bmanld = true;
					ismoving = false;
					player1.health--;
					bmandead.play();
				}
				if ((fire[i].getGlobalBounds().intersects(bmanSprite2.getGlobalBounds()) || fire1[i].getGlobalBounds().intersects(bmanSprite2.getGlobalBounds())) && (bmanld2 == false)) {
					bmanld2 = true;
					ismoving = false;
					player2.health--;
					bmandead.play();
				}
				bmanlose(bmanTexture, bmanSprite, FrameIndicator, animationIndicator, bmanld, bmanl);
				bmanlose2(bmanTexture2, bmanSprite2, FrameIndicator2, animationIndicator2, bmanld2, bmanl);
				for (int j = 0; j < 3; ++j) {
					if ((fire[i].getGlobalBounds().intersects(enemy[j].getGlobalBounds()) || fire1[i].getGlobalBounds().intersects(enemy[j].getGlobalBounds()))) {
						enemy[j].setScale(0, 0);
						enemydead.play();
						//Enemyanimation( enemyd, enemy[j], EnemyFrameIndicator, EnemyAnimationIndicator);
						player1.score += 1000;
					}
					if ((fire[i].getGlobalBounds().intersects(enemy2[j].getGlobalBounds()) || fire1[i].getGlobalBounds().intersects(enemy2[j].getGlobalBounds()))) {
						enemy2[j].setScale(0, 0);
						enemydead.play();
					//	Enemyanimation(enemyd2, enemy[j], EnemyFrameIndicator2, EnemyAnimationIndicator2);
						//enemy2[j].setPosition(-2000, -2000);
						player1.score += 1500;
					}
				}
			}

			for (int i = 0; i < blocksnumY; i++) {
				for (int j = 0; j < blocksnumX; j++) {
					for (int k = 0; k < 3; k++) {

						EnemyMotion_right_left1(Enemy, enemy, EnemyFrameIndicator, EnemyAnimationIndicator);
						Ecollisonrl(enemy, blocks[i][j]);
						if ((bmanSprite.getGlobalBounds().intersects(enemy[k].getGlobalBounds())) && (bmanld == false) && windowIndex < 20) {
							bmanld = true;
							ismoving = false;
							bmanlose(bmanTexture, bmanSprite, FrameIndicator, animationIndicator, bmanld, bmanl);
							player1.health--;
							bmandead.play();
						}
						EnemyMotion_up_down(Enemy2, enemy2, EnemyFrameIndicator2, EnemyAnimationIndicator2);
						Ecollisonud(enemy2, blocks[i][j]);
						if ((bmanSprite.getGlobalBounds().intersects(enemy2[k].getGlobalBounds())) && (bmanld == false) && windowIndex < 20) {
							bmanld = true;
							ismoving = false;
							bmanlose(bmanTexture, bmanSprite, FrameIndicator, animationIndicator, bmanld, bmanl);
							player1.health--;
							bmandead.play();
						}


					}
				}

			}




			if (f)
			{
				x += timepaused;
				f = 0;
			}
			pausetimer.restart();
		}
		//Update

		//Render
		if (windowIndex > 0) {
			//timer (need to be put in a function)
			ss.str(" ");

			timee = clock.getElapsedTime() - x;

			s = timee.asSeconds();
			m = s / 60;
			s = s - (m * 60);
			ss << m;
			if (s < 10) {
				ss << ":" << "0" << s;
			}
			else {
				ss << ":" << s;
			}
			gametimer.setString(ss.str());


			//lives
			ss.str(" ");
			ss << "x" << player1.health;
			lives.setString(ss.str());

			if (pauseOn) {
				BgScreen.setFillColor(Color(0, 101, 127));
				timepaused = pausetimer.getElapsedTime();
				f = 1;
			}

			if (windowIndex == 1 || windowIndex == 21) {
				block.setTexture(blockt);
				frame.setFillColor(Color(0, 102, 10));

			}
			else if (windowIndex == 2 || windowIndex == 22) {
				block.setTexture(blockt2);
				frame.setFillColor(Color(70, 26, 0));
			}

			for (int i = 0; i < blocksnumY; i++)
			{
				for (int j = 0; j < blocksnumX; j++) {
					if (Map[i][j] == 1) {
						blocks[i][j] = block;
						blocks[i][j].setPosition(block.getPosition().x + block.getGlobalBounds().width * i, block.getPosition().y + block.getGlobalBounds().height * j);
					}
				}
			}


			if (windowIndex < 20) {

				window.clear(Color(0, 101, 127)); //background color of the window

				//score
				ss.str(" ");
				ss << player1.score;
				score.setString(ss.str());

				//GameOver
				if (m >= 10 || player1.health <= 0) {
					BgScreen.setFillColor(Color(255, 170, 0));
					lose = true;
				}
				//Victory 
				if (enemy_num <= 0 || player1.score >= 7500) {
					win = true;
					BgScreen.setFillColor(Color(4, 92, 236));
					if (windowIndex == 1) {
						level1Completed = true;
						pass.play();
					}
					else if (windowIndex == 2) {
						level2Completed = true;
						pass.play();
					}
				}
			}

			if (windowIndex > 20) {
				window.clear(Color(208, 46, 54));
				ss.str(" ");
				ss << "x" << player2.health;
				lives2.setString(ss.str());
				if (player1.health <= 0 || player2.health <= 0 || m >= 10) {
					lose = true;
					//gameover.play();
					BgScreen.setFillColor(Color(208, 46, 54));
					ss.str(" ");
					if (player1.health > player2.health) {
						ss << "Player 1 Wins!!";
						Winner.setString(ss.str());
					}
					else if (player2.health > player1.health) {
						ss << "Player 2 Wins!!";
						Winner.setString(ss.str());
					}
				}
			}

		}

		//detroy block(frost : DONT EDIT!!!!!!!!!)
		for (int k = 0; k < 3; k++)
		{
			for (int i = 0; i < blocksnumY; i++)
			{
				for (int j = 0; j < blocksnumX; j++)
				{
					//(fire[i].getGlobalBounds().intersects(bmanSprite.getGlobalBounds()) || fire1[i].getGlobalBounds().intersects(bmanSprite.getGlobalBounds())) && (bmanld == false)
					if ((blocks[i][j].getGlobalBounds().intersects(fire[k].getGlobalBounds()) || blocks[i][j].getGlobalBounds().intersects(fire1[k].getGlobalBounds())) && (Map[i][j] == 2)) {
						cout << "BOOM!!";
						Sprite nul;
						blocks[i][j] = nul;
					}
				}
			}


		}

		//draw your game
		if (windowIndex > 0) {
			window.draw(frame);
			window.draw(gameHeader);
			window.draw(TimerBox);
			window.draw(gametimer);
			window.draw(bmanIcon);
			window.draw(lives);
			//map drawing
			for (int i = 0; i < blocksnumY; i++)
				for (int j = 0; j < blocksnumX; j++)
					window.draw(blocks[i][j]);
			//bomb
			for (int i = 0; i < 3; ++i) {
				window.draw(bomb[i]);
			}
			window.draw(bmanSprite);
			for (int i = 0; i < 3; i++) {
				window.draw(fire[i]);
			}
			for (int i = 0; i < 3; i++) {
				window.draw(fire1[i]);
			}
			if (windowIndex > 20) {
				window.draw(bmanSprite2);
				window.draw(bmanIcon2);
				window.draw(lives2);
				if (lose) {
					window.draw(BgScreen);
					window.draw(Winner);
					window.draw(instructions);
					//gameover.play();
				}
			}

			if (windowIndex < 20) {
				window.draw(ScoreBox);
				window.draw(score);
				for (int i = 0; i < 3; ++i) {
					window.draw(enemy[i]);
				}
				for (int i = 0; i < 3; ++i) {
					window.draw(enemy2[i]);
				}
				if (lose) {
					window.draw(BgScreen);
					window.draw(GameOver);
					window.draw(instructions);
					//gameover.play();
				}
				if (win) {
					window.draw(BgScreen);
					window.draw(Victory);
					window.draw(instructions);
				}
			}

			if (pauseOn) {
				window.draw(BgScreen);
				window.draw(Pause);
				window.draw(Continue);
				window.draw(Quit);
			}
			//window.draw(block);
		}
		if (windowIndex == 0) {
			window.draw(titleScreen);
			window.draw(play);
			window.draw(exit);
		}
		if (windowIndex == -1) {
			window.draw(titleScreen);
			window.draw(Campaign);
			window.draw(Vsmode);
		}
		if (windowIndex == -2) {
			window.draw(titleScreen);
			window.draw(level1);
			window.draw(level2);
		}

		if (windowIndex == -3) {
			window.draw(titleScreen);
			window.draw(MapOne);
			window.draw(MapTwo);
		}

		window.display(); //tell window you are done drawing
	}

	return 0;
}

//0: represent empty space character can move freely on
//1: represent indestructible blocks 
void map1() {
	for (int i = 1; i < 29; i++) {
		Map[i][1] = 1;
		Map[i][15] = 1;
	}
	for (int i = 1; i < 16; i++) {
		Map[1][i] = 1;
		Map[29][i] = 1;
	}
	for (int i = 1; i < 29; i += 2) {
		for (int j = 1; j < 16; j += 2) {
			Map[i][j] = 1;
		}
	}

	for (int i = 2; i < 29; i += 2) {
		if (i == 6)continue;
		for (int j = 2; j < 16; j += 2) {
			if (j == 6)continue;
			if (rand() % 2 && Map[i][j] != 1) {
				Map[i][j] = 2;
			}

		}
	}
	for (int i = 0; i < 50; i++) {
		for (int j = 0; j < 50; j++) {
			//cout << Map[i][j] << " ";
		}
		// cout << endl;
	}
}




void right(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator) {
	bmanSprite.move(stepSize, 0);
	FrameIndicator = bmanHeight * 3;
}
void left(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator) {
	bmanSprite.move(-stepSize, 0);
	FrameIndicator = bmanHeight * 2;
}
void backward(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator) {
	bmanSprite.move(0, -stepSize);
	FrameIndicator = bmanHeight * 1;
}
void forward(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator) {
	bmanSprite.move(0, stepSize);
	FrameIndicator = bmanHeight * 0;
}
void animation(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator, int& animationIndicator) {
	animationIndicator++;
	animationIndicator %= 4;
	bmanSprite.setTextureRect(IntRect(bmanWidth * animationIndicator, FrameIndicator, bmanWidth, bmanHeight));
	FrameIndicator = 0;
}
void bombfun(int& t1, int& t2, int& t3, int& index, Sprite bomb[3], bool isbom[3], Sprite& bmanSprite2) {
	if ((t1 == 0 || t1 > 30) && (t2 == 0 || t2 > 30) && (t3 == 0 || t3 > 30)) {
		for (int i = 0; i < 3; i++) {
			if (i == index) {
				bomb[index].setPosition(bmanSprite2.getPosition().x, bmanSprite2.getPosition().y);
				isbom[index] = true;
			}
		}
		index++;
		index = index % 3;
	}
}
void Enemyanimation(Texture& enemyd, Sprite& enemy, int& EnemyFrameIndicator, int& EnemyAnimationIndicator)
{

	if (enemySprite_clock.getElapsedTime().asSeconds() > 0.003)
		
			enemy.setTexture(enemyd);
			EnemyAnimationIndicator++;
			EnemyAnimationIndicator = EnemyAnimationIndicator % 4;
			enemy.setTextureRect(IntRect(EnemyAnimationIndicator * enemyWidth - 3, 0, enemyWidth - 3, enemyHeight));
			enemySprite_clock.restart();
		

}
void EnemyMotion_right_left1(Texture& enemyTexture, Sprite enemySprite[3], int& EnemyFrameIndicator, int& EnemyAnimationIndicator)
{

	if (enemySprite_clock.getElapsedTime().asSeconds() > 0.003)
		for (int i = 0; i < 3; ++i) {
			enemySprite[i].move(enemyStepSizex[i], 0);
			EnemyAnimationIndicator++;
			EnemyAnimationIndicator = EnemyAnimationIndicator % 9;
			enemySprite[i].setTextureRect(IntRect(EnemyAnimationIndicator * enemyWidth - 3, 0, enemyWidth - 3, enemyHeight));
			enemySprite_clock.restart();
		}

}
void EnemyMotion_up_down(Texture& enemyTexture, Sprite enemySprite[3], int& EnemyFrameIndicator, int& EnemyAnimationIndicator) {
	if (enemySprite_clock.getElapsedTime().asSeconds() > 0.003)
	{
		for (int i = 0; i < 3; i++) {
			enemySprite[i].move(0, enemyStepSizey2[i]);
			EnemyAnimationIndicator++;
			EnemyAnimationIndicator = EnemyAnimationIndicator % 9;
			enemySprite[i].setTextureRect(IntRect(EnemyAnimationIndicator * enemyWidth - 3, 0, enemyWidth - 3, enemyHeight));
			enemySprite_clock.restart();
		}
	}
}


void collison(Sprite& S, Sprite& b)
{
	if (S.getGlobalBounds().intersects(b.getGlobalBounds())) {
		// Right
		if (S.getGlobalBounds().left + S.getGlobalBounds().width >= b.getGlobalBounds().left &&
			!(S.getGlobalBounds().left + S.getGlobalBounds().width >= b.getGlobalBounds().left + 2 * stepSize)) {
			cright = false;
			S.move(-2, 0);
		}
		//Left
		if (S.getGlobalBounds().left <= b.getGlobalBounds().left + b.getGlobalBounds().width &&
			!(S.getGlobalBounds().left <= b.getGlobalBounds().left + b.getGlobalBounds().width - 2 * stepSize - 10)) {
			cleft = false;
			S.move(2, 0);
		}
		// Up
		if (S.getGlobalBounds().top <= b.getGlobalBounds().top + b.getGlobalBounds().height &&
			!(S.getGlobalBounds().top <= b.getGlobalBounds().top + b.getGlobalBounds().height - 2 * stepSize)) {
			cup = false;
			S.move(0, 2);
		}
		//Down
		if (S.getGlobalBounds().top + S.getGlobalBounds().height >= b.getGlobalBounds().top &&
			!(S.getGlobalBounds().top + S.getGlobalBounds().height >= b.getGlobalBounds().top + 2 * stepSize)) {
			cdown = false;
			S.move(0, -2);
		}
	}
}

void bmanlose(Texture& bmanTexture, Sprite& bmanSprite, int& FrameIndicator, int& animationIndicator, bool& bmanld, Texture& bmanl) {
	if (bmanld == true) {
		bmanSprite.setTexture(bmanl);
		if (respawn.getElapsedTime().asSeconds() > 0.4) {
			respawn.restart();
			animationIndicator++;
			bmanSprite.setTextureRect(IntRect(animationIndicator * bmanWidth, 0, bmanWidth, bmanHeight));
		}
		if (animationIndicator >= 6) {
			bmanld = false;
			left(bmanTexture, bmanSprite, FrameIndicator);
			animation(bmanTexture, bmanSprite, FrameIndicator, animationIndicator);
			bmanSprite.setPosition(140, 860);
			ismoving = true;
		}
	}
	else {
		bmanSprite.setTexture(bmanTexture);
	}
}

void bmanlose2(Texture& bmanTexture2, Sprite& bmanSprite, int& FrameIndicator, int& animationIndicator, bool& bmanld, Texture& bmanl) {
	if (bmanld == true) {
		bmanSprite.setTexture(bmanl);
		if (respawn.getElapsedTime().asSeconds() > 0.4) {
			respawn.restart();
			animationIndicator++;
			bmanSprite.setTextureRect(IntRect(animationIndicator * bmanWidth, 0, bmanWidth, bmanHeight));
		}
		if (animationIndicator >= 6) {
			bmanld = false;
			left(bmanTexture2, bmanSprite, FrameIndicator);
			animation(bmanTexture2, bmanSprite, FrameIndicator, animationIndicator);
			bmanSprite.setPosition(1630, 900);
			ismoving = true;
		}
	}
	else {
		bmanSprite.setTexture(bmanTexture2);
	}
}

void Ecollisonrl(Sprite S[3], Sprite& b) {
	int c1 = 0, c2 = 1, c3 = 2;
	for (int i = 0; i < 3; i++) {
		if (S[i].getGlobalBounds().intersects(b.getGlobalBounds())) {
			if (i == c1) {
				enemyStepSizex[c1] *= -1;
				enemyStepSizey[c1] *= -1;

			}
			if (i == c2) {
				enemyStepSizex[c2] *= -1;
				enemyStepSizey[c2] *= -1;
			}
			if (i == c3) {
				enemyStepSizex[c3] *= -1;
				enemyStepSizey[c3] *= -1;
			}

		}
	}
}
void Ecollisonud(Sprite S[3], Sprite& b) {

	int c1 = 0, c2 = 1, c3 = 2;
	for (int i = 0; i < 3; i++) {
		if (S[i].getGlobalBounds().intersects(b.getGlobalBounds())) {

			if (i == c1) {
				enemyStepSizex2[c1] *= -1;
				enemyStepSizey2[c1] *= -1;

			}
			if (i == c2) {
				enemyStepSizex2[c2] *= -1;
				enemyStepSizey2[c2] *= -1;
			}
			if (i == c3) {
				enemyStepSizex2[c3] *= -1;
				enemyStepSizey2[c3] *= -1;
			}

		}
	}
}
