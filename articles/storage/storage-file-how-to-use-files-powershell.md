---
title: aaaHow toouse PowerShell toomanage Azure File storage |} Microsoft Docs
description: Ismerje meg, hogy toouse PowerShell toomanage Azure File storage.
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
ms.openlocfilehash: 0e30e8796cf8bbf5f9249b26179d5e0f9077c8fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a><span data-ttu-id="92106-103">Hogyan toouse PowerShell toomanage Azure File storage</span><span class="sxs-lookup"><span data-stu-id="92106-103">How toouse PowerShell toomanage Azure File storage</span></span>
<span data-ttu-id="92106-104">Használhatja az Azure PowerShell toocreate és fájlmegosztásokhoz.</span><span class="sxs-lookup"><span data-stu-id="92106-104">You can use Azure PowerShell toocreate and manage file shares.</span></span>

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="92106-105">Az Azure Storage hello PowerShell-parancsmagjainak telepítése</span><span class="sxs-lookup"><span data-stu-id="92106-105">Install hello PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="92106-106">tooprepare toouse PowerShell, töltse le és telepítse hello Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="92106-106">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="92106-107">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) hello telepítse pont és a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="92106-107">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for hello install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="92106-108">Javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="92106-108">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="92106-109">Kattintson a **Start** gombra, és írja be a **Windows PowerShell** kifejezést egy Azure PowerShell ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="92106-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="92106-110">hello PowerShell ablakban terhelésének hello Azure Powershell modul.</span><span class="sxs-lookup"><span data-stu-id="92106-110">hello PowerShell window loads hello Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="92106-111">Környezet létrehozása a tárfiókhoz és a fiókkulcshoz</span><span class="sxs-lookup"><span data-stu-id="92106-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="92106-112">Hozzon létre hello tárfiók környezetét.</span><span class="sxs-lookup"><span data-stu-id="92106-112">Create hello storage account context.</span></span> <span data-ttu-id="92106-113">hello környezet magában foglalja a hello tárfiók neve és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="92106-113">hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="92106-114">Útmutatás a fiókkulcs másolását hello [Azure-portálon](https://portal.azure.com), lásd: [megtekintése és másolása tárelérési kulcsok](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="92106-114">For instructions on copying your account key from hello [Azure portal](https://portal.azure.com), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="92106-115">Cserélje le `storage-account-name` és `storage-account-key` a tárfiók neve és az alábbi példa hello kulccsal.</span><span class="sxs-lookup"><span data-stu-id="92106-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in hello following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="92106-116">Új fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="92106-116">Create a new file share</span></span>
<span data-ttu-id="92106-117">Hozzon létre hello nevű új megosztást `logs`.</span><span class="sxs-lookup"><span data-stu-id="92106-117">Create hello new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="92106-118">Így létrejött egy fájlmegosztás a fájltárolóban.</span><span class="sxs-lookup"><span data-stu-id="92106-118">You now have a file share in File storage.</span></span> <span data-ttu-id="92106-119">A következő lépésben hozzá kell adnia egy könyvtárat és egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="92106-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="92106-120">a fájlmegosztás nevét hello összes kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="92106-120">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="92106-121">A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="92106-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a><span data-ttu-id="92106-122">Hozzon létre egy könyvtárat hello fájlmegosztásban</span><span class="sxs-lookup"><span data-stu-id="92106-122">Create a directory in hello file share</span></span>
<span data-ttu-id="92106-123">Hozzon létre egy könyvtárat hello megosztáson található.</span><span class="sxs-lookup"><span data-stu-id="92106-123">Create a directory in hello share.</span></span> <span data-ttu-id="92106-124">A következő példa hello, hello könyvtár neve a `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="92106-124">In hello following example, hello directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a><span data-ttu-id="92106-125">Töltse fel a helyi fájl toohello könyvtár</span><span class="sxs-lookup"><span data-stu-id="92106-125">Upload a local file toohello directory</span></span>
<span data-ttu-id="92106-126">Töltsön fel egy helyi fájl toohello könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="92106-126">Now upload a local file toohello directory.</span></span> <span data-ttu-id="92106-127">hello alábbi példa feltölt egy fájlt a `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="92106-127">hello following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="92106-128">Szerkesztése hello fájl elérési útját, hogy tooa érvényes fájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="92106-128">Edit hello file path so that it points tooa valid file on your local machine.</span></span>

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a><span data-ttu-id="92106-129">Hello könyvtárban hello fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="92106-129">List hello files in hello directory</span></span>
<span data-ttu-id="92106-130">toosee hello fájl hello könyvtárban, listázhatja összes hello directory fájlt.</span><span class="sxs-lookup"><span data-stu-id="92106-130">toosee hello file in hello directory, you can list all of hello directory's files.</span></span> <span data-ttu-id="92106-131">A parancs visszaadja hello fájlt és alkönyvtárt (ha vannak ilyenek) hello CustomLogs könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="92106-131">This command returns hello files and subdirectories (if there are any) in hello CustomLogs directory.</span></span>

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="92106-132">A Get-AzureStorageFile parancs bármilyen átadott könyvtárobjektum fájljait és könyvtárait listázza.</span><span class="sxs-lookup"><span data-stu-id="92106-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="92106-133">"Get-AzureStorageFile-Share $s" hello gyökérkönyvtárában fájlok és könyvtárak listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="92106-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in hello root directory.</span></span> <span data-ttu-id="92106-134">tooget egy alkönyvtár fájljait a listáját, hogy toopass hello alkönyvtár tooGet-azurestoragefile parancsnak.</span><span class="sxs-lookup"><span data-stu-id="92106-134">tooget a list of files in a subdirectory, you have toopass hello subdirectory tooGet-AzureStorageFile.</span></span> <span data-ttu-id="92106-135">Ez az a funkciója, – hello hello parancs mentése toohello cső első része egy CustomLogs alkönyvtár hello directory példányát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="92106-135">That's what this does -- hello first part of hello command up toohello pipe returns a directory instance of hello subdirectory CustomLogs.</span></span> <span data-ttu-id="92106-136">Amelyet aztán átad a Get-azurestoragefile parancsnak, ami hello fájlok és könyvtárak visszaadja a CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="92106-136">Then that is passed into Get-AzureStorageFile, which returns hello files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="92106-137">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="92106-137">Copy files</span></span>
<span data-ttu-id="92106-138">Azure PowerShell 0.9.7-es verziójával kezdve másolhat egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl.</span><span class="sxs-lookup"><span data-stu-id="92106-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="92106-139">Az alábbiakban bemutatjuk, hogyan tooperform ezek másolása műveletek PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="92106-139">Below we demonstrate how tooperform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="92106-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92106-140">Next steps</span></span>
<span data-ttu-id="92106-141">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="92106-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="92106-142">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="92106-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="92106-143">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="92106-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)