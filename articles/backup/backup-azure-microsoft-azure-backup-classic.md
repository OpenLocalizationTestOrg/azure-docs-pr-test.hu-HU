---
title: "aaaUse Azure Backup Server tooback mentése munkaterhelések tooAzure klasszikus portál |} Microsoft Docs"
description: "Győződjön meg arról, hogy a környezet megfelelő előkészítése tooback munkaterhelés Azure Backup Server mentése"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: "az Azure biztonsági mentési kiszolgáló; mentési tároló"
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: 7b574824c448096e0c0ba74a872ab8f2a434f6a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-using-azure-backup-server"></a>Munkaterhelés Azure Backup Server mentése tooback előkészítése
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Az Azure Backup Server (klasszikus)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasszikus)](backup-azure-dpm-introduction-classic.md)
>
>

Ez a cikk a munkaterhelés Azure Backup Server másolatot a környezet tooback előkészítésével kapcsolatban van. Az Azure Backup Server a Hyper-V virtuális gépek, a Microsoft SQL Server, a SharePoint Server, a Microsoft Exchange és a Windows-ügyfelek például alkalmazások és szolgáltatások védelmet biztosíthat egyetlen konzolról.

> [!WARNING]
> Az Azure Backup Server örökli hello funkció Data Protection Manager (DPM) a munkaterhelések biztonsági mentéséhez. Mutatók néhány képességek tooDPM dokumentációjában talál. Azonban Azure Backup Server nem adja meg a védelem szalagon vagy integrálása a System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Windows Server-számítógépen
![1. lépés](./media/backup-azure-microsoft-azure-backup/step1.png)

hello első lépést hello Azure Backup Server használatba toohave Windows Server-számítógépen.

| Hely | Minimumkövetelmények | További utasítások |
| --- | --- | --- |
| Azure |Az Azure infrastruktúra-szolgáltatási virtuális gép<br><br>A2 méretű Standard: 2 mag, 3.5-ös GB RAM |Egy egyszerű gyűjteménye a Windows Server 2012 R2 Datacenter képe indítható. [Azure Backup Server (DPM) használatával IaaS munkaterhelések védelme](https://technet.microsoft.com/library/jj852163.aspx) sok apró rendelkezik. Győződjön meg arról, hogy olvassa el a cikk hello teljesen hello gép üzembe helyezése előtt. |
| Helyszíni követelmények |A Hyper-V virtuális gépek<br> VMWare virtuális gép<br> vagy a fizikai gazdagép<br><br>2 processzor és 4GB RAM |Windows Server deduplikálásával hello DPM-tároló is deduplikálni. További tudnivalók [DPM és deduplikáció](https://technet.microsoft.com/library/dn891438.aspx) együttműködése a Hyper-V virtuális gépek telepítésekor. |

> [!NOTE]
> Javasoljuk, hogy az Azure Backup Server telepíthető-e a virtuális gép Windows Server 2012 R2 Datacenter. Nagy mennyiségű hello Előfeltételek automatikusan ismertetnek hello hello Windows operációs rendszer legújabb verzióját.
>
>

Ha azt tervezi, toojoin Azure Backup Server tooa tartomány, javasoljuk, hogy csatlakoztatása hello fizikai kiszolgáló vagy a virtuális gép toohello tartomány hello Azure Backup Server szoftver telepítése előtt. Egy Azure Backup Server tooa új tartományban megköveteli a telepítést követően *nem támogatott*.

## <a name="2-backup-vault"></a>2. Mentési tároló
![2. lépés](./media/backup-azure-microsoft-azure-backup/step2.png)

Biztonsági mentési adatok tooAzure küld, vagy helyileg legyen, hogy hello Azure Backup Server regisztrált tooa tárolóban kell lennie. Ha egy új Azure Backup-felhasználó, és szeretné, hogy toouse Azure Backup Server, lásd: hello Azure portál verziója – Ez a cikk [készítse elő a munkaterhelés Azure Backup Server mentése tooback](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> 2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>



## <a name="3-software-package"></a>3. Szoftvercsomag
![3. lépés](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-hello-software-package"></a>Hello szoftverfrissítési csomag letöltése
Hasonló toovault hitelesítő adatokat, letöltheti a Microsoft Azure Backup az alkalmazások és szolgáltatások a hello **gyors kezdés lapon** hello biztonsági mentési tároló.

1. Kattintson a **az alkalmazások és szolgáltatások (lemez tooDisk tooCloud)**. A rendszer ekkor toohello letöltőközpontból oldal ahol hello szoftvercsomag tölthető le.

    ![A Microsoft Azure biztonsági mentési üdvözlőképernyője](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Kattintson a **Letöltés** gombra.

    ![A letöltőközpontból 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Válassza ki az összes hello fájlokat, és kattintson a **következő**. Összes hello hello Microsoft Azure Backup letöltési oldala érkező fájlok letöltése és a hely összes hello fájlok hello ugyanabban a mappában.
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
2. Hello üdvözlőképernyőn kattintson a hello **következő** gombra. Ezzel megnyitná toohello *előfeltételek ellenőrzésének* szakasz. Ezen a képernyőn kattintson a hello **ellenőrizze** toodetermine gombra, ha Azure Backup Server hello hardver- és előfeltétel teljesült. Összes hello előfeltételek teljesülnek sikeresen, ha látni fogja, hogy hello a gép megfelel hello jelző üzenet. Kattintson a hello **következő** gombra.

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

    következő lépés hello tooconfigure hello Microsoft Azure Recovery Services Agent. A hello konfiguráció részeként hogy tooprovide a hello tárolói hitelesítő adatok tooregister hello gép toohello mentési tároló. Akkor is nyújt egy hozzáférési kódot tooencrypt/visszafejtési hello közötti adatforgalom Azure és a helyszínen. Automatikusan egy hozzáférési kódot létrehozni, vagy adja meg a saját legalább 16 karakterből álló jelszót. Hello varázsló folytatásához, amíg hello ügynök úgy van beállítva.

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
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Az Azure Backup Server kapcsolat toohello Azure Backup szolgáltatás hello termék toowork sikeresen igényel. toovalidate-e a gépnek hello hello kapcsolat tooAzure, használnia hello ```Get-DPMCloudConnection``` parancsmag hello Azure Backup Server PowerShell-konzolban. Ha hello hello parancsmag kimenetét értéke igaz, akkor van kapcsolat, ellenkező esetben nincs kapcsolat.

Hello: egy időben, hello Azure-előfizetéssel kell toobe állapota kifogástalan. kimenő előfizetését és toomanage hello állapotának toofind azt toohello bejelentkezés [előfizetés portal](https://account.windowsazure.com/Subscriptions).

Miután eldöntötte, hello állapot hello Azure kapcsolat és hello Azure-előfizetés, a toofind hello hatás ki az alábbi hello táblázatban hello biztonsági mentés/visszaállítás funkciói a is használhatja.

| Kapcsolati állapota | Azure-előfizetés | Biztonsági mentési tooAzure | Biztonsági mentési toodisk | Állítsa vissza az Azure-ból | Állítsa vissza a lemezről |
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
