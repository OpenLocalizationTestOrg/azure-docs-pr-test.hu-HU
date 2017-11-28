---
title: az Azure API Management az API-k aaaImport |} Microsoft Docs
description: "Megtudhatja, hogyan tooimport az API-k és az Azure API Management azokat a műveleteket."
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
ms.openlocfilehash: 20fbbb53243aecc24d72833ec0904ae8fab97863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-hello-definition-of-an-api-with-operations-in-azure-api-management"></a><span data-ttu-id="be765-103">Hogyan tooimport hello műveleteket az Azure API Management az API-k meghatározása</span><span class="sxs-lookup"><span data-stu-id="be765-103">How tooimport hello definition of an API with operations in Azure API Management</span></span>
<span data-ttu-id="be765-104">Az API Management hozhatók létre új API-k, és manuálisan hozzáadni hello műveleteket vagy a hello API együtt egy lépésben hello műveletek importálhatók.</span><span class="sxs-lookup"><span data-stu-id="be765-104">In API Management, new APIs can be created and hello operations added manually, or hello API can be imported along with hello operations in one step.</span></span>

<span data-ttu-id="be765-105">API-k és a műveletek a következő formátumok hello segítségével lehet importálni.</span><span class="sxs-lookup"><span data-stu-id="be765-105">APIs and their operations can be imported using hello following formats.</span></span>

* <span data-ttu-id="be765-106">WADL</span><span class="sxs-lookup"><span data-stu-id="be765-106">WADL</span></span>
* <span data-ttu-id="be765-107">Swagger</span><span class="sxs-lookup"><span data-stu-id="be765-107">Swagger</span></span>

<span data-ttu-id="be765-108">Ez az útmutató azt mutatja be hogyan hozzon létre egy új API-t, és importálni egy lépésben műveletei.</span><span class="sxs-lookup"><span data-stu-id="be765-108">This guide shows how create a new API and import its operations in one step.</span></span> <span data-ttu-id="be765-109">Létre manuálisan az API-k és műveletek hozzáadásával kapcsolatos tudnivalókat lásd: [hogyan toocreate API-k] [ How toocreate APIs] és [hogyan tooadd műveletek tooan API] [ How tooadd operations tooan API].</span><span class="sxs-lookup"><span data-stu-id="be765-109">For information on manually creating an API and adding operations, see [How toocreate APIs][How toocreate APIs] and [How tooadd operations tooan API][How tooadd operations tooan API].</span></span>

## <span data-ttu-id="be765-110"><a name="import-api"></a>API importálása</span><span class="sxs-lookup"><span data-stu-id="be765-110"><a name="import-api"> </a>Import an API</span></span>
<span data-ttu-id="be765-111">API-k létrehozása és hello publisher portal konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="be765-111">APIs are created and configured in hello publisher portal.</span></span> <span data-ttu-id="be765-112">tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="be765-112">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="be765-113">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="be765-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="be765-115">Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **API importálása**.</span><span class="sxs-lookup"><span data-stu-id="be765-115">Click **APIs** from hello **API Management** menu on hello left, and then click **import API**.</span></span>

![Importálási API][api-management-import-apis]

<span data-ttu-id="be765-117">Hello **importálási API** ablak esetében, amelyek megfelelnek a toohello három módon tooprovide hello API specification három lappal.</span><span class="sxs-lookup"><span data-stu-id="be765-117">hello **Import API** window has three tabs that correspond toohello three ways tooprovide hello API specification.</span></span>

* <span data-ttu-id="be765-118">**A vágólapról** lehetővé teszi a hello kijelölt szövegmezőbe toopaste hello API megadását.</span><span class="sxs-lookup"><span data-stu-id="be765-118">**From clipboard** allows you toopaste hello API specification into hello designated text box.</span></span>
* <span data-ttu-id="be765-119">**A fájl** lehetővé teszi a toobrowse tooand válassza hello fájl hello API-meghatározást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="be765-119">**From file** allows you toobrowse tooand select hello file that contains hello API specification.</span></span>
* <span data-ttu-id="be765-120">**URL-címről** toosupply hello URL-cím toohello előírása hello API lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="be765-120">**From URL** allows you toosupply hello URL toohello specification for hello API.</span></span>

![Importálási API formátum][api-management-import-api-clipboard]

<span data-ttu-id="be765-122">Miután megadta a hello API megadását, hello jobb tooindicate hello meghatározás formátuma hello választógombok használatát.</span><span class="sxs-lookup"><span data-stu-id="be765-122">After providing hello API specification, use hello radio buttons on hello right tooindicate hello specification format.</span></span> <span data-ttu-id="be765-123">a következő formátumok hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="be765-123">hello following formats are supported.</span></span>

* <span data-ttu-id="be765-124">WADL</span><span class="sxs-lookup"><span data-stu-id="be765-124">WADL</span></span>
* <span data-ttu-id="be765-125">Swagger</span><span class="sxs-lookup"><span data-stu-id="be765-125">Swagger</span></span>

<span data-ttu-id="be765-126">Ezután adja meg a **webes API URL-címe utótag**.</span><span class="sxs-lookup"><span data-stu-id="be765-126">Next, enter a **Web API URL suffix**.</span></span> <span data-ttu-id="be765-127">Ez a hozzáfűzött toohello alap URL-címet a API management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="be765-127">This is appended toohello base URL for your API management service.</span></span> <span data-ttu-id="be765-128">hello alap URL-cím esetében gyakori, minden olyan API-kezelés szolgáltatás minden példányának üzemeltetett API.</span><span class="sxs-lookup"><span data-stu-id="be765-128">hello base URL is common for all APIs hosted on each instance of an API Management service.</span></span> <span data-ttu-id="be765-129">API Management az API-k által az utótag különbözteti meg, és ezért hello utótag minden API-nak bizonyos API management szolgáltatás példányainak egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="be765-129">API Management distinguishes APIs by their suffix and therefore hello suffix must be unique for every API in a specific API management service instance.</span></span>

<span data-ttu-id="be765-130">Amennyiben az összes érték lett megadva, kattintson a **mentése** toocreate hello API és hello kapcsolódó műveletek.</span><span class="sxs-lookup"><span data-stu-id="be765-130">Once all values are entered, click **Save** toocreate hello API and hello associated operations.</span></span> 

> [!NOTE]
> <span data-ttu-id="be765-131">Tekintse meg a az oktatóanyagban Swagger formátumú API egyszerű számológép importálására [kezelése az Azure API Management az első API](api-management-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="be765-131">For a tutorial of importing a basic calculator API in Swagger format, see [Manage your first API in Azure API Management](api-management-get-started.md).</span></span>
> 
> 

## <span data-ttu-id="be765-132"><a name="export-api"></a> Exportálása az API-k</span><span class="sxs-lookup"><span data-stu-id="be765-132"><a name="export-api"> </a> Export an API</span></span>
<span data-ttu-id="be765-133">Továbbá tooimporting az új API-k, exportálhatja az API-k meghatározását hello hello publisher portálról.</span><span class="sxs-lookup"><span data-stu-id="be765-133">In addition tooimporting new APIs, you can export hello definitions of your APIs from hello publisher portal.</span></span> <span data-ttu-id="be765-134">toodo kattintson **exportálása API** a hello **összefoglaló lapon** , a **API**.</span><span class="sxs-lookup"><span data-stu-id="be765-134">toodo so, click **Export API** from hello **Summary tab** of your **API**.</span></span>

![Exportálás API][api-management-export-api]

<span data-ttu-id="be765-136">WADL vagy a Swagger API-k lehet exportálni.</span><span class="sxs-lookup"><span data-stu-id="be765-136">APIs can be exported using WADL or Swagger.</span></span> <span data-ttu-id="be765-137">Jelölje ki azt a kívánt formátum hello **mentése**, és válassza ki a hello helyet mely toosave hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="be765-137">Select hello desired format, click **Save**, and choose hello location in which toosave hello file.</span></span>

![API-t az exportálási formátumát][api-management-export-api-format]

## <span data-ttu-id="be765-139"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be765-139"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="be765-140">Miután az API-k létrehozása, és hello műveletek importált, megtekintheti és konfigurálhatja a esetleges egyéb beállításokat, hello API tooa termék hozzáadása, és közzéteszi az, hogy elérhető a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="be765-140">Once an API is created and hello operations imported, you can review and configure any additional settings, add hello API tooa Product, and publish it so that it is available for developers.</span></span> <span data-ttu-id="be765-141">További információkért tekintse meg a következő útmutatók hello.</span><span class="sxs-lookup"><span data-stu-id="be765-141">For more information, see hello following guides.</span></span>

* <span data-ttu-id="be765-142">[Hogyan tooconfigure API-beállítások][How tooconfigure API settings]</span><span class="sxs-lookup"><span data-stu-id="be765-142">[How tooconfigure API settings][How tooconfigure API settings]</span></span>
* <span data-ttu-id="be765-143">[Hogyan toocreate és a termék közzététele][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="be765-143">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate APIs]: api-management-howto-create-apis.md
[How tooconfigure API settings]: api-management-howto-create-apis.md#configure-api-settings
