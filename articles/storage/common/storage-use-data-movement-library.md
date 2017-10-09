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
ms.openlocfilehash: 9aec6cb171f794cc6ca432938ce499079e7dfdec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-microsoft-azure-storage-data-movement-library"></a><span data-ttu-id="21636-105">A Microsoft Azure Storage adatátviteli könyvtár hello adatátvitelt</span><span class="sxs-lookup"><span data-stu-id="21636-105">Transfer Data with hello Microsoft Azure Storage Data Movement Library</span></span>

## <a name="overview"></a><span data-ttu-id="21636-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="21636-106">Overview</span></span>
<span data-ttu-id="21636-107">a Microsoft Azure Storage adatátviteli könyvtár hello a platformok közötti nyílt forráskódú-szalagtár egyik célja a nagy teljesítményű feltöltését, letöltése és az Azure Storage Blobs és a fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="21636-107">hello Microsoft Azure Storage Data Movement Library is a cross-platform open source library that is designed for high performance uploading, downloading, and copying of Azure Storage Blobs and Files.</span></span> <span data-ttu-id="21636-108">Ebben a könyvtárban hello alapvető adatátviteli keretrendszeren, amely rendszert működtet [AzCopy](../storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="21636-108">This library is hello core data movement framework that powers [AzCopy](../storage-use-azcopy.md).</span></span> <span data-ttu-id="21636-109">Adatátviteli könyvtár hello biztosít, amelyek nem érhetők el a hagyományos a kényelmes módszert [.NET Azure Storage ügyféloldali kódtár](../blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="21636-109">hello Data Movement Library provides convenient methods that aren't available in our traditional [.NET Azure Storage Client Library](../blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="21636-110">Ez magában foglalja a hello képességét tooset hello párhuzamos műveletek számát, nyomon követése átvitel állapota, egyszerűen folytathatja a megszakított átvitel, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="21636-110">This includes hello ability tooset hello number of parallel operations, track transfer progress, easily resume a canceled transfer, and much more.</span></span>  

<span data-ttu-id="21636-111">Ebben a könyvtárban is használja a .NET Core, ami azt jelenti, hogy a .NET-alkalmazások a Windows, Linux és macOS fejlesztéskor használhatja.</span><span class="sxs-lookup"><span data-stu-id="21636-111">This library also uses .NET Core, which means you can use it when building .NET apps for Windows, Linux and macOS.</span></span> <span data-ttu-id="21636-112">toolearn .NET Core kapcsolatos további információkért tekintse meg a toohello [.NET Core dokumentációja](https://dotnet.github.io/).</span><span class="sxs-lookup"><span data-stu-id="21636-112">toolearn more about .NET Core, refer toohello [.NET Core documentation](https://dotnet.github.io/).</span></span> <span data-ttu-id="21636-113">Ebben a könyvtárban is működik, a hagyományos .NET-keretrendszer alkalmazások Windows.</span><span class="sxs-lookup"><span data-stu-id="21636-113">This library also works for traditional .NET Framework apps for Windows.</span></span> 

<span data-ttu-id="21636-114">Ez a dokumentum bemutatja, hogyan toocreate a .NET Core Konzolalkalmazás, hogy, hogy fut a Windows, Linux és macOS és hajtja végre a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="21636-114">This document demonstrates how toocreate a .NET Core console application that that runs on Windows, Linux, and macOS and performs hello following scenarios:</span></span>

- <span data-ttu-id="21636-115">Fájlok és könyvtárak tooBlob tárolási feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21636-115">Upload files and directories tooBlob Storage.</span></span>
- <span data-ttu-id="21636-116">Adja meg a hello száma párhuzamos művelet adatátvitel során.</span><span class="sxs-lookup"><span data-stu-id="21636-116">Define hello number of parallel operations when transferring data.</span></span>
- <span data-ttu-id="21636-117">Track adatok átvitel folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="21636-117">Track data transfer progress.</span></span>
- <span data-ttu-id="21636-118">Végezze el újra a megszakított adatátvitel.</span><span class="sxs-lookup"><span data-stu-id="21636-118">Resume canceled data transfer.</span></span> 
- <span data-ttu-id="21636-119">Fájl másolása URL-cím tooBlob tároló.</span><span class="sxs-lookup"><span data-stu-id="21636-119">Copy file from URL tooBlob Storage.</span></span> 
- <span data-ttu-id="21636-120">A Blob Storage tooBlob tárolási másolása.</span><span class="sxs-lookup"><span data-stu-id="21636-120">Copy from Blob Storage tooBlob Storage.</span></span>

<span data-ttu-id="21636-121">**Mire van szüksége:**</span><span class="sxs-lookup"><span data-stu-id="21636-121">**What you need:**</span></span>

* [<span data-ttu-id="21636-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21636-122">Visual Studio Code</span></span>](https://code.visualstudio.com/)
* <span data-ttu-id="21636-123">Egy [Azure-tárfiók](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="21636-123">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

> [!NOTE]
> <span data-ttu-id="21636-124">Ez az útmutató feltételezi, hogy Ön már ismeri a [Azure Storage](https://azure.microsoft.com/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="21636-124">This guide assumes that you are already familiar with [Azure Storage](https://azure.microsoft.com/services/storage/).</span></span> <span data-ttu-id="21636-125">Ha nem, olvasási hello [bemutatása tooAzure tárolási](storage-introduction.md) dokumentáció is hasznos.</span><span class="sxs-lookup"><span data-stu-id="21636-125">If not, reading hello [Introduction tooAzure Storage](storage-introduction.md) documentation is helpful.</span></span> <span data-ttu-id="21636-126">A legfontosabb, hogy túl kell[hozzon létre egy tárfiókot](storage-create-storage-account.md#create-a-storage-account) toostart használatával hello adatátviteli könyvtár.</span><span class="sxs-lookup"><span data-stu-id="21636-126">Most importantly, you need too[create a Storage account](storage-create-storage-account.md#create-a-storage-account) toostart using hello Data Movement Library.</span></span>
> 
> 

## <a name="setup"></a><span data-ttu-id="21636-127">Beállítás</span><span class="sxs-lookup"><span data-stu-id="21636-127">Setup</span></span>  

1. <span data-ttu-id="21636-128">A Microsoft hello [.NET Core telepítési útmutató](https://www.microsoft.com/net/core) tooinstall .NET Core.</span><span class="sxs-lookup"><span data-stu-id="21636-128">Visit hello [.NET Core Installation Guide](https://www.microsoft.com/net/core) tooinstall .NET Core.</span></span> <span data-ttu-id="21636-129">A környezet kiválasztásakor válassza hello parancssori kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="21636-129">When selecting your environment, choose hello command-line option.</span></span> 
2. <span data-ttu-id="21636-130">Hello parancssorból hozzon létre egy könyvtárat a projekthez.</span><span class="sxs-lookup"><span data-stu-id="21636-130">From hello command line, create a directory for your project.</span></span> <span data-ttu-id="21636-131">Keresse meg az ebben a könyvtárban, majd írja be a `dotnet new` toocreate egy C# konzolos projektet.</span><span class="sxs-lookup"><span data-stu-id="21636-131">Navigate into this directory, then type `dotnet new` toocreate a C# console project.</span></span>
3. <span data-ttu-id="21636-132">Nyissa meg a könyvtár a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="21636-132">Open this directory in Visual Studio Code.</span></span> <span data-ttu-id="21636-133">Ez a lépés gyorsan megteheti a hello parancssori beírásával `code .`.</span><span class="sxs-lookup"><span data-stu-id="21636-133">This step can be quickly done via hello command line by typing `code .`.</span></span>  
4. <span data-ttu-id="21636-134">Telepítse a hello [C# bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) a Visual Studio Code piactér hello.</span><span class="sxs-lookup"><span data-stu-id="21636-134">Install hello [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) from hello Visual Studio Code Marketplace.</span></span> <span data-ttu-id="21636-135">Indítsa újra a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="21636-135">Restart Visual Studio Code.</span></span> 
5. <span data-ttu-id="21636-136">Ekkor meg kell jelennie két megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="21636-136">At this point, you should see two prompts.</span></span> <span data-ttu-id="21636-137">Egyet az hozzáadása "toobuild szükséges eszközök és hibakereső."</span><span class="sxs-lookup"><span data-stu-id="21636-137">One is for adding "required assets toobuild and debug."</span></span> <span data-ttu-id="21636-138">Kattintson az "Igen" gombra.</span><span class="sxs-lookup"><span data-stu-id="21636-138">Click "yes."</span></span> <span data-ttu-id="21636-139">Egy másik kérdés feloldatlan függőségek visszaállítására van.</span><span class="sxs-lookup"><span data-stu-id="21636-139">Another prompt is for restoring unresolved dependencies.</span></span> <span data-ttu-id="21636-140">Kattintson a "visszaállítás".</span><span class="sxs-lookup"><span data-stu-id="21636-140">Click "restore."</span></span>
6. <span data-ttu-id="21636-141">Az alkalmazás most tartalmaznia kell egy `launch.json` hello fájlt `.vscode` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="21636-141">Your application should now contain a `launch.json` file under hello `.vscode` directory.</span></span> <span data-ttu-id="21636-142">Ebben a fájlban, módosítsa a hello `externalConsole` érték túl`true`.</span><span class="sxs-lookup"><span data-stu-id="21636-142">In this file, change hello `externalConsole` value too`true`.</span></span>
7. <span data-ttu-id="21636-143">A Visual Studio Code toodebug .NET Core alkalmazások lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="21636-143">Visual Studio Code allows you toodebug .NET Core applications.</span></span> <span data-ttu-id="21636-144">Találati `F5` toorun az alkalmazást, és ellenőrizze, hogy működik-e a beállításai.</span><span class="sxs-lookup"><span data-stu-id="21636-144">Hit `F5` toorun your application and verify that your setup is working.</span></span> <span data-ttu-id="21636-145">Láthatja a "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="21636-145">You should see "Hello World!"</span></span> <span data-ttu-id="21636-146">nyomtatott toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="21636-146">printed toohello console.</span></span> 

## <a name="add-data-movement-library-tooyour-project"></a><span data-ttu-id="21636-147">Adatátviteli könyvtár tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21636-147">Add Data Movement Library tooyour project</span></span>

1. <span data-ttu-id="21636-148">Adja hozzá a hello hello adatátviteli könyvtár toohello legújabb verziójának `dependencies` szakasza a `project.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="21636-148">Add hello latest version of hello Data Movement Library toohello `dependencies` section of your `project.json` file.</span></span> <span data-ttu-id="21636-149">Hello írásának időpontjában ebben a verzióban lenne`"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span><span class="sxs-lookup"><span data-stu-id="21636-149">At hello time of writing, this version would be `"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span></span> 
2. <span data-ttu-id="21636-150">Adja hozzá `"portable-net45+win8"` toohello `imports` szakasz.</span><span class="sxs-lookup"><span data-stu-id="21636-150">Add `"portable-net45+win8"` toohello `imports` section.</span></span> 
3. <span data-ttu-id="21636-151">A kérdés megjelenjen-e toorestore a projekthez.</span><span class="sxs-lookup"><span data-stu-id="21636-151">A prompt should display toorestore your project.</span></span> <span data-ttu-id="21636-152">Kattintson a hello "visszaállítás" gombra.</span><span class="sxs-lookup"><span data-stu-id="21636-152">Click hello "restore" button.</span></span> <span data-ttu-id="21636-153">Visszaállíthatja a projekt hello parancssorból hello parancs beírásával `dotnet restore` a projektkönyvtárban hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="21636-153">You can also restore your project from hello command line by typing hello command `dotnet restore` in hello root of your project directory.</span></span>

<span data-ttu-id="21636-154">Módosítsa `project.json`:</span><span class="sxs-lookup"><span data-stu-id="21636-154">Modify `project.json`:</span></span>

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

## <a name="set-up-hello-skeleton-of-your-application"></a><span data-ttu-id="21636-155">Az alkalmazás hello vázat beállítása</span><span class="sxs-lookup"><span data-stu-id="21636-155">Set up hello skeleton of your application</span></span>
<span data-ttu-id="21636-156">hello végezzük elsőként be van állítva hello "skeleton" az alkalmazás kódját.</span><span class="sxs-lookup"><span data-stu-id="21636-156">hello first thing we do is set up hello "skeleton" code of our application.</span></span> <span data-ttu-id="21636-157">Ez a kód megadását kéri, a Tárfiók nevét és a fiók kulcsának, és használja ezeket a hitelesítő adatok toocreate egy `CloudStorageAccount` objektum.</span><span class="sxs-lookup"><span data-stu-id="21636-157">This code prompts us for a Storage account name and account key and uses those credentials toocreate a `CloudStorageAccount` object.</span></span> <span data-ttu-id="21636-158">Ez az objektum a Storage-fiók összes átviteli forgatókönyvekben használt toointeract.</span><span class="sxs-lookup"><span data-stu-id="21636-158">This object is used toointeract with our Storage account in all transfer scenarios.</span></span> <span data-ttu-id="21636-159">hello kódot is kér, toochoose hello típusú szeretnénk adatátviteli művelet tooexecute.</span><span class="sxs-lookup"><span data-stu-id="21636-159">hello code also prompts us toochoose hello type of transfer operation we would like tooexecute.</span></span> 

<span data-ttu-id="21636-160">Módosítsa `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="21636-160">Modify `Program.cs`:</span></span>

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

## <a name="transfer-local-file-tooazure-blob"></a><span data-ttu-id="21636-161">Helyi fájl tooAzure Blob átvitele</span><span class="sxs-lookup"><span data-stu-id="21636-161">Transfer local file tooAzure Blob</span></span>
<span data-ttu-id="21636-162">Adjon hozzá hello módszert `GetSourcePath` és `GetBlob` túl`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="21636-162">Add hello methods `GetSourcePath` and `GetBlob` too`Program.cs`:</span></span>

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

<span data-ttu-id="21636-163">Módosítsa a hello `TransferLocalFileToAzureBlob` módszert:</span><span class="sxs-lookup"><span data-stu-id="21636-163">Modify hello `TransferLocalFileToAzureBlob` method:</span></span>

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

<span data-ttu-id="21636-164">Ez a kód megadását kéri az velünk hello elérési tooa helyi fájl, egy új vagy meglévő tároló hello nevét és egy új blob hello nevét.</span><span class="sxs-lookup"><span data-stu-id="21636-164">This code prompts us for hello path tooa local file, hello name of a new or existing container, and hello name of a new blob.</span></span> <span data-ttu-id="21636-165">Hello `TransferManager.UploadAsync` metódus hello feltöltés ezen információk alapján hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="21636-165">hello `TransferManager.UploadAsync` method performs hello upload using this information.</span></span> 

<span data-ttu-id="21636-166">Találati `F5` toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21636-166">Hit `F5` toorun your application.</span></span> <span data-ttu-id="21636-167">Adott hello feltöltés történt a tárfiók hello megtekintésével ellenőrizheti [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="21636-167">You can verify that hello upload occurred by viewing your Storage account with hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="set-number-of-parallel-operations"></a><span data-ttu-id="21636-168">Készlet száma párhuzamos művelet</span><span class="sxs-lookup"><span data-stu-id="21636-168">Set number of parallel operations</span></span>
<span data-ttu-id="21636-169">Hello által kínált kiváló szolgáltatás adatátviteli könyvtár hello képességét tooset hello száma párhuzamos művelet átviteli tooincrease hello adatátvitelt.</span><span class="sxs-lookup"><span data-stu-id="21636-169">A great feature offered by hello Data Movement Library is hello ability tooset hello number of parallel operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="21636-170">Alapértelmezés szerint hello adatátviteli könyvtár beállítja hello száma párhuzamos művelet too8 * hello a számítógépen lévő magok száma.</span><span class="sxs-lookup"><span data-stu-id="21636-170">By default, hello Data Movement Library sets hello number of parallel operations too8 * hello number of cores on your machine.</span></span> 

<span data-ttu-id="21636-171">Ne feledje, hogy sok párhuzamos művelet egy kis sávszélességű környezetben ne terhelje tovább hello hálózati kapcsolatot, és ténylegesen megakadályozza a műveletek teljes befejezését.</span><span class="sxs-lookup"><span data-stu-id="21636-171">Keep in mind that many parallel operations in a low-bandwidth environment may overwhelm hello network connection and actually prevent operations from fully completing.</span></span> <span data-ttu-id="21636-172">Lesz szükség az ezt a beállítást toodetermine tooexperiment mi működik legjobban alapján a rendelkezésre álló hálózati sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="21636-172">You'll need tooexperiment with this setting toodetermine what works best based on your available network bandwidth.</span></span> 

<span data-ttu-id="21636-173">Adjunk néhány kódot, amely lehetővé teszi tooset hello száma párhuzamos művelet.</span><span class="sxs-lookup"><span data-stu-id="21636-173">Let's add some code that allows us tooset hello number of parallel operations.</span></span> <span data-ttu-id="21636-174">Adjuk hozzá is, hogy mennyi ideig tart a hello átviteli toocomplete alkalommal kódot.</span><span class="sxs-lookup"><span data-stu-id="21636-174">Let's also add code that times how long it takes for hello transfer toocomplete.</span></span>

<span data-ttu-id="21636-175">Adja hozzá a `SetNumberOfParallelOperations` metódus túl`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="21636-175">Add a `SetNumberOfParallelOperations` method too`Program.cs`:</span></span>

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like toouse?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

<span data-ttu-id="21636-176">Módosítsa a hello `ExecuteChoice` metódus toouse `SetNumberOfParallelOperations`:</span><span class="sxs-lookup"><span data-stu-id="21636-176">Modify hello `ExecuteChoice` method toouse `SetNumberOfParallelOperations`:</span></span>

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

<span data-ttu-id="21636-177">Módosítsa a hello `TransferLocalFileToAzureBlob` metódus toouse időzítő:</span><span class="sxs-lookup"><span data-stu-id="21636-177">Modify hello `TransferLocalFileToAzureBlob` method toouse a timer:</span></span>

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

## <a name="track-transfer-progress"></a><span data-ttu-id="21636-178">Track átvitel állapota</span><span class="sxs-lookup"><span data-stu-id="21636-178">Track transfer progress</span></span>
<span data-ttu-id="21636-179">Annak ismerete, hogy mennyi ideig tartott a saját adatok tootransfer nagy.</span><span class="sxs-lookup"><span data-stu-id="21636-179">Knowing how long it took for our data tootransfer is great.</span></span> <span data-ttu-id="21636-180">Azonban a képes toosee hello folyamatban van az átvitel alatt *során* hello adatátviteli művelet még élvezetesebbé lenne.</span><span class="sxs-lookup"><span data-stu-id="21636-180">However, being able toosee hello progress of our transfer *during* hello transfer operation would be even better.</span></span> <span data-ttu-id="21636-181">tooachieve ebben az esetben toocreate kell egy `TransferContext` objektum.</span><span class="sxs-lookup"><span data-stu-id="21636-181">tooachieve this scenario, we need toocreate a `TransferContext` object.</span></span> <span data-ttu-id="21636-182">Hello `TransferContext` objektum két formában származik: `SingleTransferContext` és `DirectoryTransferContext`.</span><span class="sxs-lookup"><span data-stu-id="21636-182">hello `TransferContext` object comes in two forms: `SingleTransferContext` and `DirectoryTransferContext`.</span></span> <span data-ttu-id="21636-183">korábbi hello (amely azt még tevékenységeit most) egyetlen fájl átvitelére, ez utóbbi hello pedig egy könyvtárat (amely azt később felvenni) fájlok átvitele.</span><span class="sxs-lookup"><span data-stu-id="21636-183">hello former is for transferring a single file (which is what we're doing now) and hello latter is for transferring a directory of files (which we are adding later).</span></span>

<span data-ttu-id="21636-184">Adjon hozzá hello módszert `GetSingleTransferContext` és `GetDirectoryTransferContext` túl`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="21636-184">Add hello methods `GetSingleTransferContext` and `GetDirectoryTransferContext` too`Program.cs`:</span></span> 

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

<span data-ttu-id="21636-185">Módosítsa a hello `TransferLocalFileToAzureBlob` metódus toouse `GetSingleTransferContext`:</span><span class="sxs-lookup"><span data-stu-id="21636-185">Modify hello `TransferLocalFileToAzureBlob` method toouse `GetSingleTransferContext`:</span></span>

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

## <a name="resume-a-canceled-transfer"></a><span data-ttu-id="21636-186">A megszakított átvitel folytatása</span><span class="sxs-lookup"><span data-stu-id="21636-186">Resume a canceled transfer</span></span>
<span data-ttu-id="21636-187">Egy másik kényelmes hello adatátviteli könyvtár által kínált szolgáltatása hello képességét tooresume visszavont átvitelt.</span><span class="sxs-lookup"><span data-stu-id="21636-187">Another convenient feature offered by hello Data Movement Library is hello ability tooresume a canceled transfer.</span></span> <span data-ttu-id="21636-188">Adjunk néhány kódot, amely lehetővé teszi tootemporarily Mégse hello átviteli írja be a `c`, és ezután úgy folytatódik, hello átviteli 3 másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="21636-188">Let's add some code that allows us tootemporarily cancel hello transfer by typing `c`, and then resume hello transfer 3 seconds later.</span></span>

<span data-ttu-id="21636-189">Módosítsa `TransferLocalFileToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="21636-189">Modify `TransferLocalFileToAzureBlob`:</span></span>

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

<span data-ttu-id="21636-190">Eddig a `checkpoint` mindig beállítás túl`null`.</span><span class="sxs-lookup"><span data-stu-id="21636-190">Up until now, our `checkpoint` value has always been set too`null`.</span></span> <span data-ttu-id="21636-191">Most hello átviteli megszakítása, ha azt hello utolsó ellenőrzési pont a átvitel beolvasni, akkor használja ezt az új ellenőrzőpontot az átvitel a környezetben.</span><span class="sxs-lookup"><span data-stu-id="21636-191">Now, if we cancel hello transfer, we retrieve hello last checkpoint of our transfer, then use this new checkpoint in our transfer context.</span></span> 

## <a name="transfer-local-directory-tooazure-blob-directory"></a><span data-ttu-id="21636-192">Helyi könyvtár tooAzure Blob directory átvitele</span><span class="sxs-lookup"><span data-stu-id="21636-192">Transfer local directory tooAzure Blob directory</span></span>
<span data-ttu-id="21636-193">Címkézést lenne, ha hello adatátviteli könyvtár csak továbbíthat egy fájlban egyszerre.</span><span class="sxs-lookup"><span data-stu-id="21636-193">It would be disappointing if hello Data Movement Library could only transfer one file at a time.</span></span> <span data-ttu-id="21636-194">Szerencsére ez helyzet nem hello.</span><span class="sxs-lookup"><span data-stu-id="21636-194">Luckily, this is not hello case.</span></span> <span data-ttu-id="21636-195">Adatátviteli könyvtár hello biztosít hello képességét tootransfer egy könyvtárat, fájlok és alkönyvtárak.</span><span class="sxs-lookup"><span data-stu-id="21636-195">hello Data Movement Library provides hello ability tootransfer a directory of files and all of its subdirectories.</span></span> <span data-ttu-id="21636-196">Adjunk néhány kódot, amely lehetővé teszi toodo éppen ez.</span><span class="sxs-lookup"><span data-stu-id="21636-196">Let's add some code that allows us toodo just that.</span></span>

<span data-ttu-id="21636-197">Először adja hozzá a hello metódus `GetBlobDirectory` túl`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="21636-197">First, add hello method `GetBlobDirectory` too`Program.cs`:</span></span>

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

<span data-ttu-id="21636-198">Ezután módosítsa `TransferLocalDirectoryToAzureBlobDirectory`:</span><span class="sxs-lookup"><span data-stu-id="21636-198">Then, modify `TransferLocalDirectoryToAzureBlobDirectory`:</span></span>

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

<span data-ttu-id="21636-199">Néhány dologban között és a hello módszer egyetlen fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21636-199">There are a few differences between this method and hello method for uploading a single file.</span></span> <span data-ttu-id="21636-200">Most használunk `TransferManager.UploadDirectoryAsync` és hello `getDirectoryTransferContext` korábban létrehozott metódust.</span><span class="sxs-lookup"><span data-stu-id="21636-200">We're now using `TransferManager.UploadDirectoryAsync` and hello `getDirectoryTransferContext` method we created earlier.</span></span> <span data-ttu-id="21636-201">Emellett mostantól elérhető egy `options` tooour feltöltési művelet, amely lehetővé teszi tooinclude alkönyvtárak szeretnénk az feltölt tooindicate érték.</span><span class="sxs-lookup"><span data-stu-id="21636-201">In addition, we now provide an `options` value tooour upload operation, which allows us tooindicate that we want tooinclude subdirectories in our upload.</span></span> 

## <a name="copy-file-from-url-tooazure-blob"></a><span data-ttu-id="21636-202">Másolja az URL-cím tooAzure Blob fájlt</span><span class="sxs-lookup"><span data-stu-id="21636-202">Copy file from URL tooAzure Blob</span></span>
<span data-ttu-id="21636-203">Most adjuk hozzá a kódot, amely lehetővé teszi egy fájl futtatását egy Azure Blob URL-cím tooan toocopy.</span><span class="sxs-lookup"><span data-stu-id="21636-203">Now, let's add code that allows us toocopy a file from a URL tooan Azure Blob.</span></span> 

<span data-ttu-id="21636-204">Módosítsa `TransferUrlToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="21636-204">Modify `TransferUrlToAzureBlob`:</span></span>

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

<span data-ttu-id="21636-205">A szolgáltatás egy fontos használati eset akkor, ha egy másik felhőalapú szolgáltatást (pl. AWS) tooAzure toomove adatait van szüksége.</span><span class="sxs-lookup"><span data-stu-id="21636-205">One important use case for this feature is when you need toomove data from another cloud service (e.g. AWS) tooAzure.</span></span> <span data-ttu-id="21636-206">Mindaddig, amíg egy URL-címet, amely rendelkezik toohello erőforráshoz próbál hozzáférni, egyszerűen áthelyezheti a erőforrást az Azure BLOB hello segítségével `TransferManager.CopyAsync` metódust.</span><span class="sxs-lookup"><span data-stu-id="21636-206">As long as you have a URL that gives you access toohello resource, you can easily move that resource into Azure Blobs by using hello `TransferManager.CopyAsync` method.</span></span> <span data-ttu-id="21636-207">Ez a módszer is bevezet egy új logikai paraméter.</span><span class="sxs-lookup"><span data-stu-id="21636-207">This method also introduces a new boolean parameter.</span></span> <span data-ttu-id="21636-208">A paraméter túl`true` azt jelzi, hogy szeretnénk toodo egy aszinkron kiszolgálóoldali másolása.</span><span class="sxs-lookup"><span data-stu-id="21636-208">Setting this parameter too`true` indicates that we want toodo an asynchronous server-side copy.</span></span> <span data-ttu-id="21636-209">A paraméter túl`false` jelzi a szinkron másolatot – azaz hello erőforrás letöltött tooour helyi számítógép először, majd tooAzure Blob feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21636-209">Setting this parameter too`false` indicates a synchronous copy - meaning hello resource is downloaded tooour local machine first, then uploaded tooAzure Blob.</span></span> <span data-ttu-id="21636-210">Azonban szinkron másolatot érhető el jelenleg csak egy Azure Storage erőforrás tooanother másolását.</span><span class="sxs-lookup"><span data-stu-id="21636-210">However, synchronous copy is currently only available for copying from one Azure Storage resource tooanother.</span></span> 

## <a name="transfer-azure-blob-tooazure-blob"></a><span data-ttu-id="21636-211">Azure Blob tooAzure Blob átvitele</span><span class="sxs-lookup"><span data-stu-id="21636-211">Transfer Azure Blob tooAzure Blob</span></span>
<span data-ttu-id="21636-212">Egy másik szolgáltatás egyedileg hello adatátviteli könyvtár által biztosított hello képességét toocopy egy Azure Storage erőforrás tooanother a.</span><span class="sxs-lookup"><span data-stu-id="21636-212">Another feature that's uniquely provided by hello Data Movement Library is hello ability toocopy from one Azure Storage resource tooanother.</span></span> 

<span data-ttu-id="21636-213">Módosítsa `TransferAzureBlobToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="21636-213">Modify `TransferAzureBlobToAzureBlob`:</span></span>

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

<span data-ttu-id="21636-214">Ebben a példában hivatott hello logikai paraméter `TransferManager.CopyAsync` túl`false` tooindicate, hogy szeretnénk toodo szinkron másolatot.</span><span class="sxs-lookup"><span data-stu-id="21636-214">In this example, we set hello boolean parameter in `TransferManager.CopyAsync` too`false` tooindicate that we want toodo a synchronous copy.</span></span> <span data-ttu-id="21636-215">Ez azt jelenti, hogy hello erőforrás letöltött tooour helyi számítógép először, majd tooAzure Blob feltöltése.</span><span class="sxs-lookup"><span data-stu-id="21636-215">This means that hello resource is downloaded tooour local machine first, then uploaded tooAzure Blob.</span></span> <span data-ttu-id="21636-216">hello szinkron másolatot lehetőség egy kiváló módja tooensure, hogy rendelkezik-e a másolási művelet egységes sebesség esetén.</span><span class="sxs-lookup"><span data-stu-id="21636-216">hello synchronous copy option is a great way tooensure that your copy operation has a consistent speed.</span></span> <span data-ttu-id="21636-217">Ezzel szemben egy aszinkron kiszolgálóoldali másolatot hello sebességétől függ hello rendelkezésre álló hálózati sávszélességet hello kiszolgálón, amely képes ingadozik.</span><span class="sxs-lookup"><span data-stu-id="21636-217">In contrast, hello speed of an asynchronous server-side copy is dependent on hello available network bandwidth on hello server, which can fluctuate.</span></span> <span data-ttu-id="21636-218">Azonban a szinkron másolatot hozhat létre további kilépő képest költség tooasynchronous másolása.</span><span class="sxs-lookup"><span data-stu-id="21636-218">However, synchronous copy may generate additional egress cost compared tooasynchronous copy.</span></span> <span data-ttu-id="21636-219">hello ajánlott megközelítés toouse szinkron másolatot egy Azure virtuális gép, amely hello és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="21636-219">hello recommended approach is toouse synchronous copy in an Azure VM that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="conclusion"></a><span data-ttu-id="21636-220">Összegzés</span><span class="sxs-lookup"><span data-stu-id="21636-220">Conclusion</span></span>
<span data-ttu-id="21636-221">Az adatok mozgása alkalmazás most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="21636-221">Our data movement application is now complete.</span></span> <span data-ttu-id="21636-222">[hello teljes kódminta érhető el a Githubon](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span><span class="sxs-lookup"><span data-stu-id="21636-222">[hello full code sample is available on GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="21636-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21636-223">Next steps</span></span>
<span data-ttu-id="21636-224">Az első lépések, létrehozott egy alkalmazás, amely együttműködik az Azure Storage és a Windows, Linux és macOS futtatja.</span><span class="sxs-lookup"><span data-stu-id="21636-224">In this getting started, we created an application that interacts with Azure Storage and runs on Windows, Linux, and macOS.</span></span> <span data-ttu-id="21636-225">Ez az első lépések a Blob Storage összpontosít.</span><span class="sxs-lookup"><span data-stu-id="21636-225">This getting started focused on Blob Storage.</span></span> <span data-ttu-id="21636-226">Azonban ez azonos Tudásbázis nem alkalmazott tooFile tároló.</span><span class="sxs-lookup"><span data-stu-id="21636-226">However, this same knowledge can be applied tooFile Storage.</span></span> <span data-ttu-id="21636-227">toolearn több, tekintse meg [Azure Storage adatátviteli könyvtár referenciadokumentációt](https://azure.github.io/azure-storage-net-data-movement).</span><span class="sxs-lookup"><span data-stu-id="21636-227">toolearn more, check out [Azure Storage Data Movement Library reference documentation](https://azure.github.io/azure-storage-net-data-movement).</span></span>

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]




