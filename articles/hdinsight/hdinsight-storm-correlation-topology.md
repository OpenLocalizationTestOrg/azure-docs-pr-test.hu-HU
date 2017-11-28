---
title: "A Storm és HBase a HDInsight időbeli összefüggésbe események"
description: "Útmutató a HDInsight alatt futó Storm és HBase használatával különböző időpontokban érkező események összefüggéseket."
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
ms.openlocfilehash: 06630096383601e48e8f69f8553314cee42f5f3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a><span data-ttu-id="1942e-103">Az eseményeket, amelyek különböző időpontokban érkeznek összefüggéseket Storm és HBase használatával</span><span class="sxs-lookup"><span data-stu-id="1942e-103">Correlate events that arrive at different times using Storm and HBase</span></span>

<span data-ttu-id="1942e-104">Apache Storm egy állandó adattár használatával hozhatók adatok bejegyzéseit, amelyek a kiszolgálófarmban lévő különböző időpontokban.</span><span class="sxs-lookup"><span data-stu-id="1942e-104">By using a persistent data store with Apache Storm, you can correlate data entries that arrive at different times.</span></span> <span data-ttu-id="1942e-105">Például létrehozhatja a bejelentkezési és kijelentkezési események kiszámításához, hogy mennyi ideig tartott a a munkamenet felhasználói munkamenet.</span><span class="sxs-lookup"><span data-stu-id="1942e-105">For example, linking login and logout events for a user session to calculate how long the session lasted.</span></span>

<span data-ttu-id="1942e-106">Ebből a dokumentumból megismerheti, hogyan egy alapszintű C# Storm-topológia, amely nyomon követi a bejelentkezési és kijelentkezési események a felhasználói munkamenetekben, és kiszámítja az időtartamot, a munkamenet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1942e-106">In this document, you learn how to create a basic C# Storm topology that tracks login and logout events for user sessions, and calculates the duration of the session.</span></span> <span data-ttu-id="1942e-107">A topológia állandó adattárként HBase eszközt használja.</span><span class="sxs-lookup"><span data-stu-id="1942e-107">The topology uses HBase as a persistent data store.</span></span> <span data-ttu-id="1942e-108">HBase is lehetővé teszi a korábbi adatok létrehozásához további betekintést nyerjen a kötegelt lekérdezések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="1942e-108">HBase also allows you to perform batch queries on the historical data to produce additional insights.</span></span> <span data-ttu-id="1942e-109">Például hány felhasználói munkamenetek lettek elindítva, vagy egy adott időszakra vonatkozóan befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1942e-109">For example, how many user sessions were started or ended during a specific time period.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1942e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1942e-110">Prerequisites</span></span>

