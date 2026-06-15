import tkinter as tk
import test2
from PIL import Image, ImageTk

def start_actual_snake():
    test2.start_snake_game(page5)
    
def go_to_snake_game():
    global reward
    reward=False
    # 게임 화면으로 이동만 함 (게임 시작은 안 함)
    show_frame(page5)
    # 기존에 남아있을 수 있는 캔버스 정리 및 초기 화면 구성
    test2.canvas.pack(pady=20)
    test2.show_game_canvas()

def go_back_to_menu():
    test2.stop_snake_game() # 게임 타이머 중지
    show_frame(page4)

def game_clear():
    global reward
    if reward:
        return
    if len(test2.game_state["snake"])>=15:
        reward=True
        gain_experience(page5)
        label_reward=tk.Label(page5,text=f"게임 클리어 15경험치를 얻었습니다.\n현재레벨:{current_level},경험치:{current_exp}",font=("한컴 윤고딕 240", 10))
        label_reward.pack(pady=10)
        root.after(5000,label_reward.destroy)
        return
    else:
        root.after(500,game_clear)    
        
def show_frame(frame):
    frame.tkraise()

# 화면 전환과 동시에 이름을 업데이트하는 함수
def start_game():
    user_name = name_entry.get() # 2번 화면 입력창에서 이름 가져오기
    if not user_name.strip():    # 이름을 입력 안 했을 경우 대비
        user_name = "고양이"
# 3번 화면의 이름 라벨 업데이트
    char_name_label.config(text=user_name)
    # 3번 화면으로 전환
    show_frame(page3)

    welcome_label.pack(pady=30)
    welcome_label.config(text="집사가 된 걸 축하합니다!")
    root.after(3000, welcome_label.pack_forget)

#고양이 선택시 나오는 텍스트
def alert(n):
    if n==1:
        label_text.config(text="첫번째 고양이를 선택하셨습니다.",font=("한컴 윤고딕 200",20)) 
        cat_char.config(image=tk_img1)   
    elif n==2:
        label_text.config(text="두번째 고양이를 선택하셨습니다.",font=("한컴 윤고딕 200",20))  
        cat_char.config(image=tk_img2)   
    elif n==3:
        label_text.config(text="세번째 고양이를 선택하셨습니다.",font=("한컴 윤고딕 200",20))
        cat_char.config(image=tk_img3)   
    root.after(3000,lambda:show_frame(page2))
    
root = tk.Tk()
root.title("키우기 게임")
root.geometry("700x700")

container = tk.Frame(root)
container.pack(fill="both", expand=True)

#화면 생성
page1 = tk.Frame(container, bg="lightyellow")
page2 = tk.Frame(container, bg="lightgrey")
page3= tk.Frame(container, bg="lightgrey")
page4= tk.Frame(container, bg="lightgrey")
page5= tk.Frame(container, bg="skyblue")
page6=tk.Frame(container,bg="lightgrey")

#레벨 바 변수 지정
current_exp = 0  # 현재 경험치 변수
max_exp = 100  # 레벨업에 필요한 총 경험치
current_level = 1  # 현재 레벨 변수
bar_max_width = 100  # 레벨 바의 최대 가로 길이(픽셀)
reward=False

# 화면들을 같은 자리에 겹쳐서 배치
for frame in (page1, page2, page3, page4, page5,page6):
    frame.place(x=0, y=0, relwidth=1, relheight=1)


# --- 1번 화면 구성 ---
label1 = tk.Label(page1, text=" 고양이 키우기 ", font=("한컴 윤고딕 240", 20), bg="skyblue")
label1.pack(pady=50)

btn_start = tk.Button(page1, text="시작하기",width=20,height=2, font=("한컴 윤고딕 240",18),command=lambda: show_frame(page6))
btn_start.pack()

# --- 2번 화면 구성 ---
label2 = tk.Label(page2, text="고양이 이름을 정해봐요!", font=("한컴 윤고딕 240", 20), bg="white")
label2.pack(pady=50)

name_entry = tk.Entry(page2, font=("한컴 윤고딕 240", 18), justify='center')
name_entry.pack(pady=20)

# --- 결정 버튼 ---
btn_enter = tk.Button(page2, text="결정", width=15, font=("한컴 윤고딕 240", 15),command=start_game)
btn_enter.pack(pady=20)

# --- 3번 화면 구성 ---
# 캐릭터 이름
char_name_label = tk.Label(page3, text="", font=("한컴 윤고딕 240", 15, "bold"), bg="yellow")
char_name_label.pack(pady=(150, 0))

# 고양이 캐릭터
cat_char = tk.Label(page3,bg="lightgrey")
cat_char.pack(pady=10)

welcome_label = tk.Label(page3, text="", font=("한컴 윤고딕 240", 15))
welcome_label.pack(pady=30)

