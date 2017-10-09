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
# <a name="using-webjobs-in-azure-app-service"></a>WebJobs használata az Azure App Service-ben
Ez a cikk kapcsolatos toodocumentation erőforrások hivatkozásait toouse Azure webjobs-feladatok és hello Azure WebJobs SDK-t. Azure webjobs-feladatok, adjon meg egy egyszerűen toorun parancsfájlok vagy programok, a háttér-folyamatok [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Töltse fel, és egy végrehajtható fájl például futtató cmd, bat, exe (.NET) ps1, sh, php, másolása, js és jar. Ezeket a programokat (cron) ütemezés szerint vagy folyamatos webjobs-feladatok futtatása.

hello WebJobs SDK megkönnyíti toouse Azure Storage segítségével. hello WebJobs SDK a kötés és eseményindító rendszert, amely együttműködik a Microsoft Azure Storage Blobs, üzenetsorok és táblák, valamint Service Bus-üzenetsorok rendelkezik.

Létrehozás, telepítés és webjobs-feladatok kezelésére a Visual Studio integrált tooling zökkenőmentes. Webjobs-feladatok létrehozása sablonból, közzététele és kezelése (Futtatás/leállítás/figyelő/debug) őket.

WebJobs-irányítópulttal hello hello Azure-portálon a hatékony funkciókat biztosít, amely a webjobs-feladatok, például hello képességét tooinvoke egyéni függvényei belül webjobs-feladatok végrehajtásának hello teljes szabályozható. hello irányítópult is függvény futtatókörnyezetek és naplózási kimeneti jeleníti meg.

[!INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]

