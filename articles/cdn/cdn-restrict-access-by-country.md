---
title: "Azure CDN-tartalom ország korlátozása |} Microsoft Docs"
description: "Útmutató az Azure CDN-tartalom a földrajzi-szűrés szolgáltatás használatával korlátozza a hozzáférést."
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
ms.openlocfilehash: 30160088d9c770400f342e67527e1cf1cabc4f6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a><span data-ttu-id="53937-103">Ország Azure CDN-tartalom korlátozása</span><span class="sxs-lookup"><span data-stu-id="53937-103">Restrict Azure CDN content by country</span></span>

## <a name="overview"></a><span data-ttu-id="53937-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="53937-104">Overview</span></span>
<span data-ttu-id="53937-105">Amikor egy felhasználó alapértelmezés szerint a tartalmat igényel, függetlenül attól, ahol a felhasználó kérelmet ezt a kiszolgált a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="53937-105">When a user requests your content, by default, the content is served regardless of where the user made this request from.</span></span> <span data-ttu-id="53937-106">Bizonyos esetekben érdemes lehet a tartalmat az ország való hozzáférésének korlátozásához.</span><span class="sxs-lookup"><span data-stu-id="53937-106">In some cases, you may want to restrict access to your content by country.</span></span> <span data-ttu-id="53937-107">Ez a témakör ismerteti, hogyan a **földrajzi-szűrés** szolgáltatás a szolgáltatás engedélyezi vagy letiltja a hozzáférést, ország konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="53937-107">This topic explains how to use the **Geo-Filtering** feature in order to configure the service to allow or block access by country.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53937-108">Verizon és Akamai termékek funkcionalitása azonos földrajzi-szűrés, de egy kis értékkülönbségeket te országhívó számokat támogatják-e.</span><span class="sxs-lookup"><span data-stu-id="53937-108">The Verizon and Akamai products provide the same geo-filtering functionality but have a small difference in te country codes they support.</span></span> <span data-ttu-id="53937-109">Tekintse meg a 3. lépés a különbségek mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="53937-109">See Step 3 for a link to the differences.</span></span>


