# Challenge: asm1

* **Category:** Reverse Engineering
* **Level:** Medium
* **Points:** 200

## Steps to Solve

1. Download the assembly source file:

```bash
wget https://challenge-files.picoctf.net/c_fickle_tempest/2495cb38ce287ca8ea254d897188445d80871d1385e00fb7fa65dd5904747b41/test.S
```

2. View the assembly code:

```bash
cat test.S
```

3. Identify the function input:

```c
asm1(0x3ef)
```

4. Convert the hexadecimal value:

```text
0x3ef = 1007
```

5. Analyze the first comparison:

```asm
cmp DWORD PTR [ebp+0x8],0x6e6
jg  <asm1+38>
```

* `0x6e6 = 1766`
* `1007` is not greater than `1766`
* The jump is NOT taken.

6. Analyze the second comparison:

```asm
cmp DWORD PTR [ebp+0x8],0x8
jne <asm1+30>
```

* `1007 != 8`
* Jump to `<asm1+30>`

7. Execute the instructions at `<asm1+30>`:

```asm
mov eax,DWORD PTR [ebp+0x8]
sub eax,0x9
```

Calculation:

```text
0x3ef - 0x9 = 0x3e6
```

## Final Flag

```text
0x3e6
```
