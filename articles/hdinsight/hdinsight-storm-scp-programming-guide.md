---
title: "aaaSCP.NET programozási útmutatója |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse SCP.NET toocreate. A NET-alapú Storm-topológiák a HDInsight alatt futó Storm használható."
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
ms.openlocfilehash: a57f4217b07e0e82a3f36650308695fbb45d9128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scp-programming-guide"></a><span data-ttu-id="8bb6c-103">Szolgáltatáskapcsolódási pont programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="8bb6c-103">SCP programming guide</span></span>
<span data-ttu-id="8bb6c-104">Szolgáltatáskapcsolódási pont, a platform toobuild valós idejű, megbízható, következetes és magas teljesítmény adatfeldolgozási alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-104">SCP is a platform toobuild real time, reliable, consistent and high performance data processing application.</span></span> <span data-ttu-id="8bb6c-105">Be van építve a [alatt futó Apache Storm](http://storm.incubator.apache.org/) – a streamfeldolgozási rendszer hello OSS Közösségek szerint.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-105">It is built on top of [Apache Storm](http://storm.incubator.apache.org/) -- a stream processing system designed by hello OSS communities.</span></span> <span data-ttu-id="8bb6c-106">A Storm készült Nathan Marz, és nyissa meg által Twitter forrása.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-106">Storm is designed by Nathan Marz and open sourced by Twitter.</span></span> <span data-ttu-id="8bb6c-107">Ez a módszer a [Apache ZooKeeper](http://zookeeper.apache.org/), egy másik Apache tooenable nagymértékben megbízható elosztott koordinálásának és állapotkezelés projektre.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-107">It leverages [Apache ZooKeeper](http://zookeeper.apache.org/), another Apache project tooenable highly reliable distributed coordination and state management.</span></span> 

<span data-ttu-id="8bb6c-108">Nem csak hello SCP projekt legelterjedtebb Windows alatt futó Storm, de hello projekt adott bővítmények és hello Windows ökoszisztéma testreszabása.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-108">Not only hello SCP project ported Storm on Windows but also hello project added extensions and customization for hello Windows ecosystem.</span></span> <span data-ttu-id="8bb6c-109">hello bővítmények közé tartozik a .NET-fejlesztők számára, és a könyvtárak, hello testreszabás tartalmazza a Windows-alapú telepítést.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-109">hello extensions include .NET developer experience, and libraries, hello customization includes Windows-based deployment.</span></span> 

<span data-ttu-id="8bb6c-110">hello bővítményt, és a Testreszabás úgy, hogy azt nem kell toofork hello OSS-projektek, és azt sikerült kihasználhatják a Storm épülő származtatott ökoszisztéma történik.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-110">hello extension and customization is done in such a way that we do not need toofork hello OSS projects and we could leverage derived ecosystems built on top of Storm.</span></span>

## <a name="processing-model"></a><span data-ttu-id="8bb6c-111">Folyamatmodell</span><span class="sxs-lookup"><span data-stu-id="8bb6c-111">Processing model</span></span>
<span data-ttu-id="8bb6c-112">a szolgáltatáskapcsolódási pont hello adatok folyamatos listának van modellezve.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-112">hello data in SCP is modeled as continuous streams of tuples.</span></span> <span data-ttu-id="8bb6c-113">Általában hello rekordokat flow néhány várólistán először, majd fel, és át legyenek-e a Storm-topológia belül üzleti logika, végül hello kimeneti sikerült kell adatcsatornán rekordokat tooanother SCP rendszert, vagy véglegesített toostores, például az elosztott fájlrendszer vagy adatbázisok, SQL Server például.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-113">Typically hello tuples flow into some queue first, then picked up, and transformed by business logic hosted inside a Storm topology, finally hello output could be piped as tuples tooanother SCP system, or be committed toostores like distributed file system or databases like SQL Server.</span></span>

![Adatok tooprocessing, amely adattárat-adatcsatornákat etetési várólista ábrázoló diagram](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

<span data-ttu-id="8bb6c-115">Egy alkalmazás topológia a Storm, egy számítási grafikont határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-115">In Storm, an application topology defines a graph of computation.</span></span> <span data-ttu-id="8bb6c-116">Minden csomópont-topológiában feldolgozási logikát tartalmaz, és csomópontok közötti kapcsolatok jelölése.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-116">Each node in a topology contains processing logic, and links between nodes indicate data flow.</span></span> <span data-ttu-id="8bb6c-117">hello csomópontok tooinject bemeneti adatok hello topológia spoutokkal kapcsolatban, amelyek használt toosequence hello adatokat is nevezik.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-117">hello nodes tooinject input data into hello topology are called Spouts, which can be used toosequence hello data.</span></span> <span data-ttu-id="8bb6c-118">hello bemeneti adatok sikerült található fájl naplókat, a tranzakciós adatbázis, a rendszer teljesítményszámláló stb hello csomópontokat a mindkét bemeneti és kimeneti adatfolyamok Boltokhoz, amelyek tényleges adatok szűrése hello és beállításokat és összesítési beállítások nevezzük.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-118">hello input data could reside in file logs, transactional database, system performance counter etc. hello nodes with both input and output data flows are called Bolts, which do hello actual data filtering and selections and aggregation.</span></span>

<span data-ttu-id="8bb6c-119">SCP-JE támogatja az ajánlott azon törekvéseit, a következő legalább egyszeri és pontosan-egyszer adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-119">SCP supports best efforts, at-least-once and exactly-once data processing.</span></span> <span data-ttu-id="8bb6c-120">Az elosztott adatfolyam feldolgozása alkalmazásokban több hiba fordulhat elő, során az adatfeldolgozás, például a hálózati kimaradás, a gép hibájának vagy a felhasználói kód hiba stb. A legalább egyszeri feldolgozási biztosítja dolgoz fel az összes adat legalább egyszer által automatikusan visszajátszását hello ugyanazokat az adatokat, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-120">In a distributed streaming processing application, various errors may happen during data processing, such as network outage, machine failure, or user code error etc. At-least-once processing ensures all data will be processed at least once by replaying automatically hello same data when error happens.</span></span> <span data-ttu-id="8bb6c-121">A legalább egyszeri feldolgozási egyszerű és megbízható, és számos alkalmazásban is megfelel.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-121">At-least-once processing is simple and reliable and suits well in many applications.</span></span> <span data-ttu-id="8bb6c-122">Azonban ha hello alkalmazás szükséges a pontos leltár, például: legalább egyszeri feldolgozási nem elegendő óta hello ugyanazokat az adatokat sikertelen potenciálisan lejátszandó hello alkalmazás topológiában.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-122">However, when hello application requires exact counting, for example, at-least-once processing is insufficient since hello same data could potentially be played in hello application topology.</span></span> <span data-ttu-id="8bb6c-123">Ebben az esetben, pontosan-feldolgozási úgy van kialakítva, miután toomake meg arról, hogy hello eredménye helyes-e akkor is, ha hello adatokat a rendszer játssza és többször feldolgozása történhet.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-123">In that case, exactly-once processing is designed toomake sure hello result is correct even when hello data may be replayed and processed multiple times.</span></span>

<span data-ttu-id="8bb6c-124">Használja ki az hello Java virtuális gép (JVM)-alapú Storm alatt hello SCP bekapcsolása .NET-fejlesztők toodevelop valós idejű adatok folyamat alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-124">SCP enables .NET developers toodevelop real time data process applications while leverage hello Java Virtual Machine (JVM) based Storm under hello cover.</span></span> <span data-ttu-id="8bb6c-125">hello .NET és a JVM TCP helyi szoftvercsatorna keresztül kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-125">hello .NET and JVM communicate via TCP local socket.</span></span> <span data-ttu-id="8bb6c-126">Alapvetően minden Spout vagy Bolt .net/Java-folyamat párból hello felhasználói programot futtató .net folyamatban, a beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-126">Basically each Spout/Bolt is a .Net/Java process pair, where hello user logic runs in .Net process as a plugin.</span></span>

<span data-ttu-id="8bb6c-127">toobuild egy adatfeldolgozási alkalmazás SCP fölött, több lépésre van szükség:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-127">toobuild a data processing application on top of SCP, several steps are needed:</span></span>

* <span data-ttu-id="8bb6c-128">Kialakításával és a várólistában lévő adatok hello spoutokkal kapcsolatban toopull megvalósításával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-128">Design and implement hello Spouts toopull in data from queue.</span></span>
* <span data-ttu-id="8bb6c-129">Tervezése és megvalósítása Boltokhoz tooprocess hello bemeneti adatokat, és az adatok mentése tooexternal tárolja, például adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-129">Design and implement Bolts tooprocess hello input data, and save data tooexternal stores such as Database.</span></span>
* <span data-ttu-id="8bb6c-130">Hello topológia megtervezésére, majd küldje el, és hello topológia futtassa.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-130">Design hello topology, then submit and run hello topology.</span></span> <span data-ttu-id="8bb6c-131">hello topológia meghatározása csomópontok és hello adatok hello csomópontok közötti forgalom.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-131">hello topology defines vertexes and hello data flows between hello vertexes.</span></span> <span data-ttu-id="8bb6c-132">Szolgáltatáskapcsolódási pont hello topológia meghatározása fogad, majd azt egy Storm-fürt, minden egyes csúcspont futtató egy logikai csomóponton telepítette.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-132">SCP will take hello topology specification and deploy it on a Storm cluster, where each vertex runs on one logical node.</span></span> <span data-ttu-id="8bb6c-133">hello feladatátvételi és a méretezésről fog kell venni megvagyunk hello Storm Feladatütemező.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-133">hello failover and scaling will be taken care of by hello Storm task scheduler.</span></span>

<span data-ttu-id="8bb6c-134">Néhány egyszerű példák toowalk azon, hogyan használja majd a dokumentum toobuild adatfeldolgozási alkalmazás SCP-je.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-134">This document will use some simple examples toowalk through how toobuild data processing application with SCP.</span></span>

## <a name="scp-plugin-interface"></a><span data-ttu-id="8bb6c-135">Szolgáltatáskapcsolódási pont beépülő modul felület</span><span class="sxs-lookup"><span data-stu-id="8bb6c-135">SCP Plugin Interface</span></span>
<span data-ttu-id="8bb6c-136">Szolgáltatáskapcsolódási pont beépülő modulok (vagy alkalmazások) olyan önálló exe is futtatható belül a Visual Studio hello fejlesztési fázis során, és éles környezetben a telepítés utáni hello Storm csővezeték csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-136">SCP plugins (or applications) are standalone EXEs that can both run inside Visual Studio during hello development phase, and be plugged into hello Storm pipeline after deployment in production.</span></span> <span data-ttu-id="8bb6c-137">Szolgáltatáskapcsolódási pont hello beépülő modul írása van csak hello ugyanaz, mint bármely más szabványos Windows konzol biztosító alkalmazások írására.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-137">Writing hello SCP plugin is just hello same as writing any other standard Windows console applications.</span></span> <span data-ttu-id="8bb6c-138">SCP.NET platform deklarál spout vagy bolt néhány felületet, és hello beépülő modul a programkód inkább a konfigurációkezelővel.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-138">SCP.NET platform declares some interface for spout/bolt, and hello user plugin code should implement these interfaces.</span></span> <span data-ttu-id="8bb6c-139">hello fő Ez a kialakítás célja, hogy hello a felhasználó a saját üzleti logics, és más dolgokat toobe SCP.NET platform által kezelt elhagyása összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-139">hello main purpose of this design is that hello user can focus on their own business logics, and leaving other things toobe handled by SCP.NET platform.</span></span>

<span data-ttu-id="8bb6c-140">hello beépülő modul a programkód meg kell valósítania az egyik hello következőket adapterhez, hogy hello topológia tranzakciós vagy nem tranzakciós-e, és hogy hello összetevő spout vagy bolt függ.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-140">hello user plugin code should implement one of hello followings interfaces, depends on whether hello topology is transactional or non-transactional, and whether hello component is spout or bolt.</span></span>

* <span data-ttu-id="8bb6c-141">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-141">ISCPSpout</span></span>
* <span data-ttu-id="8bb6c-142">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-142">ISCPBolt</span></span>
* <span data-ttu-id="8bb6c-143">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-143">ISCPTxSpout</span></span>
* <span data-ttu-id="8bb6c-144">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-144">ISCPBatchBolt</span></span>

### <a name="iscpplugin"></a><span data-ttu-id="8bb6c-145">ISCPPlugin</span><span class="sxs-lookup"><span data-stu-id="8bb6c-145">ISCPPlugin</span></span>
<span data-ttu-id="8bb6c-146">ISCPPlugin hello közös felület különféle beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-146">ISCPPlugin is hello common interface for all kinds of plugins.</span></span> <span data-ttu-id="8bb6c-147">Azt jelenleg egy üres felületet.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-147">Currently, it is a dummy interface.</span></span>

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a><span data-ttu-id="8bb6c-148">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-148">ISCPSpout</span></span>
<span data-ttu-id="8bb6c-149">ISCPSpout hello felület nem tranzakciós spout.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-149">ISCPSpout is hello interface for non-transactional spout.</span></span>

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

<span data-ttu-id="8bb6c-150">Ha `NextTuple()` nevezik hello C\# felhasználói kód el tudná küldeni egy vagy több rekordokat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-150">When `NextTuple()` is called, hello C\# user code can emit one or more tuples.</span></span> <span data-ttu-id="8bb6c-151">Ha semmilyen tooemit, ezt a módszert kell visszaadnia nélkül kibocsátó semmit.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-151">If there is nothing tooemit, this method should return without emitting anything.</span></span> <span data-ttu-id="8bb6c-152">Fontos megjegyezni, hogy `NextTuple()`, `Ack()`, és `Fail()` összes nevezzük egy egyetlen szálon c. szoros ismétlődő\# folyamat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-152">It should be noted that `NextTuple()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="8bb6c-153">Nincsenek nem rekordokat tooemit, esetén udvarias toohave NextTuple alvó a rövid idő (például 10 ezredmásodperc) nem toowaste, így túl sok CPU.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-153">When there are no tuples tooemit, it is courteous toohave NextTuple sleep for a short amount of time (such as 10 milliseconds) so as not toowaste too much CPU.</span></span>

<span data-ttu-id="8bb6c-154">`Ack()`és `Fail()` hívja meg a rendszer csak akkor, ha a nyugtázási mechanizmus fájlmegadásában fájlban engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-154">`Ack()` and `Fail()` will be called only when ack mechanism is enabled in spec file.</span></span> <span data-ttu-id="8bb6c-155">Hello `seqId` van használt tooidentify hello rekordot, amely korrektúrák, vagy nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-155">hello `seqId` is used tooidentify hello tuple which is acked or failed.</span></span> <span data-ttu-id="8bb6c-156">Így ha nyugtázási engedélyezve van a nem tranzakciós topológia, kibocsátása függvény a következő hello Spout kell használható:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-156">So if ack is enabled in non-transactional topology, hello following emit function should be used in Spout:</span></span>

    public abstract void Emit(string streamId, List<object> values, long seqId); 

<span data-ttu-id="8bb6c-157">Ha nyugtázási nem támogatott nem tranzakciós topológia, hello `Ack()` és `Fail()` maradhatnak, üres függvényében.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-157">If ack is not supported in non-transactional topology, hello `Ack()` and `Fail()` can be left as empty function.</span></span>

<span data-ttu-id="8bb6c-158">Hello `parms` ezeket a funkciókat a bemeneti paraméterek csak üres szótár, olyan fenntartja a jövőbeli használatra.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-158">hello `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbolt"></a><span data-ttu-id="8bb6c-159">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-159">ISCPBolt</span></span>
<span data-ttu-id="8bb6c-160">ISCPBolt hello felület nem tranzakciós bolt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-160">ISCPBolt is hello interface for non-transactional bolt.</span></span>

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

<span data-ttu-id="8bb6c-161">Új rekord nem érhető el, hello `Execute()` függvényt hívja tooprocess azt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-161">When new tuple is available, hello `Execute()` function will be called tooprocess it.</span></span>

### <a name="iscptxspout"></a><span data-ttu-id="8bb6c-162">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-162">ISCPTxSpout</span></span>
<span data-ttu-id="8bb6c-163">ISCPTxSpout tranzakciós spout hello felület.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-163">ISCPTxSpout is hello interface for transactional spout.</span></span>

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

<span data-ttu-id="8bb6c-164">Akárcsak a nem tranzakciós másik részét `NextTx()`, `Ack()`, és `Fail()` összes nevezzük egy egyetlen szálon c. szoros ismétlődő\# folyamat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-164">Just like their non-transactional counter-part, `NextTx()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="8bb6c-165">Ha nincsenek adatok tooemit,-e udvarias toohave `NextTx` rövid időn (10 ezredmásodperc) alvó nem toowaste, így túl sok CPU.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-165">When there are no data tooemit, it is courteous toohave `NextTx` sleep for a short amount of time (10 milliseconds) so as not toowaste too much CPU.</span></span>

<span data-ttu-id="8bb6c-166">`NextTx()`feltüntettük toostart egy új tranzakció hello paraméter `seqId` is használva van használt tooidentify hello tranzakció `Ack()` és `Fail()`.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-166">`NextTx()` is called toostart a new transaction, hello out parameter `seqId` is used tooidentify hello transaction, which is also used in `Ack()` and `Fail()`.</span></span> <span data-ttu-id="8bb6c-167">A `NextTx()`, felhasználói el tudná küldeni adatokat tooJava oldalán.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-167">In `NextTx()`, user can emit data tooJava side.</span></span> <span data-ttu-id="8bb6c-168">ZooKeeper toosupport ismétlési hello adatokat fog tárolni.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-168">hello data will be stored in ZooKeeper toosupport replay.</span></span> <span data-ttu-id="8bb6c-169">ZooKeeper hello kapacitása korlátozott, mert felhasználói csak kell számú metaadat, a tranzakciós spout nem tömeges adatok.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-169">Because hello capacity of ZooKeeper is very limited, user should only emit metadata, not bulk data in transactional spout.</span></span>

<span data-ttu-id="8bb6c-170">A Storm fog visszajátszásos automatikusan egy tranzakció, ha a hiba, így `Fail()` normál esetben nem szabad meghívni.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-170">Storm will replay a transaction automatically if it fails, so `Fail()` should not be called in normal case.</span></span> <span data-ttu-id="8bb6c-171">De ha a szolgáltatáskapcsolódási pont ellenőrizheti a hello metaadatok tranzakciós spout által kibocsátott, akkor meghívhatja `Fail()` hello metaadat érvénytelen esetén.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-171">But if SCP can check hello metadata emitted by transactional spout, it can call `Fail()` when hello metadata is invalid.</span></span>

<span data-ttu-id="8bb6c-172">Hello `parms` ezeket a funkciókat a bemeneti paraméterek csak üres szótár, olyan fenntartja a jövőbeli használatra.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-172">hello `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbatchbolt"></a><span data-ttu-id="8bb6c-173">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-173">ISCPBatchBolt</span></span>
<span data-ttu-id="8bb6c-174">ISCPBatchBolt tranzakciós bolt hello felület.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-174">ISCPBatchBolt is hello interface for transactional bolt.</span></span>

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

<span data-ttu-id="8bb6c-175">`Execute()`Ha bejövő hello bolt új rekordot neve.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-175">`Execute()` is called when there is new tuple arriving at hello bolt.</span></span> <span data-ttu-id="8bb6c-176">`FinishBatch()`Amikor befejeződik a tranzakció neve.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-176">`FinishBatch()` is called when this transaction is ended.</span></span> <span data-ttu-id="8bb6c-177">Hello `parms` bemeneti paraméter későbbi használatra van fenntartva.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-177">hello `parms` input parameter is reserved for future use.</span></span>

<span data-ttu-id="8bb6c-178">Tranzakciós topológia, egy fontos tényező – nincs `StormTxAttempt`.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-178">For transactional topology, there is an important concept – `StormTxAttempt`.</span></span> <span data-ttu-id="8bb6c-179">Rendelkezik két mező `TxId` és `AttemptId`.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-179">It has two fields, `TxId` and `AttemptId`.</span></span> <span data-ttu-id="8bb6c-180">`TxId`használt tooidentify egy bizonyos tranzakció, és a megadott tranzakció nem lehet több kísérlet hello tranzakció sikertelen, és ha a rendszer játssza vissza.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-180">`TxId` is used tooidentify a specific transaction, and for a given transaction, there may be multiple attempt if hello transaction fails and is replayed.</span></span> <span data-ttu-id="8bb6c-181">Egy másik ISCPBatchBolt objektum tooprocess minden új fog SCP.NET `StormTxAttempt`, csak a például milyen Storm tegye a Java oldalon.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-181">SCP.NET will new a different ISCPBatchBolt object tooprocess each `StormTxAttempt`, just like what Storm do in Java side.</span></span> <span data-ttu-id="8bb6c-182">hello Ez a kialakítás célja toosupport párhuzamos tranzakciók feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-182">hello purpose of this design is toosupport parallel transactions processing.</span></span> <span data-ttu-id="8bb6c-183">Felhasználói tartsa azt szem előtt, ha tranzakció kísérlet befejeződött, hello megfelelő ISCPBatchBolt objektumot meg kell semmisíteni, és szemétgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-183">User should keep it in mind that if transaction attempt is finished, hello corresponding ISCPBatchBolt object will be destroyed and garbage collected.</span></span>

## <a name="object-model"></a><span data-ttu-id="8bb6c-184">Hálózatiobjektum-modellje</span><span class="sxs-lookup"><span data-stu-id="8bb6c-184">Object Model</span></span>
<span data-ttu-id="8bb6c-185">SCP.NET is biztosít egy egyszerű objektumok az fejlesztők tooprogram.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-185">SCP.NET also provides a simple set of key objects for developers tooprogram with.</span></span> <span data-ttu-id="8bb6c-186">Ezek **környezetben**, **Állapottárolója**, és **SCPRuntime**.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-186">They are **Context**, **StateStore**, and **SCPRuntime**.</span></span> <span data-ttu-id="8bb6c-187">Ezek hello többi részében Ez a szakasz tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-187">They will be discussed in hello rest part of this section.</span></span>

### <a name="context"></a><span data-ttu-id="8bb6c-188">Környezet</span><span class="sxs-lookup"><span data-stu-id="8bb6c-188">Context</span></span>
<span data-ttu-id="8bb6c-189">A környezetben futó környezet toohello alkalmazásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-189">Context provides a running environment toohello application.</span></span> <span data-ttu-id="8bb6c-190">Minden egyes ISCPPlugin példány (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) rendelkezik egy megfelelő adatkörnyezet példányához.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-190">Each ISCPPlugin instance (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) has a corresponding Context instance.</span></span> <span data-ttu-id="8bb6c-191">Környezet által biztosított hello funkciókat lehet osztani két részből áll: (1) hello statikus része, amely található hello teljes C\# feldolgozni, (2) hello dinamikus rész, amelyhez a lehetőség csak a hello adott adatkörnyezet példányához.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-191">hello functionality provided by Context can be divided into two parts: (1) hello static part which is available in hello whole C\# process, (2) hello dynamic part which is only available for hello specific Context instance.</span></span>

### <a name="static-part"></a><span data-ttu-id="8bb6c-192">Statikus részében szerepel</span><span class="sxs-lookup"><span data-stu-id="8bb6c-192">Static Part</span></span>
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

<span data-ttu-id="8bb6c-193">`Logger`napló célra valósul meg.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-193">`Logger` is provided for log purpose.</span></span>

<span data-ttu-id="8bb6c-194">`pluginType`van tooindicate hello beépülő modul típusú hello C használt\# folyamat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-194">`pluginType` is used tooindicate hello plugin type of hello C\# process.</span></span> <span data-ttu-id="8bb6c-195">Ha hello C\# folyamat (nélkül Java) tesztcélú helyi módban fut, hello beépülő modul típusa `SCP_NET_LOCAL`.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-195">If hello C\# process is run in local test mode (without Java), hello plugin type is `SCP_NET_LOCAL`.</span></span>

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

<span data-ttu-id="8bb6c-196">`Config`tooget konfigurációs paraméterek Java oldaláról valósul meg.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-196">`Config` is provided tooget configuration parameters from Java side.</span></span> <span data-ttu-id="8bb6c-197">hello paraméterek át lettek adva, a Java oldaláról amikor C\# beépülő modul inicializálása.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-197">hello parameters are passed from Java side when C\# plugin is initialized.</span></span> <span data-ttu-id="8bb6c-198">Hello `Config` paraméterek oszthatók két részből áll: `stormConf` és `pluginConf`.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-198">hello `Config` parameters are divided into two parts: `stormConf` and `pluginConf`.</span></span>

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

<span data-ttu-id="8bb6c-199">`stormConf`a Storm által definiált paraméterek és `pluginConf` szolgáltatáskapcsolódási pont által definiált hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-199">`stormConf` is parameters defined by Storm and `pluginConf` is hello parameters defined by SCP.</span></span> <span data-ttu-id="8bb6c-200">Példa:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-200">For example:</span></span>

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

<span data-ttu-id="8bb6c-201">`TopologyContext`tooget hello topológia környezetben, akkor a leghasznosabb, összetevők több párhuzamosság együtt kerül.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-201">`TopologyContext` is provided tooget hello topology context, it is most useful for components with multiple parallelism.</span></span> <span data-ttu-id="8bb6c-202">Például:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-202">Here is an example:</span></span>

    //demo how tooget TopologyContext info
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

### <a name="dynamic-part"></a><span data-ttu-id="8bb6c-203">Dinamikus részében szerepel</span><span class="sxs-lookup"><span data-stu-id="8bb6c-203">Dynamic Part</span></span>
<span data-ttu-id="8bb6c-204">következő illesztők hello vannak vonatkozó tooa egyes adatkörnyezet példányához.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-204">hello following interfaces are pertinent tooa certain Context instance.</span></span> <span data-ttu-id="8bb6c-205">hello környezetben példány SCP.NET platform által létrehozott, az átadott toohello felhasználói kód:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-205">hello Context instance is created by SCP.NET platform and passed toohello user code:</span></span>

    // Declare hello Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple toodefault stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple toohello specific stream.
    public abstract void Emit(string streamId, List<object> values);  

<span data-ttu-id="8bb6c-206">Nem tranzakciós spout nyugtázási támogatása a következő metódus hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-206">For non-transactional spout supporting ack, hello following method is provided:</span></span>

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

<span data-ttu-id="8bb6c-207">A nem tranzakciós bolt nyugtázási támogató, legyen, vagy ha kifejezetten `Ack()` vagy `Fail()` hello kapott rekordot.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-207">For non-transactional bolt supporting ack, it should explicitly `Ack()` or `Fail()` hello tuple it received.</span></span> <span data-ttu-id="8bb6c-208">És kibocsátó új rekordot, amikor meg kell adnia hello horgonyok hello új rekord.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-208">And when emitting new tuple, it must also specify hello anchors of hello new tuple.</span></span> <span data-ttu-id="8bb6c-209">a következő módszerek hello vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-209">hello following methods are provided.</span></span>

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a><span data-ttu-id="8bb6c-210">Állapottárolója</span><span class="sxs-lookup"><span data-stu-id="8bb6c-210">StateStore</span></span>
<span data-ttu-id="8bb6c-211">`StateStore`metaadatok szolgáltatások, a monoton sorrend létrehozása és a várakozási szabad koordinációs biztosít.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-211">`StateStore` provides metadata services, monotonic sequence generation, and wait-free coordination.</span></span> <span data-ttu-id="8bb6c-212">Elosztott feldolgozási magasabb szintű absztrakciók építhetők `StateStore`, beleértve az elosztott zárolásokat, elosztott várólisták, korlátok és tranzakciós szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-212">Higher-level distributed concurrency abstractions can be built on `StateStore`, including distributed locks, distributed queues, barriers, and transaction services.</span></span>

<span data-ttu-id="8bb6c-213">Szolgáltatáskapcsolódási pont alkalmazások is használhatnak hello `State` objektum toopersist ZooKeeper, különösen a tranzakciós topológia egyes információk.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-213">SCP applications may use hello `State` object toopersist some information in ZooKeeper, especially for transactional topology.</span></span> <span data-ttu-id="8bb6c-214">Ennek a tranzakciós spout összeomlik, és indítsa újra, ha hello szükséges adatok lekérését ZooKeeper, és indítsa újra a hello folyamat során.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-214">Doing so, if transactional spout crashes and restart, it can retrieve hello necessary information from ZooKeeper and restart hello pipeline.</span></span>

<span data-ttu-id="8bb6c-215">Hello `StateStore` főként objektumnak ezen módszerek:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-215">hello `StateStore` object mainly has these methods:</span></span>

    /// <summary>
    /// Static method tooretrieve a state store of hello given path and connStr 
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
    /// Get all hello States in hello StateStore
    /// </summary>
    /// <returns>All hello States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all hello committed states
    /// </summary>
    /// <returns>Registries contain hello Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all hello Aborted State in hello StateStore
    /// </summary>
    /// <returns>Registries contain hello Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of hello State</typeparam>
    public State GetState(long stateId)

<span data-ttu-id="8bb6c-216">Hello `State` főként objektumnak ezen módszerek:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-216">hello `State` object mainly has these methods:</span></span>

    /// <summary>
    /// Set hello status of hello state object toocommit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set hello status of hello state object tooabort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under hello give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get hello attribute value associated with hello given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

<span data-ttu-id="8bb6c-217">A hello `Commit()` metódust, ha simpleMode tootrue, egyszerűen törli az ZNode ZooKeeper a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-217">For hello `Commit()` method, when simpleMode is set tootrue, it will simply delete hello corresponding ZNode in ZooKeeper.</span></span> <span data-ttu-id="8bb6c-218">Ellenkező esetben törölni fogja hello aktuális ZNode, és egy új csomópont hozzáadása a hello LEKÖTÖTT\_elérési ÚTJA.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-218">Otherwise, it will delete hello current ZNode, and adding a new node in hello COMMITTED\_PATH.</span></span>

### <a name="scpruntime"></a><span data-ttu-id="8bb6c-219">SCPRuntime</span><span class="sxs-lookup"><span data-stu-id="8bb6c-219">SCPRuntime</span></span>
<span data-ttu-id="8bb6c-220">SCPRuntime hello a következő két módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-220">SCPRuntime provides hello following two methods.</span></span>

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

<span data-ttu-id="8bb6c-221">`Initialize()`van használt tooinitialize hello SCP futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-221">`Initialize()` is used tooinitialize hello SCP runtime environment.</span></span> <span data-ttu-id="8bb6c-222">Ezzel a módszerrel hello C\# folyamat toohello Java oldalon, és lekérdezi-konfigurációs paraméterek és topológia környezet csatlakozni fognak.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-222">In this method, hello C\# process will connect toohello Java side, and gets configuration parameters and topology context.</span></span>

<span data-ttu-id="8bb6c-223">`LaunchPlugin()`hurok feldolgozása használt tookick üdvözlőüzenetére ki.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-223">`LaunchPlugin()` is used tookick off hello message processing loop.</span></span> <span data-ttu-id="8bb6c-224">Ez a ciklus a hello C\# beépülő modul üzenetek űrlap Java ügyféloldali (beleértve a rekordokat és a vezérlő jeleket) kap, és majd folyamat köszönőüzenetei, lehet, hogy a hello felület metódus hívásakor adja meg az hello felhasználói kód.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-224">In this loop, hello C\# plugin will receive messages form Java side (including tuples and control signals), and then process hello messages, perhaps calling hello interface method provide by hello user code.</span></span> <span data-ttu-id="8bb6c-225">hello metódus bemeneti paramétere `LaunchPlugin()` van olyan delegált esetén, amely ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt felületet megvalósító objektum adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-225">hello input parameter for method `LaunchPlugin()` is a delegate that can return an object that implement ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt interface.</span></span>

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

<span data-ttu-id="8bb6c-226">A ISCPBatchBolt, beszerezheti a Microsoft `StormTxAttempt` a `parms`, és ezzel toojudge hogy megismételt kísérlet-e.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-226">For ISCPBatchBolt, we can get `StormTxAttempt` from `parms`, and use it toojudge whether it is a replayed attempt.</span></span> <span data-ttu-id="8bb6c-227">Ez általában hello véglegesítési bolt szabályonkénti, és azt mutatják be hello `HelloWorldTx` példa.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-227">This is usually done at hello commit bolt, and it is demonstrated in hello `HelloWorldTx` example.</span></span>

<span data-ttu-id="8bb6c-228">Általánosságban véve a hello SCP beépülő modulok Itt két módban futhat:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-228">Generally speaking, hello SCP plugins may run in two modes here:</span></span>

1. <span data-ttu-id="8bb6c-229">Helyi tesztelése mód: Ebben a módban hello SCP beépülő modulok (hello C\# felhasználói kód) belül a Visual Studio hello fejlesztési fázis során futtassa.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-229">Local Test Mode: In this mode, hello SCP plugins (hello C\# user code) run inside Visual Studio during hello development phase.</span></span> <span data-ttu-id="8bb6c-230">`LocalContext`Ebben a módban, ami metódus tooserialize kibocsátott hello rekordokat toolocal fájlokat, és olvasási toomemory újra használható.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-230">`LocalContext` can be used in this mode, which provides method tooserialize hello emitted tuples toolocal files, and read them back toomemory.</span></span>
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. <span data-ttu-id="8bb6c-231">Normál mód: Ebben a módban hello SCP beépülő modulok indítja storm java folyamat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-231">Regular Mode: In this mode, hello SCP plugins are launched by storm java process.</span></span>
   
    <span data-ttu-id="8bb6c-232">Íme egy példa SCP beépülő modul megnyitása:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-232">Here is an example of launching SCP plugin:</span></span>
   
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
            /* Setting hello environment variable here can change hello log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a><span data-ttu-id="8bb6c-233">Topológia nyelv</span><span class="sxs-lookup"><span data-stu-id="8bb6c-233">Topology Specification Language</span></span>
<span data-ttu-id="8bb6c-234">SCP topológia meghatározása az adott nyelvhez és SCP topológiák konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-234">SCP Topology Specification is a domain specific language for describing and configuring SCP topologies.</span></span> <span data-ttu-id="8bb6c-235">A Storm Clojure DSL alapul (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) és az SCP bővített.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-235">It is based on Storm’s Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) and is extended by SCP.</span></span>

<span data-ttu-id="8bb6c-236">Topológia specifikációk küldheti el, közvetlenül keresztül hello végrehajtásra fürt toostorm ***runspec*** parancsot.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-236">Topology specifications can be submitted directly toostorm cluster for execution via hello ***runspec*** command.</span></span>

<span data-ttu-id="8bb6c-237">SCP.NET kövesse funkciók toodefine hello tranzakciós topológia rendelkezik fel:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-237">SCP.NET has add follow functions toodefine hello Transactional Topology:</span></span>

| <span data-ttu-id="8bb6c-238">**Új funkciók**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-238">**New Functions**</span></span> | <span data-ttu-id="8bb6c-239">**Paraméterek**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-239">**Parameters**</span></span> | <span data-ttu-id="8bb6c-240">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-240">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8bb6c-241">**Tx-topolopy**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-241">**tx-topolopy**</span></span> |<span data-ttu-id="8bb6c-242">topológia-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-242">topology-name</span></span><br /><span data-ttu-id="8bb6c-243">spout-leképezés</span><span class="sxs-lookup"><span data-stu-id="8bb6c-243">spout-map</span></span><br /><span data-ttu-id="8bb6c-244">bolt-leképezés</span><span class="sxs-lookup"><span data-stu-id="8bb6c-244">bolt-map</span></span> |<span data-ttu-id="8bb6c-245">Adja meg a tranzakciós topológia hello topológia nevű &nbsp;spoutok definition térkép és hello boltokhoz definition térkép</span><span class="sxs-lookup"><span data-stu-id="8bb6c-245">Define a transactional topology with hello topology name, &nbsp;spouts definition map and hello bolts definition map</span></span> |
| <span data-ttu-id="8bb6c-246">**SCP-tx-spout**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-246">**scp-tx-spout**</span></span> |<span data-ttu-id="8bb6c-247">Exec-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-247">exec-name</span></span><br /><span data-ttu-id="8bb6c-248">argumentum</span><span class="sxs-lookup"><span data-stu-id="8bb6c-248">args</span></span><br /><span data-ttu-id="8bb6c-249">Mezők</span><span class="sxs-lookup"><span data-stu-id="8bb6c-249">fields</span></span> |<span data-ttu-id="8bb6c-250">Adja meg a tranzakciós spout.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-250">Define a transactional spout.</span></span> <span data-ttu-id="8bb6c-251">Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-251">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="8bb6c-252">Hello ***mezők*** hello kimeneti mezők a spout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-252">hello ***fields*** is hello Output Fields for spout</span></span> |
| <span data-ttu-id="8bb6c-253">**SCP-tx-batch-bolt**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-253">**scp-tx-batch-bolt**</span></span> |<span data-ttu-id="8bb6c-254">Exec-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-254">exec-name</span></span><br /><span data-ttu-id="8bb6c-255">argumentum</span><span class="sxs-lookup"><span data-stu-id="8bb6c-255">args</span></span><br /><span data-ttu-id="8bb6c-256">Mezők</span><span class="sxs-lookup"><span data-stu-id="8bb6c-256">fields</span></span> |<span data-ttu-id="8bb6c-257">Adja meg egy olyan tranzakciós kötegben Bolthoz.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-257">Define a transactional Batch Bolt.</span></span> <span data-ttu-id="8bb6c-258">Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum.***</span><span class="sxs-lookup"><span data-stu-id="8bb6c-258">It will run hello application with ***exec-name*** using ***args.***</span></span><br /><br /><span data-ttu-id="8bb6c-259">hello mezők hello kimeneti mezők a bolt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-259">hello Fields is hello Output Fields for bolt.</span></span> |
| <span data-ttu-id="8bb6c-260">**SCP-tx-commit-bolt**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-260">**scp-tx-commit-bolt**</span></span> |<span data-ttu-id="8bb6c-261">Exec-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-261">exec-name</span></span><br /><span data-ttu-id="8bb6c-262">argumentum</span><span class="sxs-lookup"><span data-stu-id="8bb6c-262">args</span></span><br /><span data-ttu-id="8bb6c-263">Mezők</span><span class="sxs-lookup"><span data-stu-id="8bb6c-263">fields</span></span> |<span data-ttu-id="8bb6c-264">Adja meg egy olyan tranzakciós Committer Bolthoz.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-264">Define a transactional Committer Bolt.</span></span> <span data-ttu-id="8bb6c-265">Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-265">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="8bb6c-266">Hello ***mezők*** hello kimeneti mezők a bolt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-266">hello ***fields*** is hello Output Fields for bolt</span></span> |
| <span data-ttu-id="8bb6c-267">**nontx-topolopy**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-267">**nontx-topolopy**</span></span> |<span data-ttu-id="8bb6c-268">topológia-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-268">topology-name</span></span><br /><span data-ttu-id="8bb6c-269">spout-leképezés</span><span class="sxs-lookup"><span data-stu-id="8bb6c-269">spout-map</span></span><br /><span data-ttu-id="8bb6c-270">bolt-leképezés</span><span class="sxs-lookup"><span data-stu-id="8bb6c-270">bolt-map</span></span> |<span data-ttu-id="8bb6c-271">Adja meg a nem tranzakciós topológia hello topológia nevű&nbsp; spoutok definition térkép és hello boltokhoz definition térkép</span><span class="sxs-lookup"><span data-stu-id="8bb6c-271">Define a nontransactional topology with hello topology name,&nbsp; spouts definition map and hello bolts definition map</span></span> |
| <span data-ttu-id="8bb6c-272">**SCP-spout**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-272">**scp-spout**</span></span> |<span data-ttu-id="8bb6c-273">Exec-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-273">exec-name</span></span><br /><span data-ttu-id="8bb6c-274">argumentum</span><span class="sxs-lookup"><span data-stu-id="8bb6c-274">args</span></span><br /><span data-ttu-id="8bb6c-275">Mezők</span><span class="sxs-lookup"><span data-stu-id="8bb6c-275">fields</span></span><br /><span data-ttu-id="8bb6c-276">paraméterek</span><span class="sxs-lookup"><span data-stu-id="8bb6c-276">parameters</span></span> |<span data-ttu-id="8bb6c-277">Adja meg egy nem tranzakciós spout.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-277">Define a nontransactional spout.</span></span> <span data-ttu-id="8bb6c-278">Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-278">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="8bb6c-279">Hello ***mezők*** hello kimeneti mezők a spout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-279">hello ***fields*** is hello Output Fields for spout</span></span><br /><br /><span data-ttu-id="8bb6c-280">Hello ***paraméterek*** nem kötelező, toospecify használja bizonyos paraméterek, például "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="8bb6c-280">hello ***parameters*** is optional, using it toospecify some parameters such as "nontransactional.ack.enabled".</span></span> |
| <span data-ttu-id="8bb6c-281">**SCP-bolt**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-281">**scp-bolt**</span></span> |<span data-ttu-id="8bb6c-282">Exec-név</span><span class="sxs-lookup"><span data-stu-id="8bb6c-282">exec-name</span></span><br /><span data-ttu-id="8bb6c-283">argumentum</span><span class="sxs-lookup"><span data-stu-id="8bb6c-283">args</span></span><br /><span data-ttu-id="8bb6c-284">Mezők</span><span class="sxs-lookup"><span data-stu-id="8bb6c-284">fields</span></span><br /><span data-ttu-id="8bb6c-285">paraméterek</span><span class="sxs-lookup"><span data-stu-id="8bb6c-285">parameters</span></span> |<span data-ttu-id="8bb6c-286">Adja meg egy nem tranzakciós Bolt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-286">Define a nontransactional Bolt.</span></span> <span data-ttu-id="8bb6c-287">Akkor fog futni hello alkalmazás ***exec-name*** használatával ***argumentum***.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-287">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="8bb6c-288">Hello ***mezők*** hello kimeneti mezők a bolt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-288">hello ***fields*** is hello Output Fields for bolt</span></span><br /><br /><span data-ttu-id="8bb6c-289">Hello ***paraméterek*** nem kötelező, toospecify használja bizonyos paraméterek, például "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="8bb6c-289">hello ***parameters*** is optional, using it toospecify some parameters such as "nontransactional.ack.enabled".</span></span> |

<span data-ttu-id="8bb6c-290">SCP.NET rendelkezik hajtsa végre a kulcsok definiálva szavak:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-290">SCP.NET has follow keys words defined:</span></span>

| <span data-ttu-id="8bb6c-291">**Kulcs szavakat**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-291">**Key Words**</span></span> | <span data-ttu-id="8bb6c-292">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-292">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="8bb6c-293">**: neve**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-293">**:name**</span></span> |<span data-ttu-id="8bb6c-294">Hello topológia nevének megadása</span><span class="sxs-lookup"><span data-stu-id="8bb6c-294">Define hello Topology Name</span></span> |
| <span data-ttu-id="8bb6c-295">**: topológia**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-295">**:topology**</span></span> |<span data-ttu-id="8bb6c-296">Adja meg hello topológiáját hello funkciók felett, és azokat a build.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-296">Define hello Topology using hello above functions and build in ones.</span></span> |
| <span data-ttu-id="8bb6c-297">**: p**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-297">**:p**</span></span> |<span data-ttu-id="8bb6c-298">Adja meg az egyes spout vagy bolt hello párhuzamossági mutató.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-298">Define hello parallelism hint for each spout or bolt.</span></span> |
| <span data-ttu-id="8bb6c-299">**: config**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-299">**:config**</span></span> |<span data-ttu-id="8bb6c-300">Adja meg konfigurálása paraméter vagy frissítés hello meglévőket</span><span class="sxs-lookup"><span data-stu-id="8bb6c-300">Define configure parameter or update hello existing ones</span></span> |
| <span data-ttu-id="8bb6c-301">**: séma**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-301">**:schema**</span></span> |<span data-ttu-id="8bb6c-302">Adja meg a hello séma az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-302">Define hello Schema of Stream.</span></span> |

<span data-ttu-id="8bb6c-303">És a gyakran használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-303">And frequently-used parameters:</span></span>

| <span data-ttu-id="8bb6c-304">**A paraméter**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-304">**Parameter**</span></span> | <span data-ttu-id="8bb6c-305">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-305">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="8bb6c-306">**"plugin.name"**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-306">**"plugin.name"**</span></span> |<span data-ttu-id="8bb6c-307">hello C# beépülő modul exe-fájl neve</span><span class="sxs-lookup"><span data-stu-id="8bb6c-307">exe file name of hello C# plugin</span></span> |
| <span data-ttu-id="8bb6c-308">**"plugin.args"**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-308">**"plugin.args"**</span></span> |<span data-ttu-id="8bb6c-309">beépülő modul argumentum</span><span class="sxs-lookup"><span data-stu-id="8bb6c-309">plugin args</span></span> |
| <span data-ttu-id="8bb6c-310">**"output.schema"**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-310">**"output.schema"**</span></span> |<span data-ttu-id="8bb6c-311">Kimeneti séma</span><span class="sxs-lookup"><span data-stu-id="8bb6c-311">Output schema</span></span> |
| <span data-ttu-id="8bb6c-312">**"nontransactional.ack.enabled"**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-312">**"nontransactional.ack.enabled"**</span></span> |<span data-ttu-id="8bb6c-313">Nyugtázási engedélyezve van-e a nem tranzakciós topológia</span><span class="sxs-lookup"><span data-stu-id="8bb6c-313">Whether ack is enabled for nontransactional topology</span></span> |

<span data-ttu-id="8bb6c-314">hello runspec parancs telepíti hello bits együtt, hello használata hasonlít:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-314">hello runspec command will be deployed together with hello bits, hello usage is like:</span></span>

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

<span data-ttu-id="8bb6c-315">Hello ***erőforrás-dir*** paraméter nem kötelező, toospecify van szüksége, ha azt szeretné, hogy egy C tooplug\# az alkalmazás és a könyvtár hello alkalmazás, hello függőségeket és konfigurációk fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-315">hello ***resource-dir*** parameter is optional, you need toospecify it when you want tooplug a C\# application, and this directory will contain hello application, hello dependencies and configurations.</span></span>

<span data-ttu-id="8bb6c-316">Hello ***classpath*** paramétert is nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-316">hello ***classpath*** parameter is also optional.</span></span> <span data-ttu-id="8bb6c-317">Ha hello fájlmegadásában fájl tartalmaz Java Spout vagy Bolt használt toospecify hello Java classpath.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-317">It is used toospecify hello Java classpath if hello spec file contains Java Spout or Bolt.</span></span>

## <a name="miscellaneous-features"></a><span data-ttu-id="8bb6c-318">Egyéb szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="8bb6c-318">Miscellaneous Features</span></span>
### <a name="input-and-output-schema-declaration"></a><span data-ttu-id="8bb6c-319">Bemeneti és kimeneti séma nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="8bb6c-319">Input and Output Schema Declaration</span></span>
<span data-ttu-id="8bb6c-320">hello felhasználó el tudná küldeni c. rekordot\# feldolgozni, hello platform kell tooserialize hello rekordot a byte [], átviteli tooJava oldalon, és a Storm a rekord toohello megcélzott át.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-320">hello user can emit tuple in C\# process, hello platform needs tooserialize hello tuple into byte[], transfer tooJava side, and Storm will transfer this tuple toohello targets.</span></span> <span data-ttu-id="8bb6c-321">Eközben alsóbb rétegbeli összetevők C hello\# folyamat rekordot kap vissza java oldaláról, és platform toohello eredeti típusok konvertálja, ezeket a műveleteket rejtettek hello Platform.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-321">Meanwhile in downstream component, hello C\# process will receive tuple back from java side, and convert it toohello original types by platform, all these operations are hidden by hello Platform.</span></span>

<span data-ttu-id="8bb6c-322">toosupport hello szerializálás és a deszerializálás, a felhasználói kód toodeclare hello sémája hello bemenetekhez és kimenetekhez van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-322">toosupport hello serialization and deserialization, user code needs toodeclare hello schema of hello inputs and outputs.</span></span>

<span data-ttu-id="8bb6c-323">hello bemeneti/kimeneti adatfolyam séma van definiálva, dictionary, hello kulcs hello StreamId hello értéke pedig hello oszlopok hello típusú.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-323">hello input/output stream schema is defined as a dictionary, hello key is hello StreamId and hello value is hello Types of hello columns.</span></span> <span data-ttu-id="8bb6c-324">hello összetevő rendelkezhet több adatfolyamok deklarálva.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-324">hello component can have multi-streams declared.</span></span>

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


<span data-ttu-id="8bb6c-325">Környezeti objektumot, a következő API hozzáadott hello vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-325">In Context object, we have hello following API added:</span></span>

    public void DeclareComponentSchema(ComponentStreamSchema schema)

<span data-ttu-id="8bb6c-326">Biztosítsa, hogy a felhasználói kód, kibocsátott hello rekordokat veszi fel, hogy az adatfolyam definiált hello séma, vagy hello rendszer kivételhibát futásidejű kivételt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-326">User code must make sure hello tuples emitted obey hello schema defined for that stream, or hello system will throw a runtime exception.</span></span>

### <a name="multi-stream-support"></a><span data-ttu-id="8bb6c-327">Több adatfolyam-támogatás</span><span class="sxs-lookup"><span data-stu-id="8bb6c-327">Multi-Stream Support</span></span>
<span data-ttu-id="8bb6c-328">Szolgáltatáskapcsolódási pont által támogatott felhasználó code tooemit, vagy több különböző adatfolyam: hello fogadnak azonos idő.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-328">SCP supports user code tooemit or receive from multiple distinct streams at hello same time.</span></span> <span data-ttu-id="8bb6c-329">hello kibocsátása metódus egy választható adatfolyam-azonosító paramétert fogad, mert a hello környezeti objektumot a hello támogatási tükrözi.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-329">hello support reflects in hello Context object as hello Emit method takes an optional stream ID parameter.</span></span>

<span data-ttu-id="8bb6c-330">Két módszer a hello SCP.NET környezeti objektumot hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-330">Two methods in hello SCP.NET Context object have been added.</span></span> <span data-ttu-id="8bb6c-331">Használt tooemit rekordot, vagy rekordokat toospecify StreamId.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-331">They are used tooemit Tuple or Tuples toospecify StreamId.</span></span> <span data-ttu-id="8bb6c-332">hello StreamId: karakterlánc, és mindkét c. konzisztens kell toobe\# és topológia meghatározása Spec hello.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-332">hello StreamId is a string and it needs toobe consistent in both C\# and hello Topology Definition Spec.</span></span>

        /* Emit tuple toohello specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

<span data-ttu-id="8bb6c-333">hello kibocsátás tooa nem létező adatfolyam futásidejű kivételek miatt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-333">hello emitting tooa non-existing stream will cause runtime exceptions.</span></span>

### <a name="fields-grouping"></a><span data-ttu-id="8bb6c-334">Mezők csoportosítás</span><span class="sxs-lookup"><span data-stu-id="8bb6c-334">Fields Grouping</span></span>
<span data-ttu-id="8bb6c-335">beépített mezők csoportosításának Strom nem működik megfelelően a SCP.NET hello.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-335">hello build-in Fields Grouping in Strom is not working properly in SCP.NET.</span></span> <span data-ttu-id="8bb6c-336">Hello Java Proxy oldalon, a minden hello mezők adattípusok ténylegesen byte [], és csoportosítási hello mezők hello byte [] objektum kivonatoló kódot tooperform hello csoportosítási használja.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-336">On hello Java Proxy side, all hello fields data types are actually byte[], and hello fields grouping uses hello byte[] object hash code tooperform hello grouping.</span></span> <span data-ttu-id="8bb6c-337">hello byte [] objektum kivonatkód az hello cím az objektum a memóriában.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-337">hello byte[] object hash code is hello address of this object in memory.</span></span> <span data-ttu-id="8bb6c-338">Így hello csoportosítás nem megfelelő, a két bájthoz [] objektumokat, hogy megosztás hello azonos tartalmakat, de nem hello ugyanazt a címet.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-338">So hello grouping will be wrong for two byte[] objects that share hello same content but not hello same address.</span></span>

<span data-ttu-id="8bb6c-339">SCP.NET hozzáadja egy testreszabott csoportosítási módszer, majd hello byte [] toodo hello csoportosítási hello tartalmának fogja használni.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-339">SCP.NET adds a customized grouping method, and it will use hello content of hello byte[] toodo hello grouping.</span></span> <span data-ttu-id="8bb6c-340">A **SPEC** fájl, hello szintaxis hasonlít:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-340">In **SPEC** file, hello syntax is like:</span></span>

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


<span data-ttu-id="8bb6c-341">itt</span><span class="sxs-lookup"><span data-stu-id="8bb6c-341">Here,</span></span>

1. <span data-ttu-id="8bb6c-342">"scp-mező-csoport": "Testre szabott mező csoportosítási szolgáltatáskapcsolódási pont által megvalósított".</span><span class="sxs-lookup"><span data-stu-id="8bb6c-342">"scp-field-group" means "Customized field grouping implemented by SCP".</span></span>
2. <span data-ttu-id="8bb6c-343">": tx"vagy": nem tx" azt jelenti, hogy tranzakciós topológia esetén.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-343">":tx" or ":non-tx" means if it’s transactional topology.</span></span> <span data-ttu-id="8bb6c-344">Mivel hello index kezdődően eltér a tx és nem tx topológiák kell ezeket az információkat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-344">We need this information since hello starting index is different in tx vs. non-tx topologies.</span></span>
3. <span data-ttu-id="8bb6c-345">[0,1] azt jelenti, hogy a hashset osztály mező azonosítókat, 0-tól kezdődően.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-345">[0,1] means a hashset of field Ids, starting from 0.</span></span>

### <a name="hybrid-topology"></a><span data-ttu-id="8bb6c-346">Hibrid topológia</span><span class="sxs-lookup"><span data-stu-id="8bb6c-346">Hybrid topology</span></span>
<span data-ttu-id="8bb6c-347">hello natív Storm Java nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-347">hello native Storm is written in Java.</span></span> <span data-ttu-id="8bb6c-348">És SCP.Net továbbfejlesztett azt tooenable a vámügyi toowrite C\# code toohandle az üzleti logikát.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-348">And SCP.Net has enhanced it tooenable our customs toowrite C\# code toohandle their business logic.</span></span> <span data-ttu-id="8bb6c-349">De is támogatja a hibrid topológiák, amely tartalmaz, nem csak a C\# spoutokkal kapcsolatban/boltokhoz, emellett pedig Java Spout/Boltokhoz.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-349">But we also support hybrid topologies, which contains not only C\# spouts/bolts, but also Java Spout/Bolts.</span></span>

### <a name="specify-java-spoutbolt-in-spec-file"></a><span data-ttu-id="8bb6c-350">Adja meg a Java Spout vagy Bolt fájlmegadásában fájlban</span><span class="sxs-lookup"><span data-stu-id="8bb6c-350">Specify Java Spout/Bolt in spec file</span></span>
<span data-ttu-id="8bb6c-351">Speciális fájlban használt toospecify Java Spoutok és Boltokhoz is lehet "scp-spout" és "scp-bolt", például:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-351">In spec file, "scp-spout" and "scp-bolt" can also be used toospecify Java Spouts and Bolts, here is an example:</span></span>

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

<span data-ttu-id="8bb6c-352">Itt `microsoft.scp.example.HybridTopology.Generator` hello hello Java Spout osztály neve.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-352">Here `microsoft.scp.example.HybridTopology.Generator` is hello name of hello Java Spout class.</span></span>

### <a name="specify-java-classpath-in-runspec-command"></a><span data-ttu-id="8bb6c-353">Adja meg a Java Classpath runSpec parancs</span><span class="sxs-lookup"><span data-stu-id="8bb6c-353">Specify Java Classpath in runSpec Command</span></span>
<span data-ttu-id="8bb6c-354">Ha azt szeretné, hogy a Java Spoutok vagy Boltokhoz tartalmazó toosubmit topológia, Java Spoutok toofirst fordítási hello vagy Boltokhoz szükséges, és hello Jar fájlok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-354">If you want toosubmit topology containing Java Spouts or Bolts, you need toofirst compile hello Java Spouts or Bolts and get hello Jar files.</span></span> <span data-ttu-id="8bb6c-355">Majd adja meg, amely tartalmazza a hello Jar fájlok topológia elküldésekor hello java classpath.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-355">Then you should specify hello java classpath that contains hello Jar files when submitting topology.</span></span> <span data-ttu-id="8bb6c-356">Például:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-356">Here is an example:</span></span>

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

<span data-ttu-id="8bb6c-357">Itt **példák\\HybridTopology\\java\\cél\\**  van hello mappa hello Java Spout vagy Bolt Jar-fájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-357">Here **examples\\HybridTopology\\java\\target\\** is hello folder containing hello Java Spout/Bolt Jar file.</span></span>

### <a name="serialization-and-deserialization-between-java-and-c"></a><span data-ttu-id="8bb6c-358">Szerializálás és a deszerializálás között a Java és C\\</span><span class="sxs-lookup"><span data-stu-id="8bb6c-358">Serialization and Deserialization between Java and C\\</span></span>
<span data-ttu-id="8bb6c-359">A szolgáltatáskapcsolódási pont a következő Java és a C\# oldalán.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-359">Our SCP component includes Java side and C\# side.</span></span> <span data-ttu-id="8bb6c-360">A sorrend toointeract a natív Java spoutokkal kapcsolatban/Boltokhoz, szerializálással/Deszerializálással el kell végezni között a Java és a C\# oldalán, a következő diagram hello ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-360">In order toointeract with native Java Spouts/Bolts, Serialization/Deserialization must be carried out between Java side and C\# side, as illustrated in hello following graph.</span></span>

![java-összetevő tooJava összetevő küldése tooSCP összetevő küldése ábrája](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. <span data-ttu-id="8bb6c-362">**A Java és a deszerializálás c. szerializálási\# oldal**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-362">**Serialization in Java side and Deserialization in C\# side**</span></span>
   
   <span data-ttu-id="8bb6c-363">Először igazolnia implementálásához alapértelmezett szerializálás Java oldal és a deszerializálás c.\# oldalán.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-363">First we provide default implementation for serialization in Java side and deserialization in C\# side.</span></span> <span data-ttu-id="8bb6c-364">hello szerializálási metódus a Java ügyféloldali speciális fájl adható meg:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-364">hello serialization method in Java side can be specified in SPEC file:</span></span>
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   <span data-ttu-id="8bb6c-365">Deszerializálási metódus c. hello\# oldalon kell megadni C\# felhasználói kód:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-365">hello deserialization method in C\# side should be specified in C\# user code:</span></span>
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   <span data-ttu-id="8bb6c-366">Az alapértelmezett implementációja kezelnie kell a legtöbb esetben, ha hello adattípus nem túl összetett.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-366">This default implementation should handle most cases if hello data type is not too complex.</span></span> <span data-ttu-id="8bb6c-367">Bizonyos esetekben vagy mert hello felhasználói adattípusnak: túl összetett, vagy mert hello az alapértelmezett implementációja teljesítménye nem éri el hello felhasználói követelményeket, a beépülő modul felhasználó is a saját megvalósítási.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-367">For certain cases, either because hello user data type is too complex, or because hello performance of our default implementation does not meet hello user's requirement, user can plug-in their own implementation.</span></span>
   
   <span data-ttu-id="8bb6c-368">hello szerializálni felület java nyelven ügyféloldali típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-368">hello serialize interface in java side is defined as:</span></span>
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   <span data-ttu-id="8bb6c-369">hello deszerializálni C felülettel\# ügyféloldali típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-369">hello deserialize interface in C\# side is defined as:</span></span>
   
   <span data-ttu-id="8bb6c-370">Nyilvános csatoló ICustomizedInteropCSharpDeserializer</span><span class="sxs-lookup"><span data-stu-id="8bb6c-370">public interface ICustomizedInteropCSharpDeserializer</span></span>
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. <span data-ttu-id="8bb6c-371">**Szerializálási c.\# ügyféloldali és a Java párhuzamosan a deszerializálás**</span><span class="sxs-lookup"><span data-stu-id="8bb6c-371">**Serialization in C\# side and Deserialization in Java side side**</span></span>
   
   <span data-ttu-id="8bb6c-372">szerializálási metódus a C hello\# oldalon kell megadni C\# felhasználói kód:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-372">hello serialization method in C\# side should be specified in C\# user code:</span></span>
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   <span data-ttu-id="8bb6c-373">hello deszerializálása metódus Java oldal FÁJLMEGADÁSÁBAN fájlt kell megadni:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-373">hello Deserialization method in Java side should be specified in SPEC file:</span></span>
   
     <span data-ttu-id="8bb6c-374">(scp-spout</span><span class="sxs-lookup"><span data-stu-id="8bb6c-374">(scp-spout</span></span>
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   <span data-ttu-id="8bb6c-375">Itt "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" deszerializáló hello nevét, és "microsoft.scp.example.HybridTopology.Person" hello target osztály hello adatokat a rendszer deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-375">Here "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" is hello name of Deserializer, and "microsoft.scp.example.HybridTopology.Person" is hello target class hello data is deserialized to.</span></span>
   
   <span data-ttu-id="8bb6c-376">Felhasználó is beépülő modul a következőket teheti a saját C végrehajtásának\# szerializáló és Java deszerializáló.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-376">User can also plug-in their own implementation of C\# serializer and Java Deserializer.</span></span> <span data-ttu-id="8bb6c-377">Ez a C hello felület\# szerializáló:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-377">This is hello interface for C\# serializer:</span></span>
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   <span data-ttu-id="8bb6c-378">Ez az Java deszerializáló hello felület:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-378">This is hello interface for Java Deserializer:</span></span>
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a><span data-ttu-id="8bb6c-379">Szolgáltatáskapcsolódási pont a gazdagép módja</span><span class="sxs-lookup"><span data-stu-id="8bb6c-379">SCP Host Mode</span></span>
<span data-ttu-id="8bb6c-380">Ebben a módban a felhasználó lefordítani a kódok tooDLL, és használja a szolgáltatáskapcsolódási pont toosubmit topológia által biztosított SCPHost.exe.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-380">In this mode, user can compile their codes tooDLL, and use SCPHost.exe provided by SCP toosubmit topology.</span></span> <span data-ttu-id="8bb6c-381">hello fájlmegadásában fájl így néz ki:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-381">hello spec file looks like this:</span></span>

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

<span data-ttu-id="8bb6c-382">Itt `plugin.name` megadott `SCPHost.exe` SCP SDK által biztosított.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-382">Here, `plugin.name` is specified as `SCPHost.exe` provided by SCP SDK.</span></span> <span data-ttu-id="8bb6c-383">Pontosan három paramétert fogad, amely SCPHost.exe:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-383">SCPHost.exe which accepts exactly three parameters:</span></span>

1. <span data-ttu-id="8bb6c-384">hello először egy hello dll-fájl neve, amely `"HelloWorld.dll"` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-384">hello first one is hello DLL name, which is `"HelloWorld.dll"` in this example.</span></span>
2. <span data-ttu-id="8bb6c-385">hello második hello osztály neve, amely `"Scp.App.HelloWorld.Generator"` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-385">hello second one is hello Class name, which is `"Scp.App.HelloWorld.Generator"` in this example.</span></span>
3. <span data-ttu-id="8bb6c-386">hello harmadik egyik hello nyilvános statikus metódus, amely lehet meghívott tooget ISCPPlugin példányának nevét.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-386">hello third one is hello name of a public static method, which can be invoked tooget an instance of ISCPPlugin.</span></span>

<span data-ttu-id="8bb6c-387">Állomás üzemmódban a felhasználói kód, dll-fájl fordítása, és SCP platform által indított.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-387">In host mode, user code is compiled as DLL, and is invoked by SCP platform.</span></span> <span data-ttu-id="8bb6c-388">Szolgáltatáskapcsolódási pont platform, teljes körű hozzáférést engedélyezzenek hello teljes feldolgozó logika kérheti le.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-388">So SCP platform can get full control of hello whole processing logic.</span></span> <span data-ttu-id="8bb6c-389">Ezért azt javasoljuk ügyfeleink toosubmit topológia SCP-t állomás üzemmódban óta hello fejlesztési élmény egyszerűsítése és velünk, valamint a későbbi kiadásban kapcsolása nagyobb rugalmasságot és jobban visszamenőleges kompatibilitás érdekében.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-389">So we recommend our customers toosubmit topology in SCP host mode since it can simplify hello development experience and bring us more flexibility and better backward compatibility for later release as well.</span></span>

## <a name="scp-programming-examples"></a><span data-ttu-id="8bb6c-390">Szolgáltatáskapcsolódási pont programozási példák</span><span class="sxs-lookup"><span data-stu-id="8bb6c-390">SCP Programming Examples</span></span>
### <a name="helloworld"></a><span data-ttu-id="8bb6c-391">HelloWorld</span><span class="sxs-lookup"><span data-stu-id="8bb6c-391">HelloWorld</span></span>
<span data-ttu-id="8bb6c-392">**HelloWorld** van egy nagyon egyszerű példa tooshow egy SCP.Net ízét.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-392">**HelloWorld** is a very simple example tooshow a taste of SCP.Net.</span></span> <span data-ttu-id="8bb6c-393">A nem tranzakciós topológia használ egy nevű spout **generátor**, és két boltokhoz nevű **elválasztó** és **számláló**.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-393">It uses a non-transactional topology, with a spout called **generator**, and two bolts called **splitter** and **counter**.</span></span> <span data-ttu-id="8bb6c-394">hello spout **generátor** fog véletlenszerűen generálja néhány mondatot, és ezeket a mondatok túl kibocsátás**elválasztó**.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-394">hello spout **generator** will randomly generate some sentences, and emit these sentences too**splitter**.</span></span> <span data-ttu-id="8bb6c-395">hello bolt **elválasztó** fog hello mondat toowords felosztása és ezeknek a szavaknak túl kibocsátás**számláló** bolt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-395">hello bolt **splitter** will split hello sentences toowords and emit these words too**counter** bolt.</span></span> <span data-ttu-id="8bb6c-396">hello bolt "számláló" szótár toorecord hello előfordulási számú használ minden szó.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-396">hello bolt "counter" uses a dictionary toorecord hello occurrence number of each word.</span></span>

<span data-ttu-id="8bb6c-397">Két fájlmegadásában fájlt **HelloWorld.spec** és **HelloWorld\_EnableAck.spec** ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-397">There are two spec files, **HelloWorld.spec** and **HelloWorld\_EnableAck.spec** for this example.</span></span> <span data-ttu-id="8bb6c-398">A hello C\# kódot, akkor talál, nyugtázási engedélyezve van-e hello pluginConf lekérésével Java oldaláról.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-398">In hello C\# code, it can find out whether ack is enabled by getting hello pluginConf from Java side.</span></span>

    /* demo how tooget pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

<span data-ttu-id="8bb6c-399">Hello spout Ha engedélyezve van a nyugtázási, dictionary használt toocache hello rekordokat, amelyek még nincsenek korrektúrák.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-399">In hello spout, if ack is enabled, a dictionary is used toocache hello tuples that have not been acked.</span></span> <span data-ttu-id="8bb6c-400">Ha Fail() nevezik, hello sikertelen rekordot fog bejegyzéseit meg kell ismételni:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-400">If Fail() is called, hello failed tuple will be replayed:</span></span>

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get hello cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay hello failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a><span data-ttu-id="8bb6c-401">HelloWorldTx</span><span class="sxs-lookup"><span data-stu-id="8bb6c-401">HelloWorldTx</span></span>
<span data-ttu-id="8bb6c-402">Hello **HelloWorldTx** példa bemutatja, hogyan tooimplement tranzakciós topológia.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-402">hello **HelloWorldTx** example demonstrates how tooimplement transactional topology.</span></span> <span data-ttu-id="8bb6c-403">Van-e egy spout nevű **generátor**, egy kötegelt boltokhoz nevű **részleges-count**, és egy véglegesítési bolt nevű **száma összeg**.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-403">It have one spout called **generator**, a batch bolts called **partial-count**, and a commit bolt called **count-sum**.</span></span> <span data-ttu-id="8bb6c-404">Van még három előre létrehozott txt-fájloknál: **DataSource0.txt**, **DataSource1.txt** és **DataSource2.txt**.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-404">There are also three pre-created txt files: **DataSource0.txt**, **DataSource1.txt** and **DataSource2.txt**.</span></span>

<span data-ttu-id="8bb6c-405">Az egyes tranzakciókra, hello spout **generátor** fog véletlenszerűen két fájlt választhat hello előre létrehozott, mindhárom fájlt, majd kiadni hello két fájl nevének toohello **részleges-count** bolt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-405">In each transaction, hello spout **generator** will randomly choose two files from hello pre-created three files, and emit hello two file names toohello **partial-count** bolt.</span></span> <span data-ttu-id="8bb6c-406">hello bolt **részleges-count** először beszerezni hello fájlnév kapott hello rekordot, majd nyissa meg hello fájl- és count hello szavak száma ebben a fájlban, és végül a hello word számú toohello kibocsátás **száma összeg**bolthoz.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-406">hello bolt **partial-count** will first get hello file name from hello received tuple, then open hello file and count hello number of words in this file, and finally emit hello word number toohello **count-sum** bolt.</span></span> <span data-ttu-id="8bb6c-407">Hello **száma összeg** bolt hello számuk azokat.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-407">hello **count-sum** bolt will summarize hello total count.</span></span>

<span data-ttu-id="8bb6c-408">tooachieve **pontosan egyszer** szemantikáját, hello véglegesítési bolt **száma összeg** toojudge kell, hogy egy megismételt tranzakció-e.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-408">tooachieve **exactly once** semantics, hello commit bolt **count-sum** need toojudge whether it is a replayed transaction.</span></span> <span data-ttu-id="8bb6c-409">Ebben a példában egy statikus tag változó rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-409">In this example, it has a static member variable:</span></span>

    public static long lastCommittedTxId = -1; 

<span data-ttu-id="8bb6c-410">ISCPBatchBolt példány létrehozásakor kap-e hello `txAttempt` bemeneti paraméterek közül:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-410">When an ISCPBatchBolt instance is created, it will get hello `txAttempt` from input parameters:</span></span>

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from hello input parms */
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

<span data-ttu-id="8bb6c-411">Ha `FinishBatch()` nevezik, hello `lastCommittedTxId` frissítés lesz, ha még nincs megismételt tranzakció.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-411">When `FinishBatch()` is called, hello `lastCommittedTxId` will be update if it is not a replayed transaction.</span></span>

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update hello toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a><span data-ttu-id="8bb6c-412">HybridTopology</span><span class="sxs-lookup"><span data-stu-id="8bb6c-412">HybridTopology</span></span>
<span data-ttu-id="8bb6c-413">Ez a topológia tartalmaz egy Java Spout és egy C\# Bolt.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-413">This topology contains a Java Spout and a C\# Bolt.</span></span> <span data-ttu-id="8bb6c-414">Hello alapértelmezett szerializálása és deszerializálása implementációja SCP platform által biztosított használ.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-414">It uses hello default serialization and deserialization implementation provided by SCP platform.</span></span> <span data-ttu-id="8bb6c-415">Kérjük, ref hello **HybridTopology.spec** a **példák\\HybridTopology** mappában hello fájlmegadásában fájlt, és **SubmitTopology.bat** arról, hogyan toospecify Java classpath.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-415">Please ref hello **HybridTopology.spec** in **examples\\HybridTopology** folder for hello spec file details, and **SubmitTopology.bat** for how toospecify Java classpath.</span></span>

### <a name="scphostdemo"></a><span data-ttu-id="8bb6c-416">SCPHostDemo</span><span class="sxs-lookup"><span data-stu-id="8bb6c-416">SCPHostDemo</span></span>
<span data-ttu-id="8bb6c-417">Ebben a példában az hello ugyanaz, mint a HelloWorld lényegében.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-417">This example is hello same as HelloWorld in essence.</span></span> <span data-ttu-id="8bb6c-418">hello csak különbség az, hogy hello felhasználói kód lefordított dll-fájl, és hello topológia beküldött SCPHost.exe használatával.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-418">hello only difference is that hello user code is compiled as DLL and hello topology is submitted by using SCPHost.exe.</span></span> <span data-ttu-id="8bb6c-419">Kérjük, ref hello szakasz "SCP-t állomás üzemmódban" részletes ismertetése.</span><span class="sxs-lookup"><span data-stu-id="8bb6c-419">Please ref hello section "SCP Host Mode" for more detailed explanation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bb6c-420">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8bb6c-420">Next Steps</span></span>
<span data-ttu-id="8bb6c-421">A szolgáltatáskapcsolódási pont használatával létrehozott Storm-topológiák, tekintse meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8bb6c-421">For examples of Storm topologies created using SCP, see hello following:</span></span>

* [<span data-ttu-id="8bb6c-422">Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="8bb6c-422">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="8bb6c-423">Az Azure Event Hubs a HDInsight alatt futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="8bb6c-423">Process events from Azure Event Hubs with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [<span data-ttu-id="8bb6c-424">Hozzon létre több adatfolyamokat a C# Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="8bb6c-424">Create multiple data streams in a C# Storm topology</span></span>](hdinsight-storm-twitter-trending.md)
* [<span data-ttu-id="8bb6c-425">A Power Bi toovisualize adat használata a Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="8bb6c-425">Use Power Bi toovisualize data from a Storm topology</span></span>](hdinsight-storm-power-bi-topology.md)
* [<span data-ttu-id="8bb6c-426">Az Event Hubs a HDInsight alatt futó Storm használatával vehicle érzékelő adatok feldolgozása</span><span class="sxs-lookup"><span data-stu-id="8bb6c-426">Process vehicle sensor data from Event Hubs using Storm on HDInsight</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [<span data-ttu-id="8bb6c-427">Kinyerési, átalakítási és betöltési (ETL) az Azure Event Hubs tooHBase</span><span class="sxs-lookup"><span data-stu-id="8bb6c-427">Extract, Transform, and Load (ETL) from Azure Event Hubs tooHBase</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [<span data-ttu-id="8bb6c-428">A HDInsight alatt futó Storm és HBase használatával összefüggésbe események</span><span class="sxs-lookup"><span data-stu-id="8bb6c-428">Correlate events using Storm and HBase on HDInsight</span></span>](hdinsight-storm-correlation-topology.md)

