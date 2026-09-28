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
