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
# <span data-ttu-id="53b88-103"><a name="heading"></a>A speciális elemzés Azure blob-adatok feldolgozása</span><span class="sxs-lookup"><span data-stu-id="53b88-103"><a name="heading"></a>Process Azure blob data with advanced analytics</span></span>
<span data-ttu-id="53b88-104">Ez a dokumentum ismerteti az adatok felfedezése és az Azure Blob storage-ban tárolt adatok előállítása szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="53b88-104">This document covers exploring data and generating features from data stored in Azure Blob storage.</span></span> 

## <a name="load-hello-data-into-a-pandas-data-frame"></a><span data-ttu-id="53b88-105">Pandas adatok keretbe hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="53b88-105">Load hello data into a Pandas data frame</span></span>
<span data-ttu-id="53b88-106">A sorrend tooexplore és kezelheti a DataSet adatkészlet, forrásfájlból hello blob tooa helyi amely majd tölthetők be adatok Pandas keretben kell letölteni.</span><span class="sxs-lookup"><span data-stu-id="53b88-106">In order tooexplore and manipulate a dataset, it must be downloaded from hello blob source tooa local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="53b88-107">Az alábbiakban hello lépéseket toofollow az eljárás végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="53b88-107">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="53b88-108">Az Azure-ból hello adatok letöltése a blob szolgáltatással Python mintakód a következő hello blob.</span><span class="sxs-lookup"><span data-stu-id="53b88-108">Download hello data from Azure blob with hello following sample Python code using blob service.</span></span> <span data-ttu-id="53b88-109">Cserélje le az alábbi kódot hello hello változó saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="53b88-109">Replace hello variable in hello code below with your specific values:</span></span> 
   
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
2. <span data-ttu-id="53b88-110">Hello adatok olvasása a keretbe Pandas adatok-hello a letöltött fájlt.</span><span class="sxs-lookup"><span data-stu-id="53b88-110">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="53b88-111">Most már készen áll a tooexplore hello adatok, és ehhez az adatkészlethez funkcióinak készítése.</span><span class="sxs-lookup"><span data-stu-id="53b88-111">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="53b88-112"><a name="blob-dataexploration"></a>Az adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="53b88-112"><a name="blob-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="53b88-113">Íme néhány példa a Pandas tooexplore adatokat:</span><span class="sxs-lookup"><span data-stu-id="53b88-113">Here are a few examples of ways tooexplore data using Pandas:</span></span>

1. <span data-ttu-id="53b88-114">Vizsgálja meg a sorok és oszlopok hello száma</span><span class="sxs-lookup"><span data-stu-id="53b88-114">Inspect hello number of rows and columns</span></span> 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="53b88-115">Először vizsgálja meg hello vagy utolsó néhány sor hello adatkészlet a következő:</span><span class="sxs-lookup"><span data-stu-id="53b88-115">Inspect hello first or last few rows in hello dataset as below:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="53b88-116">Egyes oszlopok használatával a következő példakód hello importált hello adatok típusának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="53b88-116">Check hello data type each column was imported as using hello following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="53b88-117">Ellenőrizze az alábbiak szerint hello alapszintű hello adatkészlet hello oszlopai statisztikái</span><span class="sxs-lookup"><span data-stu-id="53b88-117">Check hello basic stats for hello columns in hello data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="53b88-118">Tekintse meg minden egyes oszlop értékre bejegyzések hello száma az alábbiak szerint</span><span class="sxs-lookup"><span data-stu-id="53b88-118">Look at hello number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="53b88-119">Hiányzó értékek száma és az egyes oszlopok a következő példakód hello segítségével bejegyzések hello tényleges száma</span><span class="sxs-lookup"><span data-stu-id="53b88-119">Count missing values versus hello actual number of entries in each column using hello following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="53b88-120">Ha egy adott oszlopban a hiányzó értékeket hello adatokban, elvetné azokat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="53b88-120">If you have missing values for a specific column in hello data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="53b88-121">dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =</span><span class="sxs-lookup"><span data-stu-id="53b88-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="53b88-122">Hiányzó értékek egy másik módja tooreplace hello mód függvénnyel van:</span><span class="sxs-lookup"><span data-stu-id="53b88-122">Another way tooreplace missing values is with hello mode function:</span></span>
   
     <span data-ttu-id="53b88-123">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< oszlopnév >: .mode()[0]}) dataframe_blobdata ["< oszlopnév >"]</span><span class="sxs-lookup"><span data-stu-id="53b88-123">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="53b88-124">Hisztogram rajzot változó számú bins tooplot hello terjesztési egy változó létrehozása</span><span class="sxs-lookup"><span data-stu-id="53b88-124">Create a histogram plot using variable number of bins tooplot hello distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="53b88-125">Nézze meg változók egy scatterplot vagy hello beépített korrelációs függvény használatával közötti összefüggések</span><span class="sxs-lookup"><span data-stu-id="53b88-125">Look at correlations between variables using a scatterplot or using hello built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <span data-ttu-id="53b88-126"><a name="blob-featuregen"></a>Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="53b88-126"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="53b88-127">A Microsoft hozhat létre a funkciók Python az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="53b88-127">We can generate features using Python as follows:</span></span>

