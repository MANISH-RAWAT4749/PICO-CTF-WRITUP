picoCTF challenge 
_______________________________________

🏆 Challenge Writeup

📌 Challenge Name : 
Shared Secrets

📂 Category : 
Cryptography

📊 Level : 
Easy

✍️ Author : 
Yahaya Meddy
________________________________________
📖 Description : 
A message was encrypted using a shared secret, but one side of the key exchange leaked its private value. Using this leak, we must reconstruct the shared secret and decrypt the message.
________________________________________
📁 Given Files
📄 message.txt
g = 2

p=2353855297163603230718659874235969189211904801854831749272164556125452734594903161934794163403497762726605985627409234420965176613218858624400366466853770681834062810872998733968827992223178685800438220675248064587881077258421057484096470778462916510705650691145000008812988897851274637295629476960165825128891910793

A=30068494220298681891330724690865821714005524540061986470484739235070782982875821399659951687122702703103707881496322805008910484300535488856317760253374747778591137899431437661178126794196697625682849937450966265318752778435469929326325997632296225043026449906879570022308070668814741550246868893784772044251768510

b=2252298426168643058908675994457951125000150972241848576912471697630407791150123415344081881294633730616725212738161375429295138914772118107750191302567546774258844274932453145325172167340764005882111243473134993891638369747707408050760320040529699904616084248365714753162172475940998916650267854128138430013641052250

enc = 839a909cb0a7b588979bac80c09081c087acc291c1c696c2ca958e
________________________________________
📄 encryption.py
from Crypto.Util.number import getPrime
from random import randint

# Public parameters
g = 2
p = getPrime(1048)

# Server's secret
a = randint(2, p-2)
A = pow(g, a, p)

# Client secret
b = '???'  

B = pow(g, b, p)

# Shared key
shared = pow(A, b, p)

# Encrypt flag
flag = b"picoCTF{...}"
enc = bytes([x ^ (shared % 256) for x in flag])
________________________________________
🧠 Concept Used
This challenge is based on
👉 Diffie–Hellman key exchange
Key Idea:
•	Shared secret =
[
shared = A^b \mod p
]
________________________________________
🚨 Vulnerability
•	The private key b is leaked
•	Encryption uses:
•	shared % 256
•	XOR encryption is weak
👉 So we can directly compute the secret and decrypt
________________________________________
⚙️ Steps to Solve

✅ Step 1: Identify values
From message.txt:
•	A → public key
•	b → leaked private key
•	p → modulus
•	enc → encrypted flag
________________________________________
✅ Step 2: Compute shared secret
[
shared = A^b \mod p
]
________________________________________
✅ Step 3: Derive key
key = shared % 256
________________________________________
✅ Step 4: Decrypt using XOR
Each byte:
plaintext = ciphertext XOR key
________________________________________
🧪 Solution Code
from binascii import unhexlify

# Given values
p = 2353855297163603230718659874235969189211904801854831749272164556125452734594903161934794163403497762726605985627409234420965176613218858624400366466853770681834062810872998733968827992223178685800438220675248064587881077258421057484096470778462916510705650691145000008812988897851274637295629476960165825128891910793

A = 30068494220298681891330724690865821714005524540061986470484739235070782982875821399659951687122702703103707881496322805008910484300535488856317760253374747778591137899431437661178126794196697625682849937450966265318752778435469929326325997632296225043026449906879570022308070668814741550246868893784772044251768510

b = 2252298426168643058908675994457951125000150972241848576912471697630407791150123415344081881294633730616725212738161375429295138914772118107750191302567546774258844274932453145325172167340764005882111243473134993891638369747707408050760320040529699904616084248365714753162172475940998916650267854128138430013641052250

enc_hex = "839a909cb0a7b588979bac80c09081c087acc291c1c696c2ca958e"

# Step 1: Shared secret
shared = pow(A, b, p)

# Step 2: Key
key = shared % 256

# Step 3: Decrypt
cipher = unhexlify(enc_hex)
flag = bytes([c ^ key for c in cipher])

print(flag.decode())
________________________________________
❓ Why This Code Works
🔹 pow(A, b, p)
•	Efficient modular exponentiation
•	Computes:
[
A^b \mod p
]
________________________________________
🔹 shared % 256
•	Encryption only uses last 8 bits
•	So we extract 1-byte key
________________________________________
🔹 unhexlify(enc_hex)
•	Converts hex → raw bytes
________________________________________
🔹 XOR Decryption
bytes([c ^ key for c in cipher])
•	Reverses encryption because:
[
(x \oplus k) \oplus k = x
]
________________________________________
🏁 Final Flag
picoCTF{dh_s3cr3t_1b25e19f}
________________________________________
💡 Key Takeaways
•	Never leak private keys in Diffie-Hellman ❌
•	XOR with small key = weak encryption ❌
•	If private key is known → shared secret is trivial ✅
________________________________________






