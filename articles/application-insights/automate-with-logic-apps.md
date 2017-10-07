---
title: "aaaAutomate Azure Application Insights Logic Apps segítségével dolgozza fel."
description: "Ismerje meg, milyen gyorsan automatizálhatja ismételhető folyamatok hello Application Insights összekötő tooyour logikai alkalmazás hozzáadásával."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 8565aadf0644ffb10da8a0964821901907db2e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Az Application Insights-folyamatok automatizálása a Logic Apps segítségével

Tegye veszi észre magát ismételten futó hello azonos lekérdezések a telemetriai adatok toocheck a szolgáltatás megfelelően működnek-e? Kinézetű tooautomate ezeket a lekérdezéseket, a trendek és rendellenességeket kereséséhez és majd a felhasználókat ezekbe a csoportokba a saját munkafolyamatok építhetők fel? hello Azure Application Insights-összekötő (előzetes verzió) Logic Apps hello ideális eszközt erre a célra.

Ezt az integrációt a automatizálhat egysoros kód írása nélkül számos folyamatokat. Létrehozhat egy logikai alkalmazást az Application Insights hello összekötő tooquickly bármely Application Insights folyamat automatizálásához. 

Hozzáadhat további műveleteket is. hello Logic Apps szolgáltatás az Azure App Service elérhetővé több száz műveletek. Például a logikai alkalmazás, automatikusan küldjön e-mailben értesítést vagy hozzon létre egy hibajelentést a Visual Studio Team Services. Is használhatja a hello számos elérhető [sablonok](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp felgyorsítása hello a logikai alkalmazás létrehozásának folyamatán. 

## <a name="create-a-logic-app-for-application-insights"></a>Az Application Insights logikai alkalmazás létrehozása

Ebben az oktatóanyagban elsajátíthatja, hogyan toocreate hello Analytics autocluster algoritmus toogroup használó logikai alkalmazás attribútumok webalkalmazás hello adataiban. hello folyamata hello eredmények automatikusan elküldi e-mailben, hogyan használhatja az alkalmazásokhoz kapcsolódó elemzések elemzés és a Logic Apps együtt egy példát. 

### <a name="step-1-create-a-logic-app"></a>1. lépés: Logikai alkalmazás létrehozása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello **új** ablaktáblán válassza előbb **Web + mobil**, majd válassza ki **logikai alkalmazás**.

    ![Új logikai alkalmazás ablak](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>2. lépés: A Logic Apps alkalmazást eseményindító létrehozása
1. A hello **Logic App Designer** ablakban, a **indítsa el az általános eseményindító**, jelölje be **ismétlődési**.

    ![Logic App-Tervező ablak](./media/automate-with-logic-apps/logicapp2.png)

2. A hello **gyakoriság** mezőben válassza **nap** , majd a hello **időköz** mezőbe írja be **1**.

    ![Logic App Designer "Ismétlődési" ablak](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>3. lépés: Új Application Insights művelet
1. Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.

2. A hello **művelet kiválasztását** keresési mezőbe, írja be **Azure Application Insights**.

3. A **műveletek**, kattintson a **Azure Application Insights – megjelenítése Analytics lekérdezés előnézeti**.

    ![Logic App Designer "Művelet kiválasztása" ablak](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a>4. lépés: Csatlakozás tooan Application Insights-erőforrás

toocomplete ezt a lépést, az erőforrás Azonosítóját és API-kulcs szükséges. Helyreállíthatók a hello Azure-portálon a hello a következő ábrán látható módon:

![Alkalmazás Azonosítójának hello Azure-portálon](./media/automate-with-logic-apps/appid.png) 

Adja meg a kapcsolat hello Alkalmazásazonosítót és hello API-kulcs nevét.

![Logic App Tervező folyamata kapcsolat ablak](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a>5. lépés: Adja meg a hello Analytics lekérdezési és diagramtípus
A következő példa hello a hello lekérdezés kiválasztása hello sikertelen kérelmek hello utolsó napon belül, és korrelálja őket a kivételeket, amelyek hello művelet részeként történt. Elemzés hello sikertelen kérelmek hello operation_Id azonosítója alapján ad eredményül. hello lekérdezés hello eredmények majd szegmensek hello autocluster algoritmus használatával. 

A saját lekérdezések létrehozása, ellenőrizze, hogy megfelelően működnek az Analytics mielőtt hozzáadja tooyour folyamata.

1. A hello **lekérdezés** mezőben adja meg a következő Analytics lekérdezési hello: 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. A hello **diagramtípus** mezőben válassza **Html tábla**.

    ![Elemzés lekérdezés konfigurációs ablak](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-hello-logic-app-toosend-email"></a>6. lépés: Hello logic app toosend e-mail konfigurálása

1. Kattintson a **új lépés**, majd válassza ki **művelet hozzáadása**.

2. Hello keresési mezőbe, írja be a **Office 365 Outlook**.

3. Kattintson a **Office 365 Outlook – az e-mailek küldése**.

    ![Az Office 365 Outlook kiválasztása](./media/automate-with-logic-apps/flow2b.png)

4. A hello **egy e-mailek küldése** ablakban, a következő hello:

   a. Írja be a hello hello címzett e-mail címét.

   b. Írja be az üdvözlő e-mail tárgyát.

   c. Kattintson bárhova hello **törzs** mezőbe, majd, hello dinamikus tartalom megnyíló menüben jobb hello: válasszon **törzs**.

   d. Kattintson a **speciális beállítások megjelenítése**.

      ![Az Office 365 Outlook konfiguráció](./media/automate-with-logic-apps/flow5.png)

5. Az hello hello dinamikus tartalom menüben, a következő:

    a. Válassza ki **melléklet neve**.

    b. Válassza ki **melléklet tartalom**.
    
    c. A hello **HTML** mezőben válassza **Igen**.

      ![Az Office 365 e-mailek konfigurálására szolgáló képernyőn](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>7. lépés: Mentéséhez, és tesztelje a Logic Apps alkalmazást
* Kattintson a **mentése** toosave a módosításokat.

Hello eseményindító toorun hello logikai alkalmazás megvárhatja, vagy futtassa hello logikai alkalmazás azonnal kiválasztásával **futtatása**.

![Logikai alkalmazás létrehozása képernyő](./media/automate-with-logic-apps/step7.png)

A logikai alkalmazás futtatásakor hello e-mail listában megadott hello címzettek megkapják az egy e-mailt, amely a következőhöz hasonló hello:

![Logic app e-mailt](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók: létrehozása [elemzési lekérdezések](app-insights-analytics-using.md).
- További információ [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





