# SAP 교육 2주차

## 1주차 REVIEW
1. SAP HISTORY
    - 버전
      > SAP R/2-> SAP R/3 -> SAP ERP(Any DB) -> SAP S/4HANA(Only SAP HANA)(현재)

2.	SAP System(예: SAP ERP)
     - PAS(Primary Application Server)+AAS(Additional…)
               (MS+ENQ W/P)		
		T.code: SM51에서 확인

3. 하나의 AS(Application Server)에 W/P(Work Processes)
	- T.code: SM50
	- W/P종류 : DIA, UPD, UPD2, BGD, SPO, ENQ
	
4. ABAP 프로그램 개발
    - Local Package vs. (Normal) Package

5. SAP이관의 단위(=형상관리 단위) => T.code: SE09
	- Change Request
            -Task 1
            -Task 2
            -Task n
6. ABAP 프로그램 개발 T.code: SE38, SE80

7. 모든 ABAP 프로그램은 딱 2종류
    - Report Program     +    Online(=Screen Module) Program
		  1) Z____orY______
		  2)T.code 생성해봄.
          
8. ABAP 프로그램의 데이터 타입의 종류
	- Primitive Data Type(C,N,X,STRING,P…)
		1-1)Complete Data Type
		1-2) Incomplete Data Type
	- Local Data Type
		>오직 해당 프로그램 안에서만 사용가능
	- Global Data Type
		>모든 프로그램에서 사용가능
		>ABAP Dictionary에 존재함

9. 변수 선언
	- DATA<변수명> TYPE<위에 3개 타입 중 하나>
	  DATA<변수명> LIKE<앞에 선언된 변수명>

10. 상수 선언
	 - CONSTANTS<상수 이름>TYPE<위에 3개 타입 중 하나>

11. 다국어 처리
	 - Text Symbol을 사용함.



# 2주차

## UNIT 9  Using Basic ABAP Statments
##### (pg.257)

### Value Assignmrnts
- MOVE A TO B. OR B = A.
    > B=A 를 많이 사용함
- CLEAR <변수명>. -> 변수의 데이터를 지우는 역할
> - 숫자형 데이터 -> 0. 
  - 문자형 데이터 -> 모든 값을 space로 채움.
  - ex) 
           - DATA age TYPE i.
           AGE - 0. (X)  ----> CLEAR name.(0).        
           - DATA name Type C LENGTH 5.
           Name = ‘     ‘ (x)  ---> CLEAR name. (0)
           - My_name =’Hong’.
           Strlen( My_name ). ---->4개의 리턴값

- SAP는 NULL은 존재하지 않음.



### 사용자가 화면에서 이름을 입력하지 않을 경우
- IF p_name = ‘    ‘ (X)

- IF p_name IS INITIAL.(0)  or IF p_name IS NOT INITIAL

- predefined function
 	 1) Strlen: 문자열 길이 취득
	  2) ABS:절대값
    A:’가나’
    B:’다’
    C= A && B  -> 가나다  BUT) 거의 안쓴다. 
    ABAP 프로그래머
    > CONCATNATE a b into c.

- DO.
	->무제한 루프(탈출을 위해서는 EXIT)

  ENDDO.


- DO<횟수>TIMES.

    ENDDO.


- WHILE <조건>.

    ENDWHILE.


### 시스템 변수 = System Fields외우기 pg.263
- 1)	루프문의 카운트 : SY-INDEX
- 2)	오늘 날짜: SY-DATUM
- 3)	현재 시간: SY-UZEIT
- 4)	로그인한 유저 : SY-UNAME
- 5)	로그인한 언어 : SY-LANGU
- 6)	????????????????: SY-SUBRC
>바로 앞문장에 종속적인 시스템 변수.  
Ex)pg.264:
       - SELECT * FROM xxxxx   
       WRITE:’aaaaaa'.   
        IF sy-subrc = 0. 앞문장에 대한 체크(Write:’aaaaa’)   
        - CALL FUNCTION ‘XZY’
        IF sy-subrc = 0. ->call function을 했으면 if문 아니면 else문   
        ELSE.
        ENDIF.   
        - DELETE FROM SCARR WHERE CARRID = ‘AA’.
        IF sy-subrc = 0. ->삭제했으면 0 아니면 else문
        ELSE.
        ENDID