<span data-ttu-id="53937-110">Az ilyen korlátozásokat konfigurálására vonatkozó megfontolások kapcsolatos információkért lásd: a [szempontok](cdn-restrict-access-by-country.md#considerations) szakasz a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="53937-110">For information about considerations that apply to configuring this type of restriction, see the [Considerations](cdn-restrict-access-by-country.md#considerations) section at the end of the topic.</span></span>  

![Ország szerinti szűrés](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a><span data-ttu-id="53937-112">1. lépés: A könyvtár elérési útjának megadása</span><span class="sxs-lookup"><span data-stu-id="53937-112">Step 1: Define the directory path</span></span>
<span data-ttu-id="53937-113">Válassza ki a végponthoz, a portálon, és ez a szolgáltatás található a bal oldali navigációs a földrajzi-szűrés lapon található.</span><span class="sxs-lookup"><span data-stu-id="53937-113">Select your endpoint within the portal, and find the Geo-Filtering tab on the left-hand navigation to find this feature.</span></span>

<span data-ttu-id="53937-114">Ország szűrő konfigurálásakor meg kell adnia a relatív elérési útját, amelyhez felhasználók engedélyez vagy megtagadta a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="53937-114">When configuring a country filter, you must specify the relative path to the location to which users will be allowed or denied access.</span></span> <span data-ttu-id="53937-115">Az összes fájl földrajzi-szűrés alkalmazhatja "/" vagy a kijelölt mappák elérési útjaiban "/ képek /" megadásával.</span><span class="sxs-lookup"><span data-stu-id="53937-115">You can apply geo-filtering for all your files with "/" or selected folders by specifying directory paths "/pictures/".</span></span> <span data-ttu-id="53937-116">Is alkalmazhat földrajzi-szűrés egyetlen fájl megadása a fájl, és a záró perjelet kihagyva "/ képek/város.png".</span><span class="sxs-lookup"><span data-stu-id="53937-116">You can also apply geo-filtering to a single file by specifying the file, and leaving out the trailing slash "/pictures/city.png".</span></span>

<span data-ttu-id="53937-117">Példa könyvtár elérési útja szűrő:</span><span class="sxs-lookup"><span data-stu-id="53937-117">Example directory path filter:</span></span>

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a><span data-ttu-id="53937-118">2. lépés: Adja meg a műveletet: letiltása vagy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="53937-118">Step 2: Define the action: block or allow</span></span>
<span data-ttu-id="53937-119">**Blokkolás:** megadott országokból származó felhasználók hozzáférését a rendszer megtagadja lekéri a rekurzív út eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="53937-119">**Block:** Users from the specified countries will be denied access to assets requested from that recursive path.</span></span> <span data-ttu-id="53937-120">Ha nincs más országban szűrési beállítások vannak konfigurálva, hogy a helyen, majd a többi felhasználó hozzáférése engedélyezett lesz.</span><span class="sxs-lookup"><span data-stu-id="53937-120">If no other country filtering options have been configured for that location, then all other users will be allowed access.</span></span>

<span data-ttu-id="53937-121">**Engedélyezése:** csak a megadott országokból származó felhasználók kapnak hozzáférést, lekéri a rekurzív út eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="53937-121">**Allow:** Only users from the specified countries will be allowed access to assets requested from that recursive path.</span></span>

## <a name="step-3-define-the-countries"></a><span data-ttu-id="53937-122">3. lépés: A országok meghatározása</span><span class="sxs-lookup"><span data-stu-id="53937-122">Step 3: Define the countries</span></span>
<span data-ttu-id="53937-123">Válassza ki az elérési utat engedélyezése vagy blokkolása kívánt országok.</span><span class="sxs-lookup"><span data-stu-id="53937-123">Select the countries that you want to block or allow for the path.</span></span> 

<span data-ttu-id="53937-124">A szabály az blokkolás /Photos/Strasbourgban/például fájlok például szűrheti:</span><span class="sxs-lookup"><span data-stu-id="53937-124">For example, the rule of blocking /Photos/Strasbourg/ will filter files including:</span></span>

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a><span data-ttu-id="53937-125">Országkód</span><span class="sxs-lookup"><span data-stu-id="53937-125">Country codes</span></span>
<span data-ttu-id="53937-126">A **földrajzi-szűrés** szolgáltatás országhívó számokat használ, amelyből a kérelem fogja engedélyezett vagy letiltott biztonságos könyvtár országok meghatározására.</span><span class="sxs-lookup"><span data-stu-id="53937-126">The **Geo-Filtering** feature uses country codes to define the countries from which a request will be allowed or blocked for a secured directory.</span></span> <span data-ttu-id="53937-127">Megtalálja az ország kódok [Azure CDN országhívószámok](https://msdn.microsoft.com/library/mt761717.aspx).</span><span class="sxs-lookup"><span data-stu-id="53937-127">You will find the country codes in [Azure CDN  Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).</span></span> 

## <span data-ttu-id="53937-128"><a id="considerations"></a>Szempontok</span><span class="sxs-lookup"><span data-stu-id="53937-128"><a id="considerations"></a>Considerations</span></span>
* <span data-ttu-id="53937-129">Verizon vagy néhány percig Akamai, a szűrés konfigurálásának életbelépéséhez országa 90 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="53937-129">It may take up to 90 minutes for Verizon, or a couple minutes with Akamai, for changes to your country filtering configuration to take effect.</span></span>
* <span data-ttu-id="53937-130">Ez a funkció nem támogatja a helyettesítő karakterek (például "*").</span><span class="sxs-lookup"><span data-stu-id="53937-130">This feature does not support wildcard characters (for example, ‘*’).</span></span>
* <span data-ttu-id="53937-131">A földrajzi-szűrés konfigurációs társított a következő relatív elérési út rekurzív módon alkalmazza lesz.</span><span class="sxs-lookup"><span data-stu-id="53937-131">The geo-filtering configuration associated with the relative path will be applied recursively to that path.</span></span>
* <span data-ttu-id="53937-132">Csak egy szabály alkalmazhatja a azonos relatív elérési (nem hozható létre több ország szűrőket, amelyek ugyanazt az relatív elérési utat.</span><span class="sxs-lookup"><span data-stu-id="53937-132">Only one rule can be applied to the same relative path (you cannot create multiple country filters that point to the same relative path.</span></span> <span data-ttu-id="53937-133">Azonban a mappa több országban szűrő állhat.</span><span class="sxs-lookup"><span data-stu-id="53937-133">However, a folder may have multiple country filters.</span></span> <span data-ttu-id="53937-134">Ez az ország szűrők rekurzív jellemzői miatt.</span><span class="sxs-lookup"><span data-stu-id="53937-134">This is due to the recursive nature of country filters.</span></span> <span data-ttu-id="53937-135">Más szóval a korábban konfigurált mappa almappája más országban szűrő rendelhetők.</span><span class="sxs-lookup"><span data-stu-id="53937-135">In other words, a subfolder of a previously configured folder can be assigned a different country filter.</span></span>

