---
title: "API-k létrehozása az Azure API Management"
description: "Megtudhatja, hogyan hozza létre és az Azure API Management API-k konfigurálja."
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
ms.openlocfilehash: ab08256fbc3caca05bf23a12016ad2acf4fc7412
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a><span data-ttu-id="e1767-103">API-k létrehozása az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e1767-103">How to create APIs in Azure API Management</span></span>
<span data-ttu-id="e1767-104">Az API Management API ügyfélalkalmazások meghívható műveletkészlet jelöli.</span><span class="sxs-lookup"><span data-stu-id="e1767-104">An API in API Management represents a set of operations that can be invoked by client applications.</span></span> <span data-ttu-id="e1767-105">Új API-k jönnek létre a közzétevő portálon, és majd a kívánt műveletek kerülnek.</span><span class="sxs-lookup"><span data-stu-id="e1767-105">New APIs are created in the publisher portal, and then the desired operations are added.</span></span> <span data-ttu-id="e1767-106">A műveletek kerülnek, az API-t egy termék hozzáadódik és tehetők közzé.</span><span class="sxs-lookup"><span data-stu-id="e1767-106">Once the operations are added, the API is added to a product and can be published.</span></span> <span data-ttu-id="e1767-107">Miután közzétette az API-k, az előfizetett, és a fejlesztők használják.</span><span class="sxs-lookup"><span data-stu-id="e1767-107">Once an API is published, it can be subscribed to and used by developers.</span></span>

<span data-ttu-id="e1767-108">Ez az útmutató bemutatja az első lépés a folyamat: létrehozása és konfigurálása egy új API-t az API Management.</span><span class="sxs-lookup"><span data-stu-id="e1767-108">This guide shows the first step in the process: how to create and configure a new API in API Management.</span></span> <span data-ttu-id="e1767-109">Műveletek hozzáadását és közzététele a termék további információkért lásd: [műveletek felvétele az API-k] [ How to add operations to an API] és [létrehozása és közzététele egy termék][How to create and publish a product].</span><span class="sxs-lookup"><span data-stu-id="e1767-109">For more information on adding operations and publishing a product, see [How to add operations to an API][How to add operations to an API] and [How to create and publish a product][How to create and publish a product].</span></span>

## <span data-ttu-id="e1767-110"><a name="create-new-api"></a>Egy új API-t létrehozni</span><span class="sxs-lookup"><span data-stu-id="e1767-110"><a name="create-new-api"> </a>Create a new API</span></span>
<span data-ttu-id="e1767-111">API-k létrehozása és a közzétevő portálon konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e1767-111">APIs are created and configured in the publisher portal.</span></span> <span data-ttu-id="e1767-112">A közzétevő portál eléréséhez kattintson **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e1767-112">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="e1767-114">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="e1767-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="e1767-115">Kattintson a **API-k** a a **API Management** a bal oldali menüben, majd **API hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e1767-115">Click **APIs** from the **API Management** menu on the left, and then click **add API**.</span></span>

![API-t létrehozni][api-management-create-api]

<span data-ttu-id="e1767-117">Használja a **adja hozzá az új API** ablak konfigurálása az új API-t.</span><span class="sxs-lookup"><span data-stu-id="e1767-117">Use the **Add new API** window to configure the new API.</span></span>

![Új API hozzáadása][api-management-add-new-api]

<span data-ttu-id="e1767-119">A következő mezőket az új API-t konfigurálására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="e1767-119">The following fields are used to configure the new API.</span></span>

* <span data-ttu-id="e1767-120">**Webes API neve** az API-t egyedinek és leírónak elnevezi.</span><span class="sxs-lookup"><span data-stu-id="e1767-120">**Web API name** provides a unique and descriptive name for the API.</span></span> <span data-ttu-id="e1767-121">A fejlesztői és publisher portálon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="e1767-121">It is displayed in the developer and publisher portals.</span></span>
* <span data-ttu-id="e1767-122">**Webszolgáltatás URL-címe** végrehajtási az API-t a HTTP-szolgáltatás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e1767-122">**Web service URL** references the HTTP service implementing the API.</span></span> <span data-ttu-id="e1767-123">Az API management erre a címre kérelmeket továbbítja.</span><span class="sxs-lookup"><span data-stu-id="e1767-123">API management forwards requests to this address.</span></span>
* <span data-ttu-id="e1767-124">**Webes API URL-címe utótag** az alap URL-címet a API management szolgáltatás a rendszer hozzáfűzi.</span><span class="sxs-lookup"><span data-stu-id="e1767-124">**Web API URL suffix** is appended to the base URL for the API management service.</span></span> <span data-ttu-id="e1767-125">Az alap URL-cím esetében gyakori, minden API-kezelés szolgáltatás példánya által üzemeltetett API.</span><span class="sxs-lookup"><span data-stu-id="e1767-125">The base URL is common for all APIs hosted by an API Management service instance.</span></span> <span data-ttu-id="e1767-126">API Management az API-k által az utótag különbözteti meg, és ezért a utótag minden API-hoz. a megadott közzétevő egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e1767-126">API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API for a given publisher.</span></span>
* <span data-ttu-id="e1767-127">**Webes API URL-séma** meghatározza, hogy mely protokollokkal az API eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e1767-127">**Web API URL scheme** determines which protocols can be used to access the API.</span></span> <span data-ttu-id="e1767-128">HTTPs alapértelmezés szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="e1767-128">HTTPs is specified by default.</span></span>
* <span data-ttu-id="e1767-129">Opcionálisan adja hozzá az új API termékre, kattintson a **(nem kötelező) termékek** legördülő listán, és válassza ki a termék.</span><span class="sxs-lookup"><span data-stu-id="e1767-129">To optionally add this new API to a product, click the **Products (optional)** drop-down and choose a product.</span></span> <span data-ttu-id="e1767-130">Ezt a lépést többször hozzáadni az API-t több termék is kell ismételni.</span><span class="sxs-lookup"><span data-stu-id="e1767-130">This step can be repeated multiple times to add the API to multiple products.</span></span>

