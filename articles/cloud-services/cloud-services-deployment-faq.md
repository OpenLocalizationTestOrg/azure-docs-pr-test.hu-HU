---
title: "a Microsoft Azure Cloud Services – gyakori kérdések aaaDeployment problémái |} Microsoft Docs"
description: "Ez a cikk gyakran ismételt kérdések a Microsoft Azure-szolgáltatásokhoz központi telepítési hello sorolja fel."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 8d67e36aa87fb5794d358e5cc235123ac7286028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure-szolgáltatásokhoz központi telepítési problémák: gyakran ismételt kérdések (GYIK)

Ez a cikk tartalmazza a központi telepítési problémái kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Is további részleteket hello [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-toohello-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-hello-production-slot"></a>Miért nem egy felhőalapú szolgáltatás toohello telepítése néha átmeneti tárolóhely hibajelzés erőforrás foglalási hiba miatt már hello éles tárolóhelyre a meglévő telepítés?
Ha egy felhőalapú szolgáltatás a központi telepítés vagy tárolóhelye van, a hello teljes felhőalapú szolgáltatás nem rögzített tooa adott fürt. Ez azt jelenti, hogy ha egy telepítés már létezik a hello éles tárolóhelyre, egy új átmeneti telepítési csak lehet hozzárendelni hello azonos fürtöt hello éles tárolóhelyre.

Pufferallokációs hibák történjen, ahol a felhőszolgáltatás hello fürtnek nincs elég fizikai számítási erőforrások toosatisfy központi telepítési kérelmét.

Ilyen pufferallokációs hibák kiküszöböléséhez útmutatásért lásd: [Felhőszolgáltatás memóriafoglalási hiba: megoldásokkal](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>Miért nem vertikális felskálázásával vagy scaling out néha egy felhőalapú szolgáltatás telepítését, ezért foglalási nem?
Amikor telepít egy felhőalapú szolgáltatás, rögzített tooa adott fürt általában lekérdezi. Ez azt jelenti, hogy vertikális felskálázásával/ki egy meglévő felhőalapú szolgáltatás kell rendelnie a hello új példányok ugyanabban a fürtben. Ha hello fürt közelít kapacitásának határához, vagy hello kívánt virtuális gép mérete/típus nem érhető el, a hello kérelem sikertelenek lehetnek.

Ilyen pufferallokációs hibák kiküszöböléséhez útmutatásért lásd: [Felhőszolgáltatás memóriafoglalási hiba: megoldásokkal](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>Miért nem olyan lesznek üzembe helyezve egy felhőalapú szolgáltatás affinitáscsoporthoz néha eredményez memóriafoglalási hiba?
Egy új központi telepítési tooan üres felhőalapú szolgáltatás oszthat ki az adott régióban, a fürt hello fabric által, kivéve, ha hello felhőszolgáltatás rögzített tooan affinitáscsoport. Központi telepítések toohello ugyanabban az affinitáscsoportban kísérli meg a rendszer a hello ugyanabban a fürtben. Ha hello fürt közelít kapacitásának határához, hello kérelem sikertelen lehet.

Ilyen pufferallokációs hibák kiküszöböléséhez útmutatásért lásd: [Felhőszolgáltatás memóriafoglalási hiba: megoldásokkal](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-tooan-existing-cloud-service-sometimes-result-in-allocation-failure"></a>Miért nem módosítja a Virtuálisgép-méretet vagy néha ad hozzá egy új virtuális gép tooan meglévő felhőalapú szolgáltatást, ezért foglalási nem?
hello fürtök esetén egy adatközpontban lehet (például egy sorozat Av2 adatsorozat, D sorozat, Dv2 adatsorozat, G adatsorozat, H adatsorozat, stb.) a gép típusok különböző konfigurációiról. De nem minden hello fürtök feltétlenül kell a virtuális gépek különböző hello. Például ha tooadd D sorozat már üzembe van helyezve A csak adatsorozat-fürtben lévő virtuális gép tooa felhőalapú szolgáltatás, a működés egy memóriafoglalási hiba. Ez akkor is történik, ha meg (például az A sorozat tooa D sorozatú való váltás) méretének toochange VM Termékváltozat.

Ilyen pufferallokációs hibák kiküszöböléséhez útmutatásért lásd: [Felhőszolgáltatás memóriafoglalási hiba: megoldásokkal](cloud-services-allocation-failures.md#solutions).

toocheck hello használható méret az adott régióban, lásd: [Microsoft Azure: régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-toolimitsquotasconstraints-on-my-subscription-or-service"></a>Miért does telepítése egy felhőalapú szolgáltatás bizonyos sikertelen esedékes toolimits/kvóták vagy korlátozza a saját előfizetés vagy szolgáltatás?
Egy felhőalapú szolgáltatás telepítése sikertelen lehet, ha a szükséges toobe lefoglalt hello erőforrásokhoz haladhatja meg a hello alapértelmezett vagy a szolgáltatás hello régió/adatközpont szinten engedélyezett kvóta felső határát. További információkért lásd: [Felhőszolgáltatások korlátozza](../azure-subscription-service-limits.md#cloud-services-limits).

Az előfizetést, hello portal sikerült is nyomon hello aktuális használati/kvótája: Azure portál = > előfizetések = > \<a megfelelő előfizetés > = > "Használati + kvóta".

Erőforrás-használat/fogyasztás kapcsolatos információkat is beolvashatók hello Azure számlázási API-k használatával. Lásd: [az Azure erőforrás-használat (előzetes verzió) API](../billing/billing-usage-rate-card-overview.md#azure-resource-usage-api-preview).

## <a name="how-can-i-change-hello-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>Hogyan lehet módosítani, üzembe helyezésével hello telepített felhőszolgáltatás virtuális gép mérete?
Hello Virtuálisgép-méretet a telepített felhőszolgáltatást, üzembe helyezésével nem módosítható. hello CSDEF, amely csak akkor lehet frissíteni a helyezze üzembe újra a beépített hello Virtuálisgép-méretet.

További információkért lásd: [hogyan tooupdate egy felhőalapú szolgáltatás](cloud-services-update-azure-service.md).

 
