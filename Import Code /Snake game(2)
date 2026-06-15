import tkinter as tk
import random

# 게임 설정
WIDTH = 400
HEIGHT = 400
CELL_SIZE = 20
SPEED = 100  # 밀리초 단위 (작을수록 빠름)

#초기 상태 및 변수 설정
snake=[]
direction="Right"
food=None
canvas=None
is_running=False
after_id=None

def prepare_game_screen(frame):
    global canvas
    canvas=tk.Canvas(frame,width=WIDTH,height=HEIGHT,bg="black",takefocus=True)
    
def show_game_canvas():
    global snake, direction, is_running
    # 게임 상태 초기화
    snake = [(100, 100), (80, 100), (60, 100)]
    direction = "Right"
    is_running=False
    
    #캔버스 배치 및 초기화
    canvas.delete("all")
    
    #초기 뱀가 음식 그리기
    place_food()
    draw()

def hide_game_canvas():
    stop_snake_game()
    if canvas:
        canvas.pack_forget()
    
def place_food():
    global food
    x = random.randint(0, (WIDTH - CELL_SIZE) // CELL_SIZE) * CELL_SIZE
    y = random.randint(0, (HEIGHT - CELL_SIZE) // CELL_SIZE) * CELL_SIZE
    food = (x, y)

def draw():
    canvas.delete("all")
    # 뱀 그리기
    for x, y in snake:
        canvas.create_rectangle(x, y, x + CELL_SIZE, y + CELL_SIZE, fill="green")
    # 음식 그리기
    if food:
        canvas.create_oval(food[0], food[1], food[0] + CELL_SIZE, food[1] + CELL_SIZE, fill="red")

def move():
    global snake, after_id
    if not is_running: return
    
    head_x, head_y = snake[0]

    if direction == "Up":
        head_y -= CELL_SIZE
    elif direction == "Down":
        head_y += CELL_SIZE
    elif direction == "Left":
        head_x -= CELL_SIZE
    elif direction == "Right":
        head_x += CELL_SIZE

    new_head = (head_x, head_y)

    # 충돌 체크
    if (head_x < 0 or head_x >= WIDTH or head_y < 0 or head_y >= HEIGHT or new_head in snake):
        game_over()
        return

    snake.insert(0, new_head)

    # 음식 먹기
    if new_head == food:
        place_food()
    else:
        snake.pop()

    draw()
    after_id=canvas.after(SPEED, move)

def change_direction(event):
    global direction
    new_dir = event.keysym
    opposites = {"Up": "Down", "Down": "Up", "Left": "Right", "Right": "Left"}
    if new_dir in opposites and opposites[new_dir] != direction:
        direction = new_dir

def game_over():
    global is_running
    is_running=False
    canvas.create_text(WIDTH/2, HEIGHT/2, text="GAME OVER", fill="white", font=("Arial", 24))

def start_snake_game():
    global is_running
    if is_running:return
    
    is_running=True
    canvas.after(100,canvas.focus_set)
    canvas.bind("<KeyPress>",change_direction)
    move()

def stop_snake_game():
    global is_running, after_id
    is_running=False
    if canvas and after_id:
        canvas.after_cancel(after_id)
        after_id=None


