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
# <a name="using-webjobs-in-azure-app-service"></a>WebJobs használata az Azure App Service-ben
Ez a cikk a dokumentációs forrásokat Azure webjobs-feladatok és az Azure WebJobs SDK használatával kapcsolatos mutató hivatkozásokat tartalmaz. Azure webjobs-feladatok futtatásához programok és parancsfájlok háttérfolyamatként könnyű megoldást biztosítson a [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Töltse fel, és egy végrehajtható fájl például futtató cmd, bat, exe (.NET) ps1, sh, php, másolása, js és jar. Ezeket a programokat (cron) ütemezés szerint vagy folyamatos webjobs-feladatok futtatása.

A WebJobs SDK megkönnyíti az Azure Storage használata. A WebJobs SDK a kötés és eseményindító rendszert, amely együttműködik a Microsoft Azure Storage Blobs, üzenetsorok és táblák, valamint Service Bus-üzenetsorok rendelkezik.

Létrehozás, telepítés és webjobs-feladatok kezelésére a Visual Studio integrált tooling zökkenőmentes. Webjobs-feladatok létrehozása sablonból, közzététele és kezelése (Futtatás/leállítás/figyelő/debug) őket.

A WebJobs-irányítópulttal az Azure portálon hatékony funkciókat biztosít, amely a webjobs-feladatok, például a webjobs-feladatok belül egyedi függvények meghívása végrehajtásának teljes szabályozható. Az irányítópult is függvény futtatókörnyezetek és naplózási kimeneti jeleníti meg.

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

