---
title: "[크롤러] 개발 환경 설정"

categories:
  - Crawler
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true
show_date: true
read_time: false
use_math: true

date: 2021-09-27
last_modified_at: 2021-09-27
sitemap :
  changefreq : daily
  priority : 1.0
---

## 개발 환경 설정  

📌**<u>개발 환경</u>**
```
OS = Window and Mac (사무공간과 휴대용으로 둘 다 사용합니다.)
Python = 3.8 (Selenium)
IDE : Pycharm
PyQt
Qt Designer
```

### 1. Selenium 설치

사전에 Python은 설치되어야 하고, Mac 사용자 중 conda를 이용하고 있는 분들은 가상 환경으로 Python version 지정해서 진행.  

```
cmd 창 실행 -> pip install selenium
```  

python 환경 변수 설정이 완료되어 있어서 pip 명령어를 사용해 selenium을 설치 해줍니다.  
설치가 성공적으로 완료되는지 확인하고 넘어갑시다.  

### 2. ChromeDriver 설치  

우선, 개발 완성을 목적으로 하기에 현재 사용하고 있는 Chrome 버전을 확인해줍니다.  
추후, 자동으로 Chrome Driver를 설치해주는 라이브러리를 이용해 수정할 예정입니다. (배포 시 사용자의 크롬 버전이 다 다를 수 있기 때문.)  

#### 1) 크롬 버전 확인  

- 설정을 클릭합니다.  
![image](https://user-images.githubusercontent.com/37467408/134831911-c64cc68c-7299-4b16-9d5d-b3fdb78d9e09.PNG)  

- Chrome 정보를 클릭합니다.  
![image](https://user-images.githubusercontent.com/37467408/134832014-ce4b6e05-8dcf-418f-bf15-9052571b7554.PNG)  

- Chrome 정보에서 Chrome의 버전을 확인합니다.  

![image](https://user-images.githubusercontent.com/37467408/134832154-d211be4a-7065-44ca-81d6-284585e5b003.PNG)  

#### 2) Chrome Driver 다운  

- 다운로드 사이트로 이동합니다.  
[다운로드 사이트](https://chromedriver.chromium.org/downloads)  

- 위에서 확인한 Chrome 버전에 일치하는 Chrome Driver를 다운로드 받습니다. (버전 : 94.0.4606.61)  

![image](https://user-images.githubusercontent.com/37467408/134832620-85643e1d-36a4-4096-abc5-b533c0049aa5.PNG)  

- 자신의 OS에 맞는 Chrome Driver을 설치해줍니다.  

- 압축을 해제한 Chrome Driver을 사용할 곳에 위치시킵니다. 저같은 경우는 Window는 파이썬 프로젝트 폴더에 Mac은 어디서나 사용할 수 있도록 폴더를 만든 후, 해당 폴더로 접근하도록 했습니다.  

#### 3) 정상 설치 테스트  

- 사용하시는 IDE를 실행시킨 후, 테스트 코드를 입력할 파이썬 파일을 만듭니다.  

- 아래 코드를 입력해 사이트가 열리는지 확인해봅니다.  

```python
from selenium import webdriver

driver = webdriver.Chrome('./chromedriver.exe')
url = "https://www.naver.com/"
driver.get(url)
```

- 정상적으로 사이트가 열리면 Selenium과 Chrome Driver가 정상적으로 설치 되었습니다.  


### 3. PyQt5 설치  
해당 프로젝트에서는 UI도 다룰 예정이므로, PyQt5를 설치해줍니다.  
PyQt5 License가 `GPL v3`이므로 상업적 목적으로 사용이 가능하나 코드를 공개해야 합니다.  
설치방법이 `pip 명렁어`, `pycharm 에서 설치`가 있으나 해당 프로젝트 IDE를 Pycharm을 사용하므로 pycharm에서 설치하도록 합니다.  

```
File -> Settings -> Project: 프로젝트 이름 -> Python Interpreter -> pyqt5 검색 -> Install package
```  

File -> Setting를 클릭한다.  
![image](https://user-images.githubusercontent.com/37467408/134833963-893f892b-cb0d-4153-b470-4d135d75f9d0.PNG)  

Project: 프로젝트 이름 -> Python Interpreter -> + 버튼 클릭  
![image](https://user-images.githubusercontent.com/37467408/134834067-7d778cd0-1f51-45c2-991b-a38760500ce6.PNG)  

PyQt5 검색 -> Install Package 클릭  
![image](https://user-images.githubusercontent.com/37467408/134834135-a710dcee-8c02-449a-8513-ea55eeda5c3b.PNG)  

설치 확인  
![image](https://user-images.githubusercontent.com/37467408/134834388-f82ef318-c8c4-45a1-b7cf-a83c24d73a3c.PNG)  

### 4. Qt Designer 설치  

위에서 Pycharm을 이용한 설치방법을 설명했으니, 아래는 `pip` 명렁어를 이용해 설치해보자.  

`pip install pyqt5designer` 를 cmd창에 입력해 설치하도록 하자.  

설치 후, Pycharm External Tool에 등록하자. (자주 사용되므로)

```
File -> Settings -> Tools -> External Tools -> + 버튼 클릭  
```  

아래와 같이 추가해줍니다.  

Qt Designer  
- Name : Qt Designer  
- Program : 파일 경로  
- Arguments : $FilePath$  
- Working directory : $ProjectFileDir$  

![image](https://user-images.githubusercontent.com/37467408/134835514-62f88770-6f13-4583-bdd7-b43f899e49f2.PNG)  

PyUIC  
- Name : PyUIC  
- Program : 파일 경로  
- Arguments : $FileName$ -o $FileNameWithoutExtension$.py  
- Working directory : $ProjectFileDir$  

![image](https://user-images.githubusercontent.com/37467408/134835569-b8a9ccd4-c21a-4190-915e-a56270d32a8c.PNG)  

PyRcc  
- Name : PyRcc  
- Program : 파일 경로  
- Arguments : $FileName$ -o $FileNameWithoutExtension$_rc.py  
- Working directory : $ProjectFileDir$  

![image](https://user-images.githubusercontent.com/37467408/134835638-92274086-20a4-436a-89dd-d90ca2a32379.PNG)  

설치 확인은 아래와 같이 나오면 됩니다.  

![image](https://user-images.githubusercontent.com/37467408/134836232-c0504d80-efb6-49c4-a192-4854da250eaf.PNG)  

---
**🐢 현재 개발하고 있는 웹크롤러의 개발 과정을 기록하기 위한 공간입니다. 또한 해당 코드는 상업적 목적으로 사용될 예정이지만 PyQt5 GPL v3에 따라 코드는 공개됩니다. 🐢**
{: .notice--primary}

감사합니다.😊
