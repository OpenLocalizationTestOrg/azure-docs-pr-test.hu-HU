---
title: "a Microsoft Azure Storage adatátviteli könyvtár hello adatok aaaTransfer |} Microsoft Docs"
description: "Hello adatátviteli könyvtár toomove vagy másolat adatok tooor a blob és a fájl tartalmát használja. Adatok tooAzure tárolási átmásolhatja a helyi fájlokból, vagy adatok belül vagy tárfiókok között. Az adatok tooAzure Storage könnyen át."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/22/2017
ms.author: seguler
ms.openlocfilehash: 5de902d132565a8eafdc672f7a1a18e1a2db3a06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-microsoft-azure-storage-data-movement-library"></a>A Microsoft Azure Storage adatátviteli könyvtár hello adatátvitelt

## <a name="overview"></a>Áttekintés
a Microsoft Azure Storage adatátviteli könyvtár hello a platformok közötti nyílt forráskódú-szalagtár egyik célja a nagy teljesítményű feltöltését, letöltése és az Azure Storage Blobs és a fájlok másolása. Ebben a könyvtárban hello alapvető adatátviteli keretrendszeren, amely rendszert működtet [AzCopy](storage-use-azcopy.md). Adatátviteli könyvtár hello biztosít, amelyek nem érhetők el a hagyományos a kényelmes módszert [.NET Azure Storage ügyféloldali kódtár](storage-dotnet-how-to-use-blobs.md). Ez magában foglalja a hello képességét tooset hello párhuzamos műveletek számát, nyomon követése átvitel állapota, egyszerűen folytathatja a megszakított átvitel, és még sok más.  