### zca01_00004.

``` 
** 변수 연습
DATA my_name TYPE C LENGTH 10.

**2)my_name  에 본인 영문 성 저장.
my_name = 'KIM'.

**3)my_name 변수 깨끗히 지우기(sapca로 모두 설정)
Clear my_name.

**4)본인 성 출력하기 -> 포멧: MY name is KIM.
my_name = 'Kim'.
Write:/ 'MY name is ', my_name.


*5)퀴즈 my_name의 길이를 화면에 출렷=>
* 포멧:Name length is 3

Write:/ 'Name Length is', strlen( my_name ).


*1-1)1,2번을 통합해서 화면을 통해 사용자로 부터 입력을 받고자함
PARAMETERS GV_name TYPE C LENGTH 10.
Write:/ 'My name is ', GV_name.
Write:/ 'Name Length is', strlen( GV_name ).

**화면에서 입력한 이름의 크기가 5보다 크면 , "NAME is too Long"출력
**그렇지 않으면 "Name is too short"출력

IF strlen( GV_name ) > 5.
  Write:/ 'NAME is too Long'.
ELSE.
  Write:/'Name is too short'.
ENDIF.

NEW-LINE.

* 이름 출력하는 문장(11번째 라인)을 화면에서 회수를 입력받아서 횟수만큼 출력
PARAMETERS cnt_name TYPE c LENGTH 10.
PARAMETERS cnt_c TYPE i.

IF cnt_name is INITIAL.
  Write:/'Write your name'.
Else.
  DO cnt_c Times.
    Write:/ sy-index, 'Name is', cnt_name.
   ENDDO.
Endif.

write:/ 'Today: ', sy-Datum.
Write:/ 'TIME: ', sy-UZEIT.
Write:/ 'USER: ', sy-UNAME.
Write:/ 'LANGUAGE: ', sy-LANGU.

``` 

### 연습문제 14번 pg.265
 > Program – ZBC400_01_COMPUTE


``` 

REPORT ZPRATICE_01_ZBC_0001.

PARAMETERS: pa_int1 TYPE i,
            pa_int2 TYPE i,
            pa_op TYPE C LENGTH 2.

DATA GV_result TYPE P LENGTH 16 DECIMALS 2.

IF ( pa_op = '+' OR
     pa_op = '-' OR
     pa_op = '*' OR
     pa_op = '/' AND
     pa_int2 <> 0 ).   "<> 0 이 아니라면

  CASE pa_op.
    WHEN '+'.
      GV_result = pa_int1 + pa_int2.
    WHEN '-'.
      GV_result = pa_int1 - pa_int2.
    WHEN '*'.
      GV_result = pa_int1 * pa_int2.
    WHEN '/'.
      GV_result = pa_int1 / pa_int2.
  ENDCASE.
  Write:/ 'RESULT = ', GV_result.

ELSEIF pa_op = '/' AND pa_int2 = 0.
  WRITE: / 'NO division by 0 '(ntl).
ELSE.
   WRITE: / 'Invaild operator '(iop).
*(iop) = text symbol  더블클릭하면 번역할수 있다.
ENDIF.
```


```python
!pip install IPython

```

    Requirement already satisfied: IPython in c:\users\kjwoo\anaconda3\lib\site-packages (7.12.0)
    Requirement already satisfied: traitlets>=4.2 in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (4.3.3)
    Requirement already satisfied: setuptools>=18.5 in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (45.2.0.post20200210)
    Requirement already satisfied: pygments in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (2.5.2)
    Requirement already satisfied: pickleshare in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (0.7.5)
    Requirement already satisfied: jedi>=0.10 in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (0.14.1)
    Requirement already satisfied: backcall in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (0.1.0)
    Requirement already satisfied: prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0 in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (3.0.3)
    Requirement already satisfied: colorama; sys_platform == "win32" in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (0.4.3)
    Requirement already satisfied: decorator in c:\users\kjwoo\anaconda3\lib\site-packages (from IPython) (4.4.1)
    Requirement already satisfied: ipython-genutils in c:\users\kjwoo\anaconda3\lib\site-packages (from traitlets>=4.2->IPython) (0.2.0)
    Requirement already satisfied: six in c:\users\kjwoo\anaconda3\lib\site-packages (from traitlets>=4.2->IPython) (1.14.0)
    Requirement already satisfied: parso>=0.5.0 in c:\users\kjwoo\anaconda3\lib\site-packages (from jedi>=0.10->IPython) (0.5.2)
    Requirement already satisfied: wcwidth in c:\users\kjwoo\anaconda3\lib\site-packages (from prompt-toolkit!=3.0.0,!=3.0.1,<3.1.0,>=2.0.0->IPython) (0.1.8)
    


