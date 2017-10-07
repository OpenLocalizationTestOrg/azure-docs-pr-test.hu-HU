---
title: "egy Azure fájlmegosztás aaaMount és a hozzáférés hello ugyanazt a Windows |} Microsoft Docs"
description: "Csatlakoztassa egy Azure fájlmegosztás és a Windows hello megosztásra hozzáférést."
services: storage
documentationcenter: na
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
ms.openlocfilehash: 15ac468d9d7b8e0a195b024926ed4dd9790360d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-an-azure-file-share-and-access-hello-share-in-windows"></a><span data-ttu-id="67512-103">Egy Azure fájlmegosztás és a Windows hello megosztásra hozzáférés csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="67512-103">Mount an Azure File share and access hello share in Windows</span></span>
<span data-ttu-id="67512-104">[Az Azure File storage](storage-dotnet-how-to-use-files.md) a Microsoft easy toouse felhő fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="67512-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy toouse cloud file system.</span></span> <span data-ttu-id="67512-105">Az Azure-fájlmegosztások Windows és Windows Server rendszeren csatlakoztathatók.</span><span class="sxs-lookup"><span data-stu-id="67512-105">Azure File shares can be mounted in Windows and Windows Server.</span></span> <span data-ttu-id="67512-106">Ez a cikk bemutatja három különböző módon toomount egy Azure fájlmegosztás Windows rendszeren: a fájl Explorer felhasználói felületén, és PowerShell keresztül hello parancssor hello.</span><span class="sxs-lookup"><span data-stu-id="67512-106">This article shows three different ways toomount an Azure File share on Windows: with hello File Explorer UI, via PowerShell, and via hello Command Prompt.</span></span> 

<span data-ttu-id="67512-107">Rendelés toomount egy Azure fájlmegosztás hello kívül az Azure-régió, például a helyszínen vagy a különböző Azure-régiót helyezkedik el, a hello az operációs rendszer támogatniuk kell az SMB 3.0-s.</span><span class="sxs-lookup"><span data-stu-id="67512-107">In order toomount an Azure File share outside of hello Azure region it is hosted in, such as on-premises or in a different Azure region, hello OS must support SMB 3.0.</span></span> 

<span data-ttu-id="67512-108">Az Azure-fájlmegosztás az operációs rendszer verziójától függően egy helyszíni vagy Azure-beli virtuális gépen lévő Windows-gépen csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="67512-108">Azure File share can be mounted on Windows machine either on-premises or in Azure VM depending on OS version.</span></span> <span data-ttu-id="67512-109">Alábbi táblázat bemutatja a hello</span><span class="sxs-lookup"><span data-stu-id="67512-109">Below table illustrates hello</span></span> 

