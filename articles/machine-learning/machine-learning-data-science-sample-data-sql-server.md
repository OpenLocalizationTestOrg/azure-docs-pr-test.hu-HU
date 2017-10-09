---
title: az Azure SQL Server adatainak aaaSample |} Microsoft Docs
description: A mintaadatok az SQL Server az Azure-on
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: dc7f9529c771f6deb633775557e64a04b774f5b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Mintaadatok az SQL Server az Azure-on
Ez a dokumentum bemutatja, hogyan toosample adatok SQL Server az Azure-on tárolt SQL vagy hello Python programozási nyelv használatával. Azt is bemutatja, hogyan toomove összehasonlítást az adatminta az Azure Machine Learning úgy, hogy elmenti azt tooa fájl feltöltése az Azure blob tooan, és olvassa Azure Machine Learning Studio.

hello Python mintavételi használ hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC könyvtár tooconnect tooSQL Azure és a hello Server [Pandas](http://pandas.pydata.org/) könyvtár toodo hello mintavételi.

> [!NOTE]
> SQL mintakód hello ebben a dokumentumban azt feltételezi, hogy egy SQL Server Azure van adatok hello. Ha nem, olvassa el túl[adatok tooSQL kiszolgáló áthelyezése az Azure-on](machine-learning-data-science-move-sql-server-virtual-machine.md) témakör útmutatást toomove az adatok tooSQL Azure-kiszolgáló.
> 
> 

hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik tootopics hogyan toosample adatait tároló különböző környezetekben. 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Miért érdemes az az adatokat?**
Ha azt tervezi, hogy tooanalyze hello adatkészlet túl nagy, a rendszer általában egy jó ötlet toodown-minta hello adatok tooreduce azt tooa kisebb, de reprezentatív és könnyebben kezelhető méretét. Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz. Szerepét a hello [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) tooenable gyors prototípusának hello adatfeldolgozási funkciók és a gépi tanulási modellek van.

Ez a mintavételi feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="SQL"></a>SQL használatával
Ez a szakasz hello adatbázis SQL tooperform egyszerű véletlenszerű mintavétel hello adatok alapján használatával több módszerét ismerteti. Válassza ki az adatok mérete és a kiosztás alapuló módszer.

az alábbi két elemek hello jelennek meg, hogyan toouse newid az SQL Server tooperform hello mintavételi. hello módszertől függ hogyan véletlenszerű szeretnénk hello minta toobe (az alábbi hello mintakód pk_id feltételezett toobe egy automatikusan létrehozott elsődleges kulcs).

1. Kevésbé szigorú véletlenszerű minta
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Több véletlenszerű minta 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample is lehet mintavételek, valamint alábbi. Ez lehet jobb megközelítés az adatok mérete nagy esetén (feltéve, hogy az adatok különböző oldalain nem korrelált) és a hello lekérdezés toocomplete elfogadható időn belül.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Vizsgálatát, és a mintaadatokat egy új tábla elhelyezésével szolgáltatások generálása
> 
> 

### <a name="sql-aml"></a>Csatlakozás tooAzure gépi tanulás
Hello mintalekérdezések fent közvetlenül használható hello Azure Machine Learning [és adatokat importálhat] [ import-data] modul toodown-minta hello adatok hello keresnie, és vonja az Azure Machine Learning kísérlet. Alább látható képernyőfelvétel hello olvasó modul tooread mintát hello adatok használatával:

![olvasó sql][1]

## <a name="python"></a>Hello Python programozási nyelv használatával
Ez a szakasz azt mutatja be, hello segítségével [pyodbc könyvtár](https://code.google.com/p/pyodbc/) tooestablish az ODBC-csatlakozás tooa SQL server-adatbázis a Python. hello adatbázis-kapcsolati karakterláncot a következőképpen történik: (cserélje kiszolgálónév, dbname, felhasználónevet és jelszót a konfiguráció):

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Hello [Pandas](http://pandas.pydata.org/) a Python kódtár adatkezelési Python programozási széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket biztosít. hello kódot hello adatok 0,1 % minta beolvassa az Azure SQL-adatbázis egy táblából egy Pandas adatokat:

    import pandas as pd

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Most már hello Pandas adatok keretében mintát hello adatokkal dolgozhat. 

### <a name="python-aml"></a>Csatlakozás tooAzure gépi tanulás
A következő kód toosave hello le mintát tooa mintaadatfájlokat hello használja, és töltse fel az Azure blob tooan. hello adatokat hello BLOB közvetlenül elolvashatja az Azure Machine Learning kísérlet hello segítségével [és adatokat importálhat] [ import-data] modul. hello lépései a következők: 

1. Hello pandas adatok keret tooa helyi fájl írása
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Helyi fájl tooAzure blob feltöltése
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Adatokat olvasni az Azure Machine Learning segítségével az Azure blob [és adatokat importálhat] [ import-data] modul látható hello képernyő fogd az alábbi módon:

![olvasó blob][2]

## <a name="hello-team-data-science-process-in-action-example"></a>hello Team adatok tudományos folyamat művelet példa
Hello Team adatok tudományos folyamat végpont bemutató példa használata a nyilvános dataset, lásd: [Team adatok tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
