---
title: "aaaTutorial - használata hello Azure Batch ügyféloldali kódtára a .NET-hez |} Microsoft Docs"
description: "További tudnivalók az Azure Batch hello alapvető fogalmait, és egyszerű megoldás létrehozása a .NET használatával."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 76cb9807-cbc1-405a-8136-d1e53e66e82b
ms.service: batch
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06062b3886a8081bd9a831824a981503ef55f9b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-building-solutions-with-hello-batch-client-library-for-net"></a>Ismerkedés a .NET-alapú megoldásaikat hello kötegelt ügyféloldali kódtár

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

A hello alapvető [Azure Batch] [ azure_batch] és hello [Batch .NET] [ net_api] könyvtár ebben a cikkben egy C# minta alkalmazás lépésről arról lesz szó, a lépést. Úgy tekintünk, hogyan hello mintaalkalmazás hello Batch szolgáltatás tooprocess használja. Ez a párhuzamos munkaterhelés hello felhőben, és milyen hatással van az [Azure Storage](../storage/common/storage-introduction.md) fájl átmeneti és lekérése. Kell egy közös kötegelt alkalmazás munkafolyamata bemutatása és szerezzen egy alapszintű hello fő összetevőit kötegelt feladatok, feladatok, készletek megértése és számítási csomópontjain.

![Batch-megoldás munkafolyamata (alapszintű)][11]<br/>

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk a C# és a Visual Studio gyakorlati ismeretét feltételezi. Azt is feltételezi, hogy Ön képes toosatisfy hello létrehozása követelmények az alább megadott Azure- és hello Batch-és tárolási szolgáltatások.

### <a name="accounts"></a>Fiókok
* **Azure-fiók**: Ha még nincs Azure-előfizetése, [hozzon létre egy ingyenes Azure-fiókot][azure_free_account].
* **Batch-fiók**: Ha már rendelkezik Azure-előfizetéssel, [hozzon létre egy Azure Batch-fiókot](batch-account-create-portal.md).
* **Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) cikk [Tárfiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account) szakaszát.

> [!IMPORTANT]
> Jelenleg a Batch-támogatja *csak* hello **általános célú** tárfióktípus, #5. lépésben leírtak [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) a [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).
>
>

### <a name="visual-studio"></a>Visual Studio
Rendelkeznie kell **Visual Studio 2015-ös vagy újabb** toobuild hello mintaprojektet. Visual Studio ingyenes, mind a próbaverziós verziójának hello található [Visual Studio termékek áttekintése][visual_studio].

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial* kódminta
Hello [DotNetTutorial] [ github_dotnettutorial] minta egyike sok kötegelt mintakódok található hello hello [azure-köteg-minták] [ github_samples] tárházából GitHub. Összes hello minta kattintva letöltheti **Klónozás vagy letöltési > töltse le a ZIP-** hello tárház kezdőlapján, vagy kattintson a hello [azure-köteg-minták-master.zip] [ github_samples_zip]közvetlen letöltési hivatkozását. Miután kibontotta már hello hello ZIP-fájl tartalmát, található hello megoldás hello a következő mappát:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Batch Explorer (nem kötelező)
Hello [Azure Batch Explorer] [ github_batchexplorer] szabad segédprogram, amely része a hello [azure-köteg-minták] [ github_samples] GitHub tárházából. Nem szükséges toocomplete közben ebben az oktatóanyagban ez hasznos lehet a fejlesztési és a kötegelt megoldások hibakeresési közben.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial mintaprojekt áttekintése
Hello *DotNetTutorial* kódminta egy Visual Studio megoldás, amely két projektet tartalmaz: **DotNetTutorial** és **TaskApplication**.

* **DotNetTutorial** hello ügyfél alkalmazás, amely a számítási csomópontok (virtuális gépek) párhuzamos terhelése hello kötegelt és tárolási szolgáltatások tooexecute kommunikál. A DotNetTutorial a helyi munkaállomáson fut.
* **TaskApplication** Azure tooperform hello tényleges munka számítási csomópontjain futó hello program. Hello mintában `TaskApplication.exe` elemez hello (hello bemeneti fájl) Azure Storage-ból letöltött fájlok szöveget. Ezután azt szöveges fájlt hoz létre (hello kimeneti fájl), amely hello a három legfontosabb szereplő szavakkal hello bemeneti fájl listáját tartalmazza. Miután létrehoz hello kimeneti fájlt, TaskApplication fájlfeltöltések hello fájl tooAzure tárolási. Így elérhető toohello ügyfélalkalmazás letölthető. TaskApplication hello Batch szolgáltatás több számítási csomóponton párhuzamosan fut.

