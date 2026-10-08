```
# AI辅助编程实录
## 1. 任务与提示词
要做什么：读取生词表CSV，挑出HSK4级词语，统计词性，生成造句练习题
我写的提示词：
>写Python代码读取csv生词表，筛选HSK4词汇，统计每种词性数量，输出txt造句题，格式：用“词语”造一个句子。（词性）

## 2. AI 初版代码
```python
import csv
def load_words(path):
    with open(path) as f:
        return list(csv.DictReader(f))

def filter_by_level(words, level=4):
    return [w for w in words if w["HSK等级"] == level]

def count_by_pos(words):
    d = {}
    for w in words:
        if w["词性"] in d:
            d[w["词性"]] +=1
        else:
            d[w["词性"]] =1
    return d

def gen_exercises(words, out_path):
    with open(out_path,"w") as f:
        for w in words:
            f.write(f"用\"{w['词汇']}\"造一个句子。（{w['词性']}）\n")

if __name__ == "__main__":
    words = load_words("生词表.csv")
    lv4 = filter_by_level(words,4)
    print(count_by_pos(lv4))
    gen_exercises(lv4,"练习.txt")
```

##我的修改点
① 在所有open()函数增加encoding="utf-8"。
原因：AI原版没有编码设置，Windows电脑运行中文会出现乱码，加上编码保证中文正常显示。

② count_by_pos函数，把if‑else判断key是否存在，改成dict.get()写法，并添加注释。
原因：简化字典计数代码，练习字典get方法这个知识点。

③ 使用weekpath工具管理文件路径，不再直接写死文件名。
原因：硬编码文件名，换文件夹运行程序就找不到文件；weekpath可以让代码任意位置都能运行。

④ 修改txt练习题输出格式，更换题目句式，使用【】标记词语。
原因：让题目排版更醒目，区分原版输出效果，学生做题更容易定位重点词语。

##最终版vs初版差异说明
1. AI初版open没有utf-8编码，中文容易乱码；我添加encoding="utf-8"解决中文乱码。
2. AI原版count_by_pos使用if判断字典key；我替换为dict.get()写法，代码更加简洁。
3. AI初版写死文件路径；我接入weekpath工具，统一管理文件位置，代码可移植性更好。
4. AI原版输出句子格式老旧；我改写输出文本样式，用方括号突出词语，生成出来txt文件肉眼可见不一样。
```
