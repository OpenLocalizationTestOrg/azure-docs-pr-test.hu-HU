---
title: "aaaProcess Azure blob-adatok speciális elemzésekre |} Microsoft Docs"
description: "Folyamat adataihoz az Azure Blob Storage tárolóban."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8a59078-91d3-4440-b85c-430363c3f4d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 5911d4211c4135680555a8cdd99e745499a24215
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>A speciális elemzés Azure blob-adatok feldolgozása
Ez a dokumentum ismerteti az adatok felfedezése és az Azure Blob storage-ban tárolt adatok előállítása szolgáltatások. 

## <a name="load-hello-data-into-a-pandas-data-frame"></a>Pandas adatok keretbe hello adatok betöltése
A sorrend tooexplore és kezelheti a DataSet adatkészlet, forrásfájlból hello blob tooa helyi amely majd tölthetők be adatok Pandas keretben kell letölteni. Az alábbiakban hello lépéseket toofollow az eljárás végrehajtásához:

1. Az Azure-ból hello adatok letöltése a blob szolgáltatással Python mintakód a következő hello blob. Cserélje le az alábbi kódot hello hello változó saját értékekkel: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds toodownload "+blobname) % (t2 - t1))
2. Hello adatok olvasása a keretbe Pandas adatok-hello a letöltött fájlt.
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Most már készen áll a tooexplore hello adatok, és ehhez az adatkészlethez funkcióinak készítése.

## <a name="blob-dataexploration"></a>Az adatok feltárása
Íme néhány példa a Pandas tooexplore adatokat:

1. Vizsgálja meg a sorok és oszlopok hello száma 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Először vizsgálja meg hello vagy utolsó néhány sor hello adatkészlet a következő:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Egyes oszlopok használatával a következő példakód hello importált hello adatok típusának ellenőrzése
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Ellenőrizze az alábbiak szerint hello alapszintű hello adatkészlet hello oszlopai statisztikái
   
        dataframe_blobdata.describe()
5. Tekintse meg minden egyes oszlop értékre bejegyzések hello száma az alábbiak szerint
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Hiányzó értékek száma és az egyes oszlopok a következő példakód hello segítségével bejegyzések hello tényleges száma
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Ha egy adott oszlopban a hiányzó értékeket hello adatokban, elvetné azokat az alábbiak szerint:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Hiányzó értékek egy másik módja tooreplace hello mód függvénnyel van:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< oszlopnév >: .mode()[0]}) dataframe_blobdata ["< oszlopnév >"]        
8. Hisztogram rajzot változó számú bins tooplot hello terjesztési egy változó létrehozása    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Nézze meg változók egy scatterplot vagy hello beépített korrelációs függvény használatával közötti összefüggések
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Szolgáltatás létrehozása
A Microsoft hozhat létre a funkciók Python az alábbiak szerint:

### <a name="blob-countfeature"></a>Kijelző értékének alapú szolgáltatás létrehozása
A kockák szolgáltatásai az alábbiak szerint hozhatók létre:

1. Hello terjesztési hello kategorikus oszlop vizsgálata:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Az egyes oszlopok értékeinek hello kijelző értékek generálásához
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Csatlakozás hello kijelző oszlop hello eredeti adatok keret 
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Távolítsa el az eredeti változó hello saját magát:
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>A dobozolás szolgáltatás létrehozása
Binned szolgáltatások létrehozásának, a Folytatás az alábbiak szerint:

1. Az oszlopok toobin egy numerikus oszlopot a sorozat hozzáadása
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. A dobozolás tooa feladatütemezési változók logikai átalakítása
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Végezetül illesztési hello dummy változók biztonsági toohello eredeti adatok keret
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="sql-featuregen"></a>Az adatok írásakor biztonsági tooAzure blob és az Azure Machine Learning felhasználása
Miután felfedezte hello adatokat, és a létrehozott hello szükséges szolgáltatást, hello az adatait feltöltheti (mintát vagy featurized) tooan Azure blob-, és ezért használja fel az Azure Machine Learning hello lépések segítségével: hello további szolgáltatásokat lehet létrehozni Az Azure Machine Learning Studio is. 

1. Hello adatok keret toolocal fájl írása
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Hello adatok tooAzure blob feltöltése az alábbiak szerint:
   
        from azure.storage.blob import BlobService
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
3. Most hello adat olvasható hello blob használatával hello Azure Machine Learning [és adatokat importálhat] [ import-data] modul az alábbi hello képernyőn látható módon:

![olvasó blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

