# 定时关机软件

**定时关机软件**
版本：v1.0
发布日期：2025年4月28日
开发语言：Python 3
运行环境：Windows 7及以上（Windows 7及以下需预装 Python 3.3）
授权协议：免费软件
安全性：绿色版，无病毒，无捆绑，无广告
有原代码

📌 软件简介
定时关机软件是一款轻量级的系统辅助工具，支持快速设置定时关机或重启任务，操作简便，界面清爽，适用于各种办公与日常使用场景。

✨ 核心功能
定时关机：根据设定时间，自动关闭计算机。

定时重启：根据设定时间，自动重启计算机。

轻量运行：软件体积小，资源占用极低。

绿色安全：无需安装，拷贝即用，无后台驻留。

📥 下载地址
[夸克网盘](https://pan.quark.cn/s/b8eb8566b305)
[蓝奏云（密码:f423）](https://bilibili-xiaohui.lanzouq.com/iT14C2ph6k6h)

📷 软件截图

![](assets/png/articles/31881a10f305.png)
⚙️ 使用说明
打开软件，选择关机或重启模式。

设置定时的具体时间（单位：秒/分钟）。

点击【开始定时】按钮，软件将在设定时间执行操作。

🛠️ 源代码

```
import os
import time
import tkinter as tk
from tkinter import messagebox
from threading import Thread
from datetime import datetime, timedelta
import webbrowser

# 定时关机函数
def shutdown():
    if os.name == 'nt':  # Windows 系统
        os.system("shutdown /s /f /t 0")
    else:  # Linux / macOS 系统
        os.system("sudo shutdown -h now")

# 定时重启函数
def restart():
    if os.name == 'nt':  # Windows 系统
        os.system("shutdown /r /f /t 0")
    else:  # Linux / macOS 系统
        os.system("sudo reboot")

# 取消关机函数
def cancel_shutdown():
    if os.name == 'nt':  # Windows 系统
        os.system("shutdown /a")
    else:  # Linux / macOS 系统
        os.system("sudo shutdown -c")
    messagebox.showinfo("提示", "关机任务已取消！")

# 在倒计时时提醒用户
def countdown_reminder(target_time):
    current_time = datetime.now()
    remaining_time = target_time - current_time
    while remaining_time.total_seconds() > 0:
        if remaining_time.total_seconds() in [60, 30, 15, 10, 5, 1]:  # 提前提醒
            messagebox.showinfo("提示", f"{int(remaining_time.total_seconds())}秒后系统将关机！")
        time.sleep(1)
        remaining_time = target_time - datetime.now()

# 计算剩余时间并执行定时关机
def schedule_shutdown(target_time, is_reboot=False):
    countdown_thread = Thread(target=countdown_reminder, args=(target_time,))
    countdown_thread.start()

    # 等待到达目标时间
    current_time = datetime.now()
    remaining_time = target_time - current_time
    if remaining_time.total_seconds() > 0:
        time.sleep(remaining_time.total_seconds())  # 等待直到目标时间
        if is_reboot:
            restart()
        else:
            shutdown()
    else:
        messagebox.showerror("错误", "目标时间必须在当前时间之后！")

# 获取用户输入并启动定时关机或重启
def start_shutdown():
    try:
        # 获取具体日期和时间的输入
        if use_datetime.get():
            year = int(entry_year.get())
            month = int(entry_month.get())
            day = int(entry_day.get())
            hour = int(entry_hour.get())
            minute = int(entry_minute.get())
            second = int(entry_second.get())
            # 拼接成目标时间
            target_time = datetime(year, month, day, hour, minute, second)
        else:
            # 获取多久后自动关机的输入
            hours_after = int(entry_hours_after.get())
            minutes_after = int(entry_minutes_after.get())
            seconds_after = int(entry_seconds_after.get())
            # 当前时间加上指定的间隔
            target_time = datetime.now() + timedelta(hours=hours_after, minutes=minutes_after, seconds=seconds_after)

        # 获取用户选择的操作类型
        is_reboot = reboot_var.get()

        # 启动定时关机或重启
        messagebox.showinfo("提示", f"系统将在 {target_time} 自动{'重启' if is_reboot else '关机'}。")

        # 启动一个线程来执行定时关机或重启
        shutdown_thread = Thread(target=schedule_shutdown, args=(target_time, is_reboot))
        shutdown_thread.start()

    except ValueError:
        messagebox.showerror("错误", "请输入有效的日期和时间！")

# 打开作者主页
def open_author_website():
    webbrowser.open("https://space.bilibili.com/3546654714104666?spm_id_from=333.1007.0.0")

def open_author_rj():
    webbrowser.open("https://space.bilibili.com/3546654714104666?spm_id_from=333.1007.0.0")

def ds():
    webbrowser.open("https://imgur.la/image/52833926112556c58c1e23f3a8c842d.Pn81U")

# 创建主窗口
root = tk.Tk()
root.title("定时关机程序")

# 设置窗口大小
root.geometry("500x500")

# 使用日期和时间的选项
use_datetime = tk.BooleanVar(value=True)
reboot_var = tk.BooleanVar(value=False)

# 标签
label = tk.Label(root, text="请选择定时关机的方式：")
label.pack(pady=10)

# 选择框：是否使用具体的日期和时间
frame_option = tk.Frame(root)
frame_option.pack(pady=5)

radio_datetime = tk.Radiobutton(frame_option, text="设置具体日期和时间", variable=use_datetime, value=True)
radio_datetime.pack(side="left", padx=5)
radio_after = tk.Radiobutton(frame_option, text="设置多久后自动关机", variable=use_datetime, value=False)
radio_after.pack(side="left", padx=5)

# 重启选项
frame_reboot = tk.Frame(root)
frame_reboot.pack(pady=5)
reboot_check = tk.Checkbutton(frame_reboot, text="选择重启", variable=reboot_var)
reboot_check.pack()

# 日期时间输入框部分
frame_datetime = tk.Frame(root)
frame_datetime.pack(pady=5)

label_year = tk.Label(frame_datetime, text="年：")
label_year.grid(row=0, column=0, padx=5)
entry_year = tk.Entry(frame_datetime, width=5)
entry_year.grid(row=0, column=1, padx=5)

label_month = tk.Label(frame_datetime, text="月：")
label_month.grid(row=0, column=2, padx=5)
entry_month = tk.Entry(frame_datetime, width=3)
entry_month.grid(row=0, column=3, padx=5)

label_day = tk.Label(frame_datetime, text="日：")
label_day.grid(row=0, column=4, padx=5)
entry_day = tk.Entry(frame_datetime, width=3)
entry_day.grid(row=0, column=5, padx=5)

label_hour = tk.Label(frame_datetime, text="时：")
label_hour.grid(row=1, column=0, padx=5)
entry_hour = tk.Entry(frame_datetime, width=3)
entry_hour.grid(row=1, column=1, padx=5)

label_minute = tk.Label(frame_datetime, text="分：")
label_minute.grid(row=1, column=2, padx=5)
entry_minute = tk.Entry(frame_datetime, width=3)
entry_minute.grid(row=1, column=3, padx=5)

label_second = tk.Label(frame_datetime, text="秒：")
label_second.grid(row=1, column=4, padx=5)
entry_second = tk.Entry(frame_datetime, width=3)
entry_second.grid(row=1, column=5, padx=5)

# 多久后关机输入框部分
frame_after = tk.Frame(root)
frame_after.pack(pady=5)

label_hours_after = tk.Label(frame_after, text="小时：")
label_hours_after.grid(row=0, column=0, padx=5)
entry_hours_after = tk.Entry(frame_after, width=5)
entry_hours_after.grid(row=0, column=1, padx=5)

label_minutes_after = tk.Label(frame_after, text="分钟：")
label_minutes_after.grid(row=0, column=2, padx=5)
entry_minutes_after = tk.Entry(frame_after, width=5)
entry_minutes_after.grid(row=0, column=3, padx=5)

label_seconds_after = tk.Label(frame_after, text="秒：")
label_seconds_after.grid(row=0, column=4, padx=5)
entry_seconds_after = tk.Entry(frame_after, width=5)
entry_seconds_after.grid(row=0, column=5)

# 启动定时关机按钮
button_start = tk.Button(root, text="开始定时关机", command=start_shutdown)
button_start.pack(pady=10)

# 取消关机按钮
button_cancel = tk.Button(root, text="取消关机", command=cancel_shutdown)
button_cancel.pack(pady=5)

# 关于按钮
button_about = tk.Button(root, text="关于", command=open_author_website)
button_about.pack(pady=5)

button_about = tk.Button(root, text="关于软件", command=open_author_rj)
button_about.pack(pady=5)

button_ds = tk.Button(root,text="打赏",command=ds)
button_ds.pack(pady=20)

# 更新输入框显示：根据选择是否设置具体时间或设置多久后关机
def update_inputs():
    if use_datetime.get():
        frame_datetime.pack(pady=5)
        frame_after.pack_forget()
    else:
        frame_after.pack(pady=5)
        frame_datetime.pack_forget()

use_datetime.trace_add("write", lambda *args: update_inputs())  # 监听 radio 按钮的变化

# 初始化显示
update_inputs()

# 运行窗口
root.mainloop()
```
