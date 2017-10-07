---
title: aaaSample adatok Azure blob storage |} Microsoft Docs
description: Az Azure Blob Storage mintaadatokat
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8d9ad2c-86c5-43d6-80b8-d355b5c0dccf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: cceadf1fb1fb4804fc5b5a3da55c82854651026e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="bebd4-103"><a name="heading"></a>A mintaadatok az Azure blob-tároló</span><span class="sxs-lookup"><span data-stu-id="bebd4-103"><a name="heading"></a>Sample data in Azure blob storage</span></span>
<span data-ttu-id="bebd4-104">Ez a dokumentum programozott módon letöltheti, és ezután mintavételi pythonban írt eljárások használatával az Azure blob storage szolgáltatásban tárolt mintavételi adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bebd4-104">This document covers sampling data stored in Azure blob storage by downloading it programmatically and then sampling it using procedures written in Python.</span></span>

<span data-ttu-id="bebd4-105">hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik tootopics hogyan toosample adatait tároló különböző környezetekben.</span><span class="sxs-lookup"><span data-stu-id="bebd4-105">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="bebd4-106">**Miért érdemes az az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="bebd4-106">**Why sample your data?**</span></span>
<span data-ttu-id="bebd4-107">Ha azt tervezi, hogy tooanalyze hello adatkészlet túl nagy, a rendszer általában egy jó ötlet toodown-minta hello adatok tooreduce azt tooa kisebb, de reprezentatív és könnyebben kezelhető méretét.</span><span class="sxs-lookup"><span data-stu-id="bebd4-107">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="bebd4-108">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="bebd4-108">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="bebd4-109">Hello Cortana Analytics folyamat a szerepköre tooenable gyors prototípusának hello adatfeldolgozási funkciók és a gépi tanulási modellek.</span><span class="sxs-lookup"><span data-stu-id="bebd4-109">Its role in hello Cortana Analytics Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="bebd4-110">Ez a mintavételi feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="bebd4-110">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="download-and-down-sample-data"></a><span data-ttu-id="bebd4-111">Töltse le és down kétmintás adatok</span><span class="sxs-lookup"><span data-stu-id="bebd4-111">Download and down-sample data</span></span>
1. <span data-ttu-id="bebd4-112">Töltse le a hello adatokat az Azure blob storage hello blob szolgáltatás használatát a következő példakód Python hello:</span><span class="sxs-lookup"><span data-stu-id="bebd4-112">Download hello data from Azure blob storage using hello blob service from hello following sample Python code:</span></span> 
   
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

2. <span data-ttu-id="bebd4-113">Olvassa el az adatok keretbe Pandas adatok-fent letöltött hello fájlból.</span><span class="sxs-lookup"><span data-stu-id="bebd4-113">Read data into a Pandas data-frame from hello file downloaded above.</span></span>
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. <span data-ttu-id="bebd4-114">Lefelé-minta hello adatok hello `numpy`tartozó `random.choice` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="bebd4-114">Down-sample hello data using hello `numpy`'s `random.choice` as follows:</span></span>
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

<span data-ttu-id="bebd4-115">Most dolgozhat hello hello 1 százalék példával további feltárására és a szolgáltatás létrehozása az adatok keret felett.</span><span class="sxs-lookup"><span data-stu-id="bebd4-115">Now you can work with hello above data frame with hello 1 Percent sample for further exploration and feature generation.</span></span>

## <span data-ttu-id="bebd4-116"><a name="heading"></a>Adatok feltöltése és olvassa el, az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="bebd4-116"><a name="heading"></a>Upload data and read it into Azure Machine Learning</span></span>
<span data-ttu-id="bebd4-117">A következő kód a minta toodown hello mintaadatok hello használja, és közvetlenül az Azure Machine Learning használni:</span><span class="sxs-lookup"><span data-stu-id="bebd4-117">You can use hello following sample code toodown-sample hello data and use it directly in Azure Machine Learning:</span></span>

1. <span data-ttu-id="bebd4-118">Hello adatok keret tooa helyi fájl írása</span><span class="sxs-lookup"><span data-stu-id="bebd4-118">Write hello data frame tooa local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. <span data-ttu-id="bebd4-119">Töltse fel a hello helyi fájl tooan Azure blob, a következő példakód hello használata:</span><span class="sxs-lookup"><span data-stu-id="bebd4-119">Upload hello local file tooan Azure blob using hello following sample code:</span></span>
   
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
            print ("Something went wrong with uploading toohello blob:"+ BLOBNAME)

3. <span data-ttu-id="bebd4-120">Hello adatokat olvasni az Azure Machine Learning segítségével az Azure blob hello [és adatokat importálhat](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) az alábbi hello ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="bebd4-120">Read hello data from hello Azure blob using Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) as shown in hello image below:</span></span>

![olvasó blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

