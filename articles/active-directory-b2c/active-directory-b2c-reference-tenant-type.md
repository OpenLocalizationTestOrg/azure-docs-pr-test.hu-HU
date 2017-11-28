---
title: "Az Azure Active Directory B2C: Régió rendelkezésre állási & adatok rezidens |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C-bérlők hello típusai"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: d382921fe9d62183b6d52c47d62f952dabd116c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a><span data-ttu-id="7b25a-103">Az Azure Active Directory B2C: Régió rendelkezésre állási & adatok rezidens</span><span class="sxs-lookup"><span data-stu-id="7b25a-103">Azure Active Directory B2C: Region availability & data residency</span></span>
<span data-ttu-id="7b25a-104">Régiónkénti elérhetőség és adatok rezidens két nagyon különböző fogalom alkalmazó másképp tooAzure AD B2C el hello többi Azure.</span><span class="sxs-lookup"><span data-stu-id="7b25a-104">Region availability and data residency are two very different concepts that apply differently tooAzure AD B2C from hello rest of Azure.</span></span> <span data-ttu-id="7b25a-105">Ez a cikk fogja a két módszer hello különbségei ismertetik, és hasonlítsa össze, hogyan alkalmazhatók az Azure AD B2C és tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7b25a-105">This article will explain hello differences between these two concepts and compare how they apply tooAzure versus Azure AD B2C.</span></span>

## <a name="summary"></a><span data-ttu-id="7b25a-106">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="7b25a-106">Summary</span></span>
<span data-ttu-id="7b25a-107">Az Azure AD B2C jelenleg **általánosan elérhető világszerte** hello beállítással **adatok rezidens Amerikai Egyesült Államokban vagy Európa**.</span><span class="sxs-lookup"><span data-stu-id="7b25a-107">Azure AD B2C is **generally available worldwide** with hello option for **data residency in United States or Europe**.</span></span>

## <a name="concepts"></a><span data-ttu-id="7b25a-108">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="7b25a-108">Concepts</span></span>
* <span data-ttu-id="7b25a-109">**Régiónkénti elérhetőség** toowhere hivatkozik a "szolgáltatás" használható.</span><span class="sxs-lookup"><span data-stu-id="7b25a-109">**Region availability** refers toowhere a service is available for use.</span></span>
* <span data-ttu-id="7b25a-110">**Adatok rezidens** toowhere felhasználói adat hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="7b25a-110">**Data residency** refers toowhere user data is stored.</span></span>

## <a name="region-availability"></a><span data-ttu-id="7b25a-111">Régiónkénti elérhetőség</span><span class="sxs-lookup"><span data-stu-id="7b25a-111">Region availability</span></span>
<span data-ttu-id="7b25a-112">Az Azure AD B2C érhető világszerte hello Azure nyilvános felhőjében keresztül.</span><span class="sxs-lookup"><span data-stu-id="7b25a-112">Azure AD B2C is available worldwide via hello Azure public cloud.</span></span> 

