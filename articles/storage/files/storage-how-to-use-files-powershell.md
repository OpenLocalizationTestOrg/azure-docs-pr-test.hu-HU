---
title: "Az Azure File Storage kezelése PowerShell használatával | Microsoft Docs"
description: "Itt megismerheti, hogyan kezelheti az Azure File Storage szolgáltatást PowerShell használatával."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: ce62d4423ce711a6902aed7b8174ff4e827f6083
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a><span data-ttu-id="3c7a2-103">Az Azure File Storage kezelése PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="3c7a2-103">How to use PowerShell to manage Azure File storage</span></span>
<span data-ttu-id="3c7a2-104">Az Azure PowerShell szolgáltatást is használhatja fájlmegosztások létrehozására és kezelésére.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-104">You can use Azure PowerShell to create and manage file shares.</span></span>

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="3c7a2-105">Az Azure Storage PowerShell-parancsmagjainak telepítése</span><span class="sxs-lookup"><span data-stu-id="3c7a2-105">Install the PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="3c7a2-106">A PowerShell használatának előkészítéseként töltse le és telepítse az Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-106">To prepare to use PowerShell, download and install the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="3c7a2-107">A telepítési helyre és a telepítésre vonatkozó utasításokért lásd: [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="3c7a2-107">See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for the install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="3c7a2-108">Javasoljuk, hogy frissítsen a legújabb Azure PowerShell modulra, vagy töltse le és telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-108">It's recommended that you download and install or upgrade to the latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="3c7a2-109">Kattintson a **Start** gombra, és írja be a **Windows PowerShell** kifejezést egy Azure PowerShell ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="3c7a2-110">A PowerShell-ablak betölti az Azure PowerShell modult.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-110">The PowerShell window loads the Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="3c7a2-111">Környezet létrehozása a tárfiókhoz és a fiókkulcshoz</span><span class="sxs-lookup"><span data-stu-id="3c7a2-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="3c7a2-112">Hozza létre a tárfiók környezetét.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-112">Create the storage account context.</span></span> <span data-ttu-id="3c7a2-113">A környezet magában foglalja a tárfiók nevét és a fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-113">The context encapsulates the storage account name and account key.</span></span> <span data-ttu-id="3c7a2-114">Útmutatás a fiókkulcs átmásolásához az [Azure Portalról](https://portal.azure.com): [A tárelérési kulcs megtekintése és másolása](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="3c7a2-114">For instructions on copying your account key from the [Azure portal](https://portal.azure.com), see [View and copy storage access keys](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="3c7a2-115">Az alábbi példában cserélje ki a `storage-account-name` és a `storage-account-key` elemet a tárfiók nevére és kulcsára.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in the following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="3c7a2-116">Új fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c7a2-116">Create a new file share</span></span>
<span data-ttu-id="3c7a2-117">Hozzon létre egy `logs` nevű új megosztást.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-117">Create the new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="3c7a2-118">Így létrejött egy fájlmegosztás a fájltárolóban.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-118">You now have a file share in File storage.</span></span> <span data-ttu-id="3c7a2-119">A következő lépésben hozzá kell adnia egy könyvtárat és egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c7a2-120">A fájlmegosztás nevében csak kisbetű szerepelhet.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-120">The name of your file share must be all lowercase.</span></span> <span data-ttu-id="3c7a2-121">A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="3c7a2-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-the-file-share"></a><span data-ttu-id="3c7a2-122">Könyvtár létrehozása a fájlmegosztásban</span><span class="sxs-lookup"><span data-stu-id="3c7a2-122">Create a directory in the file share</span></span>
<span data-ttu-id="3c7a2-123">Hozzon létre a megosztásban egy könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-123">Create a directory in the share.</span></span> <span data-ttu-id="3c7a2-124">Az alábbi példában szereplő könyvtár neve `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-124">In the following example, the directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a><span data-ttu-id="3c7a2-125">Helyi fájl feltöltése a könyvtárba</span><span class="sxs-lookup"><span data-stu-id="3c7a2-125">Upload a local file to the directory</span></span>
<span data-ttu-id="3c7a2-126">Töltsön fel egy helyi fájlt a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-126">Now upload a local file to the directory.</span></span> <span data-ttu-id="3c7a2-127">Az alábbi példa a következő helyről tölt fel egy fájlt: `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-127">The following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="3c7a2-128">A fájl elérési útját úgy szerkessze, hogy egy, a helyi gépen található érvényes fájlra mutasson.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-128">Edit the file path so that it points to a valid file on your local machine.</span></span>

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a><span data-ttu-id="3c7a2-129">A könyvtárban található fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="3c7a2-129">List the files in the directory</span></span>
<span data-ttu-id="3c7a2-130">Ha látni szeretné a fájlt a könyvtárban, listázhatja a könyvtárban található összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-130">To see the file in the directory, you can list all of the directory's files.</span></span> <span data-ttu-id="3c7a2-131">A parancs visszaadja a CustomLogs könyvtárban található összes fájlt és alkönyvtárt (ha van alkönyvtár).</span><span class="sxs-lookup"><span data-stu-id="3c7a2-131">This command returns the files and subdirectories (if there are any) in the CustomLogs directory.</span></span>

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="3c7a2-132">A Get-AzureStorageFile parancs bármilyen átadott könyvtárobjektum fájljait és könyvtárait listázza.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="3c7a2-133">A „Get-AzureStorageFile -Share $s” parancs a gyökérkönyvtár fájljait és könyvtárait listázza.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in the root directory.</span></span> <span data-ttu-id="3c7a2-134">Ha egy alkönyvtár fájljait szeretné listázni, meg az alkönyvtárat kell megadnia a Get-AzureStorageFile parancsnak.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-134">To get a list of files in a subdirectory, you have to pass the subdirectory to Get-AzureStorageFile.</span></span> <span data-ttu-id="3c7a2-135">Így a parancs függőleges vonalig tartó első része visszaadja a CustomLogs alkönyvtár egy könyvtárpéldányát,</span><span class="sxs-lookup"><span data-stu-id="3c7a2-135">That's what this does -- the first part of the command up to the pipe returns a directory instance of the subdirectory CustomLogs.</span></span> <span data-ttu-id="3c7a2-136">amelyet aztán átad a Get-AzureStorageFile parancsnak, ami visszaadja a CustomLogs könyvtárban található fájlok és könyvtárak listáját.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-136">Then that is passed into Get-AzureStorageFile, which returns the files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="3c7a2-137">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="3c7a2-137">Copy files</span></span>
<span data-ttu-id="3c7a2-138">Az Azure PowerShell 0.9.7-es verziójától kezdve másolhat egy fájlt egy másik fájlba, egy fájlt egy blobba vagy egy blobot egy fájlba.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="3c7a2-139">Alább bemutatjuk, hogyan hajthatja végre ezeket a másolási műveleteket a PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-139">Below we demonstrate how to perform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="3c7a2-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c7a2-140">Next steps</span></span>
<span data-ttu-id="3c7a2-141">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="3c7a2-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="3c7a2-142">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="3c7a2-142">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="3c7a2-143">Hibaelhárítás a Windows rendszerben</span><span class="sxs-lookup"><span data-stu-id="3c7a2-143">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="3c7a2-144">Hibaelhárítás a Linux rendszerben</span><span class="sxs-lookup"><span data-stu-id="3c7a2-144">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    