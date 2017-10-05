---
title: "Az R Server on HDInsight - Azure Rstudióból telepítése |} Microsoft Docs"
description: "Hogyan Rstudióból telepíteni az R Server on HDInsight."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a><span data-ttu-id="7af51-103">Az R Server on HDInsight az Rstudióból telepítése</span><span class="sxs-lookup"><span data-stu-id="7af51-103">Installing RStudio with R Server on HDInsight</span></span>

<span data-ttu-id="7af51-104">Ez a cikk ismerteti a community (ingyenes) verziójának telepítéséhez [Rstudióból Server](https://www.rstudio.com/products/rstudio-server/) egyéni parancsfájl használata a fürt peremhálózati csomóponton.</span><span class="sxs-lookup"><span data-stu-id="7af51-104">This article describes how to install the community (free) version of [RStudio Server](https://www.rstudio.com/products/rstudio-server/) on the edge node of a cluster using a custom script.</span></span> <span data-ttu-id="7af51-105">Rstudióból kiszolgáló távoli ügyfelek számára biztosít egy webböngésző-alapú IDE, és Linux széles körben használható.</span><span class="sxs-lookup"><span data-stu-id="7af51-105">RStudio Server provides a browser-based IDE for use by remote clients and is widely used on Linux.</span></span> <span data-ttu-id="7af51-106">Van több integrált fejlesztési környezet (IDEs) érhető el, az R ma, beleértve:</span><span class="sxs-lookup"><span data-stu-id="7af51-106">There are multiple integrated development environments (IDEs) available for R today, including:</span></span>

- <span data-ttu-id="7af51-107">A Microsoft [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span><span class="sxs-lookup"><span data-stu-id="7af51-107">Microsoft’s  [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span></span> 
- [<span data-ttu-id="7af51-108">Rstudióból kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="7af51-108">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio-server/) 
- <span data-ttu-id="7af51-109">Walware tartozó Eclipse-alapú [StatET](http://www.walware.de/goto/statet).</span><span class="sxs-lookup"><span data-stu-id="7af51-109">Walware’s Eclipse-based [StatET](http://www.walware.de/goto/statet).</span></span>

<span data-ttu-id="7af51-110">A HDInsight-fürtök élcsomópont Rstudióból-kiszolgálót telepítse előnye, hogy az teljes IDE élményt nyújt a fejlesztési és az R parancsfájlok végrehajtása az R Server a fürtön.</span><span class="sxs-lookup"><span data-stu-id="7af51-110">The advantage of installing RStudio Server on the edge node of an HDInsight cluster is that it provides a full IDE experience for the development and execution of R scripts with R Server on the cluster.</span></span> <span data-ttu-id="7af51-111">Lehet, hogy ez a konfiguráció jelentős mértékben hatékonyabb, mint az alapértelmezett R konzol használata.</span><span class="sxs-lookup"><span data-stu-id="7af51-111">This configuration can be considerably more productive than default use of the R Console.</span></span>

> [!NOTE]
> <span data-ttu-id="7af51-112">Az ebben a cikkben ismertetett eljárás csak fontos, ha nem jelölte be telepítse Rstudióból Server közösségi verziót, ha a fürt.</span><span class="sxs-lookup"><span data-stu-id="7af51-112">The procedure described in this article is only relevant if you did not select to install RStudio Server community edition when provisioning your cluster.</span></span> <span data-ttu-id="7af51-113">Ha a kiépítés során hozzáadott, akkor az eléréséhez kattintson a a **R Server irányítópultok** csempét a az Azure portál bejegyzése a fürtöt, majd a **R Studio Server** csempére.</span><span class="sxs-lookup"><span data-stu-id="7af51-113">If you added it during provisioning, then to access it you click on the **R Server Dashboards** tile in the Azure portal entry for your cluster, then on the **R Studio Server** tile.</span></span> 

<span data-ttu-id="7af51-114">Ha inkább az Rstudióból Server kereskedelmileg licencelt Pro verzióját használja, a telepítési utasításokat hajtsa végre az [Rstudióból Server](https://www.rstudio.com/products/rstudio/download-server/).</span><span class="sxs-lookup"><span data-stu-id="7af51-114">If you prefer to use the commercially licensed Pro version of RStudio Server, you must follow the installation instructions from [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span></span>

> [!NOTE]
> <span data-ttu-id="7af51-115">Amelynek R telepítette HDInsight-fürtök használata a [telepítése R parancsfájlművelet](hdinsight-hadoop-r-scripts-linux.md), a jelen dokumentumban leírt lépések nem működnek megfelelően igényelnek-e a HDInsight-fürt egyik R Server.</span><span class="sxs-lookup"><span data-stu-id="7af51-115">If you are using an HDInsight cluster for which R was installed using the [Install R Script Action](hdinsight-hadoop-r-scripts-linux.md), the steps in this document will not work correctly as they require an R Server on the HDInsight cluster.</span></span>
>
> 

## <a name="prerequisites"></a><span data-ttu-id="7af51-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7af51-116">Prerequisites</span></span>

* <span data-ttu-id="7af51-117">R Server telepített Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="7af51-117">An Azure HDInsight cluster with R Server installed.</span></span> <span data-ttu-id="7af51-118">Útmutatásért lásd: [az R Server első lépései a HDInsight-fürtök](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7af51-118">For instructions, see [Get started with R Server on HDInsight clusters](hdinsight-hadoop-r-server-get-started.md).</span></span>
* <span data-ttu-id="7af51-119">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="7af51-119">An SSH client.</span></span> <span data-ttu-id="7af51-120">A Linux és Unix megfelelő kiadásának vagy Macintosh-OS X a `ssh` parancs az operációs rendszer által biztosított.</span><span class="sxs-lookup"><span data-stu-id="7af51-120">For Linux and Unix distributions or Macintosh OS X, the `ssh` command is provided with the operating system.</span></span> <span data-ttu-id="7af51-121">A Windows, ajánlott [Cygwin](http://www.redhat.com/services/custom/cygwin/) rendelkező a [OpenSSH beállítás](https://www.youtube.com/watch?v=CwYSvvGaiWU), vagy [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="7af51-121">For Windows, we recommend [Cygwin](http://www.redhat.com/services/custom/cygwin/) with the [OpenSSH option](https://www.youtube.com/watch?v=CwYSvvGaiWU), or [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a><span data-ttu-id="7af51-122">Egyéni parancsfájl használata a fürt Rstudióból telepítése</span><span class="sxs-lookup"><span data-stu-id="7af51-122">Install RStudio on the cluster using a custom script</span></span>

<span data-ttu-id="7af51-123">A lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="7af51-123">Here are the steps:</span></span>

1. <span data-ttu-id="7af51-124">A fürt élcsomópont azonosításához.</span><span class="sxs-lookup"><span data-stu-id="7af51-124">Identify the edge node of the cluster.</span></span> <span data-ttu-id="7af51-125">A HDInsight-fürtök az R Server az alábbiakban az átjárócsomópont és élcsomópont elnevezési konvenciót.</span><span class="sxs-lookup"><span data-stu-id="7af51-125">For an HDInsight cluster with R Server, following is the naming convention for head node and edge node.</span></span>
   * <span data-ttu-id="7af51-126">Átjárócsomópont`CLUSTERNAME-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="7af51-126">Head node `CLUSTERNAME-ssh.azurehdinsight.net`</span></span>
   * <span data-ttu-id="7af51-127">Élcsomópont`CLUSTERNAME-ed-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="7af51-127">Edge node `CLUSTERNAME-ed-ssh.azurehdinsight.net`</span></span> 

2. <span data-ttu-id="7af51-128">SSH-ból az élcsomóponthoz a fürt az elnevezési minta az 1. lépésben megadott használatával.</span><span class="sxs-lookup"><span data-stu-id="7af51-128">SSH into the edge node of the cluster using the naming pattern provided in step 1.</span></span> <span data-ttu-id="7af51-129">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7af51-129">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="7af51-130">Miután csatlakozott, vált a gyökér szintű felhasználó a fürtön.</span><span class="sxs-lookup"><span data-stu-id="7af51-130">Once you are connected, become a root user on the cluster.</span></span> <span data-ttu-id="7af51-131">Az SSH-munkamenetet a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="7af51-131">In the SSH session, use the following command:</span></span>

        sudo su -

4. <span data-ttu-id="7af51-132">Töltse le a egyéni parancsfájl Rstudióból telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7af51-132">Download the custom script to install RStudio.</span></span> <span data-ttu-id="7af51-133">Használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="7af51-133">Use the following command:</span></span>

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. <span data-ttu-id="7af51-134">Az egyéni parancsfájl a engedélyeit, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="7af51-134">Change the permissions on the custom script file and run the script.</span></span> <span data-ttu-id="7af51-135">Az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="7af51-135">Use the following commands:</span></span>

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. <span data-ttu-id="7af51-136">R Server a HDInsight-fürtök létrehozásakor egy SSH-jelszó használata esetén ezt a lépést kihagyhatja, és folytassa a következő.</span><span class="sxs-lookup"><span data-stu-id="7af51-136">If you used an SSH password while creating an HDInsight cluster with R Server, you can skip this step and proceed to the next.</span></span> <span data-ttu-id="7af51-137">Ha SSH-kulcs inkább a fürt létrehozása, meg kell adni az SSH-felhasználó jelszót.</span><span class="sxs-lookup"><span data-stu-id="7af51-137">If you used an SSH key instead to create the cluster, you must set a password for your SSH user.</span></span> <span data-ttu-id="7af51-138">Ezt a jelszót kell Rstudióból történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="7af51-138">You need this password when connecting to RStudio.</span></span> <span data-ttu-id="7af51-139">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="7af51-139">Run the following commands:</span></span>

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. <span data-ttu-id="7af51-140">Ha a rendszer kéri **aktuális Kerberos jelszó**, nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="7af51-140">When prompted for **Current Kerberos password**, press **ENTER**.</span></span>  <span data-ttu-id="7af51-141">Vegye figyelembe, hogy le kell cserélnie `USERNAME` rendelkező a HDInsight-fürthöz az SSH-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7af51-141">Note that you must replace `USERNAME` with an SSH user for your HDInsight cluster.</span></span> <span data-ttu-id="7af51-142">Ha a jelszó sikeresen be van állítva, a következő üzenetet kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7af51-142">If your password is successfully set, you should see the following message:</span></span>

        passwd: password updated successfully

    <span data-ttu-id="7af51-143">Lépjen ki az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="7af51-143">Exit the SSH session.</span></span>

8. <span data-ttu-id="7af51-144">A fürthöz SSH-alagút létrehozása leképezi `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` a HDInsight-fürt, az ügyfélszámítógépnek.</span><span class="sxs-lookup"><span data-stu-id="7af51-144">Create an SSH tunnel to the cluster by mapping `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` on the HDInsight cluster to the client machine.</span></span> <span data-ttu-id="7af51-145">Az SSH-alagút létre kell hoznia egy új böngésző-munkamenet megnyitása előtt.</span><span class="sxs-lookup"><span data-stu-id="7af51-145">You must create an SSH tunnel before opening a new browser session.</span></span>

   * <span data-ttu-id="7af51-146">A Linux-ügyfél vagy egy Windows ügyfél [Cygwin](http://www.redhat.com/services/custom/cygwin/)nyisson meg egy terminál-munkamenetet, és használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7af51-146">On a Linux client or a Windows client with [Cygwin](http://www.redhat.com/services/custom/cygwin/), open a terminal session and use the following command:</span></span>

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       <span data-ttu-id="7af51-147">Cserélje le **felhasználónév** az SSH-felhasználó a HDInsight-fürtöt, és cserélje le a **CLUSTERNAME** a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="7af51-147">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>
       <span data-ttu-id="7af51-148">Használhatja a jelszó helyett egy SSH-kulcs hozzáadásával `-i id_rsa_key`.</span><span class="sxs-lookup"><span data-stu-id="7af51-148">You can also use an SSH key rather than a password by adding `-i id_rsa_key`.</span></span>        
   * <span data-ttu-id="7af51-149">Ha a Windows-ügyfél majd a PuTTY használatával</span><span class="sxs-lookup"><span data-stu-id="7af51-149">If using a Windows client with PuTTY then</span></span>

     1. <span data-ttu-id="7af51-150">Nyissa meg a PuTTY, és adja meg a kapcsolódási adatokat.</span><span class="sxs-lookup"><span data-stu-id="7af51-150">Open PuTTY, and enter your connection information.</span></span>
     2. <span data-ttu-id="7af51-151">Az a **kategória** a párbeszédpanel bal területen bontsa ki a **kapcsolat**, bontsa ki a **SSH**, majd válassza ki **alagutak**.</span><span class="sxs-lookup"><span data-stu-id="7af51-151">In the **Category** section to the left of the dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>
     3. <span data-ttu-id="7af51-152">A következő információkat biztosítanak a **vezérlő beállításokban SSH port átirányítása** űrlap:</span><span class="sxs-lookup"><span data-stu-id="7af51-152">Provide the following information on the **Options controlling SSH port forwarding** form:</span></span>

        * <span data-ttu-id="7af51-153">**Source port** (Forrásport) - Az ügyfélen az a port, amelyet továbbítani szeretne.</span><span class="sxs-lookup"><span data-stu-id="7af51-153">**Source port** - The port on the client that you wish to forward.</span></span> <span data-ttu-id="7af51-154">Például **8787**.</span><span class="sxs-lookup"><span data-stu-id="7af51-154">For example, **8787**.</span></span>
        * <span data-ttu-id="7af51-155">**Cél** -a cél, hogy le kell képezni a helyi ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="7af51-155">**Destination** - The destination that must be mapped to the local client machine.</span></span> <span data-ttu-id="7af51-156">Például **localhost:8787**.</span><span class="sxs-lookup"><span data-stu-id="7af51-156">For example, **localhost:8787**.</span></span>

            <span data-ttu-id="7af51-157">![SSH-alagút létrehozása](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "SSH-alagút létrehozása")</span><span class="sxs-lookup"><span data-stu-id="7af51-157">![Create an SSH tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Create an SSH tunnel")</span></span>

     4. <span data-ttu-id="7af51-158">Kattintson a **Hozzáadás** vegye fel a beállításokat, majd **nyissa meg a** az SSH-kapcsolat megnyitása.</span><span class="sxs-lookup"><span data-stu-id="7af51-158">Click **Add** to add the settings, and then click **Open** to open an SSH connection.</span></span>
     5. <span data-ttu-id="7af51-159">Amikor a rendszer kéri, jelentkezzen be a kiszolgálóra SSH-munkamenetet létesít, és engedélyezi az alagutat.</span><span class="sxs-lookup"><span data-stu-id="7af51-159">When prompted, log in to the server to establish an SSH session and enable the tunnel.</span></span>

9. <span data-ttu-id="7af51-160">Nyisson meg egy webböngészőt, és írja be a következő URL-cím az az alagutat a megadott port alapján:</span><span class="sxs-lookup"><span data-stu-id="7af51-160">Open a web browser and enter the following URL based on the port you entered for the tunnel:</span></span>

        http://localhost:8787/ 

10. <span data-ttu-id="7af51-161">Adja meg az SSH-felhasználónév és jelszó kapcsolódik a fürthöz kéri.</span><span class="sxs-lookup"><span data-stu-id="7af51-161">You are prompted to enter the SSH username and password to connect to the cluster.</span></span> <span data-ttu-id="7af51-162">Ha a fürt létrehozásakor egy SSH-kulcsot, meg kell adnia az 5. lépésben létrehozott jelszó.</span><span class="sxs-lookup"><span data-stu-id="7af51-162">If you used an SSH key while creating the cluster, you must enter the password you created in step 5.</span></span>

    <span data-ttu-id="7af51-163">![R Studio csatlakozni](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "SSH-alagút létrehozása")</span><span class="sxs-lookup"><span data-stu-id="7af51-163">![Connect to R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Create an SSH tunnel")</span></span>

11. <span data-ttu-id="7af51-164">Annak megállapítására, hogy a Rstudióból telepítés sikeres volt, a teszt parancsfájl, amely végrehajtja az R-alapú MapReduce és Spark feladatok a fürtön is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="7af51-164">To test whether the RStudio installation was successful, you can run a test script that executes R-based MapReduce and Spark jobs on the cluster.</span></span> <span data-ttu-id="7af51-165">Töltse le a teszt parancsfájl futtatását Rstudióból, lépjen vissza az SSH-konzolra, és adja meg a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="7af51-165">To download the test script to run in RStudio, go back to the SSH console and enter the following commands:</span></span>

    *    <span data-ttu-id="7af51-166">A Hadoop fürtök R hozta létre, ha az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="7af51-166">If you created a Hadoop cluster with R, use this command:</span></span>

            <span data-ttu-id="7af51-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span><span class="sxs-lookup"><span data-stu-id="7af51-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span></span>
    *    <span data-ttu-id="7af51-168">Ha létrehozott egy Spark-fürt az R, az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="7af51-168">If you created a Spark cluster with R, use this command:</span></span>

            <span data-ttu-id="7af51-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span><span class="sxs-lookup"><span data-stu-id="7af51-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span></span>

12. <span data-ttu-id="7af51-170">Az Rstudióból tekintse meg a letöltött teszt parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="7af51-170">In RStudio, you see the test script you downloaded.</span></span> <span data-ttu-id="7af51-171">Kattintson duplán a fájlra a megnyitásához, válassza ki a fájl tartalmát, majd **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="7af51-171">Double-click the file to open it, select the contents of the file, and then click **Run**.</span></span> <span data-ttu-id="7af51-172">A kimenetnek kell megjelennie a **konzol** panelen:</span><span class="sxs-lookup"><span data-stu-id="7af51-172">You should see the output in the **Console** pane:</span></span>

   <span data-ttu-id="7af51-173">![A telepítés sikerességének ellenőrzése](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "a telepítés sikerességének ellenőrzése")</span><span class="sxs-lookup"><span data-stu-id="7af51-173">![Test the installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test the installation")</span></span>

<span data-ttu-id="7af51-174">Egy másik lehetőség lenne, írja be a `source(testhdi.r)` vagy `source(testhdi_spark.r)` a parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="7af51-174">Another option would be to type `source(testhdi.r)` or `source(testhdi_spark.r)` to execute the script.</span></span>

## <a name="see-also"></a><span data-ttu-id="7af51-175">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7af51-175">See also</span></span>

* [<span data-ttu-id="7af51-176">Számítási környezet lehetőségek R Server a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="7af51-176">Compute context options for R Server on HDInsight clusters</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="7af51-177">Azure Storage lehetőségek a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="7af51-177">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)

