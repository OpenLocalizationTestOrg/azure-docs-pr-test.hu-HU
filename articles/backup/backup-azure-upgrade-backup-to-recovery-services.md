---
title: "a biztonsági mentési tároló tooa Recovery Services-tároló (előzetes verzió) aaaUpgrade |} Microsoft Docs"
description: "Útmutatás és támogatási információk tooupgrade az Azure biztonsági mentés tároló tooa Recovery Services-tároló."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
ms.assetid: 228fef19-2f6b-4067-acc3-fb6e501afb88
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/03/2017
ms.author: sogup;markgal;arunak
ms.openlocfilehash: 49062ca4556a009c82f143bb3a60ec71748bed01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-backup-vault-tooa-recovery-services-vault"></a>Frissítse a biztonsági mentési tároló tooa Recovery Services-tároló

Ez a cikk azt ismerteti, hogyan tooupgrade a biztonsági másolatok tárolóját tooa Recovery Services tároló. hello frissítési folyamat nem befolyásolja a futó biztonsági mentési feladat, és a biztonsági mentési adatok nem vesztek el. elsődleges hello tooupgrade egy biztonsági mentési tároló tooa Recovery Services-tároló okok miatt:
- A mentési tároló összes funkciójának Recovery Services-tároló maradnak.
- Helyreállítási szolgáltatások tárolók mentési tárolókban, beleértve a több szolgáltatással rendelkezik: nagyobb biztonságot, integrált figyelés, gyorsabb visszaállítás és elemszintű visszaállítások.
- Kezelheti a biztonsági másolati elemei egy továbbfejlesztett, egyszerűsített portálról.
- Új funkciók csak tooRecovery szolgáltatások tárolók érvényesek.

## <a name="impact-toooperations-during-upgrade"></a>A frissítés során a hatás toooperations

A biztonsági mentési tároló tooa Recovery Services-tároló frissítésekor, akkor ennek nincs hatása tooyour az vezérlősík műveleteket. Minden biztonsági mentési feladatok folytatása a szokásos módon, és az aktív helyreállítási feladatokat megszakítás nélkül tovább. Hello a frissítés során felügyeleti műveletek fel Önnek egy rövid állásidőt, és nem lehet új elemek védelme vagy ad hoc biztonsági mentés feladatok létrehozása. Infrastruktúra-szolgáltatási virtuális gépek feladatok visszaállításakor ne futtassa hello frissítés során. hello tároló frissítés alatt egy óra toocomplete vesz igénybe. Miután befejeződött, a Recovery Services-tároló helyettesíti hello Backup-tárolóban.

## <a name="changes-tooyour-automation-and-tool-after-upgrading"></a>Módosítások tooyour automatizálás és az eszköz frissítése után

