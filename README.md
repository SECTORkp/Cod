Game 
код для игры

 #include <SFML/Graphics.hpp>
#include <time.h>
#include <SFML/Audio.hpp>

using namespace sf;

class setSound {
public:
    SoundBuffer w, t, p, ta;
    Sound win, time, put, take;
    bool stopwin;

    setSound() {
        p.loadFromFile("C:/Users/a/Desktop/Game/Sound/put.ogg");
        ta.loadFromFile("C:/Users/a/Desktop/Game/Sound/take.ogg");
        w.loadFromFile("C:/Users/a/Desktop/Game/Sound/win.ogg");
        t.loadFromFile("C:/Users/a/Desktop/Game/Sound/time.ogg");

        win.setBuffer(w);
        time.setBuffer(t);
        put.setBuffer(p);
        take.setBuffer(ta);

        time.setLoop(true);

        stopwin = false;
    }
};

setSound sound;

class Puzzles {
public:
    Texture pu[12];
    Sprite sprite[12];
    bool dvig[12];

    Puzzles() {
        pu[0].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/1.1.png");
        pu[1].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/1.2.png");
        pu[2].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/1.3.png");
        pu[3].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/2.1.png");
        pu[4].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/2.2.png");
        pu[5].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/2.3.png");
        pu[6].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/3.1.png");
        pu[7].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/3.2.png");
        pu[8].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/3.3.png");
        pu[9].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/4.1.png");
        pu[10].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/4.2.png");
        pu[11].loadFromFile("C:/Users/a/Desktop/Game/Paint/puzzles/4.3.png");

        for (int i = 0; i < 12; i++) {
            sprite[i].setTexture(pu[i]);

            dvig[i] = false;
        }

        for (int j = 0; j < 4; j++)
            for (int i = 0; i < 3; i++)
                sprite[j * 3 + i].setPosition(600 + 175 * i, 125 * j);
    }
};

Puzzles puzzles;

int k = 0;

class RandTabels {
public:
    int a[12];

    RandTabels() {
        for (int i = 0; i < 12; i++)
            a[i] = 0;

        for (int i = 0; i < 12; i++)
        {
            int tmp = rand() % 12;
            while (a[tmp] != 0)
                tmp = rand() % 12;
            a[tmp] = i + 1;
        }

        while (k < 12) {
            int x = k;
            int y = 0;

            while (x >= 3) {
                x -= 3;
                y++;
            }

            puzzles.sprite[a[k] - 1].setPosition(600 + 150 * x, 125 * y + 75);

            k++;
        }
        k = 0;
    }
};

class Stopwatch {
public:
    Sprite min[2], sec[2], toc;
    int m[2], s[2], ms;

    Stopwatch(Texture& image) {
        for (int i = 0; i < 2; i++) {
            min[i].setTexture(image);
            sec[i].setTexture(image);

            min[i].setPosition(1010 + 30 * i, 25);
            sec[i].setPosition(1080 + 30 * i, 25);

            min[i].setTextureRect(IntRect(0, 0, 24, 38));
            sec[i].setTextureRect(IntRect(0, 0, 24, 38));

            m[i] = 0;
            s[i] = 0;
        }

        toc.setTexture(image);
        toc.setPosition(1060, 25);
        toc.setTextureRect(IntRect(240, 0, 24, 38));

        ms = 0;
    }

    void update() {
        ms++;

        if (ms >= 1000) {
            s[1]++;
            ms = 0;
        }

        if (s[1] == 10) {
            s[0]++;
            s[1] = 0;
        }

        if (s[0] == 6) {
            m[1]++;
            s[0] = 0;
        }

        if (m[1] == 10) {
            m[0]++;
            m[1] = 0;
        }

        for (int i = 0; i < 2; i++) {
            sec[i].setTextureRect(IntRect(24 * s[i], 0, 24, 38));
            min[i].setTextureRect(IntRect(24 * m[i], 0, 24, 38));
        }
    }
};

