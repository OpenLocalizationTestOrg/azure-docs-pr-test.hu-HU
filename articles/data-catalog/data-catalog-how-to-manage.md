---
title: "az Azure Data Catalog aaaManage adategységeket |} Microsoft Docs"
description: "hello cikk mutatja be, hogyan toocontrol láthatóságának és adategységeket tulajdonjogát regisztrálva az Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 48a634b92d7da19c32c9e551f295eec257f54f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a><span data-ttu-id="cd0ac-103">Az Azure Data Catalog adategységek felügyelete</span><span class="sxs-lookup"><span data-stu-id="cd0ac-103">Manage data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="cd0ac-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cd0ac-104">Introduction</span></span>
<span data-ttu-id="cd0ac-105">Az Azure Data Catalog adatforrás felderítés, célja, hogy akkor is könnyen megtalálhatóvá és értelmezhetővé hello adatforrások kell tooperform elemzés és döntéseket.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-105">Azure Data Catalog is designed for data-source discovery, so that you can easily discover and understand hello data sources you need tooperform analysis and make decisions.</span></span> <span data-ttu-id="cd0ac-106">E felderítés képességeivel hello legnagyobb hatást és más felhasználók is található, és elérhető adatforrások legszélesebb skáláját hello ismertetése.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-106">These discovery capabilities make hello biggest impact when you and other users can find and understand hello broadest range of available data sources.</span></span> <span data-ttu-id="cd0ac-107">Ezeknek az elemeknek szem előtt, a hello alapértelmezett Data Catalog működése minden regisztrált adatforrások toobe látható tooand felderíthető katalógus minden felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-107">With these elements in mind, hello default behavior of Data Catalog is for all registered data sources toobe visible tooand discoverable by all catalog users.</span></span>

<span data-ttu-id="cd0ac-108">A Data Catalog nem ad hozzáférési toohello adatokat saját magát.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-108">Data Catalog does not give you access toohello data itself.</span></span> <span data-ttu-id="cd0ac-109">Adat-hozzáférési hello adatforrás hello tulajdonosának vezérli.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-109">Data access is controlled by hello owner of hello data source.</span></span> <span data-ttu-id="cd0ac-110">A Data Catalog adatforrások felfedezése és a hello metaadatok hello katalógusban regisztrált kapcsolódó toohello adatforrások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-110">With Data Catalog, you can discover data sources and view hello metadata that's related toohello sources that are registered in hello catalog.</span></span>

<span data-ttu-id="cd0ac-111">Előfordulhat, azonban ha adatforrások használatuk csak akkor látható toospecific felhasználók vagy adott csoportok toomembers.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-111">There might be situations, however, where data sources should only be visible toospecific users, or toomembers of specific groups.</span></span> <span data-ttu-id="cd0ac-112">Ilyen esetekben a felhasználók saját tulajdonba vétele belül hello katalógusban regisztrált adategységeket és majd a hello láthatóságának saját hello eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-112">In such scenarios, users can take ownership of registered data assets within hello catalog and then control hello visibility of hello assets they own.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0ac-113">Ebben a cikkben leírt hello funkció csak a Standard Edition Azure Data Catalog hello érhető el.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-113">hello functionality described in this article is available only in hello Standard Edition of Azure Data Catalog.</span></span> <span data-ttu-id="cd0ac-114">hello ingyenes kiadás nem biztosít tulajdonjoga és adategységet láthatósági korlátozása képességét.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-114">hello Free Edition does not provide capabilities for ownership and restricting data-asset visibility.</span></span>
>
>

