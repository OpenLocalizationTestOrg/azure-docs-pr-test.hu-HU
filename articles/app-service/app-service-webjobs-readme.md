---
title: az Azure App Service aaaWebJobs
description: "Ismerje meg, hogyan toobuild WebJobs toorun háttér teszteli, szolgáltatásokat, mint a tárolás és a Service Bus kezelhet, és hozzon létre ütemezett feladatot."
services: app-service
documentationcenter: 
author: christopheranderson
manager: erikre
editor: mollybos
ms.assetid: 85975432-04c9-4b83-b937-b30c082d52a1
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2015
ms.author: chrande
ms.openlocfilehash: 25c24bfe71a64036cd48e58f471995b4a06e3b33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-webjobs-in-azure-app-service"></a><span data-ttu-id="6313c-103">WebJobs használata az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="6313c-103">Using WebJobs in Azure App Service</span></span>
<span data-ttu-id="6313c-104">Ez a cikk kapcsolatos toodocumentation erőforrások hivatkozásait toouse Azure webjobs-feladatok és hello Azure WebJobs SDK-t.</span><span class="sxs-lookup"><span data-stu-id="6313c-104">This article links toodocumentation resources about how toouse Azure WebJobs and hello Azure WebJobs SDK.</span></span> <span data-ttu-id="6313c-105">Azure webjobs-feladatok, adjon meg egy egyszerűen toorun parancsfájlok vagy programok, a háttér-folyamatok [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="6313c-105">Azure WebJobs provide an easy way toorun scripts or programs as background processes on [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="6313c-106">Töltse fel, és egy végrehajtható fájl például futtató cmd, bat, exe (.NET) ps1, sh, php, másolása, js és jar.</span><span class="sxs-lookup"><span data-stu-id="6313c-106">You can upload and run an executable file such as as cmd, bat, exe (.NET), ps1, sh, php, py, js and jar.</span></span> <span data-ttu-id="6313c-107">Ezeket a programokat (cron) ütemezés szerint vagy folyamatos webjobs-feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="6313c-107">These programs run as WebJobs on a schedule (cron) or continuously.</span></span>

<span data-ttu-id="6313c-108">hello WebJobs SDK megkönnyíti toouse Azure Storage segítségével.</span><span class="sxs-lookup"><span data-stu-id="6313c-108">hello WebJobs SDK makes it easier toouse Azure Storage.</span></span> <span data-ttu-id="6313c-109">hello WebJobs SDK a kötés és eseményindító rendszert, amely együttműködik a Microsoft Azure Storage Blobs, üzenetsorok és táblák, valamint Service Bus-üzenetsorok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6313c-109">hello WebJobs SDK has a binding and trigger system which works with Microsoft Azure Storage Blobs, Queues and Tables as well as Service Bus Queues.</span></span>

<span data-ttu-id="6313c-110">Létrehozás, telepítés és webjobs-feladatok kezelésére a Visual Studio integrált tooling zökkenőmentes.</span><span class="sxs-lookup"><span data-stu-id="6313c-110">Creating, deploying, and managing WebJobs is seamless with integrated tooling in Visual Studio.</span></span> <span data-ttu-id="6313c-111">Webjobs-feladatok létrehozása sablonból, közzététele és kezelése (Futtatás/leállítás/figyelő/debug) őket.</span><span class="sxs-lookup"><span data-stu-id="6313c-111">You can create WebJobs from templates, publish, and manage (run/stop/monitor/debug) them.</span></span>

<span data-ttu-id="6313c-112">WebJobs-irányítópulttal hello hello Azure-portálon a hatékony funkciókat biztosít, amely a webjobs-feladatok, például hello képességét tooinvoke egyéni függvényei belül webjobs-feladatok végrehajtásának hello teljes szabályozható.</span><span class="sxs-lookup"><span data-stu-id="6313c-112">hello WebJobs dashboard in hello Azure portal provides powerful management capabilities that give you full control over hello execution of WebJobs, including hello ability tooinvoke individual functions within WebJobs.</span></span> <span data-ttu-id="6313c-113">hello irányítópult is függvény futtatókörnyezetek és naplózási kimeneti jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6313c-113">hello dashboard also displays function runtimes and logging output.</span></span>

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

