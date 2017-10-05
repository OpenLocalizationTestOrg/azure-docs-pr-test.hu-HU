---
title: "Az R Server használatának első lépései a HDInsightban – Azure| Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre R Servert tartalmazó Apache Sparkot a HDInsight-fürtön, majd hogyan küldhet el R-szkriptet a fürtön."
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
ms.openlocfilehash: 14e2a14c74e00709e18a80325fbdd3cbcd71da37
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a><span data-ttu-id="758a0-103">R Server a HDInsightban – első lépések</span><span class="sxs-lookup"><span data-stu-id="758a0-103">Get started using R Server on HDInsight</span></span>

<span data-ttu-id="758a0-104">A HDInsight olyan R Server beállítással rendelkezik, amely a HDInsight-fürtbe integrálható.</span><span class="sxs-lookup"><span data-stu-id="758a0-104">HDInsight includes an R Server option to be integrated into your HDInsight cluster.</span></span> <span data-ttu-id="758a0-105">Ez lehetővé teszi, hogy az R-szkriptek a Spark és a MapReduce eszközökkel elosztott számításokat futtathassanak.</span><span class="sxs-lookup"><span data-stu-id="758a0-105">This option allows R scripts to use Spark and MapReduce to run distributed computations.</span></span> <span data-ttu-id="758a0-106">A dokumentumban foglaltakat követve elsajátíthatja az R Server létrehozását a HDInsight-fürtökön, majd R-szkriptet futtathat, amely a Spark elosztott R számításokhoz való használatát mutatja be.</span><span class="sxs-lookup"><span data-stu-id="758a0-106">In this document, you learn how to create an R Server on HDInsight cluster and then run an R script that demonstrates using Spark for distributed R computations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="758a0-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="758a0-107">Prerequisites</span></span>

