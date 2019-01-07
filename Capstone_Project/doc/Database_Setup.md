# Database Setup in Docker

Before running anything, download microsoft/mssql-server-linux image file. 
```bash
docker pull mcr.microsoft.com/mssql/server:latest
```
Then,

1. Run SQL Server on docker container with SA access
- the passcode should meet minimum requirement
- use -d to run at backend
```bash
docker run -e "HOMEBREW_NO_ENV_FILTERING=1" -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=1StrongPassword"\
  -p 1433:1433 --name db2017 --restart=on-failure:3 \
  -v ~/Downloads/StackOverflow-SQL-Server-201712:/var/opt/mssql/mydata \
  -d microsoft/mssql-server-linux:latest
```
2. Check container status
```bash
docker ps -a
```

3. Transfer MDF/NDF/LDF files to container
The host folder is already mounted, but somehow cannot be used to build database. The work around is to make a copy
inside the container so that it has root:root ownership. (There must be better alternative..)

```bash
# Run bash inside container
docker exec -it db2017 "bash"
# check data files
ls /var/opt/mssql/mydata -l
# make copies of all files
cd /var/opt/mssql
cp mydata rootdata -R -v
# check data copies
ls /var/opt/mssql/rootdata -l
```

4. Run `sqlcmd` to query from the docker server.

There are two ways to do it.
The first way is to run it in bash within docker container
```
# bash in container
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 1StrongPassword
```
The more straightforward approach is to install `sqlcmd` in host environment and directly run it
```bash
# bash in host
sqlcmd -S localhost -U SA -P 1StrongPassword
```

5. in sqlcmd, create database with mdf/ndf/ldf files.
```SQL
CREATE DATABASE StackOverflow2017 ON 
(NAME = StackOverflow_1, FILENAME = '/var/opt/mssql/rootdata/StackOverflow_1.mdf'), 
(NAME = StackOverflow_2, FILENAME = '/var/opt/mssql/rootdata/StackOverflow_2.mdf'), 
(NAME = StackOverflow_3, FILENAME = '/var/opt/mssql/rootdata/StackOverflow_3.mdf'), 
(NAME = StackOverflow_4, FILENAME = '/var/opt/mssql/rootdata/StackOverflow_4.ndf') 
LOG ON (NAME = StackOverflow_log, FILENAME = '/var/opt/mssql/rootdata/StackOverflow_log.ldf') 
FOR ATTACH
GO
```
6. Test query to make sure tables are good

Print all tables
```SQL
USE StackOverflow2017
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE='BASE TABLE'
GO
```
Print the first three records from USERS table
```
SELECT TOP (3) * FROM dbo.Users
GO
```

7. Connect from Jupyter Notebook/Python, and conver to CSV

Load library
```python
import pyodbc
import pandas as pd
import csv
```
Setup server connection
```python
server = '0.0.0.0,1433'
database = 'StackOverflow2010'
username = 'sa'
password = 'YourStrongPassword'
# My Computer cannot use driver alias so I used the full path
con = pyodbc.connect('Driver=/usr/local/lib/libmsodbcsql.17.dylib;SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+password)
cursor = con.cursor()
```
Test query with `cursor.fetchall()`
```python
cursor.execute("select top 10 title from posts")
rows = cursor.fetchall()
for row in rows:
    print(row)
```
Test query with `pd.read_sql()`
```python
sql10 = 'select top 10 * from posts'
data10 = pd.read_sql(sql10, con)
print(data10)
```
To retrieve table and save to a CSV file, there are also two approaches. Suppose we would like to convert the following query to a CSV:
```python
sql = 'select ID,AcceptedAnswerId,CreationDate,Body,Title,' +\
    'AnswerCount,CommentCount,FavoriteCount,ViewCount,' +\
    'Score,Tags from posts where PostTypeID = 1'
```
First, using `csv.writer()`
```python
rows = cursor.execute(sql)
with open('/Volumes/SharedData/StackOverflow/SQL/StackOverflow2017.csv', 'w', newline='') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow([x[0] for x in cursor.description])  # column headers
    for row in rows:
        writer.writerow(row)
```
Second, using `pd.read_sql()` then save to csv with `pd.to_csv()`. This would probably need to keep the whole query file in the memory.
```python
data = pd.read_sql(sql, con)
data.to_csv('/Volumes/SharedData/StackOverflow/SQL/StackOverflow2017_v2.csv')
```
