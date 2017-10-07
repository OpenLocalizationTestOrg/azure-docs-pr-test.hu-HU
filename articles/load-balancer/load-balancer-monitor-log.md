---
title: "aaaMonitor műveleteket, eseményeket és Load Balancer számlálói |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable riasztási események, és a mintavételi egészségügyi állapot naplózása Azure Load Balancer"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac53c2254e06cad780ad6144c5c30f0085d12576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a>Naplóelemzés az Azure Load Balancerhez

A naplók különböző típusait használják az Azure toomanage, és terheléselosztók hibaelhárítása. Ezek a naplók némelyike hello portálon keresztül elérhető. Minden naplók az Azure blob storage kicsomagolja, és különböző eszközök, például az Excel és a Power bi megtekintett. Többet tudhat meg hello különböző típusú naplók hello listán.

* **A naplók:** használható [Azure-beli Auditnaplók](../monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén műveleti naplókat) tooview elküldött tooyour társított Azure-előfizetéseit, és az állapotukat minden műveletnél. Naplók alapértelmezés szerint engedélyezve vannak, és megtekinthetők az hello Azure-portálon.
* **Eseménynaplók riasztási:** a napló tooview riasztások rasied hello terheléselosztó által is használhatja. hello állapot hello terheléselosztóhoz 5 percenként összegyűjtött. Ez a napló csak íródott, ha egy terhelés terheléselosztó figyelmeztetési esemény jelenik meg.
* **Állapot-mintavételi naplók:** a napló-tooview a állapotmintáihoz, például hello száma, amelyek nem fogadnak kérelmek hello terheléselosztó állapot-mintavételi hibák miatt a háttérkészlet példánya által észlelt problémák is használhatja. Ez a napló írása toowhen hello állapot-mintavételi egy módosult.

> [!IMPORTANT]
> Naplófájl analytics jelenleg csak az internetre irányuló terheléselosztók működik. Hello Resource Manager üzembe helyezési modellel üzembe helyezett erőforrás csak érhetők el naplók. Naplók erőforrások hello klasszikus üzembe helyezési modellben nem használhat. Hello üzembe helyezési modellel kapcsolatos további információkért lásd: [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Naplózás engedélyezése

Minden erőforrás-kezelő erőforrás naplózásra automatikusan engedélyezve van. Tooenable esemény és állapotfigyelő mintavételi naplózási toostart hello adatok elérhetők lesznek a naplók gyűjtésére van szükség. A következő lépéseket tooenable naplózási hello használata.

Bejelentkezési toohello [Azure-portálon](http://portal.azure.com). Ha még nem rendelkezik egy terhelés-kiegyenlítő [hozzon létre egy terhelés-kiegyenlítő](load-balancer-get-started-internet-arm-ps.md) a folytatás előtt.

1. Hello portálon kattintson **Tallózás**.
2. Válassza ki **Terheléselosztók**.

    ![portál - terheléselosztó](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Válassza ki a meglévő terheléselosztó >> **összes beállítás**.
4. Hello párbeszédpanel hello terheléselosztó hello néven hello jobb oldalán, görgessen túl**figyelés**, kattintson a **diagnosztika**.

    ![portál - terheléselosztási--beállításokat](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. A hello **diagnosztika** ablaktáblán, a **állapot**, jelölje be **a**.
6. Kattintson a **Tárfiók**.
7. A **naplók**, válasszon egy meglévő tárfiókot, vagy hozzon létre egy újat. Hello csúszkát toodetermine esemény adatmennyiséget felhasználó hello eseménynaplók kívánja tárolni, hogy hány nap használja. 
8. Kattintson a **Save** (Mentés) gombra.

    ![portál – diagnosztikai naplók](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Naplók nem igényelnek külön tárfiókot. hello használata tárolási esemény és állapota Hálózatfigyelő naplózási szolgáltatási díjak gyakorisága.

## <a name="audit-log"></a>Napló

hello napló alapértelmezés szerint létrejön. hello naplók Azure eseménynaplók tárolójában 90 napig megőrződnek. További információ a naplók hello olvasásával [események megtekintése és a naplók](../monitoring-and-diagnostics/insights-debugging-with-events.md) cikk.

## <a name="alert-event-log"></a>Riasztási eseménynaplót

Ez a napló csak akkor keletkezik, ha engedélyezte a egy / load balancer alapján. hello események naplózása JSON formátumban, és ha engedélyezte a naplózási hello megadott hello tárfiók tárolja. hello az alábbiakban látható egy példa egy eseményt.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

hello JSON kimeneti látható hello *eventname* tulajdonság, amely ismerteti, hello terheléselosztó hello okát létrehozott egy riasztást. Ebben az esetben hello által generált riasztást tooTCP port Erőforrásfogyás forrás IP-NAT korlátozza által okozott (SNAT) miatt volt.

## <a name="health-probe-log"></a>Állapot-mintavételi napló

Ez a napló csak akkor keletkezik, ha engedélyezte a egy betöltési terheléselosztó módon részletes fenti száma. Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatok tárolja. Egy "insights-logs-loadbalancerprobehealthstatus" nevű tárolót jön létre, és a következő adatok hello naplózására akkor kerül sor:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

hello tulajdonságok mező hello alapvető információkat hello mintavételi állapotadatok hello JSON-kimenetét mutatja. Hello *dipDownCount* tulajdonság mutatja hello háttér-, amely nem fordulnak elő a hálózati forgalom miatt toofailed mintavételi válaszok hello példányok száma összesen.

## <a name="view-and-analyze-hello-audit-log"></a>Megtekintése és elemzése hello audit napló

Megtekintheti és elemezheti a napló adatairól hello a következő módszerek bármelyikével:

* **Azure-eszközök:** adatok lekérését hello vizsgálati naplóit Azure PowerShell hello Azure parancssori felület (CLI) hello Azure REST API használatával, vagy hello Azure betekintő portálon. Részletes útmutatás az egyes módszerek részletes leírást talál hello [naplózási műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md) cikk.
* **A Power BI:** Ha még nem rendelkezik egy [Power BI](https://powerbi.microsoft.com/pricing) fiók, próbálkozzon az ingyenes. Hello segítségével [Azure-beli Auditnaplók content pack a Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), elemezheti az adatokat, előre konfigurált irányítópultok, vagy testre szabhatja nézetek toosuit igényeinek.

## <a name="view-and-analyze-hello-health-probe-and-event-log"></a>Megtekintése és elemzése hello állapotmintáihoz és az Eseménynapló

Szüksége tooconnect tooyour tárolási fiókját, és hello JSON naplóbejegyzések esemény és állapota Hálózatfigyelő naplók beolvasása. Amikor hello JSON-fájlok letöltéséhez tooCSV, tekintse meg Excel-, Power BI, vagy bármely más képi megjelenítés eszköz alakíthatja őket.

> [!TIP]
> Ha ismeri a Visual Studio és állandók és a C# változók értékeinek módosítása alapvető fogalmait, használhatja a hello [konverter eszközök jelentkezzen](https://github.com/Azure-Samples/networking-dotnet-log-converter) elérhető a Githubon.

## <a name="additional-resources"></a>További források

* [Megjelenítheti a Power BI az Azure-beli Auditnaplók](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzést.
* [Megtekintése és elemzése a Power bi-ban és több Azure-beli Auditnaplók](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.

## <a name="next-steps"></a>Következő lépések

[A Load Balancer vizsgálatok ismertetése](load-balancer-custom-probe-overview.md)
