
import tkinter as tk
import random

game_state = {
    "snake": [],
    "direction": "Right",
    "food": None,
    "running": False,
    "timer_id": None
}


def ready_game(parent_frame):
    stop_snake_game()
    if game_state["timer_id"]:
        parent_frame.after_cancel(game_state["timer_id"])
        game_state["timer_id"] = None

    # 2. 기존에 생성된 캔버스들 모두 삭제
    for widget in parent_frame.winfo_children():
        if isinstance(widget, tk.Canvas):
            widget.destroy()

    # 3. 빈 검정색 캔버스만 생성해서 배치 (아직 뱀은 없음)
    canvas = tk.Canvas(parent_frame, width=400, height=400, bg="black")
    canvas.pack(pady=20)
    canvas.create_text(200, 200, text="준비되셨나요?\n\n'게임 시작' 버튼을 눌러주세요!",
                       fill="white", font=("Arial", 16), justify="center")


def start_snake_game(parent_frame):
    """실제로 게임을 시작하는 함수"""
    # 이미 게임이 진행 중이라면 중복 실행 방지
    if game_state["running"]:
        return

    WIDTH, HEIGHT, CELL_SIZE, SPEED = 400, 400, 20, 150

    # 준비 화면의 텍스트를 지우기 위해 캔버스를 새로 그림
    for widget in parent_frame.winfo_children():
        if isinstance(widget, tk.Canvas):
            widget.destroy()

    canvas = tk.Canvas(parent_frame, width=WIDTH, height=HEIGHT, bg="black")
    canvas.pack(pady=20)

    # 게임 데이터 초기화
    game_state["snake"] = [(100, 100), (80, 100), (60, 100)]
    game_state["direction"] = "Right"
    game_state["running"] = True

    # --- 내부 함수 (draw, move, place_food 등은 기존과 동일) ---
    def place_food():
        x = random.randint(0, (WIDTH - CELL_SIZE) // CELL_SIZE) * CELL_SIZE
        y = random.randint(0, (HEIGHT - CELL_SIZE) // CELL_SIZE) * CELL_SIZE
        game_state["food"] = (x, y)

    def draw():
        canvas.delete("all")
        for x, y in game_state["snake"]:
            canvas.create_rectangle(x, y, x + CELL_SIZE, y + CELL_SIZE, fill="blue")
        f = game_state["food"]
        if f: canvas.create_oval(f[0], f[1], f[0] + CELL_SIZE, f[1] + CELL_SIZE, fill="red")

    def move():
        if not game_state["running"]: return
        head_x, head_y = game_state["snake"][0]
        if game_state["direction"] == "Up":
            head_y -= CELL_SIZE
        elif game_state["direction"] == "Down":
            head_y += CELL_SIZE
        elif game_state["direction"] == "Left":
            head_x -= CELL_SIZE
        elif game_state["direction"] == "Right":
            head_x += CELL_SIZE

        new_head = (head_x, head_y)
    
        if (head_x < 0 or head_x >= WIDTH or head_y < 0 or head_y >= HEIGHT or new_head in game_state["snake"]):
            game_over()
            return

        game_state["snake"].insert(0, new_head)
        if new_head == game_state["food"]:
            place_food()
        else:
            game_state["snake"].pop()
        draw()
        game_state["timer_id"] = parent_frame.after(SPEED, move)

    def game_over():
        game_state["running"] = False
        canvas.create_text(WIDTH / 2, HEIGHT / 2, text="GAME OVER", fill="white", font=("Arial", 24))

    def change_direction(event):
        new_dir = event.keysym
        opposites = {"Up": "Down", "Down": "Up", "Left": "Right", "Right": "Left"}
        if new_dir in opposites and opposites[new_dir] != game_state["direction"]:
            game_state["direction"] = new_dir

    canvas.focus_set()
    canvas.bind("<KeyPress>", change_direction)
    place_food()
    draw()
    move()


def stop_snake_game():
    game_state["running"] = False