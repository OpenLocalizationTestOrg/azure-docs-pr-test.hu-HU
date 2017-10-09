---
title: "Azure CDN-tartalom ország aaaRestrict |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestrict hozzáférés tooyour Azure CDN tartalom használatával hello szolgáltatás Geo-szűrés."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: ffdd994612b6c9cfbf1a6e29d260709b4afa86e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a><span data-ttu-id="44c82-103">Ország Azure CDN-tartalom korlátozása</span><span class="sxs-lookup"><span data-stu-id="44c82-103">Restrict Azure CDN content by country</span></span>

## <a name="overview"></a><span data-ttu-id="44c82-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="44c82-104">Overview</span></span>
<span data-ttu-id="44c82-105">Ha egy felhasználó alapértelmezés szerint a tartalmat igényel, függetlenül attól, hol hello felhasználói kérelmet ezt a kiszolgált hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="44c82-105">When a user requests your content, by default, hello content is served regardless of where hello user made this request from.</span></span> <span data-ttu-id="44c82-106">Bizonyos esetekben érdemes lehet toorestrict tooyour tartalom elérése ország.</span><span class="sxs-lookup"><span data-stu-id="44c82-106">In some cases, you may want toorestrict access tooyour content by country.</span></span> <span data-ttu-id="44c82-107">Ez a témakör azt ismerteti, hogyan toouse hello **földrajzi-szűrés** rendelés tooconfigure hello szolgáltatás tooallow blokkolja a hozzáférést ország szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="44c82-107">This topic explains how toouse hello **Geo-Filtering** feature in order tooconfigure hello service tooallow or block access by country.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44c82-108">hello Verizon és Akamai kínálnak hello azonos földrajzi-szűrés funkciót, de egy kis értékkülönbségeket te országhívó számokat támogatják-e.</span><span class="sxs-lookup"><span data-stu-id="44c82-108">hello Verizon and Akamai products provide hello same geo-filtering functionality but have a small difference in te country codes they support.</span></span> <span data-ttu-id="44c82-109">Tekintse meg a 3. lépés a hivatkozás toohello különbségek miatt.</span><span class="sxs-lookup"><span data-stu-id="44c82-109">See Step 3 for a link toohello differences.</span></span>


