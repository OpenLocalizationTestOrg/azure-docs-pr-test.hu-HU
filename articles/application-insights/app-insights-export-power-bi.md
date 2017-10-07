---
title: az Application Insights a BI aaaExport tooPower |} Microsoft Docs
description: "A Power BI elemzési lekérdezések olvasható."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 6668cd7f4e0fbf41695972617f5f8ec207356659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Az Application Insights hírcsatorna a Power bi-ban
[A Power BI](http://www.powerbi.com/) üzleti analytics eszközöket tartalmazza, amelyek segítséget nyújtanak a csomagok elemzéséhez és insights megosztani. Gazdag az irányítópultok olyan rendelkezésre álljanak minden eszközön. Számos más forrásból, beleértve az elemzési lekérdezések adatok kombinálhatja [Azure Application Insights](app-insights-overview.md).

Az Application Insights adatok tooPower BI exportáló három javasolt módszer áll rendelkezésre. Használhatja őket külön-külön vagy együtt.

* [**A Power BI adapter** ](#power-pi-adapter) -állítson be egy teljes irányítópult telemetriai adatot az alkalmazásból. előre definiált diagramok hello készletét, de más forrásból is hozzáadhat a saját lekérdezések.
* [**Elemzési lekérdezések exportálásáról** ](#export-analytics-queries) -bármely írás Analytics segítségével, és exportálni onnan tooPower BI lekérdezés. Ez a lekérdezés elhelyezheti az egyéb adatokat irányítópulton.
* [**A folyamatos exportálás és a Stream Analytics** ](app-insights-export-stream-analytics.md) -Ez magában foglalja a további munkahelyi tooset fel. Ez akkor hasznos, ha azt szeretné, tookeep az adatokat hosszú ideig. Ellenkező esetben hello többi módszer használata ajánlott.

## <a name="power-bi-adapter"></a>A Power BI-adapter
Ez a módszer létrehoz egy teljes irányítópult telemetriai adatot. hello kezdeti adatkészlet előre definiált, de további adatok tooit is hozzáadhat.

### <a name="get-hello-adapter"></a>Hello adapter beolvasása
1. Jelentkezzen be a túl[Power BI](https://app.powerbi.com/).
2. Nyissa meg **adatok**, **szolgáltatások**, **Application insights szolgáltatással**
   
    ![Az Application Insights adatforrás beszerzése](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Adja meg az Application Insights-erőforrás hello adatait.
   
    ![Az Application Insights adatforrás beszerzése](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Várjon egy percet, amíg hello adatok toobe importálva.
   
    ![A Power BI-adapter](./media/app-insights-export-power-bi/010.png)

Hello irányítópult hello Application Insights diagramok kombinálásának más forrásokból, valamint elemzési lekérdezések szerkesztheti. Nincs a képi megjelenítés gyűjteménye, ahol kaphat további diagramokat, és minden egyes diagram egy beállítható paraméterrel rendelkezik.

Hello kezdeti importálása, hello irányítópult és jelentések hello után továbbra is tooupdate naponta. Azt is szabályozhatja hello frissítési ütemezés hello adatkészlethez.

## <a name="export-analytics-queries"></a>Elemzési lekérdezések exportálása
Ez az útvonal toowrite lehetővé teszi bármely Analytics lekérdezési Önnek, majd exportálja a adott tooa Power BI-irányítópultot. (Hello csatoló által létrehozott toohello irányítópult is hozzáadhat.)

### <a name="one-time-install-power-bi-desktop"></a>Egy alkalommal: telepítse a Power BI Desktop
tooimport az Application Insights lekérdezés hello a Power BI asztali verzióját használja. De majd közzéteheti azt toohello web vagy tooyour Power BI felhő munkaterületen. 

Telepítés [a Power BI Desktopban](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Az elemzési lekérdezések exportálása
1. [Nyissa meg a elemzés és a lekérdezés írása](app-insights-analytics-tour.md).
2. Tesztelje, pontosítsa a hello lekérdezés, amíg hello eredmények megfelelőnek találja.

   **Győződjön meg arról, hogy hello lekérdezés futtatása megfelelően Analytics előtt exportálni kell.**
3. A hello **exportálása** menüben válasszon **Power BI (M)**. Hello szöveges fájlt mentse.
   
    ![A Power BI lekérdezés exportálása](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. A Power BI Desktop select **adatok beolvasása, üres lekérdezés** és majd hello a lekérdezés-szerkesztő a **nézet** válasszon **speciális lekérdezés-szerkesztő**.

    Beillesztés exportált hello M nyelvi parancsfájlt hello speciális lekérdezés-szerkesztő.

    ![Speciális lekérdezés-szerkesztő](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Lehetséges, hogy tooprovide hitelesítő adatok tooallow Power BI tooaccess Azure. Használja a "szervezeti fiók" toosign be a Microsoft-fiókjával.
   
    ![Adja meg Azure hitelesítő adataival tooenable Power BI toorun az Application Insights-lekérdezés](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Ha tooverify hello hitelesítő adatok szükségesek, paranccsal hello adatforrás-beállítások menü a Lekérdezésszerkesztő hello. Gondossággal járjanak el a toospecify hello hitelesítő adatokkal kell az Azure-ba, elképzelhető, hogy a hitelesítő adatait a Power BI eltér.)
2. Válassza ki a lekérdezést a képi megjelenítés, és válassza ki azokat az x tengely, y tengely és szegmentálja a dimenzió hello mezőket.
   
    ![Válassza ki a képi megjelenítés](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. A jelentés tooyour Power BI felhő munkaterület közzététele. Ott egy szinkronizált verziót is beágyazása más weblapokat.
   
    ![Válassza ki a képi megjelenítés](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Frissítse manuálisan a hello jelentés időközönként, vagy állítson be egy ütemezett frissítés hello-beállítások lapon.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="401-or-403-unauthorized"></a>401-es vagy 403-as nem engedélyezett 
Ez akkor fordulhat elő, ha a refesh token nem lett frissítve. Próbálja meg a lépéseket tooensure továbbra is hozzáfér. Ha Ön rendelkezik hozzáféréssel, és refershing hello hitelesítő adatok nem működik, nyisson egy támogatási jegy.

1. Jelentkezzen be hello Azure portálon, és győződjön meg arról, hogy van-e hozzáférési hello erőforrás
2. Próbálja meg hello irányítópult toorefresh hello hitelesítő adatai

### <a name="502-bad-gateway"></a>502 Hibás átjáró
Ezt általában az Analytics-lekérdezés túl sok adat visszaadó okozza. Meg kell egy kisebb időtartományt használatával vagy hello segítségével [ezelőtt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) vagy [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) funkciót csak [projekt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello szükséges mezők.

Ha hello Analytics lekérdezési érkező hello dataset csökkentése nem felel meg a követelményeknek érdemes hello [API](https://dev.applicationinsights.io/documentation/overview) toopull nagyobb adatkészletet. Az alábbiakban a hogyan tooconvert hello M-lekérdezés exportálja toouse hello API utasításokat.

1. Hozzon létre egy [API-kulcs](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. Frissítés hello Power BI M parancsfájl tartományvezérlőkkel történő lecserélésével Analytics exportált hello ARM URL-cím AI API-hoz (lásd az alábbi példa)
   * Cserélje le **https://management.azure.com/subscriptions/...**
   * a **https://api.applicationinsights.io/beta/apps/...**
3. Végül frissítse a hitelesítő adatok toobasic, és az API-kulcs használata
  

**Meglévő parancsfájl**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Frissített parancsfájl**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Mintavételi kapcsolatos
Az alkalmazás nagy mennyiségű adatot küld, ha hello adaptív mintavételi szolgáltatás működik, és csak százalékaként a telemetriai adatok küldése. hello is igaz, ha manuálisan mintavételi hello SDK vagy adatfeldolgozást. [További tudnivalók a mintavételezésről.](app-insights-sampling.md)


## <a name="next-steps"></a>Következő lépések
* [A Power BI - információ](http://www.powerbi.com/learning/)
* [Elemzés oktatóanyag](app-insights-analytics-tour.md)

