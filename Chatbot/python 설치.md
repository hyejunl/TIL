## 1. python 설치

path 설정 check 박스 해주고 다운로드

다운로드 완료 후에 아래거 눌러주고 완료!!

바탕화면에서 bash로 버전 확인

```bash
$ python -V
Python 3.8.0
```

## 2. open with code로 들어가기

`ctrl + shift + p`  하고 shell 입력

아래것 선택하고 거기서 `Git Bash` 눌러주기

ctrl + `  눌러서 terminal 열기

```bash
$ python -m venv venv #venv라는 가상환경 만들기
$ source venv/Scrips/activate #venv라는 가상환경에 진입
```

테트리스 모양 눌러서 python치고 맨 위에 있는 아이 설치해주기

`ctrl + shift + p`  하고 interpreter 치고,  python interpreter 눌러주기

venv 어쩌구 저쩌구 click

```bash
$ deactivate #가상환경 탈출하는 방법
```

## 3. gitignore 만들어 주기

[gitignore](gitignore.io)

`flask` `visualstudiocode` `venv` 로 생성

bash로 돌아가서

```bash
$ vi .gitignore # gitignore 파일 생성
```

`i`누르고 아까 생성된 code 복붙하고 

`esc`하고 `:wq`

## 4. upgrade pip

```bash
$ pip list
$ python -m pip install --upgrade pip
```

### 기타 turminal 조작

```bash
$ cd .. # 상위폴더로 가는 것
$ cd Chatbot/ # Chatbot 폴더로 가는 것
```

---

# Python 문법 +

## 1. 문자열 삽입 (문자열 포매팅)

* 파이썬 3.0 이상 -> **format 함수를 이용한 포매팅**
* 파이썬 3.6 이상 -> **f-string 포매팅**

```python
# format 함수
'{1} {0}'.format('one','two')

# f-string
a, b = 'one', 'two'
f'{a}, {b}'
```



 

