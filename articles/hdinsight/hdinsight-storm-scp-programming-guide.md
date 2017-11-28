---
title: "SCP.NET programozási útmutatója |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet SCP.NET használatával létrehozni. A NET-alapú Storm-topológiák a HDInsight alatt futó Storm használható."
services: hdinsight
documentationcenter: 
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: 3d76aebd2a1fd729c8e0639e6afcbde4c3fb752b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scp-programming-guide"></a><span data-ttu-id="3fc90-103">Szolgáltatáskapcsolódási pont programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="3fc90-103">SCP programming guide</span></span>
<span data-ttu-id="3fc90-104">Szolgáltatáskapcsolódási pont a egy platformja valós idejű, megbízható, következetes és magas teljesítmény adatfeldolgozási alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3fc90-104">SCP is a platform to build real time, reliable, consistent and high performance data processing application.</span></span> <span data-ttu-id="3fc90-105">Be van építve a [alatt futó Apache Storm](http://storm.incubator.apache.org/) – a streamfeldolgozási szerint a OSS Közösségek rendszer.</span><span class="sxs-lookup"><span data-stu-id="3fc90-105">It is built on top of [Apache Storm](http://storm.incubator.apache.org/) -- a stream processing system designed by the OSS communities.</span></span> <span data-ttu-id="3fc90-106">A Storm készült Nathan Marz, és nyissa meg által Twitter forrása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-106">Storm is designed by Nathan Marz and open sourced by Twitter.</span></span> <span data-ttu-id="3fc90-107">Ez a módszer a [Apache ZooKeeper](http://zookeeper.apache.org/), egy másik Apache projekt engedélyezése nagymértékben megbízható elosztott problémakoordinálás és állapotát.</span><span class="sxs-lookup"><span data-stu-id="3fc90-107">It leverages [Apache ZooKeeper](http://zookeeper.apache.org/), another Apache project to enable highly reliable distributed coordination and state management.</span></span> 

<span data-ttu-id="3fc90-108">Nem csak a szolgáltatáskapcsolódási pont projekt legelterjedtebb Windows alatt futó Storm, hanem szintén hozzáadott a projekt a bővítmények és a Windows-ökoszisztéma testreszabása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-108">Not only the SCP project ported Storm on Windows but also the project added extensions and customization for the Windows ecosystem.</span></span> <span data-ttu-id="3fc90-109">A bővítmények közé tartozik a .NET-fejlesztők számára, és a szalagtárak, a Testreszabás tartalmazza a Windows-alapú telepítést.</span><span class="sxs-lookup"><span data-stu-id="3fc90-109">The extensions include .NET developer experience, and libraries, the customization includes Windows-based deployment.</span></span> 

<span data-ttu-id="3fc90-110">A bővítmény és a Testreszabás úgy, hogy azt nem kell oszthatja ketté a OSS-projektek, és azt sikerült kihasználhatják a Storm épülő származtatott ökoszisztéma történik.</span><span class="sxs-lookup"><span data-stu-id="3fc90-110">The extension and customization is done in such a way that we do not need to fork the OSS projects and we could leverage derived ecosystems built on top of Storm.</span></span>

## <a name="processing-model"></a><span data-ttu-id="3fc90-111">Folyamatmodell</span><span class="sxs-lookup"><span data-stu-id="3fc90-111">Processing model</span></span>
<span data-ttu-id="3fc90-112">A szolgáltatáskapcsolódási pont az adatok folyamatos listának van modellezve.</span><span class="sxs-lookup"><span data-stu-id="3fc90-112">The data in SCP is modeled as continuous streams of tuples.</span></span> <span data-ttu-id="3fc90-113">Általában a rekordokat egymást követő néhány várólistán először, majd fel, és át legyenek-e a Storm-topológia belül található, végül a kimeneti másik SCP rendszerbe rekordokat, sikertelen lehet adatcsatornán, vagy tárolja, például elosztott fájlrendszerhez vagy például az SQL Server adatbázisok véglegesíteni üzleti logika.</span><span class="sxs-lookup"><span data-stu-id="3fc90-113">Typically the tuples flow into some queue first, then picked up, and transformed by business logic hosted inside a Storm topology, finally the output could be piped as tuples to another SCP system, or be committed to stores like distributed file system or databases like SQL Server.</span></span>

![Adatok feldolgozása, amely adattárat-adatcsatornákat takarmányozására várólista ábrázoló diagram](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

<span data-ttu-id="3fc90-115">Egy alkalmazás topológia a Storm, egy számítási grafikont határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3fc90-115">In Storm, an application topology defines a graph of computation.</span></span> <span data-ttu-id="3fc90-116">Minden csomópont-topológiában feldolgozási logikát tartalmaz, és csomópontok közötti kapcsolatok jelölése.</span><span class="sxs-lookup"><span data-stu-id="3fc90-116">Each node in a topology contains processing logic, and links between nodes indicate data flow.</span></span> <span data-ttu-id="3fc90-117">A bemeneti adatok behelyezése a topológia csomópontok nevezzük spoutokkal kapcsolatban, amely előkészítése az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-117">The nodes to inject input data into the topology are called Spouts, which can be used to sequence the data.</span></span> <span data-ttu-id="3fc90-118">A bemeneti adatok volt várakozó fájl naplókat, a tranzakciós adatbázis, a rendszer teljesítményszámláló stb. A csomópont mindkét bemeneti és kimeneti adatfolyamok Boltokhoz, amelyek a tényleges adatok szűrése és beállításokat és összesítési beállítások nevezzük.</span><span class="sxs-lookup"><span data-stu-id="3fc90-118">The input data could reside in file logs, transactional database, system performance counter etc. The nodes with both input and output data flows are called Bolts, which do the actual data filtering and selections and aggregation.</span></span>

<span data-ttu-id="3fc90-119">SCP-JE támogatja az ajánlott azon törekvéseit, a következő legalább egyszeri és pontosan-egyszer adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-119">SCP supports best efforts, at-least-once and exactly-once data processing.</span></span> <span data-ttu-id="3fc90-120">Az elosztott adatfolyam feldolgozása alkalmazásokban több hiba fordulhat elő, során az adatfeldolgozás, például a hálózati kimaradás, a gép hibájának vagy a felhasználói kód hiba stb. Következő legalább egyszeri feldolgozási biztosítja, hogy minden adat legalább egyszer általi feldolgozásának automatikusan ugyanazokat az adatokat visszajátszását, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="3fc90-120">In a distributed streaming processing application, various errors may happen during data processing, such as network outage, machine failure, or user code error etc. At-least-once processing ensures all data will be processed at least once by replaying automatically the same data when error happens.</span></span> <span data-ttu-id="3fc90-121">A legalább egyszeri feldolgozási egyszerű és megbízható, és számos alkalmazásban is megfelel.</span><span class="sxs-lookup"><span data-stu-id="3fc90-121">At-least-once processing is simple and reliable and suits well in many applications.</span></span> <span data-ttu-id="3fc90-122">Azonban pontos leltár szükséges az alkalmazás számára, például: legalább egyszeri feldolgozási esetén elegendő óta ugyanazokat az adatokat potenciálisan szerepeltek a alkalmazás topológia.</span><span class="sxs-lookup"><span data-stu-id="3fc90-122">However, when the application requires exact counting, for example, at-least-once processing is insufficient since the same data could potentially be played in the application topology.</span></span> <span data-ttu-id="3fc90-123">Ebben az esetben, pontosan-után feldolgozási arra tervezték, hogy ellenőrizze, hogy az eredmény helyességét még ha az adatokat a rendszer játssza és feldolgozása történhet többször.</span><span class="sxs-lookup"><span data-stu-id="3fc90-123">In that case, exactly-once processing is designed to make sure the result is correct even when the data may be replayed and processed multiple times.</span></span>

<span data-ttu-id="3fc90-124">SCP-je lehetővé teszi, hogy a valós idejű adatok folyamat alkalmazások fejlesztéséhez során kihasználhatja a Java virtuális gép (JVM)-alapú Storm alatt a .NET-fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="3fc90-124">SCP enables .NET developers to develop real time data process applications while leverage the Java Virtual Machine (JVM) based Storm under the cover.</span></span> <span data-ttu-id="3fc90-125">A .NET és a JVM TCP helyi szoftvercsatorna keresztül kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="3fc90-125">The .NET and JVM communicate via TCP local socket.</span></span> <span data-ttu-id="3fc90-126">Alapvetően minden Spout vagy Bolt .net/Java folyamat két, a felhasználó programot futtató .net folyamatban, a beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="3fc90-126">Basically each Spout/Bolt is a .Net/Java process pair, where the user logic runs in .Net process as a plugin.</span></span>

<span data-ttu-id="3fc90-127">Hozható létre olyan adatfeldolgozási alkalmazás SCP fölött, több lépésre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3fc90-127">To build a data processing application on top of SCP, several steps are needed:</span></span>

* <span data-ttu-id="3fc90-128">Tervezése és megvalósítása a spoutokkal kapcsolatban való várólistából adatok lekérésére.</span><span class="sxs-lookup"><span data-stu-id="3fc90-128">Design and implement the Spouts to pull in data from queue.</span></span>
* <span data-ttu-id="3fc90-129">Tervezési valósítja meg a bemeneti adatok feldolgozására szögek és adatok mentése külső áruházak, például az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3fc90-129">Design and implement Bolts to process the input data, and save data to external stores such as Database.</span></span>
* <span data-ttu-id="3fc90-130">A topológia megtervezésére, majd küldje el, és futtassa a topológia.</span><span class="sxs-lookup"><span data-stu-id="3fc90-130">Design the topology, then submit and run the topology.</span></span> <span data-ttu-id="3fc90-131">A topológia meghatározása csomópontok és az adatokat a csomópontok közötti forgalom.</span><span class="sxs-lookup"><span data-stu-id="3fc90-131">The topology defines vertexes and the data flows between the vertexes.</span></span> <span data-ttu-id="3fc90-132">Szolgáltatáskapcsolódási pont a topológia meghatározása fogad, majd azt egy Storm-fürt, minden egyes csúcspont futtató egy logikai csomóponton telepítette.</span><span class="sxs-lookup"><span data-stu-id="3fc90-132">SCP will take the topology specification and deploy it on a Storm cluster, where each vertex runs on one logical node.</span></span> <span data-ttu-id="3fc90-133">A feladatátvétel és a Storm Feladatütemező kell venni megvagyunk méretezési lesz.</span><span class="sxs-lookup"><span data-stu-id="3fc90-133">The failover and scaling will be taken care of by the Storm task scheduler.</span></span>

<span data-ttu-id="3fc90-134">Ez a dokumentum egyszerű példák segítségével ismerteti azon, hogyan hozhat létre a szolgáltatáskapcsolódási pont az adatfeldolgozás alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3fc90-134">This document will use some simple examples to walk through how to build data processing application with SCP.</span></span>

## <a name="scp-plugin-interface"></a><span data-ttu-id="3fc90-135">Szolgáltatáskapcsolódási pont beépülő modul felület</span><span class="sxs-lookup"><span data-stu-id="3fc90-135">SCP Plugin Interface</span></span>
<span data-ttu-id="3fc90-136">Szolgáltatáskapcsolódási pont beépülő modulok (vagy alkalmazások) olyan önálló exe is futtatható belül a Visual Studio fejlesztői fázis során, és a Storm-feldolgozási folyamat alkalmazást éles környezetben kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="3fc90-136">SCP plugins (or applications) are standalone EXEs that can both run inside Visual Studio during the development phase, and be plugged into the Storm pipeline after deployment in production.</span></span> <span data-ttu-id="3fc90-137">A szolgáltatáskapcsolódási pont beépülő modul írása ugyanúgy, mint korábban mint bármely más szabványos Windows konzol biztosító alkalmazások írására van.</span><span class="sxs-lookup"><span data-stu-id="3fc90-137">Writing the SCP plugin is just the same as writing any other standard Windows console applications.</span></span> <span data-ttu-id="3fc90-138">SCP.NET platform deklarál spout vagy bolt néhány felületet, és a beépülő modul a programkód inkább a konfigurációkezelővel.</span><span class="sxs-lookup"><span data-stu-id="3fc90-138">SCP.NET platform declares some interface for spout/bolt, and the user plugin code should implement these interfaces.</span></span> <span data-ttu-id="3fc90-139">A fő a tervezési célja, hogy a felhasználó összpontosíthatnak, a saját üzleti logics és egyebek SCP.NET platform által kezelt hagyja.</span><span class="sxs-lookup"><span data-stu-id="3fc90-139">The main purpose of this design is that the user can focus on their own business logics, and leaving other things to be handled by SCP.NET platform.</span></span>

<span data-ttu-id="3fc90-140">A beépülő modul a programkód meg kell valósítania a következőket felületek egyikét, attól függ, hogy a topológia tranzakciós vagy nem tranzakciós-e, és hogy az összetevő spout vagy bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-140">The user plugin code should implement one of the followings interfaces, depends on whether the topology is transactional or non-transactional, and whether the component is spout or bolt.</span></span>

* <span data-ttu-id="3fc90-141">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="3fc90-141">ISCPSpout</span></span>
* <span data-ttu-id="3fc90-142">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="3fc90-142">ISCPBolt</span></span>
* <span data-ttu-id="3fc90-143">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="3fc90-143">ISCPTxSpout</span></span>
* <span data-ttu-id="3fc90-144">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="3fc90-144">ISCPBatchBolt</span></span>

### <a name="iscpplugin"></a><span data-ttu-id="3fc90-145">ISCPPlugin</span><span class="sxs-lookup"><span data-stu-id="3fc90-145">ISCPPlugin</span></span>
<span data-ttu-id="3fc90-146">ISCPPlugin a közös felület különféle beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="3fc90-146">ISCPPlugin is the common interface for all kinds of plugins.</span></span> <span data-ttu-id="3fc90-147">Azt jelenleg egy üres felületet.</span><span class="sxs-lookup"><span data-stu-id="3fc90-147">Currently, it is a dummy interface.</span></span>

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a><span data-ttu-id="3fc90-148">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="3fc90-148">ISCPSpout</span></span>
<span data-ttu-id="3fc90-149">ISCPSpout rendszer a nem tranzakciós spout,</span><span class="sxs-lookup"><span data-stu-id="3fc90-149">ISCPSpout is the interface for non-transactional spout.</span></span>

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

<span data-ttu-id="3fc90-150">Ha `NextTuple()` neve, a C\# felhasználói kód el tudná küldeni egy vagy több rekordokat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-150">When `NextTuple()` is called, the C\# user code can emit one or more tuples.</span></span> <span data-ttu-id="3fc90-151">Nincs mit hozható létre, ha ez a módszer kibocsátó semmit nem kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="3fc90-151">If there is nothing to emit, this method should return without emitting anything.</span></span> <span data-ttu-id="3fc90-152">Fontos megjegyezni, hogy `NextTuple()`, `Ack()`, és `Fail()` összes nevezzük egy egyetlen szálon c. szoros ismétlődő\# folyamat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-152">It should be noted that `NextTuple()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="3fc90-153">Ha nem hozható létre rekordokat, udvarias NextTuple alvó kaphatnak a rövid időn (például 10 ezredmásodperc), hogy ne hulladék túl sok CPU.</span><span class="sxs-lookup"><span data-stu-id="3fc90-153">When there are no tuples to emit, it is courteous to have NextTuple sleep for a short amount of time (such as 10 milliseconds) so as not to waste too much CPU.</span></span>

<span data-ttu-id="3fc90-154">`Ack()`és `Fail()` hívja meg a rendszer csak akkor, ha a nyugtázási mechanizmus fájlmegadásában fájlban engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="3fc90-154">`Ack()` and `Fail()` will be called only when ack mechanism is enabled in spec file.</span></span> <span data-ttu-id="3fc90-155">A `seqId` alapján határozza meg a rekord, amely korrektúrák, vagy nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="3fc90-155">The `seqId` is used to identify the tuple which is acked or failed.</span></span> <span data-ttu-id="3fc90-156">Így ha nyugtázási nem tranzakciós topológia engedélyezve van, a következő kibocsátása függvény Spout kell használni:</span><span class="sxs-lookup"><span data-stu-id="3fc90-156">So if ack is enabled in non-transactional topology, the following emit function should be used in Spout:</span></span>

    public abstract void Emit(string streamId, List<object> values, long seqId); 

<span data-ttu-id="3fc90-157">Ha nyugtázási nem támogatott nem tranzakciós topológia a `Ack()` és `Fail()` maradhatnak, üres függvényében.</span><span class="sxs-lookup"><span data-stu-id="3fc90-157">If ack is not supported in non-transactional topology, the `Ack()` and `Fail()` can be left as empty function.</span></span>

<span data-ttu-id="3fc90-158">A `parms` ezeket a funkciókat a bemeneti paraméterek csak üres szótár, olyan fenntartja a jövőbeli használatra.</span><span class="sxs-lookup"><span data-stu-id="3fc90-158">The `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbolt"></a><span data-ttu-id="3fc90-159">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="3fc90-159">ISCPBolt</span></span>
<span data-ttu-id="3fc90-160">ISCPBolt rendszer a nem tranzakciós bolt,</span><span class="sxs-lookup"><span data-stu-id="3fc90-160">ISCPBolt is the interface for non-transactional bolt.</span></span>

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

<span data-ttu-id="3fc90-161">Ha új rekordot érhető el, a `Execute()` függvényt hívja feldolgozni azt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-161">When new tuple is available, the `Execute()` function will be called to process it.</span></span>

### <a name="iscptxspout"></a><span data-ttu-id="3fc90-162">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="3fc90-162">ISCPTxSpout</span></span>
<span data-ttu-id="3fc90-163">ISCPTxSpout a tranzakciós spout felületet.</span><span class="sxs-lookup"><span data-stu-id="3fc90-163">ISCPTxSpout is the interface for transactional spout.</span></span>

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

<span data-ttu-id="3fc90-164">Akárcsak a nem tranzakciós másik részét `NextTx()`, `Ack()`, és `Fail()` összes nevezzük egy egyetlen szálon c. szoros ismétlődő\# folyamat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-164">Just like their non-transactional counter-part, `NextTx()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="3fc90-165">Nem állnak rendelkezésre adatok kibocsátásához, esetén szeretné, hogy udvarias `NextTx` a rövid időn (10 ezredmásodperc), hogy ne túl sok CPU hulladék az alvó állapot.</span><span class="sxs-lookup"><span data-stu-id="3fc90-165">When there are no data to emit, it is courteous to have `NextTx` sleep for a short amount of time (10 milliseconds) so as not to waste too much CPU.</span></span>

<span data-ttu-id="3fc90-166">`NextTx()`új tranzakció, a kimeneti paramétert indításához nevezik `seqId` alapján határozza meg a tranzakció, amely is használva van `Ack()` és `Fail()`.</span><span class="sxs-lookup"><span data-stu-id="3fc90-166">`NextTx()` is called to start a new transaction, the out parameter `seqId` is used to identify the transaction, which is also used in `Ack()` and `Fail()`.</span></span> <span data-ttu-id="3fc90-167">A `NextTx()`, felhasználói el tudná küldeni az adatok Java oldalára.</span><span class="sxs-lookup"><span data-stu-id="3fc90-167">In `NextTx()`, user can emit data to Java side.</span></span> <span data-ttu-id="3fc90-168">Az adatok ismétlési támogatásához ZooKeeper tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="3fc90-168">The data will be stored in ZooKeeper to support replay.</span></span> <span data-ttu-id="3fc90-169">ZooKeeper kapacitása korlátozott, mert a felhasználó kell csak számú metaadat, a tranzakciós spout adatok tömeges sikertelen.</span><span class="sxs-lookup"><span data-stu-id="3fc90-169">Because the capacity of ZooKeeper is very limited, user should only emit metadata, not bulk data in transactional spout.</span></span>

<span data-ttu-id="3fc90-170">A Storm fog visszajátszásos automatikusan egy tranzakció, ha a hiba, így `Fail()` normál esetben nem szabad meghívni.</span><span class="sxs-lookup"><span data-stu-id="3fc90-170">Storm will replay a transaction automatically if it fails, so `Fail()` should not be called in normal case.</span></span> <span data-ttu-id="3fc90-171">De ha a szolgáltatáskapcsolódási pont ellenőrizheti a metaadatok tranzakciós spout által kibocsátott, akkor meghívhatja `Fail()` érvénytelen a metaadatok esetén.</span><span class="sxs-lookup"><span data-stu-id="3fc90-171">But if SCP can check the metadata emitted by transactional spout, it can call `Fail()` when the metadata is invalid.</span></span>

<span data-ttu-id="3fc90-172">A `parms` ezeket a funkciókat a bemeneti paraméterek csak üres szótár, olyan fenntartja a jövőbeli használatra.</span><span class="sxs-lookup"><span data-stu-id="3fc90-172">The `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbatchbolt"></a><span data-ttu-id="3fc90-173">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="3fc90-173">ISCPBatchBolt</span></span>
<span data-ttu-id="3fc90-174">ISCPBatchBolt a tranzakciós bolt felületet.</span><span class="sxs-lookup"><span data-stu-id="3fc90-174">ISCPBatchBolt is the interface for transactional bolt.</span></span>

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

<span data-ttu-id="3fc90-175">`Execute()`Ha új rekordot tartalmaz a bolt érkező neve.</span><span class="sxs-lookup"><span data-stu-id="3fc90-175">`Execute()` is called when there is new tuple arriving at the bolt.</span></span> <span data-ttu-id="3fc90-176">`FinishBatch()`Amikor befejeződik a tranzakció neve.</span><span class="sxs-lookup"><span data-stu-id="3fc90-176">`FinishBatch()` is called when this transaction is ended.</span></span> <span data-ttu-id="3fc90-177">A `parms` bemeneti paraméter későbbi használatra van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="3fc90-177">The `parms` input parameter is reserved for future use.</span></span>

<span data-ttu-id="3fc90-178">Tranzakciós topológia, egy fontos tényező – nincs `StormTxAttempt`.</span><span class="sxs-lookup"><span data-stu-id="3fc90-178">For transactional topology, there is an important concept – `StormTxAttempt`.</span></span> <span data-ttu-id="3fc90-179">Rendelkezik két mező `TxId` és `AttemptId`.</span><span class="sxs-lookup"><span data-stu-id="3fc90-179">It has two fields, `TxId` and `AttemptId`.</span></span> <span data-ttu-id="3fc90-180">`TxId`egy bizonyos tranzakció azonosítására szolgál a megadott tranzakció, előfordulhat, hogy több kísérlet a tranzakció sikertelen, és ha a rendszer játssza vissza.</span><span class="sxs-lookup"><span data-stu-id="3fc90-180">`TxId` is used to identify a specific transaction, and for a given transaction, there may be multiple attempt if the transaction fails and is replayed.</span></span> <span data-ttu-id="3fc90-181">SCP.NET új fog feldolgozni az egyes másik ISCPBatchBolt objektumot `StormTxAttempt`, csak a például milyen Storm tegye a Java oldalon.</span><span class="sxs-lookup"><span data-stu-id="3fc90-181">SCP.NET will new a different ISCPBatchBolt object to process each `StormTxAttempt`, just like what Storm do in Java side.</span></span> <span data-ttu-id="3fc90-182">Ez a kialakítás célja támogatja a párhuzamos tranzakciókat feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-182">The purpose of this design is to support parallel transactions processing.</span></span> <span data-ttu-id="3fc90-183">Felhasználói tartsa azt, hogy ha a tranzakció kísérlet befejeződött, a megfelelő ISCPBatchBolt objektum megsemmisülnek szem előtt tartva és szemétgyűjtő.</span><span class="sxs-lookup"><span data-stu-id="3fc90-183">User should keep it in mind that if transaction attempt is finished, the corresponding ISCPBatchBolt object will be destroyed and garbage collected.</span></span>

## <a name="object-model"></a><span data-ttu-id="3fc90-184">Hálózatiobjektum-modellje</span><span class="sxs-lookup"><span data-stu-id="3fc90-184">Object Model</span></span>
<span data-ttu-id="3fc90-185">SCP.NET is tartalmaz egy egyszerű objektumok a fejlesztők számára a program.</span><span class="sxs-lookup"><span data-stu-id="3fc90-185">SCP.NET also provides a simple set of key objects for developers to program with.</span></span> <span data-ttu-id="3fc90-186">Ezek **környezetben**, **Állapottárolója**, és **SCPRuntime**.</span><span class="sxs-lookup"><span data-stu-id="3fc90-186">They are **Context**, **StateStore**, and **SCPRuntime**.</span></span> <span data-ttu-id="3fc90-187">Azok a többi részében Ez a szakasz tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="3fc90-187">They will be discussed in the rest part of this section.</span></span>

### <a name="context"></a><span data-ttu-id="3fc90-188">Környezet</span><span class="sxs-lookup"><span data-stu-id="3fc90-188">Context</span></span>
<span data-ttu-id="3fc90-189">Az alkalmazás futó környezetet biztosít a környezetben.</span><span class="sxs-lookup"><span data-stu-id="3fc90-189">Context provides a running environment to the application.</span></span> <span data-ttu-id="3fc90-190">Minden egyes ISCPPlugin példány (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) rendelkezik egy megfelelő adatkörnyezet példányához.</span><span class="sxs-lookup"><span data-stu-id="3fc90-190">Each ISCPPlugin instance (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) has a corresponding Context instance.</span></span> <span data-ttu-id="3fc90-191">A környezet által biztosított funkciókat lehet osztani két részből áll: (1) a statikus részét, amelyik elérhető a teljes c\# feldolgozni, (2) a dinamikus részben, amely csak a helyi példány.</span><span class="sxs-lookup"><span data-stu-id="3fc90-191">The functionality provided by Context can be divided into two parts: (1) the static part which is available in the whole C\# process, (2) the dynamic part which is only available for the specific Context instance.</span></span>

### <a name="static-part"></a><span data-ttu-id="3fc90-192">Statikus részében szerepel</span><span class="sxs-lookup"><span data-stu-id="3fc90-192">Static Part</span></span>
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

<span data-ttu-id="3fc90-193">`Logger`napló célra valósul meg.</span><span class="sxs-lookup"><span data-stu-id="3fc90-193">`Logger` is provided for log purpose.</span></span>

<span data-ttu-id="3fc90-194">`pluginType`jelzi a beépülő modul típusa a C\# folyamat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-194">`pluginType` is used to indicate the plugin type of the C\# process.</span></span> <span data-ttu-id="3fc90-195">Ha a C\# folyamat (nélkül Java) tesztcélú helyi módban fut, a beépülő modul típusa `SCP_NET_LOCAL`.</span><span class="sxs-lookup"><span data-stu-id="3fc90-195">If the C\# process is run in local test mode (without Java), the plugin type is `SCP_NET_LOCAL`.</span></span>

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

<span data-ttu-id="3fc90-196">`Config`konfigurációs paraméterek lekérése Java ügyféloldali valósul meg.</span><span class="sxs-lookup"><span data-stu-id="3fc90-196">`Config` is provided to get configuration parameters from Java side.</span></span> <span data-ttu-id="3fc90-197">A paraméterek átadása Java oldaláról amikor C\# beépülő modul inicializálása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-197">The parameters are passed from Java side when C\# plugin is initialized.</span></span> <span data-ttu-id="3fc90-198">A `Config` paraméterek oszthatók két részből áll: `stormConf` és `pluginConf`.</span><span class="sxs-lookup"><span data-stu-id="3fc90-198">The `Config` parameters are divided into two parts: `stormConf` and `pluginConf`.</span></span>

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

<span data-ttu-id="3fc90-199">`stormConf`a Storm által definiált paraméterek és `pluginConf` a szolgáltatáskapcsolódási pont által megadott paramétereket.</span><span class="sxs-lookup"><span data-stu-id="3fc90-199">`stormConf` is parameters defined by Storm and `pluginConf` is the parameters defined by SCP.</span></span> <span data-ttu-id="3fc90-200">Példa:</span><span class="sxs-lookup"><span data-stu-id="3fc90-200">For example:</span></span>

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

<span data-ttu-id="3fc90-201">`TopologyContext`megadott ahhoz, hogy a topológia a környezetben, esetén a leghasznosabb összetevők több párhuzamosság együtt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-201">`TopologyContext` is provided to get the topology context, it is most useful for components with multiple parallelism.</span></span> <span data-ttu-id="3fc90-202">Például:</span><span class="sxs-lookup"><span data-stu-id="3fc90-202">Here is an example:</span></span>

    //demo how to get TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a><span data-ttu-id="3fc90-203">Dinamikus részében szerepel</span><span class="sxs-lookup"><span data-stu-id="3fc90-203">Dynamic Part</span></span>
<span data-ttu-id="3fc90-204">A következő kapcsolódási pontok bizonyos környezetben példányra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="3fc90-204">The following interfaces are pertinent to a certain Context instance.</span></span> <span data-ttu-id="3fc90-205">A helyi példány SCP.NET platform által létrehozott, a felhasználói kód átadott:</span><span class="sxs-lookup"><span data-stu-id="3fc90-205">The Context instance is created by SCP.NET platform and passed to the user code:</span></span>

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

<span data-ttu-id="3fc90-206">Nem tranzakciós spout nyugtázási támogatása a következő metódus biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3fc90-206">For non-transactional spout supporting ack, the following method is provided:</span></span>

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

<span data-ttu-id="3fc90-207">A nem tranzakciós bolt nyugtázási támogató, legyen, vagy ha kifejezetten `Ack()` vagy `Fail()` kapott rekordban.</span><span class="sxs-lookup"><span data-stu-id="3fc90-207">For non-transactional bolt supporting ack, it should explicitly `Ack()` or `Fail()` the tuple it received.</span></span> <span data-ttu-id="3fc90-208">És kibocsátó új rekordot, amikor meg kell adnia az új rekord horgonyok.</span><span class="sxs-lookup"><span data-stu-id="3fc90-208">And when emitting new tuple, it must also specify the anchors of the new tuple.</span></span> <span data-ttu-id="3fc90-209">A következő módszerek állnak rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="3fc90-209">The following methods are provided.</span></span>

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a><span data-ttu-id="3fc90-210">Állapottárolója</span><span class="sxs-lookup"><span data-stu-id="3fc90-210">StateStore</span></span>
<span data-ttu-id="3fc90-211">`StateStore`metaadatok szolgáltatások, a monoton sorrend létrehozása és a várakozási szabad koordinációs biztosít.</span><span class="sxs-lookup"><span data-stu-id="3fc90-211">`StateStore` provides metadata services, monotonic sequence generation, and wait-free coordination.</span></span> <span data-ttu-id="3fc90-212">Elosztott feldolgozási magasabb szintű absztrakciók építhetők `StateStore`, beleértve az elosztott zárolásokat, elosztott várólisták, korlátok és tranzakciós szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-212">Higher-level distributed concurrency abstractions can be built on `StateStore`, including distributed locks, distributed queues, barriers, and transaction services.</span></span>

<span data-ttu-id="3fc90-213">Szolgáltatáskapcsolódási pont alkalmazások is használhatnak a `State` objektum egyes információk ZooKeeper, különösen a tranzakciós topológia megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="3fc90-213">SCP applications may use the `State` object to persist some information in ZooKeeper, especially for transactional topology.</span></span> <span data-ttu-id="3fc90-214">Ennek során, ha a tranzakciós spout összeomlik, és indítsa újra, a szükséges adatok lekérését ZooKeeper, és indítsa újra a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="3fc90-214">Doing so, if transactional spout crashes and restart, it can retrieve the necessary information from ZooKeeper and restart the pipeline.</span></span>

<span data-ttu-id="3fc90-215">A `StateStore` főként objektumnak ezen módszerek:</span><span class="sxs-lookup"><span data-stu-id="3fc90-215">The `StateStore` object mainly has these methods:</span></span>

    /// <summary>
    /// Static method to retrieve a state store of the given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommited States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all the States in the StateStore
    /// </summary>
    /// <returns>All the States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all the committed states
    /// </summary>
    /// <returns>Registries contain the Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all the Aborted State in the StateStore
    /// </summary>
    /// <returns>Registries contain the Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of the State</typeparam>
    public State GetState(long stateId)

<span data-ttu-id="3fc90-216">A `State` főként objektumnak ezen módszerek:</span><span class="sxs-lookup"><span data-stu-id="3fc90-216">The `State` object mainly has these methods:</span></span>

    /// <summary>
    /// Set the status of the state object to commit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set the status of the state object to abort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under the give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get the attribute value associated with the given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

<span data-ttu-id="3fc90-217">Az a `Commit()` metódust, ha simpleMode értéke igaz, egyszerűen törli a ZooKeeper megfelelő ZNode.</span><span class="sxs-lookup"><span data-stu-id="3fc90-217">For the `Commit()` method, when simpleMode is set to true, it will simply delete the corresponding ZNode in ZooKeeper.</span></span> <span data-ttu-id="3fc90-218">Ellenkező esetben a művelet törli a jelenlegi ZNode, és egy új csomópont hozzáadása a LEKÖTÖTT a\_elérési ÚTJA.</span><span class="sxs-lookup"><span data-stu-id="3fc90-218">Otherwise, it will delete the current ZNode, and adding a new node in the COMMITTED\_PATH.</span></span>

### <a name="scpruntime"></a><span data-ttu-id="3fc90-219">SCPRuntime</span><span class="sxs-lookup"><span data-stu-id="3fc90-219">SCPRuntime</span></span>
<span data-ttu-id="3fc90-220">SCPRuntime az alábbi két módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="3fc90-220">SCPRuntime provides the following two methods.</span></span>

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

<span data-ttu-id="3fc90-221">`Initialize()`a szolgáltatáskapcsolódási pont futásidejű környezet inicializálása szolgál.</span><span class="sxs-lookup"><span data-stu-id="3fc90-221">`Initialize()` is used to initialize the SCP runtime environment.</span></span> <span data-ttu-id="3fc90-222">Ennél a módszernél a C\# folyamat a Java-oldalon, és lekérdezi konfigurációs paramétereket, és a topológia környezet csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="3fc90-222">In this method, the C\# process will connect to the Java side, and gets configuration parameters and topology context.</span></span>

<span data-ttu-id="3fc90-223">`LaunchPlugin()`az üzenet feldolgozása hurok indítsa szolgál.</span><span class="sxs-lookup"><span data-stu-id="3fc90-223">`LaunchPlugin()` is used to kick off the message processing loop.</span></span> <span data-ttu-id="3fc90-224">Ez a ciklus, a C a\# beépülő modul üzenetek űrlap Java ügyféloldali (beleértve a rekordokat és a vezérlő jeleket) kap, és majd feldolgozása az üzeneteket, lehet, hogy a kapcsolat metódus hívása adja meg a felhasználói kód.</span><span class="sxs-lookup"><span data-stu-id="3fc90-224">In this loop, the C\# plugin will receive messages form Java side (including tuples and control signals), and then process the messages, perhaps calling the interface method provide by the user code.</span></span> <span data-ttu-id="3fc90-225">A bemeneti paraméter metódus `LaunchPlugin()` van olyan delegált esetén, amely ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt felületet megvalósító objektum adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="3fc90-225">The input parameter for method `LaunchPlugin()` is a delegate that can return an object that implement ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt interface.</span></span>

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

<span data-ttu-id="3fc90-226">A ISCPBatchBolt, beszerezheti a Microsoft `StormTxAttempt` a `parms`, és annak segítségével mennyire, hogy-e egy megismételt kísérlet után.</span><span class="sxs-lookup"><span data-stu-id="3fc90-226">For ISCPBatchBolt, we can get `StormTxAttempt` from `parms`, and use it to judge whether it is a replayed attempt.</span></span> <span data-ttu-id="3fc90-227">Ez általában a véglegesítési bolt szabályonkénti, és azt mutatják be a `HelloWorldTx` példa.</span><span class="sxs-lookup"><span data-stu-id="3fc90-227">This is usually done at the commit bolt, and it is demonstrated in the `HelloWorldTx` example.</span></span>

<span data-ttu-id="3fc90-228">A szolgáltatáskapcsolódási pont beépülő modulok általánosan fogalmazva, itt két módban futhat:</span><span class="sxs-lookup"><span data-stu-id="3fc90-228">Generally speaking, the SCP plugins may run in two modes here:</span></span>

1. <span data-ttu-id="3fc90-229">Helyi tesztelése mód: Ebben a módban, a szolgáltatáskapcsolódási pont beépülő modulok (a C\# felhasználói kód) Visual Studio belül a fejlesztési fázis során futtassa.</span><span class="sxs-lookup"><span data-stu-id="3fc90-229">Local Test Mode: In this mode, the SCP plugins (the C\# user code) run inside Visual Studio during the development phase.</span></span> <span data-ttu-id="3fc90-230">`LocalContext`Ebben a módban a helyi fájlok kibocsátott rekordokat szerializálni, és vissza memória a olvashatja őket módszert biztosít használható.</span><span class="sxs-lookup"><span data-stu-id="3fc90-230">`LocalContext` can be used in this mode, which provides method to serialize the emitted tuples to local files, and read them back to memory.</span></span>
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. <span data-ttu-id="3fc90-231">Normál mód: Ebben a módban a szolgáltatáskapcsolódási pont beépülő modulok indítja storm java folyamat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-231">Regular Mode: In this mode, the SCP plugins are launched by storm java process.</span></span>
   
    <span data-ttu-id="3fc90-232">Íme egy példa SCP beépülő modul megnyitása:</span><span class="sxs-lookup"><span data-stu-id="3fc90-232">Here is an example of launching SCP plugin:</span></span>
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting the environment variable here can change the log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a><span data-ttu-id="3fc90-233">Topológia nyelv</span><span class="sxs-lookup"><span data-stu-id="3fc90-233">Topology Specification Language</span></span>
<span data-ttu-id="3fc90-234">SCP topológia meghatározása az adott nyelvhez és SCP topológiák konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-234">SCP Topology Specification is a domain specific language for describing and configuring SCP topologies.</span></span> <span data-ttu-id="3fc90-235">A Storm Clojure DSL alapul (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) és az SCP bővített.</span><span class="sxs-lookup"><span data-stu-id="3fc90-235">It is based on Storm’s Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) and is extended by SCP.</span></span>

<span data-ttu-id="3fc90-236">Topológia specifikációk küldheti el közvetlenül a storm-fürt végrehajtásra keresztül a ***runspec*** parancsot.</span><span class="sxs-lookup"><span data-stu-id="3fc90-236">Topology specifications can be submitted directly to storm cluster for execution via the ***runspec*** command.</span></span>

<span data-ttu-id="3fc90-237">SCP.NET van a tranzakciós topológia meghatározásához kövesse funkciók hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="3fc90-237">SCP.NET has add follow functions to define the Transactional Topology:</span></span>

| <span data-ttu-id="3fc90-238">**Új funkciók**</span><span class="sxs-lookup"><span data-stu-id="3fc90-238">**New Functions**</span></span> | <span data-ttu-id="3fc90-239">**Paraméterek**</span><span class="sxs-lookup"><span data-stu-id="3fc90-239">**Parameters**</span></span> | <span data-ttu-id="3fc90-240">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="3fc90-240">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3fc90-241">**Tx-topolopy**</span><span class="sxs-lookup"><span data-stu-id="3fc90-241">**tx-topolopy**</span></span> |<span data-ttu-id="3fc90-242">topológia-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-242">topology-name</span></span><br /><span data-ttu-id="3fc90-243">spout-leképezés</span><span class="sxs-lookup"><span data-stu-id="3fc90-243">spout-map</span></span><br /><span data-ttu-id="3fc90-244">bolt-leképezés</span><span class="sxs-lookup"><span data-stu-id="3fc90-244">bolt-map</span></span> |<span data-ttu-id="3fc90-245">Adja meg a topológia néven, a tranzakciós topológia &nbsp;spoutok definition térkép és a boltokhoz definition térkép</span><span class="sxs-lookup"><span data-stu-id="3fc90-245">Define a transactional topology with the topology name, &nbsp;spouts definition map and the bolts definition map</span></span> |
| <span data-ttu-id="3fc90-246">**SCP-tx-spout**</span><span class="sxs-lookup"><span data-stu-id="3fc90-246">**scp-tx-spout**</span></span> |<span data-ttu-id="3fc90-247">Exec-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-247">exec-name</span></span><br /><span data-ttu-id="3fc90-248">argumentum</span><span class="sxs-lookup"><span data-stu-id="3fc90-248">args</span></span><br /><span data-ttu-id="3fc90-249">Mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-249">fields</span></span> |<span data-ttu-id="3fc90-250">Adja meg a tranzakciós spout.</span><span class="sxs-lookup"><span data-stu-id="3fc90-250">Define a transactional spout.</span></span> <span data-ttu-id="3fc90-251">Futtatná az alkalmazást ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="3fc90-251">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="3fc90-252">A ***mezők*** a spout a kimeneti mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-252">The ***fields*** is the Output Fields for spout</span></span> |
| <span data-ttu-id="3fc90-253">**SCP-tx-batch-bolt**</span><span class="sxs-lookup"><span data-stu-id="3fc90-253">**scp-tx-batch-bolt**</span></span> |<span data-ttu-id="3fc90-254">Exec-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-254">exec-name</span></span><br /><span data-ttu-id="3fc90-255">argumentum</span><span class="sxs-lookup"><span data-stu-id="3fc90-255">args</span></span><br /><span data-ttu-id="3fc90-256">Mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-256">fields</span></span> |<span data-ttu-id="3fc90-257">Adja meg egy olyan tranzakciós kötegben Bolthoz.</span><span class="sxs-lookup"><span data-stu-id="3fc90-257">Define a transactional Batch Bolt.</span></span> <span data-ttu-id="3fc90-258">Futtatná az alkalmazást ***exec-name*** használatával ***argumentum.***</span><span class="sxs-lookup"><span data-stu-id="3fc90-258">It will run the application with ***exec-name*** using ***args.***</span></span><br /><br /><span data-ttu-id="3fc90-259">A mezők a mező a kimenetre bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-259">The Fields is the Output Fields for bolt.</span></span> |
| <span data-ttu-id="3fc90-260">**SCP-tx-commit-bolt**</span><span class="sxs-lookup"><span data-stu-id="3fc90-260">**scp-tx-commit-bolt**</span></span> |<span data-ttu-id="3fc90-261">Exec-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-261">exec-name</span></span><br /><span data-ttu-id="3fc90-262">argumentum</span><span class="sxs-lookup"><span data-stu-id="3fc90-262">args</span></span><br /><span data-ttu-id="3fc90-263">Mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-263">fields</span></span> |<span data-ttu-id="3fc90-264">Adja meg egy olyan tranzakciós Committer Bolthoz.</span><span class="sxs-lookup"><span data-stu-id="3fc90-264">Define a transactional Committer Bolt.</span></span> <span data-ttu-id="3fc90-265">Futtatná az alkalmazást ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="3fc90-265">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="3fc90-266">A ***mezők*** a bolt a kimeneti mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-266">The ***fields*** is the Output Fields for bolt</span></span> |
| <span data-ttu-id="3fc90-267">**nontx-topolopy**</span><span class="sxs-lookup"><span data-stu-id="3fc90-267">**nontx-topolopy**</span></span> |<span data-ttu-id="3fc90-268">topológia-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-268">topology-name</span></span><br /><span data-ttu-id="3fc90-269">spout-leképezés</span><span class="sxs-lookup"><span data-stu-id="3fc90-269">spout-map</span></span><br /><span data-ttu-id="3fc90-270">bolt-leképezés</span><span class="sxs-lookup"><span data-stu-id="3fc90-270">bolt-map</span></span> |<span data-ttu-id="3fc90-271">Adja meg a nem tranzakciós-topológia a topológia nevű&nbsp; spoutok definition térkép és a boltokhoz definition térkép</span><span class="sxs-lookup"><span data-stu-id="3fc90-271">Define a nontransactional topology with the topology name,&nbsp; spouts definition map and the bolts definition map</span></span> |
| <span data-ttu-id="3fc90-272">**SCP-spout**</span><span class="sxs-lookup"><span data-stu-id="3fc90-272">**scp-spout**</span></span> |<span data-ttu-id="3fc90-273">Exec-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-273">exec-name</span></span><br /><span data-ttu-id="3fc90-274">argumentum</span><span class="sxs-lookup"><span data-stu-id="3fc90-274">args</span></span><br /><span data-ttu-id="3fc90-275">Mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-275">fields</span></span><br /><span data-ttu-id="3fc90-276">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="3fc90-276">parameters</span></span> |<span data-ttu-id="3fc90-277">Adja meg egy nem tranzakciós spout.</span><span class="sxs-lookup"><span data-stu-id="3fc90-277">Define a nontransactional spout.</span></span> <span data-ttu-id="3fc90-278">Futtatná az alkalmazást ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="3fc90-278">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="3fc90-279">A ***mezők*** a spout a kimeneti mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-279">The ***fields*** is the Output Fields for spout</span></span><br /><br /><span data-ttu-id="3fc90-280">A ***paraméterek*** nem kötelező, segítségével adja meg az egyes paraméterek, például "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="3fc90-280">The ***parameters*** is optional, using it to specify some parameters such as "nontransactional.ack.enabled".</span></span> |
| <span data-ttu-id="3fc90-281">**SCP-bolt**</span><span class="sxs-lookup"><span data-stu-id="3fc90-281">**scp-bolt**</span></span> |<span data-ttu-id="3fc90-282">Exec-név</span><span class="sxs-lookup"><span data-stu-id="3fc90-282">exec-name</span></span><br /><span data-ttu-id="3fc90-283">argumentum</span><span class="sxs-lookup"><span data-stu-id="3fc90-283">args</span></span><br /><span data-ttu-id="3fc90-284">Mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-284">fields</span></span><br /><span data-ttu-id="3fc90-285">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="3fc90-285">parameters</span></span> |<span data-ttu-id="3fc90-286">Adja meg egy nem tranzakciós Bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-286">Define a nontransactional Bolt.</span></span> <span data-ttu-id="3fc90-287">Futtatná az alkalmazást ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="3fc90-287">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="3fc90-288">A ***mezők*** a bolt a kimeneti mezők</span><span class="sxs-lookup"><span data-stu-id="3fc90-288">The ***fields*** is the Output Fields for bolt</span></span><br /><br /><span data-ttu-id="3fc90-289">A ***paraméterek*** nem kötelező, segítségével adja meg az egyes paraméterek, például "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="3fc90-289">The ***parameters*** is optional, using it to specify some parameters such as "nontransactional.ack.enabled".</span></span> |

<span data-ttu-id="3fc90-290">SCP.NET rendelkezik hajtsa végre a kulcsok definiálva szavak:</span><span class="sxs-lookup"><span data-stu-id="3fc90-290">SCP.NET has follow keys words defined:</span></span>

| <span data-ttu-id="3fc90-291">**Kulcs szavakat**</span><span class="sxs-lookup"><span data-stu-id="3fc90-291">**Key Words**</span></span> | <span data-ttu-id="3fc90-292">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="3fc90-292">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="3fc90-293">**: neve**</span><span class="sxs-lookup"><span data-stu-id="3fc90-293">**:name**</span></span> |<span data-ttu-id="3fc90-294">A topológia nevének meghatározása</span><span class="sxs-lookup"><span data-stu-id="3fc90-294">Define the Topology Name</span></span> |
| <span data-ttu-id="3fc90-295">**: topológia**</span><span class="sxs-lookup"><span data-stu-id="3fc90-295">**:topology**</span></span> |<span data-ttu-id="3fc90-296">Adja meg a topológia a fenti függvények használatával, valamint azokat, a build.</span><span class="sxs-lookup"><span data-stu-id="3fc90-296">Define the Topology using the above functions and build in ones.</span></span> |
| <span data-ttu-id="3fc90-297">**: p**</span><span class="sxs-lookup"><span data-stu-id="3fc90-297">**:p**</span></span> |<span data-ttu-id="3fc90-298">Adja meg az egyes spout vagy bolt a párhuzamos végrehajtás mutató.</span><span class="sxs-lookup"><span data-stu-id="3fc90-298">Define the parallelism hint for each spout or bolt.</span></span> |
| <span data-ttu-id="3fc90-299">**: config**</span><span class="sxs-lookup"><span data-stu-id="3fc90-299">**:config**</span></span> |<span data-ttu-id="3fc90-300">Adja meg konfigurálása paraméter, vagy módosíthat a már meglévő</span><span class="sxs-lookup"><span data-stu-id="3fc90-300">Define configure parameter or update the existing ones</span></span> |
| <span data-ttu-id="3fc90-301">**: séma**</span><span class="sxs-lookup"><span data-stu-id="3fc90-301">**:schema**</span></span> |<span data-ttu-id="3fc90-302">Az adatfolyam-séma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="3fc90-302">Define the Schema of Stream.</span></span> |

<span data-ttu-id="3fc90-303">És a gyakran használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="3fc90-303">And frequently-used parameters:</span></span>

| <span data-ttu-id="3fc90-304">**A paraméter**</span><span class="sxs-lookup"><span data-stu-id="3fc90-304">**Parameter**</span></span> | <span data-ttu-id="3fc90-305">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="3fc90-305">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="3fc90-306">**"plugin.name"**</span><span class="sxs-lookup"><span data-stu-id="3fc90-306">**"plugin.name"**</span></span> |<span data-ttu-id="3fc90-307">a C# beépülő modul exe-fájl neve</span><span class="sxs-lookup"><span data-stu-id="3fc90-307">exe file name of the C# plugin</span></span> |
| <span data-ttu-id="3fc90-308">**"plugin.args"**</span><span class="sxs-lookup"><span data-stu-id="3fc90-308">**"plugin.args"**</span></span> |<span data-ttu-id="3fc90-309">beépülő modul argumentum</span><span class="sxs-lookup"><span data-stu-id="3fc90-309">plugin args</span></span> |
| <span data-ttu-id="3fc90-310">**"output.schema"**</span><span class="sxs-lookup"><span data-stu-id="3fc90-310">**"output.schema"**</span></span> |<span data-ttu-id="3fc90-311">Kimeneti séma</span><span class="sxs-lookup"><span data-stu-id="3fc90-311">Output schema</span></span> |
| <span data-ttu-id="3fc90-312">**"nontransactional.ack.enabled"**</span><span class="sxs-lookup"><span data-stu-id="3fc90-312">**"nontransactional.ack.enabled"**</span></span> |<span data-ttu-id="3fc90-313">Nyugtázási engedélyezve van-e a nem tranzakciós topológia</span><span class="sxs-lookup"><span data-stu-id="3fc90-313">Whether ack is enabled for nontransactional topology</span></span> |

<span data-ttu-id="3fc90-314">A runspec parancs telepíti a bits együtt, a használati hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3fc90-314">The runspec command will be deployed together with the bits, the usage is like:</span></span>

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

<span data-ttu-id="3fc90-315">A ***erőforrás-dir*** paraméter nem kötelező, meg kell adnia azt, ha azt szeretné, hogy csatlakoztassák a C\# alkalmazás és az ebben a könyvtárban fogja tartalmazni az alkalmazást, a függőségek és konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-315">The ***resource-dir*** parameter is optional, you need to specify it when you want to plug a C\# application, and this directory will contain the application, the dependencies and configurations.</span></span>

<span data-ttu-id="3fc90-316">A ***classpath*** paramétert is nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="3fc90-316">The ***classpath*** parameter is also optional.</span></span> <span data-ttu-id="3fc90-317">Adja meg a Java classpath, ha a fájlmegadásában fájl tartalmaz Java Spout vagy Bolt szolgál.</span><span class="sxs-lookup"><span data-stu-id="3fc90-317">It is used to specify the Java classpath if the spec file contains Java Spout or Bolt.</span></span>

## <a name="miscellaneous-features"></a><span data-ttu-id="3fc90-318">Egyéb szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3fc90-318">Miscellaneous Features</span></span>
### <a name="input-and-output-schema-declaration"></a><span data-ttu-id="3fc90-319">Bemeneti és kimeneti séma nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="3fc90-319">Input and Output Schema Declaration</span></span>
<span data-ttu-id="3fc90-320">A felhasználó el tudná küldeni c. rekordot\# folyamat, a platform kell szerializálni a rekord a byte [], Java ügyféloldali átvitele, és a Storm a rekord átkerül a célokat.</span><span class="sxs-lookup"><span data-stu-id="3fc90-320">The user can emit tuple in C\# process, the platform needs to serialize the tuple into byte[], transfer to Java side, and Storm will transfer this tuple to the targets.</span></span> <span data-ttu-id="3fc90-321">Eközben a alsóbb rétegbeli összetevők, a C\# folyamat rekordot fogadja vissza java oldaláról, és konvertálja az eredeti típusok platform, ezeket a műveleteket a platform rejtett.</span><span class="sxs-lookup"><span data-stu-id="3fc90-321">Meanwhile in downstream component, the C\# process will receive tuple back from java side, and convert it to the original types by platform, all these operations are hidden by the Platform.</span></span>

<span data-ttu-id="3fc90-322">A szerializálás és a deszerializálás támogatására, felhasználói kód kell a be- és kimenetekkel sémája deklarálja.</span><span class="sxs-lookup"><span data-stu-id="3fc90-322">To support the serialization and deserialization, user code needs to declare the schema of the inputs and outputs.</span></span>

<span data-ttu-id="3fc90-323">A bemeneti/kimeneti adatfolyam séma dictionary típusúként van definiálva, a kulcs a StreamId és az oszlopok típusú érték.</span><span class="sxs-lookup"><span data-stu-id="3fc90-323">The input/output stream schema is defined as a dictionary, the key is the StreamId and the value is the Types of the columns.</span></span> <span data-ttu-id="3fc90-324">Az összetevő rendelkezhet több adatfolyamok deklarálva.</span><span class="sxs-lookup"><span data-stu-id="3fc90-324">The component can have multi-streams declared.</span></span>

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


<span data-ttu-id="3fc90-325">A környezeti objektumot a következő API, hozzáadott vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="3fc90-325">In Context object, we have the following API added:</span></span>

    public void DeclareComponentSchema(ComponentStreamSchema schema)

<span data-ttu-id="3fc90-326">Biztosítsa, hogy a felhasználói kód, hogy a kibocsátott rekordokat veszi fel az adott adatfolyam definiált séma, vagy a rendszer kivételhibát futásidejű kivételt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-326">User code must make sure the tuples emitted obey the schema defined for that stream, or the system will throw a runtime exception.</span></span>

### <a name="multi-stream-support"></a><span data-ttu-id="3fc90-327">Több adatfolyam-támogatás</span><span class="sxs-lookup"><span data-stu-id="3fc90-327">Multi-Stream Support</span></span>
<span data-ttu-id="3fc90-328">Szolgáltatáskapcsolódási pont felhasználói kód kibocsátás / egyszerre több különböző adatfolyam fogadását támogatja.</span><span class="sxs-lookup"><span data-stu-id="3fc90-328">SCP supports user code to emit or receive from multiple distinct streams at the same time.</span></span> <span data-ttu-id="3fc90-329">A támogatási által adott jelentéseket tükrözik a Context objektumra, kibocsátása metódus egy választható adatfolyam-azonosító paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="3fc90-329">The support reflects in the Context object as the Emit method takes an optional stream ID parameter.</span></span>

<span data-ttu-id="3fc90-330">A SCP.NET környezeti objektumot a két módszer hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="3fc90-330">Two methods in the SCP.NET Context object have been added.</span></span> <span data-ttu-id="3fc90-331">Segítségükkel számú rekordot, vagy a rekordokat StreamId adja meg.</span><span class="sxs-lookup"><span data-stu-id="3fc90-331">They are used to emit Tuple or Tuples to specify StreamId.</span></span> <span data-ttu-id="3fc90-332">A StreamId: karakterlánc, és mindkét C konzisztens kell\# és a topológia meghatározása specifikációi.</span><span class="sxs-lookup"><span data-stu-id="3fc90-332">The StreamId is a string and it needs to be consistent in both C\# and the Topology Definition Spec.</span></span>

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

<span data-ttu-id="3fc90-333">Egy nem létező adatfolyam vezérlés futásidejű kivételek miatt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-333">The emitting to a non-existing stream will cause runtime exceptions.</span></span>

### <a name="fields-grouping"></a><span data-ttu-id="3fc90-334">Mezők csoportosítás</span><span class="sxs-lookup"><span data-stu-id="3fc90-334">Fields Grouping</span></span>
<span data-ttu-id="3fc90-335">A beépített mezők csoportosításának Strom a SCP.NET nem működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="3fc90-335">The build-in Fields Grouping in Strom is not working properly in SCP.NET.</span></span> <span data-ttu-id="3fc90-336">A Java-Proxy oldalon a mezők az összes adattípus ténylegesen byte [], és a csoportosítási mezőket a byte [] objektum kivonatkód segítségével hajtsa végre a csoportosítási.</span><span class="sxs-lookup"><span data-stu-id="3fc90-336">On the Java Proxy side, all the fields data types are actually byte[], and the fields grouping uses the byte[] object hash code to perform the grouping.</span></span> <span data-ttu-id="3fc90-337">A byte [] objektum kivonatkód az a cím az objektum a memóriában.</span><span class="sxs-lookup"><span data-stu-id="3fc90-337">The byte[] object hash code is the address of this object in memory.</span></span> <span data-ttu-id="3fc90-338">Ezért a csoportosítás két byte [] objektumok, amelyek ugyanahhoz a tartalomhoz, de nem ugyanazt a címet helytelen lesz.</span><span class="sxs-lookup"><span data-stu-id="3fc90-338">So the grouping will be wrong for two byte[] objects that share the same content but not the same address.</span></span>

<span data-ttu-id="3fc90-339">SCP.NET hozzáadja egy testreszabott csoportosítási módszer, majd a byte [] tartalmának fogja használni ehhez a csoportosítást.</span><span class="sxs-lookup"><span data-stu-id="3fc90-339">SCP.NET adds a customized grouping method, and it will use the content of the byte[] to do the grouping.</span></span> <span data-ttu-id="3fc90-340">A **SPEC** fájl, a szintaxis hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3fc90-340">In **SPEC** file, the syntax is like:</span></span>

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


<span data-ttu-id="3fc90-341">itt</span><span class="sxs-lookup"><span data-stu-id="3fc90-341">Here,</span></span>

1. <span data-ttu-id="3fc90-342">"scp-mező-csoport": "Testre szabott mező csoportosítási szolgáltatáskapcsolódási pont által megvalósított".</span><span class="sxs-lookup"><span data-stu-id="3fc90-342">"scp-field-group" means "Customized field grouping implemented by SCP".</span></span>
2. <span data-ttu-id="3fc90-343">": tx"vagy": nem tx" azt jelenti, hogy tranzakciós topológia esetén.</span><span class="sxs-lookup"><span data-stu-id="3fc90-343">":tx" or ":non-tx" means if it’s transactional topology.</span></span> <span data-ttu-id="3fc90-344">Mivel a kezdő index különbözik a tx és nem tx topológiák kell ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-344">We need this information since the starting index is different in tx vs. non-tx topologies.</span></span>
3. <span data-ttu-id="3fc90-345">[0,1] azt jelenti, hogy a hashset osztály mező azonosítókat, 0-tól kezdődően.</span><span class="sxs-lookup"><span data-stu-id="3fc90-345">[0,1] means a hashset of field Ids, starting from 0.</span></span>

### <a name="hybrid-topology"></a><span data-ttu-id="3fc90-346">Hibrid topológia</span><span class="sxs-lookup"><span data-stu-id="3fc90-346">Hybrid topology</span></span>
<span data-ttu-id="3fc90-347">A natív Storm Java nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="3fc90-347">The native Storm is written in Java.</span></span> <span data-ttu-id="3fc90-348">És SCP.Net továbbfejlesztett engedélyezéséhez a vámügyi írása C\# kezelésére az üzleti logika kódot.</span><span class="sxs-lookup"><span data-stu-id="3fc90-348">And SCP.Net has enhanced it to enable our customs to write C\# code to handle their business logic.</span></span> <span data-ttu-id="3fc90-349">De is támogatja a hibrid topológiák, amely tartalmaz, nem csak a C\# spoutokkal kapcsolatban/boltokhoz, emellett pedig Java Spout/Boltokhoz.</span><span class="sxs-lookup"><span data-stu-id="3fc90-349">But we also support hybrid topologies, which contains not only C\# spouts/bolts, but also Java Spout/Bolts.</span></span>

### <a name="specify-java-spoutbolt-in-spec-file"></a><span data-ttu-id="3fc90-350">Adja meg a Java Spout vagy Bolt fájlmegadásában fájlban</span><span class="sxs-lookup"><span data-stu-id="3fc90-350">Specify Java Spout/Bolt in spec file</span></span>
<span data-ttu-id="3fc90-351">Speciális fájlban "scp-spout" és "scp-bolt" is segítségével adja meg a Java Spoutok és Boltokhoz, például:</span><span class="sxs-lookup"><span data-stu-id="3fc90-351">In spec file, "scp-spout" and "scp-bolt" can also be used to specify Java Spouts and Bolts, here is an example:</span></span>

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

<span data-ttu-id="3fc90-352">Itt `microsoft.scp.example.HybridTopology.Generator` a Java Spout osztály neve.</span><span class="sxs-lookup"><span data-stu-id="3fc90-352">Here `microsoft.scp.example.HybridTopology.Generator` is the name of the Java Spout class.</span></span>

### <a name="specify-java-classpath-in-runspec-command"></a><span data-ttu-id="3fc90-353">Adja meg a Java Classpath runSpec parancs</span><span class="sxs-lookup"><span data-stu-id="3fc90-353">Specify Java Classpath in runSpec Command</span></span>
<span data-ttu-id="3fc90-354">Ha azt szeretné elküldeni a Java Spoutok vagy Boltokhoz tartalmazó topológia, először a Java Spoutok vagy Boltokhoz fordítása, és a Jar-fájlok szüksége.</span><span class="sxs-lookup"><span data-stu-id="3fc90-354">If you want to submit topology containing Java Spouts or Bolts, you need to first compile the Java Spouts or Bolts and get the Jar files.</span></span> <span data-ttu-id="3fc90-355">Majd adja meg a java classpath, amely tartalmazza a Jar-fájlok topológia elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="3fc90-355">Then you should specify the java classpath that contains the Jar files when submitting topology.</span></span> <span data-ttu-id="3fc90-356">Például:</span><span class="sxs-lookup"><span data-stu-id="3fc90-356">Here is an example:</span></span>

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

<span data-ttu-id="3fc90-357">Itt **példák\\HybridTopology\\java\\cél\\**  a Java Spout vagy Bolt Jar-fájlt tartalmazó mappa.</span><span class="sxs-lookup"><span data-stu-id="3fc90-357">Here **examples\\HybridTopology\\java\\target\\** is the folder containing the Java Spout/Bolt Jar file.</span></span>

### <a name="serialization-and-deserialization-between-java-and-c"></a><span data-ttu-id="3fc90-358">Szerializálás és a deszerializálás között a Java és C\\</span><span class="sxs-lookup"><span data-stu-id="3fc90-358">Serialization and Deserialization between Java and C\\</span></span>
<span data-ttu-id="3fc90-359">A szolgáltatáskapcsolódási pont a következő Java és a C\# oldalán.</span><span class="sxs-lookup"><span data-stu-id="3fc90-359">Our SCP component includes Java side and C\# side.</span></span> <span data-ttu-id="3fc90-360">Ahhoz, hogy kommunikáljanak a natív Java spoutokkal kapcsolatban/Boltokhoz, szerializálással/Deszerializálással el kell végezni között a Java és a C\# oldalán, az alábbi diagramon ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="3fc90-360">In order to interact with native Java Spouts/Bolts, Serialization/Deserialization must be carried out between Java side and C\# side, as illustrated in the following graph.</span></span>

![java-összetevő küldése a szolgáltatáskapcsolódási pont összetevő Java összetevő küldése ábrája](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. <span data-ttu-id="3fc90-362">**A Java és a deszerializálás c. szerializálási\# oldal**</span><span class="sxs-lookup"><span data-stu-id="3fc90-362">**Serialization in Java side and Deserialization in C\# side**</span></span>
   
   <span data-ttu-id="3fc90-363">Először igazolnia implementálásához alapértelmezett szerializálás Java oldal és a deszerializálás c.\# oldalán.</span><span class="sxs-lookup"><span data-stu-id="3fc90-363">First we provide default implementation for serialization in Java side and deserialization in C\# side.</span></span> <span data-ttu-id="3fc90-364">A szerializálási metódus a Java oldalon FÁJLMEGADÁSÁBAN fájl adható meg:</span><span class="sxs-lookup"><span data-stu-id="3fc90-364">The serialization method in Java side can be specified in SPEC file:</span></span>
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   <span data-ttu-id="3fc90-365">A deszerializálási metódus c.\# oldalon kell megadni C\# felhasználói kód:</span><span class="sxs-lookup"><span data-stu-id="3fc90-365">The deserialization method in C\# side should be specified in C\# user code:</span></span>
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   <span data-ttu-id="3fc90-366">Az alapértelmezett implementációja kezelnie kell a legtöbb esetben, ha az adattípus nem túl összetett.</span><span class="sxs-lookup"><span data-stu-id="3fc90-366">This default implementation should handle most cases if the data type is not too complex.</span></span> <span data-ttu-id="3fc90-367">Egyes esetekben, mert a felhasználói adatok típusa túl összetett, vagy mert az alapértelmezett implementációja teljesítményének nem felel meg a felhasználói követelményeket, a beépülő modul felhasználó is a saját megvalósítási.</span><span class="sxs-lookup"><span data-stu-id="3fc90-367">For certain cases, either because the user data type is too complex, or because the performance of our default implementation does not meet the user's requirement, user can plug-in their own implementation.</span></span>
   
   <span data-ttu-id="3fc90-368">A java ügyféloldali szerializálása felület típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="3fc90-368">The serialize interface in java side is defined as:</span></span>
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   <span data-ttu-id="3fc90-369">A C deserialize felület\# ügyféloldali típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="3fc90-369">The deserialize interface in C\# side is defined as:</span></span>
   
   <span data-ttu-id="3fc90-370">Nyilvános csatoló ICustomizedInteropCSharpDeserializer</span><span class="sxs-lookup"><span data-stu-id="3fc90-370">public interface ICustomizedInteropCSharpDeserializer</span></span>
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. <span data-ttu-id="3fc90-371">**Szerializálási c.\# ügyféloldali és a Java párhuzamosan a deszerializálás**</span><span class="sxs-lookup"><span data-stu-id="3fc90-371">**Serialization in C\# side and Deserialization in Java side side**</span></span>
   
   <span data-ttu-id="3fc90-372">A szerializálási metódus a C\# oldalon kell megadni C\# felhasználói kód:</span><span class="sxs-lookup"><span data-stu-id="3fc90-372">The serialization method in C\# side should be specified in C\# user code:</span></span>
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   <span data-ttu-id="3fc90-373">A deszerializálási metódus Java oldal FÁJLMEGADÁSÁBAN fájlt kell megadni:</span><span class="sxs-lookup"><span data-stu-id="3fc90-373">The Deserialization method in Java side should be specified in SPEC file:</span></span>
   
     <span data-ttu-id="3fc90-374">(scp-spout</span><span class="sxs-lookup"><span data-stu-id="3fc90-374">(scp-spout</span></span>
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   <span data-ttu-id="3fc90-375">Itt "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" deszerializáló nevét, és "microsoft.scp.example.HybridTopology.Person" tartozó az adatokat a rendszer deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="3fc90-375">Here "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" is the name of Deserializer, and "microsoft.scp.example.HybridTopology.Person" is the target class the data is deserialized to.</span></span>
   
   <span data-ttu-id="3fc90-376">Felhasználó is beépülő modul a következőket teheti a saját C végrehajtásának\# szerializáló és Java deszerializáló.</span><span class="sxs-lookup"><span data-stu-id="3fc90-376">User can also plug-in their own implementation of C\# serializer and Java Deserializer.</span></span> <span data-ttu-id="3fc90-377">Ez a felület C-hez az\# szerializáló:</span><span class="sxs-lookup"><span data-stu-id="3fc90-377">This is the interface for C\# serializer:</span></span>
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   <span data-ttu-id="3fc90-378">Ez az a Java-deszerializáló felület:</span><span class="sxs-lookup"><span data-stu-id="3fc90-378">This is the interface for Java Deserializer:</span></span>
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a><span data-ttu-id="3fc90-379">Szolgáltatáskapcsolódási pont a gazdagép módja</span><span class="sxs-lookup"><span data-stu-id="3fc90-379">SCP Host Mode</span></span>
<span data-ttu-id="3fc90-380">Ebben a módban a felhasználó DLL a kód fordítása, és küldje el a topológia a szolgáltatáskapcsolódási pont által biztosított SCPHost.exe segítségével.</span><span class="sxs-lookup"><span data-stu-id="3fc90-380">In this mode, user can compile their codes to DLL, and use SCPHost.exe provided by SCP to submit topology.</span></span> <span data-ttu-id="3fc90-381">A speciális fájl így néz ki:</span><span class="sxs-lookup"><span data-stu-id="3fc90-381">The spec file looks like this:</span></span>

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

<span data-ttu-id="3fc90-382">Itt `plugin.name` megadott `SCPHost.exe` SCP SDK által biztosított.</span><span class="sxs-lookup"><span data-stu-id="3fc90-382">Here, `plugin.name` is specified as `SCPHost.exe` provided by SCP SDK.</span></span> <span data-ttu-id="3fc90-383">Pontosan három paramétert fogad, amely SCPHost.exe:</span><span class="sxs-lookup"><span data-stu-id="3fc90-383">SCPHost.exe which accepts exactly three parameters:</span></span>

1. <span data-ttu-id="3fc90-384">Az első címtárra a dll-fájl neve, amely `"HelloWorld.dll"` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="3fc90-384">The first one is the DLL name, which is `"HelloWorld.dll"` in this example.</span></span>
2. <span data-ttu-id="3fc90-385">A második érték az osztály nevét, amely `"Scp.App.HelloWorld.Generator"` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="3fc90-385">The second one is the Class name, which is `"Scp.App.HelloWorld.Generator"` in this example.</span></span>
3. <span data-ttu-id="3fc90-386">A harmadik egy esetén nyilvános statikus metódus, amely ISCPPlugin példányának eléréséhez is elindítható.</span><span class="sxs-lookup"><span data-stu-id="3fc90-386">The third one is the name of a public static method, which can be invoked to get an instance of ISCPPlugin.</span></span>

<span data-ttu-id="3fc90-387">Állomás üzemmódban a felhasználói kód, dll-fájl fordítása, és SCP platform által indított.</span><span class="sxs-lookup"><span data-stu-id="3fc90-387">In host mode, user code is compiled as DLL, and is invoked by SCP platform.</span></span> <span data-ttu-id="3fc90-388">Szolgáltatáskapcsolódási pont platform, teljes körű hozzáférést engedélyezzenek a teljes feldolgozó logika kérheti le.</span><span class="sxs-lookup"><span data-stu-id="3fc90-388">So SCP platform can get full control of the whole processing logic.</span></span> <span data-ttu-id="3fc90-389">Ezért ajánlott SCP-t állomás üzemmódban topológia elküldeni, mivel a fejlesztési élmény egyszerűsítése és velünk, valamint a későbbi kiadásban kapcsolása nagyobb rugalmasságot és jobban előző verziókkal való kompatibilitás ügyfeleink.</span><span class="sxs-lookup"><span data-stu-id="3fc90-389">So we recommend our customers to submit topology in SCP host mode since it can simplify the development experience and bring us more flexibility and better backward compatibility for later release as well.</span></span>

## <a name="scp-programming-examples"></a><span data-ttu-id="3fc90-390">Szolgáltatáskapcsolódási pont programozási példák</span><span class="sxs-lookup"><span data-stu-id="3fc90-390">SCP Programming Examples</span></span>
### <a name="helloworld"></a><span data-ttu-id="3fc90-391">HelloWorld</span><span class="sxs-lookup"><span data-stu-id="3fc90-391">HelloWorld</span></span>
<span data-ttu-id="3fc90-392">**HelloWorld** egy nagyon egyszerű példa egy SCP.Net ízét megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3fc90-392">**HelloWorld** is a very simple example to show a taste of SCP.Net.</span></span> <span data-ttu-id="3fc90-393">A nem tranzakciós topológia használ egy nevű spout **generátor**, és két boltokhoz nevű **elválasztó** és **számláló**.</span><span class="sxs-lookup"><span data-stu-id="3fc90-393">It uses a non-transactional topology, with a spout called **generator**, and two bolts called **splitter** and **counter**.</span></span> <span data-ttu-id="3fc90-394">A spout **generátor** fog véletlenszerűen generálja néhány mondatot, és a kibocsátás a mondatok **elválasztó**.</span><span class="sxs-lookup"><span data-stu-id="3fc90-394">The spout **generator** will randomly generate some sentences, and emit these sentences to **splitter**.</span></span> <span data-ttu-id="3fc90-395">A bolt **elválasztó** fog ossza fel a mondatok szavakat, és ezeket a szavakat kibocsátás **számláló** bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-395">The bolt **splitter** will split the sentences to words and emit these words to **counter** bolt.</span></span> <span data-ttu-id="3fc90-396">A bolt "számláló" dictionary használatával jegyezze fel minden szó előfordulási száma.</span><span class="sxs-lookup"><span data-stu-id="3fc90-396">The bolt "counter" uses a dictionary to record the occurrence number of each word.</span></span>

<span data-ttu-id="3fc90-397">Két fájlmegadásában fájlt **HelloWorld.spec** és **HelloWorld\_EnableAck.spec** ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="3fc90-397">There are two spec files, **HelloWorld.spec** and **HelloWorld\_EnableAck.spec** for this example.</span></span> <span data-ttu-id="3fc90-398">A c\# kódot, akkor talál, nyugtázási engedélyezve van-e el a pluginConf Java oldaláról.</span><span class="sxs-lookup"><span data-stu-id="3fc90-398">In the C\# code, it can find out whether ack is enabled by getting the pluginConf from Java side.</span></span>

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

<span data-ttu-id="3fc90-399">A spout a nyugtázási engedélyezve van, ha dictionary szolgál a rekordokat, amelyek még nincsenek korrektúrák gyorsítótárazásához.</span><span class="sxs-lookup"><span data-stu-id="3fc90-399">In the spout, if ack is enabled, a dictionary is used to cache the tuples that have not been acked.</span></span> <span data-ttu-id="3fc90-400">Ha Fail() neve, a hibás rekord fog bejegyzéseit meg kell ismételni:</span><span class="sxs-lookup"><span data-stu-id="3fc90-400">If Fail() is called, the failed tuple will be replayed:</span></span>

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get the cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay the failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a><span data-ttu-id="3fc90-401">HelloWorldTx</span><span class="sxs-lookup"><span data-stu-id="3fc90-401">HelloWorldTx</span></span>
<span data-ttu-id="3fc90-402">A **HelloWorldTx** példa bemutatja, hogyan tranzakciós topológia végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="3fc90-402">The **HelloWorldTx** example demonstrates how to implement transactional topology.</span></span> <span data-ttu-id="3fc90-403">Van-e egy spout nevű **generátor**, egy kötegelt boltokhoz nevű **részleges-count**, és egy véglegesítési bolt nevű **száma összeg**.</span><span class="sxs-lookup"><span data-stu-id="3fc90-403">It have one spout called **generator**, a batch bolts called **partial-count**, and a commit bolt called **count-sum**.</span></span> <span data-ttu-id="3fc90-404">Van még három előre létrehozott txt-fájloknál: **DataSource0.txt**, **DataSource1.txt** és **DataSource2.txt**.</span><span class="sxs-lookup"><span data-stu-id="3fc90-404">There are also three pre-created txt files: **DataSource0.txt**, **DataSource1.txt** and **DataSource2.txt**.</span></span>

<span data-ttu-id="3fc90-405">Az egyes tranzakciókra, a spout **generátor** fog véletlenszerűen két fájlt választhat az előre létrehozott három fájlt, majd kiadni a két nevet, hogy a **részleges-count** bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-405">In each transaction, the spout **generator** will randomly choose two files from the pre-created three files, and emit the two file names to the **partial-count** bolt.</span></span> <span data-ttu-id="3fc90-406">A bolt **részleges-count** fog először beolvasni a fájlnevet a fogadott rekord, majd nyissa meg a fájlt az ebben a fájlban szavak számát, és végül hozható létre a word számát a **száma összeg** bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-406">The bolt **partial-count** will first get the file name from the received tuple, then open the file and count the number of words in this file, and finally emit the word number to the **count-sum** bolt.</span></span> <span data-ttu-id="3fc90-407">A **száma összeg** bolt azokat a teljes száma.</span><span class="sxs-lookup"><span data-stu-id="3fc90-407">The **count-sum** bolt will summarize the total count.</span></span>

<span data-ttu-id="3fc90-408">Eléréséhez **pontosan egyszer** szemantikáját, a véglegesítési bolt **száma összeg** mennyire, hogy-e egy megismételt tranzakció szükséges.</span><span class="sxs-lookup"><span data-stu-id="3fc90-408">To achieve **exactly once** semantics, the commit bolt **count-sum** need to judge whether it is a replayed transaction.</span></span> <span data-ttu-id="3fc90-409">Ebben a példában egy statikus tag változó rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3fc90-409">In this example, it has a static member variable:</span></span>

    public static long lastCommittedTxId = -1; 

<span data-ttu-id="3fc90-410">Amikor ISCPBatchBolt példánya jön létre, azt fogja kapni az `txAttempt` bemeneti paraméterek közül:</span><span class="sxs-lookup"><span data-stu-id="3fc90-410">When an ISCPBatchBolt instance is created, it will get the `txAttempt` from input parameters:</span></span>

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from the input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

<span data-ttu-id="3fc90-411">Ha `FinishBatch()` neve, a `lastCommittedTxId` frissítés lesz, ha még nincs megismételt tranzakció.</span><span class="sxs-lookup"><span data-stu-id="3fc90-411">When `FinishBatch()` is called, the `lastCommittedTxId` will be update if it is not a replayed transaction.</span></span>

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update the toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a><span data-ttu-id="3fc90-412">HybridTopology</span><span class="sxs-lookup"><span data-stu-id="3fc90-412">HybridTopology</span></span>
<span data-ttu-id="3fc90-413">Ez a topológia tartalmaz egy Java Spout és egy C\# Bolt.</span><span class="sxs-lookup"><span data-stu-id="3fc90-413">This topology contains a Java Spout and a C\# Bolt.</span></span> <span data-ttu-id="3fc90-414">A szolgáltatáskapcsolódási pont platform által biztosított alapértelmezett szerializálása és deszerializálása végrehajtására használ.</span><span class="sxs-lookup"><span data-stu-id="3fc90-414">It uses the default serialization and deserialization implementation provided by SCP platform.</span></span> <span data-ttu-id="3fc90-415">Kérjük, ref a **HybridTopology.spec** a **példák\\HybridTopology** mappában a fájlmegadásában fájlt, és **SubmitTopology.bat** a Java classpath megadása.</span><span class="sxs-lookup"><span data-stu-id="3fc90-415">Please ref the **HybridTopology.spec** in **examples\\HybridTopology** folder for the spec file details, and **SubmitTopology.bat** for how to specify Java classpath.</span></span>

### <a name="scphostdemo"></a><span data-ttu-id="3fc90-416">SCPHostDemo</span><span class="sxs-lookup"><span data-stu-id="3fc90-416">SCPHostDemo</span></span>
<span data-ttu-id="3fc90-417">Ez a példa megegyezik a HelloWorld lényegében.</span><span class="sxs-lookup"><span data-stu-id="3fc90-417">This example is the same as HelloWorld in essence.</span></span> <span data-ttu-id="3fc90-418">Az egyetlen különbség, hogy a felhasználói kód lefordított dll-fájl, és a topológia beküldött SCPHost.exe használatával.</span><span class="sxs-lookup"><span data-stu-id="3fc90-418">The only difference is that the user code is compiled as DLL and the topology is submitted by using SCPHost.exe.</span></span> <span data-ttu-id="3fc90-419">Kérjük, ref részletes ismertetése a "SCP-t állomás üzemmódban" szakasz.</span><span class="sxs-lookup"><span data-stu-id="3fc90-419">Please ref the section "SCP Host Mode" for more detailed explanation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3fc90-420">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fc90-420">Next Steps</span></span>
<span data-ttu-id="3fc90-421">Szolgáltatáskapcsolódási pont használatával létrehozott Storm-topológiák példákért lásd a következő:</span><span class="sxs-lookup"><span data-stu-id="3fc90-421">For examples of Storm topologies created using SCP, see the following:</span></span>

* [<span data-ttu-id="3fc90-422">Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="3fc90-422">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="3fc90-423">Az Azure Event Hubs a HDInsight alatt futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="3fc90-423">Process events from Azure Event Hubs with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [<span data-ttu-id="3fc90-424">Hozzon létre több adatfolyamokat a C# Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="3fc90-424">Create multiple data streams in a C# Storm topology</span></span>](hdinsight-storm-twitter-trending.md)
* [<span data-ttu-id="3fc90-425">A Storm-topológia adatainak megjelenítése Power Bi használatával</span><span class="sxs-lookup"><span data-stu-id="3fc90-425">Use Power Bi to visualize data from a Storm topology</span></span>](hdinsight-storm-power-bi-topology.md)
* [<span data-ttu-id="3fc90-426">Az Event Hubs a HDInsight alatt futó Storm használatával vehicle érzékelő adatok feldolgozása</span><span class="sxs-lookup"><span data-stu-id="3fc90-426">Process vehicle sensor data from Event Hubs using Storm on HDInsight</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [<span data-ttu-id="3fc90-427">Bontsa ki, átalakítás és betöltés (ETL) az Azure Event Hubs HBase</span><span class="sxs-lookup"><span data-stu-id="3fc90-427">Extract, Transform, and Load (ETL) from Azure Event Hubs to HBase</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [<span data-ttu-id="3fc90-428">A HDInsight alatt futó Storm és HBase használatával összefüggésbe események</span><span class="sxs-lookup"><span data-stu-id="3fc90-428">Correlate events using Storm and HBase on HDInsight</span></span>](hdinsight-storm-correlation-topology.md)

