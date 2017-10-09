---
title: "Azure Mobile Engagement aaaComparing más hasonló Azure-szolgáltatásokkal"
description: "Azure Mobile Engagement összehasonlítva az egyéb hasonló Azure-szolgáltatások - Hockeyappra, az appinsights által biztosított, a Notification Hubs"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f114775-3a9a-4dd4-8d59-b10d1da9a68b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0859ae9980d0fa96ffbc0edfbd795ccc58d2c843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Azure Mobile Engagement összehasonlításával más hasonló Azure-szolgáltatásokkal
Microsoft Azure által kínált szolgáltatások hello listája egyre növekszik, és esetenként Felvetődik hogyan nem egyezik a szolgáltatás, amely csak olvasható vagy szóló értesítésekre az Azure Mobile Engagement. Ez a cikk kísérletet tett tooclear hello zavart, és irányítja toochoose Azure Mobile Engagement leginkább megfelelő a használatra. 

Az Azure Mobile Engagement egy olyan szolgáltatás, kifejezetten megcélzott **a digitális forgalmazók/CMOs** bármelyik használható, de **mobilalkalmazás tulajdonos vagy a közzétevő** ki szeretné tooincrease hello használati, megőrzés és a mobilalkalmazások a jeleníthetnek. 

*A szoftver,--szolgáltatás (SaaS) felhasználóelérési platform, amely alkalmazások használatát, valós idejű felhasználói szegmentálást, adatvezérelt betekintést biztosít, és lehetővé teszi, hogy analitikát leküldéses értesítésekkel és alkalmazáson belüli üzenetek is.* 

Ez a definíció bontásához, a következő főbb jellemzőit hello, is kiemeli az egyedi értékajánlatához kell:

1. **Analitikát leküldéses értesítésekkel és alkalmazáson belüli üzenetek**
   
   Ez a hello core fókusz hello termék - hajtsa végre a megcélzott és a testreszabott leküldéses értesítéseket. És a toohappen a gazdag viselkedéselemzés adatokat gyűjtünk. 
2. **Alkalmazás használatának adatvezérelt betekintést**
   
   Azt adja meg a többplatformos SDK-k toocollect hello viselkedéselemzés hello alkalmazás felhasználói. Vegye figyelembe hello kifejezés viselkedéselemzés (szemben teljesítmény analytics), mert azt összpontosítani hogyan hello alkalmazást használó felhasználókat hello alkalmazás. Alapvető analitikai adatok kapcsolatos hibákat, a összeomlások stb azonban, amely nem hello core fókusz hello termék gyűjtünk. 
3. **Valós idejű felhasználói szegmentálást**
   
   Miután összegyűjtötte alkalmazás felhasználói viselkedés elemzéséhez adatait, azt teszik lehetővé a célközönséget a különböző paraméterek alapján toosegment és az összegyűjtött adatok tooenable céloz meg toorun leküldéses kampányokra. 
4. **Szoftverek,--szolgáltatás (SaaS):**
   
   A Microsoft hello Azure kezelési portálon, amely optimalizált toointeract portál külön és hello app felhasználókról rich viselkedéselemzés megtekintése, és futtassa a leküldéses marketingkampányok. hello termék tooget célja, akkor az nem fog!   

Ez az aktuális megkülönböztetési vannak beállítva, ez hogyan azt összehasonlításhoz más látszólag hasonló Azure-szolgáltatásokkal – hello cikkben egy szolgáltatás szintű összehasonlító nem használ, de mely termék működik a legjobban több forgatókönyv-alapú módszer toodefine fogad:

