---
title: "aaaTroubleshoot védelmi hibák VMware vagy fizikai tooAzure |} Microsoft Docs"
description: "Ez a cikk ismerteti a hello közös VMware gép replikációs hibák, és hogyan tootroubleshoot őket"
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
ms.openlocfilehash: b821e9aa2610482ba1900645fb75e75744dc442f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a><span data-ttu-id="6e715-103">Helyszíni VMware vagy fizikai kiszolgáló replikációs problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="6e715-103">Troubleshoot on-premises VMware/Physical server replication issues</span></span>
<span data-ttu-id="6e715-104">Specifikus hibaüzenet jelenhet meg, ha a VMware virtuális gépek vagy fizikai kiszolgálók Azure Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="6e715-104">You may receive a specific error message when protecting your VMware virtual machines or physical servers using Azure Site Recovery.</span></span> <span data-ttu-id="6e715-105">Ez a cikk részletek néhány gyakori hibaüzenetek észlelt, és hibaelhárítási lépéseket tooresolve hello őket.</span><span class="sxs-lookup"><span data-stu-id="6e715-105">This article details some of hello more common error messages encountered, along with troubleshooting steps tooresolve them.</span></span>


## <a name="initial-replication-is-stuck-at-0"></a><span data-ttu-id="6e715-106">A(z) 0 % elakadt a kezdeti replikálás</span><span class="sxs-lookup"><span data-stu-id="6e715-106">Initial replication is stuck at 0%</span></span>
<span data-ttu-id="6e715-107">A Microsoft terméktámogatási előforduló hello kezdeti replikációs hibák többsége kiszolgálófolyamat forráskiszolgáló vagy folyamat kiszolgáló Azure között tooconnectivity problémák miatt.</span><span class="sxs-lookup"><span data-stu-id="6e715-107">Most of hello initial replication failures that we encounter at support are due tooconnectivity issues between source server-to-process server or process server-to-Azure.</span></span>
<span data-ttu-id="6e715-108">A legtöbb esetben is maga a problémák elhárítására hello az alábbi lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="6e715-108">For most cases, you can self troubleshoot these issues by following hello steps listed below.</span></span>

###<a name="check-hello-following-on-source-machine"></a><span data-ttu-id="6e715-109">Ellenőrizze a VÉDENDŐ GÉPEN hello következőket</span><span class="sxs-lookup"><span data-stu-id="6e715-109">Check hello following on SOURCE MACHINE</span></span>
* <span data-ttu-id="6e715-110">A forráskiszolgáló gép parancssorból használata Telnet tooping hello Folyamatkiszolgáló https-portja (alapértelmezés szerint 9443) toosee lent látható módon, ha a hálózati kapcsolat hibái vagy tűzfalportot blokkolja a problémákat.</span><span class="sxs-lookup"><span data-stu-id="6e715-110">From Source Server machine command line, use Telnet tooping hello Process Server with https port (default 9443) as shown below toosee if there are any network connectivity issues or firewall port blocking issues.</span></span>
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > <span data-ttu-id="6e715-111">Telnet használja, ne használjon PING tootest kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="6e715-111">Use Telnet, don’t use PING tootest connectivity.</span></span>  <span data-ttu-id="6e715-112">Ha nincs telepítve a Telnet, hajtsa végre a hello lépéseket lista [Itt](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="6e715-112">If Telnet is not installed, follow hello steps list [here](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span></span>

<span data-ttu-id="6e715-113">Ha nem tooconnect engedélyezése a Folyamatkiszolgáló hello 9443 a bejövő portot, és ellenőrizze, hogy hello a probléma továbbra is kilép.</span><span class="sxs-lookup"><span data-stu-id="6e715-113">If unable tooconnect, allow inbound port 9443 on hello Process Server and check if hello problem still exits.</span></span> <span data-ttu-id="6e715-114">Egyes esetekben, ahol a folyamatkiszolgáló DMZ, amely a problémás volt mögött lett lett.</span><span class="sxs-lookup"><span data-stu-id="6e715-114">There has been some cases where process server was behind DMZ, which was causing this problem.</span></span>

* <span data-ttu-id="6e715-115">Ellenőrizze a szolgáltatás állapotának hello `InMage Scout VX Agent – Sentinel/OutpostStart` Ha nem fut, valamint ellenőrzés esetén hello a probléma továbbra is létezik.</span><span class="sxs-lookup"><span data-stu-id="6e715-115">Check hello status of service `InMage Scout VX Agent – Sentinel/OutpostStart` if it is not running and check if hello problem still exists.</span></span>   
 
###<a name="check-hello-following-on-process-server"></a><span data-ttu-id="6e715-116">Ellenőrizze a folyamatkiszolgáló hello következő</span><span class="sxs-lookup"><span data-stu-id="6e715-116">Check hello following on PROCESS SERVER</span></span>

* <span data-ttu-id="6e715-117">**Ha a folyamatkiszolgáló aktívan küldését adatok tooAzure ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="6e715-117">**Check if process server is actively pushing data tooAzure**</span></span> 

<span data-ttu-id="6e715-118">A Folyamatkiszolgáló gépről nyissa meg a hello Feladatkezelő (nyomja meg a Ctrl-Shift-Esc).</span><span class="sxs-lookup"><span data-stu-id="6e715-118">From Process Server machine, open hello Task Manager (press Ctrl-Shift-Esc ).</span></span> <span data-ttu-id="6e715-119">Nyissa meg toohello teljesítmény fülre, és "Nyissa meg az erőforrás-figyelő" hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6e715-119">Go toohello Performance tab and click ‘Open Resource Monitor’ link.</span></span> <span data-ttu-id="6e715-120">Az erőforrás-kezelő tooNetwork lapon lépjen. Ellenőrizze, hogy ha a "Hálózati tevékenységet folyamatokban" cbengine.exe aktívan küld nagy mennyiségű (MB-ban) adat.</span><span class="sxs-lookup"><span data-stu-id="6e715-120">From Resource Manager, go tooNetwork tab. Check if cbengine.exe in ‘Processes with Network Activity’ is actively sending large volume (in Mbs) of data.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/cbengine.png)

<span data-ttu-id="6e715-122">Ha nem hello az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="6e715-122">If not follow hello steps listed below:</span></span>

* <span data-ttu-id="6e715-123">**Annak ellenőrzése, hogy a folyamatkiszolgáló képes tooconnect Azure Blob**: Jelölje ki és cbengine.exe tooview hello TCP-kapcsolatok toosee ellenőrizze, hogy nincs kapcsolat folyamat tooAzure tárolási blob URL-címe.</span><span class="sxs-lookup"><span data-stu-id="6e715-123">**Check if Process server is able tooconnect Azure Blob**: Select and check cbengine.exe tooview hello ‘TCP Connections’ toosee if there is connectivity from Process server tooAzure Storage blob URL.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/rmonitor.png)

<span data-ttu-id="6e715-125">Ha nem, folytassa a tooControl Panel > szolgáltatások, ellenőrizze, hogy e szolgáltatások a következő hello működik, és:</span><span class="sxs-lookup"><span data-stu-id="6e715-125">If not then go tooControl Panel > Services, check if hello following services are up and running:</span></span>

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
<span data-ttu-id="6e715-126">(Újra) Indítsa el a szolgáltatást, amely nem fut, és ellenőrizze, hogy hello a probléma továbbra is léteznek.</span><span class="sxs-lookup"><span data-stu-id="6e715-126">(Re)Start any service which is not running and check if hello problem still exists.</span></span>

* <span data-ttu-id="6e715-127">**Annak ellenőrzése, hogy a folyamatkiszolgáló képes tooconnect tooAzure nyilvános IP-cím 443-as porton**</span><span class="sxs-lookup"><span data-stu-id="6e715-127">**Check if Process server is able tooconnect tooAzure Public IP address using port 443**</span></span>

<span data-ttu-id="6e715-128">Nyissa meg a legújabb CBEngineCurr.errlog hello `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` keresse meg a: 443-as és a kapcsolódási kísérlet sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="6e715-128">Open hello latest CBEngineCurr.errlog from `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` and search for :443  and connection attempt failed.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/logdetails1.png)

<span data-ttu-id="6e715-130">Ha problémák vannak, a Folyamatkiszolgáló parancssorból, használja a telnet tooping a Azure nyilvános IP-cím (maszkolva a fenti kép) hello CBEngineCurr.currLog a 443-as porton.</span><span class="sxs-lookup"><span data-stu-id="6e715-130">If there are issues, then from Process Server command line, use telnet tooping your Azure Public IP address (masked in above image) found in hello CBEngineCurr.currLog using port 443.</span></span>

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
<span data-ttu-id="6e715-131">Ha nem tooconnect, majd ellenőrizze meg, hogy hello hozzáférési probléma miatt toofirewall vagy a Proxy a következő lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6e715-131">If you are unable tooconnect, then check if hello access issue is due toofirewall or Proxy as described in next step.</span></span>


* <span data-ttu-id="6e715-132">**Ellenőrizze, hogy ha a folyamatkiszolgáló IP cím alapú tűzfal nem blokkolja-e hozzáférési**: Ha egy IP-címeken alapuló tűzfalszabályok szabályokat használ hello kiszolgálón, majd töltse le hello teljes listája a Microsoft Azure Datacenter IP-címtartományok a [Itt ](https://www.microsoft.com/download/details.aspx?id=41653) tooyour tűzfal konfigurációs tooensure kommunikációs tooAzure (és hello HTTPS (443) port) lehetővé teszik adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="6e715-132">**Check if IP address-based firewall on Process server are not blocking access**: If you are using an IP address-based firewall rules on hello server, then download hello complete list of Microsoft Azure Datacenter IP Ranges from [here](https://www.microsoft.com/download/details.aspx?id=41653) and add them tooyour firewall configuration tooensure they allow communication tooAzure (and hello HTTPS (443) port).</span></span>  <span data-ttu-id="6e715-133">Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).</span><span class="sxs-lookup"><span data-stu-id="6e715-133">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

* <span data-ttu-id="6e715-134">**Ellenőrizze, hogy ha folyamatkiszolgáló URL-alapú tűzfal nem blokkolja a hozzáférést**: Ha egy URL-cím alapú tűzfalszabályok hello kiszolgálón használ, győződjön meg arról, a következő URL-címek hello kerülnek toofirewall konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="6e715-134">**Check if URL-based firewall on Process server is not blocking access**:  If you are using a URL based firewall rules on hello server, ensure hello following URLs are added toofirewall configuration.</span></span> 
     
  <span data-ttu-id="6e715-135">`*.accesscontrol.windows.net:` hozzáférés-vezérléshez és identitáskezeléshez</span><span class="sxs-lookup"><span data-stu-id="6e715-135">`*.accesscontrol.windows.net:` Used for access control and identity management</span></span>

  <span data-ttu-id="6e715-136">`*.backup.windowsazure.com:` replikációs adatátvitelhez és vezényléshez</span><span class="sxs-lookup"><span data-stu-id="6e715-136">`*.backup.windowsazure.com:` Used for replication data transfer and orchestration</span></span>

  <span data-ttu-id="6e715-137">`*.blob.core.windows.net:`Hozzáférés toohello használt tárfiókot, amely tárolja a replikált adatok</span><span class="sxs-lookup"><span data-stu-id="6e715-137">`*.blob.core.windows.net:` Used for access toohello storage account that stores replicated data</span></span>

  <span data-ttu-id="6e715-138">`*.hypervrecoverymanager.windowsazure.com:` replikációkezelési műveletekhez és vezényléshez</span><span class="sxs-lookup"><span data-stu-id="6e715-138">`*.hypervrecoverymanager.windowsazure.com:` Used for replication management operations and orchestration</span></span>

  <span data-ttu-id="6e715-139">`time.nist.gov`és `time.windows.com`: használt toocheck időszinkronizálást a rendszer és a globális időhöz között.</span><span class="sxs-lookup"><span data-stu-id="6e715-139">`time.nist.gov` and `time.windows.com`: Used toocheck time synchronization between system and global time.</span></span>

<span data-ttu-id="6e715-140">URL-címéből **Azure Government felhő**:</span><span class="sxs-lookup"><span data-stu-id="6e715-140">URLs for **Azure Government Cloud**:</span></span>

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* <span data-ttu-id="6e715-141">**Ellenőrizze, hogy ha a proxykiszolgáló beállításait a folyamatkiszolgáló nem blokkolja-e hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="6e715-141">**Check if Proxy Settings on Process server are not blocking access**.</span></span>  <span data-ttu-id="6e715-142">Ha proxykiszolgálót használ, győződjön meg arról, hello DNS-kiszolgáló megoldása, hello proxykiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="6e715-142">If you are using a Proxy Server, ensure hello proxy server name is resolving by hello DNS server.</span></span>
<span data-ttu-id="6e715-143">toocheck hello konfigurációs kiszolgáló telepítése során van megadva.</span><span class="sxs-lookup"><span data-stu-id="6e715-143">toocheck what you have provided at hello time of Configuration Server setup.</span></span> <span data-ttu-id="6e715-144">Nyissa meg tooregistry kulcs</span><span class="sxs-lookup"><span data-stu-id="6e715-144">Go tooregistry key</span></span>

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

<span data-ttu-id="6e715-145">Most győződjön meg arról, hogy hello Azure Site Recovery ügynök toosend adatok használja ugyanazokat a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6e715-145">Now ensure that hello same settings are being used by Azure Site Recovery agent toosend data.</span></span>
<span data-ttu-id="6e715-146">Keresés a Microsoft Azure Backup szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6e715-146">Search Microsoft Azure  Backup</span></span> 

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/mab.png)

