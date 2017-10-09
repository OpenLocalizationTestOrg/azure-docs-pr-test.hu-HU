---
title: "aaaHow toosecure hozzáférés tooAzure Data Catalog |} Microsoft Docs"
description: "Ez a cikk azt ismertetik, hogyan toosecure a data catalog és az adategységeket."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: d7c35fd57d8add1cdc152b75f111879288e1548f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-access-toodata-catalog-and-data-assets"></a><span data-ttu-id="62a9c-103">Hogyan érik el toosecure toodata katalógust és az adatok eszközök</span><span class="sxs-lookup"><span data-stu-id="62a9c-103">How toosecure access toodata catalog and data assets</span></span>
> [!IMPORTANT]
> <span data-ttu-id="62a9c-104">Ez a funkció csak a hello az Azure Data Catalog standard kiadásában érhető el.</span><span class="sxs-lookup"><span data-stu-id="62a9c-104">This feature is available only in hello standard edition of Azure Data Catalog.</span></span>

<span data-ttu-id="62a9c-105">Az Azure Data Catalog lehetővé teszi a toospecify, ki férhet hozzá hello a data catalog, és milyen műveletek (regisztrálásához, megjegyzésekkel, vegye saját tulajdonba) a metaadatok hello katalógusban ellátására.</span><span class="sxs-lookup"><span data-stu-id="62a9c-105">Azure Data Catalog allows you toospecify who can access hello data catalog and what operations (register, annotate, take ownership) they can perform on metadata in hello catalog.</span></span> 

## <a name="catalog-users-and-permissions"></a><span data-ttu-id="62a9c-106">Katalógus-felhasználók és engedélyek</span><span class="sxs-lookup"><span data-stu-id="62a9c-106">Catalog users and permissions</span></span>
<span data-ttu-id="62a9c-107">toogive egy felhasználó vagy egy csoport hello tooa a data catalog eléréséhez, és az engedélyek beállítása:</span><span class="sxs-lookup"><span data-stu-id="62a9c-107">toogive a user or a group hello access tooa data catalog and set permissions:</span></span>

