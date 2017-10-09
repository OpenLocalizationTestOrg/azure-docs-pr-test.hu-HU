---
title: "a Storm és HBase a HDInsight időbeli aaaCorrelate események"
description: "Megtudhatja, hogyan toocorrelate események kiszolgálófarmban lévő különböző időpontokban, a HDInsight alatt futó Storm és HBase használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: f915eef77bbda5b02bfd02ad0b5a4923ff2f4f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a><span data-ttu-id="e203a-103">Az eseményeket, amelyek különböző időpontokban érkeznek összefüggéseket Storm és HBase használatával</span><span class="sxs-lookup"><span data-stu-id="e203a-103">Correlate events that arrive at different times using Storm and HBase</span></span>

<span data-ttu-id="e203a-104">Apache Storm egy állandó adattár használatával hozhatók adatok bejegyzéseit, amelyek a kiszolgálófarmban lévő különböző időpontokban.</span><span class="sxs-lookup"><span data-stu-id="e203a-104">By using a persistent data store with Apache Storm, you can correlate data entries that arrive at different times.</span></span> <span data-ttu-id="e203a-105">Például létrehozhatja, ha egy felhasználói munkamenet toocalculate bejelentkezési és kijelentkezési eseményeket hogyan hosszú hello munkamenet tartott.</span><span class="sxs-lookup"><span data-stu-id="e203a-105">For example, linking login and logout events for a user session toocalculate how long hello session lasted.</span></span>

<span data-ttu-id="e203a-106">Ebből a dokumentumból megismerheti, hogyan toocreate egy alapszintű C# Storm-topológia, amely nyomon követi a bejelentkezési és kijelentkezési eseményeit a felhasználói munkamenetekben, és hello munkamenet időtartama alatt hello számítja ki.</span><span class="sxs-lookup"><span data-stu-id="e203a-106">In this document, you learn how toocreate a basic C# Storm topology that tracks login and logout events for user sessions, and calculates hello duration of hello session.</span></span> <span data-ttu-id="e203a-107">hello topológia állandó adattárként HBase eszközt használja.</span><span class="sxs-lookup"><span data-stu-id="e203a-107">hello topology uses HBase as a persistent data store.</span></span> <span data-ttu-id="e203a-108">A HBase segítségével tooperform kötegelt lekérdezések hello előzményadatokat tooproduce további betekintést nyerjen a.</span><span class="sxs-lookup"><span data-stu-id="e203a-108">HBase also allows you tooperform batch queries on hello historical data tooproduce additional insights.</span></span> <span data-ttu-id="e203a-109">Például hány felhasználói munkamenetek lettek elindítva, vagy egy adott időszakra vonatkozóan befejeződött.</span><span class="sxs-lookup"><span data-stu-id="e203a-109">For example, how many user sessions were started or ended during a specific time period.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e203a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e203a-110">Prerequisites</span></span>

