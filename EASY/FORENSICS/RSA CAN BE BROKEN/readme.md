RSA CAN BE BROKEN

Steps (in order)
Step 1 — Connect & copy values
nc verbal-sleep.picoctf.net 58069
Copy N, e, and ciphertext from the service output.
Step 2 — Notice the flaw
Check that N is even → that means one prime is p = 2 and q = N // 2.
Step 3 — Compute φ(N)
For N=p⋅qN = p \cdot q with p=2p=2:
φ(N)=(p−1)(q−1)=1⋅(q−1)=q−1\varphi(N) = (p-1)(q-1) = 1 \cdot (q-1) = q-1.
Step 4 — Compute the private exponent d
d = e^{-1} mod φ(N) (modular inverse).
Step 5 — Decrypt
m = pow(ciphertext, d, N).
Step 6 — Convert integer → bytes → text
Turn m into bytes and decode to get the flag.
________________________________________
One-file Python solve (paste & run)
# Even RSA solver (no external libraries needed)

N = 18076173452061870911936863913087196842701035927734139572016245206750590550408522408701234653729314951292575669167430436230447101132616423737178622778565598
e = 65537
c = 7079887381013264293756893897220240496115562466502264912076389988048396711369172108697805282412558551214973567703697364190621643884891479844779785406706487

# 1) Factor the even modulus
assert N % 2 == 0, "N should be even in this challenge"
p = 2
q = N // p

# 2) Euler's totient
phi = (p - 1) * (q - 1)  # = q - 1

# 3) Private exponent (Python 3.8+ supports pow with negative exponent for mod inverse)
d = pow(e, -1, phi)

# 4) Decrypt
m = pow(c, d, N)

# 5) Convert to bytes/string (strip leading 0x00 if any)
k = (N.bit_length() + 7) // 8
pt_bytes = m.to_bytes(k, 'big').lstrip(b'\x00')
try:
    print(pt_bytes.decode())
except UnicodeDecodeError:
    # fallback: print raw bytes
    print(pt_bytes)
Expected output (your flag):
picoCTF{tw0_1$_pr!m33991588e}
