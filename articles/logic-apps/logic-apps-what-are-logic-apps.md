---
title: "Alkalmazások összekapcsolása és adatok integrálása munkafolyamatokkal – Azure Logic Apps | Microsoft Docs"
description: "Hozzon létre munkafolyamatokat, és automatizálja a folyamatokat az alkalmazások összekapcsolása és az adatok integrálása révén az Azure Logic Apps segítségével."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 07765c05-72a6-4169-a8ab-f6420bfbaf07
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: klam
ms.openlocfilehash: 59d35852d6c703f3c96089a8bf426b57660441a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="what-are-logic-apps"></a>Mi a Logic Apps szolgáltatás?
A Logic Apps lehetőséget teremt a skálázható integrálások és munkafolyamatok felhőbe implementálásának leegyszerűsítésére és implementálása. Egy vizuális tervezőt biztosít a folyamatai lépéssorozatként, más néven munkafolyamatként történő modellezésére és automatizálására.  Sok helyszíni és felhőben található [összekötő](../connectors/apis-list.md) létezik, amelyeket gyorsan integrálni lehet a szolgáltatások és protokollok között.  A logikai alkalmazás egy eseményindítóval kezdődik (például „amikor egy fiókot hozzáadnak a Dynamics CRM-hez”), és az indítás után számos műveleti, konverziós és feltétellogikai kombinációt kezdhet.

A Logic Apps használatának előnyei többek között a következők:  

* Idő spórolható meg, mert az összetett folyamatok tervezése könnyen átlátható tervezőeszközökkel történik
* Minták és munkafolyamatok zökkenőmentes megvalósítása, amelyek amúgy nehezen implementálhatók lennének a programkódban
* Gyors kezdés sablonok segítségével
* A logikai alkalmazás testre szabható egyéni API-kkal, programkóddal és műveletekkel
* Különálló rendszerek összekapcsolása és szinkronizálása a helyszíni rendszer és a felhő között
* BizTalk Server, API Management, Azure Functions és Azure Service Bus kiépítése első osztályú integrációs támogatással

A Logic Apps egy teljesen felügyelt iPaaS (szolgáltatásként nyújtott integrációs platform), amely lehetővé teszi a fejlesztők számára, hogy ne kelljen aggódniuk az üzemeltetés, a méretezhetőség és a felügyelet kiépítése miatt.  A Logic Apps automatikusan vertikálisan felskálázódik, hogy kielégítse az igényeket.

