# sqlite_database_operations

  ## Dataset
The datasets I utilized are from Stony Brook Hospital and St. Joseph Hospital. Both datasets contain data reguarding billing/charges, codes, prices, insurance company information, and descriptions such as description of charges, and description of codes. 
  ## Exploratory Data Analysis(EDA) Process
I preformed basic exploratory analysis using python, focusing on missing values, basic statistics and data distribution. The values in the datasets that were marked as NaN, were dropped to clean up and provide a more accurate view of the dataset. I also preformed basic statistics for the datasets, such as utilizing the commands ``` .value_counts()```  and ``` .describe()``` . For the data distribution aspect of EDA, I created histograms to visualize the numerical data values and frequencies. 
  ## Instructions to Replicate my SQLite Database Setup

To replicate the the SQLite database setup process, one would need to:

1) Load in the packages ``` import pandas as pd``` , ``` from sqlalchemy import create_engine```  and ``` import sqlite3```
2) Then one would create a local SQLite databse by using the command ``` conn = sqlite3.connect('type_the_name_of_what_you_want_to_call_your_desired_database.db')```  and ``` c = conn.cursor()```
3) After that, to start to manually create a table, use this command:
  ``` 
c.execute("""
            CREATE TABLE stonybrook_system #intead of stonybrook_system, write the name you would want the table name to be
                (
                    hospital_name text,     #these are the columns within the table, chooe the names of the columns you would like to include in your table
                    insurance_type text,
                    code text,
                    code_description text,
                    cost_negotiated real,
                    cost_minimum real,
                    cost_maximum real
                );
          """)

conn.commit()
```

and then this command:
```
c.execute('''
  SELECT * FROM stonybrook_system;
''')

print(c.fetchall())
```
4) Next, one would utilize the ```INSERT INTO``` SQL query command to populate rows of data, fake data in this case. My insert into command looked like this:
 ```
   sql_query = """

INSERT INTO stonybrook_system (
  'hospital_name',
  'insurance_type',
  'code',
  'code_description',
  'cost_negotiated',
  'cost_minimum',
  'cost_maximum'
  )
  values (
    'eastern long island hospital', #according to the respective column names above, insert corresponding values you would like to insert into your table as a row
    'united healthcare',
    '99214',
    'outpatient visit',
    150.00,
    1.00,
    1500.00
  );

"""

print(sql_query)
c.execute(sql_query)
conn.commit()
```
5) After that, you can check if the row  has been inserted by:
```sql_query_2 = """
select *
from stonybrook_system; #replace stonybrook_system with your table name
"""

c.execute(sql_query_2)
print(c.fetchall())
```
6) Then, you will close the connection by ```conn.close()
7) Next, you need to create an engine to connect to the SQLite database, the command that can be used to accomplish this is ```engine = create_engine('sqlite:///your_database_name.db')```
8) When that is done running, you can check if the row has been created and how it looks by using pandas. This command can be used ```pd.read_sql_query("select * from stonybrook_system;", conn) #replace stonybrook_system with your table name```
9) After that for automatic table creation, one would first load a file such as the cvs utilized earlier in the assignment, and then use the command ```.sql('your_table_name', conn, if_exists='replace')```
10) Lastly, to check the response utilize the commands:
```query = """
  select *
  from stonybrook_system #replace with your table name
  where description = 'Bone Marrow Transplant' #replace with whic values you want the table to consists of 
  limit 100;
"""

response = pd.read_sql(query, conn)
response
```
