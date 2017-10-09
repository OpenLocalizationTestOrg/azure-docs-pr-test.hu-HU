---
title: "elérési út alapú aaaCreate szabály - Azure Application Gateway - Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate használatával Alkalmazásátjáró elérési alapú szabály hello portál"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: 21cb52c426ca5f7dfedf07a96e87fbc85d243647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-hello-portal"></a><span data-ttu-id="2c045-103">Alkalmazásátjáró elérési alapú szabály létrehozásához hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="2c045-103">Create a Path-based rule for an application gateway by using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c045-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c045-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="2c045-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c045-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="2c045-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2c045-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="2c045-107">URL-cím elérési út-alapú útválasztási lehetővé teszi, hogy Ön tooassociate útvonalakat a Http-kérelem hello URL-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="2c045-107">URL Path-based routing enables you tooassociate routes based on hello URL path of Http request.</span></span> <span data-ttu-id="2c045-108">Ellenőrzi, hogy az útvonal tooa háttér-készlet hello URL-címe a hello Alkalmazásátjáró konfigurálva van, és elküldi a hello hálózati forgalom toohello definiált háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="2c045-108">It checks if there is a route tooa back-end pool configured for hello URL listed in hello Application Gateway and sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="2c045-109">URL-alapú útválasztási gyakori felhasználási tooload-érkező kérések elosztása a különböző típusú tartalmakat toodifferent háttér-kiszolgálófiók készletek.</span><span class="sxs-lookup"><span data-stu-id="2c045-109">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="2c045-110">Új szabály típusa tooapplication átjáró URL-alapú útválasztási vezet be.</span><span class="sxs-lookup"><span data-stu-id="2c045-110">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="2c045-111">Alkalmazásátjáró két szabály tartozik: alapszintű és az elérési út-alapú szabályok.</span><span class="sxs-lookup"><span data-stu-id="2c045-111">Application gateway has two rule types: basic and path-based rules.</span></span> <span data-ttu-id="2c045-112">Alapszintű szabálytípus hello, biztosít a ciklikus multiplexelési hello háttér-szolgáltatás a készletbe közben elérési-alapú szabályok továbbá tooround multiplexelés terjesztési, is hello kérelem URL-címének elérési út mintája figyelembe veszi a hello megfelelő háttérkészlet kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="2c045-112">hello basic rule type, provides round-robin service for hello back-end pools while path-based rules in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello appropriate backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="2c045-113">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2c045-113">Scenario</span></span>

