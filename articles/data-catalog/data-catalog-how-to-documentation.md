---
title: "Adatforrások dokumentálása |} Microsoft Docs"
description: "Útmutató a cikk kiemelés az Azure Data Catalog adategységeket dokumentálása."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 053b1701-b848-4ada-b726-6f485caa9961
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: ffe951f60afb57524568fe1ed3b3668d0088e124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="document-data-sources"></a><span data-ttu-id="d9165-103">Adatforrások dokumentálása</span><span class="sxs-lookup"><span data-stu-id="d9165-103">Document data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="d9165-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="d9165-104">Introduction</span></span>
<span data-ttu-id="d9165-105">**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál.</span><span class="sxs-lookup"><span data-stu-id="d9165-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="d9165-106">Más szóval **Azure Data Catalog** minden körül útmutatás nyújtása a felhasználók felderítésére, *megértéséhez*, és adatforrások használja, és segíti a szervezeteket kihasználása érdekében a meglévő adatokból.</span><span class="sxs-lookup"><span data-stu-id="d9165-106">In other words, **Azure Data Catalog** is all about helping people discover, *understand*, and use data sources, and helping organizations to get more value from their existing data.</span></span>

<span data-ttu-id="d9165-107">Ha egy adatforrás regisztrálva van **Azure Data Catalog**, a metaadatai másolt és a szolgáltatás indexelni, de a szövegegység nincs nem végződhet.</span><span class="sxs-lookup"><span data-stu-id="d9165-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by the service, but the story doesn’t end there.</span></span> <span data-ttu-id="d9165-108">**Az Azure Data Catalog** is lehetővé teszi a felhasználók a saját teljes dokumentáció, amely a használati és a gyakori forgatókönyvek az adatforrásra vonatkozóan is ismertetik.</span><span class="sxs-lookup"><span data-stu-id="d9165-108">**Azure Data Catalog** also allows users to provide their own complete documentation that can describe the usage and common scenarios for the data source.</span></span>

<span data-ttu-id="d9165-109">A [hogyan adatforrások ellátása megjegyzésekkel](data-catalog-how-to-annotate.md), elsajátíthatja, hogy az adatforrás ismerő szakértők megjegyzéseket fűzhet az címkék és egy leírást.</span><span class="sxs-lookup"><span data-stu-id="d9165-109">In [How to annotate data sources](data-catalog-how-to-annotate.md), you learn that experts who know the data source can annotate it with tags and a description.</span></span> <span data-ttu-id="d9165-110">A **Azure Data Catalog** portál tartalmaz egy rich text formátumú szerkesztőt, így a felhasználók is teljes mértékben dokumentum-adategységeket és tárolók.</span><span class="sxs-lookup"><span data-stu-id="d9165-110">The **Azure Data Catalog** portal includes a rich text editor so that users can fully document data assets and containers.</span></span> <span data-ttu-id="d9165-111">A szerkesztő bekezdésformázási lehetőségeket, például a fejlécére kattintva rendezhető, szövegformázás, felsorolásokat, számozott listák és táblákat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d9165-111">The editor includes paragraph formatting, such as headings, text formatting, bulleted lists, numbered lists, and tables.</span></span>

