---
title: "aaaHow toodocument adatforrások |} Microsoft Docs"
description: "Hogyan-tooarticle kiemelés hogyan az Azure Data Catalog toodocument adategységeket."
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
ms.openlocfilehash: 4e46838d91ab4d0c0bc569ac526a0c729134bb10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="document-data-sources"></a><span data-ttu-id="ef1d8-103">Adatforrások dokumentálása</span><span class="sxs-lookup"><span data-stu-id="ef1d8-103">Document data sources</span></span>
## <a name="introduction"></a><span data-ttu-id="ef1d8-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="ef1d8-104">Introduction</span></span>
<span data-ttu-id="ef1d8-105">**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-105">**Microsoft Azure Data Catalog** is a fully managed cloud service that serves as a system of registration and system of discovery for enterprise data sources.</span></span> <span data-ttu-id="ef1d8-106">Más szóval **Azure Data Catalog** minden körül útmutatás nyújtása a felhasználók felderítésére, *megértéséhez*, és adatforrások használja, és a meglévő adatok több értéket szervezetek tooget védi.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-106">In other words, **Azure Data Catalog** is all about helping people discover, *understand*, and use data sources, and helping organizations tooget more value from their existing data.</span></span>

<span data-ttu-id="ef1d8-107">Ha egy adatforrás regisztrálva van **Azure Data Catalog**, a metaadatai másolt és indexelik hello szolgáltatást, de hello szövegegység nincs nem végződhet.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-107">When a data source is registered with **Azure Data Catalog**, its metadata is copied and indexed by hello service, but hello story doesn’t end there.</span></span> <span data-ttu-id="ef1d8-108">**Az Azure Data Catalog** is lehetővé teszi a felhasználók tooprovide saját hello használati és hello adatforrás gyakori forgatókönyvei leíró teljes dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-108">**Azure Data Catalog** also allows users tooprovide their own complete documentation that can describe hello usage and common scenarios for hello data source.</span></span>

<span data-ttu-id="ef1d8-109">A [hogyan tooannotate adatforrások](data-catalog-how-to-annotate.md), elsajátíthatja, hogy hello adatforrás ismerő szakértők megjegyzéseket fűzhet, a címkéket és egy leírást.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-109">In [How tooannotate data sources](data-catalog-how-to-annotate.md), you learn that experts who know hello data source can annotate it with tags and a description.</span></span> <span data-ttu-id="ef1d8-110">Hello **Azure Data Catalog** portál tartalmaz egy rich text formátumú szerkesztőt, így a felhasználók is teljes mértékben dokumentum-adategységeket és tárolók.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-110">hello **Azure Data Catalog** portal includes a rich text editor so that users can fully document data assets and containers.</span></span> <span data-ttu-id="ef1d8-111">hello szerkesztőjének bekezdésformázás, például a fejlécére kattintva rendezhető, szövegformázási, felsorolásokat, számozott listák és táblákat.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-111">hello editor includes paragraph formatting, such as headings, text formatting, bulleted lists, numbered lists, and tables.</span></span>

