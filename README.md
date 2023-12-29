import pygame
import sys

pygame.init()

WIDTH, HEIGHT = 800, 600
win = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("MT Game Çükür ᑅ ᐧ ᑀ")

WHITE = (255, 255, 255)
ROAD_COLOR = (100, 100, 100)
BRIGHT_SKY_COLOR = (135, 206, 250)

title_font = pygame.font.Font(None, 64)
start_font = pygame.font.Font(None, 36)
copyright_font = pygame.font.Font(None, 18)

title_text = title_font.render("MT Game Çükür", True, (255, 0, 0))
title_rect = title_text.get_rect(center=(WIDTH // 2, HEIGHT // 4))

start_text = start_font.render("Press any key to start the game", True, (255, 0, 0))
start_rect = start_text.get_rect(center=(WIDTH // 2, HEIGHT * 3 // 4))

copyright_text = copyright_font.render("© 2023 - MT Games", True, (255, 0, 0))
copyright_rect = copyright_text.get_rect(topleft=(10, HEIGHT - 30))

# Player variables
player_x = WIDTH // 2
player_y = HEIGHT // 2
player_speed = 5
player_velocity = 0

# Light variables
light_on = False

# Game variables
game_started = False
game_over = False

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

        if event.type == pygame.KEYDOWN:
            if not game_started:
                game_started = True
            else:
                if event.key == pygame.K_SPACE:
                    light_on = not light_on  # Toggle the light status
                    print(f"Light {'On' if light_on else 'Off'}")

                print("The game is starting!")

    if game_started and not game_over:
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            player_velocity -= player_speed
        if keys[pygame.K_RIGHT]:
            player_velocity += player_speed

        player_x += player_velocity
        player_velocity *= 0.9  # Add some friction to slow down the player

        # Boundary checks to keep the player within the window
        player_x = max(0, min(player_x, WIDTH))

        # Game over condition
        if player_velocity > 15:
            print("Game Over! Car crashed at high speed.")
            game_over = True

    win.fill(BRIGHT_SKY_COLOR)
    pygame.draw.rect(win, ROAD_COLOR, (0, HEIGHT // 2, WIDTH, HEIGHT // 2))

    win.blit(title_text, title_rect)
    win.blit(copyright_text, copyright_rect)

    if not game_started:
        win.blit(start_text, start_rect)

    pygame.draw.rect(win, WHITE, (player_x - 20, player_y - 20, 40, 40))  # Draw player (for demonstration purposes)

    # Draw light
    if light_on:
        pygame.draw.circle(win, (255, 255, 0), (WIDTH - 30, 30), 15)

    pygame.display.flip()
    pygame.time.Clock().tick(60)
