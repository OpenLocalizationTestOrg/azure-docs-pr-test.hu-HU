---
title: "aaaTroubleshoot Azure File storage problémák a Windows rendszerben |} Microsoft Docs"
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
ms.openlocfilehash: ba0315d1add76a27fec93a9aee3aa99ccb7fa164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a><span data-ttu-id="9eb7c-103">A Windows Azure File storage kapcsolatos problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="9eb7c-103">Troubleshoot Azure File storage problems in Windows</span></span>

<span data-ttu-id="9eb7c-104">A cikk ismerteti a gyakori problémák, amelyek kapcsolódó tooMicrosoft Azure File storage Windows-ügyfelek csatlakozásakor.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-104">This article lists common problems that are related tooMicrosoft Azure File storage when you connect from Windows clients.</span></span> <span data-ttu-id="9eb7c-105">Is biztosít lehetséges okokért és megoldásokért ezeket a problémákat.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-105">It also provides possible causes and resolutions for these problems.</span></span> <span data-ttu-id="9eb7c-106">Továbbá ebben a cikkben toohello hibaelhárítási lépések, is [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) győződjön meg arról, hogy a Windows hello ügyfél környezet számára megfelelő előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-106">In addition toohello troubleshooting steps in this article, you can also use [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) to ensure that hello Windows client environment has correct prerequisites.</span></span> <span data-ttu-id="9eb7c-107">AzFileDiagnostics automatizálja a cikkben említett hello tünetei és állítsa be a környezet tooget optimális teljesítményt nyújt segítséget a legtöbb észlelése.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-107">AzFileDiagnostics automates detection of most of hello symptoms mentioned in this article and helps set up your environment tooget optimal performance.</span></span> <span data-ttu-id="9eb7c-108">Ez az információ is található hello [Azure fájlmegosztási hibaelhárító](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) lépéseket tooassist biztosít problémák való csatlakozás/leképezési/csatlakoztatása Azure fájlmegosztási.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-108">You can also find this information in hello [Azure Files shares Troubleshooter](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) that provides steps tooassist you with problems connecting/mapping/mounting Azure Files shares.</span></span>


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a><span data-ttu-id="9eb7c-109">53-as hiba, a 67-es hiba vagy a hiba 87, amikor csatlakoztatni vagy leválasztani az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="9eb7c-109">Error 53, Error 67, or Error 87 when you mount or unmount an Azure file share</span></span>

<span data-ttu-id="9eb7c-110">Amikor egy fájlmegosztás a helyszíni vagy egy másik datacenter toomount, akkor fordulhat elő, a következő hibák hello:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-110">When you try toomount a file share from on-premises or from a different datacenter, you might receive hello following errors:</span></span>

- <span data-ttu-id="9eb7c-111">Rendszer 53-as hiba.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-111">System error 53 has occurred.</span></span> <span data-ttu-id="9eb7c-112">hello hálózati elérési út nem található.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-112">hello network path was not found.</span></span>
- <span data-ttu-id="9eb7c-113">67-es rendszerhiba történt.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-113">System error 67 has occurred.</span></span> <span data-ttu-id="9eb7c-114">hello hálózatnév nem található.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-114">hello network name cannot be found.</span></span>
- <span data-ttu-id="9eb7c-115">87 rendszerhiba történt.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-115">System error 87 has occurred.</span></span> <span data-ttu-id="9eb7c-116">hello paraméter nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-116">hello parameter is incorrect.</span></span>

### <a name="cause-1-unencrypted-communication-channel"></a><span data-ttu-id="9eb7c-117">1. ok: Titkosítatlan kommunikációs csatorna</span><span class="sxs-lookup"><span data-stu-id="9eb7c-117">Cause 1: Unencrypted communication channel</span></span>

