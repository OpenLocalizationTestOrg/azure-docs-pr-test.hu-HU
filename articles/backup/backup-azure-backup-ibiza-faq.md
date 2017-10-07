---
title: "aaaRecovery szolgáltatások tároló – gyakori kérdések |} Microsoft Docs"
description: "Gyakran ismételt kérdések hello ezen verziója támogatja az Azure Backup szolgáltatás hello hello nyilvános előzetes kiadását. Válaszok toofrequently kérdések hello biztonságimásolat-készítő ügynök, biztonsági mentési és adatmegőrzési, helyreállítási, biztonsági és egyéb hello Azure biztonsági mentési megoldás gyakori kérdésekre."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "biztonsági mentési megoldás; biztonsági mentési szolgáltatás"
ms.assetid: 5f55b500-1ee9-4f64-9306-02d6f7a8eded
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: markgal;trinadhk;
ms.openlocfilehash: 882b2e67ed424dc9f3681a8870e6b4c7b4cdcaec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="recovery-services-vault---faq"></a>Recovery Services-tároló – gyakori kérdések
Ez a cikk nyújt információkat adott tooRecovery Services-tároló, és azt kiegészíti hello [Azure biztonsági mentés – gyakori kérdések](backup-azure-backup-faq.md). Azure biztonsági mentés – gyakori kérdések hello hello Azure Backup szolgáltatás kapcsolatos kérdések és válaszok teljes készletét hello biztosít.  

Azure biztonsági mentéssel kapcsolatos kérdéseit teheti fel a hello disqus-beszélgetésben teheti szakasz ebben a cikkben vagy a kapcsolódó cikkében. A hello is beküldheti hello Azure Backup szolgáltatás kapcsolatos kérdéseket [vitafóruma](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>A helyreállítási tárak a Resource Manageren alapulnak. A biztonsági mentési tárak (klasszikus módban) továbbra is támogatottak? <br/>
Igen, a Backup tárolók még támogatottak. Hozzon létre biztonsági mentési tárolók hello [klasszikus portál](https://manage.windowsazure.com). Recovery Services-tárolók létrehozása a hello [Azure-portálon](https://portal.azure.com). Azonban Ön toocreate recovery services-tároló az összes jövőbeli fejlesztések, erősen ajánlott lesz csak a Recovery Services-tároló.

## <a name="can-i-migrate-a-backup-vault-tooa-recovery-services-vault-br"></a>Áttelepítheti a biztonsági mentési tároló tooa Recovery Services-tároló? <br/>
Sajnos nem, most nem telepíthetők át a biztonsági mentési tároló tooa Recovery Services-tároló hello tartalmát. Jelenleg is dolgozunk ezen funkción, de a Nyilvános előzetes kiadásnak nem része.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>A Recovery Services-tárolók támogatják a klasszikus vagy a Resource Manager alapú virtuális gépeket? <br/>
A Recovery Services-tárolók mindkét modellt támogatják.  Készíthet biztonsági másolatot a virtuális gépek (amelyek klasszikus módban virtuális gépek) hello klasszikus portálon létrehozott, vagy egy virtuális Géphez létre hello Azure-portálon (amelyek Resource Manager-alapú) tooa Recovery Services-tároló.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-toomigrate-my-vms-from-classic-mode-tooresource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Biztonsági másolatot készítettem a klasszikus virtuális gépemről a Backup-tárolóban. Most kívánt toomigrate klasszikus módban tooResource Manager üzemmódról virtuális gépek.  Hogyan készíthetek róluk biztonsági másolatot a Recovery Services-tárolóban?
A mentési tároló klasszikus virtuális gépek biztonsági mentése nem telepíti át automatikusan toorecovery services-tároló hello virtuális gépeket telepít át klasszikus tooResource kezelő módban. Kövesse a virtuális gép biztonsági mentéseinek áttelepítésére vonatkozó lépéseket:

1. A mentési tároló váltson túl**védett elemek** fülre, és válassza ki a virtuális gép hello. Kattintson a [Stop Protection](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines) (Védelem leállítása) gombra. Hagyja a *Delete associated backup data* (Társított biztonsági mentési adatok törlése) beállítást **bejelöletlenül**.
2. A hello [Azure-portálon](https://portal.azure.com), toohello Ugrás **bővítmények** menüje hello a virtuális gép, és távolítsa el a hello **VMSnapshot/VMSnapshotLinux** bővítmény.
3. Hello virtuális gép áttelepítése a klasszikus mód tooResource Manager üzemmódból. Ellenőrizze, hogy tárolási és hálózati toovirtual gép megfelelő is áttelepített tooResource kezelő módban.
4. A recovery services-tároló létrehozása és konfigurálása hello a biztonsági mentési áttelepítése a virtuális gép **biztonsági mentési** művelet tároló irányítópult felett. További tudnivalókért, hogyan túl[engedélyezése a biztonsági mentés a recovery services-tároló](backup-azure-vms-first-look-arm.md)
