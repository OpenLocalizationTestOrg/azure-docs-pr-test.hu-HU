---
title: "aaaAzure hello piactér-alkalmazások felügyelete |} Microsoft Docs"
description: "Ismerteti az Azure által felügyelt alkalmazások hello piactéren keresztül elérhető."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b3cdf3f1fccdd47db699e4892ae8bce35118bfd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-in-hello-marketplace"></a><span data-ttu-id="3de63-103">Azure által felügyelt alkalmazások hello piactér</span><span class="sxs-lookup"><span data-stu-id="3de63-103">Azure managed applications in hello Marketplace</span></span>

 <span data-ttu-id="3de63-104">MSPs ISV-k és rendszerintegrátorok (SIs) használható Azure által felügyelt alkalmazások toooffer megoldások tooall Azure piactér ügyfeleiknek.</span><span class="sxs-lookup"><span data-stu-id="3de63-104">MSPs, ISVs, and system integrators (SIs) can use Azure managed applications toooffer their solutions tooall Azure Marketplace customers.</span></span> <span data-ttu-id="3de63-105">Az ilyen megoldások hello karbantartási és a terhelés karbantartásának ügyfelek csökkentése.</span><span class="sxs-lookup"><span data-stu-id="3de63-105">Such solutions reduce hello maintenance and servicing overhead for customers.</span></span> <span data-ttu-id="3de63-106">Közzétevők biztosíthatja az infrastruktúra- és szoftver hello piactéren keresztül.</span><span class="sxs-lookup"><span data-stu-id="3de63-106">Publishers can sell infrastructure and software through hello Marketplace.</span></span> <span data-ttu-id="3de63-107">Ezek csatolhat a szolgáltatások és alkalmazások működési támogatását toomanaged.</span><span class="sxs-lookup"><span data-stu-id="3de63-107">They can attach services and operational support toomanaged applications.</span></span> <span data-ttu-id="3de63-108">További információkért lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-108">For more information, see [Managed application overview](managed-application-overview.md).</span></span>

<span data-ttu-id="3de63-109">Ez a cikk azt ismerteti, hogyan egy MSP, ISV vagy SI közzétenni egy alkalmazást toohello piactér és toocustomers körben elérhetővé tegye.</span><span class="sxs-lookup"><span data-stu-id="3de63-109">This article explains how an MSP, ISV, or SI can publish an application toohello Marketplace and make it broadly available toocustomers.</span></span>

## <a name="prerequisites-for-publishing-a-managed-application"></a><span data-ttu-id="3de63-110">Kezelt alkalmazás közzététele előfeltételei</span><span class="sxs-lookup"><span data-stu-id="3de63-110">Prerequisites for publishing a managed application</span></span>

<span data-ttu-id="3de63-111">A piactér hello toolisting Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="3de63-111">Prerequisites toolisting in hello Marketplace:</span></span>

