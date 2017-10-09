---
title: "a Service Fabric-szolgáltatások aaaPartitioning |} Microsoft Docs"
description: "Ismerteti, hogyan toopartition Service Fabric állapotalapú szolgáltatások. Partíciók lehetővé teszi, hogy adattárolás hello helyi gépen, adatokat és a számítás is méretezhető együtt."
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
ms.openlocfilehash: 6ead48716c08f4212535202ee69d169067d5c6d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partition-service-fabric-reliable-services"></a><span data-ttu-id="3fd61-104">A Service Fabric megbízható szolgáltatások partíció</span><span class="sxs-lookup"><span data-stu-id="3fd61-104">Partition Service Fabric reliable services</span></span>
<span data-ttu-id="3fd61-105">A cikkben egy bevezető toohello Azure Service Fabric megbízható szolgáltatások particionálás alapvető fogalmait.</span><span class="sxs-lookup"><span data-stu-id="3fd61-105">This article provides an introduction toohello basic concepts of partitioning Azure Service Fabric reliable services.</span></span> <span data-ttu-id="3fd61-106">hello hello cikkben használt forráskód is rendelkezésre áll a [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="3fd61-106">hello source code used in hello article is also available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="partitioning"></a><span data-ttu-id="3fd61-107">Particionálás</span><span class="sxs-lookup"><span data-stu-id="3fd61-107">Partitioning</span></span>
<span data-ttu-id="3fd61-108">Particionálás nincs egyedi tooService háló.</span><span class="sxs-lookup"><span data-stu-id="3fd61-108">Partitioning is not unique tooService Fabric.</span></span> <span data-ttu-id="3fd61-109">Ez valójában egy alapvető szerkezet méretezhető szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="3fd61-109">In fact, it is a core pattern of building scalable services.</span></span> <span data-ttu-id="3fd61-110">Egy tágabb értelemben véve a azt gondolja át particionálás egy fogalom felosztásával állapota (adatok), és kisebb elérhető egységek tooimprove méretezhetőséget és teljesítményt nyújt a számítási.</span><span class="sxs-lookup"><span data-stu-id="3fd61-110">In a broader sense, we can think about partitioning as a concept of dividing state (data) and compute into smaller accessible units tooimprove scalability and performance.</span></span> <span data-ttu-id="3fd61-111">Egy jól ismert particionálás formátuma [adatparticionálás][wikipartition], más néven horizontális.</span><span class="sxs-lookup"><span data-stu-id="3fd61-111">A well-known form of partitioning is [data partitioning][wikipartition], also known as sharding.</span></span>

### <a name="partition-service-fabric-stateless-services"></a><span data-ttu-id="3fd61-112">A Service Fabric állapotmentes szolgáltatások partíció</span><span class="sxs-lookup"><span data-stu-id="3fd61-112">Partition Service Fabric stateless services</span></span>
<span data-ttu-id="3fd61-113">Állapotmentes szolgáltatások esetén azt is gondolja át éppen egy logikai egységet a szolgáltatás egy vagy több példányát tartalmazó partíció.</span><span class="sxs-lookup"><span data-stu-id="3fd61-113">For stateless services, you can think about a partition being a logical unit that contains one or more instances of a service.</span></span> <span data-ttu-id="3fd61-114">1. ábra mutatja egy állapotmentes szolgáltatások elosztva a fürt egy partíciót öt osztályt.</span><span class="sxs-lookup"><span data-stu-id="3fd61-114">Figure 1 shows a stateless service with five instances distributed across a cluster using one partition.</span></span>

![Állapotmentes szolgáltatások](./media/service-fabric-concepts-partitioning/statelessinstances.png)

<span data-ttu-id="3fd61-116">Valóban két típusa van állapotmentes szolgáltatások megoldások.</span><span class="sxs-lookup"><span data-stu-id="3fd61-116">There are really two types of stateless service solutions.</span></span> <span data-ttu-id="3fd61-117">hello először egyik olyan szolgáltatás, amely továbbra is fennáll állapotában kívülről, például egy Azure SQL-adatbázis (például egy webhely, amely tárolja a hello munkamenet-információk és adatok).</span><span class="sxs-lookup"><span data-stu-id="3fd61-117">hello first one is a service that persists its state externally, for example in an Azure SQL database (like a website that stores hello session information and data).</span></span> <span data-ttu-id="3fd61-118">hello második csak számítási-szolgáltatások (például egy Számológép vagy kép thumbnailing), amelyeket nem kezel olyan állandó állapotokat.</span><span class="sxs-lookup"><span data-stu-id="3fd61-118">hello second one is computation-only services (like a calculator or image thumbnailing) that do not manage any persistent state.</span></span>

<span data-ttu-id="3fd61-119">A vagy állapotmentes szolgáltatások particionálás esetben egy nagyon ritkán forgatókönyv – a méretezhetőség és rendelkezésre állási általában több példány hozzáadásával érhető el.</span><span class="sxs-lookup"><span data-stu-id="3fd61-119">In either case, partitioning a stateless service is a very rare scenario--scalability and availability are normally achieved by adding more instances.</span></span> <span data-ttu-id="3fd61-120">kérelmek hello csak időtartamát tooconsider toomeet különleges útválasztási van szüksége több partíciót állapotmentes szolgáltatások-példányok esetén.</span><span class="sxs-lookup"><span data-stu-id="3fd61-120">hello only time you want tooconsider multiple partitions for stateless service instances is when you need toomeet special routing requests.</span></span>

<span data-ttu-id="3fd61-121">Tegyük fel fontolja meg egy olyan esetben, ha egy bizonyos tartomány-azonosítóval rendelkező felhasználók csak szolgáltatható által egy adott szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="3fd61-121">As an example, consider a case where users with IDs in a certain range should only be served by a particular service instance.</span></span> <span data-ttu-id="3fd61-122">Ha sikerült partícióazonosító állapotmentes szolgáltatások egy másik példa, amikor egy valóban particionált háttér (pl. szilánkos SQL-adatbázis) rendelkezik, és azt szeretné, hogy mely szolgáltatáspéldány kell toohello adatbázis shard--írási vagy más belül előkészítő feladatok végrehajtására toocontrol hello állapotmentes szolgáltatások igénylő hello azonos partíciós információi szerint hello háttér használatban van.</span><span class="sxs-lookup"><span data-stu-id="3fd61-122">Another example of when you could partition a stateless service is when you have a truly partitioned backend (e.g. a sharded SQL database) and you want toocontrol which service instance should write toohello database shard--or perform other preparation work within hello stateless service that requires hello same partitioning information as is used in hello backend.</span></span> <span data-ttu-id="3fd61-123">Ilyen típusú forgatókönyvek is különböző módon kell megoldani, és nem feltétlenül igényel szolgáltatás particionálást.</span><span class="sxs-lookup"><span data-stu-id="3fd61-123">Those types of scenarios can also be solved in different ways and do not necessarily require service partitioning.</span></span>

<span data-ttu-id="3fd61-124">Ez a bemutató részében hello állapotalapú szolgáltatások összpontosít.</span><span class="sxs-lookup"><span data-stu-id="3fd61-124">hello remainder of this walkthrough focuses on stateful services.</span></span>

### <a name="partition-service-fabric-stateful-services"></a><span data-ttu-id="3fd61-125">A Service Fabric állapotalapú szolgáltatások partíció</span><span class="sxs-lookup"><span data-stu-id="3fd61-125">Partition Service Fabric stateful services</span></span>
<span data-ttu-id="3fd61-126">A Service Fabric teszi könnyen toodevelop méretezhető állapotalapú szolgáltatások első osztályú úgy felajánlásával toopartition állapota (adatok).</span><span class="sxs-lookup"><span data-stu-id="3fd61-126">Service Fabric makes it easy toodevelop scalable stateful services by offering a first-class way toopartition state (data).</span></span> <span data-ttu-id="3fd61-127">Fogalmilag, is gondol a partíció egy állapotalapú szolgáltatás, amely nagymértékben megbízható keresztül skálázási egységként [replikák](service-fabric-availability-services.md) , amely az elosztott és hello fürtben található csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="3fd61-127">Conceptually, you can think about a partition of a stateful service as a scale unit that is highly reliable through [replicas](service-fabric-availability-services.md) that are distributed and balanced across hello nodes in a cluster.</span></span>

<span data-ttu-id="3fd61-128">A Service Fabric állapotalapú szolgáltatások hello környezetében particionálás hivatkozik toohello folyamat meghatározása, hogy egy adott szolgáltatáshoz partíció feladata hello hello szolgáltatás teljes állapota része.</span><span class="sxs-lookup"><span data-stu-id="3fd61-128">Partitioning in hello context of Service Fabric stateful services refers toohello process of determining that a particular service partition is responsible for a portion of hello complete state of hello service.</span></span> <span data-ttu-id="3fd61-129">(Ahogy korábban említettük, a partíció egy olyan [replikák](service-fabric-availability-services.md)).</span><span class="sxs-lookup"><span data-stu-id="3fd61-129">(As mentioned before, a partition is a set of [replicas](service-fabric-availability-services.md)).</span></span> <span data-ttu-id="3fd61-130">A Service Fabric kiváló számítógépben, hogy különböző csomópontokon hello partíciók helyezi.</span><span class="sxs-lookup"><span data-stu-id="3fd61-130">A great thing about Service Fabric is that it places hello partitions on different nodes.</span></span> <span data-ttu-id="3fd61-131">Ez lehetővé teszi őket toogrow tooa csomópont erőforrás korlátját.</span><span class="sxs-lookup"><span data-stu-id="3fd61-131">This allows them toogrow tooa node's resource limit.</span></span> <span data-ttu-id="3fd61-132">Hello adatok igények nő, partíciók nő, és a Service Fabric partíciók-csomópontokon keresztüli újra egyensúlyba hozza..</span><span class="sxs-lookup"><span data-stu-id="3fd61-132">As hello data needs grow, partitions grow, and Service Fabric rebalances partitions across nodes.</span></span> <span data-ttu-id="3fd61-133">Ez biztosítja, hogy hello további hardver-erőforrások hatékony használatát.</span><span class="sxs-lookup"><span data-stu-id="3fd61-133">This ensures hello continued efficient use of hardware resources.</span></span>

<span data-ttu-id="3fd61-134">toogive például mondja meg az 5-csomópontból álló fürt és a beállított toohave 10 partíciók és három replikák célja egy szolgáltatás elindítása.</span><span class="sxs-lookup"><span data-stu-id="3fd61-134">toogive you an example, say you start with a 5-node cluster and a service that is configured toohave 10 partitions and a target of three replicas.</span></span> <span data-ttu-id="3fd61-135">Ebben az esetben a Service Fabric volna elosztása és hello replikák elosztják a hello fürt--és meg kellene végül két fő [replikák](service-fabric-availability-services.md) csomópontonként.</span><span class="sxs-lookup"><span data-stu-id="3fd61-135">In this case, Service Fabric would balance and distribute hello replicas across hello cluster--and you would end up with two primary [replicas](service-fabric-availability-services.md) per node.</span></span>
<span data-ttu-id="3fd61-136">Ha most kell tooscale hello too10 fürtcsomópontok ki, a Service Fabric volna egyensúlyba hello elsődleges [replikák](service-fabric-availability-services.md) minden 10 csomópontra.</span><span class="sxs-lookup"><span data-stu-id="3fd61-136">If you now need tooscale out hello cluster too10 nodes, Service Fabric would rebalance hello primary [replicas](service-fabric-availability-services.md) across all 10 nodes.</span></span> <span data-ttu-id="3fd61-137">Hasonlóképpen méretezhető csomópontja hátsó too5, ha a Service Fabric volna egyensúlyba összes hello replika hello 5-csomópont között.</span><span class="sxs-lookup"><span data-stu-id="3fd61-137">Likewise, if you scaled back too5 nodes, Service Fabric would rebalance all hello replicas across hello 5 nodes.</span></span>  

<span data-ttu-id="3fd61-138">2. ábrán látható hello terjesztési 10 partíciók előtt és után hello fürt méretezése.</span><span class="sxs-lookup"><span data-stu-id="3fd61-138">Figure 2 shows hello distribution of 10 partitions before and after scaling hello cluster.</span></span>

![Az állapotalapú szolgáltatás](./media/service-fabric-concepts-partitioning/partitions.png)

<span data-ttu-id="3fd61-140">Ennek eredményeképpen hello kibővített érhető el, mert ügyfelek számítógépek különböző pontjain, hello alkalmazás általános teljesítmény akkor javul, és az adatok a hozzáférés toochunks versengés csökken.</span><span class="sxs-lookup"><span data-stu-id="3fd61-140">As a result, hello scale-out is achieved since requests from clients are distributed across computers, overall performance of hello application is improved, and contention on access toochunks of data is reduced.</span></span>

## <a name="plan-for-partitioning"></a><span data-ttu-id="3fd61-141">Particionálás tervezése</span><span class="sxs-lookup"><span data-stu-id="3fd61-141">Plan for partitioning</span></span>
<span data-ttu-id="3fd61-142">A szolgáltatás üzembe, mielőtt mindig érdemes particionálási stratégia, amely szükséges tooscale kimenő hello. Különböző módja van, de ezek milyen hello alkalmazást tooachieve kell összpontosítania.</span><span class="sxs-lookup"><span data-stu-id="3fd61-142">Before implementing a service, you should always consider hello partitioning strategy that is required tooscale out. There are different ways, but all of them focus on what hello application needs tooachieve.</span></span> <span data-ttu-id="3fd61-143">Ez a cikk hello környezethez Mérlegeljük hello némelyike több fontos szempont.</span><span class="sxs-lookup"><span data-stu-id="3fd61-143">For hello context of this article, let's consider some of hello more important aspects.</span></span>

<span data-ttu-id="3fd61-144">Egy jó megoldás, toothink kapcsolatos hello struktúra, amelyet a particionált, hello első lépéseként toobe hello állapot.</span><span class="sxs-lookup"><span data-stu-id="3fd61-144">A good approach is toothink about hello structure of hello state that needs toobe partitioned, as hello first step.</span></span>

<span data-ttu-id="3fd61-145">Vegyünk egy egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="3fd61-145">Let's take a simple example.</span></span> <span data-ttu-id="3fd61-146">Ha a szolgáltatás egy countywide lekérdezési toobuild, létrehozhat egy partíció mindegyik városhoz hello megye a.</span><span class="sxs-lookup"><span data-stu-id="3fd61-146">If you were toobuild a service for a countywide poll, you could create a partition for each city in hello county.</span></span> <span data-ttu-id="3fd61-147">Ezt követően tárolhatja minden személy hello szavazatok hello városban, amely megfelel a toothat város hello partícióban.</span><span class="sxs-lookup"><span data-stu-id="3fd61-147">Then, you could store hello votes for every person in hello city in hello partition that corresponds toothat city.</span></span> <span data-ttu-id="3fd61-148">3. ábra azt mutatja be, személyek és hello város, amelyben található készlete.</span><span class="sxs-lookup"><span data-stu-id="3fd61-148">Figure 3 illustrates a set of people and hello city in which they reside.</span></span>

![Egyszerű partíció](./media/service-fabric-concepts-partitioning/cities.png)

<span data-ttu-id="3fd61-150">Mivel hello feltöltési város széles körben változik, akkor előfordulhat, hogy végül néhány nagy mennyiségű adatot (pl. budapest) tartalmazó partíciókat és a többi partíció csekély állapotú (pl. Kirkland).</span><span class="sxs-lookup"><span data-stu-id="3fd61-150">As hello population of cities varies widely, you may end up with some partitions that contain a lot of data (e.g. Seattle) and other partitions with very little state (e.g. Kirkland).</span></span> <span data-ttu-id="3fd61-151">De mi, hogy a partíciók állapot egyenetlen mennyiségű hello hatás?</span><span class="sxs-lookup"><span data-stu-id="3fd61-151">So what is hello impact of having partitions with uneven amounts of state?</span></span>

<span data-ttu-id="3fd61-152">Ha úgy gondolja, hogy kapcsolatos hello példa újra, könnyen láthatja, hogy hello partíció, amely tárolja a budapesti kvórumszavazatokat hello kap egy hello Kirkland-nál nagyobb forgalmat.</span><span class="sxs-lookup"><span data-stu-id="3fd61-152">If you think about hello example again, you can easily see that hello partition that holds hello votes for Seattle will get more traffic than hello Kirkland one.</span></span> <span data-ttu-id="3fd61-153">Alapértelmezés szerint a Service Fabric gondoskodik arról, hogy nincs kapcsolatos hello azonos számú elsődleges és másodlagos replikák minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="3fd61-153">By default, Service Fabric makes sure that there is about hello same number of primary and secondary replicas on each node.</span></span> <span data-ttu-id="3fd61-154">Így használható a replikák átadott nagyobb forgalmat, míg mások kisebb forgalmat rendelkező csomópont.</span><span class="sxs-lookup"><span data-stu-id="3fd61-154">So you may end up with nodes that hold replicas that serve more traffic and others that serve less traffic.</span></span> <span data-ttu-id="3fd61-155">Lehetőleg érdemes tooavoid kiemelt és cold tesztüzeméhez, például a fürtben.</span><span class="sxs-lookup"><span data-stu-id="3fd61-155">You would preferably want tooavoid hot and cold spots like this in a cluster.</span></span>

<span data-ttu-id="3fd61-156">A rendezés tooavoid ezt, a particionálási szempontjából két dolgot kell tenni:</span><span class="sxs-lookup"><span data-stu-id="3fd61-156">In order tooavoid this, you should do two things, from a partitioning point of view:</span></span>

* <span data-ttu-id="3fd61-157">Próbálja toopartition hello állapotát, hogy az összes partíciójára egyenletesen vannak elosztva.</span><span class="sxs-lookup"><span data-stu-id="3fd61-157">Try toopartition hello state so that it is evenly distributed across all partitions.</span></span>
* <span data-ttu-id="3fd61-158">Az egyes hello replikák hello szolgáltatás betöltésének jelentéséhez.</span><span class="sxs-lookup"><span data-stu-id="3fd61-158">Report load from each of hello replicas for hello service.</span></span> <span data-ttu-id="3fd61-159">(Kapcsolatban tekintse meg a cikk a [metrikák és a betöltés](service-fabric-cluster-resource-manager-metrics.md)).</span><span class="sxs-lookup"><span data-stu-id="3fd61-159">(For information on how, check out this article on [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md)).</span></span> <span data-ttu-id="3fd61-160">A Service Fabric biztosítja hello funkció tooreport hálózati szolgáltatások, például a memória vagy a rekordok száma használni.</span><span class="sxs-lookup"><span data-stu-id="3fd61-160">Service Fabric provides hello capability tooreport load consumed by services, such as amount of memory or number of records.</span></span> <span data-ttu-id="3fd61-161">Jelentett hello mérőszámok alapján, a Service Fabric észleli, hogy az egyes partíciók többinél magasabb terhelés szolgál, és újra egyensúlyba hozza a mozgóátlag replikák toomore megfelelő csomópontok, hello fürt teljes nincs csomópont túl van terhelve.</span><span class="sxs-lookup"><span data-stu-id="3fd61-161">Based on hello metrics reported, Service Fabric detects that some partitions are serving higher loads than others and rebalances hello cluster by moving replicas toomore suitable nodes, so that overall no node is overloaded.</span></span>

<span data-ttu-id="3fd61-162">Néha nem tudja, mennyi adatot lesznek az egyes partíciók eseménysorozatában.</span><span class="sxs-lookup"><span data-stu-id="3fd61-162">Sometimes, you cannot know how much data will be in a given partition.</span></span> <span data-ttu-id="3fd61-163">Így egy általános javaslat toodo mindkét--először jó particionálási stratégia elfogadásával, amelyek között osztja el hello adatok egyenletesen hello partíciókat és a második, jelentéskészítési terhelés által.</span><span class="sxs-lookup"><span data-stu-id="3fd61-163">So a general recommendation is toodo both--first, by adopting a partitioning strategy that spreads hello data evenly across hello partitions and second, by reporting load.</span></span>  <span data-ttu-id="3fd61-164">első módszer hello megakadályozza, hogy a példában szavazás során hello második segítségével zökkenőmentes hozzáférést vagy terheléselosztási ideiglenes különbségeit kimenő időbeli hello helyzet.</span><span class="sxs-lookup"><span data-stu-id="3fd61-164">hello first method prevents situations described in hello voting example, while hello second helps smooth out temporary differences in access or load over time.</span></span>

<span data-ttu-id="3fd61-165">Egy másik partíció tervezési célja toochoose hello számát a partíciók toobegin.</span><span class="sxs-lookup"><span data-stu-id="3fd61-165">Another aspect of partition planning is toochoose hello correct number of partitions toobegin with.</span></span>
<span data-ttu-id="3fd61-166">A Service Fabric szempontjából nincs szükség, amely megakadályozza, hogy kezdte meg az magasabb számú partíciót adott esetben a vártnál.</span><span class="sxs-lookup"><span data-stu-id="3fd61-166">From a Service Fabric perspective, there is nothing that prevents you from starting out with a higher number of partitions than anticipated for your scenario.</span></span>
<span data-ttu-id="3fd61-167">Feltéve, hogy hello több partíció valójában egy érvényes megközelítés.</span><span class="sxs-lookup"><span data-stu-id="3fd61-167">In fact, assuming hello maximum number of partitions is a valid approach.</span></span>

<span data-ttu-id="3fd61-168">Ritka esetekben használható kellene kezdetben kiválasztott számánál több partíciót.</span><span class="sxs-lookup"><span data-stu-id="3fd61-168">In rare cases, you may end up needing more partitions than you have initially chosen.</span></span> <span data-ttu-id="3fd61-169">Hello partíciószám után hello tény nem módosítható, mert kellene tooapply néhány speciális partíció módszerek, például létrehozhat egy új szolgáltatás példányának hello azonos szolgáltatás típusa.</span><span class="sxs-lookup"><span data-stu-id="3fd61-169">As you cannot change hello partition count after hello fact, you would need tooapply some advanced partition approaches, such as creating a new service instance of hello same service type.</span></span> <span data-ttu-id="3fd61-170">Módosítania kell tooimplement néhány ügyféloldali logika, amely továbbítja a hello kérelmek toohello megfelelő szolgáltatáspéldány, az Ügyfélkód kell karbantartani ügyféloldali ismeretek alapján.</span><span class="sxs-lookup"><span data-stu-id="3fd61-170">You would also need tooimplement some client-side logic that routes hello requests toohello correct service instance, based on client-side knowledge that your client code must maintain.</span></span>

<span data-ttu-id="3fd61-171">Meg kell vizsgálni a particionálás tervezési, hello elérhető számítógép-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3fd61-171">Another consideration for partitioning planning is hello available computer resources.</span></span> <span data-ttu-id="3fd61-172">Hello állapot igények toobe érhető el, és tárolja, kötött toofollow áll:</span><span class="sxs-lookup"><span data-stu-id="3fd61-172">As hello state needs toobe accessed and stored, you are bound toofollow:</span></span>

* <span data-ttu-id="3fd61-173">Hálózati sávszélesség korlátja</span><span class="sxs-lookup"><span data-stu-id="3fd61-173">Network bandwidth limits</span></span>
* <span data-ttu-id="3fd61-174">Rendszer memóriakorlátokat</span><span class="sxs-lookup"><span data-stu-id="3fd61-174">System memory limits</span></span>
* <span data-ttu-id="3fd61-175">Lemez tárolási korlátai</span><span class="sxs-lookup"><span data-stu-id="3fd61-175">Disk storage limits</span></span>

<span data-ttu-id="3fd61-176">Így mi történik, ha egy futó fürt erőforrás korlátokat futtatja? hello választ ki, hogy egyszerűen méretezheti hello fürt tooaccommodate hello új követelményeket kell.</span><span class="sxs-lookup"><span data-stu-id="3fd61-176">So what happens if you run into resource constraints in a running cluster? hello answer is that you can simply scale out hello cluster tooaccommodate hello new requirements.</span></span>

<span data-ttu-id="3fd61-177">[kapacitástervezési útmutató hello](service-fabric-capacity-planning.md) útmutatást kínál toodetermine hány csomópontokat a fürthöz kell.</span><span class="sxs-lookup"><span data-stu-id="3fd61-177">[hello capacity planning guide](service-fabric-capacity-planning.md) offers guidance for how toodetermine how many nodes your cluster needs.</span></span>

## <a name="get-started-with-partitioning"></a><span data-ttu-id="3fd61-178">Ismerkedés a particionálás</span><span class="sxs-lookup"><span data-stu-id="3fd61-178">Get started with partitioning</span></span>
<span data-ttu-id="3fd61-179">Ez a szakasz ismerteti, hogyan tooget a szolgáltatás particionálás használatába.</span><span class="sxs-lookup"><span data-stu-id="3fd61-179">This section describes how tooget started with partitioning your service.</span></span>

<span data-ttu-id="3fd61-180">A Service Fabric kiválaszthatja, hogy három partíciós séma kínálja:</span><span class="sxs-lookup"><span data-stu-id="3fd61-180">Service Fabric offers a choice of three partition schemes:</span></span>

* <span data-ttu-id="3fd61-181">Címkiosztási particionálás (más néven UniformInt64Partition).</span><span class="sxs-lookup"><span data-stu-id="3fd61-181">Ranged partitioning (otherwise known as UniformInt64Partition).</span></span>
* <span data-ttu-id="3fd61-182">Nevű particionálást.</span><span class="sxs-lookup"><span data-stu-id="3fd61-182">Named partitioning.</span></span> <span data-ttu-id="3fd61-183">Ez a modell általában használó alkalmazások is lehet bucketed, a kötött belül adatokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="3fd61-183">Applications using this model usually have data that can be bucketed, within a bounded set.</span></span> <span data-ttu-id="3fd61-184">Néhány gyakori példán elnevezett partíciókulcsok használt adatmezők lenne, régiók, postai, felhasználói csoportok vagy egyéb üzleti határokat.</span><span class="sxs-lookup"><span data-stu-id="3fd61-184">Some common examples of data fields used as named partition keys would be regions, postal codes, customer groups, or other business boundaries.</span></span>
* <span data-ttu-id="3fd61-185">Particionálás egypéldányos.</span><span class="sxs-lookup"><span data-stu-id="3fd61-185">Singleton partitioning.</span></span> <span data-ttu-id="3fd61-186">Hello szolgáltatást nem igényel további útválasztási egypéldányos partíciók általában használják.</span><span class="sxs-lookup"><span data-stu-id="3fd61-186">Singleton partitions are typically used when hello service does not require any additional routing.</span></span> <span data-ttu-id="3fd61-187">Például a állapotmentes szolgáltatásokhoz használja ezt a particionálási sémát alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="3fd61-187">For example, stateless services use this partitioning scheme by default.</span></span>

<span data-ttu-id="3fd61-188">Nevű és egypéldányos particionálási sémák a következők: címkiosztási partíciók speciális formája.</span><span class="sxs-lookup"><span data-stu-id="3fd61-188">Named and Singleton partitioning schemes are special forms of ranged partitions.</span></span> <span data-ttu-id="3fd61-189">Alapértelmezés szerint a Service Fabric használatra hello Visual Studio sablonok címkiosztási particionálás, mert az hello közös és a hasznos egy.</span><span class="sxs-lookup"><span data-stu-id="3fd61-189">By default, hello Visual Studio templates for Service Fabric use ranged partitioning, as it is hello most common and useful one.</span></span> <span data-ttu-id="3fd61-190">hello a cikk hátralévő része hello címkiosztási particionálási sémát összpontosít.</span><span class="sxs-lookup"><span data-stu-id="3fd61-190">hello remainder of this article focuses on hello ranged partitioning scheme.</span></span>

### <a name="ranged-partitioning-scheme"></a><span data-ttu-id="3fd61-191">Címkiosztási particionálási sémát</span><span class="sxs-lookup"><span data-stu-id="3fd61-191">Ranged partitioning scheme</span></span>
<span data-ttu-id="3fd61-192">Ez az egész használt toospecify (azonosított egy kis és nagy kulcsot) tartomány- és a partíciók (n) száma.</span><span class="sxs-lookup"><span data-stu-id="3fd61-192">This is used toospecify an integer range (identified by a low key and high key) and a number of partitions (n).</span></span> <span data-ttu-id="3fd61-193">N partíciók, minden egyes felelős a hello egy mozaikként, átfedés nélkül alosztály hoz létre teljes partícióazonosító kulcs tartományon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-193">It creates n partitions, each responsible for a non-overlapping subrange of hello overall partition key range.</span></span> <span data-ttu-id="3fd61-194">Például egy ranged particionálási sémát 0 alacsony kulccsal, 99 magas kulcs, és a 4 számát hozna létre négy partíciót alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-194">For example, a ranged partitioning scheme with a low key of 0, a high key of 99, and a count of 4 would create four partitions, as shown below.</span></span>

![Particionálás között](./media/service-fabric-concepts-partitioning/range-partitioning.png)

<span data-ttu-id="3fd61-196">Általános gyakorlatként javasolt toocreate hello adatkészlet belül egyedi kulcs alapján kivonatát.</span><span class="sxs-lookup"><span data-stu-id="3fd61-196">A common approach is toocreate a hash based on a unique key within hello data set.</span></span> <span data-ttu-id="3fd61-197">Néhány gyakori példán kulcsok lenne, a vehicle azonosító szám (VIN), az alkalmazott azonosítója vagy egy egyedi karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="3fd61-197">Some common examples of keys would be a vehicle identification number (VIN), an employee ID, or a unique string.</span></span> <span data-ttu-id="3fd61-198">Az egyedi kulcs használatával, hoz majd létre egy kivonatoló modulus hello kulcs tartomány, a toouse a kulcsként.</span><span class="sxs-lookup"><span data-stu-id="3fd61-198">By using this unique key, you would then generate a hash code, modulus hello key range, toouse as your key.</span></span> <span data-ttu-id="3fd61-199">Hello felső és alsó határainak hello megengedett kulcs tartományt is megadhat.</span><span class="sxs-lookup"><span data-stu-id="3fd61-199">You can specify hello upper and lower bounds of hello allowed key range.</span></span>

### <a name="select-a-hash-algorithm"></a><span data-ttu-id="3fd61-200">A kivonatoló algoritmus kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3fd61-200">Select a hash algorithm</span></span>
<span data-ttu-id="3fd61-201">A kivonatolás fontos része a kivonatoló algoritmus kijelölése.</span><span class="sxs-lookup"><span data-stu-id="3fd61-201">An important part of hashing is selecting your hash algorithm.</span></span> <span data-ttu-id="3fd61-202">Egy kell vizsgálni, hogy-e hello cél toogroup hasonló kulcsok egymást (helység bizalmas kivonatoláshoz) –, vagy ha körben tevékenységet kell terjeszteni összes partíciójára (terjesztési kivonatoláshoz), amely napjainkban egyre általánosabbá.</span><span class="sxs-lookup"><span data-stu-id="3fd61-202">A consideration is whether hello goal is toogroup similar keys near each other (locality sensitive hashing)--or if activity should be distributed broadly across all partitions (distribution hashing), which is more common.</span></span>

<span data-ttu-id="3fd61-203">hello jó terjesztési kivonatoló algoritmus mutatókat, hogy könnyen toocompute, van néhány ütközések, és egyenletesen terjesztett hello kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3fd61-203">hello characteristics of a good distribution hashing algorithm are that it is easy toocompute, it has few collisions, and it distributes hello keys evenly.</span></span> <span data-ttu-id="3fd61-204">Egy jó példa egy hatékony kivonatoló algoritmust, hogy hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) kivonatoló algoritmus.</span><span class="sxs-lookup"><span data-stu-id="3fd61-204">A good example of an efficient hash algorithm is hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash algorithm.</span></span>

<span data-ttu-id="3fd61-205">Általános kivonatoló kódot algoritmus lehetőségekért helyes erőforrás hello [Wikipedia oldalon, kivonatoló függvényeket](http://en.wikipedia.org/wiki/Hash_function).</span><span class="sxs-lookup"><span data-stu-id="3fd61-205">A good resource for general hash code algorithm choices is hello [Wikipedia page on hash functions](http://en.wikipedia.org/wiki/Hash_function).</span></span>

## <a name="build-a-stateful-service-with-multiple-partitions"></a><span data-ttu-id="3fd61-206">Több partíciót az állapotalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fd61-206">Build a stateful service with multiple partitions</span></span>
<span data-ttu-id="3fd61-207">Hozzuk létre az első megbízható állapotalapú szolgáltatás több partíciókat.</span><span class="sxs-lookup"><span data-stu-id="3fd61-207">Let's create your first reliable stateful service with multiple partitions.</span></span> <span data-ttu-id="3fd61-208">Ebben a példában fog létrehozni egy nagyon egyszerű alkalmazás toostore kívánt összes utolsó neve az azonos hello a levél hello egyazon partícióra kerüljenek.</span><span class="sxs-lookup"><span data-stu-id="3fd61-208">In this example, you will build a very simple application where you want toostore all last names that start with hello same letter in hello same partition.</span></span>

<span data-ttu-id="3fd61-209">Kód írása előtt kell toothink hello partíciókról és partíciós kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3fd61-209">Before you write any code, you need toothink about hello partitions and partition keys.</span></span> <span data-ttu-id="3fd61-210">Partíciókra van szüksége 26 (egy hello ábécé minden betű), de mi kapcsolatos alacsony és magas kulcsok hello?</span><span class="sxs-lookup"><span data-stu-id="3fd61-210">You need 26 partitions (one for each letter in hello alphabet), but what about hello low and high keys?</span></span>
<span data-ttu-id="3fd61-211">Szeretnénk szó toohave egy partíciót engedélyez betű, azt használatával 0 hello alsó kulcsa és 25 kulcsként hello magas, mert mindegyik betűnek a saját kulcs.</span><span class="sxs-lookup"><span data-stu-id="3fd61-211">As we literally want toohave one partition per letter, we can use 0 as hello low key and 25 as hello high key, as each letter is its own key.</span></span>

> [!NOTE]
> <span data-ttu-id="3fd61-212">Ez egy webfarmos egyszerűsített, valójában hello terjesztési egyenetlen lenne.</span><span class="sxs-lookup"><span data-stu-id="3fd61-212">This is a simplified scenario, as in reality hello distribution would be uneven.</span></span> <span data-ttu-id="3fd61-213">Gyakori hello állók közül. "X" kezdő "S" vagy "M" hello betűket kezdve Vezetéknév vagy az "Y".</span><span class="sxs-lookup"><span data-stu-id="3fd61-213">Last names starting with hello letters "S" or "M" are more common than hello ones starting with "X" or "Y".</span></span>
> 
> 

1. <span data-ttu-id="3fd61-214">Nyissa meg **Visual Studio** > **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="3fd61-214">Open **Visual Studio** > **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="3fd61-215">A hello **új projekt** párbeszédpanelen válassza ki a Service Fabric-alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="3fd61-215">In hello **New Project** dialog box, choose hello Service Fabric application.</span></span>
3. <span data-ttu-id="3fd61-216">Hello projekt "AlphabetPartitions" hívása.</span><span class="sxs-lookup"><span data-stu-id="3fd61-216">Call hello project "AlphabetPartitions".</span></span>
4. <span data-ttu-id="3fd61-217">A hello **szolgáltatás létrehozása** párbeszédpanelen válassza ki **állapotalapú alkalmazások és szolgáltatások** szolgáltatást, és az "Alphabet.Processing" hívás a hello az alábbi képen látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-217">In hello **Create a Service** dialog box, choose **Stateful** service and call it "Alphabet.Processing" as shown in hello image below.</span></span>
       <span data-ttu-id="3fd61-218">![Visual Studio új szolgáltatás párbeszédpanelje][1]</span><span class="sxs-lookup"><span data-stu-id="3fd61-218">![New service dialog in Visual Studio][1]</span></span>

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. <span data-ttu-id="3fd61-219">A partíciók számának hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="3fd61-219">Set hello number of partitions.</span></span> <span data-ttu-id="3fd61-220">Nyissa meg hello Applicationmanifest.xml fájl található hello ApplicationPackageRoot mappa hello AlphabetPartitions projektet és a frissítés hello paraméter Processing_PartitionCount too26 alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-220">Open hello Applicationmanifest.xml file located in hello ApplicationPackageRoot folder of hello AlphabetPartitions project and update hello parameter Processing_PartitionCount too26 as shown below.</span></span>
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    <span data-ttu-id="3fd61-221">Szükség tooupdate hello LowKey és HighKey tulajdonságok hello StatefulService elemének hello ApplicationManifest.xml alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-221">You also need tooupdate hello LowKey and HighKey properties of hello StatefulService element in hello ApplicationManifest.xml as shown below.</span></span>
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. <span data-ttu-id="3fd61-222">Hello szolgáltatás toobe érhető el nyissa meg egy portot a végpont mentése hello végpontelem ServiceManifest.xml (hello PackageRoot mappában található), az alább látható módon Alphabet.Processing szolgáltatás hello hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="3fd61-222">For hello service toobe accessible, open up an endpoint on a port by adding hello endpoint element of ServiceManifest.xml (located in hello PackageRoot folder) for hello Alphabet.Processing service as shown below:</span></span>
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    <span data-ttu-id="3fd61-223">Hello szolgáltatás most konfigurált toolisten tooan belső végpont 26 partíciókkal.</span><span class="sxs-lookup"><span data-stu-id="3fd61-223">Now hello service is configured toolisten tooan internal endpoint with 26 partitions.</span></span>
7. <span data-ttu-id="3fd61-224">A következő lépésben toooverride hello `CreateServiceReplicaListeners()` hello feldolgozási osztály.</span><span class="sxs-lookup"><span data-stu-id="3fd61-224">Next, you need toooverride hello `CreateServiceReplicaListeners()` method of hello Processing class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3fd61-225">Ebben a példában feltételezzük, hogy egy egyszerű HttpCommunicationListener használunk.</span><span class="sxs-lookup"><span data-stu-id="3fd61-225">For this sample, we assume that you are using a simple HttpCommunicationListener.</span></span> <span data-ttu-id="3fd61-226">A megbízható szolgáltatás kommunikációja további információkért lásd: [hello megbízható kommunikáció modell](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="3fd61-226">For more information on reliable service communication, see [hello Reliable Service communication model](service-fabric-reliable-services-communication.md).</span></span>
   > 
   > 
8. <span data-ttu-id="3fd61-227">Az ajánlott mintázatát hello URL-címet, amely figyeli a replika hello a következő formátumban: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span><span class="sxs-lookup"><span data-stu-id="3fd61-227">A recommended pattern for hello URL that a replica listens on is hello following format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span></span>
    <span data-ttu-id="3fd61-228">Ezért érdemes tooconfigure a kommunikációs figyelő toolisten hello megfelelő végpontok, hogy az ebben a mintában.</span><span class="sxs-lookup"><span data-stu-id="3fd61-228">So you want tooconfigure your communication listener toolisten on hello correct endpoints and with this pattern.</span></span>
   
    <span data-ttu-id="3fd61-229">Előfordulhat, hogy a szolgáltatás több replika futó hello ugyanazon a számítógépen, ezért ezt a címet kell toobe egyedi toohello replika.</span><span class="sxs-lookup"><span data-stu-id="3fd61-229">Multiple replicas of this service may be hosted on hello same computer, so this address needs toobe unique toohello replica.</span></span> <span data-ttu-id="3fd61-230">Ezért Partícióazonosító + másodpéldány-azonosító van hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="3fd61-230">This is why   partition ID + replica ID are in hello URL.</span></span> <span data-ttu-id="3fd61-231">HttpListener figyelheti az azonos port mindaddig, amíg hello URL-előtagját egyedi hello több címen.</span><span class="sxs-lookup"><span data-stu-id="3fd61-231">HttpListener can listen on multiple addresses on hello same port as long as hello URL prefix    is unique.</span></span>
   
    <span data-ttu-id="3fd61-232">hello extra GUID van-e egy speciális eset, amelyen másodlagos replika is a kéréseket kell figyelnie csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="3fd61-232">hello extra GUID is there for an advanced case where secondary replicas also listen for read-only requests.</span></span> <span data-ttu-id="3fd61-233">Amikor hello esetben szüksége arra, hogy egy új egyedi címet használja, amikor elsődleges toosecondary tooforce ügyfelek toore feloldása hello cím átállás toomake.</span><span class="sxs-lookup"><span data-stu-id="3fd61-233">When that's hello case, you want toomake sure that a new unique address is used when transitioning from primary toosecondary tooforce clients toore-resolve hello address.</span></span> <span data-ttu-id="3fd61-234">"+ a" hello címet, hogy hello replika figyeli az összes elérhető gazdagépet (IP, FQDM, localhost, stb.) hello kódot mutat egy példát használatos.</span><span class="sxs-lookup"><span data-stu-id="3fd61-234">'+' is used as hello address here so that hello replica listens on all available hosts (IP, FQDM, localhost, etc.) hello code below shows an example.</span></span>
   
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
   
    <span data-ttu-id="3fd61-235">Célszerű is érdemes megjegyezni, hogy hello közzétett URL-címe eltér némileg hello figyelő URL-előtagját.</span><span class="sxs-lookup"><span data-stu-id="3fd61-235">It's also worth noting that hello published URL is slightly different from hello listening URL prefix.</span></span>
    <span data-ttu-id="3fd61-236">tooHttpListener hello figyelő URL-címet kap.</span><span class="sxs-lookup"><span data-stu-id="3fd61-236">hello listening URL is given tooHttpListener.</span></span> <span data-ttu-id="3fd61-237">közzétett URL-címe: hello URL-címet, amely közzétett toohello Service Fabric-szolgáltatás, a szolgáltatásészlelés használt hello.</span><span class="sxs-lookup"><span data-stu-id="3fd61-237">hello published URL is hello URL that is published toohello Service Fabric Naming Service, which is used for service discovery.</span></span> <span data-ttu-id="3fd61-238">Az ügyfelek ekkor megkérdezi, ehhez a címhez, a felderítés szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="3fd61-238">Clients will ask for this address through that discovery service.</span></span> <span data-ttu-id="3fd61-239">hello címét, hogy az ügyfelek igényeinek toohave hello tényleges IP vagy FQDN-jét hello csomópont beolvasni a rendelés tooconnect.</span><span class="sxs-lookup"><span data-stu-id="3fd61-239">hello address that clients get needs toohave hello actual IP or FQDN of hello node in order tooconnect.</span></span> <span data-ttu-id="3fd61-240">Ezért meg kell tooreplace "+" rendelkező hello csomópont IP-cím vagy FQDN látható a fenti.</span><span class="sxs-lookup"><span data-stu-id="3fd61-240">So you need tooreplace '+' with hello node's IP or FQDN as shown above.</span></span>
9. <span data-ttu-id="3fd61-241">hello utolsó lépése tooadd hello feldolgozási logika toohello szolgáltatás alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-241">hello last step is tooadd hello processing logic toohello service as shown below.</span></span>
   
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
   
    <span data-ttu-id="3fd61-242">`ProcessInternalRequest`olvasási hello hello lekérdezési karakterlánc paraméter használt toocall hello partíció és hívások értékének `AddUserAsync` tooadd hello Vezetéknév toohello megbízható szótár `dictionary`.</span><span class="sxs-lookup"><span data-stu-id="3fd61-242">`ProcessInternalRequest` reads hello values of hello query string parameter used toocall hello partition and calls `AddUserAsync` tooadd hello lastname toohello reliable dictionary `dictionary`.</span></span>
10. <span data-ttu-id="3fd61-243">Adjuk hozzá egy állapotmentes szolgáltatások toohello projekt toosee hogyan hívása egy adott partíció.</span><span class="sxs-lookup"><span data-stu-id="3fd61-243">Let's add a stateless service toohello project toosee how you can call a particular partition.</span></span>
    
    <span data-ttu-id="3fd61-244">Ez a szolgáltatás egyszerű webes felületet, amely elfogadja a lekérdezési karakterlánc paraméterként hello Vezetéknév, hello partíciós kulcs határozza meg, és elküldi toohello Alphabet.Processing szolgáltatás feldolgozásra funkcionál.</span><span class="sxs-lookup"><span data-stu-id="3fd61-244">This service serves as a simple web interface that accepts hello lastname as a query string parameter, determines hello partition key, and sends it toohello Alphabet.Processing service for processing.</span></span>
11. <span data-ttu-id="3fd61-245">A hello **szolgáltatás létrehozása** párbeszédpanelen válassza ki **Stateless** szolgáltatás, és hívja meg az "Alphabet.Web" alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-245">In hello **Create a Service** dialog box, choose **Stateless** service and call it "Alphabet.Web" as shown below.</span></span>
    
    ![Állapotmentes szolgáltatások képernyőképe](./media/service-fabric-concepts-partitioning/createnewstateless.png)<span data-ttu-id="3fd61-247">.</span><span class="sxs-lookup"><span data-stu-id="3fd61-247">.</span></span>
12. <span data-ttu-id="3fd61-248">Hello végpont-információ frissítésére hello ServiceManifest.xml a hello Alphabet.WebApi szolgáltatás tooopen be a portot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="3fd61-248">Update hello endpoint information in hello ServiceManifest.xml of hello Alphabet.WebApi service tooopen up a port as shown below.</span></span>
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. <span data-ttu-id="3fd61-249">Tooreturn hello osztályban webes ServiceInstanceListeners gyűjteménye van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3fd61-249">You need tooreturn a collection of ServiceInstanceListeners in hello class Web.</span></span> <span data-ttu-id="3fd61-250">Ebben az esetben választható tooimplement egy egyszerű HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="3fd61-250">Again, you can choose tooimplement a simple HttpCommunicationListener.</span></span>
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is hello node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. <span data-ttu-id="3fd61-251">Most tooimplement hello feldolgozó logika van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3fd61-251">Now you need tooimplement hello processing logic.</span></span> <span data-ttu-id="3fd61-252">hello HttpCommunicationListener hívások `ProcessInputRequest` Ha kérelem érkezik.</span><span class="sxs-lookup"><span data-stu-id="3fd61-252">hello HttpCommunicationListener calls `ProcessInputRequest` when a request comes in.</span></span> <span data-ttu-id="3fd61-253">Ezért lépjen tovább, és adja hozzá az alábbi hello kódot.</span><span class="sxs-lookup"><span data-stu-id="3fd61-253">So let's go ahead and add hello code below.</span></span>
    
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
                    "Result: {0}. <p>Partition key: '{1}' generated from hello first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
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
                output = output + "added tooPartition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    <span data-ttu-id="3fd61-254">Rajta lépésről lépésre bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="3fd61-254">Let's walk through it step by step.</span></span> <span data-ttu-id="3fd61-255">hello kód beolvassa hello lekérdezési karakterlánc paraméter első betűjének hello `lastname` történő karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="3fd61-255">hello code reads hello first letter of hello query string parameter `lastname` into a char.</span></span> <span data-ttu-id="3fd61-256">Majd, meghatározza, hogy ez a levél hello partíciókulcs hexadecimális értékét hello kivonásával `A` hello hexadecimális értékét hello Vezetéknév első betűjét.</span><span class="sxs-lookup"><span data-stu-id="3fd61-256">Then, it determines hello partition key for this letter by subtracting hello hexadecimal value of `A` from hello hexadecimal value of hello last names' first letter.</span></span>
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    <span data-ttu-id="3fd61-257">Ne feledje, hogy a jelen példában használjuk 26 partíciók partíciónként több partíciós kulccsal.</span><span class="sxs-lookup"><span data-stu-id="3fd61-257">Remember, for this example, we are using 26 partitions with one partition key per partition.</span></span>
    <span data-ttu-id="3fd61-258">A következő azt beszerzése hello szolgáltatás partíció `partition` hello segítségével a kulcs `ResolveAsync` hello metódusa `servicePartitionResolver` objektum.</span><span class="sxs-lookup"><span data-stu-id="3fd61-258">Next, we obtain hello service partition `partition` for this key by using hello `ResolveAsync` method on hello `servicePartitionResolver` object.</span></span> <span data-ttu-id="3fd61-259">`servicePartitionResolver`típusúként van definiálva</span><span class="sxs-lookup"><span data-stu-id="3fd61-259">`servicePartitionResolver` is defined as</span></span>
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    <span data-ttu-id="3fd61-260">Hello `ResolveAsync` metódust vesz hello szolgáltatás URI-azonosítója, hello partíciókulcs és paraméterekként cancellation jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="3fd61-260">hello `ResolveAsync` method takes hello service URI, hello partition key, and a cancellation token as parameters.</span></span> <span data-ttu-id="3fd61-261">hello szolgáltatás URI-azonosítója a feldolgozási szolgáltatás hello `fabric:/AlphabetPartitions/Processing`.</span><span class="sxs-lookup"><span data-stu-id="3fd61-261">hello service URI for hello processing service is `fabric:/AlphabetPartitions/Processing`.</span></span> <span data-ttu-id="3fd61-262">A következő azt lekérése hello végpont hello partíció.</span><span class="sxs-lookup"><span data-stu-id="3fd61-262">Next, we get hello endpoint of hello partition.</span></span>
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    <span data-ttu-id="3fd61-263">Végül azt hello végpont URL-cím és hello lekérdezési karakterlánc felépítéséhez, és hívja service feldolgozása hello.</span><span class="sxs-lookup"><span data-stu-id="3fd61-263">Finally, we build hello endpoint URL plus hello querystring and call hello processing service.</span></span>
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    <span data-ttu-id="3fd61-264">Ha hello feldolgozási végzett, azt visszaírni hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="3fd61-264">Once hello processing is done, we write hello output back.</span></span>
15. <span data-ttu-id="3fd61-265">hello utolsó lépés egy tootest hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3fd61-265">hello last step is tootest hello service.</span></span> <span data-ttu-id="3fd61-266">A Visual Studio által használt alkalmazás paramétereket a helyi és a felhő üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="3fd61-266">Visual Studio uses application parameters for local and cloud deployment.</span></span> <span data-ttu-id="3fd61-267">tootest hello szolgáltatás helyileg 26 partíciókkal van szüksége tooupdate hello `Local.xml` fájl hello AlphabetPartitions projekt hello ApplicationParameters mappában a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="3fd61-267">tootest hello service with 26 partitions locally, you need tooupdate hello `Local.xml` file in hello ApplicationParameters folder of hello AlphabetPartitions project as shown below:</span></span>
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. <span data-ttu-id="3fd61-268">Miután befejezte a központi telepítés, ellenőrizheti a hello szolgáltatás és a Service Fabric Explorer hello a partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="3fd61-268">Once you finish deployment, you can check hello service and all of its partitions in hello Service Fabric Explorer.</span></span>
    
    ![Service Fabric Explorer képernyőképe](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. <span data-ttu-id="3fd61-270">A böngészőben, tesztelheti a particionálás logika megadásával hello `http://localhost:8081/?lastname=somename`.</span><span class="sxs-lookup"><span data-stu-id="3fd61-270">In a browser, you can test hello partitioning logic by entering `http://localhost:8081/?lastname=somename`.</span></span> <span data-ttu-id="3fd61-271">Látni fogja, hogy minden egyes utolsó, amely kezdetű névvel rendelkező azonos levél tárolása az hello hello egyazon partícióra kerüljenek.</span><span class="sxs-lookup"><span data-stu-id="3fd61-271">You will see that each last name that starts with hello same letter is being stored in hello same partition.</span></span>
    
    ![Böngésző képernyőképe](./media/service-fabric-concepts-partitioning/samplerunning.png)

<span data-ttu-id="3fd61-273">hello teljes forráskód hello minta nem érhető el a [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="3fd61-273">hello entire source code of hello sample is available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3fd61-274">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fd61-274">Next steps</span></span>
<span data-ttu-id="3fd61-275">A Service Fabric fogalmakat további információkért lásd: hello következő:</span><span class="sxs-lookup"><span data-stu-id="3fd61-275">For information on Service Fabric concepts, see hello following:</span></span>

* [<span data-ttu-id="3fd61-276">A Service Fabric-szolgáltatások rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="3fd61-276">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="3fd61-277">Méretezhetőséget biztosít a Service Fabric-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3fd61-277">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="3fd61-278">Kapacitástervezés a Service Fabric-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="3fd61-278">Capacity planning for Service Fabric applications</span></span>](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png