<span data-ttu-id="2c045-114">hello következő forgatókönyv végighalad az elérési út alapú szabályt hoz létre, egy meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2c045-114">hello following scenario goes through creating a Path-based rule in an existing application gateway.</span></span>
<span data-ttu-id="2c045-115">hello a forgatókönyv azt feltételezi, hogy már követte hello lépéseket túl[Alkalmazásátjáró létrehozása](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2c045-115">hello scenario assumes that you have already followed hello steps too[Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

![URL-cím útvonal][scenario]

## <span data-ttu-id="2c045-117"><a name="createrule"></a>Hello elérési alapú szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c045-117"><a name="createrule"></a>Create hello Path-based rule</span></span>

<span data-ttu-id="2c045-118">Elérési út alapuló szabály saját figyelő szükséges lehet, hogy rendelkezik egy elérhető figyelő toouse tooverify hello szabály létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="2c045-118">A Path-based rule requires its own listener, before creating hello rule be sure tooverify you have an available listener toouse.</span></span>

### <a name="step-1"></a><span data-ttu-id="2c045-119">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2c045-119">Step 1</span></span>

<span data-ttu-id="2c045-120">Keresse meg a toohello [Azure-portálon](http://portal.azure.com) válassza ki a meglévő Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="2c045-120">Navigate toohello [Azure portal](http://portal.azure.com) and select an existing application gateway.</span></span> <span data-ttu-id="2c045-121">Kattintson a **szabályok**</span><span class="sxs-lookup"><span data-stu-id="2c045-121">Click **Rules**</span></span>

![Az Application Gateway áttekintése][1]

### <a name="step-2"></a><span data-ttu-id="2c045-123">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2c045-123">Step 2</span></span>

<span data-ttu-id="2c045-124">Kattintson a **elérési alapú** gomb tooadd új elérési alapuló szabály.</span><span class="sxs-lookup"><span data-stu-id="2c045-124">Click **Path-based** button tooadd a new Path-based rule.</span></span>

### <a name="step-3"></a><span data-ttu-id="2c045-125">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2c045-125">Step 3</span></span>

<span data-ttu-id="2c045-126">Hello **Hozzáadás elérési alapú szabály** panel két részből áll.</span><span class="sxs-lookup"><span data-stu-id="2c045-126">hello **Add path-based rule** blade has two sections.</span></span> <span data-ttu-id="2c045-127">első szakasz hello, ahol definiálva hello figyelő, szabály hello és hello alapértelmezett elérési útjának megadása hello neve.</span><span class="sxs-lookup"><span data-stu-id="2c045-127">hello first section is where you defined hello listener, hello name of hello rule and hello default path settings.</span></span> <span data-ttu-id="2c045-128">hello alapértelmezett elérési utak beállításait, amelyek nem tartoznak az egyéni elérési út-alapú útvonal hello útvonalak vannak.</span><span class="sxs-lookup"><span data-stu-id="2c045-128">hello default path settings are for routes that do not fall under hello custom path-based route.</span></span> <span data-ttu-id="2c045-129">második része hello hello **Hozzáadás elérési alapú szabály** panel, ahol megadhatja a hello elérési-alapú szabályok magukat.</span><span class="sxs-lookup"><span data-stu-id="2c045-129">hello second section of hello **Add path-based rule** blade is where you define hello path-based rules themselves.</span></span>

<span data-ttu-id="2c045-130">**Alapbeállítások**</span><span class="sxs-lookup"><span data-stu-id="2c045-130">**Basic Settings**</span></span>

* <span data-ttu-id="2c045-131">**Név** -ezt az értéket, amely elérhető a portál hello rövid név toohello szabály.</span><span class="sxs-lookup"><span data-stu-id="2c045-131">**Name** - This value is a friendly name toohello rule that is accessible in hello portal.</span></span>
* <span data-ttu-id="2c045-132">**Figyelő** -hello szabály használt hello figyelő értéke.</span><span class="sxs-lookup"><span data-stu-id="2c045-132">**Listener** - This value is hello listener that is used for hello rule.</span></span>
* <span data-ttu-id="2c045-133">**Alapértelmezett háttérkészlet** – Ez a beállítás akkor hello beállítás, amely meghatározza a hello háttér-toobe használt hello alapértelmezett szabály</span><span class="sxs-lookup"><span data-stu-id="2c045-133">**Default backend pool** - This setting is hello setting that defines hello back-end toobe used for hello default rule</span></span>
* <span data-ttu-id="2c045-134">**Alapértelmezett HTTP beállítások** – Ez a beállítás akkor hello beállítás, amely meghatározza a hello HTTP beállítások toobe hello alapértelmezett szabály használatos.</span><span class="sxs-lookup"><span data-stu-id="2c045-134">**Default HTTP settings** - This setting is hello setting that defines hello HTTP settings toobe used for hello default rule.</span></span>

<span data-ttu-id="2c045-135">**Elérési út-alapú szabályok**</span><span class="sxs-lookup"><span data-stu-id="2c045-135">**Path-based rules**</span></span>

* <span data-ttu-id="2c045-136">**Név** -e értéke egy rövid név toopath alapuló szabály.</span><span class="sxs-lookup"><span data-stu-id="2c045-136">**Name** - This value is a friendly name toopath-based rule.</span></span>
* <span data-ttu-id="2c045-137">**Elérési utak** – Ez a beállítás határozza meg hello elérési hello szabály keresni kezdi-forgalom</span><span class="sxs-lookup"><span data-stu-id="2c045-137">**Paths** - This setting defines hello path hello rule looks for when forwarding traffic</span></span>
* <span data-ttu-id="2c045-138">**Háttérkészlet** – Ez a beállítás akkor hello háttér-toobe hello szabályhoz használt definiáló hello beállítása</span><span class="sxs-lookup"><span data-stu-id="2c045-138">**Backend Pool** - This setting is hello setting that defines hello back-end toobe used for hello rule</span></span>
* <span data-ttu-id="2c045-139">**HTTP-beállítása** -ezt a beállítást, amely meghatározza a hello HTTP beállítások toobe hello szabályhoz használt hello beállítás esetén.</span><span class="sxs-lookup"><span data-stu-id="2c045-139">**HTTP setting** - This setting is hello setting that defines hello HTTP settings toobe used for hello rule.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2c045-140">Útvonalak megadása: minták toomatch elérési hello listája.</span><span class="sxs-lookup"><span data-stu-id="2c045-140">Paths: hello list of path patterns toomatch.</span></span> <span data-ttu-id="2c045-141">Minden egyes kell kezdődnie / és hello egyetlen hely, ahol a "\*" engedélyezett hello végén van.</span><span class="sxs-lookup"><span data-stu-id="2c045-141">Each must start with / and hello only place a "\*" is allowed is at hello end.</span></span> <span data-ttu-id="2c045-142">Érvényes többek között az /xyz, /xyz* vagy/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="2c045-142">Valid examples are /xyz, /xyz* or /xyz/*.</span></span>  

![Hozzáadása az elérési út alapú szabály panel adataival kitöltött][2]

<span data-ttu-id="2c045-144">Egy elérési utat alapú szabály tooan hozzáadása meglévő Alkalmazásátjáró az egy könnyen folyamat hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="2c045-144">Adding a path-based rule tooan existing application gateway is an easy process through hello portal.</span></span> <span data-ttu-id="2c045-145">Elérési út alapú szabály létrehozása után célszerű szerkesztett tooadd további szabályok.</span><span class="sxs-lookup"><span data-stu-id="2c045-145">After a path-based rule has been created, it can be edited tooadd additional rules.</span></span> 

![További elérési-alapú szabályok hozzáadása][3]

<span data-ttu-id="2c045-147">Ez a lépés konfigurál egy útvonal-alapú útvonal.</span><span class="sxs-lookup"><span data-stu-id="2c045-147">This step configures a path-based route.</span></span> <span data-ttu-id="2c045-148">Fontos, hogy a rendszer nem újraírja kérelmek toounderstand, mivel kérelmek Alkalmazásátjáró térjen megvizsgálja hello kérés és az alapszintű hello URL-cím mintát küld hello kérelem toohello megfelelő back-end.</span><span class="sxs-lookup"><span data-stu-id="2c045-148">It is important toounderstand that requests are not rewritten, as requests come in application gateway inspects hello request and basic on hello url pattern sends hello request toohello appropriate back-end.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c045-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c045-149">Next steps</span></span>

<span data-ttu-id="2c045-150">Hogyan SSL kiszervezésével az Azure Application Gateway, tooconfigure: toolearn [SSL-kiszervezés konfigurálása](application-gateway-ssl-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2c045-150">toolearn how tooconfigure SSL Offloading with Azure Application Gateway, see [Configure SSL Offload](application-gateway-ssl-portal.md)</span></span>

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
