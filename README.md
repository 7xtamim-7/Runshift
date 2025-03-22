# Runshiftimport pygame
import random

# Initialize Pygame
pygame.init()

# Screen setup
WIDTH, HEIGHT = 800, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Temple Run 2D")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Player settings
player_width = 50
player_height = 50
player_x = 100
player_y = HEIGHT - player_height - 50
player_velocity = 10

# Obstacle settings
obstacle_width = 50
obstacle_height = 50
obstacle_velocity = 5
obstacle_frequency = 25

# Game variables
game_over = False
score = 0

# Font setup
font = pygame.font.Font(None, 36)

# Player drawing function
def draw_player(x, y):
    pygame.draw.rect(screen, GREEN, (x, y, player_width, player_height))

# Obstacle drawing function
def draw_obstacle(x, y):
    pygame.draw.rect(screen, RED, (x, y, obstacle_width, obstacle_height))

# Score display function
def show_score(score):
    score_text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))

# Game logic
def game_loop():
    global player_x, player_y, score, game_over

    # Random obstacle position
    obstacle_x = WIDTH
    obstacle_y = HEIGHT - obstacle_height - 50

    clock = pygame.time.Clock()

    while not game_over:
        screen.fill(WHITE)
        
        # User input (keyboard)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
        
        keys = pygame.key.get_pressed()

        # Moving left/right
        if keys[pygame.K_LEFT] and player_x > 0:
            player_x -= player_velocity
        if keys[pygame.K_RIGHT] and player_x < WIDTH - player_width:
            player_x += player_velocity

        # Moving the obstacle
        obstacle_x -= obstacle_velocity
        if obstacle_x < 0:
            obstacle_x = WIDTH
            obstacle_y = HEIGHT - obstacle_height - 50
            score += 1
        
        # Check for collision with the obstacle
        player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
        obstacle_rect = pygame.Rect(obstacle_x, obstacle_y, obstacle_width, obstacle_height)
        
        if player_rect.colliderect(obstacle_rect):
            game_over = True

        # Draw player and obstacle
        draw_player(player_x, player_y)
        draw_obstacle(obstacle_x, obstacle_y)

        # Display score
        show_score(score)

        pygame.display.flip()

        # Control frame rate
        clock.tick(30)

    # Game over screen
    pygame.quit()

# Run the game
if __name__ == "__main__":
    game_loop()
