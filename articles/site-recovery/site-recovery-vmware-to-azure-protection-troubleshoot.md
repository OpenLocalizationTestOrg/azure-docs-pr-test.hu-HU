---
title: "Az Azure-bA VMware vagy fizikai védelmi hibák elhárítása |} Microsoft Docs"
description: "Ez a cikk ismerteti a közös VMware gép replikációs hibák, és azokat hibaelhárítása"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/26/2017
ms.author: asgang
ms.openlocfilehash: 6ebec2e06566b1e2d6834fdd81c0d8b2801b80b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a><span data-ttu-id="638bc-103">Helyszíni VMware vagy fizikai kiszolgáló replikációs problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="638bc-103">Troubleshoot on-premises VMware/Physical server replication issues</span></span>
<span data-ttu-id="638bc-104">Specifikus hibaüzenet jelenhet meg, ha a VMware virtuális gépek vagy fizikai kiszolgálók Azure Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="638bc-104">You may receive a specific error message when protecting your VMware virtual machines or physical servers using Azure Site Recovery.</span></span> <span data-ttu-id="638bc-105">Ez a cikk részletesen néhány, a hibát, és azok megoldását hibaelhárítási Gyakori hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="638bc-105">This article details some of the more common error messages encountered, along with troubleshooting steps to resolve them.</span></span>


## <a name="initial-replication-is-stuck-at-0"></a><span data-ttu-id="638bc-106">A(z) 0 % elakadt a kezdeti replikálás</span><span class="sxs-lookup"><span data-stu-id="638bc-106">Initial replication is stuck at 0%</span></span>
<span data-ttu-id="638bc-107">A kezdeti replikációs hibák, a Microsoft terméktámogatási előforduló többsége kiszolgálófolyamat forráskiszolgáló vagy folyamat kiszolgáló Azure közötti kapcsolat problémái miatt.</span><span class="sxs-lookup"><span data-stu-id="638bc-107">Most of the initial replication failures that we encounter at support are due to connectivity issues between source server-to-process server or process server-to-Azure.</span></span>
<span data-ttu-id="638bc-108">A legtöbb esetben is maga az alábbi lépéseket követve ezek a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="638bc-108">For most cases, you can self troubleshoot these issues by following the steps listed below.</span></span>

