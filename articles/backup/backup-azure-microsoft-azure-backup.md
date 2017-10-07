---
title: "aaaUse Azure Backup Server tooback mentése munkaterhelések tooAzure |} Microsoft Docs"
description: "Azure Backup Server tooprotect használja, vagy készítsen biztonsági másolatot a munkaterhelések toohello Azure-portálon."
services: backup
documentationcenter: 
author: PVRK
manager: shivamg
editor: 
keywords: "az Azure biztonsági mentési kiszolgáló; munkaterhelések; védelme -munkaterhelések biztonsági mentése"
ms.assetid: e7fb1907-9dc1-4ca1-8c61-50423d86540c
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: a99b2919ffd44c6133960e3a935038a2bb1281c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-using-azure-backup-server"></a>Munkaterhelés Azure Backup Server mentése tooback előkészítése
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Ez a cikk azt ismerteti, hogyan tooprepare a környezet tooback mentése munkaterhelések Azure Backup Server használatával. Az Azure Backup Server a Hyper-V virtuális gépek, a Microsoft SQL Server, a SharePoint Server, a Microsoft Exchange és a Windows-ügyfelek például alkalmazások és szolgáltatások védelmet biztosíthat egyetlen konzolról.

> [!NOTE]
> Az Azure Backup Server most megvédheti a VMware virtuális gépeket, és magasabb szintű biztonságra képességeket biztosít. Hello termék telepítéséhez az alábbi; hello szakaszokban leírtak szerint 1. frissítés vonatkoznak, és a legújabb Azure Backup szolgáltatás ügynökének hello. További információ az Azure Backup Server, a VMware-kiszolgálóinak biztonsági mentése toolearn cikke hello, [VMware-kiszolgáló használata Azure Backup Server tooback](backup-azure-backup-server-vmware.md). biztonsági képességeivel kapcsolatos toolearn tekintse meg a túl[Azure biztonsági mentési biztonsági jellemzőkkel dokumentáció](backup-azure-security-feature.md).
>
>

Infrastruktúra, a szolgáltató (IaaS) munkaterhelések, például az Azure virtuális gépeken is védheti.

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk ismerteti, hello telepített hello Resource Manager modellt használó virtuális gépek visszaállítására.
>
>

Az Azure Backup Server örökli nagy részét hello munkaterhelés biztonsági mentési funkció a Data Protection Manager (DPM). Ez a cikk tooDPM dokumentáció tooexplain hivatkozásokat tartalmaz néhány hello megosztott funkciót. Azure biztonsági mentés kiszolgáló osztja meg hello jelentős részét, ha a DPM azonos funkciókat. Az Azure Backup-kiszolgáló nem tootape mentésére, és nem integrálható a System Center.

## <a name="1-choose-an-installation-platform"></a>1. Válasszon egy telepítési platform
hello első lépést hello Azure Backup Server használatba egy Windows Server tooset. A kiszolgáló Azure vagy a helyszíni lehet.

### <a name="using-a-server-in-azure"></a>Az Azure-kiszolgáló használatával
Egy Azure Backup Servert futtató kiszolgáló kiválasztásakor ajánlott a kiindulási pont egy Windows Server 2012 R2 Datacenter gyűjtemény képe. hello cikk [az első Windows rendszerű virtuális gép létrehozása az Azure-portálon hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), nyújt segítséget az Azure virtuális gép első lépések hello javasolt még akkor is, ha soha nem használta az Azure-t. hello ajánlott minimális követelmények hello kiszolgáló virtuális gép (VM) kell lennie: A2 szabványosnak kétmagos processzor és 3.5-ös GB RAM-MAL.

