# 附录A. I386手册勘误

17.2.1 Table 17-3:

```diff
@@ -?,2 +?,2 @@
 disp8[EDX]           010   42    4A    52    5A    62    6A    72    7A
-disp8[EPX]           011   43    4B    53    5B    63    6B    73    7B
+disp8[EBX]           011   43    4B    53    5B    63    6B    73    7B
```

17.2.1 Table 17-4:

```diff
@@ -?,2 +?,2 @@
    Base =                   0     1     2     3     4     5     6     7
-   r32                      EAX   ECX   EDX   EBX   ESP   EBP   ESI   EDI
+   r32                      EAX   ECX   EDX   EBX   ESP   [*]   ESI   EDI
@@ -?,2 +?,2 @@
 [ECX*2]              001    48    49    4A    4B    4C    4D    4E    4F
-[ECX*2]              010    50    51    52    53    54    55    56    57
+[EDX*2]              010    50    51    52    53    54    55    56    57
@@ -?,2 +?,2 @@
 [EDX*4]              010    90    91    92    93    94    95    96    97
-[EBX*4]              011    98    89    9A    9B    9C    9D    9E    9F
+[EBX*4]              011    98    99    9A    9B    9C    9D    9E    9F
@@ -?,2 +?,2 @@
 NOTES:
- [*] means a disp32 with no base if MOD is 00, [ESP] otherwise. 
  This provides the following addressing modes:
+ [*] means a disp32 with no base if MOD is 00. Otherwise, [*] means disp8[EBP] or disp32[EBP]. 
  This provides the following addressing modes:
```

17.2.2.11 Instruction Set Detail中的DEC -- Decrement by 1

```diff
@@ -?,2 +?,2 @@
 FF /1     DEC r/m16          2/6      Decrement r/m word by 1
-          DEC r/m32          2/6      Decrement r/m dword by 1
+FF /1     DEC r/m32          2/6      Decrement r/m dword by 1
```

17.2.2.11 Instruction Set Detail中的INC -- Increment by 1

```diff
@@ -?,2 +?,2 @@
 FF  /0      INC r/m16                      Increment r/m word by 1
-FF  /6      INC r/m32                      Increment r/m dword by 1
+FF  /0      INC r/m32                      Increment r/m dword by 1
```

17.2.2.11 Instruction Set Detail中的Jcc -- Jump if Condition is Met

```diff
@@ -?,2 +?,2 @@
 72  cb         JB rel8     7+m,3    Jump short if below (CF=1)
-76  cb         JBE rel8    7+m,3    Jump short if below or (CF=1 or ZF=1)
+76  cb         JBE rel8    7+m,3    Jump short if below or equal (CF=1 or ZF=1)
@@ -?,2 +?,2 @@
 7C  cb         JL rel8     7+m,3    Jump short if less (SF!=OF)
-7E  cb         JLE rel8    7+m,3    Jump short if less or equal (ZF=1 and SF!=OF)
+7E  cb         JLE rel8    7+m,3    Jump short if less or equal (ZF=1 or SF!=OF)
@@ -?,2 +?,2 @@
 0F  8C cw/cd   JL rel16/32   7+m,3    Jump near if less (SF!=OF)
-0F  8E cw/cd   JLE rel16/32  7+m,3    Jump near if less or equal (ZF=1 and SF!=OF)
+0F  8E cw/cd   JLE rel16/32  7+m,3    Jump near if less or equal (ZF=1 or SF!=OF)
```

17.2.2.11 Instruction Set Detail中的MOV -- Move Data