<span data-ttu-id="7b25a-113">Ez eltér hello modell legtöbb más Azure-szolgáltatások hajtsa végre, amely az adatok rezidens gömbcsatlakozók a rendelkezésre állási.</span><span class="sxs-lookup"><span data-stu-id="7b25a-113">This differs from hello model most other Azure services follow which couple availability with data residency.</span></span> <span data-ttu-id="7b25a-114">Erre példa látható, két Azure-ban [termékek elérhető területek szerint](https://azure.microsoft.com/regions/services/) lap és hello [Active Directory B2C díjszabási Számológép](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span><span class="sxs-lookup"><span data-stu-id="7b25a-114">You can see examples of this in both Azure's [Products Available By Region](https://azure.microsoft.com/regions/services/) page and hello [Active Directory B2C pricing calculator](https://azure.microsoft.com/pricing/details/active-directory-b2c/).</span></span>

## <a name="data-residency"></a><span data-ttu-id="7b25a-115">Adattárolás helye</span><span class="sxs-lookup"><span data-stu-id="7b25a-115">Data residency</span></span>
<span data-ttu-id="7b25a-116">Az Azure AD B2C az Amerikai Egyesült Államokban vagy Európa felhasználói adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="7b25a-116">Azure AD B2C stores user data in either United States or Europe.</span></span>

<span data-ttu-id="7b25a-117">Adatok rezidens határozza meg, melyik ország vagy régió van kiválasztva alapján amikor [létrehozása az Azure AD B2C-bérlő](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7b25a-117">Data residency is determined based on which country/region is selected when [creating an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

![A kép bérlő képernyőfelvétele](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

<span data-ttu-id="7b25a-119">Az adatok találhatók a következő országokban és régiókban hello hello Amerikai Egyesült Államokban:</span><span class="sxs-lookup"><span data-stu-id="7b25a-119">Data resides in hello United States for hello following countries/regions:</span></span>

> <span data-ttu-id="7b25a-120">Az Amerikai Egyesült Államok, Kanada, hondurasi, Dominikai Köztársaság, El Salvador, Guatemala, mexikói, Panama, Puerto Rico és Trinidad és Tobago</span><span class="sxs-lookup"><span data-stu-id="7b25a-120">United States, Canada, Costa Rica, Dominican Republic, El Salvador, Guatemala, Mexico, Panama, Puerto Rico and Trinidad & Tobago</span></span>

<span data-ttu-id="7b25a-121">Az adatok találhatók az európai országokban és régiókban következő hello a:</span><span class="sxs-lookup"><span data-stu-id="7b25a-121">Data resides in Europe for hello following countries/regions:</span></span>

> <span data-ttu-id="7b25a-122">Algéria, Ausztria, Azerbajdzsán, Bahreini Királyság, Fehéroroszországból, Belgium, bolgár, horvát, Ciprus, Cseh Köztársaság, Dánia, Egyiptom, észt, Finnország, Franciaország, Németország, Görögország, Magyarország, Izland, Írországban, izraeli, Olaszország, Jordánia, Kazahsztán, Kenya, kuvaiti, Lativa, Libanon, Liechtenstein, Lituania, Luxembourgban, Macedónia FYRO, Málta, Montenegró, Marokkó, holland, Nigéria, Norvégia, Omán, pakisztáni, Lengyelország, Portugália, Katar, román, orosz, Szaúd-Arábia, szerb, Szlovákia, szlovén, Dél-afrikai, Spanyolország, Svédország, Svájc, Tunézia, Törökország, ukrán, Egyesült Arab emírségekbeli és Egyesült Királyság.</span><span class="sxs-lookup"><span data-stu-id="7b25a-122">Algeria, Austria, Azerbaijan, Bahrain, Belarus, Belgium, Bulgaria, Croatia, Cyprus, Czech Republic, Denmark, Egypt, Estonia, Finland, France, Germany, Greece, Hungary, Iceland, Ireland, Israel, Italy, Jordan, Kazakhstan, Kenya, Kuwait, Lativa, Lebanon, Liechtenstein, Lituania, Luxembourg, Macedonia FYRO, Malta, Montenegro, Morocco, Netherlands, Nigeria, Norway, Oman, Pakistan, Poland, Portugal, Qatar, Romania, Russia, Saudi Arabia, Serbia, Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, Tunisia, Turkey, Ukraine, United Arab Emirates and United Kingdom.</span></span>

<span data-ttu-id="7b25a-123">hello fennmaradó országokban és régiókban hello folyamata toohello lista hozzáadott szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="7b25a-123">hello remaining countries/regions are in hello process of being added toohello list.</span></span>  <span data-ttu-id="7b25a-124">Most, továbbra is használhatja az Azure AD B2C ki bármelyik hello országokban és régiókban fenti.</span><span class="sxs-lookup"><span data-stu-id="7b25a-124">For now, you can still use Azure AD B2C by picking any of hello countries/regions above.</span></span>

> <span data-ttu-id="7b25a-125">Afganisztáni, argentin, Ausztrália, Brazília, chilei, Kolumbia, Ecuadori, Hongkong KKT, India, indonéziai, Irak, japán, koreai, malajziai, Új-Zéland, paraguayi, Perui, Fülöp-szigeteki, szingapúri, Srí Lanka, Tajvan, Thaiföld, Uruguayi és Bolivári.</span><span class="sxs-lookup"><span data-stu-id="7b25a-125">Afghanistan, Argentina, Australia, Brazil, Chile, Colombia, Ecuador, Hong Kong SAR, India, Indonesia, Iraq, Japan, Korea, Malaysia, New Zealand, Paraguay, Peru, Philippines, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay and Venezuela.</span></span>

## <a name="preview-tenant"></a><span data-ttu-id="7b25a-126">Előzetes bérlői</span><span class="sxs-lookup"><span data-stu-id="7b25a-126">Preview tenant</span></span>
<span data-ttu-id="7b25a-127">A B2C-bérlő során az Azure AD B2C előzetes verzió idejének hozna létre, ha valószínű, amely a **típus bérlői** szerint **Preview bérlői**.</span><span class="sxs-lookup"><span data-stu-id="7b25a-127">If you had created a B2C tenant during Azure AD B2C's preview period, it is likely that your **Tenant type** says **Preview tenant**.</span></span> <span data-ttu-id="7b25a-128">Ha ez helyzet hello, csak a fejlesztési és tesztelési célra, és nem éles alkalmazások esetén a bérlő kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7b25a-128">If this is hello case, you MUST use your tenant only for development and testing purposes, and NOT for production apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b25a-129">Nincs a B2C bérlő tooa-éles környezetben való méretezésre B2C-bérlő Preview áttelepítési útvonal.</span><span class="sxs-lookup"><span data-stu-id="7b25a-129">There is no migration path from a preview B2C tenant tooa production-scale B2C tenant.</span></span> <span data-ttu-id="7b25a-130">Vegye figyelembe, hogy vannak ismert problémák a B2C előzetes bérlő törlésekor, és hozza létre újból egy éles környezetben való méretezésre B2C-bérlőben hello ugyanazon a néven.</span><span class="sxs-lookup"><span data-stu-id="7b25a-130">Note that there are known issues when you delete a preview B2C tenant and re-create a production-scale B2C tenant with hello same domain name.</span></span> <span data-ttu-id="7b25a-131">Toocreate rendelkezik egy éles környezetben való méretezésre B2C-bérlő egy másik tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="7b25a-131">You have toocreate a production-scale B2C tenant with a different domain name.</span></span>


![A kép bérlő képernyőfelvétele](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
