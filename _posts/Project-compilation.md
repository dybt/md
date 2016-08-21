title: 汇编小程序
date: 2016-02-28 11:27:21
tags:
---
#### 需求
+ 编写一个程序。 先从键盘输入一个字符串（有英文字母，可能也有数字），然后显示其中数字符的个数、英文字母的个数和字符串的长度；字符串中不能有空格，若有将其删除，并将改变后的字符串按相反的顺序显示出来；输入第二个字符串，显示的时全都以‘*’显示，然后将输入的字符串与前面处理后的字符串比较是否相同，若相同，输出“Password Right!”，否则输出 “Password Error!”。
+ 样例 ![](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/55533731.jpg)
<!-- more -->

#### 分析
+ 个人在处理输入原始的密码并计算其中的字母和数字的长度时，所用的方法是单个字符输入（调用中断），每次输入都检查是否是回车，是否是空格，是否是数字或是字母；
+ 在输出长度时，如果长度小于10，可以直接将二进制表示与‘0’相加再用字符的形式输出，但是当长度大于10就行不通了，要把十位和个位分离出来；所用的方法是将长度去除以10，这样div会直接把个位和十位分开；

#### 实现代码
```
DATAS SEGMENT
    MSG  DB      "Please input a string:",  '$'
    MSG1 DB      "The number of alphabet:", '$'
    MSG2 DB      "The number of digit:", '$'
    MSG3 DB      "The length of string: ",'$'
    MSG4 DB      "The opposite direction string: " ,'$'
    MSG5 DB      "Please input password:",'$'
    MSG6 DB      "The result:", '$'
    MSG7 DB      "Password Right",'$'
    MSG8 DB      "Password Error", '$'
    DIG_NUM DB    0  ;数字长度
    ALP_NUM DB    0   ;字母长度
    BLANK_NUM DB  0   ;空格个数
    FLAG DB       '*', '$'
    
    PASSWORD DB 30 DUP ('$')  ;密码内容与长度
    PASSWORD_LEN DB 0
     
    TEST_PASS DB 30 DUP ('$') ;输入的测试密码内容与长度
    TEST_PASS_LEN DB 0
    
    CRLF DB 0DH, 0AH, '$' ;回车,换行 结束
DATAS ENDS

STACKS SEGMENT
    DB 8 DUP (0)
    DB 8 DUP (0)
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES, DS:DATAS, SS:STACKS
START: ;PROC FAR ;主函数 
    MOV AX, DATAS
    MOV DS, AX
    MOV AX, STACKS
    MOV SS,AX
    ;MOV AX, CODES  ;怎么回事
    ;MOV CS,AX
    
    LEA DX, MSG
    MOV AH, 9
    INT 21H
    ;输入并处理密码,处理指去空格并同时计算各种长度
    LEA DI, PASSWORD
    CALL SET_AND_DEAL_POSSWORD
    
    LEA DX, CRLF
    MOV AH, 9
    INT 21H
    ;输出各种长度
    LEA DX, MSG1
    MOV AH, 9
    INT 21H
    
    MOV AL, ALP_NUM
    MOV AH, 0
    CALL OUTPUT_LEN
    
    LEA DX, CRLF
    MOV AH, 9
    INT 21H
    
    LEA DX, MSG2
    MOV AH, 9
    INT 21H
    
    MOV AL, DIG_NUM
    MOV AH, 0
    CALL OUTPUT_LEN
    
    LEA DX, CRLF
    MOV AH, 9
    INT 21H
    
    LEA DX, MSG3
    MOV AH ,9
    INT 21H
    
    MOV AL, PASSWORD_LEN
    ADD AL, BLANK_NUM
    MOV AH, 0
    CALL OUTPUT_LEN
    
    LEA DX, CRLF
    MOV AH, 9
    INT 21H
    ;输出真正的字符串,有空格就反序输出
    LEA DX, MSG4
    MOV AH, 9
    INT 21H
    
    ;LEA DX, PASSWORD
    ;MOV AH, 9
    ;INT 21H
    
    LEA SI, PASSWORD
    CALL OUTPUT_PASSWORD
    
    LEA DX, CRLF
    MOV AH, 9
    INT 21H
    
    ;输入密码
    LEA DX, MSG5
    MOV AH, 9
    INT 21H
    
    LEA DI, TEST_PASS
    CALL INPUT_PASSWORD
    
    LEA DX, CRLF
    MOV AH, 9
    INT 21H
    
    ;测试密码
    ;输入参数
    LEA SI, TEST_PASS
    LEA DI, PASSWORD
    MOV AL, [PASSWORD_LEN]
    MOV AH, [TEST_PASS_LEN]
    
    CALL IS_RIGHT
    
    LEA DX,CRLF
    MOV AH,9
    INT 21H
    
    MOV AH,4CH
    INT 21H

;******************* SET_AND_DEAL_POSSWORD ***************
SET_AND_DEAL_POSSWORD PROC NEAR
  SO:
    MOV AH, 1
    INT 21H
    CMP AL, 0DH ;输入回车结束
    JNZ NOT_END
    MOV AH, '$'
    MOV [DI], AH ;输入字符串结束符
    RET
    
  NOT_END:
    CMP AL, ' '
    JZ BLANK_CASE
    
    MOV [DI], AL ;把数据装入密码中
    INC DI
    
    CMP AL, 'A'; 'A' = 65, 'a' = 97, '9' = 57
    JB  DIGIT_CASE ; 数字的情况
    JMP ALPHABEG_CASE ;字母的情况
    
  BLANK_CASE:
    INC BLANK_NUM
    JMP SO
    
  DIGIT_CASE:
    INC DIG_NUM
    INC PASSWORD_LEN
    JMP SO
    
  ALPHABEG_CASE:
    INC ALP_NUM
    INC PASSWORD_LEN
    JMP SO

SET_AND_DEAL_POSSWORD ENDP

;*************** OUTPUT_LEN ************
OUTPUT_LEN PROC NEAR
    MOV BL, 10
    DIV BL
    
    MOV BH, AH ;保存余数
    
    MOV DL, AL
    CMP DL, 0
    JZ OL_S1  ;如果十位为0不输出
    ADD DL, '0'
    MOV AH, 2
    INT 21H
    
  OL_S1:
    MOV DL, BH ;得到个位
    ADD DL, '0'
    MOV AH, 2
    INT 21H
    RET
    
OUTPUT_LEN ENDP
;**************************
OUTPUT_PASSWORD PROC NEAR
    CMP BLANK_NUM, 0
    JNZ OP_S1 ;反序
    ;ELSE
    MOV DX, SI
    MOV AH, 9
    INT 21H
    RET
    
  OP_S1:
    MOV DL, PASSWORD_LEN
    MOV DH, 0
    ADD SI, DX ;从后面开始
    DEC SI
    ;按字符输出
    MOV CL, PASSWORD_LEN
    MOV CH, 0
  OP_S2:
    MOV DL, [SI]
    MOV AH, 2
    INT 21H
    DEC SI
    LOOP OP_S2
    
    RET
OUTPUT_PASSWORD ENDP

;********************** INPUT_PASSWORD **************
INPUT_PASSWORD PROC NEAR
    S2:
    MOV AH, 8
    INT 21H
    CMP AL, 0DH ; 判断是不是输入换行符
    JNZ IP_S3
    MOV AH, '$'
    MOV [DI], AH
    RET
    
  IP_S3:
    MOV [DI], AL;[si]是当前地址，相当于c语言中的*pt = value; 
    INC DI
    INC [TEST_PASS_LEN]
    MOV DL, '*'
    MOV AH, 2
    INT 21H
    JMP S2
    
INPUT_PASSWORD ENDP

;*************************
IS_RIGHT PROC NEAR
    CMP AL, AH
    JZ S3
    LEA DX, MSG8 ;长度不相等
    MOV AH, 9
    INT 21H
    RET
    
     S3:
    MOV CL, PASSWORD_LEN
    MOV CH, 0
    
   S4: 
    MOV AL, [SI]
    MOV AH, [DI]
    INC SI
    INC DI
    CMP AL, AH
    JNZ S5
    LOOP S4
    ;right
    LEA DX, MSG7
    MOV AH,9
    INT 21H
    RET
  S5:
    ;error
    LEA DX, MSG8
    MOV AH,9
    INT 21H
    RET

IS_RIGHT ENDP
;********
CODES ENDS
END START
```