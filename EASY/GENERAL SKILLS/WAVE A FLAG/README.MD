
Wave a flag — step-by-step solution

Challenge: Wave a flag

Author: syreal

Description: Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...

Steps (copy-paste ready)

1.	Download the binary
wget https://mercury.picoctf.net/static/f95b1ee9f29d631d99073e34703a2826/warm -O warm

2.	Make it executable
chmod +x warm

3.	Inspect the file (optional but useful)
file warm
ls -l warm

4.	Try common help flags (this one-liner tries multiple common variants)
for f in -h --help -? help -help --usage; do
  echo "=== $f ==="
  ./warm $f 2>&1 | sed -n '1,200p'
done

5.	If the flag appears in output, extract it directly
(you can also search the binary for embedded flags)
./warm -h | grep -oE 'picoCTF\{[^}]+\}'

# or

strings warm | grep -oE 'picoCTF\{[^}]+\}'
What happened when these were run
Running the for loop produced the help output; the program responds to -h with the flag and rejects --help. The exact output for -h included the flag.
Flag (final)
picoCTF{b1scu1ts_4nd_gr4vy_f0668f62}
If you want, I can format this into a short README or a one-line command you can paste into your writeup.
