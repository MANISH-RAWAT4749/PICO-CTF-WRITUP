🧩 Challenge Write-up: Bytemancy 0

Category: General Skills

Level: Easy
________________________________________
🔍 Challenge Description
We are given a remote service and a Python source file (app.py).
The program asks:
“Send me ASCII DECIMAL 101, 101, 101, side-by-side, no space.”
________________________________________
🧠 Step 1: Analyze the Source Code
Key condition in the code:
if user_input == "\x65\x65\x65":
________________________________________
🔬 Step 2: Understand the Condition
•	\x65 = hexadecimal value
•	Hex 65 = decimal 101
•	ASCII 101 = character 'e'
So:
"\x65\x65\x65" = "eee"
________________________________________
🧩 Step 3: Interpret the Question
The prompt says:
•	ASCII decimal 101 → 'e'
•	“side-by-side” → concatenate
•	“no space” → no separators
👉 Therefore:
101 101 101 → e e e → eee
________________________________________
⚡ Step 4: Send the Correct Input
Method 1 (Manual)
nc candy-mountain.picoctf.net 57443
Enter:
eee
________________________________________
Method 2 (One-liner as hinted)
echo -n "eee" | nc candy-mountain.picoctf.net 57443
________________________________________
⚠️ Why eee Works
•	The program directly compares strings
•	It expects the exact byte sequence:
•	\x65 \x65 \x65
•	These bytes represent:
•	e e e
•	Any variation (spaces, numbers, newline) will fail
________________________________________
❌ Common Mistakes
•	101101101 → wrong (numbers, not ASCII chars)
•	e e e → wrong (spaces included)
•	echo "eee" → adds newline (\n)
________________________________________
🎯 Final Result
picoCTF{pr1n74813_ch4r5_4daf27d8}
________________________________________
🧠 Key Learning
•	ASCII conversion (decimal ↔ character)
•	Hex escape sequences (\xNN)
•	Exact input matching in programs
•	Importance of byte-level precision
________________________________________

