picoCTF challenge 245
________________________________________

🧩 Challenge Write-up: MultiCode

📌 Challenge Name : MultiCode

🏷️ Category :
General Skills

⚡ Difficulty :
Easy
________________________________________
📖 Description
We are given a file (message.txt) containing an encoded string.
The hint suggests that the flag is hidden behind multiple layers of encoding such as:
•	Base64
•	Hex
•	URL Encoding
•	ROT13
________________________________________
🔍 Step-by-Step Solution
🥇 Step 1: View the encoded message

cat message.txt

Output:
NjM3NjcwNjI1MDQ3NTMyNTM3NDI2MTcyNjY2NzcyNzE1ZjcyNjE3MDMwNzE3NjYxNzQ1ZjczNzM2ZjM2NzA2ZTZlMzIyNTM3NDQ=
👉 This looks like Base64 (because of = at the end)
________________________________________
🥈 Step 2: Decode Base64

echo "NjM3NjcwNjI1MDQ3NTMyNTM3NDI2MTcyNjY2NzcyNzE1ZjcyNjE3MDMwNzE3NjYxNzQ1ZjczNzM2ZjM2NzA2ZTZlMzIyNTM3NDQ=" | base64 -d

Output:
637670625047532537426172666772715f72617030717661745f73736f36706e6e32253744
👉 This is Hexadecimal
________________________________________
🥉 Step 3: Decode Hex using xxd

echo "637670625047532537426172666772715f72617030717661745f73736f36706e6e32253744" | xxd -r -p

📌 What is xxd?
•	xxd is a Linux tool used for hex dump and reverse hex conversion
•	-r → reverse (hex → text)
•	-p → plain hex format
Output:
cvpbPGS%7Barfgrq_rap0qvat_sso6pnn2%7D
👉 This looks like URL encoding (%7B, %7D)
________________________________________
🏅 Step 4: URL Decode using Python

python3 -c "import urllib.parse; print(urllib.parse.unquote('cvpbPGS%7Barfgrq_rap0qvat_sso6pnn2%7D'))"

📌 Why Python?
We use Python’s urllib.parse.unquote() to decode URL-encoded strings.
Output:
cvpbPGS{arfgrq_rap0qvat_sso6pnn2}
👉 This looks like ROT13 encoded text
________________________________________
🏆 Step 5: Apply ROT13 using tr

echo "cvpbPGS{arfgrq_rap0qvat_sso6pnn2}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
📌 What is tr?
•	tr = translate characters
•	It replaces characters from one set with another
•	Here:
o	A-Z → N-ZA-M
o	a-z → n-za-m
👉 This performs ROT13 cipher
________________________________________
🎉 Final Flag
picoCTF{nested_enc0ding_ffb6caa2}
________________________________________

🔁 Encoding Chain Used
Base64 → Hex → URL Decode → ROT13
________________________________________
💡 Tips 
•	= at end → Base64
•	Only 0-9a-f → Hex
•	%xx → URL encoding
•	Weird readable text → ROT13
________________________________________
🚀 Bonus (One-liner solve)
echo "NjM3NjcwNjI1MDQ3NTMyNTM3NDI2MTcyNjY2NzcyNzE1ZjcyNjE3MDMwNzE3NjYxNzQ1ZjczNzM2ZjM2NzA2ZTZlMzIyNTM3NDQ=" | base64 -d | xxd -r -p | python3 -c "import sys,urllib.parse; print(urllib.parse.unquote(sys.stdin.read()))" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
________________________________________