* <span data-ttu-id="e203a-111">A Visual Studio és hello HDInsight Visual Studio eszközök.</span><span class="sxs-lookup"><span data-stu-id="e203a-111">Visual Studio and hello HDInsight tools for Visual Studio.</span></span> <span data-ttu-id="e203a-112">További információkért lásd: [Ismerkedés hello HDInsight tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e203a-112">For more information, see [Get started using hello HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="e203a-113">A HDInsight alatt futó Apache Storm a fürt (Windows-alapú).</span><span class="sxs-lookup"><span data-stu-id="e203a-113">Apache Storm on HDInsight cluster (Windows-based).</span></span>

  > [!WARNING]
  > <span data-ttu-id="e203a-114">SCP.NET topológiák támogatottak a Linux-alapú Storm-fürtök létrehozása után 10/28/2016, hello HBase SDK elérhető 10/28/2016 .NET-csomag nem működik megfelelően a Linux-alapú HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="e203a-114">While SCP.NET topologies are supported on Linux-based Storm clusters created after 10/28/2016, hello HBase SDK for .NET package available as of 10/28/2016 does not work correctly on Linux-based HDInsight</span></span>

* <span data-ttu-id="e203a-115">Apache HBase on HDInsight-fürt (Linux vagy a Windows-alapú).</span><span class="sxs-lookup"><span data-stu-id="e203a-115">Apache HBase on HDInsight cluster (Linux or Windows-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e203a-116">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="e203a-116">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e203a-117">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e203a-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e203a-118">[Java](https://java.com) 1.7, vagy annál nagyobb a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="e203a-118">[Java](https://java.com) 1.7 or greater on your development environment.</span></span> <span data-ttu-id="e203a-119">Java használt toopackage hello topológia, ha az elküldött toohello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="e203a-119">Java is used toopackage hello topology when it is submitted toohello HDInsight cluster.</span></span>

  * <span data-ttu-id="e203a-120">Hello **JAVA_HOME** környezeti változó kell pont toohello tartalmazó könyvtár Java.</span><span class="sxs-lookup"><span data-stu-id="e203a-120">hello **JAVA_HOME** environment variable must point toohello directory that contains Java.</span></span>
  * <span data-ttu-id="e203a-121">Hello **%JAVA_HOME%/bin** könyvtárnak kell lennie a hello elérési útja</span><span class="sxs-lookup"><span data-stu-id="e203a-121">hello **%JAVA_HOME%/bin** directory must be in hello path</span></span>

## <a name="architecture"></a><span data-ttu-id="e203a-122">Architektúra</span><span class="sxs-lookup"><span data-stu-id="e203a-122">Architecture</span></span>

![Hello adatfolyama keresztül hello topológia ábrája](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

<span data-ttu-id="e203a-124">Hello eseményforrás megegyező azonosító adatok események igényel.</span><span class="sxs-lookup"><span data-stu-id="e203a-124">Correlating events requires a common identifier for hello event source.</span></span> <span data-ttu-id="e203a-125">For example, egy felhasználói Azonosítót, munkamenet-azonosító vagy egyéb adat, amely nem a) egyedi és b) szerepelnek az összes küldött adatok tooStorm.</span><span class="sxs-lookup"><span data-stu-id="e203a-125">For example, a user ID, session ID, or other piece of data that is a) unique and b) included in all data sent tooStorm.</span></span> <span data-ttu-id="e203a-126">Ez a példa egy GUID-érték toorepresent egy munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e203a-126">This example uses a GUID value toorepresent a session ID.</span></span>

<span data-ttu-id="e203a-127">Ebben a példában két HDInsight-fürtök áll:</span><span class="sxs-lookup"><span data-stu-id="e203a-127">This example consists of two HDInsight clusters:</span></span>

* <span data-ttu-id="e203a-128">HBase: állandó adattár korábbi adatok</span><span class="sxs-lookup"><span data-stu-id="e203a-128">HBase: persistent data store for historical data</span></span>
* <span data-ttu-id="e203a-129">A Storm: használt tooingest bejövő adatok</span><span class="sxs-lookup"><span data-stu-id="e203a-129">Storm: used tooingest incoming data</span></span>

<span data-ttu-id="e203a-130">hello adatok véletlenszerűen generálja a hello Storm-topológia, és a következő elemek hello áll:</span><span class="sxs-lookup"><span data-stu-id="e203a-130">hello data is randomly generated by hello Storm topology, and consists of hello following items:</span></span>

* <span data-ttu-id="e203a-131">Munkamenet-azonosító: egy GUID, amely egyedileg azonosítja mindegyik munkamenethez</span><span class="sxs-lookup"><span data-stu-id="e203a-131">Session ID: a GUID that uniquely identifies each session</span></span>
* <span data-ttu-id="e203a-132">Esemény: egy kezdeti vagy záró esemény.</span><span class="sxs-lookup"><span data-stu-id="e203a-132">Event: a START or END event.</span></span> <span data-ttu-id="e203a-133">Ebben a példában START mindig várakozni vége előtt</span><span class="sxs-lookup"><span data-stu-id="e203a-133">For this example, START always occurs before END</span></span>
* <span data-ttu-id="e203a-134">: Hello idő hello esemény.</span><span class="sxs-lookup"><span data-stu-id="e203a-134">Time: hello time of hello event.</span></span>