hello következő diagram azt ábrázolja, hello hello ügyfélalkalmazás, által elvégzett elsődleges műveletek *DotNetTutorial*, és hello alkalmazás hello feladatok által végrehajtott *TaskApplication*. Ez az alapvető munkafolyamat számos, a Batch használatával létrehozott számítási megoldásra jellemző. Minden elérhető, a Batch szolgáltatás hello szolgáltatást nem azt mutatják, amíg a szinte minden kötegelt az eset tartalmazza a munkafolyamat részeit.

![Példa Batch-munkafolyamat][8]<br/>

[**1. lépés**](#step-1-create-storage-containers) **Tárolók** létrehozása az Azure Blob Storage-ban.<br/>
[**2. lépés**](#step-2-upload-task-application-and-data-files) Feltöltési feladat fájljainak és a bemeneti fájlok toocontainers.<br/>
[**3. lépés**](#step-3-create-batch-pool) Batch-**készlet** létrehozása.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** készlet hello **StartTask** letöltések hello feladat bináris fájlokat (TaskApplication) toonodes, mivel azok csatlakoztatását hello készlet.<br/>
[**4. lépés**](#step-4-create-batch-job) Batch-**feladat** létrehozása.<br/>
[**5. lépés**](#step-5-add-tasks-to-job) Adja hozzá **feladatok** toohello feladat.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** hello feladatok ütemezett tooexecute csomópontján.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Mindegyik tevékenység letölti a bemeneti adatait az Azure Storage-ból, majd elkezdi a végrehajtást.<br/>
[**6. lépés**](#step-6-monitor-tasks) Tevékenységek figyelése.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Feladatok befejezésekor, ezek a kimeneti adatok tooAzure tárolási feltöltése.<br/>
[**7. lépés**](#step-7-download-task-output) Tevékenység kimenetének letöltése a Storage-ból.

Ahogy azt korábban említettük, nem minden kötegelt megoldásnak a pontos lépéseket végzi el, és előfordulhat, hogy sok más többek között hello *DotNetTutorial* mintaalkalmazás található kötegelt megoldás az általános folyamatoktól mutatja be.

## <a name="build-hello-dotnettutorial-sample-project"></a>Build hello *DotNetTutorial* mintaprojektet
Hello minta sikeres futtatásához, meg kell adnia kötegelt és a tárolási fiók hitelesítő adatait hello *DotNetTutorial* projekt `Program.cs` fájlt. Ha még nem tette meg, nyissa meg hello megoldást a Visual Studio hello duplán kattintva `DotNetTutorial.sln` megoldásfájlt. Nyissa meg a Visual Studio használatával hello vagy **fájl > Nyissa meg a > Projekt/megoldás** menü.

Nyissa meg `Program.cs` belül hello *DotNetTutorial* projekt. Majd adja hozzá a hitelesítő adatok megadott hello tetején hello fájlt:

```csharp
// Update hello Batch and Storage account credential strings below with hello values
// unique tooyour accounts. These are used when constructing connection strings
// for hello Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> Fent említett jelenleg adjon meg hitelesítő adatokat hello egy **általános célú** storage Azure Storage-fiókot. A Batch-alkalmazások használata a blob storage belül hello **általános célú** storage-fiók. Ne adjon meg hello hello kiválasztásával létrehozott tárfiók hitelesítő adatainak *Blob-tároló* fiók típusa.
>
>

A kötegelt és a tárolási fiók hitelesítő adataival belül minden szolgáltatás hello fiók panelen található hello [Azure-portálon][azure_portal]:

![A Batch-hitelesítő adatok hello portálon][9]
![hello portal tároló hitelesítő adatait][10]<br/>

Most, hogy a hitelesítő adataival hello projekt frissítette, kattintson a jobb gombbal a hello megoldás a Megoldáskezelőben, majd kattintson **megoldás fordítása**. Hello helyreállítása a minden NuGet-csomagok, győződjön meg arról, ha a számítógép.

> [!TIP]
> Ha hello NuGet-csomagok nem automatikusan vissza, vagy egy hiba toorestore hello csomagok hibába ütközik, ellenőrizze, hogy rendelkezik-e hello [NuGet-Csomagkezelő] [ nuget_packagemgr] telepítve. Engedélyezze a hello letöltése a csomagok hiányzik. Lásd: [engedélyezése csomag visszaállítása során Build] [ nuget_restore] tooenable csomag letöltése.
>
>

A következő részekben hello azt felosztása le hello mintaalkalmazás hello lépéseket, hogy tooprocess segítségével végzi a Batch szolgáltatás hello munkaterhelés, és ezeket a lépéseket részletesen ismertetik. Javasoljuk, toorefer toohello megoldás nyissa meg a Visual Studióban, mivel nem minden kódsort hello minta tárgyalt a módja, ez a cikk többi hello útján munka közben.

Keresse meg a toohello felső részén hello `MainAsync` metódus a hello *DotNetTutorial* projekt `Program.cs` fájl toostart az 1. lépés. Minden egyes lépést alatt, majd nagyjából követi hello előmenetel metódus meghívja `MainAsync`.

## <a name="step-1-create-storage-containers"></a>1. lépés: Storage-tárolók létrehozása
![Tárolók létrehozása az Azure Storage-ban][1]
<br/>

A Batch beépített támogatást tartalmaz az Azure Storage használatához. A tárfiók a tárolók hello feladatok futtatása a Batch-fiók szükséges hello fájlokat ad meg. hello tárolók nagy előnye, egy hely toostore hello kimeneti adatok, amelyek hello feladatok egymással. először thing hello hello *DotNetTutorial* ügyfél alkalmazás hozható létre a három tároló [Azure Blob Storage](../storage/common/storage-introduction.md):

* **alkalmazás**: Ez a tároló hello feladatok, valamint minden függőségét, például DLL-ek által futtatott hello alkalmazás fogja tárolni.
* **bemeneti**: feladatok töltik le hello adatok fájlok tooprocess hello *bemeneti* tároló.
* **kimeneti**: a bemeneti fájl feldolgozása végeztével a feladatok hello eredmények toohello feltölti *kimeneti* tároló.

Egy tároló rendelés toointeract a fiókot, és tárolók, hozzon létre hello használjuk [Azure Storage ügyféloldali kódtára a .NET][net_api_storage]. Egy hivatkozási toohello fiókhoz létrehozhatunk [CloudStorageAccount][net_cloudstorageaccount], és, hogy hozzon létre egy [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct hello Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve hello storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create hello blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Hello használjuk `blobClient` hello alkalmazás teljes hivatkoznak, és adja át paraméterként tooseveral módszerek. Például az utána következő hello fenti, ahol nevezzük hello kódblokk van `CreateContainerIfNotExistAsync` tooactually hello tárolók létrehozása.

```csharp
// Use hello blob client toocreate hello containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Hello tárolók létrehozása után, hello alkalmazás most már feltöltheti hello feladatok által használt hello fájlokat.

> [!TIP]
> [Hogyan toouse Blob Storage a .NET-](../storage/blobs/storage-dotnet-how-to-use-blobs.md) jó áttekintést nyújt az Azure Storage tárolók és blobok használata. Meg kell az olvasási lista hello tetején, az Batch használatának megkezdése.
>
>

## <a name="step-2-upload-task-application-and-data-files"></a>2. lépés: Tevékenységalkalmazás- és adatfájlok feltöltése
![Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers][2]
<br/>

Hello fájl feltöltése a művelet, *DotNetTutorial* először határozza meg a gyűjteményei **alkalmazás** és **bemeneti** elérési utak fájlt a helyi számítógépen hello léteznek. Majd ezen fájlok toohello tárolók hello előző lépésben létrehozott feltölti azt.

```csharp
// Paths toohello executable and its dependencies that will be executed by hello tasks
List<string> applicationFilePaths = new List<string>
{
    // hello DotNetTutorial project includes a project reference tooTaskApplication,
    // allowing us toodetermine hello path of hello task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// hello collection of data files that are toobe processed by hello tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload hello application and its dependencies tooAzure Storage. This is the
// application that will process hello data files, and will be executed by each
// of hello tasks on hello compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload hello data files. This is hello data that will be processed by each of
// hello tasks that are executed on hello compute nodes within hello pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

A két módszer `Program.cs` hello feltöltési folyamat részt:

* `UploadFilesToContainerAsync`: Ez a módszer gyűjteményének beolvasása az [ResourceFile] [ net_resourcefile] objektumok (alább), és belső hívások `UploadFileToContainerAsync` minden fájlt tooupload átadott hello *filePaths* paraméter.
* `UploadFileToContainerAsync`: Ez a ténylegesen hello fájlfeltöltés hajt végre, és létrehozza a hello hello módszer [ResourceFile] [ net_resourcefile] objektumok. Hello fájl feltöltése után azt jut hozzá a közös hozzáférésű jogosultságkód (SAS) hello fájl és egy azt képviselő ResourceFile objektum beállítása/beolvasása. A közös hozzáférésű jogosultságkódok ismertetése is az alábbiakban olvasható.

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} toocontainer [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath);

        // Set hello expiry time and permissions for hello blob shared access signature.
        // In this case, no start time is specified, so hello shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct hello SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles
A [ResourceFile] [ net_resourcefile] biztosít hello URL-cím tooa fájl, amely letöltött tooa Azure Storage a kötegelt feladatok számítási csomópont e feladat futása előtt. Hello [ResourceFile.BlobSource] [ net_resourcefile_blobsource] tulajdonság hello teljes URL-címet hello fájl határozza meg, az Azure Storage található. hello URL-címet is a közös hozzáférésű jogosultságkód (SAS), amely toohello fájlt biztonságos hozzáférést biztosít. A Batch .NET-en belüli legtöbb tevékenység tartalmaz egy *ResourceFiles* tulajdonságot, beleértve a következőket:

* [CloudTask][net_task]
* [StartTask][net_pool_starttask]
* [JobPreparationTask][net_jobpreptask]
* [JobReleaseTask][net_jobreltask]

hello DotNetTutorial mintaalkalmazás hello JobPreparationTask vagy JobReleaseTask tevékenységtípusok nem használható, de további a rájuk vonatkozó [Futtatás feladat előkészítése és a befejezési feladatok Azure Batch számítási csomópontjain](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Közös hozzáférésű jogosultságkód (SAS)
Közös hozzáférésű aláírások karakterláncok, amelyek – amikor egy URL-cím részét képező – adja meg a biztonságos hozzáférést toocontainers és az Azure Storage blobs. hello DotNetTutorial alkalmazás mindkét blob és tároló megosztott hozzáférési aláírást URL-címeket, és bemutatja, hogyan ezek megosztott tooobtain elérje aláírás karakterláncok hello tároló szolgáltatást.

* **A BLOB megosztott hozzáférési aláírásokkal**: hello készlet StartTask DotNetTutorial a blob megosztott hozzáférési aláírásokkal használ, letöltésekor hello alkalmazás bináris fájljait és a bemeneti adatok fájlok a tároló (lásd az alábbi #3. lépés). Hello `UploadFileToContainerAsync` DotNetTutorial tartozó metódus `Program.cs` hello kódot tartalmaz, amely minden egyes blob megosztott hozzáférési aláírást beolvassa. Ezt a [CloudBlob.GetSharedAccessSignature][net_sas_blob] meghívásával végzi el.
* **Tároló közös hozzáférésű jogosultságkód**: minden tevékenység befejezése hello számítási csomóponton teendőit, mivel azt feltölti a kimeneti fájl toohello *kimeneti* az Azure Storage tárolóban. toodo tehát TaskApplication használ egy tároló közös hozzáférésű jogosultságkódot, amely írási hozzáférés toohello tárolót biztosít hello elérési út részeként, ha azt feltölt hello fájlt. Nem sikerült beolvasni hello tároló közös hozzáférésű jogosultságkódot, ha nem sikerült beolvasni hello blob megosztott hozzáférési aláírást hasonló módon végezhető el. DotNetTutorial, adott hello található `GetContainerSasUrl` segítő metódushívások [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] toodo így. Lesz olvasható további információ arról, hogyan TaskApplication használja hello tároló megosztott hozzáférési aláírást a "6. lépés: figyelési feladatok."

> [!TIP]
> A közös hozzáférésű jogosultságkódokról hello kétrészes adatsorozat kivételének [1. rész: Understanding hello megosztott hozzáférési jogosultságkód (SAS) alapuló modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) és [2. rész: létrehozása és a közös hozzáférésű jogosultságkód (SAS) használata a Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), további biztosítanak, és a tárfiókban lévő biztonságos hozzáférést toodata toolearn.
>
>

## <a name="step-3-create-batch-pool"></a>3. lépés: Batch-készlet létrehozása
![Batch-készlet létrehozása][3]
<br/>

A Batch-**készletek** olyan számítási csomópontok (virtuális gépek) gyűjteményei, amelyeken a Batch a feladatok tevékenységeit végzi el.

Hello alkalmazás és adatok fájlok toohello Azure Storage API-khoz, tárfiók feltöltése után *DotNetTutorial* hívások toohello Batch szolgáltatás végez hello Batch .NET kódtár által nyújtott API-khoz. hello kód először létrehoz egy [BatchClient][net_batchclient]:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

A következő hello minta létrehoz számítási csomópontok készletét a Batch-fiók hello hívással túl`CreatePoolIfNotExistsAsync`. `CreatePoolIfNotExistsAsync`felhasználási hello [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metódus toocreate egy új készletet a Batch szolgáltatás hello:

```csharp
private static async Task CreatePoolIfNotExistAsync(BatchClient batchClient, string poolId, IList<ResourceFile> resourceFiles)
{
    CloudPool pool = null;
    try
    {
        Console.WriteLine("Creating pool [{0}]...", poolId);

        // Create hello unbound pool. Until we call CloudPool.Commit() or CommitAsync(), no pool is actually created in the
        // Batch service. This CloudPool instance is therefore considered "unbound," and we can modify its properties.
        pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicatedComputeNodes: 3,                                             // 3 compute nodes
            virtualMachineSize: "small",                                                // single-core, 1.75 GB memory, 225 GB disk
            cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));   // Windows Server 2012 R2

        // Create and assign hello StartTask that will be executed when compute nodes join hello pool.
        // In this case, we copy hello StartTask's resource files (that will be automatically downloaded
        // toohello node by hello StartTask) into hello shared directory that all tasks will have access to.
        pool.StartTask = new StartTask
        {
            // Specify a command line for hello StartTask that copies hello task application files toothe
            // node's shared directory. Every compute node in a Batch pool is configured with a number
            // of pre-defined environment variables that can be referenced by commands or applications
            // run by tasks.

            // Since a successful execution of robocopy can return a non-zero exit code (e.g. 1 when one or
            // more files were successfully copied) we need toomanually exit with a 0 for Batch toorecognize
            // StartTask execution success.
            CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
            ResourceFiles = resourceFiles,
            WaitForSuccess = true
        };

        await pool.CommitAsync();
    }
    catch (BatchException be)
    {
        // Swallow hello specific error code PoolExists since that is expected if hello pool already exists
        if (be.RequestInformation?.BatchError != null && be.RequestInformation.BatchError.Code == BatchErrorCodeStrings.PoolExists)
        {
            Console.WriteLine("hello pool {0} already existed when we tried toocreate it", poolId);
        }
        else
        {
            throw; // Any other exception is unexpected
        }
    }
}
```

A készlet létrehozásakor [CreatePool][net_pool_create], akkor adja meg például a számítási csomópontok hello számos paraméter hello [hello csomópontok méret](../cloud-services/cloud-services-sizes-specs.md), és működési csomópontok hello a rendszer. A *DotNetTutorial*, használjuk [CloudServiceConfiguration] [ net_cloudserviceconfiguration] a Windows Server 2012 R2 toospecify [Felhőszolgáltatások](../cloud-services/cloud-services-guestos-update-matrix.md). 

Hello megadásával is létrehozhat, amelyek az Azure virtuális gépek (VM) számítási csomópontok készleteinek [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] a készlethez. A virtuális gépek számításicsomópont-készletei Windows- vagy [Linux-rendszerképből](batch-linux-nodes.md) hozhatóak létre. a Virtuálisgép-lemezképek hello forrása is lehet:

- Hello [Azure virtuális gépek piactér][vm_marketplace], amely biztosítja, hogy Windows és Linux lemezképeket, amelyek használatra kész. 
- Egy Ön által előkészített és biztosított, egyéni rendszerkép. További részletek az egyéni rendszerképekről: [Nagy léptékű párhuzamos számítási megoldások fejlesztése a Batch segítségével](batch-api-basics.md#pool).

> [!IMPORTANT]
> A Batchben lévő számítási erőforrások díjkötelesek. csökkentheti toominimize költségek `targetDedicatedComputeNodes` too1 hello minta futtatása előtt.
>
>

A fizikai csomópont tulajdonságok együtt is megadhat egy [StartTask] [ net_pool_starttask] hello készlet. hello StartTask végrehajtása minden egyes csomóponton, hogy a csomópont csatlakozik hello készletet, és minden alkalommal, amikor újraindítja a csomópontot. hello StartTask esetén különösen hasznos alkalmazásokat telepít a számítási csomópontok előzetes toohello feladatok végrehajtását. Például a feladatok adatok feldolgozása a Python-parancsfájlok használatával, használhatja a StartTask tooinstall Python hello számítási csomóponton.

A mintaalkalmazás hello StartTask hello tárolásból tölt le fájlokat másolja át. (amely hello használatával megadott [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] tulajdonság) hello StartTask working directory toohello megosztott könyvtárból, amely *összes* hello csomóponton futó feladatok férhetnek hozzá. Alapvetően, ennek `TaskApplication.exe` és a függőségek toohello, megosztott directory minden egyes csomóponton hello csomópont csatlakozik hello készlet, mivel így hello csomóponton futó minden feladatot-e hozzáférési engedélye.

> [!TIP]
> Hello **alkalmazáscsomagok** az Azure Batch szolgáltatás más módon tooget biztosít az alkalmazás telepítését a készlet számítási csomópontjai hello. Lásd: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md) részleteiről.
>
>

A fenti hello kódrészletet lényeges hello hello két környezeti változók használatát a rendszer *CommandLine* hello StartTask tulajdonsága: `%AZ_BATCH_TASK_WORKING_DIR%` és `%AZ_BATCH_NODE_SHARED_DIR%`. A Batch-készlet belül minden számítási csomópont automatikusan a több környezeti változókat, amelyek adott tooBatch van konfigurálva. E feladat által végrehajtott folyamat rendelkezik hozzáférési toothese környezeti változókat.

> [!TIP]
> További információk a számítási csomópontok a Batch-készlet és a feladat működő könyvtárak, információkat elérhető hello környezeti változók toofind lásd: hello [környezeti beállítások feladatok](batch-api-basics.md#environment-settings-for-tasks) és [fájlok és könyvtárak ](batch-api-basics.md#files-and-directories) hello részeiben [Batch funkcióinak áttekintése a fejlesztők](batch-api-basics.md).
>
>

## <a name="step-4-create-batch-job"></a>4. lépés: Batch-feladat létrehozása
![Batch-feladat létrehozása][4]<br/>

A Batch-**feladatok** számítási csomópontok készletéhez társított tevékenységek gyűjteményei. hello feladatot a feladatok végrehajtása tartozó hello készlet számítási csomópontok.

Használhatja a feladat nem csak rendszerezése és nyomon követni a feladatokat a kapcsolódó munkaterheléseknél, hanem is előíró bizonyos megkötéseknek – például a maximális futási hello hello feladat (és -kiterjesztés, a feladatok által) valamint a kapcsolat tooother feladatok hello kötegelt feladat prioritása fiók. Ebben a példában azonban hello feladat tartozik csak a #3. lépésben létrehozott alkalmazáskészlet hello. Nincsenek konfigurálva további tulajdonságok.

Mindegyik Batch-feladat egy adott készlettel van társítva. Ez a társítás hello feladat tevékenységeit fogja végrehajtani a csomópontok jelzi. Ezek megadása hello segítségével [CloudJob.PoolInformation] [ net_job_poolinfo] tulajdonság, ahogy az alábbi hello kódrészlet.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Most, hogy a feladat létrehozása után feladatok tooperform hello munkahelyi lettek hozzáadva.

## <a name="step-5-add-tasks-toojob"></a>5. lépés: A feladatok toojob hozzáadása
![Feladatok toojob hozzáadása][5]<br/>
*(1) feladatok toohello feladat kerülnek, (2) hello feladatok ütemezett toorun csomópontján, és (3) hello feladatok hello adatok fájlok tooprocess letöltése*

Kötegelt **feladatok** hello egyes Munkaegységek hello hajt végre, a számítási csomópontok. A feladatot a parancssor és hello parancsfájlok vagy végrehajtható fájlok, amely megadja, hogy a parancssor futtatja.

tooactually feladatok végrehajtására, a feladatok tooa feladat hozzá kell adni. Minden egyes [CloudTask] [ net_task] parancssori tulajdonság használatával van konfigurálva, és [ResourceFiles] [ net_task_resourcefiles] (például hello készlet StartTask), hello feladat toohello csomópont tölti le, a parancssor automatikusan végrehajtása előtt. A hello *DotNetTutorial* mintaprojektet, minden tevékenység csak egy fájl dolgozza fel. A ResourceFiles gyűjtemény így egyetlen elemet tartalmaz.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks toojob [{1}]...", inputFiles.Count, jobId);

    // Create a collection toohold hello tasks that we'll be adding toohello job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of hello tasks. Because we copied hello task application toothe
    // node's shared directory with hello pool's StartTask, we can access it via
    // hello shared directory on hello node that hello task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add hello tasks as a collection, as opposed tooissuing a separate AddTask call
    // for each. Bulk task submission helps tooensure efficient underlying API calls
    // toohello Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> Ha környezeti változók hozzáférésük van például `%AZ_BATCH_NODE_SHARED_DIR%` vagy egy alkalmazás nem található a hello csomópont végrehajtása `PATH`, feladat parancssorokat kell előtagként `cmd /c`. Ezzel explicit módon hello parancsértelmező hajtható végre, és kérje meg az tooterminate a parancs végrehajtása után. Ez a követelmény nem szükséges, ha a feladatok végrehajtása egy alkalmazás hello csomópontban `PATH` (például *robocopy.exe* vagy *powershell.exe*), és nem környezeti változókat szolgálnak.
>
>

Hello belül `foreach` hurok a fenti hello kódrészletet, láthatja, hogy úgy, hogy három parancssori argumentumok túl átadott összeállított hello feladat parancssorának hello*TaskApplication.exe*:

1. Hello **első argumentum** hello fájl tooprocess hello elérési útja. Ez az hello helyi elérési út toohello fájlt, mert létezik hello csomóponton. Ha hello ResourceFile objektum `UploadFileToContainerAsync` létrehozásának újabb hello fájlnév volt megadva ehhez a tulajdonsághoz (paraméter toohello ResourceFile konstruktorban). Ez azt jelzi, hogy hello fájl található hello azonos könyvtárhoz, mint a *TaskApplication.exe*.
2. Hello **második argumentum** határozza meg, hogy hello felső *N* szavak toohello kimeneti fájl kell írni. Hello mintában ez van kódolva, hogy hello felső három szavak írt toohello kimeneti fájl.
3. Hello **harmadik argumentum** hello közös hozzáférésű jogosultságkód (SAS), amely írási toohello van **kimeneti** az Azure Storage tárolóban. *TaskApplication.exe* használja ezt a megosztott aláírás URL-cím érhető el, amikor feltölti a hello kimeneti fájl tooAzure tároló. Hello kód ehhez az található hello `UploadFileToContainer` hello TaskApplication projektben metódus `Program.cs` fájlt:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference toohello container using hello SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload hello file (as a new blob) toohello container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when hello Batch service
                // sets hello CloudTask.ExecutionInformation.ExitCode for hello task that
                // executed this application, it properly indicates that there was a
                // problem with hello task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>6. lépés: Tevékenységek figyelése
![Tevékenységek figyelése][6]<br/>
*hello ügyfél alkalmazás (1) figyelők hello-feladatok befejezési és sikeres állapotát, és (2) hello feladatok feltöltés eredmény adatok tooAzure tároló*

Feladatok tooa feladatot ad hozzá, amikor ezeket automatikusan aszinkron és számítási csomóponton belül hello hello feladattal társított végrehajtásra ütemezni. Hello megadott beállítások alapján, kötegelt kezeli az összes feladat queuing, ütemezés, újrapróbálkozás és más tevékenység felügyeleti feladatokat meg.

Nincsenek számos különböző módszer toomonitoring feladat végrehajtása. A DotNetTutorial egy egyszerű példát mutat be, amely csak a tevékenységek befejezéséről, illetve a meghiúsult vagy a sikeres állapotról küld jelentést. Hello belül `MonitorTasks` DotNetTutorial tartozó metódus `Program.cs`, vitafórum szavatolja három Batch .NET fogalmat van. Az alábbiakban láthatja ezeket a megjelenésük sorrendjében:

1. **ODATADetailLevel**: Az [ODATADetailLevel][net_odatadetaillevel] meghatározása a listaműveletekben (amilyen például a feladatok tevékenységlistáinak beszerzése) elengedhetetlen a Batch-alkalmazások megfelelő teljesítményének biztosítása érdekében. Adja hozzá [hello Azure Batch szolgáltatás hatékony lekérdezéséhez](batch-efficient-list-queries.md) tooyour olvasási lista, ha tervezi ennek során tetszőleges belül a Batch-alkalmazások figyelése.
2. **TaskStateMonitor**: A [TaskStateMonitor][net_taskstatemonitor] segédprogramokat biztosít a Batch .NET-alkalmazásoknak a feladatállapotok megfigyeléséhez. A `MonitorTasks`, *DotNetTutorial* megvárja-e a minden feladat tooreach [TaskState.Completed] [ net_taskstate] a határidőn belül. Ezután a rendszer megszakítja hello feladat.
3. **TerminateJobAsync**: egy feladatot leállítja [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (vagy hello blokkolja JobOperations.TerminateJob) befejezettként jelöli meg a feladatot. Alapvető toodo, ha a köteg megoldást használ egy [JobReleaseTask][net_jobreltask]. Ez egy speciális típusú feladat, amelyről a [feladatok előkészítési és befejezési tevékenységeit](batch-job-prep-release.md) ismertető szakaszban olvashat.

Hello `MonitorTasks` módszerrel *DotNetTutorial*tartozó `Program.cs` alatt jelenik meg:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed tooreach hello Completed state within hello timeout period.";

    // Obtain hello collection of tasks currently managed by hello job. Note that we use
    // a detail level too specify that only hello "id" property of each task should be
    // populated. Using a detail level for all list operations helps toolower
    // response time from hello Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor toomonitor hello state of our tasks. In this case, we
    // will wait for all tasks tooreach hello Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached hello "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property tooensure that it did not encounter a failure
    // or return a non-zero exit code.

    // Update hello detail level toopopulate only hello task id and executionInfo
    // properties. We refresh hello tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate hello task's properties with hello latest info from hello Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.Result == TaskExecutionResult.Failure)
        {
            // A task with failure information set indicates there was a problem with hello task. It is important toonote that
            // hello task's state can be "Completed," yet still have encountered a failure.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a failure: {1}", task.Id, task.ExecutionInformation.FailureInformation.Message);
            if (task.ExecutionInformation.ExitCode != 0)
            {
                // A non-zero exit code may indicate that hello application executed by hello task encountered an error
                // during execution. As not every application returns non-zero on failure by default (e.g. robocopy),
                // your implementation of error checking may differ from this example.

                Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
            }
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within hello specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>7. lépés: Feladat kimenetének letöltése
![Tevékenység kimenetének letöltése a Storage-ból][7]<br/>

Most, hogy hello feladat befejezését követően hello feladatok kimenete hello Azure Storage-ból letölthető. Ez a lépés hívása túl`DownloadBlobsFromContainerAsync` a *DotNetTutorial*tartozó `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference tooa previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all hello block blobs in hello specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference toohello current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents tooa file in hello specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded too{0}", directoryPath);
}
```

> [!NOTE]
> hívás túl hello`DownloadBlobsFromContainerAsync` a hello *DotNetTutorial* alkalmazás megadhatja, hogy hello fájlok letöltött tooyour `%TEMP%` mappa. Szabad toomodify érzi, hogy a kimeneti helyet.
>
>

## <a name="step-8-delete-containers"></a>8. lépés: Tárolók törlése
Mivel van szó, az Azure-tárolóban lévő adatok, még mindig egy jó ötlet tooremove blobokat, már nem szükséges a kötegelt feladatok. A DotNetTutorial tartozó `Program.cs`, ez a lépés három hívások toohello segédmetódus `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

hello metódusból csupán jut hozzá a hivatkozás toohello tárolója, és ekkor meghívja a [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a>9. lépés: Hello feladat és hello készlet törlése
Hello utolsó lépést, az Ön felszólító toodelete hello feladat és hello készlet hello DotNetTutorial alkalmazás által létrehozott. Bár magukért a munkákért és feladatokért nem kell fizetnie, a számítási csomópontokért *igen*. Ezért ajánlott csak szükség szerint lefoglalni a csomópontokat. A nem használt készletek törlése a karbantartási folyamat része lehet.

hello BatchClient [JobOperations] [ net_joboperations] és [PoolOperations] [ net_pooloperations] egyaránt rendelkezik a megfelelő törlési metódussal, ha néven hello felhasználó megerősíti, hogy törlése:

```csharp
// Clean up hello resources we've created in hello Batch account if hello user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [!IMPORTANT]
> Ne feledje, hogy a számítási erőforrások díjkötelesek – a nem használt készletek törlése minimalizálja a költségeket. Emellett vegye figyelembe, hogy törli a készlet törlése belül, hogy minden számítási csomópontokat, és, hogy bármely hello csomópontok adatainak lesz helyreállíthatatlan hello készlet törlése után.
>
>

## <a name="run-hello-dotnettutorial-sample"></a>Futtassa a hello *DotNetTutorial* minta
Hello mintaalkalmazás futtatásakor hello a konzol kimeneti lesz hasonló toohello következő. Végrehajtásakor, a szünetelést fog tapasztalni `Awaiting task completion, timeout in 00:30:00...` közben hello készlet számítási csomópontok indulnak el. Használjon hello [Azure-portálon] [ azure_portal] toomonitor a készlet, a számítási csomópontok, a feladat és a feladatok során, és végrehajtása után. Használjon hello [Azure-portálon] [ azure_portal] vagy hello [Azure Tártallózó] [ storage_explorers] tooview hello tárolási erőforrások (tárolók és blobok), amelyek hello alkalmazás által létrehozott.

Tipikus végrehajtási idő **körülbelül 5 percig** futtatásakor hello alkalmazás alapértelmezett konfigurációval.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe toocontainer [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll toocontainer [application]...
Uploading file ..\..\taskdata1.txt toocontainer [input]...
Uploading file ..\..\taskdata2.txt toocontainer [input]...
Uploading file ..\..\taskdata3.txt toocontainer [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks toojob [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within hello specified timeout period.
Downloading all files from container [output]...
All files downloaded tooC:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a>Következő lépések
Szabad toomake módosítások érzi, hogy túl*DotNetTutorial* és *TaskApplication* tooexperiment másik számítási forgatókönyvek. Például, próbálja meg a túl hozzáadni egy végrehajtási késedelem*TaskApplication*, például a [Thread.Sleep][net_thread_sleep], toosimulate hosszan futó feladatokat, és figyelheti azokat hello portálon. Próbálja meg további feladatok hozzáadása vagy módosítása a számítási csomópontok száma hello. Adja hozzá a logikai toocheck és egy meglévő készlet toospeed végrehajtási idő hello használatának engedélyezése (*mutató*: látogasson el `ArticleHelpers.cs` a hello [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] a projekt [azure-köteg-minták][github_samples]).

Most, hogy ismeri a kötegelt megoldás hello alapvető munkafolyamattal, idő toodig a Batch szolgáltatás hello toohello további szolgáltatásait is.

* Felülvizsgálati hello [áttekintés az Azure Batch funkcióinak](batch-api-basics.md) cikk, amely ajánlott, ha még új toohello szolgáltatás.
* A Start hello egyéb kötegelt fejlesztési cikkek alapján **részletes fejlesztési** a hello [kötegelt képzési terv][batch_learning_path].
* Tekintse meg a "legfontosabb N szavak" hello munkaterhelés feldolgozása kötegelt hello segítségével különböző végrehajtásának [TopNWords] [ github_topnwords] minta.
* Felülvizsgálati hello Batch .NET [kibocsátási megjegyzéseket](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) a legfrissebb változásokat hello hello könyvtár.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/vs/
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Tárolók létrehozása az Azure Storage-ban"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Batch-készlet létrehozása"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Batch-feladat létrehozása"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Feladatok toojob hozzáadása"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Tevékenységek figyelése"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Tevékenység kimenetének letöltése a Storage-ból"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Batch-megoldás munkafolyamata (teljes diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "A Batch hitelesítő adatai a portálon"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "A Storage hitelesítő adatai a portálon"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Batch-megoldás munkafolyamata (minimális diagram)"
