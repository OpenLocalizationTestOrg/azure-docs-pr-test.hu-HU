---
title: "az Azure storage és a Visual Studio elindítva aaaGetting csatlakoztatva (webjobs-feladat projektek) szolgáltatások"
description: "Hogyan tooget használatának Azure Table storage a Visual Studio Azure webjobs-feladatok projektben után a Visual Studio használatával tooa tárfiók kapcsolódás kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 80d9f8d8b493ce612623dfed7e89325fb154a1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a><span data-ttu-id="76664-103">Ismerkedés az Azure Storage (az Azure webjobs-feladat projektek)</span><span class="sxs-lookup"><span data-stu-id="76664-103">Getting Started with Azure Storage (Azure WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="76664-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="76664-104">Overview</span></span>
<span data-ttu-id="76664-105">Ez a cikk ismerteti a C# mintakódok megjelenítése megjelenítése hogyan toouse hello Azure WebJobs SDK-verzió 1.x az hello Azure table storage szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="76664-105">This article provides C# code samples that show show how toouse hello Azure WebJobs SDK version 1.x with hello Azure table storage service.</span></span> <span data-ttu-id="76664-106">Kódminták hello használata hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="76664-106">hello code samples use hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="76664-107">hello Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű toostore strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="76664-107">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="76664-108">hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="76664-108">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="76664-109">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="76664-109">Azure tables are ideal for storing structured, non-relational data.</span></span>  <span data-ttu-id="76664-110">Lásd: [Ismerkedés az Azure Table storage használatának .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) további információt.</span><span class="sxs-lookup"><span data-stu-id="76664-110">See [Get started with Azure Table storage using .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) for more information.</span></span>

<span data-ttu-id="76664-111">Néhány hello kódtöredékek megjelenítése hello **tábla** , melynek neve manuálisan, ez azt jelenti, hogy nem egyikével hello eseményindító attribútum függvényekben használt attribútum.</span><span class="sxs-lookup"><span data-stu-id="76664-111">Some of hello code snippets show hello **Table** attribute used in functions that are called manually, that is, not by using one of hello trigger attributes.</span></span>

## <a name="how-tooadd-entities-tooa-table"></a><span data-ttu-id="76664-112">Hogyan tooadd entitások tooa tábla</span><span class="sxs-lookup"><span data-stu-id="76664-112">How tooadd entities tooa table</span></span>
<span data-ttu-id="76664-113">tooadd entitások tooa tábla, használjon hello **tábla** az attribútum egy **ICollector<T>**  vagy **IAsyncCollector<T>**  paraméter ahol **T** hello séma meghatározza az hello entitások tooadd szeretné.</span><span class="sxs-lookup"><span data-stu-id="76664-113">tooadd entities tooa table, use hello **Table** attribute with an **ICollector<T>** or **IAsyncCollector<T>** parameter where **T** specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="76664-114">hello attribútum konstruktora a hello tábla hello nevét megadó karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="76664-114">hello attribute constructor takes a string parameter that specifies hello name of hello table.</span></span>

<span data-ttu-id="76664-115">hello következő példakód hozzáadja **személy** entitások tooa táblában *érkező*.</span><span class="sxs-lookup"><span data-stu-id="76664-115">hello following code sample adds **Person** entities tooa table named *Ingress*.</span></span>

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

<span data-ttu-id="76664-116">Általában hello használata típus **ICollector** származik **TableEntity** vagy megvalósítja **ITableEntity**, de nem kell.</span><span class="sxs-lookup"><span data-stu-id="76664-116">Typically hello type you use with **ICollector** derives from **TableEntity** or implements **ITableEntity**, but it doesn't have to.</span></span> <span data-ttu-id="76664-117">Hello alábbi eljárások egyikével **személy** munka az előző hello hello kód osztályokat **érkező** metódust.</span><span class="sxs-lookup"><span data-stu-id="76664-117">Either of hello following **Person** classes work with hello code shown in hello preceding **Ingress** method.</span></span>

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

<span data-ttu-id="76664-118">Ha azt szeretné, közvetlenül az Azure storage API hello toowork, hozzáadhat egy **CloudStorageAccount** paraméter toohello metódus-aláírás.</span><span class="sxs-lookup"><span data-stu-id="76664-118">If you want toowork directly with hello Azure storage API, you can add a **CloudStorageAccount** parameter toohello method signature.</span></span>

## <a name="real-time-monitoring"></a><span data-ttu-id="76664-119">Valós idejű figyelése</span><span class="sxs-lookup"><span data-stu-id="76664-119">Real-time monitoring</span></span>
<span data-ttu-id="76664-120">Adatbeviteli függvényektől a nagy mennyiségű adatot gyakran feldolgozni, mert a WebJobs SDK irányítópult hello biztosít a valós idejű figyelési adatok.</span><span class="sxs-lookup"><span data-stu-id="76664-120">Because data ingress functions often process large volumes of data, hello WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="76664-121">Hello **meghívása napló** szakasz azt ismerteti, hogy hello funkció továbbra is fut-e.</span><span class="sxs-lookup"><span data-stu-id="76664-121">hello **Invocation Log** section tells you if hello function is still running.</span></span>