### <span data-ttu-id="53b88-128"><a name="blob-countfeature"></a>Kijelző értékének alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="53b88-128"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="53b88-129">A kockák szolgáltatásai az alábbiak szerint hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="53b88-129">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="53b88-130">Hello terjesztési hello kategorikus oszlop vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="53b88-130">Inspect hello distribution of hello categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="53b88-131">Az egyes oszlopok értékeinek hello kijelző értékek generálásához</span><span class="sxs-lookup"><span data-stu-id="53b88-131">Generate indicator values for each of hello column values</span></span>
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="53b88-132">Csatlakozás hello kijelző oszlop hello eredeti adatok keret</span><span class="sxs-lookup"><span data-stu-id="53b88-132">Join hello indicator column with hello original data frame</span></span> 
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="53b88-133">Távolítsa el az eredeti változó hello saját magát:</span><span class="sxs-lookup"><span data-stu-id="53b88-133">Remove hello original variable itself:</span></span>
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="53b88-134"><a name="blob-binningfeature"></a>A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="53b88-134"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="53b88-135">Binned szolgáltatások létrehozásának, a Folytatás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="53b88-135">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="53b88-136">Az oszlopok toobin egy numerikus oszlopot a sorozat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53b88-136">Add a sequence of columns toobin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="53b88-137">A dobozolás tooa feladatütemezési változók logikai átalakítása</span><span class="sxs-lookup"><span data-stu-id="53b88-137">Convert binning tooa sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="53b88-138">Végezetül illesztési hello dummy változók biztonsági toohello eredeti adatok keret</span><span class="sxs-lookup"><span data-stu-id="53b88-138">Finally, Join hello dummy variables back toohello original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <span data-ttu-id="53b88-139"><a name="sql-featuregen"></a>Az adatok írásakor biztonsági tooAzure blob és az Azure Machine Learning felhasználása</span><span class="sxs-lookup"><span data-stu-id="53b88-139"><a name="sql-featuregen"></a>Writing data back tooAzure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="53b88-140">Miután felfedezte hello adatokat, és a létrehozott hello szükséges szolgáltatást, hello az adatait feltöltheti (mintát vagy featurized) tooan Azure blob-, és ezért használja fel az Azure Machine Learning hello lépések segítségével: hello további szolgáltatásokat lehet létrehozni Az Azure Machine Learning Studio is.</span><span class="sxs-lookup"><span data-stu-id="53b88-140">After you have explored hello data and created hello necessary features, you can upload hello data (sampled or featurized) tooan Azure blob and consume it in Azure Machine Learning using hello following steps: Note that additional features can be created in hello Azure Machine Learning Studio as well.</span></span> 

1. <span data-ttu-id="53b88-141">Hello adatok keret toolocal fájl írása</span><span class="sxs-lookup"><span data-stu-id="53b88-141">Write hello data frame toolocal file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="53b88-142">Hello adatok tooAzure blob feltöltése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="53b88-142">Upload hello data tooAzure blob as follows:</span></span>
   
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
3. <span data-ttu-id="53b88-143">Most hello adat olvasható hello blob használatával hello Azure Machine Learning [és adatokat importálhat] [ import-data] modul az alábbi hello képernyőn látható módon:</span><span class="sxs-lookup"><span data-stu-id="53b88-143">Now hello data can be read from hello blob using hello Azure Machine Learning [Import Data][import-data] module as shown in hello screen below:</span></span>

![olvasó blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

