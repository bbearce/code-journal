# CSV Loader

Packages needed:

```python
os
pandas
sqlite3
numpy
```

This sets of scripts works as 2 independent situations:

## [1] Bulk folder load

The function in this script looks inside a folder and using all \*.csv begins a process of concatenating all csvs together and removing duplpicates on each merge. 

```python
import pandas as pd, os, pdb, sqlite3, numpy as np

pd.set_option('display.min_rows', 25) # How many to show
pd.set_option('display.max_rows', 25) # How many to show

death_folder = 'data_Death'
intubation_folder = 'data_Intubation'
demographics_folder = 'data_demographics'
covid_test_results_folder = 'data_COVID_TestResults'
chest_imaging_reports_folder = 'data_chestImagingReports'

db_dir = 'db'

# Death example for function (Could require lots of Ram and DB grows in size...poorly coded to scale, but good for initializing a DB.)
def create_table_from_folder_csvs(folder=None, demographic_table=False):
    # Could freeze 8 GB RAM machines as this lazily reads all csvs into RAM
    # HOWEVER IF YOU HAVE A MONSTER MACHINE USE IT...IT'S ABOUT 50+ TIMES FASTER TO USE THIS FUNCTION IF YOU CAN
    # Recommended: Use only a coulple files to define scheme and then use load csvs to load 1 at a time.

    conn = sqlite3.connect(os.path.join(db_dir,'covid.db'))

    if not isinstance(folder, str):
        return("Not a valid folder")
    
    files = [i for i in os.listdir(folder) if i.find('.csv') != -1]
    print(files)

    # base frame for all but demographics data
    data = pd.DataFrame()
    # base frame for demographics data (2 table types)
    demographics = pd.DataFrame()
    socialhistory = pd.DataFrame()

    for file in files:
        csv = pd.read_csv(os.path.join(folder, file))

        # demographic tables have 2 schemes
        if demographic_table == True:

            if file.find('Demographics') != -1:
                demographics = pd.concat([demographics, csv], axis=0)
                print(demographics.shape)
                demographics = demographics[~demographics.duplicated(keep='first')]
                

            elif file.find('SocialHistory') != -1:
                socialhistory = pd.concat([socialhistory, csv], axis=0)
                print(socialhistory.shape)
                socialhistory = socialhistory[~socialhistory.duplicated(keep='first')]
                

        else:
            data = pd.concat([data, csv], axis=0)
            print(data.shape)
            data = data[~data.duplicated(keep='first')]
            
    frames = {'data':data, 'demographics':demographics, 'socialhistory':socialhistory}
    for key in frames.keys():
        frame = frames[key]

        if frame.shape[0] != 0:
            # pdb.set_trace()
            if key == "demographics":
                frame.to_sql(f"{folder}_demographics", conn, if_exists="replace", index=False)
            elif key == "socialhistory":
                frame.to_sql(f"{folder}_socialhistory", conn, if_exists="replace", index=False)
            else:
                frame.to_sql(f"{folder}", conn, if_exists="replace", index=False)




create_table_from_folder_csvs(death_folder)
create_table_from_folder_csvs(intubation_folder)
create_table_from_folder_csvs(demographics_folder, demographic_table=True)
create_table_from_folder_csvs(covid_test_results_folder)
create_table_from_folder_csvs(chest_imaging_reports_folder)

```

## [2] Load a new csv

This script could load every csv but it much much slower. Use this once a bulk database is made andd you want to add a new csv.

```python
import pandas as pd, os, pdb, sqlite3, numpy as np

pd.set_option('display.min_rows', 25) # How many to show
pd.set_option('display.max_rows', 25) # How many to show

death_folder = 'data_Death'
intubation_folder = 'data_Intubation'
demographics_folder = 'data_demographics'
covid_test_results_folder = 'data_COVID_TestResults'
chest_imaging_reports_folder = 'data_chestImagingReports'

db_dir = 'db'

def load_csv(folder=None, file=None, demographic_table=False):
    # Proper input check
    if not isinstance(folder, str):
        return("Not a valid folder\\table name")
    if not isinstance(file, str):
        return("Not a valid file")
    # DB Connection
    conn = sqlite3.connect(os.path.join(db_dir,'covid.db'))
    c = conn.cursor()
    # Utility 
    def check_if_table_exists_in_db(table):
        c.execute(f''' SELECT count(name) FROM sqlite_master WHERE type='table' AND name='{table}' ''')
        if c.fetchone()[0]==1 : 
            print('Table exists.')
            return(True)
        else:
            print('Table does not exist.')
            return(False)
    ### table name 
    table = folder
    if demographic_table:
        if file.find('Demographics') != -1:
            table = folder+'_demographic'
        elif file.find('SocialHistory') != -1:
            table = folder+'_socialhistory'
    # Load Data
    csv = pd.read_csv(os.path.join(folder, file))
    # Create Table if it doesn't exist
    if not check_if_table_exists_in_db(table):
        print("creating table")
        schema = ''
        for col in csv.columns:
            type_key = {'object':'TEXT', 'int64':'INTEGER', 'float64':'FLOAT'}
            schema += f"'{col}' {type_key[str(csv.dtypes[col])]}, "
        schema = schema[0:-2]
        c.execute(f""" create table '{table}' ({schema})""")
    
    def check_row_against_table(df_row, table=table ,conn=conn):
        # Here I had to pass in conn as it wasn't able to be seen otherwise.
        # This is because this is apart of the "apply" function in pandas (probably some sort of scoping issue)
        # See "csv.apply(check_row_against_table, axis=1)" below this function definition
        print(df_row) # Optional and makes things slower, but it's a good visual
        where_filter = ''
        insert_statements = ''
        for col in df_row.keys():
            if pd.isnull(df_row[col]) == True:
                # Build element for queries escaping single quotes in text string for sqlite
                where_filter += f"{col} is null and "
                insert_statements += f"null, "
            else:
                value = str(df_row[col]).replace('\'','\'\'') # escape quotes for use in sqlite
                where_filter += f"{col} = '{value}' and "
                insert_statements += f"'{value}', "
        # pdb.set_trace()
        # Trim commas
        where_filter = where_filter[0:-5] # trimming " and "
        insert_statements = insert_statements[0:-2] # trimming ", "
        # Execute search for current record
        query = f'''select * from {table} where {where_filter};'''
        c.execute(query)
        # c.execute("select * from data_Death where MRN = 'not a chance'") # Use this for debugging
        try:
            # Record is a duplicate if it doesn't throw exception
            original_record = c.fetchone()[0]
        except:
            # Record is brand new
            # pdb.set_trace()
            c.execute(f"insert into {table} values ({insert_statements});")
        # commit the changes to db          
        conn.commit()
    # Loop over every row in csv and call above function
    csv.apply(check_row_against_table, axis=1)

    #close the connection
    conn.close()


# #xample executions:

# data_Death
load_csv(folder=death_folder, file='Mortality_20200414110000.csv')
load_csv(folder=death_folder, file='Mortality_20200423110000.csv')
load_csv(folder=death_folder, file='Mortality_20200417110000.csv')
load_csv(folder=death_folder, file='Mortality_20200422110000.csv')
load_csv(folder=death_folder, file='Mortality_20200420090000.csv')
load_csv(folder=death_folder, file='Mortality_20200421110000.csv')
load_csv(folder=death_folder, file='Mortality_20200424080000.csv')
load_csv(folder=death_folder, file='Mortality_20200418090000.csv')
load_csv(folder=death_folder, file='Mortality_20200419090000.csv')
load_csv(folder=death_folder, file='Mortality_20200416090000.csv')
load_csv(folder=death_folder, file='Mortality_20200415090000.csv')

# data_Intubation
load_csv(folder=intubation_folder, file='Intubation_20200421090000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200418180000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200420180000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200416100000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200423110000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200422090000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200415100000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200424080000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200414160000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200417100000.csv')
load_csv(folder=intubation_folder, file='Intubation_20200419180000.csv')

# data_COVID_TestResults
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200421090000.csv')
load_csv(folder=covid_test_results_folder, file='CVOID_testResults_20200423110000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200415180000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200418180000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200416180000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200422090000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200417180000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200420180000.csv')
load_csv(folder=covid_test_results_folder, file='CVOID_testResults_20200424080000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200419180000.csv')
load_csv(folder=covid_test_results_folder, file='COVID_testResults_20200414160000.csv')

# data_chestImagingReports
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200421090000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200417100000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200414110000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200422090000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200419180000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200424080000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200423110000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200416090000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200418160000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200420180000.csv')
load_csv(folder=chest_imaging_reports_folder, file='XRChest_20200415110000.csv')


# data_demographics
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200415110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200420180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200418160000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200414110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200415180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200417180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200418160000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200415180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200421090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200418160000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200416180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200423110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200416090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200423110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200420180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200419180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200422090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200415180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200423110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200417140000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200418160000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200414110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200416180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200422090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200423110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200415110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200420180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200414110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200414110000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200419180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200421090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200422090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200417180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200416090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200420180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200415180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_Demographics_20200421090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200421090000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200419180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_SocialHistory_20200419180000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='Inpatient_ADT_Demographics_20200417140000.csv', demographic_table=True)
load_csv(folder=demographics_folder, file='RIC_ED_visits_SocialHistory_20200422090000.csv', demographic_table=True)


```