## <a name="manage-ownership-of-data-assets"></a><span data-ttu-id="cd0ac-115">Adategységek tulajdonjogát kezelése</span><span class="sxs-lookup"><span data-stu-id="cd0ac-115">Manage ownership of data assets</span></span>
<span data-ttu-id="cd0ac-116">Alapértelmezés szerint a Data Catalog regisztrált adategységeket található, a tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-116">By default, data assets that are registered in Data Catalog are unowned.</span></span> <span data-ttu-id="cd0ac-117">Minden engedélyt tooaccess hello katalógus rendelkező felhasználó felderíteni, és ezeknek az eszközöknek a jegyzet.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-117">Any user with permission tooaccess hello catalog can discover and annotate these assets.</span></span> <span data-ttu-id="cd0ac-118">A felhasználók tulajdonos nélküli adategységek saját tulajdonba és hello látható-e saját hello eszközök majd korlátozza.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-118">Users can take ownership of unowned data assets and then limit hello visibility of hello assets they own.</span></span>

<span data-ttu-id="cd0ac-119">A Data Catalog egy adategységet tulajdonosa, ha csak olyan felhasználók, akik jogosultak a hello tulajdonosai által hello eszköz felderítését, és megtekintheti a metaadatait, és csak hello tulajdonosok hello eszköz törlése hello katalógusból.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-119">When a data asset in Data Catalog is owned, only users who are authorized by hello owners can discover hello asset and view its metadata, and only hello owners can delete hello asset from hello catalog.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0ac-120">A Data Catalog tulajdonjoga csak hello-katalógusban tárolt hello metaadatok hatással van.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-120">Ownership in Data Catalog affects only hello metadata that's stored in hello catalog.</span></span> <span data-ttu-id="cd0ac-121">Tulajdonjog nem rendelkezik engedéllyel a hello alapul szolgáló adatforrásban.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-121">Ownership does not confer any permissions on hello underlying data source.</span></span>
>
>

### <a name="take-ownership"></a><span data-ttu-id="cd0ac-122">Saját tulajdonba</span><span class="sxs-lookup"><span data-stu-id="cd0ac-122">Take ownership</span></span>
<span data-ttu-id="cd0ac-123">Felhasználók tulajdonba az adategységek hello kiválasztásával **tulajdonba** hello Data Catalog-portál beállítást.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-123">Users can take ownership of data assets by selecting hello **Take Ownership** option in hello Data Catalog portal.</span></span> <span data-ttu-id="cd0ac-124">Nincsenek speciális engedélyek egy tulajdonos nélküli adategységet szükséges tootake tulajdonjogát.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-124">No special permissions are required tootake ownership of an unowned data asset.</span></span> <span data-ttu-id="cd0ac-125">Bármely felhasználó tulajdonba tulajdonos nélküli adatok eszköz.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-125">Any user can take ownership of an unowned data asset.</span></span>

### <a name="add-owners-and-co-owners"></a><span data-ttu-id="cd0ac-126">Adja hozzá a tulajdonosok és a társtulajdonosok</span><span class="sxs-lookup"><span data-stu-id="cd0ac-126">Add owners and co-owners</span></span>
<span data-ttu-id="cd0ac-127">Ha már van tulajdonosa egy adategységet, más felhasználók nem egyszerűen saját tulajdonba.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-127">If a data asset is already owned, other users cannot simply take ownership.</span></span> <span data-ttu-id="cd0ac-128">Kell őket hozzáadni, társtulajdonosok meglévő tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-128">They must be added as co-owners by an existing owner.</span></span> <span data-ttu-id="cd0ac-129">A tulajdonos szerint társtulajdonosok is felvehet további felhasználók és biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-129">Any owner can add additional users or security groups as co-owners.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0ac-130">Az érték a tulajdonos legalább két egyéni felhasználók számára a bevált gyakorlat toohave az bármelyik birtokolt adategységet.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-130">It is a best practice toohave at least two individuals as owners for any owned data asset.</span></span>
>
>

### <a name="remove-owners"></a><span data-ttu-id="cd0ac-131">Távolítsa el a tulajdonosok</span><span class="sxs-lookup"><span data-stu-id="cd0ac-131">Remove owners</span></span>
<span data-ttu-id="cd0ac-132">Ugyanúgy, mint bármely az eszköz tulajdonosa társtulajdonosok hozzáadására, bármely eszköz tulajdonosa távolíthatja el a társtulajdonos.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-132">Just as any asset owner can add co-owners, any asset owner can remove any co-owner.</span></span>

