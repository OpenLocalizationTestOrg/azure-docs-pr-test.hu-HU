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
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a><span data-ttu-id="24e90-103">Funkciók létrehozása az Azure Blob Storage-adatokból a Pandas használatával</span><span class="sxs-lookup"><span data-stu-id="24e90-103">Create features for Azure blob storage data using Panda</span></span>
<span data-ttu-id="24e90-104">Ez a dokumentum bemutatja, hogyan toocreate funkcióit hello használata az Azure blob-tárolóban tárolt adatok [Pandas](http://pandas.pydata.org/) Python-csomag.</span><span class="sxs-lookup"><span data-stu-id="24e90-104">This document shows how toocreate features for data that is stored in Azure blob container using hello [Pandas](http://pandas.pydata.org/) Python package.</span></span> <span data-ttu-id="24e90-105">Után tagolás hogyan tooload hello adatok Panda adatok keretbe, azt illusztrálja, hogyan toogenerate kategorikus szolgáltatás Python-parancsfájl használata a kijelző értékekkel, és a szolgáltatások dobozolás.</span><span class="sxs-lookup"><span data-stu-id="24e90-105">After outlining how tooload hello data into a Panda data frame, it shows how toogenerate categorical features using Python scripts with indicator values and binning features.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="24e90-106">Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics.</span><span class="sxs-lookup"><span data-stu-id="24e90-106">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="24e90-107">Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="24e90-107">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24e90-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="24e90-108">Prerequisites</span></span>
<span data-ttu-id="24e90-109">Ez a cikk feltételezi, hogy az Azure blob storage-fiók létrehozása, és az adatok ott tárolt.</span><span class="sxs-lookup"><span data-stu-id="24e90-109">This article assumes that you have created an Azure blob storage account and have stored your data there.</span></span> <span data-ttu-id="24e90-110">Ha utasításokat tooset fiókot van szüksége, tekintse meg [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="24e90-110">If you need instructions tooset up an account, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>

## <a name="load-hello-data-into-a-pandas-data-frame"></a><span data-ttu-id="24e90-111">Pandas adatok keretbe hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="24e90-111">Load hello data into a Pandas data frame</span></span>
<span data-ttu-id="24e90-112">Rendelés toodo feltárja és kezelheti a DataSet adatkészlet, forrásfájlból hello blob tooa helyi amely majd tölthetők be adatok Pandas keretben kell letölteni.</span><span class="sxs-lookup"><span data-stu-id="24e90-112">In order toodo explore and manipulate a dataset, it must be downloaded from hello blob source tooa local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="24e90-113">Az alábbiakban hello lépéseket toofollow az eljárás végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="24e90-113">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="24e90-114">Az Azure-ból hello adatok letöltése a blob szolgáltatással Python mintakód a következő hello blob.</span><span class="sxs-lookup"><span data-stu-id="24e90-114">Download hello data from Azure blob with hello following sample Python code using blob service.</span></span> <span data-ttu-id="24e90-115">Cserélje le az alábbi kódot hello hello változó saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="24e90-115">Replace hello variable in hello code below with your specific values:</span></span>
   
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
2. <span data-ttu-id="24e90-116">Hello adatok olvasása a keretbe Pandas adatok-hello a letöltött fájlt.</span><span class="sxs-lookup"><span data-stu-id="24e90-116">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="24e90-117">Most már készen áll a tooexplore hello adatok, és ehhez az adatkészlethez funkcióinak készítése.</span><span class="sxs-lookup"><span data-stu-id="24e90-117">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="24e90-118"><a name="blob-featuregen"></a>Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="24e90-118"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="24e90-119">hello következő két szakaszok bemutatják, hogyan toogenerate kategorikus szolgáltatások kijelző értékkel, és a dobozolás szolgáltatásokat, Python parancsfájlok segítségével.</span><span class="sxs-lookup"><span data-stu-id="24e90-119">hello next two sections show how toogenerate categorical features with indicator values and binning features using Python scripts.</span></span>

### <span data-ttu-id="24e90-120"><a name="blob-countfeature"></a>Kijelző értékének alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="24e90-120"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="24e90-121">A kockák szolgáltatásai az alábbiak szerint hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="24e90-121">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="24e90-122">Hello terjesztési hello kategorikus oszlop vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="24e90-122">Inspect hello distribution of hello categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="24e90-123">Az egyes oszlopok értékeinek hello kijelző értékek generálásához</span><span class="sxs-lookup"><span data-stu-id="24e90-123">Generate indicator values for each of hello column values</span></span>
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="24e90-124">Csatlakozás hello kijelző oszlop hello eredeti adatok keret</span><span class="sxs-lookup"><span data-stu-id="24e90-124">Join hello indicator column with hello original data frame</span></span>
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="24e90-125">Távolítsa el az eredeti változó hello saját magát:</span><span class="sxs-lookup"><span data-stu-id="24e90-125">Remove hello original variable itself:</span></span>
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="24e90-126"><a name="blob-binningfeature"></a>A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="24e90-126"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="24e90-127">Binned szolgáltatások létrehozásának, a Folytatás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="24e90-127">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="24e90-128">Az oszlopok toobin egy numerikus oszlopot a sorozat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24e90-128">Add a sequence of columns toobin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="24e90-129">A dobozolás tooa feladatütemezési változók logikai átalakítása</span><span class="sxs-lookup"><span data-stu-id="24e90-129">Convert binning tooa sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="24e90-130">Végezetül illesztési hello dummy változók biztonsági toohello eredeti adatok keret</span><span class="sxs-lookup"><span data-stu-id="24e90-130">Finally, Join hello dummy variables back toohello original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <span data-ttu-id="24e90-131"><a name="sql-featuregen"></a>Az adatok írásakor biztonsági tooAzure blob és az Azure Machine Learning felhasználása</span><span class="sxs-lookup"><span data-stu-id="24e90-131"><a name="sql-featuregen"></a>Writing data back tooAzure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="24e90-132">Miután felfedezte hello adatokat, és a létrehozott hello szükséges szolgáltatást, hello az adatait feltöltheti (mintát vagy featurized) tooan Azure blob-, és ezért használja fel az Azure Machine Learning hello lépések segítségével: hello további szolgáltatásokat lehet létrehozni Az Azure Machine Learning Studio is.</span><span class="sxs-lookup"><span data-stu-id="24e90-132">After you have explored hello data and created hello necessary features, you can upload hello data (sampled or featurized) tooan Azure blob and consume it in Azure Machine Learning using hello following steps: Note that additional features can be created in hello Azure Machine Learning Studio as well.</span></span>

1. <span data-ttu-id="24e90-133">Hello adatok keret toolocal fájl írása</span><span class="sxs-lookup"><span data-stu-id="24e90-133">Write hello data frame toolocal file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="24e90-134">Hello adatok tooAzure blob feltöltése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="24e90-134">Upload hello data tooAzure blob as follows:</span></span>
   
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
3. <span data-ttu-id="24e90-135">Most hello adat olvasható hello blob használatával hello Azure Machine Learning [és adatokat importálhat](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul az alábbi hello képernyőn látható módon:</span><span class="sxs-lookup"><span data-stu-id="24e90-135">Now hello data can be read from hello blob using hello Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module as shown in hello screen below:</span></span>

![olvasó blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