###<a name="check-the-following-on-source-machine"></a><span data-ttu-id="638bc-109">Ellenőrizze a következőket a VÉDENDŐ GÉPEN</span><span class="sxs-lookup"><span data-stu-id="638bc-109">Check the following on SOURCE MACHINE</span></span>
* <span data-ttu-id="638bc-110">A forráskiszolgáló gép parancssorból használja a Telnet pingelni a https-port (alapértelmezett 9443) a Folyamatkiszolgáló, megtekintéséhez, hogy vannak-e a hálózati kapcsolat hibái vagy a tűzfal port problémák elhárítását alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="638bc-110">From Source Server machine command line, use Telnet to ping the Process Server with https port (default 9443) as shown below to see if there are any network connectivity issues or firewall port blocking issues.</span></span>
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > <span data-ttu-id="638bc-111">Használja a Telnet, ne használjon a PING való kapcsolat teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="638bc-111">Use Telnet, don’t use PING to test connectivity.</span></span>  <span data-ttu-id="638bc-112">Ha Telnet nincs telepítve, kövesse a lépéseket lista [Itt](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="638bc-112">If Telnet is not installed, follow the steps list [here](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span></span>

<span data-ttu-id="638bc-113">Ha nem lehet csatlakozni, engedélyezze a folyamatkiszolgáló 9443 a bejövő portot, és ellenőrizze, hogy ha a probléma továbbra is kilép.</span><span class="sxs-lookup"><span data-stu-id="638bc-113">If unable to connect, allow inbound port 9443 on the Process Server and check if the problem still exits.</span></span> <span data-ttu-id="638bc-114">Egyes esetekben, ahol a folyamatkiszolgáló DMZ, amely a problémás volt mögött lett lett.</span><span class="sxs-lookup"><span data-stu-id="638bc-114">There has been some cases where process server was behind DMZ, which was causing this problem.</span></span>

* <span data-ttu-id="638bc-115">A szolgáltatás állapotának ellenőrzése `InMage Scout VX Agent – Sentinel/OutpostStart` Ha még nem fut, valamint ellenőrizze, ha a probléma továbbra is létezik.</span><span class="sxs-lookup"><span data-stu-id="638bc-115">Check the status of service `InMage Scout VX Agent – Sentinel/OutpostStart` if it is not running and check if the problem still exists.</span></span>   
 
###<a name="check-the-following-on-process-server"></a><span data-ttu-id="638bc-116">Ellenőrizze a következőket folyamatkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="638bc-116">Check the following on PROCESS SERVER</span></span>

* <span data-ttu-id="638bc-117">**Ellenőrizze, hogy ha a folyamatkiszolgáló aktívan küldését adatokat az Azure-bA**</span><span class="sxs-lookup"><span data-stu-id="638bc-117">**Check if process server is actively pushing data to Azure**</span></span> 

<span data-ttu-id="638bc-118">A Folyamatkiszolgáló gépről nyissa meg a Feladatkezelőt (nyomja meg a Ctrl-Shift-Esc).</span><span class="sxs-lookup"><span data-stu-id="638bc-118">From Process Server machine, open the Task Manager (press Ctrl-Shift-Esc ).</span></span> <span data-ttu-id="638bc-119">Nyissa meg a Teljesítmény fülre, és "Nyissa meg az erőforrás-figyelő" hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="638bc-119">Go to the Performance tab and click ‘Open Resource Monitor’ link.</span></span> <span data-ttu-id="638bc-120">Az erőforrás-kezelő hálózati lap Ugrás.</span><span class="sxs-lookup"><span data-stu-id="638bc-120">From Resource Manager, go to Network tab.</span></span> <span data-ttu-id="638bc-121">Ellenőrizze, hogy ha a "Hálózati tevékenységet folyamatokban" cbengine.exe aktívan küld nagy mennyiségű (MB-ban) adat.</span><span class="sxs-lookup"><span data-stu-id="638bc-121">Check if cbengine.exe in ‘Processes with Network Activity’ is actively sending large volume (in Mbs) of data.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/cbengine.png)

<span data-ttu-id="638bc-123">Ha nem kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="638bc-123">If not follow the steps listed below:</span></span>

* <span data-ttu-id="638bc-124">**Annak ellenőrzése, hogy a folyamatkiszolgáló tud csatlakozni az Azure Blob**: válassza ki, és ellenőrizze a cbengine.exe megtekintéséhez a "TCP-kapcsolatok" a kiszolgáló közötti kapcsolatot folyamat Azure Storage-blob URL-címre van.</span><span class="sxs-lookup"><span data-stu-id="638bc-124">**Check if Process server is able to connect Azure Blob**: Select and check cbengine.exe to view the ‘TCP Connections’ to see if there is connectivity from Process server to Azure Storage blob URL.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/rmonitor.png)

<span data-ttu-id="638bc-126">Ha nem majd nyissa meg a Vezérlőpultot > szolgáltatások, ellenőrizze, hogy e a következő szolgáltatások működik, és:</span><span class="sxs-lookup"><span data-stu-id="638bc-126">If not then go to Control Panel > Services, check if the following services are up and running:</span></span>

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
<span data-ttu-id="638bc-127">(Újra) Indítsa el a szolgáltatást, amely nem fut, és ellenőrizze, hogy a probléma továbbra is léteznek.</span><span class="sxs-lookup"><span data-stu-id="638bc-127">(Re)Start any service which is not running and check if the problem still exists.</span></span>

