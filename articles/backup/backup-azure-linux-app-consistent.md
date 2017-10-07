---
title: "Az Azure Backup: a Linux virtuális gépek alkalmazáskonzisztens biztonsági mentések |} Microsoft Docs"
description: "Parancsfájlok tooguarantee alkalmazáskonzisztens biztonsági mentések tooAzure, használja a Linux virtuális gépek számára. hello parancsfájlok alkalmazása csak tooLinux virtuális gépek erőforrás-kezelő központi telepítés; hello parancsfájlok tooWindows virtuális gépek vagy a service manager-üzemelő példányok nem vonatkoznak. Ez a cikk végigvezeti hello hello parancsfájlok, beleértve a hibaelhárítás konfigurálásának lépéseit."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "alkalmazáskonzisztens biztonsági másolat; Azure virtuális gép alkalmazáskonzisztens biztonsági mentését; Linux virtuális gép biztonsági mentése; Azure biztonsági mentés"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.openlocfilehash: d557dd973364d79bb4d8ce954f648de835dd345f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Alkalmazáskonzisztens biztonsági mentését Azure Linux virtuális gépek (előzetes verzió)

Ez a cikk a Linux előtti parancsfájl hello és utáni parancsfájl keretrendszer, és hogyan lehet használt tootake Azure Linux virtuális gépek alkalmazáskonzisztens biztonsági mentések beszél.

> [!Note]
> hello parancsfájl előtti és utáni parancsfájl keretrendszer csak Azure Resource Manager telepített Linux virtuális gépek esetén támogatott. Parancsfájl-alkalmazás konzisztencia nem támogatottak a Service Manager telepített virtuális gépek vagy windowsos virtuális gépekre.
>

## <a name="how-hello-framework-works"></a>Hello keretrendszer működése

hello keretrendszer biztosít egy beállítás toorun egyéni előtti parancsfájlok és utáni parancsfájlok, amíg a virtuális gép pillanatképek van véve. Előtti parancsfájlok hello virtuális gép pillanatképének készítése és utáni parancsfájlok rögtön hello virtuális gép pillanatképének készítése előtt. Ez lehetőséget biztosít, hello toocontrol rugalmasságot az alkalmazás és a környezet közben a virtuális gép pillanatképek van véve.

Az ebben az esetben is fontos tooensure alkalmazáskonzisztens virtuális gép biztonsági mentését. hello előtti parancsfájl alkalmazás natív API-k tooquiesce hello IOs elindíthat és memórián belüli tartalom toohello lemezre ürítése. Ez biztosítja, hogy hello pillanatkép alkalmazáskonzisztens (Ez azt jelenti, hogy adott hello alkalmazás felmerül mikor virtuális gép indult hello visszaállítás utáni). Utáni parancsfájl használt toothaw hello IOs lehet. Ennek érdekében alkalmazás natív API-val, így hello alkalmazás folytathatja a normál működés post-virtuális gép pillanatfelvétele.

## <a name="steps-tooconfigure-pre-script-and-post-script"></a>Parancsfájl előtti és utáni parancsfájl lépéseket tooconfigure

1. A legfelső szintű felhasználói toohello Linux virtuális Gépet, amely tooback akarja hello aláírásához.

2. Töltse le **VMSnapshotScriptPluginConfig.json** a [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), majd átmásolja toohello **/etc/azure** tooback fel fog hello gépek mappájába. Hozzon létre hello **/etc/azure** könyvtárat, ha már nem létezik.

3. Hello másolás előtti parancsfájl és az alkalmazás összes hello virtuális gépek a kívánt tooback mentése utáni parancsfájlt. Hello VM hello parancsfájlok tooany helyre másolhatja. Lehet, hogy tooupdate hello teljes elérési útját hello hello a parancsfájlok **VMSnapshotScriptPluginConfig.json** fájlt.

