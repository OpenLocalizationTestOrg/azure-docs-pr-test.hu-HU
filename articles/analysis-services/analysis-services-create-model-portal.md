---
title: "a táblázatos modellek hello Azure Analysis Services Web-tervezővel aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Azure Analysis Services táblázatos modell használatával hello webes Tervező Azure-portálon."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: a37b326b76c84fc3a4300827bc1c8706b0584701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-model-in-azure-portal"></a>A modell létrehozása Azure-portálon

hello Azure Analysis Services web designer (előzetes verzió) szolgáltatás Azure-portálon egy gyorsan és egyszerűen elvégezhető toocreate biztosít, és szerkesztheti a táblázatos modellek és lekérdezés modell adatok jobb a böngészőben. 

Ne feledje, a hello webes Tervező **előzetes**. Új funkciók hozzáadott összes hello idő, kép, amíg a funkciók korlátozva. A speciális modell fejlesztéshez és teszteléshez akkor a legjobb toouse Visual Studio (SSDT) és az SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Előfeltételek

- Egy hello Standard vagy fejlesztői réteg: Azure Analysis Services-kiszolgálóhoz. Hello Web-tervezővel létrehozott új modellek DirectQuery, csak ezek a rétegek támogatja.
- Egy Azure SQL Database, Azure SQL Data warehouse-bA vagy egy adatforrást a Power BI Desktop (.pbix) fájlt. A Power BI Desktop fájlok támogatása az Azure SQL Database, Azure SQL Data Warehouse, Oracle és Teradata adatforrások létrehozott új modellek.
- Egy SQL Server fiókot és jelszót tooAzure SQL Database vagy az Azure SQL Data Warehouse adatforrások csatlakoztatásához.

## <a name="toocreate-a-new-tabular-model"></a>egy új táblázatos modell toocreate

1. A Server **áttekintése** panel > **webes Tervező**, kattintson a **nyitott**.

    ![A modell létrehozása Azure-portálon](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. A **webes Tervező** > **modellek**, kattintson a **+ Hozzáadás**.

    ![A modell létrehozása Azure-portálon](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. A **új modell**, írja be a modell neve, majd válassza ki egy adatforrást.

    ![Azure-portálon az új modell párbeszédpanel](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. A **Connect**, adja meg a hello kapcsolat tulajdonságai. Felhasználónév és jelszó SQL Server fióknak kell lennie.

     ![Csatlakozás Azure-portálon párbeszédpanel](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. A **táblák és nézetek**, jelölje ki a hello táblák tooinclude a modell, és kattintson **létrehozása**. Egy kulcspár rendelkező táblák közötti kapcsolatok jönnek létre automatikusan.

     ![Válassza ki a táblák és nézetek](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Az új modell jelenik meg a böngészőben. Itt a következőket teheti:   

- Query modelladatok húzzon mezőket toohello Lekérdezéstervező, és vegye fel a szűrők.
- Hozzon létre új mértékek táblában.
- Szerkessze a modell metaadatainak hello json-szerkesztő használatával.
- Nyissa meg a hello modellt a Visual Studio (SSDT), a Power BI Desktop vagy az Excel.

![Válassza ki a táblák és nézetek](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Modell metaadatainak szerkesztése, illetve a böngészőben új mértékek létrehozása az Azure-ban e módosítások tooyour modell menti. Ha még dolgozik a modell SSDT, a Power BI Desktop vagy az Excel, a modell szinkronban kérheti le.


## <a name="next-steps"></a>Következő lépések 
[Adatbázis-szerepkörök és a felhasználók kezelése](analysis-services-database-users.md)  
[Kapcsolódás Excellel](analysis-services-connect-excel.md)  


