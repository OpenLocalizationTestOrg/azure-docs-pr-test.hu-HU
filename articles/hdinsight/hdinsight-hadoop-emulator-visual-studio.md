---
title: "A Data Lake tools a Hortonworks védőfal - Azure HDInsight Visual Studio |} Microsoft Docs"
description: "További tudnivalók az Azure Data Lake tools for Visual Studio használata a helyi virtuális gépen futó Hortonworks védőfal. Ezekkel az eszközökkel hozhat létre, és a a védőfal, és a nézet feladatkiemenetét és a korábbi Hive és a Pig feladatok futtatásához."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 574ccaa8b2d9448a60ddf8adc7f92fa3683b1d61
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a><span data-ttu-id="cb040-104">Az Azure Data Lake tools használja a Visual Studio és a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="cb040-104">Use the Azure Data Lake tools for Visual Studio with the Hortonworks Sandbox</span></span>

<span data-ttu-id="cb040-105">Azure Data Lake az általános Hadoop-fürtök használata eszközöket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cb040-105">Azure Data Lake includes tools for working with generic Hadoop clusters.</span></span> <span data-ttu-id="cb040-106">Ez a dokumentum nyújt a Data Lake tools használata a Hortonworks védőfal szükséges lépéseket a helyi virtuális gépen futó.</span><span class="sxs-lookup"><span data-stu-id="cb040-106">This document provides the steps needed to use the Data Lake tools with the Hortonworks Sandbox running in a local virtual machine.</span></span>

<span data-ttu-id="cb040-107">A a Hortonworks védőfal lehetővé teszi, hogy helyileg dolgozik hadooppal a fejlesztési környezet.</span><span class="sxs-lookup"><span data-stu-id="cb040-107">Using the Hortonworks Sandbox allows you to work with Hadoop locally on your development environment.</span></span> <span data-ttu-id="cb040-108">Ha olyan megoldást fejlesztett ki, és léptékű telepíteni, majd áthelyezheti egy HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cb040-108">After you have developed a solution and want to deploy it at scale, you can then move to an HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb040-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cb040-109">Prerequisites</span></span>

