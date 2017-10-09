---
title: "aaaCreate szolgáltatások az Azure blob storage adatok Panda |} Microsoft Docs"
description: "Hogyan toocreate hello Panda Python-csomag az Azure blob-tárolóban tárolt adatok funkciókat."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 676b5fb0-4c89-4516-b3a8-e78ae3ca078d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: 8594046c5d76a36ad87fc77e407752489d30afcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a>Funkciók létrehozása az Azure Blob Storage-adatokból a Pandas használatával
Ez a dokumentum bemutatja, hogyan toocreate funkcióit hello használata az Azure blob-tárolóban tárolt adatok [Pandas](http://pandas.pydata.org/) Python-csomag. Után tagolás hogyan tooload hello adatok Panda adatok keretbe, azt illusztrálja, hogyan toogenerate kategorikus szolgáltatás Python-parancsfájl használata a kijelző értékekkel, és a szolgáltatások dobozolás.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics. Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy az Azure blob storage-fiók létrehozása, és az adatok ott tárolt. Ha utasításokat tooset fiókot van szüksége, tekintse meg [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)

## <a name="load-hello-data-into-a-pandas-data-frame"></a>Pandas adatok keretbe hello adatok betöltése
Rendelés toodo feltárja és kezelheti a DataSet adatkészlet, forrásfájlból hello blob tooa helyi amely majd tölthetők be adatok Pandas keretben kell letölteni. Az alábbiakban hello lépéseket toofollow az eljárás végrehajtásához:

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

## <a name="blob-featuregen"></a>Szolgáltatás létrehozása
hello következő két szakaszok bemutatják, hogyan toogenerate kategorikus szolgáltatások kijelző értékkel, és a dobozolás szolgáltatásokat, Python parancsfájlok segítségével.

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
3. Most hello adat olvasható hello blob használatával hello Azure Machine Learning [és adatokat importálhat](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul az alábbi hello képernyőn látható módon:

![olvasó blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

