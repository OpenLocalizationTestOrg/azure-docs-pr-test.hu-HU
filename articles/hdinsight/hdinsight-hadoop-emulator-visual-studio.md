---
title: "a Hortonworks védőfal - Azure HDInsight Visual Studio eszközök aaaData Lake |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure Data Lake tools for Visual Studio a helyi virtuális gépen futó hello Hortonworks védőfal. Ezekkel az eszközökkel hozhat létre, és futtassa a Hive és a Pig feladatot a hello védőfal, és a nézet feladatkiemenetét és a korábbi."
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
ms.openlocfilehash: 7a3406b28d87d953e9b5be387faa86268f47b935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-data-lake-tools-for-visual-studio-with-hello-hortonworks-sandbox"></a><span data-ttu-id="05550-104">Hello Azure Data Lake tools for Visual Studio használata hello Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="05550-104">Use hello Azure Data Lake tools for Visual Studio with hello Hortonworks Sandbox</span></span>

<span data-ttu-id="05550-105">Azure Data Lake az általános Hadoop-fürtök használata eszközöket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="05550-105">Azure Data Lake includes tools for working with generic Hadoop clusters.</span></span> <span data-ttu-id="05550-106">Ez a dokumentum lépéseit hello toouse hello Data Lake tools a hello szükséges a helyi virtuális gépen futó Hortonworks védőfal.</span><span class="sxs-lookup"><span data-stu-id="05550-106">This document provides hello steps needed toouse hello Data Lake tools with hello Hortonworks Sandbox running in a local virtual machine.</span></span>

<span data-ttu-id="05550-107">Hello Hortonworks védőfal használata lehetővé teszi helyileg a fejlesztési környezetet az hadooppal toowork.</span><span class="sxs-lookup"><span data-stu-id="05550-107">Using hello Hortonworks Sandbox allows you toowork with Hadoop locally on your development environment.</span></span> <span data-ttu-id="05550-108">Miután olyan megoldást fejlesztett ki, és szeretné, hogy toodeploy azt léptékű, majd helyezze át a tooan HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="05550-108">After you have developed a solution and want toodeploy it at scale, you can then move tooan HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05550-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05550-109">Prerequisites</span></span>

