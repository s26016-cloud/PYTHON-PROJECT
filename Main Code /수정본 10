import tkinter as tk
import test2
import test3
from PIL import Image, ImageTk


def start_actual_snake():
    test2.start_snake_game(page5)


def go_to_snake_game():
    global reward
    reward = False
    # 게임 화면으로 이동하고 스네이크 준비 화면을 만든다.
    show_frame(page5)
    test2.ready_game(page5)

#게임에서 홈 화면으로 움직이기 위한 함수
def go_back_to_menu():
    test2.stop_snake_game()
    test3.stop_reaction_game()
    show_frame(page4)

#게임 클리어 시 경험치 주기 위한 함수
def game_clear():
    global reward
    if reward:
        return
    if len(test2.game_state["snake"]) >= 15:
        reward = True
        give_reward(page5, 20, -20)
        return
    else:
        root.after(500, game_clear)

#화면 전환 함수
def show_frame(frame):
    frame.tkraise()
    #마지막 회색 화면을 띄우기 위한 조건
    if frame==page3 and current_level==3:
                root.after(2000, show_game_clear)

def go_to_reaction_game():
    test3.ready_reaction_game()
    show_frame(page7)

start_reaction_game = test3.start_reaction_game
stop_reaction_game = test3.stop_reaction_game
hit_reaction_target = test3.hit_reaction_target


# 화면 전환과 동시에 이름을 업데이트하는 함수
def start_game():
    user_name = name_entry.get()
    if not user_name.strip():
        user_name = "고양이"
    char_name_label.config(text=user_name)
    show_frame(page3)

    welcome_label.pack(pady=30)
    welcome_label.config(text="집사가 된 걸 축하합니다!")
    root.after(3000, welcome_label.pack_forget)


# 고양이 선택 안내 텍스트 함수
def alert(n):
    global selected_cat
    if n == 1:
        label_text.config(text="검정 고양이를 선택하셨습니다.", font=("한컴 윤고딕 200", 20))
        cat_char.config(image=tk_img_baby_black_cat)
        selected_cat='black'
    elif n == 2:
        label_text.config(text="치즈 고양이를 선택하셨습니다.", font=("한컴 윤고딕 200", 20))
        cat_char.config(image=tk_img_baby_yellow_cat)
        selected_cat='yellow'
    elif n == 3:
        label_text.config(text="흰 고양이를 선택하셨습니다.", font=("한컴 윤고딕 200", 20))
        cat_char.config(image=tk_img_baby_white_cat)
        selected_cat='white'
    root.after(1500, lambda: show_frame(page2))

#레벨 달성 시 고양이 사진아 변하는 함수
def char_change():
    global selected_cat
    if selected_cat=='black' and current_level==2:
        cat_char.config(image=tk_img_mid_black_cat)
        selected_cat='mid_black'
    
    elif selected_cat=='yellow' and current_level==2:
        cat_char.config(image=tk_img_mid_yellow_cat)
        selected_cat='mid_yellow'
        
    elif selected_cat=='white' and current_level==2:
        cat_char.config(image=tk_img_mid_white_cat)
        selected_cat='mid_white'
        
    elif selected_cat=='mid_black' and current_level==3:
        cat_char.config(image=tk_img_adu_black_cat)
    elif selected_cat=='mid_yellow' and current_level==3:
        cat_char.config(image=tk_img_adu_yellow_cat)
    elif selected_cat=='mid_white' and current_level==3:
        cat_char.config(image=tk_img_adu_white_cat)

