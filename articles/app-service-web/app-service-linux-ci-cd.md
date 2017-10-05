---
title: "Folyamatos üzembe helyezés az Azure-webalkalmazásban Linux |} Microsoft Docs"
description: "A telepítő a folyamatos üzembe helyezés az Azure Web Apps Linux módjáról."
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
ms.openlocfilehash: f8f7d51003f8a55b7f51e8cc2cea838e8e5a6196
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a>Folyamatos üzembe helyezés az Azure Web Apps Linux rendszeren

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Ebben az oktatóanyagban konfigurálása a folyamatos üzembe egy egyéni tároló lemezkép felügyelt [Azure tároló beállításjegyzék](https://azure.microsoft.com/en-us/services/container-registry/) tárházak vagy [Docker Hub](https://hub.docker.com).

## <a name="step-1---sign-in-to-azure"></a>1. lépés – bejelentkezés az Azure-bA

Jelentkezzen be az Azure portálon, a http://portal.azure.com

## <a name="step-2---enable-container-continuous-deployment-feature"></a>2. lépés - engedélyezési tároló folyamatos üzembe helyezés szolgáltatás

Engedélyezheti a folyamatos üzembe helyezés funkció használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és a következő parancs végrehajtása

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

Az a  **[Azure-portálon](https://portal.azure.com/)**, kattintson a **App Service** lehetőséget a bal oldali a lap.

Kattintson a nevére, amely a folyamatos üzembe Docker Hub konfigurálni szeretné az alkalmazás.

Az a **Alkalmazásbeállítások**, hozzáadhat egy alkalmazást nevű beállítása `DOCKER_ENABLE_CI` értékű `true`.

![Helyezze be a Alkalmazásbeállítás képe](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a>3. lépés - a Webhook URL-CÍMÉT előkészítése

Ezt úgy szerezheti be a Webhook URL-cím használatával [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) és a következő parancs végrehajtása

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

A Webhook URL-címhez kell rendelkeznie a következő végpontot: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Ezt úgy szerezheti be a `publishingusername` és `publishingpwd` úgy, hogy letölti a webes alkalmazás közzététele a profil az Azure portál használatával.

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a>4. lépés – a webes hook hozzáadása

### <a name="azure-container-registry"></a>Azure Container Registry

A beállításjegyzék portálpanelén kattintson **Webhookok**, hozzon létre egy új webhook kattintva **Hozzáadás**. Az a **webhook létrehozása** panelen adjon a webhook nevét. A Webhook URI-hoz, meg kell adnia az URL-címet szerzett **3. lépés**

Győződjön meg arról, mint a tárház, amely tartalmazza a tároló lemezkép hatókörének meghatározása.

![Helyezze be a webhook képe](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

A lemezkép frissítésekor a lekérdezi a web app frissül automatikusan a új lemezképpel.

### <a name="docker-hub"></a>Docker központ

A Docker Hub oldalon kattintson **Webhookok**, majd **A WEBHOOK létrehozása**.

![Helyezze be a webhook 1 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

A Webhook URL-címhez, meg kell adnia az URL-címet szerzett **3. lépés**

![Helyezze be a webhook 2 hozzáadása képe](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

A lemezkép frissítésekor a lekérdezi a web app frissül automatikusan a új lemezképpel.

## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](./app-service-linux-intro.md)
* [Azure-tárolót beállításjegyzék](https://azure.microsoft.com/en-us/services/container-registry/)
* [PM2 Configuration for Node.js Linux Azure Web App használatával](app-service-linux-using-nodejs-pm2.md)
* [.NET Core Linux Azure Web App használatával](app-service-linux-using-dotnetcore.md)
* [Ruby Linux Azure Web App használatával](app-service-linux-ruby-get-started.md)
* [Egyéni Docker-lemezkép használata Linux Azure webalkalmazás számára](./app-service-linux-using-custom-docker-image.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](./app-service-linux-faq.md) 
* [Webalkalmazás az Azure CLI 2.0 verziót használja Linux kezelése](./app-service-linux-cli.md)



