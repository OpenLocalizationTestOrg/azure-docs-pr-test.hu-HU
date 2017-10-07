---
title: az AMQP 1.0 az Azure Service Bus aaaOverview |} Microsoft Docs
description: "További információ használatáról hello speciális Message Queuing protokoll (AMQP) 1.0 az Azure-ban."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/27/2017
ms.author: sethm
ms.openlocfilehash: c35a20c50dd24845d9a4c7a0251e8240848a6f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="amqp-10-support-in-service-bus"></a>A Service Bus AMQP 1.0-támogatás
Mindkét hello Azure Service Bus cloud service és a helyszíni [Service Bus a Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) támogatási hello speciális üzenet Várólistázást protokoll (AMQP) 1.0-s. AMQP lehetővé teszi, hogy Ön toobuild platformok közötti, egy megnyitott szabványos protokollt használó hibrid alkalmazások. Alkalmazás-összetevők, amelyek különböző nyelv és keretrendszer használatával készített, és a különböző operációs rendszereken futó, hogyan hozhat létre. Ezek az összetevők tooService Bus csatlakozhat, és problémamentesen üzenetváltásra strukturált üzleti hatékonyan és teljes visszaadása.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Bemutatása: Mi az az AMQP 1.0-s, és miért fontos?
Hagyományosan üzenet célú termékekre használt saját protokoll ügyfél-alkalmazások és a brókerek közötti kommunikációhoz. Ez azt jelenti, hogy Miután kijelölt egy adott szállító üzenetkezelési broker, kell használnia a gyártó által készített szalagtárak tooconnect az ügyfél alkalmazások toothat broker. Az eredmény egy bizonyos fokú függés forgalmazóval, az eljárás egy alkalmazás tooa másik termékhez szükséges az összes kapcsolódó hello alkalmazás kódmódosításokat. 

Ezenkívül üzenetkezelő közvetítők csatlakozás a különböző szállítóktól származó legbonyolultabb. Ehhez általában alkalmazásszintű áthidaló toomove üzeneteit egy rendszer tooanother és a szellemi tulajdont képező üzenetformátumok közötti tootranslate. Ez feltétele közös; például amikor kell adjon meg egy új egyesített felület tooolder különböző rendszerek, vagy az informatikai rendszerek, a következő egyesülés integrálni.

hello szoftvergyártói ágazat a gyorsan változó üzleti; néha bewildering ütemben bevezetett új programozási nyelveket és alkalmazás-keretrendszerek számára. Ehhez hasonlóan az informatikai rendszerek hello követelményeinek verzióinformációk, és a fejlesztők szeretné hello legújabb platform szolgáltatásainak tootake előnyeit. Azonban néha hello kiválasztott üzenetkezelési szállító nem támogatja a ezekről a platformokról. Mert üzenetküldési protokoll saját fejlesztésű, nincs lehetőség mások tooprovide szalagtárak ezek új platformokat. Ezért például felépítése átjárók vagy hidak, amelyek lehetővé teszik a toocontinue toouse hello üzenetkezelési termék megközelítések kell használnia.

a speciális Message Queuing protokoll (AMQP) 1.0 indokolta, hogy ezek a problémák hello hello fejlesztéséhez. A JP Morgan Chase, akik, például a pénzügyi szolgáltatásokat cégek, nehéz felhasználók üzenet célú köztes származik. hello cél egyszerű volt: egy nyílt szabvány szerinti üzenetkezelési protokollt, hogy a lehetséges toobuild üzenet-alapú alkalmazások több különböző nyelvet, keretrendszerek és operációs rendszerek használatával készített összetevők toocreate összes használata ajánlott a fajta-összetevőt egy Szállítók tartományán.

## <a name="amqp-10-technical-features"></a>Műszaki AMQP 1.0-szolgáltatásokat
AMQP 1.0 nem használható toobuild robusztus, platformfüggetlen, üzenetküldő alkalmazások hatékony, megbízható, vezetékes szintű üzenetkezelési protokoll. hello protokollnak a egyszerű célja van: toodefine hello idejéről hello biztonságos, megbízható és hatékony üzenetátvitel két fél között. a hordozható adatok megjelenítése, amely lehetővé teszi a heterogén küldő és strukturált tooexchange üzleti üzenetekben teljes visszaadása a maguk köszönőüzenetei kódolt. hello az alábbiakban látható hello legfontosabb funkciókról összefoglalása:

* **Hatékony**: AMQP 1.0 egy kapcsolat alapú protokoll, hogy a bináris hello kódolást használó utasításokat protokollt, és továbbítja az üzleti üzenetek hello. Magában foglalja a kifinomult folyamatvezérlés sémák toomaximize hello hello hálózati és felhasználását hello kapcsolódó összetevők. Említett, hello protokoll lett tervezett toostrike hatékonyságát, a rugalmasság és a kölcsönös átjárhatóságot egyensúlyára.
* **Megbízható**: hello AMQP 1.0 protokoll lehetővé teszi, hogy pontosan kicserélt megbízhatóság garanciák tooreliable tűz-és-elfelejti a tartománnyal üzenetek toobe-egyszer nyugtázott kézbesítését.
* **Rugalmas**: AMQP 1.0 egy rugalmas protokoll, amely különböző használt toosupport-topológiával. hello ugyanazt a protokollt használható ügyfél-ügyfél, ügyfél-átvitelszervező és broker-broker kommunikációhoz.
* **Munkamenet-átvitelszervező-modell független**: hello AMQP 1.0-specifikációja nem teszi szemben támasztott követelményeket hello ügynök által használt üzenetkezelési modellt. Ez azt jelenti, hogy lehetséges tooeasily adja hozzá az AMQP 1.0 támogatási tooexisting üzenetkezelő közvetítők.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0-s szabvány (az egy beruházási ")
AMQP 1.0 az nemzetközi szabványa, ISO és IEC, ISO/IEC 19464:2014 jóvá.

AMQP 1.0 fejlesztési több mint 20 vállalatok core csoportja által 2008 óta nem történt technológia szállítók és a végfelhasználói vállalkozások. Ebben az időszakban felhasználói vállalkozások hozzájárultak azok valós üzleti követelmények és hello technológia szállítók lett hatékonyabb hello protokoll toomeet ezt a követelményt. Hello folyamat során a szállítók toovalidate hello együttműködésével az megvalósítások közreműködő műhelyek részt vett.

A 2011. októberi hello fejlesztési munkálatok során, állapotra váltott tooa technikai bizottság hello fizetési a strukturált adatokat szabványok (OASIS) a szervezet hello belül, és OASIS AMQP 1.0-s szabvány hello jelent 2012. októberi. hello következő vállalkozások részt vett hello technikai bizottság szabványos hello hello fejlesztése során:

* **Technológia szállítók**: Axway szoftver, Huawei technológiákat, IIT szoftver, INETCO rendszerek, Kaazing, a Microsoft, Mitre Corporation, Primeton technológiákat, folyamatban lévő szoftver, Red Hat, SITA, szoftver rendelkezésre állási csoport, Solace rendszerek, VMware, WSO2, Zenika.
* **Felhasználói vállalkozások**: amerikai banki, jóváírás Suisse, német Boerse, Goldman Sachs, JPMorgan Chase.

Néhány hello gyakran idézett szabványok nyissa meg a következő előnyöket nyújtja:

* Kisebb a szállító zárolás a valószínűsége.
* Együttműködési lehetőség
* Széles körű rendelkezésre állását, valamint a szalagtárak tooling
* Elavulás elleni védelem
* Tájékozott személyzet rendelkezésre állása
* Alacsonyabb és kezelhető is legyen kockázata

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0-ás és Service Bus
Azure Service Bus AMQP 1.0 támogatást azt jelenti, hogy mostantól kihasználhatja a hello Service Bus queuing és közzétételi/előfizetési közvetítőalapú üzenettovábbítás szolgáltatások egy hatékony bináris protokollal platformok közötti. Ezenkívül összetevőket használja a nyelveket, keretrendszerek és operációs rendszerek áll alkalmazásokat hozhat létre.

hello következőképpen ábrázolható Linux hello szabványos Java üzenet szolgáltatás (JMS) API használatával készítettek Java-ügyfélszámítógépek és a Windows, exchange üzenetek AMQP 1.0 használatával a Service Bus keresztül futó .NET-ügyfelek telepítését bemutató példát.

![][0]

**1. ábra: Példa telepítési forgatókönyv megjelenítő platformok közötti Service Bus és AMQP 1.0 használatával**

Ez idő hello: következő ügyfélkódtáraival toowork a Service busszal ismert:

| Nyelv | Részletes ismertetés |
| --- | --- |
| Java |Apache Qpid Java üzenet szolgáltatás (JMS) ügyfél<br/>IIT szoftver SwiftMQ Java-ügyfél |
| C# |Apache Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apache Qpid Proton-Python |
| C# |AMQP .net Lite |

**2. ábra: Tábla AMQP 1.0-ügyfél szalagtárak**

## <a name="summary"></a>Összefoglalás
* AMQP 1.0 egy olyan nyitott, megbízható üzenetkezelési protokoll használható toobuild, hibrid alkalmazások. AMQP 1.0 az OASIS szabványa.
* AMQP 1.0 a rendszer támogatja az Azure Service Bus, valamint a Service Bus a Windows Server (Service Bus 1.1). Megegyezik a hello protokollok meglévő hello árképzési.

## <a name="next-steps"></a>Következő lépések
Készen áll a toolearn további? Látogasson el a következő hivatkozások hello:

* [Az AMQP, a Service Bus a .NET használatával]
* [Az AMQP, a Service Bus a Java használatával]
* [Apache Qpid Proton-C az Azure Linux virtuális gép telepítése]
* [A Service Bus a Windows Server AMQP]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Az AMQP, a Service Bus a .NET használatával]: service-bus-amqp-dotnet.md
[Az AMQP, a Service Bus a Java használatával]: service-bus-amqp-java.md
[Apache Qpid Proton-C az Azure Linux virtuális gép telepítése]: service-bus-amqp-apache.md
[A Service Bus a Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