<span data-ttu-id="e203a-135">Ezek az adatok feldolgozása és HBase tárolja.</span><span class="sxs-lookup"><span data-stu-id="e203a-135">This data is processed and stored in HBase.</span></span>

### <a name="storm-topology"></a><span data-ttu-id="e203a-136">Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="e203a-136">Storm topology</span></span>

<span data-ttu-id="e203a-137">A munkamenet akkor kezdődik, amikor egy **START** esemény érkezik hello topológia és tooHBase naplózza.</span><span class="sxs-lookup"><span data-stu-id="e203a-137">When a session starts, a **START** event is received by hello topology and logged tooHBase.</span></span> <span data-ttu-id="e203a-138">Ha egy **END** esemény érkezik, hello topológia lekéri hello **START** esemény, és kiszámítja az hello idő hello két események között.</span><span class="sxs-lookup"><span data-stu-id="e203a-138">When an **END** event is received, hello topology retrieves hello **START** event and calculates hello time between hello two events.</span></span> <span data-ttu-id="e203a-139">Ez **időtartam** érték aztán a HBase együtt hello **END** események adatait.</span><span class="sxs-lookup"><span data-stu-id="e203a-139">This **Duration** value is then stored in HBase along with hello **END** event information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e203a-140">Ez a topológia alapszintű hello minta mutatja be, amíg egy éles megoldás kell tootake tervezési a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="e203a-140">While this topology demonstrates hello basic pattern, a production solution would need tootake design for hello following scenarios:</span></span>
>
> * <span data-ttu-id="e203a-141">Sorrendje nem érkező események</span><span class="sxs-lookup"><span data-stu-id="e203a-141">Events arriving out of order</span></span>
> * <span data-ttu-id="e203a-142">Ismétlődő esemény</span><span class="sxs-lookup"><span data-stu-id="e203a-142">Duplicate events</span></span>
> * <span data-ttu-id="e203a-143">Az eldobott események</span><span class="sxs-lookup"><span data-stu-id="e203a-143">Dropped events</span></span>

<span data-ttu-id="e203a-144">a következő összetevők hello hello minta topológia áll:</span><span class="sxs-lookup"><span data-stu-id="e203a-144">hello sample topology is composed of hello following components:</span></span>

* <span data-ttu-id="e203a-145">Session.cs: hozzon létre egy véletlenszerű munkamenet-Azonosítót, kezdési időt és mennyi ideig hello munkamenet tart a felhasználói munkamenet szimulálja.</span><span class="sxs-lookup"><span data-stu-id="e203a-145">Session.cs: simulates a user session by creating a random session ID, start time, and how long hello session lasts.</span></span>

* <span data-ttu-id="e203a-146">Spout.cs: 100 munkamenetek hoz létre, megfelelően kibocsát egy indítási esemény, hello véletlenszerű időtúllépés megvárja, mindegyik munkamenethez, és majd bocsát ki a záró eseményt.</span><span class="sxs-lookup"><span data-stu-id="e203a-146">Spout.cs: creates 100 sessions, emits a START event, waits hello random timeout for each session, and then emits an END event.</span></span> <span data-ttu-id="e203a-147">Újrahasznosítja azt majd munkamenetek toogenerate újakat véget ért.</span><span class="sxs-lookup"><span data-stu-id="e203a-147">Then recycles ended sessions toogenerate new ones.</span></span>

