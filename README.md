# gamenglishbasic1
gamenglishbasic1
import pygame
import random

# Inicialização do Pygame
pygame.init()

# Definindo as cores
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Definindo as dimensões da janela do jogo
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Criando a classe do jogador
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([50, 50])
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = 50
        self.rect.y = SCREEN_HEIGHT // 2

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_UP]:
            self.rect.y -= 5
        if keys[pygame.K_DOWN]:
            self.rect.y += 5

# Criando a classe dos obstáculos
class Obstacle(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([30, 40])
        self.image.fill(BLACK)
        self.rect = self.image.get_rect()
        self.rect.x = SCREEN_WIDTH
        self.rect.y = random.randint(10, SCREEN_HEIGHT - 50)
        self.speed = random.randint(5, 10)

    def update(self):
        self.rect.x -= self.speed

# Inicializando a janela do jogo
screen = pygame.display.set_mode([SCREEN_WIDTH, SCREEN_HEIGHT])
pygame.display.set_caption("English Learning Game")

# Criando o grupo de sprites
all_sprites = pygame.sprite.Group()

# Adicionando o jogador ao grupo de sprites
player = Player()
all_sprites.add(player)

# Adicionando obstáculos ao grupo de sprites
obstacles = pygame.sprite.Group()
for _ in range(10):
    obstacle = Obstacle()
    obstacles.add(obstacle)
    all_sprites.add(obstacle)

# Definindo a fonte para exibir o número da fase
font = pygame.font.Font(None, 36)

# Definindo variáveis de controle do jogo
clock = pygame.time.Clock()
game_over = False
current_level = 1

# Loop principal do jogo
while not game_over:
    # Verificando eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    # Atualizando os sprites
    all_sprites.update()

    # Verificando colisões entre o jogador e os obstáculos
    if pygame.sprite.spritecollide(player, obstacles, True):
        game_over = True

    # Verificando se todos os obstáculos foram removidos
    if len(obstacles) == 0:
        current_level += 1
        if current_level > 10:
            game_over = True
        else:
            for _ in range(10):
                obstacle = Obstacle()
                obstacles.add(obstacle)
                all_sprites.add(obstacle)

    # Preenchendo o fundo da tela
    screen.fill(WHITE)

    # Desenhando os sprites na tela
    all_sprites.draw(screen)

    # Exibindo o número da fase atual