* <span data-ttu-id="758a0-108">**Azure-előfizetés**: Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="758a0-108">**An Azure subscription**: Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="758a0-109">További információt az [ingyenes Microsoft Azure-próbafiók beszerzésével](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) foglalkozó cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="758a0-109">Go to the article [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) for more information.</span></span>
* <span data-ttu-id="758a0-110">**Secure Shell- (SSH-) ügyfél**: Egy SSH-ügyféllel távolról csatlakozhat a HDInsight-fürthöz, és közvetlenül a fürtön futtathat parancsokat.</span><span class="sxs-lookup"><span data-stu-id="758a0-110">**A Secure Shell (SSH) client**: An SSH client is used to remotely connect to the HDInsight cluster and run commands directly on the cluster.</span></span> <span data-ttu-id="758a0-111">További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="758a0-111">For more information, see [Use SSH with HDInsight.](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
* <span data-ttu-id="758a0-112">**Secure Shell- (SSH-) kulcsok (nem kötelező)**: A fürthöz való csatlakozáshoz használt SSH-fiókjának biztonságát jelszóval vagy nyilvános kulccsal biztosíthatja.</span><span class="sxs-lookup"><span data-stu-id="758a0-112">**SSH keys (optional)**: You can secure the SSH account used to connect to the cluster using either a password or a public key.</span></span> <span data-ttu-id="758a0-113">A jelszó használata könnyebb, és lehetővé teszi, hogy nyilvános/titkos kulcspár létrehozása nélkül tegye meg az első lépéseket.</span><span class="sxs-lookup"><span data-stu-id="758a0-113">Using a password is easier, and allows you to get started without having to create a public/private key pair.</span></span> <span data-ttu-id="758a0-114">A kulcs használata azonban biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="758a0-114">However, using a key is more secure.</span></span>

> [!NOTE]
> <span data-ttu-id="758a0-115">A dokumentum lépései azt feltételezik, hogy jelszót használ.</span><span class="sxs-lookup"><span data-stu-id="758a0-115">The steps in this document assume that you are using a password.</span></span>


## <a name="automated-cluster-creation"></a><span data-ttu-id="758a0-116">Fürt automatikus létrehozása</span><span class="sxs-lookup"><span data-stu-id="758a0-116">Automated cluster creation</span></span>

<span data-ttu-id="758a0-117">Azure Resource Manager-sablonok, az SDK, illetve a PowerShell használatával is automatizálható a HDInsight R Server-kiszolgálók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="758a0-117">You can automate the creation of HDInsight R Servers using Azure Resource Manager templates, the SDK, and also PowerShell.</span></span>

* <span data-ttu-id="758a0-118">Az R Server Azure Resource Management-sablonnal végzett létrehozásáról az [R Server HDInsight-fürt üzembe helyezésének leírásában](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) talál további információt.</span><span class="sxs-lookup"><span data-stu-id="758a0-118">To create an R Server using an Azure Resource Management template, see [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span></span>
* <span data-ttu-id="758a0-119">R Server létrehozása .NET SDK-val: [Linux-alapú fürtök létrehozása a HDInsightban a .NET SDK-val](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="758a0-119">To create an R Server using the .NET SDK, see [create Linux-based clusters in HDInsight using the .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>
* <span data-ttu-id="758a0-120">Az R Server PowerShell-lel történő üzembe helyezéséről a következővel foglalkozó cikkben talál további információkat: [R Server létrehozása a HDInsightban a PowerShell használatával](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="758a0-120">To deploy R Server using powershell, see the article on [creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span></span>


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a><span data-ttu-id="758a0-121">Fürt létrehozása az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="758a0-121">Create the cluster using the Azure portal</span></span>

1. <span data-ttu-id="758a0-122">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="758a0-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="758a0-123">Válassza az **ÚJ** -> **Intelligencia és elemzés**, -> **HDInsight** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="758a0-123">Select **NEW** -> **Intelligence + Analytics**, -> **HDInsight**.</span></span>

    ![Új fürt létrehozásának képe](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. <span data-ttu-id="758a0-125">A **Gyorslétrehozás** felületen írja be a fürt nevét a **Fürt neve** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="758a0-125">In the **Quick create** experience, enter a name for the cluster in the **Cluster Name** field.</span></span> <span data-ttu-id="758a0-126">Ha több Azure-előfizetése van, az **Előfizetés** bejegyzésben válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="758a0-126">If you have multiple Azure subscriptions, use the **Subscription** entry to select the one you want to use.</span></span>

    ![Fürt neve és az előfizetés kiválasztása](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. <span data-ttu-id="758a0-128">Válassza ki a **Fürt típusát** a **Fürtkonfiguráció** panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="758a0-128">Select **Cluster type** to open the **Cluster configuration** blade.</span></span> <span data-ttu-id="758a0-129">A **Fürtkonfiguráció** panelen válassza ki a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="758a0-129">On the **Cluster Configuration** blade, select the following options:</span></span>

    * <span data-ttu-id="758a0-130">**Fürt típusa**: R Server</span><span class="sxs-lookup"><span data-stu-id="758a0-130">**Cluster Type**: R Server</span></span>
    * <span data-ttu-id="758a0-131">**Verzió**: válassza ki az R Server fürtre telepítendő verzióját.</span><span class="sxs-lookup"><span data-stu-id="758a0-131">**Version**: select the version of R Server to install on the cluster.</span></span> <span data-ttu-id="758a0-132">A jelenleg elérhető verzió az ***R Server 9.1 (HDI 3.6)***.</span><span class="sxs-lookup"><span data-stu-id="758a0-132">The version currently available is ***R Server 9.1 (HDI 3.6)***.</span></span> <span data-ttu-id="758a0-133">Az R Server egyes elérhető verzióinak kibocsátási megjegyzései [itt](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="758a0-133">Release notes for the available versions of R Server are available [here](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span></span>
    * <span data-ttu-id="758a0-134">**R Studio community edition for R Server**: ez a böngésző alapú IDE alapértelmezés szerint telepítve van az élcsomópontra.</span><span class="sxs-lookup"><span data-stu-id="758a0-134">**R Studio community edition for R Server**: this browser-based IDE is installed by default on the edge node.</span></span> <span data-ttu-id="758a0-135">Ha inkább nem szeretné, hogy telepítve legyen, akkor törölje a jelölőnégyzet jelölését.</span><span class="sxs-lookup"><span data-stu-id="758a0-135">If you would prefer to not have it installed, then uncheck the check box.</span></span> <span data-ttu-id="758a0-136">Ha úgy dönt, hogy telepíti, a fürt portálalkalmazás panelén találja az RStudio kiszolgáló bejelentkezésének elérésére szolgáló URL-t, miután a fürt létrejött.</span><span class="sxs-lookup"><span data-stu-id="758a0-136">If you choose to have it installed, the URL for accessing the RStudio Server login is found on a portal application blade for your cluster once it’s been created.</span></span>
    * <span data-ttu-id="758a0-137">Hagyja a többi beállítást az alapértelmezett értéken, és a **Kiválasztás** gombbal mentse a fürttípust.</span><span class="sxs-lookup"><span data-stu-id="758a0-137">Leave the other options at the default values and use the **Select** button to save the cluster type.</span></span>

        ![A Fürt típusa panel képernyőképe](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. <span data-ttu-id="758a0-139">Írja be a **Fürt bejelentkezési felhasználónevét** és a **Fürt bejelentkezési jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="758a0-139">Enter a **Cluster login username** and **Cluster login password**.</span></span>

    <span data-ttu-id="758a0-140">Adjon meg egy **SSH-felhasználónevet**.</span><span class="sxs-lookup"><span data-stu-id="758a0-140">Specify an **SSH Username**.</span></span> <span data-ttu-id="758a0-141">Az SSH-val távolról csatlakozhat a fürthöz egy **Secure Shell- (SSH-)** ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="758a0-141">SSH is used to remotely connect to the cluster using a **Secure Shell (SSH)** client.</span></span> <span data-ttu-id="758a0-142">Megadhatja az SSH-felhasználót ezen a párbeszédpanelen vagy a fürt létrehozása után (a fürt Konfiguráció lapján).</span><span class="sxs-lookup"><span data-stu-id="758a0-142">You can either specify the SSH user in this dialog or after the cluster has been created (in the Configuration tab for the cluster).</span></span> <span data-ttu-id="758a0-143">Az R Server úgy van konfigurálva, hogy a „remoteuser” **SSH-felhasználónevet** várja.</span><span class="sxs-lookup"><span data-stu-id="758a0-143">R Server is configured to expect an **SSH username** of “remoteuser”.</span></span>  <span data-ttu-id="758a0-144">**Ha más felhasználónevet használ, egy további lépést kell elvégeznie a fürt létrehozása után.**</span><span class="sxs-lookup"><span data-stu-id="758a0-144">**If you use a different username, you must perform an additional step after the cluster is created.**</span></span>

    <span data-ttu-id="758a0-145">Hagyja bejelölve a **Használja ugyanazt a jelszót, mint a fürtbe való bejelentkezésekor** jelölőnégyzetet a **JELSZÓ** használatához hitelesítési típusként, ha nem szeretne nyilvános kulcsot használni.</span><span class="sxs-lookup"><span data-stu-id="758a0-145">Leave the box checked for **Use same password as cluster login** to use **PASSWORD** as the authentication type unless you prefer use of a public key.</span></span>  <span data-ttu-id="758a0-146">Nyilvános/titkos kulcspárra van szüksége, ha a fürtön lévő R Servert egy távoli ügyfélen keresztül szeretné elérni, például RTVS-en, RStudión vagy másik asztali IDE-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="758a0-146">You need a public/private key pair to access R Server on the cluster via a remote client as, for example, RTVS, RStudio or another desktop IDE.</span></span> <span data-ttu-id="758a0-147">Ha az RStudio Server community edition kiadását telepíti, SSH-jelszót kell választania.</span><span class="sxs-lookup"><span data-stu-id="758a0-147">If you install the RStudio Server community edition, you need to choose an SSH password.</span></span>     

    <span data-ttu-id="758a0-148">Nyilvános/titkos kulcspár használatához törölje a **Használja ugyanazt a jelszót, mint a fürtbe való bejelentkezésekor** jelölőnégyzet jelölését, majd válassza a **NYILVÁNOS KULCS** elemet, és folytassa a következőképpen.</span><span class="sxs-lookup"><span data-stu-id="758a0-148">To create and use a public/private key pair, uncheck **Use same password as cluster login** and then select **PUBLIC KEY** and proceed as follows.</span></span> <span data-ttu-id="758a0-149">Ezek az utasítások feltételezik, hogy a Cygwin with ssh-keygen vagy ennek megfelelő van telepítve.</span><span class="sxs-lookup"><span data-stu-id="758a0-149">These instructions assume that you have Cygwin with ssh-keygen or an equivalent installed.</span></span>

    * <span data-ttu-id="758a0-150">Hozzon létre egy nyilvános/titkos kulcspárt a parancssorból a laptopján:</span><span class="sxs-lookup"><span data-stu-id="758a0-150">Generate a public/private key pair from the command prompt on your laptop:</span></span>

        <span data-ttu-id="758a0-151">ssh-keygen -t rsa -b 2048</span><span class="sxs-lookup"><span data-stu-id="758a0-151">ssh-keygen -t rsa -b 2048</span></span>

    * <span data-ttu-id="758a0-152">Kövesse promptot a kulcsfájl elnevezéséhez, majd írjon be egy hozzáférési kódot a további biztonság érdekében.</span><span class="sxs-lookup"><span data-stu-id="758a0-152">Follow the prompt to name a key file and then enter a passphrase for added security.</span></span> <span data-ttu-id="758a0-153">A képernyőjének a következő képhez hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="758a0-153">Your screen should look something like the following image:</span></span>

        ![SSH parancssor Windows rendszerben](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * <span data-ttu-id="758a0-155">Ez a parancs létrehoz egy titkos kulcsfájlt és egy nyilvános kulcsfájlt <titkos-kulcs-fájlnév>.pub néven, például furiosa és furiosa.pub néven.</span><span class="sxs-lookup"><span data-stu-id="758a0-155">This command creates a private key file and a public key file under the name <private-key-filename>.pub, for example furiosa and furiosa.pub.</span></span>

        ![SSH dir](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * <span data-ttu-id="758a0-157">Ezután adja meg a nyilvános kulcsfájlt (&#42;.pub), amikor a HDI-fürt hitelesítő adatait rendeli hozzá, és végül erősítse meg az erőforráscsoportot és a régiót, és válassza a **Tovább** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="758a0-157">Then specify the public key file (&#42;.pub) when assigning HDI cluster credentials and finally confirm your resource group and region and select **Next**.</span></span>

        ![Hitelesítő adatok panel](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * <span data-ttu-id="758a0-159">Engedélyek módosítása a titkos kulcsfájlon a laptopján:</span><span class="sxs-lookup"><span data-stu-id="758a0-159">Change permissions on the private keyfile on your laptop:</span></span>

        <span data-ttu-id="758a0-160">chmod 600 <titkos-kulcs-fájlnév></span><span class="sxs-lookup"><span data-stu-id="758a0-160">chmod 600 <private-key-filename></span></span>

   * <span data-ttu-id="758a0-161">A titkos kulcsfájl használata SSH-val a távoli bejelentkezéshez:</span><span class="sxs-lookup"><span data-stu-id="758a0-161">Use the private key file with SSH for remote login:</span></span>

        <span data-ttu-id="758a0-162">ssh –i <titkos-kulcs-fájlnév> remoteuser@<hostname public ip></span><span class="sxs-lookup"><span data-stu-id="758a0-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span></span>

      <span data-ttu-id="758a0-163">Vagy a Hadoop Spark számítási környezet R Serverhez definíciójának részeként az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="758a0-163">Or, as part the definition of your Hadoop Spark compute context for R Server on the client.</span></span> <span data-ttu-id="758a0-164">Lásd a **Microsoft R Server használata Hadoop-ügyfélként** alszakaszt a [számítási környezet Sparkhoz való létrehozásával](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) foglalkozó weblapon.</span><span class="sxs-lookup"><span data-stu-id="758a0-164">See the **Using Microsoft R Server as a Hadoop Client** subsection in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span></span>

6. <span data-ttu-id="758a0-165">A gyorslétrehozás átirányítja a **Tárolás** panelre, ahol kiválaszthatja a fürt által használt HDFS-fájlrendszer elsődleges helyéhez használt tárfiók beállításait.</span><span class="sxs-lookup"><span data-stu-id="758a0-165">The quick create transitions you to the **Storage** blade to select the storage account settings to be used for the primary location of the HDFS file system used by the cluster.</span></span> <span data-ttu-id="758a0-166">Válasszon új vagy meglévő Azure Storage-fiókot vagy meglévő Data Lake tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="758a0-166">Select either a new or existing Azure Storage account or an existing Data Lake Storage account.</span></span>

    - <span data-ttu-id="758a0-167">Ha Azure Storage-fiókot választ, kiválaszthat egy meglévő tárfiókot a **Tárfiók kiválasztása** elem, majd a megfelelő fiók kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="758a0-167">If you select an Azure Storage account, an existing storage account is selected by choosing **Select a storage account** and then selecting the relevant account.</span></span> <span data-ttu-id="758a0-168">Hozzon létre egy új fiókot az **Új létrehozása** hivatkozással a **Tárfiók kiválasztása** szakaszban.</span><span class="sxs-lookup"><span data-stu-id="758a0-168">Create a new account using the **Create New** link in the **Select a storage account** section.</span></span>

      > [!NOTE]
      > <span data-ttu-id="758a0-169">Ha az **Új** lehetőséget választja, be kell írnia egy nevet az új tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="758a0-169">If you select **New** you must enter a name for the new storage account.</span></span> <span data-ttu-id="758a0-170">Egy zöld pipa jelenik meg, ha a rendszer elfogadja a nevet.</span><span class="sxs-lookup"><span data-stu-id="758a0-170">A green check appears if the name is accepted.</span></span>

      <span data-ttu-id="758a0-171">Az **Alapértelmezett tároló** alapértéke a fürt neve lesz.</span><span class="sxs-lookup"><span data-stu-id="758a0-171">The **Default Container** defaults to the name of the cluster.</span></span> <span data-ttu-id="758a0-172">Hagyja meg ezt az alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="758a0-172">Leave this default as the value.</span></span>

      <span data-ttu-id="758a0-173">Ha új tárfiókot választott, megjelenik egy kérés a **Hely** kiválasztásához, ahol kiválaszthatja a tárfiók létrehozásának régióját.</span><span class="sxs-lookup"><span data-stu-id="758a0-173">If a new storage account option was selected a prompt to select **Location** is given to select which region to create the storage account.</span></span>  

         ![Adatforrás panel](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > <span data-ttu-id="758a0-175">Az alapértelmezett adatforrás helyének kiválasztása a HDInsight-fürt helyét is beállítja.</span><span class="sxs-lookup"><span data-stu-id="758a0-175">Selecting the location for the default data source  also sets the location of the HDInsight cluster.</span></span> <span data-ttu-id="758a0-176">A fürtnek és az alapértelmezett adatforrásnak ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="758a0-176">The cluster and default data source must be in the same region.</span></span>

    - <span data-ttu-id="758a0-177">Ha egy meglévő Data Lake Store használatát választja, akkor válassza ki a használni kívánt ADLS-tárfiókot, és adja a fürt *ADD*-identitását a fürthöz a tároló hozzáférésének engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="758a0-177">If you want to use an existing Data Lake Store, then select the ADLS storage account to use and add the cluster *ADD* identity to your cluster to allow access to the store.</span></span> <span data-ttu-id="758a0-178">Ezen folyamatról további információért tekintse át a [HDInsight-fürt létrehozása a Data Lake Store-ral az Azure Portalon](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) szakaszt.</span><span class="sxs-lookup"><span data-stu-id="758a0-178">For more information on this process, see [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).</span></span>

    <span data-ttu-id="758a0-179">A **Kiválasztás** gombbal mentse az adatforrás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="758a0-179">Use the **Select** button to save the data source configuration.</span></span>


7. <span data-ttu-id="758a0-180">Ezután megjelenik az **Összegzés** panel az összes beállítás érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="758a0-180">The **Summary** blade then displays to validate all your settings.</span></span> <span data-ttu-id="758a0-181">Itt módosíthatja a **Fürt méretét**, hogy módosítsa a fürtben lévő kiszolgálók számát, és megadhat minden futtatni kívánt **szkriptműveletet**.</span><span class="sxs-lookup"><span data-stu-id="758a0-181">Here you can change your **Cluster size** to modify the number of servers in your cluster and also specify any **Script actions** you want to run.</span></span> <span data-ttu-id="758a0-182">Ha tudja, hogy nincs szüksége nagy fürtre, hagyja a munkavégző csomópontok számát az alapértelmezett `4` értéken.</span><span class="sxs-lookup"><span data-stu-id="758a0-182">Unless you know that you need a larger cluster, leave the number of worker nodes at the default of `4`.</span></span> <span data-ttu-id="758a0-183">A panelen megjelenik a fürt becsült költsége.</span><span class="sxs-lookup"><span data-stu-id="758a0-183">The estimated cost of the cluster is shown within the blade.</span></span>

    ![fürt összegzése](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > <span data-ttu-id="758a0-185">Szükség esetén később átméretezheti a fürtöt a portálon keresztül (**Fürt** -> **Beállítások** -> **Fürt méretezése**) a munkavégző csomópontok számának növelése vagy csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="758a0-185">If needed, you can resize your cluster later through the Portal (**Cluster** -> **Settings** -> **Scale Cluster**) to increase or decrease the number of worker nodes.</span></span>  <span data-ttu-id="758a0-186">Ez az átméretezés hasznos lehet a fürt lelassításához, amikor nincs használatban, vagy kapacitás hozzáadásához a nagyobb feladatok igényeinek kielégítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="758a0-186">This resizing can be useful for idling down the cluster when not in use, or for adding capacity to meet the needs of larger tasks.</span></span>
   >
   >

   <span data-ttu-id="758a0-187">A fürt, az adatcsomópontok és az élcsomópont átméretezésekor néhány figyelembe veendő tényező a következő:</span><span class="sxs-lookup"><span data-stu-id="758a0-187">Some factors to keep in mind when sizing your cluster, the data nodes, and the edge node include:</span></span>  

   * <span data-ttu-id="758a0-188">A Spark rendszeren az elosztott R Server elemzések teljesítménye arányos a munkavégző csomópontok számával, amikor nagyok az adatok.</span><span class="sxs-lookup"><span data-stu-id="758a0-188">The performance of distributed R Server analyses on Spark is proportional to the number of worker nodes when the data is large.</span></span>  

   * <span data-ttu-id="758a0-189">Az R Server elemzések teljesítménye arányos az elemzett adatok méretével.</span><span class="sxs-lookup"><span data-stu-id="758a0-189">The performance of R Server analyses is linear in the size of data being analyzed.</span></span> <span data-ttu-id="758a0-190">Példa:</span><span class="sxs-lookup"><span data-stu-id="758a0-190">For example:</span></span>  

     * <span data-ttu-id="758a0-191">Kis és közepes méretű adatok esetében a teljesítmény akkor a legnagyobb, amikor az élcsomóponton helyi számítási környezetben van elemezve.</span><span class="sxs-lookup"><span data-stu-id="758a0-191">For small to modest data, performance is best when analyzed in a local compute context on the edge node.</span></span>  <span data-ttu-id="758a0-192">Azokról az alkalmazási helyzetekről, ahol a helyi és a Spark számítási környezetek a legjobban működnek, további információért lásd: Számítási környezeti beállítások a HDInsighton belüli R Server esetében.</span><span class="sxs-lookup"><span data-stu-id="758a0-192">For more information on the scenarios under which the local and Spark compute contexts work best, see  Compute context options for R Server on HDInsight.</span></span><br>
     * <span data-ttu-id="758a0-193">Ha bejelentkezik az élcsomópontra, és futtatja az R-szkriptet, akkor a ScaleR rx-függvényen kívül minden más<strong>helyileg</strong>, az élcsomóponton lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="758a0-193">If you log in to the edge node and run your R script, then all but the ScaleR rx-functions are executed <strong>locally</strong> on the edge node.</span></span> <span data-ttu-id="758a0-194">Ezért az élcsomópont memóriáját és magjainak számát ennek megfelelően kell méretezni.</span><span class="sxs-lookup"><span data-stu-id="758a0-194">So the memory and number of cores of the edge node should be sized accordingly.</span></span> <span data-ttu-id="758a0-195">Ugyanez érvényes, ha az R Server on HDI-t távoli számítási környezetként használja a laptopjáról.</span><span class="sxs-lookup"><span data-stu-id="758a0-195">The same applies if you use R Server on HDI as a remote compute context from your laptop.</span></span>

     ![Csomópontok árképzési szintjei panel](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     <span data-ttu-id="758a0-197">A **Kiválasztás** gombbal mentse a csomópontok árképzésének konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="758a0-197">Use the **Select** button to save the node pricing configuration.</span></span>

   <span data-ttu-id="758a0-198">Található egy **Sablon és paraméterek letöltése** hivatkozás is.</span><span class="sxs-lookup"><span data-stu-id="758a0-198">There is also a link for **Download template and parameters**.</span></span> <span data-ttu-id="758a0-199">Erre a hivatkozásra kattintva olyan szkriptek jelennek meg, amelyekkel automatizálható a fürtök létrehozása a kiválasztott konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="758a0-199">Click on this link to display scripts that can be used to automate the creation of a cluster with the selected configuration.</span></span> <span data-ttu-id="758a0-200">Ezek a szkriptek a fürt Azure Portal bejegyzéséről is elérhetők, miután az létrejött.</span><span class="sxs-lookup"><span data-stu-id="758a0-200">These scripts are also available from the Azure portal entry for your cluster once it has been created.</span></span>

   > [!NOTE]
   > <span data-ttu-id="758a0-201">Némi időt vesz igénybe a fürt létrehozása, általában körülbelül 20 percet.</span><span class="sxs-lookup"><span data-stu-id="758a0-201">It takes some time for the cluster to be created, usually around 20 minutes.</span></span> <span data-ttu-id="758a0-202">A kezdőpulton vagy az **Értesítések** bejegyzésen az oldal bal oldalán lévő csempével ellenőrizheti a létrehozási folyamatot.</span><span class="sxs-lookup"><span data-stu-id="758a0-202">Use the tile on the Startboard, or the **Notifications** entry on the left of the page to check on the creation process.</span></span>
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a><span data-ttu-id="758a0-203">Csatlakozás az RStudio Serverhez</span><span class="sxs-lookup"><span data-stu-id="758a0-203">Connect to RStudio Server</span></span>

<span data-ttu-id="758a0-204">Ha úgy döntött, hogy belefoglalja az RStudio Server community edition kiadást a telepítésbe, akkor az RStudio bejelentkezési oldalát két módon érheti el.</span><span class="sxs-lookup"><span data-stu-id="758a0-204">If you’ve chosen to include RStudio Server community edition in your installation, then you can access the RStudio login via two different methods.</span></span>

1. <span data-ttu-id="758a0-205">Látogasson el a következő URL-re (ahol a **FÜRTNÉV** a létrehozott fürt neve):</span><span class="sxs-lookup"><span data-stu-id="758a0-205">Go to the following URL (where **CLUSTERNAME** is the name of the cluster you've created):</span></span>

    <span data-ttu-id="758a0-206">https://**FÜRTNÉV**.azurehdinsight.net/rstudio/</span><span class="sxs-lookup"><span data-stu-id="758a0-206">https://**CLUSTERNAME**.azurehdinsight.net/rstudio/</span></span>

2. <span data-ttu-id="758a0-207">Nyissa meg a fürt bejegyzését az Azure Portalon, válassza az **R Server irányítópultok** gyorshivatkozást, majd az **R Studio irányítópultot**:</span><span class="sxs-lookup"><span data-stu-id="758a0-207">Open the entry for your cluster in the Azure portal, select the **R Server Dashboards** quick link and then selecting the **R Studio Dashboard**:</span></span>

     ![Az R Studio irányítópult elérése](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Az R Studio irányítópult elérése](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > <span data-ttu-id="758a0-210">A használt módszertől függetlenül, az első bejelentkezéskor kétszer kell elvégeznie a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="758a0-210">Regardless of the method used, the first time you log in you need to authenticate twice.</span></span>  <span data-ttu-id="758a0-211">Az első hitelesítéskor adja meg a *fürt rendszergazdai felhasználói azonosítóját* és *jelszavát*.</span><span class="sxs-lookup"><span data-stu-id="758a0-211">At the first authentication, provide the *cluster Admin userid* and *password*.</span></span> <span data-ttu-id="758a0-212">A második adatkéréskor adja meg az *SSH felhasználói azonosítót* és *jelszót*.</span><span class="sxs-lookup"><span data-stu-id="758a0-212">At the second prompt, provide the *SSH userid* and *password*.</span></span> <span data-ttu-id="758a0-213">A későbbi bejelentkezések csak az *SSH-jelszót* és a *felhasználói azonosítót* kérik.</span><span class="sxs-lookup"><span data-stu-id="758a0-213">Subsequent log ins only require the *SSH password* and *userid*.</span></span>

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a><span data-ttu-id="758a0-214">Csatlakozás az R Server élcsomóponthoz</span><span class="sxs-lookup"><span data-stu-id="758a0-214">Connect to the R Server edge node</span></span>

<span data-ttu-id="758a0-215">Csatlakoztassa a HDInsight-fürt R Server élcsomópontját SSH használatával, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="758a0-215">Connect to R Server edge node of the HDInsight cluster using SSH with the command:</span></span>

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> <span data-ttu-id="758a0-216">A `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` címet megtalálhatja az Azure Portalon, ha kiválasztja a fürtöt, majd az **Összes beállítás** -> **Alkalmazások** -> **RServer** elemekre kattint.</span><span class="sxs-lookup"><span data-stu-id="758a0-216">You can find the `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` address in the Azure portal by selecting your cluster then **All Settings** -> **Apps** -> **RServer**.</span></span> <span data-ttu-id="758a0-217">Ez megjeleníti az élcsomópont SSH végpontinformációit.</span><span class="sxs-lookup"><span data-stu-id="758a0-217">This displays the SSH Endpoint information for the edge node.</span></span>
>
> ![Az élcsomópont SSH végpontjának képe](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

<span data-ttu-id="758a0-219">Ha az SSH-felhasználói fiókhoz jelszót használt, a rendszer felkéri annak megadására.</span><span class="sxs-lookup"><span data-stu-id="758a0-219">If you used a password to secure your SSH user account, you are prompted to enter it.</span></span> <span data-ttu-id="758a0-220">Nyilvános kulcs használatakor lehetséges, hogy az `-i` paraméter használatára van szükség a megfelelő titkos kulcs megadásához.</span><span class="sxs-lookup"><span data-stu-id="758a0-220">If you used a public key, you may have to use the `-i` parameter to specify the matching private key.</span></span> <span data-ttu-id="758a0-221">Példa:</span><span class="sxs-lookup"><span data-stu-id="758a0-221">For example:</span></span>

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="758a0-222">További információért lásd: [Csatlakozás a HDInsighthoz (Hadoop) SSH-val](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="758a0-222">For more information, see [Connect to HDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="758a0-223">Ha csatlakoztatva van, a következőhöz hasonló adatkérés érkezik:</span><span class="sxs-lookup"><span data-stu-id="758a0-223">Once connected, you arrive at a prompt similar to the following:</span></span>

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a><span data-ttu-id="758a0-224">Több párhuzamos felhasználó engedélyezése</span><span class="sxs-lookup"><span data-stu-id="758a0-224">Enable multiple concurrent users</span></span>

<span data-ttu-id="758a0-225">Több párhuzamos felhasználó úgy tud engedélyezni, ha több felhasználót is hozzáad ahhoz az élcsomóponthoz, amelyen az RStudio közösségi verziója fut.</span><span class="sxs-lookup"><span data-stu-id="758a0-225">You can enable multiple concurrent users by adding more users for the edge node on which the RStudio community version runs.</span></span>

<span data-ttu-id="758a0-226">HDInsight-fürt létrehozásakor két felhasználót kell megadnia, egy HTTP-felhasználót és egy SSH-felhasználót:</span><span class="sxs-lookup"><span data-stu-id="758a0-226">When you create an HDInsight cluster, you must provide two users, an HTTP user and an SSH user:</span></span>

![1. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- <span data-ttu-id="758a0-228">**Fürt bejelentkezési felhasználóneve**: HTTP-felhasználó a létrehozott HDInsight-fürtöket védő HDInsight-átjárón át történő hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="758a0-228">**Cluster login username**: an HTTP user for authentication through the HDInsight gateway that is used to protect the HDInsight clusters you created.</span></span> <span data-ttu-id="758a0-229">Ez a HTTP-felhasználó használatos az Ambari, a YARN felhasználói felületének és más felhasználóifelület-összetevőknek az elérésére.</span><span class="sxs-lookup"><span data-stu-id="758a0-229">This HTTP user is used to access the Ambari UI, YARN UI, as well as other UI components.</span></span>
- <span data-ttu-id="758a0-230">**Secure Shell- (SSH-) felhasználónév**: SSH-felhasználó, aki a fürtöt biztonságos felületen keresztül éri el.</span><span class="sxs-lookup"><span data-stu-id="758a0-230">**Secure Shell (SSH) username**: an SSH user to access the cluster through secure shell.</span></span> <span data-ttu-id="758a0-231">Ez a felhasználó a Linux rendszerben az összes főcsomópont, munkavégző csomópont és élcsomópont felhasználója.</span><span class="sxs-lookup"><span data-stu-id="758a0-231">This user is a user in the Linux system for all the head nodes, worker nodes, and edge nodes.</span></span> <span data-ttu-id="758a0-232">Így Secure Shellt használhat a távoli fürt bármely csomópontjának elérésére.</span><span class="sxs-lookup"><span data-stu-id="758a0-232">So you can use secure shell to access any of the nodes in a remote cluster.</span></span>

<span data-ttu-id="758a0-233">A Microsoft R Serveren a HDInsight típusú fürtökben használt R Studio Server Community verziója bejelentkezési mechanizmusként csak Linux-felhasználónevet és -jelszót fogad el.</span><span class="sxs-lookup"><span data-stu-id="758a0-233">The R Studio Server Community version used in the Microsoft R Server on HDInsight type cluster accepts only Linux username and password as a login mechanism.</span></span> <span data-ttu-id="758a0-234">Nem támogatja a jogkivonatok átadását.</span><span class="sxs-lookup"><span data-stu-id="758a0-234">It does not support passing tokens.</span></span> <span data-ttu-id="758a0-235">Tehát, ha létrehozott egy új fürtöt, és az R Studiót szeretné használni az eléréséhez akkor, kétszer kell bejelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="758a0-235">So if you have created a new cluster and want to use R Studio to access it, you need to log in twice.</span></span>

- <span data-ttu-id="758a0-236">Először jelentkezzen be a HTTP felhasználói hitelesítő adatokkal a HDInsight-átjárón:</span><span class="sxs-lookup"><span data-stu-id="758a0-236">First log in using the HTTP user credentials through the HDInsight Gateway:</span></span> 

    ![2a párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- <span data-ttu-id="758a0-238">Majd használja az SSH felhasználói hitelesítő adatokat az RStudióba való bejelentkezéshez:</span><span class="sxs-lookup"><span data-stu-id="758a0-238">Then use the SSH user credentials to log in to RStudio:</span></span>
  
    ![2b párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

<span data-ttu-id="758a0-240">Jelenleg HDInsight-fürt üzembe helyezésekor csak egy SSH felhasználói fiókot lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="758a0-240">Currently, only one SSH user account can be created when provisioning an HDInsight cluster.</span></span> <span data-ttu-id="758a0-241">Ezért, ha HDInsight-fürtökben több felhasználó számára engedélyezni szeretné a Microsoft R Server elérését, akkor további felhasználókat kell létrehoznia a Linux rendszerben.</span><span class="sxs-lookup"><span data-stu-id="758a0-241">So to enable multiple users to access Microsoft R Server on HDInsight clusters, we need to create additional users in the Linux system.</span></span>

<span data-ttu-id="758a0-242">Mivel az RStudio Server Community kiadása a fürt élcsomópontján fut, több lépést is meg kell tenni:</span><span class="sxs-lookup"><span data-stu-id="758a0-242">Because RStudio Server Community is running on the cluster’s edge node, there are several steps here:</span></span>

1. <span data-ttu-id="758a0-243">A létrehozott SSH-felhasználó használata az élcsomópontra való bejelentkezéshez</span><span class="sxs-lookup"><span data-stu-id="758a0-243">Use the created SSH user to log in to the edge node</span></span>
2. <span data-ttu-id="758a0-244">További Linux-felhasználók hozzáadása az élcsomópontban</span><span class="sxs-lookup"><span data-stu-id="758a0-244">Add more Linux users in edge node</span></span>
3. <span data-ttu-id="758a0-245">Az RStudio Community verziójának használata a létrehozott felhasználóval</span><span class="sxs-lookup"><span data-stu-id="758a0-245">Use RStudio Community version with the user created</span></span>

### <a name="step-1-use-the-created-ssh-user-to-log-in-to-the-edge-node"></a><span data-ttu-id="758a0-246">1. lépés: A létrehozott SSH-felhasználó használata az élcsomópontra való bejelentkezéshez</span><span class="sxs-lookup"><span data-stu-id="758a0-246">Step 1: Use the created SSH user to log in to the edge node</span></span>

<span data-ttu-id="758a0-247">Töltsön le egy SSH-eszközt (például a Putty-t), és használja a meglévő SSH-felhasználót a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="758a0-247">Download any SSH tool (such as Putty) and use the existing SSH user to log in.</span></span> <span data-ttu-id="758a0-248">Ezután az élcsomópont eléréséhez kövesse a [Csatlakozás a HDInsighthoz (Hadoop) SSH-val](hdinsight-hadoop-linux-use-ssh-unix.md) szakasz utasításait.</span><span class="sxs-lookup"><span data-stu-id="758a0-248">Then follow the instructions provided in [Connect to HDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) to access the edge node.</span></span> <span data-ttu-id="758a0-249">Az élcsomópont címe az R Serverhez a HDInsight-fürtön a következő: *clustername-ed-ssh.azurehdinsight.net*</span><span class="sxs-lookup"><span data-stu-id="758a0-249">The edge node address for R Server on HDInsight cluster is: *clustername-ed-ssh.azurehdinsight.net*</span></span>


### <a name="step-2-add-more-linux-users-in-edge-node"></a><span data-ttu-id="758a0-250">2. lépés: További Linux-felhasználók hozzáadása az élcsomópontban</span><span class="sxs-lookup"><span data-stu-id="758a0-250">Step 2: Add more Linux users in edge node</span></span>

<span data-ttu-id="758a0-251">Felhasználó élcsomóponthoz adásához hajtsa végre a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="758a0-251">To add a user to the edge node, execute the commands:</span></span>

    sudo useradd yournewusername -m
    sudo passwd yourusername

<span data-ttu-id="758a0-252">A programnak a következő elemeket kell visszaadnia:</span><span class="sxs-lookup"><span data-stu-id="758a0-252">You should see the following items returned:</span></span> 

![3. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

<span data-ttu-id="758a0-254">Amikor a rendszer az „aktuális Kerberos-jelszót” kéri, nyomja meg az **Enter** billentyűt a figyelmen kívül hagyásához.</span><span class="sxs-lookup"><span data-stu-id="758a0-254">When prompted for “Current Kerberos password:”, just press **Enter** to ignore it.</span></span> <span data-ttu-id="758a0-255">A `useradd` parancs `-m` kapcsolója jelzi, hogy a rendszer létrehoz egy kezdőmappát a felhasználó számára, amely szükséges az RStudio Community verziójához.</span><span class="sxs-lookup"><span data-stu-id="758a0-255">The `-m` option in `useradd` command indicates that the system will create a home folder for the user, which is required for RStudio Community version.</span></span>


### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a><span data-ttu-id="758a0-256">3. lépés: Az RStudio Community verziójának használata a létrehozott felhasználóval</span><span class="sxs-lookup"><span data-stu-id="758a0-256">Step 3: Use RStudio Community version with the user created</span></span>

<span data-ttu-id="758a0-257">A létrehozott felhasználóval jelentkezhet be az RStudióba:</span><span class="sxs-lookup"><span data-stu-id="758a0-257">Use the user created to log in to RStudio:</span></span>

![4. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

<span data-ttu-id="758a0-259">Az RStudio jelzi, hogy az új felhasználót használja a fürtre való bejelentkezéshez (itt például az *sshuser6* felhasználót):</span><span class="sxs-lookup"><span data-stu-id="758a0-259">Notice that RStudio indicates that you are using the new user (here, for example, *sshuser6*) to log in the cluster:</span></span> 

![5. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

<span data-ttu-id="758a0-261">Az eredeti hitelesítő adatokkal (alapértelmezés szerint ez az *sshuser*) egyidejűleg egy másik böngészőablakból is bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="758a0-261">You can also log in using the original credentials (by default, it is *sshuser*) concurrently from another browser window.</span></span>

<span data-ttu-id="758a0-262">Feladat küldéséhez használhatja a scaleR-függvényeket.</span><span class="sxs-lookup"><span data-stu-id="758a0-262">You can submit a job using scaleR functions.</span></span> <span data-ttu-id="758a0-263">Íme egy példa a feladat futtatására használt parancsokra:</span><span class="sxs-lookup"><span data-stu-id="758a0-263">Here is an example of the commands used to run a job:</span></span>

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
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

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


<span data-ttu-id="758a0-264">Figyelje meg, hogy a beküldött munkák más felhasználónév alatt szerepelnek a YARN felhasználói felületén:</span><span class="sxs-lookup"><span data-stu-id="758a0-264">Notice that the jobs submitted are under different user names in YARN UI:</span></span>

![6. párhuzamos felhasználó](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

<span data-ttu-id="758a0-266">Figyelje meg azt is, hogy az újonnan felvett felhasználók nem rendelkeznek gyökérjogosultságokkal a Linux rendszerben, de ugyanolyan hozzáférésük van a távoli HDFS- és WASB-tárolón az összes fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="758a0-266">Note also that the newly added users do not have root privileges in Linux system, but they do have the same access to all the files in the remote HDFS and WASB storage.</span></span>


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a><span data-ttu-id="758a0-267">Az R-konzol használata</span><span class="sxs-lookup"><span data-stu-id="758a0-267">Use the R console</span></span>

1. <span data-ttu-id="758a0-268">Az SSH-munkamenetből a következő paranccsal indíthatja el az R-konzolt:</span><span class="sxs-lookup"><span data-stu-id="758a0-268">From the SSH session, use the following command to start the R console:</span></span>  

        R

2. <span data-ttu-id="758a0-269">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="758a0-269">You should see output similar to the following:</span></span>
    
    <span data-ttu-id="758a0-270">R 3.2.2 (2015-08-14) verzió – „Fire Safety”  Copyright (C) 2015 The R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64 bites)</span><span class="sxs-lookup"><span data-stu-id="758a0-270">R version 3.2.2 (2015-08-14) -- "Fire Safety"  Copyright (C) 2015 The R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)</span></span>

    <span data-ttu-id="758a0-271">Az R egy ingyenes szoftver, és EGYÁLTALÁN SEMMILYEN JÓTÁLLÁS SEM vonatkozik rá.</span><span class="sxs-lookup"><span data-stu-id="758a0-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span></span>
    <span data-ttu-id="758a0-272">Bizonyos feltételek teljesítése esetén nyugodtan terjesztheti.</span><span class="sxs-lookup"><span data-stu-id="758a0-272">You are welcome to redistribute it under certain conditions.</span></span>
    <span data-ttu-id="758a0-273">A terjesztésre vonatkozó részletekért írja be a 'license()' vagy a 'licence()' parancsot.</span><span class="sxs-lookup"><span data-stu-id="758a0-273">Type 'license()' or 'licence()' for distribution details.</span></span>

    <span data-ttu-id="758a0-274">Támogatja a természetes nyelveket, de angol területi beállítással fut</span><span class="sxs-lookup"><span data-stu-id="758a0-274">Natural language support but running in an English locale</span></span>

    <span data-ttu-id="758a0-275">Az R egy együttműködésen alapuló projekt, sok hozzájárulóval.</span><span class="sxs-lookup"><span data-stu-id="758a0-275">R is a collaborative project with many contributors.</span></span>
    <span data-ttu-id="758a0-276">További információért írja be a 'contributors()' parancsot, az R vagy az R-csomagok publikációkban történő idézésének módját pedig a 'citation()' beírásával ismerheti meg.</span><span class="sxs-lookup"><span data-stu-id="758a0-276">Type 'contributors()' for more information and 'citation()' on how to cite R or R packages in publications.</span></span>

    <span data-ttu-id="758a0-277">Írja be a 'demo()' parancsot néhány bemutató, a 'help()' parancsot az online súgó megtekintéséhez vagy a 'help.start()' parancsot, ha HTML-böngészőfelületen szeretné megtekinteni a súgót.</span><span class="sxs-lookup"><span data-stu-id="758a0-277">Type 'demo()' for some demos, 'help()' for on-line help, or 'help.start()' for an HTML browser interface to help.</span></span>
    <span data-ttu-id="758a0-278">Írja be a 'q()' parancsot a kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="758a0-278">Type 'q()' to quit R.</span></span>

    <span data-ttu-id="758a0-279">Microsoft R Server 8.0-s verzió: az R Microsoft-csomagok továbbfejlesztett terjesztési verziója Copyright (C) 2016 Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="758a0-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span></span>

    <span data-ttu-id="758a0-280">A kiadási megjegyzések megtekintéséhez írja be a 'readme()' parancsot.</span><span class="sxs-lookup"><span data-stu-id="758a0-280">Type 'readme()' for release notes.</span></span>
    >

3. <span data-ttu-id="758a0-281">A `>` adatkérésben beírhatja az R-kódot.</span><span class="sxs-lookup"><span data-stu-id="758a0-281">From the `>` prompt, you can enter R code.</span></span> <span data-ttu-id="758a0-282">Az R Server olyan csomagokat tartalmaz, amelyekkel könnyedén használhatja a Hadoopot és futtathat elosztott számításokat.</span><span class="sxs-lookup"><span data-stu-id="758a0-282">R server includes packages that allow you to easily interact with Hadoop and run distributed computations.</span></span> <span data-ttu-id="758a0-283">A HDInsight-fürt alapértelmezett fájlrendszerének gyökerét például a következő paranccsal tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="758a0-283">For example, use the following command to view the root of the default file system for the HDInsight cluster:</span></span>

    <span data-ttu-id="758a0-284">rxHadoopListFiles("/")</span><span class="sxs-lookup"><span data-stu-id="758a0-284">rxHadoopListFiles("/")</span></span>

4. <span data-ttu-id="758a0-285">A WASB stíluscímzést is használhatja.</span><span class="sxs-lookup"><span data-stu-id="758a0-285">You can also use the WASB style addressing.</span></span>

    <span data-ttu-id="758a0-286">rxHadoopListFiles("wasb:///")</span><span class="sxs-lookup"><span data-stu-id="758a0-286">rxHadoopListFiles("wasb:///")</span></span>


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a><span data-ttu-id="758a0-287">Az R Server használata HDI-n a Microsoft R Server vagy a Microsoft R ügyfél egy távoli példányáról</span><span class="sxs-lookup"><span data-stu-id="758a0-287">Using R Server on HDI from a remote instance of Microsoft R Server or Microsoft R Client</span></span>

<span data-ttu-id="758a0-288">A HDI Hadoop Spark számítási környezetet el lehet érni a Microsoft R Server vagy a Microsoft R-ügyfél számítógépen vagy laptopon futó távoli példányáról.</span><span class="sxs-lookup"><span data-stu-id="758a0-288">It is possible to set up access to the HDI Hadoop Spark compute context from a remote instance of Microsoft R Server or Microsoft R Client running on a desktop or laptop.</span></span> <span data-ttu-id="758a0-289">Lásd a **Microsoft R Server használata Hadoop-ügyfélként** alszakaszt a [számítási környezet Sparkhoz való létrehozásával](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) foglalkozó weblapon.</span><span class="sxs-lookup"><span data-stu-id="758a0-289">See **Using Microsoft R Server as a Hadoop Client** subsection in the [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span></span> <span data-ttu-id="758a0-290">Ehhez meg kell adnia a következő beállításokat, amikor meghatározza az RxSpark számítási környezetet a laptopon: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches és sshProfileScript.</span><span class="sxs-lookup"><span data-stu-id="758a0-290">To do so, you need to specify the following options when defining the RxSpark compute context on your laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, and sshProfileScript.</span></span> <span data-ttu-id="758a0-291">Példa:</span><span class="sxs-lookup"><span data-stu-id="758a0-291">For example:</span></span>


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


## <a name="use-a-compute-context"></a><span data-ttu-id="758a0-292">Számítási környezet használata</span><span class="sxs-lookup"><span data-stu-id="758a0-292">Use a compute context</span></span>

<span data-ttu-id="758a0-293">A számítási környezetekkel vezérelheti, hogy a számítás helyben történik-e az élcsomóponton, vagy elosztottan a HDInsight-fürtben lévő csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="758a0-293">A compute context allows you to control whether computation is performed locally on the edge node or distributed across the nodes in the HDInsight cluster.</span></span>

1. <span data-ttu-id="758a0-294">Az RStudio Serverről vagy az R-konzolról (SSH-munkamenetben) a következő kód használatával töltse be a példaadatokat a HDInsight alapértelmezett tárolójába:</span><span class="sxs-lookup"><span data-stu-id="758a0-294">From RStudio Server or the R console (in an SSH session), use the following code to load example data into the default storage for HDInsight:</span></span>

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
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

        # Set directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. <span data-ttu-id="758a0-295">Ezután hozzon létre adatinformációkat, és határozzon meg két adatforrást, hogy használhassuk az adatokat.</span><span class="sxs-lookup"><span data-stu-id="758a0-295">Next, let's create some data info and define two data sources so that we can work with the data.</span></span>

        # Define the HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. <span data-ttu-id="758a0-296">Futtasson logisztikai regressziót az adatokon a helyi számítási környezettel.</span><span class="sxs-lookup"><span data-stu-id="758a0-296">Let's run a logistic regression over the data using the local compute context.</span></span>

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    <span data-ttu-id="758a0-297">Az alábbihoz hasonló sorokkal végződő kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="758a0-297">You should see output that ends with lines similar to the following:</span></span>

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

4. <span data-ttu-id="758a0-298">Ezután futtassa ugyanezt a logisztikai regressziót a Spark környezettel.</span><span class="sxs-lookup"><span data-stu-id="758a0-298">Next, let's run the same logistic regression using the Spark context.</span></span> <span data-ttu-id="758a0-299">A Spark környezet elosztja a feldolgozást a HDInsight-fürt összes munkavégző csomópontja között.</span><span class="sxs-lookup"><span data-stu-id="758a0-299">The Spark context distributes the processing over all the worker nodes in the HDInsight cluster.</span></span>

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > <span data-ttu-id="758a0-300">A MapReduce eszközzel is eloszthatja a számítást a fürtcsomópontok között.</span><span class="sxs-lookup"><span data-stu-id="758a0-300">You can also use MapReduce to distribute computation across cluster nodes.</span></span> <span data-ttu-id="758a0-301">A számítási környezetről további információért lásd: [Számítási környezeti beállítások a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-compute-contexts.md).</span><span class="sxs-lookup"><span data-stu-id="758a0-301">For more information on compute context, see [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span></span>


## <a name="distribute-r-code-to-multiple-nodes"></a><span data-ttu-id="758a0-302">R-kód elosztása több csomópontra</span><span class="sxs-lookup"><span data-stu-id="758a0-302">Distribute R code to multiple nodes</span></span>

<span data-ttu-id="758a0-303">Az R Serverrel könnyedén futtathatja a meglévő R-kódokat a fürt több csomópontján az `rxExec` használatával.</span><span class="sxs-lookup"><span data-stu-id="758a0-303">With R Server, you can easily take existing R code and run it across multiple nodes in the cluster by using `rxExec`.</span></span> <span data-ttu-id="758a0-304">Ez a függvény akkor hasznos, amikor paraméteres frissítést vagy szimulációkat végez.</span><span class="sxs-lookup"><span data-stu-id="758a0-304">This function is useful when doing a parameter sweep or simulations.</span></span> <span data-ttu-id="758a0-305">A következő kód az `rxExec` használatának példája:</span><span class="sxs-lookup"><span data-stu-id="758a0-305">The following code is an example of how to use `rxExec`:</span></span>

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

<span data-ttu-id="758a0-306">Ha továbbra is a Spark vagy a MapReduce környezetet használja, ez visszaadja azon munkavégző csomópontok csomópontnév értékét, amelyen a `(Sys.info()["nodename"])` kódot futtatta.</span><span class="sxs-lookup"><span data-stu-id="758a0-306">If you are still using the Spark or MapReduce context, this  command returns the nodename value for the worker nodes that the code `(Sys.info()["nodename"])` is run on.</span></span> <span data-ttu-id="758a0-307">Egy négy csomópontból álló fürtön például a következőhöz hasonló kimenetet kaphat:</span><span class="sxs-lookup"><span data-stu-id="758a0-307">For example, on a four node cluster, you expect to receive output similar to the following:</span></span>

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


## <a name="accessing-data-in-hive-and-parquet"></a><span data-ttu-id="758a0-308">Adatok elérése a Hive és a Parquet eszközökben</span><span class="sxs-lookup"><span data-stu-id="758a0-308">Accessing Data in Hive and Parquet</span></span>

<span data-ttu-id="758a0-309">Az R Server 9.1 és újabb verziókban elérhető funkciója lehetővé teszi az adatok közvetlen elérését a Hive és a Parquet eszközökben a Spark számítási környezet ScaleR-függvényei általi használatra.</span><span class="sxs-lookup"><span data-stu-id="758a0-309">A feature available in R Server 9.1 allows direct access to data in Hive and Parquet for use by ScaleR functions in the Spark compute context.</span></span> <span data-ttu-id="758a0-310">Ezek a képességek az RxHiveData és RxParquetData nevű új ScaleR adatforrás-függvényeken keresztül érhetők el, amelyek a Spark SQL-en keresztül töltenek adatokat közvetlenül a Spark DataFrame-be a ScaleR által végzett elemzéshez.</span><span class="sxs-lookup"><span data-stu-id="758a0-310">These capabilities are available through new ScaleR data source functions called RxHiveData and RxParquetData that work through use of Spark SQL to load data directly into a Spark DataFrame for analysis by ScaleR.</span></span>  

<span data-ttu-id="758a0-311">A következő kód tartalmazza az új függvények használatának néhány mintakódját:</span><span class="sxs-lookup"><span data-stu-id="758a0-311">The following code provides some sample code on use of the new functions:</span></span>

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

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


<span data-ttu-id="758a0-312">Ezen új függvények használatával kapcsolatos további információkat az R Server online súgójában találhat a `?RxHivedata` és a `?RxParquetData` parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="758a0-312">For additional info on use of these new functions see the online help in R Server through use of the `?RxHivedata` and `?RxParquetData` commands.</span></span>  


## <a name="install-additional-r-packages-on-the-edge-node"></a><span data-ttu-id="758a0-313">További R-csomagok telepítése az élcsomópontra</span><span class="sxs-lookup"><span data-stu-id="758a0-313">Install additional R packages on the edge node</span></span>

<span data-ttu-id="758a0-314">Ha további R csomagokat szeretne telepíteni az élcsomóponton, az `install.packages()` parancsot használhatja közvetlenül az R-konzolról, amikor SSH-n keresztül csatlakozik az élcsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="758a0-314">If you would like to install additional R packages on the edge node, you can use `install.packages()` directly from within the R console when connected to the edge node through SSH.</span></span> <span data-ttu-id="758a0-315">Ha azonban a fürt munkavégző csomópontjaira kell R csomagokat telepítenie, szkriptműveletet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="758a0-315">However, if you need to install R packages on the worker nodes of the cluster, you must use a Script Action.</span></span>

<span data-ttu-id="758a0-316">A szkriptműveletek olyan Bash-szkriptek, amelyekkel konfigurációs módosítások végezhetők a HDInsight-fürtön, vagy további szoftverek, például további R-csomagok telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="758a0-316">Script Actions are Bash scripts that are used to make configuration changes to the HDInsight cluster or to install additional software, such as additional R packages.</span></span> <span data-ttu-id="758a0-317">Ha szkriptművelettel szeretne további csomagokat telepíteni, használja a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="758a0-317">To install additional packages using a Script Action, use the following steps:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="758a0-318">A további R csomagok szkriptműveletekkel végzett telepítése csak a fürt létrehozása után használható.</span><span class="sxs-lookup"><span data-stu-id="758a0-318">Using Script Actions to install additional R packages can only be used after the cluster has been created.</span></span> <span data-ttu-id="758a0-319">Ne használja ezt az eljárást a fürt létrehozása során, mivel a szkript az R Server teljes telepítésétől és konfigurálásától függ.</span><span class="sxs-lookup"><span data-stu-id="758a0-319">Do not use this procedure during cluster creation, as the script relies on R Server being completely installed and configured.</span></span>
>
>

1. <span data-ttu-id="758a0-320">Az [Azure Portalon](https://portal.azure.com) válassza ki az R Servert a HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="758a0-320">From the [Azure portal](https://portal.azure.com), select your R Server on HDInsight cluster.</span></span>

2. <span data-ttu-id="758a0-321">A **Beállítások** panelen válassza a **Szkriptműveletek** elemet, majd az **Új küldése** elemet egy új szkriptművelet elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="758a0-321">From the **Settings** blade, select **Script Actions** and then **Submit New** to submit a new Script Action.</span></span>

   ![A Szkriptműveletek panel képe](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. <span data-ttu-id="758a0-323">A **Szkriptművelet küldése** panelen adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="758a0-323">From the **Submit script action** blade, provide the following information:</span></span>

   * <span data-ttu-id="758a0-324">**Név**: A szkriptet azonosító egyszerű név</span><span class="sxs-lookup"><span data-stu-id="758a0-324">**Name**: A friendly name to identify this script</span></span>

   * <span data-ttu-id="758a0-325">**Bash-szkript URI azonosítója**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span><span class="sxs-lookup"><span data-stu-id="758a0-325">**Bash script URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span></span>

   * <span data-ttu-id="758a0-326">**Fej**: Ez az elem **ne legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="758a0-326">**Head**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="758a0-327">**Feldolgozó**: Ez az elem **legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="758a0-327">**Worker**: This item should be **checked**</span></span>

   * <span data-ttu-id="758a0-328">**Szegélycsomópontok**: Ez az elem **ne legyen bejelölve**.</span><span class="sxs-lookup"><span data-stu-id="758a0-328">**Edge nodes**: This item should be **unchecked**.</span></span>

   * <span data-ttu-id="758a0-329">**Zookeeper**: Ez az elem **ne legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="758a0-329">**Zookeeper**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="758a0-330">**Paraméterek**: A telepíteni kívánt R csomagok.</span><span class="sxs-lookup"><span data-stu-id="758a0-330">**Parameters**: The R packages to be installed.</span></span> <span data-ttu-id="758a0-331">Például: `bitops stringr arules`</span><span class="sxs-lookup"><span data-stu-id="758a0-331">For example, `bitops stringr arules`</span></span>

   * <span data-ttu-id="758a0-332">**Ha megőrzi a...**: Ez az elem **legyen bejelölve**</span><span class="sxs-lookup"><span data-stu-id="758a0-332">**Persist this script...**: This item should be **Checked**</span></span>  

   > [!NOTE]
   > 1. <span data-ttu-id="758a0-333">Alapértelmezés szerint az összes R csomag a telepített R Server verziójának megfelelő Microsoft MRAN tár-pillanatfelvételéből van telepítve.</span><span class="sxs-lookup"><span data-stu-id="758a0-333">By default, all R packages are installed from a snapshot of the Microsoft MRAN repository consistent with the version of R Server that has been installed.</span></span> <span data-ttu-id="758a0-334">Ha a csomagok újabb verzióját szeretné telepíteni, akkor van némi inkompatibilitási kockázat.</span><span class="sxs-lookup"><span data-stu-id="758a0-334">If you want to install newer versions of packages, then there is some risk of incompatibility.</span></span> <span data-ttu-id="758a0-335">Ez a fajta telepítés azonban lehetséges, ha a `useCRAN` parancsot adja meg a csomaglista első elemeként, például: `useCRAN bitops, stringr, arules`.</span><span class="sxs-lookup"><span data-stu-id="758a0-335">However this kind of install is possible by specifying `useCRAN` as the first element of the package list, for example `useCRAN bitops, stringr, arules`.</span></span>  
   > 2. <span data-ttu-id="758a0-336">Néhány R csomag további Linux rendszerű könyvtárakat igényel.</span><span class="sxs-lookup"><span data-stu-id="758a0-336">Some R packages require additional Linux system libraries.</span></span> <span data-ttu-id="758a0-337">A kényelem érdekében előre telepítettük a 100 legnépszerűbb R csomag szükséges függőségeit.</span><span class="sxs-lookup"><span data-stu-id="758a0-337">For convenience, we have pre-installed the dependencies needed by the top 100 most popular R packages.</span></span> <span data-ttu-id="758a0-338">Ha azonban a telepített R csomag(ok) további könyvtárakat telepítését igényli(k), akkor le kell töltenie az itt használt alapvető szkriptet, és lépéseket kell hozzáadnia a rendszerkönyvtárak telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="758a0-338">However, if the R package(s) you install require libraries beyond these then you must download the base script used here and add steps to install the system libraries.</span></span> <span data-ttu-id="758a0-339">Ezután fel kell töltenie a módosított szkriptet egy nyilvános blob-tárolóba az Azure Storage-ben, és a módosított szkripttel kell telepítenie a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="758a0-339">You must then upload the modified script to a public blob container in Azure storage and use the modified script to install the packages.</span></span>
   >    <span data-ttu-id="758a0-340">A szkriptműveletek fejlesztésével kapcsolatos további információért lásd: [Szkriptművelet fejlesztése](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="758a0-340">For more information on developing Script Actions, see [Script Action development](hdinsight-hadoop-script-actions-linux.md).</span></span>  
   >
   >

   ![Szkriptműveletek hozzáadása](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. <span data-ttu-id="758a0-342">Válassza a **Létrehozás** lehetőséget a szkript futtatásához.</span><span class="sxs-lookup"><span data-stu-id="758a0-342">Select **Create** to run the script.</span></span> <span data-ttu-id="758a0-343">A szkript befejezése után az R csomagok elérhetők az összes munkavégző csomóponton.</span><span class="sxs-lookup"><span data-stu-id="758a0-343">Once the script completes, the R packages are available on all worker nodes.</span></span>


## <a name="using-microsoft-r-server-operationalization"></a><span data-ttu-id="758a0-344">A Microsoft R Server operacionalizálás használata</span><span class="sxs-lookup"><span data-stu-id="758a0-344">Using Microsoft R Server Operationalization</span></span>

<span data-ttu-id="758a0-345">Amikor elkészült az adatmodellezés, működőképessé teheti a modellt, hogy előrejelzéseket végezzen.</span><span class="sxs-lookup"><span data-stu-id="758a0-345">When your data modeling is complete, you can operationalize the model to make predictions.</span></span> <span data-ttu-id="758a0-346">A Microsoft R Server működőképessé tételhez való konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="758a0-346">To configure for Microsoft R Server operationalization, perform the following steps:</span></span>

<span data-ttu-id="758a0-347">Először jelentkezzen be SSH-n keresztül az élcsomópontba.</span><span class="sxs-lookup"><span data-stu-id="758a0-347">First, ssh into the Edge node.</span></span> <span data-ttu-id="758a0-348">Például:</span><span class="sxs-lookup"><span data-stu-id="758a0-348">For example,</span></span> 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="758a0-349">Az SSH használata után lépjen a megfelelő verzió könyvtárára, és használja a következő sudo dotnet dll-t:</span><span class="sxs-lookup"><span data-stu-id="758a0-349">After using ssh, change directory for the relevant version and sudo the dotnet dll:</span></span> 

- <span data-ttu-id="758a0-350">Microsoft R Server 9.1 esetén:</span><span class="sxs-lookup"><span data-stu-id="758a0-350">For Microsoft R Server 9.1:</span></span>

    <span data-ttu-id="758a0-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="758a0-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span></span>

- <span data-ttu-id="758a0-352">Microsoft R Server 9.0 esetén:</span><span class="sxs-lookup"><span data-stu-id="758a0-352">For Microsoft R Server 9.0:</span></span>

    <span data-ttu-id="758a0-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="758a0-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span></span>

<span data-ttu-id="758a0-354">Ha a Microsoft R Server működőképessé tételét egy beépített konfigurációban szeretné konfigurálni, tegye a következőt:</span><span class="sxs-lookup"><span data-stu-id="758a0-354">To configure Microsoft R Server operationalization with a One-box configuration do the following:</span></span>

1. <span data-ttu-id="758a0-355">Válassza a „Configure R Server for Operationalization” (Az R Server konfigurálása a Működőképessé tétel művelethez) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="758a0-355">Select “Configure R Server for Operationalization”</span></span>
2. <span data-ttu-id="758a0-356">Válassza az „A.</span><span class="sxs-lookup"><span data-stu-id="758a0-356">Select “A.</span></span> <span data-ttu-id="758a0-357">One-box (web + compute nodes)” (Beépített (web + számítási csomópontok)) elemet</span><span class="sxs-lookup"><span data-stu-id="758a0-357">One-box (web + compute nodes)”</span></span>
3. <span data-ttu-id="758a0-358">Írja be a **rendszergazdai** felhasználó jelszót</span><span class="sxs-lookup"><span data-stu-id="758a0-358">Enter a password for the **admin** user</span></span>

![one box op](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

<span data-ttu-id="758a0-360">Választható lépésként diagnosztikai ellenőrzéseket végezhet az alább látható diagnosztikai tesztelés futtatásával:</span><span class="sxs-lookup"><span data-stu-id="758a0-360">As an optional step you can perform Diagnostic checks by running a diagnostic test as follows:</span></span>

1. <span data-ttu-id="758a0-361">Válassza a „6.</span><span class="sxs-lookup"><span data-stu-id="758a0-361">Select “6.</span></span> <span data-ttu-id="758a0-362">Run diagnostic tests” (Diagnosztikai tesztek futtatása) elemet</span><span class="sxs-lookup"><span data-stu-id="758a0-362">Run diagnostic tests”</span></span>
2. <span data-ttu-id="758a0-363">Válassza az „A.</span><span class="sxs-lookup"><span data-stu-id="758a0-363">Select “A.</span></span> <span data-ttu-id="758a0-364">Test configuration” (Tesztkonfiguráció) elemet</span><span class="sxs-lookup"><span data-stu-id="758a0-364">Test configuration”</span></span>
3. <span data-ttu-id="758a0-365">Írja be a Username = „admin” parancsot, valamint a jelszót az előző konfigurációs lépésből</span><span class="sxs-lookup"><span data-stu-id="758a0-365">Enter Username = “admin” and password from previous configuration step</span></span>
4. <span data-ttu-id="758a0-366">Erősítse meg az Overall Health = pass parancsot</span><span class="sxs-lookup"><span data-stu-id="758a0-366">Confirm Overall Health = pass</span></span>
5. <span data-ttu-id="758a0-367">Lépjen ki az admin segédprogramból</span><span class="sxs-lookup"><span data-stu-id="758a0-367">Exit the Admin Utility</span></span>
6. <span data-ttu-id="758a0-368">Lépjen ki az SSH-ból</span><span class="sxs-lookup"><span data-stu-id="758a0-368">Exit SSH</span></span>

![Diagnostic for op](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
><span data-ttu-id="758a0-370">**Hosszú késések a webszolgáltatások Sparkban történő felhasználásakor**</span><span class="sxs-lookup"><span data-stu-id="758a0-370">**Long delays when consuming web service on Spark**</span></span>
>
><span data-ttu-id="758a0-371">Ha hosszú késleltetést tapasztal, amikor egy Spark számítási környezetben mrsdeploy függvényekkel létrehozott webszolgáltatást próbál felhasználni, előfordulhat, hogy hozzá kell adnia néhány hiányzó mappát.</span><span class="sxs-lookup"><span data-stu-id="758a0-371">If you encounter long delays when trying to consume a web service created with mrsdeploy functions in a Spark compute context, you may need to add some missing folders.</span></span> <span data-ttu-id="758a0-372">A Spark-alkalmazás egy *rserve2* nevű felhasználóhoz tartozik, ha egy mrsdeploy függvényeket használó webszolgáltatásból hívja meg.</span><span class="sxs-lookup"><span data-stu-id="758a0-372">The Spark application belongs to a user called '*rserve2*' whenever it is invoked from a web service using mrsdeploy functions.</span></span> <span data-ttu-id="758a0-373">Megkerülő megoldás a problémára:</span><span class="sxs-lookup"><span data-stu-id="758a0-373">To work around this issue:</span></span>

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


<span data-ttu-id="758a0-374">Az operacionalizálás konfigurációja ezzel befejeződött.</span><span class="sxs-lookup"><span data-stu-id="758a0-374">At this stage, the configuration for Operationalization is complete.</span></span> <span data-ttu-id="758a0-375">Most már használhatja az „mrsdeploy” csomagot RClientben, hogy kapcsolódhasson az élcsomóponti operacionalizáláshoz, és elkezdhet alkalmazni olyan szolgáltatásokat, mint a [távoli végrehajtás](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) és [webszolgáltatás](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span><span class="sxs-lookup"><span data-stu-id="758a0-375">Now you can use the ‘mrsdeploy’ package on your RClient to connect to the Operationalization on edge node and start using its features like [remote execution](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) and [web-services](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span></span> <span data-ttu-id="758a0-376">Attól függően, hogy fürt virtuális hálózaton van-e beállítva, szükség lehet porttovábbító bújtatás kialakítására SSH-bejelentkezésen keresztül.</span><span class="sxs-lookup"><span data-stu-id="758a0-376">Depending on whether your cluster is set up on a virtual network or not, you may need to set up port forward tunneling through SSH login.</span></span> <span data-ttu-id="758a0-377">Az alábbi szakaszok ismertetik, hogyan állíthatja be ezt az alagutat.</span><span class="sxs-lookup"><span data-stu-id="758a0-377">The following sections explain how to set up this tunnel.</span></span>

### <a name="rserver-cluster-on-virtual-network"></a><span data-ttu-id="758a0-378">Az RServer fürt virtuális hálózaton van</span><span class="sxs-lookup"><span data-stu-id="758a0-378">RServer Cluster on virtual network</span></span>

<span data-ttu-id="758a0-379">Bizonyosodjon meg róla, hogy engedélyezett a forgalom az 12800-as porton az élcsomópont felé.</span><span class="sxs-lookup"><span data-stu-id="758a0-379">Make sure you allow traffic through port 12800 to the edge node.</span></span> <span data-ttu-id="758a0-380">Így az élcsomópont használatával kapcsolódhat az operacionalizálási szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="758a0-380">That way, you can use the edge node to connect to the Operationalization feature.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


<span data-ttu-id="758a0-381">Ha a `remoteLogin()` metódus nem tud kapcsolódni az élcsomóponthoz, de SSH-n be tud jelentkezni az élcsomópontba, győződjön meg róla, hogy a szabály, amely engedélyezi a forgalmat az 12800-as porton, megfelelően van-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="758a0-381">If the `remoteLogin()` cannot connect to the edge node, but you can SSH to the edge node, then you need to verify whether the rule to allow traffic on port 12800 has been set properly or not.</span></span> <span data-ttu-id="758a0-382">Ha a probléma továbbra is jelentkezik, egy másik megoldás segítségével is beállíthat porttovábbító alagutat az SSH-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="758a0-382">If you continue to face the issue, you can work around it by setting up port forward tunneling through SSH.</span></span> <span data-ttu-id="758a0-383">Útmutatásért lásd a következő szakaszt.</span><span class="sxs-lookup"><span data-stu-id="758a0-383">For instructions, see the following section.</span></span>

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a><span data-ttu-id="758a0-384">Az RServer fürt nem virtuális hálózaton van beállítva</span><span class="sxs-lookup"><span data-stu-id="758a0-384">RServer Cluster not set up on virtual network</span></span>

<span data-ttu-id="758a0-385">Ha a fürt nem a virtuális hálózaton van beállítva vagy problémás a kapcsolódás a virtuális hálózaton keresztül, akkor használhatja az SSH porttovábbító alagutat:</span><span class="sxs-lookup"><span data-stu-id="758a0-385">If your cluster is not set up on vnet or if you are having troubles with connectivity through vnet, you can use SSH port forward tunneling:</span></span>

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="758a0-386">A Putty szoftveren is beállítható.</span><span class="sxs-lookup"><span data-stu-id="758a0-386">On Putty, you can set it up as well.</span></span>

![putty ssh kapcsolat](./media/hdinsight-hadoop-r-server-get-started/putty.png)

<span data-ttu-id="758a0-388">Ha az SSH-munkamenet aktív, a rendszer számítógépe 12800-as portjáról az élcsomópont 12800-as portjára továbbítja a forgalmat az SSH-munkameneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="758a0-388">Once your SSH session is active, the traffic from your machine’s port 12800 is forwarded to the edge node’s port 12800 through SSH session.</span></span> <span data-ttu-id="758a0-389">A `127.0.0.1:12800` címet használja a `remoteLogin()` metódusban.</span><span class="sxs-lookup"><span data-stu-id="758a0-389">Make sure you use `127.0.0.1:12800` in your `remoteLogin()` method.</span></span> <span data-ttu-id="758a0-390">Ezzel a porttovábbításon keresztül jelentkezik be az élcsomóponti operacionalizálásra.</span><span class="sxs-lookup"><span data-stu-id="758a0-390">This logs in to the edge node’s operationalization through port forwarding.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a><span data-ttu-id="758a0-391">Hogyan méretezhetők a Microsoft R Server operacionalizálási számítási csomópontjai a HDInsight feldolgozó csomópontjain?</span><span class="sxs-lookup"><span data-stu-id="758a0-391">How to scale Microsoft R Server Operationalization compute nodes on HDInsight worker nodes</span></span>

### <a name="decommission-the-worker-nodes"></a><span data-ttu-id="758a0-392">A feldolgozó csomópont(ok) leszerelése</span><span class="sxs-lookup"><span data-stu-id="758a0-392">Decommission the worker node(s)</span></span>

<span data-ttu-id="758a0-393">A Microsoft R Servert jelenleg nem a Yarnon keresztül kezeli a rendszer.</span><span class="sxs-lookup"><span data-stu-id="758a0-393">Microsoft R Server is currently not managed through Yarn.</span></span> <span data-ttu-id="758a0-394">Ha a feldolgozó csomópontokat nem szereli le, a Yarn Resource Manager nem a várakozásoknak megfelelően fog működni, mert nem fogja látni a kiszolgáló által felhasznált erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="758a0-394">If the worker nodes are not decommissioned, the Yarn Resource Manager will not work as expected because it will not be aware of the resources being taken up by the server.</span></span> <span data-ttu-id="758a0-395">Ennek a helyzetnek az elkerülésére javasoljuk a feldolgozó csomópontok leszerelését a számítási csomópontok horizontális felskálázása előtt.</span><span class="sxs-lookup"><span data-stu-id="758a0-395">In order to avoid this situation, we recommend decommissioning the worker nodes before you scale out the compute nodes.</span></span>

<span data-ttu-id="758a0-396">A feldolgozó csomópontok leszerelésének lépései:</span><span class="sxs-lookup"><span data-stu-id="758a0-396">Steps to decommissioning worker nodes:</span></span>

* <span data-ttu-id="758a0-397">Jelentkezzen be a HDI-fürt Ambari konzoljába, és kattintson a „Hosts” (Gazdagépek) lapra.</span><span class="sxs-lookup"><span data-stu-id="758a0-397">Log in to HDI cluster's Ambari console and click on "hosts" tab</span></span>
* <span data-ttu-id="758a0-398">Jelölje ki a leszerelendő feldolgozó csomópontokat, és kattintson az „Actions” (Műveletek) > „Selected Hosts” (Kiválasztott gazdagépek) > „Hosts” (Gazdagépek) panelen a „Turn ON Maintenance Mode” (Karbantartási mód bekapcsolása) elemre.</span><span class="sxs-lookup"><span data-stu-id="758a0-398">Select worker nodes (to be decommissioned), Click on "Actions" > "Selected Hosts" > "Hosts" > click on "Turn ON Maintenance Mode".</span></span> <span data-ttu-id="758a0-399">A következő képen például a wn3 és a wn4 pontokat választottuk ki leszerelésre.</span><span class="sxs-lookup"><span data-stu-id="758a0-399">For example, in the following image we have selected wn3 and wn4 to decommission.</span></span>  

   ![feldolgozó csomópont(ok) leszerelése](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* <span data-ttu-id="758a0-401">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **DataNodes** (AdatCsomópontok) elemet > kattintson a **Decommission** (Leszerelés) gombra</span><span class="sxs-lookup"><span data-stu-id="758a0-401">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Decommission**</span></span>
* <span data-ttu-id="758a0-402">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **NodeManagers** (CsomópontKezelők) elemet > kattintson a **Decommission** (Leszerelés) gombra</span><span class="sxs-lookup"><span data-stu-id="758a0-402">Select **Actions** > **Selected Hosts** > **NodeManagers** > click **Decommission**</span></span>
* <span data-ttu-id="758a0-403">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **DataNodes** (AdatCsomópontok) elemet > kattintson a **Stop** gombra</span><span class="sxs-lookup"><span data-stu-id="758a0-403">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Stop**</span></span>
* <span data-ttu-id="758a0-404">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott csomópontok) > **NodeManagers** (CsomópontKezelők) elemet > kattintson a **Stop** gombra</span><span class="sxs-lookup"><span data-stu-id="758a0-404">Select **Actions** > **Selected Hosts** > **NodeManagers** > click on **Stop**</span></span>
* <span data-ttu-id="758a0-405">Válassza az **Actions**(Műveletek) > **Selected Hosts**(Kiválasztott gazdagépek) > **Hosts** (Gazdagépek) elemet > kattintson a **Stop All Components** (Összes összetevő leállítása) gombra</span><span class="sxs-lookup"><span data-stu-id="758a0-405">Select **Actions** > **Selected Hosts** > **Hosts** > click **Stop All Components**</span></span>
* <span data-ttu-id="758a0-406">Szüntesse meg a feldolgozó csomópontok kijelölését, és jelölje ki az élcsomópontokat</span><span class="sxs-lookup"><span data-stu-id="758a0-406">Unselect the worker nodes and select the head nodes</span></span>
* <span data-ttu-id="758a0-407">Válassza az **Actions**(Műveletek) > **Selected Hosts** (Kiválasztott gazdagépek) > "**Hosts**(Gazdagépek) > **Restart All Components**(Összes gazdagép újraindítása) elemet</span><span class="sxs-lookup"><span data-stu-id="758a0-407">Select **Actions** > **Selected Hosts** > "**Hosts** > **Restart All Components**</span></span>

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a><span data-ttu-id="758a0-408">Számítási csomópontok konfigurálása az összes leszerelt feldolgozó csomóponton</span><span class="sxs-lookup"><span data-stu-id="758a0-408">Configure Compute nodes on each decommissioned worker node(s)</span></span>

1. <span data-ttu-id="758a0-409">Jelentkezzen be SSH-n keresztül minden egyes leszerelt feldolgozó csomópontba.</span><span class="sxs-lookup"><span data-stu-id="758a0-409">SSH into each decommissioned worker node.</span></span>
2. <span data-ttu-id="758a0-410">Futtassa az admin segédprogramot a következő paranccsal: `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span><span class="sxs-lookup"><span data-stu-id="758a0-410">Run admin utility using `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span></span>
3. <span data-ttu-id="758a0-411">Adja meg az „1” értéket „Configure R Server for Operationalization” (Az R Server konfigurálása a Működőképessé tétel művelethez) lehetőség kijelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="758a0-411">Enter "1" to select option "Configure R Server for Operationalization".</span></span>
4. <span data-ttu-id="758a0-412">Írja be a „c” karaktert a „C.</span><span class="sxs-lookup"><span data-stu-id="758a0-412">Enter "c" to select option "C.</span></span> <span data-ttu-id="758a0-413">Compute node” (Számítási csomópont) lehetőség kijelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="758a0-413">Compute node".</span></span> <span data-ttu-id="758a0-414">Ez konfigurálja a számítási csomópontot a feldolgozó csomóponton.</span><span class="sxs-lookup"><span data-stu-id="758a0-414">This configures the compute node on the worker node.</span></span>
5. <span data-ttu-id="758a0-415">Lépjen ki az admin segédprogramból.</span><span class="sxs-lookup"><span data-stu-id="758a0-415">Exit the Admin Utility.</span></span>

### <a name="add-compute-nodes-details-on-web-node"></a><span data-ttu-id="758a0-416">Számítási csomópontok részleteinek megadása a Web csomóponton</span><span class="sxs-lookup"><span data-stu-id="758a0-416">Add compute nodes details on Web Node</span></span>

<span data-ttu-id="758a0-417">Ha minden leszerelt feldolgozó csomópontot konfigurált a számítási csomópont futtatására, térjen vissza az élcsomóponthoz, és adja hozzá a leszerelt feldolgozó csomópontok IP címét a Microsoft R Server webcsomópontjának konfigurációjában:</span><span class="sxs-lookup"><span data-stu-id="758a0-417">Once all decommissioned worker nodes have been configured to run compute node, come back on the edge node and add decommissioned worker nodes' IP addresses in the Microsoft R Server web node's configuration:</span></span>

* <span data-ttu-id="758a0-418">Jelentkezzen be SSH-n keresztül az élcsomópontba.</span><span class="sxs-lookup"><span data-stu-id="758a0-418">SSH into the edge node.</span></span>
* <span data-ttu-id="758a0-419">Futtassa az `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` parancsot.</span><span class="sxs-lookup"><span data-stu-id="758a0-419">Run `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span></span>
* <span data-ttu-id="758a0-420">Keresse meg az „URIs” szakaszt, és adja hozzá a feldolgozó csomópontok IP-címét és portrészleteit.</span><span class="sxs-lookup"><span data-stu-id="758a0-420">Look for the "URIs" section, and add worker node's IP and port details.</span></span>

    ![feldolgozó csomópont(ok) leszerelési parancssora](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a><span data-ttu-id="758a0-422">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="758a0-422">Troubleshoot</span></span>

<span data-ttu-id="758a0-423">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="758a0-423">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>


## <a name="next-steps"></a><span data-ttu-id="758a0-424">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="758a0-424">Next steps</span></span>

<span data-ttu-id="758a0-425">Mostanra biztosan megértette, hogyan kell R Servert tartalmazó HDInsight-fürtöt létrehozni, és tisztában van az R-konzol SSH-munkamenetből történő használatának alapjaival.</span><span class="sxs-lookup"><span data-stu-id="758a0-425">Now you should understand how to create a new HDInsight cluster that includes the R Server and the basics of using the R console from an SSH session.</span></span> <span data-ttu-id="758a0-426">A következő témakörök az R Server HDInsighton történő kezelésének és az azzal történő munkavégzésnek egyéb módjait ismertetik:</span><span class="sxs-lookup"><span data-stu-id="758a0-426">The following topics explain other ways of managing and working with R Server on HDInsight:</span></span>

* [<span data-ttu-id="758a0-427">RStudio Server hozzáadása a HDInsight szolgáltatáshoz (ha a fürt létrehozása közben nem telepítette)</span><span class="sxs-lookup"><span data-stu-id="758a0-427">Add RStudio Server to HDInsight (if not installed during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="758a0-428">Számítási környezeti beállítások a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="758a0-428">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="758a0-429">Azure Storage lehetőségek a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="758a0-429">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)
