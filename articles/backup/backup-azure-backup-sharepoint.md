---
title: "aaaDPM vagy az Azure Backup server védelméről a SharePoint-farm tooAzure |} Microsoft Docs"
description: "Ez a cikk egy SharePoint-farm tooAzure DPM vagy az Azure Backup server védelmének áttekintése"
services: backup
documentationcenter: 
author: adigan
manager: Nkolli1
editor: 
ms.assetid: e0c0c252-dc1d-4072-b777-7222c13950b0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: adigan;giridham;jimpark;trinadhk;markgal
ms.openlocfilehash: 726d59320b8d9f14b38e0f041308019eebcfb77b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-sharepoint-farm-tooazure"></a>Készítsen biztonsági másolatot egy SharePoint-farm tooAzure
Biztonsági másolatot egy SharePoint-farm tooMicrosoft Azure mennyi hello a System Center Data Protection Manager (DPM) használatával biztonsági másolatot készíteni más adatforrások ugyanúgy. Azure biztonsági mentés napi, heti, havi vagy éves biztonsági mentési pontok hello biztonsági mentés ütemezése toocreate rugalmasságot biztosít, és lehetővé teszi az adatmegőrzési házirend-beállítások a különféle biztonsági mentési pontok. A DPM hello funkció toostore helyi lemez másolatok biztosít gyors helyreállítási idő célkitűzés (RTO) és toostore másolja át a gazdaságos, hosszú távú megőrzési tooAzure.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint támogatott verziója, és a kapcsolódó védelmi forgatókönyvek
Azure biztonsági mentés a DPM támogatja a következő forgatókönyvek hello:

| Számítási feladat | Verzió | A SharePoint központi telepítés | DPM-telepítés típusa | DPM – System Center 2012 R2 | Védelem és helyreállítás |
| --- | --- | --- | --- | --- | --- |
| SharePoint |A SharePoint 2013, SharePoint 2007, 2010 SharePoint SharePoint 3.0 |A fizikai kiszolgálóként vagy Hyper-V vagy VMware virtuális gépeket telepített SharePoint <br> -------------- <br> Az SQL AlwaysOn |Fizikai kiszolgáló vagy a helyszíni Hyper-V virtuális gép |Támogatja a biztonsági mentési tooAzure az 5. kumulatív |Helyreállítási beállítások SharePoint-Farm védelme: a helyreállítási farm, adatbázis és a lemezes helyreállítási pontok a fájl vagy listaelem.  Adatbázis és a farm helyreállítási Azure helyreállítási pontokból. |

## <a name="before-you-start"></a>Előkészületek
Néhány dolgot előtt készítsen biztonsági másolatot egy SharePoint-farm tooAzure tooconfirm kell.