<span data-ttu-id="e1767-131">Kattintson a kívánt értékeket konfigurálása után **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e1767-131">Once the desired values are configured, click **Save**.</span></span> <span data-ttu-id="e1767-132">Az új API létrehozása után az API-t az összefoglalás lapon megjelenik a közzétevő portálon.</span><span class="sxs-lookup"><span data-stu-id="e1767-132">Once the new API is created, the summary page for the API is displayed in the publisher portal.</span></span>

![API összefoglaló][api-management-api-summary]

## <span data-ttu-id="e1767-134"><a name="configure-api-settings"></a>Konfigurálhatja az API-beállítások</span><span class="sxs-lookup"><span data-stu-id="e1767-134"><a name="configure-api-settings"> </a>Configure API settings</span></span>
<span data-ttu-id="e1767-135">Használhatja a **beállítások** lapon győződjön meg arról, és az API konfigurációjának a szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="e1767-135">You can use the **Settings** tab to verify and edit the configuration for an API.</span></span> <span data-ttu-id="e1767-136">**Webes API neve**, **webszolgáltatás URL-címe**, és **webes API URL-címe utótag** vannak először állítja az API-t jön létre, és módosíthatja itt.</span><span class="sxs-lookup"><span data-stu-id="e1767-136">**Web API name**, **Web service URL**, and **Web API URL suffix** are initially set when the API is created and can be modified here.</span></span> <span data-ttu-id="e1767-137">**Leírás** biztosít az opcionális leírást, és **webes API URL-séma** meghatározza, hogy mely protokollokkal az API eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e1767-137">**Description** provides an optional description, and **Web API URL scheme** determines which protocols can be used to access the API.</span></span>

![Az API-beállítások][api-management-api-settings]

<span data-ttu-id="e1767-139">Az API-t végrehajtási háttérszolgáltatást átjáró hitelesítés konfigurálásához jelölje ki a **biztonsági** fülre.</span><span class="sxs-lookup"><span data-stu-id="e1767-139">To configure gateway authentication for the backend service implementing the API, select the **Security** tab.</span></span> <span data-ttu-id="e1767-140">A **hitelesítő adatokkal rendelkező** legördülő konfigurálásához használható **HTTP basic** vagy **ügyféltanúsítványok** hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="e1767-140">The **With credentials** drop-down can be used to configure **HTTP basic** or **Client certificates** authentication.</span></span> <span data-ttu-id="e1767-141">Egyszerű HTTP-hitelesítést használ, egyszerűen adja meg a kívánt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e1767-141">To use HTTP basic authentication, simply enter the desired credentials.</span></span> <span data-ttu-id="e1767-142">Ügyféltanúsítvány-alapú hitelesítés használatával kapcsolatos információkért lásd: [biztonságossá tétele a háttér-szolgáltatásaihoz ügyfél használatával az Azure API Management tanúsítványhitelesítés][How to secure back-end services using client certificate authentication in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e1767-142">For information on using client certificate authentication, see [How to secure back-end services using client certificate authentication in Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].</span></span>

<span data-ttu-id="e1767-143">A **biztonsági** lapon is használható konfigurálása **felhasználói engedélyezési** az OAuth 2.0 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="e1767-143">The **Security** tab can also be used to configure **User authorization** using OAuth 2.0.</span></span> <span data-ttu-id="e1767-144">További információkért lásd: [hogyan szeretné engedélyekkel felruházni az OAuth 2.0 verziót használja az Azure API Management fejlesztői fiókok][How to authorize developer accounts using OAuth 2.0 in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e1767-144">For more information, see [How to authorize developer accounts using OAuth 2.0 in Azure API Management][How to authorize developer accounts using OAuth 2.0 in Azure API Management].</span></span>

![Egyszerű hitelesítési beállítások][api-management-api-settings-credentials]

<span data-ttu-id="e1767-146">Kattintson a **mentése** az API-beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="e1767-146">Click **Save** to save any changes you make to the API settings.</span></span>

## <span data-ttu-id="e1767-147"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1767-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="e1767-148">Miután az API-k jön létre, és a konfigurált beállítások a következő lépések hozzáadása a műveletek az API-t, adja hozzá az API-t egy termék, és közzéteszi az, hogy elérhető a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="e1767-148">Once an API is created and the settings configured, the next steps are to add the operations to the API, add the API to a product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="e1767-149">További információkért tekintse meg a következő cikkekben talál.</span><span class="sxs-lookup"><span data-stu-id="e1767-149">For more information, see the following articles.</span></span>

* <span data-ttu-id="e1767-150">[Műveletek az API-k hozzáadása][How to add operations to an API]</span><span class="sxs-lookup"><span data-stu-id="e1767-150">[How to add operations to an API][How to add operations to an API]</span></span>
* <span data-ttu-id="e1767-151">[Hogyan hozhat létre, és a termék közzététele][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="e1767-151">[How to create and publish a product][How to create and publish a product]</span></span>

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

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
