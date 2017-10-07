---
title: "aaaConnect alkalmazások és adatok integrálása az Azure Logic Apps-munkafolyamatok – |} Microsoft Docs"
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
ms.openlocfilehash: 53d4e165bb2205ddd56c1950719389725267ddea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-logic-apps"></a>Mi a Logic Apps szolgáltatás?
A Logic Apps adjon meg egy módon toosimplify és méretezhető integrációja és a munkafolyamatok megvalósításához hello felhőben. A vizuális Tervező toomodel biztosít, és a munkafolyamat néven lépések sorozataként folyamat automatizálása.  Nincsenek [sok összekötők](../connectors/apis-list.md) hello felhőalapú és helyszíni tooquickly keresztül kapcsolódik szolgáltatások és protokollok.  Az eseményindító (like "hozzáadásakor fiók tooDynamics CRM") megkezdi a logic app és az indítási műveletek, átalakítás és feltétel logika sok kombinációi megkezdése után.

a Logic Apps használatának előnyei hello hello alábbiakat foglalja magába:  

* Így időt takarít meg által könnyen toounderstand tervezési eszközökkel is kiterjedő összetett folyamatokig tervezése
* Mintákat és munkafolyamatok zökkenőmentesen végrehajtási, amelyek egyébként lenne nehéz tooimplement kódban
* Gyors kezdés sablonok segítségével
* A logikai alkalmazás testre szabható egyéni API-kkal, programkóddal és műveletekkel
* Csatlakozás és szinkronizálás a különféle rendszerek között a helyszíni és felhőalapú hello
* BizTalk Server, API Management, Azure Functions és Azure Service Bus kiépítése első osztályú integrációs támogatással

A Logic Apps egy teljes körűen felügyelt iPaaS (integráció Platform szolgáltatásként) fejlesztők így nem toohave tooworry üzemeltető, méretezhetőség, rendelkezésre állását és felügyeleti kiépítésével foglalkozó.  A Logic Apps lesz automatikusan toomeet igény szerint növelheti.

![Alkalmazásfolyamat-tervező](media/logic-apps-what-are-logic-apps/LogicAppCapture2.png)

Ahogy korábban már említettük, a Logic Apps segítségével automatizálhatja az üzleti folyamatokat. Íme néhány példa erre:  

* Helyezze át a fájlokat feltölteni a tooan FTP-kiszolgáló Azure
* Rendelések feldolgozása és irányítása a helyszíni és felhőrendszerek között
* Minden Twitter-üzeneteket egy bizonyos témakör figyelése, hello véleményeket elemzése és riasztásokat és a feladatokat az elemek kártevővel kapcsolatos követő műveleteket kell létrehozni.

Ehhez hasonló helyzeteknek konfigurálható minden a hello vizuális tervező használatára, és egyetlen sor kód írása nélkül. Kezdje el [összeállítani saját logikai alkalmazását][create].  Ha már megírták, a logikai alkalmazás [gyorsan üzembe helyezhető és újrakonfigurálható](../logic-apps/logic-apps-create-deploy-template.md) több környezetben és régióban is.

## <a name="why-logic-apps"></a>Miért válasszam a Logic Apps szolgáltatást?
A Logic Apps során sebesség és a méretezhetőség hello vállalati integrációs térbe kerülnek.  hello könnyű hello designer, számos elérhető eseményindítók és műveletek és erőteljes felügyeleti eszközökre ellenőrizze központosítja azokat az API-kat egyszerűbb, mint a valaha is.  Vállalatok mozgatása digitalization felé, Logic Apps lehetővé teszi a hagyományos és a legmodernebb rendszerek tooconnect együtt.

Emellett a a [vállalati integrációs fiók] [ biztalk] toomature integrációs feladatok hello hatványa méretezheti a [XML üzenetküldési] [ xml], [kereskedelmipartner-kezelés][tpm], stb.