<span data-ttu-id="ef1d8-112">Címkék és leírások nem egyszerű jegyzetek nagy-e.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-112">Tags and descriptions are great for simple annotations.</span></span> <span data-ttu-id="ef1d8-113">Azonban toohelp az adatfelhasználók jobb megértése érdekében hello használata egy adatforrás, és üzleti forgatókönyvek egy adatforrásban, szakértő nyújthat teljes, részletes dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-113">However, toohelp data consumers better understand hello use of a data source, and business scenarios for a data source, an expert can provide complete, detailed documentation.</span></span> <span data-ttu-id="ef1d8-114">Egyszerű toodocument adatforrás.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-114">It's easy toodocument a data source.</span></span> <span data-ttu-id="ef1d8-115">Jelöljön ki egy adategységet, vagy a tárolót, és válassza a **dokumentáció**.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-115">Select a data asset or container, and choose **Documentation**.</span></span>

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a><span data-ttu-id="ef1d8-116">Adategységek dokumentálása</span><span class="sxs-lookup"><span data-stu-id="ef1d8-116">Documenting data assets</span></span>
<span data-ttu-id="ef1d8-117">hello előnye, hogy **Azure Data Catalog** dokumentáció toouse lehetővé teszi az adatkatalógus-, a tartalomtár toocreate az adategységet egy teljes leírását.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-117">hello benefit of **Azure Data Catalog** documentation allows you toouse your Data Catalog as a content repository toocreate a complete narrative of your data assets.</span></span> <span data-ttu-id="ef1d8-118">Ismerje meg a részletes tartalom, amely leírja a tárolók és táblákat.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-118">You can explore detailed content that describes containers and tables.</span></span> <span data-ttu-id="ef1d8-119">Ha már rendelkezik tartalmat egy másik tartalomtár, SharePoint vagy egy fájlmegosztást, például a meglévő tartalom toohello eszköz dokumentációja hivatkozások tooreference adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-119">If you already have content in another content repository, such as SharePoint or a file share, you can add toohello asset documentation links tooreference this existing content.</span></span> <span data-ttu-id="ef1d8-120">A szolgáltatás elérhetővé teszi a meglévő dokumentumokat felfedezését.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-120">This feature makes your existing documents more discoverable.</span></span>

> [!NOTE]
> <span data-ttu-id="ef1d8-121">Dokumentációjában nem szerepel a search-index.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-121">Documentation is not included in search index.</span></span>
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

<span data-ttu-id="ef1d8-122">hello dokumentáció szint között lehet: hello jellemzőit és a értéke eszköz tároló tooa részletes leírása a táblaséma olyan tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-122">hello level of documentation can range from describing hello characteristics and value of a data asset container tooa detailed description of table schema within a container.</span></span> <span data-ttu-id="ef1d8-123">hello szintű dokumentációjában kell vezeti által az üzleti igényeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-123">hello level of documentation provided should be driven by your business needs.</span></span> <span data-ttu-id="ef1d8-124">De általában az alábbiakban néhány és dokumentálásához adategységek:</span><span class="sxs-lookup"><span data-stu-id="ef1d8-124">But in general, here are a few pros and cons of documenting data assets:</span></span>

* <span data-ttu-id="ef1d8-125">Dokumentálja csak tárolóként: Tartalomtallózó hello egy helyen, de előfordulhat, hogy kevés szükséges adatokat a felhasználók toomake tájékozott döntést.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-125">Document just a container: All hello content is in one place, but might lack necessary details for users toomake an informed decision.</span></span>
* <span data-ttu-id="ef1d8-126">Dokumentálja csak hello táblák: tartalom adott toothat objektum, de a felhasználók rendelkeznek-e a dokumentumok több helyen.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-126">Document just hello tables: Content is specific toothat object, but your users have multiple places for documents.</span></span>
* <span data-ttu-id="ef1d8-127">Dokumentálja a tárolók és táblák: a minden részletre módszert használja, de további karbantartási hello dokumentumok tervezi.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-127">Document containers and tables: Most comprehensive approach, but might introduce more maintenance of hello documents.</span></span>

## <a name="summary"></a><span data-ttu-id="ef1d8-128">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ef1d8-128">Summary</span></span>
<span data-ttu-id="ef1d8-129">Adatforrások dokumentálása **Azure Data Catalog** mértékű részletesen szükség szerint a leírását az adategységekre vonatkozó hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-129">Documenting data sources with **Azure Data Catalog** can create a narrative about your data assets in as much detail as you need.</span></span>  <span data-ttu-id="ef1d8-130">Hivatkozások használatával tárolja egy meglévő tartalomtár, amely egyesíti a meglévő docs és adategységeket toocontent társíthatja.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-130">By using links, you can link toocontent stored in an existing content repository, which brings your existing docs and data assets together.</span></span> <span data-ttu-id="ef1d8-131">Ha a felhasználók adategységeket megfelelő, rendelkezhetnek dokumentáció teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="ef1d8-131">Once your users discover appropriate data assets, they can have a complete set of documentation.</span></span>
