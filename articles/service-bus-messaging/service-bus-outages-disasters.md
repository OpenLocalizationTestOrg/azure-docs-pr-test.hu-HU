---
title: "Azure Service Bus-alkalmazást aaaInsulating kimaradások és vészhelyzetek ellen |} Microsoft Docs"
description: "Egy lehetséges a Service Bus leállás tooprotect alkalmazásokat használható megoldásokat ismerteti."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: fd9fa8ab-f4c4-43f7-974f-c876df1614d4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2017
ms.author: sethm
ms.openlocfilehash: 349b4968456c9f15375753d83495246f5a3ddfdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>A Service Bus kimaradások és vészhelyzetek alkalmazások szigetelő ajánlott eljárásai
Kritikus fontosságú alkalmazások kell működniük folyamatosan, még akkor is nem tervezett leállások vagy vészhelyzetek hello jelenlétét. Ez a témakör ismerteti a használható tooprotect Service Bus-alkalmazást a potenciális szolgáltatáskimaradás vagy katasztrófa megoldásokat.

Nem tervezett kimaradás hello elérhetetlenség az Azure Service Bus típusúként van definiálva. hello kimaradás hatással lehet egyes összetevői a Service Bus, például üzenetküldési tárolóban, vagy még akkor is, hello egész adatközpont. Hello a probléma kijavítása után a Service Bus ismét elérhetővé válik. Nem tervezett kimaradás általában nem okoznak üzenetek vagy egyéb adatok elvesztését. Példa összetevőhiba: hello hiányában egy adott üzenetküldési tárolóban. Példa egy datacenter kiterjedő leállás áramkimaradás hello datacenter, vagy egy hibás datacenter hálózati kapcsoló:. Nem tervezett kimaradás néhány perc tooa a tarthat néhány nap múlva.

Egy olyan vészhelyzet esetén a Service Bus skálázási egység vagy datacenter hello végleges adatvesztést típusúként van definiálva. hello datacenter is, vagy előfordulhat, hogy nem ismét elérhetővé válik. Egy olyan vészhelyzet esetén általában néhány vagy az összes üzenetek vagy egyéb adatok elvesztését okozza. Katasztrófák példák tűz, elárasztás vagy földrengés.

## <a name="current-architecture"></a>Aktuális architektúrája
A Service Bus több üzenetküldési tároló toostore küldött üzenetek tooqueues vagy témakörök használ. Nem particionált üzenetsor vagy témakör tooone üzenetküldési tárolóban van hozzárendelve. Az üzenetküldési tárolóban nem érhető el, ha az adott üzenetsor vagy témakör összes művelet sikertelen lesz.

Az összes Service Bus üzenetküldési entitásokat (üzenetsorok, témakörök, továbbítók) egy névtér, amely tagja-e a datacenter találhatók. A Service Bus nem engedélyezi az automatikus georeplikációt adatok, és lehetővé teszi azt, hogy a névtér toospan több adatközpontot.

## <a name="protecting-against-acs-outages"></a>ACS kimaradások elleni védelem
Ha ACS hitelesítő adatokat használ, és az ACS nem érhető el, az ügyfelek már nem kaphatnak jogkivonatokat. ACS leáll hello időpontban jogkivonat rendelkező ügyfelek továbbra is toouse Service Bus hello jogkivonatok lejártáig. hello alapértelmezett a jogkivonatok élettartama 3 óra.

