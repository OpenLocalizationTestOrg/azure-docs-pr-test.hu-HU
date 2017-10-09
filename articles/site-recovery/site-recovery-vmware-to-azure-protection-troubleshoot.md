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
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a>Helyszíni VMware vagy fizikai kiszolgáló replikációs problémák elhárítása
Specifikus hibaüzenet jelenhet meg, ha a VMware virtuális gépek vagy fizikai kiszolgálók Azure Site Recovery segítségével. Ez a cikk részletek néhány gyakori hibaüzenetek észlelt, és hibaelhárítási lépéseket tooresolve hello őket.


## <a name="initial-replication-is-stuck-at-0"></a>A(z) 0 % elakadt a kezdeti replikálás
A Microsoft terméktámogatási előforduló hello kezdeti replikációs hibák többsége kiszolgálófolyamat forráskiszolgáló vagy folyamat kiszolgáló Azure között tooconnectivity problémák miatt.
A legtöbb esetben is maga a problémák elhárítására hello az alábbi lépéseket követve.

###<a name="check-hello-following-on-source-machine"></a>Ellenőrizze a VÉDENDŐ GÉPEN hello következőket
* A forráskiszolgáló gép parancssorból használata Telnet tooping hello Folyamatkiszolgáló https-portja (alapértelmezés szerint 9443) toosee lent látható módon, ha a hálózati kapcsolat hibái vagy tűzfalportot blokkolja a problémákat.
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > Telnet használja, ne használjon PING tootest kapcsolat.  Ha nincs telepítve a Telnet, hajtsa végre a hello lépéseket lista [Itt](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Ha nem tooconnect engedélyezése a Folyamatkiszolgáló hello 9443 a bejövő portot, és ellenőrizze, hogy hello a probléma továbbra is kilép. Egyes esetekben, ahol a folyamatkiszolgáló DMZ, amely a problémás volt mögött lett lett.

* Ellenőrizze a szolgáltatás állapotának hello `InMage Scout VX Agent – Sentinel/OutpostStart` Ha nem fut, valamint ellenőrzés esetén hello a probléma továbbra is létezik.   
 
###<a name="check-hello-following-on-process-server"></a>Ellenőrizze a folyamatkiszolgáló hello következő

* **Ha a folyamatkiszolgáló aktívan küldését adatok tooAzure ellenőrzése** 

A Folyamatkiszolgáló gépről nyissa meg a hello Feladatkezelő (nyomja meg a Ctrl-Shift-Esc). Nyissa meg toohello teljesítmény fülre, és "Nyissa meg az erőforrás-figyelő" hivatkozásra. Az erőforrás-kezelő tooNetwork lapon lépjen. Ellenőrizze, hogy ha a "Hálózati tevékenységet folyamatokban" cbengine.exe aktívan küld nagy mennyiségű (MB-ban) adat.

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/cbengine.png)

Ha nem hello az alábbi lépéseket követve:

* **Annak ellenőrzése, hogy a folyamatkiszolgáló képes tooconnect Azure Blob**: Jelölje ki és cbengine.exe tooview hello TCP-kapcsolatok toosee ellenőrizze, hogy nincs kapcsolat folyamat tooAzure tárolási blob URL-címe.

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/rmonitor.png)

Ha nem, folytassa a tooControl Panel > szolgáltatások, ellenőrizze, hogy e szolgáltatások a következő hello működik, és:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
(Újra) Indítsa el a szolgáltatást, amely nem fut, és ellenőrizze, hogy hello a probléma továbbra is léteznek.

* **Annak ellenőrzése, hogy a folyamatkiszolgáló képes tooconnect tooAzure nyilvános IP-cím 443-as porton**

Nyissa meg a legújabb CBEngineCurr.errlog hello `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` keresse meg a: 443-as és a kapcsolódási kísérlet sikertelen volt.

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/logdetails1.png)

Ha problémák vannak, a Folyamatkiszolgáló parancssorból, használja a telnet tooping a Azure nyilvános IP-cím (maszkolva a fenti kép) hello CBEngineCurr.currLog a 443-as porton.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Ha nem tooconnect, majd ellenőrizze meg, hogy hello hozzáférési probléma miatt toofirewall vagy a Proxy a következő lépésben leírtak szerint.


* **Ellenőrizze, hogy ha a folyamatkiszolgáló IP cím alapú tűzfal nem blokkolja-e hozzáférési**: Ha egy IP-címeken alapuló tűzfalszabályok szabályokat használ hello kiszolgálón, majd töltse le hello teljes listája a Microsoft Azure Datacenter IP-címtartományok a [Itt ](https://www.microsoft.com/download/details.aspx?id=41653) tooyour tűzfal konfigurációs tooensure kommunikációs tooAzure (és hello HTTPS (443) port) lehetővé teszik adja hozzá.  Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).

* **Ellenőrizze, hogy ha folyamatkiszolgáló URL-alapú tűzfal nem blokkolja a hozzáférést**: Ha egy URL-cím alapú tűzfalszabályok hello kiszolgálón használ, győződjön meg arról, a következő URL-címek hello kerülnek toofirewall konfigurációs. 
     
  `*.accesscontrol.windows.net:` hozzáférés-vezérléshez és identitáskezeléshez

  `*.backup.windowsazure.com:` replikációs adatátvitelhez és vezényléshez

  `*.blob.core.windows.net:`Hozzáférés toohello használt tárfiókot, amely tárolja a replikált adatok

  `*.hypervrecoverymanager.windowsazure.com:` replikációkezelési műveletekhez és vezényléshez

  `time.nist.gov`és `time.windows.com`: használt toocheck időszinkronizálást a rendszer és a globális időhöz között.

URL-címéből **Azure Government felhő**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* **Ellenőrizze, hogy ha a proxykiszolgáló beállításait a folyamatkiszolgáló nem blokkolja-e hozzáférési**.  Ha proxykiszolgálót használ, győződjön meg arról, hello DNS-kiszolgáló megoldása, hello proxykiszolgáló nevét.
toocheck hello konfigurációs kiszolgáló telepítése során van megadva. Nyissa meg tooregistry kulcs

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Most győződjön meg arról, hogy hello Azure Site Recovery ügynök toosend adatok használja ugyanazokat a beállításokat.
Keresés a Microsoft Azure Backup szolgáltatás 

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/mab.png)

Nyissa meg azt, majd kattintson a művelet > tulajdonságainak módosítása. A proxykonfiguráció lapon megtekintheti az hello proxy címe, amely hello beállításjegyzék-beállítások eredményobjektumokról azonosnak kell lennie. Ha nem, változtassa meg a toohello ugyanazt a címet.

![A replikáció engedélyezése](./media/site-recovery-protection-common-errors/mabproxy.png)

* **Ellenőrizze, ha a sávszélesség-szabályozás nem korlátozott folyamatkiszolgáló**: hello sávszélesség növelése, és ellenőrizze, hogy hello a probléma továbbra is léteznek.

##<a name="next-steps"></a>Következő lépések
Ha további segítségre van szüksége, majd indítsa el a lekérdezés túl[ASR fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Egy aktív közösségi van, és a mérnökök egyik képes tooassist meg.