Ebben a könyvtárban is használja a .NET Core, ami azt jelenti, hogy a .NET-alkalmazások a Windows, Linux és macOS fejlesztéskor használhatja. toolearn .NET Core kapcsolatos további információkért tekintse meg a toohello [.NET Core dokumentációja](https://dotnet.github.io/). Ebben a könyvtárban is működik, a hagyományos .NET-keretrendszer alkalmazások Windows. 

Ez a dokumentum bemutatja, hogyan toocreate a .NET Core Konzolalkalmazás, hogy, hogy fut a Windows, Linux és macOS és hajtja végre a következő forgatókönyvek hello:

- Fájlok és könyvtárak tooBlob tárolási feltöltése.
- Adja meg a hello száma párhuzamos művelet adatátvitel során.
- Track adatok átvitel folyamatban van.
- Végezze el újra a megszakított adatátvitel. 
- Fájl másolása URL-cím tooBlob tároló. 
- A Blob Storage tooBlob tárolási másolása.

**Mire van szüksége:**

* [Visual Studio Code](https://code.visualstudio.com/)
* Egy [Azure-tárfiók](storage-create-storage-account.md#create-a-storage-account)

> [!NOTE]
> Ez az útmutató feltételezi, hogy Ön már ismeri a [Azure Storage](https://azure.microsoft.com/services/storage/). Ha nem, olvasási hello [bemutatása tooAzure tárolási](storage-introduction.md) dokumentáció is hasznos. A legfontosabb, hogy túl kell[hozzon létre egy tárfiókot](storage-create-storage-account.md#create-a-storage-account) toostart használatával hello adatátviteli könyvtár.
> 
> 

## <a name="setup"></a>Beállítás  

1. A Microsoft hello [.NET Core telepítési útmutató](https://www.microsoft.com/net/core) tooinstall .NET Core. A környezet kiválasztásakor válassza hello parancssori kapcsolót. 
2. Hello parancssorból hozzon létre egy könyvtárat a projekthez. Keresse meg az ebben a könyvtárban, majd írja be a `dotnet new` toocreate egy C# konzolos projektet.
3. Nyissa meg a könyvtár a Visual Studio Code. Ez a lépés gyorsan megteheti a hello parancssori beírásával `code .`.  
4. Telepítse a hello [C# bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) a Visual Studio Code piactér hello. Indítsa újra a Visual Studio Code. 
5. Ekkor meg kell jelennie két megjelenő utasításokat. Egyet az hozzáadása "toobuild szükséges eszközök és hibakereső." Kattintson az "Igen" gombra. Egy másik kérdés feloldatlan függőségek visszaállítására van. Kattintson a "visszaállítás".
6. Az alkalmazás most tartalmaznia kell egy `launch.json` hello fájlt `.vscode` könyvtár. Ebben a fájlban, módosítsa a hello `externalConsole` érték túl`true`.
7. A Visual Studio Code toodebug .NET Core alkalmazások lehetővé teszi. Találati `F5` toorun az alkalmazást, és ellenőrizze, hogy működik-e a beállításai. Láthatja a "Hello World!" nyomtatott toohello konzol. 

## <a name="add-data-movement-library-tooyour-project"></a>Adatátviteli könyvtár tooyour projekt hozzáadása

1. Adja hozzá a hello hello adatátviteli könyvtár toohello legújabb verziójának `dependencies` szakasza a `project.json` fájlt. Hello írásának időpontjában ebben a verzióban lenne`"Microsoft.Azure.Storage.DataMovement": "0.5.0"` 
2. Adja hozzá `"portable-net45+win8"` toohello `imports` szakasz. 
3. A kérdés megjelenjen-e toorestore a projekthez. Kattintson a hello "visszaállítás" gombra. Visszaállíthatja a projekt hello parancssorból hello parancs beírásával `dotnet restore` a projektkönyvtárban hello gyökérmappájában.

Módosítsa `project.json`:

    {
      "version": "1.0.0-*",
      "buildOptions": {
        "debugType": "portable",
        "emitEntryPoint": true
      },
      "dependencies": {
        "Microsoft.Azure.Storage.DataMovement": "0.5.0"
      },
      "frameworks": {
        "netcoreapp1.1": {
          "dependencies": {
            "Microsoft.NETCore.App": {
              "type": "platform",
              "version": "1.1.0"
            }
          },
          "imports": [
            "dnxcore50",
            "portable-net45+win8"
          ]
        }
      }
    }

## <a name="set-up-hello-skeleton-of-your-application"></a>Az alkalmazás hello vázat beállítása
hello végezzük elsőként be van állítva hello "skeleton" az alkalmazás kódját. Ez a kód megadását kéri, a Tárfiók nevét és a fiók kulcsának, és használja ezeket a hitelesítő adatok toocreate egy `CloudStorageAccount` objektum. Ez az objektum a Storage-fiók összes átviteli forgatókönyvekben használt toointeract. hello kódot is kér, toochoose hello típusú szeretnénk adatátviteli művelet tooexecute. 

Módosítsa `Program.cs`:

```csharp
using System;
using System.Threading;
using System.Diagnostics;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.DataMovement;

namespace DMLibSample
{
    public class Program
    {
        public static void Main()
        {
            Console.WriteLine("Enter Storage account name:");           
            string accountName = Console.ReadLine();

            Console.WriteLine("\nEnter Storage account key:");           
            string accountKey = Console.ReadLine();

            string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + accountName + ";AccountKey=" + accountKey;
            CloudStorageAccount account = CloudStorageAccount.Parse(storageConnectionString);

            ExecuteChoice(account);
        }

        public static void ExecuteChoice(CloudStorageAccount account)
        {
            Console.WriteLine("\nWhat type of transfer would you like tooexecute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
            int choice = int.Parse(Console.ReadLine());

            if(choice == 1)
            {
                TransferLocalFileToAzureBlob(account).Wait();
            }
            else if(choice == 2)
            {
                TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
            }
            else if(choice == 3)
            {
                TransferUrlToAzureBlob(account).Wait();
            }
            else if(choice == 4)
            {
                TransferAzureBlobToAzureBlob(account).Wait();
            }
        }

        public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
        { 
            
        }

        public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
        { 
            
        }

        public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
        {

        }

        public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
        {

        }
    }
}
```

## <a name="transfer-local-file-tooazure-blob"></a>Helyi fájl tooAzure Blob átvitele
Adjon hozzá hello módszert `GetSourcePath` és `GetBlob` túl`Program.cs`:

```csharp
public static string GetSourcePath()
{
    Console.WriteLine("\nProvide path for source:");
    string sourcePath = Console.ReadLine();

    return sourcePath;
}

public static CloudBlockBlob GetBlob(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    Console.WriteLine("\nProvide name of new Blob:");
    string blobName = Console.ReadLine();
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    return blob;
}
```

Módosítsa a hello `TransferLocalFileToAzureBlob` módszert:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    await TransferManager.UploadAsync(localFilePath, blob);
    Console.WriteLine("\nTransfer operation complete.");
    ExecuteChoice(account);
}
```

Ez a kód megadását kéri az velünk hello elérési tooa helyi fájl, egy új vagy meglévő tároló hello nevét és egy új blob hello nevét. Hello `TransferManager.UploadAsync` metódus hello feltöltés ezen információk alapján hajtja végre. 

Találati `F5` toorun az alkalmazást. Adott hello feltöltés történt a tárfiók hello megtekintésével ellenőrizheti [Microsoft Azure Tártallózó](http://storageexplorer.com/).

## <a name="set-number-of-parallel-operations"></a>Készlet száma párhuzamos művelet
Hello által kínált kiváló szolgáltatás adatátviteli könyvtár hello képességét tooset hello száma párhuzamos művelet átviteli tooincrease hello adatátvitelt. Alapértelmezés szerint hello adatátviteli könyvtár beállítja hello száma párhuzamos művelet too8 * hello a számítógépen lévő magok száma. 

Ne feledje, hogy sok párhuzamos művelet egy kis sávszélességű környezetben ne terhelje tovább hello hálózati kapcsolatot, és ténylegesen megakadályozza a műveletek teljes befejezését. Lesz szükség az ezt a beállítást toodetermine tooexperiment mi működik legjobban alapján a rendelkezésre álló hálózati sávszélességet. 

Adjunk néhány kódot, amely lehetővé teszi tooset hello száma párhuzamos művelet. Adjuk hozzá is, hogy mennyi ideig tart a hello átviteli toocomplete alkalommal kódot.

Adja hozzá a `SetNumberOfParallelOperations` metódus túl`Program.cs`:

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like toouse?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

Módosítsa a hello `ExecuteChoice` metódus toouse `SetNumberOfParallelOperations`:

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like tooexecute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
    int choice = int.Parse(Console.ReadLine());

    SetNumberOfParallelOperations();

    if(choice == 1)
    {
        TransferLocalFileToAzureBlob(account).Wait();
    }
    else if(choice == 2)
    {
        TransferLocalDirectoryToAzureBlobDirectory(account).Wait();
    }
    else if(choice == 3)
    {
        TransferUrlToAzureBlob(account).Wait();
    }
    else if(choice == 4)
    {
        TransferAzureBlobToAzureBlob(account).Wait();
    }
}
```

Módosítsa a hello `TransferLocalFileToAzureBlob` metódus toouse időzítő:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    Console.WriteLine("\nTransfer started...");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="track-transfer-progress"></a>Track átvitel állapota
Annak ismerete, hogy mennyi ideig tartott a saját adatok tootransfer nagy. Azonban a képes toosee hello folyamatban van az átvitel alatt *során* hello adatátviteli művelet még élvezetesebbé lenne. tooachieve ebben az esetben toocreate kell egy `TransferContext` objektum. Hello `TransferContext` objektum két formában származik: `SingleTransferContext` és `DirectoryTransferContext`. korábbi hello (amely azt még tevékenységeit most) egyetlen fájl átvitelére, ez utóbbi hello pedig egy könyvtárat (amely azt később felvenni) fájlok átvitele.

Adjon hozzá hello módszert `GetSingleTransferContext` és `GetDirectoryTransferContext` túl`Program.cs`: 

```csharp
public static SingleTransferContext GetSingleTransferContext(TransferCheckpoint checkpoint)
{
    SingleTransferContext context = new SingleTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });
    
    return context;
}

public static DirectoryTransferContext GetDirectoryTransferContext(TransferCheckpoint checkpoint)
{
    DirectoryTransferContext context = new DirectoryTransferContext(checkpoint);

    context.ProgressHandler = new Progress<TransferStatus>((progress) =>
    {
        Console.Write("\rBytes transferred: {0}", progress.BytesTransferred );
    });
    
    return context;
}
```

Módosítsa a hello `TransferLocalFileToAzureBlob` metódus toouse `GetSingleTransferContext`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account);
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint);
    Console.WriteLine("\nTransfer started...\n");
    Stopwatch stopWatch = Stopwatch.StartNew();
    await TransferManager.UploadAsync(localFilePath, blob, null, context);
    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

## <a name="resume-a-canceled-transfer"></a>A megszakított átvitel folytatása
Egy másik kényelmes hello adatátviteli könyvtár által kínált szolgáltatása hello képességét tooresume visszavont átvitelt. Adjunk néhány kódot, amely lehetővé teszi tootemporarily Mégse hello átviteli írja be a `c`, és ezután úgy folytatódik, hello átviteli 3 másodperc múlva.

Módosítsa `TransferLocalFileToAzureBlob`:

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.UploadAsync(localFilePath, blob, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadAsync(localFilePath, blob, null, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Eddig a `checkpoint` mindig beállítás túl`null`. Most hello átviteli megszakítása, ha azt hello utolsó ellenőrzési pont a átvitel beolvasni, akkor használja ezt az új ellenőrzőpontot az átvitel a környezetben. 

## <a name="transfer-local-directory-tooazure-blob-directory"></a>Helyi könyvtár tooAzure Blob directory átvitele
Címkézést lenne, ha hello adatátviteli könyvtár csak továbbíthat egy fájlban egyszerre. Szerencsére ez helyzet nem hello. Adatátviteli könyvtár hello biztosít hello képességét tootransfer egy könyvtárat, fájlok és alkönyvtárak. Adjunk néhány kódot, amely lehetővé teszi toodo éppen ez.

Először adja hozzá a hello metódus `GetBlobDirectory` túl`Program.cs`:

```csharp
public static CloudBlobDirectory GetBlobDirectory(CloudStorageAccount account)
{
    CloudBlobClient blobClient = account.CreateCloudBlobClient();

    Console.WriteLine("\nProvide name of Blob container. This can be a new or existing Blob container:");
    string containerName = Console.ReadLine();
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);
    container.CreateIfNotExistsAsync().Wait();

    CloudBlobDirectory blobDirectory = container.GetDirectoryReference("");

    return blobDirectory;
}
```

Ezután módosítsa `TransferLocalDirectoryToAzureBlobDirectory`:

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{ 
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account); 
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    UploadDirectoryOptions options = new UploadDirectoryOptions()
    {
        Recursive = true
    };

    try
    {
        task = TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetDirectoryTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.UploadDirectoryAsync(localDirectoryPath, blobDirectory, options, context);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Néhány dologban között és a hello módszer egyetlen fájl feltöltése. Most használunk `TransferManager.UploadDirectoryAsync` és hello `getDirectoryTransferContext` korábban létrehozott metódust. Emellett mostantól elérhető egy `options` tooour feltöltési művelet, amely lehetővé teszi tooinclude alkönyvtárak szeretnénk az feltölt tooindicate érték. 

## <a name="copy-file-from-url-tooazure-blob"></a>Másolja az URL-cím tooAzure Blob fájlt
Most adjuk hozzá a kódot, amely lehetővé teszi egy fájl futtatását egy Azure Blob URL-cím tooan toocopy. 

Módosítsa `TransferUrlToAzureBlob`:

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(uri, blob, true, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

A szolgáltatás egy fontos használati eset akkor, ha egy másik felhőalapú szolgáltatást (pl. AWS) tooAzure toomove adatait van szüksége. Mindaddig, amíg egy URL-címet, amely rendelkezik toohello erőforráshoz próbál hozzáférni, egyszerűen áthelyezheti a erőforrást az Azure BLOB hello segítségével `TransferManager.CopyAsync` metódust. Ez a módszer is bevezet egy új logikai paraméter. A paraméter túl`true` azt jelzi, hogy szeretnénk toodo egy aszinkron kiszolgálóoldali másolása. A paraméter túl`false` jelzi a szinkron másolatot – azaz hello erőforrás letöltött tooour helyi számítógép először, majd tooAzure Blob feltöltése. Azonban szinkron másolatot érhető el jelenleg csak egy Azure Storage erőforrás tooanother másolását. 

## <a name="transfer-azure-blob-tooazure-blob"></a>Azure Blob tooAzure Blob átvitele
Egy másik szolgáltatás egyedileg hello adatátviteli könyvtár által biztosított hello képességét toocopy egy Azure Storage erőforrás tooanother a. 

Módosítsa `TransferAzureBlobToAzureBlob`:

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' tootemporarily cancel your transfer...\n");

    Stopwatch stopWatch = Stopwatch.StartNew();
    Task task;
    ConsoleKeyInfo keyinfo;
    try
    {
        task = TransferManager.CopyAsync(sourceBlob, destinationBlob, true, null, context, cancellationSource.Token);
        while(!task.IsCompleted)
        {
            if(Console.KeyAvailable)
            {
                keyinfo = Console.ReadKey(true);
                if(keyinfo.Key == ConsoleKey.C)
                {
                    cancellationSource.Cancel();
                }
            }
        }
        await task;
    }
    catch(Exception e)
    {
        Console.WriteLine("\nThe transfer is canceled: {0}", e.Message);  
    }

    if(cancellationSource.IsCancellationRequested)
    {
        Console.WriteLine("\nTransfer will resume in 3 seconds...");
        Thread.Sleep(3000);
        checkpoint = context.LastCheckpoint;
        context = GetSingleTransferContext(checkpoint);
        Console.WriteLine("\nResuming transfer...\n");
        await TransferManager.CopyAsync(sourceBlob, destinationBlob, false, null, context, cancellationSource.Token);
    }

    stopWatch.Stop();
    Console.WriteLine("\nTransfer operation completed in " + stopWatch.Elapsed.TotalSeconds + " seconds.");
    ExecuteChoice(account);
}
```

Ebben a példában hivatott hello logikai paraméter `TransferManager.CopyAsync` túl`false` tooindicate, hogy szeretnénk toodo szinkron másolatot. Ez azt jelenti, hogy hello erőforrás letöltött tooour helyi számítógép először, majd tooAzure Blob feltöltése. hello szinkron másolatot lehetőség egy kiváló módja tooensure, hogy rendelkezik-e a másolási művelet egységes sebesség esetén. Ezzel szemben egy aszinkron kiszolgálóoldali másolatot hello sebességétől függ hello rendelkezésre álló hálózati sávszélességet hello kiszolgálón, amely képes ingadozik. Azonban a szinkron másolatot hozhat létre további kilépő képest költség tooasynchronous másolása. hello ajánlott megközelítés toouse szinkron másolatot egy Azure virtuális gép, amely hello és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.

## <a name="conclusion"></a>Összegzés
Az adatok mozgása alkalmazás most már befejeződött. [hello teljes kódminta érhető el a Githubon](https://github.com/azure-samples/storage-dotnet-data-movement-library-app). 

## <a name="next-steps"></a>Következő lépések
Az első lépések, létrehozott egy alkalmazás, amely együttműködik az Azure Storage és a Windows, Linux és macOS futtatja. Ez az első lépések a Blob Storage összpontosít. Azonban ez azonos Tudásbázis nem alkalmazott tooFile tároló. toolearn több, tekintse meg [Azure Storage adatátviteli könyvtár referenciadokumentációt](https://azure.github.io/azure-storage-net-data-movement).

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]




