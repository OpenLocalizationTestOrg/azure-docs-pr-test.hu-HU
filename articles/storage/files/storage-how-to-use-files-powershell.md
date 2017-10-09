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
ms.openlocfilehash: 7bd84c9cfa31782aedf4a209cb15d4b8d92e2737
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a><span data-ttu-id="4d520-103">Hogyan toouse PowerShell toomanage Azure File storage</span><span class="sxs-lookup"><span data-stu-id="4d520-103">How toouse PowerShell toomanage Azure File storage</span></span>
<span data-ttu-id="4d520-104">Használhatja az Azure PowerShell toocreate és fájlmegosztásokhoz.</span><span class="sxs-lookup"><span data-stu-id="4d520-104">You can use Azure PowerShell toocreate and manage file shares.</span></span>

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="4d520-105">Az Azure Storage hello PowerShell-parancsmagjainak telepítése</span><span class="sxs-lookup"><span data-stu-id="4d520-105">Install hello PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="4d520-106">tooprepare toouse PowerShell, töltse le és telepítse hello Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="4d520-106">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="4d520-107">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) hello telepítse pont és a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4d520-107">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for hello install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="4d520-108">Javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="4d520-108">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="4d520-109">Kattintson a **Start** gombra, és írja be a **Windows PowerShell** kifejezést egy Azure PowerShell ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="4d520-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="4d520-110">hello PowerShell ablakban terhelésének hello Azure Powershell modul.</span><span class="sxs-lookup"><span data-stu-id="4d520-110">hello PowerShell window loads hello Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="4d520-111">Környezet létrehozása a tárfiókhoz és a fiókkulcshoz</span><span class="sxs-lookup"><span data-stu-id="4d520-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="4d520-112">Hozzon létre hello tárfiók környezetét.</span><span class="sxs-lookup"><span data-stu-id="4d520-112">Create hello storage account context.</span></span> <span data-ttu-id="4d520-113">hello környezet magában foglalja a hello tárfiók neve és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4d520-113">hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="4d520-114">Útmutatás a fiókkulcs másolását hello [Azure-portálon](https://portal.azure.com), lásd: [megtekintése és másolása tárelérési kulcsok](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="4d520-114">For instructions on copying your account key from hello [Azure portal](https://portal.azure.com), see [View and copy storage access keys](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="4d520-115">Cserélje le `storage-account-name` és `storage-account-key` a tárfiók neve és az alábbi példa hello kulccsal.</span><span class="sxs-lookup"><span data-stu-id="4d520-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in hello following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="4d520-116">Új fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d520-116">Create a new file share</span></span>
<span data-ttu-id="4d520-117">Hozzon létre hello nevű új megosztást `logs`.</span><span class="sxs-lookup"><span data-stu-id="4d520-117">Create hello new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="4d520-118">Így létrejött egy fájlmegosztás a fájltárolóban.</span><span class="sxs-lookup"><span data-stu-id="4d520-118">You now have a file share in File storage.</span></span> <span data-ttu-id="4d520-119">A következő lépésben hozzá kell adnia egy könyvtárat és egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="4d520-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4d520-120">a fájlmegosztás nevét hello összes kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4d520-120">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="4d520-121">A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="4d520-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a><span data-ttu-id="4d520-122">Hozzon létre egy könyvtárat hello fájlmegosztásban</span><span class="sxs-lookup"><span data-stu-id="4d520-122">Create a directory in hello file share</span></span>
<span data-ttu-id="4d520-123">Hozzon létre egy könyvtárat hello megosztáson található.</span><span class="sxs-lookup"><span data-stu-id="4d520-123">Create a directory in hello share.</span></span> <span data-ttu-id="4d520-124">A következő példa hello, hello könyvtár neve a `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="4d520-124">In hello following example, hello directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a><span data-ttu-id="4d520-125">Töltse fel a helyi fájl toohello könyvtár</span><span class="sxs-lookup"><span data-stu-id="4d520-125">Upload a local file toohello directory</span></span>
<span data-ttu-id="4d520-126">Töltsön fel egy helyi fájl toohello könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="4d520-126">Now upload a local file toohello directory.</span></span> <span data-ttu-id="4d520-127">hello alábbi példa feltölt egy fájlt a `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="4d520-127">hello following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="4d520-128">Szerkesztése hello fájl elérési útját, hogy tooa érvényes fájlt a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4d520-128">Edit hello file path so that it points tooa valid file on your local machine.</span></span>

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a><span data-ttu-id="4d520-129">Hello könyvtárban hello fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="4d520-129">List hello files in hello directory</span></span>
<span data-ttu-id="4d520-130">toosee hello fájl hello könyvtárban, listázhatja összes hello directory fájlt.</span><span class="sxs-lookup"><span data-stu-id="4d520-130">toosee hello file in hello directory, you can list all of hello directory's files.</span></span> <span data-ttu-id="4d520-131">A parancs visszaadja hello fájlt és alkönyvtárt (ha vannak ilyenek) hello CustomLogs könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="4d520-131">This command returns hello files and subdirectories (if there are any) in hello CustomLogs directory.</span></span>

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="4d520-132">A Get-AzureStorageFile parancs bármilyen átadott könyvtárobjektum fájljait és könyvtárait listázza.</span><span class="sxs-lookup"><span data-stu-id="4d520-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="4d520-133">"Get-AzureStorageFile-Share $s" hello gyökérkönyvtárában fájlok és könyvtárak listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4d520-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in hello root directory.</span></span> <span data-ttu-id="4d520-134">tooget egy alkönyvtár fájljait a listáját, hogy toopass hello alkönyvtár tooGet-azurestoragefile parancsnak.</span><span class="sxs-lookup"><span data-stu-id="4d520-134">tooget a list of files in a subdirectory, you have toopass hello subdirectory tooGet-AzureStorageFile.</span></span> <span data-ttu-id="4d520-135">Ez az a funkciója, – hello hello parancs mentése toohello cső első része egy CustomLogs alkönyvtár hello directory példányát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4d520-135">That's what this does -- hello first part of hello command up toohello pipe returns a directory instance of hello subdirectory CustomLogs.</span></span> <span data-ttu-id="4d520-136">Amelyet aztán átad a Get-azurestoragefile parancsnak, ami hello fájlok és könyvtárak visszaadja a CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="4d520-136">Then that is passed into Get-AzureStorageFile, which returns hello files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="4d520-137">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="4d520-137">Copy files</span></span>
<span data-ttu-id="4d520-138">Azure PowerShell 0.9.7-es verziójával kezdve másolhat egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl.</span><span class="sxs-lookup"><span data-stu-id="4d520-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="4d520-139">Az alábbiakban bemutatjuk, hogyan tooperform ezek másolása műveletek PowerShell-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="4d520-139">Below we demonstrate how tooperform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="4d520-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d520-140">Next steps</span></span>
<span data-ttu-id="4d520-141">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="4d520-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="4d520-142">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="4d520-142">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="4d520-143">Hibaelhárítás a Windows rendszerben</span><span class="sxs-lookup"><span data-stu-id="4d520-143">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="4d520-144">Hibaelhárítás a Linux rendszerben</span><span class="sxs-lookup"><span data-stu-id="4d520-144">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    