<span data-ttu-id="6e715-148">Nyissa meg azt, majd kattintson a művelet > tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="6e715-148">Open it and click on Action > Change Properties.</span></span> <span data-ttu-id="6e715-149">A proxykonfiguráció lapon megtekintheti az hello proxy címe, amely hello beállításjegyzék-beállítások eredményobjektumokról azonosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6e715-149">Under Proxy Configuration tab, you should see hello proxy address, which should be same as shown by hello registry settings.</span></span> <span data-ttu-id="6e715-150">Ha nem, változtassa meg a toohello ugyanazt a címet.</span><span class="sxs-lookup"><span data-stu-id="6e715-150">If not, please change it toohello same address.</span></span>

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/mabproxy.png)

* <span data-ttu-id="6e715-152">**Ellenőrizze, ha a sávszélesség-szabályozás nem korlátozott folyamatkiszolgáló**: hello sávszélesség növelése, és ellenőrizze, hogy hello a probléma továbbra is léteznek.</span><span class="sxs-lookup"><span data-stu-id="6e715-152">**Check if Throttle bandwidth is not constrained on Process server**:  Increase hello bandwidth  and check if hello problem still exists.</span></span>

##<a name="next-steps"></a><span data-ttu-id="6e715-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e715-153">Next steps</span></span>
<span data-ttu-id="6e715-154">Ha további segítségre van szüksége, majd indítsa el a lekérdezés túl[ASR fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6e715-154">If you need more help, then post your query too[ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span></span> <span data-ttu-id="6e715-155">Egy aktív közösségi van, és a mérnökök egyik képes tooassist meg.</span><span class="sxs-lookup"><span data-stu-id="6e715-155">We have an active community and one of our engineers will be able tooassist you.</span></span>
