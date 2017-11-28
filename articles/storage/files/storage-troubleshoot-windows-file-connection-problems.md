---
title: "A Windows Azure File storage kapcsolatos problémák elhárítása |} Microsoft Docs"
description: "A Windows Azure File storage problémáinak"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: genli
ms.openlocfilehash: daaf7d0589f5e2d82b43dad879bffd23370a2c81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a><span data-ttu-id="b68ae-103">A Windows Azure File storage kapcsolatos problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="b68ae-103">Troubleshoot Azure File storage problems in Windows</span></span>

<span data-ttu-id="b68ae-104">Ez a cikk a Windows-ügyfelek csatlakozás a Microsoft Azure File storage kapcsolatos gyakori problémák sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b68ae-104">This article lists common problems that are related to Microsoft Azure File storage when you connect from Windows clients.</span></span> <span data-ttu-id="b68ae-105">Is biztosít lehetséges okokért és megoldásokért ezeket a problémákat.</span><span class="sxs-lookup"><span data-stu-id="b68ae-105">It also provides possible causes and resolutions for these problems.</span></span> <span data-ttu-id="b68ae-106">Ebben a cikkben a hibaelhárítási mellett is használhatja [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) győződjön meg arról, hogy a Windows ügyfél-környezet van-e a megfelelő előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="b68ae-106">In addition to the troubleshooting steps in this article, you can also use [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) to ensure that the Windows client environment has correct prerequisites.</span></span> <span data-ttu-id="b68ae-107">AzFileDiagnostics automatizálja a jelenség a cikkben említett, és a segítségével állítsa be a környezetet az optimális teljesítmény eléréséhez a legtöbb észlelése.</span><span class="sxs-lookup"><span data-stu-id="b68ae-107">AzFileDiagnostics automates detection of most of the symptoms mentioned in this article and helps set up your environment to get optimal performance.</span></span> <span data-ttu-id="b68ae-108">Ezt az információt is tájékozódhat a [Azure fájlmegosztási hibaelhárító](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) , amelyek segítenek a kapcsolódás/leképezési/csatlakoztatása Azure fájlmegosztási problémák lépéseit.</span><span class="sxs-lookup"><span data-stu-id="b68ae-108">You can also find this information in the [Azure Files shares Troubleshooter](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) that provides steps to assist you with problems connecting/mapping/mounting Azure Files shares.</span></span>


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a><span data-ttu-id="b68ae-109">53-as hiba, a 67-es hiba vagy a hiba 87, amikor csatlakoztatni vagy leválasztani az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="b68ae-109">Error 53, Error 67, or Error 87 when you mount or unmount an Azure file share</span></span>

<span data-ttu-id="b68ae-110">Amikor fájlmegosztást csatlakoztatni a helyszíni vagy egy másik datacenter próbál, akkor fordulhat elő, a következő hibákat:</span><span class="sxs-lookup"><span data-stu-id="b68ae-110">When you try to mount a file share from on-premises or from a different datacenter, you might receive the following errors:</span></span>

- <span data-ttu-id="b68ae-111">Rendszer 53-as hiba.</span><span class="sxs-lookup"><span data-stu-id="b68ae-111">System error 53 has occurred.</span></span> <span data-ttu-id="b68ae-112">A hálózati elérési út nem található.</span><span class="sxs-lookup"><span data-stu-id="b68ae-112">The network path was not found.</span></span>
- <span data-ttu-id="b68ae-113">67-es rendszerhiba történt.</span><span class="sxs-lookup"><span data-stu-id="b68ae-113">System error 67 has occurred.</span></span> <span data-ttu-id="b68ae-114">A hálózatnév nem található.</span><span class="sxs-lookup"><span data-stu-id="b68ae-114">The network name cannot be found.</span></span>
- <span data-ttu-id="b68ae-115">87 rendszerhiba történt.</span><span class="sxs-lookup"><span data-stu-id="b68ae-115">System error 87 has occurred.</span></span> <span data-ttu-id="b68ae-116">A paraméter nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="b68ae-116">The parameter is incorrect.</span></span>

### <a name="cause-1-unencrypted-communication-channel"></a><span data-ttu-id="b68ae-117">1. ok: Titkosítatlan kommunikációs csatorna</span><span class="sxs-lookup"><span data-stu-id="b68ae-117">Cause 1: Unencrypted communication channel</span></span>