4. Ellenőrizze, hogy a következő engedélyeket a fájlok hello:

   - **VMSnapshotScriptPluginConfig.json**: engedély "600." Például csak a "Gyökér" felhasználó "read" és "write" engedélyek toothis fájl kell rendelkeznie, és nincs felhasználói "hajtható végre" engedélyekkel kell rendelkeznie.

   - **Előtti parancsfájl**: engedély "700."  Például csak a "Gyökér" felhasználói rendelkeznie kell "Olvasás", "write" és "végrehajtás" engedélyek toothis fájlt.
  
   - **Utáni parancsfájl** engedély "700." Például csak a "Gyökér" felhasználói rendelkeznie kell "Olvasás", "write" és "végrehajtás" engedélyek toothis fájlt.

   > [!Important]
   > hello keretrendszer teljesítményigényű nyújt a felhasználóknak. Fontos, hogy biztonságos, és csak "Gyökér" rendelkezik toocritical JSON és a parancsfájl fájlok elérése.
   > Ha hello előző követelmények nem teljesültek, hello parancsprogram nem futott. Az eredmény rendszerösszeomlás/konzisztens biztonsági mentés.
   >

5. Konfigurálása **VMSnapshotScriptPluginConfig.json** itt leírtak szerint:
    - **pluginName**: mivel ezt a mezőt hagyja, vagy a parancsfájlok nem működnek megfelelően.

    - **preScriptLocation**: hello hello előtti parancsfájl teljes elérési útja meg folyamatos toobe biztonsági másolatba mentett virtuális gép hello biztosítanak.

    - **postScriptLocation**: hello hello utáni parancsfájl teljes elérési útja meg folyamatos toobe biztonsági másolatba mentett virtuális gép hello biztosítanak.

    - **preScriptParams**: hello választható paraméterek: toohello előtti parancsfájl átadott toobe igénylő adja meg. Minden paraméter idézőjelek között kell lennie, és vesszővel elválasztva, ha több paramétert kell lennie.

    - **postScriptParams**: hello választható paraméterek: toohello utáni parancsfájl átadott toobe igénylő adja meg. Minden paraméter idézőjelek között kell lennie, és vesszővel elválasztva, ha több paramétert kell lennie.

    - **preScriptNoOfRetries**: hello ennyiszer hello előtti parancsfájl meg kell ismételni, ha hiba történik megszakítása előtt állítsa be. Nulla azt jelenti, hogy csak egy try és nem az újra gombra, ha hiba történik.

    - **postScriptNoOfRetries**: hello ennyiszer hello utáni parancsfájl meg kell ismételni, ha hiba történik megszakítása előtt állítsa be. Nulla azt jelenti, hogy csak egy try és nem az újra gombra, ha hiba történik.
    
    - **Időkorlát_másodpercben**: Adjon meg egyedi időtúllépések hello előtti parancsfájl és hello utáni parancsfájlt.

    - **continueBackupOnFailure**: az érték túl**igaz** Ha szeretné az Azure biztonsági mentési toofall hátsó tooa fájlrendszer konzisztens/összeomlás konzisztens biztonsági mentése, ha előtti parancsfájl vagy utáni parancsfájl sikertelen. Beállítás túl**hamis** sikertelen hello biztonsági mentési (kivéve, ha rendelkezik egyszerű VM esik vissza toocrash konzisztens biztonsági mentését végző ettől a beállítástól függetlenül) a parancsfájl hiba esetén.

    - **fsFreezeEnabled**: Adja meg, hogy Linux fsfreeze kell hívható, amíg hello VM pillanatkép tooensure fájlrendszer-konzisztenciával van véve. Azt javasoljuk, hogy ez a beállítás túl beállítása**igaz** , ha az alkalmazás a fsfreeze letiltása függőség van.

