---
title: "Web- és API-alkalmazások felhasználók számára elérhetővé az Azure-verem |} Microsoft Docs"
description: "Az oktatóanyag segítséget nyújt az App Service erőforrás-szolgáltató telepítéséhez, és hozzon létre kínál, amelyek biztosíthat Azure verem felhasználóinak való web- és API-alkalmazások létrehozásának képessége."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 7/03/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: 42d94eea8b9e4db611b821cd84e7d02f1d226293
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/27/2017
---
# <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web- és API-alkalmazások felhasználók számára elérhetővé az Azure-verem

Rendszergazdaként Azure verem felhő ajánlatokat, amelyek segítségével a felhasználók hozhat létre (bérlőkkel) az Azure Functions és a webes és API-alkalmazások létrehozása. Igény szerint a felhőalapú alkalmazások elérhetőségének biztosításával a felhasználók számára, mentheti azokat időt és erőforrásokat. Ennek beállításához a fogja végrehajtani:

> [!div class="checklist"]
> * Az App Service erőforrás-szolgáltató telepítése
> * Ajánlat létrehozása
> * Az ajánlat tesztelése

## <a name="deploy-the-app-service-resource-provider"></a>Az App Service erőforrás-szolgáltató telepítése

1. [Készítse elő a Azure verem szoftverfejlesztői készlet állomás](azure-stack-app-service-before-you-get-started.md). Ez magában foglalja az SQL Server erőforrás-szolgáltató, néhány alkalmazás létrehozásához szükséges telepítése.
2. [Töltse le a telepítő és a segítő parancsfájlok](azure-stack-app-service-deploy.md).
3. [A segítő parancsprogrammal hozzon létre a szükséges tanúsítványok](azure-stack-app-service-deploy.md).
4. [Az App Service erőforrás-szolgáltató telepítéséhez](azure-stack-app-service-deploy.md) (telepítés néhány órát fog igénybe venni, és minden a feldolgozói szerepköröket jelennek meg).
5. [A telepítés ellenőrzése](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Ajánlat létrehozása

Tegyük fel az ajánlat, amely lehetővé teszi, hogy a felhasználók létre DNN webes tartalom felügyeleti rendszerekkel is létrehozhat. Az SQL Server szolgáltatás, amely már engedélyezve van, az SQL Server erőforrás-szolgáltató telepítésével igényel.

1.  [Állítsa be a kvóta](azure-stack-setting-quotas.md) és adjon neki nevet *AppServiceQuota*. Válassza ki **Microsoft.Web** a a **Namespace** mező.
2.  [Hozzon létre egy csomagot](azure-stack-create-plan.md). Nevezze el *TestAppServicePlan*, jelölje be a **Microsoft.SQL** szolgáltatás, és **AppService kvóta** kvótát.

    > [!NOTE]
    > Ahhoz, hogy a felhasználók más-alkalmazásai létrehozására, a terv más szolgáltatások akkor lehet szükség. Például az Azure Functions az szükséges, hogy a tervben a **Microsoft.Storage** szolgáltatás, amíg a Wordpress igényel **Microsoft.MySQL**.
    > 
    >

3.  [Hozzon létre egy ajánlatot](azure-stack-create-offer.md), adjon neki nevet **TestAppServiceOffer** válassza ki a **TestAppServicePlan** terv.

## <a name="test-the-offer"></a>Az ajánlat tesztelése

Most, hogy az App Service erőforrás-szolgáltató telepítése után, és létrehozott egy ajánlatot, bármikor beléphet egy olyan felhasználó nevében, az ajánlat előfizetni, és hozzon létre egy alkalmazást. Ehhez a példához létrehozunk egy DNN Platform tartalomkezelési rendszer. Először létre kell hoznia egy SQL-adatbázis és a DNN webalkalmazás.

### <a name="subscribe-to-the-offer"></a>Az ajánlat előfizetés
1. Jelentkezzen be a verem Azure portal (https://portal.local.azurestack.external) a bérlőt.
2. Kattintson a **egy előfizetés** > típus **TestAppServiceSubscription** alatt **megjelenített név** > **válasszon egy ajánlatot**  >  **TestAppServiceOffer** > **létrehozása**.

### <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

1. Kattintson a  **+**   >  **adatok + tárolás** > **SQL-adatbázis**.
2. Hagyja meg az alapértelmezett értéket a mezőket, kivéve az alábbiak szerint:
    - **Adatbázis neve**: DNNdb
    - **Maximális méretét megabájtban**: 100
    - **Előfizetés**: TestAppServiceOffer
    - **Erőforráscsoport**: DNN-rg-n
3. Kattintson a **bejelentkezési beállítások**, adja meg az adatbázis hitelesítő adatait, és kattintson a **OK**. Ezeket a hitelesítő adatokat fogja használni a következő lépéseket.
4. Kattintson a **SKU** > Válassza ki a létrehozott üzemeltető SQL Server SQL SKU > **OK**.
5. Kattintson a **Create** (Létrehozás) gombra.

### <a name="create-a-dnn-app"></a>DNN-alkalmazás létrehozása    

1. Kattintson a  **+**   >  **láthatja az összes** > **DNN Platform preview** > **létrehozása**.
2. Típus *DNNapp* alatt **alkalmazásnév** válassza **TestAppServiceOffer** alatt **előfizetés**.
3. Kattintson a **kötelező beállítások konfigurálása** > **hozzon létre új** > típus egy **App Service-csomag** nevét.
4. Kattintson a **tarifacsomag** > **F1 ingyenes** > **válasszon** > **OK**.
5. Kattintson a **adatbázis** és írja be a korábban létrehozott az SQL-adatbázis adatait.
6. Kattintson a **Create** (Létrehozás) gombra.

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Az App Service erőforrás-szolgáltató telepítése
> * Ajánlat létrehozása
> * Az ajánlat tesztelése

Előzetes tovább az oktatóanyaghoz, megtudhatja, hogyan:

> [!div class="nextstepaction"]
> [Alkalmazások telepítése Azure és az Azure verem](user/azure-stack-solution-pipeline.md)