```diff
@@ -?,7 +?,7 @@
 A3           MOV moffs32,EAX   2             Move EAX to (seg:offset)
-B0 + rb      MOV reg8,imm8     2             Move immediate byte to register
-B8 + rw      MOV reg16,imm16   2             Move immediate word to register
-B8 + rd      MOV reg32,imm32   2             Move immediate dword to register
-Ciiiiii      MOV r/m8,imm8     2/2           Move immediate byte to r/m byte
-C7           MOV r/m16,imm16   2/2           Move immediate word to r/m word
-C7           MOV r/m32,imm32   2/2           Move immediate dword to r/m dword
+B0 + rb ib   MOV reg8,imm8     2             Move immediate byte to register
+B8 + rw iw   MOV reg16,imm16   2             Move immediate word to register
+B8 + rd id   MOV reg32,imm32   2             Move immediate dword to register
+C6 ib        MOV r/m8,imm8     2/2           Move immediate byte to r/m byte
+C7 iw        MOV r/m16,imm16   2/2           Move immediate word to r/m word
+C7 id        MOV r/m32,imm32   2/2           Move immediate dword to r/m dword
```

17.2.2.11 Instruction Set Detail中的MUL -- Unsigned Multiplication of AL or AX

```diff
@@ -?,2 +?,2 @@
 Flags Affected
-OF and CF as described above; SF, ZF, AF, PF, and CF are undefined
+OF and CF as described above; SF, ZF, AF, PF are undefined
```

17.2.2.11 Instruction Set Detail中的PUSH -- Push Operand onto the Stack

```diff
@@ -?,3 +?,3 @@
 FF   /6    PUSH m32      5        Push memory dword
-50 + /r    PUSH r16      2        Push register word
-50 + /r    PUSH r32      2        Push register dword
+50 + rw    PUSH r16      2        Push register word
+50 + rd    PUSH r32      2        Push register dword
```

17.2.2.11 Instruction Set Detail中的SETcc - Byte Set on Condition

```diff
@@ -?,2 +?,2 @@
 0F  94   SETE r/m8    4/5     Set byte if equal (ZF=1)
-0F  9F   SETG r/m8    4/5     Set byte if greater (ZF=0 or SF=OF)
+0F  9F   SETG r/m8    4/5     Set byte if greater (ZF=0 and SF=OF)
@@ -?,3 +?,3 @@
 0F  9C   SETLE r/m8   4/5     Set byte if less (SF!=OF)
-0F  9E   SETLE r/m8   4/5     Set byte if less or equal (ZF=1 and SF!=OF)
-0F  96   SETNA r/m8   4/5     Set byte if not above (CF=1)
+0F  9E   SETLE r/m8   4/5     Set byte if less or equal (ZF=1 or SF!=OF)
+0F  96   SETNA r/m8   4/5     Set byte if not above (CF=1 or ZF=1)
@@ -?,2 +?,2 @@
 0F  9D   SETNL r/m8   4/5     Set byte if not less (SF=OF)
-0F  9F   SETNLE r/m8  4/5     Set byte if not less or equal (ZF=1 and SF!=OF)
+0F  9F   SETNLE r/m8  4/5     Set byte if not less or equal (ZF=0 and SF=OF)
```
<p align="right">——感谢16级许翔、闫雨呼同学的细心发现</p>

17.2.2.11 Instruction Set Detail中的SHLD -- Double Precision Shift Left

```diff
@@ -?,2 +?,2 @@
 Flags Affected
-OF, SF, ZF, PF, and CF as described above; AF and OF are undefined
+OF, SF, ZF, PF, and CF as described above; AF is undefined
```

17.2.2.11 Instruction Set Detail中的SHLR -- Double Precision Shift Right

```diff
@@ -?,2 +?,2 @@
 Flags Affected
-OF, SF, ZF, PF, and CF as described above; AF and OF are undefined
+OF, SF, ZF, PF, and CF as described above; AF is undefined
```

Appendix A. Opcode Map 中的 One-Byte Opcode Map

```diff
0x68 的位置应对应于 PUSH Iv

-0x68: PUSH Ib
+0x68: PUSH Iv

```
<p align="right">——感谢16级张航帆同学提供的信息</p>

Appendix A. Opcode Map 中的 Two-Byte Opcode Map (First byte is 0FH)

```diff
0x20和0x22对应的两条MOV指令的操作数顺序写反了

-0x20: MOV Cd, Rd
+0x20: MOV Rd, Cd

-0x22: MOV Rd, Cd
+0x22: MOV Cd, Rd
```