<span data-ttu-id="d9165-112">Címkék és leírások nem egyszerű jegyzetek nagy-e.</span><span class="sxs-lookup"><span data-stu-id="d9165-112">Tags and descriptions are great for simple annotations.</span></span> <span data-ttu-id="d9165-113">Az adatfelhasználók jobb megértése érdekében egy adatforrást egy adatforrást, és üzleti forgatókönyvek használata érdekében azonban szakértő nyújthat teljes, részletes dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="d9165-113">However, to help data consumers better understand the use of a data source, and business scenarios for a data source, an expert can provide complete, detailed documentation.</span></span> <span data-ttu-id="d9165-114">Akkor is könnyen adatforrás dokumentálása.</span><span class="sxs-lookup"><span data-stu-id="d9165-114">It's easy to document a data source.</span></span> <span data-ttu-id="d9165-115">Jelöljön ki egy adategységet, vagy a tárolót, és válassza a **dokumentáció**.</span><span class="sxs-lookup"><span data-stu-id="d9165-115">Select a data asset or container, and choose **Documentation**.</span></span>

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a><span data-ttu-id="d9165-116">Adategységek dokumentálása</span><span class="sxs-lookup"><span data-stu-id="d9165-116">Documenting data assets</span></span>
<span data-ttu-id="d9165-117">Az előnye, hogy **Azure Data Catalog** dokumentáció lehetővé teszi, hogy a tartalomtár a Data Catalog egy teljes leírását az adategységet létrehozásához használhatja.</span><span class="sxs-lookup"><span data-stu-id="d9165-117">The benefit of **Azure Data Catalog** documentation allows you to use your Data Catalog as a content repository to create a complete narrative of your data assets.</span></span> <span data-ttu-id="d9165-118">Ismerje meg a részletes tartalom, amely leírja a tárolók és táblákat.</span><span class="sxs-lookup"><span data-stu-id="d9165-118">You can explore detailed content that describes containers and tables.</span></span> <span data-ttu-id="d9165-119">Ha már rendelkezik tartalmat egy másik tartalomtár, SharePoint vagy egy fájlmegosztást, például a eszköz dokumentációjában mutató hivatkozás a meglévő tartalom adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="d9165-119">If you already have content in another content repository, such as SharePoint or a file share, you can add to the asset documentation links to reference this existing content.</span></span> <span data-ttu-id="d9165-120">A szolgáltatás elérhetővé teszi a meglévő dokumentumokat felfedezését.</span><span class="sxs-lookup"><span data-stu-id="d9165-120">This feature makes your existing documents more discoverable.</span></span>

> [!NOTE]
> <span data-ttu-id="d9165-121">Dokumentációjában nem szerepel a search-index.</span><span class="sxs-lookup"><span data-stu-id="d9165-121">Documentation is not included in search index.</span></span>
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

<span data-ttu-id="d9165-122">A szint a dokumentáció: a jellemzők és egy adattároló eszköz részletes leírása a táblaséma olyan tárolóban való érték között lehet.</span><span class="sxs-lookup"><span data-stu-id="d9165-122">The level of documentation can range from describing the characteristics and value of a data asset container to a detailed description of table schema within a container.</span></span> <span data-ttu-id="d9165-123">Dokumentációjában szintjét a üzleti igények alapján kell vezeti.</span><span class="sxs-lookup"><span data-stu-id="d9165-123">The level of documentation provided should be driven by your business needs.</span></span> <span data-ttu-id="d9165-124">De általában az alábbiakban néhány és dokumentálásához adategységek:</span><span class="sxs-lookup"><span data-stu-id="d9165-124">But in general, here are a few pros and cons of documenting data assets:</span></span>

* <span data-ttu-id="d9165-125">Dokumentálja csak tárolóként: a tartalom egy helyen, de előfordulhat, hogy nem rendelkezik a szükséges adatokat a felhasználók számára jól informált döntést.</span><span class="sxs-lookup"><span data-stu-id="d9165-125">Document just a container: All the content is in one place, but might lack necessary details for users to make an informed decision.</span></span>
* <span data-ttu-id="d9165-126">Csak a táblázatok dokumentum: tartalom csak az adott objektum, de a felhasználók rendelkeznek-e a dokumentumok több helyen.</span><span class="sxs-lookup"><span data-stu-id="d9165-126">Document just the tables: Content is specific to that object, but your users have multiple places for documents.</span></span>
* <span data-ttu-id="d9165-127">Dokumentálja a tárolók és a táblázatok: a minden részletre módszert használja, de okozhat a dokumentumok több karbantartási.</span><span class="sxs-lookup"><span data-stu-id="d9165-127">Document containers and tables: Most comprehensive approach, but might introduce more maintenance of the documents.</span></span>

## <a name="summary"></a><span data-ttu-id="d9165-128">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d9165-128">Summary</span></span>
<span data-ttu-id="d9165-129">Adatforrások dokumentálása **Azure Data Catalog** mértékű részletesen szükség szerint a leírását az adategységekre vonatkozó hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="d9165-129">Documenting data sources with **Azure Data Catalog** can create a narrative about your data assets in as much detail as you need.</span></span>  <span data-ttu-id="d9165-130">Hivatkozások használatával is kapcsolhat egy meglévő tartalomtár, amely egyesíti a meglévő docs és adategységeket tárolt tartalmat.</span><span class="sxs-lookup"><span data-stu-id="d9165-130">By using links, you can link to content stored in an existing content repository, which brings your existing docs and data assets together.</span></span> <span data-ttu-id="d9165-131">Ha a felhasználók adategységeket megfelelő, rendelkezhetnek dokumentáció teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="d9165-131">Once your users discover appropriate data assets, they can have a complete set of documentation.</span></span>
