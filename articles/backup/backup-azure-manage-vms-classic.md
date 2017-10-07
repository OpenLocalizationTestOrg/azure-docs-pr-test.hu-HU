---
title: "aaaManage és megfigyelése az Azure virtuális gépek biztonsági mentéseinek |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage és megfigyelése az Azure virtuális gép biztonsági mentések"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cb95e0b3760c96f7fd563c42cb4c635553f48957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-hello-classic-portal"></a>Közös Azure biztonsági mentési feladatok és a klasszikus portálon hello eseményindító riasztások kezelése
> [!div class="op_single_selector"]
> * [Azure virtuális gép biztonsági mentések kezelése](backup-azure-manage-vms.md)
> * [Klasszikus virtuális gép biztonsági mentések kezelése](backup-azure-manage-vms-classic.md)
>
>

Ez a cikk tájékoztatást ad azokról a közös felügyeleti és megfigyelési feladatok Klasszikus-modell virtuális gépek Azure-ral védett.  

> [!NOTE]
> Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Lásd: [készítse elő a környezetet tooback Azure virtuális gépek](backup-azure-vms-prepare.md) talál részletes információt használata a klasszikus telepítési modell virtuális gépek.
>
> [!IMPORTANT]
>2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.
>
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.

## <a name="manage-protected-virtual-machines"></a>Védett virtuális gépek kezelése
toomanage védett virtuális gépet:

1. tooview és kezelheti a biztonsági mentési beállításait, a virtuális gépek kattintson hello **védett elemek** fülre.
2. Kattintson a védett elem toosee hello hello nevét a **biztonsági mentési részletek** fülre, amely hello utolsó biztonsági mentés adatait jeleníti meg.

    ![Virtuális gép biztonsági mentése](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. tooview és kezelheti a biztonsági mentési házirend beállításai a virtuális gépek kattintson hello **házirendek** fülre.

    ![Virtuális gép házirend](./media/backup-azure-manage-vms/manage-policy-settings.png)

    Hello **biztonsági mentési házirendek** lapra mutat be, akkor a meglévő házirend hello. Igény szerint módosíthatók. Ha toocreate egy új házirendet kell kattintson **létrehozása** a hello **házirendek** lap. Ne feledje, hogy ha azt szeretné, hogy tooremove házirend, nem a vele társított virtuális gépek.

    ![Virtuális gép házirend](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Műveletek vagy állapottal kapcsolatos további információk a virtuális gép hello kaphat **feladatok** lap. Kattintson egy feladat hello lista tooget további részleteket, vagy egy adott virtuális gép szűrheti a feladatokat.

    ![Feladatok](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Igény szerinti biztonsági mentést a virtuális gépek
Igény szerinti biztonsági mentését egy virtuális gép eltarthat, ha konfigurálva van a védelem. Ha hello kezdeti biztonsági másolatot függőben lévő hello virtuális géphez, igény szerinti biztonsági másolatot hoz létre teljes hello virtuális gép Azure biztonságimásolat-tárolóban. Ha első biztonsági mentés befejeződött, igény szerinti biztonsági mentése nem fog csak a korábbi biztonsági mentési tooAzure biztonsági mentés küldési módosítások tároló azaz azt nem mindig növekményes.

> [!NOTE]
> Egy igény szerinti biztonsági mentés megőrzési idejét a biztonsági mentési házirend megfelelő toohello VM napi megőrzési megadott tooretention érték van beállítva.  
>
>

tootake igény szerinti biztonsági mentést a virtuális gépek:

1. Keresse meg a toohello **védett elemek** lapon, és válassza **Azure virtuális gép** , **típus** (Ha még nincs kiválasztva), majd kattintson a **Válasszon**gombra.

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. Válassza ki a hello virtuális gép, amelyen szeretné tootake igény szerinti biztonsági mentési, majd kattintson a **a biztonsági mentés gombra** alján hello hello gombra.

    ![Biztonsági másolat készítése](./media/backup-azure-manage-vms/backup-now.png)

    Ezzel létrehoz egy biztonsági mentési feladat kijelölt hello virtuális gépen. Ez a feladat segítségével létrehozott helyreállítási pont megőrzési időtartamának ugyanaz, mint a megadott hello virtuális géphez társított hello házirend lesz.

    ![Biztonsági mentési feladat létrehozása](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > a virtuális géphez társított, részletezési le a virtuális gép hello tooview hello házirend **védett elemek** lap, és lépjen toobackup szabályzatlap.
   >
   >
3. Miután hello feladat jön létre, rákattinthat a **feladat megtekintése** hello bejelentési toosee hello megfelelő feladat a hello feladatok lapján sáv gombjára.

    ![Biztonsági mentési feladat létrehozása](./media/backup-azure-manage-vms/created-job.png)
4. Hello feladat sikeres befejezése után egy helyreállítási pontot létrehozza amellyel toorestore hello virtuális gépet. Ez hello helyreállítási oszlop értékét is növeli a 1 **védett elemek** lap.

## <a name="stop-protecting-virtual-machines"></a>Virtuális gépek védelmének megszüntetése
Az alábbi beállítások hello toostop hello a jövőbeni biztonsági mentés, a virtuális gép lehet választani:

* Azure biztonságimásolat-kezelő tárolójában a virtuális géphez tartozó biztonsági mentési adatok megőrzése mellett
* A virtuális géphez tartozó biztonsági mentési adatok törlése

Ha be van jelölve a virtuális géphez tartozó biztonsági mentési adatok tooretain, hello biztonsági mentési adatok toorestore hello virtuális gép is használhatja. Ilyen virtuális gépek díjszabása, kattintson a [Itt](https://azure.microsoft.com/pricing/details/backup/).

a virtuális gép tooStop védelmét:

1. Keresse meg a túl**védett elemek** lapon, és válassza **Azure virtuális gép** hello szűrőtípus (Ha még nincs kiválasztva), és kattintson a **válasszon** gombra.

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. Válassza ki hello virtuális gépet, majd kattintson a **védelem kikapcsolása** hello lap hello alján.

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/stop-protection.png)
3. Alapértelmezés szerint Azure Backup szolgáltatással nem törli hello hello virtuális géphez tartozó biztonsági mentési adatokat.

    ![Állítsa le a védelmet megerősítése](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Ha azt szeretné, hogy a biztonsági mentési adatok toodelete, jelölje be a hello jelölőnégyzetet.

    ![Jelölőnégyzet](./media/backup-azure-manage-vms/checkbox.png)

    Válasszon ki egy hello biztonsági mentés leállításának oka. Ez nem választható, ok megadása a rendszer Azure biztonsági mentés toowork hello visszajelzések segítségével és rangsorolhatja hello forgatókönyvet.
4. Kattintson a **Submit** gomb toosubmit hello **állítsa le a védelmet** feladat. Kattintson a **feladat megtekintése** toosee hello megfelelő hello feladat **feladatok** lap.

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/stop-protect-success.png)

    Ha nem választott **Delete tartozó biztonsági mentési adatok** során lehetőséget **védelem kikapcsolása** varázsló, majd a feladás egy vagy több feladat befejezése után védelmi állapota túl**állítvaavédelem**. az Azure Backup hello adatok marad, amíg le nem explicit módon. Hello adatok hello hello virtuális gép kiválasztásával mindig törölhetők **védett elemek** lap, és kattintson a **törlése**.

    ![Leállított védelme](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Ha be van jelölve hello **Delete tartozó biztonsági mentési adatok** beállításnál hello virtuális gép nem lesz része hello **védett elemek** lap.

## <a name="re-protect-virtual-machine"></a>Virtuális gép védelmének újbóli beállításához
Ha nincs kiválasztva hello **társítása biztonsági mentési adatok** beállítást **védelem kikapcsolása**, ismételt védelemmel láthatná el hello virtuális gép által a következő hello lépéseket hasonló toobacking be virtuális regisztrálva gépek. Ha védett, a virtuális gép lesz a biztonsági mentési adatok őrződnek meg előzetes toostop védelem és a helyreállítási pontok létrehozása után védelmének újbóli beállítását.

Ismételt védelemmel való ellátása után hello virtuális gép védelmi állapotát változnak túl**védett** Ha nincsenek helyreállítási pontok korábbi túl**védelem kikapcsolása**.

  ![A feladatátvételen VM](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Hello virtuális gép ismételt védelmével, amikor dönthet úgy, mint hello házirend, amellyel a virtuális gép védett eredetileg másik szabályt.
>
>

## <a name="unregister-virtual-machines"></a>Virtuális gépek regisztrációjának törlése
Ha azt szeretné, hogy tooremove hello virtuális gépet a mentési tároló hello:

1. Kattintson a hello **UNREGISTER** alján hello hello gombra.

    ![Tiltsa le a védelmet](./media/backup-azure-manage-vms/unregister-button.png)

    Egy bejelentési értesítést megerősítést kér üdvözlő képernyőt hello alján jelenik meg. Kattintson a **Igen** toocontinue.

    ![Tiltsa le a védelmet](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Biztonságimásolat-adatok törlése
Hello vagy egy virtuális géphez tartozó biztonsági mentési adatokat is törli:

* Védelemleállítási feladat során
* Után állítsa le a védelmet feladat befejezése után a virtuális gépen

a virtuális gépen, amely hello toodelete a biztonsági mentési adatok *állítva a védelem* állapota sikeres befejezése utáni egy **biztonsági mentés leállítása** feladat:

1. Keresse meg a toohello **védett elemek** lapon, és válassza **Azure virtuális gép** , *típus* hello kattintson **kiválasztása** gombra.

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. Válassza ki a hello virtuális gépet. hello virtuális gép lesz a **állítva a védelem** állapotát.

    ![Védelem leállítása](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Kattintson a hello **törlése** alján hello hello gombra.

    ![Törölje a biztonsági mentés](./media/backup-azure-manage-vms/delete-backup.png)
4. A hello **biztonsági mentési adatok törlése** varázsló, jelöljön ki egy (ajánlott) biztonsági mentési adatok törlésének okát, majd **Submit**.

    ![biztonsági mentési adatok törlése](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Ekkor létrejön egy feladat toodelete biztonsági mentési adatok kijelölt virtuális gép. Kattintson a **feladat megtekintése** toosee megfelelő feladatot a feladatok lapot.

    ![Adatok törlése sikeresen megtörtént](./media/backup-azure-manage-vms/delete-data-success.png)

    Ha hello feladat befejeződött, hello bejegyzést a megfelelő toohello virtuális gép törlődik **védett elemek** lap.

## <a name="dashboard"></a>Irányítópult
A hello **irányítópult** lapon tekintse át az információkat az Azure virtuális gépek, a tároló és a feladatok a hozzájuk társított runbookokat hello az elmúlt 24 órában. Megtekintheti a biztonsági mentés állapotának és a kapcsolódó biztonsági mentési hibák.

![Irányítópult](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Hello irányítópult értékeket 24 óránként egyszer frissülnek.
>
>

## <a name="auditing-operations"></a>Naplózási műveletek
Azure biztonsági mentés biztosít hello "művelet logs" biztonsági mentési műveleteket, így könnyen toosee pontosan milyen felügyeleti műveleteket a mentési tároló hello végrehajtott hello ügyfél által indított át kell tekinteni. Műveleti naplók engedélyezése a nagy levágást, és a naplózási hello biztonsági mentési műveletek támogatása.

a következő műveletek hello műveletnaplók vannak bejelentkezve:

* Regisztráljon
* Regisztrálás törlése
* A védelem konfigurálása
* Biztonsági mentés (mindkettő ütemezett és igény szerinti biztonsági mentést BackupNow keresztül)
* Visszaállítás
* Állítsa le a védelmet
* biztonsági mentési adatok törlése
* Házirend hozzáadása
* Szabályzat törlése
* Házirend frissítése
* Megszakítása

tooview műveletnaplókat tooa megfelelő mentési tároló:

1. Keresse meg a túl**szolgáltatások** Azure-portálon, és kattintson a hello **műveletnaplók** fülre.

    ![A műveletnaplók](./media/backup-azure-manage-vms/ops-logs.png)
2. Hello szűrők, válassza ki **biztonsági mentési** , *típus* , és adja meg a mentési tároló nevére hello *szolgáltatásnév* , majd kattintson a **Submit**.

    ![A művelet naplók szűrő](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. Hello műveletek naplófájlokban, jelölje ki az összes műveletet, majd kattintson **részletek** toosee részletek megfelelő tooan műveletet.

    ![Művelet naplók beolvasása részletei](./media/backup-azure-manage-vms/ops-logs-details.png)

    Hello **részletek varázsló** hello művelet működésbe. a feladat azonosítóját, amelyen ez a művelet akkor váltódik ki, és kezdési időpont hello művelet erőforrás kapcsolatos információt tartalmazza.

    ![Művelet részletei](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Riasztási értesítések
Hello feladatok egyéni riasztási értesítéseket kaphat a portálon. PowerShell-alapú riasztási szabályok definiálása a műveleti naplókat események ehhez. Azt javasoljuk, *PowerShell 1.3.0 verzió vagy újabb*.

a biztonsági mentési hibák az értesítő üzenet egyéni szövegében tooalert toodefine, egy minta parancs fog megjelenni:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: letölthető ez Operations Logs előugró fenti szakaszban leírtak szerint. Egy művelet részletei előugró ablakban ResourceUri hello ResourceId toobe ennek a parancsmagnak megadott.

**OperationName**: hello formátumban lesz "Microsoft.Backup/backupvault/<EventName>" EventName az egyik regisztrálása, Unregister, ConfigureProtection, biztonsági mentése, visszaállítása, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy

**Állapot**: támogatott értékek vannak-elindítva, sikeres és sikertelen volt.

**Erőforráscsoport**: erőforráscsoport, amelyen művelet indított hello erőforrás. Ezt úgy szerezheti be ResourceId értékből. Érték mező között */resourceGroups/* és */providers/* a ResourceId érték hello a ResourceGroup.

**Név**: hello riasztási szabály nevét.

**CustomEmail**: Adja meg a kívánt toosend riasztási értesítés hello egyéni e-mail cím toowhich

**SendToServiceOwners**: ezt a beállítást küldi el – riasztási értesítés tooall rendszergazdák és a társadminisztrátorok hello előfizetés. A használat **New-AzureRmAlertRuleEmail** parancsmag

### <a name="limitations-on-alerts"></a>Riasztások korlátozásai
Eseményalapú riasztások az éppen vetni toohello a következő korlátozások vonatkoznak:

1. A figyelmeztetéseket hello mentési tároló összes virtuális gépeken. Nem szabhatja testre az tooget riasztásokat az adott virtuális gépek csoportja tartalmazza a biztonságimásolat-tárolóban.
2. A funkció jelenleg előzetes verzió. [További információ](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Értesítéseket küld a "alerts-noreply@mail.windowsazure.com". Hello e-mailt olyasvalaki jelenleg nem módosíthatók.

## <a name="next-steps"></a>Következő lépések
* [Állítsa vissza az Azure virtuális gépek](backup-azure-restore-vms.md)
