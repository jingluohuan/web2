# 检讨生成器

**检讨生成器介绍与使用说明**
在学习、工作和生活中，我们难免会遇到需要写检讨书的情况。为了帮助大家节省时间、提升效率，我们开发了一款简洁实用的“检讨生成器”小工具。本工具基于 Python 编写，绿色无毒，开源免费，适用于 Windows、macOS 和 Linux 系统。

**一、工具简介**

检讨生成器是一款基于命令行运行的文本生成工具，通过输入一些关键参数（如错误类型、检讨对象、改正态度等），即可自动生成一篇结构清晰、态度诚恳的检讨书。非常适合学生、员工以及各类需要“自我反省”的人士*①使用。

可以选择50字、100字、200字、300字、400字、500字*②

**二、运行环境**
Windows 用户：需要安装 Python 3.6 或以上版本。如果你的系统是 Windows 7 以下，请手动安装 Python 3.6 以上版本（建议使用绿色版，无需管理员权限）。

macOS 和 Linux 用户：系统自带 Python 或通过终端可轻松安装。只需打开终端，运行下列代码即可使用本工具。

**三、下载**

[直接下载](https://itxiaohui.top/upload/%E6%A3%80%E8%AE%A8%E7%94%9F%E6%88%90%E5%99%A8.exe)

**四、哔哩哔哩视频**

**五、Python 代码（复制粘贴即可运行）**

```
import tkinter as tk
from tkinter import messagebox, filedialog
import webbrowser

def generate_self_criticism(name, mistake, reason, improvement_plan, word_count):
   if word_count == 50:
       return f"我叫{name}，因{mistake}而深感抱歉。{reason}。我会改正错误，制定改进计划。"
   elif word_count == 100:
       return (f"尊敬的老师，我是{name}。因{mistake}而感到非常懊悔，"
               f"主要原因是{reason}。我会努力改进，制定明确的计划，争取不再犯同样的错误。")
   elif word_count == 200:
       return (f"尊敬的老师：我叫{name}。最近因{mistake}而感到非常内疚。"
               f"经过反思，我认识到{reason}是我犯错的主要原因。为了避免再次发生，我会制定一个详细的改进计划。"
               f"感谢老师的理解和支持！")
   elif word_count == 300:
       return (f"尊敬的老师：我叫{name}，我想对我的错误{mistake}表示诚恳的歉意。"
               f"经过深思熟虑，我认为{reason}是我导致这一错误的主要原因。这不仅影响了我的学习，也影响了同学们的情绪。"
               f"为了改正错误，我计划通过学习和实践来提升自己，并制定详细的改进计划，确保今后不再犯同样的错误。")
   elif word_count == 400:
       return (f"尊敬的老师：我叫{name}，我为近期的错误{mistake}深感懊悔。"
               f"经过认真反思，我意识到{reason}是我失误的根本原因。我的行为不仅影响了自己的学习成绩，也让同学们感到失望。"
               f"为了弥补我的错误，我将采取积极的措施，认真学习，提升自我。同时，我会与受到影响的同学们沟通，向他们表达我的歉意。")
   elif word_count == 500:
       return (f"尊敬的老师：我叫{name}，在此诚恳地向您检讨我近期所犯的错误。"
               f"最近，我因{mistake}而感到懊悔。经过深刻反思，我认为造成这一错误的主要原因是{reason}。"
               f"这种行为不仅影响了我的学习成绩，也对班级的和谐造成了负面影响。\n\n"
               "在这段时间里，我对自己的行为进行了深刻的反思。回顾事情的经过，"
               "我发现自己在处理问题时缺乏足够的思考和责任感。我没有充分考虑到自己行为可能带来的后果，"
               "以至于让身边的同学和老师感到失望。我深感自责，也对自己的行为感到无比懊悔。\n\n"
               "为了改正这一错误，我计划采取以下措施：首先，我将认真学习相关知识，提升自己的能力，"
               "以便更好地应对今后的挑战。其次，我会向受到影响的同学和老师道歉，表达我的歉意，并努力修复因我错误所造成的影响。"
               f"此外，我会制定一个详细的学习计划，确保自己在学习上不再出现问题，并积极参与班级活动，贡献自己的力量。\n\n"
               "我意识到，仅仅停留在口头上是不够的，实际行动才是最重要的。"
               "我会努力提升自己的自律能力，确保不再犯同样的错误。同时，我也希望能得到老师和同学们的支持与帮助，"
               "在今后的学习生活中，成为一个更优秀的人。\n\n"
               "最后，再次感谢老师的包容和教导，希望能在今后的学习中不断进步，争取成为一个对班级、对社会都有所贡献的人。\n\n"
               "谢谢您的理解与支持！\n\n"
               f"[姓名]")

def on_submit():
   name = entry_name.get()
   mistake = entry_mistake.get()
   reason = entry_reason.get()
   improvement_plan = entry_improvement.get()
   
   word_count = int(word_count_var.get())

   if not name or not mistake or not reason or not improvement_plan:
       messagebox.showwarning("输入错误", "请确保所有字段都已填写。")
       return
   
   criticism = generate_self_criticism(name, mistake, reason, improvement_plan, word_count)
   text_output.delete(1.0, tk.END)
   text_output.insert(tk.END, criticism)

def save_to_txt():
   text = text_output.get(1.0, tk.END).strip()
   if not text:
       messagebox.showwarning("保存错误", "没有可保存的内容。")
       return

   file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt")])
   if file_path:
       with open(file_path, 'w', encoding='utf-8') as file:
           file.write(text)
       messagebox.showinfo("保存成功", f"检讨书已保存到 {file_path}")

def save_to_doc():
   text = text_output.get(1.0, tk.END).strip()
   if not text:
       messagebox.showwarning("保存错误", "没有可保存的内容。")
       return

   file_path = filedialog.asksaveasfilename(defaultextension=".doc", filetypes=[("Word documents", "*.doc")])
   if file_path:
       with open(file_path, 'w', encoding='utf-8') as file:
           file.write(text)
       messagebox.showinfo("保存成功", f"检讨书已保存为 DOC 文档：{file_path}")

def open_author_page():
   webbrowser.open("https://space.bilibili.com/3546654714104666?spm_id_from=333.1007.0.0")

def ds():
   webbrowser.open("https://imgur.la/image/52833926112556c58c1e23f3a8c842d.Pn81U")

# 创建主窗口
root = tk.Tk()
root.title("检讨书生成器 Created on Bilibili: 万能的小慧")

# 创建输入框和标签
tk.Label(root, text="名字：").grid(row=0, column=0)
entry_name = tk.Entry(root)
entry_name.grid(row=0, column=1)

tk.Label(root, text="错误：").grid(row=1, column=0)
entry_mistake = tk.Entry(root)
entry_mistake.grid(row=1, column=1)

tk.Label(root, text="原因：").grid(row=2, column=0)
entry_reason = tk.Entry(root)
entry_reason.grid(row=2, column=1)

tk.Label(root, text="改进计划：").grid(row=3, column=0)
entry_improvement = tk.Entry(root)
entry_improvement.grid(row=3, column=1)

# 创建字数选择
word_count_var = tk.StringVar(value="500")
tk.Label(root, text="字数选择：").grid(row=4, column=0)
tk.OptionMenu(root, word_count_var, "50", "100", "200", "300", "400", "500").grid(row=4, column=1)

# 创建提交按钮
btn_submit = tk.Button(root, text="生成检讨书", command=on_submit)
btn_submit.grid(row=5, columnspan=2)

# 创建保存按钮
btn_save_txt = tk.Button(root, text="保存为TXT", command=save_to_txt)
btn_save_txt.grid(row=6, column=0)

# 创建导出为 DOC 按钮
btn_save_doc = tk.Button(root, text="导出为DOC", command=save_to_doc)
btn_save_doc.grid(row=6, column=1)

# 创建作者主页按钮
btn_author_page = tk.Button(root, text="作者主页", command=open_author_page)
btn_author_page.grid(row=7, columnspan=2)

btn_author_page = tk.Button(root, text="打赏", command=ds)
btn_author_page.grid(row=7, columnspan=1)

# 创建文本输出框
text_output = tk.Text(root, width=50, height=15)
text_output.grid(row=8, columnspan=2)

# 运行主循环
root.mainloop()
```

**六、使用说明**
打开终端或命令提示符（Windows 可使用 PowerShell）。

将以上代码保存为 apology_generator.py。

使用命令运行：`python apology_generator.py`
**七、免责声明**
本工具仅供学习交流使用，生成内容请酌情修改以符合实际情况。作者不承担由使用本工具所引起的任何后果。

*①虽然适合所有人群，但是需要稍做修改

*②套的模板并没有那么多，因为设计时是想着需要输入内容的，所以字数有时会少