6. hello parancsfájl keretrendszer konfigurálva van. Ha hello VM biztonsági mentés már be van állítva, hello következő biztonsági mentés hello parancsfájlok hív, és elindítja a alkalmazáskonzisztens biztonsági mentését. Ha nincs konfigurálva a hello virtuális gép biztonsági mentése, konfigurálja úgy a [készítsen biztonsági másolatot az Azure virtuális gépek tooRecovery szolgáltatások tárolók.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Hibaelhárítás

Győződjön meg arról, hogy hozzáadja a megfelelő naplózási írásakor, a parancsfájl előtti és utáni parancsfájl, és tekintse át a parancsfájl naplók toofix parancsfájl probléma merül fel. Ha a parancsfájlok futtatásához hiba továbbra is fennáll, tekintse meg a toohello a következő táblázat további információt.

| Hiba | Hibaüzenet | Javasolt művelet |
| ------------------------ | -------------- | ------------------ |
| Előre-ScriptExecutionFailed |hello előtti parancsfájl hibát jelzett, így előfordulhat, hogy a biztonsági másolat nem használható alkalmazáskonzisztens. | Tekintse meg a parancsfájl toofix hello probléma hello hiba naplókat.|  
|   POST-ScriptExecutionFailed |    hello utáni parancsfájl, amely hatással lehet a alkalmazásállapot hibát adott vissza. |  Hello hibát talál a parancsfájl toofix hello problémára, és hello állapotának ellenőrzése. |
| Előre-ScriptNotFound |  hello előtti parancsfájl nem található hello megadott hello helyen **VMSnapshotScriptPluginConfig.json** konfigurációs fájlban. | Győződjön meg arról, hogy előtti parancsfájl verziója hello config tooensure alkalmazáskonzisztens biztonsági mentés megadott hello elérési úton található.|
| POST-ScriptNotFound | hello utáni parancsfájl nem található a következő hello helyen hello megadott **VMSnapshotScriptPluginConfig.json** konfigurációs fájlban. | Győződjön meg arról, hogy utáni parancsfájl verziója hello config tooensure alkalmazáskonzisztens biztonsági mentés megadott hello elérési úton található.|
| IncorrectPluginhostFile | Hello **Pluginhost** fájl, amely hello VmSnapshotLinux bővítményt, sérült, így parancsfájl előtti és utáni parancsfájl nem futtatható, és hello a biztonsági másolat nem használható alkalmazáskonzisztens.   | Távolítsa el a hello **VmSnapshotLinux** bővítményt, és automatikusan újratelepíti hello következő biztonsági mentési toofix hello probléma megoldásában. |
| IncorrectJSONConfigFile | Hello **VMSnapshotScriptPluginConfig.json** fájl helytelen, így előtti parancsfájl és utáni parancsfájl nem futtatható és hello a biztonsági másolat nem használható alkalmazáskonzisztens. | Töltse le a hello másolását [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) és állítsa be újra. |
| InsufficientPermissionforPre-parancsfájl | Parancsfájlok futtatásához, "Gyökér" felhasználói hello fájl hello tulajdonosának kell lennie, és hello fájl "700" engedélyekkel kell rendelkezniük (Ez azt jelenti, hogy csak a "tulajdonos" kell "Olvasás", "write" és "végrehajtási" engedélyeket). | Ellenőrizze, hogy "Gyökér" felhasználó "tulajdonosként" hello parancsfájl hello és, hogy csak a "tulajdonos" tartalmaz "Olvasás", "write" és "hajtható végre". |
| InsufficientPermissionforPost-parancsfájl | A parancsfájlok futtatásához, gyökér szintű felhasználó hello fájl hello tulajdonosának kell lennie, és hello fájl "700" engedélyekkel kell rendelkezniük (Ez azt jelenti, hogy csak a "tulajdonos" kell "Olvasás", "write" és "végrehajtási" engedélyeket). | Ellenőrizze, hogy "Gyökér" felhasználó "tulajdonosként" hello parancsfájl hello és, hogy csak a "tulajdonos" tartalmaz "Olvasás", "write" és "hajtható végre". |
| Előre-ScriptTimeout | hello hello alkalmazáskonzisztens biztonsági mentés előtti parancsfájl végrehajtása időtúllépés miatt megszakadt. | Ellenőrizze a hello parancsfájl, és növelje a hello hello időtúllépés **VMSnapshotScriptPluginConfig.json** címen található fájl **/etc/azure**. |
| POST-ScriptTimeout | hello végrehajtási hello alkalmazáskonzisztens biztonsági mentés utáni parancsfájlja túllépte az időkorlátot. | Ellenőrizze a hello parancsfájl, és növelje a hello hello időtúllépés **VMSnapshotScriptPluginConfig.json** címen található fájl **/etc/azure**. |

## <a name="next-steps"></a>Következő lépések
[Virtuális gép biztonsági mentési tooa Recovery Services-tároló konfigurálása](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