1. [Hockeyappra](https://azure.microsoft.com/services/hockeyapp/) hello Microsoft mobil DevOps megoldás. Azt terjeszteni buildek toobeta tesztelők webhelyről, összeomlásokkal kapcsolatos adatok gyűjtéséről és felhasználói visszajelzéseket. Azt is integrálható a Visual Studio Team Services engedélyezésével könnyen build telepítések és a munkalem-integrációt. 
   
   DevOps és gyűjtését analytics teljesítményadatainak hello mobilalkalmazások is itt hello hangsúlyt. Akkor fordulhatnak elő mindkét HockeyApps integrálása és a Mobile Engagement az alkalmazás, amely nem lesz szokatlan mivel annak ellenére, hogy néhány hello gyűjtött adatok között átfedés van, hello core hello termékek elsősorban különböző, és annak elérésében eltérő elősegítése célok meg.  
2. [Az Application Insights](../application-insights/app-insights-overview.md) , ha a mobilalkalmazás van a kiszolgálón található, akkor az Application Insights toomonitor hello webes kiszolgálóoldali az alkalmazás használja, de hello ügyféloldali mobil ügyfélalkalmazások, érdemes használni a Hockeyappra. 
3. [A Notification Hubs](https://azure.microsoft.com/services/notification-hubs/) hello értelemben, hogy nem kell az integrált hello mobilalkalmazás SDK és teljes körű hozzáférést engedélyezzenek a mobilalkalmazás lehet, és lehetővé teszi a leküldéses értesítések küldése az alapszintű címkézési képességek egy egyszerűsített szolgáltatást biztosít. Ez az bármely alkalmazás tulajdonost, aki számára fontos kisebb célcsoport-kezelési és kapcsolatos további információk küldése/való kommunikációhoz nagyszerű azonnal tootheir alkalmazás felhasználói (szórási tooa nagy feltöltése). 
   
   Vegye figyelembe a hello helyezi a hangsúlyt itt rendkívül gyors értesítések küldésének alapvető Szegmentálás alkalmas. 

A következőkben néhány olyan forgatókönyvet:

1. TIM hello digitális marketing csapat tesz közzé a felhasználók az Adventure Works üzletben részét képezi. TIM a célja, hogy töltse le a mobilalkalmazások hello felhasználók továbbra is toouse tooensure, és gyakran. Ez nem csak növeli a márka kapcsolódni hello az alkalmazásaik felhasználóit, de növeli jeleníthetnek amikor hello alkalmazás felhasználói vásárlások hello mobilalkalmazás használatával. Az a Tim szüksége lesz a megcélzott toosend értesítések toohello alkalmazásaik felhasználóit, amely akkor hasznosak, tooencourage őket tooopen hello alkalmazást, majd visszatérve tooit gyakran. Ez azért, ahol Tim megkeresi a Mobile Engagement hasznos. 
2. Joann része hello fejlesztői csapat hello az Adventure Works a mobilalkalmazások a termék toolog vonatkozó további információért összeomlik, kivételek, keresése és teljesítmény telemetriai adatainak lekérése egy alkalmazást. Ez azért, ahol Joann megkeresi Hockeyappra hasznos. Joann integrálhat mindkét Hockeyappra, a saját fejlesztői összpontosítani forgatókönyvek és Azure Mobile Engagement a Tim hello az ugyanazon alkalmazás tooget hello legjobb együtt. 
3. Multiplexelés hello fejlesztői csapat hello mobilalkalmazásából Contoso hírek hálózati és az összes toosend hírek riasztások tooall felhasználók sok Szegmentálás nélkül feltörésével, amint hello hírek figyelmeztetés jelenik meg szeretné részét képezi. Ez azért, ahol multiplexelés megkeresi a Notification Hubs hasznos. 
   Ebben a forgatókönyvben lehetőség azonban, hogy hello mobilalkalmazás Miután kiforrottá válik, van egy követelmény toodo hello alkalmazás felhasználói viselkedés jóval szélesebb szegmentáláshoz és az adatait. Ilyenkor multiplexelés tooevaluate Azure Mobile Engagement fog rendelkezni. 

toorecap, hello a Mobile Engagement célja nem csupán toocollect analytics - nem "még egy másik Analytics termékhez a Microsoft". Célzott leküldéses értesítések küldése szól, és a célcsoport-kezelési, a viselkedéselemzés adatokat gyűjtünk, de hello megmarad a küldött leküldéses értesítések, amelyek hello legtöbb logika toohello az alkalmazásaik felhasználóit, hogy azt nem bővítményeként történő kezelését. 

További részletekért – tekintse meg a [gyors videó](mobile-engagement-overview.md) egy nutshell a Mobile Engagement kapcsolatban. 