* <span data-ttu-id="e203a-148">HBaseLookupBolt.cs: hello munkamenet azonosítója toolook munkamenet adatait a HBase használja.</span><span class="sxs-lookup"><span data-stu-id="e203a-148">HBaseLookupBolt.cs: uses hello session ID toolook up session information from HBase.</span></span> <span data-ttu-id="e203a-149">Záró események feldolgozása után hello megfelelő KEZDŐ esemény megkeresése, és hello munkamenet időtartama alatt hello számítja ki.</span><span class="sxs-lookup"><span data-stu-id="e203a-149">When an END event is processed, it finds hello corresponding START event and calculates hello duration of hello session.</span></span>

* <span data-ttu-id="e203a-150">HBaseBolt.cs: HBase adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="e203a-150">HBaseBolt.cs: Stores information into HBase.</span></span>

* <span data-ttu-id="e203a-151">TypeHelper.cs: Amikor olvasása / írása tooHBase típuskonverziós nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="e203a-151">TypeHelper.cs: Helps with type conversion when reading from/writing tooHBase.</span></span>

### <a name="hbase-schema"></a><span data-ttu-id="e203a-152">A HBase séma</span><span class="sxs-lookup"><span data-stu-id="e203a-152">HBase schema</span></span>

<span data-ttu-id="e203a-153">A HBase hello adatok tárolt egy táblázatot a következő séma/beállításai hello:</span><span class="sxs-lookup"><span data-stu-id="e203a-153">In HBase, hello data is stored in a table with hello following schema/settings:</span></span>

* <span data-ttu-id="e203a-154">Sorkulcsa: hello munkamenet-Azonosítót használja a tábla sorainak hello kulcsaként.</span><span class="sxs-lookup"><span data-stu-id="e203a-154">Row key: hello session ID is used as hello key for rows in this table.</span></span>

* <span data-ttu-id="e203a-155">Oszlop család: hello családi név megadása "cf".</span><span class="sxs-lookup"><span data-stu-id="e203a-155">Column family: hello family name is 'cf'.</span></span> <span data-ttu-id="e203a-156">A családban tárolt oszlopok a következők:</span><span class="sxs-lookup"><span data-stu-id="e203a-156">Columns stored in this family are:</span></span>

  * <span data-ttu-id="e203a-157">esemény: KEZDŐ vagy záró.</span><span class="sxs-lookup"><span data-stu-id="e203a-157">event: START or END.</span></span>

  * <span data-ttu-id="e203a-158">idő: hello idő ezredmásodpercben, amelyet hello esemény történt.</span><span class="sxs-lookup"><span data-stu-id="e203a-158">time: hello time in milliseconds that hello event occurred.</span></span>

  * <span data-ttu-id="e203a-159">időtartam: hello közötti KEZDÉSI és BEFEJEZÉSI esemény.</span><span class="sxs-lookup"><span data-stu-id="e203a-159">duration: hello length between START and END event.</span></span>

* <span data-ttu-id="e203a-160">VERZIÓK: hello "cf" termékcsalád van beállítva, hogy minden egyes sorára tooretain 5 verzióit.</span><span class="sxs-lookup"><span data-stu-id="e203a-160">VERSIONS: hello 'cf' family is set tooretain 5 versions of each row.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e203a-161">Azok az egy adott sortól kulcs tárolt előző értékek naplózása verziók.</span><span class="sxs-lookup"><span data-stu-id="e203a-161">Versions are a log of previous values stored for a specific row key.</span></span> <span data-ttu-id="e203a-162">Alapértelmezés szerint a HBase csak egy sor legújabb verziója hello hello értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e203a-162">By default, HBase only returns hello value for hello most recent version of a row.</span></span> <span data-ttu-id="e203a-163">Ebben az esetben hello ugyanabban a sorban szolgál az összes esemény (KEZDŐ, záró.) egy sor minden verziója hello timestamp érték azonosít.</span><span class="sxs-lookup"><span data-stu-id="e203a-163">In this case, hello same row is used for all events (START, END.) each version of a row is identified by hello timestamp value.</span></span> <span data-ttu-id="e203a-164">Verziók, adjon meg egy egyedi azonosítót. a naplózott események ügyfélállapotainak</span><span class="sxs-lookup"><span data-stu-id="e203a-164">Versions provide a historical view of events logged for a specific ID.</span></span>

