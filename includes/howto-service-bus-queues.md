## <a name="what-are-service-bus-queues"></a>Mik azok a Service Bus-üzenetsorok?
A Service Bus-üzenetsorok **közvetítőalapú üzenettovábbítási** kommunikációs modellt biztosítanak az üzenettovábbításhoz. Üzenetsorok használata esetén az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, hanem egy közvetítőként szolgáló üzenetsoron keresztül. Üzenet létrehozója (küldő) átadja az üzenet-várólista toohello, és majd folytatja annak feldolgozását. Aszinkron módon üzenetfogyasztó (fogadó) üdvözlőüzenetére hello várólistából kéri le, és feldolgozza őket. hello készítő nem toowait a hello fogyasztó válaszára a rendelés toocontinue tooprocess és további üzenetek küldéséhez. Az üzenetsorok elsőnek **First In, First Out (, FIFO)** üzenet kézbesítési tooone vagy több versengő fogyasztó számára. Ez azt jelenti, hogy üzenetek vannak általában fogad és dolgoz fel hello fogadók, amelyben toohello várólista addig adták hozzá, és minden üzenetet kapott, és csak egy üzenetfogyasztó által feldolgozott hello sorrendben.

![Az üzenetsorok alapfogalmai](./media/howto-service-bus-queues/sb-queues-08.png)

A Service Bus-üzenetsor egy olyan általános célú technológia, amely széles körben felhasználható:

* Egy többszintes Azure-alkalmazásban, a webes és feldolgozói szerepkörök közötti kommunikáció során.
* Egy hibrid megoldásban, a helyszíni és az Azure által kezelt alkalmazások közötti kommunikáció során.
* A különböző szervezetekben, vagy egy szervezet különböző részlegein helyileg futó elosztott alkalmazások összetevői közötti kommunikáció során.

Üzenetsorok használata lehetővé teszi, hogy Ön tooscale az alkalmazások könnyebb, és további rugalmasságot tooyour architektúra engedélyezése.


