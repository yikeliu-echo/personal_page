# Project: monitor competitors' charts
We're working on copyright negotiation preparation. One thing to notice is that which label have a higher consumption data in competitors' charts and to see whether we have these songs or not. Therefore, it is important to monitor competitors' charts. I use the scrap skills I've learnt on class and also the numpy/Pandas knowledge to deal with the raw data.

## 1. Scrap the data
```python
import os
import numpy as np
import pandas as pd
import time
import random
import datetime

from bs4                             import BeautifulSoup
from selenium                        import webdriver
from sklearn.feature_extraction.text import CountVectorizer

pd.set_option('display.max_rows', 10)
pd.set_option('display.max_columns', 5)
pd.set_option('display.width',800)
path = 'C:/Users/admin/Desktop/chromedriver_win32'
os.chdir(path)

def get_date():
    begin = datetime.date(2020, 10, 23)
    end = datetime.date(2020, 11, 25)

    day_range = []
    for i in range((end - begin).days + 1):
        day = begin + datetime.timedelta(days=i)
        day_range.append(str(day))

    return day_range

date_store = get_date()

link_store = []
for i in date_store:
    link = "https://app.chartmetric.com/charts/spotify/daily?type=regional&region=id&date=" + i
    link_store.append(link)

driver = webdriver.Chrome()
for i in range(len(link_store)):
    link_scrap = link_store[i].replace("‘", "")
    driver.get(link_scrap)
    time.sleep(12)
    driver.find_element_by_xpath("//button[@class='btn btn-rounded btn-purple']").click()
```

## 2. Merge all csv into one file
```python
import pandas as pd
import os

Folder_Path = r'C:\Users\Admin\Desktop\Deezer1026-1125'  # Folder path to be spliced, be careful not to include Chinese
SaveFile_Path = r'C:\Users\Admin\Desktop'  # File path to be saved after splicing
SaveFile_Name = r'AppleIndonesiaPlaylist2.csv'  # File name to be saved after merging


# Modify current working path
os.chdir(Folder_Path)
# Save all file names in this folder into a list
file_list = os.listdir()

# Read the first CSV file and include the header and add a new column to include the filename
df = pd.read_csv(Folder_Path + '\\' + file_list[0])
#df["Playlist Name"] = os.path.splitext(file_list[0])[0]
# Write the first CSV file into the merged file and save
df.to_csv(SaveFile_Path + '\\' + SaveFile_Name, encoding="utf_8_sig", index=False)

# Loop through each CSV file name in the list and append to the merged file
for i in range(1, len(file_list)):
    df = pd.read_csv(Folder_Path + '\\' + file_list[i])
    #df["Playlist Name"] = os.path.splitext(file_list[i])[0]
    df.to_csv(SaveFile_Path + '\\' + SaveFile_Name, encoding="utf_8_sig", index=False, header=False, mode='a+')
```

