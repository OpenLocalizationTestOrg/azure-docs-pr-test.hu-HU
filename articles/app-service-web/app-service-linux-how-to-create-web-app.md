---
title: "Hozzon létre egy Azure Linux rendszeren futó webalkalmazás |} Microsoft Docs"
description: "Webes alkalmazás létrehozási munkafolyamat Linux Azure webalkalmazás számára."
keywords: "az Azure app service, webalkalmazás, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 49091d4a85bed23927850f9c0bbc5ea8b6e8c9e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a>Hozzon létre egy Azure Linux rendszeren futó webalkalmazás

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-the-azure-portal-to-create-your-web-app"></a>A webalkalmazás létrehozása az Azure-portál használatával
Megkezdheti a webalkalmazás létrehozása a Linux rendszeren a [Azure-portálon](https://portal.azure.com) a következő ábrán látható módon:

![Egy webalkalmazás létrehozása az Azure portálon][1]

Ezt követően a **létrehozás panel** nyitja meg a következő ábrán látható módon:

![A létrehozás panel][2]

1. Adjon meg egy nevet a webalkalmazás.
2. Válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat. (Lásd az elérhető régiók a [korlátozások szakasz](app-service-linux-intro.md).)
3. Válassza ki a meglévő Azure App Service-csomagot, vagy hozzon létre egy újat. (Lásd az App Service-csomag jegyzetének a [korlátozások szakasz](app-service-linux-intro.md).)
4. Válassza ki a használni kívánt alkalmazáscsoportokat. Választhat a különböző verzióiban Node.js, PHP, a .net Core és a Ruby.

Miután létrehozta az alkalmazást, módosíthatja az alkalmazáscsoportokat az alkalmazásbeállítások az alábbi ábrán látható módon:

![Alkalmazásbeállítások][3]

## <a name="deploy-your-web-app"></a>A webalkalmazás telepítése
Kiválasztása **központi telepítési beállítások** a felügyeleti portál felajánlja a lehetőséget az alkalmazás központi telepítéséhez a helyi Git- vagy GitHub-tárház használandó. A további utasításokat hasonlóak a nem Linux webalkalmazás. Az utasítások a [helyi Git-telepítésének](app-service-deploy-local-git.md) vagy [folyamatos üzembe helyezés](app-service-continuous-deployment.md) segítségével telepítse az alkalmazást.

FTP segítségével töltse fel az alkalmazást a webhelyhez. Letölthető az FTP-végpont a webalkalmazás a diagnosztikai naplók szakaszban a következő ábrán látható módon:

![Diagnosztikai naplók][4]

## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](app-service-linux-intro.md)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](app-service-linux-using-nodejs-pm2.md)
* [Ruby használatát az Azure App Service webalkalmazásba Linux rendszeren](app-service-linux-ruby-get-started.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
