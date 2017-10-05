---
title: "Az Azure Data Catalog adategységek felügyelete |} Microsoft Docs"
description: "A cikk látható és az Azure Data Catalog regisztrált adategységeket tulajdonjogát vezérlése mutatja be."
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
ms.openlocfilehash: 8b9159b7bc4f7b2dac12d9012c6c903e75a6ac16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a><span data-ttu-id="570e7-103">Az Azure Data Catalog adategységek felügyelete</span><span class="sxs-lookup"><span data-stu-id="570e7-103">Manage data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="570e7-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="570e7-104">Introduction</span></span>
<span data-ttu-id="570e7-105">Az Azure Data Catalog adatforrás felderítés, célja, hogy könnyen felderítése és értelmezni azon adatforrásokat elemzést és döntéseket kell.</span><span class="sxs-lookup"><span data-stu-id="570e7-105">Azure Data Catalog is designed for data-source discovery, so that you can easily discover and understand the data sources you need to perform analysis and make decisions.</span></span> <span data-ttu-id="570e7-106">E felderítés képességeivel a legnagyobb mértékben Ha más felhasználók is felkutatni és értelmezni a elérhető adatforrások legszélesebb skáláját.</span><span class="sxs-lookup"><span data-stu-id="570e7-106">These discovery capabilities make the biggest impact when you and other users can find and understand the broadest range of available data sources.</span></span> <span data-ttu-id="570e7-107">Ezeknek az elemeknek szem előtt, az alapértelmezett Data Catalog a rendszer nem minden regisztrált adatforrások számára látható és senki katalógus által felderíthető.</span><span class="sxs-lookup"><span data-stu-id="570e7-107">With these elements in mind, the default behavior of Data Catalog is for all registered data sources to be visible to and discoverable by all catalog users.</span></span>

<span data-ttu-id="570e7-108">A Data Catalog nem hozzáférést biztosít az adatokat mozgatná.</span><span class="sxs-lookup"><span data-stu-id="570e7-108">Data Catalog does not give you access to the data itself.</span></span> <span data-ttu-id="570e7-109">Az adatforrás tulajdonosi adatelérési vezérli.</span><span class="sxs-lookup"><span data-stu-id="570e7-109">Data access is controlled by the owner of the data source.</span></span> <span data-ttu-id="570e7-110">A Data Catalog adatforrások felderítésére, és a metaadatok a forrásokat a katalógusban regisztrált kapcsolódó megtekintése.</span><span class="sxs-lookup"><span data-stu-id="570e7-110">With Data Catalog, you can discover data sources and view the metadata that's related to the sources that are registered in the catalog.</span></span>

<span data-ttu-id="570e7-111">Előfordulhat, azonban ha adatforrások csak akkor látható, az adott felhasználóknak, vagy adott csoportra.</span><span class="sxs-lookup"><span data-stu-id="570e7-111">There might be situations, however, where data sources should only be visible to specific users, or to members of specific groups.</span></span> <span data-ttu-id="570e7-112">Ilyen esetekben a felhasználók saját tulajdonba vétele belül a katalógusban regisztrált adategységeket és majd a vezérlő látható-e a saját eszközök.</span><span class="sxs-lookup"><span data-stu-id="570e7-112">In such scenarios, users can take ownership of registered data assets within the catalog and then control the visibility of the assets they own.</span></span>

> [!NOTE]
> <span data-ttu-id="570e7-113">A jelen cikkben ismertetett funkció csak a Standard Edition Azure Data Catalog érhető el.</span><span class="sxs-lookup"><span data-stu-id="570e7-113">The functionality described in this article is available only in the Standard Edition of Azure Data Catalog.</span></span> <span data-ttu-id="570e7-114">Az ingyenes kiadás nem biztosít tulajdonjoga és korlátozásáról adategységet láthatósági képességét.</span><span class="sxs-lookup"><span data-stu-id="570e7-114">The Free Edition does not provide capabilities for ownership and restricting data-asset visibility.</span></span>
>
>

## <a name="manage-ownership-of-data-assets"></a><span data-ttu-id="570e7-115">Adategységek tulajdonjogát kezelése</span><span class="sxs-lookup"><span data-stu-id="570e7-115">Manage ownership of data assets</span></span>
<span data-ttu-id="570e7-116">Alapértelmezés szerint a Data Catalog regisztrált adategységeket található, a tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="570e7-116">By default, data assets that are registered in Data Catalog are unowned.</span></span> <span data-ttu-id="570e7-117">A katalógus hozzáféréssel rendelkező felhasználók felderítésére, és ezeknek az eszközöknek a jegyzet.</span><span class="sxs-lookup"><span data-stu-id="570e7-117">Any user with permission to access the catalog can discover and annotate these assets.</span></span> <span data-ttu-id="570e7-118">A felhasználók tulajdonos nélküli adategységek saját tulajdonba és látható-e a saját eszközök majd korlátozza.</span><span class="sxs-lookup"><span data-stu-id="570e7-118">Users can take ownership of unowned data assets and then limit the visibility of the assets they own.</span></span>

<span data-ttu-id="570e7-119">A Data Catalog egy adategységet tulajdonosa, ha csak olyan felhasználók, akik jogosultak a tulajdonosai által az eszköz felderítése, és megtekintheti a metaadatait, és csak a tulajdonosok listája az eszköz törlése a katalógusból.</span><span class="sxs-lookup"><span data-stu-id="570e7-119">When a data asset in Data Catalog is owned, only users who are authorized by the owners can discover the asset and view its metadata, and only the owners can delete the asset from the catalog.</span></span>

> [!NOTE]
> <span data-ttu-id="570e7-120">A Data Catalog tulajdonjoga csak a katalógusban tárolt metaadatok hatással van.</span><span class="sxs-lookup"><span data-stu-id="570e7-120">Ownership in Data Catalog affects only the metadata that's stored in the catalog.</span></span> <span data-ttu-id="570e7-121">Tulajdonjog nem rendelkezik engedéllyel a az alapul szolgáló adatforrásban.</span><span class="sxs-lookup"><span data-stu-id="570e7-121">Ownership does not confer any permissions on the underlying data source.</span></span>
>
>

### <a name="take-ownership"></a><span data-ttu-id="570e7-122">Saját tulajdonba</span><span class="sxs-lookup"><span data-stu-id="570e7-122">Take ownership</span></span>
<span data-ttu-id="570e7-123">Felhasználók tulajdonba az adategységek kiválasztásával a **tulajdonba** lehetőséget a Data Catalog-portál.</span><span class="sxs-lookup"><span data-stu-id="570e7-123">Users can take ownership of data assets by selecting the **Take Ownership** option in the Data Catalog portal.</span></span> <span data-ttu-id="570e7-124">Tulajdonos nélküli adatok eszköz saját tulajdonba nem különleges engedélyekre van szükség.</span><span class="sxs-lookup"><span data-stu-id="570e7-124">No special permissions are required to take ownership of an unowned data asset.</span></span> <span data-ttu-id="570e7-125">Bármely felhasználó tulajdonba tulajdonos nélküli adatok eszköz.</span><span class="sxs-lookup"><span data-stu-id="570e7-125">Any user can take ownership of an unowned data asset.</span></span>

### <a name="add-owners-and-co-owners"></a><span data-ttu-id="570e7-126">Adja hozzá a tulajdonosok és a társtulajdonosok</span><span class="sxs-lookup"><span data-stu-id="570e7-126">Add owners and co-owners</span></span>
<span data-ttu-id="570e7-127">Ha már van tulajdonosa egy adategységet, más felhasználók nem egyszerűen saját tulajdonba.</span><span class="sxs-lookup"><span data-stu-id="570e7-127">If a data asset is already owned, other users cannot simply take ownership.</span></span> <span data-ttu-id="570e7-128">Kell őket hozzáadni, társtulajdonosok meglévő tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="570e7-128">They must be added as co-owners by an existing owner.</span></span> <span data-ttu-id="570e7-129">A tulajdonos szerint társtulajdonosok is felvehet további felhasználók és biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="570e7-129">Any owner can add additional users or security groups as co-owners.</span></span>

> [!NOTE]
> <span data-ttu-id="570e7-130">Egy ajánlott legalább két egyéni felhasználók számára, mint bármely tulajdonában lévő adatok eszköz tulajdonosait.</span><span class="sxs-lookup"><span data-stu-id="570e7-130">It is a best practice to have at least two individuals as owners for any owned data asset.</span></span>
>
>

### <a name="remove-owners"></a><span data-ttu-id="570e7-131">Távolítsa el a tulajdonosok</span><span class="sxs-lookup"><span data-stu-id="570e7-131">Remove owners</span></span>
<span data-ttu-id="570e7-132">Ugyanúgy, mint bármely az eszköz tulajdonosa társtulajdonosok hozzáadására, bármely eszköz tulajdonosa távolíthatja el a társtulajdonos.</span><span class="sxs-lookup"><span data-stu-id="570e7-132">Just as any asset owner can add co-owners, any asset owner can remove any co-owner.</span></span>

<span data-ttu-id="570e7-133">Egy eszköz tulajdonost, aki számára, vagy saját magát eltávolítja a tulajdonos már nem felügyelheti az eszközt.</span><span class="sxs-lookup"><span data-stu-id="570e7-133">An asset owner who removes him or herself as an owner can no longer manage the asset.</span></span> <span data-ttu-id="570e7-134">Ha az eszköz tulajdonosa számára, vagy saját magát eltávolítja a tulajdonos, és nincs más társtulajdonosok vannak, az eszköz tulajdonos nélküli állapotba áll vissza.</span><span class="sxs-lookup"><span data-stu-id="570e7-134">If the asset owner removes him or herself as an owner and there are no other co-owners, the asset reverts to an unowned state.</span></span>

## <a name="control-visibility"></a><span data-ttu-id="570e7-135">Láthatóságának</span><span class="sxs-lookup"><span data-stu-id="570e7-135">Control visibility</span></span>
<span data-ttu-id="570e7-136">Adategységet tulajdonosok szabályozhatja a saját adategységek láthatóságát.</span><span class="sxs-lookup"><span data-stu-id="570e7-136">Data-asset owners can control the visibility of the data assets they own.</span></span> <span data-ttu-id="570e7-137">Korlátozható a láthatóságuk az alapértelmezett, ahol az összes Data Catalog felhasználó felderítése és megtekintése a adategységet, hogy az eszköz tulajdonosa az a láthatósági visszaváltható **mindenki** való **tulajdonosok és ezek felhasználók** a az objektum tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="570e7-137">To restrict visibility as the default, where all Data Catalog users can discover and view the data asset, the asset owner can toggle the visibility setting from **Everyone** to **Owners & These Users** in the properties for the asset.</span></span> <span data-ttu-id="570e7-138">Tulajdonosok majd adhat hozzá a megadott felhasználók és biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="570e7-138">Owners can then add specific users and security groups.</span></span>

> [!NOTE]
> <span data-ttu-id="570e7-139">Amikor csak lehetséges – eszköz tulajdonjoga és a láthatóság érdekében engedélyek hozzá kell rendelni biztonsági csoportokra, és nem az egyes felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="570e7-139">Whenever possible, asset ownership and visibility permissions should be assigned to security groups and not to individual users.</span></span>
>
>

## <a name="catalog-administrators"></a><span data-ttu-id="570e7-140">Katalógus-rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="570e7-140">Catalog administrators</span></span>
<span data-ttu-id="570e7-141">Katalógus-rendszergazdák implicit módon társtulajdonosok az összes eszköz a katalógusban.</span><span class="sxs-lookup"><span data-stu-id="570e7-141">Data Catalog administrators are implicitly co-owners of all assets in the catalog.</span></span> <span data-ttu-id="570e7-142">Eszköz tulajdonosait nem távolítható el látható a rendszergazdák és rendszergazdák kezelhetik a tulajdonosi és a katalógusban az összes adategységek láthatóságát.</span><span class="sxs-lookup"><span data-stu-id="570e7-142">Asset owners cannot remove visibility from administrators, and administrators can manage ownership and visibility for all data assets in the catalog.</span></span>

## <a name="summary"></a><span data-ttu-id="570e7-143">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="570e7-143">Summary</span></span>
<span data-ttu-id="570e7-144">A Data Catalog közösségi modell metaadatokat és eszköz felderítése lehetővé teszi, hogy minden katalógus felhasználói közreműködés és Fedezze fel.</span><span class="sxs-lookup"><span data-stu-id="570e7-144">The Data Catalog crowdsourcing model to metadata and data asset discovery allows all catalog users to contribute and discover.</span></span> <span data-ttu-id="570e7-145">A Data Catalog Standard Edition tulajdonjoga és készült felügyeleti korlátozni a láthatóság és a meghatározott eszközök használatát.</span><span class="sxs-lookup"><span data-stu-id="570e7-145">The Standard Edition of Data Catalog is designed for ownership and management to limit the visibility and use of specific data assets.</span></span>
