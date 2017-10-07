---
title: "az Azure kiszolgáló nélküli aaaOverview |} Microsoft Docs"
description: "Hatékony megoldások létrehozása hello felhőben anélkül, hogy az infrastruktúrával kapcsolatos toothink."
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
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7c9c09d96e472edd1631892982ac60aae97342a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-serverless-with-functions-and-logic-apps"></a>Azure-kiszolgáló nélküli funkciók és a Logic Apps áttekintése

Kiszolgáló nélküli alkalmazások fejlesztését sebesség növekedése, szükséges kódot, és amely egyszerűség csökkentése előnyei kínálnak.  Ez a cikk hello különböző attribútumokat kiszolgáló nélküli megoldások, és az Azure kiszolgáló nélküli ajánlatok állapotba kerül.

## <a name="what-is-serverless"></a>Mi az a kiszolgáló nélküli?

Kiszolgáló nélküli nem jelenti azt, nincsenek kiszolgálók – csak akkor hello developer-kiszolgálókkal kapcsolatos tooworry nem rendelkezik.  A hagyományos alkalmazásfejlesztés nagy része válaszol kérdések skálázás, üzemeltető és figyelési megoldások toomeet hello hello alkalmazás követelményeinek.  A kiszolgáló nélküli ezeket a kérdéseket vannak végrehajtott megvagyunk hello megoldás részeként.  Emellett kiszolgáló nélküli alkalmazások számlázása a fogyasztás alapján terv.  Hello alkalmazás soha nem használatos, ha egy ingyenesen elérhető soha nem merül fel.  Ezeket a szolgáltatásokat a fejlesztők toofocus kizárólag a hello üzleti logika hello megoldás.

hello alapvető szolgáltatások az Azure-kiszolgáló nélküli körül [Azure Functions](https://azure.microsoft.com/services/functions/) és [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Ezek a megoldások kövesse a fenti hello elveket, mind a fejlesztők toobuild robusztus felhőalkalmazásokhoz minimális kóddal.

## <a name="what-are-azure-functions"></a>Mik az Azure Functions?

Az Azure Functions egy megoldással egyszerűen futtathatók kisebb kódrészletek, más néven "függvények" hello felhőben. Írhat csak hello kódot megírja, hello probléma nélkül aggódni egy egész alkalmazás vagy hello infrastruktúra toorun azt van szüksége. Funkciók is legyen még hatékonyabbá, és a fejlesztési nyelvi szerkesztőprogramban, például a C#, F #, Node.js, Python vagy a PHP is használhatja. Csak a fut a hello idő után kell fizetnie, és Azure méretezi igény szerint.

Ha a kívánt toojump jobbra, és Ismerkedés az Azure Functions, kezdje [az első Azure-függvény létrehozása](../azure-functions/functions-create-first-azure-function.md). Ha a Functions szolgáltatással kapcsolatos további műszaki információkat keres, tekintse meg a hello [fejlesztői leírás](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Mik az Azure Logic Apps?

Az Azure Logic Apps tartalmaz egy módja toosimplify és méretezhető integrációja és a munkafolyamatok megvalósításához hello felhőben. A vizuális Tervező toomodel biztosít, és automatizálható a folyamat munkafolyamat nevű lépések sorozataként.  Nincsenek [sok összekötők](../connectors/apis-list.md) felhőalapú és helyszíni szolgáltatásban tooquickly csatlakozzon egy kiszolgáló nélküli alkalmazást tooother API-k.  Az eseményindító (like "hozzáadásakor fiók tooDynamics CRM") megkezdi a logic app és az indítási sok kombinációk műveletek, átalakítás és feltétel logika megkezdése után.  A Logic Apps akkor kiváló választás, ha különböző Azure Functions - folyamatban futó, különösen akkor, ha egy külső rendszer vagy az API-val való interakció hello folyamathoz az szükséges.

tooget lépések, kezdje [az első logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md).  Ha a Logic Apps kapcsolatos további műszaki információkat keres, tekintse meg a hello [fejlesztői leírás](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Hogyan felépítéséhez és az Azure-ban kiszolgáló nélküli alkalmazások telepítéséhez?

Azure fejlesztési, telepítési és felügyeleti kiszolgáló nélküli alkalmazások lehetővé teszi az eszközök széles skáláját.  Alkalmazások közvetlenül hello Azure-portálon, vagy a építhetők [tooling eszköz, a Visual Studio eszközből](logic-apps-serverless-get-started-vs.md).  Ha egy alkalmazás lehet [azonnal telepített](logic-apps-create-deploy-template.md).  Azure is biztosít a kiszolgáló nélküli alkalmazások figyelését.  A figyelés elérhető az Azure-portálon keresztül hello API-t vagy az SDK-k, vagy integrált tooling tooOMS és az Application Insights hello.

## <a name="next-steps"></a>Következő lépések

* [Első lépések a Visual Studio egy kiszolgáló nélküli alkalmazást felépítése](logic-apps-serverless-get-started-vs.md)
* [Hozzon létre egy felhasználói irányítópult kiszolgáló nélküli](logic-apps-scenario-social-serverless.md)
* [A központi telepítési sablont a logikai alkalmazás létrehozása](logic-apps-create-deploy-template.md)