Sok apró munkaterhelések védelme a Azure Backup Server rendelkezik. hello cikk [DPM telepítése Azure virtuális gépként](https://technet.microsoft.com/library/jj852163.aspx), segítséget nyújt a apró ismertetik. Hello gépen való telepítése előtt olvassa el ebben a cikkben teljesen.

### <a name="using-an-on-premises-server"></a>A helyszíni kiszolgáló használata
Ha nem szeretné, hogy toorun hello alap server az Azure-ban, hello server futtathatja a Hyper-V virtuális gépek, VMware virtuális gép vagy egy fizikai gazdagéphez. hello ajánlott hello kiszolgálói hardverre vonatkozó minimális követelmények kétmagos processzor és 4 GB RAM-MAL. a következő táblázat hello hello támogatott operációs rendszerek listáját:

| Operációs rendszer | Platform | SKU |
|:--- | --- |:--- |
| Windows Server 2012 R2 és a legújabb szervizcsomagok |64 bit |Standard, Datacenter, Foundation |
| Windows Server 2012 és a legújabb szervizcsomagok |64 bit |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 és a legújabb szervizcsomagok |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 és a legújabb szervizcsomagok |64 bit |Standard, Workgroup |

Windows Server deduplikálásával hello DPM-tároló is deduplikálni. További tudnivalók [DPM és deduplikáció](https://technet.microsoft.com/library/dn891438.aspx) együttműködése a Hyper-V virtuális gépek telepítésekor.

> [!NOTE]
> Az Azure Backup Server tervezett toorun dedikált, meghatározott célú kiszolgálón. Az Azure Backup Server nem telepíthető:
> - Tartományvezérlőként futtató számítógép
> - A számítógépen mely hello Alkalmazáskiszolgáló szerepkör telepítve van
> - Olyan számítógépre, amely a System Center Operations Manager felügyeleti kiszolgáló
> - Egy számítógép, amelyen az Exchange Server fut.
> - Olyan számítógépre, amely egy fürt csomópontja

Mindig csatlakozzon az Azure Backup Server tooa tartományhoz. Ha azt tervezi, toomove hello server tooa másik tartományba, ajánlott hello server toohello új tartományhoz csatlakoztatnia az Azure Backup Server telepítése előtt. Új tartomány tooa helyezze át egy meglévő Azure Backup Server számítógép-telepítés *nem támogatott*.

## <a name="2-recovery-services-vault"></a>2. Recovery Services-tároló
Biztonsági mentési adatok tooAzure küld, vagy helyileg legyen, hogy hello szoftver csatlakoztatott toobe tooAzure van szüksége. pontosabb toobe hello Azure Backup Server gép a beavatkozását toobe regisztrálva a recovery services-tároló.

a helyreállítás toocreate tároló szolgáltatások:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **Tallózás** hello az erőforrások listájához, írja be a **Recovery Services**. Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Kattintson a **Recovery Services-tároló** elemre.

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png) <br/>

    Recovery Services-tárolók hello listája jelenik meg.
3. A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-azure-microsoft-azure-backup/rs-vault-menu.png)

    hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.

    ![Recovery Services-tároló létrehozása – 5. lépés](./media/backup-azure-microsoft-azure-backup/rs-vault-attributes.png)
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. hello nevének kell toobe egyedi hello Azure-előfizetés esetében. Írjon be egy 2–50 karakter hosszúságú nevet. Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.
5. Kattintson a **előfizetés** toosee hello rendelkezésre álló előfizetések listáját. Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés. Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.
6. Kattintson a **erőforráscsoport** toosee elérhető erőforráscsoportok listáját hello, vagy kattintson a **új** toocreate egy új erőforráscsoportot. Átfogó információk az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
7. Kattintson a **hely** tooselect hello földrajzi régióban hello tároló.
8. Kattintson a **Create** (Létrehozás) gombra. A Recovery Services-tároló létrehozása toobe hello egy ideig is igénybe vehet. Hello állapot értesítések hello jobb felső területen hello portálon figyelése.
   A tároló létrehozása után hello portálon jelenik meg.

### <a name="set-storage-replication"></a>Tárreplikáció beállítása
hello tárolási replikációs beállítás lehetővé teszi toochoose georedundáns tárolás és a helyileg redundáns tárolás között. Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Ha ebben a tárolóban a elsődleges tároló, akkor hagyja hello tárolási lehetőség set toogeo-redundáns tárolás. Ha egy olcsóbb, rövidebb élettartamú megoldást szeretne, válassza a helyileg redundáns tárolást. Tudjon meg többet az [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolásáról: hello [Azure Storage replikáció – áttekintés](../storage/common/storage-redundancy.md).

tooedit hello tárolási replikációs beállítását:

1. Válassza ki a tároló tooopen hello tároló irányítópult hello-beállítások panelen. Ha hello **beállítások** panel nem nyitható meg **összes beállítás** hello tároló irányítópulton.
2. A hello **beállítások** panelen kattintson a **biztonsági infrastruktúra** > **biztonsági mentési konfigurációhoz** tooopen hello **biztonságimentésikonfigurációhoz** panelen. A hello **biztonsági mentési konfigurációhoz** panelen válassza ki a replikációs tárolómegoldást hello a tároló számára.

    ![A Backup-tárolók listája](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Miután kiválasztotta a tároló számára hello tárolási lehetőség, készen áll a tooassociate hello VM hello tárolóban. toobegin hello társítás, érdemes felderítése, és regisztrálja az Azure virtuális gépek hello.

## <a name="3-software-package"></a>3. Szoftvercsomag
### <a name="downloading-hello-software-package"></a>Hello szoftverfrissítési csomag letöltése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Ha már rendelkezik nyissa meg a Recovery Services-tároló, folytassa a toostep 3. Ha nem rendelkezik a Recovery Services-tároló nyílt, de a hello Azure-portálon, hello központ menüben kattintson a **Tallózás**.

   * Írja be az erőforrások listájához hello, **Recovery Services**.
   * Írja be megkezdése előtt, hello listát szűrheti a megadott feltételeknek. Amikor meglátja a **Recovery Services-tárolót**, kattintson rá.

     ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     Recovery Services-tárolók hello listája jelenik meg.
   * Recovery Services-tárolók hello listában jelölje ki a tárolóban.

     Megnyitja a kijelölt tároló irányítópult hello.

     ![Tároló panelének megnyitása](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. Hello **beállítások** alapértelmezés szerint megnyílik panelen. Ha be van zárva, kattintson a **beállítások** tooopen hello-beállítások panelen.

    ![Tároló panelének megnyitása](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Kattintson a **biztonsági mentés** tooopen hello első lépések varázsló.

    ![Biztonsági mentés első lépések](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    A hello **Ismerkedés a biztonsági mentés** panelt megnyitó, **biztonsági mentési célok** lesz automatikusan kiválasztva.

    ![Biztonsági célok-alapértelmezett-megnyitott](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. A hello **biztonsági mentési cél** paneljén hello **a számítási feladatok futtató** menüjében válassza **helyszíni**.

    ![a helyszíni és a munkaterhelések célok](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    A hello **miről szeretne toobackup?** legördülő menüben válassza hello munkaterhelések tooprotect Azure Backup Server használatával szeretné, és kattintson a **OK**.

    Hello **Ismerkedés a biztonsági mentés** varázsló kapcsolók hello **infrastruktúra előkészítése** beállítás tooback munkaterhelések tooAzure fel.

   > [!NOTE]
   > Ha csak a fájlok és mappák tooback szeretné, ajánlott hello Azure Backup-ügynök használatával, és hello cikkben hello útmutatást követve [először: fájlok és mappák biztonsági mentését](backup-try-azure-backup-in-10-mins.md). Ha tooprotect több, mint a fájlok és mappák, vagy tervezi tooexpand hello védelmi kell hello jövőben, válassza ki azokat a munkaterheléseket.
   >
   >

    ![Első lépések varázsló módosítása](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. A hello **infrastruktúra előkészítése** panelen, kattintson hello **letöltése** Azure Backup Server telepítése és a letöltési tárolói hitelesítő adatokat. Hello tárolói hitelesítő adatokat használja az Azure Backup Server toohello recovery services-tároló regisztrálás során. hello hivatkozások toohello ahol hello szoftvercsomag tölthető le a letöltőközpontból.

    ![Az Azure Backup Server infrastruktúra előkészítése](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Válassza ki az összes hello fájlokat, és kattintson a **következő**. Összes hello hello Microsoft Azure Backup letöltési oldala érkező fájlok letöltése és a hely összes hello fájlok hello ugyanabban a mappában.

    ![A letöltőközpontból 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Hello hello fájlok letöltési mérete nem haladhatja meg a > 3G, mivel a 10 MB/s letöltési hivatkozás too60 igénybe vehet hello percig, amíg le toocomplete.

### <a name="extracting-hello-software-package"></a>Hello szoftvercsomag kibontása
Minden hello fájl letöltése után kattintson **MicrosoftAzureBackupInstaller.exe**. Ez a parancs elindít hello **Microsoft Azure biztonsági mentés beállítása varázsló** tooextract hello telepítési fájlok az Ön által megadott tooa helyre. Kövesse hello varázsló lépéseit, majd kattintson a hello **kibontása** toobegin hello kinyerési folyamat gombra.

> [!WARNING]
> Legalább 4GB szabad lemezterület szükséges tooextract hello telepítőfájljainak.
>
>

![A Microsoft Azure biztonsági mentési beállítása varázsló](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Egyszer hello a kinyerési folyamat befejeződött, ellenőrizze hello mezőben toolaunch hello frissen kibontott *setup.exe* toobegin Microsoft Azure Backup Server telepítése, majd kattintson a hello **Befejezés** gombra.

### <a name="installing-hello-software-package"></a>Hello szoftvercsomag telepítése
1. Kattintson a **a Microsoft Azure Backup szolgáltatás** toolaunch hello beállítása varázsló.

    ![A Microsoft Azure biztonsági mentési beállítása varázsló](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Hello üdvözlőképernyőn kattintson a hello **következő** gombra. Ezzel megnyitná toohello *előfeltételek ellenőrzésének* szakasz. Ezen a képernyőn kattintson a **ellenőrizze** toodetermine, ha Azure Backup Server hello hardver- és előfeltétel teljesült. Minden előfeltétele sikeresen, ha látni fogja, hogy hello a gép megfelel hello jelző üzenet. Kattintson a hello **következő** gombra.

    ![Az Azure Backup Server - üdvözlő és az Előfeltételek ellenőrzése](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup Server szükséges SQL Server Standard, és hello Azure Backup Server telepítési csomag részeként elérhető csomagolt hello megfelelő SQL Server bináris fájlok szükséges. Egy új Azure Backup Server telepítésének indításakor ki kell választania hello beállítás **ezzel a beállítással új SQL Server-példány telepítése** hello kattintson **ellenőrzés és telepítés** gombra. Amikor hello előfeltételek telepítése sikeresen megtörtént, kattintson a **következő**.

    ![Az Azure Backup Server - SQL ellenőrzése](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Ha hiba lép fel egy javaslat toorestart hello géppel, ehhez, és kattintson az **ellenőrizze újra**.

   > [!NOTE]
   > Az Azure Backup Server egy távoli SQL Server-példány nem fog működni. Azure Backup Server által használt hello példányt kell toobe helyi.
   >
   >
4. Adjon meg egy helyet a Microsoft Azure Backup server fájlok hello telepítését, és kattintson a **következő**.

    ![A Microsoft Azure biztonsági mentési PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    hello ideiglenes hely biztonsági mentése tooAzure feltétele. Győződjön meg arról, hello ideiglenes hely: legalább 5 % hello adatok tervezett toobe biztonsági másolat toohello felhő. A lemezvédelem különálló lemezek toobe hello telepítése után konfigurálni kell. Tárolókészletek kapcsolatos további információkért lásd: [tárolókészletek konfigurálása és a lemezes tárolás](https://technet.microsoft.com/library/hh758075.aspx).
5. Adjon meg egy erős jelszót a korlátozott helyi felhasználói fiókokhoz, és kattintson a **következő**.

    ![A Microsoft Azure biztonsági mentési PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Válassza ki, hogy toouse *Microsoft Update* a frissítéseket, és kattintson toocheck **következő**.

   > [!NOTE]
   > Javasoljuk, a Windows Update tooMicrosoft a frissítést, ami biztonsági és fontos frissítéseket kínál a Windowshoz és más termékek, például a Microsoft Azure Backup Server átirányítása.
   >
   >

    ![A Microsoft Azure biztonsági mentési PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Felülvizsgálati hello *összegzése a beállítások* kattintson **telepítése**.

    ![A Microsoft Azure biztonsági mentési PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. hello telepítési fázisban történik. A Microsoft Azure Recovery Services Agent hello első fázis hello hello kiszolgálón van telepítve. hello varázsló is ellenőrzi az internetkapcsolat. Ha internetkapcsolat érhető el nyugodtan folytathatja a telepítést, ha nem, tooprovide proxy részletek tooconnect toohello Internet van szüksége.

    következő lépés hello tooconfigure hello Microsoft Azure Recovery Services Agent. A hello konfiguráció részeként, hogy tooprovide a tárolói hitelesítő adatok tooregister hello gép toohello helyreállítási szolgáltatások tárolóban. Akkor is nyújt egy hozzáférési kódot tooencrypt/visszafejtési hello közötti adatforgalom Azure és a helyszínen. Automatikusan egy hozzáférési kódot létrehozni, vagy adja meg a saját legalább 16 karakterből álló jelszót. Hello varázsló folytatásához, amíg hello ügynök úgy van beállítva.

    ![Az Azure biztonsági mentési Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Miután hello Microsoft Azure Backup server regisztrálása sikeresen befejeződött, hello általános telepítővarázsló halad toohello telepítése és konfigurálása az SQL Server és hello Azure Backup Server-összetevőt. Hello SQL Server-összetevő telepítése után hello Azure biztonsági mentés kiszolgáló-összetevők telepítve vannak.

    ![Azure Backup Server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Hello telepítési lépés befejezése után hello termék asztali ikonok lesz létrehozva is. Hello ikon toolaunch hello termék duplán.

### <a name="add-backup-storage"></a>Biztonsági mentési tároló hozzáadása
hello első biztonsági másolat másolatok csatlakoztatott tároló toohello Azure biztonsági mentési kiszolgálóként működő számítógép. Lemezek hozzáadásával kapcsolatos további információkért lásd: [tárolókészletek konfigurálása és a lemezes tárolás](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Akkor is, ha azt tervezi, hogy toosend adatok tooAzure szüksége tooadd biztonságimásolat-tároláshoz. Hello Azure Backup Server aktuális architektúrájának, hello Azure mentési tárolóval rendelkezik hello *második* ideje hello helyi tároló hello első (és kötelező) a biztonsági másolat hello adatok másolatát.
>
>

## <a name="4-network-connectivity"></a>4. Hálózati kapcsolat
Az Azure Backup Server kapcsolat toohello Azure Backup szolgáltatás hello termék toowork sikeresen igényel. toovalidate-e a gépnek hello hello kapcsolat tooAzure, használnia hello ```Get-DPMCloudConnection``` parancsmag hello Azure Backup Server PowerShell-konzolban. Ha hello hello parancsmag kimenetét értéke igaz, akkor van kapcsolat, ellenkező esetben nincs kapcsolat.

Hello: egy időben, hello Azure-előfizetéssel kell toobe állapota kifogástalan. kimenő előfizetését és toomanage hello állapotának toofind azt toohello bejelentkezés [előfizetés portal](https://account.windowsazure.com/Subscriptions).

Miután eldöntötte, hello állapot hello Azure kapcsolat és hello Azure-előfizetés, a toofind hello hatás ki az alábbi hello táblázatban hello biztonsági mentés/visszaállítás funkciói a is használhatja.

| Kapcsolati állapota | Azure-előfizetés | Készítsen biztonsági másolatot tooAzure | Készítsen biztonsági másolatot toodisk | Állítsa vissza az Azure-ból | Állítsa vissza a lemezről |
| --- | --- | --- | --- | --- | --- |
| Csatlakoztatva |Aktív |Engedélyezett |Engedélyezett |Engedélyezett |Engedélyezett |
| Csatlakoztatva |Lejárt |Leállítva |Leállítva |Engedélyezett |Engedélyezett |
| Csatlakoztatva |Platformelőfizetés |Leállítva |Leállítva |Leállított és az Azure helyreállítási pontjainak törlése |Leállítva |
| Elveszett kapcsolat > 15 nap |Aktív |Leállítva |Leállítva |Engedélyezett |Engedélyezett |
| Elveszett kapcsolat > 15 nap |Lejárt |Leállítva |Leállítva |Engedélyezett |Engedélyezett |
| Elveszett kapcsolat > 15 nap |Platformelőfizetés |Leállítva |Leállítva |Leállított és az Azure helyreállítási pontjainak törlése |Leállítva |

### <a name="recovering-from-loss-of-connectivity"></a>Végezze el a kapcsolat megszakadása
Ha tűzfal vagy egy proxy, amely megakadályozza az access tooAzure, a következő tartomány címek hello tűzfal /-proxy profilban toowhitelist hello szüksége:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Kapcsolat tooAzure visszaállított toohello Azure biztonsági mentés kiszolgálógép követően végrehajtható műveletek hello hello Azure-előfizetés állapota határozza meg. hello fenti táblához hello machine "Csatlakozás" után engedélyezett hello műveletek részleteit.

### <a name="handling-subscription-states"></a>– Előfizetési állapotok kezelése
Az Azure-előfizetés lehetséges tootake egy *lejárt* vagy *Deprovisioned* állapot toohello *aktív* állapotát. Azonban ez rendelkezik néhány hatással vannak a termék működéséről hello hello állapota nem *aktív*:

* A *Deprovisioned* előfizetés elveszti a funkciót, amely azt az platformelőfizetés hello időszakra. Forduljon a *aktív*, a biztonsági mentés/visszaállítás hello szolgáltatások újjáélesztett van. hello biztonsági mentési hello helyi lemezre is lehet adatokat beolvasni Ha megfelelően nagy megőrzési időtartam tartották. Azonban hello biztonsági mentési adatokat az Azure-ban végérvényesen elvesznek után hello előfizetés kerül hello *Deprovisioned* állapotát.
* Egy *lejárt* előfizetés csak elveszíti funkcióját, amíg nem lett végrehajtva *aktív* újra. Minden ütemezett biztonsági mentések hello időszak, amely előfizetési hello lett *lejárt* nem fog futni.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha a Microsoft Azure Backup server hibák hello telepítési fázis (vagy biztonsági mentési vagy visszaállítási) során, tekintse meg a toothis [hiba kódok dokumentum](https://support.microsoft.com/kb/3041338) további információt.
Olvassa el a is túl[Azure biztonsági mentés kapcsolatos gyakori kérdések](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Következő lépések
Részletes információt kaphat a [a környezet előkészítése a DPM](https://technet.microsoft.com/library/hh758176.aspx) hello Microsoft TechNet webhelyen. Támogatott konfigurációk, amelyen Azure Backup Server telepítése és használt adatait is tartalmazza.

Ezek a cikkek toogain bemutatják, munkaterhelések védelme a Microsoft Azure Backup server is használhatja.

* [SQL Server biztonsági másolat](backup-azure-backup-sql.md)
* [A SharePoint server biztonsági másolat](backup-azure-backup-sharepoint.md)
* [Alternatív kiszolgáló biztonsági mentése](backup-azure-alternate-dpm-server.md)
