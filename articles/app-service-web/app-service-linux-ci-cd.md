---
title: "központi telepítése Linux Azure Web Apps aaaContinuous |} Microsoft Docs"
description: "Hogyan toosetup folyamatos üzembe helyezés Linux Azure Web App alkalmazásban."
keywords: az Azure app service, linux, oss, acr
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: f94d837e27605da58428f507ab2b0eb3af3297e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a>Folyamatos üzembe helyezés az Azure Web Apps Linux rendszeren

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Ebben az oktatóanyagban konfigurálása a folyamatos üzembe egy egyéni tároló lemezkép felügyelt [Azure tároló beállításjegyzék](https://azure.microsoft.com/en-us/services/container-registry/) tárházak vagy [Docker Hub](https://hub.docker.com).

## <a name="step-1---sign-in-tooazure"></a>1. lépés – tooAzure bejelentkezés

Az Azure portálon, a http://portal.azure.com toohello bejelentkezés

## <a name="step-2---enable-container-continuous-deployment-feature"></a>2. lépés - engedélyezési tároló folyamatos üzembe helyezés szolgáltatás

Engedélyezheti a hello folyamatos üzembe helyezés funkció használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és hello a következő parancs végrehajtása

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

A hello  **[Azure-portálon](https://portal.azure.com/)**, hello kattintson **App Service** hello lap hello bal oldali lehetőséget.

Kattintson a az alkalmazás, amelyet tooconfigure Docker Hub folyamatos üzembe hello nevére.

A hello **Alkalmazásbeállítások**, hozzáadhat egy alkalmazást nevű beállítása `DOCKER_ENABLE_CI` hello értékű `true`.

![Helyezze be a Alkalmazásbeállítás képe](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a>3. lépés - a Webhook URL-CÍMÉT előkészítése

Ezt úgy szerezheti be hello Webhook URL-cím használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és hello a következő parancs végrehajtása

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

Hello Webhook URL-CÍMÉT, a következő végpont toohave hello kell: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Ezt úgy szerezheti be a `publishingusername` és `publishingpwd` hello webalkalmazás letöltésével közzététele a profil hello Azure-portál használatával.

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a>4. lépés – a webes hook hozzáadása

### <a name="azure-container-registry"></a>Azure Container Registry

A beállításjegyzék portálpanelén kattintson **Webhookok**, hozzon létre egy új webhook kattintva **Hozzáadás**. A hello **webhook létrehozása** panelen adjon a webhook nevét. Hello Webhook URI, meg kell tooprovide hello URL-címet szerzett **3. lépés**

Győződjön meg arról, hogy hello hatókör határozza meg, a tároló képet tartalmazó tárház hello.

![Helyezze be a webhook képe](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Hello lemezkép frissítésekor a lekérdezi hello webalkalmazás frissített automatikusan hello új lemezképpel.

### <a name="docker-hub"></a>Docker központ

A Docker Hub oldalon kattintson **Webhookok**, majd **A WEBHOOK létrehozása**.

![Helyezze be a webhook 1 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

A Webhook URL-CÍMÉT. hello, tooprovide hello URL-címet szerzett kell **3. lépés**

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Hello lemezkép frissítésekor a lekérdezi hello webalkalmazás frissített automatikusan hello új lemezképpel.

## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](./app-service-linux-intro.md)
* [Azure-tárolót beállításjegyzék](https://azure.microsoft.com/en-us/services/container-registry/)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](app-service-linux-using-nodejs-pm2.md)
* [.NET Core Linux Azure Web App használatával](app-service-linux-using-dotnetcore.md)
* [Ruby Linux Azure Web App használatával](app-service-linux-ruby-get-started.md)
* [Hogyan toouse egyéni Docker kép Linux Azure webalkalmazás számára](./app-service-linux-using-custom-docker-image.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](./app-service-linux-faq.md) 
* [Webalkalmazás az Azure CLI 2.0 verziót használja Linux kezelése](./app-service-linux-cli.md)



