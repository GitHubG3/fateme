# fateme rezaei
پروژه بازی لاکپشت با زبان پایتون

import turtle #کتابخانه لاکپشت
import random  #انتخاب رنگ تصادفي
import time #زمان



# return second
def time_second():
    time.localtime()
    s = time.localtime().tm_sec
    return s


# تنظیمات صحنه
win = turtle.Screen()
win.title("لاکپشت و توپ")
win.bgpic("ftm.gif")
win.setup(width=560, height=590)
win.tracer(0)

# ایجاد لاکپشت
lak = turtle.Turtle()
lak.shape("turtle")
lak.color("light green")
lak.penup()


#border
w = turtle.Turtle()
w.color("dark blue")
w.up()
w.goto(-254, 255)
w.down()
w.width(5)
for i in range(4): 
    w.forward(510)
    w.right(90)
w.ht()

# ایجاد توپ‌
balls = []
num_balls = 1
def create_ball():
    ball = turtle.Turtle()
    ball.shape("circle")
    ball.color(random.choice(["red", "brown", "green", "pink", "purple", "orange", "blue", "yellow"]))  
    ball.penup()
    ball.goto(0, 0)  # توپ‌ها از وسط صحنه ظاهر می‌شوند
    ball.goto(random.randint(-90, 90), random.randint(-90, 90))  # توپ‌ها در محدوده اطراف وسط ظاهر می‌شوند
    balls.append(ball)

# امتیاز
score = 0 #در ابتدا امتياز صفره

# نمایش امتیاز روی صفحه
emt = turtle.Turtle()
emt.color("black")
emt.penup()
emt.hideturtle()
emt.goto(0, 260)
emt.write("جایزه: {}".format(score), align="center", font=("Courier", 20, "bold"))


# تابع بررسی برخورد لاکپشت و توپ
def is_collision(t1, t2):
    distance = t1.distance(t2)
    if distance < 20:
        return True
    else:
        return False


# حرکت لاکپشت
def move_up():
    y = lak.ycor()
    y += 20
    lak.sety(y)
    lak.setheading(90)  # تنظیم جهت صورت لاکپشت به بالا

def move_down():
    y = lak.ycor()
    y -= 20
    lak.sety(y)
    lak.setheading(270)  # تنظیم جهت صورت لاکپشت به پایین

def move_left():
    x = lak.xcor()
    x -= 20
    lak.setx(x)
    lak.setheading(180)  # تنظیم جهت صورت لاکپشت به چپ

def move_right():
    x = lak.xcor()
    x += 20
    lak.setx(x)
    lak.setheading(0)  # تنظیم جهت صورت لاکپشت به راست



# اتصال حرکت به کلیدها
win.listen()
win.onkeypress(move_up, "Up")
win.onkeypress(move_down, "Down")
win.onkeypress(move_left, "Left")
win.onkeypress(move_right, "Right")

# حلقه اصلی بازی
count = 0
s1 = time_second()
while True:
    tx = int(lak.xcor())
    ty = int(lak.ycor())
    if tx > 250 or tx < -250 or ty > 220 or ty < -265:
        score -= 2
        emt.clear()  
        emt.write("امتیاز: {}".format(score), align="center", font=("Courier", 20, "normal"))
        if tx > 250:
            lak.setx(lak.xcor() - 50)
        if tx < -250:
            lak.setx(lak.xcor() + 50)
        if ty > 220:
            lak.sety(lak.ycor() - 50)
        if ty < -265:
            lak.sety(lak.ycor() + 50)
    s2 = time_second()
    if s1 != s2:
        count += 1
        s1 = s2
    if count == 4:
        count = 0
        create_ball()
    win.update()
    for ball in balls:
        if (
            ball.xcor() > 248
            or ball.xcor() < -248
            or ball.ycor() > 220
            or ball.ycor() < -265
        ):
            ball.hideturtle()
            balls.remove(ball)
        else:
            ball.setx(ball.xcor() - 0.5)  # حرکت توپ به سمت لاکپشت
            # بررسی برخورد لاکپشت و توپ
            if is_collision(lak, ball):
                ball.goto(random.randint(-230, 230), random.randint(-270, 200))
                score += 2  # افزایش امتیاز
                emt.clear()  # پاک کردن
                emt.write("امتیاز: {}".format(score), align="center", font=("Courier", 20, "normal"))

