---
title: "[WebScraping] PyQt5 GUI(QPushButton, QLabel) 사용 방법"

categories:
  - WebScraping
tags:
  - [Crawler, Python, Selenium, PyQt5, Qt Designer]

toc:  true
toc_sticky: true

date: 2022-01-10
last_modified_at: 2022-01-10
---

📌**<u>개발 환경</u>**  
```
OS = Window and Mac
Python = 3.8 (Selenium)
IDE : Pycharm
PyQt
Qt Designer
```  

---
📌**<u>전체코드는 https://github.com/dltjrals2/WebScrapingTool 에 있으니, 참고해주세요.</u>**  

## PyQt5 QPushButton 사용 방법  

`QPushButton`의 시그널  
- self.QPushButton.clicked.connect(함수)  

> 프로젝트 내 사용 예시  

```python
# 예제 1 : QPushButton을 눌러 함수 실행
self.ui.QPushButton1.clicked.connect(self.dataUpdate)
self.ui.QPushButton2.clicked.connect(self.RefreshFunc)
```

## PyQt5 QPushButton 동작 함수  

```python
targetUrl = "URL Address"

class MainWindow(QtWidgets.QDialog):
    def __init__(self):
        super().__init__()
        self.ui = uic.loadUi(BASE_DIR + r'\DataSelect.ui')
        self.ui.UpdateButton.clicked.connect(self.dataUpdate)
        self.ui.RefreshButton.clicked.connect(self.RefreshFunc)

        # ... Init ...

        self.ui.extractButton.clicked.connect(self.dataExtractFunc)

    def dataUpdate(self):
        time.sleep(2)
        # 로그인 정보 유무 파악
        if os.path.isfile("./user_info.txt"): # 로그인 정보 파일이 존재 하는지 파악
            with open("./user_info.txt", "r") as login_file:
              login_info = login_file.readlines() # txt 파일 내용 전부 읽어오기
              for i in range(len(login_info)):
                  login_info[i] = login_info[i].replace("\n", "") # 개행 문자("\n")를 공백으로 처리

              # user_info.txt 파일 형식
              # 0, 1 > 로그인 성공 여부
              # ID
              # Password

              if login_info[0] == "0": # 로그인 실패
                  self.ui.StatusLabel.setText("로그인에 실패하였습니다.\n로그인 버튼을 클릭해주세요.")
                  return

              # ID and Password Info
              self.LoginID = login_info[1] # txt 중 ID
              self.LoginPassword = login_info[2] # txt 중 Password

        driver.get(targetUrl) # Selenium으로 Web 데이터 가져오기

        # 자신이 로그인하고자 하는 목표 URL의 로그인 과정을 Selenium으로 웹 조작을 통해 로그인 과정을 처리하고,
        # 웹에서 보여지는 정보를 통해 로그인 성공/실패 여부를 파악합니다.
        # ... Login Process

        try:
            # 로그인 성공
            self.ui.StatusLabel.setText("로그인 성공")
        except:
          # Web에서 보여지는 로그인 성공/실패 여부를 파악할 수 있는 Web 정보를 양 끝, 공백을 제거해 String으로 가져온다.
          login_error = driver.find_element_by_xpath('Web_Xpath').text.strip()
          # 비밀번호가 틀린 경우
          if "Password is not correct" in login_error:          
              self.ui.StatusLabel.setText("아이디 혹은 패스워드를 확인해주세요.")
          # 이미 로그인 되어 있는 경우
          # 저는 로그아웃 후, 재 로그인을 하는 방법뿐이라 해당 경우를 추가했습니다.
              elif "is already login" in login_error:
                # ... Login Process
                S_Password.send_keys(self.LoginPassword) # Password 데이터 입력
                S_Password.send_keys(Keys.RETURN) # Keys.RETURN == 엔터

        # HTML5  <Select> 처리하기
        # 구조
        # <select>
        #   <option>1</option>
        #   <option>2</option>
        # </select>
        GroupSelector = Select(driver.find_element_by_name('WEB_name'))
        # <Select>의 원소들을 options를 통해 가져오기
        # 더 자세히는 Select의 options에 대해 검색해보시는 것을 추천합니다.
        GroupOptions = GroupSelector.options
        GroupList = list()
        eachList = list()

        # <Select>의 원소값의 길이만큼 반복문을 통해 List()에 저장해 데이터를 보유할 생각!
        for index in range(0, len(GroupOptions)):
            GroupList.append(GroupOptions[index].text)

        # 제가 처리할 Select 데이터는 카테고리, 카테고리 별 데이터
        # 카테고리의 데이터를 가진 리스트를 반복
        for index in range(0, len(GroupList)):
            # 카테고리 내 데이터의 XPath에 접근하기 위해 문자열 처리
            eachXpath = "//*[@id=\"itemGroupList." + GroupList[index] + "\"]"
            # 카테고리 내 xPath 접근
            eachElement = driver.find_element_by_xpath(eachXpath)
            # 카테고리 내 데이터를 접근하기 위한 HTML 태그 구조
            # <div id = "itemGroupList.GroupList[index]">
            #   <select>
            #     <option></option>
            #     <option></option>
            #   </select>
            # </div>
            # div 태그 내, text 내용 전부 가져와 확인 후, tempList에 탭을 통한 데이터 구분
            eachElementText = eachElement.get_attribute('textContent')
            tempList = eachElementText.split('\t')
            tempList = [x for x in tempList if x]
            # 카테고리별 리스트에 삽입
            eachList.append(tempList)

        # 데이터를 보유하고자 하는 txt 파일에 위에서 처리한 Data 쓰기
        # 데이터 별 구분하기 위해 '#' 추가
        with open("./data_list.txt", "w") as dataList_file:
            for group in GroupList:
                if group != GroupList[-1]:
                    dataList_file.write(group + "#")
                else:
                    dataList_file.write(group)

            dataList_file.write("\n")

            for index in range(0, len(eachList)):
                for each in eachList[index]:
                    if each != eachList[index][-1]:
                        dataList_file.write(each + "#")
                    else:
                        dataList_file.write(each)

        self.ui.StatusLabel.setText("데이터를 업데이트가 완료되었습니다. 리프레시 버튼을 눌러주세요.\n데이터를 선택 후, 추출을 눌러주세요.")

    def RefreshFunc(self):
        # user_info.txt 파일 존재 여부 파악
        if os.path.isfile("./user_info.txt"):
            with open("./user_info.txt", "r") as login_file:
                # readline을 통해 최상위 한줄만 읽어오기
                # 로그인 상태(0, 1)
                StatusLabel = login_file.readline()
                # 로그인 정보 일치
                if StatusLabel == "1\n":
                    self.ui.UpdateButton.setEnabled(True)
                # 로그인 정보 틀림
                else:
                    self.ui.UpdateButton.setEnabled(False)

        # 기존 카테고리에 있는 데이터 삭제
        self.ui.GroupBox.clear()
        # data_list 파일 존재 여부 파악해 있을 경우 업데이트
        if os.path.isfile("./data_list.txt"):
            with open("./data_list.txt", "r") as dataList_file:
                try:
                    # 카테고리를 한 줄안에서 #으로 구분해 처리했기 때문에 아래와 같이 처리
                    ComboBoxData = dataList_file.readline()
                    ComboBoxData = ComboBoxData.strip()
                    ComboBoxList = ComboBoxData.split("#")
                    # 리스트 데이터를 QComboBox에 추가
                    for item in ComboBoxList:
                        self.ui.GroupBox.addItem(item)
                except:
                    return

        # 기존 카테고리별 데이터 삭제
        self.ui.GroupListWidget.clear()
        # data_list 파일 존재 여부 파악해 있을 경우 업데이트
        if os.path.isfile("./data_list.txt"):
            with open("./data_list.txt", "r") as dataList_file:
                try:
                    # 데이터 맨 첫 줄은 카테고리 이므로, 2번째 부터 읽을 수 있도록 아래와 같이 처리
                    data = dataList_file.readlines()[1]
                    # 중간에 생길 수 있는 공백 제거
                    data = data.strip()
                    dataList = data.split("#")
                    for dafaultItem in dataList:
                        self.ui.GroupListWidget.addItem(dafaultItem)
                except:
                    return

    def dataExtractFunc(self):
      # 추후 설명...
```
위 과정을 진행하면서, 가장 복잡했던 부분은 HTML5의 Select 내 데이터를 가져오는 부분이였다.  