* <span data-ttu-id="cb040-110">A Hortonworks védőfal, a virtuális gépen futó a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="cb040-110">The Hortonworks Sandbox, running in a virtual machine on your development environment.</span></span> <span data-ttu-id="cb040-111">Ez a dokumentum lett írva, és a védőfal Oracle VirtualBox-beli tesztelése.</span><span class="sxs-lookup"><span data-stu-id="cb040-111">This document was written and tested with the sandbox running in Oracle VirtualBox.</span></span> <span data-ttu-id="cb040-112">A védőfal beállításával kapcsolatos információkért lásd: a [Ismerkedjen meg a Hortonworks védőfal.](hdinsight-hadoop-emulator-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="cb040-112">For information on setting up the sandbox, see the [Get started with the Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span></span> <span data-ttu-id="cb040-113">a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cb040-113">document.</span></span>

* <span data-ttu-id="cb040-114">A Visual Studio 2013, Visual Studio 2015-öt vagy a Visual Studio 2017 (minden kiadás).</span><span class="sxs-lookup"><span data-stu-id="cb040-114">Visual Studio 2013, Visual Studio 2015, or Visual Studio 2017 (any edition).</span></span>

* <span data-ttu-id="cb040-115">A [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="cb040-115">The [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 or later.</span></span>

* <span data-ttu-id="cb040-116">A [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="cb040-116">The [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="configure-passwords-for-the-sandbox"></a><span data-ttu-id="cb040-117">A védőfal jelszavak beállítása</span><span class="sxs-lookup"><span data-stu-id="cb040-117">Configure passwords for the sandbox</span></span>

<span data-ttu-id="cb040-118">Győződjön meg arról, hogy fut-e a Hortonworks védőfal.</span><span class="sxs-lookup"><span data-stu-id="cb040-118">Make sure that the Hortonworks Sandbox is running.</span></span> <span data-ttu-id="cb040-119">Ezután kövesse a [Ismerkedés a Hortonworks védőfal](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cb040-119">Then follow the steps in the [Get started in the Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) document.</span></span> <span data-ttu-id="cb040-120">Ezeket a lépéseket a jelszó beállítása az SSH `root` fiókot, és az Ambari `admin` fiók.</span><span class="sxs-lookup"><span data-stu-id="cb040-120">These steps configure the password for the SSH `root` account, and the Ambari `admin` account.</span></span> <span data-ttu-id="cb040-121">Ezek a jelszavak használják, a Visual Studio eszközből a védőfal való csatlakozás esetén.</span><span class="sxs-lookup"><span data-stu-id="cb040-121">These passwords are used when you connect to the sandbox from Visual Studio.</span></span>

## <a name="connect-the-tools-to-the-sandbox"></a><span data-ttu-id="cb040-122">Az eszközök csatlakozni a védőfal</span><span class="sxs-lookup"><span data-stu-id="cb040-122">Connect the tools to the sandbox</span></span>

1. <span data-ttu-id="cb040-123">Nyissa meg a Visual Studio, válassza ki **nézet**, majd válassza ki **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="cb040-123">Open Visual Studio, select **View**, and then select **Server Explorer**.</span></span>

2. <span data-ttu-id="cb040-124">A **Server Explorer**, kattintson a jobb gombbal a **HDInsight** bejegyzést, és válassza **HDInsight emulátoron kapcsolódás**.</span><span class="sxs-lookup"><span data-stu-id="cb040-124">From **Server Explorer**, right-click the **HDInsight** entry, and then select **Connect to HDInsight Emulator**.</span></span>

    ![Képernyőfelvétel a Server Explorer eszközben a Kapcsolódás a HDInsight emulátoron kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. <span data-ttu-id="cb040-126">Az a **HDInsight emulátoron kapcsolódás** párbeszédpanelen adja meg a jelszót az Ambari konfigurált.</span><span class="sxs-lookup"><span data-stu-id="cb040-126">From the **Connect to HDInsight Emulator** dialog box, enter the password that you configured for Ambari.</span></span>

    ![A kijelölt mezőbe a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    <span data-ttu-id="cb040-128">Válassza ki **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="cb040-128">Select **Next** to continue.</span></span>

4. <span data-ttu-id="cb040-129">Használja a **jelszó** mezőben adja meg a jelszót a konfigurálta a `root` fiók.</span><span class="sxs-lookup"><span data-stu-id="cb040-129">Use the **Password** field to enter the password you configured for the `root` account.</span></span> <span data-ttu-id="cb040-130">Az alapértelmezett értéket a többi mezőt hagyja.</span><span class="sxs-lookup"><span data-stu-id="cb040-130">Leave the other fields at the default value.</span></span>

    ![A kijelölt mezőbe a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    <span data-ttu-id="cb040-132">Válassza ki **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="cb040-132">Select **Next** to continue.</span></span>

5. <span data-ttu-id="cb040-133">Várjon, amíg befejeződik a szolgáltatások érvényesítéshez.</span><span class="sxs-lookup"><span data-stu-id="cb040-133">Wait for validation of the services to finish.</span></span> <span data-ttu-id="cb040-134">Bizonyos esetekben a érvényesítése sikertelen, és felszólítja a konfiguráció frissítése.</span><span class="sxs-lookup"><span data-stu-id="cb040-134">In some cases, validation may fail and prompt you to update the configuration.</span></span> <span data-ttu-id="cb040-135">Ha az érvényesítés meghiúsul, válassza ki a **frissítés**, és várja meg, a konfiguráció és az ellenőrzés befejezéséhez a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cb040-135">If validation fails, select **Update**, and wait for the configuration and verification for the service to finish.</span></span>

    ![A kijelölt frissítés gombra a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > <span data-ttu-id="cb040-137">A frissítési folyamat Ambari használatával módosíthatja a Hortonworks védőfal konfigurációját, és mi az elvárás szerint a Data Lake tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb040-137">The update process uses Ambari to modify the Hortonworks Sandbox configuration to what is expected by the Data Lake tools for Visual Studio.</span></span>

6. <span data-ttu-id="cb040-138">Érvényesítési befejeződését követően válassza ki a **Befejezés** konfigurálásának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb040-138">After validation has finished, select **Finish** to complete configuration.</span></span>
    <span data-ttu-id="cb040-139">![A kijelölt a Befejezés gombra a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span><span class="sxs-lookup"><span data-stu-id="cb040-139">![Screenshot of dialog box, with Finish button highlighted](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span></span>

     >[!NOTE]
     > <span data-ttu-id="cb040-140">Attól függően, hogy a fejlesztési környezetet, és a virtuális gép számára lefoglalt memória sebességétől konfigurálása, és ellenőrizze a szolgáltatások több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="cb040-140">Depending on the speed of your development environment, and the amount of memory allocated to the virtual machine, it can take several minutes to configure and validate the services.</span></span>

<span data-ttu-id="cb040-141">Ezeket a lépéseket követve után most már rendelkezik egy **helyi HDInsight-fürt** a Server Explorer eszközben bejegyzés alatt a **HDInsight** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cb040-141">After following these steps, you now have an **HDInsight local cluster** entry in Server Explorer, under the **HDInsight** section.</span></span>

## <a name="write-a-hive-query"></a><span data-ttu-id="cb040-142">Hive-lekérdezések írása</span><span class="sxs-lookup"><span data-stu-id="cb040-142">Write a Hive query</span></span>

<span data-ttu-id="cb040-143">Hive SQL-szerű lekérdezésnyelvet (HiveQL) biztosít a strukturált adatok használata.</span><span class="sxs-lookup"><span data-stu-id="cb040-143">Hive provides a SQL-like query language (HiveQL) for working with structured data.</span></span> <span data-ttu-id="cb040-144">Az alábbi lépések segítségével megtudhatja, hogyan igény lekérdezéseinek futtatásához a helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cb040-144">Use the following steps to learn how to run on-demand queries against the local cluster.</span></span>

1. <span data-ttu-id="cb040-145">A **Server Explorer**, kattintson a jobb gombbal a helyi fürthöz, amelyeket a korábban hozzáadott bejegyzést, és válassza **Hive lekérdezés írása**.</span><span class="sxs-lookup"><span data-stu-id="cb040-145">In **Server Explorer**, right-click the entry for the local cluster that you added previously, and then select **Write a Hive Query**.</span></span>

    ![Képernyőfelvétel a Server Explorer, a kiemelt Hive lekérdezés írása](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    <span data-ttu-id="cb040-147">Egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="cb040-147">A new query window appears.</span></span> <span data-ttu-id="cb040-148">Itt is gyorsan írása, és küldje el a lekérdezést a helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cb040-148">Here you can quickly write and submit a query to the local cluster.</span></span>

2. <span data-ttu-id="cb040-149">Az új lekérdezési ablak adja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cb040-149">In the new query window, enter the following command:</span></span>

        select count(*) from sample_08;

    <span data-ttu-id="cb040-150">Válassza ki a lekérdezés futtatásához **Submit** az ablak tetején.</span><span class="sxs-lookup"><span data-stu-id="cb040-150">To run the query, select **Submit** at the top of the window.</span></span> <span data-ttu-id="cb040-151">Hagyja meg az egyéb értékek (**kötegelt** és a kiszolgáló neve), az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="cb040-151">Leave the other values (**Batch** and server name) at the default values.</span></span>

    ![Képernyőkép a lekérdezési ablakban, a Küldés gomb](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    <span data-ttu-id="cb040-153">Használhatja a legördülő menü mellett **Submit** kiválasztásához **speciális**.</span><span class="sxs-lookup"><span data-stu-id="cb040-153">You can also use the drop-down menu next to **Submit** to select **Advanced**.</span></span> <span data-ttu-id="cb040-154">Speciális beállítások lehetővé teszik a további lehetőségeket nyújtanak, ha a feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb040-154">Advanced options allow you to provide additional options when you submit the job.</span></span>

    ![Parancsfájl elküldése képernyőkép párbeszédpanel](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. <span data-ttu-id="cb040-156">A lekérdezés elküldése után megjelenik a feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="cb040-156">After you submit the query, the job status appears.</span></span> <span data-ttu-id="cb040-157">A feladat állapotát a feladat információit jeleníti meg, azt Hadoop dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="cb040-157">The job status displays information about the job as it is processed by Hadoop.</span></span> <span data-ttu-id="cb040-158">**Feladat állapota** a állapotát mutatja meg a feladatot.</span><span class="sxs-lookup"><span data-stu-id="cb040-158">**Job State** provides the status of the job.</span></span> <span data-ttu-id="cb040-159">Az állapot rendszeresen frissül, vagy az ikon segítségével manuálisan frissítse a állapotát.</span><span class="sxs-lookup"><span data-stu-id="cb040-159">The state is updated periodically, or you can use the refresh icon to refresh the state manually.</span></span>

    ![Képernyőfelvétel a feladat megtekintése párbeszédpanelen, a kijelölt feladat állapota](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    <span data-ttu-id="cb040-161">Miután a **feladatállapotot** vált, **befejezett**, egy irányított aciklikus diagramhoz (DAG) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb040-161">After the **Job State** changes to **Finished**, a Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="cb040-162">Ezen a diagramon ismerteti a végrehajtás elérési utat, amely a Tez lett határozza meg a Hive-lekérdezés feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="cb040-162">This diagram describes the execution path that was determined by Tez when processing the Hive query.</span></span> <span data-ttu-id="cb040-163">Tez a alapértelmezett-végrehajtó motor a Hive a helyi fürtön.</span><span class="sxs-lookup"><span data-stu-id="cb040-163">Tez is the default execution engine for Hive on the local cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cb040-164">Tez is az alapértelmezett beállítás használata Linux-alapú HDInsight-fürtök esetén.</span><span class="sxs-lookup"><span data-stu-id="cb040-164">Tez is also the default when you are using Linux-based HDInsight clusters.</span></span> <span data-ttu-id="cb040-165">A Windows-alapú HDInsight alapértelmezett nincs.</span><span class="sxs-lookup"><span data-stu-id="cb040-165">It is not the default on Windows-based HDInsight.</span></span> <span data-ttu-id="cb040-166">A használatára, hozzá kell adnia a sor `set hive.execution.engine = tez;` a Hive-lekérdezést elejére.</span><span class="sxs-lookup"><span data-stu-id="cb040-166">To use it there, you must add the line `set hive.execution.engine = tez;` to the beginning of your Hive query.</span></span>

    <span data-ttu-id="cb040-167">Használja a **Job Output** hivatkozás eredményének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb040-167">Use the **Job Output** link to view the output.</span></span> <span data-ttu-id="cb040-168">Ebben az esetben is 823, a sample_08 tábla sorainak száma.</span><span class="sxs-lookup"><span data-stu-id="cb040-168">In this case, it is 823, the number of rows in the sample_08 table.</span></span> <span data-ttu-id="cb040-169">A feladat diagnosztikai információ segítségével tekintheti a **Job Log** és **YARN naplók letöltése** hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="cb040-169">You can view diagnostics information about the job by using the **Job Log** and **Download YARN Log** links.</span></span>

4. <span data-ttu-id="cb040-170">Futtathat Hive-feladatok interaktív módosításával a **kötegelt** mezőről **interaktív**.</span><span class="sxs-lookup"><span data-stu-id="cb040-170">You can also run Hive jobs interactively by changing the **Batch** field to **Interactive**.</span></span> <span data-ttu-id="cb040-171">Válassza ki **Execute**.</span><span class="sxs-lookup"><span data-stu-id="cb040-171">Then select **Execute**.</span></span>

    ![A kijelölt képernyőkép interaktív és végrehajtási gombok](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    <span data-ttu-id="cb040-173">Az interaktív lekérdezések az adatfolyamokat a feldolgozás során generált kimeneti naplót a **hiveserver2-n kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="cb040-173">An interactive query streams the output log generated during processing to the **HiveServer2 Output** window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cb040-174">Az információ megegyezik, amely elérhető a **Job Log** hivatkozás egy feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="cb040-174">The information is the same that is available from the **Job Log** link after a job has finished.</span></span>

    ![Képernyőkép a kimeneti naplót](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a><span data-ttu-id="cb040-176">Hive-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb040-176">Create a Hive project</span></span>

<span data-ttu-id="cb040-177">A projekt több Hive parancsfájl tartalmazó is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="cb040-177">You can also create a project that contains multiple Hive scripts.</span></span> <span data-ttu-id="cb040-178">A projekt használhatók lehetnek kapcsolódó parancsfájlokat vagy parancsfájlok tárolása egy verziókezelő rendszert szeretné.</span><span class="sxs-lookup"><span data-stu-id="cb040-178">Use a project when you have related scripts or want to store scripts in a version control system.</span></span>

1. <span data-ttu-id="cb040-179">A Visual Studio válassza **fájl**, **új**, majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="cb040-179">In Visual Studio, select **File**, **New**, and then **Project**.</span></span>

2. <span data-ttu-id="cb040-180">A projektek listájában bontsa ki **sablonok**, bontsa ki a **Azure Data Lake**, majd válassza ki **struktúra (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="cb040-180">From the list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **HIVE (HDInsight)**.</span></span> <span data-ttu-id="cb040-181">Válassza ki a listáról a sablonok **minta Hive**.</span><span class="sxs-lookup"><span data-stu-id="cb040-181">From the list of templates, select **Hive Sample**.</span></span> <span data-ttu-id="cb040-182">Adja meg a nevét és helyét, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb040-182">Enter a name and location, and then select **OK**.</span></span>

    ![Képernyőfelvétel az új projekt ablakról, Azure Data Lake, a HIVE, a minta Hive és a OK kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

<span data-ttu-id="cb040-184">A **minta Hive** -projekt tartalmazza két parancsfájlok **WebLogAnalysis.hql** és **SensorDataAnalysis.hql**.</span><span class="sxs-lookup"><span data-stu-id="cb040-184">The **Hive Sample** project contains two scripts, **WebLogAnalysis.hql** and **SensorDataAnalysis.hql**.</span></span> <span data-ttu-id="cb040-185">Elküldheti a ezen parancsfájlok használatával azonos **Submit** gombra az ablak tetején.</span><span class="sxs-lookup"><span data-stu-id="cb040-185">You can submit these scripts by using the same **Submit** button at the top of the window.</span></span>

## <a name="create-a-pig-project"></a><span data-ttu-id="cb040-186">A Pig-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb040-186">Create a Pig project</span></span>

<span data-ttu-id="cb040-187">Hive egy SQL-szerű nyelv biztosít a strukturált adatok használata, amíg a Pig adatokon átalakítások elvégzésével működik.</span><span class="sxs-lookup"><span data-stu-id="cb040-187">While Hive provides a SQL-like language for working with structured data, Pig works by performing transformations on data.</span></span> <span data-ttu-id="cb040-188">Pig biztosít egy nyelv (a Pig latin betűs), amely lehetővé teszi átalakítások adatcsatorna fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb040-188">Pig provides a language (Pig Latin) that allows you to develop a pipeline of transformations.</span></span> <span data-ttu-id="cb040-189">A Pig használata a helyi fürthöz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cb040-189">To use Pig with the local cluster, follow these steps:</span></span>

1. <span data-ttu-id="cb040-190">Nyissa meg a Visual Studio, és válassza ki **fájl**, **új**, majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="cb040-190">Open Visual Studio, and select **File**, **New**, and then **Project**.</span></span> <span data-ttu-id="cb040-191">Bontsa ki a projektek listája, **sablonok**, bontsa ki a **Azure Data Lake**, majd válassza ki **Pig (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="cb040-191">From the list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **Pig (HDInsight)**.</span></span> <span data-ttu-id="cb040-192">Válassza ki a listáról a sablonok **Pig alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cb040-192">From the list of templates, select **Pig Application**.</span></span> <span data-ttu-id="cb040-193">Adjon meg egy nevet, a helyre, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb040-193">Enter a name, location, and then select **OK**.</span></span>

    ![Képernyőfelvétel az új projekt ablakról, az Azure Data Lake, a Pig, a Pig alkalmazás és az OK gombra a kijelölt](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. <span data-ttu-id="cb040-195">Adja meg a következő szöveget a tartalmát a **script.pig** projekthez létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="cb040-195">Enter the following text as the contents of the **script.pig** file that was created with this project.</span></span>

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    <span data-ttu-id="cb040-196">Pig más nyelvű Hive-nál, amíg a feladatok futtatásának módját mindkét nyelven, konzisztensek keresztül a **Submit** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb040-196">While Pig uses a different language than Hive, how you run the jobs is consistent between both languages, through the **Submit** button.</span></span> <span data-ttu-id="cb040-197">Válassza a legördülő lista melletti **Submit** egy speciális Küldés párbeszédpanel megjeleníti a Pig-feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="cb040-197">Selecting the drop-down beside **Submit** displays an advanced submit dialog box for Pig.</span></span>

    ![Parancsfájl elküldése képernyőkép párbeszédpanel](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. <span data-ttu-id="cb040-199">A feladat állapotát és a kimeneti is látható, ugyanaz, mint a Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="cb040-199">The job status and output is also displayed, the same as a Hive query.</span></span>

    ![Képernyőkép a Pig projekt](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a><span data-ttu-id="cb040-201">Feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="cb040-201">View jobs</span></span>

<span data-ttu-id="cb040-202">A Data Lake tools is engedélyezi, hogy könnyen tekintse meg a Hadoop futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="cb040-202">Data Lake tools also allow you to easily view information about jobs that have been run on Hadoop.</span></span> <span data-ttu-id="cb040-203">Az alábbi lépések segítségével tekintse meg a helyi fürtön futó feladatok.</span><span class="sxs-lookup"><span data-stu-id="cb040-203">Use the following steps to see the jobs that have been run on the local cluster.</span></span>

1. <span data-ttu-id="cb040-204">A **Server Explorer**, kattintson a jobb gombbal a helyi fürthöz, és válassza **feladatok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="cb040-204">From **Server Explorer**, right-click the local cluster, and then select **View Jobs**.</span></span> <span data-ttu-id="cb040-205">A fürthöz benyújtott feladatok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb040-205">A list of jobs that have been submitted to the cluster is displayed.</span></span>

    ![Képernyőfelvétel a Server Explorer, a kiemelt feladatok megtekintése](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. <span data-ttu-id="cb040-207">A feladatok listáját válassza ki egy, a feladat részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb040-207">From the list of jobs, select one to view the job details.</span></span>

    ![Képernyőfelvétel a feladat böngészőben valamelyik, a kiemelt feladatok](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    <span data-ttu-id="cb040-209">A megjelenő információk eredményének megtekintéséhez és naplózza az adatokat, beleértve a Hive vagy Pig lekérdezés futtatása után megjelenő hasonlít.</span><span class="sxs-lookup"><span data-stu-id="cb040-209">The information displayed is similar to what you see after running a Hive or Pig query, including links to view the output and log information.</span></span>

3. <span data-ttu-id="cb040-210">Módosítja, és küldje el újra a feladatot, itt.</span><span class="sxs-lookup"><span data-stu-id="cb040-210">You can also modify and resubmit the job from here.</span></span>

## <a name="view-hive-databases"></a><span data-ttu-id="cb040-211">Hive adatbázisok megtekintése</span><span class="sxs-lookup"><span data-stu-id="cb040-211">View Hive databases</span></span>

1. <span data-ttu-id="cb040-212">A **Server Explorer**, bontsa ki a **helyi HDInsight-fürt** bejegyzést, majd bontsa ki a **Hive adatbázisokat**.</span><span class="sxs-lookup"><span data-stu-id="cb040-212">In **Server Explorer**, expand the **HDInsight local cluster** entry, and then expand **Hive Databases**.</span></span> <span data-ttu-id="cb040-213">A **alapértelmezett** és **xademo** a helyi fürtön lévő adatbázisok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="cb040-213">The **Default** and **xademo** databases on the local cluster are displayed.</span></span> <span data-ttu-id="cb040-214">Egy adatbázis bővíteni jeleníti meg a táblák az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="cb040-214">Expanding a database shows the tables within the database.</span></span>

    ![Képernyőfelvétel a Server Explorer eszközben kibontva adatbázisok](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. <span data-ttu-id="cb040-216">A tábla oszlopainak egy táblázat kibontása jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="cb040-216">Expanding a table displays the columns for that table.</span></span> <span data-ttu-id="cb040-217">Az adatok gyors megtekintéséhez kattintson a jobb gombbal egy táblát, és válassza ki **View Top 100 sor**.</span><span class="sxs-lookup"><span data-stu-id="cb040-217">To quickly view the data, right-click a table, and select **View Top 100 Rows**.</span></span>

    ![Képernyőfelvétel a Server Explorer eszközben kibontva tábla és nézet kijelölt Top 100 sorok](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a><span data-ttu-id="cb040-219">Adatbázis és tábla tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="cb040-219">Database and table properties</span></span>

<span data-ttu-id="cb040-220">Egy adatbázis vagy táblázat tulajdonságainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="cb040-220">You can view the properties of a database or table.</span></span> <span data-ttu-id="cb040-221">Kiválasztása **tulajdonságok** megjeleníti a kijelölt elem részleteket a Tulajdonságok ablak.</span><span class="sxs-lookup"><span data-stu-id="cb040-221">Selecting **Properties** displays details for the selected item in the properties window.</span></span> <span data-ttu-id="cb040-222">Például olvassa el az alábbi képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="cb040-222">For example, see the information shown in the following screenshot:</span></span>

![Képernyőfelvétel a Tulajdonságok ablak](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a><span data-ttu-id="cb040-224">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb040-224">Create a table</span></span>

<span data-ttu-id="cb040-225">Hozzon létre egy táblát, kattintson a jobb gombbal egy adatbázist, és válassza **Create Table**.</span><span class="sxs-lookup"><span data-stu-id="cb040-225">To create a table, right-click a database, and then select **Create Table**.</span></span>

![Képernyőfelvétel a Server Explorer eszközben a Create Table kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

<span data-ttu-id="cb040-227">A tábla űrlap használatával is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="cb040-227">You can then create the table using a form.</span></span> <span data-ttu-id="cb040-228">Az alábbi képernyőfelvételen alján megtekintheti a nyers HiveQL a tábla létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="cb040-228">At the bottom of the following screenshot, you can see the raw HiveQL that is used to create the table.</span></span>

![Képernyőkép a tábla létrehozásához használt űrlap](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a><span data-ttu-id="cb040-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb040-230">Next steps</span></span>

* [<span data-ttu-id="cb040-231">Az a Hortonworks védőfal drótkötelek tanulási</span><span class="sxs-lookup"><span data-stu-id="cb040-231">Learning the ropes of the Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="cb040-232">Hadoop oktatóanyag – első lépések HDP</span><span class="sxs-lookup"><span data-stu-id="cb040-232">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
