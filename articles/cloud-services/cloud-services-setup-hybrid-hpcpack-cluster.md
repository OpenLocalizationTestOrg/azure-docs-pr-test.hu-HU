---
title: "Az Azure-ban hibrid HPC Pack fürt beállítása |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a Microsoft HPC Pack és az Azure kis, hibrid nagy teljesítményű számítástechnikai (HPC) fürt beállítása"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: f6dc9657e64160be1e68a7356863b53131e9b3c3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a><span data-ttu-id="2bbd8-103">A hibrid nagy teljesítményű számítástechnikai igény Azure számítási csomópontok és a Microsoft HPC Pack (HPC) fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="2bbd8-103">Set up a hybrid high performance computing (HPC) cluster with Microsoft HPC Pack and on-demand Azure compute nodes</span></span>
<span data-ttu-id="2bbd8-104">A Microsoft HPC Pack 2012 R2 és az Azure használatával állítson be egy kisméretű, hibrid nagyteljesítményű számítástechnikai (HPC) fürt.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-104">Use Microsoft HPC Pack 2012 R2 and Azure to set up a small, hybrid high performance computing (HPC) cluster.</span></span> <span data-ttu-id="2bbd8-105">A fürt látható az ebben a cikkben egy helyszíni HPC Pack átjárócsomópont tartalmaz, és néhány számítási csomópont telepítése egy Azure-ban igény a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-105">The cluster shown in this article consists of an on-premises HPC Pack head node and some compute nodes you deploy on-demand in an Azure cloud service.</span></span> <span data-ttu-id="2bbd8-106">Ezt követően futtathatja a számítási feladatok, a hibrid fürtön.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-106">You can then run compute jobs on the hybrid cluster.</span></span>

![Hibrid HPC-fürt][Overview] 

<span data-ttu-id="2bbd8-108">Ez az oktatóanyag egy módszert használja, más néven a fürt "kapacitásnövelés a felhőbe, a" számítási igényű alkalmazások futtatásához méretezhető, igény szerinti Azure-erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-108">This tutorial shows one approach, sometimes called cluster "burst to the cloud," to use scalable, on-demand Azure resources to run compute-intensive applications.</span></span>

