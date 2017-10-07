---
title: az Azure table storage hello WebJobs SDK a aaaHow toouse
description: "Ismerje meg, hogyan toouse Azure table tároló hello WebJobs SDK a. Táblák létrehozása, és adja hozzá az entitások tootables olvassa el a létező táblák."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 451432cc-c780-4310-85d3-84f44fe48afe
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 8e28c69df4a934646add9e50c6de28e76dca1636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-with-hello-webjobs-sdk"></a>Hogyan toouse Azure table storage a hello WebJobs SDK
## <a name="overview"></a>Áttekintés
Ez az útmutató Kódminták C#, hogy hogyan tooread és az Azure storage írási táblázatok használatával megjelenítése [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.

hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md) vagy túl[több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Néhány hello kódtöredékek megjelenítése hello `Table` funkciókat használt attribútum [manuálisan nevű](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), ez azt jelenti, hogy nem használatával hello eseményindító attribútumok egyikét. 

## <a id="ingress"></a>Hogyan tooadd entitások tooa tábla
tooadd entitások tooa tábla, használjon hello `Table` az attribútum egy `ICollector<T>` vagy `IAsyncCollector<T>` paraméter ahol `T` hello séma meghatározza hello entitások tooadd szeretné. hello attribútum konstruktora a hello tábla hello nevét megadó karakterlánc-paramétert fogad. 

hello következő példakód hozzáadja `Person` entitások tooa táblában *érkező*.

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() { 
                        PartitionKey = "Test", 
                        RowKey = i.ToString(), 
                        Name = "Name" }
                    );
            }
        }

Általában hello használata típus `ICollector` származik `TableEntity` vagy megvalósítja `ITableEntity`, de nem kell. Hello alábbi eljárások egyikével `Person` munka az előző hello hello kód osztályokat `Ingress` metódust.

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

Ha azt szeretné, közvetlenül az Azure storage API hello toowork, hozzáadhat egy `CloudStorageAccount` paraméter toohello metódus-aláírás.

## <a id="monitor"></a>Valós idejű figyelése
Adatbeviteli függvényektől a nagy mennyiségű adatot gyakran feldolgozni, mert a WebJobs SDK irányítópult hello biztosít a valós idejű figyelési adatok. Hello **meghívása napló** szakasz azt ismerteti, hogy hello funkció továbbra is fut-e.

![Érkező függvény fut](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

Hello **meghívása részletek** lap jelentések hello függvény folyamatban (írt entitások száma) működik, valamint egy lehetőség tooabort lehetővé teszi azt. 

![Érkező függvény fut](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

Ha hello függvény befejezése után hello **meghívása részletek** lap hello írt sorok számát jelenti.

![Befejezett érkező függvény](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <a id="multiple"></a>Hogyan tooread több entitás egy táblából
egy tábla tooread hello használata `Table` az attribútum egy `IQueryable<T>` paraméter ahol írja be a `T` származik `TableEntity` vagy megvalósítja `ITableEntity`.

hello alábbi kódminta beolvassa és naplózza minden sor hello `Ingress` tábla:

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}", 
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <a id="readone"></a>Hogyan tooread egyetlen entitás az táblából
Nincs olyan `Table` , amelyek lehetővé teszik a hello partíciókulcs- és sorkulcsa megadásakor toobind tooa egyetlen tábla entitást két további paraméterekkel rendelkező attribútumok konstruktorában.

hello alábbi kódminta olvas egy tábla sort a `Person` entitás partíció kulcs és a sor kulcsértékei egy üzenetsor-üzenetet kapott alapján:  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


Hello `Person` ebben a példában az osztály nem rendelkezik tooimplement `ITableEntity`.

## <a id="storageapi"></a>Hogyan toouse hello .NET tárolási API közvetlenül egy táblázatot toowork
Is használhatja a hello `Table` az attribútum egy `CloudTable` objektum végzett munka során egy táblázat további rugalmasságot biztosít.

hello következő kódban a minta egy `CloudTable` tooadd egy egyetlen entitás toohello objektum *érkező* tábla. 

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

További információ toouse hello `CloudTable` objektumazonosító, lásd: [hogyan toouse Table Storage a .NET-](../cosmos-db/table-storage-how-to-use-dotnet.md). 

## <a id="queues"></a>Hello várólisták hogyan-tooarticle hatálya kapcsolódó témakörök
Hogyan toohandle tábla feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem specifikus tootable feldolgozása, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

A cikkben szereplő témakörök hello alábbiakat foglalja magába:

* Aszinkron funkciók
* Több példánya
* Biztonságos leállításának
* Egy függvény törzséhez hello a WebJobs SDK attribútumok használata
* A kód hello SDK kapcsolati karakterláncok beállítása
* Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* Manuálisan kezdeményezi egy függvény
* Naplók írása

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei használata Azure-táblákban. További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