```python
from IPython.display import Image
Image("img1.PNG")
```




![png](output_10_0.png)



> 번역하는 방법 
*만약 번역 안되어져 있으면 공란 출력
 Write:/ TEXT-NTL
     * 만약 번역 안되어져 있으면 default메세지 출력
     WRITE: / 'NO division by 0 '(ntl).



### Parameters 이름 변경

![img2.PNG](attachment:img2.PNG)

![img3.PNG](attachment:img3.PNG)

![image.png](attachment:image.png)

### MESSAGE 종류
> SE91(pg.270)

![image.png](attachment:image.png)

![image.png](attachment:image.png)

![image.png](attachment:image.png)

MESSAGE E000(ZMC01)
- S: success(초록)
- E:error(빨강) 

=> STATUS BAR에서 확인


### ABAP Debugger  -> 버그 발견 툴
##### (pg.272)

#### Se38 -> BC400_DED_DEBUGGER

![image.png](attachment:image.png)

![image.png](attachment:image.png)

![image.png](attachment:image.png)
> Val 값수정하려면 동그라미 연필 누르면 수정가능

![image.png](attachment:image.png)
 Pg274
- F5: single step (다음라인)   
	->함수 안으로 들어가고 싶다.   
- F6: Execute   
	->함수를 하나의 명령어로 처리   
- F7: Return   
	->함수 안에서 밖으로 나가고싶다   
- F8: 프로그램 끝까지 진행   


*ZBC400_DED_DEBUGGER_01 => BC400_DED_DEBUGGER copy한것
![image.png](attachment:image.png)
> /h 치고 실행하면 버튼누를 때 디버깅.

*WatchPoint(pg276)
-> 특정 변수가 어떠한 값을 갖을 때 프로그램을 세우고 싶을 때.
![image.png](attachment:image.png)
> gv_count가 2인 지점 확인한다(= 2 할때 띄어쓴다).

```
REPORT  zbc400_ded_debugger_01.

CONSTANTS: gc_qf TYPE s_carr_id VALUE 'QF'.
DATA: gv_carrid1 TYPE s_carr_id,
      gv_carrid2 TYPE s_carr_id VALUE 'LH',
      gv_count   TYPE i.

BREAK-POINT. "무조건 해당 위치에서 디버깅.

MOVE gc_qf TO gv_carrid1.
gv_carrid2 = gv_carrid1.

ADD 1 TO gv_count.
gv_count = gv_count + 1.

*whatchpoint를 통해 gv_count = 2일때 프로그램을 세워 봄.
CLEAR: gv_carrid1,
       gv_carrid2,
       gv_count.

WRITE: 'END'
```

# UNIT 10. Modularization 

##### pg.291

- Module
> - 목적: 반복적으로 사용되는 로직을 하나의 Module로 정의 후 반복적으로 재사용함    
> - Database 처리 부분은 보통 Module로 구현함.(가급적)


* SE11 통해 DB확인가능
![image.png](attachment:image.png)

### Open SQL 문법
> Open SQL문법   
> SELECT <원하는 컬럼> INTO <변수>    
> From<테이블>     
> Where<조건>     
> - SELECT SINGLE * INTO RESULT      
>  FROM SCARR      
>  WHERE CARRID = P_ID      
> >   * SINGLE 붙이는 이유는 CARRID = P_ID 이 조건에 데이터가 하나 또는 없기 때문에.



## Module 종류
- 1. Local Module      
> 외부 module 사용 불가능.     
 Subroutine & local class      

