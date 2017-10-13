---
title: "Azure Analysis Services-oktatóanyag – 12. lecke: Elemzés az Excelben | Microsoft Docs"
description: "A lecke azt ismerteti, hogyan használható az Elemzés az Excelben az Azure Analysis Services oktatóprojektjében."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/20/2017
ms.author: owend
ms.openlocfilehash: e257862a88d39b96360703117f544c43e82b0e3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-12-analyze-in-excel"></a>12. lecke: Elemzés az Excelben

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a leckében az Elemzés az Excelben használatával megnyitja a Microsoft Excelt, automatikusan létrehoz egy kapcsolatot a modell-munkaterülettel, és automatikusan hozzáad egy kimutatást a munkalaphoz. Az Elemzés az Excelben funkció gyors és könnyű módot biztosít a modellfelépítés hatékonyságának tesztelésére a modell üzembe helyezése előtt. Ez a lecke nem tartalmaz adatelemzést. A lecke célja, hogy megismertesse Önt, a modell alkotóját a modellfelépítés tesztelésére használható eszközökkel.   
  
A lecke elvégzéséhez az Excelnek és az SSDT-nek ugyanazon a számítógépen kell telepítve lennie.
  
A lecke elvégzésének várható időtartama: **5 perc**.  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. A leckében foglalt feladatok végrehajtása előtt el kell végeznie az előző leckét ([11. lecke: Szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md)).  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a>Tallózás az alapértelmezett és az Internetes értékesítés perspektíva használatával  
Ezekben az első feladatokban az összes modellobjektumot tartalmazó alapértelmezett perspektíva, valamint a korábban létrehozott Internetes értékesítés perspektíva használatával tallózhat a modellben. Az Internetes értékesítés perspektíva nem tartalmazza a Customer (Ügyfél) táblaobjektumot.  
  
#### <a name="to-browse-by-using-the-default-perspective"></a>Tallózás az alapértelmezett perspektívával  
  
1.  Kattintson a **Modell** menü **Elemzés az Excelben** elemére.  
  
2.  Az **Elemzés az Excelben** párbeszédpanelen kattintson az **OK** gombra.  
  
    Megnyílik az Excel, és megjelenít egy új munkafüzetet. Létrejön egy adatforrás-kapcsolat az aktuális felhasználói fiók használatával, a megtekinthető mezőket pedig az alapértelmezett perspektíva határozza meg. A rendszer automatikusan hozzáad egy kimutatást a munkalaphoz.  
  
3.  Az Excel **Kimutatás mezőlistája** területén figyelje meg, hogy a **DimDate** és **FactInternetSales** mértékcsoportok is láthatók. A **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** és **FactInternetSales** táblák is megjelennek a saját oszlopaikkal együtt.  
  
4.  Zárja be az Excelt a munkafüzet mentése nélkül.  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a>Tallózás az Internetes értékesítés perspektívával  
  
1.  Kattintson a **Modell** menü **Elemzés az Excelben** elemére.  
  
2.  Az **Elemzés az Excelben** párbeszédpanelen hagyja bejelölve **A Windows jelenlegi felhasználója** beállítást, majd a **Perspektíva** legördülő listából válassza ki az **Internetes értékesítés** elemet, és kattintson az **OK** gombra. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  Az Excel **Kimutatás mezőlistája** területén figyelje meg, hogy a DimCustomer tábla nem szerepel a mezők listájában.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Zárja be az Excelt a munkafüzet mentése nélkül.  
  
## <a name="browse-by-using-roles"></a>Tallózás szerepkörök használatával  
A szerepkörök minden táblázatos modell fontos részét képezik. Ha nincs legalább egy szerepkör, amelyhez felhasználók vannak hozzáadva tagként, a felhasználók nem érhetik el és nem elemezhetik az adatokat a modell használatával. Az Elemzés az Excelben funkció lehetővé teszi a definiált szerepkörök tesztelését.  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a>Tallózás az Értékesítési vezető felhasználói szerepkörrel  
  
1.  Az SSDT-ben kattintson a **Modell** menüre, majd az **Elemzés az Excelben** elemre.  
  
2.  Az **Adja meg a modellhez való kapcsolódáshoz használandó felhasználónevet vagy szerepkört** mezőben válassza ki a **Szerepkör** lehetőséget, majd a legördülő listából válassza ki az **Értékesítési vezető** szerepkört, és kattintson az **OK** gombra.  
  
    Megnyílik az Excel, és megjelenít egy új munkafüzetet. Ekkor automatikusan létrejön egy kimutatás. A kimutatás mezőlistája az új modellben elérhető összes adatmezőt tartalmazza.  
      
3.  Zárja be az Excelt a munkafüzet mentése nélkül.  
  
## <a name="whats-next"></a>A következő lépések
Tovább a következő leckére: [13. lecke: Üzembe helyezés](../tutorials/aas-lesson-13-deploy.md).

  
  
  
