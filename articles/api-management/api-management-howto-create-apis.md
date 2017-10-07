---
title: aaaHow toocreate Azure API Management API-k
description: "Megtudhatja, hogyan toocreate és API-k konfigurálása az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 48ed8d93947253aa1e67ad995927ed6101cac072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-apis-in-azure-api-management"></a><span data-ttu-id="2c3ee-103">Hogyan toocreate Azure API Management API-k</span><span class="sxs-lookup"><span data-stu-id="2c3ee-103">How toocreate APIs in Azure API Management</span></span>
<span data-ttu-id="2c3ee-104">Az API Management API ügyfélalkalmazások meghívható műveletkészlet jelöli.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-104">An API in API Management represents a set of operations that can be invoked by client applications.</span></span> <span data-ttu-id="2c3ee-105">Új API-k jönnek létre hello publisher portálon, és majd hello szükséges műveletek kerülnek.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-105">New APIs are created in hello publisher portal, and then hello desired operations are added.</span></span> <span data-ttu-id="2c3ee-106">Miután hello műveletek kerülnek, a hello API tooa termék kerül, és tehetők közzé.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-106">Once hello operations are added, hello API is added tooa product and can be published.</span></span> <span data-ttu-id="2c3ee-107">Miután közzétette az API-k, azok előfizetett tooand fejlesztők használják.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-107">Once an API is published, it can be subscribed tooand used by developers.</span></span>

<span data-ttu-id="2c3ee-108">Ez az útmutató bemutatja a hello első lépéseként hello folyamat: hogyan toocreate és egy új API-t konfigurálhatja az API Management.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-108">This guide shows hello first step in hello process: how toocreate and configure a new API in API Management.</span></span> <span data-ttu-id="2c3ee-109">Műveletek hozzáadását és közzététele a termék további információkért lásd: [hogyan tooadd műveletek tooan API] [ How tooadd operations tooan API] és [hogyan toocreate és közzététele egy termék] [ How toocreate and publish a product].</span><span class="sxs-lookup"><span data-stu-id="2c3ee-109">For more information on adding operations and publishing a product, see [How tooadd operations tooan API][How tooadd operations tooan API] and [How toocreate and publish a product][How toocreate and publish a product].</span></span>

## <span data-ttu-id="2c3ee-110"><a name="create-new-api"></a>Egy új API-t létrehozni</span><span class="sxs-lookup"><span data-stu-id="2c3ee-110"><a name="create-new-api"> </a>Create a new API</span></span>
<span data-ttu-id="2c3ee-111">API-k létrehozása és hello publisher portal konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-111">APIs are created and configured in hello publisher portal.</span></span> <span data-ttu-id="2c3ee-112">tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-112">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="2c3ee-114">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="2c3ee-115">Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **API hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-115">Click **APIs** from hello **API Management** menu on hello left, and then click **add API**.</span></span>

![API-t létrehozni][api-management-create-api]

<span data-ttu-id="2c3ee-117">Használjon hello **adja hozzá az új API** ablak tooconfigure hello új API-t.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-117">Use hello **Add new API** window tooconfigure hello new API.</span></span>

![Új API hozzáadása][api-management-add-new-api]

<span data-ttu-id="2c3ee-119">a következő mezők hello használt tooconfigure hello új API.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-119">hello following fields are used tooconfigure hello new API.</span></span>

* <span data-ttu-id="2c3ee-120">**Webes API neve** hello API egyedinek és leírónak elnevezi.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-120">**Web API name** provides a unique and descriptive name for hello API.</span></span> <span data-ttu-id="2c3ee-121">Hello fejlesztői és publisher portálon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-121">It is displayed in hello developer and publisher portals.</span></span>
* <span data-ttu-id="2c3ee-122">**Webszolgáltatás URL-címe** hivatkozások hello végrehajtási hello API HTTP-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-122">**Web service URL** references hello HTTP service implementing hello API.</span></span> <span data-ttu-id="2c3ee-123">Az API management toothis cím továbbítja a kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-123">API management forwards requests toothis address.</span></span>
* <span data-ttu-id="2c3ee-124">**Webes API URL-címe utótag** hozzáfűzött toohello alap URL-cím hello API management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-124">**Web API URL suffix** is appended toohello base URL for hello API management service.</span></span> <span data-ttu-id="2c3ee-125">hello alap URL-cím esetében gyakori, az API Management szolgáltatás példánya által üzemeltetett minden API-k.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-125">hello base URL is common for all APIs hosted by an API Management service instance.</span></span> <span data-ttu-id="2c3ee-126">API Management az API-k által az utótag különbözteti meg, és ezért hello utótag minden API-hoz. a megadott közzétevő egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-126">API Management distinguishes APIs by their suffix and therefore hello suffix must be unique for every API for a given publisher.</span></span>
* <span data-ttu-id="2c3ee-127">**Webes API URL-séma** határozza meg, mely protokollok használt tooaccess hello API lehet.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-127">**Web API URL scheme** determines which protocols can be used tooaccess hello API.</span></span> <span data-ttu-id="2c3ee-128">HTTPs alapértelmezés szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-128">HTTPs is specified by default.</span></span>
* <span data-ttu-id="2c3ee-129">toooptionally hozzáadni az új API tooa terméket, kattintson a hello **(nem kötelező) termékek** legördülő listán, és válassza ki a termék.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-129">toooptionally add this new API tooa product, click hello **Products (optional)** drop-down and choose a product.</span></span> <span data-ttu-id="2c3ee-130">Ez a lépés tooadd hello API toomultiple termékek ismételt többször is lehet.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-130">This step can be repeated multiple times tooadd hello API toomultiple products.</span></span>

