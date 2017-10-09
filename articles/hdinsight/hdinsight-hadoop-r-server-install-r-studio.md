---
title: "az R Server on HDInsight - Azure az Rstudióból aaaInstall |} Microsoft Docs"
description: "Hogyan tooinstall az R Server on HDInsight az Rstudióból."
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
ms.openlocfilehash: b3a23021fcf99217e8f551f8b2e89bf1f1e5b967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a><span data-ttu-id="63dd7-103">Az R Server on HDInsight az Rstudióból telepítése</span><span class="sxs-lookup"><span data-stu-id="63dd7-103">Installing RStudio with R Server on HDInsight</span></span>

<span data-ttu-id="63dd7-104">Ez a cikk ismerteti, hogyan tooinstall hello community (ingyenes) verzióját [Rstudióból Server](https://www.rstudio.com/products/rstudio-server/) hello peremhálózati egy fürt csomópontján egyéni parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="63dd7-104">This article describes how tooinstall hello community (free) version of [RStudio Server](https://www.rstudio.com/products/rstudio-server/) on hello edge node of a cluster using a custom script.</span></span> <span data-ttu-id="63dd7-105">Rstudióból kiszolgáló távoli ügyfelek számára biztosít egy webböngésző-alapú IDE, és Linux széles körben használható.</span><span class="sxs-lookup"><span data-stu-id="63dd7-105">RStudio Server provides a browser-based IDE for use by remote clients and is widely used on Linux.</span></span> <span data-ttu-id="63dd7-106">Van több integrált fejlesztési környezet (IDEs) érhető el, az R ma, beleértve:</span><span class="sxs-lookup"><span data-stu-id="63dd7-106">There are multiple integrated development environments (IDEs) available for R today, including:</span></span>

- <span data-ttu-id="63dd7-107">A Microsoft [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span><span class="sxs-lookup"><span data-stu-id="63dd7-107">Microsoft’s  [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span></span> 
- [<span data-ttu-id="63dd7-108">Rstudióból kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="63dd7-108">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio-server/) 
- <span data-ttu-id="63dd7-109">Walware tartozó Eclipse-alapú [StatET](http://www.walware.de/goto/statet).</span><span class="sxs-lookup"><span data-stu-id="63dd7-109">Walware’s Eclipse-based [StatET](http://www.walware.de/goto/statet).</span></span>

<span data-ttu-id="63dd7-110">hello Rstudióból kiszolgáló telepítése egy HDInsight-fürt hello peremhálózati csomóponton előnye, hogy az teljes IDE élményt nyújt hello fejlesztési és az R parancsfájlok végrehajtása az R Server hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="63dd7-110">hello advantage of installing RStudio Server on hello edge node of an HDInsight cluster is that it provides a full IDE experience for hello development and execution of R scripts with R Server on hello cluster.</span></span> <span data-ttu-id="63dd7-111">Lehet, hogy ez a konfiguráció jelentősen hatékonyabb, mint hello R konzol alapértelmezett használatát.</span><span class="sxs-lookup"><span data-stu-id="63dd7-111">This configuration can be considerably more productive than default use of hello R Console.</span></span>

> [!NOTE]
> <span data-ttu-id="63dd7-112">Ebben a cikkben leírt hello eljárás csak akkor megfelelő, ha nem jelölte be tooinstall Rstudióból Server community edition a fürt kiépítésekor.</span><span class="sxs-lookup"><span data-stu-id="63dd7-112">hello procedure described in this article is only relevant if you did not select tooinstall RStudio Server community edition when provisioning your cluster.</span></span> <span data-ttu-id="63dd7-113">Ha hozzáadott telepítése során, majd, kattintson a hello tooaccess **R Server irányítópultok** csempe az Azure portál bejegyzés a fürt számára, majd a hello hello **R Studio Server** csempére.</span><span class="sxs-lookup"><span data-stu-id="63dd7-113">If you added it during provisioning, then tooaccess it you click on hello **R Server Dashboards** tile in hello Azure portal entry for your cluster, then on hello **R Studio Server** tile.</span></span> 

<span data-ttu-id="63dd7-114">Toouse kereskedelmileg licencelt hello Pro Rstudióból Server verziója tetszés szerint hajtsa végre az hello telepítési utasításokat a [Rstudióból Server](https://www.rstudio.com/products/rstudio/download-server/).</span><span class="sxs-lookup"><span data-stu-id="63dd7-114">If you prefer toouse hello commercially licensed Pro version of RStudio Server, you must follow hello installation instructions from [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span></span>

> [!NOTE]
> <span data-ttu-id="63dd7-115">Egy HDInsight-fürt, amelynek R telepítette hello használata [R parancsfájlművelet telepítése](hdinsight-hadoop-r-scripts-linux.md), nem működnek a jelen dokumentumban leírt lépések hello igényelnek-e az R Server on HDInsight-fürt hello megfelelően.</span><span class="sxs-lookup"><span data-stu-id="63dd7-115">If you are using an HDInsight cluster for which R was installed using hello [Install R Script Action](hdinsight-hadoop-r-scripts-linux.md), hello steps in this document will not work correctly as they require an R Server on hello HDInsight cluster.</span></span>
>
> 

## <a name="prerequisites"></a><span data-ttu-id="63dd7-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63dd7-116">Prerequisites</span></span>

* <span data-ttu-id="63dd7-117">R Server telepített Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="63dd7-117">An Azure HDInsight cluster with R Server installed.</span></span> <span data-ttu-id="63dd7-118">Útmutatásért lásd: [az R Server első lépései a HDInsight-fürtök](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="63dd7-118">For instructions, see [Get started with R Server on HDInsight clusters](hdinsight-hadoop-r-server-get-started.md).</span></span>
* <span data-ttu-id="63dd7-119">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="63dd7-119">An SSH client.</span></span> <span data-ttu-id="63dd7-120">A Linux és Unix megfelelő kiadásának vagy Macintosh-OS X, hello `ssh` parancs hello operációs rendszer által biztosított.</span><span class="sxs-lookup"><span data-stu-id="63dd7-120">For Linux and Unix distributions or Macintosh OS X, hello `ssh` command is provided with hello operating system.</span></span> <span data-ttu-id="63dd7-121">A Windows, ajánlott [Cygwin](http://www.redhat.com/services/custom/cygwin/) a hello [OpenSSH beállítás](https://www.youtube.com/watch?v=CwYSvvGaiWU), vagy [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="63dd7-121">For Windows, we recommend [Cygwin](http://www.redhat.com/services/custom/cygwin/) with hello [OpenSSH option](https://www.youtube.com/watch?v=CwYSvvGaiWU), or [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>  

## <a name="install-rstudio-on-hello-cluster-using-a-custom-script"></a><span data-ttu-id="63dd7-122">Egyéni parancsfájl használata hello fürtön telepíteni a Rstudióból</span><span class="sxs-lookup"><span data-stu-id="63dd7-122">Install RStudio on hello cluster using a custom script</span></span>

<span data-ttu-id="63dd7-123">Az alábbiakban hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63dd7-123">Here are hello steps:</span></span>

1. <span data-ttu-id="63dd7-124">Hello élcsomópont hello fürt azonosításához.</span><span class="sxs-lookup"><span data-stu-id="63dd7-124">Identify hello edge node of hello cluster.</span></span> <span data-ttu-id="63dd7-125">A HDInsight-fürtök az R Server az alábbiakban az átjárócsomópont és élcsomópont hello elnevezési.</span><span class="sxs-lookup"><span data-stu-id="63dd7-125">For an HDInsight cluster with R Server, following is hello naming convention for head node and edge node.</span></span>
   * <span data-ttu-id="63dd7-126">Átjárócsomópont`CLUSTERNAME-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="63dd7-126">Head node `CLUSTERNAME-ssh.azurehdinsight.net`</span></span>
   * <span data-ttu-id="63dd7-127">Élcsomópont`CLUSTERNAME-ed-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="63dd7-127">Edge node `CLUSTERNAME-ed-ssh.azurehdinsight.net`</span></span> 

2. <span data-ttu-id="63dd7-128">SSH-ból hello élcsomópont hello fürt hello elnevezési minta az 1. lépésben megadott használatával.</span><span class="sxs-lookup"><span data-stu-id="63dd7-128">SSH into hello edge node of hello cluster using hello naming pattern provided in step 1.</span></span> <span data-ttu-id="63dd7-129">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="63dd7-129">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="63dd7-130">Miután csatlakozott, vált a gyökér szintű felhasználó hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="63dd7-130">Once you are connected, become a root user on hello cluster.</span></span> <span data-ttu-id="63dd7-131">Hello SSH-munkamenetet, a parancs a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="63dd7-131">In hello SSH session, use hello following command:</span></span>

        sudo su -

4. <span data-ttu-id="63dd7-132">Töltse le a hello egyéni parancsfájl tooinstall Rstudióból.</span><span class="sxs-lookup"><span data-stu-id="63dd7-132">Download hello custom script tooinstall RStudio.</span></span> <span data-ttu-id="63dd7-133">A következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="63dd7-133">Use hello following command:</span></span>

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. <span data-ttu-id="63dd7-134">Egyéni parancsfájl hello hello engedélyeinek módosítása, és hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="63dd7-134">Change hello permissions on hello custom script file and run hello script.</span></span> <span data-ttu-id="63dd7-135">A következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="63dd7-135">Use hello following commands:</span></span>

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. <span data-ttu-id="63dd7-136">R Server a HDInsight-fürtök létrehozásakor egy SSH-jelszó használata esetén ezt a lépést kihagyhatja, és folytassa tovább toohello.</span><span class="sxs-lookup"><span data-stu-id="63dd7-136">If you used an SSH password while creating an HDInsight cluster with R Server, you can skip this step and proceed toohello next.</span></span> <span data-ttu-id="63dd7-137">Ha SSH-kulcs helyette toocreate hello fürtöt, meg kell adni az SSH-felhasználó jelszót.</span><span class="sxs-lookup"><span data-stu-id="63dd7-137">If you used an SSH key instead toocreate hello cluster, you must set a password for your SSH user.</span></span> <span data-ttu-id="63dd7-138">Ezt a jelszót kell tooRStudio kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="63dd7-138">You need this password when connecting tooRStudio.</span></span> <span data-ttu-id="63dd7-139">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="63dd7-139">Run hello following commands:</span></span>

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. <span data-ttu-id="63dd7-140">Ha a rendszer kéri **aktuális Kerberos jelszó**, nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="63dd7-140">When prompted for **Current Kerberos password**, press **ENTER**.</span></span>  <span data-ttu-id="63dd7-141">Vegye figyelembe, hogy le kell cserélnie `USERNAME` rendelkező a HDInsight-fürthöz az SSH-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="63dd7-141">Note that you must replace `USERNAME` with an SSH user for your HDInsight cluster.</span></span> <span data-ttu-id="63dd7-142">Ha sikeresen beállította a jelszavát, a következő üzenet hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="63dd7-142">If your password is successfully set, you should see hello following message:</span></span>

        passwd: password updated successfully

    <span data-ttu-id="63dd7-143">Lépjen ki a hello SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="63dd7-143">Exit hello SSH session.</span></span>

8. <span data-ttu-id="63dd7-144">Hozzon létre egy SSH-alagút toohello fürt leképezési `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` hello HDInsight fürtön toohello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="63dd7-144">Create an SSH tunnel toohello cluster by mapping `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` on hello HDInsight cluster toohello client machine.</span></span> <span data-ttu-id="63dd7-145">Az SSH-alagút létre kell hoznia egy új böngésző-munkamenet megnyitása előtt.</span><span class="sxs-lookup"><span data-stu-id="63dd7-145">You must create an SSH tunnel before opening a new browser session.</span></span>

   * <span data-ttu-id="63dd7-146">A Linux-ügyfél vagy egy Windows ügyfél [Cygwin](http://www.redhat.com/services/custom/cygwin/)nyisson meg egy terminál-munkamenetet, és használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="63dd7-146">On a Linux client or a Windows client with [Cygwin](http://www.redhat.com/services/custom/cygwin/), open a terminal session and use hello following command:</span></span>

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       <span data-ttu-id="63dd7-147">Cserélje le **felhasználónév** az SSH-felhasználó a HDInsight-fürtöt, és cserélje le a **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="63dd7-147">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>
       <span data-ttu-id="63dd7-148">Használhatja a jelszó helyett egy SSH-kulcs hozzáadásával `-i id_rsa_key`.</span><span class="sxs-lookup"><span data-stu-id="63dd7-148">You can also use an SSH key rather than a password by adding `-i id_rsa_key`.</span></span>        
   * <span data-ttu-id="63dd7-149">Ha a Windows-ügyfél majd a PuTTY használatával</span><span class="sxs-lookup"><span data-stu-id="63dd7-149">If using a Windows client with PuTTY then</span></span>

     1. <span data-ttu-id="63dd7-150">Nyissa meg a PuTTY, és adja meg a kapcsolódási adatokat.</span><span class="sxs-lookup"><span data-stu-id="63dd7-150">Open PuTTY, and enter your connection information.</span></span>
     2. <span data-ttu-id="63dd7-151">A hello **kategória** szakasz toohello hello párbeszédpanel bal oldali, bontsa ki a **kapcsolat**, bontsa ki a **SSH**, majd válassza ki **alagutak**.</span><span class="sxs-lookup"><span data-stu-id="63dd7-151">In hello **Category** section toohello left of hello dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>
     3. <span data-ttu-id="63dd7-152">Adja meg a következő információ a hello hello **vezérlő beállításokban SSH port átirányítása** űrlap:</span><span class="sxs-lookup"><span data-stu-id="63dd7-152">Provide hello following information on hello **Options controlling SSH port forwarding** form:</span></span>

        * <span data-ttu-id="63dd7-153">**Forrásport** -port, hogy kívánja-e tooforward hello ügyfélen hello.</span><span class="sxs-lookup"><span data-stu-id="63dd7-153">**Source port** - hello port on hello client that you wish tooforward.</span></span> <span data-ttu-id="63dd7-154">Például **8787**.</span><span class="sxs-lookup"><span data-stu-id="63dd7-154">For example, **8787**.</span></span>
        * <span data-ttu-id="63dd7-155">**Cél** – hello kell lennie a cél leképezése toohello helyi ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="63dd7-155">**Destination** - hello destination that must be mapped toohello local client machine.</span></span> <span data-ttu-id="63dd7-156">Például **localhost:8787**.</span><span class="sxs-lookup"><span data-stu-id="63dd7-156">For example, **localhost:8787**.</span></span>

            <span data-ttu-id="63dd7-157">![SSH-alagút létrehozása](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "SSH-alagút létrehozása")</span><span class="sxs-lookup"><span data-stu-id="63dd7-157">![Create an SSH tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Create an SSH tunnel")</span></span>

     4. <span data-ttu-id="63dd7-158">Kattintson a **Hozzáadás** tooadd hello beállításait, és kattintson a **nyitott** tooopen az SSH-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="63dd7-158">Click **Add** tooadd hello settings, and then click **Open** tooopen an SSH connection.</span></span>
     5. <span data-ttu-id="63dd7-159">Amikor a rendszer kéri, jelentkezzen be toohello server tooestablish SSH munkamenet és engedélyezése hello-alagutat.</span><span class="sxs-lookup"><span data-stu-id="63dd7-159">When prompted, log in toohello server tooestablish an SSH session and enable hello tunnel.</span></span>

9. <span data-ttu-id="63dd7-160">Nyisson meg egy webböngészőt, és írja be a következő URL-cím a megadott hello alagúthoz hello port alapján hello:</span><span class="sxs-lookup"><span data-stu-id="63dd7-160">Open a web browser and enter hello following URL based on hello port you entered for hello tunnel:</span></span>

        http://localhost:8787/ 

10. <span data-ttu-id="63dd7-161">Rákérdezéses tooenter hello SSH felhasználónév és jelszó tooconnect toohello fürt áll.</span><span class="sxs-lookup"><span data-stu-id="63dd7-161">You are prompted tooenter hello SSH username and password tooconnect toohello cluster.</span></span> <span data-ttu-id="63dd7-162">Ha SSH-kulcs hello fürt létrehozása során, 5. lépésben létrehozott hello jelszót kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="63dd7-162">If you used an SSH key while creating hello cluster, you must enter hello password you created in step 5.</span></span>

    <span data-ttu-id="63dd7-163">![Csatlakozás bemutató Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "SSH-alagút létrehozása")</span><span class="sxs-lookup"><span data-stu-id="63dd7-163">![Connect tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Create an SSH tunnel")</span></span>

11. <span data-ttu-id="63dd7-164">tootest hello Rstudióból telepítése sikeres volt, akkor futtatható-e a teszt parancsfájl, amely végrehajtja az R-alapú MapReduce és Spark feladatok hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="63dd7-164">tootest whether hello RStudio installation was successful, you can run a test script that executes R-based MapReduce and Spark jobs on hello cluster.</span></span> <span data-ttu-id="63dd7-165">toodownload hello teszt parancsfájl toorun az Rstudióból, lépjen vissza toohello SSH-konzolon, és adja meg a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="63dd7-165">toodownload hello test script toorun in RStudio, go back toohello SSH console and enter hello following commands:</span></span>

    *    <span data-ttu-id="63dd7-166">A Hadoop fürtök R hozta létre, ha az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="63dd7-166">If you created a Hadoop cluster with R, use this command:</span></span>

            <span data-ttu-id="63dd7-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span><span class="sxs-lookup"><span data-stu-id="63dd7-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span></span>
    *    <span data-ttu-id="63dd7-168">Ha létrehozott egy Spark-fürt az R, az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="63dd7-168">If you created a Spark cluster with R, use this command:</span></span>

            <span data-ttu-id="63dd7-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span><span class="sxs-lookup"><span data-stu-id="63dd7-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span></span>

12. <span data-ttu-id="63dd7-170">Az Rstudióból lásd: hello letöltött parancsprogram teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="63dd7-170">In RStudio, you see hello test script you downloaded.</span></span> <span data-ttu-id="63dd7-171">Hello fájl tooopen, jelölje ki a hello hello fájl tartalmát, és kattintson duplán **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="63dd7-171">Double-click hello file tooopen it, select hello contents of hello file, and then click **Run**.</span></span> <span data-ttu-id="63dd7-172">A hello hello kimenetet kell látnia **konzol** panelen:</span><span class="sxs-lookup"><span data-stu-id="63dd7-172">You should see hello output in hello **Console** pane:</span></span>

   <span data-ttu-id="63dd7-173">![Hello-telepítés sikerességének ellenőrzése](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "hello-telepítés sikerességének ellenőrzése")</span><span class="sxs-lookup"><span data-stu-id="63dd7-173">![Test hello installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test hello installation")</span></span>

<span data-ttu-id="63dd7-174">Egy másik lehetőség lenne tootype `source(testhdi.r)` vagy `source(testhdi_spark.r)` tooexecute hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="63dd7-174">Another option would be tootype `source(testhdi.r)` or `source(testhdi_spark.r)` tooexecute hello script.</span></span>

## <a name="see-also"></a><span data-ttu-id="63dd7-175">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="63dd7-175">See also</span></span>

* [<span data-ttu-id="63dd7-176">Számítási környezet lehetőségek R Server a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="63dd7-176">Compute context options for R Server on HDInsight clusters</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="63dd7-177">Azure Storage lehetőségek a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="63dd7-177">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)

