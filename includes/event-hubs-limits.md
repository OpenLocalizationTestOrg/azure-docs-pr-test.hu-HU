hello következő táblázat felsorolja a kvóták és adott tooAzure Event Hubs korlátozza. Az Event Hubs árazással kapcsolatos információkért lásd: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).

| Korlát | Hatókör | Típus | Viselkedés túllépésekor | Érték |
| --- | --- | --- | --- | --- |
| Az Event Hubs névtér másodpercenkénti száma |Namespace |Statikus |A program elutasítja későbbi kérelmeket egy új névtér létrehozásához. |10 |
| Az Event Hubs egy partíciók száma |Entitás |Statikus |- |32 |
| Az Event Hubs egy fogyasztói csoportok száma |Entitás |Statikus |- |20 |
| Névtér AMQP-kapcsolatok száma |Namespace |Statikus |További kapcsolatokat későbbi kérelmek vissza kell utasítani, és kivételt érkezik hello kód hívása. |5,000 |
| Az Event Hubs esemény maximális mérete|Rendszerszintű |Statikus |- |256 KB |
| Az Eseményközpont nevét maximális mérete |Entitás |Statikus |- |50 karakter hosszú lehet |
| Nem epoch fogadók egyes fogyasztói csoportok száma |Entitás |Statikus |- |5 |
| Az eseményadatok maximális megőrzési időtartam |Entitás |Statikus |- |1-7 nap |
| Kapacitásegységek maximális száma |Namespace |Statikus |Meghaladó hello átviteli egység korlát hatására az adatok toobe halmozódni és állít elő egy  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Átviteli egységek a szokásos nagyobb számú kérhet bejelentés réteghez egy [támogatási kérelem](/azure/azure-supportability/how-to-create-azure-support-request). [További átviteli egységek](../articles/event-hubs/event-hubs-auto-inflate.md) véglegesített beszerzési alapon 20 blokkok érhetők el. |20 |