## <a name="download-hello-project"></a><span data-ttu-id="e203a-165">Hello projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="e203a-165">Download hello project</span></span>

<span data-ttu-id="e203a-166">hello minta projekt letölthető [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span><span class="sxs-lookup"><span data-stu-id="e203a-166">hello sample project can be downloaded from [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span></span>

<span data-ttu-id="e203a-167">Ez a letöltés a következő C# projektek hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e203a-167">This download contains hello following C# projects:</span></span>

* <span data-ttu-id="e203a-168">CorrelationTopology: C# Storm-topológia, hogy véletlenszerűen bocsát ki a kezdő és záró események a felhasználói munkamenetekben.</span><span class="sxs-lookup"><span data-stu-id="e203a-168">CorrelationTopology: C# Storm topology that randomly emits start and end events for user sessions.</span></span> <span data-ttu-id="e203a-169">Minden munkamenet érvényes, 1 és 5 perc között.</span><span class="sxs-lookup"><span data-stu-id="e203a-169">Each session lasts between 1 and 5 minutes.</span></span>

* <span data-ttu-id="e203a-170">SessionInfo: C# konzolalkalmazást, amely hello HBase táblát hoz létre, és tájékoztatást mintalekérdezéseket tooreturn tárolt munkamenet-adatokat.</span><span class="sxs-lookup"><span data-stu-id="e203a-170">SessionInfo: C# console application that creates hello HBase table, and provides example queries tooreturn information about stored session data.</span></span>

## <a name="create-hello-table"></a><span data-ttu-id="e203a-171">Hello tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="e203a-171">Create hello table</span></span>

1. <span data-ttu-id="e203a-172">Nyissa meg hello **SessionInfo** projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="e203a-172">Open hello **SessionInfo** project in Visual Studio.</span></span>

2. <span data-ttu-id="e203a-173">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **SessionInfo** projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e203a-173">In **Solution Explorer**, right-click hello **SessionInfo** project and select **Properties**.</span></span>

    ![A kijelölt tulajdonság menü képernyőképe](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. <span data-ttu-id="e203a-175">Válassza ki **beállítások**, majd a készlet hello a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="e203a-175">Select **Settings**, then set hello following values:</span></span>

   * <span data-ttu-id="e203a-176">HBaseClusterURL: hello URL-cím tooyour HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e203a-176">HBaseClusterURL: hello URL tooyour HBase cluster.</span></span> <span data-ttu-id="e203a-177">Például https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="e203a-177">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="e203a-178">HBaseClusterUserName: hello admin/HTTP felhasználói fiók a fürt</span><span class="sxs-lookup"><span data-stu-id="e203a-178">HBaseClusterUserName: hello admin/HTTP user account for your cluster</span></span>

   * <span data-ttu-id="e203a-179">HBaseClusterPassword: hello hello admin/HTTP felhasználói fiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="e203a-179">HBaseClusterPassword: hello password for hello admin/HTTP user account</span></span>

   * <span data-ttu-id="e203a-180">HBaseTableName: Ebben a példában a hello tábla toouse hello neve</span><span class="sxs-lookup"><span data-stu-id="e203a-180">HBaseTableName: hello name of hello table toouse with this example</span></span>

   * <span data-ttu-id="e203a-181">HBaseTableColumnFamily: hello oszlop családnév</span><span class="sxs-lookup"><span data-stu-id="e203a-181">HBaseTableColumnFamily: hello column family name</span></span>

     ![Beállítások párbeszédpanel képe](./media/hdinsight-storm-correlation-topology/settings.png)

4. <span data-ttu-id="e203a-183">Futtassa a hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="e203a-183">Run hello solution.</span></span> <span data-ttu-id="e203a-184">Amikor a rendszer kéri, válassza ki a hello "c" kulcs toocreate hello tábla a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e203a-184">When prompted, select hello 'c' key toocreate hello table on your HBase cluster.</span></span>

## <a name="build-and-deploy-hello-storm-topology"></a><span data-ttu-id="e203a-185">Hozza létre és hello Storm-topológia központi telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="e203a-185">Build and deploy hello Storm topology</span></span>

1. <span data-ttu-id="e203a-186">Nyissa meg hello **CorrelationTopology** a Visual Studio megoldás.</span><span class="sxs-lookup"><span data-stu-id="e203a-186">Open hello **CorrelationTopology** solution in Visual Studio.</span></span>

2. <span data-ttu-id="e203a-187">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **CorrelationTopology** projektre, és válassza a tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e203a-187">In **Solution Explorer**, right-click hello **CorrelationTopology** project and select properties.</span></span>

3. <span data-ttu-id="e203a-188">Hello tulajdonságai ablakban válassza ki **beállítások** és adja meg a hello konfigurációs ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="e203a-188">In hello properties window, select **Settings** and enter hello configuration values for this project.</span></span> <span data-ttu-id="e203a-189">hello első 5 hello értékeitől használt hello **SessionInfo** projekt:</span><span class="sxs-lookup"><span data-stu-id="e203a-189">hello first 5 are hello same values used by hello **SessionInfo** project:</span></span>

   * <span data-ttu-id="e203a-190">HBaseClusterURL: hello URL-cím tooyour HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e203a-190">HBaseClusterURL: hello URL tooyour HBase cluster.</span></span> <span data-ttu-id="e203a-191">Például https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="e203a-191">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="e203a-192">HBaseClusterUserName: hello admin/HTTP felhasználói fiók a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="e203a-192">HBaseClusterUserName: hello admin/HTTP user account for your cluster.</span></span>

   * <span data-ttu-id="e203a-193">HBaseClusterPassword: hello hello admin/HTTP felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e203a-193">HBaseClusterPassword: hello password for hello admin/HTTP user account.</span></span>

   * <span data-ttu-id="e203a-194">HBaseTableName: Ebben a példában a hello tábla toouse hello neve.</span><span class="sxs-lookup"><span data-stu-id="e203a-194">HBaseTableName: hello name of hello table toouse with this example.</span></span> <span data-ttu-id="e203a-195">Ez az érték van hello azonos módon hello SessionInfo projektben használt tábla neve.</span><span class="sxs-lookup"><span data-stu-id="e203a-195">This value is hello same table name as used in hello SessionInfo project.</span></span>

   * <span data-ttu-id="e203a-196">HBaseTableColumnFamily: hello oszlop családnév.</span><span class="sxs-lookup"><span data-stu-id="e203a-196">HBaseTableColumnFamily: hello column family name.</span></span> <span data-ttu-id="e203a-197">Ez az érték van hello azonos oszlop családnév szerint hello SessionInfo projektben használt.</span><span class="sxs-lookup"><span data-stu-id="e203a-197">This value is hello same column family name as used in hello SessionInfo project.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="e203a-198">Nem módosítható hello HBaseTableColumnNames, mert a hello azok által használt hello nevének **SessionInfo** tooretrieve adatokat.</span><span class="sxs-lookup"><span data-stu-id="e203a-198">Do not change hello HBaseTableColumnNames, as hello defaults are hello names used by **SessionInfo** tooretrieve data.</span></span>

4. <span data-ttu-id="e203a-199">Hello tulajdonságok mentéséhez, majd hello projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e203a-199">Save hello properties, then build hello project.</span></span>

5. <span data-ttu-id="e203a-200">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="e203a-200">In **Solution Explorer**, right-click hello project and select **Submit tooStorm on HDInsight**.</span></span> <span data-ttu-id="e203a-201">Ha a rendszer kéri, adja meg hello hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e203a-201">If prompted, enter hello credentials for your Azure subscription.</span></span>

   ![Hello képe nyújt toostorm menüpont](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. <span data-ttu-id="e203a-203">A hello **nyújt topológia** párbeszédablak, ez a topológia a toodeploy kívánt válassza hello Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="e203a-203">In hello **Submit Topology** dialog, select hello Storm cluster you want toodeploy this topology to.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e203a-204">hello első alkalommal, amikor elküld, hogy a topológia eltarthat néhány másodpercig tooretrieve hello nevét a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="e203a-204">hello first time you submit a topology, it may take a few seconds tooretrieve hello name of your HDInsight clusters.</span></span>

7. <span data-ttu-id="e203a-205">Hello topológia feltöltött és elküldött toohello fürt követően hello **Storm-topológia nézet** megnyitja és topológia futtató hello megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="e203a-205">Once hello topology has been uploaded and submitted toohello cluster, hello **Storm Topology View** opens and displays hello running topology.</span></span> <span data-ttu-id="e203a-206">toorefresh hello adatokat, válassza hello **CorrelationTopology** és hello frissítés gomb használatának hello hello oldal jobb felső.</span><span class="sxs-lookup"><span data-stu-id="e203a-206">toorefresh hello data, select hello **CorrelationTopology** and use hello refresh button at hello top right of hello page.</span></span>

   ![Hello topológia e nézetében képe](./media/hdinsight-storm-correlation-topology/topologyview.png)

   <span data-ttu-id="e203a-208">Elindulásakor hello topológia adatok hello hello érték **Emitted** oszlop növekmények esetén.</span><span class="sxs-lookup"><span data-stu-id="e203a-208">When hello topology begins generating data, hello value in hello **Emitted** column increments.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e203a-209">Ha hello **Storm-topológia nézet** automatikusan nyílik meg, ne használja hello tooopen lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="e203a-209">If hello **Storm Topology View** does not open automatically, use hello following steps tooopen it:</span></span>
   >
   > 1. <span data-ttu-id="e203a-210">A **Megoldáskezelőben**, bontsa ki a **Azure**, majd bontsa ki a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e203a-210">In **Solution Explorer**, expand **Azure**, and then expand **HDInsight**.</span></span>
   > 2. <span data-ttu-id="e203a-211">Kattintson a jobb gombbal hello Storm-fürt, amely topológia hello fut, és válassza **nézet Storm-topológiák**</span><span class="sxs-lookup"><span data-stu-id="e203a-211">Right-click hello Storm cluster that hello topology is running on, and then select **View Storm Topologies**</span></span>

## <a name="query-hello-data"></a><span data-ttu-id="e203a-212">Hello adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="e203a-212">Query hello data</span></span>

<span data-ttu-id="e203a-213">Amennyiben az adatok kibocsátott használja a következő lépéseket tooquery hello adatok hello.</span><span class="sxs-lookup"><span data-stu-id="e203a-213">Once data has been emitted, use hello following steps tooquery hello data.</span></span>

1. <span data-ttu-id="e203a-214">Térjen vissza a toohello **SessionInfo** projekt.</span><span class="sxs-lookup"><span data-stu-id="e203a-214">Return toohello **SessionInfo** project.</span></span> <span data-ttu-id="e203a-215">Ha nem fut, indítsa el egy új példányát.</span><span class="sxs-lookup"><span data-stu-id="e203a-215">If not running, start a new instance of it.</span></span>

2. <span data-ttu-id="e203a-216">Amikor a rendszer kéri, válassza ki a **s** toosearch START esemény.</span><span class="sxs-lookup"><span data-stu-id="e203a-216">When prompted, select **s** toosearch for START event.</span></span> <span data-ttu-id="e203a-217">Rákérdezéses tooenter egy kezdő és záró idő toodefine egy időtartományt - visszaadott csak események ezen két időpontok között.</span><span class="sxs-lookup"><span data-stu-id="e203a-217">You are prompted tooenter a start and end time toodefine a time range - only events between these two times are returned.</span></span>

    <span data-ttu-id="e203a-218">Használjon hello alábbi formázása hello start megadásakor, és befejezési időpontja: óó: PP és "vagyok" vagy "pm".</span><span class="sxs-lookup"><span data-stu-id="e203a-218">Use hello following format when entering hello start and end times: HH:MM and 'am' or 'pm'.</span></span> <span data-ttu-id="e203a-219">Ha például 23:20 óra.</span><span class="sxs-lookup"><span data-stu-id="e203a-219">For example, 11:20pm.</span></span>

    <span data-ttu-id="e203a-220">tooreturn naplózott események, használja a kezdő időpont előtt hello Storm-topológia telepítve lett, és most a befejezési ideje.</span><span class="sxs-lookup"><span data-stu-id="e203a-220">tooreturn logged events, use a start time from before hello Storm topology was deployed, and an end time of now.</span></span> <span data-ttu-id="e203a-221">hello visszatérési adatainak bejegyzéseket a következő szöveg hasonló toohello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e203a-221">hello return data contains entries similar toohello following text:</span></span>

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

<span data-ttu-id="e203a-222">Keresése záró események works hello azonos KEZDŐ eseményként is rögzíti.</span><span class="sxs-lookup"><span data-stu-id="e203a-222">Searching for END events works hello same as START events.</span></span> <span data-ttu-id="e203a-223">Azt jelzi, hogy 1 és 5 perc után hello START esemény között véletlenszerűen jönnek-azonban záró eseményeket.</span><span class="sxs-lookup"><span data-stu-id="e203a-223">However, END events are generated randomly between 1 and 5 minutes after hello START event.</span></span> <span data-ttu-id="e203a-224">Előfordulhat, hogy tootry néhány alkalommal tartományok toofind hello záró eseményeket.</span><span class="sxs-lookup"><span data-stu-id="e203a-224">You may have tootry a few time ranges toofind hello END events.</span></span> <span data-ttu-id="e203a-225">ZÁRÓ eseményeket is tartalmazza hello munkamenet időtartama alatt hello - hello esemény kezdete és vége esemény hello különbsége.</span><span class="sxs-lookup"><span data-stu-id="e203a-225">END events also contain hello duration of hello session - hello difference between hello START event time and END event time.</span></span> <span data-ttu-id="e203a-226">Itt látható egy példa az END eseményekre vonatkozó adatokat:</span><span class="sxs-lookup"><span data-stu-id="e203a-226">Here is an example of data for END events:</span></span>

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> <span data-ttu-id="e203a-227">Míg hello idő értékeknek meg helyi idő, hello hello lekérdezés által visszaadott ideje UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="e203a-227">While hello time values you enter are in local time, hello time returned from hello query is in UTC.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="e203a-228">Hello topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="e203a-228">Stop hello topology</span></span>

<span data-ttu-id="e203a-229">Amikor készen áll a toostop hello topológia, térjen vissza a toohello **CorrelationTopology** projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="e203a-229">When you are ready toostop hello topology, return toohello **CorrelationTopology** project in Visual Studio.</span></span> <span data-ttu-id="e203a-230">A hello **Storm-topológia nézet**, válassza ki hello topológiát, majd hello **Kill** hello tetején hello topológia megtekintése gombra.</span><span class="sxs-lookup"><span data-stu-id="e203a-230">In hello **Storm Topology View**, select hello topology and then use hello **Kill** button at hello top of hello topology view.</span></span>

## <a name="delete-your-cluster"></a><span data-ttu-id="e203a-231">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="e203a-231">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="e203a-232">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e203a-232">Next steps</span></span>

<span data-ttu-id="e203a-233">További Storm, tekintse meg a [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="e203a-233">For more Storm examples, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
