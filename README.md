# simple-linear-regression
Takes inputted points and returns a graph of the line of best fit and the equation
import pygame
pygame.init()
print("Make sure to seperate x and y values with only a space")
running = True
coordinates = []
done = False
point_count = 0
while not done:
    point = input(f"Point {point_count+1}: ")
    point = tuple(map(int, point.split(" ")))
    coordinates.append(point)
    choice_made = False
    while not choice_made:
        choice = input("Would you like to add another point?\n")
        if choice.lower() == "yes":
            point_count += 1
            choice_made = True
        elif choice.lower() == "no":
            choice_made = True
            done = True
        else:
            print("Sorry, I didn't understand that")
print(coordinates)
sum_x = 0
sum_y = 0
sum_x_2 = 0
sum_y_2 = 0
sum_x_y = 0
for coordinate in coordinates:
    sum_x += coordinate[0]
    sum_y += coordinate[1]
    sum_x_2 += coordinate[0] ** 2
    sum_y_2 += coordinate[1] ** 2
    sum_x_y += coordinate[0] * coordinate[1]
number = len(coordinates)
slope_numer = (number * sum_x_y)-(sum_x * sum_y)
slope_denom = (number * sum_x_2)-(sum_x ** 2)
slope = slope_numer / slope_denom
intercept_numer = (sum_y * sum_x_2)-(sum_x*sum_x_y)
intercept_denom = (number * sum_x_2)-(sum_x ** 2)
intercept = intercept_numer / intercept_denom
final_equation = f"f(x) = {slope}x + {intercept}"
print(final_equation)
input("Press enter to open graph")



# window
win_x = 500
win_y = 500
win = pygame.display.set_mode((win_x, win_y))
pygame.display.set_caption("Line of Best Fit")


def draw_win():
    increment = 30
    win.fill((255, 255, 255))
    for num in range(1, 15):
        draw_text(str(num), 15, (50+num * increment, 465), (0, 0, 0), "Font/Walkway_Oblique.ttf")
    for num in range(1, 15):
        draw_text(str(num), 15, (35, 450-num*increment), (0, 0, 0), "Font/Walkway_Oblique.ttf")
    pygame.draw.line(win, (0, 0, 0), (50, 0), (50, 500), 1)
    pygame.draw.line(win, (0, 0, 0), (0, 450), (500, 450), 1)
    for num in range(1, 15):
        pygame.draw.line(win, (128, 128, 128), (50+num * increment, 0), (50+num * increment, 450), 1)
    for num in range(1, 15):
        pygame.draw.line(win, (128, 128, 128), (50, num * increment), (500, num * increment), 1)
    for point in coordinates:
        pygame.draw.rect(win, (0, 0, 0), (50+point[0]*increment-3, 450-point[1]*increment-3, 6, 6))
    pygame.draw.line(win, (255, 0, 0), (50, 450-intercept*increment), (500, 450-(slope*15+intercept)*increment))
    pygame.display.update()

def text_objects(text, font, color):
    text_surface = font.render(text, True, color)
    return text_surface, text_surface.get_rect()


def draw_text(text, size, place, color, font):
    font = pygame.font.Font(font, size)
    text_surf, text_rect = text_objects(text, font, color)
    text_rect.center = place
    win.blit(text_surf, text_rect)


while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
    draw_win()

