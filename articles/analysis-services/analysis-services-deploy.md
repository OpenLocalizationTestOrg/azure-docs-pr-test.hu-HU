---
title: "aaaDeploy tooAzure Analysis Services SSDT használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy táblázatos modell tooan Azure Analysis Services-kiszolgáló SSDT használatával."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: e3f3771fe32a37b9e0173c274080c647152edd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-model-from-ssdt"></a>Modell üzembe helyezése SSDT-ről
A létrehozását követően a kiszolgáló az Azure-előfizetése, most készen áll a toodeploy egy táblázatos modell adatbázis tooit. SQL Server Data Tools (SSDT) toobuild használja, és dolgozunk a táblázatos modell projekt telepítése. 

## <a name="prerequisites"></a>Előfeltételek
tooget elindult, lesz szüksége:

* **Analysis Services-kiszolgáló** az Azure-ban. több, lásd: toolearn [hozzon létre egy Azure Analysis Services-kiszolgáló](analysis-services-create-server.md).
* **A táblázatos modell projekt** SSDT vagy meglévő hello 1200-as vagy magasabb kompatibilitási szintű táblázatos modell. Korábban még nem hozott létre egyet sem? Próbálja meg hello [Adventure Works Internet értékesítési táblázatos modellezési oktatóanyag](https://msdn.microsoft.com/library/hh231691.aspx).
* **A helyszíni átjáró** – Ha egy vagy több adatforrást a vállalati hálózathoz a helyszíni, tooinstall kell egy [helyszíni adatátjáró](analysis-services-gateway.md). hello átjáró szükség, az a kiszolgáló hello felhőben csatlakozás tooyour a helyszíni adatok források tooprocess és frissítési adatok hello modellben.

> [!TIP]
> Ahhoz, hogy telepíteni, győződjön meg arról is feldolgozhat hello adatok a táblázatban. Az SSDT-ben kattintson a **Modell** > **Feldolgozás** > **Az összes feldolgozása** lehetőségre. Ha a feldolgozás meghiúsul, nem fog sikerülni a telepítés.
> 
> 

## <a name="toodeploy-a-tabular-model-from-ssdt"></a>a táblázatos modellek az SSDT toodeploy

1. Telepít, meg kell tooget hello kiszolgáló nevét. A **Azure-portálon** > server > **áttekintése** > **kiszolgálónév**, hello server példányneve.
   
    ![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. Az SSDT > **Megoldáskezelőben**, kattintson a jobb gombbal hello project > **tulajdonságok**. Ezt a **telepítési** > **Server** illessze be a hello kiszolgáló nevét.   
   
    ![Az üzembehelyezési kiszolgáló tulajdonságához illessze be a kiszolgáló nevét.](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. A **Megoldáskezelőben** kattintson a jobb gombbal a **Tulajdonságok** elemre, majd kattintson az **Üzembe helyezés** lehetőségre. Előfordulhat, hogy a tooAzure rákérdezéses toosign.
   
    ![Tooserver telepítése](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Telepítés állapota akkor jelenik meg, mindkét hello kimeneti ablakban és a telepítés.
   
    ![Üzembe helyezés állapota](./media/analysis-services-deploy/aas-deploy-status.png)

Ez minden nincs tooit!


## <a name="troubleshooting"></a>Hibaelhárítás
Ha nem sikerül a telepítés, metaadatok telepítésekor, valószínű, mert az SSDT tooyour kiszolgáló nem tudott kapcsolódni. Ellenőrizze, hogy tooyour server SSMS használatával is elérheti. Végezze el, hogy megfelelő-e a központi telepítési kiszolgáló tulajdonság hello projekt hello.

Ha táblán nem sikerül a telepítés, valószínű, mert a kiszolgáló nem tudott kapcsolódni a tooa adatforrás. Ha az adatforrás a vállalati hálózathoz a helyszíni, hogy meg arról, hogy tooinstall egy [helyszíni adatátjáró](analysis-services-gateway.md).

## <a name="next-steps"></a>Következő lépések
Most, hogy a táblázatos modell telepített tooyour kiszolgáló, most készen áll a tooconnect tooit. Is [tooit kapcsolattartásnak SSMS](analysis-services-manage.md) toomanage azt. Ráadásul, [tooit ügyfél eszköz használatával csatlakoztassa](analysis-services-connect.md) például a Power bi-ban, a Power BI Desktopban, vagy az Excel és a jelentések létrehozásának indítása.

