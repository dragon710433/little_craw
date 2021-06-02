## 爬網頁轉出csv檔

調整前
```python
import requests as rq
import re
import csv
import pandas as pd
from bs4 import BeautifulSoup


code_list = []
url = "https://isin.twse.com.tw/isin/C_public.jsp?strMode=4&fbclid=IwAR1PTQQMv_uz-1uzTZ4fFUUemflV6v7esR5xnTF9HllFftvORTSEZ_J2we4" # PTT NBA 板
response = rq.get(url)
html_doc = response.text
soup = BeautifulSoup(response.text, "html.parser") # 爬取網頁 html 資料
td_data = soup.find_all('td')

data = re.split(r"<td bgcolor=\"#FAFAD2\">", str(td_data))
regex = re.compile('\s+')
for item in data[1:-1:7]:# 擷取所需行數
    item = item.replace("</td>,", "")# 刪除多餘字元
    code_list.append(regex.split(item))

code_list_df =  pd.DataFrame(list(code_list))# DATAFRAME資料創建
clean_code_list_df = code_list_df.drop(2,axis = 1)# DATAFRAME資料整理
clean_code_list_df.columns = ["code","title"]

print(clean_code_list_df)
clean_code_list_df.to_csv('OTC_CODE.csv',index=False,encoding='utf-8-sig')

```

精簡調整後
```python
import requests
import re
import csv
import pandas as pd
from bs4 import BeautifulSoup

url = 'https://isin.twse.com.tw/isin/C_public.jsp?strMode=4&fbclid=IwAR1PTQQMv_uz-1uzTZ4fFUUemflV6v7esR5xnTF9HllFftvORTSEZ_J2we4'
resp = requests.get(url)
soup = BeautifulSoup(resp.text, 'html.parser') # 爬取網頁 html 資料
td_data = soup.find_all('td')

code_list = []
data = re.split(r'<td bgcolor=\"#FAFAD2\">', str(td_data))
regex = re.compile('\s+')
for item in data[1:-1:7]: # 擷取所需行數
    item = item.replace('</td>,', '') # 刪除多餘字元
    code_list.append(regex.split(item)[:2])

code_list_df = pd.DataFrame(code_list)
code_list_df.columns = ['code','title']
code_list_df.to_csv('OTC_CODE.csv', index=False, encoding='utf-8-sig')
```