* <span data-ttu-id="1942e-111">A Visual Studio és a HDInsight tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1942e-111">Visual Studio and the HDInsight tools for Visual Studio.</span></span> <span data-ttu-id="1942e-112">További információkért lásd: [első lépései a HDInsight tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1942e-112">For more information, see [Get started using the HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="1942e-113">A HDInsight alatt futó Apache Storm a fürt (Windows-alapú).</span><span class="sxs-lookup"><span data-stu-id="1942e-113">Apache Storm on HDInsight cluster (Windows-based).</span></span>

  > [!WARNING]
  > <span data-ttu-id="1942e-114">SCP.NET topológiák támogatottak a Linux-alapú Storm-fürtök létrehozása után 10/28/2016, a HBase SDK elérhető 10/28/2016 .NET-csomag nem működik megfelelően a Linux-alapú HDInsight-on</span><span class="sxs-lookup"><span data-stu-id="1942e-114">While SCP.NET topologies are supported on Linux-based Storm clusters created after 10/28/2016, the HBase SDK for .NET package available as of 10/28/2016 does not work correctly on Linux-based HDInsight</span></span>

* <span data-ttu-id="1942e-115">Apache HBase on HDInsight-fürt (Linux vagy a Windows-alapú).</span><span class="sxs-lookup"><span data-stu-id="1942e-115">Apache HBase on HDInsight cluster (Linux or Windows-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="1942e-116">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="1942e-116">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1942e-117">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1942e-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="1942e-118">[Java](https://java.com) 1.7, vagy annál nagyobb a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="1942e-118">[Java](https://java.com) 1.7 or greater on your development environment.</span></span> <span data-ttu-id="1942e-119">Java csomag a topológia a HDInsight-fürthöz elküldésekor szolgál.</span><span class="sxs-lookup"><span data-stu-id="1942e-119">Java is used to package the topology when it is submitted to the HDInsight cluster.</span></span>

  * <span data-ttu-id="1942e-120">A **JAVA_HOME** környezeti változó Java tartalmazó könyvtárat kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="1942e-120">The **JAVA_HOME** environment variable must point to the directory that contains Java.</span></span>
  * <span data-ttu-id="1942e-121">A **%JAVA_HOME%/bin** könyvtárnak az elérési úton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1942e-121">The **%JAVA_HOME%/bin** directory must be in the path</span></span>

## <a name="architecture"></a><span data-ttu-id="1942e-122">Architektúra</span><span class="sxs-lookup"><span data-stu-id="1942e-122">Architecture</span></span>

![Az adatfolyam keresztül a topológia ábrája](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

<span data-ttu-id="1942e-124">A forrás egy közös azonosító adatok események igényel.</span><span class="sxs-lookup"><span data-stu-id="1942e-124">Correlating events requires a common identifier for the event source.</span></span> <span data-ttu-id="1942e-125">For example egy felhasználói Azonosítót, munkamenet-azonosító, vagy egyéb adat, amely egyedi a), valamint b) megtalálható a Storm küldött minden adathoz.</span><span class="sxs-lookup"><span data-stu-id="1942e-125">For example, a user ID, session ID, or other piece of data that is a) unique and b) included in all data sent to Storm.</span></span> <span data-ttu-id="1942e-126">Ez a példa egy GUID-érték képviseli egy munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="1942e-126">This example uses a GUID value to represent a session ID.</span></span>

<span data-ttu-id="1942e-127">Ebben a példában két HDInsight-fürtök áll:</span><span class="sxs-lookup"><span data-stu-id="1942e-127">This example consists of two HDInsight clusters:</span></span>

* <span data-ttu-id="1942e-128">HBase: állandó adattár korábbi adatok</span><span class="sxs-lookup"><span data-stu-id="1942e-128">HBase: persistent data store for historical data</span></span>
* <span data-ttu-id="1942e-129">Storm: a bejövő adatok használt</span><span class="sxs-lookup"><span data-stu-id="1942e-129">Storm: used to ingest incoming data</span></span>

<span data-ttu-id="1942e-130">Az adatok véletlenszerűen generálja a Storm-topológia, és az alábbi elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1942e-130">The data is randomly generated by the Storm topology, and consists of the following items:</span></span>

* <span data-ttu-id="1942e-131">Munkamenet-azonosító: egy GUID, amely egyedileg azonosítja mindegyik munkamenethez</span><span class="sxs-lookup"><span data-stu-id="1942e-131">Session ID: a GUID that uniquely identifies each session</span></span>
* <span data-ttu-id="1942e-132">Esemény: egy kezdeti vagy záró esemény.</span><span class="sxs-lookup"><span data-stu-id="1942e-132">Event: a START or END event.</span></span> <span data-ttu-id="1942e-133">Ebben a példában START mindig várakozni vége előtt</span><span class="sxs-lookup"><span data-stu-id="1942e-133">For this example, START always occurs before END</span></span>
* <span data-ttu-id="1942e-134">Idő: az esemény időpontja.</span><span class="sxs-lookup"><span data-stu-id="1942e-134">Time: the time of the event.</span></span>

<span data-ttu-id="1942e-135">Ezek az adatok feldolgozása és HBase tárolja.</span><span class="sxs-lookup"><span data-stu-id="1942e-135">This data is processed and stored in HBase.</span></span>

### <a name="storm-topology"></a><span data-ttu-id="1942e-136">Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="1942e-136">Storm topology</span></span>

<span data-ttu-id="1942e-137">A munkamenet akkor kezdődik, amikor egy **START** esemény a topológia által fogadott és a HBase a rendszer naplózza.</span><span class="sxs-lookup"><span data-stu-id="1942e-137">When a session starts, a **START** event is received by the topology and logged to HBase.</span></span> <span data-ttu-id="1942e-138">Ha egy **END** esemény érkezik, a topológia lekéri a **START** esemény, és kiszámítja a két esemény között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="1942e-138">When an **END** event is received, the topology retrieves the **START** event and calculates the time between the two events.</span></span> <span data-ttu-id="1942e-139">Ez **időtartam** érték aztán a HBase, valamint a **END** események adatait.</span><span class="sxs-lookup"><span data-stu-id="1942e-139">This **Duration** value is then stored in HBase along with the **END** event information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1942e-140">Ebben a topológiában az alapvető minta mutatja be, amíg egy éles megoldást kell vennie a kialakítás az alábbi helyzetekben:</span><span class="sxs-lookup"><span data-stu-id="1942e-140">While this topology demonstrates the basic pattern, a production solution would need to take design for the following scenarios:</span></span>
>
> * <span data-ttu-id="1942e-141">Sorrendje nem érkező események</span><span class="sxs-lookup"><span data-stu-id="1942e-141">Events arriving out of order</span></span>
> * <span data-ttu-id="1942e-142">Ismétlődő esemény</span><span class="sxs-lookup"><span data-stu-id="1942e-142">Duplicate events</span></span>
> * <span data-ttu-id="1942e-143">Az eldobott események</span><span class="sxs-lookup"><span data-stu-id="1942e-143">Dropped events</span></span>

<span data-ttu-id="1942e-144">A minta topológia a következő összetevőkből áll:</span><span class="sxs-lookup"><span data-stu-id="1942e-144">The sample topology is composed of the following components:</span></span>

* <span data-ttu-id="1942e-145">Session.cs: hozzon létre egy véletlenszerű munkamenet-Azonosítót, kezdési időt és mennyi ideig tart a munkamenet felhasználói munkamenet szimulálja.</span><span class="sxs-lookup"><span data-stu-id="1942e-145">Session.cs: simulates a user session by creating a random session ID, start time, and how long the session lasts.</span></span>

* <span data-ttu-id="1942e-146">Spout.cs: 100 munkamenetek hoz létre, megfelelően kibocsát egy indítási esemény, véletlenszerű időkorlát megvárja, mindegyik munkamenethez, és majd bocsát ki a záró eseményt.</span><span class="sxs-lookup"><span data-stu-id="1942e-146">Spout.cs: creates 100 sessions, emits a START event, waits the random timeout for each session, and then emits an END event.</span></span> <span data-ttu-id="1942e-147">Újrahasznosítja azt majd újakat létrehozni munkamenet véget ért.</span><span class="sxs-lookup"><span data-stu-id="1942e-147">Then recycles ended sessions to generate new ones.</span></span>

* <span data-ttu-id="1942e-148">HBaseLookupBolt.cs: a munkamenet-Azonosítót használja a munkamenet-információk a HBase kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="1942e-148">HBaseLookupBolt.cs: uses the session ID to look up session information from HBase.</span></span> <span data-ttu-id="1942e-149">ZÁRÓ eseményt dolgoz fel, ha a megfelelő KEZDŐ esemény megkeresése, és kiszámítja a munkamenet időtartama alatt.</span><span class="sxs-lookup"><span data-stu-id="1942e-149">When an END event is processed, it finds the corresponding START event and calculates the duration of the session.</span></span>

* <span data-ttu-id="1942e-150">HBaseBolt.cs: HBase adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="1942e-150">HBaseBolt.cs: Stores information into HBase.</span></span>

* <span data-ttu-id="1942e-151">TypeHelper.cs: Elősegíti a típuskonverziós olvasása / írása HBase.</span><span class="sxs-lookup"><span data-stu-id="1942e-151">TypeHelper.cs: Helps with type conversion when reading from/writing to HBase.</span></span>

### <a name="hbase-schema"></a><span data-ttu-id="1942e-152">A HBase séma</span><span class="sxs-lookup"><span data-stu-id="1942e-152">HBase schema</span></span>

<span data-ttu-id="1942e-153">Az adatok a HBase, a következő séma beállításokat tartalmazó tábla tárolja:</span><span class="sxs-lookup"><span data-stu-id="1942e-153">In HBase, the data is stored in a table with the following schema/settings:</span></span>

* <span data-ttu-id="1942e-154">Sorkulcsa: a munkamenet-Azonosítót az ebben a táblázatban szereplő sorok kulcsként használja.</span><span class="sxs-lookup"><span data-stu-id="1942e-154">Row key: the session ID is used as the key for rows in this table.</span></span>

