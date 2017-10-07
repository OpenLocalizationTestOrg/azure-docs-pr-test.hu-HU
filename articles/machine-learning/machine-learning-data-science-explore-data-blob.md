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
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a><span data-ttu-id="50721-103">Az Azure Blob Storage-ban található adatok megismerése a Pandas használatával</span><span class="sxs-lookup"><span data-stu-id="50721-103">Explore data in Azure blob storage with Pandas</span></span>
<span data-ttu-id="50721-104">Ez a dokumentum ismerteti, hogyan tooexplore az Azure-ban tárolt adatok blob-tároló segítségével [Pandas](http://pandas.pydata.org/) Python-csomag.</span><span class="sxs-lookup"><span data-stu-id="50721-104">This document covers how tooexplore data that is stored in Azure blob container using [Pandas](http://pandas.pydata.org/) Python package.</span></span>

<span data-ttu-id="50721-105">hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toouse eszközök különböző tárolási környezetekben tooexplore adatait tootopics.</span><span class="sxs-lookup"><span data-stu-id="50721-105">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span> <span data-ttu-id="50721-106">Ez a feladat ez hello lépés [adatok tudományos folyamat]().</span><span class="sxs-lookup"><span data-stu-id="50721-106">This task is a step in hello [Data Science Process]().</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="50721-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="50721-107">Prerequisites</span></span>
<span data-ttu-id="50721-108">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="50721-108">This article assumes that you have:</span></span>

* <span data-ttu-id="50721-109">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="50721-109">Created an Azure storage account.</span></span> <span data-ttu-id="50721-110">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="50721-110">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="50721-111">Az adatok tárolódnak az Azure blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="50721-111">Stored your data in an Azure blob storage account.</span></span> <span data-ttu-id="50721-112">Ha módosítania kell az utasításokat, lásd: [áthelyezése adatok tooand Azure Storage-ból](../storage/common/storage-moving-data.md)</span><span class="sxs-lookup"><span data-stu-id="50721-112">If you need instructions, see [Moving data tooand from Azure Storage](../storage/common/storage-moving-data.md)</span></span>

## <a name="load-hello-data-into-a-pandas-dataframe"></a><span data-ttu-id="50721-113">Hello adatok betöltése az egy Pandas DataFrame</span><span class="sxs-lookup"><span data-stu-id="50721-113">Load hello data into a Pandas DataFrame</span></span>
<span data-ttu-id="50721-114">tooexplore és kezelheti a DataSet adatkészlet, először azt kell tölthető le: hello blob tooa helyi forrásfájlhoz, amelynek tölthetők egy Pandas DataFrame majd.</span><span class="sxs-lookup"><span data-stu-id="50721-114">tooexplore and manipulate a dataset, it must first be downloaded from hello blob source tooa local file, which can then be loaded in a Pandas DataFrame.</span></span> <span data-ttu-id="50721-115">Az alábbiakban hello lépéseket toofollow az eljárás végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="50721-115">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="50721-116">Az Azure-ból hello adatok letöltése a Python kódminta blob szolgáltatás használatakor a következő hello blob.</span><span class="sxs-lookup"><span data-stu-id="50721-116">Download hello data from Azure blob with hello following Python code sample using blob service.</span></span> <span data-ttu-id="50721-117">A következő kódot az adott értékek hello hello változó helyére:</span><span class="sxs-lookup"><span data-stu-id="50721-117">Replace hello variable in hello following code with your specific values:</span></span> 
   
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
2. <span data-ttu-id="50721-118">Hello adatok olvasása a keretbe Pandas adatok-hello a letöltött fájlt.</span><span class="sxs-lookup"><span data-stu-id="50721-118">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="50721-119">Most már készen áll a tooexplore hello adatok, és ehhez az adatkészlethez funkcióinak készítése.</span><span class="sxs-lookup"><span data-stu-id="50721-119">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="50721-120"><a name="blob-dataexploration"></a>Példák az adatok feltárása Pandas használatával</span><span class="sxs-lookup"><span data-stu-id="50721-120"><a name="blob-dataexploration"></a>Examples of data exploration using Pandas</span></span>
<span data-ttu-id="50721-121">Íme néhány példa a Pandas tooexplore adatokat:</span><span class="sxs-lookup"><span data-stu-id="50721-121">Here are a few examples of ways tooexplore data using Pandas:</span></span>

1. <span data-ttu-id="50721-122">Vizsgálja meg a hello **sorok és oszlopok száma**</span><span class="sxs-lookup"><span data-stu-id="50721-122">Inspect hello **number of rows and columns**</span></span> 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="50721-123">**Vizsgálja meg** első vagy utolsó néhány hello **sorok** a következő adatkészlet hello:</span><span class="sxs-lookup"><span data-stu-id="50721-123">**Inspect** hello first or last few **rows** in hello following dataset:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="50721-124">Ellenőrizze a hello **adattípus** minden oszlop a következő példakód hello használatával lett importálva.</span><span class="sxs-lookup"><span data-stu-id="50721-124">Check hello **data type** each column was imported as using hello following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="50721-125">Ellenőrizze a hello **alapvető statisztikák** a hello oszlopok a következő hello adatkészlet</span><span class="sxs-lookup"><span data-stu-id="50721-125">Check hello **basic stats** for hello columns in hello data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="50721-126">Tekintse meg minden egyes oszlop értékre bejegyzések hello száma az alábbiak szerint</span><span class="sxs-lookup"><span data-stu-id="50721-126">Look at hello number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="50721-127">**Hiányzó értékek száma** és az egyes oszlopok a következő példakód hello segítségével bejegyzések hello tényleges száma</span><span class="sxs-lookup"><span data-stu-id="50721-127">**Count missing values** versus hello actual number of entries in each column using hello following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="50721-128">Ha rendelkezik **hiányzó értékek** egy adott oszlop hello adatokat, akkor dobhatja el őket az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="50721-128">If you have **missing values** for a specific column in hello data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="50721-129">dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =</span><span class="sxs-lookup"><span data-stu-id="50721-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="50721-130">Hiányzó értékek egy másik módja tooreplace hello mód függvénnyel van:</span><span class="sxs-lookup"><span data-stu-id="50721-130">Another way tooreplace missing values is with hello mode function:</span></span>
   
     <span data-ttu-id="50721-131">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< oszlopnév >: .mode()[0]}) dataframe_blobdata ["< oszlopnév >"]</span><span class="sxs-lookup"><span data-stu-id="50721-131">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="50721-132">Hozzon létre egy **hisztogram** használatával bins tooplot hello terjesztési egy változó változó száma ábrázolásához</span><span class="sxs-lookup"><span data-stu-id="50721-132">Create a **histogram** plot using variable number of bins tooplot hello distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="50721-133">Nézze meg **korrelációk** közötti változók egy scatterplot vagy hello beépített korrelációs függvény használatával</span><span class="sxs-lookup"><span data-stu-id="50721-133">Look at **correlations** between variables using a scatterplot or using hello built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