<span data-ttu-id="9eb7c-118">Biztonsági okokból tooAzure fájlmegosztások le vannak tiltva, ha hello kommunikációs csatorna nincs titkosítva, és ha hello kapcsolódási kísérlet nem kapcsolatok hello hello Azure fájlmegosztásokat tároló ugyanabban az adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-118">For security reasons, connections tooAzure file shares are blocked if hello communication channel isn’t encrypted and if hello connection attempt isn't made from hello same datacenter where hello Azure file shares reside.</span></span> <span data-ttu-id="9eb7c-119">Csak akkor, ha hello felhasználói ügyfél operációs rendszer támogatja az SMB-titkosítás a kommunikációs csatorna titkosítást biztosítja.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-119">Communication channel encryption is provided only if hello user’s client OS supports SMB encryption.</span></span>

<span data-ttu-id="9eb7c-120">Windows 8, Windows Server 2012 és egyes rendszerek újabb verzióit egyezteti az SMB 3.0-s, amely támogatja a titkosítást kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-120">Windows 8, Windows Server 2012, and later versions of each system negotiate requests that include SMB 3.0, which supports encryption.</span></span>

### <a name="solution-for-cause-1"></a><span data-ttu-id="9eb7c-121">Megoldás ok 1</span><span class="sxs-lookup"><span data-stu-id="9eb7c-121">Solution for cause 1</span></span>

<span data-ttu-id="9eb7c-122">Csatlakozás, amelyet hello következő ügyfélről:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-122">Connect from a client that does one of hello following:</span></span>

- <span data-ttu-id="9eb7c-123">Hello megfelel a Windows 8 és Windows Server 2012 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="9eb7c-123">Meets hello requirements of Windows 8 and Windows Server 2012 or later versions</span></span>
- <span data-ttu-id="9eb7c-124">Csatlakoztatja a virtuális gép a hello azonos módon hello Azure fájlmegosztás használt Azure storage-fiók hello datacenter</span><span class="sxs-lookup"><span data-stu-id="9eb7c-124">Connects from a virtual machine in hello same datacenter as hello Azure storage account that is used for hello Azure file share</span></span>

### <a name="cause-2-port-445-is-blocked"></a><span data-ttu-id="9eb7c-125">2. ok: 445-ös Port blokkolva van</span><span class="sxs-lookup"><span data-stu-id="9eb7c-125">Cause 2: Port 445 is blocked</span></span>

<span data-ttu-id="9eb7c-126">Rendszerhiba 53-as vagy rendszerhiba 67 akkor fordulhat elő, ha port 445-ös kimenő kommunikáció tooan Azure File storage datacenter le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-126">System error 53 or system error 67 can occur if port 445 outbound communication tooan Azure File storage datacenter is blocked.</span></span> <span data-ttu-id="9eb7c-127">toosee hello összegzését, 445-ös portot a elérésének engedélyezése vagy letiltása az internetszolgáltatók lépjen túl[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span><span class="sxs-lookup"><span data-stu-id="9eb7c-127">toosee hello summary of ISPs that allow or disallow access from port 445, go too[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span></span>

<span data-ttu-id="9eb7c-128">toounderstand Ez oka hello "System error 53" üdvözlőüzenetére mögött, hogy használhatja-e Portqry tooquery hello TCP:445 végpont.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-128">toounderstand whether this is hello reason behind hello "System error 53" message, you can use Portqry tooquery hello TCP:445 endpoint.</span></span> <span data-ttu-id="9eb7c-129">Hello TCP:445 végpont szűrt jelenik meg, ha a TCP-port hello le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-129">If hello TCP:445 endpoint is displayed as filtered, hello TCP port is blocked.</span></span> <span data-ttu-id="9eb7c-130">Íme egy példa lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-130">Here is an example query:</span></span>

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

<span data-ttu-id="9eb7c-131">Ha hello hálózati elérési út mentén szabály blokkolja a 445-ös TCP-portot, a következő kimeneti hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-131">If TCP port 445 is blocked by a rule along hello network path, you will see hello following output:</span></span>

  `TCP port 445 (microsoft-ds service): FILTERED`

