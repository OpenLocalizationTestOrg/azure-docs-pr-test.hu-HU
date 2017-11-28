---
title: "A Service Fabric szolgáltatások particionálás |} Microsoft Docs"
description: "A Service Fabric állapotalapú szolgáltatások partícióazonosító ismerteti. Partíciók lehetővé teszi, hogy a helyi gépen adattárolás, adatokat és a számítás is méretezhető együtt."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 3c1e80305cb65f41a6981b99f69e8b87f89599ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="partition-service-fabric-reliable-services"></a><span data-ttu-id="3e00d-104">A Service Fabric megbízható szolgáltatások partíció</span><span class="sxs-lookup"><span data-stu-id="3e00d-104">Partition Service Fabric reliable services</span></span>
<span data-ttu-id="3e00d-105">Ez a cikk bemutatja azokat a megbízható Azure Service Fabric-szolgáltatások particionálás alapvető fogalmait.</span><span class="sxs-lookup"><span data-stu-id="3e00d-105">This article provides an introduction to the basic concepts of partitioning Azure Service Fabric reliable services.</span></span> <span data-ttu-id="3e00d-106">A cikkben használt forráskódját is rendelkezésre áll, a [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="3e00d-106">The source code used in the article is also available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="partitioning"></a><span data-ttu-id="3e00d-107">Particionálás</span><span class="sxs-lookup"><span data-stu-id="3e00d-107">Partitioning</span></span>
<span data-ttu-id="3e00d-108">Particionálás nincs egyedi, hogy a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3e00d-108">Partitioning is not unique to Service Fabric.</span></span> <span data-ttu-id="3e00d-109">Ez valójában egy alapvető szerkezet méretezhető szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="3e00d-109">In fact, it is a core pattern of building scalable services.</span></span> <span data-ttu-id="3e00d-110">Egy tágabb értelemben véve a azt gondolja át egy fogalom felosztásával állapota (adatok), particionálás és méretezhetőség és teljesítmény javítása érdekében kisebb elérhető egységekbe számítási.</span><span class="sxs-lookup"><span data-stu-id="3e00d-110">In a broader sense, we can think about partitioning as a concept of dividing state (data) and compute into smaller accessible units to improve scalability and performance.</span></span> <span data-ttu-id="3e00d-111">Egy jól ismert particionálás formátuma [adatparticionálás][wikipartition], más néven horizontális.</span><span class="sxs-lookup"><span data-stu-id="3e00d-111">A well-known form of partitioning is [data partitioning][wikipartition], also known as sharding.</span></span>

### <a name="partition-service-fabric-stateless-services"></a><span data-ttu-id="3e00d-112">A Service Fabric állapotmentes szolgáltatások partíció</span><span class="sxs-lookup"><span data-stu-id="3e00d-112">Partition Service Fabric stateless services</span></span>
<span data-ttu-id="3e00d-113">Állapotmentes szolgáltatások esetén azt is gondolja át éppen egy logikai egységet a szolgáltatás egy vagy több példányát tartalmazó partíció.</span><span class="sxs-lookup"><span data-stu-id="3e00d-113">For stateless services, you can think about a partition being a logical unit that contains one or more instances of a service.</span></span> <span data-ttu-id="3e00d-114">1. ábra mutatja egy állapotmentes szolgáltatások elosztva a fürt egy partíciót öt osztályt.</span><span class="sxs-lookup"><span data-stu-id="3e00d-114">Figure 1 shows a stateless service with five instances distributed across a cluster using one partition.</span></span>

![Állapotmentes szolgáltatások](./media/service-fabric-concepts-partitioning/statelessinstances.png)

<span data-ttu-id="3e00d-116">Valóban két típusa van állapotmentes szolgáltatások megoldások.</span><span class="sxs-lookup"><span data-stu-id="3e00d-116">There are really two types of stateless service solutions.</span></span> <span data-ttu-id="3e00d-117">Az első címtárra olyan szolgáltatás, amely továbbra is fennáll állapotában kívülről, például egy Azure SQL-adatbázis (például egy webhely, amely tárolja a munkamenet-információk és adatok).</span><span class="sxs-lookup"><span data-stu-id="3e00d-117">The first one is a service that persists its state externally, for example in an Azure SQL database (like a website that stores the session information and data).</span></span> <span data-ttu-id="3e00d-118">A második érték csak számítási-szolgáltatások (például egy Számológép vagy kép thumbnailing), amelyeket nem kezel olyan állandó állapotokat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-118">The second one is computation-only services (like a calculator or image thumbnailing) that do not manage any persistent state.</span></span>

<span data-ttu-id="3e00d-119">A vagy állapotmentes szolgáltatások particionálás esetben egy nagyon ritkán forgatókönyv – a méretezhetőség és rendelkezésre állási általában több példány hozzáadásával érhető el.</span><span class="sxs-lookup"><span data-stu-id="3e00d-119">In either case, partitioning a stateless service is a very rare scenario--scalability and availability are normally achieved by adding more instances.</span></span> <span data-ttu-id="3e00d-120">Csak akkor érdemes megfontolni a állapotmentes szolgáltatások példányok több partíciót, amikor különleges útválasztási kérések teljesítéséhez szüksége.</span><span class="sxs-lookup"><span data-stu-id="3e00d-120">The only time you want to consider multiple partitions for stateless service instances is when you need to meet special routing requests.</span></span>

<span data-ttu-id="3e00d-121">Tegyük fel fontolja meg egy olyan esetben, ha egy bizonyos tartomány-azonosítóval rendelkező felhasználók csak szolgáltatható által egy adott szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="3e00d-121">As an example, consider a case where users with IDs in a certain range should only be served by a particular service instance.</span></span> <span data-ttu-id="3e00d-122">Ha sikerült partícióazonosító állapotmentes szolgáltatások egy másik példa, amikor egy valóban particionált háttér (pl. szilánkos SQL-adatbázis) rendelkezik, és szeretne szabályozni, mely szolgáltatáspéldány kell írni a adatbázis shard – vagy más belül előkészítő feladatok végrehajtására a állapot nélküli szolgáltatás, amely a azonos particionálási információra van szüksége, a háttér használatban van.</span><span class="sxs-lookup"><span data-stu-id="3e00d-122">Another example of when you could partition a stateless service is when you have a truly partitioned backend (e.g. a sharded SQL database) and you want to control which service instance should write to the database shard--or perform other preparation work within the stateless service that requires the same partitioning information as is used in the backend.</span></span> <span data-ttu-id="3e00d-123">Ilyen típusú forgatókönyvek is különböző módon kell megoldani, és nem feltétlenül igényel szolgáltatás particionálást.</span><span class="sxs-lookup"><span data-stu-id="3e00d-123">Those types of scenarios can also be solved in different ways and do not necessarily require service partitioning.</span></span>

<span data-ttu-id="3e00d-124">Ez a bemutató részében az állapotalapú szolgáltatások összpontosít.</span><span class="sxs-lookup"><span data-stu-id="3e00d-124">The remainder of this walkthrough focuses on stateful services.</span></span>

### <a name="partition-service-fabric-stateful-services"></a><span data-ttu-id="3e00d-125">A Service Fabric állapotalapú szolgáltatások partíció</span><span class="sxs-lookup"><span data-stu-id="3e00d-125">Partition Service Fabric stateful services</span></span>
<span data-ttu-id="3e00d-126">A Service Fabric megkönnyíti a partíció állapotba (adatok) első osztályú úgy felajánlásával méretezhető állapotalapú szolgáltatások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-126">Service Fabric makes it easy to develop scalable stateful services by offering a first-class way to partition state (data).</span></span> <span data-ttu-id="3e00d-127">Fogalmilag, is gondol a partíció egy állapotalapú szolgáltatás, amely nagymértékben megbízható keresztül skálázási egységként [replikák](service-fabric-availability-services.md) , amely az elosztott és a fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="3e00d-127">Conceptually, you can think about a partition of a stateful service as a scale unit that is highly reliable through [replicas](service-fabric-availability-services.md) that are distributed and balanced across the nodes in a cluster.</span></span>

<span data-ttu-id="3e00d-128">A Service Fabric állapotalapú szolgáltatások keretében particionálás határozhatja meg, hogy egy adott szolgáltatáshoz partíció feladata a teljes állapotát a szolgáltatás része hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="3e00d-128">Partitioning in the context of Service Fabric stateful services refers to the process of determining that a particular service partition is responsible for a portion of the complete state of the service.</span></span> <span data-ttu-id="3e00d-129">(Ahogy korábban említettük, a partíció egy olyan [replikák](service-fabric-availability-services.md)).</span><span class="sxs-lookup"><span data-stu-id="3e00d-129">(As mentioned before, a partition is a set of [replicas](service-fabric-availability-services.md)).</span></span> <span data-ttu-id="3e00d-130">A Service Fabric kiváló számítógépben, hogy másik csomópontjára helyezi a partíciókat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-130">A great thing about Service Fabric is that it places the partitions on different nodes.</span></span> <span data-ttu-id="3e00d-131">Ez lehetővé teszi egy erőforrás csomópontszámkorlát növekedjen őket.</span><span class="sxs-lookup"><span data-stu-id="3e00d-131">This allows them to grow to a node's resource limit.</span></span> <span data-ttu-id="3e00d-132">Az adatok igények nő, partíciók nő, és a Service Fabric partíciók-csomópontokon keresztüli újra egyensúlyba hozza..</span><span class="sxs-lookup"><span data-stu-id="3e00d-132">As the data needs grow, partitions grow, and Service Fabric rebalances partitions across nodes.</span></span> <span data-ttu-id="3e00d-133">Ez biztosítja, hogy a hardver-erőforrások hatékony alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="3e00d-133">This ensures the continued efficient use of hardware resources.</span></span>

<span data-ttu-id="3e00d-134">Így például, tegyük fel például, a kiindulási pont 5-csomópontból álló fürt és egy szolgáltatás, amely 10 partíciók és három replikák célja van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="3e00d-134">To give you an example, say you start with a 5-node cluster and a service that is configured to have 10 partitions and a target of three replicas.</span></span> <span data-ttu-id="3e00d-135">Ebben az esetben a Service Fabric ehhez elosztása és a replikák elosztják a fürt és meg szeretné végül két fő [replikák](service-fabric-availability-services.md) csomópontonként.</span><span class="sxs-lookup"><span data-stu-id="3e00d-135">In this case, Service Fabric would balance and distribute the replicas across the cluster--and you would end up with two primary [replicas](service-fabric-availability-services.md) per node.</span></span>
<span data-ttu-id="3e00d-136">Ha most szeretné terjessze ki a fürtöt, 10 csomópontok, a Service Fabric volna egyensúlyba az elsődleges [replikák](service-fabric-availability-services.md) minden 10 csomópontra.</span><span class="sxs-lookup"><span data-stu-id="3e00d-136">If you now need to scale out the cluster to 10 nodes, Service Fabric would rebalance the primary [replicas](service-fabric-availability-services.md) across all 10 nodes.</span></span> <span data-ttu-id="3e00d-137">Hasonlóképpen akkor vissza 5 csomópontok méretezhető, ha a Service Fabric volna egyensúlyba a replikák a 5 csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="3e00d-137">Likewise, if you scaled back to 5 nodes, Service Fabric would rebalance all the replicas across the 5 nodes.</span></span>  

<span data-ttu-id="3e00d-138">2. ábra 10 partíciók előtt és után a fürt skálázás eloszlását mutatja.</span><span class="sxs-lookup"><span data-stu-id="3e00d-138">Figure 2 shows the distribution of 10 partitions before and after scaling the cluster.</span></span>

![Az állapotalapú szolgáltatás](./media/service-fabric-concepts-partitioning/partitions.png)

<span data-ttu-id="3e00d-140">Ennek eredményeképpen a kibővített érhető el, mivel az ügyfelek kérelmeinek számítógépek különböző pontjain, az alkalmazás általános teljesítmény akkor javul, és adattömböket írnak való versengés csökken.</span><span class="sxs-lookup"><span data-stu-id="3e00d-140">As a result, the scale-out is achieved since requests from clients are distributed across computers, overall performance of the application is improved, and contention on access to chunks of data is reduced.</span></span>

## <a name="plan-for-partitioning"></a><span data-ttu-id="3e00d-141">Particionálás tervezése</span><span class="sxs-lookup"><span data-stu-id="3e00d-141">Plan for partitioning</span></span>
<span data-ttu-id="3e00d-142">A szolgáltatás üzembe, mielőtt mindig vegye figyelembe a particionálási stratégia szükséges ahhoz, hogy ki.</span><span class="sxs-lookup"><span data-stu-id="3e00d-142">Before implementing a service, you should always consider the partitioning strategy that is required to scale out.</span></span> <span data-ttu-id="3e00d-143">Különböző módja van, de ezek összpontosítani kell az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-143">There are different ways, but all of them focus on what the application needs to achieve.</span></span> <span data-ttu-id="3e00d-144">Ez a cikk a környezet Mérlegeljük, néhány fontosabb szempontjait.</span><span class="sxs-lookup"><span data-stu-id="3e00d-144">For the context of this article, let's consider some of the more important aspects.</span></span>

<span data-ttu-id="3e00d-145">Egy jó megoldás, az állapot, amely lehet particionálni első lépéseként meg kell a struktúra gondolniuk.</span><span class="sxs-lookup"><span data-stu-id="3e00d-145">A good approach is to think about the structure of the state that needs to be partitioned, as the first step.</span></span>

<span data-ttu-id="3e00d-146">Vegyünk egy egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="3e00d-146">Let's take a simple example.</span></span> <span data-ttu-id="3e00d-147">Ha a szolgáltatás egy countywide lekérdezési létrehozásához, létrehozhat egy partíció mindegyik városhoz megyét a.</span><span class="sxs-lookup"><span data-stu-id="3e00d-147">If you were to build a service for a countywide poll, you could create a partition for each city in the county.</span></span> <span data-ttu-id="3e00d-148">Ezt követően a szavazatok minden személy tárolhatja a partícióban, amely megfelel a város városban.</span><span class="sxs-lookup"><span data-stu-id="3e00d-148">Then, you could store the votes for every person in the city in the partition that corresponds to that city.</span></span> <span data-ttu-id="3e00d-149">3. ábra azt mutatja be, személyek és a város, amelyben található.</span><span class="sxs-lookup"><span data-stu-id="3e00d-149">Figure 3 illustrates a set of people and the city in which they reside.</span></span>

![Egyszerű partíció](./media/service-fabric-concepts-partitioning/cities.png)

<span data-ttu-id="3e00d-151">Mivel a feltöltési város széles körben változik, akkor fordulhatnak elő néhány nagy mennyiségű adatot (pl. budapest) tartalmazó partíciókat és a többi partíció csekély állapotú (pl. Kirkland).</span><span class="sxs-lookup"><span data-stu-id="3e00d-151">As the population of cities varies widely, you may end up with some partitions that contain a lot of data (e.g. Seattle) and other partitions with very little state (e.g. Kirkland).</span></span> <span data-ttu-id="3e00d-152">De mi hatása, hogy a partíciók állapot egyenetlen mennyiségű?</span><span class="sxs-lookup"><span data-stu-id="3e00d-152">So what is the impact of having partitions with uneven amounts of state?</span></span>

<span data-ttu-id="3e00d-153">Ha úgy gondolja, hogy a példában kapcsolatos újra, könnyen láthatja, hogy a partíció, amely tárolja a szavazatok Budapest fogja kapni az egyik Kirkland-nál nagyobb forgalmat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-153">If you think about the example again, you can easily see that the partition that holds the votes for Seattle will get more traffic than the Kirkland one.</span></span> <span data-ttu-id="3e00d-154">Alapértelmezés szerint a Service Fabric teszi arról, hogy minden egyes csomóponton elsődleges és másodlagos replikák azonos számú kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3e00d-154">By default, Service Fabric makes sure that there is about the same number of primary and secondary replicas on each node.</span></span> <span data-ttu-id="3e00d-155">Így használható a replikák átadott nagyobb forgalmat, míg mások kisebb forgalmat rendelkező csomópont.</span><span class="sxs-lookup"><span data-stu-id="3e00d-155">So you may end up with nodes that hold replicas that serve more traffic and others that serve less traffic.</span></span> <span data-ttu-id="3e00d-156">Lehetőleg célszerű és meleg tesztüzeméhez ilyen fürt elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="3e00d-156">You would preferably want to avoid hot and cold spots like this in a cluster.</span></span>

<span data-ttu-id="3e00d-157">Ennek elkerülése érdekében a particionálási szempontjából két dolgot kell tenni:</span><span class="sxs-lookup"><span data-stu-id="3e00d-157">In order to avoid this, you should do two things, from a partitioning point of view:</span></span>

* <span data-ttu-id="3e00d-158">Próbálja meg a partícióazonosító állapotát, hogy az összes partíciójára egyenletesen vannak elosztva.</span><span class="sxs-lookup"><span data-stu-id="3e00d-158">Try to partition the state so that it is evenly distributed across all partitions.</span></span>
* <span data-ttu-id="3e00d-159">A szolgáltatás a replikák mindegyike a betöltésének jelentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-159">Report load from each of the replicas for the service.</span></span> <span data-ttu-id="3e00d-160">(Kapcsolatban tekintse meg a cikk a [metrikák és a betöltés](service-fabric-cluster-resource-manager-metrics.md)).</span><span class="sxs-lookup"><span data-stu-id="3e00d-160">(For information on how, check out this article on [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md)).</span></span> <span data-ttu-id="3e00d-161">A Service Fabric lehetővé teszi a szolgáltatások, például a memória vagy a rekordok száma által felhasznált betöltésének jelentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-161">Service Fabric provides the capability to report load consumed by services, such as amount of memory or number of records.</span></span> <span data-ttu-id="3e00d-162">A jelentett mérőszámok alapján, a Service Fabric észleli, hogy az egyes partíciók többinél magasabb terhelés szolgál, és újra egyensúlyba hozza a fürt elérhetővé tétele a megfelelő csomópontok replikák azáltal, hogy a teljes nincs csomópont túl van terhelve.</span><span class="sxs-lookup"><span data-stu-id="3e00d-162">Based on the metrics reported, Service Fabric detects that some partitions are serving higher loads than others and rebalances the cluster by moving replicas to more suitable nodes, so that overall no node is overloaded.</span></span>

<span data-ttu-id="3e00d-163">Néha nem tudja, mennyi adatot lesznek az egyes partíciók eseménysorozatában.</span><span class="sxs-lookup"><span data-stu-id="3e00d-163">Sometimes, you cannot know how much data will be in a given partition.</span></span> <span data-ttu-id="3e00d-164">Egy általános javasoljuk, hogy szabaddá--először jó particionálási stratégia elfogadásával, amely az adatok egyenletesen legyen a partíciókat és a második, által terjed jelentéskészítési betöltése</span><span class="sxs-lookup"><span data-stu-id="3e00d-164">So a general recommendation is to do both--first, by adopting a partitioning strategy that spreads the data evenly across the partitions and second, by reporting load.</span></span>  <span data-ttu-id="3e00d-165">Az első módszer megakadályozza, hogy közben a második segítségével zökkenőmentes hozzáférést vagy terheléselosztási ideiglenes különbségeit ki adott idő alatt a szavazó példában bemutatott esetekben.</span><span class="sxs-lookup"><span data-stu-id="3e00d-165">The first method prevents situations described in the voting example, while the second helps smooth out temporary differences in access or load over time.</span></span>

<span data-ttu-id="3e00d-166">Egy másik partíció tervezési célja a először válassza ki a megfelelő számú partíciót.</span><span class="sxs-lookup"><span data-stu-id="3e00d-166">Another aspect of partition planning is to choose the correct number of partitions to begin with.</span></span>
<span data-ttu-id="3e00d-167">A Service Fabric szempontjából nincs szükség, amely megakadályozza, hogy kezdte meg az magasabb számú partíciót adott esetben a vártnál.</span><span class="sxs-lookup"><span data-stu-id="3e00d-167">From a Service Fabric perspective, there is nothing that prevents you from starting out with a higher number of partitions than anticipated for your scenario.</span></span>
<span data-ttu-id="3e00d-168">Feltéve, hogy a maximális számú partíciót valójában egy érvényes megközelítés.</span><span class="sxs-lookup"><span data-stu-id="3e00d-168">In fact, assuming the maximum number of partitions is a valid approach.</span></span>

<span data-ttu-id="3e00d-169">Ritka esetekben használható kellene kezdetben kiválasztott számánál több partíciót.</span><span class="sxs-lookup"><span data-stu-id="3e00d-169">In rare cases, you may end up needing more partitions than you have initially chosen.</span></span> <span data-ttu-id="3e00d-170">Bekövetkeztek a partíciók száma nem módosítható, mert néhány speciális partíció megoldások, például egy új szolgáltatás példányának létrehozásakor az azonos típusú szolgáltatás alkalmazni kellene.</span><span class="sxs-lookup"><span data-stu-id="3e00d-170">As you cannot change the partition count after the fact, you would need to apply some advanced partition approaches, such as creating a new service instance of the same service type.</span></span> <span data-ttu-id="3e00d-171">Meg kell valósítania néhány ügyféloldali logikát, amely a kérelmeket továbbítja a megfelelő szolgáltatáspéldány, az Ügyfélkód kell karbantartani ügyféloldali ismeretek alapján.</span><span class="sxs-lookup"><span data-stu-id="3e00d-171">You would also need to implement some client-side logic that routes the requests to the correct service instance, based on client-side knowledge that your client code must maintain.</span></span>

<span data-ttu-id="3e00d-172">Meg kell vizsgálni a particionálás a tervezés, a rendelkezésre álló számítógép-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-172">Another consideration for partitioning planning is the available computer resources.</span></span> <span data-ttu-id="3e00d-173">Az állapot igények is elérhető, és tárolja, akkor kötött kövesse:</span><span class="sxs-lookup"><span data-stu-id="3e00d-173">As the state needs to be accessed and stored, you are bound to follow:</span></span>

* <span data-ttu-id="3e00d-174">Hálózati sávszélesség korlátja</span><span class="sxs-lookup"><span data-stu-id="3e00d-174">Network bandwidth limits</span></span>
* <span data-ttu-id="3e00d-175">Rendszer memóriakorlátokat</span><span class="sxs-lookup"><span data-stu-id="3e00d-175">System memory limits</span></span>
* <span data-ttu-id="3e00d-176">Lemez tárolási korlátai</span><span class="sxs-lookup"><span data-stu-id="3e00d-176">Disk storage limits</span></span>

<span data-ttu-id="3e00d-177">Így mi történik, ha egy futó fürt erőforrás korlátokat futtatja?</span><span class="sxs-lookup"><span data-stu-id="3e00d-177">So what happens if you run into resource constraints in a running cluster?</span></span> <span data-ttu-id="3e00d-178">A válasz, hogy ki lehet egyszerűen terjeszteni a fürt az új követelmények teljesítése.</span><span class="sxs-lookup"><span data-stu-id="3e00d-178">The answer is that you can simply scale out the cluster to accommodate the new requirements.</span></span>

<span data-ttu-id="3e00d-179">[A kapacitástervezési útmutató](service-fabric-capacity-planning.md) arról, hogyan határozható meg a fürt kell hány csomópontja útmutatást nyújt.</span><span class="sxs-lookup"><span data-stu-id="3e00d-179">[The capacity planning guide](service-fabric-capacity-planning.md) offers guidance for how to determine how many nodes your cluster needs.</span></span>

## <a name="get-started-with-partitioning"></a><span data-ttu-id="3e00d-180">Ismerkedés a particionálás</span><span class="sxs-lookup"><span data-stu-id="3e00d-180">Get started with partitioning</span></span>
<span data-ttu-id="3e00d-181">Ez a szakasz ismerteti, hogyan lásson particionálás a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3e00d-181">This section describes how to get started with partitioning your service.</span></span>

<span data-ttu-id="3e00d-182">A Service Fabric kiválaszthatja, hogy három partíciós séma kínálja:</span><span class="sxs-lookup"><span data-stu-id="3e00d-182">Service Fabric offers a choice of three partition schemes:</span></span>

* <span data-ttu-id="3e00d-183">Címkiosztási particionálás (más néven UniformInt64Partition).</span><span class="sxs-lookup"><span data-stu-id="3e00d-183">Ranged partitioning (otherwise known as UniformInt64Partition).</span></span>
* <span data-ttu-id="3e00d-184">Nevű particionálást.</span><span class="sxs-lookup"><span data-stu-id="3e00d-184">Named partitioning.</span></span> <span data-ttu-id="3e00d-185">Ez a modell általában használó alkalmazások is lehet bucketed, a kötött belül adatokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="3e00d-185">Applications using this model usually have data that can be bucketed, within a bounded set.</span></span> <span data-ttu-id="3e00d-186">Néhány gyakori példán elnevezett partíciókulcsok használt adatmezők lenne, régiók, postai, felhasználói csoportok vagy egyéb üzleti határokat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-186">Some common examples of data fields used as named partition keys would be regions, postal codes, customer groups, or other business boundaries.</span></span>
* <span data-ttu-id="3e00d-187">Particionálás egypéldányos.</span><span class="sxs-lookup"><span data-stu-id="3e00d-187">Singleton partitioning.</span></span> <span data-ttu-id="3e00d-188">A szolgáltatás nem igényel további útválasztási egypéldányos partíciók általában használják.</span><span class="sxs-lookup"><span data-stu-id="3e00d-188">Singleton partitions are typically used when the service does not require any additional routing.</span></span> <span data-ttu-id="3e00d-189">Például a állapotmentes szolgáltatásokhoz használja ezt a particionálási sémát alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="3e00d-189">For example, stateless services use this partitioning scheme by default.</span></span>

<span data-ttu-id="3e00d-190">Nevű és egypéldányos particionálási sémák a következők: címkiosztási partíciók speciális formája.</span><span class="sxs-lookup"><span data-stu-id="3e00d-190">Named and Singleton partitioning schemes are special forms of ranged partitions.</span></span> <span data-ttu-id="3e00d-191">Alapértelmezés szerint a Visual Studio-sablonok a Service Fabric használatra címkiosztási particionálás, mert az általános és a hasznos azt.</span><span class="sxs-lookup"><span data-stu-id="3e00d-191">By default, the Visual Studio templates for Service Fabric use ranged partitioning, as it is the most common and useful one.</span></span> <span data-ttu-id="3e00d-192">Ez a cikk fennmaradó ranged particionálási sémát összpontosít.</span><span class="sxs-lookup"><span data-stu-id="3e00d-192">The remainder of this article focuses on the ranged partitioning scheme.</span></span>

### <a name="ranged-partitioning-scheme"></a><span data-ttu-id="3e00d-193">Címkiosztási particionálási sémát</span><span class="sxs-lookup"><span data-stu-id="3e00d-193">Ranged partitioning scheme</span></span>
<span data-ttu-id="3e00d-194">Adjon meg egy egész tartomány (a kis és nagy kulcsot által azonosított) és a partíciók (n) számos szolgál.</span><span class="sxs-lookup"><span data-stu-id="3e00d-194">This is used to specify an integer range (identified by a low key and high key) and a number of partitions (n).</span></span> <span data-ttu-id="3e00d-195">N partíciók, minden egyes felelős a teljes partíció-tartomány egy mozaikként, átfedés nélkül alosztály hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3e00d-195">It creates n partitions, each responsible for a non-overlapping subrange of the overall partition key range.</span></span> <span data-ttu-id="3e00d-196">Például egy ranged particionálási sémát 0 alacsony kulccsal, 99 magas kulcs, és a 4 számát hozna létre négy partíciót alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3e00d-196">For example, a ranged partitioning scheme with a low key of 0, a high key of 99, and a count of 4 would create four partitions, as shown below.</span></span>

![Particionálás között](./media/service-fabric-concepts-partitioning/range-partitioning.png)

<span data-ttu-id="3e00d-198">Általános gyakorlatként javasolt, ha az adatkészlet belül egyedi kulcs alapján kivonatát.</span><span class="sxs-lookup"><span data-stu-id="3e00d-198">A common approach is to create a hash based on a unique key within the data set.</span></span> <span data-ttu-id="3e00d-199">Néhány gyakori példán kulcsok lenne, a vehicle azonosító szám (VIN), az alkalmazott azonosítója vagy egy egyedi karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="3e00d-199">Some common examples of keys would be a vehicle identification number (VIN), an employee ID, or a unique string.</span></span> <span data-ttu-id="3e00d-200">Az egyedi kulccsal, majd egy kivonatoló kódot, a kulcs tartomány, a kulcs használandó modulus hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3e00d-200">By using this unique key, you would then generate a hash code, modulus the key range, to use as your key.</span></span> <span data-ttu-id="3e00d-201">Az alsó és felső határát az engedélyezett kulcs is megadhat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-201">You can specify the upper and lower bounds of the allowed key range.</span></span>

### <a name="select-a-hash-algorithm"></a><span data-ttu-id="3e00d-202">A kivonatoló algoritmus kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3e00d-202">Select a hash algorithm</span></span>
<span data-ttu-id="3e00d-203">A kivonatolás fontos része a kivonatoló algoritmus kijelölése.</span><span class="sxs-lookup"><span data-stu-id="3e00d-203">An important part of hashing is selecting your hash algorithm.</span></span> <span data-ttu-id="3e00d-204">Egy kell vizsgálni, hogy a cél az, hogy hasonló kulcsok egymást (helység bizalmas kivonatoláshoz) – csoport, vagy ha körben tevékenységet kell terjeszteni összes partíciójára (terjesztési kivonatoláshoz), amely napjainkban egyre általánosabbá.</span><span class="sxs-lookup"><span data-stu-id="3e00d-204">A consideration is whether the goal is to group similar keys near each other (locality sensitive hashing)--or if activity should be distributed broadly across all partitions (distribution hashing), which is more common.</span></span>

<span data-ttu-id="3e00d-205">Egy jó kivonatoló algoritmus mutatókat, hogy könnyen számítási, van néhány ütközések, és a kulcsok egyenletesen terjesztett.</span><span class="sxs-lookup"><span data-stu-id="3e00d-205">The characteristics of a good distribution hashing algorithm are that it is easy to compute, it has few collisions, and it distributes the keys evenly.</span></span> <span data-ttu-id="3e00d-206">Egy jó példa egy hatékony kivonatoló algoritmust a [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) kivonatoló algoritmus.</span><span class="sxs-lookup"><span data-stu-id="3e00d-206">A good example of an efficient hash algorithm is the [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash algorithm.</span></span>

<span data-ttu-id="3e00d-207">Általános kivonatoló kódot algoritmus lehetőségekért megfelelő erőforrás a [Wikipedia oldalon, kivonatoló függvényeket](http://en.wikipedia.org/wiki/Hash_function).</span><span class="sxs-lookup"><span data-stu-id="3e00d-207">A good resource for general hash code algorithm choices is the [Wikipedia page on hash functions](http://en.wikipedia.org/wiki/Hash_function).</span></span>

## <a name="build-a-stateful-service-with-multiple-partitions"></a><span data-ttu-id="3e00d-208">Több partíciót az állapotalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e00d-208">Build a stateful service with multiple partitions</span></span>
<span data-ttu-id="3e00d-209">Hozzuk létre az első megbízható állapotalapú szolgáltatás több partíciókat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-209">Let's create your first reliable stateful service with multiple partitions.</span></span> <span data-ttu-id="3e00d-210">Ebben a példában, hol szeretné tárolni a partícióra ugyanazzal a betűvel kezdődő összes Vezetéknév egy nagyon egyszerű alkalmazást fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3e00d-210">In this example, you will build a very simple application where you want to store all last names that start with the same letter in the same partition.</span></span>

<span data-ttu-id="3e00d-211">Kód írása előtt kell gondolniuk a partíciókat és a partíciós kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3e00d-211">Before you write any code, you need to think about the partitions and partition keys.</span></span> <span data-ttu-id="3e00d-212">26 partíciók (egy az ábécé minden betű), de mi van szükség az alacsony és magas kulcsok?</span><span class="sxs-lookup"><span data-stu-id="3e00d-212">You need 26 partitions (one for each letter in the alphabet), but what about the low and high keys?</span></span>
<span data-ttu-id="3e00d-213">Szó szeretnénk / levél egy partíciót, azt használatával 0, a kis és 25 magas kulcsa, mivel mindegyik betűnek a saját kulcs.</span><span class="sxs-lookup"><span data-stu-id="3e00d-213">As we literally want to have one partition per letter, we can use 0 as the low key and 25 as the high key, as each letter is its own key.</span></span>

> [!NOTE]
> <span data-ttu-id="3e00d-214">Ez egy webfarmos egyszerűsített, valójában a terjesztési egyenetlen lenne.</span><span class="sxs-lookup"><span data-stu-id="3e00d-214">This is a simplified scenario, as in reality the distribution would be uneven.</span></span> <span data-ttu-id="3e00d-215">Vezetéknév "S" vagy "M" betűk kezdve gyakoribb, mint az "X" kezdetű vagy az "Y".</span><span class="sxs-lookup"><span data-stu-id="3e00d-215">Last names starting with the letters "S" or "M" are more common than the ones starting with "X" or "Y".</span></span>
> 
> 

1. <span data-ttu-id="3e00d-216">Nyissa meg **Visual Studio** > **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="3e00d-216">Open **Visual Studio** > **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="3e00d-217">Az a **új projekt** párbeszédpanelen válassza ki a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3e00d-217">In the **New Project** dialog box, choose the Service Fabric application.</span></span>
3. <span data-ttu-id="3e00d-218">Hívja meg a projekt "AlphabetPartitions".</span><span class="sxs-lookup"><span data-stu-id="3e00d-218">Call the project "AlphabetPartitions".</span></span>
4. <span data-ttu-id="3e00d-219">Az a **szolgáltatás létrehozása** párbeszédpanelen válassza ki **állapotalapú alkalmazások és szolgáltatások** szolgáltatás, és hívja meg az "Alphabet.Processing" az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="3e00d-219">In the **Create a Service** dialog box, choose **Stateful** service and call it "Alphabet.Processing" as shown in the image below.</span></span>
       <span data-ttu-id="3e00d-220">![Visual Studio új szolgáltatás párbeszédpanelje][1]</span><span class="sxs-lookup"><span data-stu-id="3e00d-220">![New service dialog in Visual Studio][1]</span></span>

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. <span data-ttu-id="3e00d-221">A partíciók számának megadása.</span><span class="sxs-lookup"><span data-stu-id="3e00d-221">Set the number of partitions.</span></span> <span data-ttu-id="3e00d-222">Nyissa meg a Applicationmanifest.xml fájlt a AlphabetPartitions projekt ApplicationPackageRoot mappában található, és frissítse a paraméter Processing_PartitionCount 26 alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3e00d-222">Open the Applicationmanifest.xml file located in the ApplicationPackageRoot folder of the AlphabetPartitions project and update the parameter Processing_PartitionCount to 26 as shown below.</span></span>
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    <span data-ttu-id="3e00d-223">Szükség az alább látható módon ApplicationManifest.xml StatefulService elemében LowKey és HighKey tulajdonságainak frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-223">You also need to update the LowKey and HighKey properties of the StatefulService element in the ApplicationManifest.xml as shown below.</span></span>
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. <span data-ttu-id="3e00d-224">A szolgáltatás számára érhető el megnyílik egy portot a végpont ServiceManifest.xml (a PackageRoot mappában található), a végpont elem felvétele a Alphabet.Processing szolgáltatás alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="3e00d-224">For the service to be accessible, open up an endpoint on a port by adding the endpoint element of ServiceManifest.xml (located in the PackageRoot folder) for the Alphabet.Processing service as shown below:</span></span>
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    <span data-ttu-id="3e00d-225">A szolgáltatás most 26 partíciók belső végpont figyelésére van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="3e00d-225">Now the service is configured to listen to an internal endpoint with 26 partitions.</span></span>
7. <span data-ttu-id="3e00d-226">A következő lépésben bírálja felül a `CreateServiceReplicaListeners()` feldolgozási osztály.</span><span class="sxs-lookup"><span data-stu-id="3e00d-226">Next, you need to override the `CreateServiceReplicaListeners()` method of the Processing class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3e00d-227">Ebben a példában feltételezzük, hogy egy egyszerű HttpCommunicationListener használunk.</span><span class="sxs-lookup"><span data-stu-id="3e00d-227">For this sample, we assume that you are using a simple HttpCommunicationListener.</span></span> <span data-ttu-id="3e00d-228">A megbízható szolgáltatás kommunikációja további információkért lásd: [a megbízható kommunikációt modell](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="3e00d-228">For more information on reliable service communication, see [The Reliable Service communication model](service-fabric-reliable-services-communication.md).</span></span>
   > 
   > 
8. <span data-ttu-id="3e00d-229">Az URL-címhez, amely figyeli a replika egy ajánlott mintát a következő formátumban: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span><span class="sxs-lookup"><span data-stu-id="3e00d-229">A recommended pattern for the URL that a replica listens on is the following format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span></span>
    <span data-ttu-id="3e00d-230">Ezért konfigurálni szeretné a megfelelő végpontok, hogy az ebben a mintában figyelni a kommunikációs figyelő.</span><span class="sxs-lookup"><span data-stu-id="3e00d-230">So you want to configure your communication listener to listen on the correct endpoints and with this pattern.</span></span>
   
    <span data-ttu-id="3e00d-231">A szolgáltatás több replika előfordulhat, hogy futhat ugyanarra a számítógépre, ezért ez a cím egyedinek kell lennie a replikára.</span><span class="sxs-lookup"><span data-stu-id="3e00d-231">Multiple replicas of this service may be hosted on the same computer, so this address needs to be unique to the replica.</span></span> <span data-ttu-id="3e00d-232">Ezért Partícióazonosító + másodpéldány-azonosító az URL-cím van.</span><span class="sxs-lookup"><span data-stu-id="3e00d-232">This is why   partition ID + replica ID are in the URL.</span></span> <span data-ttu-id="3e00d-233">HttpListener figyelheti a több címet ugyanazt a portot, amíg az URL-előtagját egyedi.</span><span class="sxs-lookup"><span data-stu-id="3e00d-233">HttpListener can listen on multiple addresses on the same port as long as the URL prefix    is unique.</span></span>
   
    <span data-ttu-id="3e00d-234">A felesleges GUID van egy speciális eset, amelyen másodlagos replika is a kéréseket kell figyelnie csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="3e00d-234">The extra GUID is there for an advanced case where secondary replicas also listen for read-only requests.</span></span> <span data-ttu-id="3e00d-235">Ha ez a helyzet, győződjön meg arról, hogy egy új egyedi címet használatos való áttérés menetének elsődleges a másodlagos újra a címek feloldására ügyfelek kényszerítése szeretné.</span><span class="sxs-lookup"><span data-stu-id="3e00d-235">When that's the case, you want to make sure that a new unique address is used when transitioning from primary to secondary to force clients to re-resolve the address.</span></span> <span data-ttu-id="3e00d-236">"+" használatos a címet, hogy a replika figyel az összes elérhető gazdagépet (IP, FQDM, localhost stb.) Az alábbi kódot a példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="3e00d-236">'+' is used as the address here so that the replica listens on all available hosts (IP, FQDM, localhost, etc.) The code below shows an example.</span></span>
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    <span data-ttu-id="3e00d-237">Akkor is érdemes megjegyezni, hogy a közzétett URL-cím némileg eltér a figyelő URL-előtagját.</span><span class="sxs-lookup"><span data-stu-id="3e00d-237">It's also worth noting that the published URL is slightly different from the listening URL prefix.</span></span>
    <span data-ttu-id="3e00d-238">A figyelő URL-címet kap arra, hogy HttpListener.</span><span class="sxs-lookup"><span data-stu-id="3e00d-238">The listening URL is given to HttpListener.</span></span> <span data-ttu-id="3e00d-239">A közzétett URL-címe a Service Fabric-szolgáltatás, a szolgáltatásészlelés használt közzétett URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="3e00d-239">The published URL is the URL that is published to the Service Fabric Naming Service, which is used for service discovery.</span></span> <span data-ttu-id="3e00d-240">Az ügyfelek ekkor megkérdezi, ehhez a címhez, a felderítés szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="3e00d-240">Clients will ask for this address through that discovery service.</span></span> <span data-ttu-id="3e00d-241">A cím, az ügyfelek letöltik van szüksége a tényleges IP vagy FQDN-jét a csomópont használata esetén csatlakozhassanak.</span><span class="sxs-lookup"><span data-stu-id="3e00d-241">The address that clients get needs to have the actual IP or FQDN of the node in order to connect.</span></span> <span data-ttu-id="3e00d-242">Ki kell cserélni, "+", a csomópont IP-cím vagy teljes Tartománynevét, ahogy fent látható.</span><span class="sxs-lookup"><span data-stu-id="3e00d-242">So you need to replace '+' with the node's IP or FQDN as shown above.</span></span>
9. <span data-ttu-id="3e00d-243">Az utolsó lépés a feldolgozó logika hozzáadása a szolgáltatás alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3e00d-243">The last step is to add the processing logic to the service as shown below.</span></span>
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    <span data-ttu-id="3e00d-244">`ProcessInternalRequest`olvassa be a lekérdezési karakterlánc paraméter hívni a partíció és hívások használt értékek `AddUserAsync` a Vezetéknév hozzáadása a megbízható szótár `dictionary`.</span><span class="sxs-lookup"><span data-stu-id="3e00d-244">`ProcessInternalRequest` reads the values of the query string parameter used to call the partition and calls `AddUserAsync` to add the lastname to the reliable dictionary `dictionary`.</span></span>
10. <span data-ttu-id="3e00d-245">Adjunk állapotmentes szolgáltatások megtekintéséhez, hogy egy adott partíció meghívása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-245">Let's add a stateless service to the project to see how you can call a particular partition.</span></span>
    
    <span data-ttu-id="3e00d-246">Ez a szolgáltatás, amely elfogadja a lekérdezési karakterlánc paraméterként a Vezetéknév, meghatározza, hogy a partíciós kulcs, és elküldi a feldolgozás Alphabet.Processing szolgáltatás egyszerű webes felületet funkcionál.</span><span class="sxs-lookup"><span data-stu-id="3e00d-246">This service serves as a simple web interface that accepts the lastname as a query string parameter, determines the partition key, and sends it to the Alphabet.Processing service for processing.</span></span>
11. <span data-ttu-id="3e00d-247">Az a **szolgáltatás létrehozása** párbeszédpanelen válassza ki **Stateless** szolgáltatás, és hívja meg az "Alphabet.Web" alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3e00d-247">In the **Create a Service** dialog box, choose **Stateless** service and call it "Alphabet.Web" as shown below.</span></span>
    
    ![Állapotmentes szolgáltatások képernyőképe](./media/service-fabric-concepts-partitioning/createnewstateless.png)<span data-ttu-id="3e00d-249">.</span><span class="sxs-lookup"><span data-stu-id="3e00d-249">.</span></span>
12. <span data-ttu-id="3e00d-250">A végpont-információ a Alphabet.WebApi szolgáltatást egy portot az alább látható módon megnyílik a ServiceManifest.xml frissítésére.</span><span class="sxs-lookup"><span data-stu-id="3e00d-250">Update the endpoint information in the ServiceManifest.xml of the Alphabet.WebApi service to open up a port as shown below.</span></span>
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. <span data-ttu-id="3e00d-251">Kell visszaadnia ServiceInstanceListeners webes osztályban.</span><span class="sxs-lookup"><span data-stu-id="3e00d-251">You need to return a collection of ServiceInstanceListeners in the class Web.</span></span> <span data-ttu-id="3e00d-252">Ebben az esetben választhat egy egyszerű HttpCommunicationListener végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="3e00d-252">Again, you can choose to implement a simple HttpCommunicationListener.</span></span>
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is the node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. <span data-ttu-id="3e00d-253">Most kell megvalósítani a feldolgozó logika.</span><span class="sxs-lookup"><span data-stu-id="3e00d-253">Now you need to implement the processing logic.</span></span> <span data-ttu-id="3e00d-254">A HttpCommunicationListener hívások `ProcessInputRequest` Ha kérelem érkezik.</span><span class="sxs-lookup"><span data-stu-id="3e00d-254">The HttpCommunicationListener calls `ProcessInputRequest` when a request comes in.</span></span> <span data-ttu-id="3e00d-255">Ezért lépjen tovább, és adja hozzá az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="3e00d-255">So let's go ahead and add the code below.</span></span>
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from the first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added to Partition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    <span data-ttu-id="3e00d-256">Rajta lépésről lépésre bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="3e00d-256">Let's walk through it step by step.</span></span> <span data-ttu-id="3e00d-257">A kód beolvassa a lekérdezési karakterlánc paraméter első betűjének `lastname` történő karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="3e00d-257">The code reads the first letter of the query string parameter `lastname` into a char.</span></span> <span data-ttu-id="3e00d-258">Ezután határozza meg a partíciókulcs a levél hexadecimális értéket `A` és a Vezetéknév első betűjének hexadecimális érték közötti.</span><span class="sxs-lookup"><span data-stu-id="3e00d-258">Then, it determines the partition key for this letter by subtracting the hexadecimal value of `A` from the hexadecimal value of the last names' first letter.</span></span>
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    <span data-ttu-id="3e00d-259">Ne feledje, hogy a jelen példában használjuk 26 partíciók partíciónként több partíciós kulccsal.</span><span class="sxs-lookup"><span data-stu-id="3e00d-259">Remember, for this example, we are using 26 partitions with one partition key per partition.</span></span>
    <span data-ttu-id="3e00d-260">A következő azt beszerzése a szolgáltatás partíció `partition` a kulcs használatával a `ResolveAsync` metódust a `servicePartitionResolver` objektum.</span><span class="sxs-lookup"><span data-stu-id="3e00d-260">Next, we obtain the service partition `partition` for this key by using the `ResolveAsync` method on the `servicePartitionResolver` object.</span></span> <span data-ttu-id="3e00d-261">`servicePartitionResolver`típusúként van definiálva</span><span class="sxs-lookup"><span data-stu-id="3e00d-261">`servicePartitionResolver` is defined as</span></span>
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    <span data-ttu-id="3e00d-262">A `ResolveAsync` paraméterekként token a szolgáltatás URI-azonosítója, a partíciós kulcs és a megszakítási metódust vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3e00d-262">The `ResolveAsync` method takes the service URI, the partition key, and a cancellation token as parameters.</span></span> <span data-ttu-id="3e00d-263">A szolgáltatás a feldolgozási szolgáltatás URI nem `fabric:/AlphabetPartitions/Processing`.</span><span class="sxs-lookup"><span data-stu-id="3e00d-263">The service URI for the processing service is `fabric:/AlphabetPartitions/Processing`.</span></span> <span data-ttu-id="3e00d-264">A következő azt lekérése a végpont a partíció.</span><span class="sxs-lookup"><span data-stu-id="3e00d-264">Next, we get the endpoint of the partition.</span></span>
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    <span data-ttu-id="3e00d-265">Végül azt a végpont URL-cím és a lekérdezési karakterláncban hozza létre, és a feldolgozási szolgáltatás hívásához.</span><span class="sxs-lookup"><span data-stu-id="3e00d-265">Finally, we build the endpoint URL plus the querystring and call the processing service.</span></span>
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    <span data-ttu-id="3e00d-266">Ha a feldolgozás befejezése után azt írni a kimeneti vissza.</span><span class="sxs-lookup"><span data-stu-id="3e00d-266">Once the processing is done, we write the output back.</span></span>
15. <span data-ttu-id="3e00d-267">Az utolsó lépés a szolgáltatás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3e00d-267">The last step is to test the service.</span></span> <span data-ttu-id="3e00d-268">A Visual Studio által használt alkalmazás paramétereket a helyi és a felhő üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="3e00d-268">Visual Studio uses application parameters for local and cloud deployment.</span></span> <span data-ttu-id="3e00d-269">A szolgáltatás helyi 26 partíciókkal rendelkező teszteléséhez frissítenie kell a `Local.xml` fájlt a AlphabetPartitions projekt ApplicationParameters mappában alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="3e00d-269">To test the service with 26 partitions locally, you need to update the `Local.xml` file in the ApplicationParameters folder of the AlphabetPartitions project as shown below:</span></span>
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. <span data-ttu-id="3e00d-270">Miután befejezte a központi telepítés, ellenőrizheti a szolgáltatás és a Service Fabric Explorerben a partíciókat.</span><span class="sxs-lookup"><span data-stu-id="3e00d-270">Once you finish deployment, you can check the service and all of its partitions in the Service Fabric Explorer.</span></span>
    
    ![Service Fabric Explorer képernyőképe](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. <span data-ttu-id="3e00d-272">A böngészőben, tesztelheti a particionálási logika megadásával `http://localhost:8081/?lastname=somename`.</span><span class="sxs-lookup"><span data-stu-id="3e00d-272">In a browser, you can test the partitioning logic by entering `http://localhost:8081/?lastname=somename`.</span></span> <span data-ttu-id="3e00d-273">Látni fogja, hogy minden egyes Vezetéknév ugyanazzal a betűvel kezdődő tárolása az egyazon partícióra kerüljenek.</span><span class="sxs-lookup"><span data-stu-id="3e00d-273">You will see that each last name that starts with the same letter is being stored in the same partition.</span></span>
    
    ![Böngésző képernyőképe](./media/service-fabric-concepts-partitioning/samplerunning.png)

<span data-ttu-id="3e00d-275">A teljes forráskód a minta érhető el a [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="3e00d-275">The entire source code of the sample is available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e00d-276">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e00d-276">Next steps</span></span>
<span data-ttu-id="3e00d-277">A Service Fabric fogalmak információkért tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="3e00d-277">For information on Service Fabric concepts, see the following:</span></span>

* [<span data-ttu-id="3e00d-278">A Service Fabric-szolgáltatások rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="3e00d-278">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="3e00d-279">Méretezhetőséget biztosít a Service Fabric-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3e00d-279">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="3e00d-280">Kapacitástervezés a Service Fabric-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="3e00d-280">Capacity planning for Service Fabric applications</span></span>](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png