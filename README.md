# 오픈소스 명령어 과제

# 목차

## 1.[getopt & getopts](#getopt-&-getopts)
## 2.[sed](#sed)

---


# getopt & getopts
+ getopt는 옵션을 쉽게 처리하기 위해 만들어진 함수이다.

+ 함수의 원형은 다음과 같다.
	```
	#include<unistd.h>
	int getopt(int argc, char *const argv[], const char *opstring)
	```
	`int argc`: 인수의 개수

	`char *const argv`: 인수의 내용

	`const char *opstring`: 옵션

	+ 함수와 관련된 전역 변수에는 다음과 같은 것들이 있다.
	```
	extern char *optarg
	extern int optind, opterr, optopt;
	```

		+ optarg: 인수를 필요로 하는 옵션을 처리할 때 필요한 인수 포인터

		+ optind: 처리될 옵션의 index이다. 

		+ opterr: 에러 문자열 출력 여부를 결정한다.(0일 경우 출력하지 않는다.)

		+ optopt: 지정되지 않은 옵션을 저장하는 변수이다.

+ getopt()를 반복적으로 호출하게 되면 각각의 옵션인자에서 각각의 옵션 문자들이 성공적으로 반환된다.

**그러나 getopt는 몇가지 문제들을 가지고있었다.**

	1) 인수에서 공백이나 쉘 특수문자(~,.,[] 등등)를 처리할 수 없었다.

	2) 오류 메세지의 출력을 비활성화할 수 있는 기능이 없었다.

이를 보완하고자 나온 것이 **getopts**이다.

+ 사용은 다음과 같다.
	`getopts optstring varname [parameter]`


## * 실행 예제
```
#!/bin/bash
while getopts "ab:" opt
do
	case $opt in
	a)
	echo "I am a pizza"
	;;
	b) b=$OPTARG
	echo "from $b"
	;;
	esac
done
```
+ 출력 결과

