---
title: "a .NET- és AMQP 1.0 Bus aaaService |} Microsoft Docs"
description: "A .NET-Azure Service Bus használata AMQP"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
ms.openlocfilehash: d8b40f92ba29058951556fa3db1adcf9383ee69f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-service-bus-from-net-with-amqp-10"></a>A Service Bus a .NET-AMQP 1.0 használatával

## <a name="downloading-hello-service-bus-sdk"></a>Hello Service Bus SDK letöltése

AMQP 1.0 támogatási hello Service Bus SDK 2.1-es vagy újabb verziója érhető el. Biztosíthatja a letöltés hello Service Bus bits a hello legújabb verziójával rendelkezik [NuGet][NuGet].

## <a name="configuring-net-applications-toouse-amqp-10"></a>.NET-alkalmazások toouse AMQP 1.0 konfigurálása

Alapértelmezés szerint hello Service Bus .NET ügyféloldali kódtár hello Service Bus szolgáltatás egy dedikált SOAP-alapú protokoll segítségével kommunikál. toouse AMQP 1.0 hello alapértelmezett protokoll helyett hello Service Bus kapcsolati karakterlánc, explicit konfigurációt igényel, hello a következő szakaszban leírtak szerint. Ez a változás nem alkalmazáskód változatlan marad AMQP 1.0 használata esetén.

Hello jelenlegi kiadásban van néhány AMQP használata esetén nem támogatott API-funkciókat. Nem támogatott szolgáltatások hello szakaszban később szereplő [nem támogatott funkciók, korlátozások és viselkedési különbségek](#unsupported-features-restrictions-and-behavioral-differences). Néhány speciális konfigurációs beállítások hello is eltérő jelentéssel rendelkezhetnek AMQP használatakor.

### <a name="configuration-using-appconfig"></a>Konfigurációs App.config használatával

Tanácsos alkalmazások toouse hello App.config konfigurációs fájl toostore beállítások. A Service Bus-alkalmazást használhatja az App.config toostore hello Service Bus kapcsolati karakterlánc. Egy példa App.config fájl a következőképpen történik:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

hello értékének hello `Microsoft.ServiceBus.ConnectionString` beállítás hello Service Bus kapcsolati karakterlánc, amely használt tooconfigure hello kapcsolat tooService busz. hello formátum a következőképpen történik:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Ha `[namespace]` és `SharedAccessKey` hello kapott [Azure-portálon] [ Azure portal] létrehozásakor egy Szolgáltatásbusz-névtér. További információkért lásd: [hello Azure-portál használatával a Service Bus-névtér létrehozása][Create a Service Bus namespace using hello Azure portal].

AMQP használatakor hozzáfűzése hello kapcsolati karakterlánc `;TransportType=Amqp`. Ez a notation arra utasítja a hello ügyfél könyvtár toomake a kapcsolat tooService Bus AMQP 1.0 használatával.

## <a name="message-serialization"></a>Üzenet szerializálási

Hello alapértelmezett protokoll használata esetén hello alapértelmezett szerializálási hello .NET ügyféloldali kódtár működése toouse hello [DataContractSerializer] [ DataContractSerializer] tooserialize írja be a [BrokeredMessage ] [ BrokeredMessage] példány átvitel hello ügyféloldali kódtár és hello Service Bus szolgáltatás között. Amikor hello AMQP átviteli mód használatával, hello ügyféloldali kódtár hello AMQP típusrendszernek használ hello szerializálása [közvetítőalapú üzenet] [ BrokeredMessage] egy AMQP üzenetbe. A szerializálás lehetővé teszi, hogy a hello üzenet toobe kapott, és a fogadó alkalmazás, potenciálisan futó különböző platform, például Java-alkalmazások által használt hello JMS API tooaccess Service Bus értelmezi.

Ha, hozhat létre egy [BrokeredMessage] [ BrokeredMessage] példány, megadhat egy .NET-objektum mint egy paraméterrel toohello konstruktor tooserve hello hello üzenet szövege. Objektumoknál, amelyek csatlakoztatott tooAMQP egyszerű típusok lehetnek hello törzs szerializálni AMQP adattípusokat. Ha hello objektum leképezése nem végezhető el közvetlenül a következő egy AMQP primitív típusra; Ez azt jelenti, hogy egy egyéni hello alkalmazást, majd hello objektum által megadott típus szerializált hello segítségével [DataContractSerializer][DataContractSerializer], és a szerializált hello a bájtok küldésének az AMQP-adatok üzenet.

a .NET ügyfelek toofacilitate együttműködés csak közvetlenül a hello üzenet törzsét hello AMQP típusokat akkor szerializálható .NET típusok használja. a következő táblázat hello ezen típusok és hello megfelelő leképezési toohello AMQP típusrendszernek részletezi.

| .NET törzs objektum típusa | Csatlakoztatott AMQP típusa | AMQP törzs szakasz típusa |
| --- | --- | --- |
| logikai érték |Logikai érték |AMQP érték |
| Bájt |ubyte |AMQP érték |
| ushort |ushort |AMQP érték |
| uint |uint |AMQP érték |
| ulong |ulong |AMQP érték |
| sbyte |Bájt |AMQP érték |
| rövid |rövid |AMQP érték |
| int |int |AMQP érték |
| hosszú |hosszú |AMQP érték |
| Lebegőpontos |Lebegőpontos |AMQP érték |
| Dupla |Dupla |AMQP érték |
| Decimális |decimal128 |AMQP érték |
| Karakter |Karakter |AMQP érték |
| Dátum és idő |időbélyeg |AMQP érték |
| GUID |UUID |AMQP érték |
| Byte] |Bináris |AMQP érték |
| Karakterlánc |Karakterlánc |AMQP érték |
| System.Collections.IList illesztőfelületet |lista |AMQP értéke: hello gyűjteményben szereplő elemek csak lehet azokat ebben a táblázatban definiált. |
| System.Array |A tömb |AMQP értéke: hello gyűjteményben szereplő elemek csak lehet azokat ebben a táblázatban definiált. |
| System.Collections.IDictionary |térkép |AMQP értéke: hello gyűjteményben szereplő elemek csak lehet azokat ebben a táblázatban definiált. Megjegyzés: csak a karakterlánc-kulcsok használata támogatott. |
| URI |Karakterlánc leírt (lásd a következő táblázat hello) |AMQP érték |
| DateTimeOffset |Hosszú leírt (lásd a következő táblázat hello) |AMQP érték |
| A TimeSpan |Hosszú leírt (lásd a következő hello) |AMQP érték |
| Az adatfolyam |Bináris |AMQP adatok (több is lehet). hello adatok szakaszok tartalmaznak hello nyers bájt hello adatfolyam objektumból olvasni. |
| Másik objektum |Bináris |AMQP adatok (több is lehet). Hello objektum, amely hello DataContractSerializer vagy hello alkalmazás által biztosított szerializálót szerializált hello bináris tartalmazza. |

