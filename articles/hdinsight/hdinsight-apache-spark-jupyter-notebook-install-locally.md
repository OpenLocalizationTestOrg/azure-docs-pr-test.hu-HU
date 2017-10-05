---
title: "Jupyter helyileg telepíteni, és csatlakozzon az Azure HDInsight Spark-fürt |} Microsoft Docs"
description: "Útmutató a Jupyter notebook helyileg telepíteni a számítógépére, és csatlakoztassa egy Azure HDInsight az Apache Spark-fürt."
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
ms.openlocfilehash: fe9dcdb643aa6a8ee5d55738b7a446e4b0153986
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a><span data-ttu-id="cc349-103">Jupyter notebook telepítse a számítógépre, és kapcsolódjon az Apache Spark on HDInsight</span><span class="sxs-lookup"><span data-stu-id="cc349-103">Install Jupyter notebook on your computer and connect to Apache Spark on HDInsight</span></span>

<span data-ttu-id="cc349-104">Ebben a cikkben megismerheti, hogyan kell telepíteni a Jupyter notebook egyéni PySpark (a Python) és a Spark magic (a Scala) Spark mag, és a notebook kapcsolódni HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="cc349-104">In this article you learn how to install Jupyter notebook, with the custom PySpark (for Python) and Spark (for Scala) kernels with Spark magic, and connect the notebook to an HDInsight cluster.</span></span> <span data-ttu-id="cc349-105">Számos Jupyter telepítéséhez a helyi számítógépen oka lehet, és némi kihívást is lehet.</span><span class="sxs-lookup"><span data-stu-id="cc349-105">There can be a number of reasons to install Jupyter on your local computer, and there can be some challenges as well.</span></span> <span data-ttu-id="cc349-106">Bővebben a részben [Miért célszerű telepíteni Jupyter a számítógépemen](#why-should-i-install-jupyter-on-my-computer) Ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="cc349-106">For more on this, see the section [Why should I install Jupyter on my computer](#why-should-i-install-jupyter-on-my-computer) at the end of this article.</span></span>

<span data-ttu-id="cc349-107">Három fő lépésből áll részt Jupyter és a Spark magic telepíti a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="cc349-107">There are three key steps involved in installing Jupyter and the Spark magic on your computer.</span></span>

* <span data-ttu-id="cc349-108">Jupyter notebook telepítése</span><span class="sxs-lookup"><span data-stu-id="cc349-108">Install Jupyter notebook</span></span>
* <span data-ttu-id="cc349-109">Telepítse a PySpark és Spark mag a Spark varázsszám</span><span class="sxs-lookup"><span data-stu-id="cc349-109">Install the PySpark and Spark kernels with the Spark magic</span></span>
* <span data-ttu-id="cc349-110">A HDInsight Spark-fürt eléréséhez Spark magic konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc349-110">Configure Spark magic to access Spark cluster on HDInsight</span></span>

<span data-ttu-id="cc349-111">Az egyéni kernelek és a Spark HDInsight-fürthöz használt Jupyter notebookokban elérhető magic kapcsolatos további információkért lásd: [Apache Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="cc349-111">For more information about the custom kernels and the Spark magic available for Jupyter notebooks with HDInsight cluster, see [Kernels available for Jupyter notebooks with Apache Spark Linux clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc349-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cc349-112">Prerequisites</span></span>
<span data-ttu-id="cc349-113">Az itt felsorolt előfeltételeket nem Jupyter telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cc349-113">The prerequisites listed here are not for installing Jupyter.</span></span> <span data-ttu-id="cc349-114">Ezek azok a Jupyter notebook kapcsolódáshoz HDInsight-fürtöt, a notebook telepítése után.</span><span class="sxs-lookup"><span data-stu-id="cc349-114">These are for connecting the Jupyter notebook to an HDInsight cluster once the notebook is installed.</span></span>

* <span data-ttu-id="cc349-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="cc349-115">An Azure subscription.</span></span> <span data-ttu-id="cc349-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="cc349-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="cc349-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="cc349-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="cc349-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="cc349-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="install-jupyter-notebook-on-your-computer"></a><span data-ttu-id="cc349-119">Jupyter notebook telepítse a számítógépre</span><span class="sxs-lookup"><span data-stu-id="cc349-119">Install Jupyter notebook on your computer</span></span>

<span data-ttu-id="cc349-120">Jupyter notebookok telepítése előtt telepítenie kell az Python.</span><span class="sxs-lookup"><span data-stu-id="cc349-120">You  must install Python before you can install Jupyter notebooks.</span></span> <span data-ttu-id="cc349-121">Python és a Jupyter állnak rendelkezésre, egy részének a [Anaconda terjesztési](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="cc349-121">Both Python and Jupyter are available as part of the [Anaconda distribution](https://www.continuum.io/downloads).</span></span> <span data-ttu-id="cc349-122">Ha Anaconda telepíti, a Python egy terjesztési telepítése.</span><span class="sxs-lookup"><span data-stu-id="cc349-122">When you install Anaconda, you install a distribution of Python.</span></span> <span data-ttu-id="cc349-123">Anaconda telepítése után adja hozzá a Jupyter telepítés megfelelő parancsok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="cc349-123">Once Anaconda is installed, you add the Jupyter installation by running appropriate commands.</span></span>

1. <span data-ttu-id="cc349-124">Töltse le a [Anaconda telepítő](https://www.continuum.io/downloads) a platform és futtassa a telepítőt.</span><span class="sxs-lookup"><span data-stu-id="cc349-124">Download the [Anaconda installer](https://www.continuum.io/downloads) for your platform and run the setup.</span></span> <span data-ttu-id="cc349-125">A telepítő varázsló futtatása közben, gondoskodjon róla, hogy a beállítás Anaconda hozzáadása a PATH változóban.</span><span class="sxs-lookup"><span data-stu-id="cc349-125">While running the setup wizard, make sure you select the option to add Anaconda to your PATH variable.</span></span>
2. <span data-ttu-id="cc349-126">A következő paranccsal telepítse a Jupyter.</span><span class="sxs-lookup"><span data-stu-id="cc349-126">Run the following command to install Jupyter.</span></span>

        conda install jupyter

    <span data-ttu-id="cc349-127">Jupyter telepítésével kapcsolatos további információkért lásd: [telepítése Jupyter használatával Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span><span class="sxs-lookup"><span data-stu-id="cc349-127">For more information on installing Jupyter, see [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span></span>

## <a name="install-the-kernels-and-spark-magic"></a><span data-ttu-id="cc349-128">Telepítse a kernelek és Spark varázsszám</span><span class="sxs-lookup"><span data-stu-id="cc349-128">Install the kernels and Spark magic</span></span>

<span data-ttu-id="cc349-129">A Spark magic telepítéséhez útmutatást a PySpark és Spark mag, hajtsa végre a szereplő telepítési útmutatást a [sparkmagic dokumentáció](https://github.com/jupyter-incubator/sparkmagic#installation) a Githubon.</span><span class="sxs-lookup"><span data-stu-id="cc349-129">For instructions on how to install the Spark magic, the PySpark and Spark kernels, follow the installation instructions in the [sparkmagic documentation](https://github.com/jupyter-incubator/sparkmagic#installation) on GitHub.</span></span> <span data-ttu-id="cc349-130">Az első lépés a Spark magic dokumentációjának arra kéri, hogy a Spark magic telepítése.</span><span class="sxs-lookup"><span data-stu-id="cc349-130">The first step in the Spark magic documentation asks you to install Spark magic.</span></span> <span data-ttu-id="cc349-131">Cserélje le a következő parancsokat a HDInsight-fürthöz fog csatlakozni verziójától függően, hogy a hivatkozás első lépése.</span><span class="sxs-lookup"><span data-stu-id="cc349-131">Replace that first step in the link with the following commands, depending on the version of the HDInsight cluster you will connect to.</span></span> <span data-ttu-id="cc349-132">Ezután kövesse a további lépéseket a Spark magic dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="cc349-132">After that, follow the remaining steps in the Spark magic documentation.</span></span> <span data-ttu-id="cc349-133">Ha a különböző kernelek telepíteni kívánja, 3. lépés a Spark magic telepítési utasításokat szakaszban kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="cc349-133">If you want to install the different kernels, you must perform Step 3 in the Spark magic installation instructions section.</span></span>

* <span data-ttu-id="cc349-134">A fürtök v3.4 telepítenie sparkmagic 0.2.3`pip install sparkmagic==0.2.3`</span><span class="sxs-lookup"><span data-stu-id="cc349-134">For clusters v3.4, install sparkmagic 0.2.3 by executing `pip install sparkmagic==0.2.3`</span></span>

* <span data-ttu-id="cc349-135">A fürtök v3.5 és v3.6 telepítse a következő futtatásával sparkmagic 0.11.2`pip install sparkmagic==0.11.2`</span><span class="sxs-lookup"><span data-stu-id="cc349-135">For clusters v3.5 and v3.6, install sparkmagic 0.11.2 by executing `pip install sparkmagic==0.11.2`</span></span>

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a><span data-ttu-id="cc349-136">Spark magic való kapcsolódáshoz a HDInsight Spark-fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc349-136">Configure Spark magic to connect to HDInsight Spark cluster</span></span>

<span data-ttu-id="cc349-137">Ez a szakasz a korábban létrehozott kell már Azure HDInsight az Apache Spark-fürt való csatlakozáshoz telepített Spark magic konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="cc349-137">In this section you configure the Spark magic that you installed earlier to connect to an Apache Spark cluster that you must have already created in Azure HDInsight.</span></span>

1. <span data-ttu-id="cc349-138">A Jupyter konfigurációs adatokat általában tárolja a felhasználók kezdőkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="cc349-138">The Jupyter configuration information is typically stored in the users home directory.</span></span> <span data-ttu-id="cc349-139">Keresse meg a kezdőkönyvtár az operációsrendszer-platformhoz, írja be a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="cc349-139">To locate your home directory on any OS platform, type the following commands.</span></span>

    <span data-ttu-id="cc349-140">Indítsa el a Python-rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="cc349-140">Start the Python shell.</span></span> <span data-ttu-id="cc349-141">Egy parancsablakot írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="cc349-141">On a command window, type the following:</span></span>

        python

    <span data-ttu-id="cc349-142">A Python felületet adja meg a kezdőkönyvtár megállapítása a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="cc349-142">On the Python shell, enter the following command to find out the home directory.</span></span>

        import os
        print(os.path.expanduser('~'))

2. <span data-ttu-id="cc349-143">Nyissa meg a kezdőkönyvtár, és hozzon létre egy nevű **.sparkmagic** Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="cc349-143">Navigate to the home directory and create a folder called **.sparkmagic** if it does not already exist.</span></span>
3. <span data-ttu-id="cc349-144">A mappában található nevű fájl létrehozása **config.json** , és adja hozzá a következő JSON kódrészletet, azon belül.</span><span class="sxs-lookup"><span data-stu-id="cc349-144">Within the folder, create a file called **config.json** and add the following JSON snippet inside it.</span></span>

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

4. <span data-ttu-id="cc349-145">Helyettesítő **{USERNAME}**, **{CLUSTERDNSNAME}**, és **{BASE64ENCODEDPASSWORD}** megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="cc349-145">Substitute **{USERNAME}**, **{CLUSTERDNSNAME}**, and **{BASE64ENCODEDPASSWORD}** with appropriate values.</span></span> <span data-ttu-id="cc349-146">A kedvenc programozási nyelven vagy online segédprogramok számos segítségével készítése base64-kódolású jelszót a tényleges jelszót.</span><span class="sxs-lookup"><span data-stu-id="cc349-146">You can use a number of utilities in your favorite programming language or online to generate a base64 encoded password for your actual password.</span></span>

5. <span data-ttu-id="cc349-147">Adja meg a megfelelő szívverés-beállításokat a `config.json`.</span><span class="sxs-lookup"><span data-stu-id="cc349-147">Configure the right Heartbeat settings in `config.json`.</span></span> <span data-ttu-id="cc349-148">Adja hozzá ezeket a beállításokat ugyanazon a szinten az `kernel_python_credentials` és `kernel_scala_credentials` kódtöredékek a hozzáadott korábban.</span><span class="sxs-lookup"><span data-stu-id="cc349-148">You should add these settings at the same level as the `kernel_python_credentials` and `kernel_scala_credentials` snippets your added earlier.</span></span> <span data-ttu-id="cc349-149">Például hogyan és hol vegye fel a szívverés-beállításokat, lásd: a [minta config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span><span class="sxs-lookup"><span data-stu-id="cc349-149">For an example on how and where to add the heartbeat settings, see this [sample config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span></span>

    * <span data-ttu-id="cc349-150">A `sparkmagic 0.2.3` (v3.4 fürtöket) tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="cc349-150">For `sparkmagic 0.2.3` (clusters v3.4), include:</span></span>

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * <span data-ttu-id="cc349-151">A `sparkmagic 0.11.2` (fürtök v3.5 és v3.6), a következők:</span><span class="sxs-lookup"><span data-stu-id="cc349-151">For `sparkmagic 0.11.2` (clusters v3.5 and v3.6), include:</span></span>

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    ><span data-ttu-id="cc349-152">Annak érdekében, hogy munkamenetek nem szivárognak szívveréseket küld.</span><span class="sxs-lookup"><span data-stu-id="cc349-152">Heartbeats are sent to ensure that sessions are not leaked.</span></span> <span data-ttu-id="cc349-153">Ha a számítógép alvó állapotba, vagy le van állítva, a szívverés nem küldi el, ami azt eredményezi, hogy a munkamenet éppen törölni a.</span><span class="sxs-lookup"><span data-stu-id="cc349-153">When a computer goes to sleep or is shut down, the heartbeat is not sent, resulting in the session being cleaned up.</span></span> <span data-ttu-id="cc349-154">Fürtök v3.4, ha szeretné letiltani ezt a viselkedést állíthatók be a Livy config `livy.server.interactive.heartbeat.timeout` való `0` a az Ambari felhasználói Felületéről.</span><span class="sxs-lookup"><span data-stu-id="cc349-154">For clusters v3.4, if you wish to disable this behavior, you can set the Livy config `livy.server.interactive.heartbeat.timeout` to `0` from the Ambari UI.</span></span> <span data-ttu-id="cc349-155">A fürtök v3.5 Ha nincs megadva a fenti, 3.5-ös configuration a munkamenet nem törlődik.</span><span class="sxs-lookup"><span data-stu-id="cc349-155">For clusters v3.5, if you do not set the 3.5 configuration above, the session will not be deleted.</span></span>

6. <span data-ttu-id="cc349-156">Indítsa el a Jupyter.</span><span class="sxs-lookup"><span data-stu-id="cc349-156">Start Jupyter.</span></span> <span data-ttu-id="cc349-157">Használja a következő parancsot a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="cc349-157">Use the following command from the command prompt.</span></span>

        jupyter notebook

7. <span data-ttu-id="cc349-158">Győződjön meg arról, hogy a fürt a Jupyter notebook használatával csatlakozhat, és hogy használata a Spark magic érhető el a kernelek.</span><span class="sxs-lookup"><span data-stu-id="cc349-158">Verify that you can connect to the cluster using the Jupyter notebook and that you can use the Spark magic available with the kernels.</span></span> <span data-ttu-id="cc349-159">A következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="cc349-159">Perform the following steps.</span></span>

    <span data-ttu-id="cc349-160">a.</span><span class="sxs-lookup"><span data-stu-id="cc349-160">a.</span></span> <span data-ttu-id="cc349-161">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="cc349-161">Create a new notebook.</span></span> <span data-ttu-id="cc349-162">Kattintson a jobb oldali sarokban **új**.</span><span class="sxs-lookup"><span data-stu-id="cc349-162">From the right-hand corner, click **New**.</span></span> <span data-ttu-id="cc349-163">Az alapértelmezett kernel kell megjelennie **Python2** és a két új kernelek, a telepítés **PySpark** és **Spark**.</span><span class="sxs-lookup"><span data-stu-id="cc349-163">You should see the default kernel **Python2** and the two new kernels that you install, **PySpark** and **Spark**.</span></span> <span data-ttu-id="cc349-164">Kattintson a **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="cc349-164">Click **PySpark**.</span></span>

    <span data-ttu-id="cc349-165">![A Jupyter notebook kernelek](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "kernelek a Jupyter notebook")</span><span class="sxs-lookup"><span data-stu-id="cc349-165">![Kernels in Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter notebook")</span></span>

    <span data-ttu-id="cc349-166">b.</span><span class="sxs-lookup"><span data-stu-id="cc349-166">b.</span></span> <span data-ttu-id="cc349-167">Futtassa a következő kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="cc349-167">Run the following code snippet.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    <span data-ttu-id="cc349-168">A kimeneti sikeresen le, ha a kapcsolat a HDInsight-fürthöz tesztelése.</span><span class="sxs-lookup"><span data-stu-id="cc349-168">If you can successfully retrieve the output, your connection to the HDInsight cluster is tested.</span></span>

    >[!TIP]
    ><span data-ttu-id="cc349-169">Ha szeretné frissíteni a notebook konfigurációját egy másik fürthöz való csatlakozáshoz, frissítse a config.json az új értékhalmazt látható a fenti a 3. lépésben.</span><span class="sxs-lookup"><span data-stu-id="cc349-169">If you want to update the notebook configuration to connect to a different cluster, update the config.json with the new set of values, as shown in Step 3 above.</span></span>

## <a name="why-should-i-install-jupyter-on-my-computer"></a><span data-ttu-id="cc349-170">Miért kell telepíteni a számítógépre Jupyter?</span><span class="sxs-lookup"><span data-stu-id="cc349-170">Why should I install Jupyter on my computer?</span></span>
<span data-ttu-id="cc349-171">Számos oka, hogy miért érdemes Jupyter telepítse a számítógépre, és csatlakoztassa a HDInsight Spark-fürt lehet.</span><span class="sxs-lookup"><span data-stu-id="cc349-171">There can be a number of reasons why you might want to install Jupyter on your computer and then connect it to a Spark cluster on HDInsight.</span></span>

* <span data-ttu-id="cc349-172">Annak ellenére, hogy a Jupyter notebookok már elérhetők az Azure HDInsight Spark-fürt, Jupyter telepíti a számítógépre, lehetőséget biztosít a notebookok helyileg létrehozása, tesztelheti az alkalmazást egy működő fürthöz, majd töltse fel a notebookok a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cc349-172">Even though Jupyter notebooks are already available on the Spark cluster in Azure HDInsight, installing Jupyter on your computer provides you the option to create your notebooks locally, test your application against a running cluster, and then upload the notebooks to the cluster.</span></span> <span data-ttu-id="cc349-173">Töltse fel a notebookok a fürthöz, is feltöltheti ezeket a Jupyter notebook futtató vagy a fürt, vagy menti a tárfiók a fürthöz tartozó /HdiNotebooks mappájában.</span><span class="sxs-lookup"><span data-stu-id="cc349-173">To upload the notebooks to the cluster, you can either upload them using the Jupyter notebook that is running or the cluster, or save them to the /HdiNotebooks folder in the storage account associated with the cluster.</span></span> <span data-ttu-id="cc349-174">A fürt notebookok tárolásával további információkért lásd: [Jupyter notebookok tároló](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span><span class="sxs-lookup"><span data-stu-id="cc349-174">For more information on how notebooks are stored on the cluster, see [Where are Jupyter notebooks stored](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span></span>
* <span data-ttu-id="cc349-175">Elérhető notebookok helyileg, akkor csatlakozhatnak különböző Spark-fürtjei alapján az alkalmazás követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="cc349-175">With the notebooks available locally, you can connect to different Spark clusters based on your application requirement.</span></span>
* <span data-ttu-id="cc349-176">GitHub segítségével valósítja meg a forrásrendszerben vezérlő és a notebookok verzió-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="cc349-176">You can use GitHub to implement a source control system and have version control for the notebooks.</span></span> <span data-ttu-id="cc349-177">A együttműködési környezetekben, ahol több felhasználó dolgozhat ugyanazon a notebook is lehet.</span><span class="sxs-lookup"><span data-stu-id="cc349-177">You can also have a collaborative environment where multiple users can work with the same notebook.</span></span>
* <span data-ttu-id="cc349-178">Dolgozhat notebookok helyileg egy fürtöt is nélkül.</span><span class="sxs-lookup"><span data-stu-id="cc349-178">You can work with notebooks locally without even having a cluster up.</span></span> <span data-ttu-id="cc349-179">Csak akkor kell tesztelni a notebookok ellen, nem kézi kezelését a jegyzetfüzetek és a környezet egy fürt.</span><span class="sxs-lookup"><span data-stu-id="cc349-179">You only need a cluster to test your notebooks against, not to manually manage your notebooks or a development environment.</span></span>
* <span data-ttu-id="cc349-180">Elképzelhető, hogy könnyebben a saját helyi fejlesztési környezet konfigurálása, mint a Jupyter telepítésének konfigurálása a fürtön.</span><span class="sxs-lookup"><span data-stu-id="cc349-180">It may be easier to configure your own local development environment than it is to configure the Jupyter installation on the cluster.</span></span>  <span data-ttu-id="cc349-181">Kihasználhatja a egy vagy több távoli fürtök beállítása nélkül helyileg telepített összes szoftver.</span><span class="sxs-lookup"><span data-stu-id="cc349-181">You can take advantage of all the software you have installed locally without configuring one or more remote clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="cc349-182">Jupyter a helyi számítógépen, a több felhasználó futtathatja a azonos notebook Spark-fürt egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cc349-182">With Jupyter installed on your local computer, multiple users can run the same notebook on the same Spark cluster at the same time.</span></span> <span data-ttu-id="cc349-183">Ilyen esetben több Livy munkamenetek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cc349-183">In such a situation, multiple Livy sessions are created.</span></span> <span data-ttu-id="cc349-184">Ha problémát tapasztal, és végezzen azon hibakeresést szeretne, mely felhasználó tartozik egy összetett feladat, mely Livy munkamenet nyomkövetéséhez lesz.</span><span class="sxs-lookup"><span data-stu-id="cc349-184">If you run into an issue and want to debug that, it will be a complex task to track which Livy session belongs to which user.</span></span>
>
>

## <span data-ttu-id="cc349-185"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="cc349-185"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="cc349-186">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="cc349-186">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="cc349-187">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="cc349-187">Scenarios</span></span>
* [<span data-ttu-id="cc349-188">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="cc349-188">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="cc349-189">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="cc349-189">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="cc349-190">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="cc349-190">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="cc349-191">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="cc349-191">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="cc349-192">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="cc349-192">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="cc349-193">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="cc349-193">Create and run applications</span></span>
* [<span data-ttu-id="cc349-194">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="cc349-194">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="cc349-195">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="cc349-195">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="cc349-196">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="cc349-196">Tools and extensions</span></span>
* [<span data-ttu-id="cc349-197">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="cc349-197">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="cc349-198">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="cc349-198">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="cc349-199">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="cc349-199">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="cc349-200">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="cc349-200">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="cc349-201">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="cc349-201">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a><span data-ttu-id="cc349-202">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="cc349-202">Manage resources</span></span>
* [<span data-ttu-id="cc349-203">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="cc349-203">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="cc349-204">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="cc349-204">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
