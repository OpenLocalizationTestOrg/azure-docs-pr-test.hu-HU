---
title: "aaaScenario - hozzon létre egy felhasználói irányítópult Azure kiszolgáló nélküli |} Microsoft Docs"
description: "Példa bemutatja, hogyan hozhat létre irányítópult toomanage ügyfél visszajelzést, közösségi adatok és további Azure Logic Apps és az Azure Functions."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: db175e895e37aa795a9c34bf4d65566bf68f8c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Az Azure Logic Apps és az Azure Functions egy valós idejű felhasználói irányítópult létrehozása

Azure-kiszolgáló nélküli eszközök alkalmazások biztosítása a hatékony képességekkel tooquickly build és a gazdagép hello felhőben anélkül, hogy az infrastruktúrával kapcsolatos toothink.  Ebben az esetben a rendszer létrehoz egy irányítópult tootrigger az ügyfelek visszajelzései, visszajelzés machine Learning segítségével elemezheti, és közzététele insights egy forrás, például a Power bi-ban vagy az Azure Data Lake.

## <a name="overview-of-hello-scenario-and-tools-used"></a>Hello forgatókönyv és a használt eszközök áttekintése

A rendezés tooimplement ebben a megoldásban, azt fogja használni a legfontosabb összetevők hello két kiszolgáló nélküli alkalmazások az Azure-ban: [Azure Functions](https://azure.microsoft.com/services/functions/) és [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

A Logic Apps egy kiszolgáló nélküli munkafolyamat-motor hello felhőben.  Vezénylési biztosít a kiszolgáló nélküli összetevői között, és tooover 100 szolgáltatások és API-kat is csatlakozik.  A jelen esetben az ügyfelek visszajelzései létrehozunk egy logic app tootrigger.  Hello összekötők, amelyek segítik a reakciójú toocustomer visszajelzés többek között Outlook.com-os, az Office 365, a felmérést Monkey, a Twitter és a HTTP-kérelem [egy webes űrlap](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  Az alábbi hello munkafolyamat azt figyeli egy hashtaggel történő a Twitteren.

Funkciók adja meg a kiszolgáló nélküli számítási hello felhőben.  Ebben a forgatókönyvben használjuk az Azure Functions tooflag Twitter-üzeneteket az ügyfelektől, előre definiált kulcsszavakat sorozata alapján.

hello teljes megoldás lehet [a Visual Studio build](logic-apps-deploy-from-vs.md) és [erőforrás sablon részeként](logic-apps-create-deploy-template.md).  Szerepel továbbá hello forgatókönyv bemutató videó [a Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-hello-logic-app-tootrigger-on-customer-data"></a>Hello logic app tootrigger ügyféladatok létrehozása

Miután [logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md) a Visual Studio vagy hello Azure-portálon:

1. Adja hozzá az eseményindító **az új Twitter-üzeneteket** a Twitteren
2. Hello eseményindító toolisten tootweets konfigurálása kulcsszó vagy hashtaggel történő.

   > [!NOTE]
   > hello eseményindító hello ismétlődési tulajdonság határozza meg, hogy milyen gyakran hello logikai alkalmazás ellenőrzi a lekérdezési alapú eseményindítók új elemek

   ![Twitter eseményindító – példa][1]

Ez az alkalmazás most már az összes új Twitter-üzeneteket fog érvényesítést.  Azt majd tweetet adatok igénybe vehet, és részletesebb kifejezett hello véleményeket.  A hello használjuk [Azure kognitív szolgáltatás](https://azure.microsoft.com/services/cognitive-services/) szöveg toodetect céggel kapcsolatos véleményeket.

1. Kattintson a **új lépés**
1. Válassza ki, vagy keresse meg a hello **Szövegelemzések** összekötő
1. Jelölje be hello **észlelése céggel kapcsolatos véleményeket** művelet
1. Ha a rendszer kéri, adjon meg egy érvényes kognitív szolgáltatások kulcs hello Szövegelemzések szolgáltatás
1. Adja hozzá a hello **Tweetet szöveg** , szöveges tooanalyze hello.

Most, hogy hello tweetet adatokat és az elemzések a hello tweetet, más összekötőket vonatkozó lehet:
* Power BI - sorok tooStreaming adatkészlet hozzáadása: valós idejű egy Power BI-irányítópult nézet Twitter-üzeneteket.
* Azure Data Lake - fájl hozzáfűzése: felhasználói adatok tooan Azure Data Lake dataset tooinclude hozzáadása az analytics-feladatok.
* SQL - sorok felvételének: adatok tárolása a későbbi beolvasásához adatbázis.
* Slack - üzenetet küldeni: egy közzététele a slack-csatornát negatív visszajelzés műveletek igénylő riasztást.

Egy Azure-függvény használt toodo hello adatok felett számítási további egyéni is lehet.

## <a name="enriching-hello-data-with-an-azure-function"></a>Egy Azure-függvény hello adatok bővítése

Azt is hozzon létre egy függvényt, igazolnia kell toohave egy függvény alkalmazást az Azure-előfizetésben.  Egy Azure-függvény létrehozásának hello portálon is [itt található](../azure-functions/functions-create-first-azure-function-azure-portal.md)

Egy függvény toobe hívása közvetlenül egy logikai alkalmazást, az azt kell toohave HTTP indítható el, kötés.  Azt javasoljuk, hello **HttpTrigger** sablont.

Ebben a forgatókönyvben a hello kérelem törzse hello Azure-függvény lenne hello tweetet szöveg.  Hello függvény kódban egyszerűen meg logika Ha hello tweetet szöveg tartalmazza-e egy kulcsszót vagy kifejezést.  hello függvény önmagára sikerült megmarad, mint egyszerűek vagy összetettek, hello a forgatókönyvhöz szükséges.

Hello függvény hello végén egyszerűen vissza válasz toohello logikai alkalmazás adatokkal.  Ennek oka lehet egy egyszerű logikai értéket (pl. `containsKeyword`), vagy egy összetett objektumot.

![Konfigurált Azure-függvény lépés][2]

> [!TIP]
> Egy logikai alkalmazás függvényt az összetett választ elérésekor művelettel hello elemzése JSON.

Miután hello függvény mentettük, a fentiekben létrehozott hello logika alkalmazásba vehető fel.  A hello logic app:

1. Kattintson a tooadd egy **új lépés**
1. Jelölje be hello **Azure Functions** összekötő
1. Válasszon egy meglévő függvény toochoose, és keresse meg a létrehozott toohello függvény
1. Hello küldése **Tweetet szöveg** a hello **kérelem törzsét.**

## <a name="running-and-monitoring-hello-solution"></a>Futtatnia vagy felügyelnie hello megoldás

Jelentéskészítő kiszolgáló nélküli álló üzenettípusok összehangolását a Logic Apps előnyeit hello egyik hello hatékony hibakeresési és figyelési képességek.  A Futtatás (jelenlegi vagy történelmi) tekintheti meg a Visual Studio hello Azure-portálon vagy REST API hello és SDK-k segítségével.

Hello legegyszerűbb módja tootest logikai alkalmazás egyike hello használja **futtatása** hello Designer gombra.  Kattintson a **futtatása** továbbra is toopoll hello eseményindító minden 5 másodperc, amíg a rendszer észlelt egy eseményt, és futtassa hello előrehaladtával egy élő betekintést.

Előző futtatási alábbi előzményeinek hello áttekintése panelen hello Azure-portálon, vagy a Visual Studio Cloud Explorer hello segítségével tekintheti meg.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Az automatikus központi telepítés központi telepítési sablon létrehozása

Ha olyan megoldást fejlesztett ki, rögzített, és egy Azure-telepítés sablon tooany hello world az Azure-régiót keresztül telepíthetők.  Ez akkor hasznos, mindkét módosítása paraméter különböző a munkafolyamat-verziót, hanem is használható az ebben a megoldásban egy build és kiadás folyamat.  A központi telepítési sablonok létrehozásának részletes ismertetése [ebben a cikkben](logic-apps-create-deploy-template.md).

Az Azure Functions is építhető hello központi telepítési sablont -, így hello teljes megoldás az összes függősége kezelhető egyetlen sablont.  Példa függvény a központi telepítési sablont hello található [Azure gyors üzembe helyezés sablon tárház](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Következő lépések

* [Tekintse meg az egyéb példák és forgatókönyvek az Azure Logic Apps](logic-apps-examples-and-scenarios.md)
* [Tekintse meg a video-útmutatót a megoldás-végpontok létrehozásáról](http://aka.ms/logicappsdemo)
* [Megtudhatja, hogyan toohandle és catch kivételek logikai alkalmazás belül](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png