---
title: "egyéni tevékenységeket aaaUse egy Azure Data Factory-folyamat"
description: "Megtudhatja, hogyan toocreate egyéni tevékenységeket és az Azure Data Factory-folyamat használja őket."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 23e33727b2160541ab40938ffd911fdd484b3daa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Egyéni tevékenységek használata Azure Data Factory-folyamatban

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-tevékenység](data-factory-hive-activity.md) 
> * [A Pig-tevékenység](data-factory-pig-activity.md)
> * [MapReduce művelethez](data-factory-map-reduce.md)
> * [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
> * [A Spark-tevékenység](data-factory-spark.md)
> * [Machine Learning kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update-erőforrástevékenység](data-factory-azure-ml-update-resource-activity.md)
> * [Tárolt eljárási tevékenység](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md)
> * [.NET egyéni tevékenység](data-factory-use-custom-activities.md)

Egy Azure Data Factory-folyamathoz használható tevékenységeknek két típusa van.

- [Adatok mozgása tevékenységek](data-factory-data-movement-activities.md) toomove adatok között [támogatott forrás és a fogadó adattárolókhoz](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) tootransform-adatokat, például az Azure HDInsight, az Azure Batch és az Azure Machine Learning szolgáltatás számítási. 

toomove adatok/egy adattárból, amely nem támogatja a Data Factory, hozzon létre egy **egyéni tevékenység** saját adatok mozgása logika és -felhasználási hello tevékenységet egy folyamaton belül. Hasonlóképpen tootransform/folyamat adatokat úgy, hogy a Data Factory nem támogatja egyéni tevékenység létrehozása a saját adatok átalakítása logikával, és hello tevékenység a folyamat. 

Egy egyéni tevékenység toorun konfigurálható egy **Azure Batch** készlete virtuális gépek vagy a Windows-alapú **Azure HDInsight** fürt. Azure Batch használatakor csak egy meglévő Azure Batch-készlet is használhatja. Mivel a HDInsight használata esetén használható egy meglévő HDInsight-fürtre vagy olyan fürt, amely automatikusan jön létre, igény szerinti futásidőben.  

hello következő forgatókönyv részletesen bemutatja egy egyéni .NET tevékenység létrehozásáért és hello egyéni tevékenység egy folyamaton belül. hello forgatókönyv egy **Azure Batch** társított szolgáltatás. inkább a társított szolgáltatásnak az Azure HDInsight toouse, típusú társított szolgáltatás létrehozása **HDInsight** (egyéni HDInsight-fürt) vagy **HDInsightOnDemand** (Data Factory létrehoz egy HDInsight-fürt igény szerinti). Ezt követően konfigurálja az egyéni tevékenység toouse hello HDInsight társított szolgáltatás. Lásd: [használata Azure HDInsight összekapcsolt szolgáltatások](#use-hdinsight-compute-service) című szakaszban talál információt az Azure HDInsight toorun hello egyéni tevékenység.

> [!IMPORTANT]
> - hello egyéni .NET tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön. Ez a korlátozás az áthidaló toouse hello térkép csökkentése tevékenység toorun egyéni Java-kód egy Linux-alapú HDInsight-fürtön. Egy másik lehetőség a virtuális gépek toorun egyéni tevékenységeket a HDInsight-fürt használata helyett az Azure Batch-készlet toouse.
> - Már nem lehetséges toouse egy egyéni tevékenység tooaccess a helyszíni adatforrások az adatkezelési átjárót. Jelenleg [az adatkezelési átjáró](data-factory-data-management-gateway.md) csak a hello másolási tevékenység és a tárolt eljárási tevékenység támogatja az adat-előállítóban.   

## <a name="walkthrough-create-a-custom-activity"></a>Forgatókönyv: egyéni tevékenység létrehozása
### <a name="prerequisites"></a>Előfeltételek
* A Visual Studio 2012/2013 vagy 2015
* Az [Azure .NET SDK](https://azure.microsoft.com/downloads/) letöltése és telepítése.

### <a name="azure-batch-prerequisites"></a>Azure Batch-Előfeltételek
Hello forgatókönyv futtatása az egyéni .NET-tevékenységek használata az Azure Batch számítási erőforrásként. **Az Azure Batch** van egy, a nagyméretű párhuzamosan futó szolgáltatást és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan hello felhőben. Az Azure Batch számítási igényű munkahelyi toorun a egy felügyelt ütemezi **virtuális gépek**, és képes automatikusan méretezési számítási erőforrások toomeet hello igényeinek feladatot. Lásd: [Azure Batch alapjai] [ batch-technical-overview] hello Azure Batch szolgáltatás részletes áttekintés cikkében.

Hello az oktatóanyagban az Azure Batch-fiók létrehozása virtuális gépek készletét. Az alábbiakban hello lépéseket:

1. Hozzon létre egy **Azure Batch-fiók** hello segítségével [Azure-portálon](http://portal.azure.com). Lásd: [létrehozása és kezelése az Azure Batch-fiók] [ batch-create-account] miként.
2. Jegyezze fel hello Azure Batch-fiók neve, a fiókkulcsot, URI és az alkalmazáskészlet neve. Egy csatolt Azure Batch szolgáltatás toocreate kell őket.
    1. Kezdőlapon hello Azure Batch-fiókhoz, megjelenik egy **URL-cím** a formátum a következő hello: `https://myaccount.westus.batch.azure.com`. Ebben a példában **myaccount** hello hello Azure Batch-fiók neve. Használhatja a kapcsolódó hello szolgáltatásdefiníció URI megadása hello URL-cím hello fiók hello név nélkül. Például: `https://<region>.batch.azure.com`.
    2. Kattintson a **kulcsok** hello bal oldali menüben, és az másolási hello **elsődleges ELÉRÉSI kulcsot**.
    3. Kattintson egy meglévő készlet toouse **készletek** hello menüben, és jegyezze fel a hello **azonosító** hello készlet. Ha egy meglévő készlet nem rendelkezik, helyezze át a toohello következő lépésre.     
2. Hozzon létre egy **Azure Batch-készlet**.

   1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **Tallózás** a bal oldali menü hello, és kattintson a **Batch-fiókok**.
   2. Válassza ki az Azure Batch-fiók tooopen hello **Batch-fiók** panelen.
   3. Kattintson a **készletek** csempére.
   4. A hello **készletek** panelen hello eszköztár tooadd a készlet hozzáadása gombra.
      1. Adja meg az azonosító hello csoport (alkalmazáskészlet azonosítója). Megjegyzés: hello **hello készlet azonosítója**; amikor hello adat-előállító megoldás létrehozásakor.
      2. Adja meg **Windows Server 2012 R2** hello operációsrendszer-családot beállítás.
      3. Válassza ki a **csomóponti tarifacsomagot**.
      4. Adja meg **2** hello értékként **cél dedikált** beállítást.
      5. Adja meg **2** hello értékként **csomópontonkénti tevékenységek maximális** beállítást.
   5. Kattintson a **OK** toocreate hello készlet.
   6. Jegyezze fel a hello **azonosító** hello készlet. 



### <a name="high-level-steps"></a>Magas szintű lépései
Az alábbiakban a két magas szintű lépései hello elvégezhető ez a forgatókönyv részeként: 

1. Egyszerű adat-átalakítási/feldolgozó logika tartalmazó egyéni tevékenység létrehozása.
2. Hozzon létre egy Azure data factory egy folyamatot, amely hello egyéni tevékenység használja.

### <a name="create-a-custom-activity"></a>Egyéni tevékenység létrehozása
toocreate .NET egyéni tevékenység, hozzon létre egy **.NET Class Library** egy osztály, amely megvalósítja az, hogy a projekt **IDotNetActivity** felületet. Ez az interfész csak egy metódusa: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) , és az aláírása:

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


hello metódus négy paramétereket fogadja:

- **linkedServices**. Ez a tulajdonság egy-egy adattároló kapcsolódó szolgáltatások bemeneti/kimeneti adatkészletek hello tevékenység által hivatkozott enumerálható lista.   
- **adatkészletek**. Ez a tulajdonság egy bemeneti/kimeneti adatkészletek hello tevékenység enumerálható listája. A paraméter tooget hello helyek és -sémákat határozzák meg a bemeneti és kimeneti adathalmazokat is használhatja.
- **tevékenység**. Ez a tulajdonság aktuális tevékenység hello jelöli. További tulajdonságok hello egyéni tevékenység társított használt tooaccess lehet. Lásd: [további tulajdonságok hozzáférés](#access-extended-properties) részleteiről.
- **naplózó**. Ez az objektum lehetővé teszi a felhasználó bejelentkezése hello hello adatcsatorna az adott felület hibakeresési megjegyzések írását.

hello metódus, amely lehet használt toochain egyéni tevékenységek együtt hello jövőben dictionary adja vissza. Ez a funkció még nem használható, így egy üres szótár visszaadásának hello metódus.  

### <a name="procedure"></a>Eljárás
1. Hozzon létre egy **.NET Class Library** projekt.
   <ol type="a">
     <li>Indítsa el <b>Visual Studio 2017</b> vagy <b>Visual Studio 2015-öt</b> vagy <b>Visual Studio 2013</b> vagy <b>Visual Studio 2012</b>.</li>
     <li>Kattintson a <b>fájl</b>, pont túl<b>új</b>, és kattintson a <b>projekt</b>.</li>
     <li>Bontsa ki a <b>Sablonok</b> lehetőséget, és válassza a <b>Visual C#</b> lehetőséget. Ebben a bemutatóban használhat C#, de használhatja a .NET nyelvi toodevelop hello egyéni tevékenység.</li>
     <li>Válassza ki <b>Class Library</b> jobb hello projekttípusok hello listája. VS 2017, válassza a <b>Class Library (.NET-keretrendszer)</b> </li>
     <li>Adja meg <b>MyDotNetActivity</b> a hello <b>neve</b>.</li>
     <li>Válassza ki <b>C:\ADFGetStarted</b> a hello <b>hely</b>.</li>
     <li>Kattintson a <b>OK</b> toocreate hello projekt.</li>
   </ol>
2.Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.
3. A Csomagkezelő konzol hello, hajtsa végre a következő parancs tooimport hello **Microsoft.Azure.Management.DataFactories**.

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importálás hello **Azure Storage** toohello projekt NuGet-csomagot.

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Data Factory szolgáltatás indítója windowsazure.Storage kifejezésre hello 4.3 verziója szükséges. Ha egy hivatkozás tooa az egyéni tevékenység projektben Azure Storage szerelvény újabb verziója, hibaüzenet jelenik meg hello tevékenység végrehajtásakor. tooresolve hello hiba, lásd: [Appdomain elkülönítési](#appdomain-isolation) szakasz. 
5. Adja hozzá a következő hello **használatával** utasítások toohello forrásfájl hello projektben.

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Hello nevének módosítása a hello **névtér** túl**MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Hello osztály hello nevének módosítása túl**MyDotNetActivity** , és amelyek hello **IDotNetActivity** csatoló, ahogy az a következő kódrészletet hello:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Megvalósítása (Hozzáadás) hello **Execute** hello metódusában **IDotNetActivity** toohello csatoló **MyDotNetActivity** minta kód toohello metódus a következő osztály és példány hello.

    hello következő minta található hello hello keresési kifejezés ("Microsoft") előfordulásainak száma az adatok szelet társított minden egyes blob.

    ```csharp
    /// <summary>
    /// Execute method is hello only method of IDotNetActivity interface you must implement.
    /// In this sample, hello method invokes hello Calculate method tooperform hello core logic.  
    /// </summary>
    
    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // toolog information, use hello logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get hello input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables toohold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from hello dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and hello other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get hello first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using hello same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get hello connection string in hello linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get hello folder path from hello input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.
    
        // create storage client for input. Pass hello connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
        // initialize hello continuation token before using it in hello do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get hello list of input blobs from hello input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);
    
            // Calculate method returns hello number of occurrences of
            // hello search term (“Microsoft”) in each blob associated
               // with hello data slice. definition of hello method is shown in hello next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for hello output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get hello folder path from hello output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log hello output folder path 
        logger.Write("Writing blob toohello folder: {0}", folderPath);
    
        // create a storage object for hello output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write hello name of hello file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log hello output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload hello output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} toohello output blob", output);
        outputBlob.UploadText(output);
    
        // hello dictionary can be used toochain custom activities together in hello future.
        // This feature is not implemented yet, so just return an empty dictionary.  
    
        return new Dictionary<string, string>();
    }
    ```
9. Adja hozzá a következő segédmódszereket hello: 

    ```csharp
    /// <summary>
    /// Gets hello folderPath value from hello input/output dataset.
    /// </summary>
    
    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of hello dataset 
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello folder path found in hello type properties
        return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets hello fileName value from hello input/output dataset.   
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }
    
        // get type properties of hello dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello blob/file name in hello type properties
        return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in hello folder, counts hello number of instances of search term in hello file,
    /// and prepares hello output text that is written toohello output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
        string output = string.Empty;
        logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
        foreach (IListBlobItem listBlobItem in Bresult.Results)
        {
            CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
            if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
            {
                string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                logger.Write("input blob text: {0}", blobText);
                string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                var matchQuery = from word in source
                                 where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                 select word;
                int wordCount = matchQuery.Count();
                output += string.Format("{0} occurrences(s) of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    hello GetFolderPath metódus beolvasása hello elérési toohello mappa hello dataset tooand hello GetFileName metódus értéket ad vissza hello hello blob/fájl nevét, amely a DataSet adatkészlet mutat hello mutat. Ha Ön havefolderPath meghatározása változókkal például {Year}, {Month}, {Day} stb., hello metódus beolvasása hello karakterlánc, mert az nem törli őket futásidejű értékekkel. Lásd: [további tulajdonságok hozzáférés](#access-extended-properties) című szakaszban talál információt elérése SliceStart, SliceEnd, stb.    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    hello Calculate metódus kulcsszó Microsoft hello bemeneti fájlok (BLOB hello mappában) a példányok száma hello számítja ki. hello keresési kifejezés ("Microsoft") nem változtatható hello kódban.
10. Fordítsa le hello projektet. Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.

    > [!IMPORTANT]
    > Hello célkeretrendszer a projekthez, .NET-keretrendszer 4.5.2-es set verziója: kattintson a jobb gombbal a projekt hello, és kattintson a **tulajdonságok** tooset hello célkeretrendszer. Adat-előállító nem támogatja a lefordított elleni .NET-keretrendszer verziója 4.5.2 később egyéni tevékenységeket.

11. Indítsa el **Windows Explorer**, és keresse meg a túl**bin\debug** vagy **bin\release** mappa build hello típusától függően.
12. Hozzon létre egy zip fájlt **MyDotNetActivity.zip** hello összes hello bináris fájlokat tartalmazó <project folder>\bin\Debug mappába. Hello tartalmaznak **MyDotNetActivity.pdb** fájlt úgy, hogy be további részletekről, mint a sor számának hello forráskódban, ha hiba történt a hello hibát okozó. 

    > [!IMPORTANT]
    > Az egyéni tevékenység hello el kell érnie hello fájlok hello zip-fájlban szereplő összes hello **felső szintű** nem sub mappák.

    ![Bináris kimeneti fájlok](./media/data-factory-use-custom-activities/Binaries.png)
14. Hozzon létre egy blob-tároló nevű **customactivitycontainer** Ha még nem létezik. 
15. MyDotNetActivity.zip feltöltése a blob toohello customactivitycontainer a, egy **általános célú** AzureStorageLinkedService által hivatkozott Azure blob storage (nem közbeni/ritkán Blob-tároló).  

> [!IMPORTANT]
> Ha a .NET tevékenység projekt tooa megoldás hozzáadása a Visual Studio Data Factory projektet tartalmaz, és adja hozzá egy hivatkozást too.NET tevékenység projekt hello adat-előállító projektet, nem kell tooperform hello utolsó két lépést manuális létrehozása hello zip-fájlt, majd ismét feltölteni a toohello általános célú Azure blob Storage tárolóban. Ha közzéteszi a Visual Studio használatával a Data Factory entitások, ezeket a lépéseket automatikusan végzett hello közzétételi folyamat. További információkért lásd: [adat-előállító projektre a Visual Studio](#data-factory-project-in-visual-studio) szakasz.

## <a name="create-a-pipeline-with-custom-activity"></a>Hozzon létre egy folyamatot egyéni tevékenység
Létrehozott egy egyéni tevékenység és a bináris fájlok tooa blob tároló a hello zip-fájl feltöltése a **általános célú** Azure Storage-fiókot. Ebben a szakaszban hoz létre egy Azure data factory egy folyamatot, amely hello egyéni tevékenység használja.

hello bemeneti adatkészlet hello egyéni tevékenység hello customactivityinput mappában adftutorial tároló hello blob Storage tárolóban található blobok (fájlok) jelöli. hello kimeneti adatkészlet hello tevékenység kimeneti BLOB adftutorial tároló hello blob Storage tárolóban hello customactivityoutput mappában jelöli.

Hozzon létre **file.txt** fájl a következő tartalmat, és töltse fel az túl hello**customactivityinput** mappában található hello **adftutorial** tároló. Hello adftutorial tároló létrehozása, ha még nem létezik. 

```
test custom activity Microsoft test custom activity Microsoft
```

hello bemeneti mappa felel meg az Azure Data Factory tooa szelet, akkor is, ha hello mappa két vagy több fájlt. Minden szelet feldolgozása hello folyamat után hello egyéni tevékenység hello bemeneti mappában, hogy a szeletre vonatkozó összes hello BLOB telepítéseket.

Egy kimeneti fájl hello adftutorial\customactivityoutput mappában (azonos számú BLOB hello bemeneti mappában) egy vagy több sort tartalmazó jelenik meg:

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
```


Az alábbiakban a jelen szakaszban végrehajtandó hello lépések:

1. Hozzon létre egy **adat-előállító**.
2. Hozzon létre **összekapcsolt szolgáltatások** a virtuális gépek melyik hello az egyéni tevékenység lefut, és hello bemeneti/kimeneti BLOB tároló Azure Storage hello hello Azure Batch-készlet.
3. Hozzon létre a bemeneti és kimeneti **adatkészletek** , amelyek megfelelnek a bemeneti és kimeneti hello egyéni tevékenység.
4. Hozzon létre egy **csővezeték** használó hello egyéni tevékenység.

> [!NOTE]
> Hozzon létre hello **file.txt** , és töltse fel tooa blob tároló Ha még nem tette meg. A szakasz fenti hello tudnivalókat.   

### <a name="step-1-create-hello-data-factory"></a>1. lépés: Hello adat-előállító létrehozása
1. Az hello a bejelentkezés után toohello Azure-portálon, a következő lépéseket:
   1. Kattintson a **új** hello bal oldali menüben.
   2. Kattintson a **adatok + analitika** a hello **új** panelen.
   3. Kattintson a **adat-előállító** a hello **adatelemzés** panelen.
   
    ![Új Azure Data Factory menü](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. A hello **új adat-előállító** panelen adja meg **CustomActivityFactory** a hello nevét. az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "CustomActivityFactory"**, adat-előállító hello hello nevének módosítása (például **yournameCustomActivityFactory**), és próbálja meg létrehozni újra.

    ![Új Azure Data Factory panel](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. Kattintson a **ERŐFORRÁSCSOPORT-név**, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot.
4. Győződjön meg arról, hogy helyes-e hello használunk **előfizetés** és **régió** hello data factory toobe létrehozni kívánt.
5. Kattintson a **létrehozása** a hello **új adat-előállító** panelen.
6. Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon.
7. Miután hello adat-előállító létrehozása sikerült, hello adat-előállító panel, amely jelzi, hogy látja hello hello adat-előállító tartalmát.
    
    ![A Data Factory panel](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>2. lépés: A társított szolgáltatások létrehozásához
Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási. Ebben a lépésben csatolja a az Azure Storage-fiók és az Azure Batch tooyour adat-előállítóban.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
1. Kattintson a hello **Szerző és központi telepítése** hello csempét **DATA FACTORY** paneljén **CustomActivityFactory**. Megjelenik a Data Factory Editor hello.
2. Kattintson a **az új adattároló** a hello parancssávon, és válassza a **az Azure storage**. Megtekintheti az hello JSON-parancsfájl létrehozásához egy Azure Storage társított szolgáltatásnak hello-szerkesztőben.
    
    ![Az új adattároló - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Cserélje le `<accountname>` az Azure storage-fiók nevére és `<accountkey>` hello Azure storage-fiók hozzáférési kulcsával. toolearn hogyan tooget tárhelyét férnek hozzá, tekintse meg [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

    ![Az Azure Storage szolgáltatás tetszését](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

#### <a name="create-azure-batch-linked-service"></a>Csatolt Azure Batch szolgáltatás létrehozása
1. A Data Factory Editor hello, kattintson **... További** hello parancssávon kattintson **új számítási**, majd válassza ki **Azure Batch** hello menüből.

    ![Új számítási - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. Hajtsa végre a következő módosításokat toohello JSON-parancsfájl hello:

   1. Adja meg az Azure Batch-fiók nevét a hello **accountName** tulajdonság. Hello **URL-cím** a hello **Azure Batch-fiók panelen** hello formátuma a következő szerepel: `http://accountname.region.batch.azure.com`. A hello **batchUri** tulajdonság hello JSON, tooremove kell `accountname.` a hello URL-cím és -felhasználási hello `accountname` a hello `accountName` JSON tulajdonság.
   2. Adja meg az Azure Batch-fiók kulcsot hello a hello **accessKey** tulajdonság.
   3. Adja meg a létrehozott hello készlet neve hello hello előfeltételei részeként **poolName** tulajdonság. Hello készlet hello hello neve helyett hello azonosító is megadható.
   4. Azure Batch URI megadása hello **batchUri** tulajdonság. Példa: `https://westus.batch.azure.com`.  
   5. Adja meg a hello **AzureStorageLinkedService** a hello **linkedServiceName** tulajdonság.

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       A hello **poolName** tulajdonság, azt is megadhatja, hello azonosító hello készlet hello hello neve helyett.

      > [!IMPORTANT]
      > hello Data Factory szolgáltatásnak nem támogatja egy igény szerinti beállítást az Azure hdinsight azonban nem. Csak a saját Azure Batch-készlet használhatja az Azure data factory.   
    

### <a name="step-3-create-datasets"></a>3. lépés: Adatkészletek létrehozása
Ebben a lépésben létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai.

#### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
1. A hello **szerkesztő** hello adat-előállítót, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, majd válassza ki **Azure Blob Storage tárolóban** hello legördülő menüből.
2. Cserélje le a következő JSON részlet hello hello JSON hello jobb oldali ablaktáblában:

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
         },
         "availability": {
             "frequency": "Hour",
             "interval": 1
         },
         "external": true,
         "policy": {}
     }
    }
    ```

   Ez a forgatókönyv a kezdő időpont későbbi részében hozzon létre egy folyamatot: 2016-11-16T00:00:00Z és záró idő: 2016-11-16T05:00:00Z. Már ütemezett tooproduce adatok óránkénti, így öt bemeneti/kimeneti szeletek (közötti **00**: 00:00 -> **05**: 00:00).

   Hello **gyakoriság** és **időköz** hello bemeneti adatkészlet túl van-e állítva a**óra** és **1**, ami azt jelenti, hogy hello bemeneti szelet érhető el óránként. Az ebben a példában is azonos hello hello intputfolder (file.txt) fájlt.

   Az alábbiakban minden szelet, amely a fenti JSON részlet hello SliceStart rendszerváltozó által képviselt hello kezdési idejét.
3. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **InputDataset**. Ellenőrizze, hogy látható-e hello **tábla sikeresen LÉTREHOZVA** hello címsorában hello szerkesztő-üzeneteket.

#### <a name="create-an-output-dataset"></a>Kimeneti adatkészlet létrehozása
1. A hello **Data Factory editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, majd válassza ki **Azure Blob Storage tárolóban**.
2. Cserélje le a JSON-parancsfájl hello hello jobb oldali hello JSON-parancsfájl a következő:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
                "partitionedBy": [
                    {
                        "name": "slice",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy-MM-dd-HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

     Kimeneti hely **adftutorial/customactivityoutput/** és kimeneti fájlnév: éééé-HH-NN-HH.txt, ha éééé-hh-nn óó hello év, hónap, dátum és óra alatt előállított hello szelet. Lásd: [fejlesztői leírás] [ adf-developer-reference] részleteiről.

    Egy kimeneti blob/fájl az egyes bemeneti szeletek jön létre. Ez hogyan kimeneti fájl neve az egyes szeletek. Minden hello kimeneti fájlok akkor jönnek létre, egy kimeneti mappában: **adftutorial\customactivityoutput**.

   | Szelet | Kezdési idő | Kimeneti fájlja |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Ne feledje, hogy egy bemeneti mappában lévő összes hello fájlt a fent említett hello indítási idejének szelet részét. A szelet feldolgozása után hello egyéni tevékenység minden egyes fájl vizsgálja, és hozza létre a keresési kifejezés ("Microsoft") előfordulásainak száma hello hello kimeneti fájlban egy sor. Ha hello inputfolder három fájlok, vannak-e három sort hello kimeneti fájl az egyes óránkénti szeletek: 2016-11-16:01:00:00.txt 2016-11-16-00.txt, stb.
3. toodeploy hello **OutputDataset**, kattintson a **telepítés** hello parancssávon.

### <a name="create-and-run-a-pipeline-that-uses-hello-custom-activity"></a>Hozzon létre és futtasson egy folyamatot, amely hello egyéni tevékenység használja
1. A Data Factory Editor hello, kattintson **... További**, majd válassza ki **új adatcsatorna** hello parancssávon. 
2. Cserélje le a JSON-parancsfájl a következő hello hello JSON hello jobb oldali ablaktáblában:

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Vegye figyelembe a következő pontok hello:

   * **Egyidejűségi** értéke túl**2** , hogy két szeletek párhuzamosan dolgozza fel a hello Azure Batch-készlet 2 virtuális gép.
   * Egy tevékenység hello tevékenységek szakaszban van, és a típusa: **DotNetActivity**.
   * **AssemblyName** hello DLL toohello neve van beállítva: **MyDotnetActivity.dll**.
   * **EntryPoint** értéke túl**MyDotNetActivityNS.MyDotNetActivity**.
   * **PackageLinkedService** értéke túl**AzureStorageLinkedService** toohello blob-tároló hello egyéni tevékenység zip fájlt tartalmazó mutat. Ha használ különböző Azure Storage-fiókok a bemeneti/kimeneti fájlok és hello egyéni tevékenység zip-fájl, létrehozhat egy másik Azure tárolás társított szolgáltatása. Ez a cikk feltételezi, hogy a hello azonos Azure Storage-fiók.
   * **PackageFile** értéke túl**customactivitycontainer/MyDotNetActivity.zip**. A hello formátumban: containerforthezip/nameofthezip.zip.
   * hello egyéni tevékenység veszi **InputDataset** bemenetként és **OutputDataset** output típusúként.
   * hello linkedServiceName tulajdonsága hello egyéni tevékenység mutat toohello **AzureBatchLinkedService**, amely közli az Azure Data Factory hello egyéni tevékenységre kell toorun Azure Batch virtuális gépeken.
   * **isPaused** tulajdonsága túl**hamis** alapértelmezés szerint. hello csővezeték azonnal futtatja ebben a példában, mert hello szeletek indítsa el a korábbi hello. Ez a tulajdonság tootrue toopause hello folyamat beállítása, és állítsa be úgy a hátsó toofalse toorestart.
   * Hello **start** idő és **end** idők **öt** egymástól óra és szeletek előállítása hourly, így öt szeletek hello csővezeték hozzák létre.
3. toodeploy hello sorban, kattintson a **telepítés** hello parancssávon.

### <a name="monitor-hello-pipeline"></a>A figyelő hello folyamat
1. Hello a Data Factory panelen hello Azure-portálon, kattintson a **Diagram**.

    ![Diagram csempe](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. A Diagram nézet hello Ekkor kattintson a hello OutputDataset.

    ![Diagramnézet](./media/data-factory-use-custom-activities/diagram.png)
3. Meg kell jelennie, hogy hello öt kimeneti szeletek hello kész állapotban van-e. Ha nincsenek hello üzemkész állapotban, hogy még nem készült még. 

   ![Kimeneti szeletek](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Győződjön meg arról, hogy a hello kimeneti fájlok legyenek létrehozva hello blob Storage tárolóban lévő hello **adftutorial** tároló.

   ![egyéni tevékenység kimenetét][image-data-factory-ouput-from-custom-activity]
5. Hello kimeneti fájl megnyitásakor, láthatja a hello kimeneti hasonló toohello kimenete a következő:

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
    ```
6. Használjon hello [Azure-portálon] [ azure-preview-portal] vagy az Azure PowerShell-parancsmagok toomonitor a data factory, folyamatok és adatkészletek. Hello üzeneteit látható **ActivityLogger** hello kódban hello egyéni tevékenység hello-naplók (kifejezetten felhasználói-0.log) tölthető le: hello portál vagy parancsmagok használatával.

   ![az egyéni tevékenység naplók letöltése][image-data-factory-download-logs-from-custom-activity]

Lásd: [figyelő és folyamatok kezelése](data-factory-monitor-manage-pipelines.md) adathalmazok és adatcsatornák figyelemmel kísérésére részletes leírást.      

## <a name="data-factory-project-in-visual-studio"></a>Data Factory projektre a Visual Studióban  
Hozzon létre, és tegye közzé a Data Factory entitásokat Visual Studio használatával Azure portál használata helyett. Részletes információkat létrehozása és közzététele a Data Factory entitások Visual Studio használatával, lásd: [felépítheti első folyamatát Visual Studio használatával](data-factory-build-your-first-pipeline-using-vs.md) és [adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) cikkek.

További lépések Data Factory-projekt létrehozása a Visual Studio hello:
 
1. Adja hozzá a hello adat-előállító projekt toohello Visual Studio megoldás, amely hello egyéni tevékenység projekt tartalmazza. 
2. Adja hozzá egy hivatkozást toohello .NET tevékenység projekt hello adat-előállító projekt. Jobb gombbal az adat-előállító projekt túl**Hozzáadás**, és kattintson a **hivatkozás**. 
3. A hello **hivatkozás hozzáadása** párbeszédpanel megnyitásához, jelölje be hello **MyDotNetActivity** projektre, majd kattintson az **OK**.
4. Hozza létre és hello megoldás közzététele.

    > [!IMPORTANT]
    > Ha közzéteszi a Data Factory entitások, automatikusan létrejön egy zip-fájlt, és feltöltött toohello blob tároló: customactivitycontainer. Ha hello blob tároló nem létezik, automatikusan létrejön túl.  


## <a name="data-factory-and-batch-integration"></a>Adat-előállító és kötegelt integrációja
hello Data Factory szolgáltatásnak hoz létre egy feladatot az Azure Batch névvel hello: **adf-poolname: feladat-xxx**. Kattintson a **feladatok** hello bal oldali menüből. 

![Az Azure Data Factory - kötegelt feladatok](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Egy feladat minden egyes tevékenység futtatásához a szelet jön létre. Ha öt szeletek készen toobe feldolgozott, ez a feladat öt feladatok jönnek létre. Ha több számítási csomópontok szerepelnek hello Batch-készlet, két vagy több szeletek párhuzamosan is futtathatja. Ha hello maximális tevékenységek maximális száma számítási csomópont értéke túl > 1 is lehet hello futó egynél több szelet azonos számítási.

![Az Azure Data Factory - kötegelt munka feladatai](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

a következő diagram hello Azure Data Factory és kötegelt feladatok hello kapcsolatát mutatja be.

![Adat-előállító & kötegelt](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Hibák elhárítása
Néhány alapvető technikák áll:

1. Ha megjelenik a következő hiba hello, használhat egy gyakran használt adatok/ritkán blob-tároló egy általános célú Azure blob storage használata helyett. Töltse fel a zip-fájl tooa hello **általános célú Azure Storage-fiók**. 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of hello specified Azure Blob(s).
    ``` 
2. Ha megjelenik a következő hiba hello, győződjön meg arról, hogy a hello CS egyezések hello fájlnév hello megadott hello osztály hello neve **EntryPoint** hello adatcsatorna JSON tulajdonság. Hello forgatókönyv hello osztály neve van: MyDotNetActivity, illetve a hello JSON EntryPoint hello: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement hello type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Ha hello nevei egyeznek, ellenőrizze, hogy minden hello bináris fájlok a hello **gyökérmappa** hello zip-fájl. Ez azt jelenti, hogy hello zip-fájl megnyitásakor, megtekintheti az összes hello fájlok hello gyökérmappában, és nem minden almappa.   
3. Ha hello bemeneti szelet értéke túl**készen**, ellenőrizze a bemeneti mappaszerkezet hello és **file.txt** hello bemeneti mappák szerepel.
3. A hello **Execute** az egyéni tevékenység használja hello metódusában **IActivityLogger** toolog objektuminformáció, amely segít a problémák elhárításához. hello felhasználói naplófájlok megjelennek a naplóba köszönőüzenetei (egy vagy több fájlt nevű: felhasználó-0.log, felhasználó-1.log, felhasználó-2.log, stb.).

   A hello **OutputDataset** panelen hello szelet toosee hello kattintson **ADATSZELET** , hogy a szelet paneljét. Látni **tevékenységek** az adott szeletek. Futtassa a hello szelet tevékenységenként kell megjelennie. Hello parancs sávon kattintson a Futtatás, ha egy másik tevékenységgel, futtassa a hello elindíthatja azonos szelet.

   Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello **tevékenység futtatása részletei** naplófájlok listáját tartalmazó panel. Megjelenik a naplózott üzeneteket hello user_0.log fájlban. Ha hiba történik, megjelenik az három tevékenység fut mert hello újrapróbálkozások száma too3 hello csővezeték/tevékenységben JSON. Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello naplófájlokat, hogy áttekintheti tootroubleshoot hello hiba.

   A naplófájlok hello listájában kattintson a hello **felhasználói-0.log**. Jobb oldali panelen hello vannak hello segítségével hello eredményeit **IActivityLogger.Write** metódust. Ha az összes üzenet nem látható, ellenőrizze, hogy van-e további naplófájlokat nevű: user_1.log, user_2.log stb. Ellenkező esetben hello kód nem sikerült hello utolsó üzenet bejelentkezést követően.

   Ezenkívül ellenőrizze **rendszer-0.log** rendszer hibaüzeneteket és kivételeket.
4. Hello tartalmaznak **PDB** hello zip-fájlban szereplő fájlt, hogy a hello hiba legutolsó részletes adatai információ például **hívási verem** Ha hiba történik.
5. Az egyéni tevékenység hello el kell érnie hello fájlok hello zip-fájlban szereplő összes hello **felső szintű** nem sub mappák.
6. Győződjön meg arról, hogy hello **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip), és **packageLinkedService** (kell mutatnia toohello **általános célú**, amely hello zip-fájl tartalmazza az Azure blob-tároló) toocorrect értékek vannak beállítva.
7. Ha rögzített egy hiba és a kívánt tooreprocess hello szelet, kattintson a jobb gombbal a hello hello szelet **OutputDataset** panel megnyitásához, és kattintson **futtatása**.
8. Ha megjelenik a következő hiba hello, hello Azure Storage csomag verziója > 4.3.0 használ. Data Factory szolgáltatás indítója windowsazure.Storage kifejezésre hello 4.3 verziója szükséges. Lásd: [Appdomain elkülönítési](#appdomain-isolation) szakasz egy munkahelyi körül hello használata Azure Storage-szerelvény újabb verziója. 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by hello target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    Azure Storage csomag hello 4.3.0 verziója használható, ha hello meglévő hivatkozás tooAzure tárolási csomag eltávolításához a verzió > 4.3.0. Ezután futtassa a következő parancsot a NuGet-Csomagkezelő konzolról hello. 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Hello projekt felépítéséhez. A verzió > 4.3.0 Azure.Storage szerelvény hello bin\Debug mappa törlése. Hozzon létre egy zip-fájl bináris fájljait és hello PDB-fájl. Cserélje le a régi zip-fájl hello hello blob-tárolóban (customactivitycontainer) erre. Futtassa újra a műveletet hello szeletek, melyeknél nem sikerült (kattintson a jobb gombbal a szelet, majd kattintson a Futtatás).   
8. hello egyéni tevékenység nem használja a hello **app.config** fájlt a csomagból. Ezért ha a kód a kapcsolati karakterláncok hello konfigurációs fájlból olvassa be, nem működik futásidőben. hello célszerű esetén használja az Azure Batch toohold bármely titkos kulcsainak egy **Azure KeyVault**, használja a tanúsítvány alapú szolgáltatás egyszerű tooprotect hello **keyvault**, és hello tanúsítvány terjesztése tooAzure Batch-készlet. hello .NET egyéni tevékenység majd el titkok hello KeyVault futásidőben. Ez a megoldás egy általános megoldás, és titkos kulcsot, nem csak a kapcsolati karakterlánc típusú tooany méretezheti.

   Van egy egyszerűbb megoldást (de nem ajánlott): hozhat létre egy **Azure SQL társított szolgáltatásnak** rendelkező kapcsolatikarakterlánc-beállításokat, hozzon létre, hogy használ hello társított szolgáltatás, és típusú bemeneti adatkészletként lánc hello dataset adatkészlet Egyéni .NET tevékenység toohello. Ezután a hozzáférés hello kapcsolódó szolgáltatás kapcsolati karakterlánc hello egyéni tevékenység kódban.  

## <a name="update-custom-activity"></a>Egyéni tevékenység frissítése
Hello kód hello egyéni tevékenység frissítésekor összeállítani, és új bináris toohello blob-tároló tartalmazó hello zip-fájl feltöltése.

## <a name="appdomain-isolation"></a>Az alkalmazástartomány elkülönítési
Lásd: [Cross AppDomain minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) , amely bemutatja, hogyan toocreate egyéni tevékenység, amely nem korlátozott hello adat-előállító indítója által használt tooassembly verziók (Példa: windowsazure.Storage kifejezésre v4.3.0, Newtonsoft.Json v6.0.x, stb.).

## <a name="access-extended-properties"></a>További tulajdonságok hozzáférés
Kiterjesztett tulajdonságok deklarálhatnak hello tevékenység JSON látható hello minta a következő módon:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


Hello példában két további tulajdonságainak vannak: **SliceStart** és **DataFactoryName**. hello értéke SliceStart hello SliceStart rendszerváltozó alapul. Lásd: [rendszerváltozók](data-factory-functions-variables.md) támogatott rendszerváltozók listáját. hello DataFactoryName értéke kódolt tooCustomActivityFactory.

tooaccess ezek kiterjesztett tulajdonságok hello **Execute** metódus használata kód hasonló toohello a következő kódot:

```csharp
// tooget extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// toolog all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Az Azure Batch automatikus skálázás
Az Azure Batch-készlet is létrehozhat **automatikus skálázás** szolgáltatás. 0 dedikált virtuális gépek és az automatikus skálázási képlet alapján a függőben lévő feladatok száma hello segítségével például létrehozhat egy azure batch-készlet. 

Itt hello minta képlet éri el a következő viselkedés hello: hello készlet létrehozásakor 1 virtuális gép kezdődik. $PendingTasks metrika meghatározza hello feladatok futtatásának + (aszinkron) aktív állapotban.  hello képlet hello átlagos száma függőben lévő feladatok keresi hello az elmúlt 180 másodperc alatt, és ennek megfelelően állítja be TargetDedicated. Biztosítja, hogy TargetDedicated soha nem túllép 25 virtuális gépeket. Igen új feladatok nyújtják, készlet automatikusan növekszik és feladatok befejezését, mint a virtuális gépek válik a szabad egyenként és hello automatikus skálázás zsugorítja a virtuális gépek. módosított tooyour igényei lehetnek startingNumberOfVMs és maxNumberofVMs.

Automatikus skálázás képlet:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Lásd: [automatikus méretezési számítási csomópontok az Azure Batch-készlet](../batch/batch-automatic-scaling.md) részleteiről.

Ha hello készlet hello alapértelmezett [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch szolgáltatás 15-30 perces tooprepare hello VM is beletelhet hello egyéni tevékenység futtatása előtt.  Ha hello készletet használ egy másik autoScaleEvaluationInterval, hello Batch szolgáltatás autoScaleEvaluationInterval + 10 percet is beletelhet.

## <a name="use-hdinsight-compute-service"></a>HDInsight számítási szolgáltatás használata
Hello forgatókönyv használt Azure Batch számítási toorun hello egyéni tevékenység. Is használhatja a saját Windows-alapú HDInsight-fürt vagy adat-előállító igény szerinti Windows-alapú HDInsight-fürtöt, és futtassa a HDInsight-fürt hello hello egyéni tevékenység rendelkezik. Az alábbiakban hello magas szintű lépései a HDInsight-fürtöt használ.

> [!IMPORTANT]
> hello egyéni .NET tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön. Ez a korlátozás az áthidaló toouse hello térkép csökkentése tevékenység toorun egyéni Java-kód egy Linux-alapú HDInsight-fürtön. Egy másik lehetőség a virtuális gépek toorun egyéni tevékenységeket a HDInsight-fürt használata helyett az Azure Batch-készlet toouse.
 

1. Hozzon létre egy Azure HDInsight társított szolgáltatást.   
2. Használja a HDInsight társított szolgáltatás helyett **AzureBatchLinkedService** hello a csővezeték-JSON.

Ha azt szeretné, hogy tootest hello forgatókönyv, módosítsa a **start** és **end** alkalommal hello adatcsatorna, így hello Azure HDInsight szolgáltatás hello forgatókönyv teszteléséhez.

#### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight társított szolgáltatás létrehozása
hello Azure Data Factory szolgáltatásnak az igény szerinti fürt létrehozását támogatja, és használja úgy tooprocess bemeneti tooproduce kimeneti adatokat. Használhatja a saját fürt tooperform hello azonos. Igény szerinti HDInsight-fürt használata esetén a fürt minden egyes szeletre vonatkozó végrehajtásakor létrejön. Mivel a saját HDInsight-fürtöt használ, ha készen áll a fürt hello tooprocess azonnal hello szelet. Ezért ha igény-fürtöt használ, nem jelenhet meg hello kimeneti adatok leggyorsabban saját fürt használatakor.

> [!NOTE]
> Futásidőben a .NET tevékenység példányának csak a HDInsight-fürt hello; egy munkavégző csomópontja fut nem lehet több csomóponton méretezett toorun. .NET tevékenység több példánya hello HDInsight-fürt csomópontjai párhuzamosan is futtathatók.
>
>

##### <a name="toouse-an-on-demand-hdinsight-cluster"></a>igény szerinti HDInsight-fürtök toouse
1. A hello **Azure-portálon**, kattintson a **fejlesztés és üzembe helyezés** a hello adat-előállító kezdőlapján.
2. A Data Factory Editor hello, kattintson **új számítási** hello parancs menüsoron, majd válassza a **igény szerinti HDInsight-fürt** hello menüből.
3. Hajtsa végre a következő módosításokat toohello JSON-parancsfájl hello:

   1. A hello **nagyobbnak** tulajdonság, adja meg a HDInsight-fürt hello hello méretét.
   2. A hello **timeToLive** tulajdonság, adja meg, hogy mennyi ideig hello ügyfél üresjáratban lehet, mielőtt törli.
   3. A hello **verzió** tulajdonság, adja meg a kívánt toouse hello HDInsight-verzió. Ha kizárja a ezt a tulajdonságot, hello legújabb verzióját használja.  
   4. A hello **linkedServiceName**, adja meg **AzureStorageLinkedService**.

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > hello egyéni .NET tevékenységek futtatásához csak a Windows-alapú HDInsight-fürtökön. Ez a korlátozás az áthidaló toouse hello térkép csökkentése tevékenység toorun egyéni Java-kód egy Linux-alapú HDInsight-fürtön. Egy másik lehetőség a virtuális gépek toorun egyéni tevékenységeket a HDInsight-fürt használata helyett az Azure Batch-készlet toouse.

4. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

##### <a name="toouse-your-own-hdinsight-cluster"></a>toouse saját HDInsight-fürt:
1. A hello **Azure-portálon**, kattintson a **fejlesztés és üzembe helyezés** a hello adat-előállító kezdőlapján.
2. A hello **Data Factory Editor**, kattintson a **új számítási** hello parancs menüsoron, majd válassza a **HDInsight-fürt** hello menüből.
3. Hajtsa végre a következő módosításokat toohello JSON-parancsfájl hello:

   1. A hello **clusterUri** tulajdonság, a HDInsight meg hello URL-CÍMÉT. Például: https://<clustername>.azurehdinsight.net/     
   2. A hello **felhasználónév** tulajdonság, adja meg a hozzáférés toohello HDInsight-fürt rendelkező hello felhasználónév.
   3. A hello **jelszó** tulajdonság, írja be a hello jelszót hello felhasználó.
   4. A hello **LinkedServiceName** tulajdonság, adja meg **AzureStorageLinkedService**.
4. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) részleteiről.

A hello **JSON-feldolgozási folyamat**, HDInsight használata (igény vagy a saját) társított szolgáltatáshoz:

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="create-a-custom-activity-by-using-net-sdk"></a>Egyéni tevékenység létrehozása .NET SDK használatával
Hello forgatókönyv ebben a cikkben akkor hozzon létre egy adat-előállító egy folyamatot, amely hello egyéni tevékenység használ hello Azure-portál használatával. hello a következő kód bemutatja, hogyan toocreate hello helyette .NET SDK használatával adat-előállítóban. SDK tooprogrammatically használatával kapcsolatos további részletekért hozzon létre adatcsatornák hello található [.NET API használatával hozzon létre egy folyamatot másolási tevékenység](data-factory-copy-activity-tutorial-using-dotnet-api.md) cikk. 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with hello name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need tooset slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed tooacquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a>A Visual Studio egyéni tevékenység hibakeresése
Hello [Azure Data Factory - helyi környezetben](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) mintát a Githubon tartalmaz olyan eszköz, amely lehetővé teszi a toodebug egyéni .NET tevékenységeket a Visual studióban.  


## <a name="sample-custom-activities-on-github"></a>Egyéni tevékenységek mintát a Githubon
| Minta | Milyen egyéni tevékenység does |
| --- | --- |
| [HTTP-adatok letöltő](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Letölti az adatokat a egy HTTP-végpont tooAzure egyéni tevékenység C# használatával a Data Factory Blob-tároló. |
| [Twitter véleményeket elemzési minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Meghívja az Azure ML modellje és do véleményeket elemzése, pontozási, előrejelzés stb. |
| [R-parancsfájl futtatása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |Meghívja az R-parancsfájl RScript.exe futtatásával a HDInsight-fürtre, amelyen már a R telepítve van rajta. |
| [Kereszt-AppDomain .NET tevékenység](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Használja azokat, hello adat-előállító indítója által használt verziókat különböző szerelvény |
| [Újból feldolgozza a modellt az Azure Analysis Services](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  Feldolgozza a modellt az Azure Analysis Servicesben. |

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