- 2. Global Module
> 외부 module 사용 가능.
  Function group & global class
 


## SUBROUTINE
###### pg.300

##### Parameter 전송방법

- 1. Call By Value
> Formal parameter에 영향을미치고      
 Actual Parameter에는 영향 X      
 ![image.png](attachment:image.png)
 
 ### VS.       


- 2. Call By Reference 
> Formal parameter에 영향을미치고      
  Actual Parameter에도 영향을 미친다.      
  ![image.png](attachment:image.png)
 


![image.png](attachment:image.png)

> Actual Parameter -> 데이터를 보내준다     
> Formal Parameter -> 데이터를 보관하는장소.

#### EX) 항공사 2개 불러오기

(CODE)
```
PARAMETERS p_id TYPE scarr-carrid.
PARAMETERS p_two TYPE scarr-carrid.
DATA result TYPE scarr. "첫번째 항공사 담을곳 항공사ID, 항공사 이름, 항공사 화폐단위, 항공사URL
DATA two_result TYPE scarr. "두번쨰 항공사 담을곳
*ZCA01_00004폴더안에 subroutines폴더안에 CARRIER 가져오기
PERFORM get_carrier(zca01_00004)
            USING p_id      "Actual Parameter
            CHANGING result. "Actual Parameter


IF result IS INITIAL.
  WRITE: / 'NOT FOUND CARRID!!'.
ELSE.
  PERFORM write_carrier USING result.
ENDIF.

***********************************************************

PERFORM get_carrier(zca01_00004)
            USING
                p_two
            CHANGING
               two_result.

IF two_result IS INITIAL.
  WRITE: / 'NOT FOUND CARRID!!'.
ELSE.
  PERFORM write_carrier USING two_result.
ENDIF.

FORM get_carrier USING VALUE(p_air) TYPE scarr-carrid "call by value
                  CHANGING c_result TYPE scarr. "call by reference
  SELECT SINGLE * INTO c_result
  FROM scarr
  WHERE carrid = p_air.
ENDFORM.

*서브루틴 정의
*<-- 파라미터 1개 있어야함 (call by value)
FORM write_carrier USING VALUE(p_result) TYPE scarr.
  WRITE:/'ID2: ', p_result-carrid.
  WRITE:/'NAME2: ', p_result-carrname.
  WRITE:/'CURR2: ', p_result-currcode.
  WRITE:/'URL2: ', p_result-url.
ENDFORM.

```


#### 변수선언 Naming Rule (Pg304)

- Global variables(전역변수)     
>GV_xxxxxxxx: 나이, 이름      
>GS_xxxxxxxx: 스트렉처      
예: DATA: gs_result type scarr       

- Local variables(지역변수)          
>LV_xxxxxxxx      
LS_xxxxxxxx


### 연습문제 16
###### pg.309

#### COMPUTE_SUBROUTINE

```
PARAMETERS: pa_int1 TYPE i,
            pa_int2 TYPE i,
            pa_op TYPE C LENGTH 2.

DATA GV_result TYPE P LENGTH 16 DECIMALS 2.

IF ( pa_op = '+' OR
     pa_op = '-' OR
     pa_op = '*' OR
     pa_op = '/' OR
     pa_op = '%' AND
     pa_int2 <> 0 ).   "<> 0 이 아니라면

  CASE pa_op.
    WHEN '+'.
      GV_result = pa_int1 + pa_int2.
    WHEN '-'.
      GV_result = pa_int1 - pa_int2.
    WHEN '*'.
      GV_result = pa_int1 * pa_int2.
    WHEN '/'.
      GV_result = pa_int1 / pa_int2.
    WHEN '%'.
      PERFORM CALC_PERCENTAGE USING pa_int1
                                    pa_int2
                              CHANGING gv_result.

  ENDCASE.
  Write:/ 'RESULT = ', GV_result.

ELSEIF pa_op = '/' AND pa_int2 = 0.
  write:/'NO DIVISION BY ZERO !!!'.
ELSE.
  Write:/ 'INVALID OPERATOR'.
ENDIF.
*********************************************************

*서브루틴 정의 

FORM CALC_PERCENTAGE USING pv_act
                           pv_max
                    CHANGING cv_result.


  IF pv_act = 0.
      pv_max = 0.
      Write:/ 'ERROR'.
  ELSE.
      cv_result = pv_act / pv_max * 100.
  ENDIF.

ENDFORM.


```


