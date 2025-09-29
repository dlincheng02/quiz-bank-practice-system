# quiz-bank-practice-system
This is a simple practice system I wrote for a certificate. You can choose how many questions you want to do, and it will show you how many questions you got right at the end.
import json
import random

# 載入題庫
with open("questions_full.json", "r", encoding="utf-8") as f:
    questions = json.load(f)

print("📘 人工智慧題庫隨機測驗系統")
num_questions = int(input("請輸入要測驗的題數： "))

# 隨機抽題
quiz = random.sample(questions, num_questions)
score = 0

for i, q in enumerate(quiz, 1):
    print(f"\n第 {i} 題：{q['question']}")

    # 單選題 & 複選題
    if q["type"] in ["single", "multiple"]:
        for idx, opt in enumerate(q["options"], 1):
            print(f"{idx}. {opt}")

        ans = input("請輸入答案（複選用逗號分隔）： ")

        if q["type"] == "single":
            # 單選
            if q["options"][int(ans) - 1] == q["answer"]:
                print("✅ 正確")
                score += 1
            else:
                print(f"❌ 錯誤，正確答案：{q['answer']}")

        else:
            # 複選
            user_ans = [q["options"][int(x) - 1] for x in ans.split(",")]
            if set(user_ans) == set(q["answer"]):
                print("✅ 正確")
                score += 1
            else:
                print(f"❌ 錯誤，正確答案：{q['answer']}")

    # 是非題
    elif q["type"] == "truefalse":
        ans = input("請輸入答案（O=對 / X=錯）： ")
        if (ans.upper() == "O" and q["answer"]) or (ans.upper() == "X" and not q["answer"]):
            print("✅ 正確")
            score += 1
        else:
            print(f"❌ 錯誤，正確答案：{'O' if q['answer'] else 'X'}")

# 測驗結束
print("\n🎉 測驗結束！")
print(f"你的分數：{score} / {num_questions}")