* <span data-ttu-id="638bc-128">**Ellenőrizze, hogy tud csatlakozni az Azure nyilvános IP-cím 443-as porton-e a folyamatkiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="638bc-128">**Check if Process server is able to connect to Azure Public IP address using port 443**</span></span>

<span data-ttu-id="638bc-129">Nyissa meg a legújabb CBEngineCurr.errlog `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` keresse meg a: 443-as és a kapcsolódási kísérlet sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="638bc-129">Open the latest CBEngineCurr.errlog from `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` and search for :443  and connection attempt failed.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/logdetails1.png)

<span data-ttu-id="638bc-131">Ha problémák vannak, majd a Folyamatkiszolgáló parancssorból, használja a telnet pingelni a 443-as porton CBEngineCurr.currLog található (maszkolva a fenti kép) Azure nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="638bc-131">If there are issues, then from Process Server command line, use telnet to ping your Azure Public IP address (masked in above image) found in the CBEngineCurr.currLog using port 443.</span></span>

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
<span data-ttu-id="638bc-132">Ha nem lehet csatlakozni, majd ellenőrizze, ha a hozzáférési probléma okozza-e tűzfal- vagy proxybeállításokat a következő lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="638bc-132">If you are unable to connect, then check if the access issue is due to firewall or Proxy as described in next step.</span></span>


* <span data-ttu-id="638bc-133">**Ellenőrizze, hogy ha a folyamatkiszolgáló IP cím alapú tűzfal nem blokkolja-e hozzáférési**: Ha egy IP-címeken alapuló tűzfalszabályok szabályokat használ a kiszolgálón, majd töltse le a Microsoft Azure Datacenter IP-címtartományok a teljes listáját [Itt](https://www.microsoft.com/download/details.aspx?id=41653) és a tűzfal konfigurációját, és győződjön meg arról, lehetővé teszik az Azure (és a HTTPS (443) port) való adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="638bc-133">**Check if IP address-based firewall on Process server are not blocking access**: If you are using an IP address-based firewall rules on the server, then download the complete list of Microsoft Azure Datacenter IP Ranges from [here](https://www.microsoft.com/download/details.aspx?id=41653) and add them to your firewall configuration to ensure they allow communication to Azure (and the HTTPS (443) port).</span></span>  <span data-ttu-id="638bc-134">Engedélyezze az előfizetéshez tartozó Azure-régió, illetve az USA nyugati régiójának IP-tartományait (a hozzáférés-vezérléshez és az identitáskezeléshez szükséges).</span><span class="sxs-lookup"><span data-stu-id="638bc-134">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

* <span data-ttu-id="638bc-135">**Ellenőrizze, hogy ha folyamatkiszolgáló URL-alapú tűzfal nem blokkolja a hozzáférést**: Ha egy URL-cím alapú tűzfalszabályok használ a kiszolgálón, győződjön meg arról, a következő URL-címek hozzáadódnak a tűzfal konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="638bc-135">**Check if URL-based firewall on Process server is not blocking access**:  If you are using a URL based firewall rules on the server, ensure the following URLs are added to firewall configuration.</span></span> 
     
  <span data-ttu-id="638bc-136">`*.accesscontrol.windows.net:` hozzáférés-vezérléshez és identitáskezeléshez</span><span class="sxs-lookup"><span data-stu-id="638bc-136">`*.accesscontrol.windows.net:` Used for access control and identity management</span></span>

  <span data-ttu-id="638bc-137">`*.backup.windowsazure.com:` replikációs adatátvitelhez és vezényléshez</span><span class="sxs-lookup"><span data-stu-id="638bc-137">`*.backup.windowsazure.com:` Used for replication data transfer and orchestration</span></span>

  <span data-ttu-id="638bc-138">`*.blob.core.windows.net:`A tárfiók, hogy a tároló replikált adatok elérésére használt</span><span class="sxs-lookup"><span data-stu-id="638bc-138">`*.blob.core.windows.net:` Used for access to the storage account that stores replicated data</span></span>

  <span data-ttu-id="638bc-139">`*.hypervrecoverymanager.windowsazure.com:` replikációkezelési műveletekhez és vezényléshez</span><span class="sxs-lookup"><span data-stu-id="638bc-139">`*.hypervrecoverymanager.windowsazure.com:` Used for replication management operations and orchestration</span></span>

  <span data-ttu-id="638bc-140">`time.nist.gov`és `time.windows.com`: rendszer és a globális időhöz közötti időszinkronizálást ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="638bc-140">`time.nist.gov` and `time.windows.com`: Used to check time synchronization between system and global time.</span></span>

<span data-ttu-id="638bc-141">URL-címéből **Azure Government felhő**:</span><span class="sxs-lookup"><span data-stu-id="638bc-141">URLs for **Azure Government Cloud**:</span></span>

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* <span data-ttu-id="638bc-142">**Ellenőrizze, hogy ha a proxykiszolgáló beállításait a folyamatkiszolgáló nem blokkolja-e hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="638bc-142">**Check if Proxy Settings on Process server are not blocking access**.</span></span>  <span data-ttu-id="638bc-143">Ha proxykiszolgálót használ, győződjön meg arról, a DNS-kiszolgáló megoldása, a proxykiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="638bc-143">If you are using a Proxy Server, ensure the proxy server name is resolving by the DNS server.</span></span>
<span data-ttu-id="638bc-144">Ellenőrizze, hogy a konfigurációs kiszolgáló telepítése során van megadva.</span><span class="sxs-lookup"><span data-stu-id="638bc-144">To check what you have provided at the time of Configuration Server setup.</span></span> <span data-ttu-id="638bc-145">Ugrás a beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="638bc-145">Go to registry key</span></span>

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

<span data-ttu-id="638bc-146">Most ellenőrizze, hogy ugyanazokat a beállításokat szolgáltatás alatt álló Azure Site Recovery-ügynök adatküldéshez.</span><span class="sxs-lookup"><span data-stu-id="638bc-146">Now ensure that the same settings are being used by Azure Site Recovery agent to send data.</span></span>
<span data-ttu-id="638bc-147">Keresés a Microsoft Azure Backup szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="638bc-147">Search Microsoft Azure  Backup</span></span> 

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/mab.png)