![Érkező függvény fut](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

<span data-ttu-id="76664-123">Hello **meghívása részletek** lap jelentések hello függvény folyamatban (írt entitások száma) működik, valamint egy lehetőség tooabort lehetővé teszi azt.</span><span class="sxs-lookup"><span data-stu-id="76664-123">hello **Invocation Details** page reports hello function's progress (number of entities written) while it's running and gives you an opportunity tooabort it.</span></span>

![Érkező függvény fut](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

<span data-ttu-id="76664-125">Ha hello függvény befejezése után hello **meghívása részletek** lap hello írt sorok számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="76664-125">When hello function finishes, hello **Invocation Details** page reports hello number of rows written.</span></span>

![Befejezett érkező függvény](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-tooread-multiple-entities-from-a-table"></a><span data-ttu-id="76664-127">Hogyan tooread több entitás egy táblából</span><span class="sxs-lookup"><span data-stu-id="76664-127">How tooread multiple entities from a table</span></span>
<span data-ttu-id="76664-128">egy tábla tooread hello használata **tábla** az attribútum egy **IQueryable<T>**  paraméter ahol írja be a **T** származik **TableEntity**vagy megvalósítja **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="76664-128">tooread a table, use hello **Table** attribute with an **IQueryable<T>** parameter where type **T** derives from **TableEntity** or implements **ITableEntity**.</span></span>

<span data-ttu-id="76664-129">hello alábbi kódminta beolvassa és naplózza minden sor hello **érkező** tábla:</span><span class="sxs-lookup"><span data-stu-id="76664-129">hello following code sample reads and logs all rows from hello **Ingress** table:</span></span>

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

### <a name="how-tooread-a-single-entity-from-a-table"></a><span data-ttu-id="76664-130">Hogyan tooread egyetlen entitás az táblából</span><span class="sxs-lookup"><span data-stu-id="76664-130">How tooread a single entity from a table</span></span>
<span data-ttu-id="76664-131">Van egy **tábla** , amelyek lehetővé teszik a hello partíciókulcs- és sorkulcsa megadásakor toobind tooa egyetlen tábla entitást két további paraméterekkel rendelkező attribútumok konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="76664-131">There is a **Table** attribute constructor with two additional parameters that let you specify hello partition key and row key when you want toobind tooa single table entity.</span></span>

<span data-ttu-id="76664-132">hello alábbi kódminta olvas egy tábla sort a **személy** entitás partíció kulcs és a sor kulcsértékei egy üzenetsor-üzenetet kapott alapján:</span><span class="sxs-lookup"><span data-stu-id="76664-132">hello following code sample reads a table row for a **Person** entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="76664-133">Hello **személy** ebben a példában az osztály nem rendelkezik tooimplement **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="76664-133">hello **Person** class in this example does not have tooimplement **ITableEntity**.</span></span>

## <a name="how-toouse-hello-net-storage-api-directly-toowork-with-a-table"></a><span data-ttu-id="76664-134">Hogyan toouse hello .NET tárolási API közvetlenül egy táblázatot toowork</span><span class="sxs-lookup"><span data-stu-id="76664-134">How toouse hello .NET Storage API directly toowork with a table</span></span>
<span data-ttu-id="76664-135">Is használhatja a hello **tábla** az attribútum egy **CloudTable** objektum végzett munka során egy táblázat további rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="76664-135">You can also use hello **Table** attribute with a **CloudTable** object for more flexibility in working with a table.</span></span>

<span data-ttu-id="76664-136">hello következő kódban a minta egy **CloudTable** tooadd egy egyetlen entitás toohello objektum *érkező* tábla.</span><span class="sxs-lookup"><span data-stu-id="76664-136">hello following code sample uses a **CloudTable** object tooadd a single entity toohello *Ingress* table.</span></span>

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

<span data-ttu-id="76664-137">További információ toouse hello **CloudTable** objektumazonosító, lásd: [Ismerkedés az Azure Table storage használatának .NET](../storage/storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="76664-137">For more information about how toouse hello **CloudTable** object, see [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md).</span></span>

## <a name="related-topics-covered-by-hello-queues-how-tooarticle"></a><span data-ttu-id="76664-138">Hello várólisták hogyan-tooarticle hatálya kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="76664-138">Related topics covered by hello queues how-tooarticle</span></span>
<span data-ttu-id="76664-139">Hogyan toohandle tábla feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem specifikus tootable feldolgozása, lásd: [Ismerkedés az Azure Queue storage és a Visual Studio csatlakoztatva (webjobs-feladat projektek) szolgáltatások ](../storage/vs-storage-webjobs-getting-started-queues.md).</span><span class="sxs-lookup"><span data-stu-id="76664-139">For information about how toohandle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific tootable processing, see [Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)](../storage/vs-storage-webjobs-getting-started-queues.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="76664-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76664-140">Next steps</span></span>
<span data-ttu-id="76664-141">Ez a cikk nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei használata Azure-táblákban.</span><span class="sxs-lookup"><span data-stu-id="76664-141">This article has provided code samples that show how toohandle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="76664-142">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="76664-142">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