* <span data-ttu-id="3de63-112">Műszaki</span><span class="sxs-lookup"><span data-stu-id="3de63-112">Technical</span></span>

    *  <span data-ttu-id="3de63-113">Hello alapszintű struktúrát és az Azure Resource Manager-sablonok szintaxisát kapcsolatos információkért lásd: [Azure Resource Manager-sablonok](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-113">For information about hello basic structure and syntax of Azure Resource Manager templates, see [Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
    *  <span data-ttu-id="3de63-114">tooview teljes sablon megoldások, lásd: [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/en-us/documentation/templates/) vagy hello [gyorsindítási sablonon tárház](https://github.com/azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="3de63-114">tooview complete template solutions, see [Azure Quickstart templates](https://azure.microsoft.com/en-us/documentation/templates/) or hello [Quickstart template repository](https://github.com/azure/azure-quickstart-templates).</span></span>
    *  <span data-ttu-id="3de63-115">További információ a hogyan toocreate hello felületet az ügyfelek, akik hello piactéren keresztül az alkalmazás központi telepítése: [hozzon létre egy felhasználói felületet csomagdefiníciós fájl](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-115">For information about how toocreate hello interface for customers who deploy your application through hello Marketplace, see [Create a user interface definition file](managed-application-createuidefinition-overview.md).</span></span>

* <span data-ttu-id="3de63-116">A felhasználóknak (üzleti követelmények)</span><span class="sxs-lookup"><span data-stu-id="3de63-116">Nontechnical (business requirements)</span></span>

    *   <span data-ttu-id="3de63-117">A vállalat vagy a helyi leányvállalatnál olyan országban, ahol értékesítési hello piactér által támogatott kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3de63-117">Your company or its subsidiary must be located in a country where sales are supported by hello Marketplace.</span></span>
    *   <span data-ttu-id="3de63-118">A termék licenccel kell rendelkezniük a oly módon, amely kompatibilis a számlázási modellt hello piactér támogat.</span><span class="sxs-lookup"><span data-stu-id="3de63-118">Your product must be licensed in a way that is compatible with billing models supported by hello Marketplace.</span></span>
    *   <span data-ttu-id="3de63-119">Ön a felelős a technikai támogatási szolgálathoz elérhető toocustomers minden üzleti szempontból ésszerű módon.</span><span class="sxs-lookup"><span data-stu-id="3de63-119">You're responsible for making technical support available toocustomers in a commercially reasonable manner.</span></span> <span data-ttu-id="3de63-120">hello támogatási ingyenes, a fizetős, és közösségi keresztül támogatja.</span><span class="sxs-lookup"><span data-stu-id="3de63-120">hello support can be free, paid, or through community support.</span></span>
    *   <span data-ttu-id="3de63-121">Ön a felelős a szoftver és a harmadik féltől származó szoftverek függőségeit.</span><span class="sxs-lookup"><span data-stu-id="3de63-121">You're responsible for licensing your software and any third-party software dependencies.</span></span>
    *   <span data-ttu-id="3de63-122">Meg kell adnia, amely megfelel az ajánlat toobe felsorolt tartalom hello piactér és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3de63-122">You must provide content that meets criteria for your offering toobe listed in hello Marketplace and in hello Azure portal.</span></span>
    *   <span data-ttu-id="3de63-123">Meg kell erősítenie hello Azure piactér részvételét házirendek és a közzétevő megállapodás toohello feltételeit.</span><span class="sxs-lookup"><span data-stu-id="3de63-123">You must agree toohello terms of hello Azure Marketplace Participation Policies and Publisher Agreement.</span></span>
    *   <span data-ttu-id="3de63-124">El kell fogadnia a használati feltételeket hello, a Microsoft adatvédelmi nyilatkozatát és a Microsoft Azure hitelesített Program szerződés toocomply.</span><span class="sxs-lookup"><span data-stu-id="3de63-124">You must agree toocomply with hello Terms of Use, Microsoft Privacy Statement, and Microsoft Azure Certified Program Agreement.</span></span>

## <a name="create-a-new-azure-application-offer"></a><span data-ttu-id="3de63-125">Hozzon létre egy új Azure-alkalmazásokban ajánlatot</span><span class="sxs-lookup"><span data-stu-id="3de63-125">Create a new Azure application offer</span></span>

<span data-ttu-id="3de63-126">Megfelel a hello Előfeltételek, után készen áll a toocreate még a felügyelt alkalmazási ajánlat.</span><span class="sxs-lookup"><span data-stu-id="3de63-126">After you meet hello prerequisites, you're ready toocreate your managed application offer.</span></span> <span data-ttu-id="3de63-127">Vegyünk egy ajánlatot és egy SKU gyors áttekintést.</span><span class="sxs-lookup"><span data-stu-id="3de63-127">Let's take a quick overview of an offer and a SKU.</span></span>

### <a name="offer"></a><span data-ttu-id="3de63-128">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="3de63-128">Offer</span></span>

<span data-ttu-id="3de63-129">hello ajánlat a kezelt alkalmazás tooa osztály egy közzétételi ajánlat termék felel meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-129">hello offer for a managed application corresponds tooa class of product offering from a publisher.</span></span> <span data-ttu-id="3de63-130">Ha egy új típusú megoldás/alkalmazás, amelyet az toomake hello piactéren elérhető, állíthat be, mint egy új ajánlat.</span><span class="sxs-lookup"><span data-stu-id="3de63-130">If you have a new type of solution/application that you want toomake available in hello Marketplace, you can set it up as a new offer.</span></span> <span data-ttu-id="3de63-131">Az ajánlat termékváltozatok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="3de63-131">An offer is a collection of SKUs.</span></span> <span data-ttu-id="3de63-132">Minden ajánlat hello piactér saját entitás jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-132">Every offer appears as its own entity in hello Marketplace.</span></span>

### <a name="sku"></a><span data-ttu-id="3de63-133">SKU</span><span class="sxs-lookup"><span data-stu-id="3de63-133">SKU</span></span>

<span data-ttu-id="3de63-134">Egy termékváltozata hello legkisebb purchasable egysége ajánlatot.</span><span class="sxs-lookup"><span data-stu-id="3de63-134">A SKU is hello smallest purchasable unit of an offer.</span></span> <span data-ttu-id="3de63-135">A Termékváltozat hello belül is használhatja ugyanazt a termék osztály (ajánlat) toodifferentiate között:</span><span class="sxs-lookup"><span data-stu-id="3de63-135">You can use a SKU within hello same product class (offer) toodifferentiate between:</span></span>

* <span data-ttu-id="3de63-136">Támogatott különböző szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="3de63-136">Different features that are supported.</span></span>
* <span data-ttu-id="3de63-137">E hello ajánlat felügyelt vagy nem felügyelt.</span><span class="sxs-lookup"><span data-stu-id="3de63-137">Whether hello offer is managed or unmanaged.</span></span>
* <span data-ttu-id="3de63-138">Számlázási modellt a támogatottak.</span><span class="sxs-lookup"><span data-stu-id="3de63-138">Billing models that are supported.</span></span>

<span data-ttu-id="3de63-139">A Termékváltozat hello szülő ajánlat hello piactér alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-139">A SKU appears under hello parent offer in hello Marketplace.</span></span> <span data-ttu-id="3de63-140">A saját purchasable entitás hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-140">It appears as its own purchasable entity in hello Azure portal.</span></span>

### <a name="set-up-an-offer"></a><span data-ttu-id="3de63-141">Az ajánlat beállítása</span><span class="sxs-lookup"><span data-stu-id="3de63-141">Set up an offer</span></span>

1. <span data-ttu-id="3de63-142">Jelentkezzen be toohello [Cloud Partner portálra](https://cloudpartner.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3de63-142">Sign in toohello [Cloud Partner portal](https://cloudpartner.azure.com/).</span></span>

2. <span data-ttu-id="3de63-143">Hello bal oldali hello navigációs ablaktábláján válassza ki **+ új ajánlat** > **Azure alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3de63-143">In hello navigation pane on hello left, select **+ New offer** > **Azure Applications**.</span></span>

    ![Új ajánlat](./media/managed-application-author-marketplace/newOffer.png)

3. <span data-ttu-id="3de63-145">Töltse ki az hello hello megjelenő hello űrlapok **szerkesztő** nézet.</span><span class="sxs-lookup"><span data-stu-id="3de63-145">Fill out hello forms that appear on hello left in hello **Editor** view.</span></span> <span data-ttu-id="3de63-146">A piros csillaggal (*) megjelölve kötelező mezőket.</span><span class="sxs-lookup"><span data-stu-id="3de63-146">Required fields are marked with a red asterisk (*).</span></span>

    ![Az ajánlat beállításait](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    <span data-ttu-id="3de63-148">Négy fő űrlap használt toocreate kezelt alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="3de63-148">Four main forms are used toocreate a managed application:</span></span>

    <span data-ttu-id="3de63-149">a.</span><span class="sxs-lookup"><span data-stu-id="3de63-149">a.</span></span> <span data-ttu-id="3de63-150">Az ajánlat beállításait</span><span class="sxs-lookup"><span data-stu-id="3de63-150">Offer Settings</span></span>

    <span data-ttu-id="3de63-151">b.</span><span class="sxs-lookup"><span data-stu-id="3de63-151">b.</span></span> <span data-ttu-id="3de63-152">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="3de63-152">SKUs</span></span>

    <span data-ttu-id="3de63-153">c.</span><span class="sxs-lookup"><span data-stu-id="3de63-153">c.</span></span> <span data-ttu-id="3de63-154">Piactér</span><span class="sxs-lookup"><span data-stu-id="3de63-154">Marketplace</span></span>

    <span data-ttu-id="3de63-155">d.</span><span class="sxs-lookup"><span data-stu-id="3de63-155">d.</span></span> <span data-ttu-id="3de63-156">Támogatás</span><span class="sxs-lookup"><span data-stu-id="3de63-156">Support</span></span>

<span data-ttu-id="3de63-157">Ezek az űrlapok a következő részekben hello részletesebben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3de63-157">These forms are described in greater detail in hello following sections.</span></span>

## <a name="offer-settings-form"></a><span data-ttu-id="3de63-158">Az ajánlat beállítások képernyő</span><span class="sxs-lookup"><span data-stu-id="3de63-158">Offer Settings form</span></span>
<span data-ttu-id="3de63-159">Az alapvető űrlap toospecify hello ajánlat beállításait használja.</span><span class="sxs-lookup"><span data-stu-id="3de63-159">Use this basic form toospecify hello offer settings.</span></span>

1. <span data-ttu-id="3de63-160">Adja meg a hello **beállításokat kínálnak** űrlap.</span><span class="sxs-lookup"><span data-stu-id="3de63-160">Fill in hello **Offer Settings** form.</span></span> <span data-ttu-id="3de63-161">hello különböző mezők a következők:</span><span class="sxs-lookup"><span data-stu-id="3de63-161">hello different fields are:</span></span>

    <span data-ttu-id="3de63-162">a.</span><span class="sxs-lookup"><span data-stu-id="3de63-162">a.</span></span> <span data-ttu-id="3de63-163">**Ajánlat azonosítója**: Ez az egyedi azonosító a közzétevő profilon belül hello ajánlat azonosítja.</span><span class="sxs-lookup"><span data-stu-id="3de63-163">**Offer ID**: This unique identifier identifies hello offer within a publisher profile.</span></span> <span data-ttu-id="3de63-164">Ezt az Azonosítót látható termék URL-címek, Resource Manager-sablonok, és számlázási jelenti.</span><span class="sxs-lookup"><span data-stu-id="3de63-164">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="3de63-165">Csak összeállítható kisbetűs alfanumerikus karaktereket és kötőjelet (-).</span><span class="sxs-lookup"><span data-stu-id="3de63-165">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="3de63-166">hello azonosítója nem végződhet kötőjellel.</span><span class="sxs-lookup"><span data-stu-id="3de63-166">hello ID can't end in a dash.</span></span> <span data-ttu-id="3de63-167">Korlátozva tooa legfeljebb 50 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="3de63-167">It's limited tooa maximum of 50 characters.</span></span> <span data-ttu-id="3de63-168">Miután ajánlatot élő kerül, ez a mező zárolva van.</span><span class="sxs-lookup"><span data-stu-id="3de63-168">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="3de63-169">b.</span><span class="sxs-lookup"><span data-stu-id="3de63-169">b.</span></span> <span data-ttu-id="3de63-170">**Gyártó Azonosítóját**: legördülő lista toochoose hello publisher profilt ezt az ajánlatot a toopublish szeretné.</span><span class="sxs-lookup"><span data-stu-id="3de63-170">**Publisher ID**: Use this drop-down list toochoose hello publisher profile you want toopublish this offer under.</span></span> <span data-ttu-id="3de63-171">Miután ajánlatot élő kerül, ez a mező zárolva van.</span><span class="sxs-lookup"><span data-stu-id="3de63-171">After an offer goes live, this field is locked.</span></span>

    <span data-ttu-id="3de63-172">c.</span><span class="sxs-lookup"><span data-stu-id="3de63-172">c.</span></span> <span data-ttu-id="3de63-173">**Név**: ezt a megjelenítési nevet az előfizetéshez hello piactér és a hello portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-173">**Name**: This display name for your offer appears in hello Marketplace and in hello portal.</span></span> <span data-ttu-id="3de63-174">Rendelkezhet egy legfeljebb 50 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="3de63-174">It can have a maximum of 50 characters.</span></span> <span data-ttu-id="3de63-175">A termék márkáját egy felismerhető nevet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3de63-175">Include a recognizable brand name for your product.</span></span> <span data-ttu-id="3de63-176">A vállalat nevét itt nem tartalmaznak, kivéve, ha ezt forgalmazott hogyan.</span><span class="sxs-lookup"><span data-stu-id="3de63-176">Don't include your company name here unless that's how it's marketed.</span></span> <span data-ttu-id="3de63-177">Ha ez az ajánlat még marketing a saját webhelyén, győződjön meg arról, hogy hello név pontosan hogyan webhelye jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-177">If you're marketing this offer on your own website, ensure that hello name is exactly how it appears on your website.</span></span>

2. <span data-ttu-id="3de63-178">Válassza ki **mentése** toosave az előrehaladást.</span><span class="sxs-lookup"><span data-stu-id="3de63-178">Select **Save** toosave your progress.</span></span> 

## <a name="skus-form"></a><span data-ttu-id="3de63-179">SKU képernyő</span><span class="sxs-lookup"><span data-stu-id="3de63-179">SKUs form</span></span>
<span data-ttu-id="3de63-180">hello következő lépésre tooadd termékváltozatok az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="3de63-180">hello next step is tooadd SKUs for your offer.</span></span>

1. <span data-ttu-id="3de63-181">Válassza ki **termékváltozatok** > **új SKU**.</span><span class="sxs-lookup"><span data-stu-id="3de63-181">Select **SKUs** > **New SKU**.</span></span> 

    ![Válassza ki az új Termékváltozat](./media/managed-application-author-marketplace/newOffer_skus.png)

2. <span data-ttu-id="3de63-183">Adjon meg egy **SKU-ID**.</span><span class="sxs-lookup"><span data-stu-id="3de63-183">Enter a **SKU ID**.</span></span> <span data-ttu-id="3de63-184">A SKU-ID hello SKU ajánlatot belül egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3de63-184">A SKU ID is a unique identifier for hello SKU within an offer.</span></span> <span data-ttu-id="3de63-185">Ezt az Azonosítót látható termék URL-címek, Resource Manager-sablonok, és számlázási jelenti.</span><span class="sxs-lookup"><span data-stu-id="3de63-185">This ID is visible in product URLs, Resource Manager templates, and billing reports.</span></span> <span data-ttu-id="3de63-186">Csak összeállítható kisbetűs alfanumerikus karaktereket és kötőjelet (-).</span><span class="sxs-lookup"><span data-stu-id="3de63-186">It can only be composed of lowercase alphanumeric characters or dashes (-).</span></span> <span data-ttu-id="3de63-187">hello azonosítója nem végződhet kötőjellel, és korlátozott tooa legfeljebb 50 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="3de63-187">hello ID can't end in a dash, and it's limited tooa maximum of 50 characters.</span></span> <span data-ttu-id="3de63-188">Miután ajánlatot élő kerül, ez a mező zárolva van.</span><span class="sxs-lookup"><span data-stu-id="3de63-188">After an offer goes live, this field is locked.</span></span> <span data-ttu-id="3de63-189">Az ajánlat belül több SKU lehet.</span><span class="sxs-lookup"><span data-stu-id="3de63-189">You can have multiple SKUs within an offer.</span></span> <span data-ttu-id="3de63-190">A Termékváltozat van szüksége az egyes lemezképek toopublish tervezi.</span><span class="sxs-lookup"><span data-stu-id="3de63-190">You need a SKU for each image you plan toopublish.</span></span>

3. <span data-ttu-id="3de63-191">Töltse ki a hello **SKU részletek** hello képernyőn a következő szakaszt:</span><span class="sxs-lookup"><span data-stu-id="3de63-191">Fill out hello **SKU Details** section on hello following form:</span></span>

    ![Adja meg az új Termékváltozat](./media/managed-application-author-marketplace/newOffer_newsku.png)

    <span data-ttu-id="3de63-193">Töltse ki a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="3de63-193">Fill out hello following fields:</span></span>
    
    <span data-ttu-id="3de63-194">a.</span><span class="sxs-lookup"><span data-stu-id="3de63-194">a.</span></span> <span data-ttu-id="3de63-195">**Cím**: Adjon meg egy címet a Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="3de63-195">**Title**: Enter a title for this SKU.</span></span> <span data-ttu-id="3de63-196">Ez a cím jelenik meg ehhez az elemhez hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="3de63-196">This title appears in hello gallery for this item.</span></span>

    <span data-ttu-id="3de63-197">b.</span><span class="sxs-lookup"><span data-stu-id="3de63-197">b.</span></span> <span data-ttu-id="3de63-198">**Összegző**: Adjon meg egy rövid összefoglaló a termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="3de63-198">**Summary**: Enter a short summary for this SKU.</span></span> <span data-ttu-id="3de63-199">Ez a szöveg hello cím alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-199">This text appears underneath hello title.</span></span>

    <span data-ttu-id="3de63-200">c.</span><span class="sxs-lookup"><span data-stu-id="3de63-200">c.</span></span> <span data-ttu-id="3de63-201">**Leírás**: hello SKU kapcsolatos részletes leírását adja meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-201">**Description**: Enter a detailed description about hello SKU.</span></span>

    <span data-ttu-id="3de63-202">d.</span><span class="sxs-lookup"><span data-stu-id="3de63-202">d.</span></span> <span data-ttu-id="3de63-203">**SKU típusú**: hello két érték engedélyezett **kezelt alkalmazás** és **Solution Templates**.</span><span class="sxs-lookup"><span data-stu-id="3de63-203">**SKU Type**: hello allowed values are **Managed Application** and **Solution Templates**.</span></span> <span data-ttu-id="3de63-204">Ebben az esetben válassza a **kezelt alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3de63-204">For this case, select **Managed Application**.</span></span>

4. <span data-ttu-id="3de63-205">Töltse ki a hello **csomag részletei** hello képernyőn a következő szakaszt:</span><span class="sxs-lookup"><span data-stu-id="3de63-205">Fill out hello **Package Details** section on hello following form:</span></span>

    ![Csomag](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    <span data-ttu-id="3de63-207">Töltse ki a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="3de63-207">Fill out hello following fields:</span></span>

    <span data-ttu-id="3de63-208">a.</span><span class="sxs-lookup"><span data-stu-id="3de63-208">a.</span></span> <span data-ttu-id="3de63-209">**Aktuális verzió**: Adjon meg egy verzió hello csomag feltöltése.</span><span class="sxs-lookup"><span data-stu-id="3de63-209">**Current Version**: Enter a version for hello package you upload.</span></span> <span data-ttu-id="3de63-210">Hello formátumban kell `{number}.{number}.{number}{number}`.</span><span class="sxs-lookup"><span data-stu-id="3de63-210">It should be in hello format `{number}.{number}.{number}{number}`.</span></span>

    <span data-ttu-id="3de63-211">b.</span><span class="sxs-lookup"><span data-stu-id="3de63-211">b.</span></span> <span data-ttu-id="3de63-212">**Válassza ki a csomagfájl**: Ez a csomag tartalmaz hello tömörített fájlok .zip fájlba a következő:</span><span class="sxs-lookup"><span data-stu-id="3de63-212">**Select a package file**: This package contains hello following files that are compressed into a .zip file:</span></span>
    * <span data-ttu-id="3de63-213">**applianceMainTemplate.json**: hello központi telepítési sablon fájl toodeploy hello megoldás/alkalmazás által használt.</span><span class="sxs-lookup"><span data-stu-id="3de63-213">**applianceMainTemplate.json**: hello deployment template file that's used toodeploy hello solution/application.</span></span> <span data-ttu-id="3de63-214">További információ központi telepítési sablon fájlok toocreate, lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-214">For information about how toocreate deployment template files, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>
    * <span data-ttu-id="3de63-215">**appliancecreateUIDefinition.json**: hello Azure portál toogenerate hello felhasználói felület által használt tooprovision a megoldás/alkalmazás használják ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="3de63-215">**appliancecreateUIDefinition.json**: This file is used by hello Azure portal toogenerate hello user interface that's used tooprovision this solution/application.</span></span> <span data-ttu-id="3de63-216">További információkért lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-216">For more information, see [Get started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
    * <span data-ttu-id="3de63-217">**mainTemplate.json**: Ez a sablon fájl csak hello Microsoft.Solution/appliances erőforrást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3de63-217">**mainTemplate.json**: This template file contains only hello Microsoft.Solution/appliances resource.</span></span> <span data-ttu-id="3de63-218">hello mainTemplate fájl tartalmazza az alábbi tulajdonságokkal hello:</span><span class="sxs-lookup"><span data-stu-id="3de63-218">hello mainTemplate file includes hello following properties:</span></span>

        *  <span data-ttu-id="3de63-219">**milyen**: használata **piactér** hello piactér a kezelt alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3de63-219">**kind**: Use **Marketplace** for managed applications in hello Marketplace.</span></span>
        *  <span data-ttu-id="3de63-220">**ManagedResourceGroupId**: Ez az erőforráscsoport hello ügyfél-előfizetést a, ahol applianceMainTemplate.json definiált összes hello erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="3de63-220">**ManagedResourceGroupId**: This resource group in hello customer's subscription is where all hello resources defined in applianceMainTemplate.json are deployed.</span></span>
        *  <span data-ttu-id="3de63-221">**PublisherPackageId**: Ez a karakterlánc egyedi azonosítására szolgál a hello csomag.</span><span class="sxs-lookup"><span data-stu-id="3de63-221">**PublisherPackageId**: This string uniquely identifies hello package.</span></span> <span data-ttu-id="3de63-222">Adjon meg értéket hello hello formátuma `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span><span class="sxs-lookup"><span data-stu-id="3de63-222">Provide hello value in hello format of `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.</span></span>

<span data-ttu-id="3de63-223">Hello beszerzése **kínálnak azonosító** és **gyártó Azonosítóját** a portálon, közzétételi hello kép a következő ábrán hello:</span><span class="sxs-lookup"><span data-stu-id="3de63-223">Obtain hello **Offer ID** and **Publisher ID** from hello publishing portal, as shown in hello following image:</span></span>

![Ajánlat azonosítója](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
<span data-ttu-id="3de63-225">Szerezze be a hello **SKU-ID**, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="3de63-225">Obtain hello **SKU ID**, as shown in hello following image:</span></span>

![TERMÉKVÁLTOZAT-AZONOSÍTÓ](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
<span data-ttu-id="3de63-227">Hello csomag beszerzése **verzió**, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="3de63-227">Obtain hello package **Version**, as shown in hello following image:</span></span>

![Csomag verziója](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  <span data-ttu-id="3de63-229">Az előző példák hello alapján, hello értékének **PublisherPackageId** van `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="3de63-229">Based on hello preceding examples, hello value of **PublisherPackageId** is `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.</span></span>

  <span data-ttu-id="3de63-230">A minta mainTemplate.json:</span><span class="sxs-lookup"><span data-stu-id="3de63-230">Sample mainTemplate.json:</span></span>

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify hello name of hello storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

<span data-ttu-id="3de63-231">A csomagnak tartalmaznia kell minden olyan beágyazott sablon vagy parancsfájlokat, amelyek a szükséges toosuccessfully telepíteni ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3de63-231">This package should contain any other nested templates or scripts that are required toosuccessfully provision this application.</span></span> <span data-ttu-id="3de63-232">hello mainTemplate.json applianceMainTemplate.json és applianceCreateUIDefinition.json fájlokat kell lennie: hello gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="3de63-232">hello mainTemplate.json, applianceMainTemplate.json, and applianceCreateUIDefinition.json files must be present at hello root folder.</span></span>

* <span data-ttu-id="3de63-233">**Engedélyek**: Ez a tulajdonság határozza meg, hogy az ügyfelek előfizetések toohello erőforrások elérését és hello szintjét, illetve jogosultak.</span><span class="sxs-lookup"><span data-stu-id="3de63-233">**Authorizations**: This property defines who gets access and hello level of access toohello resources in customers' subscriptions.</span></span> <span data-ttu-id="3de63-234">hello publisher használható toomanage hello alkalmazás hello ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="3de63-234">hello publisher can use it toomanage hello application on behalf of hello customer.</span></span>
* <span data-ttu-id="3de63-235">**PrincipalId**: Ez a tulajdonság akkor hello Azure Active Directory (Azure AD) azonosítója egy felhasználó, a felhasználói csoport vagy az alkalmazás, amely rendelkezik megfelelő engedélyekkel hello erőforrásainak hello ügyfél-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="3de63-235">**PrincipalId**: This property is hello Azure Active Directory (Azure AD) identifier of a user, user group, or application that's granted certain permissions on hello resources in hello customer's subscription.</span></span> <span data-ttu-id="3de63-236">Szerepkör-definíció hello hello engedélyeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3de63-236">hello Role Definition describes hello permissions.</span></span> 
* <span data-ttu-id="3de63-237">**Szerepkör-definíció**: Ez a tulajdonság akkor hello beépített szerepköralapú hozzáférés-vezérlést (RBAC) szerepkörök az Azure AD által biztosított listáját.</span><span class="sxs-lookup"><span data-stu-id="3de63-237">**Role Definition**: This property is a list of all hello built-in Role-Based Access Control (RBAC) roles provided by Azure AD.</span></span> <span data-ttu-id="3de63-238">Kiválaszthatja a hello szerepkört, amely leginkább megfelelő toouse toomanage hello erőforrások hello ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="3de63-238">You can select hello role that's most appropriate toouse toomanage hello resources on behalf of hello customer.</span></span>

<span data-ttu-id="3de63-239">Több engedélyeket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="3de63-239">You can add multiple authorizations.</span></span> <span data-ttu-id="3de63-240">Javasoljuk, hogy hozzon létre az Active Directory-felhasználók csoportnak, és adja meg az Azonosítót a **PrincipalId**.</span><span class="sxs-lookup"><span data-stu-id="3de63-240">We recommend that you create an AD user group and specify its ID in **PrincipalId**.</span></span> <span data-ttu-id="3de63-241">Ezzel a módszerrel adhat hozzá további felhasználókat toohello felhasználói csoport hello kell tooupdate hello SKU nélkül.</span><span class="sxs-lookup"><span data-stu-id="3de63-241">This way, you can add more users toohello user group without hello need tooupdate hello SKU.</span></span>

<span data-ttu-id="3de63-242">Az RBAC kapcsolatos további információkért lásd: [megismerheti az RBAC a hello Azure-portálon](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-242">For more information about RBAC, see [Get started with RBAC in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>

## <a name="marketplace-form"></a><span data-ttu-id="3de63-243">Piactér képernyő</span><span class="sxs-lookup"><span data-stu-id="3de63-243">Marketplace form</span></span>

<span data-ttu-id="3de63-244">hello piactér űrlap kér meg hello mezők [Azure piactér](https://azuremarketplace.microsoft.com) a hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3de63-244">hello Marketplace form asks for fields that show up on hello [Azure Marketplace](https://azuremarketplace.microsoft.com) and on hello [Azure portal](https://portal.azure.com/).</span></span>

### <a name="preview-subscription-ids"></a><span data-ttu-id="3de63-245">Előzetes előfizetés-azonosítók</span><span class="sxs-lookup"><span data-stu-id="3de63-245">Preview subscription IDs</span></span>

<span data-ttu-id="3de63-246">Írjon be egy Azure-előfizetés azonosítók hello ajánlat által elérhető, hogy közzététele után.</span><span class="sxs-lookup"><span data-stu-id="3de63-246">Enter a list of Azure subscription IDs that can access hello offer after it's published.</span></span> <span data-ttu-id="3de63-247">A fehér felsorolt előfizetések tootest megtekintett hello ajánlat előtt használható live.</span><span class="sxs-lookup"><span data-stu-id="3de63-247">You can use these white-listed subscriptions tootest hello previewed offer before you make it live.</span></span> <span data-ttu-id="3de63-248">Hello partnerportálon too100 előfizetések listája üres állíthat össze.</span><span class="sxs-lookup"><span data-stu-id="3de63-248">You can compile a white list of up too100 subscriptions in hello partner portal.</span></span>

### <a name="suggested-categories"></a><span data-ttu-id="3de63-249">Javasolt kategóriák</span><span class="sxs-lookup"><span data-stu-id="3de63-249">Suggested categories</span></span>

<span data-ttu-id="3de63-250">Válassza ki az ajánlatot is legjobb társítható hello listából toofive kategóriák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3de63-250">Select up toofive categories from hello list that your offer can be best associated with.</span></span> <span data-ttu-id="3de63-251">Ezen kategóriák vannak használt toomap az ajánlat toohello termékkategóriák hello elérhető [Azure piactér](https://azuremarketplace.microsoft.com) és hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3de63-251">These categories are used toomap your offer toohello product categories that are available in hello [Azure Marketplace](https://azuremarketplace.microsoft.com) and hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="azure-marketplace"></a><span data-ttu-id="3de63-252">Azure Piactér</span><span class="sxs-lookup"><span data-stu-id="3de63-252">Azure Marketplace</span></span>

<span data-ttu-id="3de63-253">a felügyelt alkalmazás hello összegzése a következő mezők hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="3de63-253">hello summary of your managed application displays hello following fields:</span></span>

![Piactér-összefoglaló](./media/managed-application-author-marketplace/publishvm10.png)

<span data-ttu-id="3de63-255">Hello **áttekintése** a felügyelt alkalmazás jeleníti meg a következő mezők hello lapján:</span><span class="sxs-lookup"><span data-stu-id="3de63-255">hello **Overview** tab for your managed application displays hello following fields:</span></span>

![Piactér áttekintése](./media/managed-application-author-marketplace/publishvm11.png)

<span data-ttu-id="3de63-257">Hello **tervek + árazás** a felügyelt alkalmazás jeleníti meg a következő mezők hello lapján:</span><span class="sxs-lookup"><span data-stu-id="3de63-257">hello **Plans + Pricing** tab for your managed application displays hello following fields:</span></span>

![Piactér tervek](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a><span data-ttu-id="3de63-259">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3de63-259">Azure portal</span></span>

<span data-ttu-id="3de63-260">a felügyelt alkalmazás hello összegzése a következő mezők hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="3de63-260">hello summary of your managed application displays hello following fields:</span></span>

![A portál összefoglaló](./media/managed-application-author-marketplace/publishvm12.png)

<span data-ttu-id="3de63-262">a felügyelt alkalmazás hello áttekintése a következő mezők hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="3de63-262">hello overview for your managed application displays hello following fields:</span></span>

![Portál áttekintése](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a><span data-ttu-id="3de63-264">Emblémáinak használatáról</span><span class="sxs-lookup"><span data-stu-id="3de63-264">Logo guidelines</span></span>

<span data-ttu-id="3de63-265">Kövesse a bármely hello Cloud Partner portálra a feltöltött embléma:</span><span class="sxs-lookup"><span data-stu-id="3de63-265">Follow these guidelines for any logo that you upload in hello Cloud Partner portal:</span></span>

*   <span data-ttu-id="3de63-266">hello Azure tervezési egy egyszerű színpaletta rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3de63-266">hello Azure design has a simple color palette.</span></span> <span data-ttu-id="3de63-267">Hello száma elsődleges és másodlagos színek az embléma korlátozza.</span><span class="sxs-lookup"><span data-stu-id="3de63-267">Limit hello number of primary and secondary colors on your logo.</span></span>
*   <span data-ttu-id="3de63-268">hello téma hello portál színeket fehér és fekete.</span><span class="sxs-lookup"><span data-stu-id="3de63-268">hello theme colors of hello portal are white and black.</span></span> <span data-ttu-id="3de63-269">Nem használja ezeket a színeket hello háttérszínnel az embléma.</span><span class="sxs-lookup"><span data-stu-id="3de63-269">Don't use these colors as hello background color for your logo.</span></span> <span data-ttu-id="3de63-270">Használjon, amely lehetővé teszi az embléma jól láthatóan elhelyezett hello portálon színt.</span><span class="sxs-lookup"><span data-stu-id="3de63-270">Use a color that makes your logo prominent in hello portal.</span></span> <span data-ttu-id="3de63-271">Azt javasoljuk, hogy egyszerű alapszínek.</span><span class="sxs-lookup"><span data-stu-id="3de63-271">We recommend simple primary colors.</span></span> <span data-ttu-id="3de63-272">*Átlátszó háttérrel használatakor győződjön meg arról, hogy hello embléma és a szöveg nem fehér, fekete, vagy a kék.*</span><span class="sxs-lookup"><span data-stu-id="3de63-272">*If you use a transparent background, make sure that hello logo and text aren't white, black, or blue.*</span></span>
*   <span data-ttu-id="3de63-273">Ne használjon a háttér átmenetének hello embléma a.</span><span class="sxs-lookup"><span data-stu-id="3de63-273">Don't use a gradient background on hello logo.</span></span>
*   <span data-ttu-id="3de63-274">Ne tegyen szöveg hello embléma, még a vállalat vagy a márka neve.</span><span class="sxs-lookup"><span data-stu-id="3de63-274">Don't place text on hello logo, not even your company or brand name.</span></span> <span data-ttu-id="3de63-275">hello megjelenését és működését az embléma legyen egyszerű és átmenetek elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="3de63-275">hello look and feel of your logo should be flat and avoid gradients.</span></span>
*   <span data-ttu-id="3de63-276">Ellenőrizze, hogy hello embléma nincs archiválva a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="3de63-276">Make sure hello logo isn't stretched.</span></span>

#### <a name="hero-logo"></a><span data-ttu-id="3de63-277">Hero-embléma</span><span class="sxs-lookup"><span data-stu-id="3de63-277">Hero logo</span></span>

<span data-ttu-id="3de63-278">hello hero embléma nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="3de63-278">hello hero logo is optional.</span></span> <span data-ttu-id="3de63-279">hello publisher nem tooupload hero embléma választhat.</span><span class="sxs-lookup"><span data-stu-id="3de63-279">hello publisher can choose not tooupload a hero logo.</span></span> <span data-ttu-id="3de63-280">Miután hello hero ikon töltheti fel, nem lehet törölni.</span><span class="sxs-lookup"><span data-stu-id="3de63-280">After hello hero icon is uploaded, it can't be deleted.</span></span> <span data-ttu-id="3de63-281">Ugyanakkor a hello partner hello piactér irányelvek hero ikonok kell követnie.</span><span class="sxs-lookup"><span data-stu-id="3de63-281">At that time, hello partner must follow hello Marketplace guidelines for hero icons.</span></span>

<span data-ttu-id="3de63-282">Kövesse a hello hero embléma ikon:</span><span class="sxs-lookup"><span data-stu-id="3de63-282">Follow these guidelines for hello hero logo icon:</span></span>

*   <span data-ttu-id="3de63-283">hello publisher megjelenített név, a hello terv jogcímre és a hosszú összefoglalás hello ajánlat fehér jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3de63-283">hello publisher display name, hello plan title, and hello offer long summary are displayed in white.</span></span> <span data-ttu-id="3de63-284">Ezért ne használjon világos színű hello háttér hello hero ikon.</span><span class="sxs-lookup"><span data-stu-id="3de63-284">Therefore, don't use a light color for hello background of hello hero icon.</span></span> <span data-ttu-id="3de63-285">Egy fekete, fehér vagy átlátszó háttér hero ikonok nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="3de63-285">A black, white, or transparent background isn't allowed for hero icons.</span></span>
*   <span data-ttu-id="3de63-286">Hello ajánlat szerepel, miután hello publisher megjelenítésére neve, hello terv cím, hello ajánlat hosszú összefoglalás hello **létrehozása** gomb programozott módon beágyazott hello hero embléma belül.</span><span class="sxs-lookup"><span data-stu-id="3de63-286">After hello offer is listed, hello publisher display name, hello plan title, hello offer long summary, and hello **Create** button are embedded programmatically inside hello hero logo.</span></span> <span data-ttu-id="3de63-287">Ezért ne adjon meg szöveget hello hero embléma tervezése közben.</span><span class="sxs-lookup"><span data-stu-id="3de63-287">Consequently, don't enter any text while you design hello hero logo.</span></span> <span data-ttu-id="3de63-288">Meghagyása üres területet hello jobb mivel hello szöveg szerepel programozott módon, hogy a hely.</span><span class="sxs-lookup"><span data-stu-id="3de63-288">Leave empty space on hello right because hello text is included programmatically in that space.</span></span> <span data-ttu-id="3de63-289">hello üres hello szöveg kell lennie a jobb oldali hello 415 x 100 képpont.</span><span class="sxs-lookup"><span data-stu-id="3de63-289">hello empty space for hello text should be 415 x 100 pixels on hello right.</span></span> <span data-ttu-id="3de63-290">Hello balról 370 képpont ellensúlyozza.</span><span class="sxs-lookup"><span data-stu-id="3de63-290">It's offset by 370 pixels from hello left.</span></span>

    ![Hero embléma – példa](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a><span data-ttu-id="3de63-292">Támogatja az űrlap</span><span class="sxs-lookup"><span data-stu-id="3de63-292">Support form</span></span>

<span data-ttu-id="3de63-293">Töltse ki a hello **támogatja** űrlap-támogatással rendelkező kapcsolatba lép a vállalat.</span><span class="sxs-lookup"><span data-stu-id="3de63-293">Fill out hello **Support** form with support contacts from your company.</span></span> <span data-ttu-id="3de63-294">Ezek az információk előfordulhat, hogy mérnöki, névjegyek és ügyfél-támogatási kapcsolattartók.</span><span class="sxs-lookup"><span data-stu-id="3de63-294">This information might be engineering contacts and customer support contacts.</span></span>

## <a name="publish-an-offer"></a><span data-ttu-id="3de63-295">Ajánlat közzététele</span><span class="sxs-lookup"><span data-stu-id="3de63-295">Publish an offer</span></span>

<span data-ttu-id="3de63-296">Adja meg az összes hello szakaszok, után válassza ki **közzététel** toostart hello folyamat, amely lehetővé teszi a ajánlat elérhető toocustomers.</span><span class="sxs-lookup"><span data-stu-id="3de63-296">After you fill out all hello sections, select **Publish** toostart hello process that makes your offer available toocustomers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3de63-297">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3de63-297">Next steps</span></span>

* <span data-ttu-id="3de63-298">Egy bevezető toomanaged alkalmazások, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-298">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="3de63-299">További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-299">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
* <span data-ttu-id="3de63-300">További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-300">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="3de63-301">További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="3de63-301">For information about consuming a Service Catalog managed application, see [Consume a Service Catalog managed application](managed-application-consumption.md).</span></span>
