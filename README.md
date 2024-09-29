##- 👋 Hi, I’m @Glorian811
- 👀 I’m  in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...##

<!---
Glorian811/Glorian811 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
PLAYER_RADIUS = 30
FONT_SIZE = 32
BG_COLOR = (50, 50, 50)
TEXT_COLOR = (255, 255, 255)
PLAYER_COLORS = [(255, 0, 0), (0, 255, 0), (0, 0, 255), (255, 255, 0), (255, 165, 0), (138, 43, 226)]

# Setup screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Assassin Game')

# Font for displaying text
font = pygame.font.Font(None, FONT_SIZE)

class Player:
    def __init__(self, name, pos, color):
        self.name = name
        self.pos = pos
        self.color = color
        self.is_alive = True
        self.target = None

    def draw(self):
        if self.is_alive:
            pygame.draw.circle(screen, self.color, self.pos, PLAYER_RADIUS)
            label = font.render(self.name, True, TEXT_COLOR)
            screen.blit(label, (self.pos[0] - PLAYER_RADIUS, self.pos[1] - PLAYER_RADIUS - FONT_SIZE))

    def is_clicked(self, pos):
        return self.is_alive and ((pos[0] - self.pos[0]) ** 2 + (pos[1] - self.pos[1]) ** 2 <= PLAYER_RADIUS ** 2)

class AssassinGame:
    def __init__(self, player_names):
        self.players = []
        self.alive_players = []
        self.create_players(player_names)
        self.assign_targets()

    def create_players(self, player_names):
        positions = [(random.randint(PLAYER_RADIUS, SCREEN_WIDTH - PLAYER_RADIUS),
                      random.randint(PLAYER_RADIUS, SCREEN_HEIGHT - PLAYER_RADIUS)) for _ in player_names]
        for i, name in enumerate(player_names):
            player = Player(name, positions[i], PLAYER_COLORS[i % len(PLAYER_COLORS)])
            self.players.append(player)
            self.alive_players.append(player)

    def assign_targets(self):
        random.shuffle(self.alive_players)
        for i in range(len(self.alive_players)):
            self.alive_players[i].target = self.alive_players[(i + 1) % len(self.alive_players)]

    def eliminate(self, player):
        if player.target:
            print(f"{player.name} eliminates {player.target.name}")
            player.target.is_alive = False
            self.alive_players.remove(player.target)
            if len(self.alive_players) > 1:
                player.target = self.alive_players[(self.alive_players.index(player) + 1) % len(self.alive_players)]
            else:
                player.target = None

    def check_for_winner(self):
        if len(self.alive_players) == 1:
            return self.alive_players[0].name
        return None

    def draw(self):
        for player in self.players:
            player.draw()

def main():
    player_names = ['Alice', 'Bob', 'Charlie', 'Daisy', 'Eve', 'Frank']
    game = AssassinGame(player_names)

    # Main loop
    clock = pygame.time.Clock()
    running = True
    winner = None

    while running:
        screen.fill(BG_COLOR)
        
        # Handle events
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:
                pos = pygame.mouse.get_pos()
                for player in game.alive_players:
                    if player.is_clicked(pos):
                        game.eliminate(player)

        # Check for a winner
        winner = game.check_for_winner()
        if winner:
            print(f"{winner} wins!")
            running = False

        # Draw players
        game.draw()

        # Update display
        pygame.display.flip()

        # Cap the frame rate
        clock.tick(60)

    pygame.quit()
    sys.exit()

if __name__ == "__main__":
    main()import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
PLAYER_RADIUS = 30
FONT_SIZE = 32
BG_COLOR = (50, 50, 50)
TEXT_COLOR = (255, 255, 255)
PLAYER_COLORS = [(255, 0, 0), (0, 255, 0), (0, 0, 255), (255, 255, 0), (255, 165, 0), (138, 43, 226)]

# Setup screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Assassin Game')

# Font for displaying text
font = pygame.font.Font(None, FONT_SIZE)

class Player:
    def __init__(self, name, pos, color):
        self.name = name
        self.pos = pos
        self.color = color
        self.is_alive = True
        self.target = None

    def draw(self):
        if self.is_alive:
            pygame.draw.circle(screen, self.color, self.pos, PLAYER_RADIUS)
            label = font.render(self.name, True, TEXT_COLOR)
            screen.blit(label, (self.pos[0] - PLAYER_RADIUS, self.pos[1] - PLAYER_RADIUS - FONT_SIZE))

    def is_clicked(self, pos):
        return self.is_alive and ((pos[0] - self.pos[0]) ** 2 + (pos[1] - self.pos[1]) ** 2 <= PLAYER_RADIUS ** 2)

class AssassinGame:
    def __init__(self, player_names):
        self.players = []
        self.alive_players = []
        self.create_players(player_names)
        self.assign_targets()

    def create_players(self, player_names):
        positions = [(random.randint(PLAYER_RADIUS, SCREEN_WIDTH - PLAYER_RADIUS),
                      random.randint(PLAYER_RADIUS, SCREEN_HEIGHT - PLAYER_RADIUS)) for _ in player_names]
        for i, name in enumerate(player_names):
            player = Player(name, positions[i], PLAYER_COLORS[i % len(PLAYER_COLORS)])
            self.players.append(player)
            self.alive_players.append(player)

    def assign_targets(self):
        random.shuffle(self.alive_players)
        for i in range(len(self.alive_players)):
            self.alive_players[i].target = self.alive_players[(i + 1) % len(self.alive_players)]

    def eliminate(self, player):
        if player.target:
            print(f"{player.name} eliminates {player.target.name}")
            player.target.is_alive = False
            self.alive_players.remove(player.target)
            if len(self.alive_players) > 1:
                player.target = self.alive_players[(self.alive_players.index(player) + 1) % len(self.alive_players)]
            else:
                player.target = None

    def check_for_winner(self):
        if len(self.alive_players) == 1:
            return self.alive_players[0].name
        return None

    def draw(self):
        for player in self.players:
            player.draw()

def main():
    player_names = ['Alice', 'Bob', 'Charlie', 'Daisy', 'Eve', 'Frank']
    game = AssassinGame(player_names)

    # Main loop
    clock = pygame.time.Clock()
    running = True
    winner = None

    while running:
        screen.fill(BG_COLOR)
        
        # Handle events
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:
                pos = pygame.mouse.get_pos()
                for player in game.alive_players:
                    if player.is_clicked(pos):
                        game.eliminate(player)

        # Check for a winner
        winner = game.check_for_winner()
        if winner:
            print(f"{winner} wins!")
            running = False

        # Draw players
        game.draw()

        # Update display
        pygame.display.flip()

        # Cap the frame rate
        clock.tick(60)

    pygame.quit()
    sys.exit()

if __name__ == "__main__":
    main()
