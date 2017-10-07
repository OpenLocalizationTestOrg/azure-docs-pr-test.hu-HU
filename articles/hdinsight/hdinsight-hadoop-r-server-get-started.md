---
title: "az R Server on HDInsight - Azure használatába aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Apache Spark on HDInsight-fürt, amely tartalmazza az R Server, és küldje el az R-parancsfájl hello fürtön."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: f7e418bbac48eee080a4b4cfbb33e246324ea5c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a><span data-ttu-id="cb2c7-103">R Server a HDInsightban – első lépések</span><span class="sxs-lookup"><span data-stu-id="cb2c7-103">Get started using R Server on HDInsight</span></span>

<span data-ttu-id="cb2c7-104">HDInsight egy R Server beállítás toobe integrálva a HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-104">HDInsight includes an R Server option toobe integrated into your HDInsight cluster.</span></span> <span data-ttu-id="cb2c7-105">Ez a beállítás lehetővé teszi a R parancsfájlok toouse Spark és MapReduce toorun elosztott számítások.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-105">This option allows R scripts toouse Spark and MapReduce toorun distributed computations.</span></span> <span data-ttu-id="cb2c7-106">Ebből a dokumentumból megismerheti, hogyan toocreate egyik R Server, a HDInsight-fürtöt, majd azt mutatja be, a Spark használata parancsfájl futtatása egy R elosztott R-számítások.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-106">In this document, you learn how toocreate an R Server on HDInsight cluster and then run an R script that demonstrates using Spark for distributed R computations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="cb2c7-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cb2c7-107">Prerequisites</span></span>