* <span data-ttu-id="1942e-155">Oszlop család: a családi név megadása "cf".</span><span class="sxs-lookup"><span data-stu-id="1942e-155">Column family: the family name is 'cf'.</span></span> <span data-ttu-id="1942e-156">A családban tárolt oszlopok a következők:</span><span class="sxs-lookup"><span data-stu-id="1942e-156">Columns stored in this family are:</span></span>

  * <span data-ttu-id="1942e-157">esemény: KEZDŐ vagy záró.</span><span class="sxs-lookup"><span data-stu-id="1942e-157">event: START or END.</span></span>

  * <span data-ttu-id="1942e-158">idő: az idő, ezredmásodpercben, amelyet az esemény történt.</span><span class="sxs-lookup"><span data-stu-id="1942e-158">time: the time in milliseconds that the event occurred.</span></span>

  * <span data-ttu-id="1942e-159">időtartam: közötti KEZDÉSI és BEFEJEZÉSI esemény.</span><span class="sxs-lookup"><span data-stu-id="1942e-159">duration: the length between START and END event.</span></span>

* <span data-ttu-id="1942e-160">VERZIÓK: a "cf" termékcsalád tartsa meg az egyes sorok 5 verziók értéke.</span><span class="sxs-lookup"><span data-stu-id="1942e-160">VERSIONS: the 'cf' family is set to retain 5 versions of each row.</span></span>

  > [!NOTE]
  > <span data-ttu-id="1942e-161">Azok az egy adott sortól kulcs tárolt előző értékek naplózása verziók.</span><span class="sxs-lookup"><span data-stu-id="1942e-161">Versions are a log of previous values stored for a specific row key.</span></span> <span data-ttu-id="1942e-162">A HBase alapértelmezés szerint csak a legfrissebb sor értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1942e-162">By default, HBase only returns the value for the most recent version of a row.</span></span> <span data-ttu-id="1942e-163">Ebben az esetben a ugyanabban a sorban az összes esemény (KEZDŐ, záró.) a timestamp érték azonosít egy sor minden verziója szolgál.</span><span class="sxs-lookup"><span data-stu-id="1942e-163">In this case, the same row is used for all events (START, END.) each version of a row is identified by the timestamp value.</span></span> <span data-ttu-id="1942e-164">Verziók, adjon meg egy egyedi azonosítót. a naplózott események ügyfélállapotainak</span><span class="sxs-lookup"><span data-stu-id="1942e-164">Versions provide a historical view of events logged for a specific ID.</span></span>

## <a name="download-the-project"></a><span data-ttu-id="1942e-165">A projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="1942e-165">Download the project</span></span>

