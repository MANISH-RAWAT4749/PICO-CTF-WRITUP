Obedient Cat — Steps
1.	Challenge name
o	Obedient Cat
2.	Download the file
wget https://mercury.picoctf.net/static/2d24d50b4ebed90c704575627f1f57b2/flag
3.	Verify file type
file flag
# -> flag: ASCII text
4.	Open / view the file
cat flag
# or
nano flag
5.	Read the flag (the exact text inside the file)
picoCTF{s4n1ty_v3r1f13d_f28ac910}
6.	(Optional) Submit the flag
# If the CTF site accepts CLI submissions (example), replace <submit-command> appropriately:
# example (site-specific): curl -X POST -d "flag=picoCTF{...}" https://<ctf-site>/submit
# Usually you submit via the picoCTF web interface.
If you want, I can format the submission command for whatever interface you're using (picoCTF web vs some CLI), or prepare a one-line script that prints + saves the flag.
