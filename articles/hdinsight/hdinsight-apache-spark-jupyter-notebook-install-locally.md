---
title: "Jupyter aaaInstall helyileg & Csatlakozás az Azure HDInsight Spark-fürt tooan |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Jupyter notebook helyileg a számítógépen, és csatlakoztassa a Azure HDInsight tooan Apache Spark-fürt."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 95c052110b84b677fd23048597af9511365cacfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-tooapache-spark-on-hdinsight"></a><span data-ttu-id="702c4-103">Jupyter notebook telepítse a számítógépre, és csatlakozzon a Spark on HDInsight tooApache</span><span class="sxs-lookup"><span data-stu-id="702c4-103">Install Jupyter notebook on your computer and connect tooApache Spark on HDInsight</span></span>

<span data-ttu-id="702c4-104">A cikkben megismerheti, hogyan tooinstall Jupyter notebook hello az egyéni PySpark (a Python) és (a Scala) Spark mag az Spark magic, és csatlakozzon a hello notebook tooan HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="702c4-104">In this article you learn how tooinstall Jupyter notebook, with hello custom PySpark (for Python) and Spark (for Scala) kernels with Spark magic, and connect hello notebook tooan HDInsight cluster.</span></span> <span data-ttu-id="702c4-105">Számos okból tooinstall Jupyter a helyi számítógépen is lehet, és némi kihívást is lehet.</span><span class="sxs-lookup"><span data-stu-id="702c4-105">There can be a number of reasons tooinstall Jupyter on your local computer, and there can be some challenges as well.</span></span> <span data-ttu-id="702c4-106">Bővebben lásd: hello szakasz [Miért célszerű telepíteni Jupyter a számítógépemen](#why-should-i-install-jupyter-on-my-computer) hello Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="702c4-106">For more on this, see hello section [Why should I install Jupyter on my computer](#why-should-i-install-jupyter-on-my-computer) at hello end of this article.</span></span>

<span data-ttu-id="702c4-107">Három fő lépésből áll részt Jupyter és hello Spark magic telepíti a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="702c4-107">There are three key steps involved in installing Jupyter and hello Spark magic on your computer.</span></span>

* <span data-ttu-id="702c4-108">Jupyter notebook telepítése</span><span class="sxs-lookup"><span data-stu-id="702c4-108">Install Jupyter notebook</span></span>
* <span data-ttu-id="702c4-109">A Spark magic hello hello PySpark és a Spark mag telepítése</span><span class="sxs-lookup"><span data-stu-id="702c4-109">Install hello PySpark and Spark kernels with hello Spark magic</span></span>
* <span data-ttu-id="702c4-110">A HDInsight Spark magic tooaccess Spark-fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="702c4-110">Configure Spark magic tooaccess Spark cluster on HDInsight</span></span>

<span data-ttu-id="702c4-111">Hello egyéni kernelek és hello Spark magic HDInsight-fürthöz használt Jupyter notebookokban elérhető kapcsolatos további információkért lásd: [Apache Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="702c4-111">For more information about hello custom kernels and hello Spark magic available for Jupyter notebooks with HDInsight cluster, see [Kernels available for Jupyter notebooks with Apache Spark Linux clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="702c4-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="702c4-112">Prerequisites</span></span>
<span data-ttu-id="702c4-113">az itt felsorolt hello Előfeltételek nincsenek Jupyter telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="702c4-113">hello prerequisites listed here are not for installing Jupyter.</span></span> <span data-ttu-id="702c4-114">Ezek a kapcsolódó hello Jupyter notebook tooan HDInsight-fürthöz, hello notebook telepítése után.</span><span class="sxs-lookup"><span data-stu-id="702c4-114">These are for connecting hello Jupyter notebook tooan HDInsight cluster once hello notebook is installed.</span></span>

* <span data-ttu-id="702c4-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="702c4-115">An Azure subscription.</span></span> <span data-ttu-id="702c4-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="702c4-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="702c4-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="702c4-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="702c4-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="702c4-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="install-jupyter-notebook-on-your-computer"></a><span data-ttu-id="702c4-119">Jupyter notebook telepítse a számítógépre</span><span class="sxs-lookup"><span data-stu-id="702c4-119">Install Jupyter notebook on your computer</span></span>

<span data-ttu-id="702c4-120">Jupyter notebookok telepítése előtt telepítenie kell az Python.</span><span class="sxs-lookup"><span data-stu-id="702c4-120">You  must install Python before you can install Jupyter notebooks.</span></span> <span data-ttu-id="702c4-121">Érhetők el a Python és a Jupyter hello részeként [Anaconda terjesztési](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="702c4-121">Both Python and Jupyter are available as part of hello [Anaconda distribution](https://www.continuum.io/downloads).</span></span> <span data-ttu-id="702c4-122">Ha Anaconda telepíti, a Python egy terjesztési telepítése.</span><span class="sxs-lookup"><span data-stu-id="702c4-122">When you install Anaconda, you install a distribution of Python.</span></span> <span data-ttu-id="702c4-123">Anaconda telepítése után hozzáadhat hello Jupyter telepítési megfelelő parancsok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="702c4-123">Once Anaconda is installed, you add hello Jupyter installation by running appropriate commands.</span></span>

1. <span data-ttu-id="702c4-124">Töltse le a hello [Anaconda telepítő](https://www.continuum.io/downloads) a platform és futtatási hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="702c4-124">Download hello [Anaconda installer](https://www.continuum.io/downloads) for your platform and run hello setup.</span></span> <span data-ttu-id="702c4-125">Közben futó hello beállítása varázsló gondoskodjon róla, hogy hello beállítás tooadd Anaconda tooyour ELÉRÉSIÚT-változónak.</span><span class="sxs-lookup"><span data-stu-id="702c4-125">While running hello setup wizard, make sure you select hello option tooadd Anaconda tooyour PATH variable.</span></span>
2. <span data-ttu-id="702c4-126">Futtassa a következő parancs tooinstall Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="702c4-126">Run hello following command tooinstall Jupyter.</span></span>

        conda install jupyter

    <span data-ttu-id="702c4-127">Jupyter telepítésével kapcsolatos további információkért lásd: [telepítése Jupyter használatával Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span><span class="sxs-lookup"><span data-stu-id="702c4-127">For more information on installing Jupyter, see [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span></span>

## <a name="install-hello-kernels-and-spark-magic"></a><span data-ttu-id="702c4-128">Hello kernelek és Spark magic telepítése</span><span class="sxs-lookup"><span data-stu-id="702c4-128">Install hello kernels and Spark magic</span></span>

<span data-ttu-id="702c4-129">Hogyan tooinstall hello Spark magic, hello PySpark és Spark mag, hello telepítési utasításokat követve hello kapcsolatos utasításokat [sparkmagic dokumentáció](https://github.com/jupyter-incubator/sparkmagic#installation) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="702c4-129">For instructions on how tooinstall hello Spark magic, hello PySpark and Spark kernels, follow hello installation instructions in hello [sparkmagic documentation](https://github.com/jupyter-incubator/sparkmagic#installation) on GitHub.</span></span> <span data-ttu-id="702c4-130">hello hello Spark magic dokumentáció első lépéseként kéri tooinstall Spark magic.</span><span class="sxs-lookup"><span data-stu-id="702c4-130">hello first step in hello Spark magic documentation asks you tooinstall Spark magic.</span></span> <span data-ttu-id="702c4-131">Cserélje le, hogy első lépéseként hello hivatkozás hello a következő parancsokat, attól függően, hogy hello verziója hello HDInsight-fürthöz fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="702c4-131">Replace that first step in hello link with hello following commands, depending on hello version of hello HDInsight cluster you will connect to.</span></span> <span data-ttu-id="702c4-132">Ezután kövesse a hátralévő lépéseket hello Spark magic dokumentációjának hello.</span><span class="sxs-lookup"><span data-stu-id="702c4-132">After that, follow hello remaining steps in hello Spark magic documentation.</span></span> <span data-ttu-id="702c4-133">Ha azt szeretné, hogy tooinstall hello különböző kernelek, 3. lépés a Spark magic telepítési utasításokat szakasz hello kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="702c4-133">If you want tooinstall hello different kernels, you must perform Step 3 in hello Spark magic installation instructions section.</span></span>

* <span data-ttu-id="702c4-134">A fürtök v3.4 telepítenie sparkmagic 0.2.3`pip install sparkmagic==0.2.3`</span><span class="sxs-lookup"><span data-stu-id="702c4-134">For clusters v3.4, install sparkmagic 0.2.3 by executing `pip install sparkmagic==0.2.3`</span></span>

* <span data-ttu-id="702c4-135">A fürtök v3.5 és v3.6 telepítse a következő futtatásával sparkmagic 0.11.2`pip install sparkmagic==0.11.2`</span><span class="sxs-lookup"><span data-stu-id="702c4-135">For clusters v3.5 and v3.6, install sparkmagic 0.11.2 by executing `pip install sparkmagic==0.11.2`</span></span>

## <a name="configure-spark-magic-tooconnect-toohdinsight-spark-cluster"></a><span data-ttu-id="702c4-136">Spark magic tooconnect tooHDInsight Spark-fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="702c4-136">Configure Spark magic tooconnect tooHDInsight Spark cluster</span></span>

<span data-ttu-id="702c4-137">Ez a szakasz korábbi tooconnect tooan Apache Spark-fürt, amely kell már létrehozta Azure hdinsightban telepített hello Spark magic konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="702c4-137">In this section you configure hello Spark magic that you installed earlier tooconnect tooan Apache Spark cluster that you must have already created in Azure HDInsight.</span></span>

1. <span data-ttu-id="702c4-138">hello Jupyter konfigurációs adatokat általában hello felhasználók kezdőkönyvtár van tárolva.</span><span class="sxs-lookup"><span data-stu-id="702c4-138">hello Jupyter configuration information is typically stored in hello users home directory.</span></span> <span data-ttu-id="702c4-139">toolocate a kezdőkönyvtár bármely platformon az operációs rendszer típusa hello a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="702c4-139">toolocate your home directory on any OS platform, type hello following commands.</span></span>

    <span data-ttu-id="702c4-140">Indítsa el a hello Python rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="702c4-140">Start hello Python shell.</span></span> <span data-ttu-id="702c4-141">Meg egy parancsablakot írja be a hello következő:</span><span class="sxs-lookup"><span data-stu-id="702c4-141">On a command window, type hello following:</span></span>

        python

    <span data-ttu-id="702c4-142">Hello Python rendszerhéj írja be a következő parancs toofind kimenő hello kezdőkönyvtár hello.</span><span class="sxs-lookup"><span data-stu-id="702c4-142">On hello Python shell, enter hello following command toofind out hello home directory.</span></span>

        import os
        print(os.path.expanduser('~'))

2. <span data-ttu-id="702c4-143">Nyissa meg a kezdőkönyvtár toohello, és hozzon létre egy nevű **.sparkmagic** Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="702c4-143">Navigate toohello home directory and create a folder called **.sparkmagic** if it does not already exist.</span></span>
3. <span data-ttu-id="702c4-144">Hello mappában található nevű fájl létrehozása **config.json** , és adja hozzá a következő JSON részlet, azon belül hello.</span><span class="sxs-lookup"><span data-stu-id="702c4-144">Within hello folder, create a file called **config.json** and add hello following JSON snippet inside it.</span></span>

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. <span data-ttu-id="702c4-145">Helyettesítő **{USERNAME}**, **{CLUSTERDNSNAME}**, és **{BASE64ENCODEDPASSWORD}** megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="702c4-145">Substitute **{USERNAME}**, **{CLUSTERDNSNAME}**, and **{BASE64ENCODEDPASSWORD}** with appropriate values.</span></span> <span data-ttu-id="702c4-146">A kedvenc programozási nyelv vagy az online toogenerate a base64 kódolású jelszó segédprogramok számos használhatja a tényleges jelszót.</span><span class="sxs-lookup"><span data-stu-id="702c4-146">You can use a number of utilities in your favorite programming language or online toogenerate a base64 encoded password for your actual password.</span></span>

5. <span data-ttu-id="702c4-147">Hello jobb szívverés beállításainak konfigurálása `config.json`.</span><span class="sxs-lookup"><span data-stu-id="702c4-147">Configure hello right Heartbeat settings in `config.json`.</span></span> <span data-ttu-id="702c4-148">Adja hozzá ezeket a beállításokat, mint hello azonos szinten hello `kernel_python_credentials` és `kernel_scala_credentials` kódtöredékek a hozzáadott korábban.</span><span class="sxs-lookup"><span data-stu-id="702c4-148">You should add these settings at hello same level as hello `kernel_python_credentials` and `kernel_scala_credentials` snippets your added earlier.</span></span> <span data-ttu-id="702c4-149">Például hogyan és hol tooadd hello szívverés-beállítások a megjelenik ez [minta config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span><span class="sxs-lookup"><span data-stu-id="702c4-149">For an example on how and where tooadd hello heartbeat settings, see this [sample config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span></span>

    * <span data-ttu-id="702c4-150">A `sparkmagic 0.2.3` (v3.4 fürtöket) tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="702c4-150">For `sparkmagic 0.2.3` (clusters v3.4), include:</span></span>

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * <span data-ttu-id="702c4-151">A `sparkmagic 0.11.2` (fürtök v3.5 és v3.6), a következők:</span><span class="sxs-lookup"><span data-stu-id="702c4-151">For `sparkmagic 0.11.2` (clusters v3.5 and v3.6), include:</span></span>

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    ><span data-ttu-id="702c4-152">Munkamenetek nem szivárognak tooensure szívveréseket küld.</span><span class="sxs-lookup"><span data-stu-id="702c4-152">Heartbeats are sent tooensure that sessions are not leaked.</span></span> <span data-ttu-id="702c4-153">Ha egy számítógép toosleep vagy le van állítva, hello szívverés nem küldi el, a tisztítani eredményezve hello munkamenet alatt.</span><span class="sxs-lookup"><span data-stu-id="702c4-153">When a computer goes toosleep or is shut down, hello heartbeat is not sent, resulting in hello session being cleaned up.</span></span> <span data-ttu-id="702c4-154">A fürtök v3.4, ha ezt a viselkedést kívánja toodisable beállíthatja hello Livy config `livy.server.interactive.heartbeat.timeout` túl`0` a hello Ambari felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="702c4-154">For clusters v3.4, if you wish toodisable this behavior, you can set hello Livy config `livy.server.interactive.heartbeat.timeout` too`0` from hello Ambari UI.</span></span> <span data-ttu-id="702c4-155">A fürtök v3.5 Ha nincs megadva a fenti hello 3.5 konfigurációs hello munkamenet nem törlődik.</span><span class="sxs-lookup"><span data-stu-id="702c4-155">For clusters v3.5, if you do not set hello 3.5 configuration above, hello session will not be deleted.</span></span>

6. <span data-ttu-id="702c4-156">Indítsa el a Jupyter.</span><span class="sxs-lookup"><span data-stu-id="702c4-156">Start Jupyter.</span></span> <span data-ttu-id="702c4-157">Parancs hello parancssorból a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="702c4-157">Use hello following command from hello command prompt.</span></span>

        jupyter notebook

7. <span data-ttu-id="702c4-158">Győződjön meg arról, hogy csatlakozhasson hello Jupyter notebook és, hogy használható hello Spark magic elérhető kernelek hello toohello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="702c4-158">Verify that you can connect toohello cluster using hello Jupyter notebook and that you can use hello Spark magic available with hello kernels.</span></span> <span data-ttu-id="702c4-159">Hajtsa végre a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="702c4-159">Perform hello following steps.</span></span>

    <span data-ttu-id="702c4-160">a.</span><span class="sxs-lookup"><span data-stu-id="702c4-160">a.</span></span> <span data-ttu-id="702c4-161">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="702c4-161">Create a new notebook.</span></span> <span data-ttu-id="702c4-162">Kattintson a jobb oldali sarokban hello, **új**.</span><span class="sxs-lookup"><span data-stu-id="702c4-162">From hello right-hand corner, click **New**.</span></span> <span data-ttu-id="702c4-163">Megtekintheti az hello alapértelmezett kernel **Python2** és két új kernelek, a telepítés hello **PySpark** és **Spark**.</span><span class="sxs-lookup"><span data-stu-id="702c4-163">You should see hello default kernel **Python2** and hello two new kernels that you install, **PySpark** and **Spark**.</span></span> <span data-ttu-id="702c4-164">Kattintson a **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="702c4-164">Click **PySpark**.</span></span>

    <span data-ttu-id="702c4-165">![A Jupyter notebook kernelek](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "kernelek a Jupyter notebook")</span><span class="sxs-lookup"><span data-stu-id="702c4-165">![Kernels in Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter notebook")</span></span>

    <span data-ttu-id="702c4-166">b.</span><span class="sxs-lookup"><span data-stu-id="702c4-166">b.</span></span> <span data-ttu-id="702c4-167">Futtassa a következő kódrészletet hello.</span><span class="sxs-lookup"><span data-stu-id="702c4-167">Run hello following code snippet.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    <span data-ttu-id="702c4-168">Hello kimeneti sikeresen le, ha a kapcsolat toohello HDInsight-fürthöz lett tesztelve.</span><span class="sxs-lookup"><span data-stu-id="702c4-168">If you can successfully retrieve hello output, your connection toohello HDInsight cluster is tested.</span></span>

    >[!TIP]
    ><span data-ttu-id="702c4-169">Ha azt szeretné, hogy tooupdate hello notebook konfigurációs tooconnect tooa másik fürtre, frissítse hello config.json hello új értékhalmazt, ahogy az a fenti a 3. lépés.</span><span class="sxs-lookup"><span data-stu-id="702c4-169">If you want tooupdate hello notebook configuration tooconnect tooa different cluster, update hello config.json with hello new set of values, as shown in Step 3 above.</span></span>

## <a name="why-should-i-install-jupyter-on-my-computer"></a><span data-ttu-id="702c4-170">Miért kell telepíteni a számítógépre Jupyter?</span><span class="sxs-lookup"><span data-stu-id="702c4-170">Why should I install Jupyter on my computer?</span></span>
<span data-ttu-id="702c4-171">Egy szám lehet okból miért lehet, hogy tooinstall Jupyter szeretné, hogy a számítógépen, és csatlakoztassa tooa a Spark hdinsight fürt.</span><span class="sxs-lookup"><span data-stu-id="702c4-171">There can be a number of reasons why you might want tooinstall Jupyter on your computer and then connect it tooa Spark cluster on HDInsight.</span></span>

* <span data-ttu-id="702c4-172">Annak ellenére, hogy a Jupyter notebookok már érhetők el az Azure HDInsight Spark-fürt hello, hello beállítás toocreate jegyzetfüzetek helyileg, egy futó fürtön hajtották az alkalmazás teszteléséhez, majd töltse fel a hello Jupyter telepíti a számítógépre biztosít notebookok toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="702c4-172">Even though Jupyter notebooks are already available on hello Spark cluster in Azure HDInsight, installing Jupyter on your computer provides you hello option toocreate your notebooks locally, test your application against a running cluster, and then upload hello notebooks toohello cluster.</span></span> <span data-ttu-id="702c4-173">tooupload hello notebookok toohello fürt, vagy feltöltheti az azokat futtató hello Jupyter notebook vagy hello segítségével a fürt, vagy elmentheti toohello /HdiNotebooks mappa hello-fürthöz tartozó hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="702c4-173">tooupload hello notebooks toohello cluster, you can either upload them using hello Jupyter notebook that is running or hello cluster, or save them toohello /HdiNotebooks folder in hello storage account associated with hello cluster.</span></span> <span data-ttu-id="702c4-174">Notebookok hello fürtön tárolásával további információkért lásd: [Jupyter notebookok tároló](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span><span class="sxs-lookup"><span data-stu-id="702c4-174">For more information on how notebooks are stored on hello cluster, see [Where are Jupyter notebooks stored](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span></span>
* <span data-ttu-id="702c4-175">A rendelkezésre álló hello jegyzetfüzetek helyileg, csatlakoztathatja toodifferent Spark-fürtjei alapján az alkalmazás követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="702c4-175">With hello notebooks available locally, you can connect toodifferent Spark clusters based on your application requirement.</span></span>
* <span data-ttu-id="702c4-176">GitHub tooimplement a forrásrendszerben vezérlő használja, és hello notebookok verzió-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="702c4-176">You can use GitHub tooimplement a source control system and have version control for hello notebooks.</span></span> <span data-ttu-id="702c4-177">A együttműködési környezetekben, ahol több felhasználó is dolgozhat hello azonos is lehet notebookot.</span><span class="sxs-lookup"><span data-stu-id="702c4-177">You can also have a collaborative environment where multiple users can work with hello same notebook.</span></span>
* <span data-ttu-id="702c4-178">Dolgozhat notebookok helyileg egy fürtöt is nélkül.</span><span class="sxs-lookup"><span data-stu-id="702c4-178">You can work with notebooks locally without even having a cluster up.</span></span> <span data-ttu-id="702c4-179">Csak akkor kell a fürt tootest jegyzetfüzetek ellen, nem toomanually jegyzetfüzetek vagy a környezet felügyeletéhez.</span><span class="sxs-lookup"><span data-stu-id="702c4-179">You only need a cluster tootest your notebooks against, not toomanually manage your notebooks or a development environment.</span></span>
* <span data-ttu-id="702c4-180">Akkor lehet, hogy könnyebben tooconfigure a saját helyi fejlesztőkörnyezetet meghaladja a tooconfigure hello Jupyter telepítési hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="702c4-180">It may be easier tooconfigure your own local development environment than it is tooconfigure hello Jupyter installation on hello cluster.</span></span>  <span data-ttu-id="702c4-181">Egy vagy több távoli fürtök beállítása nélkül helyileg telepített összes hello szoftver fordíthatja előnyére.</span><span class="sxs-lookup"><span data-stu-id="702c4-181">You can take advantage of all hello software you have installed locally without configuring one or more remote clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="702c4-182">A helyi számítógépen Jupyter, több felhasználó is futtatja a hello: hello fürt azonos Spark hello azonos jegyzetfüzetet ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="702c4-182">With Jupyter installed on your local computer, multiple users can run hello same notebook on hello same Spark cluster at hello same time.</span></span> <span data-ttu-id="702c4-183">Ilyen esetben több Livy munkamenetek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="702c4-183">In such a situation, multiple Livy sessions are created.</span></span> <span data-ttu-id="702c4-184">Ha problémát tapasztal, és szeretné, hogy toodebug, azokat, hogy egy összetett feladat tootrack, mely Livy munkamenet toowhich felhasználó tartozik.</span><span class="sxs-lookup"><span data-stu-id="702c4-184">If you run into an issue and want toodebug that, it will be a complex task tootrack which Livy session belongs toowhich user.</span></span>
>
>

## <span data-ttu-id="702c4-185"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="702c4-185"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="702c4-186">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="702c4-186">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="702c4-187">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="702c4-187">Scenarios</span></span>
* [<span data-ttu-id="702c4-188">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="702c4-188">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="702c4-189">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="702c4-189">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="702c4-190">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="702c4-190">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="702c4-191">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="702c4-191">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="702c4-192">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="702c4-192">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="702c4-193">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="702c4-193">Create and run applications</span></span>
* [<span data-ttu-id="702c4-194">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="702c4-194">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="702c4-195">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="702c4-195">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="702c4-196">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="702c4-196">Tools and extensions</span></span>
* [<span data-ttu-id="702c4-197">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="702c4-197">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="702c4-198">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="702c4-198">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="702c4-199">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="702c4-199">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="702c4-200">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="702c4-200">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="702c4-201">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="702c4-201">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a><span data-ttu-id="702c4-202">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="702c4-202">Manage resources</span></span>
* [<span data-ttu-id="702c4-203">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="702c4-203">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="702c4-204">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="702c4-204">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