| .NET-típusa | Csatlakoztatott AMQP leírt típusa | Megjegyzések |
| --- | --- | --- |
| URI |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| A TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nem támogatott funkciók, korlátozások és viselkedési különbségek

Service Bus .NET API hello szolgáltatásait a következő hello jelenleg nem támogatottak AMQP használatakor:

* Tranzakciók
* Átviteli cél küldés

Is néhány kisebb különbségek vannak a Service Bus .NET API hello hello viselkedését AMQP, összehasonlított toohello alapértelmezett protokoll használata esetén:

* Hello [OperationTimeout] [ OperationTimeout] tulajdonság a rendszer figyelmen kívül hagyja.
* `MessageReceiver.Receive(TimeSpan.Zero)`valósul meg `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Üzenetek befejezése által zárolási jogkivonatok csak végezhető el, amely eredetileg köszönőüzenetei hello üzenetet fogadó.

## <a name="controlling-amqp-protocol-settings"></a>Ellenőrző AMQP protokoll beállításait

Hello [.NET API-k](/dotnet/api/) hello AMQP protokoll több beállítások toocontrol hello viselkedését elérhetővé:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: vezérlők hello alkalmazott kezdeti jóváírás tooa hivatkozásra. hello alapértelmezett érték 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: hello egyeztetéskor kapcsolat megnyitása során felajánlott vezérlők hello maximális AMQP keret méretét. hello alapértelmezett érték a 65 536 bájt.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Ha átvitelek batchable, ez az érték határozza meg, hello késleltetési dispositions küldéséhez. Alapértelmezés szerint feladók/fogadók örökli. Egyes feladó/fogadó hello alapértelmezés szerint 20 ezredmásodperc lehet felülbírálni.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: meghatározza, hogy SSL-kapcsolaton keresztül létesít AMQP-kapcsolatokat. hello alapértelmezett érték a **igaz**.

## <a name="next-steps"></a>Következő lépések

Készen áll a toolearn további? Látogasson el a következő hivatkozások hello:

* [Service Bus AMQP áttekintése]
* [Particionált Service Bus-üzenetsorok és témakörök AMQP 1.0 támogatása]
* [A Service Bus a Windows Server AMQP]

[Create a Service Bus namespace using hello Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Service Bus AMQP áttekintése]: service-bus-amqp-overview.md
[Particionált Service Bus-üzenetsorok és témakörök AMQP 1.0 támogatása]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[A Service Bus a Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