### <a name="prerequisites"></a>Előfeltételek
Mielőtt továbblépne, győződjön meg arról, hogy teljesül-e az összes hello [a Microsoft Azure Backup előfeltételei](backup-azure-dpm-introduction.md#prerequisites) tooprotect munkaterhelések. Egyes feladatok Előfeltételek a következők: hozzon létre egy mentési tárolót, töltse le a tárolói hitelesítő adatokat, telepítse az Azure Backup szolgáltatás ügynöke és regisztrálja a DPM vagy az Azure Backup Server hello tárolóban.

### <a name="dpm-agent"></a>A DPM-ügynök
hello a DPM-ügynök hello futtató kiszolgáló esetében SharePoint, az SQL Server rendszert futtató kiszolgálók hello és egyéb hello SharePoint-farm részét képező kiszolgálók telepítve kell lennie. További információ tooset be hello védelmi ügynököt, lásd: [telepítő védelmi ügynök](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  hello egyetlen kivétel ez alól, hogy hello ügynököt telepít csak egy egyetlen (WFE) előtér-webkiszolgálóján. DPM-nek egy előtér-Webkiszolgálón kiszolgáló csak tooserve hello ügynök védelemre hello belépési pontként.

### <a name="sharepoint-farm"></a>SharePoint-farm
A hello farm minden 10 millió eleme legalább 2 GB lemezterület hello köteten, ahol hello DPM mappa kell lennie. Ez a terület szükség a katalógus előállításához. DPM toorecover elemeket (webhelycsoportok, webhelyek, listák, dokumentumtárak, mappák, egyes dokumentumok és listaelemek), a katalógus-előállítás hoz létre minden egyes tartalom-adatbázist belüli hello URL-címek listáját. Megtekintheti az URL-listák hello hello helyreállítható elem paneljén hello **helyreállítási** feladat a DPM felügyeleti konzol felügyelet munkaterületén.

### <a name="sql-server"></a>SQL Server
A DPM fut, a LocalSystem fiókra. SQL Server-adatbázisok tooback, DPM-nek fiók rendszergazdai jogosultságokkal az SQL Servert futtató kiszolgáló hello. Állítsa be a NT AUTHORITY\SYSTEM túl*sysadmin* hello kiszolgálón, amelyen SQL Server előtt készítsen biztonsági másolatot.

Ha hello SharePoint-farm SQL Server-aliasokkal használt konfigurált SQL Server-adatbázisok, telepítse a hello SQL Server ügyféloldali összetevőit hello előtér-webkiszolgálón, amely a DPM védeni fogja.

### <a name="sharepoint-server"></a>SharePoint Server
Amíg teljesítmény például a SharePoint-farm méret számos tényezőtől függ, általános útmutatásként egy DPM-kiszolgáló képes 25 TB SharePoint-farm védelméhez.

### <a name="dpm-update-rollup-5"></a>A DPM kumulatív frissítés 5
egy SharePoint-farm tooAzure toobegin védelméről, kell tooinstall DPM kumulatív frissítés 5 vagy újabb. 5. kumulatív frissítés megoldással hello képességét tooprotect egy SharePoint-farm tooAzure hello farm SQL AlwaysOn használatára van konfigurálva.
További információkért lásd: hello szolgáltatásban által írt blogbejegyzés [DPM kumulatív frissítés 5](http://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### <a name="whats-not-supported"></a>Nem támogatott műveletek
* A DPM által védett SharePoint-farm nem nyújt védelmet a keresési indexek vagy szolgáltatás adatbázisai. Ezek az adatbázisok védelméről tooconfigure hello külön-külön kell.
* A DPM nem ad üzemeltetett SharePoint SQL Server-adatbázisok biztonsági mentése (SOFS) kibővíthető fájlkiszolgáló-megosztásokat.

## <a name="configure-sharepoint-protection"></a>SharePoint-védelem beállítása
A DPM tooprotect SharePoint használata előtt konfigurálnia kell hello SharePoint VSS-író szolgáltatást (WSS-író szolgáltatás) használatával **ConfigureSharePoint.exe**.

Található **ConfigureSharePoint.exe** hello [DPM telepítési útvonalán] \bin mappában hello előtér-webkiszolgálón. Ez az eszköz biztosít hello SharePoint-farm hello védelmi ügynök hello hitelesítő adatokkal. Futtatja egy WFE-kiszolgálón. Ha több ELŐTÉR-webkiszolgáló, csak egyet válasszon ki egy védelmi csoport konfigurálásakor.

### <a name="tooconfigure-hello-sharepoint-vss-writer-service"></a>tooconfigure hello SharePoint VSS-író szolgáltatás
1. Hello ELŐTÉR-webkiszolgálón, a parancssorban lépjen túl [DPM telepítési helye] \bin\
2. Adja meg a ConfigureSharePoint - EnableSharePointProtection.
3. Adja meg a hello farm rendszergazdai hitelesítő adatait. Ez a fiók tagja a helyi rendszergazdák csoportjának hello hello ELŐTÉR-webkiszolgálón kell lennie. Ha hello farm rendszergazdája nem egy helyi rendszergazda grant hello alábbi engedélyek hello ELŐTÉR-webkiszolgálón:
   * Hello WSS_Admin_WPG csoportnak teljes hozzáférés biztosítása toohello DPM mappájához (% Program Files%\Microsoft Data Protection Manager\DPM).
   * Adja meg a hello WSS_Admin_WPG csoportnak olvasási hozzáférés toohello DPM beállításkulcsot (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).

> [!NOTE]
> Toorerun ConfigureSharePoint.exe lesz szüksége, amikor megváltozik a hello SharePoint-farm rendszergazdai hitelesítő adatait.
> 
> 

## <a name="back-up-a-sharepoint-farm-by-using-dpm"></a>A DPM biztonsági mentése egy SharePoint-farm
Miután konfigurálta a DPM és a SharePoint-farm, amint azt korábban hello, a SharePoint védelme a DPM által.

### <a name="tooprotect-a-sharepoint-farm"></a>egy SharePoint-farm tooprotect
1. A hello **védelmi** hello DPM felügyeleti konzol lapján kattintson a **új**.
    ![Új védelem lap](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. A hello **védelmi csoport típusának kiválasztása** hello oldalán **új védelmi csoport létrehozása** varázsló, jelölje be **kiszolgálók**, és kattintson a **következő** .
   
    ![Válassza ki a védelmi csoport típusa](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. A hello **csoporttagok kiválasztása** képernyő, jelölje be hello jelölőnégyzetet hello SharePoint server tooprotect ki, majd kattintson a **következő**.
   
    ![Csoporttagok kiválasztása](./media/backup-azure-backup-sharepoint/select-group-members2.png)
   
   > [!NOTE]
   > Hello DPM-ügynököt futtató lásd: hello server hello varázslóban. A DPM is látható struktúrája. ConfigureSharePoint.exe futtatta, mert a DPM kommunikál a hello SharePoint VSS-író szolgáltatás és a megfelelő SQL Server-adatbázisok és hello SharePoint-farm struktúráját felismeri, hello tartozó megfelelő elemeit, valamint a tartalom-adatbázisok.
   > 
   > 
4. A hello **adatvédelmi módszer kiválasztása** lapján adjon meg hello hello **védelmi csoport**, és válassza ki a kívánt *védelmi módszert*. Kattintson a **Tovább** gombra.
   
    ![Adatvédelmi módszer kiválasztása](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)
   
   > [!NOTE]
   > hello lemez védelmi módszert toomeet rövid helyreállítási idő célok segítségével. Azure egy gazdaságos, hosszú távú védelmi cél képest tootapes. További információkért lásd: [használata Azure biztonsági mentés tooreplace a szalag infrastruktúra](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)
   > 
   > 
5. A hello **rövid távú célok megadása** lapon, válassza ki a kívánt **megőrzési időtartam** , és azonosíthatja, ha azt szeretné, hogy a biztonsági mentések toooccur.
   
    ![Rövid távú célok megadása](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)
   
   > [!NOTE]
   > Általában szükség, amely öt napnál régebbi adatok, mert jelenleg kijelölt lemezen öt napnyi megőrzési időtartamot, és biztosítani a biztonsági mentését végző hello történik, nem éles órában, ehhez a példához.
   > 
   > 
6. Tekintse át a hello tárolási tárolókészletben lefoglalt lemezterületet hello védelmi csoporthoz, majd kattintson **következő**.
7. Az összes védelmi csoportra a DPM szabad terület toostore foglal le, és a replikák kezelése. A DPM ezen a ponton hello kiválasztott adatok másolatának kell létrehoznia. Válassza ki, hogyan és mikor szeretne létrehozni hello replika, és kattintson **következő**.
   
    ![A replika-létrehozási módszer kiválasztása](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)
   
   > [!NOTE]
   > toomake meg arról, hogy a hálózati forgalom nem történik meg, válassza ki a megfelelő üzemi a munkaidőn kívül.
   > 
   > 
8. A DPM érdekében végezzen konzisztencia-ellenőrzést a replika hello az adatok integritását biztosítja. Kétféleképpen érhető el. Megadhat egy ütemezést toorun konzisztencia-ellenőrzéseket, vagy a DPM is végezzen konzisztencia-ellenőrzést automatikusan hello replikán, amikor inkonzisztenssé válik. Válassza ki a kívánt beállítást, és kattintson a **következő**.
   
    ![Konzisztencia-ellenőrzés](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. A hello **Online védelem adatainak megadása** lapon, válassza ki, hogy szeretné, hogy tooprotect, és kattintson a hello SharePoint-farm **következő**.
   
    ![A DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. A hello **Online biztonsági mentési ütemezés megadása** lapon válassza ki a kívánt ütemezést, és kattintson a **következő**.
    
    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)
    
    > [!NOTE]
    > A DPM legfeljebb két napi biztonsági mentések tooAzure különböző időpontokban biztosít. Azure biztonsági mentés is meghatározhatja, alkalmas a biztonsági másolatok maximális és kevesen a WAN sávszélesség mennyiségét hello [Azure biztonsági mentési hálózati sávszélesség-szabályozás](https://azure.microsoft.com/en-in/documentation/articles/backup-configure-vault/#enable-network-throttling).
    > 
    > 
11. Attól függően, hogy a biztonsági mentés ütemezése hello kiválasztott, hello **Online adatmegőrzési szabály megadása** lapon, jelölje be hello adatmegőrzési napi, heti, havi vagy éves biztonsági mentési pontok.
    
    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)
    
    > [!NOTE]
    > A DPM egy szerzett-édesapja-fia megőrzési séma, amelyben a különböző adatmegőrzési választható ki a másik biztonsági mentési pontok használja.
    > 
    > 
12. Hasonló toodisk kezdeti hivatkozás pont replikájának kell az Azure-ban létrehozott toobe. Válassza ki a kívánt beállítást toocreate egy kezdeti biztonsági másolatot tooAzure, és kattintson a **következő**.
    
    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Tekintse át a beállításokat a hello **összegzés** lapon, majd **csoport létrehozása**. Látni fogja a sikerről szóló üzenetet hello védelmi csoport létrehozása után.
    
    ![Összefoglalás](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-dpm"></a>Egy SharePoint-elem visszaállítása a lemezről DPM használatával
A következő példa hello, hello *helyreállítás SharePoint-elem* véletlenül törölve lett, és a helyreállított toobe kell.
![A DPM SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Nyissa meg hello **DPM felügyeleti konzol**. Minden DPM által védett SharePoint-farmok megjelennek-e hello **védelmi** fülre.
   
    ![A DPM SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. toobegin toorecover hello elem, jelölje be hello **helyreállítási** fülre.
   
    ![A DPM SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. A SharePoint kereshet *helyreállítás SharePoint-elem* kereséssel egy helyettesítő karakteres alapú belül egy helyreállítási pontot a tartományon.
   
    ![A DPM SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Válassza ki a megfelelő helyreállítási pont hello hello a keresési eredmények, kattintson a jobb gombbal a hello elemet, és válassza **helyreállítása**.
5. Tallózzon a különböző helyreállítási pontok is, majd válassza ki az adatbázis vagy elem toorecover. Válassza ki **dátum > helyreállításkor**, majd válassza ki a megfelelő hello **adatbázis > SharePoint-farm > helyreállítási pont > elem**.
   
    ![A DPM SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Kattintson a jobb gombbal a hello elemet, majd válassza ki **helyreállítása** tooopen hello **helyreállítási varázsló**. Kattintson a **Tovább** gombra.
   
    ![Helyreállítási beállítások áttekintése](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Válassza ki, hogy szeretné, hogy tooperform, és kattintson a helyreállítási hello típusú **következő**.
   
    ![Helyreállítási típus](./media/backup-azure-backup-sharepoint/select-recovery-type.png)
   
   > [!NOTE]
   > a kijelölés hello **toooriginal helyreállítása** hello példa hello elem toohello eredeti SharePoint hely helyreállítására szolgál..
   > 
   > 
8. Jelölje be hello **helyreállítási folyamat** , amelyet az toouse.
   
   * Válassza ki **helyreállítás helyreállítási farm nélkül** Ha hello SharePoint-farm nem változott, és megegyezik az hello helyreállítási pont, amely hello visszaállítása folyamatban van.
   * Válassza ki **helyreállításához a helyreállítási farm** Ha hello SharePoint-farm megváltozott a hello helyreállítási pont létrehozása óta.
     
     ![A helyreállítási folyamat](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Adjon meg egy átmeneti SQL Server példány hely toorecover hello adatbázis átmenetileg, és adjon meg egy átmeneti fájlmegosztást hello DPM-kiszolgáló és a hello futtató kiszolgáló esetében SharePoint toorecover hello elem.
   
    ![Átmeneti Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)
   
    A DPM csatlakoztatja hello tartalom-adatbázist, amelyen az hello SharePoint elem toohello ideiglenes SQL Server-példány. Hello tartalom-adatbázist, a hello DPM-kiszolgáló hello elem állítja helyre, és átmeneti tárolási helye a DPM-kiszolgáló hello hello helyezi. hello helyreállított elem, amely a most átmeneti helyre a DPM-kiszolgáló hello hello kell exportált toobe toohello átmeneti helyre hello SharePoint-farm.
   
    ![Átmeneti Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Válassza ki **helyreállítási beállítások megadása**, és alkalmazza a biztonsági beállítások toohello SharePoint-farm vagy hello hello helyreállítási pont biztonsági beállításainak alkalmazása. Kattintson a **Tovább** gombra.
    
    ![Helyreállítási beállítások](./media/backup-azure-backup-sharepoint/recovery-options.png)
    
    > [!NOTE]
    > Kiválaszthatja a toothrottle hello sávszélesség-használat. Ez minimalizálja a hatás toohello az üzemi kiszolgáló éles órában.
    > 
    > 
11. Tekintse át hello összefoglaló információit, és kattintson a **helyreállítása** toobegin helyreállítási hello fájl.
    
    ![A helyreállítási összefoglaló](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Immár hello **figyelés** hello lapján **DPM felügyeleti konzol** tooview hello **állapot** hello helyreállítási.
    
    ![Helyreállítási állapota](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)
    
    > [!NOTE]
    > hello fájl most helyreáll. Hello SharePoint webhely toocheck hello visszaállított fájl is frissítheti.
    > 
    > 

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Egy SharePoint-adatbázis visszaállítása az Azure-ból a DPM használatával
1. toorecover egy SharePoint tartalom-adatbázist, tallózzon a különböző helyreállítási pontokat (ahogy korábban), és válassza ki, hogy szeretné-e toorestore hello helyreállítási pontot.
   
    ![A DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Kattintson duplán a hello SharePoint helyreállítási pont tooshow hello elérhető SharePoint katalógus adatait.
   
   > [!NOTE]
   > Hello SharePoint-farm hosszú távú megőrzési az Azure-ban a védett, mert nincs katalógus információkkal (metaadatokkal) érhető el hello DPM-kiszolgálón. Ennek eredményeképpen időpontban a SharePoint tartalom-adatbázis helyreállítása toobe van szüksége, amikor szüksége toocatalog hello SharePoint-farm újra.
   > 
   > 
3. Kattintson a **újrakatalogizáláshoz**.
   
    ![A DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)
   
    Hello **felhő Újrakatalogizálni** állapotkezelő ablak nyílik meg.
   
    ![A DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)
   
    Katalogizálni befejezése után hello állapota túl*sikeres*. Kattintson a **Bezárás** gombra.
   
    ![A DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Kattintson a hello DPM látható hello SharePoint objektum **helyreállítási** tooget hello tartalom-adatbázist struktúra fülre. Kattintson a jobb gombbal a hello elemet, és kattintson **helyreállítása**.
   
    ![A DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Ezen a ponton, kövesse az hello [helyreállítási lépések az ebben a cikkben](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover a lemezről egy SharePoint tartalmi adatbázist.

## <a name="faqs"></a>Gyakori kérdések
K: milyen DPM támogatja az SQL Server 2014 és az SQL 2012 (SP2)?<br>
V: DPM 2012 R2 Update Rollup 4 egyaránt támogat.

K: helyreállíthatók egy SharePoint elem toohello eredeti helyre, ha SharePoint (a védelem a lemezen) az SQL AlwaysOn használatára van konfigurálva?<br>
A: Igen hello elem lehet helyreállított toohello eredeti SharePoint-webhelyen.

K: helyreállíthatók egy SharePoint adatbázis toohello eredeti helyre, ha a SharePoint SQL AlwaysOn használatára van konfigurálva?<br>
V:, mert a SharePoint-adatbázisok az SQL AlwaysOn vannak konfigurálva, akkor csak módosíthatók hello rendelkezésre állási csoport eltávolítása. Ennek eredményeképpen a DPM nem tudja visszaállítani egy adatbázis toohello eredeti helyre. Helyreállíthatja az SQL Server adatbázis tooanother SQL Server-példányt.

## <a name="next-steps"></a>Következő lépések
* További információk a DPM védelme a SharePoint - lásd [videó sorozat - a SharePoint védelme a DPM](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
* Felülvizsgálati [kibocsátási megjegyzések a System Center 2012 – Data Protection Manager](https://technet.microsoft.com/library/jj860415.aspx)
* Felülvizsgálati [kibocsátási megjegyzések a Data Protection Manager a System Center 2012 SP1](https://technet.microsoft.com/library/jj860394.aspx)

