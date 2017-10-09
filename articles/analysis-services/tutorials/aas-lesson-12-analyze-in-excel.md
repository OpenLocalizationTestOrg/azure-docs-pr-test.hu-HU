---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 12: elemzés az Excelben |} Microsoft Docs"Leírás: ismerteti, hogyan toouse elemzés az Excelben a hello Azure Analysis Services oktatóanyag projekt. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="lesson-12-analyze-in-excel"></a>12. lecke: Elemzés az Excelben

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ez a lecke hello elemzés az Excelben funkciót tooopen Microsoft Excel használjon, automatikusan hozzon létre egy kapcsolat toohello modell munkaterületet, és automatikusan adja hozzá a kimutatáshoz toohello munkalap. hello elemzése az Excel programba tooprovide célja egy a modell gyorsan és egyszerűen elvégezhető tootest hello hatékonyságát tervezési előzetes toodeploying a modell. Ez a lecke nem tartalmaz adatelemzést. hello Ez a lecke célja toofamiliarize, hello modell szerzőként, hello eszközök segítségével tootest a modell kialakítását.   
  
Ebben a leckében Excel telepíteni kell hello toocomplete SSDT ugyanarra a számítógépre.
  
Becsült idő toocomplete Ez a lecke: **öt perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 11: szerepkörök létrehozása](../tutorials/aas-lesson-11-create-roles.md).  
  
## <a name="browse-using-hello-default-and-internet-sales-perspectives"></a>Keresse meg az alapértelmezett és az Internet értékesítési perspektívák hello használata  
Első a feladatokkal, akkor keresse meg a modell mindkét hello alapértelmezett perspektívára, minden modellobjektumokat tartalmazó használatával és hello Internet értékesítési perspektíva használatával korábban meg. hello Internet értékesítési perspektíva hello felhasználói táblaobjektum nem tartalmazza.  
  
#### <a name="toobrowse-by-using-hello-default-perspective"></a>toobrowse hello alapértelmezett perspektíva használatával  
  
1.  Kattintson a hello **modell** menü > **elemzés az Excelben**.  
  
2.  A hello **elemzés az Excelben** párbeszédpanel, kattintson a **OK**.  
  
    Megnyílik az Excel, és megjelenít egy új munkafüzetet. Adatforrás-kapcsolat jön létre hello aktuális felhasználói fiókkal, és alapértelmezett perspektíva hello használt toodefine megtekinthető mezők. A kimutatás automatikusan toohello munkalapra.  
  
3.  Az Excel, a hello **kimutatás mezőlistájának**, értesítés hello **DimDate** és **FactInternetSales** Mértékcsoportok jelennek meg. Hello **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, és **FactInternetSales** is megjelenhet a megfelelő oszloppal rendelkező táblákon.  
  
4.  Zárja be az Excel hello munkafüzetet mentés nélkül.  
  
#### <a name="toobrowse-by-using-hello-internet-sales-perspective"></a>toobrowse hello Internet értékesítési perspektíva használatával  
  
1.  Kattintson a hello **modell** menüben, majd kattintson **elemzés az Excelben**.  
  
2.  A hello **elemzés az Excelben** párbeszédpanelen hagyja **aktuális Windows-felhasználó** kijelölve, majd a hello **perspektíva** legördülő listáról válassza **Internet értékesítés** , és kattintson a **OK**. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  Az Excel programban a **kimutatástábla mezői**, figyelje meg, hello DimCustomer tábla hello mezőlistáról ki van zárva.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Zárja be az Excel hello munkafüzetet mentés nélkül.  
  
## <a name="browse-by-using-roles"></a>Tallózás szerepkörök használatával  
A szerepkörök minden táblázatos modell fontos részét képezik. Legalább egy szerepkör nélkül toowhich felhasználót adnak hozzá tagként, felhasználók nem férhetnek hozzá, és elemezhetik a modell használatával. hello elemzése az Excel funkció lehetőséget biztosít az Ön tootest hello szerepköröket, meghatározott.  
  
#### <a name="toobrowse-by-using-hello-sales-manager-user-role"></a>toobrowse hello értékesítési igazgató felhasználói szerepkör használatával  
  
1.  Az SSDT, kattintson a hello **modell** menüben, majd kattintson **elemzés az Excelben**.  
  
2.  A **megadása hello felhasználó neve vagy szerepe toouse tooconnect toohello modell**, jelölje be **szerepkör**, majd a hello legördülő lista, válassza **értékesítési igazgató**, és kattintson a  **OK**.  
  
    Megnyílik az Excel, és megjelenít egy új munkafüzetet. Ekkor automatikusan létrejön egy kimutatás. hello Pivot tábla mezőlista olyan összes hello adatok mezőket az új modell érhető el.  
      
3.  Zárja be az Excel hello munkafüzetet mentés nélkül.  
  
## <a name="whats-next"></a>A következő lépések
Nyissa meg toohello következő lecke: [lecke 13: telepítése](../tutorials/aas-lesson-13-deploy.md).

  
  
  
