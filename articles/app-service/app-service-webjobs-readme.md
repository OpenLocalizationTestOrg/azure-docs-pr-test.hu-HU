---
title: Webjobs-feladatok az Azure App Service-ben
description: "Ismerje meg, hogyan hozhat létre a webjobs-feladatok a háttérben tesztek futtatása, szolgáltatásokat, mint a tárolás és a Service Bus kezelhet, és hozzon létre ütemezett feladatot."
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
ms.openlocfilehash: 1ca6d2eabe9781a8bb09fc5948ed306e3e8b013c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-webjobs-in-azure-app-service"></a><span data-ttu-id="a72be-103">WebJobs használata az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="a72be-103">Using WebJobs in Azure App Service</span></span>
<span data-ttu-id="a72be-104">Ez a cikk a dokumentációs forrásokat Azure webjobs-feladatok és az Azure WebJobs SDK használatával kapcsolatos mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a72be-104">This article links to documentation resources about how to use Azure WebJobs and the Azure WebJobs SDK.</span></span> <span data-ttu-id="a72be-105">Azure webjobs-feladatok futtatásához programok és parancsfájlok háttérfolyamatként könnyű megoldást biztosítson a [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="a72be-105">Azure WebJobs provide an easy way to run scripts or programs as background processes on [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="a72be-106">Töltse fel, és egy végrehajtható fájl például futtató cmd, bat, exe (.NET) ps1, sh, php, másolása, js és jar.</span><span class="sxs-lookup"><span data-stu-id="a72be-106">You can upload and run an executable file such as as cmd, bat, exe (.NET), ps1, sh, php, py, js and jar.</span></span> <span data-ttu-id="a72be-107">Ezeket a programokat (cron) ütemezés szerint vagy folyamatos webjobs-feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="a72be-107">These programs run as WebJobs on a schedule (cron) or continuously.</span></span>

<span data-ttu-id="a72be-108">A WebJobs SDK megkönnyíti az Azure Storage használata.</span><span class="sxs-lookup"><span data-stu-id="a72be-108">The WebJobs SDK makes it easier to use Azure Storage.</span></span> <span data-ttu-id="a72be-109">A WebJobs SDK a kötés és eseményindító rendszert, amely együttműködik a Microsoft Azure Storage Blobs, üzenetsorok és táblák, valamint Service Bus-üzenetsorok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a72be-109">The WebJobs SDK has a binding and trigger system which works with Microsoft Azure Storage Blobs, Queues and Tables as well as Service Bus Queues.</span></span>

<span data-ttu-id="a72be-110">Létrehozás, telepítés és webjobs-feladatok kezelésére a Visual Studio integrált tooling zökkenőmentes.</span><span class="sxs-lookup"><span data-stu-id="a72be-110">Creating, deploying, and managing WebJobs is seamless with integrated tooling in Visual Studio.</span></span> <span data-ttu-id="a72be-111">Webjobs-feladatok létrehozása sablonból, közzététele és kezelése (Futtatás/leállítás/figyelő/debug) őket.</span><span class="sxs-lookup"><span data-stu-id="a72be-111">You can create WebJobs from templates, publish, and manage (run/stop/monitor/debug) them.</span></span>

<span data-ttu-id="a72be-112">A WebJobs-irányítópulttal az Azure portálon hatékony funkciókat biztosít, amely a webjobs-feladatok, például a webjobs-feladatok belül egyedi függvények meghívása végrehajtásának teljes szabályozható.</span><span class="sxs-lookup"><span data-stu-id="a72be-112">The WebJobs dashboard in the Azure portal provides powerful management capabilities that give you full control over the execution of WebJobs, including the ability to invoke individual functions within WebJobs.</span></span> <span data-ttu-id="a72be-113">Az irányítópult is függvény futtatókörnyezetek és naplózási kimeneti jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a72be-113">The dashboard also displays function runtimes and logging output.</span></span>

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

