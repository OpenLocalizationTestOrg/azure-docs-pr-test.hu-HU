---
title: "Az Azure Backup Server hibaelhárítása |} Microsoft Docs"
description: "Végezzen hibaelhárítást a regisztráció az Azure Backup Server, és a biztonsági mentés és visszaállítás az alkalmazások és szolgáltatások telepítése."
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: e9517672138a4ea7577af1295dea13771733983e
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/13/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Az Azure Backup Server hibaelhárítása

Olvassa el az alábbi táblázatban, amely az Azure Backup Server használata során előforduló hibák elhárítása.

## <a name="invalid-vault-credentials-provided"></a>Megadott érvénytelen tárolói hitelesítő adatok 

A probléma megoldásához kövesse az [ezek a hibaelhárítási lépéseket](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues).

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>Az ügynök művelete sikertelen volt a DPM ügynök-koordinátor szolgáltatást a kiszolgálón kommunikációs hiba miatt 

A probléma megoldásához kövesse az [ezek a hibaelhárítási lépéseket](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues). 

## <a name="setup-could-not-update-registry-metadata"></a>A telepítő nem tudta frissíteni a beállításjegyzék-metaadatok

A probléma megoldásához kövesse az [ezek a hibaelhárítási lépéseket](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#installation-issues).




## <a name="installation-issues"></a>Telepítési problémák

| Művelet | Hiba részletei | Megkerülő megoldás |
|-----------|---------------|------------|
|Telepítés | A telepítő nem tudta frissíteni a beállításjegyzék-metaadatok. A frissítés hiba előfordulhat, hogy a tárhelyhasználat overusage. Ennek elkerülése érdekében a refs fájlrendszer díszítésre bejegyzés frissítése. | A beállításkulcs beállítása **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. Állítsa a Dword értékét 1. |
|Telepítés | A telepítő nem tudta frissíteni a beállításjegyzék-metaadatok. A frissítés hiba előfordulhat, hogy a tárhelyhasználat overusage. Ennek elkerülése érdekében a kötet SnapOptimization bejegyzés frissítése. | A beállításkulcs létrehozása **SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds** az üres karakterlánc. |
## <a name="registration-and-agent-related-issues"></a>Regisztráció és ügynök kapcsolatos problémák
| Művelet | Hiba részletei | Megkerülő megoldás |
| --- | --- | --- |
| A tárolóba való regisztrálása | Érvénytelen megadott tárolói hitelesítő adatok. A fájl sérült, vagy nem nem rendelkezik a legújabb hitelesítő adatokat a helyreállítási szolgáltatáshoz tartozó. | Ez a hiba elhárításához: <ul><li> Töltse le a legfrissebb hitelesítőadat-fájlt a tárolóból, és próbálkozzon újra. <br>(VAGY)</li> <li> Ha az előző művelet nem működik, próbálja meg letölteni a hitelesítő adatokat egy másik helyi könyvtárba, vagy hozzon létre egy új tárolót. <br>(VAGY)</li> <li> Próbálja meg frissíteni a dátum és idő beállítása a [ebben a blogban](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>(VAGY)</li> <li> Ellenőrizze, hogy rendelkezik-e a c:\windows\temp több mint 65000 fájlok. Elavult fájlok áthelyezése egy másik helyre, vagy törli az ideiglenes mappában található elemeket. <br>(VAGY)</li> <li> Tekintse meg a tanúsítványokat. <br> a. Nyissa meg **számítógép-tanúsítványok kezelése** (a Vezérlőpulton). <br> b. Bontsa ki a **személyes** csomópontot és annak gyermek csomópont **tanúsítványok**.<br> c.  Távolítsa el a tanúsítványt **Windows Azure eszközök**. <br> d. Ismételje meg a regisztráció az Azure Backup-ügyfél. <br> (VAGY) </li> <li> Ellenőrizze, hogy ha a csoportházirend érvényben van-e. </li></ul> |
| Az ügynököket a védett kiszolgálók küldését | Az ügynök művelete sikertelen volt a DPM-Ügynökkoordinátor szolgáltatással kommunikációs hiba miatt \<kiszolgáló_neve >. | **Ha a javasolt művelet található a termék nem működik, a következő lépésekkel**: <ul><li> Ha egy számítógép nem megbízható tartományból konfigurálhatóak, kövesse az [ezeket a lépéseket](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx). <br> (VAGY) </li><li> Ha a számítógép megbízható tartományhoz csatlakoztatás, hibáinak elhárítása az itt leírt lépésekkel [ebben a blogban](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>(VAGY)</li><li> Próbálja letiltása víruskereső hibaelhárítás céljából. Ha ez megoldja a problémát, mint a víruskereső beállításainak módosítása [Ez a cikk](https://technet.microsoft.com/library/hh757911.aspx).</li></ul> |
| Az ügynököket a védett kiszolgálók küldését | A kiszolgálóhoz megadott hitelesítő adatok érvénytelenek. | **Ha a javasolt művelet található a termék nem működik, a következő lépésekkel**: <br> Próbálja meg a védelmi ügynök kézi telepítése az üzemi kiszolgálón megadott [Ez a cikk](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual).|
| Az Azure Backup szolgáltatás ügynöke nem tudott kapcsolódni az Azure Backup szolgáltatáshoz (azonosító: 100050) | Az Azure Backup szolgáltatás ügynökének nem sikerült csatlakozni az Azure Backup szolgáltatás volt. | **Ha a javasolt művelet található a termék nem működik, a következő lépésekkel**: <br>1. A következő parancsot egy rendszergazda jogú parancssorból: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Ekkor megnyílik az Internet Explorer-ablakban. <br/> 2. Ugrás a **eszközök** > **Internetbeállítások** > **kapcsolatok** > **LAN-beállítások**. <br/> 3. Ellenőrizze a proxybeállítások helyességét a rendszerfiók. Állítsa be a Proxy IP-cím és port. <br/> 4. Zárja be az Internet Explorert.|
| Az Azure Backup szolgáltatás ügynökének telepítése nem sikerült | A Microsoft Azure Recovery Services telepítése nem sikerült. A rendszer a Microsoft Azure Recovery Services-telepítés által végrehajtott összes módosítás vissza lett állítva. (AZONOSÍTÓ: 4024) | Azure-ügynök manuális telepítése.


## <a name="configuring-protection-group"></a>Védelmi csoport konfigurálása
| Művelet | Hiba részletei | Megkerülő megoldás |
| --- | --- | --- |
| Védelmi csoportok konfigurálása | A DPM nem tudta felsorolni az alkalmazás-összetevő a a védett számítógép (védett számítógép neve). | Válassza ki **frissítése** konfigurálása védelmi csoport felhasználói felület képernyőn a megfelelő adatforrás/összetevő szinten. |
| Védelmi csoportok konfigurálása | Nem sikerült beállítani a védelmet | Ha a védett kiszolgáló egy SQL server, győződjön meg arról, hogy a sysadmin (rendszergazda) szerepkör-engedélyek vannak-e megadva a védett számítógépen a system fióknak (NTAuthority\System) leírtak szerint [Ez a cikk](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx).
| Védelmi csoportok konfigurálása | Nincs elegendő szabad lemezterület a tárolókészletben a védelmi csoport számára. | A lemezeket a tárolókészlethez hozzáadott [nem tartalmazhatja a partíció](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Törölni egyetlen meglévő kötetet a lemezen. Vegye fel a tárolókészletbe.|
| Házirend módosítása |A biztonsági mentési házirend nem módosítható. Hiba: Az aktuális művelet [0x29834] belső szolgáltatási hiba miatt sikertelen volt. Némi várakozás után próbálja megismételni a műveletet. Ha a probléma továbbra is fennáll, forduljon a Microsoft támogatási szolgálatához. |**OK:**<br/>Ez a hiba akkor fordul elő, a három feltétel: Ha biztonsági beállítások engedélyezve vannak, csökkenteni a megőrzési időtartam alatt a korábban megadott minimális értékek meg, és ha nem támogatott verzióját. (Nem támogatott verziói megegyeznek alatt a Microsoft Azure Backup Server verziója 2.0.9052 és az Azure Backup Server frissítési 1.) <br/>**Javasolt művelet:**<br/> -Házirendekkel kapcsolatos célgyűjteményét folytatásához beállítani a megőrzési idő, a megadott időszak minimális megőrzési fent. (Legalább egy hét nap napi, heti, három hét szükséges négy hétig havonta vagy egy év éves.) <br><br>Szükség esetén másik az előnyben részesített módszer az, hogy frissítse a biztonságimásolat-készítő ügynök és az Azure Backup Server kihasználhatják a biztonsági frissítéseket. |

## <a name="backup"></a>Biztonsági mentés
| Művelet | Hiba részletei | Megkerülő megoldás |
| --- | --- | --- |
| Biztonsági mentés | A replika inkonzisztens | Győződjön meg arról, hogy az a védelmi csoport varázslóban automatikus konzisztencia-ellenőrzés beállítás be van-e kapcsolva. A replika inkonzisztenssé és vonatkozó javaslatok a okok kapcsolatos további információkért lásd: a Microsoft TechNet cikket [a replika inkonzisztens](https://technet.microsoft.com/library/cc161593.aspx).<br> <ol><li> Rendszerállapot/operációs rendszer nélküli biztonsági mentés esetén győződjön meg arról, hogy a Windows Server biztonsági másolat telepítve van a védett kiszolgálón.</li><li> Ellenőrizze a hely kapcsolatos problémákat a a DPM-tárolókészlethez, a DPM vagy a Microsoft Azure biztonsági mentési kiszolgálón, és szükség szerint tárolót foglalni.</li><li> Ellenőrizze a kötet árnyékmásolata szolgáltatás a védett kiszolgáló állapotát. Ha egy letiltott állapotban van, állítsa be úgy, hogy indítsa el kézzel. Indítsa el a szolgáltatást a kiszolgálón. Ezután térjen vissza a DPM vagy a Microsoft Azure Backup Server konzolt, és elindítja a szinkronizálást és konzisztencia-ellenőrzést.</li></ol>|
| Biztonsági mentés | Váratlan hiba történt a feladat futása közben. Az eszköz nem áll készen. | **Ha a javasolt művelet található a termék nem működik, a következő lépéseket:** <br> <ul><li>Állítsa be az árnyékmásolat-tároló terület a védelmi csoportban található elemek a korlátlanra, és futtassa a konzisztencia-ellenőrzést.<br></li> (VAGY) <li>Próbálja meg törölni a meglévő védelmi csoportot, és több új csoportok létrehozása. Minden egyes új védelmi csoport egyetlen elemet kell azt.</li></ul> |
| Biztonsági mentés | Ha biztonsági mentést csak a rendszerállapotról, győződjön meg arról, hogy van-e elég szabad hely a védett számítógépen a rendszerállapot biztonsági mentésének tárolásához. | <ol><li>Győződjön meg arról, hogy a Windows Server biztonsági másolat telepítve van-e a védett számítógépen.</li><li>Győződjön meg arról, hogy van-e elég hely a védett számítógépen, a rendszer állapotát. A legegyszerűbben úgy, hogy e gomba a védett számítógépen nyissa meg a Windows Server biztonsági másolat, a megadott beállítások haladjon végig a, és válassza ki a BMR. A felhasználói felület majd megtudhatja, hogy mennyi helyre szükség. Nyissa meg **WSB** > **helyi biztonsági másolat** > **biztonsági mentési ütemezés** > **válassza ki a biztonsági mentés konfigurációja**  >  **Teljes kiszolgáló** (méret jelenik meg). Ez a méret használni az ellenőrzéshez.</li></ol>
| Biztonsági mentés | Online helyreállítási pont létrehozása nem sikerült | Ha a hibaüzenet szerint "a Windows Azure Backup szolgáltatás ügynöke nem tudta létrehozni a pillanatképet készítenie a kijelölt kötetről" próbálja növelni a területet az replika és a helyreállítási pont kötetén.
| Biztonsági mentés | Online helyreállítási pont létrehozása nem sikerült | Ha a hibaüzenet szerint "A Windows Azure Backup szolgáltatás ügynökének nem lehet csatlakozni az OBEngine szolgáltatáshoz", győződjön meg arról, hogy az OBEngine létezik-e a számítógépen futó szolgáltatások közül. Ha az OBEngine szolgáltatás nem fut, a "net start OBEngine" parancs segítségével indítsa el az OBEngine szolgáltatáshoz.
| Biztonsági mentés | Online helyreállítási pont létrehozása nem sikerült | Ha a hibaüzenet szerint "a titkosítási jelszót ehhez a kiszolgálóhoz nincs beállítva. Állítsa be a titkosítás jelszavát,"próbálja konfigurálása a titkosítás jelszavát. Ha nem sikerül, a következő lépéseket: <br> <ol><li>Győződjön meg arról, hogy létezik-e az ideiglenes helyet. Ez az a hely a beállításjegyzékben szereplő **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config**, nevű **ScratchLocation** léteznie kell.</li><li> Ha ideiglenes helyét, próbálja újból regisztrálja a régi jelszó használatával. *Amikor konfigurál egy titkosítási jelszót, mentse egy biztonságos helyre.*</li><ol>
| Biztonsági mentés | A BMR biztonsági mentés hibája | Ha az operációs rendszer nélküli Helyreállítás mérete nagy, helyezze át néhány alkalmazás fájlt az operációs rendszer meghajtóra, majd próbálja megismételni. |
| Biztonsági mentés | A beállítás új kiszolgálón a Microsoft Azure biztonsági mentés VMware virtuális gép védelmének újbóli beállításához nem szerepelnek a elérhetőként hozzáadásához. | Microsoft Azure Backup Server régi, elavult példányának VMware tulajdonságainak vannak hivatkozott. A probléma megoldásához:<br><ol><li>A VCenter (SC-VMM egyenértékű), válassza a **összegzés** fülre, majd a **egyéni attribútumok**.</li>  <li>Törölje a régi, a Microsoft Azure Backup Server nevet a **DPMServer** érték.</li>  <li>Az új Microsoft Azure biztonsági mentés kiszolgálóra visszalép és módosítja a old.  Miután kiválasztotta a **frissítése** védelmét hozzáadandó elérhetőként jelölőnégyzettel megjelenik a virtuális gép gomb.</li></ol> |
| Biztonsági mentés | Hiba történt a megosztott fájlok/mappák használata közben: | Próbálja meg módosítani a víruskereső beállításokat a TechNet-cikkben leírtak [víruskereső szoftver futtatása a DPM-kiszolgálón](https://technet.microsoft.com/library/hh757911.aspx).|
| Biztonsági mentés | VMware virtuális gép online helyreállítási pontjainak létrehozási feladatai sikertelenek. A DPM hibába ütközött a VMware változáskövetés információk beolvasásának megkísérlése során. Hibakód - FileFaultFault (azonosító: 33621) |  <ol><li> Alaphelyzetbe állítja a VMware rendszerben CTK az érintett virtuális gépekhez.</li> <li>Ellenőrizze, hogy a független lemez nem VMware helyen vannak.</li> <li>Állítsa le az érintett virtuális gépek védelmét, és állítsa be újra a védelmét a **frissítése** gombra. </li><li>Futtassa egy másolatot kap az érintett virtuális gépekhez.</li></ol>|


## <a name="change-passphrase"></a>Jelszó módosítása
| Művelet | Hiba részletei | Megkerülő megoldás |
| --- | --- | --- |
| Jelszó módosítása |A biztonsági megadott PIN-kód nem megfelelő. Adja meg a megfelelő biztonsági PIN-kód a művelet végrehajtásához. |**OK:**<br/> Ez a hiba akkor fordul elő, amikor megadja egy érvénytelen, vagy biztonsági PIN-kód lejárt (például a jelszó módosítása) a kritikus műveletet hajt végre. <br/>**Javasolt művelet:**<br/> A művelet végrehajtásához meg kell adnia egy érvényes biztonsági PIN-kódot. Ahhoz, hogy a PIN-kódot, jelentkezzen be az Azure-portálon, és navigáljon a Recovery Services-tároló. Ezután lépjen **beállítások** > **tulajdonságok** > **biztonsági PIN-kód készítése**. A PIN-kód segítségével módosíthatja a jelszót. |
| Jelszó módosítása |A művelet sikertelen volt. AZONOSÍTÓ: 120002 |**OK:**<br/>Ez a hiba akkor fordul elő, ha a biztonsági beállítások engedélyezve vannak, vagy ha megpróbálja módosítani a jelszót, amikor nem támogatott verzióját használja.<br/>**Javasolt művelet:**<br/> A jelszó módosításához először frissítenie kell a biztonságimásolat-készítő ügynök minimális verziójára, amely 2.0.9052. Szükség Azure Backup Server frissítése a 1-es frissítés a minimális, és adja meg egy érvényes biztonsági PIN-kódot. Ahhoz, hogy a PIN-kódot, jelentkezzen be az Azure-portálra, és navigáljon a Recovery Services-tároló. Ezután lépjen **beállítások** > **tulajdonságok** > **biztonsági PIN-kód készítése**. A PIN-kód segítségével módosíthatja a jelszót. |


## <a name="configure-email-notifications"></a>E-mail értesítések beállítása
| Művelet | Hiba részletei | Megkerülő megoldás |
| --- | --- | --- |
| Egy olyan Office 365-fiókkal e-mail értesítések beállítása |Hibaazonosító: 2013| **OK:**<br> Próbálja használni az Office 365-fiókkal <br>**Javasolt művelet:**<ol><li> A rendszer a legfontosabb győződjön meg arról, hogy "Engedélyezése névtelen továbbító a egy fogadási összekötőn" a DPM-kiszolgáló be van állítva az Exchange. A fentiek konfigurálásával kapcsolatos további információkért lásd: [névtelen továbbítási engedélyezése a fogadási összekötőn](http://technet.microsoft.com/en-us/library/bb232021.aspx) a TechNet webhelyen.</li> <li> Ha nem használ egy belső SMTP-továbbító, és be kell állítania az Office 365-kiszolgálóval, állíthatja be az IIS kell lennie a továbbító. A DPM-kiszolgáló konfigurálása [továbbítják az IIS-sel o365 SMTP](https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx).<br><br> **Fontos:** használjon a user@domain.com formátum és *nem* tartomány\felhasználó.<br><br><li>Használja a helyi kiszolgáló nevét SMTP-kiszolgáló, a DPM pont 587 portot. Ezt követően irányítsa azt a felhasználói e-mailek, az e-mailt kell származnia.<li> A felhasználónevet és jelszót a DPM SMTP beállítási oldalon kell lennie egy olyan tartományi fiók, amely a DPM a tartományban. </li><br> **Megjegyzés:**: az SMTP-kiszolgáló címére módosítja, ha a módosítást az új beállítások, és a beállítások bezárásához, majd nyissa meg újra, és ellenőrizze, hogy az új érték tükrözi.  Egyszerűen módosítása és tesztelése nem mindig okozhat az új beállítások érvényesítéséhez, így a legjobb vizsgálja, hogy így.<br><br>A folyamat során bármikor törölheti ezeket a beállításokat a DPM konzol bezárása, és a következő beállításkulcsok szerkesztésével: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword törlése és SMTPUserName kulcsok**. Később is hozzáadhatja a felhasználói felület a amikor újra elindíthatja.
