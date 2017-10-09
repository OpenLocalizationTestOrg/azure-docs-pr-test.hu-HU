---
title: "egy Analysis Services-kiszolgálóhoz, az Azure-ban aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Analysis Services-kiszolgáló-példány az Azure-ban."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3668f659039f79f3dd71498d1066e8682bf33228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a>Az Azure Analysis Services-kiszolgáló létrehozása az Azure-portálon
Ez a cikk végigvezeti egy Analysis Services-kiszolgáló erőforrás létrehozása az Azure-előfizetéshez.

## <a name="before-you-begin"></a>Előkészületek
toocomplete a gyors üzembe helyezés szüksége:

* **Azure-előfizetés**: keresse fel [Azure ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate fiókkal.
* **Az Azure Active Directory**: az előfizetéshez kell tartoznia az Azure Active Directory-bérlő. És egy olyan fiókkal, az adott Azure Active Directoryban tooAzure bejelentkezve toobe van szüksége. Microsoft-fiókok nem támogatottak. több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).
* **Erőforráscsoport**: már van erőforráscsoport használata vagy [hozzon létre egy újat](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> A kiszolgáló létrehozása új számlázható szolgáltatás létrejöttét eredményezheti. több, lásd: toolearn [Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).
> 
> 

## <a name="toocreate-a-server-in-azure-portal"></a>a kiszolgáló Azure-portálon toocreate
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).  
2. Kattintson a **+ új** > **adatok + analitika** > **Analysis Services**.
3. A hello **Analysis Services** panelen hello kötelező mezők kitöltésével, és nyomja le az **létrehozása**.
   
    ![Kiszolgáló létrehozása](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Kiszolgálónév**: írja be egy egyedi nevet használt tooreference hello kiszolgáló.
   * **Előfizetés**: válassza ki a hello előfizetés ehhez a kiszolgálóhoz való váltók stb.
   * **Erőforráscsoport**: ezek a tárolók tervezett toohelp kezelheti az Azure-erőforrások gyűjteménye. több, lásd: toolearn [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md).
   * **Hely**: az Azure datacenter hely állomások hello kiszolgáló. Válasszon a legnagyobb felhasználói bázis legközelebbi helyet.
   * **IP-címek**: tarifacsomag kiválasztása. Másolatot too400 GB táblázatos modellek támogatottak. több, lásd: toolearn [Azure Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).
4. Kattintson a **Create** (Létrehozás) gombra.

Hozzon létre egy percet; általában tesz gyakran csak néhány másodpercig. Ha a kiválasztott **tooPortal hozzáadása**, keresse meg a portál toosee tooyour az új kiszolgáló. Vagy keresse meg a túl**további szolgáltatások** > **Analysis Services** toosee, ha a kiszolgáló készen áll.

 ![Irányítópult](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Következő lépések
Miután létrehozta a kiszolgáló, [a modell rendszerbe állítása](analysis-services-deploy.md) tooit SSDT vagy ssms alkalmazásával.

Egy modell tooyour kiszolgáló telepít tooon helyi adatforrások csatlakozik, ha szüksége van-e tooinstall egy [helyszíni adatátjáró](analysis-services-gateway.md) a hálózat egyik számítógépén.