| <span data-ttu-id="67512-110">Windows-verzió</span><span class="sxs-lookup"><span data-stu-id="67512-110">Windows Version</span></span>        | <span data-ttu-id="67512-111">SMB-verzió</span><span class="sxs-lookup"><span data-stu-id="67512-111">SMB Version</span></span> |<span data-ttu-id="67512-112">Azure-beli virtuális gépen csatlakoztatható</span><span class="sxs-lookup"><span data-stu-id="67512-112">Mountable On Azure VM</span></span>|<span data-ttu-id="67512-113">Helyszínen csatlakoztatható</span><span class="sxs-lookup"><span data-stu-id="67512-113">Mountable On-Premise</span></span>|
|------------------------|-------------|---------------------|---------------------|
| <span data-ttu-id="67512-114">Windows 7</span><span class="sxs-lookup"><span data-stu-id="67512-114">Windows 7</span></span>              | <span data-ttu-id="67512-115">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="67512-115">SMB 2.1</span></span>     | <span data-ttu-id="67512-116">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-116">Yes</span></span>                 | <span data-ttu-id="67512-117">Nem</span><span class="sxs-lookup"><span data-stu-id="67512-117">No</span></span>                  |
| <span data-ttu-id="67512-118">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="67512-118">Windows Server 2008 R2</span></span> | <span data-ttu-id="67512-119">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="67512-119">SMB 2.1</span></span>     | <span data-ttu-id="67512-120">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-120">Yes</span></span>                 | <span data-ttu-id="67512-121">Nem</span><span class="sxs-lookup"><span data-stu-id="67512-121">No</span></span>                  |
| <span data-ttu-id="67512-122">Windows 8</span><span class="sxs-lookup"><span data-stu-id="67512-122">Windows 8</span></span>              | <span data-ttu-id="67512-123">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="67512-123">SMB 3.0</span></span>     | <span data-ttu-id="67512-124">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-124">Yes</span></span>                 | <span data-ttu-id="67512-125">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-125">Yes</span></span>                 |
| <span data-ttu-id="67512-126">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="67512-126">Windows Server 2012</span></span>    | <span data-ttu-id="67512-127">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="67512-127">SMB 3.0</span></span>     | <span data-ttu-id="67512-128">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-128">Yes</span></span>                 | <span data-ttu-id="67512-129">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-129">Yes</span></span>                 |
| <span data-ttu-id="67512-130">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="67512-130">Windows Server 2012 R2</span></span> | <span data-ttu-id="67512-131">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="67512-131">SMB 3.0</span></span>     | <span data-ttu-id="67512-132">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-132">Yes</span></span>                 | <span data-ttu-id="67512-133">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-133">Yes</span></span>                 |
| <span data-ttu-id="67512-134">Windows 10</span><span class="sxs-lookup"><span data-stu-id="67512-134">Windows 10</span></span>             | <span data-ttu-id="67512-135">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="67512-135">SMB 3.0</span></span>     | <span data-ttu-id="67512-136">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-136">Yes</span></span>                 | <span data-ttu-id="67512-137">Igen</span><span class="sxs-lookup"><span data-stu-id="67512-137">Yes</span></span>                 |

> [!Note]  
> <span data-ttu-id="67512-138">Véve mindig ajánlott legutóbbi KB-os verzióhoz tartozó Windows hello.</span><span class="sxs-lookup"><span data-stu-id="67512-138">We always recommend taking hello most recent KB for your version of Windows.</span></span>

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a><span data-ttu-id="67512-139"></a>Az Azure-fájlmegosztások Windowson történő csatlakoztatásának előfeltételei</span><span class="sxs-lookup"><span data-stu-id="67512-139"></a>Prerequisites for Mounting Azure File Share with Windows</span></span> 
* <span data-ttu-id="67512-140">**A tárfiók neve**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello hello tárfiókja nevére.</span><span class="sxs-lookup"><span data-stu-id="67512-140">**Storage Account Name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="67512-141">**Tárfiók kulcsa**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello elsődleges (vagy másodlagos) storage-kulcs.</span><span class="sxs-lookup"><span data-stu-id="67512-141">**Storage Account Key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="67512-142">Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="67512-142">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="67512-143">**Győződjön meg arról, hogy a 445-ös port nyitva van**: Az Azure File Storage SMB protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="67512-143">**Ensure port 445 is open**: Azure File storage uses SMB protocol.</span></span> <span data-ttu-id="67512-144">Toosee SMB 445 - TCP-porton keresztül kommunikál ellenőrizze, hogy a tűzfal nem blokkolja-e az ügyfélszámítógép 445-ös TCP-portok.</span><span class="sxs-lookup"><span data-stu-id="67512-144">SMB communicates over TCP port 445 - check toosee if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-hello-azure-file-share-with-file-explorer"></a><span data-ttu-id="67512-145">Hello Azure fájlmegosztás Fájlkezelőben csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="67512-145">Mount hello Azure File share with File Explorer</span></span>
> [!Note]  
> <span data-ttu-id="67512-146">Vegye figyelembe, hogy az utasításoknak hello látható a Windows 10 és némileg eltérőek lehetnek a régebbi kiadásokban.</span><span class="sxs-lookup"><span data-stu-id="67512-146">Note that hello following instructions are shown on Windows 10 and may differ slightly on older releases.</span></span> 

