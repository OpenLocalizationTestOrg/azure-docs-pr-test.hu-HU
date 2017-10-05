---
title: "Tez felhasználói felület használata a Windows-alapú HDInsight - Azure |} Microsoft Docs"
description: "Útmutató a Tez felhasználói felület használata a Windows-alapú HDInsight HDInsight-on Tez feladatokhoz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 3889fa1c3523eb0330cbe3b7640fd8590a5ceadf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a><span data-ttu-id="22721-103">A Tez felhasználói felület használata a Windows-alapú HDInsight-on Tez feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="22721-103">Use the Tez UI to debug Tez Jobs on Windows-based HDInsight</span></span>
<span data-ttu-id="22721-104">A Tez felhasználói felület egy olyan weblap, megértéséhez, valamint a Tez használják a Windows-alapú HDInsight-fürtök-végrehajtó motor feladatok debug használható.</span><span class="sxs-lookup"><span data-stu-id="22721-104">The Tez UI is a web page that can be used to understand and debug jobs that use Tez as the execution engine on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="22721-105">A Tez felhasználói felület lehetővé teszi a feladathoz egy grafikonon csatlakoztatott elemek megjelenítése, egyes elemek elemezze, és statisztika és naplózási információk lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="22721-105">The Tez UI allows you to visualize the job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22721-106">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Windows igényelnek.</span><span class="sxs-lookup"><span data-stu-id="22721-106">The steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="22721-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="22721-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="22721-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="22721-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22721-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="22721-109">Prerequisites</span></span>
* <span data-ttu-id="22721-110">Egy Windows-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="22721-110">A Windows-based HDInsight cluster.</span></span> <span data-ttu-id="22721-111">Az új fürt létrehozásának lépései: [Ismerkedés a Windows-alapú HDInsight eszközzel](hdinsight-hadoop-tutorial-get-started-windows.md).</span><span class="sxs-lookup"><span data-stu-id="22721-111">For steps on creating a new cluster, see [Get started using Windows-based HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="22721-112">A Tez felhasználói felületén a 2016. február 8. után létrehozott Windows-alapú HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="22721-112">The Tez UI is only available on Windows-based HDInsight clusters created after February 8th, 2016.</span></span>
  >
  >
* <span data-ttu-id="22721-113">A Windows-alapú távoli asztali ügyfél.</span><span class="sxs-lookup"><span data-stu-id="22721-113">A Windows-based Remote Desktop client.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="22721-114">Tez ismertetése</span><span class="sxs-lookup"><span data-stu-id="22721-114">Understanding Tez</span></span>
<span data-ttu-id="22721-115">Tez egy bővíthető keretrendszer, amely a hagyományos MapReduce feldolgozási-nál nagyobb sebesség biztosítja az adatok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="22721-115">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="22721-116">A Windows-alapú HDInsight-fürtök egy nem kötelező motor, amely a Hive-lekérdezés részeként a következő paranccsal engedélyezheti a Hive esetén:</span><span class="sxs-lookup"><span data-stu-id="22721-116">For Windows-based HDInsight clusters, it is an optional engine that you can enable for Hive by using the following command as part of your Hive query:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="22721-117">Amikor munkahelyi Tez, létrehoz egy irányított aciklikus diagramhoz (DAG), amely leírja a feladat által igényelt művelet végrehajtási sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="22721-117">When work is submitted to Tez, it creates a Directed Acyclic Graph (DAG) that describes the order of execution of the actions required by the job.</span></span> <span data-ttu-id="22721-118">Egyes műveletek csúcsban hívják, és egy adat, a teljes feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="22721-118">Individual actions are called vertices, and execute a piece of the overall job.</span></span> <span data-ttu-id="22721-119">A munkahelyi csúcspont szerint tényleges végrehajtása feladat neve, és előfordulhat, hogy a fürt több csomópontja legyen elosztva.</span><span class="sxs-lookup"><span data-stu-id="22721-119">The actual execution of the work described by a vertex is called a task, and may be distributed across multiple nodes in the cluster.</span></span>

### <a name="understanding-the-tez-ui"></a><span data-ttu-id="22721-120">A Tez felhasználói felület ismertetése</span><span class="sxs-lookup"><span data-stu-id="22721-120">Understanding the Tez UI</span></span>
<span data-ttu-id="22721-121">A Tez felhasználói felület egy weblap nyújt információkat folyamatok futó, és rendelkezik a korábban futtatott Tez használatával.</span><span class="sxs-lookup"><span data-stu-id="22721-121">The Tez UI is a web page provides information on processes that are running, or have previously ran using Tez.</span></span> <span data-ttu-id="22721-122">Lehetővé teszi a Tez, által generált DAG például a feladatok és a csúcsban és a hibainformációk által használt memória hogyan oszlik más fürtökre, teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="22721-122">It allows you to view the DAG generated by Tez, how it is distributed across clusters, counters such as memory used by tasks and vertices, and error information.</span></span> <span data-ttu-id="22721-123">Felajánlhatja, hogy a következő esetekben hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="22721-123">It may offer useful information in the following scenarios:</span></span>

* <span data-ttu-id="22721-124">Figyelési hosszan futó dolgozza fel a térkép állapotának megtekintése, és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="22721-124">Monitoring long-running processes, viewing the progress of map and reduce tasks.</span></span>
* <span data-ttu-id="22721-125">Megtudhatja, hogyan lehet javítani, feldolgozás, vagy sikertelenségének sikeres vagy sikertelen folyamatok előzményadatainak elemzése.</span><span class="sxs-lookup"><span data-stu-id="22721-125">Analyzing historical data for successful or failed processes to learn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="22721-126">Egy dag-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="22721-126">Generate a DAG</span></span>
<span data-ttu-id="22721-127">A Tez felhasználói felület csak fog adatokat tartalmazni, ha egy feladatot használ a Tez motor éppen fut, vagy már futott az elmúlt.</span><span class="sxs-lookup"><span data-stu-id="22721-127">The Tez UI will only contain data if a job that uses the Tez engine is currently running, or has been ran in the past.</span></span> <span data-ttu-id="22721-128">Egyszerű Hive-lekérdezések általában feloldható Tez, azonban összetett lekérdezések, amelyek szűrés, csoportosítás, rendezés, illesztéseket, stb. általában szükséges Tez használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="22721-128">Simple Hive queries can usually be resolved without using Tez, however more complex queries that do filtering, grouping, ordering, joins, etc. will usually require Tez.</span></span>

<span data-ttu-id="22721-129">A következő lépésekkel futtathat Hive-lekérdezéseket, amely végrehajtja a Tez használatával.</span><span class="sxs-lookup"><span data-stu-id="22721-129">Use the following steps to run a Hive query that will execute using Tez.</span></span>

1. <span data-ttu-id="22721-130">Egy böngészőben navigáljon a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** a HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="22721-130">In a web browser, navigate to https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="22721-131">Az oldal tetején a menüből válassza ki a **Hive szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="22721-131">From the menu at the top of the page, select the **Hive Editor**.</span></span> <span data-ttu-id="22721-132">Ekkor megjelenik egy oldal, a következő példa lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="22721-132">This will display a page with the following example query.</span></span>

        Select * from hivesampletable

    <span data-ttu-id="22721-133">A példa lekérdezés törléséhez, és cserélje le a következő.</span><span class="sxs-lookup"><span data-stu-id="22721-133">Erase the example query and replace it with the following.</span></span>

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. <span data-ttu-id="22721-134">Válassza ki a **Submit** gombra.</span><span class="sxs-lookup"><span data-stu-id="22721-134">Select the **Submit** button.</span></span> <span data-ttu-id="22721-135">A **feladat munkamenet** szakasz a lap alján a lekérdezés állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-135">The **Job Session** section at the bottom of the page will display the status of the query.</span></span> <span data-ttu-id="22721-136">Miután állapota **befejezve**, jelölje be a **részleteinek megtekintése** hivatkozásra az eredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="22721-136">Once the status changes to **Completed**, select the **View Details** link to view the results.</span></span> <span data-ttu-id="22721-137">A **Job Output** kell lennie a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="22721-137">The **Job Output** should be similar to the following:</span></span>

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a><span data-ttu-id="22721-138">A Tez felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="22721-138">Use the Tez UI</span></span>
> [!NOTE]
> <span data-ttu-id="22721-139">A Tez felhasználói felület érhető el csak az asztalról központi fürtcsomópont, ezért az átjárócsomópontokkal csatlakozni kell használnia a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="22721-139">The Tez UI is only available from the desktop of the cluster head nodes, so you must use Remote Desktop to connect to the head nodes.</span></span>
>
>

1. <span data-ttu-id="22721-140">Az a [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="22721-140">From the [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span> <span data-ttu-id="22721-141">A HDInsight panel felső részén válassza ki a **távoli asztal** ikonra.</span><span class="sxs-lookup"><span data-stu-id="22721-141">From the top of the HDInsight blade, select the **Remote Desktop** icon.</span></span> <span data-ttu-id="22721-142">Ez megjeleníti a távoli asztali panel</span><span class="sxs-lookup"><span data-stu-id="22721-142">This will display the remote desktop blade</span></span>

    ![Távoli asztali ikon](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. <span data-ttu-id="22721-144">A távoli asztal panelen válassza ki **Connect** az átjárócsomóponthoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="22721-144">From the Remote Desktop blade, select **Connect** to connect to the cluster head node.</span></span> <span data-ttu-id="22721-145">Amikor a rendszer kéri, használja a fürt távoli asztalhoz tartozó felhasználónév és jelszó a kapcsolat hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="22721-145">When prompted, use the cluster Remote Desktop user name and password to authenticate the connection.</span></span>

    ![Távoli asztali kapcsolat ikon](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > <span data-ttu-id="22721-147">Ha nem engedélyezte a távoli asztali kapcsolat, adjon meg egy felhasználónevet, jelszót és a lejárati dátumot, majd válasszon **engedélyezése** távoli asztal engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="22721-147">If you have not enabled Remote Desktop connectivity, provide a user name, password, and expiration date, then select **Enable** to enable Remote Desktop.</span></span> <span data-ttu-id="22721-148">Amennyiben engedélyezve van, használja az előző lépésekben való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="22721-148">Once it has been enabled, use the previous steps to connect.</span></span>
   >
   >
3. <span data-ttu-id="22721-149">A csatlakozás után nyissa meg az Internet Explorer, a távoli asztalon, jelölje ki a fogaskerék ikonra a képernyő jobb felső sarkában a böngészőt, és válassza **kompatibilitási nézet beállításai**.</span><span class="sxs-lookup"><span data-stu-id="22721-149">Once connected, open Internet Explorer on the remote desktop, select the gear icon in the upper right of the browser, and then select **Compatibility View Settings**.</span></span>
4. <span data-ttu-id="22721-150">Alján **kompatibilitási nézet beállításai**, törölje a jelölést az **kompatibilitási nézetben jeleníti meg az intranetes helyek** és **használata Microsoft kompatibilitási lista**, majd válassza ki **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="22721-150">From the bottom of **Compatibility View Settings**, clear the check box for **Display intranet sites in Compatibility View** and **Use Microsoft compatibility lists**, and then select **Close**.</span></span>
5. <span data-ttu-id="22721-151">Az Internet Explorer programban navigáljon a http://headnodehost:8188/tezui / #/.</span><span class="sxs-lookup"><span data-stu-id="22721-151">In Internet Explorer, browse to http://headnodehost:8188/tezui/#/.</span></span> <span data-ttu-id="22721-152">Ez megjeleníti a Tez felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="22721-152">This will display the Tez UI</span></span>

    ![Tez felhasználói felület](./media/hdinsight-debug-tez-ui/tezui.png)

    <span data-ttu-id="22721-154">A Tez felhasználói felület betöltésekor látni fogja, amely jelenleg fut, vagy dag listáját a fürtön futottak.</span><span class="sxs-lookup"><span data-stu-id="22721-154">When the Tez UI loads, you will see a list of DAGs that are currently running, or have been ran on the cluster.</span></span> <span data-ttu-id="22721-155">Az alapértelmezett nézet tartalmazza a Dag nevét, azonosítója, küldő, állapot, Kezdés időpontja, befejezési idő, időtartam, Alkalmazásazonosító, és várólista.</span><span class="sxs-lookup"><span data-stu-id="22721-155">The default view includes the Dag Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID, and Queue.</span></span> <span data-ttu-id="22721-156">További oszlopok is hozzáadhatók a fogaskerék ikonra a lap jobb használatával.</span><span class="sxs-lookup"><span data-stu-id="22721-156">More columns can be added using the gear icon at the right of the page.</span></span>

    <span data-ttu-id="22721-157">Ha csak egy bejegyzést, az előző szakaszban futtatott lekérdezés lesz.</span><span class="sxs-lookup"><span data-stu-id="22721-157">If you have only one entry, it will be for the query that you ran in the previous section.</span></span> <span data-ttu-id="22721-158">Ha több bejegyzés, továbbra is beírva a keresési feltételek mezőiben adatbázis-elérhetőségi csoportot, majd kattintson a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="22721-158">If you have multiple entries, you can search by entering search criteria in the fields above the DAGs, then hit **Enter**.</span></span>
6. <span data-ttu-id="22721-159">Válassza ki a **Dag neve** a legutóbbi DAG-bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="22721-159">Select the **Dag Name** for the most recent DAG entry.</span></span> <span data-ttu-id="22721-160">Ez megjeleníti a DAG kapcsolatos információkat, valamint egy zip a DAG információkat tartalmazó JSON-fájlok letöltése beállítás.</span><span class="sxs-lookup"><span data-stu-id="22721-160">This will display information about the DAG, as well as the option to download a zip of JSON files that contain information about the DAG.</span></span>

    ![DAG-részletek](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. <span data-ttu-id="22721-162">Fent a **DAG részletek** több kapcsolat, az adatbázis-elérhetőségi csoport kapcsolatos információk megjelenítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="22721-162">Above the **DAG Details** are several links that can be used to display information about the DAG.</span></span>

   * <span data-ttu-id="22721-163">**DAG-számlálók** a DAG számlálók adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-163">**DAG Counters** displays counters information for this DAG.</span></span>
   * <span data-ttu-id="22721-164">**Grafikus nézetének** jeleníti meg az adatbázis-elérhetőségi csoport grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="22721-164">**Graphical View** displays a graphical representation of this DAG.</span></span>
   * <span data-ttu-id="22721-165">**Minden csúcsban** a DAG a csúcsban listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-165">**All Vertices** displays a list of the vertices in this DAG.</span></span>
   * <span data-ttu-id="22721-166">**Minden feladat** az adatbázis-elérhetőségi csoport az összes csúcsban feladatok listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-166">**All Tasks** displays a list of the tasks for all vertices in this DAG.</span></span>
   * <span data-ttu-id="22721-167">**Minden TaskAttempts** a feladatok futtatása a DAG megpróbálja információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-167">**All TaskAttempts** displays information about the attempts to run tasks for this DAG.</span></span>

     > [!NOTE]
     > <span data-ttu-id="22721-168">Ha az oszlop megjelenítési csúcsban, feladatok és TaskAttempts, figyelje meg, hogy vannak-e hivatkozásra kattintva **számlálók** és **megtekintése vagy naplók letöltéséhez** minden egyes sorára.</span><span class="sxs-lookup"><span data-stu-id="22721-168">If you scroll the column display for Vertices, Tasks and TaskAttempts, notice that there are links to view **counters** and **view or download logs** for each row.</span></span>
     >
     >

     <span data-ttu-id="22721-169">Hiba történt a feladathoz, ha a DAG részleteit és a meghiúsult feladat kapcsolatos információkra mutató hivatkozásokat sikertelen, az állapot jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="22721-169">If there was a failure with the job, the DAG Details will display a status of FAILED, along with links to information about the failed task.</span></span> <span data-ttu-id="22721-170">Diagnosztikai adatok az adatbázis-elérhetőségi csoport részletek alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="22721-170">Diagnostics information will be displayed beneath the DAG details.</span></span>
8. <span data-ttu-id="22721-171">Válassza ki **grafikus nézetének**.</span><span class="sxs-lookup"><span data-stu-id="22721-171">Select **Graphical View**.</span></span> <span data-ttu-id="22721-172">Ez megjeleníti a DAG grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="22721-172">This displays a graphical representation of the DAG.</span></span> <span data-ttu-id="22721-173">Az egér elhelyezheti az információt szeretne megjeleníteni a nézetben minden csomópont alatt.</span><span class="sxs-lookup"><span data-stu-id="22721-173">You can place the mouse over each vertex in the view to display information about it.</span></span>

    ![Grafikus megtekintése](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. <span data-ttu-id="22721-175">Kattintson a csúcspont betölti a **csúcspont részletek** , a cikk.</span><span class="sxs-lookup"><span data-stu-id="22721-175">Clicking on a vertex will load the **Vertex Details** for that item.</span></span> <span data-ttu-id="22721-176">Kattintson a **térkép 1** csúcspont ezt az elemet a részletek megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="22721-176">Click on the **Map 1** vertex to display details for this item.</span></span> <span data-ttu-id="22721-177">Válassza ki **megerősítése** a navigációs megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="22721-177">Select **Confirm** to confirm the navigation.</span></span>

    ![Csomópont részletei](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. <span data-ttu-id="22721-179">Vegye figyelembe, hogy most már rendelkezik csúcsban és-feladatokhoz kapcsolódó hivatkozások az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="22721-179">Note that you now have links at the top of the page that are related to vertices and tasks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22721-180">Is is megérkezik a ezen a lapon térhet vissza a **DAG részletek**, kiválasztásával **csúcspont részletek**, és jelölje be a **térkép 1** csúcspont.</span><span class="sxs-lookup"><span data-stu-id="22721-180">You can also arrive at this page by going back to **DAG Details**, selecting **Vertex Details**, and then selecting the **Map 1** vertex.</span></span>
    >
    >

    * <span data-ttu-id="22721-181">**Csúcspont számlálók** a csúcspont számláló adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-181">**Vertex Counters** displays counter information for this vertex.</span></span>
    * <span data-ttu-id="22721-182">**Feladatok** a csúcspont feladatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-182">**Tasks** displays tasks for this vertex.</span></span>
    * <span data-ttu-id="22721-183">**Feladat kísérletek** kísérletek a csúcspont feladatok futtatásához információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="22721-183">**Task Attempts** displays information about attempts to run tasks for this vertex.</span></span>
    * <span data-ttu-id="22721-184">**Adatforrások & fogadók esetében** adatforrások jeleníti meg, és ez a csomópont a fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="22721-184">**Sources & Sinks** displays data sources and sinks for this vertex.</span></span>

      > [!NOTE]
      > <span data-ttu-id="22721-185">Mint az előző menüvel görgetve az oszlop megjelenítési feladatok, feladat kísérletet, és a források & a Sinks__ további információt az egyes elemekhez tartozó mutató hivatkozások megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="22721-185">As with the previous menu, you can scroll the column display for Tasks, Task Attempts, and Sources & Sinks__ to display links to more information for each item.</span></span>
      >
      >
11. <span data-ttu-id="22721-186">Válassza ki **feladatok**, és válassza ki a nevű elem **00_000000**.</span><span class="sxs-lookup"><span data-stu-id="22721-186">Select **Tasks**, and then select the item named **00_000000**.</span></span> <span data-ttu-id="22721-187">Ez megjeleníti **feladat részletei** ehhez a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="22721-187">This will display **Task Details** for this task.</span></span> <span data-ttu-id="22721-188">Ez a képernyő megtekintheti **feladat számlálók** és **feladat kísérletek**.</span><span class="sxs-lookup"><span data-stu-id="22721-188">From this screen, you can view **Task Counters** and **Task Attempts**.</span></span>

    ![Feladat részletei](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a><span data-ttu-id="22721-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="22721-190">Next Steps</span></span>
<span data-ttu-id="22721-191">Most, hogy rendelkezik megtudta, hogyan használja a Tez, további információ [használata a HDInsight Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="22721-191">Now that you have learned how to use the Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="22721-192">Részletesebb műszaki információkat Tez, lásd: a [Hortonworks Tez lapon](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="22721-192">For more detailed technical information on Tez, see the [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>