ACS kimaradások elleni tooprotect használjon közös hozzáférésű Jogosultságkód (SAS) tokeneket. Ebben az esetben hello ügyfél végzi a hitelesítést közvetlenül a Service Bus egy önálló minted jogkivonat titkos kulcsával aláírásával. Hívások tooACS már nem szükségesek. További információ a SAS-tokenje: [Service Bus hitelesítési][Service Bus authentication].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Üzenetsorok és témakörök üzenetküldési tároló hibák elleni védelme
Nem particionált üzenetsor vagy témakör tooone üzenetküldési tárolóban van hozzárendelve. Az üzenetküldési tárolóban nem érhető el, ha az adott üzenetsor vagy témakör összes művelet sikertelen lesz. A várólista, a hello ugyanakkor particionálva, több töredék áll. Minden egyes töredék egy másik üzenetküldési tárolóban van tárolva. Egy üzenet elküldésekor tooa particionált üzenetsor vagy témakör, a Service Bus hello üzenet tooone hello töredékek rendeli hozzá. Hello megfelelő üzenetküldési tárolóban nem érhető el, ha a Service Bus írja az hello üzenet tooa különböző töredék, ha lehetséges. Particionált entitások kapcsolatos további információkért lásd: [particionált üzenetküldési entitások][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Védelem datacenter kimaradások vagy vészhelyzetek ellen
két adatközpont között feladatátvevő tooallow, létrehozhat egy Service Bus-szolgáltatásnévtér mindkét adatközpont. Például a Service Bus-szolgáltatásnévtér hello **contosoPrimary.servicebus.windows.net** hello az Amerikai Egyesült Államok északi/központi régióban, lehet, hogy található és **contosoSecondary.servicebus.windows.net**lehet, hogy hello Velünk Dél/központi régióban található. Ha egy Service Bus üzenetküldési entitás egy adatközpontban szolgáltatáskimaradás hello jelenlétét a hozzáférhetőnek kell maradnia, mindkét névtérre is létrehozhat, hogy az entitás.

További információkért lásd a "Service Bus egy Azure-adatközpontban belül hiba esetén" című hello [aszinkron üzenetkezelési mintákat és magas rendelkezésre állású][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Datacenter valamilyen okból kimaradás vagy vészhelyzetek ellen továbbítási végpontok védelme
A továbbítási végpontok georeplikáció lehetővé teszi, hogy egy szolgáltatás, amely elérhetővé teszi a továbbítási végpont toobe érhető el a Service Bus kimaradások hello jelenlétét. tooachieve georeplikáció, hello szolgáltatás létre kell hoznia két továbbítási végpontok különböző névterekben. hello névterek különböző adatközpontokban kell lennie, és két végpontot hello eltérőnek kell lennie. Például egy elsődleges végpont elérhető alatt **contosoPrimary.servicebus.windows.net/myPrimaryService**, míg a másodlagos párjukhoz alatt elérhető **contosoSecondary.servicebus.windows.net/mySecondaryService**.

hello szolgáltatás majd figyeli mindkét végponton, és egy ügyfél hívhat meg hello szolgáltatást vagy a végpont. Egy ügyfélalkalmazás véletlenszerűen szerzi hello továbbítók egyikét, az elsődleges végpont hello, és elküldi a kérelmet toohello aktív végpontja. Hibakódú hello művelet sikertelen lesz, ha a hiba azt jelzi, adott hello továbbítási végpontra nem érhető el. hello alkalmazás megnyílik egy csatorna toohello biztonsági mentési végpontot, és reissues hello kérelem. Ezen a ponton hello aktív és a biztonsági mentési végpontok hello vált szerepkörök: hello ügyfélalkalmazás úgy ítéli meg, hello régi aktív végpontja toobe hello új biztonsági mentési végpont, és hello régi biztonsági mentési végpont toobe hello új aktív végpontot. Ha mindkét küldött műveletek sikertelenek, hello szerepkörök hello két entitások változatlan marad, és hibát ad vissza.

Hello [georeplikálási a Service Bus továbbítón keresztüli üzenetek] [ Geo-replication with Service Bus relayed Messages] példa bemutatja, hogyan tooreplicate továbbítja.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Üzenetsorok és témakörök datacenter valamilyen okból kimaradás vagy vészhelyzetek ellen védi
tooachieve rugalmasság datacenter valamilyen okból kimaradás során használatával közvetítőalapú üzenettovábbítás ellen, a Service Bus támogatja a két módszer: *aktív* és *passzív* replikációs. Az egyes módszert használja Ha az adott üzenetsor vagy témakör egy adatközpontban szolgáltatáskimaradás hello jelenlétét a hozzáférhetőnek kell maradnia, a hozhatja létre mindkét névtérre. Mindkét entitásnak hello is ugyanazzal a névvel. Elsődleges queue alatt elérhető például **contosoPrimary.servicebus.windows.net/myQueue**, míg a másodlagos párjukhoz alatt elérhető **contosoSecondary.servicebus.windows.net/myQueue**.

Ha hello alkalmazás nincs szükség folyamatos küldő fogadó kommunikációt, hello alkalmazás tartós ügyféloldali várólista tooprevent üzenet adatvesztéssel és tooshield hello feladójának a Service Bus átmeneti hiba is létrehozható.

## <a name="active-replication"></a>Aktív replikációs
Aktív replikációs mindkét névtérre entitások minden művelethez használja. Bármely ügyfél által küldött üzenet küldi hello két példánya ugyanazt az üzenetet. hello első alkalommal küldött toohello elsődleges entitás (például **contosoPrimary.servicebus.windows.net/sales**), és üdvözlőüzenetére hello másodpéldányának toohello másodlagos entitás küldi el (például  **contosoSecondary.servicebus.windows.net/sales**).

Egy ügyfél üzeneteket fogad mindkét várólisták. hello fogadó folyamatok hello első példányát egy üzenetet, és hello másodpéldány le van tiltva. toosuppress duplikált üzenetek, hello küldő címkét kell egy egyedi azonosítóval rendelkező minden üzenetet. Mindkét üdvözlőüzenetére példányát kell címkézését hello ugyanezzel az azonosítóval. Használhatja a hello [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] vagy [BrokeredMessage.Label] [ BrokeredMessage.Label] tulajdonságait, vagy egy egyéni tulajdonság tootag hello üzenet. hello fogadó kezelnie kell az üzeneteket, amelyek már kapott.

Hello [a Service Bus Közvetítőalapú üzenetek georeplikáció] [ Geo-replication with Service Bus Brokered Messages] az üzenetküldési entitások aktív replikációs mutatja be.

> [!NOTE]
> hello aktív replikációs megközelítés megduplázódik hello műveletek számát, ezért ez a megközelítés vezethet toohigher költség.
> 
> 

## <a name="passive-replication"></a>Passzív replikáció
Hello hibamentesen esetben passzív replikációs csak egyikét használja hello két üzenetküldési entitások. Egy ügyfél küldi hello üzenet toohello aktív entitás. Ha hello aktív entitás hello művelet sikertelen lesz, és azt jelzi, hogy nem érhető el lehet-e a gazdagépek hello aktív entitás hello datacenter hibakódot, hello ügyfél hello üzenet toohello biztonsági mentési entitás példányát küldi. Ezen a ponton hello aktív és a biztonsági mentési entitásokat hello vált szerepkörök: hello küldő ügyfél hello régi aktív entitás toobe hello új biztonsági mentési entitás figyelembe veszi, és hello régi biztonsági mentési entitás hello új aktív entitás. Ha mindkét küldött műveletek sikertelenek, hello szerepkörök hello két entitások változatlan marad, és hibát ad vissza.

Egy ügyfél üzeneteket fogad mindkét várólisták. Mivel valószínűséggel adott hello fogadó azonos üzenet fogadásakor, hello hello két példányt kap fogadó a duplikált üzenetek kell elrejtése. Tilthatja le lehet azonos hello hasonlóan az aktív replikációs.

Passzív replikációs általában gazdaságosabb, mint az aktív replikációs, mert a legtöbb esetben csak egy művelet. Késés, az átviteli sebesség és a pénzügyi költsége azonos toohello nem replikált forgatókönyv.

Passzív replikációs használatakor hello a következő forgatókönyvek üzenetek néha elvész, vagy kétszer érkezett:

* **Üzenet késleltetés vagy adatvesztés**: feltételezi, hogy hello küldő sikeresen elküldte-e egy m1 toohello elsődleges üzenetsor, és ezután hello várólista nem érhető el előtt hello fogadó m1 kap. a küldő hello rákövetkező állapotüzenetet m2 toohello másodlagos várólista. Hello elsődleges várólista átmenetileg nem érhető el, ha hello fogadó m1 kap, miután hello várólista újra lesz elérhető. Egy vészhelyzet esetén hello fogadó soha nem jelenhet meg m1.
* **Ismétlődő fogadása**: azt feltételezik, hogy hello küldő m toohello elsődleges várólistához. A Service Bus sikeresen m dolgozza fel, de nem sikerül toosend választ. Miután hello küldési művelet időtúllépést, hello küldő m toohello másodlagos várólista azonos példányának. Ha hello fogadó képes tooreceive hello első másolás m előtt hello elsődleges queue elérhetetlenné válik, a hello fogadó körülbelül hello m mindkét példányt kap ugyanannyi időt vesz igénybe. Ha hello fogadó nem tud tooreceive hello első másolás m előtt hello elsődleges queue elérhetetlenné válik, a hello fogadó kezdetben csak hello második másolatot kap m, de m másodpéldányának majd kap, amikor elsődleges queue hello elérhetővé válik.

Hello [georeplikálási a Service Bus közvetítőalapú üzenetek] [ Geo-replication with Service Bus Brokered Messages] az üzenetküldési entitások passzív replikációs mutatja be.

## <a name="next-steps"></a>Következő lépések
További információ a vész-helyreállítási toolearn lásd: ezek a cikkek:

* [Az Azure SQL Database üzletmenet folytonossága][Azure SQL Database Business Continuity]
* [Az Azure-rugalmas alkalmazások tervezése][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[Geo-replication with Service Bus Relayed Messages]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency
