---
title: "az Azure storage és a Visual Studio elindítva aaaGetting csatlakoztatva (webjobs-feladat projektek) szolgáltatások"
description: "Hogyan tooget használatának Azure Table storage a Visual Studio Azure webjobs-feladatok projektben után a Visual Studio használatával tooa tárfiók kapcsolódás kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 448dee3369207062ee85c6636c84bcb4e26a2085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Ismerkedés az Azure Storage (az Azure webjobs-feladat projektek)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a C# mintakódok megjelenítése megjelenítése hogyan toouse hello Azure WebJobs SDK-verzió 1.x az hello Azure table storage szolgáltatás. Kódminták hello használata hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verzió 1.x.

hello Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű toostore strukturált adatok. hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.  Lásd: [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md#create-a-table) további információt.

Néhány hello kódtöredékek megjelenítése hello **tábla** , melynek neve manuálisan, ez azt jelenti, hogy nem egyikével hello eseményindító attribútum függvényekben használt attribútum.

## <a name="how-tooadd-entities-tooa-table"></a>Hogyan tooadd entitások tooa tábla
tooadd entitások tooa tábla, használjon hello **tábla** az attribútum egy **ICollector<T>**  vagy **IAsyncCollector<T>**  paraméter ahol **T** hello séma meghatározza az hello entitások tooadd szeretné. hello attribútum konstruktora a hello tábla hello nevét megadó karakterlánc-paramétert fogad.

hello következő példakód hozzáadja **személy** entitások tooa táblában *érkező*.

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

Általában hello használata típus **ICollector** származik **TableEntity** vagy megvalósítja **ITableEntity**, de nem kell. Hello alábbi eljárások egyikével **személy** munka az előző hello hello kód osztályokat **érkező** metódust.

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

Ha azt szeretné, közvetlenül az Azure storage API hello toowork, hozzáadhat egy **CloudStorageAccount** paraméter toohello metódus-aláírás.

## <a name="real-time-monitoring"></a>Valós idejű figyelése
Adatbeviteli függvényektől a nagy mennyiségű adatot gyakran feldolgozni, mert a WebJobs SDK irányítópult hello biztosít a valós idejű figyelési adatok. Hello **meghívása napló** szakasz azt ismerteti, hogy hello funkció továbbra is fut-e.

![Érkező függvény fut](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

Hello **meghívása részletek** lap jelentések hello függvény folyamatban (írt entitások száma) működik, valamint egy lehetőség tooabort lehetővé teszi azt.

![Érkező függvény fut](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

Ha hello függvény befejezése után hello **meghívása részletek** lap hello írt sorok számát jelenti.

![Befejezett érkező függvény](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-tooread-multiple-entities-from-a-table"></a>Hogyan tooread több entitás egy táblából
egy tábla tooread hello használata **tábla** az attribútum egy **IQueryable<T>**  paraméter ahol írja be a **T** származik **TableEntity**vagy megvalósítja **ITableEntity**.

hello alábbi kódminta beolvassa és naplózza minden sor hello **érkező** tábla:

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

### <a name="how-tooread-a-single-entity-from-a-table"></a>Hogyan tooread egyetlen entitás az táblából
Van egy **tábla** , amelyek lehetővé teszik a hello partíciókulcs- és sorkulcsa megadásakor toobind tooa egyetlen tábla entitást két további paraméterekkel rendelkező attribútumok konstruktorában.

hello alábbi kódminta olvas egy tábla sort a **személy** entitás partíció kulcs és a sor kulcsértékei egy üzenetsor-üzenetet kapott alapján:  

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


Hello **személy** ebben a példában az osztály nem rendelkezik tooimplement **ITableEntity**.

## <a name="how-toouse-hello-net-storage-api-directly-toowork-with-a-table"></a>Hogyan toouse hello .NET tárolási API közvetlenül egy táblázatot toowork
Is használhatja a hello **tábla** az attribútum egy **CloudTable** objektum végzett munka során egy táblázat további rugalmasságot biztosít.

hello következő kódban a minta egy **CloudTable** tooadd egy egyetlen entitás toohello objektum *érkező* tábla.

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

További információ toouse hello **CloudTable** objektumazonosító, lásd: [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md).

## <a name="related-topics-covered-by-hello-queues-how-tooarticle"></a>Hello várólisták hogyan-tooarticle hatálya kapcsolódó témakörök
Hogyan toohandle tábla feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem specifikus tootable feldolgozása, lásd: [Ismerkedés az Azure Queue storage és a Visual Studio csatlakoztatva (webjobs-feladat projektek) szolgáltatások ](vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Következő lépések
Ez a cikk nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei használata Azure-táblákban. További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