## FUNCTION MODULE함수
##### pg.315

- Interface = parameter      
> import       
export        
changing       
exception       
tables    


#### 함수 만들기

#### 1) Function Group 만들기       
![image.png](attachment:image.png)

#### 1) 함수 그룹안에 함수 만들기       
- 그룹이 달라도 이름이 같으면 에러
![image.png](attachment:image.png)

![image.png](attachment:image.png)

### 3)Import 생성
- TYPING
![image.png](attachment:image.png)

![image.png](attachment:image.png)

>Optional 체크하면 데이터를 보내도 되고 안보내도 된다.

![image.png](attachment:image.png)

> optional 체크& default value= ‘+’      
 ->아무것도 작성 안하면 기본값 + 로 계산된다      

> Pass by ..-> 체크 : call by value        
		Non-check: call by Reference


### 4)Export 생성

![image.png](attachment:image.png)

### 5)Source code 작성 후 실행.

![image.png](attachment:image.png)

##### * 항공사 코드 상세정보 함수만들기
![image.png](attachment:image.png)


- Exception(예외처리)

![image.png](attachment:image.png)

![image.png](attachment:image.png)
>source code에 추가 

### Pattern 버튼을 통해 함수를 불러올수 있다.
##### pg.322

![image.png](attachment:image.png)

> export 와 import는 반대로 나온다

#### Test 통해 데이터 저장해 사용가능

![image.png](attachment:image.png)

### 연습문제
##### pg.325

#### Call a Function Module (Task2)
```

PARAMETERS pa_int1 Type i.
PARAMETERS pa_int2 Type i.
PARAMETERS pa_op TYPE C.
DATA gv_result TYPE p LENGTH 16 DECIMALS 2.

*<> -> 같지 않으면.
IF ( pa_op = '+' OR
    pa_op = '-' OR
    pa_op = '*' OR
    pa_op = '/' OR
    pa_op = '%' OR
    pa_op = 'P' AND
    pa_int2 <> 0 ).

  CASE pa_op.
    WHEN '+'.
      gv_result = pa_int1 + pa_int2.
    WHEN '-'.
      gv_result = pa_int1 - pa_int2.
    WHEN '*'.
      gv_result = pa_int1 * pa_int2.
    WHEN '/'.
      gv_result = pa_int1 / pa_int2.
    WHEN '%'.
      PERFORM CALC_PERCENTAGE USING pa_int1
                                    pa_int2
                             CHANGING gv_result.

   WHEN 'P'.
     CALL FUNCTION 'BC400_MOS_POWER'
       EXPORTING
         iv_base                    = pa_int1
        IV_POWER                    = pa_int2
      IMPORTING
        EV_RESULT                   = gv_result
      EXCEPTIONS
        POWER_VALUE_TOO_HIGH        = 1
        RESULT_VALUE_TOO_HIGH       = 2
        OTHERS                      = 3
               .
    CASE sy-subrc.
      When 1.
        Write:/ 'Max value of power is4'.
      when 2.
        write:/ 'Result value too high'.
      when 3.
        write:/'unknown Error'.
    ENDCASE.


  ENDCASE.

WRITE:/ 'RESULT:',gv_result.

ELSEIF pa_op = '/' AND pa_int2 = 0.
   WRITE: / 'NO division by 0 '(ntl).
ELSE.
   WRITE: / 'Invaild operator '(iop).
*(iop) = text symbol  더블클릭하면 번역할수 있다.
ENDIF.

***********************************************

FORM CALC_PERCENTAGE USing PV_ACT
                           PV_MAX
                     CHANGING CV_RESULT.


 IF PV_ACT = 0.
   cv_result = 0.
   WRITE:/ 'ERROR'.

 ELSE.
   cv_result = pv_act / pv_max * 100.
 ENDIF.

 ENDFORM.

```

### 연습문제
##### pg.335

