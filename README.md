# ProblemSet4
Problem Set 4 for Zoo6927: Creating a database


### 1. Metadata document found here: https://github.com/sammyarroyo/ProblemSet4/blob/master/EUC_metadata.md

### 2. I would split this data up into two tables. The first table, labeled Rates, would include columns for the utility company name, the commercial rate, the individual rate, and the residential rate charges. This table would only contain unique utility company names, as this information including each utility company's charge rates, seem to be very redundant throughout the dataset. The primary key for this table would be the utility company name. The second table, labeled location, would include columns for the zip code, state, utility company name, and ownership type. The foreign key in this table will be the utility company name, relating to the Rates table utility company name. Table below explains further: 
| Table name | Column names | Type of data | Keys | 
| --- | --- | --- | --- |
| Rates | utility_name | String | Primary Key | 
| | comm_rate | String | na |
| | ind_rate | String | na |
| | res_rate | String | na | 
| Location | zip | Integer | na |
| | state | String | na | 
| | utility_name | string | Foreign Key (Rates.utility_name) | 
| | ownership | string | na | 
 ### you can easily search through these two tables to find information on which utility company runs your zip code, and what the utility company charges are.  
 
 ### 3. Either directly in sqlite or in Python with SQLAlchemy, write the code needed to define the tables above.
```
from sqlalchemy import create_engine
from sqlalchemy import Table, Column, Integer, String, MetaData, ForeignKey
from sqlalchemy import sql, select, join, desc

#Creating an sqLite Database in my share folder:
engine = create_engine('sqlite:////ufrc/zoo6927/share/arroyo.sr/ProblemSet4/EUC.sqlite')

metadata=MetaData(engine)

#Code to try to load Rates table, or create it with defined columns below if not there:
try:
    Rates=Table('Rates', metadata, autoload=True)
except:
    Rates = Table('Rates', metadata,
                 Column('utility_name', String, primary_key=True),
                 Column('comm_rate', String),
                 Column('ind_rate', String),
                 Column('res_rate', String),
                 )
#Code to try to load Location table, or create it with defined columns below if not there:    
try:
    Location=Table('Location', metadata, autoload=True)
except:
    Location = Table('Location', metadata,
             Column('zip', Integer),
             Column('state', String),
             Column('utility_name', String, ForeignKey('Rates.utility_name')),
             Column('ownership', String),
             )    

metadata.create_all(engine)
```

### 4. Write the code to load the data into the database. 

```
mport csv
EUC=open("/ufrc/zoo6927/share/arroyo.sr/ProblemSet4/noniouzipcodes2015.csv")
reader = csv.DictReader(EUC)

util_dict={}

# Read through the file and make a dictionary for utility names.
# This gets a unique list of utility company names.
for Line in reader:
    if Line['utility_name'] not in util_dict:
        util_dict[Line['utility_name']]=[Line['utility_name']]

print(util_dict)

#Add the util_dict utility names to the Rates table:
conn = engine.connect()
def insert_util(utilityname):
    ins=Rates.insert(utility_name=utilityname)
    result = conn.execute(ins)

for key in util_dict.items():
    insert_util(key)
#Close the File
EUC.close()

#Reopen the File:
EUC=open("/ufrc/zoo6927/share/arroyo.sr/ProblemSet4/noniouzipcodes2015.csv")

conn = engine.connect()
reader = csv.DictReader(EUC)
for Line in reader:
    ins=Rates.insert().values(utility_name=Line['utility_name'],
                             comm_rate = Line['comm_rate'],
                             ind_rate = Line['ind_rate'],
                             res_rate = Line['res_rate']
                             )
    
   result = conn.execute(ins)
```
