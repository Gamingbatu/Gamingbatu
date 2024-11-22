- 👋 Hi, I’m @Gamingbatu
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Gamingbatu/Gamingbatu is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random

# Initialisiere Pygame
pygame.init()

# Bildschirmgröße und Farben
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)
GREEN = (0, 255, 0)

screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('2D Game mit Pygame')

# Uhr für Framerate
clock = pygame.time.Clock()

# Spielfigur
player_width = 50
player_height = 50
player_x = SCREEN_WIDTH // 2 - player_width // 2
player_y = SCREEN_HEIGHT - player_height - 10
player_speed = 5

# Gegner
enemy_width = 50
enemy_height = 50
enemy_speed = 3
enemies = []

# Schriftarten
font = pygame.font.SysFont("arial", 36)

# Funktionen
def draw_text(text, font, color, surface, x, y):
    textobj = font.render(text, 1, color)
    textrect = textobj.get_rect()
    textrect.center = (x, y)
    surface.blit(textobj, textrect)

def spawn_enemy():
    x = random.randint(0, SCREEN_WIDTH - enemy_width)
    y = random.randint(-150, -50)
    enemies.append(pygame.Rect(x, y, enemy_width, enemy_height))

def draw_player():
    pygame.draw.rect(screen, BLUE, pygame.Rect(player_x, player_y, player_width, player_height))

def draw_enemies():
    for enemy in enemies:
        pygame.draw.rect(screen, RED, enemy)

def move_enemies():
    global enemies
    for enemy in enemies:
        enemy.y += enemy_speed
        if enemy.y > SCREEN_HEIGHT:
            enemies.remove(enemy)

def check_collisions():
    player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
    for enemy in enemies:
        if player_rect.colliderect(enemy):
            return True
    return False

def game_loop():
    global player_x, player_y, enemies
    enemies = []
    run_game = True
    while run_game:
        screen.fill(WHITE)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                run_game = False

        # Tasteneingaben
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT] and player_x > 0:
            player_x -= player_speed
        if keys[pygame.K_RIGHT] and player_x < SCREEN_WIDTH - player_width:
            player_x += player_speed
        if keys[pygame.K_UP] and player_y > 0:
            player_y -= player_speed
        if keys[pygame.K_DOWN] and player_y < SCREEN_HEIGHT - player_height:
            player_y += player_speed

        # Gegner spawnen
        if random.randint(1, 100) <= 5:  # 5% Chance, einen neuen Gegner zu spawnen
            spawn_enemy()

        move_enemies()
        draw_player()
        draw_enemies()

        if check_collisions():
            draw_text("Game Over!", font, BLACK, screen, SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2)
            pygame.display.update()
            pygame.time.wait(2000)
            return  # Beendet das Spiel

        pygame.display.update()
        clock.tick(60)  # Framerate auf 60 FPS begrenzen

def home_menu():
    run_menu = True
    while run_menu:
        screen.fill(WHITE)
        draw_text("Willkommen zum Spiel!", font, BLACK, screen, SCREEN_WIDTH // 2, SCREEN_HEIGHT // 4)
        draw_text("Drücke 'ENTER' zum Starten", font, GREEN, screen, SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2)
        draw_text("Drücke 'ESC' zum Beenden", font, RED, screen, SCREEN_WIDTH // 2, SCREEN_HEIGHT // 1.5)

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                run_menu = False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RETURN:  # Spiel starten
                    game_loop()
                if event.key == pygame.K_ESCAPE:  # Beenden
                    run_menu = False

        pygame.display.update()
        clock.tick(60)

# Starte das Home-Menü
home_menu()

# Beende Pygame
pygame.quit()
