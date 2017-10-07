---
title: az Azure Service Bus alapjai aaaOverview |} Microsoft Docs
description: "Egy bevezető toousing Service Bus tooconnect Azure-alkalmazások tooother szoftver."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 12654cdd-82ab-4b95-b56f-08a5a8bbc6f9
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: sethm
ms.openlocfilehash: 1abd5cf310ef06ba35e1e2489a7c0a07e1797736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-bus"></a>Azure Service Bus

Hogy egy alkalmazás vagy szolgáltatás hello felhőben vagy helyszínen fut, gyakran kell más alkalmazásokkal vagy szolgáltatásokkal rendelkező toointeract. egy körben használható megoldást toodo tooprovide a, a Microsoft Azure-ajánlatok Service Bus. Ez a cikk ellenőrzi, hogy az ezt a technológiát, mi az, és miért érdemes toouse leíró azt.

## <a name="service-bus-fundamentals"></a>Service Bus fundamentals (A Service Bus alapjai)

A különféle helyzetekben különféle stílusú kommunikáció lehet szükséges. Előfordulhat Ha az alkalmazások egy egyszerű üzenetsoron keresztül üzeneteket küldjön és fogadjon egy hello legjobb megoldás. Más helyzetekben a hagyományos üzenetsorok nem elegendőek, és a közzétételi-előfizetési mechanizmus a jobb megoldás. Egyes esetekben mindössze kapcsolatra van szükség az alkalmazások között, és nincs szükség üzenetsorokra. A Service Bus mindhárom lehetőséget biztosítja, az alkalmazások toointeract engedélyezése több különböző módon.

A Service Bus egy több-bérlős felhőszolgáltatás, ami azt jelenti, hogy hello szolgáltatást több felhasználó által közösen. Minden felhasználó, például az alkalmazásfejlesztő, létrehoz egy *névtér*, majd határozza meg az adott névtérben szükséges hello kommunikációs mechanizmus. Az 1. ábra ezt az architektúrát mutatja be.

![][1]

**1. ábra: A Service Bus egy több-bérlős szolgáltatást alkalmazások hello felhő keresztül csatlakozó biztosít.**

Egy adott névtéren belül a négy különböző kommunikációs mechanizmus egy vagy több példányát használhatja, amelyek mindegyike különböző módokon kapcsol össze alkalmazásokat. hello lehetőségek közül választhat:

* *Üzenetsorok*, amelyek egyirányú kommunikációt tesznek lehetővé. Az egyes üzenetsorok *közvetítőként* működnek, amely az üzeneteket tárolja a fogadásukig. Mindegyik üzenetet egyetlen fogadó fogadja.
* *Témakörök*, amelyek egyirányú kommunikációt tesznek lehetővé *előfizetések* segítségével – egy témakör több előfizetéssel is rendelkezhet. Az üzenetsorokhoz hasonlóan a témakörök is közvetítőként, de egyes előfizetések használhatnak, a szűrő tooreceive üzenetek adott feltételeknek.
* *Továbbítók*, amelyek kétirányú kommunikációt tesznek lehetővé. Az üzenetsoroktól és témaköröktől eltérően a továbbító nem tárolja átvitel közben az üzeneteket, azaz nem működik közvetítőként. Ehelyett egyszerűen továbbítja azokat a cél alkalmazás toohello.

Amikor létrehoz egy üzenetsort, témakört vagy továbbítót, el kell neveznie azt. Kombinálva így a névtér, ez a név egyedi azonosítót hoz létre hello objektum. Alkalmazások adja meg a név tooService Bus, majd az adott várólista, a témakörben vagy a továbbítási toocommunicate egymással. 

Ezek hello továbbítási forgatókönyv objektumokat toouse, a Windows alkalmazások használhatják a Windows Communication Foundation (WCF). Ez a szolgáltatás a [WCF Relay](../service-bus-relay/relay-what-is-it.md). Az üzenetsorok és a témakörök esetében a Windows-alkalmazások a Service Bus által definiált üzenettovábbítási API-kat használhatják. toomake ezeket könnyebb toouse objektumok nem Windows alkalmazásokból, a Microsoft SDK-kat, Java, Node.js és egyéb nyelvek biztosít. Az üzenetsorokat és a témaköröket [REST API-k](/rest/api/servicebus/) használatával is elérheti HTTP(s)-en keresztül. 