<span data-ttu-id="638bc-149">Nyissa meg azt, majd kattintson a művelet > tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="638bc-149">Open it and click on Action > Change Properties.</span></span> <span data-ttu-id="638bc-150">A proxykonfiguráció lapon meg kell jelennie a proxykiszolgáló címét, amely a beállításjegyzékben szereplő beállítások eredményobjektumokról azonosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="638bc-150">Under Proxy Configuration tab, you should see the proxy address, which should be same as shown by the registry settings.</span></span> <span data-ttu-id="638bc-151">Ha nem, akkor módosítsa a címmel.</span><span class="sxs-lookup"><span data-stu-id="638bc-151">If not, please change it to the same address.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/mabproxy.png)

* <span data-ttu-id="638bc-153">**Ellenőrizze, ha a sávszélesség-szabályozás nem korlátozott folyamatkiszolgáló**: növelje a sávszélességet, és ellenőrizze, hogy a probléma továbbra is léteznek.</span><span class="sxs-lookup"><span data-stu-id="638bc-153">**Check if Throttle bandwidth is not constrained on Process server**:  Increase the bandwidth  and check if the problem still exists.</span></span>

##<a name="next-steps"></a><span data-ttu-id="638bc-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="638bc-154">Next steps</span></span>
<span data-ttu-id="638bc-155">Ha további segítségre van szüksége, majd indítsa el a lekérdezés [ASR fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="638bc-155">If you need more help, then post your query to [ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span></span> <span data-ttu-id="638bc-156">Egy aktív közösségi van, és a mérnökök egyik fog tudni segíteni.</span><span class="sxs-lookup"><span data-stu-id="638bc-156">We have an active community and one of our engineers will be able to assist you.</span></span>