<span data-ttu-id="cd0ac-133">Egy eszköz tulajdonost, aki számára, vagy saját magát eltávolítja a tulajdonos már nem felügyelheti hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-133">An asset owner who removes him or herself as an owner can no longer manage hello asset.</span></span> <span data-ttu-id="cd0ac-134">Ha a hello az eszköz tulajdonosa számára, vagy saját magát eltávolítja a tulajdonos, és nincsenek egyéb közös tulajdonosok, a hello eszköz át nem rendelkezik tulajdonossal állapot tooan.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-134">If hello asset owner removes him or herself as an owner and there are no other co-owners, hello asset reverts tooan unowned state.</span></span>

## <a name="control-visibility"></a><span data-ttu-id="cd0ac-135">Láthatóságának</span><span class="sxs-lookup"><span data-stu-id="cd0ac-135">Control visibility</span></span>
<span data-ttu-id="cd0ac-136">Adategységet tulajdonosok hello hello adategységek láthatóságának saját szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-136">Data-asset owners can control hello visibility of hello data assets they own.</span></span> <span data-ttu-id="cd0ac-137">hello alapértelmezett toorestrict látható, a Data Catalog képes felderíteni a felhasználók és a nézet hello adategységhez, ahol hello az eszköz tulajdonosa visszaváltható hello láthatósági a **mindenki** túl**tulajdonosok és ezek felhasználók** hello eszköz hello tulajdonságaiban.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-137">toorestrict visibility as hello default, where all Data Catalog users can discover and view hello data asset, hello asset owner can toggle hello visibility setting from **Everyone** too**Owners & These Users** in hello properties for hello asset.</span></span> <span data-ttu-id="cd0ac-138">Tulajdonosok majd adhat hozzá a megadott felhasználók és biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-138">Owners can then add specific users and security groups.</span></span>

> [!NOTE]
> <span data-ttu-id="cd0ac-139">Amikor csak lehetséges, eszköz tulajdonjoga és a láthatóság érdekében engedélyek hozzá kell rendelni a toosecurity csoportok és nem tooindividual felhasználók.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-139">Whenever possible, asset ownership and visibility permissions should be assigned toosecurity groups and not tooindividual users.</span></span>
>
>

## <a name="catalog-administrators"></a><span data-ttu-id="cd0ac-140">Katalógus-rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="cd0ac-140">Catalog administrators</span></span>
<span data-ttu-id="cd0ac-141">Katalógus-rendszergazdák az összes eszköz hello katalógusban társtulajdonosok implicit módon.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-141">Data Catalog administrators are implicitly co-owners of all assets in hello catalog.</span></span> <span data-ttu-id="cd0ac-142">Eszköz tulajdonosait nem távolítható el látható a rendszergazdák és rendszergazdák kezelhetik tulajdonjoga és hello katalógusban az összes adategységek láthatóságát.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-142">Asset owners cannot remove visibility from administrators, and administrators can manage ownership and visibility for all data assets in hello catalog.</span></span>

## <a name="summary"></a><span data-ttu-id="cd0ac-143">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cd0ac-143">Summary</span></span>
<span data-ttu-id="cd0ac-144">lehetővé teszi, hogy minden katalógus felhasználók toocontribute hello Data Catalog közösségi modell toometadata és eszköz felderítése, és felderítik.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-144">hello Data Catalog crowdsourcing model toometadata and data asset discovery allows all catalog users toocontribute and discover.</span></span> <span data-ttu-id="cd0ac-145">a Data Catalog Standard Edition hello tulajdonjoga és felügyeleti toolimit hello látható és meghatározott eszközök használatát készült.</span><span class="sxs-lookup"><span data-stu-id="cd0ac-145">hello Standard Edition of Data Catalog is designed for ownership and management toolimit hello visibility and use of specific data assets.</span></span>