Fontos, még akkor is, ha a Service Bus maga toounderstand hello felhőben fut (Ez azt jelenti, hogy a Microsoft Azure adatközpontjaiban), azt használó alkalmazások bárhol futhatnak. A Service Bus tooconnect alkalmazásokat futtató Azure, például vagy a saját adatközpontjában futó alkalmazásokat is használhat. Is használhatja tooconnect Azure vagy egy másik futó alkalmazást egy helyszíni alkalmazással vagy táblagépek és telefonok felhő platform. Tooconnect akár háztartási készülékeket, érzékelőket, és egyéb eszközök tooa központi alkalmazáshoz vagy más tooone. A Service Bus egy kommunikációs mechanizmus, amely lényegében bárhonnan elérhető hello felhőben. A használatának módja függ milyen a alkalmazásokat kell toodo.

## <a name="queues"></a>Üzenetsorok

Tegyük fel, hogy úgy dönt, hogy tooconnect két alkalmazás egy Service Bus-üzenetsorba. A 2. ábra ezt a helyzetet mutatja be.

![][2]

**2. ábra: A Service Bus-üzenetsorok egyirányú aszinkron sorkezelést biztosítanak.**

hello folyamat felettébb egyszerű: A küldő egy üzenetet tooa Service Bus-üzenetsorba, és a fogadó egy későbbi időpontban fogadja az üzenetet. Az egyes üzenetsorok rendelkezhetnek egyetlen fogadóval, amint az a 2. ábrán látható. Vagy több alkalmazás is olvashat a hello ugyanazon a várólistán. Hello ez utóbbi esetben minden üzenetet csak egyetlen fogadó olvassa. A csoportos küldési szolgáltatáshoz inkább témakört használjon.

Mindegyik üzenet két részből áll: egy sor tulajdonságból, amelyek mindegyike egy kulcs/érték pár, valamint az üzenet hasznos adattartalmából. bináris, text vagy még akkor is, XML hello hasznos lehet. Hogyan mire szolgál attól függ, hogy milyen kérelmet próbált toodo. Például a legutóbbi értékesítésről üzenetet küldő alkalmazás tartalmazhat hello tulajdonságok **értékesítő = "Ava"** és **összeg = 10000**. hello üzenettörzs tartalmazhat aláírt hello értékesítési szerződés beolvasott képét, vagy, ha nincs ilyen, üres marad.

