---
title: "A Microsoft Azure Storage adatátviteli könyvtár az adatok átviteléhez |} Microsoft Docs"
description: "Az adatátviteli könyvtár használatával helyezhet át adatokat, vagy a blob és a fájl tartalmát. Adatok másolása az Azure Storage a helyi fájlokból, vagy másolja az adatokat belül vagy tárfiókok között. Adatok áttelepítése egyszerű Azure Storage."
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
ms.openlocfilehash: 2ba94e4dd931b6d385101c7dadccfa3583b5296e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-the-microsoft-azure-storage-data-movement-library"></a><span data-ttu-id="6d02a-105">A Microsoft Azure Storage adatátviteli könyvtár az adatok átvitele</span><span class="sxs-lookup"><span data-stu-id="6d02a-105">Transfer Data with the Microsoft Azure Storage Data Movement Library</span></span>

## <a name="overview"></a><span data-ttu-id="6d02a-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6d02a-106">Overview</span></span>
<span data-ttu-id="6d02a-107">A Microsoft Azure Storage adatátviteli könyvtár a platformok közötti nyílt forráskódú-szalagtár egyik célja a nagy teljesítményű feltöltését, letöltése és az Azure Storage Blobs és a fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="6d02a-107">The Microsoft Azure Storage Data Movement Library is a cross-platform open source library that is designed for high performance uploading, downloading, and copying of Azure Storage Blobs and Files.</span></span> <span data-ttu-id="6d02a-108">Ebben a könyvtárban, az alapvető adatátviteli keretrendszeren, amely rendszert működtet [AzCopy](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="6d02a-108">This library is the core data movement framework that powers [AzCopy](storage-use-azcopy.md).</span></span> <span data-ttu-id="6d02a-109">Az adatátviteli könyvtár, amelyek nem érhetők el a hagyományos a kényelmes módszert biztosít [.NET Azure Storage ügyféloldali kódtár](storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="6d02a-109">The Data Movement Library provides convenient methods that aren't available in our traditional [.NET Azure Storage Client Library](storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="6d02a-110">Ez lehetővé teszi a párhuzamos műveletek számának beállítása, átviteli nyomon követni, egyszerűen folytathatja a megszakított átvitel, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="6d02a-110">This includes the ability to set the number of parallel operations, track transfer progress, easily resume a canceled transfer, and much more.</span></span>  

<span data-ttu-id="6d02a-111">Ebben a könyvtárban is használja a .NET Core, ami azt jelenti, hogy a .NET-alkalmazások a Windows, Linux és macOS fejlesztéskor használhatja.</span><span class="sxs-lookup"><span data-stu-id="6d02a-111">This library also uses .NET Core, which means you can use it when building .NET apps for Windows, Linux and macOS.</span></span> <span data-ttu-id="6d02a-112">A .NET Core kapcsolatos további tudnivalókért tekintse meg a [.NET Core dokumentációja](https://dotnet.github.io/).</span><span class="sxs-lookup"><span data-stu-id="6d02a-112">To learn more about .NET Core, refer to the [.NET Core documentation](https://dotnet.github.io/).</span></span> <span data-ttu-id="6d02a-113">Ebben a könyvtárban is működik, a hagyományos .NET-keretrendszer alkalmazások Windows.</span><span class="sxs-lookup"><span data-stu-id="6d02a-113">This library also works for traditional .NET Framework apps for Windows.</span></span> 

<span data-ttu-id="6d02a-114">Ez a dokumentum bemutatja, hogyan lehet .NET Core Konzolalkalmazás létrehozása, amely a Windows, Linux és macOS fut, és hajtja végre a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="6d02a-114">This document demonstrates how to create a .NET Core console application that that runs on Windows, Linux, and macOS and performs the following scenarios:</span></span>

- <span data-ttu-id="6d02a-115">Fájlok és könyvtárak feltöltése a Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6d02a-115">Upload files and directories to Blob Storage.</span></span>
- <span data-ttu-id="6d02a-116">Határozza meg a párhuzamos műveletek számát, adatátvitel során.</span><span class="sxs-lookup"><span data-stu-id="6d02a-116">Define the number of parallel operations when transferring data.</span></span>
- <span data-ttu-id="6d02a-117">Track adatok átvitel folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="6d02a-117">Track data transfer progress.</span></span>
- <span data-ttu-id="6d02a-118">Végezze el újra a megszakított adatátvitel.</span><span class="sxs-lookup"><span data-stu-id="6d02a-118">Resume canceled data transfer.</span></span> 
- <span data-ttu-id="6d02a-119">A Blob Storage URL-címről fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="6d02a-119">Copy file from URL to Blob Storage.</span></span> 
- <span data-ttu-id="6d02a-120">A Blob Storage másolása a Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6d02a-120">Copy from Blob Storage to Blob Storage.</span></span>

<span data-ttu-id="6d02a-121">**Mire van szüksége:**</span><span class="sxs-lookup"><span data-stu-id="6d02a-121">**What you need:**</span></span>

* [<span data-ttu-id="6d02a-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d02a-122">Visual Studio Code</span></span>](https://code.visualstudio.com/)
* <span data-ttu-id="6d02a-123">Egy [Azure-tárfiók](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="6d02a-123">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

> [!NOTE]
> <span data-ttu-id="6d02a-124">Ez az útmutató feltételezi, hogy Ön már ismeri a [Azure Storage](https://azure.microsoft.com/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="6d02a-124">This guide assumes that you are already familiar with [Azure Storage](https://azure.microsoft.com/services/storage/).</span></span> <span data-ttu-id="6d02a-125">Ha nem, olvasási a [Azure Storage bemutatása](storage-introduction.md) dokumentáció is hasznos.</span><span class="sxs-lookup"><span data-stu-id="6d02a-125">If not, reading the [Introduction to Azure Storage](storage-introduction.md) documentation is helpful.</span></span> <span data-ttu-id="6d02a-126">A legfontosabb, kell [hozzon létre egy tárfiókot](storage-create-storage-account.md#create-a-storage-account) az adatátviteli könyvtár elindítására.</span><span class="sxs-lookup"><span data-stu-id="6d02a-126">Most importantly, you need to [create a Storage account](storage-create-storage-account.md#create-a-storage-account) to start using the Data Movement Library.</span></span>
> 
> 

## <a name="setup"></a><span data-ttu-id="6d02a-127">Beállítás</span><span class="sxs-lookup"><span data-stu-id="6d02a-127">Setup</span></span>  

1. <span data-ttu-id="6d02a-128">Látogasson el a [.NET Core telepítési útmutató](https://www.microsoft.com/net/core) .NET Core telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6d02a-128">Visit the [.NET Core Installation Guide](https://www.microsoft.com/net/core) to install .NET Core.</span></span> <span data-ttu-id="6d02a-129">A környezet kiválasztásakor válassza a parancssori kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="6d02a-129">When selecting your environment, choose the command-line option.</span></span> 
2. <span data-ttu-id="6d02a-130">A parancssorból hozzon létre egy könyvtárat a projekthez.</span><span class="sxs-lookup"><span data-stu-id="6d02a-130">From the command line, create a directory for your project.</span></span> <span data-ttu-id="6d02a-131">Keresse meg az ebben a könyvtárban, majd írja be a `dotnet new` C# konzol projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6d02a-131">Navigate into this directory, then type `dotnet new` to create a C# console project.</span></span>
3. <span data-ttu-id="6d02a-132">Nyissa meg a könyvtár a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6d02a-132">Open this directory in Visual Studio Code.</span></span> <span data-ttu-id="6d02a-133">Ez a lépés gyorsan megteheti a parancssor beírásával `code .`.</span><span class="sxs-lookup"><span data-stu-id="6d02a-133">This step can be quickly done via the command line by typing `code .`.</span></span>  
4. <span data-ttu-id="6d02a-134">Telepítse a [C# bővítmény](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) a Visual Studio Code piactérről.</span><span class="sxs-lookup"><span data-stu-id="6d02a-134">Install the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) from the Visual Studio Code Marketplace.</span></span> <span data-ttu-id="6d02a-135">Indítsa újra a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6d02a-135">Restart Visual Studio Code.</span></span> 
5. <span data-ttu-id="6d02a-136">Ekkor meg kell jelennie két megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6d02a-136">At this point, you should see two prompts.</span></span> <span data-ttu-id="6d02a-137">Egyet az hozzáadása "szükséges eszközök létrehozása és hibakeresési."</span><span class="sxs-lookup"><span data-stu-id="6d02a-137">One is for adding "required assets to build and debug."</span></span> <span data-ttu-id="6d02a-138">Kattintson az "Igen" gombra.</span><span class="sxs-lookup"><span data-stu-id="6d02a-138">Click "yes."</span></span> <span data-ttu-id="6d02a-139">Egy másik kérdés feloldatlan függőségek visszaállítására van.</span><span class="sxs-lookup"><span data-stu-id="6d02a-139">Another prompt is for restoring unresolved dependencies.</span></span> <span data-ttu-id="6d02a-140">Kattintson a "visszaállítás".</span><span class="sxs-lookup"><span data-stu-id="6d02a-140">Click "restore."</span></span>
6. <span data-ttu-id="6d02a-141">Az alkalmazás most tartalmaznia kell egy `launch.json` a fájlt a `.vscode` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6d02a-141">Your application should now contain a `launch.json` file under the `.vscode` directory.</span></span> <span data-ttu-id="6d02a-142">Ezt a fájlt, módosítsa a `externalConsole` egy érték `true`.</span><span class="sxs-lookup"><span data-stu-id="6d02a-142">In this file, change the `externalConsole` value to `true`.</span></span>
7. <span data-ttu-id="6d02a-143">A Visual Studio Code lehetővé teszi a .NET Core-alkalmazások hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="6d02a-143">Visual Studio Code allows you to debug .NET Core applications.</span></span> <span data-ttu-id="6d02a-144">Találati `F5` futtassa az alkalmazást, és győződjön meg arról, hogy működik-e a beállításai.</span><span class="sxs-lookup"><span data-stu-id="6d02a-144">Hit `F5` to run your application and verify that your setup is working.</span></span> <span data-ttu-id="6d02a-145">Láthatja a "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="6d02a-145">You should see "Hello World!"</span></span> <span data-ttu-id="6d02a-146">kinyomtatott a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="6d02a-146">printed to the console.</span></span> 

## <a name="add-data-movement-library-to-your-project"></a><span data-ttu-id="6d02a-147">Adatátviteli könyvtár hozzáadása a projekthez</span><span class="sxs-lookup"><span data-stu-id="6d02a-147">Add Data Movement Library to your project</span></span>

1. <span data-ttu-id="6d02a-148">Az adatátviteli könyvtár a legújabb verzióját a `dependencies` szakasza a `project.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="6d02a-148">Add the latest version of the Data Movement Library to the `dependencies` section of your `project.json` file.</span></span> <span data-ttu-id="6d02a-149">Ebben a verzióban lenne írásának időpontjában`"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span><span class="sxs-lookup"><span data-stu-id="6d02a-149">At the time of writing, this version would be `"Microsoft.Azure.Storage.DataMovement": "0.5.0"`</span></span> 
2. <span data-ttu-id="6d02a-150">Adja hozzá `"portable-net45+win8"` számára a `imports` szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d02a-150">Add `"portable-net45+win8"` to the `imports` section.</span></span> 
3. <span data-ttu-id="6d02a-151">A kérdés megjelenjen-e a projekt visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="6d02a-151">A prompt should display to restore your project.</span></span> <span data-ttu-id="6d02a-152">A "visszaállítás" gombra.</span><span class="sxs-lookup"><span data-stu-id="6d02a-152">Click the "restore" button.</span></span> <span data-ttu-id="6d02a-153">Visszaállíthatja a projekt a parancssorból a parancs beírásával `dotnet restore` projektkönyvtárban gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="6d02a-153">You can also restore your project from the command line by typing the command `dotnet restore` in the root of your project directory.</span></span>

<span data-ttu-id="6d02a-154">Módosítsa `project.json`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-154">Modify `project.json`:</span></span>

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

## <a name="set-up-the-skeleton-of-your-application"></a><span data-ttu-id="6d02a-155">Az alkalmazás a vázat beállítása</span><span class="sxs-lookup"><span data-stu-id="6d02a-155">Set up the skeleton of your application</span></span>
<span data-ttu-id="6d02a-156">A legfontosabb végezzük az alkalmazás a "skeleton" kód be van állítva.</span><span class="sxs-lookup"><span data-stu-id="6d02a-156">The first thing we do is set up the "skeleton" code of our application.</span></span> <span data-ttu-id="6d02a-157">Ez a kód megadását kéri, a Tárfiók nevét és a fiók kulcsának, és ezeket a hitelesítő adatok használatával hozzon létre egy `CloudStorageAccount` objektum.</span><span class="sxs-lookup"><span data-stu-id="6d02a-157">This code prompts us for a Storage account name and account key and uses those credentials to create a `CloudStorageAccount` object.</span></span> <span data-ttu-id="6d02a-158">Ez az objektum a Storage-fiókot az összes átviteli forgatókönyv együttműködhet szolgál.</span><span class="sxs-lookup"><span data-stu-id="6d02a-158">This object is used to interact with our Storage account in all transfer scenarios.</span></span> <span data-ttu-id="6d02a-159">A kódot is kér, hogy válassza ki a fájlátviteli művelettel kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="6d02a-159">The code also prompts us to choose the type of transfer operation we would like to execute.</span></span> 

<span data-ttu-id="6d02a-160">Módosítsa `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-160">Modify `Program.cs`:</span></span>

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
            Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
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

## <a name="transfer-local-file-to-azure-blob"></a><span data-ttu-id="6d02a-161">Helyi fájl átvitele az Azure-blobba</span><span class="sxs-lookup"><span data-stu-id="6d02a-161">Transfer local file to Azure Blob</span></span>
<span data-ttu-id="6d02a-162">Adja hozzá a metódusokat `GetSourcePath` és `GetBlob` való `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-162">Add the methods `GetSourcePath` and `GetBlob` to `Program.cs`:</span></span>

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

<span data-ttu-id="6d02a-163">Módosítsa a `TransferLocalFileToAzureBlob` módszert:</span><span class="sxs-lookup"><span data-stu-id="6d02a-163">Modify the `TransferLocalFileToAzureBlob` method:</span></span>

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

<span data-ttu-id="6d02a-164">Ez a kód megadását kéri, elérési útját egy helyi fájlba, egy új vagy meglévő tároló neve és egy új blob neve.</span><span class="sxs-lookup"><span data-stu-id="6d02a-164">This code prompts us for the path to a local file, the name of a new or existing container, and the name of a new blob.</span></span> <span data-ttu-id="6d02a-165">A `TransferManager.UploadAsync` metódus hajtja végre a feltöltés ezen információk alapján.</span><span class="sxs-lookup"><span data-stu-id="6d02a-165">The `TransferManager.UploadAsync` method performs the upload using this information.</span></span> 

<span data-ttu-id="6d02a-166">Találati `F5` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="6d02a-166">Hit `F5` to run your application.</span></span> <span data-ttu-id="6d02a-167">Azt, hogy a feltöltés történt a tárfiók megtekintésével ellenőrizheti a [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6d02a-167">You can verify that the upload occurred by viewing your Storage account with the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="set-number-of-parallel-operations"></a><span data-ttu-id="6d02a-168">Készlet száma párhuzamos művelet</span><span class="sxs-lookup"><span data-stu-id="6d02a-168">Set number of parallel operations</span></span>
<span data-ttu-id="6d02a-169">Az adatátviteli könyvtár által kínált kiváló jellemzője meg az adatok átvitel átviteli sebesség növelése párhuzamos műveletek számát.</span><span class="sxs-lookup"><span data-stu-id="6d02a-169">A great feature offered by the Data Movement Library is the ability to set the number of parallel operations to increase the data transfer throughput.</span></span> <span data-ttu-id="6d02a-170">Alapértelmezés szerint az adatátviteli könyvtár beállítja a párhuzamos műveletek számát 8 * a számítógépen lévő magok száma.</span><span class="sxs-lookup"><span data-stu-id="6d02a-170">By default, the Data Movement Library sets the number of parallel operations to 8 * the number of cores on your machine.</span></span> 

<span data-ttu-id="6d02a-171">Ne feledje, hogy sok párhuzamos művelet egy kis sávszélességű környezetben ne terhelje tovább a hálózati kapcsolatot, és ténylegesen megakadályozza a műveletek teljes befejezését.</span><span class="sxs-lookup"><span data-stu-id="6d02a-171">Keep in mind that many parallel operations in a low-bandwidth environment may overwhelm the network connection and actually prevent operations from fully completing.</span></span> <span data-ttu-id="6d02a-172">Ez a beállítás határozza meg, mi működik legjobban alapján a rendelkezésre álló hálózati sávszélességet a kísérletezhet lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6d02a-172">You'll need to experiment with this setting to determine what works best based on your available network bandwidth.</span></span> 

<span data-ttu-id="6d02a-173">Adjunk néhány kódot, amely lehetővé teszi, hogy állítsa be a párhuzamos műveletek számát.</span><span class="sxs-lookup"><span data-stu-id="6d02a-173">Let's add some code that allows us to set the number of parallel operations.</span></span> <span data-ttu-id="6d02a-174">Adjunk is a kódot, amely alkalommal fordult elő az mennyi időt vesz igénybe az áthelyezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="6d02a-174">Let's also add code that times how long it takes for the transfer to complete.</span></span>

<span data-ttu-id="6d02a-175">Adja hozzá a `SetNumberOfParallelOperations` módszert `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-175">Add a `SetNumberOfParallelOperations` method to `Program.cs`:</span></span>

```csharp
public static void SetNumberOfParallelOperations()
{
    Console.WriteLine("\nHow many parallel operations would you like to use?");
    string parallelOperations = Console.ReadLine();
    TransferManager.Configurations.ParallelOperations = int.Parse(parallelOperations);
}
```

<span data-ttu-id="6d02a-176">Módosítsa a `ExecuteChoice` használandó módszer `SetNumberOfParallelOperations`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-176">Modify the `ExecuteChoice` method to use `SetNumberOfParallelOperations`:</span></span>

```csharp
public static void ExecuteChoice(CloudStorageAccount account)
{
    Console.WriteLine("\nWhat type of transfer would you like to execute?\n1. Local file --> Azure Blob\n2. Local directory --> Azure Blob directory\n3. URL (e.g. Amazon S3 file) --> Azure Blob\n4. Azure Blob --> Azure Blob");
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

<span data-ttu-id="6d02a-177">Módosítsa a `TransferLocalFileToAzureBlob` időzítő használandó módszert:</span><span class="sxs-lookup"><span data-stu-id="6d02a-177">Modify the `TransferLocalFileToAzureBlob` method to use a timer:</span></span>

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

## <a name="track-transfer-progress"></a><span data-ttu-id="6d02a-178">Track átvitel állapota</span><span class="sxs-lookup"><span data-stu-id="6d02a-178">Track transfer progress</span></span>
<span data-ttu-id="6d02a-179">Annak ismerete, hogy mennyi ideig tartott az adatok átvitele nagy.</span><span class="sxs-lookup"><span data-stu-id="6d02a-179">Knowing how long it took for our data to transfer is great.</span></span> <span data-ttu-id="6d02a-180">Azonban igényt az átviteli előrehaladásának megtekintéséhez *során* az adatátviteli művelet még élvezetesebbé lenne.</span><span class="sxs-lookup"><span data-stu-id="6d02a-180">However, being able to see the progress of our transfer *during* the transfer operation would be even better.</span></span> <span data-ttu-id="6d02a-181">Ebben a forgatókönyvben eléréséhez létre kell hoznunk egy `TransferContext` objektum.</span><span class="sxs-lookup"><span data-stu-id="6d02a-181">To achieve this scenario, we need to create a `TransferContext` object.</span></span> <span data-ttu-id="6d02a-182">A `TransferContext` objektum két formában származik: `SingleTransferContext` és `DirectoryTransferContext`.</span><span class="sxs-lookup"><span data-stu-id="6d02a-182">The `TransferContext` object comes in two forms: `SingleTransferContext` and `DirectoryTransferContext`.</span></span> <span data-ttu-id="6d02a-183">A korábbi átvitelére egyetlen fájlba (amely azt még tevékenységeit most), az utóbbi pedig egy könyvtárat (amely azt később felvenni) fájlok átvitele.</span><span class="sxs-lookup"><span data-stu-id="6d02a-183">The former is for transferring a single file (which is what we're doing now) and the latter is for transferring a directory of files (which we are adding later).</span></span>

<span data-ttu-id="6d02a-184">Adja hozzá a metódusokat `GetSingleTransferContext` és `GetDirectoryTransferContext` való `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-184">Add the methods `GetSingleTransferContext` and `GetDirectoryTransferContext` to `Program.cs`:</span></span> 

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

<span data-ttu-id="6d02a-185">Módosítsa a `TransferLocalFileToAzureBlob` használandó módszer `GetSingleTransferContext`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-185">Modify the `TransferLocalFileToAzureBlob` method to use `GetSingleTransferContext`:</span></span>

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

## <a name="resume-a-canceled-transfer"></a><span data-ttu-id="6d02a-186">A megszakított átvitel folytatása</span><span class="sxs-lookup"><span data-stu-id="6d02a-186">Resume a canceled transfer</span></span>
<span data-ttu-id="6d02a-187">Az adatátviteli könyvtár által kínált egy másik kényelmes szolgáltatás azt a képességet a megszakított átvitel folytatása.</span><span class="sxs-lookup"><span data-stu-id="6d02a-187">Another convenient feature offered by the Data Movement Library is the ability to resume a canceled transfer.</span></span> <span data-ttu-id="6d02a-188">Adjunk néhány kódot, amely lehetővé teszi, hogy ideiglenesen szakítsa meg a írja be az átvitelt `c`, és ezután úgy folytatódik, az átviteli 3 másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="6d02a-188">Let's add some code that allows us to temporarily cancel the transfer by typing `c`, and then resume the transfer 3 seconds later.</span></span>

<span data-ttu-id="6d02a-189">Módosítsa `TransferLocalFileToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-189">Modify `TransferLocalFileToAzureBlob`:</span></span>

```csharp
public static async Task TransferLocalFileToAzureBlob(CloudStorageAccount account)
{ 
    string localFilePath = GetSourcePath();
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="6d02a-190">Eddig a `checkpoint` érték mindig állították be `null`.</span><span class="sxs-lookup"><span data-stu-id="6d02a-190">Up until now, our `checkpoint` value has always been set to `null`.</span></span> <span data-ttu-id="6d02a-191">Most az átvitel megszakítása, ha azt lekérése az utolsó ellenőrzőpont az átvitel, akkor használja ezt az új ellenőrzőpontot az átviteli környezetben.</span><span class="sxs-lookup"><span data-stu-id="6d02a-191">Now, if we cancel the transfer, we retrieve the last checkpoint of our transfer, then use this new checkpoint in our transfer context.</span></span> 

## <a name="transfer-local-directory-to-azure-blob-directory"></a><span data-ttu-id="6d02a-192">Helyi könyvtár átvitele az Azure Blob-könyvtár</span><span class="sxs-lookup"><span data-stu-id="6d02a-192">Transfer local directory to Azure Blob directory</span></span>
<span data-ttu-id="6d02a-193">Címkézést lenne, ha az adatátviteli könyvtár csak továbbíthat egy fájlban egyszerre.</span><span class="sxs-lookup"><span data-stu-id="6d02a-193">It would be disappointing if the Data Movement Library could only transfer one file at a time.</span></span> <span data-ttu-id="6d02a-194">Szerencsére ez nem a helyzet.</span><span class="sxs-lookup"><span data-stu-id="6d02a-194">Luckily, this is not the case.</span></span> <span data-ttu-id="6d02a-195">Az adatátviteli könyvtár lehetővé teszi a fájlok és alkönyvtárak könyvtár átvitele.</span><span class="sxs-lookup"><span data-stu-id="6d02a-195">The Data Movement Library provides the ability to transfer a directory of files and all of its subdirectories.</span></span> <span data-ttu-id="6d02a-196">Adjunk néhány kódot, amely lehetővé teszi, hogy éppen ez.</span><span class="sxs-lookup"><span data-stu-id="6d02a-196">Let's add some code that allows us to do just that.</span></span>

<span data-ttu-id="6d02a-197">Első lépésként adja meg a metódus `GetBlobDirectory` való `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-197">First, add the method `GetBlobDirectory` to `Program.cs`:</span></span>

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

<span data-ttu-id="6d02a-198">Ezután módosítsa `TransferLocalDirectoryToAzureBlobDirectory`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-198">Then, modify `TransferLocalDirectoryToAzureBlobDirectory`:</span></span>

```csharp
public static async Task TransferLocalDirectoryToAzureBlobDirectory(CloudStorageAccount account)
{ 
    string localDirectoryPath = GetSourcePath();
    CloudBlobDirectory blobDirectory = GetBlobDirectory(account); 
    TransferCheckpoint checkpoint = null;
    DirectoryTransferContext context = GetDirectoryTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="6d02a-199">Ez a metódus és a metódus egyetlen fájl feltöltése között néhány különbségek vannak.</span><span class="sxs-lookup"><span data-stu-id="6d02a-199">There are a few differences between this method and the method for uploading a single file.</span></span> <span data-ttu-id="6d02a-200">Most használunk `TransferManager.UploadDirectoryAsync` és a `getDirectoryTransferContext` korábban létrehozott metódust.</span><span class="sxs-lookup"><span data-stu-id="6d02a-200">We're now using `TransferManager.UploadDirectoryAsync` and the `getDirectoryTransferContext` method we created earlier.</span></span> <span data-ttu-id="6d02a-201">Emellett mostantól elérhető egy `options` a feltöltési művelet, amely lehetővé teszi annak jelzésére, hogy szeretnénk alkönyvtárak szerepeljenek a feltöltési értéket.</span><span class="sxs-lookup"><span data-stu-id="6d02a-201">In addition, we now provide an `options` value to our upload operation, which allows us to indicate that we want to include subdirectories in our upload.</span></span> 

## <a name="copy-file-from-url-to-azure-blob"></a><span data-ttu-id="6d02a-202">Fájl másolása Azure Blob URL-címe</span><span class="sxs-lookup"><span data-stu-id="6d02a-202">Copy file from URL to Azure Blob</span></span>
<span data-ttu-id="6d02a-203">Most adjuk hozzá a kódot, amely lehetővé teszi, hogy a fájl másolása az Azure Blob URL-címről.</span><span class="sxs-lookup"><span data-stu-id="6d02a-203">Now, let's add code that allows us to copy a file from a URL to an Azure Blob.</span></span> 

<span data-ttu-id="6d02a-204">Módosítsa `TransferUrlToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-204">Modify `TransferUrlToAzureBlob`:</span></span>

```csharp
public static async Task TransferUrlToAzureBlob(CloudStorageAccount account)
{
    Uri uri = new Uri(GetSourcePath());
    CloudBlockBlob blob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="6d02a-205">A szolgáltatás egy fontos használati eset esetén az adatok áthelyezése egy másik felhőszolgáltatáshoz (pl. AWS) Azure kell.</span><span class="sxs-lookup"><span data-stu-id="6d02a-205">One important use case for this feature is when you need to move data from another cloud service (e.g. AWS) to Azure.</span></span> <span data-ttu-id="6d02a-206">Mindaddig, amíg hozzáférést biztosít az erőforrás URL-CÍMMEL rendelkezik, egyszerűen áthelyezheti a erőforrást az Azure BLOB használatával a `TransferManager.CopyAsync` metódust.</span><span class="sxs-lookup"><span data-stu-id="6d02a-206">As long as you have a URL that gives you access to the resource, you can easily move that resource into Azure Blobs by using the `TransferManager.CopyAsync` method.</span></span> <span data-ttu-id="6d02a-207">Ez a módszer is bevezet egy új logikai paraméter.</span><span class="sxs-lookup"><span data-stu-id="6d02a-207">This method also introduces a new boolean parameter.</span></span> <span data-ttu-id="6d02a-208">Ha a paramétert a `true` azt jelzi, hogy szeretnénk egy aszinkron kiszolgálóoldali lemásolni.</span><span class="sxs-lookup"><span data-stu-id="6d02a-208">Setting this parameter to `true` indicates that we want to do an asynchronous server-side copy.</span></span> <span data-ttu-id="6d02a-209">Ha a paramétert a `false` azt jelzi, ami azt jelenti, az erőforrás előbb letöltődik a helyi számítógépre, az Azure Blob feltöltött - szinkron másolatot.</span><span class="sxs-lookup"><span data-stu-id="6d02a-209">Setting this parameter to `false` indicates a synchronous copy - meaning the resource is downloaded to our local machine first, then uploaded to Azure Blob.</span></span> <span data-ttu-id="6d02a-210">Azonban szinkron másolatot érhető el jelenleg csak egy Azure Storage erőforrás egy másikra történő másolását.</span><span class="sxs-lookup"><span data-stu-id="6d02a-210">However, synchronous copy is currently only available for copying from one Azure Storage resource to another.</span></span> 

## <a name="transfer-azure-blob-to-azure-blob"></a><span data-ttu-id="6d02a-211">Az Azure Blob átvitele az Azure Blob</span><span class="sxs-lookup"><span data-stu-id="6d02a-211">Transfer Azure Blob to Azure Blob</span></span>
<span data-ttu-id="6d02a-212">Egy másik szolgáltatás, amely egyedileg az adatátviteli könyvtár biztosítja azt a képességet egy Azure Storage-erőforrások másolása a másikra.</span><span class="sxs-lookup"><span data-stu-id="6d02a-212">Another feature that's uniquely provided by the Data Movement Library is the ability to copy from one Azure Storage resource to another.</span></span> 

<span data-ttu-id="6d02a-213">Módosítsa `TransferAzureBlobToAzureBlob`:</span><span class="sxs-lookup"><span data-stu-id="6d02a-213">Modify `TransferAzureBlobToAzureBlob`:</span></span>

```csharp
public static async Task TransferAzureBlobToAzureBlob(CloudStorageAccount account)
{
    CloudBlockBlob sourceBlob = GetBlob(account);
    CloudBlockBlob destinationBlob = GetBlob(account); 
    TransferCheckpoint checkpoint = null;
    SingleTransferContext context = GetSingleTransferContext(checkpoint); 
    CancellationTokenSource cancellationSource = new CancellationTokenSource();
    Console.WriteLine("\nTransfer started...\nPress 'c' to temporarily cancel your transfer...\n");

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

<span data-ttu-id="6d02a-214">Az ebben a példában azt állítsa be a logikai paraméter `TransferManager.CopyAsync` való `false` annak jelzésére, hogy azt szeretné szinkron másolatot.</span><span class="sxs-lookup"><span data-stu-id="6d02a-214">In this example, we set the boolean parameter in `TransferManager.CopyAsync` to `false` to indicate that we want to do a synchronous copy.</span></span> <span data-ttu-id="6d02a-215">Ez azt jelenti, hogy az erőforrás előbb letöltődik a helyi számítógépen, majd az Azure Blob feltöltése.</span><span class="sxs-lookup"><span data-stu-id="6d02a-215">This means that the resource is downloaded to our local machine first, then uploaded to Azure Blob.</span></span> <span data-ttu-id="6d02a-216">A szinkron másolatot elem kiváló módja annak, ügyeljen arra, hogy a másolási művelet egységes sebesség.</span><span class="sxs-lookup"><span data-stu-id="6d02a-216">The synchronous copy option is a great way to ensure that your copy operation has a consistent speed.</span></span> <span data-ttu-id="6d02a-217">Ezzel szemben egy aszinkron kiszolgálóoldali másolatot sebességétől függ a rendelkezésre álló hálózati sávszélességet a kiszolgálón, amely hajlamos is.</span><span class="sxs-lookup"><span data-stu-id="6d02a-217">In contrast, the speed of an asynchronous server-side copy is dependent on the available network bandwidth on the server, which can fluctuate.</span></span> <span data-ttu-id="6d02a-218">Azonban a szinkron másolatot hozhat létre további kilépő költség aszinkron másolási képest.</span><span class="sxs-lookup"><span data-stu-id="6d02a-218">However, synchronous copy may generate additional egress cost compared to asynchronous copy.</span></span> <span data-ttu-id="6d02a-219">Az ajánlott módszer, hogy szinkron másolatot használja egy Azure virtuális gép, amely ugyanabban a régióban, a forrás tárolási fiókkal kilépő költségek elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="6d02a-219">The recommended approach is to use synchronous copy in an Azure VM that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="conclusion"></a><span data-ttu-id="6d02a-220">Összegzés</span><span class="sxs-lookup"><span data-stu-id="6d02a-220">Conclusion</span></span>
<span data-ttu-id="6d02a-221">Az adatok mozgása alkalmazás most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6d02a-221">Our data movement application is now complete.</span></span> <span data-ttu-id="6d02a-222">[A teljes kód mintát a Githubon érhető el](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span><span class="sxs-lookup"><span data-stu-id="6d02a-222">[The full code sample is available on GitHub](https://github.com/azure-samples/storage-dotnet-data-movement-library-app).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6d02a-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d02a-223">Next steps</span></span>
<span data-ttu-id="6d02a-224">Az első lépések, létrehozott egy alkalmazás, amely együttműködik az Azure Storage és a Windows, Linux és macOS futtatja.</span><span class="sxs-lookup"><span data-stu-id="6d02a-224">In this getting started, we created an application that interacts with Azure Storage and runs on Windows, Linux, and macOS.</span></span> <span data-ttu-id="6d02a-225">Ez az első lépések a Blob Storage összpontosít.</span><span class="sxs-lookup"><span data-stu-id="6d02a-225">This getting started focused on Blob Storage.</span></span> <span data-ttu-id="6d02a-226">Azonban ez azonos Tudásbázis alkalmazhatja a File Storage.</span><span class="sxs-lookup"><span data-stu-id="6d02a-226">However, this same knowledge can be applied to File Storage.</span></span> <span data-ttu-id="6d02a-227">További tudnivalókért tekintse meg [Azure Storage adatátviteli könyvtár referenciadokumentációt](https://azure.github.io/azure-storage-net-data-movement).</span><span class="sxs-lookup"><span data-stu-id="6d02a-227">To learn more, check out [Azure Storage Data Movement Library reference documentation](https://azure.github.io/azure-storage-net-data-movement).</span></span>

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]