![getopt1](https://user-images.githubusercontent.com/93031435/142662411-2f807e26-fec6-47e9-b087-66efa8b5a4f6.PNG)

![getopt2](https://user-images.githubusercontent.com/93031435/142662419-e58a5060-6c30-4282-ac79-787a786abb96.PNG)

![getopt3](https://user-images.githubusercontent.com/93031435/142662429-5c6ab330-ae02-4e4f-be5d-2bac25129505.PNG)


# 2)sed
+ sed 명령어는 원복 텍스트 파일을 편집하는 명령어이다.

+ 원본을 건드리지 않고 편집하기 때문에 작업이 완료되었어도 원본에는 영향이 없다.

## 사용법

+ *sed [옵션] 'command' filename*

+ -n: 원본을 건드리지 않기 때문에 sed로 작업한 부분만 제한해 출력시키려면 -n 옵션을 써야한다.

+ -e: 여러 개의 편집 명령을 수행하게 해준다.

+ -i: i 옵션을 사용하면 원본 파일을 수정할 수 있다.


|설명|예시|
|------|-------|
|[전체 내용 출력](#파일-전체-내용-출력)|'/$/p'|
|[특정 행 출력](#3행-출력)|'np'|
|[n1부터 n2까지 출력](#1번째-행부터-2번째-행까지-출력)|'n1,n2p'|
|[str이 포함된 줄을 삭제하여 출력한다.](#lonely,-라는-단어만-삭제하여-출력)|'str/d'|
|[str이 포함된 줄만 출력한다.](#Two만-포함된-행만-출력)|'/str/!d'|
|[특정 줄에서 str만 삭제한다.](#1행,-5행에서-minus만-삭제한다.)|'n1,n2s/str//g'|
|공백 줄을 삭제한다.|'/^$/d'|
|[str1을 str2로 치환한다](#'Cause를-Because로-치환한다.).|'s/str1/str2/'|
|[str로 시작하는 단어를 포함하는 라인의 문자열(str2)로 치환](#hope로-시작하는-단어를-포함하는-행의-hell로-치환)|'s/^str/str2/'|
|[str로 끝나는 단어를 포함하는 라인의 문자열(str2)로 치환](#one으로-끝나는-단어를-포함하는-라인의-ONE로-치환)|'s/str$/str2/'|
|[문자열 추가](#am으로-끝나는-단어-뒤에-am-a-pizza-I-am-a-pizza-문자열-추가)|a\str|


*sed s와 같이 쓰는 치환 플래그*

|플레그|설명|
|-----|-----|
|g|치환이 행 전체에서 이루어진다.|
|p|행을 출력한다.|


## 출력결과

### 원본파일
---
```
$ cat example.txt
Two minus one
I can see you're doing really good without me, baby
Two minus one
I'm doing great myself
Hope you know I am
'Cause I'm not lonely, lonely, lonely, lonely, yeah
Lonely, lonely, lonely, lonely, yeah
Two minus one
```
### 파일 전체 내용 출력
--- 
```
$ sed -n '/$/p' example.txt
Two minus one
I can see you're doing really good without me, baby
Two minus one
I'm doing great myself
Hope you know I am
'Cause I'm not lonely, lonely, lonely, lonely, yeah
Lonely, lonely, lonely, lonely, yeah
Two minus one
```
### 3행 출력
---
```
$ sed  sed -n '3p' example.txt
Two minus one
```
### 1번째 행부터 2번째 행까지 출력
---
```
$ sed -n '1,2p' example.txt
Two minus one
I can see you're doing really good without me, baby
```

### **lonely,** 라는 단어만 삭제하여 출력
---
```
$ sed '/lonely,/d' example.txt
Two minus one
I can see you're doing really good without me, baby
Two minus one
I'm doing great myself
Hope you know I am
Two minus one
```
### **Two**만 포함된 행만 출력
---
```
$ sed '/Two/!d' example.txt
Two minus one
Two minus one
```
### **1**행, **5**행에서 **minus**만 삭제한다.
---
```
$ sed -n '1,5s/minus//gp' example.txt
Two  one
Two  one
```
### **'Cause**를 **Because**로 치환한다.
---
```
$ sed -n 's/'Cause/Because/p' example.txt
Because I'm not lonely, lonely, lonely, lonely, yeah
```
### **hope**로 시작하는 단어를 포함하는 행의 **hell**로 치환
---
```
$ sed -n 's/^Hope/hell/p' example.txt
hell you know I am
```
### **one**으로 끝나는 단어를 포함하는 라인의 **ONE**로 치환
---
```
$ sed 's/one.$/ONE/g' example.txt
Two minus ONE 
I can see you're doing really good without me, baby 
Two minus ONE 
I'm doing great myself
Hope you know I am
'Cause I'm not lonely, lonely, lonely, lonely, yeah
Lonely, lonely, lonely, lonely, yeah
Two minus ONE
```

### **am**으로 끝나는 단어 뒤에 **am a pizza I am a pizza** 문자열 추가
---
```
$ sed -n -e '5p' -e '/am.$/a\a pizza I am a pizza' example.txt
Hope you know I am
a pizza I am a pizza
```




## 3) **awk**
+ awk는 파일로부터 패턴을 검사해 패턴과 일치하는 값을 얻는 기능을 수행한다.

+ awk는 기본적으로 입력 데이터를 라인 단위의 레코드로 인식한다. 

+ 그리고 각 레코드에 들어 있는 텍스트는 공백 문자로 구분된 필드 들로 분류된다. 

+ 이렇게 식별된 레코드 및 필드의 값들은 awk 프로그램에 의해 다양한 동작의 파라미터로 사용된다.

+ 기본 설정으로 레코드 분리자는 리턴, 필드 분리자는 공백과 탭이며, 전체 필드는 $0, 첫 번째 필드는 $1, 두 번째는 $2, ... 등으로 명명된다.

+ awk는 일정한 규칙을 가지고 있는 데이터를 처리하여 계산, 통계, 비교 분석, 필터링을 통한 데이터 추출 등에 다양하게 사용될 수 있다.

## * 사용법

awk /패턴/ {동작}

|설명|예시(앞에 awk와 뒤의 filename은 생략)|
|------|-------|
|[파일 전체 내용 출력](#파일-전체-내용-출력)|'{print}'|
|[필드 n값 출력](#필드-1-값-출력)|'{print $n}'|
|[str이 있는 줄 출력](#아구찜이-있는-줄-출력)|'/str/'|
|[str로 시작하는  줄 출력](#장어로-시작하-줄-출력)|'/^str/'|
|[필드 수 출력](#필드-수-출력)|'{print NF}'|
|[일치하는 문자 행 출력](#필드-1이-대게나라로-시작한다면-일치하는-행-출력)|'$n == "str" {print $n}'|
|[필드 구분 문자 변경](#필드-구분-문자-\t)|-F':''{print $1, $2}'|
|[특정 필드 합 계산](#필드-2의-값-계산)|'{sum+=$2}END{print sum}'|
|파일로부터 awk 실행|-f awk.script|


## 출력결과

#### 파일 전체 내용 출력
---
```
$ awk '{print}' awk_example.txt
가게이름	가격	위치
탱고아구찜	40000	광주 삼각동
김성용아구찜	28000	광주 문흥동
포미아구찜	15000	목포 하당
해안식당	49000	광주 상무지구
장어나라	29000	광주 문흥동
대게나라	싯가	광주 상무지구
```
### 필드 1 값 출력
---
```
$ awk '{print $1}' awk_example.txt
가게이름
탱고아구찜
김성용아구찜
포미아구찜
해안식당
장어나라
대게나라
```
### 아구찜이 있는 줄 출력
---
```
$ awk '/아구찜/' awk_example.txt
탱고아구찜	40000	광주 삼각동
김성용아구찜	28000	광주 문흥동
포미아구찜	15000	목포 하당
```
### **장어**로 시작하는 줄 출력
---
```
$ awk '/^장어/' awk_example.txt
장어나라        29000   광주 문흥동
```

### 필드 수 출력
---
```
$ awk '{print NF}' awk_example.txt
3 4 0 4 0 4 0 4 0 4 0 4
```
### 필드 1이 **대게나라**로 시작한다면 일치하는 행 출력
---
```
$ awk '$1 == "대게나라" {print $1"\t" $2"\t" $3"\t" $4}' awk_example.txt
대게나라        싯가    광주    상무지구
```
### 필드 구분 문자 \t
---
```
$ awk -F'\t' '{print $1 $2}' awk_example.txt	//필드 분리자
가게이름가격
탱고아구찜40000
김성용아구찜28000
포미아구찜15000
해안식당49000
장어나라29000
대게나라싯가
```
### 필드 2의 값 계산
---
```
$ awk '{sum+=$2}END{print sum}' awk_example.txt
161000
```



