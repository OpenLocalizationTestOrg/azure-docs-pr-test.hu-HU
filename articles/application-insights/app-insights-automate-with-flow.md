---
title: aaaAutomate Azure Application Insights dolgozza fel a Microsoft Flow
description: "Ismerje meg, hogyan használhatja a Microsoft Flow tooquickly ismételhető folyamatok automatizálása hello Application Insights-összekötő használatával."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: b34488a6b8b8b0a6add960a67f1426cbbbc13552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-azure-application-insights-processes-with-hello-connector-for-microsoft-flow"></a>Hello Connector Azure Application Insights-folyamatok automatizálása a Microsoft Flow

Tegye veszi észre magát ismételten futó hello azonos lekérdezések a telemetriai adatok toocheck, hogy a szolgáltatás megfelelően működik? Kinézetű tooautomate ezeket a lekérdezéseket, a trendek és rendellenességeket kereséséhez és majd a felhasználókat ezekbe a csoportokba a saját munkafolyamatok építhetők fel? hello Azure Application Insights-összekötő (előzetes verzió) Microsoft Flow egy hello megfelelő eszköz ezekből a célokból.

Ez az integráció mostantól automatizálhatja számos folyamatok egysoros kód írása nélkül. Az Application Insights műveletet hoz létre egy folyamatot, miután hello folyamat automatikusan futtatja az Application Insights Analytics lekérdezés. 

Hozzáadhat további műveleteket is. Microsoft Flow elérhetővé több száz műveletek. Például Microsoft Flow tooautomatically küldése e-mailben értesítést használja, vagy hozzon létre egy hibajelentést a Visual Studio Team Services. Is használhatja a hello számos [sablonok](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) hello Microsoft Flow-összekötő elérhető. Ezeket a sablonokat a folyamat létrehozásának folyamatán hello felgyorsításában. 

<!--hello Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Hozzon létre egy folyamatot az Application Insights

Ebből az oktatóanyagból megtudhatja, hogyan toocreate a folyamat által használt hello Analytics automatikus-fürt algoritmus toogroup attribútumok webalkalmazás hello adataiban. hello folyamata hello eredmények automatikusan elküldi e-mailben, hogyan használhatja a Microsoft Flow és az Application Insights Analytics együtt egy példát. 

### <a name="step-1-create-a-flow"></a>1. lépés: A folyamat létrehozása
1. Jelentkezzen be a túl[Microsoft Flow](http://flow.microsoft.com), majd válassza ki **saját Forgalomáramlás**.
2. Kattintson a **a folyamat létrehozása üres**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>2. lépés: A folyamat eseményindító létrehozása
1. Válassza ki **ütemezés**, majd válassza ki **ütemezés - ismétlődési**.
2. A hello **gyakoriság** mezőben válassza **nap**, és a hello **időköz** adja meg a **1**.

    ![Microsoft Flow eseményindító párbeszédpanel](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>3. lépés: Új Application Insights művelet
1. Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.
2. Keresse meg **Azure Application Insights**.
3. Kattintson a **Azure Application Insights – megjelenítése Analytics lekérdezés előnézeti**.

    ![Analytics lekérdezési ablakban futtassa](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a>4. lépés: Csatlakozás tooan Application Insights-erőforrás

toocomplete ezt a lépést, az erőforrás Azonosítóját és API-kulcs szükséges. Helyreállíthatók a hello Azure-portálon a hello a következő ábrán látható módon:

![Alkalmazás Azonosítójának hello Azure-portálon](./media/app-insights-automate-with-flow/appid.png) 

- Adja meg a hálózati kapcsolatot, valamint hello alkalmazás azonosítója és API-kulcs nevét.

    ![Microsoft Flow kapcsolat ablak](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a>5. lépés: Adja meg a hello Analytics lekérdezési és diagramtípus
Ez a példa lekérdezés kiválasztása hello sikertelen kérelmek hello utolsó napon belül, és korrelálja őket a kivételeket, amelyek hello művelet részeként történt. Elemzés korrelálja őket hello operation_Id azonosítója alapján. hello lekérdezés hello eredmények majd szegmensek hello autocluster algoritmus használatával. 

A saját lekérdezések létrehozása, ellenőrizze, hogy megfelelően működnek az Analytics mielőtt hozzáadja tooyour folyamata.

- Adja hozzá a következő Analytics lekérdezési hello, és válassza a hello HTML tábla diagram típusát. 

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
    
    ![Elemzés lekérdezés konfigurációs ablak](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-hello-flow-toosend-email"></a>6. lépés: Hello folyamat toosend e-mail konfigurálása

1. Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.
2. Keresse meg **Office 365 Outlook**.
3. Kattintson a **Office 365 Outlook – az e-mailek küldése**.

    ![Az Office 365 Outlook kiválasztási ablaka](./media/app-insights-automate-with-flow/flow2b.png)

4. A hello **egy e-mailek küldése** ablakban, a következő hello:

   a. Írja be a hello hello címzett e-mail címét.

   b. Írja be az üdvözlő e-mail tárgyát.

   c. Kattintson bárhova hello **törzs** mezőbe, majd, hello dinamikus tartalom megnyíló menüben jobb hello: válasszon **törzs**.

   d. Kattintson a **speciális beállítások megjelenítése**.

    ![Az Office 365 Outlook konfiguráció](./media/app-insights-automate-with-flow/flow5.png)

5. Az hello hello dinamikus tartalom menüben, a következő:

    a. Válassza ki **melléklet neve**.

    b. Válassza ki **melléklet tartalom**.
    
    c. A hello **HTML** mezőben válassza **Igen**.

    ![Az Office 365 e-mailek konfigurációs ablak](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>7. lépés: Mentéséhez és a folyamat tesztelése
- A hello **folyamat nevének** mezőbe, vegye fel a folyamat nevét, majd kattintson **folyamat létrehozása**.

    ![Adatfolyam-létrehozási ablak](./media/app-insights-automate-with-flow/flow8.png)

Megvárhatja hello eseményindító toorun ezt a műveletet, vagy hello folyamata, azonnal futtathatja [futó hello eseményindító az igény szerinti](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Amikor hello folyamat fut, a hello e-mail listában megadott hello címzettek kapnak e-mailt, amely a következőhöz hasonló hello:

![Példa e-mailre](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Következő lépések

- További tudnivalók: létrehozása [elemzési lekérdezések](app-insights-analytics-using.md).
- További információ [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