카테고리는 Select 내로 접근을 할 수 있었는데, 카테고리별 데이터의 Select에 카테고리 리스트를 통해 접근하기 위해서는 div에 접근해서 데이터를 가져와야 하는데 div 안 Select의 데이터를 가져오는 방식은 잘 정리되어 있지 않기 때문이다.  

그래서, `div에 접근해서 text의 모든 내용을 가져와 해당 데이터를 처리하는 것은 어떨까` 라고 생각한다. 또한, 프로그램 내에서 디버깅 혹은 print를 찍어보면서 데이터를 확인해가며 하는 것을 추천한다.  

지금 생각해보면 리스트를 2개 이용해서 반복적으로 작업하는 것은 추후에 문제가 생기거나 변경해야할 경우, 데이터를 확인하는데 번거로움이 있을 것 같다. 자료구조를 Dict으로 선언해 카테고리 별 리스트를 만드는게 더 좋은 것 같다.  

## PyQt5 이미지 추가  

MainWindow UI에서 하단에 이미지를 추가하기 위해 아래와 같이 작성하였습니다.  

QLabel에서 setPixmap함수를 통해서 이미지를 지정할 수 있으므로, 아래와 같은 방식으로 이미지를 추가할 수 있습니다.  

```python
# PyQt5 라이브러리 추가가 필요합니다.
BASE_DIR = os.path.dirname(os.path.abspath(__file__))

class MainWindow(QtWidgets.QDialog):
    def __init__(self):
        self.ui = uic.loadUi(BASE_DIR + r'\DataSelect.ui')
        # ....

        qPixmapVar = QPixmap()
        qPixmapVar.load(BASE_DIR + r'\IMAGE_NAME.gif')
        self.ui.CompanyLogoLabel.setPixmap(qPixmapVar)
```  

## PyQt5 QLabel 사용 방법  

`QLabel`의 주요 함수  
- .text() : Label에 쓰여있는 글자를 가져온다.  
- .setText(String) : Label에 새롭게 글자를 작성, Parameter에는 Label에 표시할 글자가 입력  
- .clear() : Label에 쓰여있는 글자를 지운다.  

> 프로젝트 내 사용 예시  

```python
version = "Version : 1.0"

class MainWindow(QtWidgets.QDialog):
    def __init__(self):
        self.ui.VersionLabel.setText(version)
```



---
**🐢개발 완료 된 프로그램에 대한 개념 및 내용을 정리하고자 하는 목적으로 작성되었습니다. 궁금하신점은 댓글 남겨주세요.🐢**
{: .notice--primary}

감사합니다.😊
