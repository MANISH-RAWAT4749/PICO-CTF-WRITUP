Hidden in plainsight — 
Challenge: Hidden in plainsight
Author: Yahaya Meddy
Below are the steps you ran — arranged cleanly and in order — with the exact commands and what each step does. Use this as a concise write-up or checklist.
________________________________________
1) See files in the directory
ls
# output: img.jpg  server.log
Purpose: confirm the target file (img.jpg) is present.
________________________________________
2) Quick file-type check (look for JPEG comment)
file img.jpg
# output includes: comment: "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9"
Purpose: file reveals the JPEG comment which contains the hidden hint (Base64).
________________________________________
3) Read full metadata (confirm comment)
exiftool img.jpg
# shows Comment : c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
Purpose: exiftool confirms and displays the comment field (explicit hint from the challenge).
________________________________________
4) Decode the JPEG comment (Base64) — first layer
echo 'c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9' | base64 -d
# output: steghide:cEF6endvcmQ=
Purpose: first Base64 decode reveals steghide: and another Base64 token.
________________________________________
5) Decode the inner Base64 to get the steghide passphrase
echo 'cEF6endvcmQ=' | base64 -d
# output: pAzzword
Purpose: obtain the passphrase pAzzword used to encrypt the embedded file.
________________________________________
6) Install steghide (if not already installed)
sudo apt update
sudo apt install steghide -y
Purpose: tool required to extract the hidden payload.
________________________________________
7) Inspect steghide info (confirm embedded file metadata)
steghide info img.jpg
# Enter passphrase: (type pAzzword)
# output:
# embedded file "flag.txt":
#   size: 34.0 Byte
#   encrypted: rijndael-128, cbc
#   compressed: yes
Purpose: confirm there is an embedded file (flag.txt) and its properties.
________________________________________
8) Extract the embedded file with the discovered passphrase
steghide extract -sf img.jpg -p 'pAzzword'
# output: wrote extracted data to "flag.txt".
Purpose: steghide decrypts, decompresses, and writes the embedded flag.txt.
________________________________________
9) Verify and read the flag
ls -l flag.txt
file flag.txt
cat flag.txt
# output:
# -rw-r--r-- 1 kali kali 34 Oct 29 10:49 flag.txt
# flag.txt: ASCII text
# picoCTF{h1dd3n_1n_1m4g3_5d4cba73}
Purpose: ensure the file exists and display the flag.
________________________________________
Final result
Flag: picoCTF{h1dd3n_1n_1m4g3_5d4cba73}
________________________________________

