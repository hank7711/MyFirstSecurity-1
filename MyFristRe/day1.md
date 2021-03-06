https://github.com/ss8651twtw/CTF/tree/master/site/csie.ctf.tw/practice


https://tlk.io/reverse


# tmux

sudo apt install tmux

tmux

# week_1

Topic_1_Basic tools

Topic_2_Assembly Language

Topic_3_gdb


# 1.Basic tools

>* file and strings
>* lstrace and strace
>* objdump
>* packing and unpacking


## Basic tools---file and strings


## 範例練習:EasyCTF IV_hexedit

https://github.com/mohamedaymenkarmous/CTF/blob/master/EasyCTF_IV/resources/reverse_engineering-50-hexedit

## 範例練習:EasyCTF 2017_ Hexable

https://github.com/easyctf/easyctf-2017-problems/blob/master/hexable-autogen/description.md

## Basic tools---strace

## 範例練習:strace

```
wget 'https://github.com/ss8651twtw/CTF/tree/master/site/csie.ctf.tw/practice/strace'
chmod +x strace
./strace
strace ./strace
strace -s 40 ./strace
```
## Basic tools---objdump

## 範例練習:
```
objdump -M intel -d ./search | grep -A 2 'mov BYTE PTR [rip+0x.*],0x4d' | grep 0x79

取出關鍵欄位 ==> cat out | awk '{print $8}'

hex to string ==> http://www.convertstring.com/zh_TW/EncodeDecode/HexDecode
```
# 2.Assembly Language

解讀關鍵組合語言(一)
>* mov
>* add、sub、imul、idiv、and、or、xor
>* inc、dec、neg、not

>* cmp
>* jmp
>* ja、jb、jna、jbe、je、jne、jz

http://www.felixcloutier.com/x86/Jcc.html


## 範例練習:組合語言的解讀

```
.global main

main:
    mov eax, XXXXXXX
    mov ebx, 0
    mov ecx, 0x5
    
loop:
    test eax, eax
    jz fin
    add ebx, ecx
    dec eax
    jmp loop
    
fin:
    cmp ebx, 0x7ee0
    je good
    mov eax, 0
    jmp end
good:
    mov eax, 1
end:
    ret
```

```
eax = xxxxxxx
ebx = 0
ecx = 5


while (eax != 0) {
	ebx += ecx
	eax--
}

if (ebx == 0x7ee0) {
	// good
	eax = 1
	return
}
else {
	eax = 0
	return
}

```
## 範例練習: picoCTF 2017Programmers Assemble
題目為 AT&T 格式

可參考下一頁 Intel 格式的題目

## 範例練習: Reverse CTF_read-asm
```
global _start

section .data
n dd 10

section .text
foo:
  push rbp
  mov rbp, rsp
  sub rsp, 0x10
  mov qword [rbp - 0x8], rdi
  cmp qword [rbp - 0x8], 1
  jne L1
  mov rax, 1
  jmp L2

L1:
  mov rbx, qword [rbp - 0x8]
  dec rbx
  mov rdi, rbx
  call foo
  imul rax, qword [rbp - 0x8]

L2:
  mov rsp, rbp
  pop rbp
  ret

_start:
  mov rax, n
  mov rdi, [rax]
  call foo

  mov rax, 60
  mov rdi, 0
  syscall
```



## 解讀關鍵組合語言(二)

>* push、pop



## 範例練習:picoCTF 2017_A Thing Called the Stack

## 開發組合語言:NASM

## 範例練習:Reverse CTF_run-asm
```
global _start

section .text
_start:
  mov rax, 1
  mov rdi, 1
  push 0x7d214e75
  push 0x525f646e
  push 0x345f6d53
  push 0x615f6531
  push 0x69706d30
  push 0x437b4654
  push 0x43747372
  push 0x6946794d //M(4d)y(79)Fi  
  mov rsi, rsp
  mov rdx, 0x40
  syscall

  mov rax, 60
  mov rdi, 0
  syscall
```

```
下載原始組合語言solve.asm
$ nasm -f elf64 solve.asm
$ ld -m elf_x86_64 -o solve solve.o
$ ./solve
```
## 範例練習:組合語言練習

git clone https://github.com/ss8651twtw/asm-practice.git

## 範例練習:EasyCTF IV_adder
```
$ file 查看題目檔案類型
$ strings 找出一些可視字串
如果是執行檔就執行看看
靜態分析執行檔---objdump 反組譯看組合語言
動態分析執行檔---GDB/IDA pro 分析
```

分析執行檔---objdump 反組譯看組合語言

objdump -M intel ./adder

找main

找 add 指令附近的組合語言

找 cmp指令附近的組合語言

cmp eas 0x539
0x539==?
使用python快速解



## 範例練習:EasyCTF IV_liar

## 範例練習:EasyCTF IV_ezreverse



## 從高階語言到組合語言| FromHIGHtoLOW |從C語言到組合語言


# 3.GDB 

## gdb

## gdb套件安裝

peda 安裝 https://github.com/longld/peda

Pwngdb 安裝 https://github.com/scwuaptx/Pwngdb

## gdb使用技術

https://henrybear327.gitbooks.io/gitbook_tutorial/content/Linux/GDB/index.html

## 範例練習:EasyCTF IV_adder|動態分析執行檔---GDB/IDA pro 分析

設定斷點與執行

方法一:先將成是執行到cmp比較之前再使用下列指令修改register的值

set $eax=0x539
print $eax

繼續執行下去就可以得到答案


x <memory address>

x/d $rbp-0xc

方法二:先將成是執行到cmp比較之前再直接使用jmp到成功之後的地方繼續執行



## 範例練習: EasyCTF 2017 LuckyGuess
