---
title: "aaaGuide toocreating hello piactér megoldás sablonját |} Microsoft Docs"
description: "Részletes utasítások hogyan toocreate, hitelesíthet és központi telepítése a virtuális Gépre kiterjedő kép Megoldássablon az beszerzése a hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: b0e7067176337dd0d3f6f6ec04c963f80f706ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-solution-template-for-azure-marketplace"></a>Az útmutató toocreate Azure piactér megoldás sablonját
1. lépésben befejezése után [fióklétrehozás és a regisztrációs][link-acct-creation], azt interaktív, a hello létrehozásakor egy Azure-kompatibilis megoldás sablon, [műszaki Előfeltételek létrehozásához egy megoldás sablon](marketplace-publishing-solution-template-creation-prerequisites.md). Most végigvezetjük meg hello lépéseket megoldás sablonok létrehozásának több virtuális géphez hello [közzétételi Portáljára] [ link-pubportal] a hello Azure piactéren.

## <a name="create-your-solution-template-offer-in-hello-publishing-portal"></a>A megoldás sablon ajánlat hello közzétételi Portáljára létrehozása
Nyissa meg túl [https://publish.windowsazure.com](http://publish.windowsazure.com). A hello első alkalommal toohello bejelentkezéskor [közzétételi Portáljára](https://publish.windowsazure.com/), a vállalat értékesítő profil regisztrálva lett a fiókot használja hello. Később a vállalat bármely alkalmazott is hozzáadhat a közzétételi Portáljára hello co-rendszergazdaként.

### <a name="1-select-solution-templates"></a>1. Válassza ki a "Solution templates"
  ![rajz][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Hozzon létre egy új megoldássablon
  ![rajz][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Indítsa el a topológiák
A megoldássablon egy hozzá tartozó topológia "szülőeleme" tooall. Egy ajánlat/megoldássablonban több topológiát is megadhat. Amikor egy ajánlatot a rendszer előkészítésre toostaging, akkor az összes topológiájával együtt továbbítja. Lépésekkel hello alatt toodefine ajánlatát:     

* Az olyan topológiák létrehozását: "topológia" azonosító általában hello megoldás sablon hello topológia hello nevét. hello topológia azonosítója hello URL-CÍMÉT használja a lent látható módon:

  Az Azure piactér: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure-portálon: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}
* Adjon hozzá egy új verziót.

### <a name="4-get-your-topology-versions-certified"></a>4. A hitelesített topológia verziók beolvasása
Amely tartalmazza az összes szükséges fájlok tooprovision hello topológia, hogy adott verziójának zip-fájl feltöltése. A zip-fájl hello következőket kell tartalmaznia:

* *mainTemplate.json* és *createUiDefinition.json* fájlt a gyökérkönyvtárban.
* Bármely kapcsolt sablonok és a parancsfájlok az összes szükséges.

  > [!TIP]
  > A fejlesztők hello megoldás topológiák sablon létrehozásakor és rávenni a hitelesített, hello üzleti munka közben a vállalat marketing, és/vagy jogi részlegek hello marketing és jogi tartalmát is dolgozhat.
  >
  >

## <a name="next-steps"></a>Következő lépések
Most, hogy a megoldás sablon létrehozása és hello zip-fájl feltöltése, kérjük, kövesse hello hello utasításait [piactér marketing content útmutató](marketplace-publishing-push-to-staging.md) hello ajánlat toostaging előtt. látogasson el a toosee hello a teljes készletének cikkek, közzététele piactér [első lépések: hogyan toopublish egy ajánlat toohello Azure piactér](marketplace-publishing-getting-started.md).

Is érdekelheti a kapcsolódó cikkekben:

* Virtuálisgép-rendszerképek: [kapcsolatos virtuálisgép-lemezképeket az Azure-ban](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* Virtuálisgép-bővítmények: [Virtuálisgép-ügynök és Virtuálisgép-bővítmények áttekintése](https://msdn.microsoft.com/library/azure/dn832621.aspx) és [Azure Virtuálisgép-bővítmények és szolgáltatások](https://msdn.microsoft.com/library/azure/dn606311.aspx)
* Az Azure Resource Manager: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) és [egyszerű sablon példák](https://github.com/rjmax/ArmExamples)
* A tárfiók azelőtt gyorsítja fel: [hogyan tárolási fiók szabályozás tooMonitor](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) és [prémium szintű storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
