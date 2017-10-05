---
title: "Az Azure App Service-alkalmazások telepítése"
description: "További tudnivalók az App Service munkahelyi telepítés alkalmazások"
keywords: "App service, azure app service-ben telepíti, a központi telepítés"
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: 
ms.assetid: de12cd6e-e124-4e48-90bc-c3a3801305da
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 347e8b5177eac8e08ab0dea701b736b86d23904a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-deployment-overview"></a><span data-ttu-id="23166-104">Az Azure App Service telepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="23166-104">Azure App Service Deployment Overview</span></span>
<span data-ttu-id="23166-105">Az Azure App Service támogatja a hatékony és rugalmas telepítési munkafolyamatok létrehozása gazdag és integrált funkcióval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="23166-105">Azure App Service provides a rich and integrated feature set to support creating powerful and flexible deployment workflows.</span></span> <span data-ttu-id="23166-106">Alkalmazás központi telepítési beállítások, amelyek tartalmazzák a folyamatos integrációt vagy helyi verziókezelő közzététel, WebDeploy és FTP-használhatják fel.</span><span class="sxs-lookup"><span data-stu-id="23166-106">App deployment can leverage options that include continuous integration or local source control publishing, WebDeploy, and FTP.</span></span> <span data-ttu-id="23166-107">Az ajánlott módszer az alkalmazások telepítésének éles telepítési tárolóhelycsere.</span><span class="sxs-lookup"><span data-stu-id="23166-107">The recommended method for production app deployment is deployment slot swap.</span></span> <span data-ttu-id="23166-108">Üzembe helyezési határoz meg az üzemi alkalmazások társított átmeneti és integrációs környezetekben.</span><span class="sxs-lookup"><span data-stu-id="23166-108">Deployment slots represent staging and integration environments associated with production apps.</span></span> <span data-ttu-id="23166-109">Üzembe helyezési konfigurálhatók és webes forgalom érvényesítéshez kapjon, és lecserélhető az igény szerinti éles nem működik, amikor üzembe helyezése és bemelegítési automatikus.</span><span class="sxs-lookup"><span data-stu-id="23166-109">Deployment slots can be configured and targeted with web traffic for validation, and traffic can be swapped on demand for deployment to production with no down time and automated warm-up.</span></span> <span data-ttu-id="23166-110">A lépéseket a telepítési munkafolyamat kiadás felügyeleti termékek, például a Visual Studio kiadási felügyeleti keresztül egyszerűen automatizálható.</span><span class="sxs-lookup"><span data-stu-id="23166-110">The steps of a deployment workflow can be easily automated via release management products such as Visual Studio Release Management.</span></span> <span data-ttu-id="23166-111">Ez akkor hasznos, ha más megoldás erőforrások együtt (pl. az adattároló), ismétlődési és a telepítés több egység közötti replikáció.</span><span class="sxs-lookup"><span data-stu-id="23166-111">This is useful for coordination with other solution resources (e.g. data store), recurrence, and replication across multiple units of deployment.</span></span> 

[!INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]

