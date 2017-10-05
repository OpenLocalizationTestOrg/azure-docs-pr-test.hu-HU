---
title: "Az Azure Functions áttekintése | Microsoft Docs"
description: "Megtudhatja, hogyan optimalizálhat aszinkron számítási feladatokat percek alatt az Azure Functions használatával."
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
ms.openlocfilehash: 4ca35962e9ff8320f74a28b62144e63f3022c6e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="an-introduction-to-azure-functions"></a>Az Azure Functions bemutatása  
Az Azure Functions megoldással egyszerűen futtathatók kisebb kódrészletek, más néven „függvények”, a felhőben. Elég, ha a szóban forgó problémára vonatkozó kódot megírja, nem kell egy egész alkalmazással vagy futtató infrastruktúrával bajlódnia. A függvények használatával még hatékonyabbá válhat a fejlesztés, amelyhez tetszőleges nyelvet használhat, legyen az akár a C#, F#, a Node.js, a Python vagy a PHP. Csak annyi időért kell fizetnie, amennyit a kódja fut, a szükség szerinti méretezést pedig rábízhatja az Azure szolgáltatásra. Az Azure Functions segítségével kiszolgáló nélküli alkalmazások fejleszthetők a Microsoft Azure-on.

A témakor általános áttekintést nyújt az Azure Functions szolgáltatásról. Ha szeretne rögtön az Azure Functions használatának első lépéseihez ugrani, kezdje [Az első Azure-függvény létrehozása](functions-create-first-azure-function.md) résszel. Ha a Functions szolgáltatással kapcsolatos további műszaki információkat keres, lépjen a [fejlesztői segédanyagok](functions-reference.md) részhez.

## <a name="features"></a>Szolgáltatások
Az Azure Functions néhány főbb jellemzője:

* **Nyelvválasztás** – C#, F#, Node.js, Python, PHP, Batch, Bash vagy bármilyen végrehajtható formátum használatával írhat függvényeket.
* **Használatalapú fizetési modell** – Csak annyi időért kell fizetnie, amennyit a kódja fut. Tekintse meg a Használatalapú futtatási csomag lehetőséget a [díjszabás szakaszban](#pricing).  
* **Saját függőségek használata** – A Functions a NuGetet és az NPM-et is támogatja, így Ön a kedvenc könyvtárait használhatja.  
* **Beépített biztonság** – a HTTP-eseményindítókkal aktivált függvényeket olyan OAuth-szolgáltatókkal védheti, mint az Azure Active Directory, a Facebook, a Google, a Twitter és a Microsoft-fiókok.  
* **Egyszerűsített integráció** – Könnyedén kihasználhatja az Azure-szolgáltatások és szolgáltatásként nyújtott szoftverek (SaaS) lehetőségeit. Erre vonatkozó példákat az [integrációs szakaszban](#integrations) talál.  
* **Rugalmas fejlesztés** – A függvényeket közvetlenül a portálon írhatja meg, vagy beállíthat folyamatos integrációt, és a kódját a GitHub, a Visual Studio Team Services vagy egyéb [támogatott fejlesztőeszközök](../app-service-web/web-sites-deploy.md#deploy-using-an-ide) segítségével helyezheti üzembe.  
* **Nyílt forráskód** – A Functions futtatókörnyezete nyílt forráskódú, és [elérhető a GitHubon](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Mire használhatom a Functions szolgáltatást?
Az Azure Functions kiválóan alkalmas adatok feldolgozására, rendszerek integrálására, az eszközök internetes hálózatával (IoT) való munkavégzésre, és egyszerű API-k és mikroszolgáltatások létrehozására. Fontolja meg a Functions használatát olyan műveletekhez, mint a rendszerképek vagy megrendelések feldolgozása, a fájlkarbantartás, illetve az ütemezés szerint futtatni kívánt feladatokhoz. 

A Functions sablonokkal segít megtenni az első lépéseket a legfontosabb forgatókönyvek, köztük az alábbiak esetében:

* **BlobTrigger** – Azure Storage-blobok feldolgozása tárolókhoz való hozzáadáskor. Ez a függvény rendszerképek átméretezéséhez használható.
* **EventHubTrigger** – Válaszadás egy Azure Eseményközponthoz kézbesített eseményekre. Ez különösen hasznos az alkalmazások felépítésében, a felhasználói élmények vagy munkafolyamatok feldolgozásában és az eszközök internetes hálózatát (IoT) érintő forgatókönyvekben.
* **Általános webhook** – Bármely webhookokat támogató szolgáltatás felől érkező webhook HTTP-kérések feldolgozása.
* **GitHub webhook** – Válaszadás a Github-adattárakban történő eseményekre. Egy vonatkozó példáért lásd: [Create a webhook or API function](functions-create-a-web-hook-or-api-function.md) (Egy webhook vagy API-függvény létrehozása).
* **HTTPTrigger** – Kód végrehajtásának aktiválása egy HTTP-kéréssel.
* **QueueTrigger** – Válaszadás az Azure Storage üzenetsorába beérkező üzenetekre. Egy vonatkozó példáért lásd: [Egy Azure-szolgáltatáshoz kötődő Azure-függvény létrehozása](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** – Kód csatlakoztatása egyéb Azure-szolgáltatásokhoz vagy helyi szolgáltatásokhoz az üzenetsor figyelésével. 
* **ServiceBusTopicTrigger** – Kód csatlakoztatása egyéb Azure-szolgáltatásokhoz vagy helyi szolgáltatásokhoz témakörökre való feliratkozással. 
* **TimerTrigger** – Törlés vagy egyéb kötegelt feladatok végrehajtása egy előre meghatározott menetrend szerint. Egy vonatkozó példáért lásd: [Create an event processing function](functions-create-an-event-processing-function.md) (Eseményfeldolgozó függvény létrehozása).

Az Azure Functions támogatja az *eseményindítókat*, amelyek egy kód végrehajtásának elindítását szolgáló módszerek, és a *kötéseket*, amelyek a be- és kimeneti adatok kódolásának leegyszerűsítését szolgáló módszerek. Az Azure Functions által biztosított eseményindítók és kötések részletes leírásáért lásd: [Azure Functions triggers and bindings developer reference](functions-triggers-bindings.md) (Az Azure Functions eseményindítóinak és kötéseinek fejlesztői segédanyagai).

## <a name="integrations"></a>Integráció
Az Azure Functions számos Azure-szolgáltatással és harmadik féltől származó szolgáltatással integrálható. Ezek a szolgáltatások a függvény aktiválására és végrehajtásának megkezdésére, vagy a kódhoz tartozó be- és kimenetként is használhatók. Az Azure Functions a következő szolgáltatásokkal való integrációt támogatja. 

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
Az Azure Functions kétféle díjszabási csomaggal érhető el – válassza azt, amelyik leginkább megfelel az igényeinek: 

* **Használatalapú csomag** – A függvény futásakor az Azure biztosít minden szükséges számítási erőforrást. Önnek nem kell foglalkoznia az erőforrás-kezeléssel, és csak annyi időért kell fizetnie, amennyit a kódja fut. 
* **App Service-csomag** – Függvényeit ugyanúgy futtathatja, ahogy a webes, mobilos és API-alkalmazásait. Ha már más alkalmazásokhoz is használja az App Service szolgáltatást, ugyanabban a csomagban további költségek nélkül futtathatja a függvényeit. 

Az árképzés további részleteiért lásd [a Functions díjszabási oldalát](https://azure.microsoft.com/pricing/details/functions/). A függvények méretezésével kapcsolatos további információkért lásd: [Az Azure Functions méretezése](functions-scale.md).

## <a name="next-steps"></a>Következő lépések
* [Az első Azure-függvény létrehozása](functions-create-first-azure-function.md)  
  Lásson azonnal munkához, és hozza létre az első függvényét az Azure Functions gyorsindítójával. 
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)  
  Részletesebb műszaki információkat tartalmaz az Azure Functions futtatókörnyezettel kapcsolatban, valamint segédanyagokat függvények kódolásához, illetve eseményindítók és kötések meghatározásához.
* [Az Azure Functions tesztelése](functions-test-a-function.md)  
  Különböző függvénytesztelési eszközöket és technikákat ír le.
* [Az Azure Functions méretezése](functions-scale.md)  
  Az Azure Functions szolgáltatáshoz elérhető szolgáltatáscsomagokat ismerteti, köztük a Használatalapú futtatási csomagot, és segít a megfelelő csomag kiválasztásában. 
* [További információ az Azure App Service szolgáltatásról](../app-service/app-service-value-prop-what-is.md)  
  Az Azure Functions az Azure App Service platform használatával biztosítja az olyan alapvető funkciókat, mint az üzembe helyezések, a környezeti változók és a diagnosztika. 