<span data-ttu-id="b68ae-118">Biztonsági okokból Azure fájlmegosztásokat kapcsolatok sem, ha a kommunikációs csatorna nincs titkosítva, és ha a kapcsolódás nem ugyanabban az adatközpontban a Azure fájlmegosztásokat tároló.</span><span class="sxs-lookup"><span data-stu-id="b68ae-118">For security reasons, connections to Azure file shares are blocked if the communication channel isn’t encrypted and if the connection attempt isn't made from the same datacenter where the Azure file shares reside.</span></span> <span data-ttu-id="b68ae-119">Csak akkor, ha a felhasználó ügyfél operációs rendszer támogatja az SMB-titkosítás a kommunikációs csatorna titkosítást biztosítja.</span><span class="sxs-lookup"><span data-stu-id="b68ae-119">Communication channel encryption is provided only if the user’s client OS supports SMB encryption.</span></span>

<span data-ttu-id="b68ae-120">Windows 8, Windows Server 2012 és egyes rendszerek újabb verzióit egyezteti az SMB 3.0-s, amely támogatja a titkosítást kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="b68ae-120">Windows 8, Windows Server 2012, and later versions of each system negotiate requests that include SMB 3.0, which supports encryption.</span></span>

### <a name="solution-for-cause-1"></a><span data-ttu-id="b68ae-121">Megoldás ok 1</span><span class="sxs-lookup"><span data-stu-id="b68ae-121">Solution for cause 1</span></span>

<span data-ttu-id="b68ae-122">Csatlakozás egy ügyfél, amely az alábbi:</span><span class="sxs-lookup"><span data-stu-id="b68ae-122">Connect from a client that does one of the following:</span></span>

- <span data-ttu-id="b68ae-123">Megfelel a Windows 8 és Windows Server 2012 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="b68ae-123">Meets the requirements of Windows 8 and Windows Server 2012 or later versions</span></span>
- <span data-ttu-id="b68ae-124">A virtuális gép ugyanabban az adatközpontban, az Azure storage-fiók Azure fájlmegosztás használt csatlakozik</span><span class="sxs-lookup"><span data-stu-id="b68ae-124">Connects from a virtual machine in the same datacenter as the Azure storage account that is used for the Azure file share</span></span>

### <a name="cause-2-port-445-is-blocked"></a><span data-ttu-id="b68ae-125">2. ok: 445-ös Port blokkolva van</span><span class="sxs-lookup"><span data-stu-id="b68ae-125">Cause 2: Port 445 is blocked</span></span>

