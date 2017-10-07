---
title: "aaaDeploying alkalmazások tooAzure App Service"
description: "Ismerje meg, hogyan működnek a tooDeploy alkalmazások tooApp szolgáltatás"
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
ms.openlocfilehash: 925341e12daf3cb05b25199f5c5218e82f062f70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-deployment-overview"></a><span data-ttu-id="5cde6-104">Az Azure App Service telepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="5cde6-104">Azure App Service Deployment Overview</span></span>
<span data-ttu-id="5cde6-105">Az Azure App Service egy gazdag biztosít, és integrált szolgáltatáskészleteket toosupport hatékony és rugalmas telepítési munkafolyamatok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5cde6-105">Azure App Service provides a rich and integrated feature set toosupport creating powerful and flexible deployment workflows.</span></span> <span data-ttu-id="5cde6-106">Alkalmazás központi telepítési beállítások, amelyek tartalmazzák a folyamatos integrációt vagy helyi verziókezelő közzététel, WebDeploy és FTP-használhatják fel.</span><span class="sxs-lookup"><span data-stu-id="5cde6-106">App deployment can leverage options that include continuous integration or local source control publishing, WebDeploy, and FTP.</span></span> <span data-ttu-id="5cde6-107">hello ajánlott módja a termelési alkalmazások telepítését a központi telepítési tárolóhelycsere.</span><span class="sxs-lookup"><span data-stu-id="5cde6-107">hello recommended method for production app deployment is deployment slot swap.</span></span> <span data-ttu-id="5cde6-108">Üzembe helyezési határoz meg az üzemi alkalmazások társított átmeneti és integrációs környezetekben.</span><span class="sxs-lookup"><span data-stu-id="5cde6-108">Deployment slots represent staging and integration environments associated with production apps.</span></span> <span data-ttu-id="5cde6-109">Üzembe helyezési konfigurálhatók és webes forgalom érvényesítéshez kapjon, és forgalom lecserélhető nem működik, amikor a központi telepítés tooproduction igény, és bemelegítési automatikus.</span><span class="sxs-lookup"><span data-stu-id="5cde6-109">Deployment slots can be configured and targeted with web traffic for validation, and traffic can be swapped on demand for deployment tooproduction with no down time and automated warm-up.</span></span> <span data-ttu-id="5cde6-110">telepítési munkafolyamat hello lépésein keresztül kiadás felügyeleti termékek, például a Visual Studio kiadási felügyeleti könnyen automatizálható.</span><span class="sxs-lookup"><span data-stu-id="5cde6-110">hello steps of a deployment workflow can be easily automated via release management products such as Visual Studio Release Management.</span></span> <span data-ttu-id="5cde6-111">Ez akkor hasznos, ha más megoldás erőforrások együtt (pl. az adattároló), ismétlődési és a telepítés több egység közötti replikáció.</span><span class="sxs-lookup"><span data-stu-id="5cde6-111">This is useful for coordination with other solution resources (e.g. data store), recurrence, and replication across multiple units of deployment.</span></span> 

[!INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]