* **Egyszerű toouse tervezőeszközök** -Logic Apps tervezett-végpontok hello böngésző vagy a Visual Studio eszközök is lehetnek. Indítsa el az eseményindító - a egy egyszerű ütemezés toowhen egy GitHub probléma jön létre. Majd levezényelni tetszőleges számú műveletet hello gazdag gyűjtemény összekötők használatával.
* **API-kat egyszerűen csatlakoztassa** -könnyen toodescribe még akkor is, összeállítási feladatokat is nehéz tooimplement kódban. A Logic Apps segítségével könnyen tooconnect különböző rendszerek. Szeretné, hogy tooconnect felhőalapú marketingmegoldását tooyour helyszíni számlázási rendszer? API-k és a rendszer a egy vállalati Service Bus üzenetkezelés toocentralize szeretné? A Logic apps hello leggyorsabb és legmegbízhatóbb megoldást toodeliver megoldások toothese problémák.
* **Gyors használatbavétel a sablonok** -első lépések toohelp nyújtunk a [sablongalériát] [ templates] , amelyek lehetővé teszik, hogy toorapidly néhány gyakori megoldás létrehozása. A speciális B2B megoldások toosimple SaaS-kapcsolatokról, és akár csak "for visszatöltött" - hello gyűjteménye hello leggyorsabb módon tooget hello power a Logic Apps használatába.
* **Bővíthetőség** -nem látja a hello összekötő van szüksége? A Logic Apps tervezett toowork saját API-k és a kód; egyszerűen létrehozhatja a saját API-alkalmazás toouse, egy egyéni összekötő, vagy hívja az egy [Azure-függvény](https://functions.azure.com) tooexecute kódtöredékek igényű kódot. 
* **Komoly integrációs teljesítmény** – Kezdje könnyedén, és növekedjen az igényekkel együtt. A Logic Apps, amelyekkel könnyedén kialakíthatják a BizTalk, a Microsoft iparági vezető integrációs megoldást tooenable integrációs szakemberek toobuild hello megoldásokat kell hello hatványa. További információk hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>A logikai alkalmazások alapjai
hello az alábbiakban néhány kulcsfontosságú elemeit hello hello Logic Apps élmény alkotó. 

* **Munkafolyamat** -Logic Apps biztosítja a grafikus felületén toomodel az üzleti folyamatok lépéseket vagy a munkafolyamat sorozataként.
* **Felügyelt összekötők** – a logic Apps alkalmazásokat kell a toodata és a szolgáltatás elérését. A felügyelt összekötők kifejezetten tooaid jönnek létre, amikor az adatok tooand csatlakozik. Most már elérhető az összekötők hello tartalmazó lista [felügyelt összekötők][managedapis].
* **Eseményindítók** – Néhány felügyelt összekötő betöltheti az eseményindító szerepét is. Indítsák el egy adott esemény, például egy e-mailben vagy az Azure Storage-fiók változása hello érkezési alapuló munkafolyamat új példányát.
* **Műveletek** -követő egy munkafolyamatban hello eseményindító művelet nevezik. A műveletek általában összekapcsolódnak tooan műveletet a felügyelt összekötő vagy egyéni API-alkalmazások.
* **Enterprise Integration Pack** – A további speciális integrálási forgatókönyvek mmegvalósításaérdekében a Logic Apps BizTalk-képességeket is tartalmaz. A BizTalk a Microsoft piacvezető integrációs platformja. hello vállalati integrációs csomag összekötők lehetővé teszik tooeasily érvényesítési, átalakítás és több tooyour logikai alkalmazás munkafolyamatok tartalmazza.

## <a name="getting-started"></a>Első lépések
* Logic Apps használatába tooget kövesse hello [logikai alkalmazás létrehozása] [ create] oktatóanyag.  
* [Gyakori példák és felhasználási helyzetek megtekintése](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Üzleti folyamatok automatizálása a Logic Apps segítségével](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Megtudhatja, hogyan tooIntegrate a rendszer a Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: logic-apps-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: logic-apps-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: logic-apps-enterprise-integration-accounts.md
[xml]: logic-apps-enterprise-integration-b2b.md
[templates]: logic-apps-use-logic-app-templates.md