1. <span data-ttu-id="62a9c-108">A hello [a data CATALOG kezdőlap](http://www.azuredatacatalog.com), kattintson a **beállítások** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="62a9c-108">On hello [home page of your data catalog](http://www.azuredatacatalog.com),  click **Settings** on hello toolbar.</span></span>

    ![a Data catalog - beállítások](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. <span data-ttu-id="62a9c-110">Hello-beállítások lapján bontsa ki a hello **katalógus felhasználói** szakasz.</span><span class="sxs-lookup"><span data-stu-id="62a9c-110">In hello settings page, expand hello **Catalog Users** section.</span></span>
    <span data-ttu-id="62a9c-111">![Katalógus - felhasználók hozzáadása](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span><span class="sxs-lookup"><span data-stu-id="62a9c-111">![Catalog Users - Add](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)</span></span>
3. <span data-ttu-id="62a9c-112">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="62a9c-112">Click **Add**.</span></span>
4. <span data-ttu-id="62a9c-113">Adja meg a teljesen minősített hello **felhasználónév** vagy hello neve **biztonsági csoport** az Azure Active Directory (AAD) társított hello katalógus hello.</span><span class="sxs-lookup"><span data-stu-id="62a9c-113">Enter hello fully qualified **user name** or name of hello **security group** in hello Azure Active Directory (AAD) associated with hello catalog.</span></span> <span data-ttu-id="62a9c-114">Használjon vesszőt (', '), egy elválasztó hozzáadásakor több mint egy felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="62a9c-114">Use comma (\`,’) as a separator if you are adding more than one user or group.</span></span>
    <span data-ttu-id="62a9c-115">![Katalógus felhasználói - felhasználók és csoportok](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span><span class="sxs-lookup"><span data-stu-id="62a9c-115">![Catalog Users - users or groups](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)</span></span>
5. <span data-ttu-id="62a9c-116">Nyomja le az **ENTER** vagy **lapon** kívül hello szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="62a9c-116">Press **ENTER** or **TAB** out of hello text box.</span></span> 
6.  <span data-ttu-id="62a9c-117">Ellenőrizze, hogy minden engedély (**jegyzet**, **regisztrálása**, és **tulajdonba**) toothese felhasználók vagy csoportok alapértelmezés szerint tartoznak.</span><span class="sxs-lookup"><span data-stu-id="62a9c-117">Confirm that all permissions (**Annotate**, **Register**, and **Take Ownership**) are assigned toothese users or groups by default.</span></span> <span data-ttu-id="62a9c-118">Ez azt jelenti, hogy hello felhasználó vagy csoport is [adategységek regisztrálása]( data-catalog-how-to-register.md), [adhat megjegyzéseket az adategységekhez]( data-catalog-how-to-annotate.md), és [az adategységek saját tulajdonba]( data-catalog-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="62a9c-118">That is, hello user or group can [register data assets]( data-catalog-how-to-register.md), [annotate data assets]( data-catalog-how-to-annotate.md), and [take ownership of data assets]( data-catalog-how-to-manage.md).</span></span> 
    <span data-ttu-id="62a9c-119">![Katalógus felhasználók – az alapértelmezett engedélyek](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span><span class="sxs-lookup"><span data-stu-id="62a9c-119">![Catalog Users - default permissions](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)</span></span>
7.  <span data-ttu-id="62a9c-120">egy felhasználó vagy csoport toogive csak hello olvasási hozzáférés toohello katalógus, törölje a jelet hello **megjegyzésekkel** lehetőség az adott felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="62a9c-120">toogive a user or group only hello read access toohello catalog, clear hello **annotate** option for that user or group.</span></span> <span data-ttu-id="62a9c-121">Ha így tesz, hello felhasználó vagy csoport nem adhat megjegyzéseket az adategységekhez hello katalógusban, de is megtekinthetik.</span><span class="sxs-lookup"><span data-stu-id="62a9c-121">When you do so, hello user or group cannot annotate data assets in hello catalog but they can view them.</span></span> 
8.  <span data-ttu-id="62a9c-122">toodeny egy felhasználó vagy csoport regisztrálja az adategységek, törölje a jelet hello **regisztrálása** lehetőség az adott felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="62a9c-122">toodeny a user or group from registering data assets, clear hello **register** option for that user or group.</span></span>
9.  <span data-ttu-id="62a9c-123">a felhasználó egy adategységet, törölje a jelet hello tulajdonba toodeny **saját tulajdonba** lehetőség az adott felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="62a9c-123">toodeny a user from taking ownership of a data asset, clear hello **take ownership** option for that user or group.</span></span> 
10. <span data-ttu-id="62a9c-124">a katalógus felhasználók számára, egy felhasználó/csoport toodelete kattintson **x** hello felhasználó vagy csoport hello lista hello alján.</span><span class="sxs-lookup"><span data-stu-id="62a9c-124">toodelete a user/group from catalog users, click **x** for hello user/group at hello bottom of hello list.</span></span> 
    <span data-ttu-id="62a9c-125">![Katalógus-felhasználók - felhasználó törlése](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="62a9c-125">![Catalog Users - delete user](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="62a9c-126">Azt javasoljuk, hogy közvetlenül hozzáadni a felhasználók hozzáadásával helyett a biztonsági csoportok toocatalog felhasználók és engedélyek hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="62a9c-126">We recommend that you add security groups toocatalog users rather than adding users directly and assign permissions.</span></span> <span data-ttu-id="62a9c-127">Adja hozzá a felhasználók toohello biztonsági csoportok, amelyek megfelelnek a szerepkörök és a szükséges hozzáférési toohello katalógus.</span><span class="sxs-lookup"><span data-stu-id="62a9c-127">Then, add users toohello security groups that match their roles and their required access toohello catalog.</span></span>

## <a name="special-considerations"></a><span data-ttu-id="62a9c-128">Különleges szempontok</span><span class="sxs-lookup"><span data-stu-id="62a9c-128">Special considerations</span></span>

- <span data-ttu-id="62a9c-129">hello jogosultságokkal toosecurity csoportok additívak.</span><span class="sxs-lookup"><span data-stu-id="62a9c-129">hello permissions assigned toosecurity groups are additive.</span></span> <span data-ttu-id="62a9c-130">Tegyük fel a felhasználó két szerepel.</span><span class="sxs-lookup"><span data-stu-id="62a9c-130">Say, a user is in two groups.</span></span> <span data-ttu-id="62a9c-131">Egy csoport rendelkezik megjegyzésekkel engedélyek és egyéb csoport nem rendelkezik megjegyzésekkel engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="62a9c-131">One group has annotate permissions and other group does not have annotate permissions.</span></span> <span data-ttu-id="62a9c-132">Ezt követően felhasználó engedélyekkel rendelkezik megjegyzésekkel.</span><span class="sxs-lookup"><span data-stu-id="62a9c-132">Then, user has annotate permissions.</span></span> 
- <span data-ttu-id="62a9c-133">hello engedélyeket explicit módon tooa felhasználót felülbírálás hello jogosultságait toogroups toowhich hello felhasználó tartozik.</span><span class="sxs-lookup"><span data-stu-id="62a9c-133">hello permissions assigned explicitly tooa user override hello permissions assigned toogroups toowhich hello user belongs.</span></span> <span data-ttu-id="62a9c-134">Hello előző példában tegyük fel például, kifejezetten hello felhasználó toocatalog felhasználók, és ne rendelje hozzá a jegyzet engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="62a9c-134">In hello previous example, say, you explicitly added hello user toocatalog users and do not assign annotate permissions.</span></span> <span data-ttu-id="62a9c-135">hello felhasználói nem adhat megjegyzéseket az adategységekhez, annak ellenére, hogy hello felhasználó tagja egy csoportot, amely rendelkezik engedélyekkel megjegyzésekkel.</span><span class="sxs-lookup"><span data-stu-id="62a9c-135">hello user cannot annotate data assets even though hello user is a member of a group that does have annotate permissions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62a9c-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62a9c-136">Next steps</span></span>
- [<span data-ttu-id="62a9c-137">Ismerkedés az Azure Data Catalog szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="62a9c-137">Get started with Azure Data Catalog</span></span>](data-catalog-get-started.md)