#고양이 게임이 끝났을 때 회색화면을 띄우는 함수
def show_game_clear():
    width = root.winfo_width()
    height = root.winfo_height()

    overlay = tk.Canvas(page3, width=width, height=height, highlightthickness=0)
    overlay.place(x=0, y=0)

    # 반투명 회색 배경 (stipple로 반투명 효과)
    overlay.create_rectangle(0, 0, width, height, fill="gray", stipple="gray50")

    # 중앙에 게임 클리어 텍스트
    overlay.create_text(width//2, height//2, text="게임 클리어!", font=("맑은 고딕", 30, "bold"), fill="white")

    
# 경험치 얻는 함수
def gain_experience(page,exp):
    global current_exp, current_level, bar_max_width, max_exp
    label_leveltext=tk.Label(page)
    label_leveltext.pack()

    current_exp +=exp
    if current_exp >= max_exp:
            current_level += 1
            current_exp -= max_exp
            max_exp += 100
            label_leveltext.config(text=f"레벨업!\n현재 레벨:{current_level} 고양이가 컸습니다",font=("한컴 윤고딕 240", 15))
            label_leveltext.after(1500,label_leveltext.destroy)
            char_change()
            # 마지막 회색 화면 띄우기 위한 조건
            if page==page3 and current_level==3:
                root.after(2000, show_game_clear)
    else:
        label_leveltext.config(text=f"{exp}경험치를 얻었습니다.\n현재레벨:{current_level},경험치:{current_exp}",font=("한컴 윤고딕 240", 15))
        label_leveltext.after(1500,label_leveltext.destroy)
        
    label_level.config(text=f'현재 레벨:{current_level},현재 경험치:{current_exp}', font= ("한컴 윤고딕 240", 15))
    exp_ratio = current_exp / max_exp
    new_width = exp_ratio * bar_max_width
    canvas_game.coords(level_bar, 0, 0, new_width, 30)


# 체력 변경 함수
def change_health(damage):
    global health
    health += damage
    if health > 100:
        health = 100
    if health < 0:
        health = 0
    label_health.config(text=f"현재 체력:{health}", font=("맑은 고딕", 15))


def give_reward(page, exp, damage):
    global health
    if health >= 100 and damage > 0:
        label_exp = tk.Label(page, text="체력은 최대입니다.", font=("맑은 고딕", 15))
        label_exp.pack()
        label_exp.after(1500, label_exp.destroy)
    elif health == 0 and damage < 0:
        label_exp = tk.Label(page, text="체력은 0입니다.", font=("맑은 고딕", 15))
        label_exp.pack()
        label_exp.after(1500, label_exp.destroy)
    else:
        gain_experience(page, exp)
        change_health(damage)


root = tk.Tk()
root.title("고양이 키우기 게임")
root.geometry("700x700")
container = tk.Frame(root)
container.pack(fill="both", expand=True)

# 화면 생성
page1 = tk.Frame(container, bg="lightyellow")
page2 = tk.Frame(container, bg="lightgrey")
page3 = tk.Frame(container, bg="lightgrey")
page4 = tk.Frame(container, bg="lightgrey")
page5 = tk.Frame(container, bg="skyblue")
page6 = tk.Frame(container, bg="lightgrey")
page7 = tk.Frame(container, bg="lightgrey")

# 레벨 및 경험치 변수
current_exp = 0
max_exp = 100
current_level = 1
bar_max_width = 100
reward = False
health = 100

# 화면들을 같은 자리에 겹쳐서 배치
for frame in (page1, page2, page3, page4, page5, page6, page7):
    frame.place(x=0, y=0, relwidth=1, relheight=1)

# --- 1번 화면 구성 ---
label1 = tk.Label(page1, text="고양이 키우기", font=("맑은 고딕", 20), bg="skyblue")
label1.pack(pady=50)

btn_start = tk.Button(page1, text="시작하기", width=20, height=2, font=("맑은 고딕", 18),
            command=lambda: show_frame(page6))
btn_start.pack()

# --- 2번 화면 구성 ---
label2 = tk.Label(page2, text="고양이 이름을 정해보세요!", font=("맑은 고딕", 20), bg="white")
label2.pack(pady=50)

name_entry = tk.Entry(page2, font=("맑은 고딕", 18), justify='center')
name_entry.pack(pady=20)

# --- 결정 버튼 ---
btn_enter = tk.Button(page2, text="결정", width=15, font=("맑은 고딕", 15), command=start_game)
btn_enter.pack(pady=20)

# --- 3번 화면 구성 ---
# 캐릭터 이름
char_name_label = tk.Label(page3, text="", font=("맑은 고딕", 15, "bold"), bg="yellow")
char_name_label.pack(pady=(150, 0))

# 고양이 캐릭터
cat_char = tk.Label(page3, bg="lightgrey")
cat_char.pack(pady=10)

welcome_label = tk.Label(page3, text="", font=("맑은 고딕", 15))
welcome_label.pack(pady=30)

btn_game = tk.Button(page3, text="게임", font=("맑은 고딕", 15), command=lambda: show_frame(page4))
btn_game.place(x=600, y=600, width=80, height=40)

# 레벨 바 화면 설정
label_level = tk.Label(page3, text=f"현재 레벨:{current_level}, 현재 경험치:{current_exp}", font=("맑은 고딕", 15))
label_level.place(x=20, y=30)

canvas_game = tk.Canvas(page3, width=bar_max_width, height=30)
canvas_game.place(x=20, y=60)

label_health = tk.Label(page3, text=f"현재 체력:{health}", font=("맑은 고딕", 15))
label_health.place(x=20, y=100)

level_bar = canvas_game.create_rectangle(0, 0, 0, 30, fill="limegreen", outline="")

# 상호작용 버튼 설정
food_button = tk.Button(page3, text="밥 먹이기", font=("맑은 고딕", 15), command=lambda: give_reward(page3, 10, 15))
food_button.pack(side="left", padx=30)

play_button = tk.Button(page3, text="놀아주기", font=("맑은 고딕", 15), command=lambda: give_reward(page3, 10, -15))
play_button.pack(side="left", padx=30)

# --- 4번 화면 구성 ---
label4 = tk.Label(page4, text="게임", font=("맑은 고딕", 15), bg="white")
label4.pack(pady=50)

btn_back = tk.Button(page4, text="돌아가기", font=("맑은 고딕", 20), command=lambda: show_frame(page3))
btn_back.place(x=500, y=600)

btn_snake = tk.Button(page4, text="스네이크 게임", font=("맑은 고딕", 15), command=go_to_snake_game)
btn_snake.place(x=100, y=200)

btn_reaction = tk.Button(page4, text="반응속도 게임", font=("맑은 고딕", 15), command=go_to_reaction_game)
btn_reaction.place(x=100, y=280)

# --- 스네이크 게임 구현(5) ---
label5 = tk.Label(page5, text="스네이크 게임", font=("맑은 고딕", 35), bg="white")
label5.pack(pady=10)

btn_real_start = tk.Button(page5, text="게임 시작", font=("맑은 고딕", 15),
            command=lambda: [test2.start_snake_game(page5), game_clear()])
btn_real_start.place(x=300, y=500)

btn_back = tk.Button(page5, text="돌아가기", font=("맑은 고딕", 10), command=go_back_to_menu)
btn_back.place(x=570, y=650)

# --- reaction game page ---
label7 = tk.Label(page7, text="반응속도 게임", font=("맑은 고딕", 32, "bold"), bg="lightgrey")
label7.pack(pady=20)

reaction_status = tk.Label(
    page7,
    text="시작 버튼을 누르면 목표물이 랜덤하게 나타납니다.",
    font=("맑은 고딕", 14),
    bg="lightgrey",
)
reaction_status.pack(pady=5)

reaction_score_label = tk.Label(page7, text="점수: 0", font=("맑은 고딕", 14), bg="lightgrey")
reaction_score_label.pack(pady=5)

reaction_canvas = tk.Canvas(page7, width=test3.REACTION_CANVAS_WIDTH, height=test3.REACTION_CANVAS_HEIGHT, bg="black")
reaction_canvas.pack(pady=15)
reaction_canvas.tag_bind("target", "<Button-1>", hit_reaction_target)

btn_reaction_start = tk.Button(page7, text="게임 시작", font=("맑은 고딕", 15), command=start_reaction_game)
btn_reaction_start.place(x=260, y=610)

btn_reaction_back = tk.Button(page7, text="돌아가기", font=("맑은 고딕", 12), command=go_back_to_menu)
btn_reaction_back.place(x=580, y=650)

test3.connect_reaction_game(
    root,
    reaction_status,
    reaction_score_label,
    reaction_canvas,
    lambda exp: gain_experience(page7, exp),
)

# --- 6번 화면 구성 (고양이 선택) ---
frame_button = tk.Frame(page6, bg="lightgrey")
frame_button.place(x=80, y=420)
# 텍스트 라벨
label_text = tk.Label(page6, text="", bg="lightgrey")
label_text.place(x=150, y=500)

label_select = tk.Label(page6, text="당신의 고양이를 골라보세요", font=("맑은 고딕", 20), bg="lightgrey")
label_select.place(x=190, y=90)

# 아기 검정 고양이
img_baby_black_cat = Image.open("아기 검정 고양이.png")
img_baby_black_cat = img_baby_black_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_baby_black_cat = ImageTk.PhotoImage(img_baby_black_cat, master=root)
label_img1 = tk.Label(page6, image=tk_img_baby_black_cat, bg="lightgrey")
label_img1.place(x=80, y=170)

# 아기 치즈 고양이
img_baby_yellow_cat = Image.open("아기 치즈 고양이.png")
img_baby_yellow_cat = img_baby_yellow_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_baby_yellow_cat= ImageTk.PhotoImage(img_baby_yellow_cat, master=root)

label_img2 = tk.Label(page6, image=tk_img_baby_yellow_cat, bg="lightgrey")
label_img2.place(x=250, y=170)

# 아기 흰 고양이
img_baby_white_cat = Image.open("아기 흰 고양이.png")
img_baby_white_cat = img_baby_white_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_baby_white_cat = ImageTk.PhotoImage(img_baby_white_cat, master=root)

label_img3 = tk.Label(page6, image=tk_img_baby_white_cat, bg="lightgrey")
label_img3.place(x=400, y=170)

# 중간 검정 고양이
img_mid_black_cat = Image.open("중간 검정 고양이.png")
img_mid_black_cat = img_mid_black_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_mid_black_cat = ImageTk.PhotoImage(img_mid_black_cat, master=root)

#중간 치즈 고양이
img_mid_yellow_cat = Image.open("중간 치즈 고양이.png")
img_mid_yellow_cat = img_mid_yellow_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_mid_yellow_cat = ImageTk.PhotoImage(img_mid_yellow_cat, master=root)

#중간 흰 고양이
img_mid_white_cat = Image.open("중간 흰 고양이.png")
img_mid_white_cat = img_mid_white_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_mid_white_cat = ImageTk.PhotoImage(img_mid_white_cat, master=root)

#어른 검정 고양이
img_adu_black_cat = Image.open("큰 검정 고양이.png")
img_adu_black_cat = img_adu_black_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_adu_black_cat = ImageTk.PhotoImage(img_adu_black_cat, master=root)

#어른 치즈 고양이
img_adu_yellow_cat = Image.open("큰 치즈 고양이.png")
img_adu_yellow_cat = img_adu_yellow_cat.resize((200, 250), Image.Resampling.LANCZOS)
tk_img_adu_yellow_cat= ImageTk.PhotoImage(img_adu_yellow_cat, master=root)

#어른 흰 고양이
img_adu_white_cat = Image.open("큰 흰 고양이.png")
img_adu_white_cat = img_adu_white_cat.resize((250, 300), Image.Resampling.LANCZOS)
tk_img_adu_white_cat = ImageTk.PhotoImage(img_adu_white_cat, master=root)

# 첫번째 고양이 선택 버튼
btn1 = tk.Button(frame_button)
btn1.config(width=20, height=2)
btn1.config(text='선택')
btn1.config(command=lambda: alert(1))
btn1.pack(side='left', padx=10)

# 두번째 고양이 선택 버튼
btn2 = tk.Button(frame_button)
btn2.config(width=20, height=2)
btn2.config(text='선택')
btn2.config(command=lambda: alert(2))
btn2.pack(side='left', padx=10)

# 세번째 고양이 선택 버튼
btn3 = tk.Button(frame_button)
btn3.config(width=20, height=2)
btn3.config(text='선택')
btn3.config(command=lambda: alert(3))
btn3.pack(side='left', padx=10)

# 처음 시작할 화면 설정
show_frame(page1)

root.mainloop()