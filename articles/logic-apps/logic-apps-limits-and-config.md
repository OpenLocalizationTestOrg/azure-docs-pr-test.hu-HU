---
title: "Logic App korlátozásai és konfigurációja |} Microsoft Docs"
description: "A szolgáltatásra vonatkozó korlátozások és a Logic Apps rendelkezésre álló konfigurációs értékek áttekintése."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: da23bd9fe71a0c41bc236b55bc9f56e123a9d77a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="logic-app-limits-and-configuration"></a>Logikai alkalmazások korlátai és beállítása

Az alábbiakban az Azure Logic Apps vonatkozó a jelenlegi korlátozások és konfigurációs információt.

## <a name="limits"></a>Korlátok

### <a name="http-request-limits"></a>HTTP-kérelmekre vonatkozó korlátok

Az alábbiakban az egyetlen HTTP kérelem és/vagy az összekötő-hívással határértékeit.

#### <a name="timeout"></a>Időtúllépés

|Név|Korlát|Megjegyzések|
|----|----|----|
|Kérelem időtúllépése|120 másodperc|Egy [aszinkron mintát](../logic-apps/logic-apps-create-api-app.md) vagy [amíg hurok](logic-apps-loops-and-scopes.md) pótolhatja igény szerint|

#### <a name="message-size"></a>Üzenet mérete

|Név|Korlát|Megjegyzések|
|----|----|----|
|Üzenet mérete|100 MB|Előfordulhat, hogy egyes összekötők és API-k nem támogatja a 100 MB |
|Kifejezés kiértékelése korlátot|131 072 karakter|`@concat()`, `@base64()`, `string` nem lehet hosszabb, mint a korlát|

#### <a name="retry-policy"></a>Ismételje meg a házirend

