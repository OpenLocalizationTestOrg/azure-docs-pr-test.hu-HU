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
# <a name="how-toouse-azure-table-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="35a4a-104">Hogyan toouse Azure table storage a hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="35a4a-104">How toouse Azure table storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="35a4a-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="35a4a-105">Overview</span></span>
<span data-ttu-id="35a4a-106">Ez az útmutató Kódminták C#, hogy hogyan tooread és az Azure storage írási táblázatok használatával megjelenítése [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="35a4a-106">This guide provides C# code samples that show how tooread and write Azure storage tables by using [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="35a4a-107">hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md) vagy túl[több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="35a4a-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="35a4a-108">Néhány hello kódtöredékek megjelenítése hello `Table` funkciókat használt attribútum [manuálisan nevű](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), ez azt jelenti, hogy nem használatával hello eseményindító attribútumok egyikét.</span><span class="sxs-lookup"><span data-stu-id="35a4a-108">Some of hello code snippets show hello `Table` attribute used in functions that are [called manually](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), that is, not by using one of hello trigger attributes.</span></span> 

## <span data-ttu-id="35a4a-109"><a id="ingress"></a>Hogyan tooadd entitások tooa tábla</span><span class="sxs-lookup"><span data-stu-id="35a4a-109"><a id="ingress"></a> How tooadd entities tooa table</span></span>
<span data-ttu-id="35a4a-110">tooadd entitások tooa tábla, használjon hello `Table` az attribútum egy `ICollector<T>` vagy `IAsyncCollector<T>` paraméter ahol `T` hello séma meghatározza hello entitások tooadd szeretné.</span><span class="sxs-lookup"><span data-stu-id="35a4a-110">tooadd entities tooa table, use hello `Table` attribute with an `ICollector<T>` or `IAsyncCollector<T>` parameter where `T` specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="35a4a-111">hello attribútum konstruktora a hello tábla hello nevét megadó karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="35a4a-111">hello attribute constructor takes a string parameter that specifies hello name of hello table.</span></span> 

<span data-ttu-id="35a4a-112">hello következő példakód hozzáadja `Person` entitások tooa táblában *érkező*.</span><span class="sxs-lookup"><span data-stu-id="35a4a-112">hello following code sample adds `Person` entities tooa table named *Ingress*.</span></span>

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

<span data-ttu-id="35a4a-113">Általában hello használata típus `ICollector` származik `TableEntity` vagy megvalósítja `ITableEntity`, de nem kell.</span><span class="sxs-lookup"><span data-stu-id="35a4a-113">Typically hello type you use with `ICollector` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="35a4a-114">Hello alábbi eljárások egyikével `Person` munka az előző hello hello kód osztályokat `Ingress` metódust.</span><span class="sxs-lookup"><span data-stu-id="35a4a-114">Either of hello following `Person` classes work with hello code shown in hello preceding `Ingress` method.</span></span>

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

<span data-ttu-id="35a4a-115">Ha azt szeretné, közvetlenül az Azure storage API hello toowork, hozzáadhat egy `CloudStorageAccount` paraméter toohello metódus-aláírás.</span><span class="sxs-lookup"><span data-stu-id="35a4a-115">If you want toowork directly with hello Azure storage API, you can add a `CloudStorageAccount` parameter toohello method signature.</span></span>

## <span data-ttu-id="35a4a-116"><a id="monitor"></a>Valós idejű figyelése</span><span class="sxs-lookup"><span data-stu-id="35a4a-116"><a id="monitor"></a> Real-time monitoring</span></span>
<span data-ttu-id="35a4a-117">Adatbeviteli függvényektől a nagy mennyiségű adatot gyakran feldolgozni, mert a WebJobs SDK irányítópult hello biztosít a valós idejű figyelési adatok.</span><span class="sxs-lookup"><span data-stu-id="35a4a-117">Because data ingress functions often process large volumes of data, hello WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="35a4a-118">Hello **meghívása napló** szakasz azt ismerteti, hogy hello funkció továbbra is fut-e.</span><span class="sxs-lookup"><span data-stu-id="35a4a-118">hello **Invocation Log** section tells you if hello function is still running.</span></span>

