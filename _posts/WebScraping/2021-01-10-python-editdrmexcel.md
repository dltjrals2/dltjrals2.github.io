---
title: "[WebScraping] DRM Excel 및 Word 문서 자동화 처리"

categories:
  - WebScraping
tags:
  - [Python, openpyxl, xlwings, win32com.client]

toc:  true
toc_sticky: true

date: 2022-01-10
last_modified_at: 2022-01-11
---

📌**<u>개발 환경</u>**  
```
OS = Window and Mac
Python = 3.8 (Selenium)
IDE : Pycharm
```  

---

## DRM Excel 문서 자동화 처리  

보통 Excel 자동화 처리를 위해서 openpyxl를 사용하는데 DRM으로 문서 보안이 걸린 경우, openpyxl로 excel 파일을 읽을 수 없다.  

그래서, Excel의 API를 조작하는 방식으로 자동화 처리를 할 수 있는데 이를 위해서는 xlwings 라이브러리를 이용해야 한다.  

또한, xlwings로 DRM Excel의 API에 접근하기 위해 Excel에서도 설정을 해줘야 하는데, 저는 아래 사이트를 참고해 설정했습니다.  

- xlwings 설치 및 설정 사이트 : <https://streamls.tistory.com/262>  

xlwings를 사용하는 방식은 엑셀 자동화 프로그램 VBA를 참고하시면 도움이 될 것 같습니다.  

여기서는 제가 사용한 방법만 설명하도록 하겠습니다.  

### DRM Excel 문서 실행  

openpyxl과 다르게 xlwings는 Excel 파일을 실행시켜서 그 위에서 작업을 하는 형태입니다.  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
```

위 코드는 Excel 문서를 실행시키고 해당 파일의 객체를 excelFile로 지정합니다.  

### DRM Excel Sheet 접근  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
excelSheet = excelFile.sheets[0]
```

sheets[0]은 가장 첫 번째 sheet 입니다. 번호는 0, 1, 2, 3 ... 으로 진행됩니다.  

### DRM Excel Sheet Cell Read/Write  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
excelSheet = excelFile.sheets[0]

data = excelSheet['{}'].value
excelSheet['{}'].value = data
```  

excelSheet['{}']에는 excelSheet['A1'], excelSheet['F4']와 같이 Cell을 지정해주면 됩니다.  

데이터를 읽고, 쓰는 방법에는 예를 들면  

data = excelSheet['A1'].value 는 A1의 셀 값을 data에 저장
data = excelSheet['F3'].value 는 F3의 셀 값을 data에 저장  
excelSheet['G3'].value = data 는 G3의 셀 값을 data로 변경  

과 같은 방식이다.  

### DRM Excel Sheet Cell Data 정렬하기(Filter)  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
excelSheet = excelFile.sheets[0]

excelSheet.range('{}:{}').api.AutoFilter(1, "={}")
excelSheet.api.ShowAllData()
```  

원하는 Sheet에서 원하는 열에 대해서 필터를 처리하고 싶은 경우는 위와 같이 처리를 하면 됩니다.  

예를 들어,  

