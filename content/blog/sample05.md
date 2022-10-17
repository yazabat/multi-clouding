+++
author = "ADS"
comments = false
date = "2022-09-29"
draft = false
image = ""
menu = ""
share = false
slug = "sample5"
title= "Using Azure Functions for ETL with Power BI reports automation"
tags = ["blog"]
+++


![Hugo_themes](/blog/images/AzureSynapse.jpg)
![Hugo_themes](/blog/images/AzureSynapse2.jpg)
![Hugo_themes](/blog/images/AzureSynapse3.jpg)


## Extract

```
import os
import yaml
import pyodbc
from azure.storage.blob import ContainerClient
import csv
from azure.storage.blob import BlobServiceClient
import pandas as pd
import findspark
from urllib.parse import urlparse
from io import BytesIO
import urllib
import sqlalchemy as sa

class IngestaData:

    __config = ""

    #Download parquet to DataFrame
    def __azure_download_parquet_to_df(url=None):
        try:
            if url:
                connect_str = IngestaData.__config["azure_storage_connectionstring"] 
                blob_service_client = BlobServiceClient.from_connection_string(connect_str)
                path = urlparse(url).path
                path = path.split("/")            
                container = path[0] + "/" + path[1]
                blob = '/'.join(path[2:])
                blob_client = blob_service_client.get_blob_client(container=container, blob=blob)
                with BytesIO() as input_blob:
                    blob_client.download_blob().download_to_stream(input_blob)
                    input_blob.seek(0)
                    df = pd.read_parquet(input_blob, engine='pyarrow')
                return df
            else:
                return None
        except OSError as err:
            print("OS error: {0}".format(err))
        except ValueError as err:
            print("ValueError error: {0}".format(err))
        except BaseException as err:
            print(f"Unexpected {err=}, {type(err)=}")
            raise

    #From Parquet to Synapse
    def __ingestaSynapse (df):
        try:
            server = 'zzzzzzdwg.sql.azuresynapse.net' ##<server>.database.windows.net'
            database = 'dwh' 
            username = 'JamesBond' 
            password = '123456.'   
            driver= '{ODBC Driver 17 for SQL Server}'

            cnxn = pyodbc.connect('DRIVER=' + driver +';SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
            cursor = cnxn.cursor()
            
            for index, row in df.iterrows():
                cursor.execute("INSERT INTO dbo.booking (BOOKINGID,BOOKINGNAME,BOOKINGREFERENCENUMBER) values(?,?,?)", row.BOOKINGID, row.BOOKINGNAME, row.BOOKINGREFERENCENUMBER)
            cnxn.commit()
            cursor.close()
        except OSError as err:
                print("OS error: {0}".format(err))

    def iniciarIngesta(config):

        IngestaData.__config = config

        #FROM PARQUET TO DF
        df = IngestaData.__azure_download_parquet_to_df(IngestaData.__config["archivos_container_clean_name"]  + "/booking.parquet")

        #PARQUET TO SYNAPSE
        IngestaData.__ingestaSynapse(df)

```

## Transform

```
import os
import yaml
from azure.storage.blob import ContainerClient
import csv
from azure.storage.blob import BlobServiceClient
import pandas as pd
import findspark
from urllib.parse import urlparse
from io import BytesIO

class Transformacion:

    __config = ""

    #METHOD FOR DOWNLOADING CSV TO DF
    def __azure_download_csv_to_df(url=None):
        try:
            if url:
                blob_service_client = BlobServiceClient.from_connection_string(Transformacion.__config["azure_storage_connectionstring"])
                path = urlparse(url).path
                path = path.split("/")            
                container = path[0] + "/" + path[1]
                blob = '/'.join(path[2:])
                blob_client = blob_service_client.get_blob_client(container=container, blob=blob)
                with BytesIO() as input_blob:
                    blob_client.download_blob().download_to_stream(input_blob)
                    input_blob.seek(0)
                    df = pd.read_csv(input_blob , sep=';',  encoding='latin-1')
                return df
            else:
                return None
        except OSError as err:
            print("OS error: {0}".format(err))
        except ValueError as err:
            print("ValueError error: {0}".format(err))
        except BaseException as err:
            print(f"Unexpected {err=}, {type(err)=}")
            raise
    
    def iniciarTransformacion(config):
        Transformacion.__config = config
        print("Transformacion")

        #FROM CSV TO DATAFRAME
        df = Transformacion.__azure_download_csv_to_df( Transformacion.__config["archivos_container_name"]+ "/booking.csv")

        #CONVERT CSV TO PARQUET
        df.to_parquet(Transformacion.__config["source_folder"] + "/parquet/booking.parquet")

        #FROM PARQUET TO BLOB STORAGE
        connect_str = Transformacion.__config["azure_storage_connectionstring"]
        blob_service_client = ContainerClient.from_connection_string(connect_str,Transformacion.__config["archivos_container_clean_name"])
        blob_client = blob_service_client.get_blob_client('booking.parquet')
        with open(Transformacion.__config["source_folder"] + "/parquet/booking.parquet","rb") as data:
            blob_client.upload_blob(data, overwrite=True)
```

## Load
```
import os
import yaml
from azure.storage.blob import ContainerClient
import csv

class Carga:

    __config = ""

    # READING CSV FILES
    def __get_files(dir):
        with os.scandir(dir) as entries:
            for entry in entries:
                if entry.is_file():# and not entry.name.startwith('.'):
                    yield entry

    #UPLOAD FILES  CSV TO BLOB STORAGE
    def __upload(files,ConnectionString,Container_blob):
        container_client = ContainerClient.from_connection_string(ConnectionString,Container_blob)
        
        print("UPLOADING TO BLOB STORAGE")
        
        for file in files:
            blob_client = container_client.get_blob_client(file.name)
            with open(file.path,"rb") as data:
                blob_client.upload_blob(data, overwrite=True)
                print(" UPLOADING FILES test2022dw")


    def iniciandoCarga(config):

        Carga.__config = config
        archivos = Carga.__get_files(Carga.__config["source_folder"] + "/csv")

        #UPLOADING CSV TO CLOUD
        Carga.__upload(archivos,Carga.__config["azure_storage_connectionstring"], Carga.__config["archivos_container_name"])

```

https://learn.microsoft.com/en-us/azure/cosmos-db/synapse-link