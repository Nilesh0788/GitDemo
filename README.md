# GitDemo
this is my first git repository.
<br>
author - Nilesh singh
import pygame
import random
import time

# Initialize Pygame
pygame.init()

# Constants
GRID_SIZE = 30
CELL_SIZE = 20
WIDTH, HEIGHT = GRID_SIZE * CELL_SIZE, GRID_SIZE * CELL_SIZE
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
BLACK = (0, 0, 0)
BLUE = (0, 0, 255)
TIMER_LIMIT = 300  # 5 minutes timer

# Screen setup
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Pirate Treasure Hunt Game")

# Font setup
font = pygame.font.SysFont('Arial', 24)

# Generate random treasure locations
def generate_treasures(num_treasures):
    treasures = []
    while len(treasures) < num_treasures:
        x = random.randint(0, GRID_SIZE - 1)
        y = random.randint(0, GRID_SIZE - 1)
        if (x, y) not in treasures:
            treasures.append((x, y))
    return treasures

# Draw the grid
def draw_grid():
    for x in range(0, WIDTH, CELL_SIZE):
        for y in range(0, HEIGHT, CELL_SIZE):
            rect = pygame.Rect(x, y, CELL_SIZE, CELL_SIZE)
            pygame.draw.rect(screen, WHITE, rect, 1)

# Draw treasures on the grid
def draw_treasures(treasures, found_treasures):
    for treasure in treasures:
        if treasure not in found_treasures:
            x, y = treasure
            pygame.draw.circle(screen, BLUE, (x * CELL_SIZE + CELL_SIZE // 2, y * CELL_SIZE + CELL_SIZE // 2), CELL_SIZE // 3)

# Main game loop
def game_loop():
    treasures = generate_treasures(5)  # 5 treasures to find
    found_treasures = []
    clock = pygame.time.Clock()
    start_time = time.time()

    # Game loop
    while True:
        screen.fill(BLACK)
        draw_grid()
        draw_treasures(treasures, found_treasures)

        # Timer countdown
        elapsed_time = int(time.time() - start_time)
        time_left = max(0, TIMER_LIMIT - elapsed_time)
        timer_text = font.render(f"Time Left: {time_left // 60}:{time_left % 60:02d}", True, WHITE)
        screen.blit(timer_text, (10, 10))

        # Handle events
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                exit()

            # Mouse click to find treasure
            if event.type == pygame.MOUSEBUTTONDOWN:
                x, y = pygame.mouse.get_pos()
                grid_x, grid_y = x // CELL_SIZE, y // CELL_SIZE
                if (grid_x, grid_y) in treasures and (grid_x, grid_y) not in found_treasures:
                    found_treasures.append((grid_x, grid_y))
                    print(f"Found a treasure at ({grid_x}, {grid_y})!")
            
            # Command input
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_q:
                    pygame.quit()
                    exit()

        # Check if all treasures found
        if len(found_treasures) == len(treasures):
            win_text = font.render("You found all treasures! You win!", True, GREEN)
            screen.blit(win_text, (WIDTH // 4, HEIGHT // 2))
            pygame.display.update()
            pygame.time.wait(2000)
            break

        # Update the screen
        pygame.display.update()
        clock.tick(60)

if __name__ == "__main__":
    game_loop()

