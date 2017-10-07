---
title: aaaSupported platformon az Azure Security Centerben |} Microsoft Docs
description: "Ez a dokumentum az Azure Security Centerben támogatott Windows és Linux-operatings rendszerek listáját jeleníti meg."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: f73e04970749271fc9d75836f4f468b0a4953f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="supported-platforms-in-azure-security-center"></a>Az Azure Security Centerben a támogatott platformok
Biztonsági állapotának megfigyelése és javaslatok hello klasszikus és Resource Manager üzembe helyezési modell használatával létrehozott virtuális gépek (VM) érhetők el.

> [!NOTE]
> További tudnivalók hello [klasszikus és Resource Manager üzembe helyezési modell](../azure-classic-rm.md) az Azure-erőforrások.
>
>

## <a name="supported-platforms-for-windows-vms"></a>Windows virtuális gépek által támogatott platformok
Támogatott Windows operációs rendszerek:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

> [!NOTE]
>
* Az operációs rendszer biztonsági rés értékelése még nem állnak rendelkezésre a Windows Server 2016.
* Összeomlás-elemzési észlelések esetén támogatottak a Windows Server 2012 és Windows Server 2012 R2.
>
>

## <a name="supported-platforms-for-linux-vms"></a>Linux virtuális gépek által támogatott platformok
Támogatott Linux operációs rendszerek:

* Ubuntu verziók 12.04, 14.04, 16.04, 16.10
* Debian verziók 7, 8
* A centOS 6 verziók. \*, 7.*
* Red Hat Enterprise Linux (RHEL) verziók 6. \*, 7.*
* SUSE Linux Enterprise Server (SLES) 11 SP4 + verziók, 12.*
* Oracle Linux 6 verziók. \*, 7.*

> [!NOTE]
> Virtuális gép viselkedéselemzés még nem állnak rendelkezésre a Linux operációs rendszereken.
>
>

## <a name="vms-and-cloud-services"></a>Virtuális gépek és Felhőszolgáltatások
Felhőszolgáltatásban futó virtuális gépeket is támogatottak. Csak felhőalapú szolgáltatások üzembe helyezési ponti figyelt éles környezetben futó webes és feldolgozói szerepköröket. További információ a felhőalapú szolgáltatás, toolearn lásd [felhőalapú szolgáltatások – áttekintés](../cloud-services/cloud-services-choose-me.md).

## <a name="next-steps"></a>Következő lépések

- [Azure Security Center tervezéséhez és az üzemeltetési útmutatóban](security-center-planning-and-operations-guide.md) – további hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése
- [Biztonsági riasztások az Azure Security Centerben típusonként](https://docs.microsoft.com/en-us/azure/security-center/security-center-alerts-type.md#virtual-machine-behavioral-analysis) – további információk a virtuális gép viselkedéselemzése és összeomlás-memóriakép memória elemzése a Security Center
- [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban
- [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi
