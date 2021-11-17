# 오픈소스 명령어 과제

# 1) **getopt**
> getopt는 옵션을 구분하는데 사용되는 함수이다.

# 2) **getopts**



# 3) **awk**
> awk는 파일로부터 패턴을 검사해 패턴과 일치하는 값을 얻는 기능을 수행한다.

> awk는 기본적으로 입력 데이터를 라인 단위의 레코드로 인식한다. 

> 그리고 각 레코드에 들어 있는 텍스트는 공백 문자로 구분된 필드 들로 분류된다. 

> 이렇게 식별된 레코드 및 필드의 값들은 awk 프로그램에 의해 다양한 동작의 파라미터로 사용된다.

> 기본 설정으로 레코드 분리자는 리턴, 필드 분리자는 공백과 탭이며, 전체 필드는 $0, 첫 번째 필드는 $1, 두 번째는 $2, ... 등으로 명명된다.

> awk는 일정한 규칙을 가지고 있는 데이터를 처리하여 계산, 통계, 비교 분석, 필터링을 통한 데이터 추출 등에 다양하게 사용될 수 있다.

## * 사용법

awk /패턴/ {동작}

|설명|예시(앞에 awk와 뒤의 filename은 생략)|
|------|-------|
|파일 전체 내용 출력|'{print}'|
|필드 n값 출력|'{print $n}'|
|str이 있는 줄 출력|'/str/'|
|str로 시작하는  줄 출력|'/^str/'|
|필드 수 출력|'{print NF}'|
|일치하는 문자 행 출력|'$n == "str" {print $n}'|
|필드 구분 문자 변경|-F':''{print $1, $2}'|
|특정 필드 합 계산|'{sum+=$2}END{print sum}'|
|파일로부터 awk 실행|-f awk.script|


<details> 
<summary>사용예시</summary>
<div markdown="1">

## 출력결과

### $ awk '{print}' awk_example.txt

가게이름	가격	위치

탱고아구찜	40000	광주 삼각동

김성용아구찜	28000	광주 문흥동


포미아구찜	15000	목포 하당

해안식당	49000	광주 상무지구

장어나라	29000	광주 문흥동

대게나라	싯가	광주 상무지구

---

### $ awk '{print $1}' awk_example.txt
가게이름

탱고아구찜

김성용아구찜

포미아구찜

해안식당

장어나라

대게나라

---

### $ awk '/아구찜/' awk_example.txt
탱고아구찜	40000	광주 삼각동

김성용아구찜	28000	광주 문흥동

포미아구찜	15000	목포 하당

---

### $ awk '/^장어/' awk_example.txt
장어나라        29000   광주 문흥동

---

### $ awk '{print NF}' awk_example.txt
3
4
0
4
0
4
0
4
0
4
0
4

---

### $ awk '$1 == "대게나라" {print $1"\t" $2"\t" $3"\t" $4}' awk_example.txt
대게나라        싯가    광주    상무지구

---

### $ awk -F'\t' '{print $1 $2}' awk_example.txt	//필드 분리자
가게이름가격

탱고아구찜40000

김성용아구찜28000

포미아구찜15000

해안식당49000

장어나라29000

대게나라싯가

---

### $ awk '{sum+=$2}END{print sum}' awk_example.txt
161000

---

</div>
</details>

# 4) **sed**
> sed 명령어는 원복 텍스트 파일을 편집하는 명령어이다.

> 원본을 건드리지 않고 편집하기 때문에 작업이 완료되었어도 원본에는 영향이 없다.

## 사용법

> *sed [옵션] 'command' filename*

> -n: 원본을 건드리지 않기 때문에 sed로 작업한 부분만 제한해 출력시키려면 -n 옵션을 써야한다.

> -e: 여러 개의 편집 명령을 수행하게 해준다.

> -i: i 옵션을 사용하면 원본 파일을 수정할 수 있다.


|설명|예시|
|------|-------|
|전체 내용 출력|'/$/p'|
|특정 행 출력|'np'|
|n1부터 n2까지 출력|'n1,n2p'|
|str이 포함된 줄을 삭제하여 출력한다.|'str/d'|
|str이 포함된 줄만 출력한다.|'/str/!d'|
|특정 줄에서 str만 삭제한다.|'n1,n2s/str//g'|
|공백 줄을 삭제한다.|'/^$/d'|
|str1을 str2로 치환한다.|'s/str1/str2/'|
|str로 시작하는 단어를 포함하는 라인의 문자열(str2)로 치환|'s/^str/str2/'|
|str로 끝나는 단어를 포함하는 라인의 문자열(str2)로 치환|'s/str$/str2/'|


*sed s와 같이 쓰는 치환 플래그*

|플레그|설명|
|-----|-----|
|g|치환이 행 전체에서 이루어진다.|
|p|행을 출력한다.|


<details> 
<summary>사용예시</summary>
<div markdown="1">

## 출력결과

### $ cat sed_example.txt
Two minus one

I can see you're doing really good without me, baby

Two minus one

I'm doing great myself

Hope you know I am

'Cause I'm not lonely, lonely, lonely, lonely, yeah

Lonely, lonely, lonely, lonely, yeah

Two minus one

I'm super fine

I don't need you anymore

---

### $ sed -n '/$/p' example.txt
Two minus one

I can see you're doing really good without me, baby

Two minus one

I'm doing great myself

Hope you know I am

'Cause I'm not lonely, lonely, lonely, lonely, yeah

Lonely, lonely, lonely, lonely, yeah

Two minus one

I'm super fine

I don't need you anymore

---

### $ sed  sed -n '3p' example.txt
Two minus one

---

### $ sed -n '1,2p' example.txt
Two minus one

I can see you're doing really good without me, baby


---

### $ sed '/lonely,/d' example.txt
Two minus one

I can see you're doing really good without me, baby

Two minus one

I'm doing great myself

Hope you know I am

Two minus one

I'm super fine

I don't need you anymore

---

### $ sed '/Two/!d' example.txt
Two minus one

Two minus one

Two minus one

---

### $ sed -n '1,5s/minus//gp' example.txt
Two  one

Two  one

---

### $ sed -n 's/'Cause/Because/p' example.txt
Because I'm not lonely, lonely, lonely, lonely, yeah

---

### $ sed -n 's/^Hope/hell/p' example.txt
hell you know I am

---

### $ sed 's/one$/Two/' example.txt
Two minus one

I can see you're doing really good without me, baby

Two minus one

I'm doing great myself

Hope you know I am

'Cause I'm not lonely, lonely, lonely, lonely, yeah

Lonely, lonely, lonely, lonely, yeah

Two minus one

I'm super fine

I don't need you anymore


</div>
</details>


