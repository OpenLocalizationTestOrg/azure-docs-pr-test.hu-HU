---
title: "Webes alkalmazás erőforrások áthelyezése egy másik erőforráscsoportban"
description: "Ez a témakör a ahol áthelyezheti webes alkalmazások és az App Service szolgáltatások az egyik erőforráscsoportból a másikba."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 1b5059dc052005b6079f70ecf6771a3771df8d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="supported-move-configurations"></a><span data-ttu-id="00627-103">Támogatott áthelyezés konfigurációk</span><span class="sxs-lookup"><span data-stu-id="00627-103">Supported Move Configurations</span></span>
<span data-ttu-id="00627-104">Azure-webalkalmazás-erőforrások áthelyezheti a [Resource Manager áthelyezése erőforrások API](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="00627-104">You can move Azure Web App resources using the [Resource Manager Move Resources API](../azure-resource-manager/resource-group-move-resources.md).</span></span>

<span data-ttu-id="00627-105">Az Azure Web Apps jelenleg a következő move-forgatókönyveket teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="00627-105">Azure Web Apps currently supports the following move scenarios:</span></span>

* <span data-ttu-id="00627-106">A teljes tartalma (a webes alkalmazásokat, a app service-csomagokról és a tanúsítványok) erőforráscsoport áthelyezése egy másik erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="00627-106">Move the entire contents of a resource group (web apps, app service plans, and certificates) to another resource group.</span></span> 
   > [!Note]
   > <span data-ttu-id="00627-107">A célként megadott erőforráscsoport nem ebben a forgatókönyvben Microsoft.Web erőforrásokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="00627-107">The destination resource group can not contain any Microsoft.Web resources in this scenario.</span></span>

* <span data-ttu-id="00627-108">Egy másik erőforráscsoportban található egyes webalkalmazások helyezhetik át, miközben továbbra is az az aktuális app service-csomag (régi az erőforráscsoportban marad az app service-csomag) futtató őket.</span><span class="sxs-lookup"><span data-stu-id="00627-108">Move individual web apps to a different resource group, while still hosting them in their current app service plan (the app service plan stays in the old resource group).</span></span>


