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
# <a name="guide-toocreate-a-solution-template-for-azure-marketplace"></a><span data-ttu-id="ac85a-103">Az útmutató toocreate Azure piactér megoldás sablonját</span><span class="sxs-lookup"><span data-stu-id="ac85a-103">Guide toocreate a solution template for Azure Marketplace</span></span>
<span data-ttu-id="ac85a-104">1. lépésben befejezése után [fióklétrehozás és a regisztrációs][link-acct-creation], azt interaktív, a hello létrehozásakor egy Azure-kompatibilis megoldás sablon, [műszaki Előfeltételek létrehozásához egy megoldás sablon](marketplace-publishing-solution-template-creation-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="ac85a-104">After completing step 1, [Account creation and registration][link-acct-creation], we guided you on hello creation of an Azure-compatible solution template at [Technical prerequisites for creating a solution template](marketplace-publishing-solution-template-creation-prerequisites.md).</span></span> <span data-ttu-id="ac85a-105">Most végigvezetjük meg hello lépéseket megoldás sablonok létrehozásának több virtuális géphez hello [közzétételi Portáljára] [ link-pubportal] a hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="ac85a-105">Now we will walk you through hello steps for creating a solution template for multiple VMs on hello [Publishing Portal][link-pubportal] for hello Azure Marketplace.</span></span>

## <a name="create-your-solution-template-offer-in-hello-publishing-portal"></a><span data-ttu-id="ac85a-106">A megoldás sablon ajánlat hello közzétételi Portáljára létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac85a-106">Create your solution template offer in hello Publishing Portal</span></span>
<span data-ttu-id="ac85a-107">Nyissa meg túl [https://publish.windowsazure.com](http://publish.windowsazure.com). A hello első alkalommal toohello bejelentkezéskor [közzétételi Portáljára](https://publish.windowsazure.com/), a vállalat értékesítő profil regisztrálva lett a fiókot használja hello.</span><span class="sxs-lookup"><span data-stu-id="ac85a-107">Go too [https://publish.windowsazure.com](http://publish.windowsazure.com). When signing in for hello first time toohello [Publishing Portal](https://publish.windowsazure.com/), use hello same account with which your company’s seller profile was registered.</span></span> <span data-ttu-id="ac85a-108">Később a vállalat bármely alkalmazott is hozzáadhat a közzétételi Portáljára hello co-rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ac85a-108">Later, you can add any employee of your company as a co-admin in hello Publishing Portal.</span></span>

### <a name="1-select-solution-templates"></a><span data-ttu-id="ac85a-109">1. Válassza ki a "Solution templates"</span><span class="sxs-lookup"><span data-stu-id="ac85a-109">1. Select "Solution templates"</span></span>
  ![rajz][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a><span data-ttu-id="ac85a-111">2. Hozzon létre egy új megoldássablon</span><span class="sxs-lookup"><span data-stu-id="ac85a-111">2. Create a new solution template</span></span>
  ![rajz][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a><span data-ttu-id="ac85a-113">3. Indítsa el a topológiák</span><span class="sxs-lookup"><span data-stu-id="ac85a-113">3. Start with topologies</span></span>
<span data-ttu-id="ac85a-114">A megoldássablon egy hozzá tartozó topológia "szülőeleme" tooall.</span><span class="sxs-lookup"><span data-stu-id="ac85a-114">A solution template is a "parent" tooall of its topologies.</span></span> <span data-ttu-id="ac85a-115">Egy ajánlat/megoldássablonban több topológiát is megadhat.</span><span class="sxs-lookup"><span data-stu-id="ac85a-115">You can define multiple topologies in one offer/solution template.</span></span> <span data-ttu-id="ac85a-116">Amikor egy ajánlatot a rendszer előkészítésre toostaging, akkor az összes topológiájával együtt továbbítja.</span><span class="sxs-lookup"><span data-stu-id="ac85a-116">When an offer is pushed toostaging, it is pushed with all of its topologies.</span></span> <span data-ttu-id="ac85a-117">Lépésekkel hello alatt toodefine ajánlatát:</span><span class="sxs-lookup"><span data-stu-id="ac85a-117">Follow hello steps below toodefine your offer:</span></span>     

* <span data-ttu-id="ac85a-118">Az olyan topológiák létrehozását: "topológia" azonosító általában hello megoldás sablon hello topológia hello nevét.</span><span class="sxs-lookup"><span data-stu-id="ac85a-118">Create a Topology: “Topology Identifier” is typically hello name of hello topology for hello solution template.</span></span> <span data-ttu-id="ac85a-119">hello topológia azonosítója hello URL-CÍMÉT használja a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="ac85a-119">hello topology identifier is used in hello URL as shown below:</span></span>

  <span data-ttu-id="ac85a-120">Az Azure piactér: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="ac85a-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}</span></span>

  <span data-ttu-id="ac85a-121">Azure-portálon: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="ac85a-121">Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}</span></span>
* <span data-ttu-id="ac85a-122">Adjon hozzá egy új verziót.</span><span class="sxs-lookup"><span data-stu-id="ac85a-122">Add a new version.</span></span>

### <a name="4-get-your-topology-versions-certified"></a><span data-ttu-id="ac85a-123">4. A hitelesített topológia verziók beolvasása</span><span class="sxs-lookup"><span data-stu-id="ac85a-123">4. Get your topology versions certified</span></span>
<span data-ttu-id="ac85a-124">Amely tartalmazza az összes szükséges fájlok tooprovision hello topológia, hogy adott verziójának zip-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="ac85a-124">Upload a zip file that contains all required files tooprovision that particular version of hello topology.</span></span> <span data-ttu-id="ac85a-125">A zip-fájl hello következőket kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="ac85a-125">This zip file must contain hello following:</span></span>

* <span data-ttu-id="ac85a-126">*mainTemplate.json* és *createUiDefinition.json* fájlt a gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="ac85a-126">*mainTemplate.json* and *createUiDefinition.json* file at its root directory.</span></span>
* <span data-ttu-id="ac85a-127">Bármely kapcsolt sablonok és a parancsfájlok az összes szükséges.</span><span class="sxs-lookup"><span data-stu-id="ac85a-127">Any linked templates and all required scripts.</span></span>

  > [!TIP]
  > <span data-ttu-id="ac85a-128">A fejlesztők hello megoldás topológiák sablon létrehozásakor és rávenni a hitelesített, hello üzleti munka közben a vállalat marketing, és/vagy jogi részlegek hello marketing és jogi tartalmát is dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="ac85a-128">While your developers work on creating hello solution template topologies and getting them certified, hello business, marketing, and/or legal departments of your company can work on hello marketing and legal content.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="ac85a-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac85a-129">Next steps</span></span>
<span data-ttu-id="ac85a-130">Most, hogy a megoldás sablon létrehozása és hello zip-fájl feltöltése, kérjük, kövesse hello hello utasításait [piactér marketing content útmutató](marketplace-publishing-push-to-staging.md) hello ajánlat toostaging előtt.</span><span class="sxs-lookup"><span data-stu-id="ac85a-130">Now that you created your solution template and uploaded hello zip file, please follow hello instructions in hello [Marketplace marketing content guide](marketplace-publishing-push-to-staging.md) before pushing hello offer toostaging.</span></span> <span data-ttu-id="ac85a-131">látogasson el a toosee hello a teljes készletének cikkek, közzététele piactér [első lépések: hogyan toopublish egy ajánlat toohello Azure piactér](marketplace-publishing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ac85a-131">toosee hello full set of marketplace publishing articles, visit [Getting started: How toopublish an offer toohello Azure Marketplace](marketplace-publishing-getting-started.md).</span></span>

<span data-ttu-id="ac85a-132">Is érdekelheti a kapcsolódó cikkekben:</span><span class="sxs-lookup"><span data-stu-id="ac85a-132">You might also be interested in these related articles:</span></span>

* <span data-ttu-id="ac85a-133">Virtuálisgép-rendszerképek: [kapcsolatos virtuálisgép-lemezképeket az Azure-ban](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span><span class="sxs-lookup"><span data-stu-id="ac85a-133">VM images: [About Virtual Machine Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span></span>
* <span data-ttu-id="ac85a-134">Virtuálisgép-bővítmények: [Virtuálisgép-ügynök és Virtuálisgép-bővítmények áttekintése](https://msdn.microsoft.com/library/azure/dn832621.aspx) és [Azure Virtuálisgép-bővítmények és szolgáltatások](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span><span class="sxs-lookup"><span data-stu-id="ac85a-134">VM extensions: [VM Agent and VM Extensions Overview](https://msdn.microsoft.com/library/azure/dn832621.aspx) and [Azure VM Extensions and Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span></span>
* <span data-ttu-id="ac85a-135">Az Azure Resource Manager: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) és [egyszerű sablon példák](https://github.com/rjmax/ArmExamples)</span><span class="sxs-lookup"><span data-stu-id="ac85a-135">Azure Resource Manager: [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) and [Simple Template Examples](https://github.com/rjmax/ArmExamples)</span></span>
* <span data-ttu-id="ac85a-136">A tárfiók azelőtt gyorsítja fel: [hogyan tárolási fiók szabályozás tooMonitor](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) és [prémium szintű storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span><span class="sxs-lookup"><span data-stu-id="ac85a-136">Storage account throttles: [How tooMonitor for Storage Account Throttling](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) and [Premium storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span></span>

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