Az infrastruktúra előkészítése hello tároló frissítési, frissítenie kell a meglévő automatizációk vagy tooling eszköz tooensure, hogy továbbra is toowork hello frissítés után.
Tekintse át a hello PowerShell parancsmagok hivatkozásokat a hello [Service Manager telepítési modell](backup-client-automation-classic.md) és hello [Resource Manager üzembe helyezési modellben](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Frissítés előtti teendők

Jelölőnégyzet hello következő kiállítja a frissítés előtt a biztonsági mentési tárolók tooRecovery szolgáltatás tárolók.

- **Minimális verziója**: tooupgrade a tároló, győződjön meg arról, hogy hello Microsoft Azure Recovery Services (MARS) ügynök van legalább 2.0.9070.0 verziója. Ha hello MARS-ügynök régebbi, mint 2.0.9070.0, frissítse a hello ügynök hello frissítési folyamat megkezdése előtt.
- **Számlázási modellt példány-alapú**: helyreállítási szolgáltatás tárolók csak hello példány-alapú számlázási modellt támogatja. Ha egy mentési tárolót, amely hello régebbi Storage-alapú számlázási modellt használja, alakítsa át a számlázási modellt hello frissítés során.
- **Nincs folyamatban biztonsági mentési konfigurációhoz műveletek**: a frissítés során a hozzáférési toohello felügyeleti vezérlősík korlátozódik. Minden felügyeleti vezérlősík műveletek végrehajtása, és indítsa el a hello frissítés.

## <a name="using-powershell-scripts-tooupgrade-your-vaults"></a>PowerShell-parancsfájlok tooupgrade a tárolók használata

Használhatja a PowerShell parancsfájlok tooupgrade a mentési tárolókat tooRecovery szolgáltatások tárolók. Ellenőrizze, hogy rendelkezik hello szükséges PowerShell összetevők tootrigger hello tároló frissítés. Mentési tárolókban PowerShell-parancsfájlok nem működnek a Recovery Services-tárolók. A környezet előkészítése a tooupgrade hello tárolók:

1. Telepítéséhez vagy verziófrissítéséhez [Windows Management Framework (WMF) tooversion 5](https://www.microsoft.com/download/details.aspx?id=50395) vagy újabb.
2. [Telepítse az Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. Töltse le a hello [PowerShell-parancsfájl](https://aka.ms/vaultupgradescript2) tooupgrade a tárolók.

### <a name="run-hello-powershell-script"></a>Hello PowerShell parancsfájl futtatása

Használja a következő parancsfájl tooupgrade hello a tárolók. a következő mintaparancsfájl hello ismereteket szeretnének elsajátítani a hello paraméterek rendelkezik.

RecoveryServicesVaultUpgrade-1.0.2.ps1 **- SubscriptionID** `<subscriptionID>` **- VaultName** `<vaultname>` **-hely** `<location>` **- ResourceType** `BackupVault` **- TargetResourceGroupName**`<rgname>`

**A SubscriptionID** -hello előfizetési azonosító szám hello tároló, amely a frissítés alatt áll.<br/>
**VaultName** – hello neve hello Backup-tárolóban, amely a frissítés alatt áll.<br/>
**Hely** -frissítés alatt hello tároló helyét.<br/>
**A ResourceType** -BackupVault használja.<br/>
**TargetResourceGroupName** – mivel a frissíteni kívánt hello tároló tooa Resource Manager-alapú telepítés, adja meg egy erőforráscsoportot. Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy új nevet megadásával. Ha hibásan hello erőforráscsoport neve, létrehozhat egy új erőforráscsoportot. További információ az erőforráscsoportok, toolearn olvassa el ezt [tudnivalók az erőforráscsoportokról áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Erőforráscsoport-nevek vannak. Lehet, hogy toofollow hello útmutatást; Hiba toodo úgy, hogy tároló frissítések toofail.
>
>

hello következő kódrészletet példája milyen a PowerShell-parancsot kell hasonlítania:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

Hello parancsfájl paraméterek nélkül is futtathatja, és kéri a tooprovide bemenetek összes szükséges paramétert.

PowerShell parancsfájl hello meg tooenter a hitelesítő adatok megadását kéri. Adja meg a hitelesítő adatok kétszer: hello Service Manager-fiókot, és az erőforrás-kezelő fiók hello még egyszer egy alkalommal.

### <a name="pre-requisites-checking"></a>Előfeltételek ellenőrzése
Miután megadta a Azure hitelesítő adatait, a Azure ellenőrzi, hogy a környezet megfelel-e a következő előfeltételek hello:

- **Minimális verziója** -frissítés mentési tárolók tooRecovery szolgáltatások tárolókban igényel hello MARS agent toobe 2.0.9070 verziója. Ha rendelkezik elemek regisztrálni tooa Backup-tárolóban egy ügynök rendszernél korábbi 2.0.9070, hello az előfeltétel-ellenőrzés sikertelen lesz. Hello az előfeltétel-ellenőrzés sikertelen lesz, ha hello ügynök frissítése, és próbálja meg újból tooupgrade hello tárolóban. Hello hello ügynök a legújabb verzióját letöltheti [http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe](http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **A folyamatban lévő konfigurációs feladatok**: Ha valaki konfigurálását feladat esetében a biztonsági másolatok tárolóját toobe frissítése, vagy egy elem regisztrálása, hello előfeltétel ellenőrzése sikertelen. Hello konfigurálása, illetve hello elem regisztrálásának befejezése, majd a hello tároló frissítési folyamat elindításához.
- **Storage-alapú számlázási modellt**: Recovery Services tárolók támogatási hello példány-alapú számlázási modellt. Ha hello tároló frissítést futtat egy biztonsági mentési tárolót, hogy használja a számlázási modellt Storage-alapú hello, felszólító tooupgrade tároló hello együtt a számlázási modellt. Ellenkező esetben frissítheti a számlázási modellt először, és futtassa a hello tároló frissítés.
- Azonosítsa az erőforráscsoport, a hello Recovery Services-tároló. tootake előnyeit hello Resource Manager üzembe helyezési funkcióival, Recovery Services-tároló erőforráscsoportban kell helyezni. Ha nem tudja, melyik erőforráscsoport toouse, adjon meg egy nevet és egy hello frissítési folyamat létrehoz hello erőforráscsoportot. hello frissítési folyamat is hozzárendeli a hello tárolóval rendelkező hello új erőforráscsoportot.

Hello frissítési folyamat befejezése után hello szükséges előfeltételek ellenőrzése után a hello folyamat akkor toostart hello tároló frissítés megadását kéri. Miután meggyőződött róla, hello frissítési folyamat általában 15-20 perc toocomplete, attól függően, hogy a tároló méretének hello körül vesz igénybe. Ha egy nagy tárolóban, frissíteni is eltarthat, too90 perc.

## <a name="managing-your-recovery-services-vaults"></a>A Recovery Services-tárolók kezelése

a következő képernyőkön hello megjelenítése egy új Recovery Services-tároló, a biztonsági mentési tárolóból, az Azure-portálon hello frissítve. hello első képernyőn látható, amely megjeleníti a kulcs entitások hello tároló hello tároló irányítópult.

![a biztonsági másolatok tárolóját verzióról frissített Recovery Services-tároló – példa](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

második üdvözlő képernyőt hello súgóhivatkozások az elérhető toohelp hello Recovery Services használatának megkezdésében tároló jeleníti meg.

![Súgóhivatkozások hello – első lépések panelen](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>A frissítés utáni lépések
Recovery Services-tároló megadását időzóna-információk támogatja a biztonsági mentési házirend. Tároló sikeres frissítése után nyissa meg tooBackup házirendek tároló-beállítások menüjében, és hello időzóna-információ az egyes hello tárolóban konfigurált hello házirendek frissítése. Ezen a képernyőn már látható hello biztonsági mentés ütemezése időpont van megadva a helyi időzónát használja, ha egy házirend létrehozásához. 

## <a name="enhanced-security"></a>Fokozott biztonság

A biztonsági másolatok tárolóját frissítésekor tooa Recovery Services-tároló, hello biztonsági beállításait, hogy a tároló automatikusan be vannak kapcsolva. Ha hello a biztonsági beállításokat és az egyes műveletek, például törlés, biztonsági mentések, vagy módosítása egy hozzáférési kódot igényel egy [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) PIN-kódot. Hello fokozott biztonsági további információkért lásd: hello cikk [biztonsági jellemzőkkel tooprotect hibrid biztonsági mentések](backup-azure-security-feature.md). 

Ha hello fokozott biztonsági be van kapcsolva, adatok mentése too14 nap hello tárolóból hello helyreállítási pont adatainak törlése után megőrzi. Az ügyfelek számlázása a biztonsági adatok tárolására. Biztonsági adatok megőrzése hello Azure Backup szolgáltatás ügynöke, az Azure Backup Server és a System Center Data Protection Manager toorecovery pontok vonatkozik. 

## <a name="gather-data-on-your-vault"></a>A tároló adatgyűjtés

Miután elvégezte a verziófrissítést tooa Recovery Services-tároló, jelentések konfigurálása az Azure biztonsági mentés (az infrastruktúra-szolgáltatási virtuális gépeket és a Microsoft Azure Recovery Services (MARS)), és használja a Power BI tooaccess hello jelentéseit. Adatgyűjtés kapcsolatos kiegészítő tudnivalókat lásd: hello cikk [Azure Backup konfigurálása jelentések](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Frissítési csomag hello befolyásolja a folyamatban levő biztonsági másolatok?**</br>
Nem. A folyamatban levő biztonsági másolatok megszakításmentes folytatásához során, és a frissítés után.

**Ha szeretnék frissítéséről hamarosan nem tervezi, mi történik, toomy tárolók?**</br>
Mivel az összes új szolgáltatás csak a tooRecovery szolgáltatások tárolók alkalmazni, javasoljuk, tooupgrade a tárolók. Microsoft végül fog érvényteleníthető hello klasszikus portálon. 2017. szeptember 1., kezdési Microsoft megkezdődik az automatikus frissítés a mentési tárolókat tooRecovery szolgáltatások tárolók. 2017. November 1., amelyet a Microsoft hello frissítési folyamat befejeződik. A tároló automatikusan frissíthető. szeptember vagy október alatt. A Microsoft azt javasolja, hogy minél hamarabb frissítse a tárolóban.

**A meglévő eszközt használunk erre a frissítési középértéket funkciója?**</br>
Frissítse a tooling toohello Resource Manager üzembe helyezési modellben. Tárolók jelentek meg a helyreállítási szolgáltatás használ a hello Resource Manager üzembe helyezési modellben. Hello Resource Manager üzembe helyezési modellben tervezéséről és a tárolók hello különbség elszámolása fontos. 

**Hello frissítés közben nem sok állásidő?**</br>
Hello olyan erőforrások száma, amelyek frissítik a függ. Kisebb környezetekhez (védett példányok néhány több tíz) teljes frissítés hello kevesebb, mint 20 percet kell végrehajtani. Nagyobb telepítések esetén elméletileg legfeljebb egy óra.

**I vissza lehet vonni a frissítés után?**</br>
Nem. FIM hello erőforrások sikeresen frissítette a rollback utasítás nem támogatott.

**Azt is szeretnék ellenőrzi az előfizetés vagy az erőforrások toosee képes a frissítés Ha?**</br>
Igen. frissítés hello első lépése ellenőrzi, hogy hello erőforrások frissítés. Abban az esetben szükséges előfeltételek hello érvényesítése sikertelen, üzeneteket fogadni hello frissítés nem hajtható végre hello okok miatt.

**Milyen engedélyekkel kell rendelkeznie tootrigger tároló frissítését?**</br>
tooperform hello tároló frissítési, hozzáadva a klasszikus Azure portálon hello hello előfizetés társadminisztrátoraként. Ez elengedhetetlen, még akkor is, ha már fel van sorolva hello Azure-portálon a tulajdonosa. Próbálja tooadd a klasszikus Azure portál toofind kimenő hello az előfizetéshez társadminisztrátoraként hello előfizetés társadminisztrátoraként esetén. Ha nem tudja tooadd társadminisztrátorának, lépjen kapcsolatba a szolgáltatás-rendszergazdai vagy társadminisztrátori hello előfizetés, akik felvehesse Önt társadminisztrátoraként.

**Frissíthetem a CSP-alapú biztonsági mentés tárolójának?**</br>
Nem. Mentési tárolók CSP-alapú jelenleg nem frissíthető. A Microsoft támogatni fogják a CSP-alapú biztonsági mentési tárolók a következő kiadásokban hello frissítése.

**Megtekintheti a post frissítési klasszikus tárolót?**</br>
Nem. Nem tekinthetők meg és kezelheti a klasszikus tároló utólagos frissítése. Csak akkor tudja toouse hello új Azure-portálon a hello tároló összes felügyeleti műveletet.

**A frissítés nem sikerült, de igénylő hello ügynök tárolt hello gép már frissíti, nem létezik. Mi a teendő ebben az esetben?**</br>
Ha toouse hello tároló van szüksége, hello hosszú távú megőrzési, majd a gépet nem lesznek képesek tooupgrade hello tárolóban. A későbbi kiadásokban azt támogatni fogják a frissítés ilyen egy tárolót.
Ha ezt a gépet toostore hello biztonsági mentések nem kell többé, majd adjon regisztrációját ez gépen hello tárolóból, és próbálja újra hello frissítést.

**Miért nem látom hello feladatok adatokat a helyszíni erőforrások frissítés után**</br>
Figyelés a helyszíni biztonsági mentések (a MARS agent, a DPM és az Azure Backup Server) az új szolgáltatása, ha a biztonsági mentési tároló tooRecovery Services-tároló frissít. monitoring információk hello elfoglalja too12 óra toosync hello szolgáltatásban.

**Hogyan jelenthetem a problémát?**</br>
Ha bármely részének hello tároló frissítése sikertelen, Megjegyzés hello OperationID azonosítójú hello hiba szerepel. Microsoft Support proaktív módon fognak működni tooresolve hello probléma. Elérhetők a tooSupport, vagy e-mail-nekünk az rsvaultupgrade@service.microsoft.com az előfizetés-azonosító, a tároló neve és a OperationID azonosítójú. Minél gyorsabban megpróbáljuk tooresolve hello probléma. Nem újra hello műveletet, hacsak kifejezetten kéri toodo Igen, a Microsoft által.


## <a name="next-steps"></a>Következő lépések
A következő cikk hello használata:</br>
[Készítsen biztonsági másolatot az infrastruktúra-szolgáltatási virtuális gép](backup-azure-arm-vms-prepare.md)</br>
[Készítsen biztonsági másolatot az Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[A Windows Server biztonsági mentése](backup-configure-vault.md).