btn_game = tk.Button(page3, text="게임", font=("한컴 윤고딕 240", 15), command=lambda: show_frame(page4))
btn_game.place(x=600, y=600, width=80, height=40)

#레벨 바 화면 설정
label_level=tk.Label(page3,text=f"현재 레벨:{current_level},현재 경험치:{current_exp}",font=("한컴 윤고딕 240", 15))
label_level.place(x=20,y=30)

canvas_game=tk.Canvas(page3,width=bar_max_width,height=30)
canvas_game.place(x=20,y=60)

level_bar=canvas_game.create_rectangle(0,0,0,30,fill="limegreen",outline="")

def gain_experience(page):
    global current_exp, current_level,bar_max_width,max_exp
    current_exp += 15
    if current_exp>=max_exp:
        current_level+=1
        current_exp-=max_exp
        max_exp+=100
        levelup_label=tk.Label(page,text=f"레벨업!\n현재 레벨:{current_level}")
        levelup_label.place(x=250,y=100)        
        levelup_label.after(5000,levelup_label.destroy)
        label_level.config(text=f"현재 레벨:{current_level},현재 경험치:{current_exp}")
    else:
        label_level.config(text=f"현재 레벨:{current_level},현재 경험치:{current_exp}")
    exp_ratio=current_exp/max_exp
    new_width=exp_ratio*bar_max_width
    canvas_game.coords(level_bar,0,0,new_width,30)

# ---4번 화면 구성 ---
label4 = tk.Label(page4, text="게임", font=("한컴 윤고딕 240", 40), bg="white")
label4.pack(pady=50)

btn_back = tk.Button(page4, text="돌아가기",font=("한컴 윤고딕 240", 20), command=lambda: show_frame(page3))
btn_back.place(x=500, y=600)

btn_snake = tk.Button(page4, text="스네이크 게임", font=("한컴 윤고딕 240", 15),command=go_to_snake_game)
btn_snake.place(x=100, y=200)

# --- snake 게임 구현(5)---
label5 = tk.Label(page5,text="스네이크 게임", font=("한컴 윤고딕 240", 35), bg="white")
label5.pack(pady=10)

btn_real_start = tk.Button(page5, text="게임 시작", font=("한컴 윤고딕 240", 15),command=lambda: [test2.start_snake_game(page5),game_clear()])
btn_real_start.place(x=300, y=500)

btn_back = tk.Button(page5, text="돌아가기", font=("한컴 윤고딕 240", 10), command=go_back_to_menu)
btn_back.place(x=570, y=650)

# ---6번 화면 구성 (고양이 선택)---
frame_button = tk.Frame(page6,bg="lightgrey")
frame_button.place(x=80,y=420)
#텍스트 라벨
label_text=tk.Label(page6,text="",bg="lightgrey")
label_text.place(x=150,y=500)

label_select=tk.Label(page6,text="당신의 고양이를 고르세요",font=("한컴 윤고딕 240", 20),bg="lightgrey")
label_select.place(x=190,y=90)

#첫 번째 검정 고양이
img1=Image.open("아기 검정 고양ㅇ.png")
img1=img1.resize((200,250),Image.Resampling.LANCZOS)
tk_img1=ImageTk.PhotoImage(img1,master=root)
label_img1=tk.Label(page6,image=tk_img1,bg="lightgrey")
label_img1.place(x=80,y=170)

#두 번째 치즈 고양이
img2=Image.open("아기 치즈 고양이.png")
img2=img2.resize((200,250),Image.Resampling.LANCZOS)
tk_img2=ImageTk.PhotoImage(img2,master=root)

label_img2=tk.Label(page6,image=tk_img2,bg="lightgrey")
label_img2.place(x=250,y=170)

#세 번째 흰 고양이
img3=Image.open("아기 흰 고양이.png")
img3=img3.resize((200,250),Image.Resampling.LANCZOS)
tk_img3=ImageTk.PhotoImage(img3,master=root)

label_img3=tk.Label(page6,image=tk_img3,bg="lightgrey")
label_img3.place(x=400,y=170)

#첫번째 고양이 선택 버튼
btn1= tk.Button(frame_button)
btn1.config(width=20,height=2)
btn1.config(text='선택')
btn1.config(command=lambda:alert(1))
btn1.pack(side='left',padx=10)

#두번째 고양이 선택 버튼
btn2= tk.Button(frame_button)
btn2.config(width=20,height=2)
btn2.config(text='선택')
btn2.config(command=lambda:alert(2))
btn2.pack(side='left',padx=10)

#세번째 고양이 선택 버튼
btn3= tk.Button(frame_button)
btn3.config(width=20,height=2)
btn3.config(text='선택')
btn3.config(command=lambda:alert(3))
btn3.pack(side='left',padx=10)


# 처음 시작할 화면 설정
show_frame(page1)

root.mainloop()