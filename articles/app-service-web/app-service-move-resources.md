---
title: "aaaMove webes alkalmazás erőforrások tooanother erőforráscsoport"
description: "Ha át lehet webes alkalmazások és az App Service szolgáltatások egy erőforráscsoport tooanother hello forgatókönyvek ismerteti."
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
ms.openlocfilehash: 931012fee656b7f8a4b2c225c38739a9171d3b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="supported-move-configurations"></a><span data-ttu-id="b8869-103">Támogatott áthelyezés konfigurációk</span><span class="sxs-lookup"><span data-stu-id="b8869-103">Supported Move Configurations</span></span>
<span data-ttu-id="b8869-104">Hello Azure webalkalmazás-erőforrások áthelyezheti [Resource Manager áthelyezése erőforrások API](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b8869-104">You can move Azure Web App resources using hello [Resource Manager Move Resources API](../azure-resource-manager/resource-group-move-resources.md).</span></span>

<span data-ttu-id="b8869-105">Az Azure Web Apps jelenleg áthelyezés forgatókönyvek a következő hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="b8869-105">Azure Web Apps currently supports hello following move scenarios:</span></span>

* <span data-ttu-id="b8869-106">Helyezze át a hello teljes tartalma (a webes alkalmazásokat, a app service-csomagokról és a tanúsítványok) erőforráscsoport tooanother erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b8869-106">Move hello entire contents of a resource group (web apps, app service plans, and certificates) tooanother resource group.</span></span> 
   > [!Note]
   > <span data-ttu-id="b8869-107">hello célként megadott erőforráscsoport nem ebben a forgatókönyvben Microsoft.Web erőforrásokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b8869-107">hello destination resource group can not contain any Microsoft.Web resources in this scenario.</span></span>

* <span data-ttu-id="b8869-108">Helyezze át a egyedi web apps tooa eltérő erőforráscsoportban, azokat továbbra is az az aktuális app service-csomag (hello app service csomag marad hello régi az erőforráscsoportban) futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="b8869-108">Move individual web apps tooa different resource group, while still hosting them in their current app service plan (hello app service plan stays in hello old resource group).</span></span>