A fogadó kétféleképpen olvashatja az üzeneteket a Service Bus-üzenetsorból. első lehetőség, hello  *[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, eltávolítja az üzenetet hello várólistából, és azonnal törli. Ez a beállítás nem egyszerű, de ha hello fogadó összeomlik, mielőtt befejezné az üdvözlő üzenet feldolgozása, üdvözlőüzenetére elvesznek. Az hello várólista lett távolítva, mert más fogadó nem férhet hozzá. 

második lehetőség hello  *[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, a probléma megoldásához toohelp célja. Például **ReceiveAndDelete**, egy **PeekLock** olvasási hello várólistából eltávolítja az üzenetet. Üdvözlőüzenetére, azonban nem törli. Ehelyett üdvözlőüzenetére, így láthatatlan tooother fogadók zárolja, majd vár az alábbi három esemény valamelyikének:

* Ha hello fogadó folyamatok sikeresen hello üzenetet, meghívja [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), és hello üzenetsor törli üdvözlőüzenetére. 
* Hello fogadó úgy dönt, hogy nem tud sikeresen is üdvözlőüzenetére feldolgozni, ha meghívja [Abandon()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon). hello várólista majd hello zárolási eltávolítja üdvözlőüzenetére, és elérhető tooother fogadók teszi.
* Ha a fogadó hello ezen módszerek nem konfigurálható időn belül (alapértelmezés szerint 60 másodperc), hello üzenetsor feltételezi, hogy hello fogadó meghibásodott. Ebben az esetben úgy viselkedik, mintha hello fogadó meghívta volna **Abandon**, így az üzenet elérhető tooother fogadók hello.

Figyelje meg, mi történhet ebben: hello ugyanazon üzenet kézbesítése kétszer is megtörténhet, akár tootwo különböző fogadónak. A Service Bus-üzenetsorokat használó alkalmazásokat fel kell készíteni erre az esetre. toomake kettős észlelés egyszerűbb, mindegyik üzenet rendelkezik egy egyedi [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) tulajdonság, amely alapértelmezés szerint mindig fennmarad hello azonos, függetlenül attól, hogy hányszor üdvözlőüzenetére olvasható az üzenetsorból. 

Az üzenetsorok számos helyzetben lehetnek hasznosak. Lehetővé teszik az alkalmazások toocommunicate akkor is, ha mindkettő nem futó hello azonos időben, amelyet a kötegelt és mobilalkalmazások különösen hasznos. A több fogadóval rendelkező üzenetsorok emellett automatikus terheléselosztást is biztosítanak, mivel a küldött üzenetek megoszlanak a fogadók között.

## <a name="topics"></a>Témakörök

Bármennyire hasznosak is, az üzenetsorok nem mindig ideális megoldás a hello. Esetenként célszerűbb Service Bus-témaköröket használni. A 3. ábra ezt az elképzelést mutatja be.

![][3]

**3. ábra: Az előfizető alkalmazás hello szűrő alapján, megkaphatja a néhány vagy összes köszönőüzenetei tooa Service Bus-témakörbe küldött.**

A *témakör* hasonló számos módon tooa várólistában. Feladók küldhetnek üzeneteket tooa témakörében hello azonos módon, hogy az üzenetek tooa várólista elküldenék, és ezek üzenetek megjelenését hello ugyanaz, mint az üzenetsorok. hello különbség az, hogy a témakörök mindegyik fogadó alkalmazás toocreate engedélyezése a saját *előfizetés* meghatározhat egy *szűrő*. Az előfizető látja csak olyan köszönőüzenetei, amelyek megfelelnek a szűrőnek. A 3. ábrán például egy küldő és egy 3 előfizetővel rendelkező témakör látható, mely előfizetők mindegyike saját szűrővel rendelkezik:

* 1. előfizető csak a hello tulajdonságot tartalmazó üzeneteket fogad *értékesítő = "Ava"*.
* 2. előfizető fogadja az üzeneteket, hello tulajdonságot tartalmazó *értékesítő = "Ruby"* és/vagy tartalmaznak egy *összeg* tulajdonsága, amelynek értéke nagyobb, mint 100 000. Lehet, hogy Ruby helyzet hello értékesítési igazgató, mind a saját értékesítéseit, és a személyétől függetlenül minden nagy értékesítési toosee szeretné.
* 3. előfizető a szűrőt túl állított*igaz*, ami azt jelenti, hogy minden üzenetet megkap. Például lehet, hogy az alkalmazás naplózásért felelős, és ezért kell toosee összes köszönőüzenetei.

Mint az üzenetsorok, előfizetők tooa témakörben olvashatja használatával [ReceiveAndDelete és PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode). Az üzenetsoroktól eltérően azonban egyetlen üzenetben küldött tooa témakör több előfizetéssel is fogadhatja. Ezt a módszert, más néven *közzététel és előfizetés* (vagy *pub/sub*), akkor hasznos, amikor több alkalmazás is érdeklődik hello ugyanazon üzenetek. Hello megfelelő szűrő meghatározásával mindegyik előfizető üzenetfolyamnak, hogy kell-e toosee hello üzenetstream csak hello részét.

## <a name="relays"></a>Továbbítók

Az üzenetsorok és a témakörök egyaránt egyirányú aszinkron kommunikációt tesznek lehetővé egy közvetítőn keresztül. A forgalom csak egy irányban folyik, és nincs közvetlen kapcsolat a küldők és a fogadók közt. De mi történik, ha nem szeretné ezt a kapcsolatot? Tegyük fel, hogy az alkalmazások kell tooboth üzeneteket küldjön és fogadjon, vagy esetleg közöttük közvetlen hivatkozást szeretne, és nem kell a broker toostore üzeneteket. tooaddress például a Service Bus forgatókönyvek *továbbítja*, mert a 4. ábrán látható.

![][4]

**4. ábra: A Service Bus Relay kétirányú szinkron kommunikációt tesz lehetővé az alkalmazások között.**

hello kapcsolatban felmerül a nyilvánvaló kérdés tooask ez: Miért használnék ilyet? Akkor is, ha nincs szükségem üzenetsorokra, miért ellenőrizze a közvetlen interakció helyett csak egy felhőszolgáltatáson keresztül kommunikáljanak az alkalmazások? hello választ ki kell, hogy van szó közvetlenül lehet nehezebb, mint érdemes.

Tegyük fel, hogy azt szeretné, hogy a két tooconnect a helyszíni alkalmazások, mindkettő vállalati adatközpontban fut. Mindkét alkalmazás egy tűzfal mögött található, és mindkét adatközpont valószínűleg hálózati címfordítást (NAT) használ. hello tűzfal blokkolja néhány portok, valamint NAT kivételével az összes bejövő adatokat azt jelenti, hogy minden alkalmazáshoz futó hello a gép nem rendelkezik rögzített IP-címnek, amely közvetlenül a külső hello datacenter érhető el. Külön segítség nélkül ezeknek az alkalmazásoknak hello keresztül csatlakozó nyilvános internet jelent problémát.

A Service Bus Relay használata ezt megkönnyítheti. két irányban keresztüli, minden egyes alkalmazás toocommunicate egy kimenő TCP-kapcsolatot létesít a Service Bus, és nyitva tartja azt. Hello két alkalmazás közötti minden kommunikáció ezeken a kapcsolatokon keresztül halad. Mivel mindegyik kapcsolat létesült a hello adatközponton belül hello tűzfala lehetővé teszi a bejövő forgalom tooeach alkalmazás felé új portok megnyitása nélkül. Ez a megközelítés megkerüli hello NAT miatti problémát, mivel mindegyik alkalmazás állandó végponttal rendelkezik hello felhőben hello kommunikáció során. Keresztül hello továbbítási adatok kicserélésével hello alkalmazások volna egyébként megnehezítenék a kommunikációt hello problémák elkerülése érdekében. 

toouse Service Bus továbbítja, akkor az alkalmazások hello Windows Communication Foundation (WCF). A Service Bus, melyek a Windows-alkalmazások toointeract keresztül egyszerű WCF-kötéseket biztosít. Alkalmazásokat, amelyek már használják a WCF is általában adja meg az egyik ilyen kötést, majd beszélgetés tooeach más továbbítón keresztül. Az üzenetsoroktól és témaköröktől eltérően a továbbítók használata nem Windows alkalmazásokból – bár lehetséges – némi programozást igényel, mivel nem állnak rendelkezésre szabványos könyvtárak.

Az üzenetsoroktól és témaköröktől eltérően az alkalmazások nem hoznak létre kifejezetten továbbítókat. Ehelyett tooreceive üzenetek szándékozó alkalmazás TCP-kapcsolatot a Service busszal létesít, amikor a továbbító automatikusan létrejön. Ha a hello kapcsolat megszakad, hello továbbítási törlődik. egy adott hallgató, a Service Bus által létrehozott egy alkalmazás toofind hello továbbítási tooenable egy jegyzéket biztosít, amely lehetővé teszi az alkalmazások toolocate megtalálhatják név szerint.

Továbbítók is hello ideális megoldás, ha közvetlen kommunikációra az alkalmazások között van szüksége. Vegyük például egy légitársaság foglalási rendszerét, amely egy olyan helyszíni adatközpontban fut, amelynek elérhetőnek kell lennie bejelentkezési pultokról, mobileszközökről és egyéb számítógépekről. Ezek a rendszerek a futó alkalmazások hello felhő toocommunicate, a Service Bus-továbbítók támaszkodhat, amikor csak lehetséges, hogy fut.

## <a name="summary"></a>Összefoglalás

Az alkalmazások összekapcsolása mindig az, hogy a teljes megoldások kialakításának része, és hello számos forgatókönyv esetén van szükség az alkalmazások és szolgáltatások toocommunicate egymással értéke tooincrease további alkalmazások és eszközök vannak csatlakoztatott toohello internet. Felhőalapú technológiákat biztosítva elérése érdekében az üzenetsorok, témakörök és továbbítók a kommunikáció, a Service Bus célja toomake az alapvető fontosságú funkciót könnyebben tooimplement és szélesebb körben elérhetővé.

## <a name="next-steps"></a>Következő lépések

Most, hogy megismerte az Azure Service Bus alapjai hello, kövesse az alábbi hivatkozások további toolearn.

* Hogyan toouse [Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md)
* Hogyan toouse [Service Bus-témakörök](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* Hogyan toouse [Service Bus-továbbító](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
* [Service Bus-minták](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
