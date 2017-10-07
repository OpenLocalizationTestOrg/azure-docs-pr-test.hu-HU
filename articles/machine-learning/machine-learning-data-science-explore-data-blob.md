---
title: "aaaExplore adatok Azure blob-tároló a Pandas |} Microsoft Docs"
description: "Hogyan tooexplore az Azure-ban tárolt adatok blob-tároló Pandas használatával."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 28f3c0aebf2300006066c4b19dcb1f0a76a1deb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Az Azure Blob Storage-ban található adatok megismerése a Pandas használatával
Ez a dokumentum ismerteti, hogyan tooexplore az Azure-ban tárolt adatok blob-tároló segítségével [Pandas](http://pandas.pydata.org/) Python-csomag.

hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toouse eszközök különböző tárolási környezetekben tooexplore adatait tootopics. Ez a feladat ez hello lépés [adatok tudományos folyamat]().

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik:

* Egy Azure storage-fiók létrehozása. Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Az adatok tárolódnak az Azure blob storage-fiók. Ha módosítania kell az utasításokat, lásd: [áthelyezése adatok tooand Azure Storage-ból](../storage/common/storage-moving-data.md)

## <a name="load-hello-data-into-a-pandas-dataframe"></a>Hello adatok betöltése az egy Pandas DataFrame
tooexplore és kezelheti a DataSet adatkészlet, először azt kell tölthető le: hello blob tooa helyi forrásfájlhoz, amelynek tölthetők egy Pandas DataFrame majd. Az alábbiakban hello lépéseket toofollow az eljárás végrehajtásához:

1. Az Azure-ból hello adatok letöltése a Python kódminta blob szolgáltatás használatakor a következő hello blob. A következő kódot az adott értékek hello hello változó helyére: 
   
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

## <a name="blob-dataexploration"></a>Példák az adatok feltárása Pandas használatával
Íme néhány példa a Pandas tooexplore adatokat:

1. Vizsgálja meg a hello **sorok és oszlopok száma** 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Vizsgálja meg** első vagy utolsó néhány hello **sorok** a következő adatkészlet hello:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Ellenőrizze a hello **adattípus** minden oszlop a következő példakód hello használatával lett importálva.
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Ellenőrizze a hello **alapvető statisztikák** a hello oszlopok a következő hello adatkészlet
   
        dataframe_blobdata.describe()
5. Tekintse meg minden egyes oszlop értékre bejegyzések hello száma az alábbiak szerint
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Hiányzó értékek száma** és az egyes oszlopok a következő példakód hello segítségével bejegyzések hello tényleges száma
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Ha rendelkezik **hiányzó értékek** egy adott oszlop hello adatokat, akkor dobhatja el őket az alábbiak szerint:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Hiányzó értékek egy másik módja tooreplace hello mód függvénnyel van:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< oszlopnév >: .mode()[0]}) dataframe_blobdata ["< oszlopnév >"]        
8. Hozzon létre egy **hisztogram** használatával bins tooplot hello terjesztési egy változó változó száma ábrázolásához    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Nézze meg **korrelációk** közötti változók egy scatterplot vagy hello beépített korrelációs függvény használatával
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

