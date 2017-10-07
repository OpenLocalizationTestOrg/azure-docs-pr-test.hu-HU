---
title: "aaaAzure esemény rács áttekintése"
description: "Azure Event rács és a fogalmakat ismerteti."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.openlocfilehash: 95dce22e9335df88e81b134143a6c14994c26b8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-tooazure-event-grid"></a>Egy bevezető tooAzure esemény rács

Azure esemény rács tooeasily build alkalmazások esemény-alapú architektúra lehetővé teszi. Azure-erőforrás volna, például a toosubscribe, és adjon meg hello eseménykezelő vagy WebHook végpont toosend hello esemény hello választja. Esemény rács rendelkezik beépített támogatása az Azure-szolgáltatások, például a tárolási BLOB és -erőforráscsoportok származó események. Esemény rács is rendelkeznek, alkalmazás és a külső események egyéni támogatása egyéni témakörök és egyéni webhookokkal használatával. 

Szűrők tooroute adott események toodifferent végpontok, csoportos küldésű toomultiple végpontok használatát, és győződjön meg arról, hogy az események megbízhatóan érkeznek. Esemény rács is rendelkezik beépített egyéni és külső események támogatása.

A hello előzetes kiadását, esemény rács támogatja **westus2** és **westcentralus** helyét. Más régiókban lesz hozzáadva.

Ez a cikk áttekintést Azure esemény rács. Ha azt szeretné, hogy az esemény rács lépései tooget, [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).

![Esemény rács működési modell](./media/overview/event-grid-functional-model.png)

A Blob Storage jelenleg nem nyilvánosan elérhető közzétevő.

## <a name="concepts"></a>Alapelvek

Nincsenek Azure esemény rács, amelyek lehetővé teszik az induláshoz öt fogalmak:

* **Események** – Mi történt.
* **Esemény források-közzétevők** - Ha hello esemény került sor.
* **Témakörök** -hello végpont, ahol közzétevők küldi az eseményeket.
* **Esemény-előfizetések** -hello végpont vagy beépített mechanizmus tooroute események, néha toomultiple kezelők. Előfizetések kezelők toointelligently szűrő bejövő események által is használt.
* **Az eseménykezelők** – hello alkalmazás vagy szolgáltatás reakciójú toohello esemény.

Ezek a fogalmak kapcsolatos további információkért lásd: [Azure esemény rácsban fogalmak](concepts.md).

## <a name="capabilities"></a>Funkciók

Az alábbiakban néhány kulcsfontosságú szolgáltatásokat hello Azure esemény rács:

* **Egyszerűség** -ponton, majd kattintson az Azure-erőforrás tooany eseménykezelő vagy a végpont tooaim események.
* **Speciális szűrési** -szűrőt a esemény típusa vagy esemény közzététele elérési út tooensure eseménykezelők csak a kapcsolódó eseményeket fogadni.
* **Szétterítési** -előfizetés több végpontok toohello azonos esemény toosend másolja át a hello esemény tooas sok helyen igény szerint.
* **Megbízhatóság** -24 órás újra exponenciális leállítási tooensure kézbesíti az eseményeket egy használják.
* **Fizetési / esemény** - kell fizetnie, amennyit csak a hello összeg esemény rács használja.
* **Magas teljesítmény** -esemény rács nagy munkaterhelések létrehozásához, támogatja a több millió esemény / másodperc.
* **Beépített események** - létrehozásához, és gyorsan futtató beépített események erőforrás által definiált.
* **Egyéni események** -esemény rács útvonal, szűrő és megbízhatóan kézbesítése egyéni események használja az alkalmazásban.

## <a name="built-in-publisher-and-handler-integration"></a>Beépített közzétevő és kezelő integrációja

Azure számos szolgáltatásokkal, beleértve a közzétevők és a kezelők esemény beépített támogatást nyújt.

### <a name="publishers"></a>Kiadók

Jelenleg hello Azure-szolgáltatásokat kell beépített publisher az esemény rács:

* Erőforráscsoportok (műveletek)
* Azure-előfizetések (műveletek)
* Event Hubs
* Egyéni kapcsolatos témakörök

Más Azure-szolgáltatásokkal fog bővülni az év.

### <a name="handlers"></a>Kivételkezelők

Jelenleg hello Azure-szolgáltatásokat kell beépített kezelő az esemény rács: 

* Azure Functions
* Logic Apps
* Azure Automation
* Webhook

