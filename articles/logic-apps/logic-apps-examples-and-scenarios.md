---
title: "aaaExamples & Gyakori forgatókönyvek - Azure Logic Apps |} Microsoft Docs"
description: "További információk a logic apps az oktatóanyagok, példák és forgatókönyvek"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/9/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 17caa8539ec6a57726b9c6c07a71fb74caa07ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="examples-and-common-scenarios-for-azure-logic-apps"></a>Példák és az Azure Logic Apps gyakori forgatókönyvei

További tájékoztatást kaphat toohelp hello számos mintákat és képességek az Azure Logic Apps, az alábbiakban gyakori példák és forgatókönyvek.

## <a name="key-scenarios-for-logic-apps"></a>A logic apps főbb forgatókönyvek

Az Azure Logic Apps rugalmas vezénylési és integrációs különböző szolgáltatásokhoz biztosít. Logic Apps szolgáltatás hello "kiszolgáló nélküli", a, a méretezési és példányok tooworry nincs – összes rendelkezik toodo hello munkafolyamat (eseményindítók és műveletek) megadása. hello alapul szolgáló platform kezeli a méretezés, a rendelkezésre állás és teljesítmény. Olyan forgatókönyv, ahol kell toocoordinate, több művelet különösen meg több rendszerben, az Azure Logic Apps egy nagy-és nagybetűhasználattal. Íme, néhány mintákat és a példákat.

## <a name="respond-tootriggers-and-extend-actions"></a>Tootriggers válaszol, és műveletek kiterjesztése

Minden logikai alkalmazás egy eseményindító kezdődik. Például a munkafolyamat elindíthatja egy ütemezés esemény, manuális hívása vagy egy külső rendszer, az esemény például a "Ha nincs hozzáadva fájl tooan FTP-kiszolgáló" eseményindító hello. Az Azure Logic Apps jelenleg több mint 100 kész használható összekötők, a helyszíni SAP tooMicrosoft kognitív szolgáltatások közötti. Rendszerekhez és a szolgáltatások, előfordulhat, hogy nem közzétett összekötők logic Apps alkalmazásokat is kiterjeszthető.

* [Hozzon létre egyéni eseményindítók és műveletek](../logic-apps/logic-apps-create-api-app.md)
* [A munkafolyamat futtatása hosszú ideig futó műveletek](../logic-apps/logic-apps-create-api-app.md)
* [Válaszoljon tooexternal eseményeket és műveleteket a webhookok](../logic-apps/logic-apps-create-api-app.md)
* [Hívja, eseményindító, vagy az aszinkron válaszok tooHTTP által munkafolyamatok egymásba](../logic-apps/logic-apps-http-endpoint.md)
* [Oktatóanyag: TooTwilio SMS webhookok válaszol, és a szöveg visszajelzés](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)
* [Oktatóanyag: AI táplált közösségi irányítópult létrehozása a Logic Apps és a Power BI percben](http://aka.ms/logicappsdemo)

## <a name="error-handling-logging-and-control-flow-capabilities"></a>Hibakezelés, naplózási és ellenőrzési folyamata képességek

A Logic apps speciális vezérlési folyamatában, például a feltételeket, kapcsolók, hurkokat és hatókörök gazdag képességeit tartalmazza. tooensure rugalmas megoldások, is megvalósíthatja hiba és a kivételkezelő a munkafolyamatokban. Értesítés és a munkafolyamat futtatási állapot diagnosztikai naplók Azure Logic Apps is biztosít figyelése és a riasztásokat.

* [E kapcsoló utasításokat különböző műveleteket](../logic-apps/logic-apps-switch-case.md)
* [A tömbök és gyűjteményeket, amelyek a hurok- és a logic apps kötegek folyamat elemek](../logic-apps/logic-apps-loops-and-scopes.md)
* [Szerző hiba és a kivételkezelő munkafolyamat](../logic-apps/logic-apps-exception-handling.md)
* [A figyelés, a naplózás vagy a meglévő logic Apps alkalmazások riasztások bekapcsolása](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [A logic apps létrehozásakor figyelési és diagnosztikai naplózás bekapcsolása](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)
* [Használati eset: hogyan használja az egészségügyi vállalati a logic app kivételkezelő HL7 FHIR munkafolyamatokhoz](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

## <a name="deploy-and-manage-logic-apps"></a>Logikai alkalmazások telepítését és kezelését

Teljes körű fejlesztése és logikai alkalmazások telepítése a Visual Studio, a Visual Studio Team Services, vagy bármely más verziókezelő és automatizált összeállítási eszközök. a munkafolyamatok és a függő kapcsolatok egy erőforrás-sablonban toosupport központi telepítését, a logic apps Azure-erőforrás központi telepítési sablonok használatával. Visual Studio eszközök automatikusan létre ezeket a sablonokat, amelyek ellenőrizheti a verziókövetés toosource vezérlőben.

* [Az automatikus központi telepítési sablon létrehozása](../logic-apps/logic-apps-create-deploy-template.md)
* [Hozza létre és logikai alkalmazások Visual Studio telepítése](../logic-apps/logic-apps-deploy-from-vs.md)
* [A logic apps hello állapotának figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Tartalomtípusokat, átalakítás és átalakítások futtató belül

Elérheti, alakítsa át, és több tartalomtípus átalakítási hello segítségével számos funkciót az Azure Logic Apps hello [munkafolyamatdefiníciós nyelve](http://aka.ms/logicappsdocs). Például átválthat egy karakterlánc, a JSON és a XML között a hello `@json()` és `@xml()` munkafolyamat-kifejezések. hello Logic Apps motor tartalomtípusok toosupport tartalomátviteli megőrzi a szolgáltatások közötti veszteségmentes módon.

* [Nem JSON tartalom kezelésére](../logic-apps/logic-apps-content-type.md), például `application/xml`, `application/octet-stream`, és`multipart/formdata`
* [A logic apps a munkafolyamat-kifejezések működése](../logic-apps/logic-apps-author-definitions.md)
* [Útmutató: Az Azure Logic Apps munkafolyamatdefiníciós nyelve](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Más integrációja és képességek

A Logic apps is kínál számos szolgáltatás, például az Azure Functions, az Azure API Management, az Azure App Service szolgáltatások és az egyéni HTTP-végpontokról, például a REST és a SOAP való integráció.

* [Hozzon létre egy valós idejű közösségimédia irányítópultot Azure kiszolgáló nélküli](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Az Azure Functions hívható meg a logic Apps alkalmazásokból](../logic-apps/logic-apps-azure-functions.md)
* [Forgatókönyv: Eseményindító logic Apps alkalmazások az Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Blog: Hívja a logic Apps alkalmazásokból SOAP-végpontok](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Végpontok közötti forgatókönyvek

* [Tanulmány: Vállalati integrációs végpont nagybetűk kezelése az Azure-szolgáltatásokkal, például a Logic Apps](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="next-steps"></a>Következő lépések

- [Hibákat és kivételeket a logic Apps alkalmazásokat kezeléséhez](../logic-apps/logic-apps-exception-handling.md)
- [Szerző munkafolyamat-definícióhoz a hello munkafolyamatdefiníciós nyelve](../logic-apps/logic-apps-author-definitions.md)
- [Küldje el a megjegyzéseket, a kérdéseket, a visszajelzések és a javaslatok, milyen azt látna Azure Logic Apps alkalmazások számára](https://feedback.azure.com/forums/287593-logic-apps)