<span data-ttu-id="2bbd8-109">Ez az oktatóanyag azt feltételezi, hogy a számítási fürtök vagy HPC Pack nincs korábbi tapasztalata kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-109">This tutorial assumes no prior experience with compute clusters or HPC Pack.</span></span> <span data-ttu-id="2bbd8-110">Az csak segítségével rendelheti hozzá egy hibrid számítási fürt gyors bemutatási célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-110">It is intended only to help you deploy a hybrid compute cluster quickly for demonstration purposes.</span></span> <span data-ttu-id="2bbd8-111">Szempontjait és lépéseit, nagyobb léptékű termelési környezetben hibrid HPC Pack fürt központi telepítése, vagy HPC Pack 2016 használ, tekintse meg a [részletes útmutatást](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-111">For considerations and steps to deploy a hybrid HPC Pack cluster at greater scale in a production environment, or to use HPC Pack 2016, see the [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span> <span data-ttu-id="2bbd8-112">HPC Pack egyéb forgatókönyvek, beleértve a fürtöt tartalmazó környezetben az Azure virtuális gépeken automatikus című [HPC-fürt beállítások Microsoft HPC Pack az Azure-ban](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-112">For other scenarios with HPC Pack, including automated cluster deployment in Azure virtual machines, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bbd8-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2bbd8-113">Prerequisites</span></span>
* <span data-ttu-id="2bbd8-114">**Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-114">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>
* <span data-ttu-id="2bbd8-115">**Egy Windows Server 2012 R2 vagy Windows Server 2012 rendszert futtató helyszíni számítógép** -ezt a számítógépet használja, a HPC-fürt átjárócsomópontjához.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-115">**An on-premises computer running Windows Server 2012 R2 or Windows Server 2012** - Use this computer as the head node of the HPC cluster.</span></span> <span data-ttu-id="2bbd8-116">Ha már nem futtat Windows Server, töltse le és telepítse az [próbaverzió](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-116">If you aren't already running Windows Server, you can download and install an [evaluation version](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span></span>
  
  * <span data-ttu-id="2bbd8-117">A számítógép Active Directory-tartományhoz kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-117">The computer must be joined to an Active Directory domain.</span></span> <span data-ttu-id="2bbd8-118">Tesztelési célokra konfigurálhatja az átjárócsomópont számítógép tartományvezérlőként működik.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-118">For test purposes, you can configure the head node computer as a domain controller.</span></span> <span data-ttu-id="2bbd8-119">Vegye fel az Active Directory tartományi szolgáltatások szerepkört, és léptesse elő tartományvezérlővé a átjárócsomópont számítógépet, a Windows Server dokumentációjában találhat.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-119">To add the Active Directory Domain Services server role and promote the head node computer as a domain controller, see the documentation for Windows Server.</span></span>
  * <span data-ttu-id="2bbd8-120">HPC Pack támogatásához az operációs rendszer telepítenie kell az ezeken a nyelveken egyik: angol, japán vagy kínai (egyszerűsített).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-120">To support HPC Pack, the operating system must be installed in one of these languages: English, Japanese, or Chinese (Simplified).</span></span>
  * <span data-ttu-id="2bbd8-121">Győződjön meg arról, hogy a fontos és kritikus frissítések telepítve vannak-e.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-121">Verify that important and critical updates are installed.</span></span>
* <span data-ttu-id="2bbd8-122">**HPC Pack 2012 R2** - [letöltése](http://go.microsoft.com/fwlink/p/?linkid=328024) a telepítési csomag ingyenesen elérhető legfrissebb, és másolja a fájlokat a head csomópont-számítógépén.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-122">**HPC Pack 2012 R2** - [Download](http://go.microsoft.com/fwlink/p/?linkid=328024) the installation package for the latest version free of charge and copy the files to the head node computer.</span></span> <span data-ttu-id="2bbd8-123">Válassza ki a telepítési fájlok a Windows Server telepítési nyelvével azonos nyelven.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-123">Choose installation files in the same language as your installation of Windows Server.</span></span>

    >[!NOTE]
    > <span data-ttu-id="2bbd8-124">Ha szeretné használni a HPC Pack 2016 helyett HPC Pack 2012 R2, további konfigurációra van szükség.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-124">If you want to use HPC Pack 2016 instead of HPC Pack 2012 R2, additional configuration is needed.</span></span> <span data-ttu-id="2bbd8-125">Tekintse meg a [részletes útmutatást](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-125">See the [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
    > 
* <span data-ttu-id="2bbd8-126">**Tartományi fiók** -ennek a fióknak helyi rendszergazdai engedélyekkel rendelkező átjárócsomópontjához HPC csomag telepítéséhez be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-126">**Domain account** - This account must be configured with local Administrator permissions on the head node to install HPC Pack.</span></span>
* <span data-ttu-id="2bbd8-127">**TCP-kapcsolatot a 443-as porton** Azure központi csomópontjából.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-127">**TCP connectivity on port 443** from the head node to Azure.</span></span>

## <a name="install-hpc-pack-on-the-head-node"></a><span data-ttu-id="2bbd8-128">HPC Pack telepítését az átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="2bbd8-128">Install HPC Pack on the head node</span></span>
<span data-ttu-id="2bbd8-129">Először telepítenie kell a Microsoft HPC Pack a helyszíni Windows Server rendszert futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-129">You first install Microsoft HPC Pack on your on-premises computer running Windows Server.</span></span> <span data-ttu-id="2bbd8-130">Ez a számítógép elérhetővé válik a fürt átjárócsomópontjához.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-130">This computer becomes the head node of the cluster.</span></span>

1. <span data-ttu-id="2bbd8-131">Jelentkezzen be az átjárócsomóponthoz, amely helyi rendszergazdai engedélyekkel rendelkezik tartományi fiókkal.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-131">Log on to the head node by using a domain account that has local Administrator permissions.</span></span>

2. <span data-ttu-id="2bbd8-132">A HPC Pack telepítési varázsló elindításához futtassa a Setup.exe a HPC Pack fájlokból.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-132">Start the HPC Pack Installation Wizard by running Setup.exe from the HPC Pack installation files.</span></span>

3. <span data-ttu-id="2bbd8-133">Az a **HPC Pack 2012 R2 telepítő** kattintson **új telepítés vagy az új szolgáltatások hozzáadása egy meglévő telepítéshez**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-133">On the **HPC Pack 2012 R2 Setup** screen, click **New installation or add new features to an existing installation**.</span></span>

    ![HPC Pack 2012 telepítése][install_hpc1]

4. <span data-ttu-id="2bbd8-135">Az a **Microsoft szoftver felhasználói oldal**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-135">On the **Microsoft Software User Agreement page**, click **Next**.</span></span>

5. <span data-ttu-id="2bbd8-136">Az a **telepítés típusának kiválasztása** kattintson **hozzon létre egy új HPC-fürtöt hozzon létre egy átjárócsomóponttal**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-136">On the **Select Installation Type** page, click **Create a new HPC cluster by creating a head node**, and then click **Next**.</span></span>

6. <span data-ttu-id="2bbd8-137">A varázsló futtat több telepítés előtti vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-137">The wizard runs several pre-installation tests.</span></span> <span data-ttu-id="2bbd8-138">Kattintson a **következő** a a **telepítési szabályokat** lapon, ha az összes teszt sikeresen lezajlott.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-138">Click **Next** on the **Installation Rules** page if all tests pass.</span></span> <span data-ttu-id="2bbd8-139">Ellenkező esetben tekintse át a megadott adatokat, és a szükséges módosításokat a környezetben.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-139">Otherwise, review the information provided and make any necessary changes in your environment.</span></span> <span data-ttu-id="2bbd8-140">Majd futtassa újból a vizsgálatot, vagy szükség esetén a telepítési varázsló ismételt futtatása.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-140">Then run the tests again or if necessary start the Installation Wizard again.</span></span>
7. <span data-ttu-id="2bbd8-141">Az a **HPC DB konfigurációs** lapon, győződjön meg arról, hogy **Átjárócsomópont** van kiválasztva a HPC-adatbázisok, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-141">On the **HPC DB Configuration** page, make sure **Head Node** is selected for all HPC databases, and then click **Next**.</span></span> 

    ![Adatbázis-konfiguráció][install_hpc4]

8. <span data-ttu-id="2bbd8-143">Fogadja el az alapértelmezett beállításokat a varázsló többi lapja.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-143">Accept default selections on the remaining pages of the wizard.</span></span> <span data-ttu-id="2bbd8-144">Az a **szükséges összetevők telepítése** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-144">On the **Install Required Components** page, click **Install**.</span></span>
   
    ![Telepítés][install_hpc6]

9. <span data-ttu-id="2bbd8-146">A telepítés befejezése után törölje a jelet **indítsa el a HPC Cluster Manager** majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-146">After the installation completes, uncheck **Start HPC Cluster Manager** and then click **Finish**.</span></span> <span data-ttu-id="2bbd8-147">(A start HPC Cluster Manager egy későbbi lépésben.)</span><span class="sxs-lookup"><span data-stu-id="2bbd8-147">(You start HPC Cluster Manager in a later step.)</span></span>
   
    ![Befejezés][install_hpc7]

## <a name="prepare-the-azure-subscription"></a><span data-ttu-id="2bbd8-149">Az Azure-előfizetés előkészítése</span><span class="sxs-lookup"><span data-stu-id="2bbd8-149">Prepare the Azure subscription</span></span>
<span data-ttu-id="2bbd8-150">Hajtsa végre a következő lépéseket a [Azure-portálon](https://portal.azure.com) Azure-előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-150">Perform the following steps in the [Azure portal](https://portal.azure.com) with your Azure subscription.</span></span> <span data-ttu-id="2bbd8-151">A lépések elvégzése után telepíthet a helyszíni átjárócsomópont az Azure csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-151">After completing these steps, you can deploy Azure nodes from the on-premises head node.</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="2bbd8-152">Továbbá jegyezze fel a Azure-előfizetése Azonosítóját, ami később szüksége.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-152">Also make a note of your Azure subscription ID, which you need later.</span></span> <span data-ttu-id="2bbd8-153">Keresse meg a Azonosítóját **előfizetések** a portálon.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-153">Find the ID in **Subscriptions** in the portal.</span></span>
  > 

### <a name="upload-the-default-management-certificate"></a><span data-ttu-id="2bbd8-154">Az alapértelmezett felügyeleti tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="2bbd8-154">Upload the default management certificate</span></span>
<span data-ttu-id="2bbd8-155">HPC Pack egy önaláírt tanúsítványt telepít az átjárócsomóponthoz, az alapértelmezett Microsoft HPC Azure felügyeleti tanúsítványt feltöltheti az Azure felügyeleti tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-155">HPC Pack installs a self-signed certificate on the head node, called the Default Microsoft HPC Azure Management certificate, that you can upload as an Azure management certificate.</span></span> <span data-ttu-id="2bbd8-156">Ez a tanúsítvány előírt tesztelési és a koncepció igazolása központi telepítésére, az átjárócsomópont és az Azure közötti kapcsolat biztonságos.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-156">This certificate is provided for testing and proof-of-concept deployments to secure the connection between the head node and Azure.</span></span>

1. <span data-ttu-id="2bbd8-157">Az átjárócsomópont a számítógépről, jelentkezzen be a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-157">From the head node computer, sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="2bbd8-158">Kattintson a **előfizetések** > *your_subscription_name*.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-158">Click **Subscriptions** > *your_subscription_name*.</span></span>

3. <span data-ttu-id="2bbd8-159">Kattintson a **felügyeleti tanúsítványok** > **feltöltése**.4.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-159">Click **Management certificates** > **Upload**.4.</span></span> <span data-ttu-id="2bbd8-160">Keresse meg a fájl C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer a head csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-160">Browse on the head node for the file C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer.</span></span> <span data-ttu-id="2bbd8-161">Kattintson a **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-161">Then, click **Upload**.</span></span>

   
<span data-ttu-id="2bbd8-162">A **alapértelmezett HPC Azure felügyeleti** tanúsítvány megjelenik a felügyeleti tanúsítványok listájában.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-162">The **Default HPC Azure Management** certificate appears in the list of management certificates.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="2bbd8-163">Azure-felhőszolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bbd8-163">Create an Azure cloud service</span></span>
> [!NOTE]
> <span data-ttu-id="2bbd8-164">A legjobb teljesítmény érdekében hozzon létre a felhőalapú szolgáltatáshoz és a storage-fiók (egy későbbi lépésben) ugyanabban a földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-164">For best performance, create the cloud service and the storage account (in a later step) in the same geographic region.</span></span>
> 
> 

1. <span data-ttu-id="2bbd8-165">Kattintson a portál **Felhőszolgáltatásokhoz (klasszikus)** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-165">In the portal, click **Cloud services (classic)** > **+Add**.</span></span>

2.  <span data-ttu-id="2bbd8-166">A szolgáltatás DNS-nevet, válasszon egy erőforráscsoportot és egy helyet, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-166">Type a DNS name for the service, choose a resource group and a location, and then click **Create**.</span></span>


### <a name="create-an-azure-storage-account"></a><span data-ttu-id="2bbd8-167">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bbd8-167">Create an Azure storage account</span></span>
1. <span data-ttu-id="2bbd8-168">Kattintson a portál **tárfiókok (klasszikus)** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-168">In the portal, click **Storage accounts (classic)** > **+Add**.</span></span>

2. <span data-ttu-id="2bbd8-169">Írja be a fiók nevét, majd válassza ki a **klasszikus** üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-169">Type a name for the account, and select the **Classic** deployment model.</span></span>

3. <span data-ttu-id="2bbd8-170">Válasszon egy erőforráscsoportot és egy helyet, és egyéb beállításokat hagyja alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-170">Choose a resource group and a location, and leave other settings at default values.</span></span> <span data-ttu-id="2bbd8-171">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-171">Then click **Create**.</span></span>

## <a name="configure-the-head-node"></a><span data-ttu-id="2bbd8-172">Az átjárócsomópont konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2bbd8-172">Configure the head node</span></span>
<span data-ttu-id="2bbd8-173">Telepítse az Azure csomópontokat és feladatok elküldéséhez a HPC Cluster Manager, először hajtsa végre néhány szükséges konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-173">To use HPC Cluster Manager to deploy Azure nodes and to submit jobs, first perform some required cluster configuration steps.</span></span>

1. <span data-ttu-id="2bbd8-174">A head csomóponton indítsa el a HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-174">On the head node, start HPC Cluster Manager.</span></span> <span data-ttu-id="2bbd8-175">Ha a **Head csomópont kiválasztása** párbeszédpanel, kattintson a **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-175">If the **Select Head Node** dialog box appears, click **Local Computer**.</span></span> <span data-ttu-id="2bbd8-176">A **telepítési feladatlista** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-176">The **Deployment To-do List** appears.</span></span>

2. <span data-ttu-id="2bbd8-177">A **szükséges telepítési feladatok**, kattintson a **a hálózat konfigurálásához**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-177">Under **Required deployment tasks**, click **Configure your network**.</span></span>
   
    ![Hálózat konfigurálása][config_hpc2]

3. <span data-ttu-id="2bbd8-179">Válassza ki a hálózat beállítása varázsló **minden csomópont csak a vállalati hálózaton** (topológia 5).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-179">In the Network Configuration Wizard, select **All nodes only on an enterprise network** (Topology 5).</span></span> <span data-ttu-id="2bbd8-180">A hálózati konfigurációt a legegyszerűbb bemutatási célokra.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-180">This network configuration is the simplest for demonstration purposes.</span></span>
   
    ![5 topológia][config_hpc3]
   
4. <span data-ttu-id="2bbd8-182">Kattintson a **tovább** a varázsló fennmaradó oldalain alapértelmezett értékek elfogadásához.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-182">Click **Next** to accept default values on the remaining pages of the wizard.</span></span> <span data-ttu-id="2bbd8-183">Ezt követően a **felülvizsgálati** lapra, majd **konfigurálása** a hálózati konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-183">Then, on the **Review** tab, click **Configure** to complete the network configuration.</span></span>

5. <span data-ttu-id="2bbd8-184">Az a **telepítési feladatlista**, kattintson a **telepítési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-184">In the **Deployment To-do List**, click **Provide installation credentials**.</span></span>

6. <span data-ttu-id="2bbd8-185">Az a **telepítés hitelesítő adatainál** párbeszédpanelen írja be a HPC csomag telepítéséhez használt tartományi fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-185">In the **Installation Credentials** dialog box, type the credentials of the domain account that you used to install HPC Pack.</span></span> <span data-ttu-id="2bbd8-186">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-186">Then click **OK**.</span></span> 
   
    ![Telepítési hitelesítő adatok][config_hpc6]
   
7. <span data-ttu-id="2bbd8-188">Az a **telepítési feladatlista**, kattintson a **konfigurálása az új csomópontok elnevezési**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-188">In the **Deployment To-do List**, click **Configure the naming of new nodes**.</span></span>

8. <span data-ttu-id="2bbd8-189">Az a **csomópont adja meg az adatsorozat elnevezési** párbeszédpanel mezőben fogadja el az alapértelmezett névhasználati adatsorozat, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-189">In the **Specify Node Naming Series** dialog box, accept the default naming series and click **OK**.</span></span> <span data-ttu-id="2bbd8-190">E lépés elvégzése után, annak ellenére, hogy ebben az oktatóanyagban hozzá Azure-csomópontok automatikusan néven szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-190">Complete this step even though the Azure nodes you add in this tutorial are named automatically.</span></span>
   
    ![Csomópont elnevezése][config_hpc8]
   
9. <span data-ttu-id="2bbd8-192">Az a **telepítési feladatlista**, kattintson a **csomópont-sablon létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-192">In the **Deployment To-do List**, click **Create a node template**.</span></span> <span data-ttu-id="2bbd8-193">Az oktatóanyag későbbi részében, a csomópont sablon segítségével Azure-csomópontok hozzáadása a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-193">Later in the tutorial, you use the node template to add Azure nodes to the cluster.</span></span>

10. <span data-ttu-id="2bbd8-194">A csomópont-sablon létrehozása varázsló tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2bbd8-194">In the Create Node Template Wizard, do the following:</span></span>
    
    <span data-ttu-id="2bbd8-195">a.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-195">a.</span></span> <span data-ttu-id="2bbd8-196">Az a **csomópont-sablon típusának kiválasztása** kattintson **csomópontsablonhoz a Windows Azure**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-196">On the **Choose Node Template Type** page, click **Windows Azure node template**, and then click **Next**.</span></span>
    
    ![Csomópont sablon][config_hpc10]
    
    <span data-ttu-id="2bbd8-198">b.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-198">b.</span></span> <span data-ttu-id="2bbd8-199">Kattintson a **következő** fogadja el az alapértelmezett sablont.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-199">Click **Next** to accept the default template name.</span></span>
    
    <span data-ttu-id="2bbd8-200">c.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-200">c.</span></span> <span data-ttu-id="2bbd8-201">Az a **előfizetés adatainak megadása** lapján adja meg az Azure előfizetés-Azonosítóval (az Azure-fiók adatainak érhető el).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-201">On the **Provide Subscription Information** page, enter your Azure subscription ID (available in your Azure account information).</span></span> <span data-ttu-id="2bbd8-202">Ezt követően a **felügyeleti tanúsítvány**, tallózással keresse meg **alapértelmezett Microsoft HPC Azure Management.**</span><span class="sxs-lookup"><span data-stu-id="2bbd8-202">Then, in **Management certificate**, browse for **Default Microsoft HPC Azure Management.**</span></span> <span data-ttu-id="2bbd8-203">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-203">Then click **Next**.</span></span>
    
    ![Csomópont sablon][config_hpc12]
    
    <span data-ttu-id="2bbd8-205">d.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-205">d.</span></span> <span data-ttu-id="2bbd8-206">Az a **szolgáltatás adatainak megadása** lapon, válassza ki a felhőszolgáltatás és a storage-fiók, amelyet az előző lépésben hozott létre.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-206">On the **Provide Service Information** page, select the cloud service and the storage account that you created in a previous step.</span></span> <span data-ttu-id="2bbd8-207">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-207">Then click **Next**.</span></span>
    
    ![Csomópont sablon][config_hpc13]
    
    <span data-ttu-id="2bbd8-209">e.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-209">e.</span></span> <span data-ttu-id="2bbd8-210">Kattintson a **tovább** a varázsló fennmaradó oldalain alapértelmezett értékek elfogadásához.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-210">Click **Next** to accept default values on the remaining pages of the wizard.</span></span> <span data-ttu-id="2bbd8-211">Ezt követően a **felülvizsgálati** lapra, majd **létrehozása** csomópont-sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-211">Then, on the **Review** tab, click **Create** to create the node template.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="2bbd8-212">Alapértelmezés szerint az Azure csomópont sablon beállításait tartalmazza (kiépíteni) elindításához, és állítsa le manuálisan, a csomópontok HPC Cluster Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-212">By default, the Azure node template includes settings for you to start (provision) and stop the nodes manually, using HPC Cluster Manager.</span></span> <span data-ttu-id="2bbd8-213">Ütemezés szerint elindíthassák és az Azure-csomópontok automatikusan konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-213">You can optionally configure a schedule to start and stop the Azure nodes automatically.</span></span>
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a><span data-ttu-id="2bbd8-214">Azure-csomópontok hozzáadása a fürthöz</span><span class="sxs-lookup"><span data-stu-id="2bbd8-214">Add Azure nodes to the cluster</span></span>
<span data-ttu-id="2bbd8-215">A csomópont sablon segítségével Azure-csomópontok hozzáadása a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-215">Now use the node template to add Azure nodes to the cluster.</span></span> <span data-ttu-id="2bbd8-216">A csomópontok hozzáadása a fürt tárolja a konfigurációs adatokat, így (kiépíteni) megkezdheti a felhőalapú szolgáltatás bármikor őket.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-216">Adding the nodes to the cluster stores their configuration information so that you can start (provision) them at any time in the cloud service.</span></span> <span data-ttu-id="2bbd8-217">Az előfizetés csak lekérdezi többletfizetésre volna szükség Azure-csomópontok után a példányok futnak a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-217">Your subscription only gets charged for Azure nodes after the instances are running in the cloud service.</span></span>

<span data-ttu-id="2bbd8-218">Kövesse az alábbi lépéseket, ha két kis fürtcsomópontokat szeretne hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-218">Follow these steps to add two Small nodes.</span></span>

1. <span data-ttu-id="2bbd8-219">A HPC-kezelőben kattintson **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack aktuális verziói) > **csomópont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-219">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack) > **Add Node**.</span></span>
   
    ![Csomópont hozzáadása][add_node1]

2. <span data-ttu-id="2bbd8-221">Csomópont hozzáadása varázsló a a **módszer kiválasztása** kattintson **hozzáadása Windows Azure-csomópontokra**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-221">In the Add Node Wizard, on the **Select Deployment Method** page, click **Add Windows Azure nodes**, and then click **Next**.</span></span>
   
    ![Az Azure csomópont hozzáadása][add_node1_1]

3. <span data-ttu-id="2bbd8-223">Az a **adja meg az új csomópontok** lapon, válassza ki a korábban létrehozott Azure csomópont sablont (alapértelmezés szerint nevű **alapértelmezett AzureNode sablon**).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-223">On the **Specify New Nodes** page, select the Azure node template you created previously (called by default **Default AzureNode Template**).</span></span> <span data-ttu-id="2bbd8-224">Adja meg **2** méretű csomópontok **kis**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-224">Then specify **2** nodes of size **Small**, and then click **Next**.</span></span>
   
    ![Adja meg a csomópontok][add_node2]
   
4. <span data-ttu-id="2bbd8-226">Az a **csomópont hozzáadása varázsló-Befejezés** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-226">On the **Completing the Add Node Wizard** page, click **Finish**.</span></span>
    
     <span data-ttu-id="2bbd8-227">Két Azure-csomópontok, nevű **AzureCN-0001** és **AzureCN-0002**, mostantól a HPC Cluster Manager jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-227">Two Azure nodes, named **AzureCN-0001** and **AzureCN-0002**, now appear in HPC Cluster Manager.</span></span> <span data-ttu-id="2bbd8-228">A mindkettő a **nem telepített** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-228">Both are in the **Not-Deployed** state.</span></span>
   
    ![Felvett csomópontjain][add_node3]

## <a name="start-the-azure-nodes"></a><span data-ttu-id="2bbd8-230">Indítsa el az Azure-csomópontok</span><span class="sxs-lookup"><span data-stu-id="2bbd8-230">Start the Azure nodes</span></span>
<span data-ttu-id="2bbd8-231">Ha azt szeretné, a fürt erőforrásainak használatához az Azure-ban, használja a HPC Cluster Manager start (kiépíteni) az Azure-csomópontok és online állapotba kerüljön.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-231">When you want to use the cluster resources in Azure, use HPC Cluster Manager to start (provision) the Azure nodes and bring them online.</span></span>

1. <span data-ttu-id="2bbd8-232">A HPC-kezelőben kattintson **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack aktuális verziói), és válassza ki az Azure-csomópontok.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-232">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack), and select the Azure nodes.</span></span>

2. <span data-ttu-id="2bbd8-233">Kattintson a **Start**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-233">Click **Start**, and then click **OK**.</span></span>
   
   ![Kiinduló csomópont][add_node4]
   
    <span data-ttu-id="2bbd8-235">Áttérés a csomópontok a **kiépítési** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-235">The nodes transition to the **Provisioning** state.</span></span> <span data-ttu-id="2bbd8-236">Megtekintheti a telepítési naplót a kiépítési folyamat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-236">View the provisioning log to track the provisioning progress.</span></span>
   
    ![Kiépítés csomópontok][add_node6]

3. <span data-ttu-id="2bbd8-238">Néhány perc elteltével az Azure-csomópontok üzembe helyezése, és a **Offline** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-238">After a few minutes, the Azure nodes finish provisioning and are in the **Offline** state.</span></span> <span data-ttu-id="2bbd8-239">Ebben az állapotban lévő szerepkör-példányok futnak, de még nem a fürt feladatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-239">In this state, the role instances are running but cannot yet accept cluster jobs.</span></span>

4. <span data-ttu-id="2bbd8-240">Győződjön meg arról, hogy a szerepkörpéldányok futnak, az Azure portálon kattintson a **Felhőszolgáltatások (klasszikus)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-240">To confirm that the role instances are running, in the Azure portal, click **Cloud Services (classic)** > *your_cloud_service_name*.</span></span>
   
   <span data-ttu-id="2bbd8-241">Két kell megjelennie **HpcWorkerRole** a szolgáltatásban futó példányok (csomópontok).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-241">You should see two **HpcWorkerRole** instances (nodes) running in the service.</span></span> <span data-ttu-id="2bbd8-242">HPC Pack is automatikusan telepíti a két **HpcProxy** példányok (közepes méretű), az átjárócsomópont és az Azure közötti kommunikáció kezelésére.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-242">HPC Pack also automatically deploys two **HpcProxy** instances (size Medium) to handle communication between the head node and Azure.</span></span>

   ![Futó példányát tekintve][view_instances1]

5. <span data-ttu-id="2bbd8-244">Ahhoz, hogy az Azure-csomópontok online fürtben feladatok futtatásához, válassza ki a csomópontot, kattintson a jobb gombbal, és végül **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-244">To bring the Azure nodes online to run cluster jobs, select the nodes, right-click, and then click **Bring Online**.</span></span>
   
    ![Kapcsolat nélküli csomópontok][add_node7]
   
    <span data-ttu-id="2bbd8-246">HPC Fürtkezelő azt jelzi, hogy a csomópontok a a **Online** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-246">HPC Cluster Manager indicates that the nodes are in the **Online** state.</span></span>

## <a name="run-a-command-across-the-cluster"></a><span data-ttu-id="2bbd8-247">Futtassa a parancsot a fürtön</span><span class="sxs-lookup"><span data-stu-id="2bbd8-247">Run a command across the cluster</span></span>
<span data-ttu-id="2bbd8-248">A telepítés ellenőrzéséhez használja a HPC Pack **clusrun** parancsot kell futtatni egy parancsot vagy az alkalmazás egy vagy több fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-248">To check the installation, use the HPC Pack **clusrun** command to run a command or application on one or more cluster nodes.</span></span> <span data-ttu-id="2bbd8-249">Egyszerű példaként használható **clusrun** beolvasni az Azure-csomópontok IP-konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-249">As a simple example, use **clusrun** to get the IP configuration of the Azure nodes.</span></span>

1. <span data-ttu-id="2bbd8-250">A head csomóponton nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-250">On the head node, open a command prompt as an administrator.</span></span>

2. <span data-ttu-id="2bbd8-251">Írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2bbd8-251">Type the following command:</span></span>
   
    `clusrun /nodes:azurecn* ipconfig`

3. <span data-ttu-id="2bbd8-252">Ha a rendszer kéri, adja meg a fürt rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-252">If prompted, enter your cluster administrator password.</span></span> <span data-ttu-id="2bbd8-253">Meg kell jelennie a következőhöz hasonló parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-253">You should see command output similar to the following.</span></span>
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a><span data-ttu-id="2bbd8-255">Egy tesztelési feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="2bbd8-255">Run a test job</span></span>
<span data-ttu-id="2bbd8-256">Most már a hibrid fürtön futó teszt feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-256">Now submit a test job that runs on the hybrid cluster.</span></span> <span data-ttu-id="2bbd8-257">Ebben a példában egy egyszerű paraméteres ismétlés feladat (belsőleg párhuzamos számítási típusú).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-257">This example is a simple parametric sweep job (a type of intrinsically parallel computation).</span></span> <span data-ttu-id="2bbd8-258">Ebben a példában résztevékenység, vegyen fel egy egész számot maga segítségével futtatja a **/a beállítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-258">This example runs subtasks that add an integer to itself by using the **set /a** command.</span></span> <span data-ttu-id="2bbd8-259">A fürt összes csomópontjának hozzájárul az egész számok résztevékenység befejezési 1 és 100.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-259">All the nodes in the cluster contribute to finishing the subtasks for integers from 1 to 100.</span></span>

1. <span data-ttu-id="2bbd8-260">A HPC-kezelőben kattintson **feladatkezelés** > **új paraméteres Sweep feladat**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-260">In HPC Cluster Manager, click **Job Management** > **New Parametric Sweep Job**.</span></span>
   
    ![Új feladat][test_job1]

2. <span data-ttu-id="2bbd8-262">Az a **új paraméteres Sweep feladat** párbeszédpanel **parancssori**, típus `set /a *+*` (felülírja az alapértelmezett parancssor, amely akkor jelenik meg).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-262">In the **New Parametric Sweep Job** dialog box, in **Command line**, type `set /a *+*` (overwriting the default command line that appears).</span></span> <span data-ttu-id="2bbd8-263">Hagyja meg a többi beállítást tartozó alapértelmezett értékeket, és kattintson **Submit** elküldeni a feladatot.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-263">Leave default values for the remaining settings, and then click **Submit** to submit the job.</span></span>
   
    ![Paraméteres ismétlés][param_sweep1]

3. <span data-ttu-id="2bbd8-265">Ha a feladat befejeződött, kattintson duplán a **saját Sweep feladat** feladat.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-265">When the job is finished, double-click the **My Sweep Task** job.</span></span>

4. <span data-ttu-id="2bbd8-266">Kattintson a **nézet feladatai**, majd kattintson egy részfeladatnál annak regisztrálása adott részfeladatnál annak regisztrálása számított eredményének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-266">Click **View Tasks**, and then click a subtask to view the calculated output of that subtask.</span></span>
   
    ![A feladat eredménye][view_job361]

5. <span data-ttu-id="2bbd8-268">Melyik csomópontján elvégzi a számítást, hogy részfeladatnál annak regisztrálása megtekintéséhez kattintson **lefoglalt csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-268">To see which node performed the calculation for that subtask, click **Allocated Nodes**.</span></span> <span data-ttu-id="2bbd8-269">(A fürt előfordulhat, hogy megjelenítése egy másik csomópont neve).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-269">(Your cluster might show a different node name.)</span></span>
   
    ![A feladat eredménye][view_job362]

## <a name="stop-the-azure-nodes"></a><span data-ttu-id="2bbd8-271">Állítsa le az Azure-csomópontok</span><span class="sxs-lookup"><span data-stu-id="2bbd8-271">Stop the Azure nodes</span></span>
<span data-ttu-id="2bbd8-272">Próbálja ki a fürtöt, az Azure-fiókjába felesleges költségek elkerülése érdekében csomópontok le.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-272">After you try out the cluster, stop the Azure nodes to avoid unnecessary charges to your account.</span></span> <span data-ttu-id="2bbd8-273">Ez a lépés leállítja a felhőszolgáltatást, és eltávolítja a Azure szerepkörpéldányokat.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-273">This step stops the cloud service and removes the Azure role instances.</span></span>

1. <span data-ttu-id="2bbd8-274">A HPC Cluster Manager, a **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack korábbi verzióiban), válassza ki mindkét Azure-csomópontok.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-274">In HPC Cluster Manager, in **Node Management** (called **Resource Management** in previous versions of HPC Pack), select both Azure nodes.</span></span> <span data-ttu-id="2bbd8-275">Kattintson a **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-275">Then, click **Stop**.</span></span>
   
    ![Csomópont leállítása][stop_node1]

2. <span data-ttu-id="2bbd8-277">Az a **állítsa le a Windows Azure-csomópontokra** párbeszédpanel, kattintson a **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-277">In the **Stop Windows Azure nodes** dialog box, click **Stop**.</span></span> 
   
3. <span data-ttu-id="2bbd8-278">Áttérés a csomópontok a **leállítása** állapotát.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-278">The nodes transition to the **Stopping** state.</span></span> <span data-ttu-id="2bbd8-279">Néhány perc elteltével HPC Cluster Manager azt mutatja, hogy a csomópontok **nem telepített**.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-279">After a few minutes, HPC Cluster Manager shows that the nodes are **Not-Deployed**.</span></span>
   
    ![Nem telepített csomópontok][stop_node4]

4. <span data-ttu-id="2bbd8-281">Győződjön meg róla, hogy a szerepkörpéldányok már nem fut, az Azure-ban az Azure portálon kattintson a **Felhőszolgáltatásokhoz (klasszikus)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-281">To confirm that the role instances are no longer running in Azure, in the Azure portal, click **Cloud services (classic)** > *your_cloud_service_name*.</span></span> <span data-ttu-id="2bbd8-282">Nincs példány az éles környezetben vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-282">No instances are deployed in the production environment.</span></span> 
   
    <span data-ttu-id="2bbd8-283">Ezzel befejezte az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2bbd8-283">This completes the tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bbd8-284">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2bbd8-284">Next steps</span></span>
* <span data-ttu-id="2bbd8-285">Megismerkedhet a dokumentációja [HPC Pack](https://technet.microsoft.com/library/cc514029).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-285">Explore the documentation for [HPC Pack](https://technet.microsoft.com/library/cc514029).</span></span>
* <span data-ttu-id="2bbd8-286">HPC Pack fürttelepítés nagyobb léptékű hibrid beállításához tekintse meg a [Azure feldolgozói szerepkör példányokhoz Microsoft HPC Pack kapacitásnövelés](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-286">To set up a hybrid HPC Pack cluster deployment at greater scale, see [Burst to Azure Worker Role Instances with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
* <span data-ttu-id="2bbd8-287">Egyéb módon hozhat létre a HPC Pack-fürtöt az Azure-ban, beleértve az Azure Resource Manager-sablonokkal, lásd: [HPC-fürt beállítások Microsoft HPC Pack az Azure-ban](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2bbd8-287">For other ways to create an HPC Pack cluster in Azure, including using Azure Resource Manager templates, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