int main()
{
    srand(time(0));
    RenderWindow window(VideoMode(1150, 650), "Puzzles!");

    RandTabels rt;

    Texture pi;
    pi.loadFromFile("C:/Users/a/Desktop/Game/Paint/window.png");
    Sprite picture(pi);
    picture.setPosition(45, 45);

    Texture pi2;
    pi2.loadFromFile("C:/Users/a/Desktop/Game/Paint/picture.png");
    Sprite picture2(pi2);
    picture2.setPosition(50, 50);

    Texture p[12];
    Sprite parts[12];
    bool wap[12];
    bool take = false, win = false, pause = false;

    //Подсказки
    {
        p[0].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/1.1.png");
        p[1].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/1.2.png");
        p[2].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/1.3.png");
        p[3].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/2.1.png");
        p[4].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/2.2.png");
        p[5].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/2.3.png");
        p[6].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/3.1.png");
        p[7].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/3.2.png");
        p[8].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/3.3.png");
        p[9].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/4.1.png");
        p[10].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/4.2.png");
        p[11].loadFromFile("C:/Users/a/Desktop/Game/Paint/parts/4.3.png");

        for (int i = 0; i < 12; i++) {
            parts[i].setTexture(p[i]);
            wap[i] = false;
        }

        parts[0].setPosition(50, 50);
        parts[1].setPosition(154 + 50, 50);
        parts[2].setPosition(318 + 50, 50);
        parts[3].setPosition(50, 98 + 50);
        parts[4].setPosition(139 + 50, 96 + 50);
        parts[5].setPosition(321 + 50, 114 + 50);
        parts[6].setPosition(50, 242 + 50);
        parts[7].setPosition(155 + 50, 226 + 50);
        parts[8].setPosition(320 + 50, 242 + 50);
        parts[9].setPosition(50, 345 + 50);
        parts[10].setPosition(158 + 50, 345 + 50);
        parts[11].setPosition(303 + 50, 345 + 50);
    }

    Texture tm;
    tm.loadFromFile("C:/Users/a/Desktop/Game/Paint/time.png");
    Stopwatch watch(tm);

    Texture pau;
    pau.loadFromFile("C:/Users/a/Desktop/Game/Paint/pause.png");
    Sprite Spause(pau);
    Spause.setPosition(700, 10);

    Texture wn;
    wn.loadFromFile("C:/Users/a/Desktop/Game/Paint/win.png");
    Sprite Swin(wn);
    Swin.setPosition(650, 10);

    sound.time.play();

    while (window.isOpen())
    {
        Vector2i pos = Mouse::getPosition(window);
        
        Vector2f ps[12], pz[12];
        for (int i = 0; i < 12; i++) {
            ps[i] = parts[i].getPosition();
            pz[i] = puzzles.sprite[i].getPosition();
        }

        Event event;
        while (window.pollEvent(event))
        {
            if (event.type == Event::Closed)
                window.close();

            if (!win) {
                if (event.type == Event::KeyPressed)
                    if (event.key.code == Keyboard::Space)
                        if (!pause) {
                            pause = true;

                            sound.time.pause();
                        }
                        else if (pause) {
                            pause = false;

                            sound.time.play();
                        }
                
                if (event.type == Event::MouseButtonPressed)
                    if (event.key.code == Mouse::Left && !pause) {
                        if (!take) {
                            for (int i = 0; i < 12; i++)
                                if (puzzles.sprite[i].getGlobalBounds().contains(pos.x, pos.y)) {
                                    puzzles.dvig[i] = true;
                                    take = true;

                                    for (int j = 0; j < 12; j++)
                                        if (puzzles.dvig[i] == puzzles.dvig[j] && i != j)
                                            puzzles.dvig[j] = false;

                                    sound.take.play();
                                }
                        }
                        else if (take) {
                            for (int i = 0; i < 12; i++)
                                if (puzzles.dvig[i]) {
                                    puzzles.dvig[i] = false;
                                    take = false;

                                    for (int j = 0; j < 12; j++)
                                        if (wap[j]) {
                                            if (i == 0)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 3);
                                            else if (i == 1)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 2);
                                            else if (i == 2)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 3);
                                            else if (i == 3)
                                                puzzles.sprite[i].setPosition(ps[j].x + 2, ps[j].y + 3);
                                            else if (i == 4)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 2);
                                            else if (i == 5)
                                                puzzles.sprite[i].setPosition(ps[j].x + 2, ps[j].y + 2);
                                            else if (i == 6)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 1);
                                            else if (i == 7)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y - 1);
                                            else if (i == 8)
                                                puzzles.sprite[i].setPosition(ps[j].x + 4, ps[j].y + 2);
                                            else if (i == 9)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 3);
                                            else if (i == 10)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 3);
                                            else if (i == 11)
                                                puzzles.sprite[i].setPosition(ps[j].x + 3, ps[j].y + 3);
                                        }
                                }

                            sound.put.play();
                        }
                    }
            }
        }

        if (pz[0].x == 53 && pz[0].y == 53 && pz[1].x == 207 && pz[1].y == 52 && pz[2].x == 371 && pz[2].y == 53 &&
            pz[3].x == 52 && pz[3].y == 151 && pz[4].x == 192 && pz[4].y == 148 && pz[5].x == 373 && pz[5].y == 166 &&
            pz[6].x == 53 && pz[6].y == 293 && pz[7].x == 208 && pz[7].y == 275 && pz[8].x == 374 && pz[8].y == 294 &&
            pz[9].x == 53 && pz[9].y == 398 && pz[10].x == 211 && pz[10].y == 398 && pz[11].x == 356 && pz[11].y == 398) {
            win = true;

            if (!sound.stopwin) {
                sound.win.play();
                sound.time.stop();

                sound.stopwin = true;
            }
        }

        for (int i = 0; i < 12; i++) {
            if (parts[i].getGlobalBounds().contains(pos.x, pos.y))
                wap[i] = true;
            else
                wap[i] = false;

            for (int j = 0; j < 12; j++)
                if (wap[i] == wap[j] && i != j)
                        wap[i] = false;
        }

        for (int i = 0; i < 12; i++)
            if (puzzles.dvig[i] && !pause)
                puzzles.sprite[i].setPosition(pos.x - 100, pos.y - 100);

        if (!win && !pause)
            watch.update();
        window.clear(Color::White);
        window.draw(picture);
        for (int i = 0; i < 12; i++) {
            //if (wap[i])
               //window.draw(parts[i]);

            window.draw(puzzles.sprite[i]);
        }
        for (int i = 0; i < 2; i++) {
            window.draw(watch.min[i]);
            window.draw(watch.sec[i]);
        }
        window.draw(watch.toc);
        if (win)
            window.draw(picture2);
        if (pause)
            window.draw(Spause);
        if (win)
            window.draw(Swin);
        window.display();
    }

    return 0;
}