* <span data-ttu-id="cb2c7-108">**Azure-előfizetés**: Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-108">**An Azure subscription**: Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="cb2c7-109">Nyissa meg toohello cikk [beszerzése a Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) további információt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-109">Go toohello article [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) for more information.</span></span>
* <span data-ttu-id="cb2c7-110">**A Secure Shell (SSH) ügyfél**: egy SSH-ügyfél használata tooremotely toohello HDInsight-fürthöz csatlakozzon, és futtassa a parancsokat közvetlenül hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-110">**A Secure Shell (SSH) client**: An SSH client is used tooremotely connect toohello HDInsight cluster and run commands directly on hello cluster.</span></span> <span data-ttu-id="cb2c7-111">További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-111">For more information, see [Use SSH with HDInsight.](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
* <span data-ttu-id="cb2c7-112">**SSH-kulcsok (nem kötelező)**: hello SSH használt fiók tooconnect toohello fürt jelszó vagy nyilvános kulcs használatával biztonságossá teheti.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-112">**SSH keys (optional)**: You can secure hello SSH account used tooconnect toohello cluster using either a password or a public key.</span></span> <span data-ttu-id="cb2c7-113">Jelszó használatával könnyebben, és lehetővé teszi, hogy Ön tooget anélkül, hogy toocreate egy nyilvános/titkos kulcspár elindult.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-113">Using a password is easier, and allows you tooget started without having toocreate a public/private key pair.</span></span> <span data-ttu-id="cb2c7-114">A kulcs használata azonban biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-114">However, using a key is more secure.</span></span>

> [!NOTE]
> <span data-ttu-id="cb2c7-115">Ez a dokumentum hello lépések azt feltételezik, hogy a jelszó használatát.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-115">hello steps in this document assume that you are using a password.</span></span>


## <a name="automated-cluster-creation"></a><span data-ttu-id="cb2c7-116">Fürt automatikus létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb2c7-116">Automated cluster creation</span></span>

<span data-ttu-id="cb2c7-117">Hello létrehozása HDInsight R kiszolgálók Azure Resource Manager sablonok hello SDK és is PowerShell használatával automatizálhatja azokat.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-117">You can automate hello creation of HDInsight R Servers using Azure Resource Manager templates, hello SDK, and also PowerShell.</span></span>

* <span data-ttu-id="cb2c7-118">toocreate egyik R Server, az Azure Resource Manager sablonnal, lásd: [R server a HDInsight-fürt üzembe helyezése](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-118">toocreate an R Server using an Azure Resource Management template, see [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span></span>
* <span data-ttu-id="cb2c7-119">az R Server hello .NET SDK használatával toocreate lásd [Linux-alapú fürtök létrehozása a Hdinsightban hello .NET SDK használatával.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="cb2c7-119">toocreate an R Server using hello .NET SDK, see [create Linux-based clusters in HDInsight using hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>
* <span data-ttu-id="cb2c7-120">R Server toodeploy hello cikk powershellel, tekintse meg a [az R-kiszolgáló létrehozása a HDInsight a PowerShell-lel](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-120">toodeploy R Server using powershell, see hello article on [creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span></span>


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-hello-cluster-using-hello-azure-portal"></a><span data-ttu-id="cb2c7-121">Hello Azure-portál használatával hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb2c7-121">Create hello cluster using hello Azure portal</span></span>

1. <span data-ttu-id="cb2c7-122">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="cb2c7-123">Válassza az **ÚJ** -> **Intelligencia és elemzés**, -> **HDInsight** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-123">Select **NEW** -> **Intelligence + Analytics**, -> **HDInsight**.</span></span>

    ![Új fürt létrehozásának képe](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. <span data-ttu-id="cb2c7-125">A hello **Gyorslétrehozás** tapasztalhat, adjon meg nevet hello fürtnek hello **fürtnév** mező.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-125">In hello **Quick create** experience, enter a name for hello cluster in hello **Cluster Name** field.</span></span> <span data-ttu-id="cb2c7-126">Ha több Azure-előfizetéssel rendelkezik, használja a hello **előfizetés** bejegyzés tooselect hello egy toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-126">If you have multiple Azure subscriptions, use hello **Subscription** entry tooselect hello one you want toouse.</span></span>

    ![Fürt neve és az előfizetés kiválasztása](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. <span data-ttu-id="cb2c7-128">Válassza ki **típusú fürt** tooopen hello **fürtkonfiguráció** panelen.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-128">Select **Cluster type** tooopen hello **Cluster configuration** blade.</span></span> <span data-ttu-id="cb2c7-129">A hello **fürtkonfiguráció** panelen, jelölje be a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-129">On hello **Cluster Configuration** blade, select hello following options:</span></span>

    * <span data-ttu-id="cb2c7-130">**Fürt típusa**: R Server</span><span class="sxs-lookup"><span data-stu-id="cb2c7-130">**Cluster Type**: R Server</span></span>
    * <span data-ttu-id="cb2c7-131">**Verzió**: R Server tooinstall hello fürtön válassza hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-131">**Version**: select hello version of R Server tooinstall on hello cluster.</span></span> <span data-ttu-id="cb2c7-132">jelenleg elérhető hello verzió ***R Server 9.1 (HDI 3.6)***.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-132">hello version currently available is ***R Server 9.1 (HDI 3.6)***.</span></span> <span data-ttu-id="cb2c7-133">Kibocsátási megjegyzések a hello elérhető R Server verziói érhetők el [Itt](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-133">Release notes for hello available versions of R Server are available [here](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span></span>
    * <span data-ttu-id="cb2c7-134">**Az R Serverhez R Studio community edition**: a böngésző alapú IDE hello élcsomópont alapértelmezés szerint telepítve van.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-134">**R Studio community edition for R Server**: this browser-based IDE is installed by default on hello edge node.</span></span> <span data-ttu-id="cb2c7-135">Ha inkább toonot telepítve rendelkezik, majd törölje a jelet hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-135">If you would prefer toonot have it installed, then uncheck hello check box.</span></span> <span data-ttu-id="cb2c7-136">Ha úgy dönt, hogy toohave még telepítve, hello URL-Címének elérése hello Rstudióból Server bejelentkezés megtalálható a portál alkalmazás panel a fürt létrehozása után van.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-136">If you choose toohave it installed, hello URL for accessing hello RStudio Server login is found on a portal application blade for your cluster once it’s been created.</span></span>
    * <span data-ttu-id="cb2c7-137">Hello alapértelmezett értékek hello egyéb beállításokat hagyja, és használja a hello **válasszon** gomb toosave hello fürt típusa.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-137">Leave hello other options at hello default values and use hello **Select** button toosave hello cluster type.</span></span>

        ![A Fürt típusa panel képernyőképe](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. <span data-ttu-id="cb2c7-139">Írja be a **Fürt bejelentkezési felhasználónevét** és a **Fürt bejelentkezési jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-139">Enter a **Cluster login username** and **Cluster login password**.</span></span>

    <span data-ttu-id="cb2c7-140">Adjon meg egy **SSH-felhasználónevet**.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-140">Specify an **SSH Username**.</span></span> <span data-ttu-id="cb2c7-141">SSH van használt tooremotely toohello fürt használatával csatlakozzon egy **Secure Shell (SSH)** ügyfél.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-141">SSH is used tooremotely connect toohello cluster using a **Secure Shell (SSH)** client.</span></span> <span data-ttu-id="cb2c7-142">Ezen a párbeszédpanelen, vagy (a hello konfiguráció lapon hello fürt) hello fürt létrehozása után hello SSH-felhasználó vagy adhat meg.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-142">You can either specify hello SSH user in this dialog or after hello cluster has been created (in hello Configuration tab for hello cluster).</span></span> <span data-ttu-id="cb2c7-143">R Server konfigurált tooexpect egy **SSH-felhasználónév** a "remoteuser".</span><span class="sxs-lookup"><span data-stu-id="cb2c7-143">R Server is configured tooexpect an **SSH username** of “remoteuser”.</span></span>  <span data-ttu-id="cb2c7-144">**Ha más felhasználónevet használ, hello fürt létrehozása után egy további lépést kell végrehajtania.**</span><span class="sxs-lookup"><span data-stu-id="cb2c7-144">**If you use a different username, you must perform an additional step after hello cluster is created.**</span></span>

    <span data-ttu-id="cb2c7-145">Hagyja hello mezőben be van jelölve, a **ugyanazt a jelszót használják a fürt bejelentkezési** toouse **jelszó** típusúként hello hitelesítési kivéve, ha inkább a nyilvános kulcs használatához.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-145">Leave hello box checked for **Use same password as cluster login** toouse **PASSWORD** as hello authentication type unless you prefer use of a public key.</span></span>  <span data-ttu-id="cb2c7-146">Szüksége van egy nyilvános/titkos kulcspár tooaccess R Server hello fürtön keresztül egy távoli ügyfélhez, mint például RTVS, Rstudióból vagy egy másik asztal IDE.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-146">You need a public/private key pair tooaccess R Server on hello cluster via a remote client as, for example, RTVS, RStudio or another desktop IDE.</span></span> <span data-ttu-id="cb2c7-147">Ha hello Rstudióból Server community edition telepíti, meg kell toochoose egy SSH-jelszó.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-147">If you install hello RStudio Server community edition, you need toochoose an SSH password.</span></span>     

    <span data-ttu-id="cb2c7-148">toocreate és használata a nyilvános és titkos kulcsból álló kulcspárt, törölje a jelet **ugyanazt a jelszót használják a fürt bejelentkezési** majd **nyilvános kulcs** , és folytassa az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-148">toocreate and use a public/private key pair, uncheck **Use same password as cluster login** and then select **PUBLIC KEY** and proceed as follows.</span></span> <span data-ttu-id="cb2c7-149">Ezek az utasítások feltételezik, hogy a Cygwin with ssh-keygen vagy ennek megfelelő van telepítve.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-149">These instructions assume that you have Cygwin with ssh-keygen or an equivalent installed.</span></span>

    * <span data-ttu-id="cb2c7-150">Hozzon létre egy nyilvános/titkos kulcspár hello parancssort azon a hordozható számítógép:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-150">Generate a public/private key pair from hello command prompt on your laptop:</span></span>

        <span data-ttu-id="cb2c7-151">ssh-keygen -t rsa -b 2048</span><span class="sxs-lookup"><span data-stu-id="cb2c7-151">ssh-keygen -t rsa -b 2048</span></span>

    * <span data-ttu-id="cb2c7-152">Hajtsa végre a hello Rákérdezés tooname kulcsfájlt, és írja be a nagyobb biztonság egy hozzáférési kódot.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-152">Follow hello prompt tooname a key file and then enter a passphrase for added security.</span></span> <span data-ttu-id="cb2c7-153">A képernyő hasonlóan kell kinéznie hello kép a következő:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-153">Your screen should look something like hello following image:</span></span>

        ![SSH parancssor Windows rendszerben](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * <span data-ttu-id="cb2c7-155">Ezzel a paranccsal létrejön egy titkos kulcs fájlja és a hello név < magánfelhő-key-fájlnév > .pub, például furiosa és furiosa.pub nyilvános kulcsfájlt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-155">This command creates a private key file and a public key file under hello name <private-key-filename>.pub, for example furiosa and furiosa.pub.</span></span>

        ![SSH dir](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * <span data-ttu-id="cb2c7-157">Majd adja meg a hello nyilvános kulcsot tartalmazó fájlt (&#42;. pub) hitelesítő adatok a fürt HDI hozzárendelése, és végül erősítse meg az erőforráscsoport és régiót, és válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-157">Then specify hello public key file (&#42;.pub) when assigning HDI cluster credentials and finally confirm your resource group and region and select **Next**.</span></span>

        ![Hitelesítő adatok panel](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * <span data-ttu-id="cb2c7-159">A hordozható számítógép hello a titkos kulcsfájl engedélyeinek módosítása:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-159">Change permissions on hello private keyfile on your laptop:</span></span>

        <span data-ttu-id="cb2c7-160">chmod 600 <titkos-kulcs-fájlnév></span><span class="sxs-lookup"><span data-stu-id="cb2c7-160">chmod 600 <private-key-filename></span></span>

   * <span data-ttu-id="cb2c7-161">A távoli bejelentkezéshez SSH használja hello titkos kulcsot tartalmazó fájlt:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-161">Use hello private key file with SSH for remote login:</span></span>

        <span data-ttu-id="cb2c7-162">ssh –i <titkos-kulcs-fájlnév> remoteuser@<hostname public ip></span><span class="sxs-lookup"><span data-stu-id="cb2c7-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span></span>

      <span data-ttu-id="cb2c7-163">Másik lehetőségként rész hello definíciófrissítések a Hadoop Spark a számítási környezet az R Serverhez hello ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-163">Or, as part hello definition of your Hadoop Spark compute context for R Server on hello client.</span></span> <span data-ttu-id="cb2c7-164">Lásd: hello **Microsoft R Server használata és a Hadoop ügyfélként** az alszakasz [számítási környezet létrehozása a Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-164">See hello **Using Microsoft R Server as a Hadoop Client** subsection in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span></span>

6. <span data-ttu-id="cb2c7-165">hello Gyorslétrehozás közeledik, toohello **tárolási** panel tooselect hello tárfiók elsődleges helye hello hello használt beállítások toobe HDFS fájlrendszer hello fürt által használt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-165">hello quick create transitions you toohello **Storage** blade tooselect hello storage account settings toobe used for hello primary location of hello HDFS file system used by hello cluster.</span></span> <span data-ttu-id="cb2c7-166">Válasszon új vagy meglévő Azure Storage-fiókot vagy meglévő Data Lake tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-166">Select either a new or existing Azure Storage account or an existing Data Lake Storage account.</span></span>

    - <span data-ttu-id="cb2c7-167">Ha egy Azure Storage-fiók lehetőséget választja, meglévő tárfiókot kiválasztásával van-e jelölve **válasszon egy tárfiókot** és jelölje ki a megfelelő fiók hello.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-167">If you select an Azure Storage account, an existing storage account is selected by choosing **Select a storage account** and then selecting hello relevant account.</span></span> <span data-ttu-id="cb2c7-168">Hozzon létre egy új fiókot hello segítségével **hozzon létre új** hello hivatkozásra **válasszon egy tárfiókot** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-168">Create a new account using hello **Create New** link in hello **Select a storage account** section.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cb2c7-169">Ha **új** hello új tárfiók nevet kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-169">If you select **New** you must enter a name for hello new storage account.</span></span> <span data-ttu-id="cb2c7-170">Egy zöld pipa jelenik meg, ha hello neve el van fogadva.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-170">A green check appears if hello name is accepted.</span></span>

      <span data-ttu-id="cb2c7-171">Hello **alapértelmezett tároló** alapértelmezett toohello hello fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-171">hello **Default Container** defaults toohello name of hello cluster.</span></span> <span data-ttu-id="cb2c7-172">Ez az alapértelmezett hagyja hello értékként.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-172">Leave this default as hello value.</span></span>

      <span data-ttu-id="cb2c7-173">Ha egy új tárolási fiók lehetőséget választotta egy kérdés tooselect **hely** van a megadott tooselect melyik régióban toocreate hello storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-173">If a new storage account option was selected a prompt tooselect **Location** is given tooselect which region toocreate hello storage account.</span></span>  

         ![Adatforrás panel](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > <span data-ttu-id="cb2c7-175">Hello alapértelmezett adatforrás hello helyét is állítja be hello HDInsight-fürt hello helyét.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-175">Selecting hello location for hello default data source  also sets hello location of hello HDInsight cluster.</span></span> <span data-ttu-id="cb2c7-176">hello fürt és az alapértelmezett adatforrás kell hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-176">hello cluster and default data source must be in hello same region.</span></span>

    - <span data-ttu-id="cb2c7-177">Ha azt szeretné, hogy egy meglévő Data Lake Store toouse, majd válassza ki hello ADLS tárolási fiók toouse, és adja hozzá hello fürtöt *Hozzáadás* identitás tooyour fürt tooallow hozzáférés toohello tárolására.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-177">If you want toouse an existing Data Lake Store, then select hello ADLS storage account toouse and add hello cluster *ADD* identity tooyour cluster tooallow access toohello store.</span></span> <span data-ttu-id="cb2c7-178">Ezen folyamatról további információért tekintse át a [HDInsight-fürt létrehozása a Data Lake Store-ral az Azure Portalon](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) szakaszt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-178">For more information on this process, see [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).</span></span>

    <span data-ttu-id="cb2c7-179">Használjon hello **válasszon** toosave hello gombra adatforrás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-179">Use hello **Select** button toosave hello data source configuration.</span></span>


7. <span data-ttu-id="cb2c7-180">Hello **összegzés** panel majd megjeleníti toovalidate a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-180">hello **Summary** blade then displays toovalidate all your settings.</span></span> <span data-ttu-id="cb2c7-181">Itt módosíthatja a **a fürt méretét** toomodify hello száma a fürtben lévő kiszolgálókat, és adja meg a **parancsfájl-műveletek** toorun szeretné.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-181">Here you can change your **Cluster size** toomodify hello number of servers in your cluster and also specify any **Script actions** you want toorun.</span></span> <span data-ttu-id="cb2c7-182">Hacsak nem tudja, hogy kell-e automatikusan nagyobb fürt, hagyja feldolgozó csomópontok száma hello hello alapértelmezett `4`.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-182">Unless you know that you need a larger cluster, leave hello number of worker nodes at hello default of `4`.</span></span> <span data-ttu-id="cb2c7-183">hello becsült költség hello fürt belül hello panelen látható.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-183">hello estimated cost of hello cluster is shown within hello blade.</span></span>

    ![fürt összegzése](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > <span data-ttu-id="cb2c7-185">Ha szükséges, a fürt később hello portálon keresztül átméretezheti (**fürt** -> **beállítások** -> **méretezési fürt**) tooincrease vagy Csökkentse a feldolgozó csomópontok száma hello.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-185">If needed, you can resize your cluster later through hello Portal (**Cluster** -> **Settings** -> **Scale Cluster**) tooincrease or decrease hello number of worker nodes.</span></span>  <span data-ttu-id="cb2c7-186">Az átméretezés akkor lehet hasznos, üresjárati le hello fürt, ha nincsenek használatban, vagy a toomeet hello kapacitásigények a nagyobb feladatok hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-186">This resizing can be useful for idling down hello cluster when not in use, or for adding capacity toomeet hello needs of larger tasks.</span></span>
   >
   >

   <span data-ttu-id="cb2c7-187">Bizonyos tényezőket tookeep szem előtt, a fürt hello adatcsomópontokat és hello élcsomópont osztályozás a következők:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-187">Some factors tookeep in mind when sizing your cluster, hello data nodes, and hello edge node include:</span></span>  

   * <span data-ttu-id="cb2c7-188">a Spark elosztott R Server elemzések hello teljesítményének feldolgozó csomópontok száma arányos toohello esetén hello adatok mérete nagy.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-188">hello performance of distributed R Server analyses on Spark is proportional toohello number of worker nodes when hello data is large.</span></span>  

   * <span data-ttu-id="cb2c7-189">R Server elemzések hello teljesítményének lineáris hello méretű adatok elemzése folyamatban.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-189">hello performance of R Server analyses is linear in hello size of data being analyzed.</span></span> <span data-ttu-id="cb2c7-190">Példa:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-190">For example:</span></span>  

     * <span data-ttu-id="cb2c7-191">Kis toomodest adatok teljesítmény esetén ajánlott, ha egy helyi számítási környezetben hello peremhálózati csomóponton elemzése.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-191">For small toomodest data, performance is best when analyzed in a local compute context on hello edge node.</span></span>  <span data-ttu-id="cb2c7-192">Hello forgatókönyvek, amely alatt a helyi hello és Spark számítási környezetek további információk a legmegfelelőbb, tekintse meg a számítási környezet beállítások R Server a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-192">For more information on hello scenarios under which hello local and Spark compute contexts work best, see  Compute context options for R Server on HDInsight.</span></span><br>
     * <span data-ttu-id="cb2c7-193">Ha toohello élcsomópont-e jelentkezni, és az R-parancsfájl futtatása, majd az összes hello ScaleR a rx-funkciók végrehajtása <strong>helyileg</strong> hello peremhálózati csomóponton.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-193">If you log in toohello edge node and run your R script, then all but hello ScaleR rx-functions are executed <strong>locally</strong> on hello edge node.</span></span> <span data-ttu-id="cb2c7-194">Így a memória hello és hello peremhálózati csomópont magok száma ennek megfelelően kell méretezni.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-194">So hello memory and number of cores of hello edge node should be sized accordingly.</span></span> <span data-ttu-id="cb2c7-195">Ugyanez vonatkozik, ha R Server HDI a távoli számítási környezetet a laptopján hello.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-195">hello same applies if you use R Server on HDI as a remote compute context from your laptop.</span></span>

     ![Csomópontok árképzési szintjei panel](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     <span data-ttu-id="cb2c7-197">Használjon hello **válasszon** gomb toosave hello csomópont árképzési konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-197">Use hello **Select** button toosave hello node pricing configuration.</span></span>

   <span data-ttu-id="cb2c7-198">Található egy **Sablon és paraméterek letöltése** hivatkozás is.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-198">There is also a link for **Download template and parameters**.</span></span> <span data-ttu-id="cb2c7-199">Kattintson a hivatkozás toodisplay parancsfájlokat, amelyek idővel használt tooautomate hello létrehozása a fürt hello kijelölt konfigurációjával.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-199">Click on this link toodisplay scripts that can be used tooautomate hello creation of a cluster with hello selected configuration.</span></span> <span data-ttu-id="cb2c7-200">A parancsfájlokat is elérhetőek hello Azure portál bejegyzés a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-200">These scripts are also available from hello Azure portal entry for your cluster once it has been created.</span></span>

   > [!NOTE]
   > <span data-ttu-id="cb2c7-201">Némi időbe, hello fürt toobe létrehozása általában körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-201">It takes some time for hello cluster toobe created, usually around 20 minutes.</span></span> <span data-ttu-id="cb2c7-202">Hello Kezdőpulton hello csempe használja, vagy hello **értesítések** hello bevitelének maradt a hello lap toocheck hello létrehozási folyamata.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-202">Use hello tile on hello Startboard, or hello **Notifications** entry on hello left of hello page toocheck on hello creation process.</span></span>
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-toorstudio-server"></a><span data-ttu-id="cb2c7-203">Csatlakozás tooRStudio kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cb2c7-203">Connect tooRStudio Server</span></span>

<span data-ttu-id="cb2c7-204">Ha a kiválasztott tooinclude Rstudióból Server community edition telepítésében, majd hello Rstudióból bejelentkezési két különböző módszereket keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-204">If you’ve chosen tooinclude RStudio Server community edition in your installation, then you can access hello RStudio login via two different methods.</span></span>

1. <span data-ttu-id="cb2c7-205">Nyissa meg a következő URL-cím toohello (ahol **CLUSTERNAME** létrehozott hello fürt hello neve):</span><span class="sxs-lookup"><span data-stu-id="cb2c7-205">Go toohello following URL (where **CLUSTERNAME** is hello name of hello cluster you've created):</span></span>

    <span data-ttu-id="cb2c7-206">https://**FÜRTNÉV**.azurehdinsight.net/rstudio/</span><span class="sxs-lookup"><span data-stu-id="cb2c7-206">https://**CLUSTERNAME**.azurehdinsight.net/rstudio/</span></span>

2. <span data-ttu-id="cb2c7-207">Nyissa meg a fürt hello bejegyzés hello Azure-portálon válassza hello **R Server irányítópultok** Gyorshivatkozás, és jelölje be hello **R Studio irányítópult**:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-207">Open hello entry for your cluster in hello Azure portal, select hello **R Server Dashboards** quick link and then selecting hello **R Studio Dashboard**:</span></span>

     ![Hello R studio irányítópulton](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Hello R studio irányítópulton](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > <span data-ttu-id="cb2c7-210">Hello módszert használja, attól függetlenül hello első alkalommal jelentkezik be kell tooauthenticate kétszer.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-210">Regardless of hello method used, hello first time you log in you need tooauthenticate twice.</span></span>  <span data-ttu-id="cb2c7-211">Hello első hitelesítési, adja meg a hello *rendszergazda felhasználói azonosítóját a fürt* és *jelszó*.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-211">At hello first authentication, provide hello *cluster Admin userid* and *password*.</span></span> <span data-ttu-id="cb2c7-212">A parancssorból hello második adja meg a hello *SSH userid* és *jelszó*.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-212">At hello second prompt, provide hello *SSH userid* and *password*.</span></span> <span data-ttu-id="cb2c7-213">Későbbi napló modulok csak hello *SSH-jelszónak* és *userid*.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-213">Subsequent log ins only require hello *SSH password* and *userid*.</span></span>

<a name="connect-to-edge-node"></a>
## <a name="connect-toohello-r-server-edge-node"></a><span data-ttu-id="cb2c7-214">Csatlakozás toohello R Server élcsomópont</span><span class="sxs-lookup"><span data-stu-id="cb2c7-214">Connect toohello R Server edge node</span></span>

<span data-ttu-id="cb2c7-215">Csatlakozás bemutató Server élcsomópont hello HDInsight-fürt SSH hello parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-215">Connect tooR Server edge node of hello HDInsight cluster using SSH with hello command:</span></span>

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> <span data-ttu-id="cb2c7-216">Hello található `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` hello Azure portálon, majd a fürt kiválasztásával címet **összes beállítás** -> **alkalmazások** -> **RServer**.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-216">You can find hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` address in hello Azure portal by selecting your cluster then **All Settings** -> **Apps** -> **RServer**.</span></span> <span data-ttu-id="cb2c7-217">Hello hello élcsomópont SSH végpont adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-217">This displays hello SSH Endpoint information for hello edge node.</span></span>
>
> ![Hello SSH végpont hello peremhálózati csomópont képe](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

<span data-ttu-id="cb2c7-219">Ha a jelszó toosecure az SSH-felhasználói fiókhoz,-e rákérdezéses tooenter azt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-219">If you used a password toosecure your SSH user account, you are prompted tooenter it.</span></span> <span data-ttu-id="cb2c7-220">Ha egy nyilvános kulcshoz, előfordulhat, hogy toouse hello `-i` paraméter toospecify hello kapcsolódó titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-220">If you used a public key, you may have toouse hello `-i` parameter toospecify hello matching private key.</span></span> <span data-ttu-id="cb2c7-221">Példa:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-221">For example:</span></span>

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="cb2c7-222">További információkért lásd: [csatlakozzon az SSH használatával tooHDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-222">For more information, see [Connect tooHDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="cb2c7-223">A csatlakozás után egy kérdés hasonló toohello következő elvégzésével:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-223">Once connected, you arrive at a prompt similar toohello following:</span></span>

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a><span data-ttu-id="cb2c7-224">Több párhuzamos felhasználó engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cb2c7-224">Enable multiple concurrent users</span></span>

<span data-ttu-id="cb2c7-225">További felhasználók melyik hello Rstudióból közösségi verziója fut. hello élcsomópont való hozzáadásával több egyidejű felhasználók engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-225">You can enable multiple concurrent users by adding more users for hello edge node on which hello RStudio community version runs.</span></span>

<span data-ttu-id="cb2c7-226">HDInsight-fürt létrehozásakor két felhasználót kell megadnia, egy HTTP-felhasználót és egy SSH-felhasználót:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-226">When you create an HDInsight cluster, you must provide two users, an HTTP user and an SSH user:</span></span>

![1. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- <span data-ttu-id="cb2c7-228">**A fürt bejelentkezési felhasználónevének**: egy HTTP-felhasználó a hitelesítéshez, amely használt tooprotect hello HDInsight hello HDInsight átjárón keresztül fürtök létrehozott.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-228">**Cluster login username**: an HTTP user for authentication through hello HDInsight gateway that is used tooprotect hello HDInsight clusters you created.</span></span> <span data-ttu-id="cb2c7-229">A HTTP-felhasználóhoz használt tooaccess hello Ambari felhasználói felület, a YARN felhasználói felületen, valamint egyéb felhasználói felületi összetevőket.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-229">This HTTP user is used tooaccess hello Ambari UI, YARN UI, as well as other UI components.</span></span>
- <span data-ttu-id="cb2c7-230">**Secure Shell (SSH) felhasználónév**: az SSH felhasználói tooaccess hello fürt biztonságos rendszerhéj keresztül.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-230">**Secure Shell (SSH) username**: an SSH user tooaccess hello cluster through secure shell.</span></span> <span data-ttu-id="cb2c7-231">Ez a felhasználó a felhasználó az összes hello átjárócsomópontokkal, a feldolgozó csomópontok és a peremhálózati csomópontok Linux rendszer hello.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-231">This user is a user in hello Linux system for all hello head nodes, worker nodes, and edge nodes.</span></span> <span data-ttu-id="cb2c7-232">Így biztonságos rendszerhéj tooaccess hello csomópontok bármelyikét távoli fürtben.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-232">So you can use secure shell tooaccess any of hello nodes in a remote cluster.</span></span>

<span data-ttu-id="cb2c7-233">hello Microsoft R Server on HDInsight-fürt típusa használt hello R Studio Server közösségi verzió bejelentkezési mechanizmusaként csak Linux felhasználónevet és jelszót fogad el.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-233">hello R Studio Server Community version used in hello Microsoft R Server on HDInsight type cluster accepts only Linux username and password as a login mechanism.</span></span> <span data-ttu-id="cb2c7-234">Nem támogatja a jogkivonatok átadását.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-234">It does not support passing tokens.</span></span> <span data-ttu-id="cb2c7-235">Ha létrehozott egy új fürtre, és toouse R Studio tooaccess Igen, van szüksége a toolog kétszer.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-235">So if you have created a new cluster and want toouse R Studio tooaccess it, you need toolog in twice.</span></span>

- <span data-ttu-id="cb2c7-236">Először jelentkezzen be hello HDInsight átjárón keresztül hello HTTP felhasználói hitelesítő adatok használatával:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-236">First log in using hello HTTP user credentials through hello HDInsight Gateway:</span></span> 

    ![2a párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- <span data-ttu-id="cb2c7-238">Majd használjon hello SSH felhasználói hitelesítő adatok toolog tooRStudio:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-238">Then use hello SSH user credentials toolog in tooRStudio:</span></span>
  
    ![2b párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

<span data-ttu-id="cb2c7-240">Jelenleg HDInsight-fürt üzembe helyezésekor csak egy SSH felhasználói fiókot lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-240">Currently, only one SSH user account can be created when provisioning an HDInsight cluster.</span></span> <span data-ttu-id="cb2c7-241">Tooenable több felhasználók tooaccess Microsoft az R Server on HDInsight clusters, így további felhasználóknak toocreate hello Linux rendszer szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-241">So tooenable multiple users tooaccess Microsoft R Server on HDInsight clusters, we need toocreate additional users in hello Linux system.</span></span>

<span data-ttu-id="cb2c7-242">Rstudióból Server közösségi hello fürt élcsomóponthoz fut, mert több lépésből áll itt:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-242">Because RStudio Server Community is running on hello cluster’s edge node, there are several steps here:</span></span>

1. <span data-ttu-id="cb2c7-243">Hello létre SSH felhasználói toolog toohello élcsomópont használata</span><span class="sxs-lookup"><span data-stu-id="cb2c7-243">Use hello created SSH user toolog in toohello edge node</span></span>
2. <span data-ttu-id="cb2c7-244">További Linux-felhasználók hozzáadása az élcsomópontban</span><span class="sxs-lookup"><span data-stu-id="cb2c7-244">Add more Linux users in edge node</span></span>
3. <span data-ttu-id="cb2c7-245">Rstudióból közösségi verzió használata hello felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb2c7-245">Use RStudio Community version with hello user created</span></span>

### <a name="step-1-use-hello-created-ssh-user-toolog-in-toohello-edge-node"></a><span data-ttu-id="cb2c7-246">1. lépés: Hello létre SSH felhasználói toolog használata toohello élcsomópont</span><span class="sxs-lookup"><span data-stu-id="cb2c7-246">Step 1: Use hello created SSH user toolog in toohello edge node</span></span>

<span data-ttu-id="cb2c7-247">Töltse le a bármely SSH eszközt (például a Putty), és hello meglévő SSH felhasználói toolog a használja.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-247">Download any SSH tool (such as Putty) and use hello existing SSH user toolog in.</span></span> <span data-ttu-id="cb2c7-248">Kövesse az utasításokat hello [csatlakozzon az SSH használatával tooHDInsight (Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-248">Then follow hello instructions provided in [Connect tooHDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello edge node.</span></span> <span data-ttu-id="cb2c7-249">hello peremhálózati csomópont cím R Server on HDInsight-fürt: *fürtnév-kell adnia végrehajtási adatokat-ssh.azurehdinsight.net*</span><span class="sxs-lookup"><span data-stu-id="cb2c7-249">hello edge node address for R Server on HDInsight cluster is: *clustername-ed-ssh.azurehdinsight.net*</span></span>


### <a name="step-2-add-more-linux-users-in-edge-node"></a><span data-ttu-id="cb2c7-250">2. lépés: További Linux-felhasználók hozzáadása az élcsomópontban</span><span class="sxs-lookup"><span data-stu-id="cb2c7-250">Step 2: Add more Linux users in edge node</span></span>

<span data-ttu-id="cb2c7-251">egy felhasználó toohello peremhálózati csomópont tooadd hello parancsokat hajthat végre:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-251">tooadd a user toohello edge node, execute hello commands:</span></span>

    sudo useradd yournewusername -m
    sudo passwd yourusername

<span data-ttu-id="cb2c7-252">A következő visszaküldött elemek hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-252">You should see hello following items returned:</span></span> 

![3. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

<span data-ttu-id="cb2c7-254">Ha a rendszer kéri "aktuális Kerberos jelszó:", csak Entert kell **Enter** tooignore azt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-254">When prompted for “Current Kerberos password:”, just press **Enter** tooignore it.</span></span> <span data-ttu-id="cb2c7-255">Hello `-m` beállítást `useradd` parancs azt jelzi, hogy hello rendszer létrehoz egy mappát hello felhasználó esetében Rstudióból közösségi verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-255">hello `-m` option in `useradd` command indicates that hello system will create a home folder for hello user, which is required for RStudio Community version.</span></span>


### <a name="step-3-use-rstudio-community-version-with-hello-user-created"></a><span data-ttu-id="cb2c7-256">3. lépés: Használja Rstudióból közösségi verzió hello felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb2c7-256">Step 3: Use RStudio Community version with hello user created</span></span>

<span data-ttu-id="cb2c7-257">Használjon hello létrehozott felhasználói toolog tooRStudio:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-257">Use hello user created toolog in tooRStudio:</span></span>

![4. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

<span data-ttu-id="cb2c7-259">Figyelje meg, hogy Rstudióból azt jelzi, hogy hello új felhasználó használ (például itt *sshuser6*) toolog hello fürt:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-259">Notice that RStudio indicates that you are using hello new user (here, for example, *sshuser6*) toolog in hello cluster:</span></span> 

![5. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

<span data-ttu-id="cb2c7-261">Hello eredeti hitelesítő adataival is bejelentkezhet (alapértelmezés szerint van *sshuser*) egyszerre egy másik böngészőben ablakból.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-261">You can also log in using hello original credentials (by default, it is *sshuser*) concurrently from another browser window.</span></span>

<span data-ttu-id="cb2c7-262">Feladat küldéséhez használhatja a scaleR-függvényeket.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-262">You can submit a job using scaleR functions.</span></span> <span data-ttu-id="cb2c7-263">Itt látható egy példa hello használt parancsok toorun egy feladatot:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-263">Here is an example of hello commands used toorun a job:</span></span>

    # Set hello HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data toohello tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot tooload hello data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create hello directory.
    rxHadoopMakeDir(inputDir)

    # Copy hello data from source tooinput.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define hello HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for hello airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all hello column names.
    varNames <- names(airlineColInfo)

    # Define hello text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define hello text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify hello formula toouse.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define hello Spark compute context.
    mySparkCluster <- RxSpark()

    # Set hello compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


<span data-ttu-id="cb2c7-264">Figyelje meg, hogy vannak-e hello feladat a különböző felhasználói neve a YARN felhasználói felületen:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-264">Notice that hello jobs submitted are under different user names in YARN UI:</span></span>

![6. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

<span data-ttu-id="cb2c7-266">Megjegyzés is, hogy hello újonnan hozzáadott felhasználók nem rendelkeznek gyökérszintű jogosultsággal Linux rendszeren, de azok rendelkezik hello azonos tooall hello fájljainak hello HDFS- és a WASB távtároló eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-266">Note also that hello newly added users do not have root privileges in Linux system, but they do have hello same access tooall hello files in hello remote HDFS and WASB storage.</span></span>


<a name="use-r-console"></a>
## <a name="use-hello-r-console"></a><span data-ttu-id="cb2c7-267">Hello R-konzollal</span><span class="sxs-lookup"><span data-stu-id="cb2c7-267">Use hello R console</span></span>

1. <span data-ttu-id="cb2c7-268">Hello SSH-munkamenetet használja a következő parancs toostart hello R konzol hello:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-268">From hello SSH session, use hello following command toostart hello R console:</span></span>  

        R

2. <span data-ttu-id="cb2c7-269">Kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-269">You should see output similar toohello following:</span></span>
    
    <span data-ttu-id="cb2c7-270">R verzió 3.2.2 (2015-08-14)--"Tűz biztonsági" Copyright (C) 2015 R Foundation hello statisztikai számítások Platform: x86_64-pc-linux-gnu (64 bites)</span><span class="sxs-lookup"><span data-stu-id="cb2c7-270">R version 3.2.2 (2015-08-14) -- "Fire Safety"  Copyright (C) 2015 hello R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)</span></span>

    <span data-ttu-id="cb2c7-271">Az R egy ingyenes szoftver, és EGYÁLTALÁN SEMMILYEN JÓTÁLLÁS SEM vonatkozik rá.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span></span>
    <span data-ttu-id="cb2c7-272">Üdvözli a tooredistribute áll ez bizonyos körülmények között.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-272">You are welcome tooredistribute it under certain conditions.</span></span>
    <span data-ttu-id="cb2c7-273">A terjesztésre vonatkozó részletekért írja be a 'license()' vagy a 'licence()' parancsot.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-273">Type 'license()' or 'licence()' for distribution details.</span></span>

    <span data-ttu-id="cb2c7-274">Támogatja a természetes nyelveket, de angol területi beállítással fut</span><span class="sxs-lookup"><span data-stu-id="cb2c7-274">Natural language support but running in an English locale</span></span>

    <span data-ttu-id="cb2c7-275">Az R egy együttműködésen alapuló projekt, sok hozzájárulóval.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-275">R is a collaborative project with many contributors.</span></span>
    <span data-ttu-id="cb2c7-276">Írja be a "contributors()" További információk és "citation()" hogyan toocite R vagy R csomagok kiadványokban a.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-276">Type 'contributors()' for more information and 'citation()' on how toocite R or R packages in publications.</span></span>

    <span data-ttu-id="cb2c7-277">Írja be a "demo()" néhány bemutatók, az online súgó "help()" vagy "help.start()" egy HTML-böngésző felület toohelp.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-277">Type 'demo()' for some demos, 'help()' for on-line help, or 'help.start()' for an HTML browser interface toohelp.</span></span>
    <span data-ttu-id="cb2c7-278">Írja be a "q()" tooquit R.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-278">Type 'q()' tooquit R.</span></span>

    <span data-ttu-id="cb2c7-279">Microsoft R Server 8.0-s verzió: az R Microsoft-csomagok továbbfejlesztett terjesztési verziója Copyright (C) 2016 Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="cb2c7-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span></span>

    <span data-ttu-id="cb2c7-280">A kiadási megjegyzések megtekintéséhez írja be a 'readme()' parancsot.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-280">Type 'readme()' for release notes.</span></span>
    >

3. <span data-ttu-id="cb2c7-281">A hello `>` parancssorba R kódot is megadhat.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-281">From hello `>` prompt, you can enter R code.</span></span> <span data-ttu-id="cb2c7-282">R server tartalmaz, amelyek lehetővé teszik tooeasily csomagok Hadoop kommunikál, és futtassa az elosztott számítások.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-282">R server includes packages that allow you tooeasily interact with Hadoop and run distributed computations.</span></span> <span data-ttu-id="cb2c7-283">Például használja a következő parancs tooview hello legfelső szintű hello alapértelmezett fájlrendszer hello HDInsight-fürthöz hello:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-283">For example, use hello following command tooview hello root of hello default file system for hello HDInsight cluster:</span></span>

    <span data-ttu-id="cb2c7-284">rxHadoopListFiles("/")</span><span class="sxs-lookup"><span data-stu-id="cb2c7-284">rxHadoopListFiles("/")</span></span>

4. <span data-ttu-id="cb2c7-285">Hello WASB stílus címzési is használható.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-285">You can also use hello WASB style addressing.</span></span>

    <span data-ttu-id="cb2c7-286">rxHadoopListFiles("wasb:///")</span><span class="sxs-lookup"><span data-stu-id="cb2c7-286">rxHadoopListFiles("wasb:///")</span></span>


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a><span data-ttu-id="cb2c7-287">Az R Server használata HDI-n a Microsoft R Server vagy a Microsoft R ügyfél egy távoli példányáról</span><span class="sxs-lookup"><span data-stu-id="cb2c7-287">Using R Server on HDI from a remote instance of Microsoft R Server or Microsoft R Client</span></span>

<span data-ttu-id="cb2c7-288">Már lehetséges tooset hozzáférés toohello HDI Hadoop Spark számítási környezet Microsoft R Server vagy a Microsoft R ügyfél egy asztali vagy hordozható futó távoli példányának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-288">It is possible tooset up access toohello HDI Hadoop Spark compute context from a remote instance of Microsoft R Server or Microsoft R Client running on a desktop or laptop.</span></span> <span data-ttu-id="cb2c7-289">Lásd: **Microsoft R Server használata és a Hadoop ügyfélként** alszakasz a hello [a számítási környezet létrehozásakor a Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-289">See **Using Microsoft R Server as a Hadoop Client** subsection in hello [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span></span> <span data-ttu-id="cb2c7-290">toodo toospecify hello hello RxSpark számítási környezet meghatározása a hordozható számítógép esetén a következő beállításokat kell tehát: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, és sshProfileScript.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-290">toodo so, you need toospecify hello following options when defining hello RxSpark compute context on your laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, and sshProfileScript.</span></span> <span data-ttu-id="cb2c7-291">Példa:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-291">For example:</span></span>


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a><span data-ttu-id="cb2c7-292">Számítási környezet használata</span><span class="sxs-lookup"><span data-stu-id="cb2c7-292">Use a compute context</span></span>

<span data-ttu-id="cb2c7-293">A számítási környezet lehetővé teszi a toocontrol hogy számítási helyben végzett hello élcsomópont vagy le van elosztva hello hello HDInsight-fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-293">A compute context allows you toocontrol whether computation is performed locally on hello edge node or distributed across hello nodes in hello HDInsight cluster.</span></span>

1. <span data-ttu-id="cb2c7-294">Rstudióból kiszolgálóról vagy hello R konzol (az SSH-munkamenetet) használja a következő kód tooload példaadatokat a hello alapértelmezett tároló hdinsight hello:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-294">From RStudio Server or hello R console (in an SSH session), use hello following code tooload example data into hello default storage for HDInsight:</span></span>

        # Set hello HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data toohello tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot tooload hello data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make hello directory
        rxHadoopMakeDir(inputDir)

        # Copy hello data from source tooinput
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. <span data-ttu-id="cb2c7-295">A következő hozzuk létre bizonyos adatok információ, és határozza meg két adatforrások, így hello adatokkal dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-295">Next, let's create some data info and define two data sources so that we can work with hello data.</span></span>

        # Define hello HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for hello airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all hello column names
        varNames <- names(airlineColInfo)

        # Define hello text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define hello text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula toouse
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. <span data-ttu-id="cb2c7-296">Most futtassa logisztikai regresszió hello adatok hello helyi számítási környezetet használ.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-296">Let's run a logistic regression over hello data using hello local compute context.</span></span>

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    <span data-ttu-id="cb2c7-297">A következő sorokat hasonló toohello végződő kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-297">You should see output that ends with lines similar toohello following:</span></span>

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. <span data-ttu-id="cb2c7-298">A következő Futtatás most hello azonos logisztikai regresszió hello Spark-környezetet használ.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-298">Next, let's run hello same logistic regression using hello Spark context.</span></span> <span data-ttu-id="cb2c7-299">hello Spark környezetben keresztül hello feldolgozó csomópontjaihoz hello HDInsight fürt feldolgozása hello terjeszti.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-299">hello Spark context distributes hello processing over all hello worker nodes in hello HDInsight cluster.</span></span>

        # Define hello Spark compute context
        mySparkCluster <- RxSpark()

        # Set hello compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > <span data-ttu-id="cb2c7-300">Használhatja a MapReduce toodistribute számítási fürt csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-300">You can also use MapReduce toodistribute computation across cluster nodes.</span></span> <span data-ttu-id="cb2c7-301">A számítási környezetről további információért lásd: [Számítási környezeti beállítások a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-compute-contexts.md).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-301">For more information on compute context, see [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span></span>


## <a name="distribute-r-code-toomultiple-nodes"></a><span data-ttu-id="cb2c7-302">R-kód toomultiple csomópontok terjesztése</span><span class="sxs-lookup"><span data-stu-id="cb2c7-302">Distribute R code toomultiple nodes</span></span>

<span data-ttu-id="cb2c7-303">R Server, egyszerűen meglévő R-kód igénybe vehet, és futtassa hello fürt több csomópontja között használatával `rxExec`.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-303">With R Server, you can easily take existing R code and run it across multiple nodes in hello cluster by using `rxExec`.</span></span> <span data-ttu-id="cb2c7-304">Ez a függvény akkor hasznos, amikor paraméteres frissítést vagy szimulációkat végez.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-304">This function is useful when doing a parameter sweep or simulations.</span></span> <span data-ttu-id="cb2c7-305">hello következő kódja: példa bemutatja, hogyan toouse `rxExec`:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-305">hello following code is an example of how toouse `rxExec`:</span></span>

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

<span data-ttu-id="cb2c7-306">Ha hello Spark vagy MapReduce környezet továbbra is használ, ez a parancs értékét adja vissza hello csomópontnév hello munkavégző csomópontokhoz hello kód `(Sys.info()["nodename"])` a futtatható.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-306">If you are still using hello Spark or MapReduce context, this  command returns hello nodename value for hello worker nodes that hello code `(Sys.info()["nodename"])` is run on.</span></span> <span data-ttu-id="cb2c7-307">Például egy négy csomópontot tartalmazó fürtben, a várt tooreceive kimeneti hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-307">For example, on a four node cluster, you expect tooreceive output similar toohello following:</span></span>

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="accessing-data-in-hive-and-parquet"></a><span data-ttu-id="cb2c7-308">Adatok elérése a Hive és a Parquet eszközökben</span><span class="sxs-lookup"><span data-stu-id="cb2c7-308">Accessing Data in Hive and Parquet</span></span>

<span data-ttu-id="cb2c7-309">R Server 9.1 egy szolgáltatása lehetővé teszi a közvetlen hozzáférést toodata a Hive és Parquet használatra ScaleR funkciók hello Spark számítási környezetben.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-309">A feature available in R Server 9.1 allows direct access toodata in Hive and Parquet for use by ScaleR functions in hello Spark compute context.</span></span> <span data-ttu-id="cb2c7-310">Ezek a képességek új ScaleR adatok forrás funkciók RxHiveData és RxParquetData használata Spark SQL tooload adatok közvetlenül a egy Spark DataFrame ScaleR analízis működő keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-310">These capabilities are available through new ScaleR data source functions called RxHiveData and RxParquetData that work through use of Spark SQL tooload data directly into a Spark DataFrame for analysis by ScaleR.</span></span>  

<span data-ttu-id="cb2c7-311">a következő kód hello hello új funkciók használatát a néhány példakódot tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-311">hello following code provides some sample code on use of hello new functions:</span></span>

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close hello Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


<span data-ttu-id="cb2c7-312">További információ ezen új funkciók használatát a hello online súgójában R Server hello használata `?RxHivedata` és `?RxParquetData` parancsok.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-312">For additional info on use of these new functions see hello online help in R Server through use of hello `?RxHivedata` and `?RxParquetData` commands.</span></span>  


## <a name="install-additional-r-packages-on-hello-edge-node"></a><span data-ttu-id="cb2c7-313">Hello élcsomópont további R csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="cb2c7-313">Install additional R packages on hello edge node</span></span>

<span data-ttu-id="cb2c7-314">Ha szeretne további R csomagokat tooinstall hello peremhálózati csomóponton, `install.packages()` közvetlenül belül hello R konzol amikor csatlakoztatott toohello él csomópont SSH-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-314">If you would like tooinstall additional R packages on hello edge node, you can use `install.packages()` directly from within hello R console when connected toohello edge node through SSH.</span></span> <span data-ttu-id="cb2c7-315">Ha tooinstall R csomagokat a hello munkavégző csomópontokhoz hello fürt, a parancsfájl műveletet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-315">However, if you need tooinstall R packages on hello worker nodes of hello cluster, you must use a Script Action.</span></span>

<span data-ttu-id="cb2c7-316">A Parancsfájlműveletek olyan Bash parancsfájlok, amelyek használt toomake konfigurációs módosítások toohello HDInsight fürt vagy tooinstall további szoftverek, például további R csomagokat.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-316">Script Actions are Bash scripts that are used toomake configuration changes toohello HDInsight cluster or tooinstall additional software, such as additional R packages.</span></span> <span data-ttu-id="cb2c7-317">tooinstall egy parancsfájlművelet használatával további csomagokat a lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-317">tooinstall additional packages using a Script Action, use hello following steps:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb2c7-318">A Parancsfájlműveletek tooinstall további R csomagok használata csak akkor használható hello fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-318">Using Script Actions tooinstall additional R packages can only be used after hello cluster has been created.</span></span> <span data-ttu-id="cb2c7-319">Ne használja ezt az eljárást fürt létrehozása során, hello parancsfájl támaszkodik R Server alatt teljesen telepítve és konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-319">Do not use this procedure during cluster creation, as hello script relies on R Server being completely installed and configured.</span></span>
>
>

1. <span data-ttu-id="cb2c7-320">A hello [Azure-portálon](https://portal.azure.com), válassza ki az R Server on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-320">From hello [Azure portal](https://portal.azure.com), select your R Server on HDInsight cluster.</span></span>

2. <span data-ttu-id="cb2c7-321">A hello **beállítások** panelen válassza **Parancsfájlműveletek** , majd **nyújt új** toosubmit egy új parancsfájlművelet.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-321">From hello **Settings** blade, select **Script Actions** and then **Submit New** toosubmit a new Script Action.</span></span>

   ![A Szkriptműveletek panel képe](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. <span data-ttu-id="cb2c7-323">A hello **parancsfájlművelet** panelen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-323">From hello **Submit script action** blade, provide hello following information:</span></span>

   * <span data-ttu-id="cb2c7-324">**Név**: egy rövid nevet tooidentify ezt a parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="cb2c7-324">**Name**: A friendly name tooidentify this script</span></span>

   * <span data-ttu-id="cb2c7-325">**Bash-szkript URI azonosítója**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span><span class="sxs-lookup"><span data-stu-id="cb2c7-325">**Bash script URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span></span>

   * <span data-ttu-id="cb2c7-326">**Fej**: Ez az elem **ne legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="cb2c7-326">**Head**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="cb2c7-327">**Feldolgozó**: Ez az elem **legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="cb2c7-327">**Worker**: This item should be **checked**</span></span>

   * <span data-ttu-id="cb2c7-328">**Szegélycsomópontok**: Ez az elem **ne legyen bejelölve**.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-328">**Edge nodes**: This item should be **unchecked**.</span></span>

   * <span data-ttu-id="cb2c7-329">**Zookeeper**: Ez az elem **ne legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="cb2c7-329">**Zookeeper**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="cb2c7-330">**Paraméterek**: hello R csomagok toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-330">**Parameters**: hello R packages toobe installed.</span></span> <span data-ttu-id="cb2c7-331">Például: `bitops stringr arules`</span><span class="sxs-lookup"><span data-stu-id="cb2c7-331">For example, `bitops stringr arules`</span></span>

   * <span data-ttu-id="cb2c7-332">**Ha megőrzi a...**: Ez az elem **legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="cb2c7-332">**Persist this script...**: This item should be **Checked**</span></span>  

   > [!NOTE]
   > 1. <span data-ttu-id="cb2c7-333">Alapértelmezés szerint az összes R csomagot hello Microsoft MRAN tárház hello R Server telepített verziója megfelel egy pillanatképből telepítve.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-333">By default, all R packages are installed from a snapshot of hello Microsoft MRAN repository consistent with hello version of R Server that has been installed.</span></span> <span data-ttu-id="cb2c7-334">Ha azt szeretné, hogy a csomagok tooinstall újabb verzióját, majd nincs inkompatibilitás néhány kockázatát.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-334">If you want tooinstall newer versions of packages, then there is some risk of incompatibility.</span></span> <span data-ttu-id="cb2c7-335">Azonban ez a telepítési típus lehetséges áll megadásával `useCRAN` , hello hello csomag első elemének listájában, például `useCRAN bitops, stringr, arules`.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-335">However this kind of install is possible by specifying `useCRAN` as hello first element of hello package list, for example `useCRAN bitops, stringr, arules`.</span></span>  
   > 2. <span data-ttu-id="cb2c7-336">Néhány R csomag további Linux rendszerű könyvtárakat igényel.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-336">Some R packages require additional Linux system libraries.</span></span> <span data-ttu-id="cb2c7-337">Kényelmi célokat szolgál azt előre telepített hello függőségek hello top 100 legnépszerűbb R csomag szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-337">For convenience, we have pre-installed hello dependencies needed by hello top 100 most popular R packages.</span></span> <span data-ttu-id="cb2c7-338">Azonban ha hello R csomag(ok) telepítése van szükség a szalagtárak túl ezek majd kell itt használt hello alap parancsfájl letöltése és könyvtár lépéseit tooinstall hello hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-338">However, if hello R package(s) you install require libraries beyond these then you must download hello base script used here and add steps tooinstall hello system libraries.</span></span> <span data-ttu-id="cb2c7-339">Meg kell majd feltöltés hello módosító parancsfájlt tooa nyilvános blob-tároló az Azure storage és módosított hello parancsfájl tooinstall hello csomagok használata.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-339">You must then upload hello modified script tooa public blob container in Azure storage and use hello modified script tooinstall hello packages.</span></span>
   >    <span data-ttu-id="cb2c7-340">A szkriptműveletek fejlesztésével kapcsolatos további információért lásd: [Szkriptművelet fejlesztése](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-340">For more information on developing Script Actions, see [Script Action development](hdinsight-hadoop-script-actions-linux.md).</span></span>  
   >
   >

   ![Szkriptműveletek hozzáadása](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. <span data-ttu-id="cb2c7-342">Válassza ki **létrehozása** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-342">Select **Create** toorun hello script.</span></span> <span data-ttu-id="cb2c7-343">Hello parancsfájl befejeztét követően az összes munkavégző csomóponton hello R csomagok érhetők el.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-343">Once hello script completes, hello R packages are available on all worker nodes.</span></span>


## <a name="using-microsoft-r-server-operationalization"></a><span data-ttu-id="cb2c7-344">A Microsoft R Server operacionalizálás használata</span><span class="sxs-lookup"><span data-stu-id="cb2c7-344">Using Microsoft R Server Operationalization</span></span>

<span data-ttu-id="cb2c7-345">Amikor befejeződött az adatok modellezési, üzembe hello modell toomake előrejelzéseket.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-345">When your data modeling is complete, you can operationalize hello model toomake predictions.</span></span> <span data-ttu-id="cb2c7-346">a Microsoft R Server operationalization tooconfigure hello a következő lépéseket hajtsa végre:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-346">tooconfigure for Microsoft R Server operationalization, perform hello following steps:</span></span>

<span data-ttu-id="cb2c7-347">Első, ssh hello peremhálózati csomópontjába.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-347">First, ssh into hello Edge node.</span></span> <span data-ttu-id="cb2c7-348">Például:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-348">For example,</span></span> 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="cb2c7-349">Használata után ssh, módosítsa a könyvtárat hello megfelelő verzióját és a sudo hello dotnet dll:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-349">After using ssh, change directory for hello relevant version and sudo hello dotnet dll:</span></span> 

- <span data-ttu-id="cb2c7-350">Microsoft R Server 9.1 esetén:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-350">For Microsoft R Server 9.1:</span></span>

    <span data-ttu-id="cb2c7-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="cb2c7-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span></span>

- <span data-ttu-id="cb2c7-352">Microsoft R Server 9.0 esetén:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-352">For Microsoft R Server 9.0:</span></span>

    <span data-ttu-id="cb2c7-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="cb2c7-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span></span>

<span data-ttu-id="cb2c7-354">egy egy-mezőben konfigurációjával tooconfigure Microsoft R Server operationalization hello a következő:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-354">tooconfigure Microsoft R Server operationalization with a One-box configuration do hello following:</span></span>

1. <span data-ttu-id="cb2c7-355">Válassza a „Configure R Server for Operationalization” (Az R Server konfigurálása a Működőképessé tétel művelethez) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="cb2c7-355">Select “Configure R Server for Operationalization”</span></span>
2. <span data-ttu-id="cb2c7-356">Válassza az „A.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-356">Select “A.</span></span> <span data-ttu-id="cb2c7-357">One-box (web + compute nodes)” (Beépített (web + számítási csomópontok)) elemet</span><span class="sxs-lookup"><span data-stu-id="cb2c7-357">One-box (web + compute nodes)”</span></span>
3. <span data-ttu-id="cb2c7-358">Írja be a jelszót hello **admin** felhasználó</span><span class="sxs-lookup"><span data-stu-id="cb2c7-358">Enter a password for hello **admin** user</span></span>

![one box op](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

<span data-ttu-id="cb2c7-360">Választható lépésként diagnosztikai ellenőrzéseket végezhet az alább látható diagnosztikai tesztelés futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-360">As an optional step you can perform Diagnostic checks by running a diagnostic test as follows:</span></span>

1. <span data-ttu-id="cb2c7-361">Válassza a „6.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-361">Select “6.</span></span> <span data-ttu-id="cb2c7-362">Run diagnostic tests” (Diagnosztikai tesztek futtatása) elemet</span><span class="sxs-lookup"><span data-stu-id="cb2c7-362">Run diagnostic tests”</span></span>
2. <span data-ttu-id="cb2c7-363">Válassza az „A.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-363">Select “A.</span></span> <span data-ttu-id="cb2c7-364">Test configuration” (Tesztkonfiguráció) elemet</span><span class="sxs-lookup"><span data-stu-id="cb2c7-364">Test configuration”</span></span>
3. <span data-ttu-id="cb2c7-365">Írja be a Username = „admin” parancsot, valamint a jelszót az előző konfigurációs lépésből</span><span class="sxs-lookup"><span data-stu-id="cb2c7-365">Enter Username = “admin” and password from previous configuration step</span></span>
4. <span data-ttu-id="cb2c7-366">Erősítse meg az Overall Health = pass parancsot</span><span class="sxs-lookup"><span data-stu-id="cb2c7-366">Confirm Overall Health = pass</span></span>
5. <span data-ttu-id="cb2c7-367">Kilépés hello felügyeleti segédprogram</span><span class="sxs-lookup"><span data-stu-id="cb2c7-367">Exit hello Admin Utility</span></span>
6. <span data-ttu-id="cb2c7-368">Lépjen ki az SSH-ból</span><span class="sxs-lookup"><span data-stu-id="cb2c7-368">Exit SSH</span></span>

![Diagnostic for op](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
><span data-ttu-id="cb2c7-370">**Hosszú késések a webszolgáltatások Sparkban történő felhasználásakor**</span><span class="sxs-lookup"><span data-stu-id="cb2c7-370">**Long delays when consuming web service on Spark**</span></span>
>
><span data-ttu-id="cb2c7-371">Ha nagy késleltetéseket mrsdeploy funkciók Spark számítási környezetben létre tooconsume egy webszolgáltatás-bővítmény közben, esetleg tooadd egyes hiányzó mappák.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-371">If you encounter long delays when trying tooconsume a web service created with mrsdeploy functions in a Spark compute context, you may need tooadd some missing folders.</span></span> <span data-ttu-id="cb2c7-372">Spark alkalmazás hello tartozik tooa felhasználói, úgynevezett "*rserve2*" mrsdeploy függvények használatával webszolgáltatásból meghívni, amikor.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-372">hello Spark application belongs tooa user called '*rserve2*' whenever it is invoked from a web service using mrsdeploy functions.</span></span> <span data-ttu-id="cb2c7-373">a probléma megoldásához toowork:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-373">toowork around this issue:</span></span>

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


<span data-ttu-id="cb2c7-374">Ebben a szakaszban Operationalization hello konfigurációja befejeződött.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-374">At this stage, hello configuration for Operationalization is complete.</span></span> <span data-ttu-id="cb2c7-375">Mostantól a hello "mrsdeploy" csomagot a RClient tooconnect toohello Operationalization peremhálózati csomóponton máris használhatja a szolgáltatásokat, például [távoli végrehajtás](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) és [-webszolgáltatások](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-375">Now you can use hello ‘mrsdeploy’ package on your RClient tooconnect toohello Operationalization on edge node and start using its features like [remote execution](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) and [web-services](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span></span> <span data-ttu-id="cb2c7-376">Attól függően, hogy a fürt beállítása a virtuális hálózaton vagy nem szükség lehet a tooset mentése port előre tunneling keresztül SSH-bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-376">Depending on whether your cluster is set up on a virtual network or not, you may need tooset up port forward tunneling through SSH login.</span></span> <span data-ttu-id="cb2c7-377">hello alábbi szakaszok azt ismertetik, hogyan tooset fel ezt az alagutat.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-377">hello following sections explain how tooset up this tunnel.</span></span>

### <a name="rserver-cluster-on-virtual-network"></a><span data-ttu-id="cb2c7-378">Az RServer fürt virtuális hálózaton van</span><span class="sxs-lookup"><span data-stu-id="cb2c7-378">RServer Cluster on virtual network</span></span>

<span data-ttu-id="cb2c7-379">Ellenőrizze, hogy forgalmat 12800 toohello élcsomópont porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-379">Make sure you allow traffic through port 12800 toohello edge node.</span></span> <span data-ttu-id="cb2c7-380">Ily módon használhatja hello peremhálózati csomópont tooconnect toohello Operationalization szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-380">That way, you can use hello edge node tooconnect toohello Operationalization feature.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


<span data-ttu-id="cb2c7-381">Ha hello `remoteLogin()` nem toohello élcsomópont, csatlakozni, de SSH toohello élcsomópontot is, majd tooverify kell, hogy port 12800 hello szabály tooallow forgalmát megfelelő vagy nem lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-381">If hello `remoteLogin()` cannot connect toohello edge node, but you can SSH toohello edge node, then you need tooverify whether hello rule tooallow traffic on port 12800 has been set properly or not.</span></span> <span data-ttu-id="cb2c7-382">Tooface hello a probléma továbbra is, ha megkerüléséhez azt port előre tunneling SSH-n keresztül beállításával.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-382">If you continue tooface hello issue, you can work around it by setting up port forward tunneling through SSH.</span></span> <span data-ttu-id="cb2c7-383">Útmutatásért lásd a következő szakasz hello.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-383">For instructions, see hello following section.</span></span>

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a><span data-ttu-id="cb2c7-384">Az RServer fürt nem virtuális hálózaton van beállítva</span><span class="sxs-lookup"><span data-stu-id="cb2c7-384">RServer Cluster not set up on virtual network</span></span>

<span data-ttu-id="cb2c7-385">Ha a fürt nem a virtuális hálózaton van beállítva vagy problémás a kapcsolódás a virtuális hálózaton keresztül, akkor használhatja az SSH porttovábbító alagutat:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-385">If your cluster is not set up on vnet or if you are having troubles with connectivity through vnet, you can use SSH port forward tunneling:</span></span>

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="cb2c7-386">A Putty szoftveren is beállítható.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-386">On Putty, you can set it up as well.</span></span>

![putty ssh kapcsolat](./media/hdinsight-hadoop-r-server-get-started/putty.png)

<span data-ttu-id="cb2c7-388">Ha az SSH-munkamenetet aktív, a gép portról 12800 hello forgalmat toohello peremhálózati csomópont port 12800 SSH-kapcsolaton keresztül továbbítja.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-388">Once your SSH session is active, hello traffic from your machine’s port 12800 is forwarded toohello edge node’s port 12800 through SSH session.</span></span> <span data-ttu-id="cb2c7-389">A `127.0.0.1:12800` címet használja a `remoteLogin()` metódusban.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-389">Make sure you use `127.0.0.1:12800` in your `remoteLogin()` method.</span></span> <span data-ttu-id="cb2c7-390">A rendszer a toohello peremhálózati csomópont operationalization port továbbítási keresztül.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-390">This logs in toohello edge node’s operationalization through port forwarding.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-tooscale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a><span data-ttu-id="cb2c7-391">Hogyan tooscale Microsoft R Server Operationalization számítási csomópontok HDInsight munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="cb2c7-391">How tooscale Microsoft R Server Operationalization compute nodes on HDInsight worker nodes</span></span>

### <a name="decommission-hello-worker-nodes"></a><span data-ttu-id="cb2c7-392">Hello munkavégző csomópont leszerelése</span><span class="sxs-lookup"><span data-stu-id="cb2c7-392">Decommission hello worker node(s)</span></span>

<span data-ttu-id="cb2c7-393">A Microsoft R Servert jelenleg nem a Yarnon keresztül kezeli a rendszer.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-393">Microsoft R Server is currently not managed through Yarn.</span></span> <span data-ttu-id="cb2c7-394">Ha hello munkavégző csomópontokhoz nincs leszerelve, hello Yarn erőforrás-kezelő nem működik megfelelően, mivel nem lesz kompatibilis foglalja el hello server hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-394">If hello worker nodes are not decommissioned, hello Yarn Resource Manager will not work as expected because it will not be aware of hello resources being taken up by hello server.</span></span> <span data-ttu-id="cb2c7-395">A sorrend tooavoid ebben az esetben ajánlott leszerelése munkavégző csomópontokhoz hello hello számítási csomópontok a horizontális előtt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-395">In order tooavoid this situation, we recommend decommissioning hello worker nodes before you scale out hello compute nodes.</span></span>

<span data-ttu-id="cb2c7-396">Toodecommissioning munkavégző csomópontokhoz lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-396">Steps toodecommissioning worker nodes:</span></span>

* <span data-ttu-id="cb2c7-397">Jelentkezzen be tooHDI fürt Ambari konzol és a "gazdagép" lapon kattintson a</span><span class="sxs-lookup"><span data-stu-id="cb2c7-397">Log in tooHDI cluster's Ambari console and click on "hosts" tab</span></span>
* <span data-ttu-id="cb2c7-398">Válassza ki a feldolgozó csomópontok (toobe szerelni), kattintson a "Műveletek" > "Kiválasztott gazdagép" > "Gazdagép" > kattintson a "Kapcsolja be a karbantartási mód".</span><span class="sxs-lookup"><span data-stu-id="cb2c7-398">Select worker nodes (toobe decommissioned), Click on "Actions" > "Selected Hosts" > "Hosts" > click on "Turn ON Maintenance Mode".</span></span> <span data-ttu-id="cb2c7-399">Például a következő kép hello jelenleg kijelölt wn3 és wn4 toodecommission.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-399">For example, in hello following image we have selected wn3 and wn4 toodecommission.</span></span>  

   ![feldolgozó csomópont(ok) leszerelése](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* <span data-ttu-id="cb2c7-401">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **DataNodes** (AdatCsomópontok) elemet > kattintson a **Decommission** (Leszerelés) gombra</span><span class="sxs-lookup"><span data-stu-id="cb2c7-401">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Decommission**</span></span>
* <span data-ttu-id="cb2c7-402">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **NodeManagers** (CsomópontKezelők) elemet > kattintson a **Decommission** (Leszerelés) gombra</span><span class="sxs-lookup"><span data-stu-id="cb2c7-402">Select **Actions** > **Selected Hosts** > **NodeManagers** > click **Decommission**</span></span>
* <span data-ttu-id="cb2c7-403">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **DataNodes** (AdatCsomópontok) elemet > kattintson a **Stop** gombra</span><span class="sxs-lookup"><span data-stu-id="cb2c7-403">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Stop**</span></span>
* <span data-ttu-id="cb2c7-404">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **NodeManagers** (CsomópontKezelők) elemet > kattintson a **Stop** gombra</span><span class="sxs-lookup"><span data-stu-id="cb2c7-404">Select **Actions** > **Selected Hosts** > **NodeManagers** > click on **Stop**</span></span>
* <span data-ttu-id="cb2c7-405">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott gazdagépek) > **Hosts** (Gazdagépek) elemet > kattintson a **Stop All Components** (Összes összetevő leállítása) gombra</span><span class="sxs-lookup"><span data-stu-id="cb2c7-405">Select **Actions** > **Selected Hosts** > **Hosts** > click **Stop All Components**</span></span>
* <span data-ttu-id="cb2c7-406">Törölje a hello munkavégző csomópontokhoz, és válassza ki a hello átjárócsomópontokkal</span><span class="sxs-lookup"><span data-stu-id="cb2c7-406">Unselect hello worker nodes and select hello head nodes</span></span>
* <span data-ttu-id="cb2c7-407">Válassza az **Actions**(Műveletek) > **Selected Hosts** (Kiválasztott gazdagépek) > "**Hosts**(Gazdagépek) > **Restart All Components**(Összes gazdagép újraindítása) elemet</span><span class="sxs-lookup"><span data-stu-id="cb2c7-407">Select **Actions** > **Selected Hosts** > "**Hosts** > **Restart All Components**</span></span>

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a><span data-ttu-id="cb2c7-408">Számítási csomópontok konfigurálása az összes leszerelt feldolgozó csomóponton</span><span class="sxs-lookup"><span data-stu-id="cb2c7-408">Configure Compute nodes on each decommissioned worker node(s)</span></span>

1. <span data-ttu-id="cb2c7-409">Jelentkezzen be SSH-n keresztül minden egyes leszerelt feldolgozó csomópontba.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-409">SSH into each decommissioned worker node.</span></span>
2. <span data-ttu-id="cb2c7-410">Futtassa az admin segédprogramot a következő paranccsal: `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-410">Run admin utility using `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span></span>
3. <span data-ttu-id="cb2c7-411">Adjon meg "1" tooselect beállítást "Konfigurálása R Server a Operationalization".</span><span class="sxs-lookup"><span data-stu-id="cb2c7-411">Enter "1" tooselect option "Configure R Server for Operationalization".</span></span>
4. <span data-ttu-id="cb2c7-412">Adja meg a "c"c"tooselect beállítás</span><span class="sxs-lookup"><span data-stu-id="cb2c7-412">Enter "c" tooselect option "C.</span></span> <span data-ttu-id="cb2c7-413">Compute node” (Számítási csomópont) lehetőség kijelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-413">Compute node".</span></span> <span data-ttu-id="cb2c7-414">Ez konfigurálja az hello számítási csomópont hello munkavégző csomóponton.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-414">This configures hello compute node on hello worker node.</span></span>
5. <span data-ttu-id="cb2c7-415">Kilépés hello felügyeleti eszközt.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-415">Exit hello Admin Utility.</span></span>

### <a name="add-compute-nodes-details-on-web-node"></a><span data-ttu-id="cb2c7-416">Számítási csomópontok részleteinek megadása a Web csomóponton</span><span class="sxs-lookup"><span data-stu-id="cb2c7-416">Add compute nodes details on Web Node</span></span>

<span data-ttu-id="cb2c7-417">Leszerelt feldolgozó csomópontjaihoz konfigurálása után toorun számítási csomópont, térjen vissza hello peremhálózati csomóponton, és adja hozzá a leszerelt munkavégző csomópontokhoz IP-címek hello Microsoft R Server webes csomópont-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-417">Once all decommissioned worker nodes have been configured toorun compute node, come back on hello edge node and add decommissioned worker nodes' IP addresses in hello Microsoft R Server web node's configuration:</span></span>

* <span data-ttu-id="cb2c7-418">SSH-ból hello élcsomópont.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-418">SSH into hello edge node.</span></span>
* <span data-ttu-id="cb2c7-419">Futtassa az `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-419">Run `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span></span>
* <span data-ttu-id="cb2c7-420">Hello "URI" szakaszban keresse meg, és adja hozzá a munkavégző csomópont IP-cím és port részleteit.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-420">Look for hello "URIs" section, and add worker node's IP and port details.</span></span>

    ![feldolgozó csomópont(ok) leszerelési parancssora](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a><span data-ttu-id="cb2c7-422">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="cb2c7-422">Troubleshoot</span></span>

<span data-ttu-id="cb2c7-423">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="cb2c7-423">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>


## <a name="next-steps"></a><span data-ttu-id="cb2c7-424">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb2c7-424">Next steps</span></span>

<span data-ttu-id="cb2c7-425">Most meg kell ismernie, hogyan toocreate egy új HDInsight-fürtöt, amely tartalmazza az hello R Server és hello használatának alapjaival hello R konzol az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="cb2c7-425">Now you should understand how toocreate a new HDInsight cluster that includes hello R Server and hello basics of using hello R console from an SSH session.</span></span> <span data-ttu-id="cb2c7-426">hello következő témaköreiben egyéb módjai kezelése és az R Server on HDInsight használata:</span><span class="sxs-lookup"><span data-stu-id="cb2c7-426">hello following topics explain other ways of managing and working with R Server on HDInsight:</span></span>

* [<span data-ttu-id="cb2c7-427">Adja hozzá a Rstudióból Server tooHDInsight (Ha nincs telepítve a fürt létrehozása során)</span><span class="sxs-lookup"><span data-stu-id="cb2c7-427">Add RStudio Server tooHDInsight (if not installed during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="cb2c7-428">Számítási környezeti beállítások a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="cb2c7-428">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="cb2c7-429">Azure Storage lehetőségek a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="cb2c7-429">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)