<span data-ttu-id="44c82-110">Tooconfiguring megfontolást információ az ilyen típusú korlátozás: hello [szempontok](cdn-restrict-access-by-country.md#considerations) szakasz hello hello témakör végén.</span><span class="sxs-lookup"><span data-stu-id="44c82-110">For information about considerations that apply tooconfiguring this type of restriction, see hello [Considerations](cdn-restrict-access-by-country.md#considerations) section at hello end of hello topic.</span></span>  

![Ország szerinti szűrés](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-hello-directory-path"></a><span data-ttu-id="44c82-112">1. lépés: Hello könyvtár elérési útjának megadása</span><span class="sxs-lookup"><span data-stu-id="44c82-112">Step 1: Define hello directory path</span></span>
<span data-ttu-id="44c82-113">Válassza ki a végpont hello portálon, és hello hello bal oldali navigációs toofind földrajzi-szűrés lapján található a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="44c82-113">Select your endpoint within hello portal, and find hello Geo-Filtering tab on hello left-hand navigation toofind this feature.</span></span>

<span data-ttu-id="44c82-114">Ország szűrő konfigurálásakor meg kell adnia a hello relatív elérési út toohello hely toowhich felhasználók engedélyez vagy megtagadta a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="44c82-114">When configuring a country filter, you must specify hello relative path toohello location toowhich users will be allowed or denied access.</span></span> <span data-ttu-id="44c82-115">Az összes fájl földrajzi-szűrés alkalmazhatja "/" vagy a kijelölt mappák elérési útjaiban "/ képek /" megadásával.</span><span class="sxs-lookup"><span data-stu-id="44c82-115">You can apply geo-filtering for all your files with "/" or selected folders by specifying directory paths "/pictures/".</span></span> <span data-ttu-id="44c82-116">Is alkalmazhat földrajzi-szűrés tooa egyfájlos hello fájl megadásával, és a záró perjelet kihagyva hello "/ képek/város.png".</span><span class="sxs-lookup"><span data-stu-id="44c82-116">You can also apply geo-filtering tooa single file by specifying hello file, and leaving out hello trailing slash "/pictures/city.png".</span></span>

<span data-ttu-id="44c82-117">Példa könyvtár elérési útja szűrő:</span><span class="sxs-lookup"><span data-stu-id="44c82-117">Example directory path filter:</span></span>

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-hello-action-block-or-allow"></a><span data-ttu-id="44c82-118">2. lépés: Hello művelet meghatározása: letiltása vagy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="44c82-118">Step 2: Define hello action: block or allow</span></span>
<span data-ttu-id="44c82-119">**Blokkolás:** hello felhasználók meghatározott országok elutasításra kerülne hozzáférés tooassets lekéri a rekurzív elérési út.</span><span class="sxs-lookup"><span data-stu-id="44c82-119">**Block:** Users from hello specified countries will be denied access tooassets requested from that recursive path.</span></span> <span data-ttu-id="44c82-120">Ha nincs más országban szűrési beállítások vannak konfigurálva, hogy a helyen, majd a többi felhasználó hozzáférése engedélyezett lesz.</span><span class="sxs-lookup"><span data-stu-id="44c82-120">If no other country filtering options have been configured for that location, then all other users will be allowed access.</span></span>

<span data-ttu-id="44c82-121">**Engedélyezése:** hello felhasználók csak meghatározott országok engedélyezett hozzáférési tooassets lekéri a rekurzív elérési út.</span><span class="sxs-lookup"><span data-stu-id="44c82-121">**Allow:** Only users from hello specified countries will be allowed access tooassets requested from that recursive path.</span></span>

## <a name="step-3-define-hello-countries"></a><span data-ttu-id="44c82-122">3. lépés: Hello országok meghatározása</span><span class="sxs-lookup"><span data-stu-id="44c82-122">Step 3: Define hello countries</span></span>
<span data-ttu-id="44c82-123">Válassza ki a hello országok tooblock szeretné, vagy lehetővé teszik a hello elérési útja.</span><span class="sxs-lookup"><span data-stu-id="44c82-123">Select hello countries that you want tooblock or allow for hello path.</span></span> 

<span data-ttu-id="44c82-124">Például az blokkolás /Photos/Strasbourgban/hello szabály beleértve fájlok szűrheti:</span><span class="sxs-lookup"><span data-stu-id="44c82-124">For example, hello rule of blocking /Photos/Strasbourg/ will filter files including:</span></span>

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a><span data-ttu-id="44c82-125">Országkód</span><span class="sxs-lookup"><span data-stu-id="44c82-125">Country codes</span></span>
<span data-ttu-id="44c82-126">Hello **földrajzi-szűrés** szolgáltatás használja, amelyből kérelmet engedélyezett vagy letiltott ország kódok toodefine hello országok biztonságos könyvtár.</span><span class="sxs-lookup"><span data-stu-id="44c82-126">hello **Geo-Filtering** feature uses country codes toodefine hello countries from which a request will be allowed or blocked for a secured directory.</span></span> <span data-ttu-id="44c82-127">Hello országhívó számokat a rendszer megtalálta [Azure CDN országhívószámok](https://msdn.microsoft.com/library/mt761717.aspx).</span><span class="sxs-lookup"><span data-stu-id="44c82-127">You will find hello country codes in [Azure CDN  Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).</span></span> 

## <span data-ttu-id="44c82-128"><a id="considerations"></a>Szempontok</span><span class="sxs-lookup"><span data-stu-id="44c82-128"><a id="considerations"></a>Considerations</span></span>
* <span data-ttu-id="44c82-129">Verizon too90 percig vagy módosításokat tooyour ország szűrési konfigurációs tootake hatás Akamai, néhány perc igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="44c82-129">It may take up too90 minutes for Verizon, or a couple minutes with Akamai, for changes tooyour country filtering configuration tootake effect.</span></span>
* <span data-ttu-id="44c82-130">Ez a funkció nem támogatja a helyettesítő karakterek (például "*").</span><span class="sxs-lookup"><span data-stu-id="44c82-130">This feature does not support wildcard characters (for example, ‘*’).</span></span>
* <span data-ttu-id="44c82-131">hello földrajzi-szűrés konfiguráció hello relatív elérési út társított lesz rekurzív módon alkalmazza toothat elérési útja.</span><span class="sxs-lookup"><span data-stu-id="44c82-131">hello geo-filtering configuration associated with hello relative path will be applied recursively toothat path.</span></span>
* <span data-ttu-id="44c82-132">Csak egy szabályt alkalmazott toohello lehetnek azonos relatív elérési út (nem hozható létre több országban szűrő adott pont toohello azonos relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="44c82-132">Only one rule can be applied toohello same relative path (you cannot create multiple country filters that point toohello same relative path.</span></span> <span data-ttu-id="44c82-133">Azonban a mappa több országban szűrő állhat.</span><span class="sxs-lookup"><span data-stu-id="44c82-133">However, a folder may have multiple country filters.</span></span> <span data-ttu-id="44c82-134">Ennek az az ország szűrők toohello rekurzív jellege miatt.</span><span class="sxs-lookup"><span data-stu-id="44c82-134">This is due toohello recursive nature of country filters.</span></span> <span data-ttu-id="44c82-135">Más szóval a korábban konfigurált mappa almappája más országban szűrő rendelhetők.</span><span class="sxs-lookup"><span data-stu-id="44c82-135">In other words, a subfolder of a previously configured folder can be assigned a different country filter.</span></span>