<span data-ttu-id="9eb7c-132">További információ toouse Portqry, lásd: [hello Portqry.exe parancssori segédprogram](https://support.microsoft.com/help/310099).</span><span class="sxs-lookup"><span data-stu-id="9eb7c-132">For more information about how toouse Portqry, see [Description of hello Portqry.exe command-line utility](https://support.microsoft.com/help/310099).</span></span>

### <a name="solution-for-cause-2"></a><span data-ttu-id="9eb7c-133">Megoldás ok 2</span><span class="sxs-lookup"><span data-stu-id="9eb7c-133">Solution for cause 2</span></span>

<span data-ttu-id="9eb7c-134">Az informatikai részleg tooopen 445-ös porton kimenő túl együttműködve[Azure IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="9eb7c-134">Work with your IT department tooopen port 445 outbound too[Azure IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

### <a name="cause-3-ntlmv1-is-enabled"></a><span data-ttu-id="9eb7c-135">3. ok: NTLMv1 engedélyezve</span><span class="sxs-lookup"><span data-stu-id="9eb7c-135">Cause 3: NTLMv1 is enabled</span></span>

<span data-ttu-id="9eb7c-136">Rendszerhiba 53-as vagy rendszerhiba 87 akkor fordulhat elő, ha NTLMv1 kommunikációs hello ügyfélen engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-136">System error 53 or system error 87 can occur if NTLMv1 communication is enabled on hello client.</span></span> <span data-ttu-id="9eb7c-137">Az Azure File storage csak NTLMv2-alapú hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-137">Azure File storage supports only NTLMv2 authentication.</span></span> <span data-ttu-id="9eb7c-138">Egy kevésbé biztonságos ügyfél engedélyezett NTLMv1 rendelkező hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-138">Having NTLMv1 enabled creates a less-secure client.</span></span> <span data-ttu-id="9eb7c-139">Ezért kommunikációja blokkolva van az Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-139">Therefore, communication is blocked for Azure File storage.</span></span> 

<span data-ttu-id="9eb7c-140">toodetermine Ez az hello OK hello hibát, hogy ellenőrizze, hogy a következő beállításkulcsot hello értéke 3 tooa értékének:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-140">toodetermine whether this is hello cause of hello error, verify that hello following registry subkey is set tooa value of 3:</span></span>

<span data-ttu-id="9eb7c-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span><span class="sxs-lookup"><span data-stu-id="9eb7c-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span></span>

<span data-ttu-id="9eb7c-142">További információkért lásd: hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) a témakör a TechNet webhelyen.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-142">For more information, see hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) topic on TechNet.</span></span>

### <a name="solution-for-cause-3"></a><span data-ttu-id="9eb7c-143">Megoldás ok 3</span><span class="sxs-lookup"><span data-stu-id="9eb7c-143">Solution for cause 3</span></span>

<span data-ttu-id="9eb7c-144">Állítsa vissza a hello **LmCompatibilityLevel** toohello alapértelmezett értéke a következő beállításkulcsot hello 3 értéket:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-144">Revert hello **LmCompatibilityLevel** value toohello default value of 3 in hello following registry subkey:</span></span>

  <span data-ttu-id="9eb7c-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span><span class="sxs-lookup"><span data-stu-id="9eb7c-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span></span>

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-tooprocess-this-command-when-you-copy-tooan-azure-file-share"></a><span data-ttu-id="9eb7c-146">Hiba 1816 "nincs elegendő kvótája a parancs van a rendelkezésre álló tooprocess" tooan Azure fájlmegosztás másolásakor</span><span class="sxs-lookup"><span data-stu-id="9eb7c-146">Error 1816 “Not enough quota is available tooprocess this command” when you copy tooan Azure file share</span></span>

### <a name="cause"></a><span data-ttu-id="9eb7c-147">Ok</span><span class="sxs-lookup"><span data-stu-id="9eb7c-147">Cause</span></span>

<span data-ttu-id="9eb7c-148">1816 hiba akkor fordul elő, amikor elérkezik egy fájlt hello számítógépen, amelyen hello fájlmegosztás csatlakoztatva számára engedélyezett egyidejű megnyitott leíróinak hello felső korlátja.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-148">Error 1816 happens when you reach hello upper limit of concurrent open handles that are allowed for a file on hello computer where hello file share is being mounted.</span></span>

### <a name="solution"></a><span data-ttu-id="9eb7c-149">Megoldás</span><span class="sxs-lookup"><span data-stu-id="9eb7c-149">Solution</span></span>

<span data-ttu-id="9eb7c-150">Csökkentse a hello száma párhuzamos megnyitott leíróinak néhány leírók bezárásával, majd próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-150">Reduce hello number of concurrent open handles by closing some handles, and then retry.</span></span> <span data-ttu-id="9eb7c-151">További információkért lásd: [Microsoft Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="9eb7c-151">For more information, see [Microsoft Azure Storage performance and scalability checklist](storage-performance-checklist.md).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-windows"></a><span data-ttu-id="9eb7c-152">A Windows Azure File storage tooand másolását lassú fájl</span><span class="sxs-lookup"><span data-stu-id="9eb7c-152">Slow file copying tooand from Azure File storage in Windows</span></span>

<span data-ttu-id="9eb7c-153">Lassú teljesítmény tootransfer fájlok toohello Azure File service meg jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-153">You might see slow performance when you try tootransfer files toohello Azure File service.</span></span>

- <span data-ttu-id="9eb7c-154">Még nem rendelkezik egy adott minimális i/o vonatkozó méretbeli követelményt, azt javasoljuk, hogy használható 1 MB hello i/o-méret az optimális teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-154">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as hello I/O size for optimal performance.</span></span>
-   <span data-ttu-id="9eb7c-155">Ha tudja, amelyet a kiterjesztése hello a végleges mérethez ír, és a szoftver nincs kompatibilitási problémákat, ha hello unwritten utóhívás hello fájl tartalmaz nulla, majd hello fájl mérete előre minden egyes egy kibővítése írási elvégzése helyett.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-155">If you know hello final size of a file that you are extending with writes, and your software doesn’t have compatibility problems when hello unwritten tail on hello file contains zeros, then set hello file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="9eb7c-156">Hello jobb copy metódust használja:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-156">Use hello right copy method:</span></span>
    -   <span data-ttu-id="9eb7c-157">Használjon [AzCopy](storage-use-azcopy.md#file-copy) bármely két fájlmegosztások közötti átvitel céljából.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-157">Use [AzCopy](storage-use-azcopy.md#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="9eb7c-158">Használjon [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) közötti fájlmegosztások a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-158">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a><span data-ttu-id="9eb7c-159">Windows 8.1 vagy Windows Server 2012 R2 szempontjai</span><span class="sxs-lookup"><span data-stu-id="9eb7c-159">Considerations for Windows 8.1 or Windows Server 2012 R2</span></span>

<span data-ttu-id="9eb7c-160">Windows 8.1 vagy Windows Server 2012 R2 rendszert futtató ügyfeleken győződjön meg arról, hogy hello [KB3114025](https://support.microsoft.com/help/3114025) gyorsjavítás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-160">For clients that are running Windows 8.1 or Windows Server 2012 R2, make sure that hello [KB3114025](https://support.microsoft.com/help/3114025) hotfix is installed.</span></span> <span data-ttu-id="9eb7c-161">Ez a gyorsjavítás javítja a Create hello teljesítményét, és zárja be a kezeli.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-161">This hotfix improves hello performance of create and close handles.</span></span>

<span data-ttu-id="9eb7c-162">E hello gyorsjavítás telepítve van a következő parancsfájl toocheck hello futtathatja:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-162">You can run hello following script toocheck whether hello hotfix has been installed:</span></span>

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

<span data-ttu-id="9eb7c-163">Ha a gyorsjavítás telepítve van, hello következő eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-163">If hotfix is installed, hello following output is displayed:</span></span>

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> <span data-ttu-id="9eb7c-164">Windows Server 2012 R2 Azure piactéren képeknek gyorsjavítás KB3114025 alapértelmezés szerint telepítve 2015. decemberi kezdve.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-164">Windows Server 2012 R2 images in Azure Marketplace have hotfix KB3114025 installed by default, starting in December 2015.</span></span>

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a><span data-ttu-id="9eb7c-165">Ne legyen a meghajtóbetűjel mappa **Sajátgép**</span><span class="sxs-lookup"><span data-stu-id="9eb7c-165">No folder with a drive letter in **My Computer**</span></span>

<span data-ttu-id="9eb7c-166">Ha a nettó használata rendszergazdaként leképez egy Azure fájlmegosztás, hello megosztás megjelenik-e toobe hiányzik.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-166">If you map an Azure file share as an administrator by using net use, hello share appears toobe missing.</span></span>

### <a name="cause"></a><span data-ttu-id="9eb7c-167">Ok</span><span class="sxs-lookup"><span data-stu-id="9eb7c-167">Cause</span></span>

<span data-ttu-id="9eb7c-168">Alapértelmezés szerint a Windows Fájlkezelőben nem futtassa rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-168">By default, Windows File Explorer does not run as an administrator.</span></span> <span data-ttu-id="9eb7c-169">Ha nettó használata futtatja a parancsot egy rendszergazdai parancssort, akkor hello hálózati meghajtó meghajtó csatlakoztatása rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-169">If you run net use from an administrative command prompt, you map hello network drive as an administrator.</span></span> <span data-ttu-id="9eb7c-170">Mivel a csatlakoztatott meghajtók felhasználó-központú, hello felhasználói fiókot, amely be van jelentkezve nem jeleníti meg hello meghajtók ha csatlakoztatva vannak egy másik felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-170">Because mapped drives are user-centric, hello user account that is logged in does not display hello drives if they are mounted under a different user account.</span></span>

### <a name="solution"></a><span data-ttu-id="9eb7c-171">Megoldás</span><span class="sxs-lookup"><span data-stu-id="9eb7c-171">Solution</span></span>
<span data-ttu-id="9eb7c-172">Hello fájlmegosztás csatlakoztatása egy nem rendszergazdai parancssorból.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-172">Mount hello share from a non-administrator command line.</span></span> <span data-ttu-id="9eb7c-173">Azt is megteheti, amelyeket követve [a TechNet témakör](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** beállításazonosítót.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-173">Alternatively, you can follow [this TechNet topic](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** registry value.</span></span>

<a id="netuse"></a>
## <a name="net-use-command-fails-if-hello-storage-account-contains-a-forward-slash"></a><span data-ttu-id="9eb7c-174">Net use parancs sikertelen lesz, ha a tárfiók hello perjellel tartalmazza</span><span class="sxs-lookup"><span data-stu-id="9eb7c-174">Net use command fails if hello storage account contains a forward slash</span></span>

### <a name="cause"></a><span data-ttu-id="9eb7c-175">Ok</span><span class="sxs-lookup"><span data-stu-id="9eb7c-175">Cause</span></span>

<span data-ttu-id="9eb7c-176">hello net use parancs perjellel (/) értelmezi a parancssori kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-176">hello net use command interprets a forward slash (/) as a command-line option.</span></span> <span data-ttu-id="9eb7c-177">Ha a felhasználói fiók nevét perjellel kezdődik, hello meghajtók hozzárendelése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-177">If your user account name starts with a forward slash, hello drive mapping fails.</span></span>

### <a name="solution"></a><span data-ttu-id="9eb7c-178">Megoldás</span><span class="sxs-lookup"><span data-stu-id="9eb7c-178">Solution</span></span>

<span data-ttu-id="9eb7c-179">A következő lépéseket toowork hello probléma hello bármelyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-179">You can use either of hello following steps toowork around hello problem:</span></span>

- <span data-ttu-id="9eb7c-180">Futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-180">Run hello following PowerShell command:</span></span>

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  <span data-ttu-id="9eb7c-181">Egy parancsfájlból hello parancs futtatható ily módon:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-181">From a batch file, you can run hello command this way:</span></span>

  `Echo new-smbMapping ... | powershell -command –`

- <span data-ttu-id="9eb7c-182">Helyezze a hello kulcs toowork a probléma megoldásához--dupla idézőjelek közé, ha hello perjelet az hello első karakter.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-182">Put double quotation marks around hello key toowork around this problem--unless hello forward slash is hello first character.</span></span> <span data-ttu-id="9eb7c-183">Ha igen, használjon hello interaktív módban és külön-külön írja be a jelszót vagy újragenerálja a kulcsokat tooget egy kulcsot, nem kezdődhet perjellel.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-183">If it is, either use hello interactive mode and enter your password separately or regenerate your keys tooget a key that doesn't start with a forward slash.</span></span>

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a><span data-ttu-id="9eb7c-184">Alkalmazás vagy szolgáltatás nem tud hozzáférni az Azure File storage meghajtóként</span><span class="sxs-lookup"><span data-stu-id="9eb7c-184">Application or service cannot access a mounted Azure File storage drive</span></span>

### <a name="cause"></a><span data-ttu-id="9eb7c-185">Ok</span><span class="sxs-lookup"><span data-stu-id="9eb7c-185">Cause</span></span>

<span data-ttu-id="9eb7c-186">Felhasználónként csatlakoztatott meghajtók.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-186">Drives are mounted per user.</span></span> <span data-ttu-id="9eb7c-187">Ha az alkalmazás vagy szolgáltatás hello egy csatlakoztatott hello meghajtó, mint egy másik felhasználói fiók alatt fut, hello alkalmazás nem jelenik meg a hello meghajtó.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-187">If your application or service is running under a different user account than hello one that mounted hello drive, hello application will not see hello drive.</span></span>

### <a name="solution"></a><span data-ttu-id="9eb7c-188">Megoldás</span><span class="sxs-lookup"><span data-stu-id="9eb7c-188">Solution</span></span>

<span data-ttu-id="9eb7c-189">A következő megoldások hello egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-189">Use one of hello following solutions:</span></span>

-   <span data-ttu-id="9eb7c-190">Hello meghajtó csatlakoztathatja hello ugyanazzal a fiókkal, amelyik hello alkalmazást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-190">Mount hello drive from hello same user account that contains hello application.</span></span> <span data-ttu-id="9eb7c-191">Használhatja például a PsExec eszköz.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-191">You can use a tool such as PsExec.</span></span>
- <span data-ttu-id="9eb7c-192">Hello felhasználói nevet és jelszót paraméterei hello net use parancs hello tárolási fióknevet és kulcsot adjon át.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-192">Pass hello storage account name and key in hello user name and password parameters of hello net use command.</span></span>

<span data-ttu-id="9eb7c-193">Miután kövesse ezeket az utasításokat, akkor fordulhat elő, hello hello rendszer és a hálózati szolgáltatás fiók nettó használható futtatásakor a következő hibaüzenet: "1312 rendszerhiba történt.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-193">After you follow these instructions, you might receive hello following error message when you run net use for hello system/network service account: "System error 1312 has occurred.</span></span> <span data-ttu-id="9eb7c-194">A megadott bejelentkezési munkamenet nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-194">A specified logon session does not exist.</span></span> <span data-ttu-id="9eb7c-195">Azt is, hogy már befejeződött."</span><span class="sxs-lookup"><span data-stu-id="9eb7c-195">It may already have been terminated."</span></span> <span data-ttu-id="9eb7c-196">Ha ez történik, ellenőrizze, hogy toonet használata átadott hello felhasználónévhez tartalmaz adatokat (például: "[tárfiók neve]. file.core.windows .net").</span><span class="sxs-lookup"><span data-stu-id="9eb7c-196">If this occurs, make sure that hello username that is passed toonet use includes domain information (for example: "[storage account name].file.core.windows.net").</span></span>

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-tooa-destination-that-does-not-support-encryption"></a><span data-ttu-id="9eb7c-197">"Másolja át a fájlt, amely nem támogatja a titkosítást tooa cél" hiba</span><span class="sxs-lookup"><span data-stu-id="9eb7c-197">Error "You are copying a file tooa destination that does not support encryption"</span></span>

<span data-ttu-id="9eb7c-198">Hello hálózaton keresztül másolja a fájlt, hello fájl esetén visszafejtett hello forrásszámítógép, nem titkosított szövegként továbbításra, és újból titkosít hello célhelyen.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-198">When a file is copied over hello network, hello file is decrypted on hello source computer, transmitted in plaintext, and re-encrypted at hello destination.</span></span> <span data-ttu-id="9eb7c-199">Azonban láthatja a következő hiba toocopy titkosított fájl regisztráláskor hello: "Hello fájl tooa cél, amely nem támogatja a titkosítást másol."</span><span class="sxs-lookup"><span data-stu-id="9eb7c-199">However, you might see hello following error when you're trying toocopy an encrypted file: "You are copying hello file tooa destination that does not support encryption."</span></span>

### <a name="cause"></a><span data-ttu-id="9eb7c-200">Ok</span><span class="sxs-lookup"><span data-stu-id="9eb7c-200">Cause</span></span>
<span data-ttu-id="9eb7c-201">Ez a probléma akkor fordulhat elő, ha titkosított fájlrendszer (EFS) használja.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-201">This problem can occur if you are using Encrypting File System (EFS).</span></span> <span data-ttu-id="9eb7c-202">A BitLocker által titkosított fájlok lehet másolt tooAzure fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-202">BitLocker-encrypted files can be copied tooAzure File storage.</span></span> <span data-ttu-id="9eb7c-203">Azure File storage azonban nem támogatja az NTFS EFS.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-203">However, Azure File storage does not support NTFS EFS.</span></span>

### <a name="workaround"></a><span data-ttu-id="9eb7c-204">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="9eb7c-204">Workaround</span></span>
<span data-ttu-id="9eb7c-205">toocopy hello hálózaton keresztül egy fájlt, akkor először vissza kell fejtenie azt.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-205">toocopy a file over hello network, you must first decrypt it.</span></span> <span data-ttu-id="9eb7c-206">Hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-206">Use one of hello following methods:</span></span>

- <span data-ttu-id="9eb7c-207">Használjon hello **/d másolása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-207">Use hello **copy /d** command.</span></span> <span data-ttu-id="9eb7c-208">Lehetővé teszi a hello titkosított fájlok toobe hello célhelyen készítését.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-208">It allows hello encrypted files toobe saved as decrypted files at hello destination.</span></span>
- <span data-ttu-id="9eb7c-209">Állítsa be a következő beállításkulcs hello:</span><span class="sxs-lookup"><span data-stu-id="9eb7c-209">Set hello following registry key:</span></span>
  - <span data-ttu-id="9eb7c-210">Elérési út = HKLM\Software\Policies\Microsoft\Windows\System</span><span class="sxs-lookup"><span data-stu-id="9eb7c-210">Path = HKLM\Software\Policies\Microsoft\Windows\System</span></span>
  - <span data-ttu-id="9eb7c-211">Értéktípus = DWORD</span><span class="sxs-lookup"><span data-stu-id="9eb7c-211">Value type = DWORD</span></span>
  - <span data-ttu-id="9eb7c-212">Name = CopyFileAllowDecryptedRemoteDestination</span><span class="sxs-lookup"><span data-stu-id="9eb7c-212">Name = CopyFileAllowDecryptedRemoteDestination</span></span>
  - <span data-ttu-id="9eb7c-213">Érték = 1</span><span class="sxs-lookup"><span data-stu-id="9eb7c-213">Value = 1</span></span>

<span data-ttu-id="9eb7c-214">Ügyeljen arra, hogy beállítás hello beállításkulcshoz hatással van az összes másolási műveletek végzett toonetwork megosztások.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-214">Be aware that setting hello registry key affects all copy operations that are made toonetwork shares.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="9eb7c-215">Segítség</span><span class="sxs-lookup"><span data-stu-id="9eb7c-215">Need help?</span></span> <span data-ttu-id="9eb7c-216">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-216">Contact support.</span></span>
<span data-ttu-id="9eb7c-217">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget gyorsan oldja meg a problémát.</span><span class="sxs-lookup"><span data-stu-id="9eb7c-217">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your problem resolved quickly.</span></span>