<span data-ttu-id="2c3ee-131">Amikor hello szükséges értékek vannak konfigurálva, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-131">Once hello desired values are configured, click **Save**.</span></span> <span data-ttu-id="2c3ee-132">Hello új API létrehozása után hello hello API az összefoglalás lapon megjelenik hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-132">Once hello new API is created, hello summary page for hello API is displayed in hello publisher portal.</span></span>

![API összefoglaló][api-management-api-summary]

## <span data-ttu-id="2c3ee-134"><a name="configure-api-settings"></a>Konfigurálhatja az API-beállítások</span><span class="sxs-lookup"><span data-stu-id="2c3ee-134"><a name="configure-api-settings"> </a>Configure API settings</span></span>
<span data-ttu-id="2c3ee-135">Használhatja a hello **beállítások** tooverify lapra, és az API-k a hello konfigurációjának a szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-135">You can use hello **Settings** tab tooverify and edit hello configuration for an API.</span></span> <span data-ttu-id="2c3ee-136">**Webes API neve**, **webszolgáltatás URL-címe**, és **webes API URL-címe utótag** kezdetben állítottak hello API jön létre, és módosíthatja itt.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-136">**Web API name**, **Web service URL**, and **Web API URL suffix** are initially set when hello API is created and can be modified here.</span></span> <span data-ttu-id="2c3ee-137">**Leírás** biztosít az opcionális leírást, és **webes API URL-séma** határozza meg, mely protokollok használt tooaccess hello API lehet.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-137">**Description** provides an optional description, and **Web API URL scheme** determines which protocols can be used tooaccess hello API.</span></span>

![Az API-beállítások][api-management-api-settings]

<span data-ttu-id="2c3ee-139">tooconfigure átjáró hitelesítési hello háttér Service végrehajtási hello API, válassza ki a hello **biztonsági** külön-külön hello **hitelesítő adatokkal rendelkező** legördülő lehet használt tooconfigure **HTTP Alapszintű** vagy **ügyféltanúsítványok** hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-139">tooconfigure gateway authentication for hello backend service implementing hello API, select hello **Security** tab. hello **With credentials** drop-down can be used tooconfigure **HTTP basic** or **Client certificates** authentication.</span></span> <span data-ttu-id="2c3ee-140">toouse HTTP alapszintű hitelesítés, egyszerűen adja meg a hello szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-140">toouse HTTP basic authentication, simply enter hello desired credentials.</span></span> <span data-ttu-id="2c3ee-141">Ügyféltanúsítvány-alapú hitelesítés használatával kapcsolatos információkért lásd: [hogyan tanúsítvány toosecure háttér-szolgáltatásaihoz ügyfél-hitelesítés az Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="2c3ee-141">For information on using client certificate authentication, see [How toosecure back-end services using client certificate authentication in Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].</span></span>

<span data-ttu-id="2c3ee-142">Hello **biztonsági** lapon is használt tooconfigure **felhasználói engedélyezési** az OAuth 2.0 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-142">hello **Security** tab can also be used tooconfigure **User authorization** using OAuth 2.0.</span></span> <span data-ttu-id="2c3ee-143">További információkért lásd: [hogyan tooauthorize fejlesztői fiókok az Azure API Management OAuth 2.0-s][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="2c3ee-143">For more information, see [How tooauthorize developer accounts using OAuth 2.0 in Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].</span></span>

![Egyszerű hitelesítési beállítások][api-management-api-settings-credentials]

<span data-ttu-id="2c3ee-145">Kattintson a **mentése** toosave módosítások toohello API-beállítások.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-145">Click **Save** toosave any changes you make toohello API settings.</span></span>

## <span data-ttu-id="2c3ee-146"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c3ee-146"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="2c3ee-147">Miután az API-k létrehozása, és hello beállításai, hello lépések vannak tooadd hello műveletek toohello API-t hello API tooa termék hozzáadása, és közzéteszi az, hogy elérhető a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-147">Once an API is created and hello settings configured, hello next steps are tooadd hello operations toohello API, add hello API tooa product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="2c3ee-148">További információkért tekintse meg a következő cikkek hello.</span><span class="sxs-lookup"><span data-stu-id="2c3ee-148">For more information, see hello following articles.</span></span>

* <span data-ttu-id="2c3ee-149">[Hogyan tooadd műveletek tooan API][How tooadd operations tooan API]</span><span class="sxs-lookup"><span data-stu-id="2c3ee-149">[How tooadd operations tooan API][How tooadd operations tooan API]</span></span>
* <span data-ttu-id="2c3ee-150">[Hogyan toocreate és a termék közzététele][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="2c3ee-150">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How toosecure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How tooauthorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