excelSheet.range('A7:A150').api.AutoFilter(1, "=10.0) 는 A7~A150의 값 중 10.0과 일치하는 것을 필터한다.  
excelSheet.range('G5:G30).api.AutoFilter(1, "=10.0")는 G5~G30의 값 중 10.0과 일치하는 것을 필터한다.  

와 같은 방식이다.  

반대로, Sheet에 적용되어진 필터를 해체하기 위해서는 다음과 같이 처리하면 된다.  

excelSheet.api.ShowAllData()  

### 마지막 데이터 위치 구하기  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
excelSheet = excelFile.sheets[0]

last_row = excelSheet.range('{}' + str(excelSheet.cells.last_cell.row)).end('up').row
```  

원하는 열의 마지막 행의 데이터를 위치는 위와 같이 처리해 얻을 수 있습니다.  

예를 들어, A열의 마지막 데이터의 행 위치를 알고 싶으면  

last_row = excelSheet.range('A' + str(excelSheet.cells.last_cell.row)).end('up').row 와 같이 처리해 A열의 마지막 데이터의 행의 위치를 얻을 수 있습니다.  

### DRM Excel 데이터 복사  

마지막 데이터 위치 구하는 것을 응용해 마지막 행 위치에 데이터를 복사할 수 있습니다.  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
excelSheet = excelFile.sheets[0]
excelSheet2 = excelFile.sheets[1]

last_row = excelSheet.range('A' + str(excelSheet.cells.last_cell.row)).end('up').row

excelSheet.range('A7:A150').copy(excelSheet.range('G{}'.format(last_row + 1)))
excelSheet.range('A7:A150').copy(excelSheet2.range('A{}'.format(last_row + 1)))
```  

excelSheet.range('A7:A150').copy(excelSheet.range('G{}'.format(last_row + 1))) 는 excelSheet의 A7~A150의 데이터를 복사해 excelSheet의 데이터가 없는 행에 데이터를 복사합니다.  
excelSheet.range('A7:A150').copy(excelSheet2.range('A{}'.format(last_row + 1))) 는 excelSheet의 A7~A150의 데이터를 복사해 excelSheet2의 데이터가 없는 행에 데이터를 복사합니다.  

### DRM Excel 문서 저장 및 닫기  

```python
import xlwings

excelFile = xlwings.Book("ExcelFile_Name")
excelFile.save('{}')
excelFile.close()
```  

excelFile.save()는 기존에 사용하던 파일 이름으로 저장하는 방식이고, excelFile.save(String)은 String 문자열 이름으로 문서를 저장하는 것이다.  
excelFile.close()를 통해 실행되어진 excelFile을 닫을 수 있다.  

## DRM Word 문서 자동화 처리  

보통 word 문서를 처리하기 위해 docx 라이브러리를 이용하는데, DRM 워드 문서를 처리하기 위해서 win32com.client를 이용해서 작업을 진행하였다.  

여기서는 제가 사용한 방법만 설명하도록 하겠습니다.  

### DRM word 문서 실행  

```python
import os
import win32com
PROGRAM_DIR = os.getcwd()

word_file = PROGRAM_DIR + r'\file_name.docx'
word = win32com.client.Dispatch("Word.Application")
wb = word.document.Open(word_file)
doc = word.ActiveDocument
```  

### DRM word 테이블 접근 및 값 수정  

테이블이 위에서 부터 숫자가 매겨지며, 처음 테이블은 1부터 시작합니다.  

또한, 테이블 Cell에 접근은 아래와 같은 방식으로 하고, 1부터 시작합니다.

```python
import os
import win32com
PROGRAM_DIR = os.getcwd()

word_file = PROGRAM_DIR + r'\file_name.docx'
word = win32com.client.Dispatch("Word.Application")
wb = word.document.Open(word_file)
doc = word.ActiveDocument

table = doc.Tables({})

table.Cell(Row = {}, Column = {}).Range = "{}"
```  

table = doc.Tables(1) : 첫 번째 테이블 지정
table = dic.Tables(2) : 두 번째 테이블 지정  
table.Cell(Row = 1, Column = 1).Range = "Data" : 1번째 행의 1번째 열 데이터를 "Data"로 입력
table.Cell(Row = 3, Column = 4).Range = "Data" : 3번째 행의 4번째 열 데이터를 "Data"로 입력

저는 Word의 테이블을 수정하는 기능만 필요해서 이 정도만 사용했습니다.

더 자세히 알고 싶은 분들은 `win32com.client`로 word를 작업하는 내용을 찾아보시면 될 것 같습니다.  

---
**🐢개발 완료 된 프로그램에 대한 개념 및 내용을 정리하고자 하는 목적으로 작성되었습니다. 궁금하신점은 댓글 남겨주세요.🐢**
{: .notice--primary}

감사합니다.😊