![Érkező függvény fut](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

<span data-ttu-id="35a4a-120">Hello **meghívása részletek** lap jelentések hello függvény folyamatban (írt entitások száma) működik, valamint egy lehetőség tooabort lehetővé teszi azt.</span><span class="sxs-lookup"><span data-stu-id="35a4a-120">hello **Invocation Details** page reports hello function's progress (number of entities written) while it's running and gives you an opportunity tooabort it.</span></span> 

![Érkező függvény fut](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

<span data-ttu-id="35a4a-122">Ha hello függvény befejezése után hello **meghívása részletek** lap hello írt sorok számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="35a4a-122">When hello function finishes, hello **Invocation Details** page reports hello number of rows written.</span></span>

![Befejezett érkező függvény](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <span data-ttu-id="35a4a-124"><a id="multiple"></a>Hogyan tooread több entitás egy táblából</span><span class="sxs-lookup"><span data-stu-id="35a4a-124"><a id="multiple"></a> How tooread multiple entities from a table</span></span>
<span data-ttu-id="35a4a-125">egy tábla tooread hello használata `Table` az attribútum egy `IQueryable<T>` paraméter ahol írja be a `T` származik `TableEntity` vagy megvalósítja `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="35a4a-125">tooread a table, use hello `Table` attribute with an `IQueryable<T>` parameter where type `T` derives from `TableEntity` or implements `ITableEntity`.</span></span>

<span data-ttu-id="35a4a-126">hello alábbi kódminta beolvassa és naplózza minden sor hello `Ingress` tábla:</span><span class="sxs-lookup"><span data-stu-id="35a4a-126">hello following code sample reads and logs all rows from hello `Ingress` table:</span></span>

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

### <span data-ttu-id="35a4a-127"><a id="readone"></a>Hogyan tooread egyetlen entitás az táblából</span><span class="sxs-lookup"><span data-stu-id="35a4a-127"><a id="readone"></a> How tooread a single entity from a table</span></span>
<span data-ttu-id="35a4a-128">Nincs olyan `Table` , amelyek lehetővé teszik a hello partíciókulcs- és sorkulcsa megadásakor toobind tooa egyetlen tábla entitást két további paraméterekkel rendelkező attribútumok konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="35a4a-128">There is a `Table` attribute constructor with two additional parameters that let you specify hello partition key and row key when you want toobind tooa single table entity.</span></span>

<span data-ttu-id="35a4a-129">hello alábbi kódminta olvas egy tábla sort a `Person` entitás partíció kulcs és a sor kulcsértékei egy üzenetsor-üzenetet kapott alapján:</span><span class="sxs-lookup"><span data-stu-id="35a4a-129">hello following code sample reads a table row for a `Person` entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="35a4a-130">Hello `Person` ebben a példában az osztály nem rendelkezik tooimplement `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="35a4a-130">hello `Person` class in this example does not have tooimplement `ITableEntity`.</span></span>

## <span data-ttu-id="35a4a-131"><a id="storageapi"></a>Hogyan toouse hello .NET tárolási API közvetlenül egy táblázatot toowork</span><span class="sxs-lookup"><span data-stu-id="35a4a-131"><a id="storageapi"></a> How toouse hello .NET Storage API directly toowork with a table</span></span>
<span data-ttu-id="35a4a-132">Is használhatja a hello `Table` az attribútum egy `CloudTable` objektum végzett munka során egy táblázat további rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="35a4a-132">You can also use hello `Table` attribute with a `CloudTable` object for more flexibility in working with a table.</span></span>

<span data-ttu-id="35a4a-133">hello következő kódban a minta egy `CloudTable` tooadd egy egyetlen entitás toohello objektum *érkező* tábla.</span><span class="sxs-lookup"><span data-stu-id="35a4a-133">hello following code sample uses a `CloudTable` object tooadd a single entity toohello *Ingress* table.</span></span> 

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

<span data-ttu-id="35a4a-134">További információ toouse hello `CloudTable` objektumazonosító, lásd: [hogyan toouse Table Storage a .NET-](../cosmos-db/table-storage-how-to-use-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="35a4a-134">For more information about how toouse hello `CloudTable` object, see [How toouse Table Storage from .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span></span> 

## <span data-ttu-id="35a4a-135"><a id="queues"></a>Hello várólisták hogyan-tooarticle hatálya kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="35a4a-135"><a id="queues"></a>Related topics covered by hello queues how-tooarticle</span></span>
<span data-ttu-id="35a4a-136">Hogyan toohandle tábla feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem specifikus tootable feldolgozása, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="35a4a-136">For information about how toohandle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific tootable processing, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="35a4a-137">A cikkben szereplő témakörök hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="35a4a-137">Topics covered in that article include hello following:</span></span>

* <span data-ttu-id="35a4a-138">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="35a4a-138">Async functions</span></span>
* <span data-ttu-id="35a4a-139">Több példánya</span><span class="sxs-lookup"><span data-stu-id="35a4a-139">Multiple instances</span></span>
* <span data-ttu-id="35a4a-140">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="35a4a-140">Graceful shutdown</span></span>
* <span data-ttu-id="35a4a-141">Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="35a4a-141">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="35a4a-142">A kód hello SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="35a4a-142">Set hello SDK connection strings in code</span></span>
* <span data-ttu-id="35a4a-143">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="35a4a-143">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="35a4a-144">Manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="35a4a-144">Trigger a function manually</span></span>
* <span data-ttu-id="35a4a-145">Naplók írása</span><span class="sxs-lookup"><span data-stu-id="35a4a-145">Write logs</span></span>

## <span data-ttu-id="35a4a-146"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35a4a-146"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="35a4a-147">Ez az útmutató nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei használata Azure-táblákban.</span><span class="sxs-lookup"><span data-stu-id="35a4a-147">This guide has provided code samples that show how toohandle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="35a4a-148">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="35a4a-148">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

