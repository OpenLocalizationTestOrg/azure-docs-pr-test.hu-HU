---
title: "Tez felhasználói felület Windows-alapú HDInsight - Azure aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Tez felhasználói felület toodebug Tez a Windows-alapú HDInsight HDInsight feladatokat."
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
ms.openlocfilehash: 7ae21242ee1f8dc34a8501bed1ca995480885540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-tez-ui-toodebug-tez-jobs-on-windows-based-hdinsight"></a><span data-ttu-id="7cc77-103">A Windows-alapú HDInsight Tez felhasználói felület toodebug Tez feladatokhoz hello használata</span><span class="sxs-lookup"><span data-stu-id="7cc77-103">Use hello Tez UI toodebug Tez Jobs on Windows-based HDInsight</span></span>
<span data-ttu-id="7cc77-104">Tez felhasználói felület hello egy weblap, amelyen a Tez használják hello végrehajtó motor a Windows-alapú HDInsight-fürtökön használt toounderstand és hibakeresési feladatokat is lehet.</span><span class="sxs-lookup"><span data-stu-id="7cc77-104">hello Tez UI is a web page that can be used toounderstand and debug jobs that use Tez as hello execution engine on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="7cc77-105">hello Tez felhasználói felület lehetővé teszi toovisualize hello feladat csatlakoztatott elemek grafikon elemezze az egyes elemek, valamint statisztikák és naplózási információk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7cc77-105">hello Tez UI allows you toovisualize hello job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7cc77-106">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Windows igényelnek.</span><span class="sxs-lookup"><span data-stu-id="7cc77-106">hello steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="7cc77-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="7cc77-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7cc77-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7cc77-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7cc77-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7cc77-109">Prerequisites</span></span>
* <span data-ttu-id="7cc77-110">Egy Windows-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="7cc77-110">A Windows-based HDInsight cluster.</span></span> <span data-ttu-id="7cc77-111">Az új fürt létrehozásának lépései: [Ismerkedés a Windows-alapú HDInsight eszközzel](hdinsight-hadoop-tutorial-get-started-windows.md).</span><span class="sxs-lookup"><span data-stu-id="7cc77-111">For steps on creating a new cluster, see [Get started using Windows-based HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="7cc77-112">hello Tez felhasználói felület a 2016. február 8. után létrehozott Windows-alapú HDInsight-fürtök csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="7cc77-112">hello Tez UI is only available on Windows-based HDInsight clusters created after February 8th, 2016.</span></span>
  >
  >
* <span data-ttu-id="7cc77-113">A Windows-alapú távoli asztali ügyfél.</span><span class="sxs-lookup"><span data-stu-id="7cc77-113">A Windows-based Remote Desktop client.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="7cc77-114">Tez ismertetése</span><span class="sxs-lookup"><span data-stu-id="7cc77-114">Understanding Tez</span></span>
<span data-ttu-id="7cc77-115">Tez egy bővíthető keretrendszer, amely a hagyományos MapReduce feldolgozási-nál nagyobb sebesség biztosítja az adatok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="7cc77-115">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="7cc77-116">A Windows-alapú HDInsight-fürtök egy nem kötelező motor, amely a Hive engedélyezheti a következő parancsot a Hive-lekérdezés részeként hello esetén:</span><span class="sxs-lookup"><span data-stu-id="7cc77-116">For Windows-based HDInsight clusters, it is an optional engine that you can enable for Hive by using hello following command as part of your Hive query:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="7cc77-117">Elküldött tooTez munka esetén létrehoz egy irányított aciklikus diagramhoz (DAG), amely leírja a hello sorrendjének hello feladat által igényelt hello műveletek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="7cc77-117">When work is submitted tooTez, it creates a Directed Acyclic Graph (DAG) that describes hello order of execution of hello actions required by hello job.</span></span> <span data-ttu-id="7cc77-118">Egyes műveletek csúcsban nevezik, és hajtható végre egy hello adat általános feladat.</span><span class="sxs-lookup"><span data-stu-id="7cc77-118">Individual actions are called vertices, and execute a piece of hello overall job.</span></span> <span data-ttu-id="7cc77-119">hello tényleges csúcspont szerint hello munkaelem végrehajtásának feladat neve és hello fürt több csomópontja között szétoszthatók.</span><span class="sxs-lookup"><span data-stu-id="7cc77-119">hello actual execution of hello work described by a vertex is called a task, and may be distributed across multiple nodes in hello cluster.</span></span>

### <a name="understanding-hello-tez-ui"></a><span data-ttu-id="7cc77-120">Understanding hello Tez felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="7cc77-120">Understanding hello Tez UI</span></span>
<span data-ttu-id="7cc77-121">Tez felhasználói felület hello egy weblap nyújt információkat folyamatok futó, és rendelkezik a korábban futtatott Tez használatával.</span><span class="sxs-lookup"><span data-stu-id="7cc77-121">hello Tez UI is a web page provides information on processes that are running, or have previously ran using Tez.</span></span> <span data-ttu-id="7cc77-122">Tez, által generált DAG tooview hello lehetővé teszi például a feladatok és a csúcsban és a hibainformációk által használt memória hogyan oszlik más fürtökre, teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="7cc77-122">It allows you tooview hello DAG generated by Tez, how it is distributed across clusters, counters such as memory used by tasks and vertices, and error information.</span></span> <span data-ttu-id="7cc77-123">Felajánlhatja, hogy a következő forgatókönyvek hello hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="7cc77-123">It may offer useful information in hello following scenarios:</span></span>

* <span data-ttu-id="7cc77-124">Hosszan futó folyamatok figyelése, megtekintése hello térkép előrehaladását, és csökkentheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="7cc77-124">Monitoring long-running processes, viewing hello progress of map and reduce tasks.</span></span>
* <span data-ttu-id="7cc77-125">Sikeres vagy sikertelen folyamatok toolearn előzményadatainak elemzése, hogyan lehet javítani, feldolgozás, vagy okát.</span><span class="sxs-lookup"><span data-stu-id="7cc77-125">Analyzing historical data for successful or failed processes toolearn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="7cc77-126">Egy dag-csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="7cc77-126">Generate a DAG</span></span>
<span data-ttu-id="7cc77-127">hello Tez felhasználói felület csak fog adatokat tartalmazni, ha egy feladat által használt hello Tez motor éppen fut, vagy rendelkezik már futott az elmúlt hello.</span><span class="sxs-lookup"><span data-stu-id="7cc77-127">hello Tez UI will only contain data if a job that uses hello Tez engine is currently running, or has been ran in hello past.</span></span> <span data-ttu-id="7cc77-128">Egyszerű Hive-lekérdezések általában feloldható Tez, azonban összetett lekérdezések, amelyek szűrés, csoportosítás, rendezés, illesztéseket, stb. általában szükséges Tez használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="7cc77-128">Simple Hive queries can usually be resolved without using Tez, however more complex queries that do filtering, grouping, ordering, joins, etc. will usually require Tez.</span></span>

<span data-ttu-id="7cc77-129">A következő lépéseket toorun, amely végrehajtja a használatával Tez Hive-lekérdezések hello használata.</span><span class="sxs-lookup"><span data-stu-id="7cc77-129">Use hello following steps toorun a Hive query that will execute using Tez.</span></span>

1. <span data-ttu-id="7cc77-130">Egy böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello neve, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7cc77-130">In a web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="7cc77-131">Hello oldal hello tetején hello menüben válassza ki a hello **Hive szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-131">From hello menu at hello top of hello page, select hello **Hive Editor**.</span></span> <span data-ttu-id="7cc77-132">Ekkor megjelenik egy lap a következő példalekérdezés hello.</span><span class="sxs-lookup"><span data-stu-id="7cc77-132">This will display a page with hello following example query.</span></span>

        Select * from hivesampletable

    <span data-ttu-id="7cc77-133">Hello példalekérdezés törlésére, és cserélje le a következő hello.</span><span class="sxs-lookup"><span data-stu-id="7cc77-133">Erase hello example query and replace it with hello following.</span></span>

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. <span data-ttu-id="7cc77-134">Jelölje be hello **Submit** gombra.</span><span class="sxs-lookup"><span data-stu-id="7cc77-134">Select hello **Submit** button.</span></span> <span data-ttu-id="7cc77-135">Hello **feladat munkamenet** szakasz alján hello hello hello lekérdezés hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-135">hello **Job Session** section at hello bottom of hello page will display hello status of hello query.</span></span> <span data-ttu-id="7cc77-136">Egyszer hello túl állapotmódosítások**befejezve**, jelölje be hello **részleteinek megtekintése** tooview hello eredmények hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="7cc77-136">Once hello status changes too**Completed**, select hello **View Details** link tooview hello results.</span></span> <span data-ttu-id="7cc77-137">Hello **Job Output** hasonló toohello következő legyen:</span><span class="sxs-lookup"><span data-stu-id="7cc77-137">hello **Job Output** should be similar toohello following:</span></span>

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-hello-tez-ui"></a><span data-ttu-id="7cc77-138">Hello Tez felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="7cc77-138">Use hello Tez UI</span></span>
> [!NOTE]
> <span data-ttu-id="7cc77-139">hello Tez felhasználói felület csak akkor hello asztali hello központi fürtcsomópont, így a távoli asztal tooconnect toohello átjárócsomópontokkal kell használni.</span><span class="sxs-lookup"><span data-stu-id="7cc77-139">hello Tez UI is only available from hello desktop of hello cluster head nodes, so you must use Remote Desktop tooconnect toohello head nodes.</span></span>
>
>

1. <span data-ttu-id="7cc77-140">A hello [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7cc77-140">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span> <span data-ttu-id="7cc77-141">Hello hello HDInsight panel felső részén jelölje ki hello **távoli asztal** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7cc77-141">From hello top of hello HDInsight blade, select hello **Remote Desktop** icon.</span></span> <span data-ttu-id="7cc77-142">Megjelenik az hello távoli asztali panel</span><span class="sxs-lookup"><span data-stu-id="7cc77-142">This will display hello remote desktop blade</span></span>

    ![Távoli asztali ikon](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. <span data-ttu-id="7cc77-144">Hello távoli asztal panelen válassza ki **Connect** tooconnect toohello fürt átjárócsomópontjából.</span><span class="sxs-lookup"><span data-stu-id="7cc77-144">From hello Remote Desktop blade, select **Connect** tooconnect toohello cluster head node.</span></span> <span data-ttu-id="7cc77-145">Amikor a rendszer kéri, hello fürt távoli asztali felhasználói nevet és jelszót tooauthenticate hello kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="7cc77-145">When prompted, use hello cluster Remote Desktop user name and password tooauthenticate hello connection.</span></span>

    ![Távoli asztali kapcsolat ikon](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > <span data-ttu-id="7cc77-147">Ha nem engedélyezte a távoli asztali kapcsolat, adjon meg egy felhasználónevet, jelszót és a lejárati dátumot, majd válasszon **engedélyezése** tooenable távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="7cc77-147">If you have not enabled Remote Desktop connectivity, provide a user name, password, and expiration date, then select **Enable** tooenable Remote Desktop.</span></span> <span data-ttu-id="7cc77-148">Ha engedélyezve van, a hello előző lépéseket tooconnect.</span><span class="sxs-lookup"><span data-stu-id="7cc77-148">Once it has been enabled, use hello previous steps tooconnect.</span></span>
   >
   >
3. <span data-ttu-id="7cc77-149">A csatlakozás után nyissa meg az Internet Explorer hello távoli asztal, hello jobb felső sarkában hello böngésző válassza hello fogaskerék ikonra, és válassza **kompatibilitási nézet beállításai**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-149">Once connected, open Internet Explorer on hello remote desktop, select hello gear icon in hello upper right of hello browser, and then select **Compatibility View Settings**.</span></span>
4. <span data-ttu-id="7cc77-150">Hello aljáról **kompatibilitási nézet beállításai**, törölje a jelet hello jelölőnégyzetét **kompatibilitási nézetben jeleníti meg az intranetes helyek** és **használata Microsoft kompatibilitási lista**, Válassza ki és **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-150">From hello bottom of **Compatibility View Settings**, clear hello check box for **Display intranet sites in Compatibility View** and **Use Microsoft compatibility lists**, and then select **Close**.</span></span>
5. <span data-ttu-id="7cc77-151">Az Internet Explorerben, keresse meg a toohttp://headnodehost:8188/tezui / #/.</span><span class="sxs-lookup"><span data-stu-id="7cc77-151">In Internet Explorer, browse toohttp://headnodehost:8188/tezui/#/.</span></span> <span data-ttu-id="7cc77-152">Megjelenik az hello Tez felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="7cc77-152">This will display hello Tez UI</span></span>

    ![Tez felhasználói felület](./media/hdinsight-debug-tez-ui/tezui.png)

    <span data-ttu-id="7cc77-154">Tez felhasználói felület hello betöltésekor látni fogja hello fürt futott, amely jelenleg fut, vagy dag listáját.</span><span class="sxs-lookup"><span data-stu-id="7cc77-154">When hello Tez UI loads, you will see a list of DAGs that are currently running, or have been ran on hello cluster.</span></span> <span data-ttu-id="7cc77-155">hello az alapértelmezett nézet tartozik hello adatbázis-elérhetőségi csoport neve, azonosítója, küldő, állapot, Kezdés időpontja, befejezési idő, időtartam, Alkalmazásazonosító, és a várólista.</span><span class="sxs-lookup"><span data-stu-id="7cc77-155">hello default view includes hello Dag Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID, and Queue.</span></span> <span data-ttu-id="7cc77-156">További oszlopok is hozzáadhatók hello sarkában hello lap segítségével hello fogaskerék ikonra.</span><span class="sxs-lookup"><span data-stu-id="7cc77-156">More columns can be added using hello gear icon at hello right of hello page.</span></span>

    <span data-ttu-id="7cc77-157">Ha csak egy bejegyzést, az előző szakaszban hello futtatott hello lekérdezés lesz.</span><span class="sxs-lookup"><span data-stu-id="7cc77-157">If you have only one entry, it will be for hello query that you ran in hello previous section.</span></span> <span data-ttu-id="7cc77-158">Ha több bejegyzés, továbbra is beírva a keresési feltételek fent hello dag hello mezőket, majd kattintson a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-158">If you have multiple entries, you can search by entering search criteria in hello fields above hello DAGs, then hit **Enter**.</span></span>
6. <span data-ttu-id="7cc77-159">Jelölje be hello **Dag neve** hello legutóbbi DAG bejegyzéshez.</span><span class="sxs-lookup"><span data-stu-id="7cc77-159">Select hello **Dag Name** for hello most recent DAG entry.</span></span> <span data-ttu-id="7cc77-160">Ez megjeleníti hello DAG, valamint hello beállítás toodownload hello DAG információkat tartalmazó JSON-fájlok zip kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="7cc77-160">This will display information about hello DAG, as well as hello option toodownload a zip of JSON files that contain information about hello DAG.</span></span>

    ![DAG-részletek](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. <span data-ttu-id="7cc77-162">Hello fent **DAG részletek** több hivatkozást, amely lehet hello DAG használt toodisplay információ.</span><span class="sxs-lookup"><span data-stu-id="7cc77-162">Above hello **DAG Details** are several links that can be used toodisplay information about hello DAG.</span></span>

   * <span data-ttu-id="7cc77-163">**DAG-számlálók** a DAG számlálók adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-163">**DAG Counters** displays counters information for this DAG.</span></span>
   * <span data-ttu-id="7cc77-164">**Grafikus nézetének** jeleníti meg az adatbázis-elérhetőségi csoport grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7cc77-164">**Graphical View** displays a graphical representation of this DAG.</span></span>
   * <span data-ttu-id="7cc77-165">**Minden csúcsban** a DAG hello csúcsban listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-165">**All Vertices** displays a list of hello vertices in this DAG.</span></span>
   * <span data-ttu-id="7cc77-166">**Minden feladat** a DAG összes csúcsban hello feladatok listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-166">**All Tasks** displays a list of hello tasks for all vertices in this DAG.</span></span>
   * <span data-ttu-id="7cc77-167">**Minden TaskAttempts** hello részletes információit jeleníti meg az adatbázis-elérhetőségi csoport kísérletek toorun feladatok.</span><span class="sxs-lookup"><span data-stu-id="7cc77-167">**All TaskAttempts** displays information about hello attempts toorun tasks for this DAG.</span></span>

     > [!NOTE]
     > <span data-ttu-id="7cc77-168">Ha hello oszlop megjelenítési csúcsban, feladatok és TaskAttempts, figyelje meg, hogy nincsenek hivatkozásait tooview **számlálók** és **megtekintése vagy naplók letöltéséhez** minden egyes sorára.</span><span class="sxs-lookup"><span data-stu-id="7cc77-168">If you scroll hello column display for Vertices, Tasks and TaskAttempts, notice that there are links tooview **counters** and **view or download logs** for each row.</span></span>
     >
     >

     <span data-ttu-id="7cc77-169">Hiba történt a hello feldolgozással, ha a hello DAG részletek együtt hivatkozások tooinformation hello sikertelen feladathoz sikertelen, az állapot jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-169">If there was a failure with hello job, hello DAG Details will display a status of FAILED, along with links tooinformation about hello failed task.</span></span> <span data-ttu-id="7cc77-170">Diagnosztikai adatok hello DAG Részletek alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-170">Diagnostics information will be displayed beneath hello DAG details.</span></span>
8. <span data-ttu-id="7cc77-171">Válassza ki **grafikus nézetének**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-171">Select **Graphical View**.</span></span> <span data-ttu-id="7cc77-172">Ez megjeleníti a hello DAG grafikus ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7cc77-172">This displays a graphical representation of hello DAG.</span></span> <span data-ttu-id="7cc77-173">Minden csomópont hello toodisplay adatainak megtekintése, a hello egér is helyezi.</span><span class="sxs-lookup"><span data-stu-id="7cc77-173">You can place hello mouse over each vertex in hello view toodisplay information about it.</span></span>

    ![Grafikus megtekintése](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. <span data-ttu-id="7cc77-175">Kattintson a csúcspont betölti a hello **csúcspont részletek** , a cikk.</span><span class="sxs-lookup"><span data-stu-id="7cc77-175">Clicking on a vertex will load hello **Vertex Details** for that item.</span></span> <span data-ttu-id="7cc77-176">Kattintson a hello **térkép 1** csúcspont toodisplay részletek ehhez az elemhez.</span><span class="sxs-lookup"><span data-stu-id="7cc77-176">Click on hello **Map 1** vertex toodisplay details for this item.</span></span> <span data-ttu-id="7cc77-177">Válassza ki **megerősítése** tooconfirm hello navigációs.</span><span class="sxs-lookup"><span data-stu-id="7cc77-177">Select **Confirm** tooconfirm hello navigation.</span></span>

    ![Csomópont részletei](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. <span data-ttu-id="7cc77-179">Vegye figyelembe, hogy most már rendelkezik, amelyek kapcsolódó toovertices és feladatok hivatkozások hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="7cc77-179">Note that you now have links at hello top of hello page that are related toovertices and tasks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7cc77-180">Is is megérkezik a ezen a lapon térhet vissza túl**DAG részletek**választ, **csúcspont részletek**, és jelölje be hello **térkép 1** csúcspont.</span><span class="sxs-lookup"><span data-stu-id="7cc77-180">You can also arrive at this page by going back too**DAG Details**, selecting **Vertex Details**, and then selecting hello **Map 1** vertex.</span></span>
    >
    >

    * <span data-ttu-id="7cc77-181">**Csúcspont számlálók** a csúcspont számláló adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-181">**Vertex Counters** displays counter information for this vertex.</span></span>
    * <span data-ttu-id="7cc77-182">**Feladatok** a csúcspont feladatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-182">**Tasks** displays tasks for this vertex.</span></span>
    * <span data-ttu-id="7cc77-183">**Feladat kísérletek** kísérletek toorun feladatok a csúcspont információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7cc77-183">**Task Attempts** displays information about attempts toorun tasks for this vertex.</span></span>
    * <span data-ttu-id="7cc77-184">**Adatforrások & fogadók esetében** adatforrások jeleníti meg, és ez a csomópont a fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="7cc77-184">**Sources & Sinks** displays data sources and sinks for this vertex.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7cc77-185">Hello előző menüben is görgetésekor hello oszlop megjelenítési feladatokhoz, a feladat kísérletet, és a források & Sinks__ toodisplay minden elem toomore adatai hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cc77-185">As with hello previous menu, you can scroll hello column display for Tasks, Task Attempts, and Sources & Sinks__ toodisplay links toomore information for each item.</span></span>
      >
      >
11. <span data-ttu-id="7cc77-186">Válassza ki **feladatok**, majd jelölje be hello elem nevű és **00_000000**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-186">Select **Tasks**, and then select hello item named **00_000000**.</span></span> <span data-ttu-id="7cc77-187">Ez megjeleníti **feladat részletei** ehhez a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="7cc77-187">This will display **Task Details** for this task.</span></span> <span data-ttu-id="7cc77-188">Ez a képernyő megtekintheti **feladat számlálók** és **feladat kísérletek**.</span><span class="sxs-lookup"><span data-stu-id="7cc77-188">From this screen, you can view **Task Counters** and **Task Attempts**.</span></span>

    ![Feladat részletei](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a><span data-ttu-id="7cc77-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cc77-190">Next Steps</span></span>
<span data-ttu-id="7cc77-191">Most, hogy megtanulta, hogyan toouse hello Tez meg, további információ [használata a HDInsight Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="7cc77-191">Now that you have learned how toouse hello Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="7cc77-192">Részletesebb műszaki információkat Tez, lásd: hello [Hortonworks Tez lapon](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="7cc77-192">For more detailed technical information on Tez, see hello [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>
