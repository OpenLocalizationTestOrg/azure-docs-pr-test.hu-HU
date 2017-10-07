---
title: "Linux rendszerű aaaCreate egy Azure webalkalmazás |} Microsoft Docs"
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
ms.openlocfilehash: de1bd030345d5e2a8024012067b5bcaa2cca09dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a>Hozzon létre egy Azure Linux rendszeren futó webalkalmazás

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-hello-azure-portal-toocreate-your-web-app"></a>Használja az Azure portál toocreate hello a webalkalmazás
Megkezdheti a webalkalmazás létrehozása Linux hello [Azure-portálon](https://portal.azure.com) látható hello kép a következő módon:

![Egy webalkalmazás létrehozása a hello Azure-portálon][1]

A következő hello **létrehozás panel** megnyílik, ahogy az a következő kép hello:

![hello létrehozás panel][2]

1. Adjon meg egy nevet a webalkalmazás.
2. Válasszon egy meglévő erőforráscsoportot, vagy hozzon létre egy újat. (Lásd a hello elérhető régiók [korlátozások szakasz](app-service-linux-intro.md).)
3. Válassza ki a meglévő Azure App Service-csomagot, vagy hozzon létre egy újat. (Lásd az App Service csomag a hello [korlátozások szakasz](app-service-linux-intro.md).)
4. Válassza ki a hello alkalmazás, hogy szeretné-e toouse verem. Választhat a különböző verzióiban Node.js, PHP, a .net Core és a Ruby.

Hello alkalmazás létrehozását követően módosíthatja hello alkalmazáscsoportokat hello Alkalmazásbeállítások látható hello kép a következő módon:

![Alkalmazásbeállítások][3]

## <a name="deploy-your-web-app"></a>A webalkalmazás telepítése
Kiválasztása **központi telepítési beállítások** hello felügyeleti portál által biztosított az Ön hello beállítás toouse helyi Git- vagy GitHub tárház toodeploy az alkalmazást. hello részeinek hello utasításokat a nem Linux webalkalmazás hasonló toothose. Hajtsa végre az hello utasításait [helyi Git-telepítésének](app-service-deploy-local-git.md) vagy [folyamatos üzembe helyezés](app-service-continuous-deployment.md) toodeploy az alkalmazást.

FTP-tooupload az alkalmazás tooyour hely is használja. Letölthető hello FTP végpont a webalkalmazás hello diagnosztikai naplók szakasz látható hello kép a következő módon:

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