Más Azure-szolgáltatásokkal fog bővülni az év.

## <a name="what-can-i-do-with-event-grid"></a>Mi a teendő esemény rácshoz?

Azure Event rács több képességeket kínál, amelyek jelentősen javítja a kiszolgáló nélküli, ops automatizálási és integrációs munkahelyi biztosít: 

### <a name="serverless-application-architectures"></a>Kiszolgáló nélküli alkalmazás-architektúrák

![Kiszolgáló nélküli alkalmazást](./media/overview/serverless_web_app.png)

Az Event Grid összeköti az adatforrásokat az eseménykezelőkkel. Egy kiszolgáló nélküli függvény toorun kép elemzés minden alkalommal, amikor egy új fénykép tooa blob storage tárolót fel például, használja a esemény rács tooinstantly eseményindító. 

### <a name="ops-automation"></a>OPS automatizálás

![Műveletek automatizálása](./media/overview/Ops_automation.png)

Esemény rács toospeed automatizálás lehetővé teszi, és egyszerűbb lehet a házirend betartatása. Az Event Grid értesítheti például az Azure Automation szolgáltatást, amikor egy virtuális gép létrejön vagy egy SQL Database működésbe lép. Ezek az események használható tooautomatically ellenőrizze, hogy az szolgáltatáskonfiguráció megfelelőnek metaadatok kerüljenek a műveletek eszközök, a címke virtuális gépek vagy a fájl munkaelemek.

### <a name="application-integration"></a>Alkalmazásintegrálás

![Alkalmazásintegrálás](./media/overview/app_integration.png)

Az Event Grids más szolgáltatásokkal kapcsolja össze alkalmazását. Például hozzon létre egy egyéni témakör toosend az alkalmazás esemény adatok tooEvent rács, és kihasználhatják a megbízható szállítási, speciális útválasztási, és az Azure-integráció közvetlen. Alternatív megoldásként használhatja esemény rács adatokkal Logic Apps tooprocess bárhol, kód írása nélkül. 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>Miben különbözik az esemény rács más Azure integrációs szolgáltatások?

Esemény rács egy esemény csatlakozópanel, amely lehetővé teszi a eseményvezérelt, reaktív programozási. Szorosan integrálódik az Azure-szolgáltatásokkal, és harmadik féltől származó szolgáltatással integrálható. hello eseményüzenetben a szolgáltatások és alkalmazások tooreact toochanges szükséges hello információkat. Esemény rács nincs adatok folyamat, és nem kézbesíti a tényleges objektum hello frissítették.

A Service Bus kiválóan alkalmas tranzakciók, rendezés, kettős észlelés és azonnali konzisztencia igénylő hagyományos vállalati alkalmazások. Esemény rács sebesség, a méretezés, a körének tervezték, és az alacsony költségű reaktív modellben. Kiválóan alkalmas tooserverless architektúra.

Esemény rács kiegészíti a Logic Apps és az Event Hubs például más Azure-szolgáltatásokkal. Rács eseményindítók hello logic app toobegin munkafolyamat. Az Event Hubs azáltal, hogy engedélyezi az Event Hubs rögzítése és build bemenő és átalakítási folyamatok tooreact tooevents esemény rács működik.

## <a name="how-much-does-event-grid-cost"></a>Milyen mértékű nem költség esemény rács?

Azure esemény rács fizetési / esemény árképzési modellt használ, így csak a valóban használt funkciókért fizet.

Esemény rács költségek 0,60 $ millió műveleteket ($0,30 előzetes), és szabadon hello havonta az első 100 000 műveletet. Esemény érkező egyeznek, a kézbesítési kísérletek és a felügyeleti hívások speciális műveleteket is meg van adva.  További részletek megtalálhatók a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Következő lépések

* [Hozzon létre, és feliratkozhat toocustom események](custom-event-quickstart.md) ugrani, és a saját egyéni események tooany végpont hello Azure esemény rács gyors üzembe helyezés küldésének megkezdése.
* [A Logic Apps használatával eseménykezelő](monitor-virtual-machine-changes-event-grid-logic-app.md) használatával Logic Apps tooreact tooevents az alkalmazás elkészítése az oktatóanyag leküldött esemény rács által.
* [Esemény rács REST API-referencia](/rest/api/eventgrid)  
  Hello Azure esemény rács kapcsolatos további műszaki információkat, valamint egy hivatkozást biztosít esemény-előfizetések kezeléséhez az Útválasztás és a szűrés.