|Név|Korlát|Megjegyzések|
|----|----|----|
|Újrapróbálkozási kísérletek|10| Alapértelmezett érték 4. Konfigurálhatja a [ismételje meg a házirend-paraméter](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Maximális késleltetése|1 óra|Konfigurálhatja a [ismételje meg a házirend-paraméter](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Ismételje meg a minimális késleltetési|5 másodperc|Konfigurálhatja a [ismételje meg a házirend-paraméter](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Futtatási tartamára és fenntartására

Az alábbiakban futtassa egy logikai alkalmazások korlátok.

|Név|Korlát|Megjegyzések|
|----|----|----|
|Futtassa a időtartama|90 nap||
|Tároló megőrzési|90 nap|A Futtatás kezdő időpontja kiindulva|
|Min ismétlési időköz|1 másodperc|| az App Service-csomag logic apps 15 másodpercre
|Maximális ismétlési időköz|500 (nap)||

Ha hosszabb legyen, mint a futtatási időtartama vagy tárolási megőrzési korlátok normál feldolgozása folyamatában, [, lépjen velünk kapcsolatba](mailto://logicappsemail@microsoft.com) , hogy a követelményeinek, és segítünk.


### <a name="looping-and-debatching-limits"></a>Hurok- és debatching korlátok

Az alábbiakban korlátok egy logikai alkalmazások futtatása.

|Név|Korlát|Megjegyzések|
|----|----|----|
|ForEach-elemek|100,000|Használhatja a [lekérdezési művelet](../connectors/connectors-native-query.md) nagyobb tömbök igény szerint szűrése|
|Amíg az ismétlés|5,000||
|SplitOn elemek|100,000||
|ForEach párhuzamossági|50| Alapértelmezett érték 20. Adja hozzá a szekvenciális foreach állíthat be `"operationOptions": "Sequential"` számára a `foreach` művelet vagy adott szintű párhuzamosság használatával`runtimeConfiguration`|


### <a name="throughput-limits"></a>Átviteli sebességének korlátai

Az alábbiakban egy logikai alkalmazás példányra vonatkozó korlátok. 

|Név|Korlát|Megjegyzések|
|----|----|----|
|Műveletek végrehajtások / 5 perc |100,000|Is elosztják a munkaterhelést több alkalmazás igény szerint|
|Műveletek párhuzamos kimenő hívások |~2,500|Csökkentse az egyidejű kérelmek száma, vagy szükség szerint időtartamának csökkentése érdekében|
|Futásidejű végpont párhuzamos bejövő hívások |~1,000|Csökkentse az egyidejű kérelmek száma, vagy szükség szerint időtartamának csökkentése érdekében|
|Futásidejű végpont olvasási hívásszám 5 perc |60,000|Is elosztják a munkaterhelést több alkalmazás igény szerint|
|Futásidejű végpont meghívása hívásszám 5 perc |45,000|Is elosztják a munkaterhelést több alkalmazás igény szerint|

Ha túllépi ezt a határt, a normál feldolgozása vagy betöltése tesztelése, előfordulhat, hogy túllépi ezt a határt, egy ideig, lefuttatja [, lépjen velünk kapcsolatba](mailto://logicappsemail@microsoft.com) , hogy a követelményeinek, és segítünk.

### <a name="definition-limits"></a>Definition korlátok

Az alábbiakban egy logikai alkalmazás definícióját korlátok.

|Név|Korlát|Megjegyzések|
|----|----|----|
|Műveletek munkafolyamat száma|500|Beágyazott munkafolyamatok bevonásával bővítheti ezt a határt, igény szerint adhat hozzá|
|A beágyazás mélységére művelet engedélyezett|8|Beágyazott munkafolyamatok bevonásával bővítheti ezt a határt, igény szerint adhat hozzá|
|Régiónként munkafolyamatok|1000||
|Eseményindítók munkafolyamat száma|10||
|Kapcsoló hatókör esetekben korlátot|25||
|A változók / munkafolyamat száma|250||
|Maximális karakterszám kifejezés|8,192||
|Maximális `trackedProperties` méret karakter|16,000|
|`action`/`trigger`név korlátot|80||
|`description`korlát|256||
|`parameters`korlát|50||
|`outputs`korlát|10||

### <a name="integration-account-limits"></a>Integráció korlátairól

Az alábbiakban integrációs fiók hozzá összetevők korlátairól

|Név|Korlát|Megjegyzések|
|----|----|----|
|Séma|8 MB|Használhat [blob URI](logic-apps-enterprise-integration-schemas.md) 2 MB-nál nagyobb fájlok feltöltéséhez |
|Térkép (XSLT-fájl)|2 MB KÖZÖTT| |
|Futásidejű végpont olvasási hívásszám 5 perc |60,000|Is elosztják a munkaterhelést szükség szerint több fiók|
|Futásidejű végpont meghívása hívásszám 5 perc |45,000|Is elosztják a munkaterhelést szükség szerint több fiók|
|Futásidejű végpont 5 perc hívásszám nyomon követése |45,000|Is elosztják a munkaterhelést szükség szerint több fiók|
|Futásidejű végpont blokkolja az egyidejű hívások |~1,000|Csökkentse az egyidejű kérelmek száma, vagy szükség szerint időtartamának csökkentése érdekében|

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>B2B protokollok (AS2, X12, EDIFACT) üzenet mérete

Az alábbiakban a B2B protokollok korlátai

|Név|Korlát|Megjegyzések|
|----|----|----|
|AS2|50 MB|Dekódolás és kódolása|
|X12|50 MB|Dekódolás és kódolása|
|EDIFACT|50 MB|Dekódolás és kódolása|

## <a name="configuration"></a>Konfiguráció

### <a name="ip-address"></a>IP-cím

#### <a name="logic-app-service"></a>Logic App Service

Közvetlenül a logikai alkalmazás felé indított hívások (Ez azt jelenti, hogy keresztül [HTTP](../connectors/connectors-native-http.md) vagy [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) vagy más HTTP-kérelmekre a következő listában megadott IP-címről származnak:

|Logic App régió|Kimenő IP|
|-----|----|
|Kelet-Ausztrália|13.75.153.66, 104.210.89.222, 104.210.89.244, 13.75.149.4, 104.210.91.55, 104.210.90.241|
|Délkelet-Ausztrália|13.73.115.153, 40.115.78.70, 40.115.78.237, 13.73.114.207, 13.77.3.139, 13.70.159.205|
|Dél-Brazília|191.235.86.199, 191.235.95.229, 191.235.94.220, 191.235.82.221, 191.235.91.7, 191.234.182.26|
|Közép-Kanada|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Kelet-Kanada|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Közép-India|52.172.157.194, 52.172.184.192, 52.172.191.194, 52.172.154.168, 52.172.186.159, 52.172.185.79|
|USA középső régiója|13.67.236.76, 40.77.111.254, 40.77.31.87, 13.67.236.125, 104.208.25.27, 40.122.170.198|
|Kelet-Ázsia|168.63.200.173, 13.75.89.159, 23.97.68.172, 13.75.94.173, 40.83.127.19, 52.175.33.254|
|USA keleti régiója|137.135.106.54, 40.117.99.79, 40.117.100.228, 13.92.98.111, 40.121.91.41, 40.114.82.191|
|USA 2. keleti régiója|40.84.25.234, 40.79.44.7, 40.84.59.136, 40.84.30.147, 104.208.155.200, 104.208.158.174|
|Kelet-Japán|13.71.146.140, 13.78.84.187, 13.78.62.130, 13.71.158.3, 13.73.4.207, 13.71.158.120|
|Nyugat-Japán|40.74.140.173, 40.74.81.13, 40.74.85.215, 40.74.140.4, 104.214.137.243, 138.91.26.45|
|USA északi középső régiója|168.62.249.81, 157.56.12.202, 65.52.211.164, 168.62.248.37, 157.55.210.61, 157.55.212.238|
|Észak-Európa|13.79.173.49, 52.169.218.253, 52.169.220.174, 40.113.12.95, 52.178.165.215, 52.178.166.21|
|USA déli középső régiója|13.65.98.39, 13.84.41.46, 13.84.43.45, 104.210.144.48, 13.65.82.17, 13.66.52.232|
|Délkelet-Ázsia|52.163.93.214, 52.187.65.81, 52.187.65.155, 13.76.133.155, 52.163.228.93, 52.163.230.166|
|Dél-India|52.172.9.47, 52.172.49.43, 52.172.51.140, 52.172.50.24, 52.172.55.231, 52.172.52.0|
|Nyugat-Európa|13.95.155.53, 52.174.54.218, 52.174.49.6, 40.68.222.65, 40.68.209.23, 13.95.147.65|
|Nyugat-India|104.211.164.112, 104.211.165.81, 104.211.164.25, 104.211.164.80, 104.211.162.205, 104.211.164.136|
|USA nyugati régiója|52.160.90.237, 138.91.188.137, 13.91.252.184, 52.160.92.112, 40.118.244.241, 40.118.241.243|
|Az Egyesült Királyság déli régiója|51.140.74.14, 51.140.73.85, 51.140.78.44|
|Az Egyesült Királyság nyugati régiója|51.141.54.185, 51.141.45.238, 51.141.47.136|

#### <a name="connectors"></a>Összekötők

Hívásoknak egy [összekötő](../connectors/apis-list.md) az IP-cím a következő listában megadott származhat:

|Logic App régió|Kimenő IP|
|-----|----|
|Kelet-Ausztrália|40.126.251.213|
|Délkelet-Ausztrália|40.127.80.34|
|Dél-Brazília|191.232.38.129|
|Közép-Kanada|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Kelet-Kanada|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Közép-India|104.211.98.164|
|USA középső régiója|40.122.49.51|
|Kelet-Ázsia|23.99.116.181|
|USA keleti régiója|191.237.41.52|
|USA 2. keleti régiója|104.208.233.100|
|Kelet-Japán|40.115.186.96|
|Nyugat-Japán|40.74.130.77|
|USA északi középső régiója|65.52.218.230|
|Észak-Európa|104.45.93.9|
|USA déli középső régiója|104.214.70.191|
|Délkelet-Ázsia|13.76.231.68|
|Dél-India|104.211.227.225|
|Nyugat-Európa|40.115.50.13|
|Nyugat-India|104.211.161.203|
|USA nyugati régiója|104.40.51.248|
|Az Egyesült Királyság déli régiója|51.140.80.51|
|Az Egyesült Királyság nyugati régiója|51.141.47.105|


## <a name="next-steps"></a>Következő lépések  

- Első lépésként a Logic Apps, kövesse a [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) oktatóanyag.  
- [Gyakori példák és felhasználási helyzetek megtekintése](../logic-apps/logic-apps-examples-and-scenarios.md)
- [Üzleti folyamatok automatizálása a Logic Apps segítségével](http://channel9.msdn.com/Events/Build/2016/T694) 
- [A Logic Apps segítségével történő rendszerintegráció módjainak megismerése](http://channel9.msdn.com/Events/Build/2016/P462)