<span data-ttu-id="1942e-166">A minta projekt letölthető [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span><span class="sxs-lookup"><span data-stu-id="1942e-166">The sample project can be downloaded from [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span></span>

<span data-ttu-id="1942e-167">Ez a letöltés a következő C# projektet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="1942e-167">This download contains the following C# projects:</span></span>

* <span data-ttu-id="1942e-168">CorrelationTopology: C# Storm-topológia, hogy véletlenszerűen bocsát ki a kezdő és záró események a felhasználói munkamenetekben.</span><span class="sxs-lookup"><span data-stu-id="1942e-168">CorrelationTopology: C# Storm topology that randomly emits start and end events for user sessions.</span></span> <span data-ttu-id="1942e-169">Minden munkamenet érvényes, 1 és 5 perc között.</span><span class="sxs-lookup"><span data-stu-id="1942e-169">Each session lasts between 1 and 5 minutes.</span></span>

* <span data-ttu-id="1942e-170">SessionInfo: C# konzolalkalmazást, amely a HBase táblát hoz létre, és információt szolgáltat tárolt munkamenetadatok mintalekérdezéseket biztosít.</span><span class="sxs-lookup"><span data-stu-id="1942e-170">SessionInfo: C# console application that creates the HBase table, and provides example queries to return information about stored session data.</span></span>

## <a name="create-the-table"></a><span data-ttu-id="1942e-171">A tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="1942e-171">Create the table</span></span>

1. <span data-ttu-id="1942e-172">Nyissa meg a **SessionInfo** projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="1942e-172">Open the **SessionInfo** project in Visual Studio.</span></span>

2. <span data-ttu-id="1942e-173">A **Megoldáskezelőben**, kattintson a jobb gombbal a **SessionInfo** projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="1942e-173">In **Solution Explorer**, right-click the **SessionInfo** project and select **Properties**.</span></span>

    ![A kijelölt tulajdonság menü képernyőképe](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. <span data-ttu-id="1942e-175">Válassza ki **beállítások**, majd állítsa be a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="1942e-175">Select **Settings**, then set the following values:</span></span>

   * <span data-ttu-id="1942e-176">HBaseClusterURL: az URL-címe a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1942e-176">HBaseClusterURL: the URL to your HBase cluster.</span></span> <span data-ttu-id="1942e-177">Például https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1942e-177">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="1942e-178">HBaseClusterUserName: a felügyelet/HTTP felhasználói fiók a fürt</span><span class="sxs-lookup"><span data-stu-id="1942e-178">HBaseClusterUserName: the admin/HTTP user account for your cluster</span></span>

   * <span data-ttu-id="1942e-179">HBaseClusterPassword: a rendszergazda/HTTP felhasználói fiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="1942e-179">HBaseClusterPassword: the password for the admin/HTTP user account</span></span>

   * <span data-ttu-id="1942e-180">HBaseTableName: Ez a példa használata a tábla neve</span><span class="sxs-lookup"><span data-stu-id="1942e-180">HBaseTableName: the name of the table to use with this example</span></span>

   * <span data-ttu-id="1942e-181">HBaseTableColumnFamily: A családja oszlopnév</span><span class="sxs-lookup"><span data-stu-id="1942e-181">HBaseTableColumnFamily: The column family name</span></span>

     ![Beállítások párbeszédpanel képe](./media/hdinsight-storm-correlation-topology/settings.png)

4. <span data-ttu-id="1942e-183">Futtassa a megoldást.</span><span class="sxs-lookup"><span data-stu-id="1942e-183">Run the solution.</span></span> <span data-ttu-id="1942e-184">Amikor a rendszer kéri, válassza ki a "c" kulcsot, a HBase-fürtöt a tábla létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1942e-184">When prompted, select the 'c' key to create the table on your HBase cluster.</span></span>

## <a name="build-and-deploy-the-storm-topology"></a><span data-ttu-id="1942e-185">Hozza létre, és a Storm-topológia központi telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="1942e-185">Build and deploy the Storm topology</span></span>

1. <span data-ttu-id="1942e-186">Nyissa meg a **CorrelationTopology** a Visual Studio megoldás.</span><span class="sxs-lookup"><span data-stu-id="1942e-186">Open the **CorrelationTopology** solution in Visual Studio.</span></span>

2. <span data-ttu-id="1942e-187">A **Megoldáskezelőben**, kattintson a jobb gombbal a **CorrelationTopology** projektre, és válassza a tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="1942e-187">In **Solution Explorer**, right-click the **CorrelationTopology** project and select properties.</span></span>

3. <span data-ttu-id="1942e-188">Válassza a Tulajdonságok ablak **beállítások** , és írja be a konfigurációs értékeket ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="1942e-188">In the properties window, select **Settings** and enter the configuration values for this project.</span></span> <span data-ttu-id="1942e-189">Az első 5 rendszer által használt ugyanazokat az értékeket a **SessionInfo** projekt:</span><span class="sxs-lookup"><span data-stu-id="1942e-189">The first 5 are the same values used by the **SessionInfo** project:</span></span>

   * <span data-ttu-id="1942e-190">HBaseClusterURL: az URL-címe a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1942e-190">HBaseClusterURL: the URL to your HBase cluster.</span></span> <span data-ttu-id="1942e-191">Például https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1942e-191">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="1942e-192">HBaseClusterUserName: a felügyelet/HTTP felhasználói fiók a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="1942e-192">HBaseClusterUserName: the admin/HTTP user account for your cluster.</span></span>

   * <span data-ttu-id="1942e-193">HBaseClusterPassword: a felügyelet/HTTP felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="1942e-193">HBaseClusterPassword: the password for the admin/HTTP user account.</span></span>

   * <span data-ttu-id="1942e-194">HBaseTableName: használata ebben a példában a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="1942e-194">HBaseTableName: the name of the table to use with this example.</span></span> <span data-ttu-id="1942e-195">Ez az érték a tábla neve megegyezik a SessionInfo projektben használt.</span><span class="sxs-lookup"><span data-stu-id="1942e-195">This value is the same table name as used in the SessionInfo project.</span></span>

   * <span data-ttu-id="1942e-196">HBaseTableColumnFamily: Az oszlop csomagcsalád nevét.</span><span class="sxs-lookup"><span data-stu-id="1942e-196">HBaseTableColumnFamily: The column family name.</span></span> <span data-ttu-id="1942e-197">Ez az érték oszlop család neve megegyezik a SessionInfo projektben használt.</span><span class="sxs-lookup"><span data-stu-id="1942e-197">This value is the same column family name as used in the SessionInfo project.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="1942e-198">Nem módosítható a HBaseTableColumnNames, mert az azok által használt nevek **SessionInfo** beolvasni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="1942e-198">Do not change the HBaseTableColumnNames, as the defaults are the names used by **SessionInfo** to retrieve data.</span></span>

4. <span data-ttu-id="1942e-199">A tulajdonságok mentéséhez, majd a projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1942e-199">Save the properties, then build the project.</span></span>

5. <span data-ttu-id="1942e-200">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="1942e-200">In **Solution Explorer**, right-click the project and select **Submit to Storm on HDInsight**.</span></span> <span data-ttu-id="1942e-201">Ha a rendszer kéri, adja meg a hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="1942e-201">If prompted, enter the credentials for your Azure subscription.</span></span>

   ![A küldés alatt futó storm menüelemre képe](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. <span data-ttu-id="1942e-203">Az a **nyújt topológia** párbeszédpanelen válassza ki a kívánt Storm-fürt központi telepítése a topológiát.</span><span class="sxs-lookup"><span data-stu-id="1942e-203">In the **Submit Topology** dialog, select the Storm cluster you want to deploy this topology to.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1942e-204">Az első alkalommal, amikor elküld a topológia eltarthat néhány másodpercet a HDInsight-fürtök nevének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="1942e-204">The first time you submit a topology, it may take a few seconds to retrieve the name of your HDInsight clusters.</span></span>

7. <span data-ttu-id="1942e-205">Amennyiben a topológia feltöltött, és elküldi a fürt a **Storm-topológia nézet** megnyílik, és megjeleníti a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="1942e-205">Once the topology has been uploaded and submitted to the cluster, the **Storm Topology View** opens and displays the running topology.</span></span> <span data-ttu-id="1942e-206">Az adatok frissítéséhez válassza ki a **CorrelationTopology** , és használja a frissítés gombra az oldal tetején sarkában a lap.</span><span class="sxs-lookup"><span data-stu-id="1942e-206">To refresh the data, select the **CorrelationTopology** and use the refresh button at the top right of the page.</span></span>

   ![A topológia e nézetében képe](./media/hdinsight-storm-correlation-topology/topologyview.png)

   <span data-ttu-id="1942e-208">Ha a topológia kezdődik, ami adatokat, az értéket a **Emitted** oszlop növekmények esetén.</span><span class="sxs-lookup"><span data-stu-id="1942e-208">When the topology begins generating data, the value in the **Emitted** column increments.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1942e-209">Ha a **Storm-topológia nézet** nem automatikusan, nyissa meg a következő lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="1942e-209">If the **Storm Topology View** does not open automatically, use the following steps to open it:</span></span>
   >
   > 1. <span data-ttu-id="1942e-210">A **Megoldáskezelőben**, bontsa ki a **Azure**, majd bontsa ki a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="1942e-210">In **Solution Explorer**, expand **Azure**, and then expand **HDInsight**.</span></span>
   > 2. <span data-ttu-id="1942e-211">Kattintson a jobb gombbal a topológia a futó Storm-fürt, majd válassza ki **nézet Storm-topológiák**</span><span class="sxs-lookup"><span data-stu-id="1942e-211">Right-click the Storm cluster that the topology is running on, and then select **View Storm Topologies**</span></span>

## <a name="query-the-data"></a><span data-ttu-id="1942e-212">Az adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1942e-212">Query the data</span></span>

<span data-ttu-id="1942e-213">Amennyiben az adatok kibocsátott tegye a következőket az adatok lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="1942e-213">Once data has been emitted, use the following steps to query the data.</span></span>

1. <span data-ttu-id="1942e-214">Lépjen vissza a **SessionInfo** projekt.</span><span class="sxs-lookup"><span data-stu-id="1942e-214">Return to the **SessionInfo** project.</span></span> <span data-ttu-id="1942e-215">Ha nem fut, indítsa el egy új példányát.</span><span class="sxs-lookup"><span data-stu-id="1942e-215">If not running, start a new instance of it.</span></span>

2. <span data-ttu-id="1942e-216">Amikor a rendszer kéri, válassza ki a **s** START esemény kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="1942e-216">When prompted, select **s** to search for START event.</span></span> <span data-ttu-id="1942e-217">Adjon meg egy időtartományt - meghatározásához kezdési és befejezési időt kéri ezek kétszer csak események adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1942e-217">You are prompted to enter a start and end time to define a time range - only events between these two times are returned.</span></span>

    <span data-ttu-id="1942e-218">A következő formátumot használja, ha a kezdő és záró időpont megadása: óó: PP és "vagyok" vagy "pm".</span><span class="sxs-lookup"><span data-stu-id="1942e-218">Use the following format when entering the start and end times: HH:MM and 'am' or 'pm'.</span></span> <span data-ttu-id="1942e-219">Ha például 23:20 óra.</span><span class="sxs-lookup"><span data-stu-id="1942e-219">For example, 11:20pm.</span></span>

    <span data-ttu-id="1942e-220">Naplózott események visszaállításához használja a kezdő időpont előtt a Storm-topológia telepítve lett, és most a befejezési ideje.</span><span class="sxs-lookup"><span data-stu-id="1942e-220">To return logged events, use a start time from before the Storm topology was deployed, and an end time of now.</span></span> <span data-ttu-id="1942e-221">A visszatérési adatai az alábbihoz hasonló bejegyzést tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="1942e-221">The return data contains entries similar to the following text:</span></span>

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

<span data-ttu-id="1942e-222">ZÁRÓ eseményeket keres ugyanúgy működik, mint START események.</span><span class="sxs-lookup"><span data-stu-id="1942e-222">Searching for END events works the same as START events.</span></span> <span data-ttu-id="1942e-223">Azt jelzi, hogy az indítási esemény után 1 és 5 perc között véletlenszerűen jönnek-azonban záró eseményeket.</span><span class="sxs-lookup"><span data-stu-id="1942e-223">However, END events are generated randomly between 1 and 5 minutes after the START event.</span></span> <span data-ttu-id="1942e-224">Előfordulhat, hogy néhány időtartományhoz segítségével megkeresheti a záró kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="1942e-224">You may have to try a few time ranges to find the END events.</span></span> <span data-ttu-id="1942e-225">ZÁRÓ eseményeket is tartalmazza az a munkamenet időtartama alatt - a KEZDÉSI időpontnál és befejezésének eseményt közötti különbség.</span><span class="sxs-lookup"><span data-stu-id="1942e-225">END events also contain the duration of the session - the difference between the START event time and END event time.</span></span> <span data-ttu-id="1942e-226">Itt látható egy példa az END eseményekre vonatkozó adatokat:</span><span class="sxs-lookup"><span data-stu-id="1942e-226">Here is an example of data for END events:</span></span>

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> <span data-ttu-id="1942e-227">Amíg megadott idő értékek nem a helyi idő, az a lekérdezés által visszaadott ideje UTC formátumban.</span><span class="sxs-lookup"><span data-stu-id="1942e-227">While the time values you enter are in local time, the time returned from the query is in UTC.</span></span>

## <a name="stop-the-topology"></a><span data-ttu-id="1942e-228">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="1942e-228">Stop the topology</span></span>

<span data-ttu-id="1942e-229">Ha készen áll a topológia leállítása, térjen vissza a **CorrelationTopology** projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="1942e-229">When you are ready to stop the topology, return to the **CorrelationTopology** project in Visual Studio.</span></span> <span data-ttu-id="1942e-230">Az a **Storm-topológia nézet**, válassza ki a topológiát, majd a **Kill** gomb a topológia e nézetében tetején.</span><span class="sxs-lookup"><span data-stu-id="1942e-230">In the **Storm Topology View**, select the topology and then use the **Kill** button at the top of the topology view.</span></span>

## <a name="delete-your-cluster"></a><span data-ttu-id="1942e-231">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="1942e-231">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="1942e-232">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1942e-232">Next steps</span></span>

<span data-ttu-id="1942e-233">További Storm, tekintse meg a [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="1942e-233">For more Storm examples, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