<span data-ttu-id="b68ae-126">Rendszerhiba 53-as vagy rendszerhiba 67 akkor fordulhat elő, ha az Azure File storage adatközpontba kimenő kommunikáció 445-ös port blokkolva van.</span><span class="sxs-lookup"><span data-stu-id="b68ae-126">System error 53 or system error 67 can occur if port 445 outbound communication to an Azure File storage datacenter is blocked.</span></span> <span data-ttu-id="b68ae-127">Szolgáltatók, amely a 445-ös porton elérésének engedélyezése vagy letiltása az összegzés megtekintéséhez keresse fel [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span><span class="sxs-lookup"><span data-stu-id="b68ae-127">To see the summary of ISPs that allow or disallow access from port 445, go to [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span></span>

<span data-ttu-id="b68ae-128">Ha szeretné megtudni, hogy ez-e a "System error 53" üzenet mögött OK, Portqry segítségével lekérdezni a TCP:445 végpont.</span><span class="sxs-lookup"><span data-stu-id="b68ae-128">To understand whether this is the reason behind the "System error 53" message, you can use Portqry to query the TCP:445 endpoint.</span></span> <span data-ttu-id="b68ae-129">A TCP:445 végpont szűrt jelenik meg, ha blokkolva van, az TCP-portot.</span><span class="sxs-lookup"><span data-stu-id="b68ae-129">If the TCP:445 endpoint is displayed as filtered, the TCP port is blocked.</span></span> <span data-ttu-id="b68ae-130">Íme egy példa lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="b68ae-130">Here is an example query:</span></span>

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

<span data-ttu-id="b68ae-131">Ha egy szabály a hálózati elérési út mentén blokkolja a 445-ös TCP-portot, a következő eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b68ae-131">If TCP port 445 is blocked by a rule along the network path, you will see the following output:</span></span>

  `TCP port 445 (microsoft-ds service): FILTERED`

<span data-ttu-id="b68ae-132">Portqry használatával kapcsolatos további információkért lásd: [a Portqry.exe parancssori segédprogram](https://support.microsoft.com/help/310099).</span><span class="sxs-lookup"><span data-stu-id="b68ae-132">For more information about how to use Portqry, see [Description of the Portqry.exe command-line utility](https://support.microsoft.com/help/310099).</span></span>

### <a name="solution-for-cause-2"></a><span data-ttu-id="b68ae-133">Megoldás ok 2</span><span class="sxs-lookup"><span data-stu-id="b68ae-133">Solution for cause 2</span></span>

<span data-ttu-id="b68ae-134">Az informatikai részleg nyissa meg a kimenő 445-ös porton való együttműködésre [Azure IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="b68ae-134">Work with your IT department to open port 445 outbound to [Azure IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

### <a name="cause-3-ntlmv1-is-enabled"></a><span data-ttu-id="b68ae-135">3. ok: NTLMv1 engedélyezve</span><span class="sxs-lookup"><span data-stu-id="b68ae-135">Cause 3: NTLMv1 is enabled</span></span>

<span data-ttu-id="b68ae-136">Rendszerhiba 53-as vagy rendszerhiba 87 akkor fordulhat elő, ha NTLMv1 kommunikáció engedélyezve van az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="b68ae-136">System error 53 or system error 87 can occur if NTLMv1 communication is enabled on the client.</span></span> <span data-ttu-id="b68ae-137">Az Azure File storage csak NTLMv2-alapú hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="b68ae-137">Azure File storage supports only NTLMv2 authentication.</span></span> <span data-ttu-id="b68ae-138">Egy kevésbé biztonságos ügyfél engedélyezett NTLMv1 rendelkező hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b68ae-138">Having NTLMv1 enabled creates a less-secure client.</span></span> <span data-ttu-id="b68ae-139">Ezért kommunikációja blokkolva van az Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="b68ae-139">Therefore, communication is blocked for Azure File storage.</span></span> 

<span data-ttu-id="b68ae-140">Annak megállapításához, hogy ez-e a hiba okának, ellenőrizze, hogy az alábbi beállításjegyzékbeli alkulcs 3 értéket:</span><span class="sxs-lookup"><span data-stu-id="b68ae-140">To determine whether this is the cause of the error, verify that the following registry subkey is set to a value of 3:</span></span>

<span data-ttu-id="b68ae-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span><span class="sxs-lookup"><span data-stu-id="b68ae-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span></span>

<span data-ttu-id="b68ae-142">További információkért lásd: a [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) a témakör a TechNet webhelyen.</span><span class="sxs-lookup"><span data-stu-id="b68ae-142">For more information, see the [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) topic on TechNet.</span></span>

### <a name="solution-for-cause-3"></a><span data-ttu-id="b68ae-143">Megoldás ok 3</span><span class="sxs-lookup"><span data-stu-id="b68ae-143">Solution for cause 3</span></span>

<span data-ttu-id="b68ae-144">Állítsa vissza a **LmCompatibilityLevel** az alapértelmezett érték a következő beállításkulcsban 3 értéket:</span><span class="sxs-lookup"><span data-stu-id="b68ae-144">Revert the **LmCompatibilityLevel** value to the default value of 3 in the following registry subkey:</span></span>

  <span data-ttu-id="b68ae-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span><span class="sxs-lookup"><span data-stu-id="b68ae-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span></span>

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a><span data-ttu-id="b68ae-146">Hiba 1816 "nincs elegendő kvótája: Ez a parancs" Azure-fájlmegosztásra másolásakor</span><span class="sxs-lookup"><span data-stu-id="b68ae-146">Error 1816 “Not enough quota is available to process this command” when you copy to an Azure file share</span></span>

### <a name="cause"></a><span data-ttu-id="b68ae-147">Ok</span><span class="sxs-lookup"><span data-stu-id="b68ae-147">Cause</span></span>

<span data-ttu-id="b68ae-148">1816 hiba akkor fordul elő, amikor elérte a felső korlátja egyidejű megnyitott leíróinak egy fájlt a számítógépen, amelyen a fájlmegosztás csatlakoztatva számára engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b68ae-148">Error 1816 happens when you reach the upper limit of concurrent open handles that are allowed for a file on the computer where the file share is being mounted.</span></span>

### <a name="solution"></a><span data-ttu-id="b68ae-149">Megoldás</span><span class="sxs-lookup"><span data-stu-id="b68ae-149">Solution</span></span>

<span data-ttu-id="b68ae-150">Adjon meg kevesebb egyidejű megnyitott leíróinak néhány leírók bezárásával, majd próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="b68ae-150">Reduce the number of concurrent open handles by closing some handles, and then retry.</span></span> <span data-ttu-id="b68ae-151">További információkért lásd: [Microsoft Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b68ae-151">For more information, see [Microsoft Azure Storage performance and scalability checklist](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-windows"></a><span data-ttu-id="b68ae-152">Fájl másolása és az Azure File storage Windows lassú</span><span class="sxs-lookup"><span data-stu-id="b68ae-152">Slow file copying to and from Azure File storage in Windows</span></span>

<span data-ttu-id="b68ae-153">Lassú teljesítmény fájlok átvitele az Azure File service megkísérlésekor jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="b68ae-153">You might see slow performance when you try to transfer files to the Azure File service.</span></span>

- <span data-ttu-id="b68ae-154">Ha nem rendelkezik egy adott minimális i/o vonatkozó méretbeli követelményt, ajánlott, hogy 1 MB, az I/O méretéről az optimális teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="b68ae-154">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as the I/O size for optimal performance.</span></span>
-   <span data-ttu-id="b68ae-155">Ha ismeri a végleges mérethez, amelyet az írási műveletek kiterjesztése, és a szoftver nincs kompatibilitási problémákat, ha a fájl unwritten utóhívás nullákat tartalmaz, majd állítsa be a fájlméret, így minden egyes egy kibővítése írási helyett előre.</span><span class="sxs-lookup"><span data-stu-id="b68ae-155">If you know the final size of a file that you are extending with writes, and your software doesn’t have compatibility problems when the unwritten tail on the file contains zeros, then set the file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="b68ae-156">A jobb oldali copy metódust használja:</span><span class="sxs-lookup"><span data-stu-id="b68ae-156">Use the right copy method:</span></span>
    -   <span data-ttu-id="b68ae-157">Használjon [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) bármely két fájlmegosztások közötti átvitel céljából.</span><span class="sxs-lookup"><span data-stu-id="b68ae-157">Use [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="b68ae-158">Használjon [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) közötti fájlmegosztások a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b68ae-158">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a><span data-ttu-id="b68ae-159">Windows 8.1 vagy Windows Server 2012 R2 szempontjai</span><span class="sxs-lookup"><span data-stu-id="b68ae-159">Considerations for Windows 8.1 or Windows Server 2012 R2</span></span>

<span data-ttu-id="b68ae-160">Windows 8.1 vagy Windows Server 2012 R2 rendszert futtató ügyfeleken győződjön meg arról, hogy a [KB3114025](https://support.microsoft.com/help/3114025) gyorsjavítás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="b68ae-160">For clients that are running Windows 8.1 or Windows Server 2012 R2, make sure that the [KB3114025](https://support.microsoft.com/help/3114025) hotfix is installed.</span></span> <span data-ttu-id="b68ae-161">Ez a gyorsjavítás javítja a teljesítményt a Create, és zárja be a kezeli.</span><span class="sxs-lookup"><span data-stu-id="b68ae-161">This hotfix improves the performance of create and close handles.</span></span>

<span data-ttu-id="b68ae-162">Ellenőrizze, hogy a gyorsjavítás telepítve van-e a következő parancsfájl futtatásával is:</span><span class="sxs-lookup"><span data-stu-id="b68ae-162">You can run the following script to check whether the hotfix has been installed:</span></span>

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

<span data-ttu-id="b68ae-163">Ha a gyorsjavítás telepítve van, a következő eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b68ae-163">If hotfix is installed, the following output is displayed:</span></span>

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> <span data-ttu-id="b68ae-164">Windows Server 2012 R2 Azure piactéren képeknek gyorsjavítás KB3114025 alapértelmezés szerint telepítve 2015. decemberi kezdve.</span><span class="sxs-lookup"><span data-stu-id="b68ae-164">Windows Server 2012 R2 images in Azure Marketplace have hotfix KB3114025 installed by default, starting in December 2015.</span></span>

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a><span data-ttu-id="b68ae-165">Ne legyen a meghajtóbetűjel mappa **Sajátgép**</span><span class="sxs-lookup"><span data-stu-id="b68ae-165">No folder with a drive letter in **My Computer**</span></span>

<span data-ttu-id="b68ae-166">Ha az az Azure fájlmegosztások rendszergazdaként a nettó használja, a megosztás úgy tűnik, hogy hiányzik.</span><span class="sxs-lookup"><span data-stu-id="b68ae-166">If you map an Azure file share as an administrator by using net use, the share appears to be missing.</span></span>

### <a name="cause"></a><span data-ttu-id="b68ae-167">Ok</span><span class="sxs-lookup"><span data-stu-id="b68ae-167">Cause</span></span>

<span data-ttu-id="b68ae-168">Alapértelmezés szerint a Windows Fájlkezelőben nem futtassa rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b68ae-168">By default, Windows File Explorer does not run as an administrator.</span></span> <span data-ttu-id="b68ae-169">Ha nettó használata futtatja a parancsot egy rendszergazdai parancssort, akkor a hálózati meghajtó meghajtó csatlakoztatása rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b68ae-169">If you run net use from an administrative command prompt, you map the network drive as an administrator.</span></span> <span data-ttu-id="b68ae-170">Mivel a csatlakoztatott meghajtók felhasználó-központú, van-e bejelentkezett felhasználói fiók nem jelenik meg a meghajtók ha csatlakoztatva vannak egy másik felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b68ae-170">Because mapped drives are user-centric, the user account that is logged in does not display the drives if they are mounted under a different user account.</span></span>

### <a name="solution"></a><span data-ttu-id="b68ae-171">Megoldás</span><span class="sxs-lookup"><span data-stu-id="b68ae-171">Solution</span></span>
<span data-ttu-id="b68ae-172">A megosztás csatlakoztatásához a nem rendszergazdai parancssorból.</span><span class="sxs-lookup"><span data-stu-id="b68ae-172">Mount the share from a non-administrator command line.</span></span> <span data-ttu-id="b68ae-173">Azt is megteheti, amelyeket követve [a TechNet témakör](https://technet.microsoft.com/library/ee844140.aspx) konfigurálása a **EnableLinkedConnections** beállításazonosítót.</span><span class="sxs-lookup"><span data-stu-id="b68ae-173">Alternatively, you can follow [this TechNet topic](https://technet.microsoft.com/library/ee844140.aspx) to configure the **EnableLinkedConnections** registry value.</span></span>

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a><span data-ttu-id="b68ae-174">Net use parancs sikertelen lesz, ha a tárfiók perjelet tartalmaz</span><span class="sxs-lookup"><span data-stu-id="b68ae-174">Net use command fails if the storage account contains a forward slash</span></span>

### <a name="cause"></a><span data-ttu-id="b68ae-175">Ok</span><span class="sxs-lookup"><span data-stu-id="b68ae-175">Cause</span></span>

<span data-ttu-id="b68ae-176">A net use parancs perjellel (/) értelmezi a parancssori kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="b68ae-176">The net use command interprets a forward slash (/) as a command-line option.</span></span> <span data-ttu-id="b68ae-177">Ha a felhasználói fiók nevét perjellel kezdődik, a meghajtó hozzárendelése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="b68ae-177">If your user account name starts with a forward slash, the drive mapping fails.</span></span>

### <a name="solution"></a><span data-ttu-id="b68ae-178">Megoldás</span><span class="sxs-lookup"><span data-stu-id="b68ae-178">Solution</span></span>

<span data-ttu-id="b68ae-179">A probléma megoldásához használhatja az alábbi lépések egyikét:</span><span class="sxs-lookup"><span data-stu-id="b68ae-179">You can use either of the following steps to work around the problem:</span></span>

- <span data-ttu-id="b68ae-180">Futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="b68ae-180">Run the following PowerShell command:</span></span>

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  <span data-ttu-id="b68ae-181">Egy parancsfájlból futtathatja a parancs így:</span><span class="sxs-lookup"><span data-stu-id="b68ae-181">From a batch file, you can run the command this way:</span></span>

  `Echo new-smbMapping ... | powershell -command –`

- <span data-ttu-id="b68ae-182">Helyezze a kulcsot a probléma megoldásához--dupla idézőjelek közé, kivéve, ha a perjel karaktert.</span><span class="sxs-lookup"><span data-stu-id="b68ae-182">Put double quotation marks around the key to work around this problem--unless the forward slash is the first character.</span></span> <span data-ttu-id="b68ae-183">Ha igen, használja az interaktív módban, és külön-külön írja be a jelszót, vagy újragenerálja a kulcsokat nem perjellel kezdődik a kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="b68ae-183">If it is, either use the interactive mode and enter your password separately or regenerate your keys to get a key that doesn't start with a forward slash.</span></span>

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a><span data-ttu-id="b68ae-184">Alkalmazás vagy szolgáltatás nem tud hozzáférni az Azure File storage meghajtóként</span><span class="sxs-lookup"><span data-stu-id="b68ae-184">Application or service cannot access a mounted Azure File storage drive</span></span>

### <a name="cause"></a><span data-ttu-id="b68ae-185">Ok</span><span class="sxs-lookup"><span data-stu-id="b68ae-185">Cause</span></span>

<span data-ttu-id="b68ae-186">Felhasználónként csatlakoztatott meghajtók.</span><span class="sxs-lookup"><span data-stu-id="b68ae-186">Drives are mounted per user.</span></span> <span data-ttu-id="b68ae-187">Ha az alkalmazás vagy szolgáltatás, hogy a meghajtó csatlakoztatott egy másik felhasználói fiók alatt fut, az alkalmazás nem jelenik meg a meghajtót.</span><span class="sxs-lookup"><span data-stu-id="b68ae-187">If your application or service is running under a different user account than the one that mounted the drive, the application will not see the drive.</span></span>

### <a name="solution"></a><span data-ttu-id="b68ae-188">Megoldás</span><span class="sxs-lookup"><span data-stu-id="b68ae-188">Solution</span></span>

<span data-ttu-id="b68ae-189">Használja az alábbi megoldások valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="b68ae-189">Use one of the following solutions:</span></span>

-   <span data-ttu-id="b68ae-190">Ugyanazzal a fiókkal, amely tartalmazza az alkalmazás csatlakoztathatja a meghajtót.</span><span class="sxs-lookup"><span data-stu-id="b68ae-190">Mount the drive from the same user account that contains the application.</span></span> <span data-ttu-id="b68ae-191">Használhatja például a PsExec eszköz.</span><span class="sxs-lookup"><span data-stu-id="b68ae-191">You can use a tool such as PsExec.</span></span>
- <span data-ttu-id="b68ae-192">A tárfiók nevét és a kulcs továbbítja a felhasználó nevében, és a password paraméter a háló paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b68ae-192">Pass the storage account name and key in the user name and password parameters of the net use command.</span></span>

<span data-ttu-id="b68ae-193">Kövesse ezeket az utasításokat, miután nettó használja a rendszer és a hálózati szolgáltatás fiók futtatásakor fordulhat elő a következő hibaüzenet: "1312 rendszerhiba történt.</span><span class="sxs-lookup"><span data-stu-id="b68ae-193">After you follow these instructions, you might receive the following error message when you run net use for the system/network service account: "System error 1312 has occurred.</span></span> <span data-ttu-id="b68ae-194">A megadott bejelentkezési munkamenet nem létezik.</span><span class="sxs-lookup"><span data-stu-id="b68ae-194">A specified logon session does not exist.</span></span> <span data-ttu-id="b68ae-195">Azt is, hogy már befejeződött."</span><span class="sxs-lookup"><span data-stu-id="b68ae-195">It may already have been terminated."</span></span> <span data-ttu-id="b68ae-196">Ha ez történik, ügyeljen arra, hogy a felhasználónév nettó használata átadott tartományadatokat (például: "[tárfiók neve]. file.core.windows .net").</span><span class="sxs-lookup"><span data-stu-id="b68ae-196">If this occurs, make sure that the username that is passed to net use includes domain information (for example: "[storage account name].file.core.windows.net").</span></span>

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a><span data-ttu-id="b68ae-197">"Másolja át a fájlt, amely nem támogatja a titkosítást egy célra" hiba</span><span class="sxs-lookup"><span data-stu-id="b68ae-197">Error "You are copying a file to a destination that does not support encryption"</span></span>

<span data-ttu-id="b68ae-198">A hálózaton keresztül másolja a fájlt, ha a fájl visszafejtése a forrásszámítógépen, nem titkosított szövegként továbbított, és újból titkosítja a célhelyen.</span><span class="sxs-lookup"><span data-stu-id="b68ae-198">When a file is copied over the network, the file is decrypted on the source computer, transmitted in plaintext, and re-encrypted at the destination.</span></span> <span data-ttu-id="b68ae-199">Azonban megjelenhet a következő hiba történt egy titkosított fájl másolása közben: "A rendszer másolja a fájlt egy cél, amely nem támogatja a titkosítást."</span><span class="sxs-lookup"><span data-stu-id="b68ae-199">However, you might see the following error when you're trying to copy an encrypted file: "You are copying the file to a destination that does not support encryption."</span></span>

### <a name="cause"></a><span data-ttu-id="b68ae-200">Ok</span><span class="sxs-lookup"><span data-stu-id="b68ae-200">Cause</span></span>
<span data-ttu-id="b68ae-201">Ez a probléma akkor fordulhat elő, ha titkosított fájlrendszer (EFS) használja.</span><span class="sxs-lookup"><span data-stu-id="b68ae-201">This problem can occur if you are using Encrypting File System (EFS).</span></span> <span data-ttu-id="b68ae-202">A BitLocker által titkosított fájlok lehet másolni az Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="b68ae-202">BitLocker-encrypted files can be copied to Azure File storage.</span></span> <span data-ttu-id="b68ae-203">Azure File storage azonban nem támogatja az NTFS EFS.</span><span class="sxs-lookup"><span data-stu-id="b68ae-203">However, Azure File storage does not support NTFS EFS.</span></span>

### <a name="workaround"></a><span data-ttu-id="b68ae-204">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="b68ae-204">Workaround</span></span>
<span data-ttu-id="b68ae-205">Fájl másolása a hálózaton keresztül, akkor először vissza kell fejtenie azt.</span><span class="sxs-lookup"><span data-stu-id="b68ae-205">To copy a file over the network, you must first decrypt it.</span></span> <span data-ttu-id="b68ae-206">Az alábbi módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="b68ae-206">Use one of the following methods:</span></span>

- <span data-ttu-id="b68ae-207">Használja a **/d másolása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b68ae-207">Use the **copy /d** command.</span></span> <span data-ttu-id="b68ae-208">Lehetővé teszi a titkosított fájlokat, mint a célhely visszafejtet fájlokat menteni.</span><span class="sxs-lookup"><span data-stu-id="b68ae-208">It allows the encrypted files to be saved as decrypted files at the destination.</span></span>
- <span data-ttu-id="b68ae-209">Állítsa be a következő beállításkulcsot:</span><span class="sxs-lookup"><span data-stu-id="b68ae-209">Set the following registry key:</span></span>
  - <span data-ttu-id="b68ae-210">Elérési út = HKLM\Software\Policies\Microsoft\Windows\System</span><span class="sxs-lookup"><span data-stu-id="b68ae-210">Path = HKLM\Software\Policies\Microsoft\Windows\System</span></span>
  - <span data-ttu-id="b68ae-211">Értéktípus = DWORD</span><span class="sxs-lookup"><span data-stu-id="b68ae-211">Value type = DWORD</span></span>
  - <span data-ttu-id="b68ae-212">Name = CopyFileAllowDecryptedRemoteDestination</span><span class="sxs-lookup"><span data-stu-id="b68ae-212">Name = CopyFileAllowDecryptedRemoteDestination</span></span>
  - <span data-ttu-id="b68ae-213">Érték = 1</span><span class="sxs-lookup"><span data-stu-id="b68ae-213">Value = 1</span></span>

<span data-ttu-id="b68ae-214">Vegye figyelembe, hogy a beállításkulcs megadása hatással van minden, hálózati megosztások végrehajtott másolási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="b68ae-214">Be aware that setting the registry key affects all copy operations that are made to network shares.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="b68ae-215">Segítség</span><span class="sxs-lookup"><span data-stu-id="b68ae-215">Need help?</span></span> <span data-ttu-id="b68ae-216">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="b68ae-216">Contact support.</span></span>
<span data-ttu-id="b68ae-217">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a gyorsan megoldott probléma eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b68ae-217">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your problem resolved quickly.</span></span>