```
PARAMETERS pa_int1 Type i.
PARAMETERS pa_int2 Type i.
PARAMETERS pa_op TYPE C.
DATA gv_result TYPE p LENGTH 16 DECIMALS 2.

*<> -> 같지 않으면.
IF ( pa_op = '+' OR
    pa_op = '-' OR
    pa_op = '*' OR
    pa_op = '/' OR
    pa_op = '%' OR
    pa_op = 'P' AND
    pa_int2 <> 0 ).

  CASE pa_op.
    WHEN '+'.
      gv_result = pa_int1 + pa_int2.
    WHEN '-'.
      gv_result = pa_int1 - pa_int2.
    WHEN '*'.
      gv_result = pa_int1 * pa_int2.
    WHEN '/'.
      gv_result = pa_int1 / pa_int2.


   WHEN 'P'.
     CALL FUNCTION 'BC400_MOS_POWER'
       EXPORTING
         iv_base                    = pa_int1
        IV_POWER                    = pa_int2
      IMPORTING
        EV_RESULT                   = gv_result
      EXCEPTIONS
        POWER_VALUE_TOO_HIGH        = 1
        RESULT_VALUE_TOO_HIGH       = 2
        OTHERS                      = 3
               .
    CASE sy-subrc.
      When 1.
        Write:/ 'Max value of power is4'.
      when 2.
        write:/ 'Result value too high'.
      when 3.
        write:/'unknown Error'.
    ENDCASE.


    WHEN '%'.
      CALL FUNCTION 'BC400_MOS_PERCENTAGE'
        EXPORTING
          iv_act                 = pa_int1
          iv_max                 = pa_int2
       IMPORTING
         EV_PERCENTAGE          = gv_result
       EXCEPTIONS
         DIVISION_BY_ZERO       = 1
         OTHERS                 = 2.
      IF sy-subrc <> 0.
        Write:/ 'Error in Function Module'.
      ENDIF.



  ENDCASE.

WRITE:/ 'RESULT:',gv_result.

ELSEIF pa_op = '/' AND pa_int2 = 0.
   WRITE: / 'NO division by 0 '(ntl).
ELSE.
   WRITE: / 'Invaild operator '(iop).
*(iop) = text symbol  더블클릭하면 번역할수 있다.
ENDIF.


```

### QUIZ)
- *화면에 출력할 내용      
	고객ID, 고객 이름, 국가 , 전화번호, 이메일 -> table: SCUSTOM       
	함수이용        
	프로그램 이름:ZCA01_00006       
	함수 이름: ZGET_CUSTOMER_01       
    
    
    
```
REPORT ZCA01_00006.

PARAMETERS p_id Type S_CUSTOMER.

DATA GS_RESULT TYPE SCUSTOM.

CALL FUNCTION 'ZGET_CUSTOMER_01'
  EXPORTING
    i_cusid         = p_id
 IMPORTING
   E_RESULT        = GS_RESULT
 EXCEPTIONS
   NOT_FOUND       = 1
   OTHERS          = 2
          .
IF sy-subrc <> 0.
  CASE sy-subrc.
    WHEN'1'.
      WRITE: 'ERROR'.
    When OTHERS.
  ENDCASE.
ELSE.
  WRITE: / 'ID' , GS_RESULT-ID.
  WRITE: / 'NAME' , GS_RESULT-NAME.
  WRITE: / 'COUNTRY' , GS_RESULT-COUNTRY.
  WRITE: / 'TELEPHONE' , GS_RESULT-TELEPHONE.
  WRITE: / 'EMAIL' , GS_RESULT-EMAIL.

ENDIF.

```

* Function 
-ZGET_CUSTOMER_01

```
FUNCTION ZGET_CUSTOMER_01.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     REFERENCE(I_CUSID) TYPE  S_CUSTOMER
*"  EXPORTING
*"     REFERENCE(E_RESULT) TYPE  SCUSTOM
*"  EXCEPTIONS
*"      NOT_FOUND
*"----------------------------------------------------------------------

SELECT single * INTO E_RESULT
FROM SCUSTOM
WHERE id = I_CUSID.

IF sy-subrc <> 0.
   RAISE NOT_FOUND.
 ENDIF.


ENDFUNCTION.
```








