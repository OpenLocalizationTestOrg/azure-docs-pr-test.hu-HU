---
title: "Az API-k importálnia kell az Azure API Management |} Microsoft Docs"
description: "Útmutató: Azure API Management az API-k és műveletei importálja."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c851b88fc1067e65044266d07775717c028e75d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a><span data-ttu-id="64f1b-103">Egy API-t az Azure API Management műveletek definíciójának importálása</span><span class="sxs-lookup"><span data-stu-id="64f1b-103">How to import the definition of an API with operations in Azure API Management</span></span>
<span data-ttu-id="64f1b-104">Az API Management hozhatók létre új API-k, és a műveletek manuálisan hozzáadni, vagy az API-t és a műveletek egy lépésben importálhatók.</span><span class="sxs-lookup"><span data-stu-id="64f1b-104">In API Management, new APIs can be created and the operations added manually, or the API can be imported along with the operations in one step.</span></span>

<span data-ttu-id="64f1b-105">API-k és a műveletek importálhatók a következő formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="64f1b-105">APIs and their operations can be imported using the following formats.</span></span>

* <span data-ttu-id="64f1b-106">WADL</span><span class="sxs-lookup"><span data-stu-id="64f1b-106">WADL</span></span>
* <span data-ttu-id="64f1b-107">Swagger</span><span class="sxs-lookup"><span data-stu-id="64f1b-107">Swagger</span></span>

<span data-ttu-id="64f1b-108">Ez az útmutató azt mutatja be hogyan hozzon létre egy új API-t, és importálni egy lépésben műveletei.</span><span class="sxs-lookup"><span data-stu-id="64f1b-108">This guide shows how create a new API and import its operations in one step.</span></span> <span data-ttu-id="64f1b-109">Létre manuálisan az API-k és műveletek hozzáadásával kapcsolatos tudnivalókat lásd: [API-k létrehozása] [ How to create APIs] és [műveletek felvétele az API-k][How to add operations to an API].</span><span class="sxs-lookup"><span data-stu-id="64f1b-109">For information on manually creating an API and adding operations, see [How to create APIs][How to create APIs] and [How to add operations to an API][How to add operations to an API].</span></span>

## <span data-ttu-id="64f1b-110"><a name="import-api"> </a>API importálása</span><span class="sxs-lookup"><span data-stu-id="64f1b-110"><a name="import-api"> </a>Import an API</span></span>
<span data-ttu-id="64f1b-111">API-k létrehozása és a közzétevő portálon konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="64f1b-111">APIs are created and configured in the publisher portal.</span></span> <span data-ttu-id="64f1b-112">A közzétevő portál eléréséhez kattintson **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="64f1b-112">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="64f1b-113">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="64f1b-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="64f1b-115">Kattintson a **API-k** a a **API Management** a bal oldali menüben, majd **API importálása**.</span><span class="sxs-lookup"><span data-stu-id="64f1b-115">Click **APIs** from the **API Management** menu on the left, and then click **import API**.</span></span>

![Importálási API][api-management-import-apis]

<span data-ttu-id="64f1b-117">A **importálási API** ablak esetében a három módszerrel adhat meg az API-specifikációnak megfelelő három lappal.</span><span class="sxs-lookup"><span data-stu-id="64f1b-117">The **Import API** window has three tabs that correspond to the three ways to provide the API specification.</span></span>

* <span data-ttu-id="64f1b-118">**A vágólapról** lehetővé teszi az API-specifikációnak illessze be a kijelölt szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="64f1b-118">**From clipboard** allows you to paste the API specification into the designated text box.</span></span>
* <span data-ttu-id="64f1b-119">**A fájl** lehetővé teszi, hogy keresse meg és jelölje ki a fájlt, amely tartalmazza az API-specifikációnak.</span><span class="sxs-lookup"><span data-stu-id="64f1b-119">**From file** allows you to browse to and select the file that contains the API specification.</span></span>
* <span data-ttu-id="64f1b-120">**URL-címről** lehetővé teszi az API-specifikációnak URL-címét.</span><span class="sxs-lookup"><span data-stu-id="64f1b-120">**From URL** allows you to supply the URL to the specification for the API.</span></span>

![Importálási API formátum][api-management-import-api-clipboard]

<span data-ttu-id="64f1b-122">Miután megadta az API-specifikációnak, a jobb oldalon a megfelelő választógomb segítségével jelzi a meghatározás formátuma.</span><span class="sxs-lookup"><span data-stu-id="64f1b-122">After providing the API specification, use the radio buttons on the right to indicate the specification format.</span></span> <span data-ttu-id="64f1b-123">A következő formátum támogatott.</span><span class="sxs-lookup"><span data-stu-id="64f1b-123">The following formats are supported.</span></span>