* <span data-ttu-id="05550-110">hello Hortonworks védőfal, a virtuális gépen futó a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="05550-110">hello Hortonworks Sandbox, running in a virtual machine on your development environment.</span></span> <span data-ttu-id="05550-111">Ez a dokumentum írása és Oracle VirtualBox futó hello védőfal tesztelték.</span><span class="sxs-lookup"><span data-stu-id="05550-111">This document was written and tested with hello sandbox running in Oracle VirtualBox.</span></span> <span data-ttu-id="05550-112">Hello védőfal beállításával kapcsolatos információkért lásd: hello [hello Hortonworks védőfal az első lépései.](hdinsight-hadoop-emulator-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="05550-112">For information on setting up hello sandbox, see hello [Get started with hello Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span></span> <span data-ttu-id="05550-113">a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="05550-113">document.</span></span>

* <span data-ttu-id="05550-114">A Visual Studio 2013, Visual Studio 2015-öt vagy a Visual Studio 2017 (minden kiadás).</span><span class="sxs-lookup"><span data-stu-id="05550-114">Visual Studio 2013, Visual Studio 2015, or Visual Studio 2017 (any edition).</span></span>

* <span data-ttu-id="05550-115">Hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="05550-115">hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 or later.</span></span>

* <span data-ttu-id="05550-116">Hello [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="05550-116">hello [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="configure-passwords-for-hello-sandbox"></a><span data-ttu-id="05550-117">Hello védőfal jelszavak beállítása</span><span class="sxs-lookup"><span data-stu-id="05550-117">Configure passwords for hello sandbox</span></span>

<span data-ttu-id="05550-118">Győződjön meg arról, hogy fut-e Hortonworks védőfal hello.</span><span class="sxs-lookup"><span data-stu-id="05550-118">Make sure that hello Hortonworks Sandbox is running.</span></span> <span data-ttu-id="05550-119">Kövesse a lépéseket hello hello [megismerheti a hello Hortonworks védőfal](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="05550-119">Then follow hello steps in hello [Get started in hello Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) document.</span></span> <span data-ttu-id="05550-120">Ezeket a lépéseket konfigurálja hello SSH hello jelszavát `root` fiókot, és az hello Ambari `admin` fiók.</span><span class="sxs-lookup"><span data-stu-id="05550-120">These steps configure hello password for hello SSH `root` account, and hello Ambari `admin` account.</span></span> <span data-ttu-id="05550-121">Ezek a jelszavak toohello védőfal Visual studióból történő csatlakozáskor használt.</span><span class="sxs-lookup"><span data-stu-id="05550-121">These passwords are used when you connect toohello sandbox from Visual Studio.</span></span>

## <a name="connect-hello-tools-toohello-sandbox"></a><span data-ttu-id="05550-122">Csatlakozás hello eszközök toohello védőfal</span><span class="sxs-lookup"><span data-stu-id="05550-122">Connect hello tools toohello sandbox</span></span>

1. <span data-ttu-id="05550-123">Nyissa meg a Visual Studio, válassza ki **nézet**, majd válassza ki **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="05550-123">Open Visual Studio, select **View**, and then select **Server Explorer**.</span></span>

2. <span data-ttu-id="05550-124">A **Server Explorer**, kattintson a jobb gombbal hello **HDInsight** bejegyzést, és válassza **tooHDInsight emulátor csatlakozás**.</span><span class="sxs-lookup"><span data-stu-id="05550-124">From **Server Explorer**, right-click hello **HDInsight** entry, and then select **Connect tooHDInsight Emulator**.</span></span>

    ![Képernyőfelvétel a Server Explorer eszközben való csatlakozás tooHDInsight emulátor kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. <span data-ttu-id="05550-126">A hello **tooHDInsight emulátor csatlakozás** párbeszédpanelen adja meg az Ambari beállított hello jelszó.</span><span class="sxs-lookup"><span data-stu-id="05550-126">From hello **Connect tooHDInsight Emulator** dialog box, enter hello password that you configured for Ambari.</span></span>

    ![A kijelölt mezőbe a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    <span data-ttu-id="05550-128">Válassza ki **következő** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="05550-128">Select **Next** toocontinue.</span></span>

4. <span data-ttu-id="05550-129">Használjon hello **jelszó** mező tooenter hello jelszó hello konfigurálta `root` fiók.</span><span class="sxs-lookup"><span data-stu-id="05550-129">Use hello **Password** field tooenter hello password you configured for hello `root` account.</span></span> <span data-ttu-id="05550-130">Hello hello alapértelmezett értéke a többi mezőt hagyja.</span><span class="sxs-lookup"><span data-stu-id="05550-130">Leave hello other fields at hello default value.</span></span>

    ![A kijelölt mezőbe a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    <span data-ttu-id="05550-132">Válassza ki **következő** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="05550-132">Select **Next** toocontinue.</span></span>

5. <span data-ttu-id="05550-133">Várjon, amíg hello szolgáltatások toofinish érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="05550-133">Wait for validation of hello services toofinish.</span></span> <span data-ttu-id="05550-134">Bizonyos esetekben a érvényesítése sikertelen, és tooupdate hello konfigurációs kéri.</span><span class="sxs-lookup"><span data-stu-id="05550-134">In some cases, validation may fail and prompt you tooupdate hello configuration.</span></span> <span data-ttu-id="05550-135">Ha az érvényesítés meghiúsul, válassza ki a **frissítés**, és várja meg, hello konfigurációs és hello szolgáltatás toofinish ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="05550-135">If validation fails, select **Update**, and wait for hello configuration and verification for hello service toofinish.</span></span>

    ![A kijelölt frissítés gombra a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > <span data-ttu-id="05550-137">hello frissítési folyamat Ambari toomodify hello Hortonworks védőfal konfigurációs toowhat várt által hello Data Lake tools for Visual Studio használja.</span><span class="sxs-lookup"><span data-stu-id="05550-137">hello update process uses Ambari toomodify hello Hortonworks Sandbox configuration toowhat is expected by hello Data Lake tools for Visual Studio.</span></span>

6. <span data-ttu-id="05550-138">Érvényesítési befejeződését követően válassza ki a **Befejezés** toocomplete konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="05550-138">After validation has finished, select **Finish** toocomplete configuration.</span></span>
    <span data-ttu-id="05550-139">![A kijelölt a Befejezés gombra a párbeszédpanel képernyőképe](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span><span class="sxs-lookup"><span data-stu-id="05550-139">![Screenshot of dialog box, with Finish button highlighted](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span></span>

     >[!NOTE]
     > <span data-ttu-id="05550-140">A fejlesztési környezet és a virtuális gép toohello kiosztott memória mennyisége hello hello sebességétől függően ez eltarthat néhány percig tooconfigure és hello szolgáltatások ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="05550-140">Depending on hello speed of your development environment, and hello amount of memory allocated toohello virtual machine, it can take several minutes tooconfigure and validate hello services.</span></span>

<span data-ttu-id="05550-141">Ezek a lépések után most már rendelkezik egy **helyi HDInsight-fürt** bejegyzésre a Server Explorer eszközben az hello **HDInsight** szakasz.</span><span class="sxs-lookup"><span data-stu-id="05550-141">After following these steps, you now have an **HDInsight local cluster** entry in Server Explorer, under hello **HDInsight** section.</span></span>

## <a name="write-a-hive-query"></a><span data-ttu-id="05550-142">Hive-lekérdezések írása</span><span class="sxs-lookup"><span data-stu-id="05550-142">Write a Hive query</span></span>

<span data-ttu-id="05550-143">Hive SQL-szerű lekérdezésnyelvet (HiveQL) biztosít a strukturált adatok használata.</span><span class="sxs-lookup"><span data-stu-id="05550-143">Hive provides a SQL-like query language (HiveQL) for working with structured data.</span></span> <span data-ttu-id="05550-144">A következő lépéseket toolearn hogyan igény toorun lekérdezi hello helyi fürtön hajtották hello használata.</span><span class="sxs-lookup"><span data-stu-id="05550-144">Use hello following steps toolearn how toorun on-demand queries against hello local cluster.</span></span>

1. <span data-ttu-id="05550-145">A **Server Explorer**, kattintson a jobb gombbal a hello helyi fürthöz, amelyeket korábban hozzáadott hello bejegyzést, és válassza **Hive lekérdezés írása**.</span><span class="sxs-lookup"><span data-stu-id="05550-145">In **Server Explorer**, right-click hello entry for hello local cluster that you added previously, and then select **Write a Hive Query**.</span></span>

    ![Képernyőfelvétel a Server Explorer, a kiemelt Hive lekérdezés írása](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    <span data-ttu-id="05550-147">Egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="05550-147">A new query window appears.</span></span> <span data-ttu-id="05550-148">Itt is gyorsan írása, és küldje el a lekérdezés toohello helyi fürt.</span><span class="sxs-lookup"><span data-stu-id="05550-148">Here you can quickly write and submit a query toohello local cluster.</span></span>

2. <span data-ttu-id="05550-149">Adja meg a következő parancs hello hello új lekérdezési ablakba:</span><span class="sxs-lookup"><span data-stu-id="05550-149">In hello new query window, enter hello following command:</span></span>

        select count(*) from sample_08;

    <span data-ttu-id="05550-150">toorun hello lekérdezés, jelölje be **Submit** hello ablak hello tetején.</span><span class="sxs-lookup"><span data-stu-id="05550-150">toorun hello query, select **Submit** at hello top of hello window.</span></span> <span data-ttu-id="05550-151">Hagyja hello más értékek (**kötegelt** és a kiszolgáló neve): hello alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="05550-151">Leave hello other values (**Batch** and server name) at hello default values.</span></span>

    ![Képernyőkép a lekérdezési ablakban, a hello Küldés gomb](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    <span data-ttu-id="05550-153">Is használhatja hello legördülő menü mellett túl**Submit** tooselect **speciális**.</span><span class="sxs-lookup"><span data-stu-id="05550-153">You can also use hello drop-down menu next too**Submit** tooselect **Advanced**.</span></span> <span data-ttu-id="05550-154">Speciális beállítások lehetővé teszik tooprovide további beállítások hello feladat elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="05550-154">Advanced options allow you tooprovide additional options when you submit hello job.</span></span>

    ![Parancsfájl elküldése képernyőkép párbeszédpanel](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. <span data-ttu-id="05550-156">Hello lekérdezés elküldése után hello feladat állapota akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="05550-156">After you submit hello query, hello job status appears.</span></span> <span data-ttu-id="05550-157">hello feladatállapot hello feladat információit jeleníti meg, azt Hadoop dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="05550-157">hello job status displays information about hello job as it is processed by Hadoop.</span></span> <span data-ttu-id="05550-158">**Feladat állapota** hello feladat állapotának hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="05550-158">**Job State** provides hello status of hello job.</span></span> <span data-ttu-id="05550-159">hello állapot rendszeresen frissül, vagy manuálisan hello frissítési ikon toorefresh hello állapot használhatja.</span><span class="sxs-lookup"><span data-stu-id="05550-159">hello state is updated periodically, or you can use hello refresh icon toorefresh hello state manually.</span></span>

    ![Képernyőfelvétel a feladat megtekintése párbeszédpanelen, a kijelölt feladat állapota](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    <span data-ttu-id="05550-161">Hello után **feladatállapotot** túl változik**befejezett**, egy irányított aciklikus diagramhoz (DAG) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="05550-161">After hello **Job State** changes too**Finished**, a Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="05550-162">Ez az ábra ismerteti hello végrehajtási elérési utat, amely határozták meg Tez hello Hive lekérdezés feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="05550-162">This diagram describes hello execution path that was determined by Tez when processing hello Hive query.</span></span> <span data-ttu-id="05550-163">Tez hello alapértelmezett végrehajtó motorja a Hive hello helyi fürtön.</span><span class="sxs-lookup"><span data-stu-id="05550-163">Tez is hello default execution engine for Hive on hello local cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05550-164">Tez egyben hello alapértelmezett Linux-alapú HDInsight-fürtök használata esetén.</span><span class="sxs-lookup"><span data-stu-id="05550-164">Tez is also hello default when you are using Linux-based HDInsight clusters.</span></span> <span data-ttu-id="05550-165">A Windows-alapú HDInsight hello alapértelmezett nincs.</span><span class="sxs-lookup"><span data-stu-id="05550-165">It is not hello default on Windows-based HDInsight.</span></span> <span data-ttu-id="05550-166">toouse azt, hozzá kell adnia hello sor `set hive.execution.engine = tez;` toohello elejéhez a Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="05550-166">toouse it there, you must add hello line `set hive.execution.engine = tez;` toohello beginning of your Hive query.</span></span>

    <span data-ttu-id="05550-167">Használjon hello **Job Output** tooview hello kimeneti hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="05550-167">Use hello **Job Output** link tooview hello output.</span></span> <span data-ttu-id="05550-168">Ebben az esetben is 823, hello hello sample_08 tábla sorainak száma.</span><span class="sxs-lookup"><span data-stu-id="05550-168">In this case, it is 823, hello number of rows in hello sample_08 table.</span></span> <span data-ttu-id="05550-169">Diagnosztikai információ hello feladat hello segítségével tekintheti **Job Log** és **YARN naplók letöltése** hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="05550-169">You can view diagnostics information about hello job by using hello **Job Log** and **Download YARN Log** links.</span></span>

4. <span data-ttu-id="05550-170">Futtathat Hive-feladatok interaktív hello módosításával **kötegelt** túl mezőben**interaktív**.</span><span class="sxs-lookup"><span data-stu-id="05550-170">You can also run Hive jobs interactively by changing hello **Batch** field too**Interactive**.</span></span> <span data-ttu-id="05550-171">Válassza ki **Execute**.</span><span class="sxs-lookup"><span data-stu-id="05550-171">Then select **Execute**.</span></span>

    ![A kijelölt képernyőkép interaktív és végrehajtási gombok](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    <span data-ttu-id="05550-173">Az interaktív lekérdezések adatfolyamok hello feldolgozási toohello során előállított kimeneti naplót **hiveserver2-n kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="05550-173">An interactive query streams hello output log generated during processing toohello **HiveServer2 Output** window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05550-174">hello információ van hello azonos hello elérhető **Job Log** hivatkozás egy feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="05550-174">hello information is hello same that is available from hello **Job Log** link after a job has finished.</span></span>

    ![Képernyőkép a kimeneti naplót](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a><span data-ttu-id="05550-176">Hive-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="05550-176">Create a Hive project</span></span>

<span data-ttu-id="05550-177">A projekt több Hive parancsfájl tartalmazó is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="05550-177">You can also create a project that contains multiple Hive scripts.</span></span> <span data-ttu-id="05550-178">A projekt használhatók lehetnek kapcsolódó parancsfájlokat vagy toostore parancsfájlok szeretne egy verziókezelő rendszert.</span><span class="sxs-lookup"><span data-stu-id="05550-178">Use a project when you have related scripts or want toostore scripts in a version control system.</span></span>

1. <span data-ttu-id="05550-179">A Visual Studio válassza **fájl**, **új**, majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="05550-179">In Visual Studio, select **File**, **New**, and then **Project**.</span></span>

2. <span data-ttu-id="05550-180">A projektek hello listájában bontsa ki **sablonok**, bontsa ki a **Azure Data Lake**, majd válassza ki **struktúra (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="05550-180">From hello list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **HIVE (HDInsight)**.</span></span> <span data-ttu-id="05550-181">Válassza ki a sablonok hello listáról **minta Hive**.</span><span class="sxs-lookup"><span data-stu-id="05550-181">From hello list of templates, select **Hive Sample**.</span></span> <span data-ttu-id="05550-182">Adja meg a nevét és helyét, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="05550-182">Enter a name and location, and then select **OK**.</span></span>

    ![Képernyőfelvétel az új projekt ablakról, Azure Data Lake, a HIVE, a minta Hive és a OK kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

<span data-ttu-id="05550-184">Hello **minta Hive** -projekt tartalmazza két parancsfájlok **WebLogAnalysis.hql** és **SensorDataAnalysis.hql**.</span><span class="sxs-lookup"><span data-stu-id="05550-184">hello **Hive Sample** project contains two scripts, **WebLogAnalysis.hql** and **SensorDataAnalysis.hql**.</span></span> <span data-ttu-id="05550-185">Ezen parancsfájlok használatával hello azonos elküldheti **Submit** hello ablak hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="05550-185">You can submit these scripts by using hello same **Submit** button at hello top of hello window.</span></span>

## <a name="create-a-pig-project"></a><span data-ttu-id="05550-186">A Pig-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="05550-186">Create a Pig project</span></span>

<span data-ttu-id="05550-187">Hive egy SQL-szerű nyelv biztosít a strukturált adatok használata, amíg a Pig adatokon átalakítások elvégzésével működik.</span><span class="sxs-lookup"><span data-stu-id="05550-187">While Hive provides a SQL-like language for working with structured data, Pig works by performing transformations on data.</span></span> <span data-ttu-id="05550-188">A Pig biztosít egy nyelv (a Pig latin betűs), amely lehetővé teszi a toodevelop átalakítások folyamat.</span><span class="sxs-lookup"><span data-stu-id="05550-188">Pig provides a language (Pig Latin) that allows you toodevelop a pipeline of transformations.</span></span> <span data-ttu-id="05550-189">a Pig toouse hello helyi fürthöz, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="05550-189">toouse Pig with hello local cluster, follow these steps:</span></span>

1. <span data-ttu-id="05550-190">Nyissa meg a Visual Studio, és válassza ki **fájl**, **új**, majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="05550-190">Open Visual Studio, and select **File**, **New**, and then **Project**.</span></span> <span data-ttu-id="05550-191">A projektek hello listájában bontsa ki **sablonok**, bontsa ki a **Azure Data Lake**, majd válassza ki **Pig (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="05550-191">From hello list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **Pig (HDInsight)**.</span></span> <span data-ttu-id="05550-192">Válassza ki a sablonok hello listáról **Pig alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="05550-192">From hello list of templates, select **Pig Application**.</span></span> <span data-ttu-id="05550-193">Adjon meg egy nevet, a helyre, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="05550-193">Enter a name, location, and then select **OK**.</span></span>

    ![Képernyőfelvétel az új projekt ablakról, az Azure Data Lake, a Pig, a Pig alkalmazás és az OK gombra a kijelölt](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. <span data-ttu-id="05550-195">Adja meg a szöveg másként hello hello tartalmát a következő hello **script.pig** projekthez létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="05550-195">Enter hello following text as hello contents of hello **script.pig** file that was created with this project.</span></span>

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

    <span data-ttu-id="05550-196">Pig más nyelvű Hive-nál, amíg a hello feladatok futtatásának módját mindkét nyelven, keresztül hello konzisztensek **Submit** gombra.</span><span class="sxs-lookup"><span data-stu-id="05550-196">While Pig uses a different language than Hive, how you run hello jobs is consistent between both languages, through hello **Submit** button.</span></span> <span data-ttu-id="05550-197">Kiválasztásával hello melletti legördülő **Submit** egy speciális Küldés párbeszédpanel megjeleníti a Pig-feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="05550-197">Selecting hello drop-down beside **Submit** displays an advanced submit dialog box for Pig.</span></span>

    ![Parancsfájl elküldése képernyőkép párbeszédpanel](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. <span data-ttu-id="05550-199">hello feladat állapotát és a kimeneti is látható, hello ugyanaz, mint a Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="05550-199">hello job status and output is also displayed, hello same as a Hive query.</span></span>

    ![Képernyőkép a Pig projekt](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a><span data-ttu-id="05550-201">Feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="05550-201">View jobs</span></span>

<span data-ttu-id="05550-202">A Data Lake tools is lehetővé teszik a Hadoop futó feladatok adatainak tooeasily megtekintése.</span><span class="sxs-lookup"><span data-stu-id="05550-202">Data Lake tools also allow you tooeasily view information about jobs that have been run on Hadoop.</span></span> <span data-ttu-id="05550-203">A következő lépéseket toosee hello feladatok hello helyi fürtön futó hello használata.</span><span class="sxs-lookup"><span data-stu-id="05550-203">Use hello following steps toosee hello jobs that have been run on hello local cluster.</span></span>

1. <span data-ttu-id="05550-204">A **Server Explorer**, kattintson a jobb gombbal a hello helyi fürthöz, és válassza **feladatok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="05550-204">From **Server Explorer**, right-click hello local cluster, and then select **View Jobs**.</span></span> <span data-ttu-id="05550-205">Lett elküldve toohello fürt feladatok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="05550-205">A list of jobs that have been submitted toohello cluster is displayed.</span></span>

    ![Képernyőfelvétel a Server Explorer, a kiemelt feladatok megtekintése](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. <span data-ttu-id="05550-207">A feladatok hello listából válasszon egyet tooview hello feladat részleteit.</span><span class="sxs-lookup"><span data-stu-id="05550-207">From hello list of jobs, select one tooview hello job details.</span></span>

    ![Képernyőfelvétel a feladat böngésző, egy kiemelt hello feladatok](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    <span data-ttu-id="05550-209">megjelenő hello információ a hasonló toowhat párbeszédeket hivatkozások tooview hello kimeneti és naplózási Hive vagy Pig lekérdezés futtatása után megjelenik.</span><span class="sxs-lookup"><span data-stu-id="05550-209">hello information displayed is similar toowhat you see after running a Hive or Pig query, including links tooview hello output and log information.</span></span>

3. <span data-ttu-id="05550-210">Módosítja, és küldje el újra a hello feladat itt.</span><span class="sxs-lookup"><span data-stu-id="05550-210">You can also modify and resubmit hello job from here.</span></span>

## <a name="view-hive-databases"></a><span data-ttu-id="05550-211">Hive adatbázisok megtekintése</span><span class="sxs-lookup"><span data-stu-id="05550-211">View Hive databases</span></span>

1. <span data-ttu-id="05550-212">A **Server Explorer**, bontsa ki a hello **helyi HDInsight-fürt** bejegyzést, majd bontsa ki a **Hive adatbázisokat**.</span><span class="sxs-lookup"><span data-stu-id="05550-212">In **Server Explorer**, expand hello **HDInsight local cluster** entry, and then expand **Hive Databases**.</span></span> <span data-ttu-id="05550-213">Hello **alapértelmezett** és **xademo** hello helyi fürtön lévő adatbázisok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="05550-213">hello **Default** and **xademo** databases on hello local cluster are displayed.</span></span> <span data-ttu-id="05550-214">Hello táblák hello adatbázison belül adatbázis bővülő jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="05550-214">Expanding a database shows hello tables within hello database.</span></span>

    ![Képernyőfelvétel a Server Explorer eszközben kibontva adatbázisok](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. <span data-ttu-id="05550-216">A tábla oszlopainak hello táblázat kibontása jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="05550-216">Expanding a table displays hello columns for that table.</span></span> <span data-ttu-id="05550-217">tooquickly nézet hello, kattintson a jobb gombbal egy táblát, és válasszon **View Top 100 sor**.</span><span class="sxs-lookup"><span data-stu-id="05550-217">tooquickly view hello data, right-click a table, and select **View Top 100 Rows**.</span></span>

    ![Képernyőfelvétel a Server Explorer eszközben kibontva tábla és nézet kijelölt Top 100 sorok](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a><span data-ttu-id="05550-219">Adatbázis és tábla tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="05550-219">Database and table properties</span></span>

<span data-ttu-id="05550-220">Egy adatbázis vagy táblázat hello tulajdonságait tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="05550-220">You can view hello properties of a database or table.</span></span> <span data-ttu-id="05550-221">Kiválasztása **tulajdonságok** hello tulajdonságai ablakban hello kijelölt elem adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="05550-221">Selecting **Properties** displays details for hello selected item in hello properties window.</span></span> <span data-ttu-id="05550-222">Például tekintse meg a következő képernyőkép hello hello információkat:</span><span class="sxs-lookup"><span data-stu-id="05550-222">For example, see hello information shown in hello following screenshot:</span></span>

![Képernyőfelvétel a Tulajdonságok ablak](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a><span data-ttu-id="05550-224">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="05550-224">Create a table</span></span>

<span data-ttu-id="05550-225">toocreate egy táblázat frissítéséhez kattintson a jobb gombbal egy adatbázist, majd válassza ki **Create Table**.</span><span class="sxs-lookup"><span data-stu-id="05550-225">toocreate a table, right-click a database, and then select **Create Table**.</span></span>

![Képernyőfelvétel a Server Explorer eszközben a Create Table kiemelve](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

<span data-ttu-id="05550-227">Hello tábla űrlap használatával is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="05550-227">You can then create hello table using a form.</span></span> <span data-ttu-id="05550-228">A következő képernyőkép hello hello alján megjelenik, amely használt toocreate hello tábla nyers HiveQL hello.</span><span class="sxs-lookup"><span data-stu-id="05550-228">At hello bottom of hello following screenshot, you can see hello raw HiveQL that is used toocreate hello table.</span></span>

![Képernyőfelvétel a hello űrlap használt toocreate tábla](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a><span data-ttu-id="05550-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05550-230">Next steps</span></span>

* [<span data-ttu-id="05550-231">Learning hello drótkötelek a hello Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="05550-231">Learning hello ropes of hello Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="05550-232">Hadoop oktatóanyag – első lépések HDP</span><span class="sxs-lookup"><span data-stu-id="05550-232">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
