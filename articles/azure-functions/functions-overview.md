---
title: "aaaAzure Functions áttekintése |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Azure Functions toooptimize aszinkron számítási feladatokat perc múlva."
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 668f9055a007fee662a4c7ec774d3c0a1d847800
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-functions"></a>Egy bevezető tooAzure működik  
Az Azure Functions egy megoldással egyszerűen futtathatók kisebb kódrészletek, más néven "függvények" hello felhőben. Írhat csak hello kódot megírja, hello probléma nélkül aggódni egy egész alkalmazás vagy hello infrastruktúra toorun azt van szüksége. A függvények használatával még hatékonyabbá válhat a fejlesztés, amelyhez tetszőleges nyelvet használhat, legyen az akár a C#, F#, a Node.js, a Python vagy a PHP. Csak a fut a hello idő után kell fizetnie, és az Azure tooscale megbízható, igény szerint. Az Azure Functions segítségével kiszolgáló nélküli alkalmazások fejleszthetők a Microsoft Azure-on.

A témakor általános áttekintést nyújt az Azure Functions szolgáltatásról. Ha a kívánt toojump jobbra, és Ismerkedés az Azure Functions, kezdje [az első Azure-függvény létrehozása](functions-create-first-azure-function.md). Ha a Functions szolgáltatással kapcsolatos további műszaki információkat keres, tekintse meg a hello [fejlesztői leírás](functions-reference.md).

## <a name="features"></a>Szolgáltatások
Az Azure Functions néhány főbb jellemzője:

* **Nyelvválasztás** – C#, F#, Node.js, Python, PHP, Batch, Bash vagy bármilyen végrehajtható formátum használatával írhat függvényeket.
* **Fizetési-használati fizetési modell** - kell fizetnie, amennyit csak a hello töltött idő, a kódja. Lásd: hello fogyasztás üzemeltetési terv hello beállítása [díjszabás szakaszban](#pricing).  
* **Saját függőségek használata** – A Functions a NuGetet és az NPM-et is támogatja, így Ön a kedvenc könyvtárait használhatja.  
* **Beépített biztonság** – a HTTP-eseményindítókkal aktivált függvényeket olyan OAuth-szolgáltatókkal védheti, mint az Azure Active Directory, a Facebook, a Google, a Twitter és a Microsoft-fiókok.  
* **Egyszerűsített integráció** – Könnyedén kihasználhatja az Azure-szolgáltatások és szolgáltatásként nyújtott szoftverek (SaaS) lehetőségeit. Lásd: hello [integrációs szakaszban](#integrations) vonatkozó példákat.  
* **Rugalmas fejlesztés** – a függvényeket közvetlenül hello portálon vagy beállíthat folyamatos integrációt és telepítse kódját a GitHub, a Visual Studio Team Services, és egyéb [támogatott Fejlesztőeszközök](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Nyílt forráskódú** -hello Functions futtatókörnyezete nyílt forráskódú és [elérhető a Githubon](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Mire használhatom a Functions szolgáltatást?
Az Azure Functions kiválóan alkalmas adatok feldolgozására, rendszerek használata integrálása hello internetes hálózata (IoT), és egyszerű API-k és mikroszolgáltatások felépítése. Fontolja meg a Functions feladatokat, például a lemezkép vagy a rendelés feldolgozása, a fájlkarbantartás, vagy olyan feladatokat, amelyet az toorun ütemezés szerint. 

A Functions a legfontosabb forgatókönyvek, beleértve a következőket hello használatba sablonok tooget:

* **BlobTrigger** -folyamat Azure Storage-blobok toocontainers hozzáadásakor. Ez a függvény rendszerképek átméretezéséhez használható.
* **EventHubTrigger** -tooevents kézbesíteni tooan Azure Event Hubs válaszol. Ez különösen hasznos az alkalmazások felépítésében, a felhasználói élmények vagy munkafolyamatok feldolgozásában és az eszközök internetes hálózatát (IoT) érintő forgatókönyvekben.
* **Általános webhook** – Bármely webhookokat támogató szolgáltatás felől érkező webhook HTTP-kérések feldolgozása.
* **GitHub webhook** -válaszoljon tooevents, amely a GitHub-adattárakban fordul elő. Egy vonatkozó példáért lásd: [Create a webhook or API function](functions-create-a-web-hook-or-api-function.md) (Egy webhook vagy API-függvény létrehozása).
* **HTTPTrigger** -indul el, egy kód végrehajtásának hello HTTP-kérelem használatával.
* **QueueTrigger** -toomessages válaszol, az Azure Storage üzenetsorába üzeneteknél. Egy vonatkozó példáért lásd: [hozzon létre egy Azure-függvény, amelyhez van Azure-szolgáltatások tooan](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** -csatlakozás Azure-szolgáltatások vagy a helyszíni szolgáltatások figyelő toomessage várólisták által a kód tooother. 
* **ServiceBusTopicTrigger** -csatlakozás a kód tooother Azure-szolgáltatások vagy a helyszíni szolgáltatások tootopics feliratkozással. 
* **TimerTrigger** – Törlés vagy egyéb kötegelt feladatok végrehajtása egy előre meghatározott menetrend szerint. Egy vonatkozó példáért lásd: [Create an event processing function](functions-create-an-event-processing-function.md) (Eseményfeldolgozó függvény létrehozása).

Az Azure Functions támogatja *eseményindítók*, mely módon a kód toostart végrehajtását és *kötések*, amely vannak módon toosimplify kódolásának bemeneti és kimeneti adatokat. Hello eseményindítók és kötések Azure Functions által biztosított részletes ismertetését lásd: [Azure Functions eseményindítók és kötések fejlesztői segédanyagai](functions-triggers-bindings.md).

## <a name="integrations"></a>Integráció
Az Azure Functions számos Azure-szolgáltatással és harmadik féltől származó szolgáltatással integrálható. Ezek a szolgáltatások a függvény aktiválására és végrehajtásának megkezdésére, vagy a kódhoz tartozó be- és kimenetként is használhatók. a következő szolgáltatásokkal való integrációt hello Azure Functions által támogatott. 

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Mobile Apps (táblák)
* Azure Notification Hubs
* Azure Service Bus (üzenetsorok és témakörök)
* Azure Storage (blob, üzenetsorok és táblák) 
* GitHub (webhookok)
* Helyszíni szolgáltatások (a Service Bus használatával)
* Twilio (SMS-üzenetek)

## <a name="pricing"></a>Mennyibe kerül a Functions szolgáltatás használata?
Az Azure Functions kétféle díjszabási csomaggal érhető rendelkezik, válasszon hello ki, amely a leginkább megfelel az igényeinek: 

* **Felhasználás terv** – Ha a függvény fut, az Azure biztosít minden szükséges számítási erőforrást hello. Az erőforrás-kezeléssel tooworry nem rendelkezik, és csak kell fizetnie hello idő, amennyit a kódja fut.. 
* **App Service-csomag** – Függvényeit ugyanúgy futtathatja, ahogy a webes, mobilos és API-alkalmazásait. Amikor az egyéb alkalmazások App Service már használ, a funkciók hello azonos megtervezése további költségek nélkül futtathatja. 

Teljes árképzése érhetők el a hello [Functions díjszabási oldalát](https://azure.microsoft.com/pricing/details/functions/). A függvények méretezésével kapcsolatos további információkért lásd: [hogyan tooscale Azure Functions](functions-scale.md).

## <a name="next-steps"></a>Következő lépések
* [Az első Azure-függvény létrehozása](functions-create-first-azure-function.md)  
  Ugrani, és hozza létre az első függvényét hello Azure Functions gyorsindítójával. 
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)  
  Hello Azure Functions futtatókörnyezettel és hivatkozást kapcsolatos további műszaki információkat biztosít a függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
* [Az Azure Functions tesztelése](functions-test-a-function.md)  
  Különböző függvénytesztelési eszközöket és technikákat ír le.
* [Hogyan tooscale Azure Functions](functions-scale.md)  
  Ismerteti az Azure Functions, beleértve a hello fogyasztás üzemeltetési terv, és hogyan toochoose hello megfelelő csomag elérhető service-csomagokról. 
* [További információ az Azure App Service szolgáltatásról](../app-service/app-service-value-prop-what-is.md)  
  Az Azure Functions hello Azure App Service platform alapvető funkciók, például a központi telepítések, a környezeti változókat és a diagnosztika. 