1. <span data-ttu-id="67512-147">**Nyissa meg a Fájlkezelőt**: Ez végezhető megnyitása a Start menü hello, vagy Win + E helyi lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="67512-147">**Open File Explorer**: This can be done by opening from hello Start Menu, or by pressing Win+E shortcut.</span></span>

2. <span data-ttu-id="67512-148">**Keresse meg a hello ablak hello bal oldalon toohello "A számítógép" elemre. Ez a művelet módosítja az hello menük hello szalagon érhető el. Az hello számítógép menüben válassza a "Hálózati meghajtó csatlakoztatása"**.</span><span class="sxs-lookup"><span data-stu-id="67512-148">**Navigate toohello "This PC" item on hello left-hand side of hello window. This will change hello menus available in hello ribbon. Under hello Computer menu, select "Map Network Drive"**.</span></span>
    
    ![A képernyőfelvétel a hello "Hálózati meghajtó csatlakoztatása" legördülő menü](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. <span data-ttu-id="67512-150">**Másolás hello UNC elérési út ablaktáblájáról hello "Csatlakozás" hello Azure-portálon**: hogyan toofind ezt az információt található részletes leírását [Itt](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span><span class="sxs-lookup"><span data-stu-id="67512-150">**Copy hello UNC path from hello "Connect" pane in hello Azure portal**: A detailed description of how toofind this information can be found [here](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span></span>

    ![hello UNC elérési út hello Azure File storage Connect panelről](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. <span data-ttu-id="67512-152">**Válasszon hello meghajtóbetűjelet, és írja be a hello UNC elérési utat.**</span><span class="sxs-lookup"><span data-stu-id="67512-152">**Select hello Drive letter and enter hello UNC path.**</span></span> 
    
    ![Egy hello "Hálózati meghajtó csatlakoztatása" párbeszédpanel képernyőképe](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. <span data-ttu-id="67512-154">**$A a Tárfiók nevét használja hello `Azure\` hello felhasználónév és a Tárfiók kulcsa hello jelszóként.**</span><span class="sxs-lookup"><span data-stu-id="67512-154">**Use hello Storage Account Name prepended with `Azure\` as hello username and a Storage Account Key as hello password.**</span></span>
    
    ![Egy hello hálózati hitelesítő párbeszédpanel képernyőképe](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. <span data-ttu-id="67512-156">**Használja az Azure-fájlmegosztást igény szerint**.</span><span class="sxs-lookup"><span data-stu-id="67512-156">**Use Azure File share as desired**.</span></span>
    
    ![Az Azure-fájlmegosztás most már csatlakoztatva van](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. <span data-ttu-id="67512-158">**Készen áll a toodismount (vagy leválasztása) hello Azure fájlmegosztás, ehhez hello bejegyzésre hello megosztás hello "hálózati helyek" a Fájlkezelőben a jobb gombbal kattint rá, majd válassza a "Kapcsolat bontása"**.</span><span class="sxs-lookup"><span data-stu-id="67512-158">**When you are ready toodismount (or disconnect) hello Azure File share, you can do so by right clicking on hello entry for hello share under hello "Network locations" in File Explorer and selecting "Disconnect"**.</span></span>

## <a name="mount-hello-azure-file-share-with-powershell"></a><span data-ttu-id="67512-159">Csatlakoztassa a hello Azure fájlmegosztás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="67512-159">Mount hello Azure File share with PowerShell</span></span>
1. <span data-ttu-id="67512-160">**Használjon hello következő parancsot a toomount hello Azure fájlmegosztás**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="67512-160">**Use hello following command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with hello proper information.</span></span>

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. <span data-ttu-id="67512-161">**Használjon hello Azure fájlmegosztás tetszés szerint**.</span><span class="sxs-lookup"><span data-stu-id="67512-161">**Use hello Azure File share as desired**.</span></span>

3. <span data-ttu-id="67512-162">**Ha elkészült, válassza le a hello Azure fájlmegosztás használata a következő parancs hello**.</span><span class="sxs-lookup"><span data-stu-id="67512-162">**When you are finished, dismount hello Azure File share using hello following command**.</span></span>

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> <span data-ttu-id="67512-163">Hello segítségével `-Persist` paraméter `New-PSDrive` toomake hello Azure File megosztás látható toohello részeinek hello operációs rendszer, amíg csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="67512-163">You may use hello `-Persist` parameter on `New-PSDrive` toomake hello Azure File share visible toohello rest of hello OS while mounted.</span></span>

## <a name="mount-hello-azure-file-share-with-command-prompt"></a><span data-ttu-id="67512-164">Csatlakoztassa a hello Azure fájlmegosztás parancssorral</span><span class="sxs-lookup"><span data-stu-id="67512-164">Mount hello Azure File share with Command Prompt</span></span>
1. <span data-ttu-id="67512-165">**Használjon hello következő parancsot a toomount hello Azure fájlmegosztás**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="67512-165">**Use hello following command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with hello proper information.</span></span>

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. <span data-ttu-id="67512-166">**Használjon hello Azure fájlmegosztás tetszés szerint**.</span><span class="sxs-lookup"><span data-stu-id="67512-166">**Use hello Azure File share as desired**.</span></span>

3. <span data-ttu-id="67512-167">**Ha elkészült, válassza le a hello Azure fájlmegosztás használata a következő parancs hello**.</span><span class="sxs-lookup"><span data-stu-id="67512-167">**When you are finished, dismount hello Azure File share using hello following command**.</span></span>

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> <span data-ttu-id="67512-168">Konfigurálhatja hello Azure File megosztás tooautomatically újracsatlakozás újraindításkor Windows tárolásakor hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="67512-168">You can configure hello Azure File share tooautomatically reconnect on reboot by persisting hello credentials in Windows.</span></span> <span data-ttu-id="67512-169">a következő parancs hello megmaradnak hello hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="67512-169">hello following command will persist hello credentials:</span></span>
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a><span data-ttu-id="67512-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67512-170">Next steps</span></span>
<span data-ttu-id="67512-171">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="67512-171">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="67512-172">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="67512-172">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="67512-173">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="67512-173">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="67512-174">Elméleti cikkek és videók</span><span class="sxs-lookup"><span data-stu-id="67512-174">Conceptual articles and videos</span></span>
* [<span data-ttu-id="67512-175">Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez</span><span class="sxs-lookup"><span data-stu-id="67512-175">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="67512-176">Hogyan toouse Azure File storage Linux</span><span class="sxs-lookup"><span data-stu-id="67512-176">How toouse Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a><span data-ttu-id="67512-177">Azure File Storage-eszköztámogatás</span><span class="sxs-lookup"><span data-stu-id="67512-177">Tooling support for Azure File storage</span></span>
* [<span data-ttu-id="67512-178">Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)</span><span class="sxs-lookup"><span data-stu-id="67512-178">Using Azure PowerShell with Azure Storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="67512-179">Hogyan toouse Microsoft Azure Storage AzCopy</span><span class="sxs-lookup"><span data-stu-id="67512-179">How toouse AzCopy with Microsoft Azure Storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="67512-180">Az Azure Storage hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="67512-180">Using hello Azure CLI with Azure Storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="67512-181">Az Azure File Storage-problémák hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="67512-181">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a><span data-ttu-id="67512-182">Blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="67512-182">Blog posts</span></span>
* [<span data-ttu-id="67512-183">Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)</span><span class="sxs-lookup"><span data-stu-id="67512-183">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="67512-184">Az Azure File Storage ismertetése</span><span class="sxs-lookup"><span data-stu-id="67512-184">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="67512-185">Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)</span><span class="sxs-lookup"><span data-stu-id="67512-185">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="67512-186">Áttelepítési adatok tooAzure fájl</span><span class="sxs-lookup"><span data-stu-id="67512-186">Migrating data tooAzure File </span></span>](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a><span data-ttu-id="67512-187">Referencia</span><span class="sxs-lookup"><span data-stu-id="67512-187">Reference</span></span>
* [<span data-ttu-id="67512-188">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="67512-188">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="67512-189">Referencia a fájlszolgáltatás REST API-jához</span><span class="sxs-lookup"><span data-stu-id="67512-189">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)