* <span data-ttu-id="64f1b-124">WADL</span><span class="sxs-lookup"><span data-stu-id="64f1b-124">WADL</span></span>
* <span data-ttu-id="64f1b-125">Swagger</span><span class="sxs-lookup"><span data-stu-id="64f1b-125">Swagger</span></span>

<span data-ttu-id="64f1b-126">Ezután adja meg a **webes API URL-címe utótag**.</span><span class="sxs-lookup"><span data-stu-id="64f1b-126">Next, enter a **Web API URL suffix**.</span></span> <span data-ttu-id="64f1b-127">A rendszer hozzáfűzi az alap URL-címet a API management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="64f1b-127">This is appended to the base URL for your API management service.</span></span> <span data-ttu-id="64f1b-128">Az alap URL-cím esetében gyakori, az API Management szolgáltatás minden példányának futó minden API.</span><span class="sxs-lookup"><span data-stu-id="64f1b-128">The base URL is common for all APIs hosted on each instance of an API Management service.</span></span> <span data-ttu-id="64f1b-129">API Management az API-k által az utótag különbözteti meg, és ezért a utótag minden API-nak bizonyos API management szolgáltatás példányainak egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="64f1b-129">API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API in a specific API management service instance.</span></span>

<span data-ttu-id="64f1b-130">Amennyiben az összes érték lett megadva, kattintson a **mentése** az API-t és a kapcsolódó műveleteket létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="64f1b-130">Once all values are entered, click **Save** to create the API and the associated operations.</span></span> 

> [!NOTE]
> <span data-ttu-id="64f1b-131">Tekintse meg a az oktatóanyagban Swagger formátumú API egyszerű számológép importálására [kezelése az Azure API Management az első API](api-management-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="64f1b-131">For a tutorial of importing a basic calculator API in Swagger format, see [Manage your first API in Azure API Management](api-management-get-started.md).</span></span>
> 
> 

## <span data-ttu-id="64f1b-132"><a name="export-api"></a> Exportálása az API-k</span><span class="sxs-lookup"><span data-stu-id="64f1b-132"><a name="export-api"> </a> Export an API</span></span>
<span data-ttu-id="64f1b-133">Is importálja az új API-k, exportálhatja az API-k meghatározását a közzétevő portálról.</span><span class="sxs-lookup"><span data-stu-id="64f1b-133">In addition to importing new APIs, you can export the definitions of your APIs from the publisher portal.</span></span> <span data-ttu-id="64f1b-134">Ehhez kattintson **exportálása API** a a **összefoglaló lapon** , a **API**.</span><span class="sxs-lookup"><span data-stu-id="64f1b-134">To do so, click **Export API** from the **Summary tab** of your **API**.</span></span>

![Exportálás API][api-management-export-api]

<span data-ttu-id="64f1b-136">WADL vagy a Swagger API-k lehet exportálni.</span><span class="sxs-lookup"><span data-stu-id="64f1b-136">APIs can be exported using WADL or Swagger.</span></span> <span data-ttu-id="64f1b-137">Válassza ki a kívánt formátumot, kattintson **mentése**, és válassza ki a helyet, ahová menteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="64f1b-137">Select the desired format, click **Save**, and choose the location in which to save the file.</span></span>

![API-t az exportálási formátumát][api-management-export-api-format]

## <span data-ttu-id="64f1b-139"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64f1b-139"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="64f1b-140">Után az API-k jön létre, és a műveletek importált, megtekintheti, és konfigurálja az esetleges egyéb beállításokat, az API-t vegye fel a termék, és közzéteszi az, hogy elérhető a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="64f1b-140">Once an API is created and the operations imported, you can review and configure any additional settings, add the API to a Product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="64f1b-141">További információkért tekintse meg az alábbi útmutatókban.</span><span class="sxs-lookup"><span data-stu-id="64f1b-141">For more information, see the following guides.</span></span>

* <span data-ttu-id="64f1b-142">[Az API-beállítások konfigurálása][How to configure API settings]</span><span class="sxs-lookup"><span data-stu-id="64f1b-142">[How to configure API settings][How to configure API settings]</span></span>
* <span data-ttu-id="64f1b-143">[Hogyan hozhat létre, és a termék közzététele][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="64f1b-143">[How to create and publish a product][How to create and publish a product]</span></span>

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
