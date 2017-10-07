---
title: "használatával a Data Factory és kötegelt aaaProcess nagy méretű adatkészletek |} Microsoft Docs"
description: "Ismerteti, hogyan tooprocess hatalmas mennyiségű adatokat az Azure Data Factory-feldolgozási folyamat Azure kötegelt párhuzamos feldolgozási képességének használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 6788f02de555d2e9d6588cc990a39043866d7e97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Nagyméretű adatkészletek feldolgozása a Data Factory és a Batch használatával
Ez a cikk ismerteti a minta megoldás, amely helyezi át, és feldolgozza a nagy méretű adatkészletek automatikus és ütemezett módon architektúrát. Egy Azure Data Factory és az Azure Batch-végpontok közötti forgatókönyv tooimplement hello megoldás is tartalmazza.

Ez a cikk nem hosszabb, mint a szokásos cikk, mert a forgatókönyv egy teljes mintát megoldás tartalmaz. Ha új tooBatch és a Data Factory megismerheti ezeket a szolgáltatásokat, és hogyan működnek együtt. Ha tudja, valami hello szolgáltatásokra vonatkozó és vannak designing/újratervezni a megoldás, előfordulhat, hogy koncentrálhat, csak hello [architektúra szakasz](#architecture-of-sample-solution) hello cikket, és ha egy prototípus vagy megoldás fejleszt, akkor is érdemes lehet tootry részletes útmutatás a hello [forgatókönyv](#implementation-of-sample-solution). A Microsoft hívhat meg a tartalmat, és használatának kapcsolatos megjegyzéseit.

Első lépésként nézzük hogyan adat-előállító és a Batch szolgáltatás nagy adatkészletek hello felhőben feldolgozással segítik.     

## <a name="why-azure-batch"></a>Miért érdemes az Azure Batch?
Az Azure Batch lehetővé teszi toorun nagyméretű párhuzamos és nagy teljesítményű számítástechnikai (HPC) alkalmazások hatékonyan hello felhőben. Egy felügyelt szervezendő virtuális gépek számítási igényű munkahelyi toorun ütemezését végző platform szolgáltatás, és képes automatikusan méretezési számítási erőforrások toomeet hello igényeinek a feladatok.

A Batch szolgáltatás hello megadhatja az Azure számítási erőforrások tooexecute az alkalmazások párhuzamosan, és léptékű. Futtatható azonnali vagy ütemezett feladatok, és nem kell toomanually létrehozása, konfigurálása és kezelése a HPC-fürt, az egyes virtuális gépek, virtuális hálózatok vagy egy összetett feladat és feladat ütemezési infrastruktúra.

Tekintse meg a következő cikkeket, ha nem ismeri az Azure Batch, segít megérteni, hello hello megoldás cikkben leírt architektúra/végrehajtásának hello.   

* [Az Azure Batch alapjai](../batch/batch-technical-overview.md)
* [A Batch funkcióinak áttekintése](../batch/batch-api-basics.md)

(választható) toolearn Azure Batch kapcsolatos további információkért lásd: hello [Azure Batch képzési terv](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Miért érdemes az Azure Data Factory?
Adat-előállító koordinálja és automatizálja hello mozgás és adatok átalakítása felhőalapú adatok integrációs szolgáltatás. Hello Data Factory szolgáltatásnak, hozhat létre, amely tárolt adatok mozgatása a helyszíni és felhőalapú tárolók tooa központosított adatokat tároló felügyelt adatok folyamatok (például: Azure Blob Storage), és a folyamat/átalakítás-adatokat, például az Azure HDInsight és az Azure szolgáltatások Gépi tanulás. Is adatok folyamatok toorun ütemezett módon (óránkénti, napi, heti, stb.) és a figyelő ütemezése és kezelheti azokat egy pillanat alatt tooidentify problémákat, és hajtsa végre a műveletet.

Tekintse meg a következő cikkeket, ha nem ismeri az Azure Data Factoryvel, segít megérteni, hello hello megoldás cikkben leírt architektúra/végrehajtásának hello.  

* [Az Azure Data Factory bevezetése](data-factory-introduction.md)
* [Felépítheti első folyamatát adatok](data-factory-build-your-first-pipeline.md)   

További információ az Azure Data Factory (nem kötelező) toolearn lásd: hello [az Azure Data Factory képzési terv](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Adat-előállító és kötegelt együtt
Adat-előállító beépített tevékenységeket magában foglalja, például a forrásadatok másolási tevékenység toocopy/move data tooa adatok céltár és Hive tevékenységek tooprocess adatait az Azure-on Hadoop-fürtök (HDInsight) használatával. Lásd: [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) támogatott átalakítása tevékenységek listáját.

Lehetővé teszi, hogy a hogy toocreate egyéni .NET tevékenység saját logikával toomove vagy folyamat adatok és a egy Azure HDInsight-fürt vagy a virtuális gépek Azure Batch-készlet futtassa le ezeket a tevékenységeket. Azure Batch használata esetén konfigurálhatja hello készlet tooauto méretű (hozzáadása vagy eltávolítása a virtuális gépek hello munkaterhelés alapján), adja meg a képlet alapján.     

## <a name="architecture-of-sample-solution"></a>A minta megoldás architektúrája
Annak ellenére, hogy ebben a cikkben leírt hello architektúra a legegyszerűbb megoldás az, akkor megfelelő toocomplex forgatókönyvek például a pénzügyi szolgáltatásokat, kép feldolgozási és megjelenítési és genomikus elemzés modellezési kockázat.

hello diagram azt ábrázolja, 1) hogyan koordinálja a Data Factory az adatmozgás, és feldolgozás és a 2.) hogyan dolgozza fel a Azure Batch hello adatok párhuzamos módon. A letöltés és nyomtatási hello diagram az egyszerűbb használat (11 x 17. vagy A3 méret): [Azure Batch és a Data Factory használatával HPC és az orchestration](http://go.microsoft.com/fwlink/?LinkId=717686).

[![A nagyméretű adatok feldolgozása diagramja](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

hello alábbi lépéseit hello alapszintű hello folyamat. hello megoldás kódot és magyarázatokat toobuild hello végpont megoldást tartalmaz.

1. **Konfigurálja az Azure Batch számítási csomópontok (VM) készletét**. Hello csomópontok száma és mérete az egyes csomópontok is megadhat.
2. **Hozzon létre egy Azure Data Factory példányt** , amelyek megfelelnek az Azure blob storage, Azure Batch számítási szolgáltatás, bemeneti adatokat és egy munkafolyamat/folyamat olyan tevékenységet, amely áthelyezése és az adatok átalakítása entitások van konfigurálva.
3. **Hozzon létre egy egyéni .NET tevékenység hello Data Factory-folyamathoz**. hello tevékenysége a hello Azure Batch-készlet futó felhasználói kód.
4. **A bemeneti adatok nagy mennyiségű tárolót, mint az Azure-tárfiókba blobok**. Adatok osztóval időszeletekre logikai (általában idő).
5. **Adat-előállító másolja át a feldolgozott adatok párhuzamosan** toohello másodlagos helyre.
6. **Adat-előállító futtatja le van foglalva kötegelt használja hello hello egyéni tevékenység**. Adat-előállító egyidejűleg futtatható tevékenységek. Minden tevékenység adatok szelet dolgozza fel. hello eredmények az Azure storage tárolja.
7. **Adat-előállító hello végleges eredmények tooa harmadik helyre helyezi át**, vagy terjesztési keresztül egy alkalmazást, vagy más eszközökkel további feldolgozásra.

## <a name="implementation-of-sample-solution"></a>A minta-megoldás megvalósítása
hello megoldást szándékosan egyszerű és tooshow, hogyan toouse adat-előállító és kötegelt együtt tooprocess adatkészletek. hello megoldás egyszerűen számolja hello egy keresési kifejezés ("Microsoft") előfordulását a bemeneti fájlok idősor rendezve. Hello száma toooutput fájlok kiírja azt.

**Idő**: Amennyiben az Azure Data Factory és kötegelt alapjainak megismerése, és rendelkezik befejezett hello Előfeltételek alább felsorolt, azt adja meg ehhez a megoldáshoz szükséges 1 – 2 óra toocomplete.

### <a name="prerequisites"></a>Előfeltételek
#### <a name="azure-subscription"></a>Azure-előfizetés
Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy ingyenes próbafiók néhány perc alatt. Lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure Storage-fiók
Egy Azure storage-fiók ebben az oktatóanyagban hello adatok tárolásához használja. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account). hello megoldást használja a blob Storage tárolóban.

#### <a name="azure-batch-account"></a>Azure Batch-fiók
Hello használata Azure Batch-fiók létrehozása [Azure-portálon](http://manage.windowsazure.com/). Lásd: [létrehozása és kezelése az Azure Batch-fiók](../batch/batch-account-create-portal.md). Vegye figyelembe a hello Azure Batch nevét és a fiók kulcsot. Is [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) parancsmag toocreate Azure Batch-fiók. Lásd: [Ismerkedés az Azure Batch PowerShell-parancsmagok](../batch/batch-powershell-cmdlets-get-started.md) részletes útmutató a parancsmag használatával.

hello megoldást Azure Batch (közvetve keresztül egy Azure Data Factory-folyamathoz) tooprocess adatokat a számítási csomópontok (a virtuális gépek felügyelt gyűjteménye) készlete egy párhuzamos módon használja.

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>A virtuális gépek (VM) az Azure Batch-készlet
Hozzon létre egy **Azure Batch-készlet** legalább 2-es számítási csomópontjain.

1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **Tallózás** a bal oldali menü hello, és kattintson a **Batch-fiókok**.
2. Válassza ki az Azure Batch-fiók tooopen hello **Batch-fiók** panelen.
3. Kattintson a **készletek** csempére.
4. A hello **készletek** panelen hello eszköztár tooadd a készlet hozzáadása gombra.
   1. Adjon meg egy Azonosítót hello készlet (**alkalmazáskészlet-azonosító**). Megjegyzés: hello **hello készlet azonosítója**; amikor hello adat-előállító megoldás létrehozásakor.
   2. Adja meg **Windows Server 2012 R2** hello operációsrendszer-családot beállítás.
   3. Válassza ki a **csomóponti tarifacsomagot**.
   4. Adja meg **2** hello értékként **cél dedikált** beállítást.
   5. Adja meg **2** hello értékként **csomópontonkénti tevékenységek maximális** beállítást.
   6. Kattintson a **OK** toocreate hello készlet.

#### <a name="azure-storage-explorer"></a>Azure Storage Explorer
[Az Azure Storage Explorer 6 (eszköz)](https://azurestorageexplorer.codeplex.com/) vagy [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (a ClumsyLeaf szoftver). Ezek az eszközök használata vizsgálatával, és a felhőben üzemeltetett alkalmazások hello naplófájlokkal együtt az Azure Storage-projektek hello adatainak módosítása.

1. Hozzon létre egy nevű tárolót **mycontainer** privát hozzáférést (nincs névtelen hozzáférés)
2. Ha használ **CloudXplorer**, mappák létrehozása, és az almappák hello struktúra a következő:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`és `outputfolder` a legfelső szintű mappák `mycontainer`. Hello `inputfolder` almappái dátum-időbélyegeket (éééé-hh-nn-ÓÓ) rendelkezik.

   Ha használ **Azure Tártallózó**, hello a következő lépésben szüksége tooupload névvel rendelkező fájlok: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` és így tovább. Ez a lépés automatikusan hello mappákat hoz létre.
3. Hozzon létre egy szövegfájlt **file.txt** hello kulcsszó tartalmat a gépen **Microsoft**. Például: "egyéni tevékenység Microsoft teszt egyéni tevékenység Microsoft teszt".
4. Töltse fel a következő bemeneti mappák az Azure blob storage hello fájl toohello.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Ha használ **Azure Tártallózó**, hello-fájl feltöltése **file.txt** túl**mycontainer**. Kattintson a **másolási** a hello eszköztár toocreate hello blob egy példányát. A hello **másolási Blob** párbeszédpanelen, a módosítás hello **cél blob neve** túl`inputfolder/2015-11-16-00/file.txt`. Ismételje meg ezt a lépést toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` és így tovább. Ez a művelet automatikusan hello mappákat hoz létre.
5. Hozzon létre egy másik tárolót: `customactivitycontainer`. Hello egyéni zip fájlt toothis tároló-be feltölteni.

#### <a name="visual-studio"></a>Visual Studio
Telepítse a Microsoft Visual Studio 2012 vagy újabb toocreate hello egyéni kötegelt tevékenység toobe hello adat-előállító megoldás szerepel.

### <a name="high-level-steps-toocreate-hello-solution"></a>Magas szintű lépései toocreate hello megoldás
1. Létrehozhat egy egyéni, amely hello adatfeldolgozási logikáját tartalmazza.
2. Hozzon létre egy az Azure data factory hello egyéni tevékenységet használó:

### <a name="create-hello-custom-activity"></a>Hello egyéni tevékenység létrehozása
Adat-előállító egyéni tevékenység hello hello lelke a megoldást. hello minta megoldás az Azure Batch toorun hello egyéni tevékenység. Lásd: [egyéni tevékenységeket használni egy Azure Data Factory-folyamathoz](data-factory-use-custom-activities.md) őket az Azure Data Factory folyamatok hello alapvető információkat toodevelop egyéni tevékenységeket és használatát.

toocreate szüksége toocreate .NET egyéni tevékenység, amely egy Azure Data Factory-folyamathoz használható, egy **.NET Class Library** egy osztály, amely megvalósítja az, hogy a projekt **IDotNetActivity** felületet. Ez az interfész csak egy metódusa: **Execute**. Hello metódus aláírása hello itt található:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

hello metódusnak, hogy kell-e toounderstand néhány kulcsfontosságú összetevők.

* hello metódus négy paramétereket fogadja:

  1. **linkedServices**. Egy enumerálható listája, amely a bemeneti/kimeneti adatforrások összekapcsolt szolgáltatások (például: Azure Blob Storage) toohello adat-előállítóban. Ez a példa nincs Azure Storage használt bemeneti és kimeneti típusú csak egy társított szolgáltatást.
  2. **adatkészletek**. A rendszer egy enumerálható állnak. A paraméter tooget hello helyek és -sémákat határozzák meg a bemeneti és kimeneti adathalmazokat is használhatja.
  3. **tevékenység**. Ez a paraméter hello aktuális számítási entitás - ebben az esetben az Azure Batch szolgáltatás jelöli.
  4. **naplózó**. hello naplózó lehetővé teszi, hogy írható hibakeresési megjegyzések adott felület hello "User" keresse meg a folyamat hello.
* hello metódus, amely lehet használt toochain egyéni tevékenységek együtt hello jövőben dictionary adja vissza. Ez a funkció még nem használható, így egy üres szótár visszaadásának hello metódus.

#### <a name="procedure-create-hello-custom-activity"></a>Az eljárás: Hello egyéni tevékenység létrehozása
1. A Visual Studio .NET Class Library projekt létrehozása

   1. Indítsa el **Visual Studio 2012**/**2013 vagy 2015**.
   2. Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.
   3. Bontsa ki a **sablonok**, és válassza ki **Visual C\#**. Ebben a bemutatóban használhat C\#, de használhatja a .NET nyelvi toodevelop hello egyéni tevékenység.
   4. Válassza ki **Class Library** jobb hello projekttípusok hello listája.
   5. Adja meg **MyDotNetActivity** a hello **neve**.
   6. Válassza ki **C:\\ADF** a hello **hely**. Hozzon létre hello mappát **ADF** Ha még nem létezik.
   7. Kattintson a **OK** toocreate hello projekt.
2. Kattintson a **eszközök**, pont túl**NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**.
3. A hello **Csomagkezelő konzol**, hajtsa végre a következő parancs tooimport hello **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importálás hello **Azure Storage** toohello projekt NuGet-csomagot. Ön ezt a csomagot kell végrehajtani, mert ez a példa hello Blob storage API-t használja.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Adja hozzá a következő hello **használatával** irányelvek toohello forrásfájl hello projektben.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Hello nevének módosítása a hello **névtér** túl**MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Hello osztály hello nevének módosítása túl**MyDotNetActivity** , és amelyek hello **IDotNetActivity** csatoló alább látható módon.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Megvalósítása (Hozzáadás) hello **Execute** hello metódusában **IDotNetActivity** toohello csatoló **MyDotNetActivity** minta kód toohello metódus a következő osztály és példány hello. Lásd: hello [metódus végrehajtása](#execute-method) magyarázat hello logika, az ezzel a módszerrel a szakaszát.

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
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using hello same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // toocreate an input storage client.
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
           // with hello data slice.
           //
           // definition of hello method is shown in hello next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob toohello folder: {0}", folderPath);
    
       // create a storage object for hello output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write hello name of hello file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
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
9. Adja hozzá a következő módszerek toohello segítőosztály hello. Ezek a módszerek hívják hello **Execute** metódust. A legfontosabb, hello **Calculate** metódus elkülöníti hello kódot, amely minden egyes blob telepítéseket.

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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    Hello **GetFolderPath** metódus visszaadja hello toohello mappára, hogy hello dataset pontok tooand hello **GetFileName** metódus hello/fájlját, hogy a DataSet adatkészlet mutat hello hello nevét adja vissza.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    Hello **Calculate** metódus kiszámítja hello több példányban kulcsszó **Microsoft** hello bemeneti fájlban (BLOB hello mappában). hello keresési kifejezés ("Microsoft") nem változtatható hello kódban.

1. Fordítsa le hello projektet. Kattintson a **Build** hello menüre, majd a **megoldás fordítása**.
2. Indítsa el **Windows Explorer**, és keresse meg a túl**bin\\debug** vagy **bin\\kiadási** mappa build hello típusától függően.
3. Hozzon létre egy zip fájlt **MyDotNetActivity.zip** hello összes hello bináris fájlokat tartalmazó ** \\bin\\Debug** mappát. Érdemes lehet tooinclude hello MyDotNetActivity. **pdb** fájlt úgy, hogy be további részletekről, mint a sor számának hello forráskód, amely hello problémát az okozza, ha hiba lép fel.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Töltse fel **MyDotNetActivity.zip** blob toohello blob tárolóként: `customactivitycontainer` a hello Azure blob-tároló adott hello **StorageLinkedService** társított szolgáltatásnak az hello ** ADFTutorialDataFactory** használja. Hello blob-tároló létrehozása `customactivitycontainer` Ha még nem létezik.

#### <a name="execute-method"></a>Metódus végrehajtása
Ez a témakör további részleteket és megjegyzéseket hello kód az Execute metódus hello.

1. hello tagjainak iteráció hello bemeneti gyűjteményben találhatók hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) névtér. Hello blob gyűjtemény iteráció szükséges hello segítségével **BlobContinuationToken** osztály. Lényegében kell használnia a do-hello jogkivonattal való kilépés hello hurok hello mechanizmusként hurok közben. További információkért lásd: [hogyan toouse Blob storage a .NET-](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Alapszintű hurkot itt jelenik meg:

    ```csharp
    // Initialize hello continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get hello list of input blobs from hello input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Hello hello dokumentációjában [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) részletes metódust.
2. hello kód a belül hello használatához, áttekintve blobok hello készlete logikailag kerül tegye-ciklus során. A hello **Execute** metódust, hello tegye-közben hurok továbbítja blobok hello listája nevű tooa metódust **Calculate**. hello metódus visszaadja egy karakterlánc-változóvá nevű **kimeneti** hello szegmensben lévő összes hello BLOB keresztül többször is rendelkezik, amely hello eredménye.

   Hello keresési kifejezés előfordulását hello számát adja vissza (**Microsoft**) hello BLOB átadott toohello **Calculate** metódust.

    ```csharp
    output += string.Format("{0} occurrences of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Egyszer hello **Calculate** metódus hello munkahelyi megtörtént, akkor úgy kell megírni, tooa új blob. Így a blobok feldolgozása minden készlete esetében egy új blob hello eredmények írhatók. toowrite tooa új blob, első keresés hello kimeneti adatkészletet.

    ```csharp
    // Get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. hello kódot is meghívja egy segédmetódust: **GetFolderPath** tooretrieve hello mappa elérési útja (hello tárolási tároló neve).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   Hello **GetFolderPath** kiterjesztések hello DataSet objektum tooan AzureBlobDataSet, amelynek FolderPath nevű tulajdonság.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. hello kód hívások hello **GetFileName** metódus tooretrieve hello fájl neve (blob neve). hello kód hasonló toohello kód tooget hello mappa elérési útja felett.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. egy URI-objektum létrehozása hello fájl hello nevét írja. hello URI konstruktor használ hello **BlobEndpoint** tulajdonság tooreturn hello tároló neve. hello mappa elérési útját és nevét tooconstruct hello kimeneti blob URI kerülnek.  

    ```csharp
    // Write hello name of hello file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. hello fájl hello neve van írva, és most írja a hello kimeneti karakterlánc érkező hello **Calculate** metódus tooa új blob:

    ```csharp
    // Create a blob and upload hello output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} toohello output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-hello-data-factory"></a>Hello adat-előállító létrehozása
A hello [hello egyéni tevékenység létrehozása](#create-the-custom-activity) szakaszban létrehozott egyéni tevékenység és a zip-fájl feltöltése hello bináris fájljait és a hello PDB tooan Azure blob-tároló. Ebben a szakaszban hoz létre egy Azure **adat-előállító** rendelkező egy **csővezeték** hello használó **egyéni tevékenység**.

hello bemeneti adatkészletet, az egyéni tevékenység hello hello blobok (fájlok) jelöli hello bemeneti mappában (`mycontainer\\inputfolder`) a blob Storage tárolóban. hello kimeneti adatkészlet a hello tevékenység jelöli hello kimeneti BLOB hello kimeneti mappában (`mycontainer\\outputfolder`) a blob Storage tárolóban.

Egy vagy több fájl eldobási hello bemeneti mappák:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Például egy fájl (file.txt) dobható el, a következő tartalom mindegyik hello mappák hello.

```
test custom activity Microsoft test custom activity Microsoft
```

Egyes bemeneti mappa felel meg az Azure Data Factory tooa szelet, akkor is, ha hello mappa 2 vagy több fájlt. Minden szelet feldolgozása hello folyamat után hello egyéni tevékenység hello bemeneti mappában, hogy a szeletre vonatkozó összes hello BLOB telepítéseket.

Láthatja, hogy öt kimeneti fájl, amelynek hello azonos tartalom. Például a hello kimeneti fájl hello 2015-11-16-00 mappában hello fájl feldolgozása a tartalom a következő hello rendelkezik:

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
```

Ha több fájl (file.txt, fájl2.ref fájllal, file3.txt) eldobja a hello azonos tartalommappa toohello bemeneti úgy, hogy a következő hello kimeneti fájl tartalma hello. Minden mappa (2015-11-16-00, stb.) felel meg ez a példa tooa szelet, annak ellenére, hogy hello mappa több bemeneti fájlokat tartalmaz.

```csharp
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file3.txt.
```

hello kimeneti fájl három sort, egy a hello szelet (2015-11-16-00) társított hello mappa minden a bemeneti fájl (blob) rendelkezik.

Egy feladat minden egyes tevékenységfuttatási jön létre. Ez a példa nincs csak egy tevékenység hello folyamat. A szelet feldolgozása hello folyamat után hello egyéni tevékenység az Azure Batch tooprocess hello szelet fut. Nincsenek öt szeletek (minden szelet rendelkezhet több bináris objektumok vagy fájl), mert nincsenek Azure Batch létrehozott öt feladatok. Kötegelt feladatként fut, esetén ténylegesen hello egyéni tevékenység fut-e.

a következő forgatókönyv hello további részleteket biztosít.

#### <a name="step-1-create-hello-data-factory"></a>1. lépés: Hello adat-előállító létrehozása
1. Toohello a bejelentkezés után [Azure-portálon](https://portal.azure.com/), hello a következő lépéseket:

   1. Kattintson a **új** hello bal oldali menüben.
   2. Kattintson a **adatok + analitika** a hello **új** panelen.
   3. Kattintson a **adat-előállító** a hello **adatelemzés** panelen.
2. A hello **új adat-előállító** panelen adja meg **CustomActivityFactory** a hello nevét. az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "CustomActivityFactory"**, adat-előállító hello hello nevének módosítása (például **yournameCustomActivityFactory**), és próbálja meg létrehozni újra.
3. Kattintson a **ERŐFORRÁSCSOPORT-név**, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot.
4. Győződjön meg arról, hogy megfelelő előfizetés hello és hello data factory toobe létrehozott eső használja.
5. Kattintson a **létrehozása** a hello **új adat-előállító** panelen.
6. Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon.
7. Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>2. lépés: A társított szolgáltatások létrehozásához
Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási. Ebben a lépésben hivatkozásra a **Azure Storage** fiók és **Azure Batch** fiók tooyour adat-előállítóban.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
1. Kattintson a hello **Szerző és központi telepítése** hello csempét **DATA FACTORY** paneljén **CustomActivityFactory**. Megjelenik a Data Factory Editor hello.
2. Kattintson a **az új adattároló** a hello parancssávon, és válassza a **az Azure storage.** Megtekintheti az hello JSON-parancsfájl létrehozásához egy Azure Storage társított szolgáltatásnak hello-szerkesztőben.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. Cserélje le **fióknév** hello nevű Azure-tárfiókot és **fiókkulcs** kulcsával hello hozzáférés hello Azure storage-fiók. toolearn hogyan tooget tárhelyét férnek hozzá, tekintse meg [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Csatolt Azure Batch szolgáltatás létrehozása
Ebben a lépésben a társított szolgáltatás létrehozása a **Azure Batch** használt toorun hello adat-előállító egyéni tevékenység fiókkal.

1. Kattintson a **új számítási** a hello parancssávon, és válassza a **Azure Batch.** Megjelenik a JSON-parancsfájl létrehozásához az Azure Batch hello társított szolgáltatás hello-szerkesztőben.
2. A JSON-parancsfájl hello:

   1. Cserélje le **fióknév** hello nevet, az Azure Batch-fiókhoz.
   2. Cserélje le **hozzáférési kulcs** kulcsával hello hozzáférés hello Azure Batch-fiókhoz.
   3. Adjon meg hello Azonosítót hello készlet hello **poolName** tulajdonság**.** Ehhez a tulajdonsághoz adja meg vagy készlet nevét vagy tárolókészlet azonosítóját.
   4. Adja meg a hello köteg URI hello **batchUri** JSON tulajdonság.

      > [!IMPORTANT]
      > Hello **URL-cím** a hello **Azure Batch-fiók panelen** hello formátuma a következő szerepel: \<accountname\>.\< régió\>. batch.azure.com. A hello **batchUri** hello JSON tulajdonságot, kell túl**távolítsa el a "accountname."** a hello URL-CÍMRŐL. Példa: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      A hello **poolName** tulajdonság, azt is megadhatja, hello azonosító hello készlet hello hello neve helyett.

      > [!NOTE]
      > hello Data Factory szolgáltatásnak nem támogatja egy igény szerinti beállítást az Azure hdinsight azonban nem. Csak a saját Azure Batch-készlet használhatja az Azure data factory.
      >
      >
   5. Adja meg **StorageLinkedService** a hello **linkedServiceName** tulajdonság. Hello előző lépésben létrehozott szolgáltatásnak. A tárolót használja a rendszer egy átmeneti területre, fájlok és a naplókat.
3. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

#### <a name="step-3-create-datasets"></a>3. lépés: Adatkészletek létrehozása
Ebben a lépésben létrehozni adatkészletek toorepresent bemeneti és kimeneti adatai.

#### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
1. A hello **szerkesztő** hello adat-előállítót, kattintson **új adatkészlet** elemre, majd hello eszköztár gomb **Azure Blob Storage tárolóban** hello legördülő menüből.
2. Cserélje le a következő JSON részlet hello hello JSON hello jobb oldali ablaktáblában:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
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

    Ez a forgatókönyv a kezdő időpont későbbi részében hozzon létre egy folyamatot: 2015-11-16T00:00:00Z és záró idő: 2015-11-16T05:00:00Z. Ütemezett tooproduce adatok **óránkénti**, így 5 bemeneti/kimeneti szeletek (közötti **00**: 00:00 -\> **05**: 00:00).

    Hello **gyakoriság** és **időköz** hello bemeneti adatkészlet túl van-e állítva a**óra** és **1**, ami azt jelenti, hogy hello bemeneti szelet érhető el óránként.

    Az alábbiakban hello indítási idejének az egyes szeletek, ami által képviselt **SliceStart** hello JSON részlet újabb rendszer változót.

    | **Szelet** | **Kezdési idő**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**: 00:00 |
    | 2         | 2015-11-16T**01**: 00:00 |
    | 3         | 2015-11-16T**02**: 00:00 |
    | 4         | 2015-11-16T**03**: 00:00 |
    | 5         | 2015-11-16T**04**: 00:00 |

    Hello **folderPath** által hello év, hónap, nap és óra részét hello szelet kezdete (**SliceStart**). Itt ezért csatlakoztatott tooa szelet Mitől egy bemeneti mappa.

    | **Szelet** | **Kezdési idő**          | **Bemeneti mappa**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16-**04** |

1. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **InputDataset** tábla.

#### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Ebben a lépésben egy másik dataset típusú AzureBlob toorepresent hello kimeneti adatokat hoz létre.

1. A hello **szerkesztő** hello adat-előállítót, kattintson **új adatkészlet** elemre, majd hello eszköztár gomb **Azure Blob Storage tárolóban** hello legördülő menüből.
2. Cserélje le a következő JSON részlet hello hello JSON hello jobb oldali ablaktáblában:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
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

    Egy kimeneti blob/fájl az egyes bemeneti szeletek jön létre. Ez hogyan kimeneti fájl neve az egyes szeletek. Minden hello kimeneti fájlok akkor jönnek létre, egy kimeneti mappában: `mycontainer\\outputfolder`.

    | **Szelet** | **Kezdési idő**          | **Kimeneti fájlja**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Ne feledje, hogy az összes hello egy bemeneti mappában lévő fájlok (például: 2015-11-16-00) hello kezdési időponttal szelet részét képezik: 2015-11-16-00. A szelet feldolgozása után hello egyéni tevékenység minden egyes fájl vizsgálja, és hozza létre a keresési kifejezés ("Microsoft") előfordulásainak száma hello hello kimeneti fájlban egy sor. Ha nincsenek három fájlok 2015-11-16-00 hello mappában, vannak-e három sort hello kimeneti fájl: 2015-11-16-00.txt.

1. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **OutputDataset**.

#### <a name="step-4-create-and-run-hello-pipeline-with-custom-activity"></a>4. lépés: Hozzon létre, és futtassa a hello csővezeték egyéni tevékenységet
Ebben a lépésben létrehoz egy folyamatot egy tevékenységet, amelyet előzőleg létrehozott egyéni tevékenység hello.

> [!IMPORTANT]
> Ha még nem töltött fel hello **file.txt** tooinput mappák hello blob a tárolóban, ehhez hello folyamat létrehozása előtt. Hello **isPaused** tulajdonsága toofalse hello adatcsatorna JSON-NÁ, így hello folyamat fut a azonnal hello **start** dátuma múltbeli hello van.
>
>

1. A Data Factory Editor hello, kattintson **új adatcsatorna** hello parancssávon. Ha nem látja hello parancsot, kattintson a **... Három (pont) ** toosee azt.
2. Cserélje le a JSON-parancsfájl a következő hello hello JSON hello jobb oldali ablaktáblában:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Vegye figyelembe a következő pontok hello:

   * Csak egy tevékenység hello feldolgozási és típusa, amely nem: **DotNetActivity**.
   * **AssemblyName** hello DLL toohello neve van beállítva: **MyDotNetActivity.dll**.
   * **EntryPoint** értéke túl**MyDotNetActivityNS.MyDotNetActivity**. Alapvetően van \<névtér\>.\< osztálynév\> a kódban.
   * **PackageLinkedService** értéke túl**StorageLinkedService** toohello blob-tároló hello egyéni tevékenység zip fájlt tartalmazó mutat. Ha különböző Azure Storage-fiókokat használ a bemeneti/kimeneti fájlok és hello egyéni tevékenység zip-fájl, akkor toocreate egy másik Azure Storage társított szolgáltatásnak. Ez a cikk feltételezi, hogy a hello azonos Azure Storage-fiók.
   * **PackageFile** értéke túl**customactivitycontainer/MyDotNetActivity.zip**. A hello formátumban: \<containerforthezip\>/\<nameofthezip.zip\>.
   * hello egyéni tevékenység veszi **InputDataset** bemenetként és **OutputDataset** output típusúként.
   * Hello **linkedServiceName** hello egyéni tevékenység tulajdonság mutat toohello **AzureBatchLinkedService**, amely közli az Azure Data Factory hello egyéni tevékenységre kell az Azure Batch toorun.
   * Hello **egyidejűségi** beállítás fontos. Ha hello alapértelmezett érték, amely 1, még akkor is, ha 2 vagy több csomópontján hello Azure Batch-készlet számítási, hello szeletek feldolgozásának egymás után. Ezért nem készítésének Azure Batch hello párhuzamos feldolgozási képességének kihasználása. Ha **egyidejűségi** tooa magasabb értéket, azaz 2, az azt jelenti, hogy a két szeletek (felel meg az Azure Batch tootwo feladatok) hello képes lehet feldolgozni azonos időben, ebben az esetben, mindkét hello virtuális gépeinek hello Azure Batch-készlet ki van használva. Ezért állítsa be megfelelően hello párhuzamossági tulajdonság.
   * Alapértelmezés szerint egyetlen virtuális gép (szelet) csak egy feladat végrehajtása. hello oka, hogy alapértelmezés szerint hello **virtuális gépenként feladatok maximális** az Azure Batch-készlet too1 van beállítva. Előfeltételek részeként létrehozott készlet e tulajdonság beállítása too2, két adat-előállító szeletek futhat egy virtuális gép egyidejű hello a ugyanannyi időt vesz igénybe.

    -   **isPaused** tulajdonsága toofalse alapértelmezés szerint. hello csővezeték azonnal futtatja ebben a példában, mert hello szeletek indítsa el a korábbi hello. Ez a tulajdonság tootrue toopause hello folyamat beállítása, és állítsa be úgy a hátsó toofalse toorestart.

    -   Hello **start** idő és **end** alkalommal öt órát egymástól, szeletek előállított hourly, így öt szeletek hello csővezeték hozzák létre.

1. Kattintson a **telepítés** a toodeploy hello csővezeték hello parancssávon.

#### <a name="step-5-test-hello-pipeline"></a>5. lépés: Hello folyamat tesztelése
Ebben a lépésben hello csővezeték hello bemeneti mappákba fájlok ejtésével tesztelése. Egy bemeneti mappa minden egy fájlba kezdjük tesztelési hello folyamat.

1. Hello a Data Factory panelen hello Azure-portálon, kattintson a **Diagram**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. Hello diagram nézetben kattintson duplán a bemeneti adatkészlet: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. Megtekintheti az hello **InputDataset** mind az öt panelről szeletek készen áll. Értesítés hello **SZELET kezdete** és **SZELET BEFEJEZÉSÉNEK** az egyes szeletek.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. A hello **diagramnézet**, ekkor kattintson a **OutputDataset**.
5. Meg kell jelennie, hogy a hello öt kimeneti szeletek hello kész állapotban van, ha azok már előállítani.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Használja az Azure portál tooview hello **feladatok** hello társított **szeletek** , és ellenőrizze, milyen VM minden szelet futott. Lásd: [adat-előállító és kötegelt integrációs](#data-factory-and-batch-integration) című szakaszban talál információt.
7. A kimeneti fájlok hello hello kell megjelennie `outputfolder` a `mycontainer` az Azure blob-tároló.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Meg kell jelennie egy, az egyes bemeneti szeletek öt kimeneti fájlokat. Egyes hello kimeneti fájl a következő kimeneti tartalom hasonló toohello kell rendelkeznie:

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
    ```
   hello következő ábra bemutatja hogyan hello adat-előállító szeletek leképezése tootasks Azure kötegben. Ebben a példában a szelet csak egy futtató rendelkezik.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Most próbáljuk meg egy mappában több fájlokkal. Fájlok létrehozása: **fájl2.ref fájllal**, **file3.txt**, **file4.txt**, és **file5.txt** hello az azonos tartalom hasonlóan file.txt hello mappában: **2015-11-06-01**.
9. Hello kimeneti mappában **törlése** hello kimeneti fájl: **2015-11-16-01.txt**.
10. Mostantól a hello **OutputDataset** panelen, kattintson a jobb gombbal hello szeletre **SZELET Kezdés időpontja** túl beállítása**11/16/2015 01:00:00-kor**, és kattintson a **futtassa**toorerun/újra-process hello szelet. Most hello szelet öt fájlokat tartalmaz egy fájl helyett.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. Után hello szelet fut, és annak állapotát **készen áll a**, ellenőrizze a szeletre vonatkozó hello kimeneti fájlban hello tartalmat (**2015-11-16-01.txt**) a hello `outputfolder` a `mycontainer` a blob Storage tárolóban. Egy sort a fájl hello szelet kell lennie.

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Ha, nem törölte a hello kimeneti fájl 2015-11-16-01.txt előtt öt bemeneti fájlokkal, látni egy sor hello előző szelet futtassa a és hello aktuális szelet futtassa az öt sorban. Alapértelmezés szerint hello tartalom hozzáfűzött toooutput fájlt is, ha már létezik.
>
>

#### <a name="data-factory-and-batch-integration"></a>Adat-előállító és kötegelt integrációja
hello Data Factory szolgáltatásnak hoz létre egy feladatot az Azure Batch névvel hello: `adf-poolname:job-xxx`.

![Az Azure Data Factory - kötegelt feladatok](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Egy feladat hello feladat minden egyes tevékenység futtatásához a szelet jön létre. Ha 10 szeletek készen toobe feldolgozott, 10 feladatok hello feladat jönnek létre. A párhuzamosan futó, ha több számítási csomópont hello készletben egynél több szelet lehet. Ha hello maximális tevékenységek maximális száma számítási csomópont értéke túl > 1 is lehet több mint egy szelet hello futó azonos számítási.

Ebben a példában nincsenek öt szeletek, így öt feladatok Azure kötegben. A hello **egyidejűségi** túl beállítása**5** hello a csővezeték-JSON-t Azure Data Factory és **virtuális gépenként feladatok maximális** túl beállítása**2** az Azure Batch tárolókészlet **2** virtuális gépeket, hello feladatok futtatása gyors (Ellenőrizze a kezdő és záró időpontjának feladatok).

Hello portál tooview hello kötegelt és a társított hello feladatainak **szeletek** , és ellenőrizze, milyen VM minden szelet futott.

![Az Azure Data Factory - kötegelt munka feladatai](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-hello-pipeline"></a>Hello folyamat hibakeresése
A hibakeresés néhány alapvető technikából áll:

1. Ha nincs beállítva túl hello bemeneti szelet**készen**, győződjön meg arról, hogy hello bemeneti mappaszerkezet helyes-e, valamint file.txt hello bemeneti mappáiban.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. A hello **Execute** az egyéni tevékenység használja hello metódusában **IActivityLogger** toolog objektuminformáció, amely segít a problémák elhárításához. hello felhasználói megjelennek a naplóba köszönőüzenetei\_0. naplófájlt.

   A hello **OutputDataset** panelen hello szelet toosee hello kattintson **ADATSZELET** , hogy a szelet paneljét. Látni **tevékenységek** az adott szeletek. Futtassa a hello szelet tevékenységenként kell megjelennie. Ha **futtatása** hello parancssávon, akkor kezdhet hello futtassa egy másik tevékenysége azonos szelet.

   Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello **tevékenység futtatása részletei** naplófájlok listáját tartalmazó panel. Hello a naplózott üzeneteket látja **felhasználói\_0. napló** fájlt. Ha hiba történik, megjelenik az három tevékenység fut mert hello újrapróbálkozások száma too3 hello csővezeték/tevékenységben JSON. Ha hello tevékenységfuttatási gombra kattint, megjelenik az hello naplófájlokat, hogy áttekintheti tootroubleshoot hello hiba.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   A naplófájlok hello listájában kattintson a hello **felhasználói-0.log**. Jobb oldali panelen hello vannak hello segítségével hello eredményeit **IActivityLogger.Write** metódust.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Ellenőrizze a rendszer-0.log rendszer hibaüzeneteket és kivételeket.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Hello tartalmaznak **PDB** hello zip-fájlban szereplő fájlt, hogy a hello hiba legutolsó részletes adatai információ például **hívási verem** Ha hiba történik.
4. Az egyéni tevékenység hello el kell érnie hello fájlok hello zip-fájlban szereplő összes hello **felső szintű** egyetlen almappát.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Győződjön meg arról, hogy hello **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip), és **packageLinkedService** (kell pont toohello Azure blob-tároló, amely tartalmazza a hello zip-fájl) toocorrect értékek vannak beállítva.
6. Ha rögzített egy hiba és a kívánt tooreprocess hello szelet, kattintson a jobb gombbal a hello hello szelet **OutputDataset** panel megnyitásához, és kattintson **futtatása**.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Megjelenik egy **tároló** az Azure Blob Storage nevű: `adfjobs`. Ez a tároló nem törli automatikusan a rendszer, de biztonságos törlése után végzett tesztelés hello megoldás. Ehhez hasonlóan hello adat-előállító megoldást hoz létre egy Azure Batch **feladat** nevű: `adf-\<pool ID/name\>:job-0000000001`. Ez a feladat hello megoldás Ha szeretné tesztelése után törölheti.
   >
   >
7. hello egyéni tevékenység nem használja a hello **app.config** fájlt a csomagból. Ezért ha a kód a kapcsolati karakterláncok hello konfigurációs fájlból olvassa be, nem működik futásidőben. hello célszerű esetén használja az Azure Batch toohold bármely titkos kulcsainak egy **Azure KeyVault**használni egy tanúsítvány alapú szolgáltatás egyszerű tooprotect hello keyvault és terjesztése hello tanúsítvány tooAzure Batch-készlet. hello .NET egyéni tevékenység majd el titkok hello KeyVault futásidőben. Ez a megoldás általános egy és titkos kulcsot, nem csak a kapcsolati karakterlánc típusú tooany méretezhető.

    Van egy egyszerűbb megoldást (de nem ajánlott): hozhat létre egy **Azure SQL társított szolgáltatásnak** rendelkező kapcsolatikarakterlánc-beállításokat, hozzon létre, hogy használ hello társított szolgáltatás, és típusú bemeneti adatkészletként lánc hello dataset adatkészlet Egyéni .NET tevékenység toohello. Ezután a hozzáférés hello kapcsolódó szolgáltatás kapcsolati karakterláncot a hello egyéni tevékenység kódja és konfigurálva kell működnie futásidőben.  

#### <a name="extend-hello-sample"></a>Hello minta kiterjesztése
Kiterjesztheti a minta toolearn további információk az Azure Data Factory és az Azure Batch-szolgáltatások. Például egy eltérő időtartományt tooprocess szeletek hello a következő lépéseket:

1. Adja hozzá a következő hello almappát hello `inputfolder`: 2015-11-16-05, 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 és helyet adjon meg a mappákban lévő fájlok. Módosítsa a hello folyamat befejezésének hello `2015-11-16T05:00:00Z` túl`2015-11-16T10:00:00Z`. A hello **diagramnézet**, kattintson duplán a hello **InputDataset**, és győződjön meg arról, hogy készen áll-e hello bemeneti szeletek. Kattintson duplán a **OuptutDataset** kimeneti szeletek toosee hello állapotát. Ha készen áll a állapotban vannak, ellenőrizze a hello kimeneti mappa hello kimeneti fájlok.
2. Növeli vagy csökkenti a hello **egyidejűségi** beállítás toounderstand hogyan azt befolyásolja, hogy a megoldás hello teljesítményét különösen hello feldolgozása, amely az Azure Batch következik be. (Lásd a 4. lépés: hozzon létre és futtasson hello csővezeték további hello **egyidejűségi** beállítás.)
3. Hozzon létre egy alkalmazáskészlet magasabb/alacsonyabb **virtuális gépenként feladatok maximális**. toouse hello új készletet létrehozta, frissítés hello csatolt Azure Batch szolgáltatás hello Data Factory-megoldásban. (Lásd a 4. lépés: hozzon létre és futtasson hello csővezeték további hello **virtuális gépenként feladatok maximális** beállítás.)
4. Az Azure Batch-készlet létrehozása **automatikus skálázás** szolgáltatás. Hello dinamikusan állítható lesz az feldolgozási kapacitása révén az alkalmazás által használt Azure Batch-készlet számítási csomópontjainak automatikus skálázás. 

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
5. A hello minta megoldás hello **Execute** metódus meghívja hello **Calculate** módszer, amely egy bemeneti adatok szelet tooproduce egy kimeneti adatszelet dolgozza fel. A saját módszer tooprocess bemeneti adatai, és cserélje le hello Calculate metódus hívása az Execute metódus hello tooyour telefonhívás írhat.

### <a name="next-steps-consume-hello-data"></a>További lépések: hello adatokat
Adatok feldolgozása után felhasználhat az online eszközökkel, például a **Microsoft Power BI**. Az alábbiakban a hivatkozások toohelp megismerte a Power bi-ban, és hogyan toouse azt az Azure-ban:

* [A Power BI dataset felfedezés](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Bevezetés a hello Power BI Desktop használatába](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Adatok frissítése a Power bi-ban](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure és a Power BI – alapvető – áttekintés](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Referencia
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Bevezetés tooAzure Data Factory szolgáltatásnak](data-factory-introduction.md)
  * [Ismerkedés az Azure Data Factory](data-factory-build-your-first-pipeline.md)
  * [Egyéni tevékenységek használata Azure Data Factory-folyamatban](data-factory-use-custom-activities.md)
* [Az Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Az Azure Batch alapjai](../batch/batch-technical-overview.md)
  * [Azure Batch funkcióinak áttekintése](../batch/batch-api-basics.md)
  * [Létrehozása és kezelése az Azure Batch-fiók hello Azure-portálon](../batch/batch-account-create-portal.md)
  * [Ismerkedés az Azure Batch Library .NET](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