![Alkalmazásfolyamat-tervező](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Ahogy korábban már említettük, a Logic Apps segítségével automatizálhatja az üzleti folyamatokat. Íme néhány példa erre:  

* FTP-kiszolgálóra feltöltött fájlok áthelyezése az Azure Storage-ba
* Rendelések feldolgozása és irányítása a helyszíni és felhőrendszerek között
* Egy megadott témához tartozó tweetek figyelése, vélemények elemzése, valamint riasztások és feladatok létrehozása utókövetést igénylő elemekhez.

Az ehhez hasonló helyzeteknek megfelelő konfiguráláshoz mindössze a vizuális tervező szükséges, egyetlen sor kódot sem kell írnia. Kezdje el [összeállítani saját logikai alkalmazását][create].  Ha már megírták, a logikai alkalmazás [gyorsan üzembe helyezhető és újrakonfigurálható](../logic-apps/logic-apps-create-deploy-template.md) több környezetben és régióban is.

## <a name="why-logic-apps"></a>Miért válasszam a Logic Apps szolgáltatást?
A Logic Apps sebességet és méretezhetőséget biztosít a vállalati integráció terén.  A tervező könnyű használata, a számos elérhető eseményindító és művelet, valamint a sokoldalú kezelőeszközök teszik minden eddiginél könnyebbé az API-k központosítását.  Ahogy a vállalkozások a digitalizáció felé mozdulnak el, a Logic Apps lehetővé teszi a régi és a legmodernebb rendszerek összekapcsolását.

Továbbá a [nagyvállalati integrációs fiókokkal][biztalk] részletes integrációs forgatókönyvekre is méretezhet az [XML-üzenetküldés][xml], a [kereskedelmipartner-kezelés][tpm] és további szolgáltatások kihasználásával.

* **Könnyen használható tervezőeszközök** – A Logic Apps teljes körűen megtervezhető a böngészőben vagy a Visual Studio-eszközökkel. Indítás eseményindítóval – egy egyszerű ütemezéstől a GitHub-problémák létrehozásáig is eljuthat. Ezt követően az összekötők széles választéka segítségével tetszőleges számú műveletet vezényelhet le.
* **API-k könnyű csatlakoztatása** – Még a könnyen leírható, de összetett feladatokat is nehéz a programkódban implementálni. A Logic Apps megkönnyíti a különböző rendszerek összekapcsolását. Szeretné összekapcsolni felhőalapú marketingmegoldását a helyszíni számlázórendszerrel? Szeretné központosítani az API-k és rendszerek közötti üzenetküldést egy Enterprise Service Bus segítségével? A Logic Apps a lehető leggyorsabb és legmegbízhatóbb megoldást kínálja ezekre a feladatokra.
* **Gyors használatbavétel a sablonok révén** – Annak érdekében, hogy megkönnyítsük a program használatának első lépéseit, összeállítottunk egy [sablongalériát][templates], amelynek alapján számos gyakran előforduló megoldás is rendkívül gyorsan megalkotható. A speciális B2B-megoldásoktól az egyszerű SaaS-kapcsolatokig és néhány olyanig, amely csak kedvtelésből készült – a katalógus a leggyorsabb módja a Logic Apps képességei megismerésének.
* **Beépített bővíthetőség** – Nem találja a szükséges összekötőt? A Logic Appst arra tervezték, hogy együttműködjön a saját API-kkal és programkóddal. Egyszerűen létrehozhatja és használhatja saját API-alkalmazását egyéni összekötőként, vagy meghívhatja egy [Azure-függvényben](https://functions.azure.com) a kódtöredékek igény szerinti végrehajtásához. 
* **Komoly integrációs teljesítmény** – Kezdje könnyedén, és növekedjen az igényekkel együtt. A Logic Apps a BizTalk, a Microsoft piacvezető integrációs megoldásának erejével eszközöket ad az integrációs szakemberek kezébe, amelyekkel könnyedén kialakíthatják a saját igényeiknek leginkább megfelelő megoldásokat. További információk az [Enterprise Integration Packről](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>A logikai alkalmazások alapjai
Az alábbiakban a Logic Apps használatának kulcsfontosságú elemeit emeljük ki. 

* **Munkafolyamat** – A Logic Apps grafikus felületén lépések sorozataként vagy munkafolyamatként modellezheti az üzleti folyamatokat.
* **Felügyelt összekötők** – A logikai alkalmazásoknak hozzá kell férniük az adatokhoz és a szolgáltatásokhoz. A felügyelt összekötők azzal a céllal jöttek létre, hogy segítséget nyújtsanak, amikor az adataihoz csatlakozik vagy azokkal dolgozik. A jelenleg elérhető összekötők listája a [felügyelt összekötők][managedapis] alatt található meg.
* **Eseményindítók** – Néhány felügyelt összekötő betöltheti az eseményindító szerepét is. Az eseményindítóknak az a szerepük, hogy egy adott esemény bekövetkezésekor (például e-mail érkezésekor vagy az Azure-tárfiókban bekövetkezett változáskor) a munkafolyamat új példányát indítsák el.
* **Műveletek** – Az eseményindítót követő lépéseket műveleteknek nevezzük. Minden művelet általában egy másik művelethez vezet a felügyelt összekötőn vagy egyéni API-alkalmazásokban.
* **Enterprise Integration Pack** – A további speciális integrálási forgatókönyvek mmegvalósításaérdekében a Logic Apps BizTalk-képességeket is tartalmaz. A BizTalk a Microsoft piacvezető integrációs platformja. Az Enterprise Integration Pack-összekötők lehetővé teszik, hogy egyszerűen belefoglaljon érvényesítést, átalakítást és még több lehetőséget a Logic App-munkafolyamatokba.

## <a name="getting-started"></a>Első lépések
* Az első lépések megismeréséhez olvassa el a [Logikai alkalmazás létrehozása][create] című oktatóanyagot.  
* [Gyakori példák és felhasználási helyzetek megtekintése](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Üzleti folyamatok automatizálása a Logic Apps segítségével](http://channel9.msdn.com/Events/Build/2016/T694) 
* [A Logic Apps segítségével történő rendszerintegráció módjainak megismerése](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-web-overview.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md
