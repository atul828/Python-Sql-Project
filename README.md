# Python-Sql-Project
Sending Sms using python , sql and Api key
# pip install requests
# pip install mysql-connector-python
# In Linux copy this script in file with .py extension 
# crontab -e 
# 0 * * * * <Path to file>


import mysql.connector
import requests
  

url = "https://www.fast2sms.com/dev/bulkV2"
headers = {
    'cache-control': "no-cache"
}

try:
    conn = mysql.connector.connect(host='localhost',
                            database='sail',
                            user='root',
                            password='password@123')
    cursor = conn.cursor()

    fetchrecord  = """select * from Employee"""

    cursor.execute(fetchrecord)
    record = cursor.fetchall()

    for row in record:
        num = row[2]
        msg = row[3]
        querystring = {"authorization":"Jdy9ja2TsbpLOcVAUFD1txlSimYNeBMIHfwg70nXPr3ZzRCvGhlsBpNzrkJf1dua3TQXIiyRoFOVPAmL",
                       "message":msg,
                       "language":"english",
                       "route":"q",
                       "numbers":num}
        response = requests.request("GET", url, headers=headers, params=querystring, verify=False)
        print(response.text)
        print(num,msg)
        
except Exception as e:
    print(e.args)

finally:
    cursor.close()
    conn.close()
