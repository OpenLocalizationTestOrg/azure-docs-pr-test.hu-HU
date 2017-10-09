---
title: "az AWS tooAzure virtuális gépek aaaMigrate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toomigrate virtuális gépen fut az Azure Site Recovery segítségével Amazon Web Services (AWS) tooAzure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: c99b781ec9cca5b8f9a847d3fc48408062b120b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-tooazure-with-azure-site-recovery"></a>Az Azure Site Recovery Amazon Web Services (AWS) tooAzure lévő virtuális gépek áttelepítése

Ez a cikk ismerteti, hogyan toomigrate AWS Windows-példányok tooAzure virtuális gépek hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Az áttelepítés akkor hatékonyan a feladatátvételt az AWS tooAzure. Feladat-visszavételi gépek tooAWS nem lehet, és nincs folyamatban lévő replikáció van. Ez a cikk hello hello Azure-portálon az áttelepítési lépéseit mutatja be, és hello utasításokat alapuló [replikálni a fizikai gép tooAzure](site-recovery-vmware-to-azure.md).

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek

A Site Recovery hello a következő operációs rendszereket futtató használt toomigrate EC2 példányok egyike lehet:

- Windows (csak a 64 bites)
    - Windows Server 2008 R2 SP1 + (Citrix PV illesztőprogramok vagy csak az AWS PV illesztőprogramok. **RedHat PV illesztőprogramok futó példányok nem támogatottak**) Windows Server 2012 Windows Server 2012 R2
- Linux (csak a 64 bites)
    - Red Hat Enterprise Linux 6.7 (csak HVM virtuális példány)

## <a name="prerequisites"></a>Előfeltételek

Ez mit kell ehhez a központi telepítéshez:

* **Konfigurációs kiszolgáló**: az Amazon EC2 virtuális gép Windows Server 2012 R2 rendszerű hello konfigurációs kiszolgálón van telepítve. Alapértelmezés szerint hello más Azure Site Recovery (a folyamatkiszolgáló és fő célkiszolgáló) ezeket az összetevőket hello konfigurációs kiszolgáló telepítésekor. Ez a cikk hello hello Azure-portálon az áttelepítési lépéseit mutatja be, és hello utasításokat alapuló [további](site-recovery-components.md)

* **EC2 példányok**: hello Amazon EC2 virtuális gépek példányok, amelyet az toomigrate.

## <a name="deployment-steps"></a>A központi telepítés lépései

1. Recovery Services-tároló létrehozása.
2. hello biztonsági csoport a EC2 példányok rendelkeznie kell beállított szabályokat tooallow kommunikációs hello EC2 példányt, amelyet toomigrate és hello példány tervezett toodeploy hello konfigurációs kiszolgáló között.

3. Hello azonos Amazon virtuális Magánfelhő a EC2 példányként telepítse az automatikus rendszer-Helyreállítás konfigurációs kiszolgáló. Tekintse meg a hello VMware / fizikai konfigurációs kiszolgáló telepítési követelményei tooAzure előfeltételeit.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Amikor a konfigurációs kiszolgáló AWS üzembe helyezve, és a Recovery Services-tároló regisztrált, megtekintheti az hello konfigurációs kiszolgáló és a Site Recovery-infrastruktúra folyamatkiszolgáló alább látható módon:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. Hello konfigurációs kiszolgáló telepítése után (is igénybe vehet fel too15 minustes azt tooappear), ellenőrizze, hogy képes legyen kommunikálni hello virtuális gépeket, hogy szeretné-e toomigrate.

6. [Replikációs beállítások megadása](site-recovery-setup-replication-settings-vmware.md).

7. Engedélyezze a replikálást: engedélyezze a hello toomigrate kívánt virtuális gépek replikációját. Hello EC2 példányok hello magánhálózati IP-címek, hello EC2 konzolról csomagját segítségével felderíthetők.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. Hello EC2 példányokat a védett és hello replikációs tooAzure befejeződése után [feladatátvételi teszt futtatása](site-recovery-test-failover-to-azure.md) toovalidate az Azure-ban az alkalmazás teljesítményét.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Az AWS tooAzure feladatátvétel futtatása az egyes virtuális gépek. Szükség esetén a helyreállítási terv létrehozása, és a feladatátvétel, toomigrate több virtuális gép futtatása az AWS tooAzure. [További](site-recovery-create-recovery-plans.md) helyreállítási tervek.

10. Az áttelepítés feladatátvevő toocommit nincs szükség. Ehelyett hello az áttelepítés végrehajtásához a beállítást választja, minden gép toomigrate kívánja. az áttelepítés végrehajtásához művelet hello hello áttelepítési folyamat befejezését, hello gép replikálása eltávolítja, és leállítja a Site Recovery hello gép számlázási.

    ![Migrate (Áttelepítés)](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Következő lépések

- [Készítse elő az áttelepített gépek tooenable replikációs](site-recovery-azure-to-azure-after-migration.md) tooanother régió vész-helyreállítási igényekre.
- Gondoskodjon számítási feladatai védelméről az [Azure virtuális gépek replikálásával](site-recovery-azure-to-azure.md).
