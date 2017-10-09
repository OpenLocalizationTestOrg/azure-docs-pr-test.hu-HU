---
title: "HPC Pack hibrid mentése aaaSet fürtön, az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Microsoft HPC Pack és Azure tooset legfeljebb egy kisméretű, hibrid nagy teljesítményű számítástechnikai (HPC) fürt"
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
ms.openlocfilehash: 5ad30d78dcd0c6a95d2a61f25015232edab3563c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a><span data-ttu-id="6775d-103">A hibrid nagy teljesítményű számítástechnikai igény Azure számítási csomópontok és a Microsoft HPC Pack (HPC) fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="6775d-103">Set up a hybrid high performance computing (HPC) cluster with Microsoft HPC Pack and on-demand Azure compute nodes</span></span>
<span data-ttu-id="6775d-104">Használja a Microsoft HPC Pack 2012 R2 és az Azure tooset (HPC) fürt a számítástechnikai, small, hibrid nagy teljesítményű fel.</span><span class="sxs-lookup"><span data-stu-id="6775d-104">Use Microsoft HPC Pack 2012 R2 and Azure tooset up a small, hybrid high performance computing (HPC) cluster.</span></span> <span data-ttu-id="6775d-105">Ez a cikk látható hello fürt tartalmaz egy helyszíni HPC Pack átjárócsomópont, és néhány számítási csomópont telepítése egy Azure-ban igény a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6775d-105">hello cluster shown in this article consists of an on-premises HPC Pack head node and some compute nodes you deploy on-demand in an Azure cloud service.</span></span> <span data-ttu-id="6775d-106">Ezt követően futtathatja számítási feladatok hello hibrid fürtön.</span><span class="sxs-lookup"><span data-stu-id="6775d-106">You can then run compute jobs on hello hybrid cluster.</span></span>

![Hibrid HPC-fürt][Overview] 

<span data-ttu-id="6775d-108">Ez az oktatóanyag az egyik módszer, más néven a fürt "kapacitásnövelés toohello felhő," toouse méretezhető, igény szerinti Azure-erőforrások toorun számítási igényű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="6775d-108">This tutorial shows one approach, sometimes called cluster "burst toohello cloud," toouse scalable, on-demand Azure resources toorun compute-intensive applications.</span></span>

<span data-ttu-id="6775d-109">Ez az oktatóanyag azt feltételezi, hogy a számítási fürtök vagy HPC Pack nincs korábbi tapasztalata kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6775d-109">This tutorial assumes no prior experience with compute clusters or HPC Pack.</span></span> <span data-ttu-id="6775d-110">Tervezett csak toohelp gyorsan bemutatási célokra hibrid számítási fürt telepítése is.</span><span class="sxs-lookup"><span data-stu-id="6775d-110">It is intended only toohelp you deploy a hybrid compute cluster quickly for demonstration purposes.</span></span> <span data-ttu-id="6775d-111">További szempontok és lépések toodeploy HPC Pack hibrid fürtön, nagyobb léptékű termelési környezetben, vagy toouse HPC Pack 2016: hello [részletes útmutatást](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="6775d-111">For considerations and steps toodeploy a hybrid HPC Pack cluster at greater scale in a production environment, or toouse HPC Pack 2016, see hello [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span> <span data-ttu-id="6775d-112">HPC Pack egyéb forgatókönyvek, beleértve a fürtöt tartalmazó környezetben az Azure virtuális gépeken automatikus című [HPC-fürt beállítások Microsoft HPC Pack az Azure-ban](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6775d-112">For other scenarios with HPC Pack, including automated cluster deployment in Azure virtual machines, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6775d-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6775d-113">Prerequisites</span></span>
* <span data-ttu-id="6775d-114">**Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="6775d-114">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>
* <span data-ttu-id="6775d-115">**Egy Windows Server 2012 R2 vagy Windows Server 2012 rendszert futtató helyszíni számítógép** -hello hello HPC-fürt átjárócsomópontjához használni ezen a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6775d-115">**An on-premises computer running Windows Server 2012 R2 or Windows Server 2012** - Use this computer as hello head node of hello HPC cluster.</span></span> <span data-ttu-id="6775d-116">Ha már nem futtat Windows Server, töltse le és telepítse az [próbaverzió](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span><span class="sxs-lookup"><span data-stu-id="6775d-116">If you aren't already running Windows Server, you can download and install an [evaluation version](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span></span>
  
  * <span data-ttu-id="6775d-117">hello számítógép illesztett tooan Active Directory-tartományban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6775d-117">hello computer must be joined tooan Active Directory domain.</span></span> <span data-ttu-id="6775d-118">Tesztelési célokra beállíthatja hello átjárócsomópont számítógépet tartományvezérlőként működik.</span><span class="sxs-lookup"><span data-stu-id="6775d-118">For test purposes, you can configure hello head node computer as a domain controller.</span></span> <span data-ttu-id="6775d-119">tooadd hello Active Directory tartományi szolgáltatások kiszolgálói szerepkör, és léptesse elő hello átjárócsomópont számítógépet tartományvezérlőként, hello Windows Server dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="6775d-119">tooadd hello Active Directory Domain Services server role and promote hello head node computer as a domain controller, see hello documentation for Windows Server.</span></span>
  * <span data-ttu-id="6775d-120">toosupport HPC Pack, hello operációs rendszer telepítenie kell az ezeken a nyelveken egyik: angol, japán vagy kínai (egyszerűsített).</span><span class="sxs-lookup"><span data-stu-id="6775d-120">toosupport HPC Pack, hello operating system must be installed in one of these languages: English, Japanese, or Chinese (Simplified).</span></span>
  * <span data-ttu-id="6775d-121">Győződjön meg arról, hogy a fontos és kritikus frissítések telepítve vannak-e.</span><span class="sxs-lookup"><span data-stu-id="6775d-121">Verify that important and critical updates are installed.</span></span>
* <span data-ttu-id="6775d-122">**HPC Pack 2012 R2** - [letöltése](http://go.microsoft.com/fwlink/p/?linkid=328024) hello telepítési csomag legújabb verzióhoz hello ingyenes, a költség, és másolja hello fájlok toohello központi csomópont-számítógépén.</span><span class="sxs-lookup"><span data-stu-id="6775d-122">**HPC Pack 2012 R2** - [Download](http://go.microsoft.com/fwlink/p/?linkid=328024) hello installation package for hello latest version free of charge and copy hello files toohello head node computer.</span></span> <span data-ttu-id="6775d-123">Válassza ki a telepítési fájlok hello azonos nyelv, a Windows Server telepítése.</span><span class="sxs-lookup"><span data-stu-id="6775d-123">Choose installation files in hello same language as your installation of Windows Server.</span></span>

    >[!NOTE]
    > <span data-ttu-id="6775d-124">Ha azt szeretné, hogy a HPC Pack 2016 toouse helyett HPC Pack 2012 R2-ben, további konfigurációra van szükség.</span><span class="sxs-lookup"><span data-stu-id="6775d-124">If you want toouse HPC Pack 2016 instead of HPC Pack 2012 R2, additional configuration is needed.</span></span> <span data-ttu-id="6775d-125">Lásd: hello [részletes útmutatást](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="6775d-125">See hello [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
    > 
* <span data-ttu-id="6775d-126">**Tartományi fiók** -ennek a fióknak helyi rendszergazdai engedélyekkel hello átjárócsomópont tooinstall HPC Pack kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6775d-126">**Domain account** - This account must be configured with local Administrator permissions on hello head node tooinstall HPC Pack.</span></span>
* <span data-ttu-id="6775d-127">**TCP-kapcsolatot a 443-as porton** a hello átjárócsomópont tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6775d-127">**TCP connectivity on port 443** from hello head node tooAzure.</span></span>

## <a name="install-hpc-pack-on-hello-head-node"></a><span data-ttu-id="6775d-128">HPC Pack hello átjárócsomópont telepítése</span><span class="sxs-lookup"><span data-stu-id="6775d-128">Install HPC Pack on hello head node</span></span>
<span data-ttu-id="6775d-129">Először telepítenie kell a Microsoft HPC Pack a helyszíni Windows Server rendszert futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6775d-129">You first install Microsoft HPC Pack on your on-premises computer running Windows Server.</span></span> <span data-ttu-id="6775d-130">Ez a számítógép hello hello fürt átjárócsomópontjához válik.</span><span class="sxs-lookup"><span data-stu-id="6775d-130">This computer becomes hello head node of hello cluster.</span></span>

1. <span data-ttu-id="6775d-131">Jelentkezzen be toohello átjárócsomópont egy tartományi fiókkal, amely helyi rendszergazdai engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6775d-131">Log on toohello head node by using a domain account that has local Administrator permissions.</span></span>

2. <span data-ttu-id="6775d-132">Hello HPC Pack telepítési varázsló elindításához futtassa a Setup.exe hello HPC Pack telepítési fájlokból.</span><span class="sxs-lookup"><span data-stu-id="6775d-132">Start hello HPC Pack Installation Wizard by running Setup.exe from hello HPC Pack installation files.</span></span>

3. <span data-ttu-id="6775d-133">A hello **HPC Pack 2012 R2 telepítő** kattintson **új telepítést, vagy adja hozzá az új funkciók tooan példánya**.</span><span class="sxs-lookup"><span data-stu-id="6775d-133">On hello **HPC Pack 2012 R2 Setup** screen, click **New installation or add new features tooan existing installation**.</span></span>

    ![HPC Pack 2012 telepítése][install_hpc1]

4. <span data-ttu-id="6775d-135">A hello **Microsoft szoftver felhasználói oldal**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6775d-135">On hello **Microsoft Software User Agreement page**, click **Next**.</span></span>

5. <span data-ttu-id="6775d-136">A hello **telepítés típusának kiválasztása** kattintson **hozzon létre egy új HPC-fürtöt hozzon létre egy átjárócsomóponttal**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6775d-136">On hello **Select Installation Type** page, click **Create a new HPC cluster by creating a head node**, and then click **Next**.</span></span>

6. <span data-ttu-id="6775d-137">hello varázsló számos telepítés előtti tesztet futtatja.</span><span class="sxs-lookup"><span data-stu-id="6775d-137">hello wizard runs several pre-installation tests.</span></span> <span data-ttu-id="6775d-138">Kattintson a **következő** a hello **telepítési szabályokat** lapon, ha az összes teszt sikeresen lezajlott.</span><span class="sxs-lookup"><span data-stu-id="6775d-138">Click **Next** on hello **Installation Rules** page if all tests pass.</span></span> <span data-ttu-id="6775d-139">Ellenkező esetben ellenőrizze a hello útmutatását, és végezze el a szükséges módosításokat a környezetben.</span><span class="sxs-lookup"><span data-stu-id="6775d-139">Otherwise, review hello information provided and make any necessary changes in your environment.</span></span> <span data-ttu-id="6775d-140">Majd futtassa újra hello teszteket, vagy ha szükséges start újra hello-e telepítési varázsló.</span><span class="sxs-lookup"><span data-stu-id="6775d-140">Then run hello tests again or if necessary start hello Installation Wizard again.</span></span>
7. <span data-ttu-id="6775d-141">A hello **HPC DB konfigurációs** lapon, győződjön meg arról, hogy **Átjárócsomópont** van kiválasztva a HPC-adatbázisok, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="6775d-141">On hello **HPC DB Configuration** page, make sure **Head Node** is selected for all HPC databases, and then click **Next**.</span></span> 

    ![Adatbázis-konfiguráció][install_hpc4]

8. <span data-ttu-id="6775d-143">Fogadja el a hello hello varázsló fennmaradó oldalain alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6775d-143">Accept default selections on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="6775d-144">A hello **szükséges összetevők telepítése** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="6775d-144">On hello **Install Required Components** page, click **Install**.</span></span>
   
    ![Telepítés][install_hpc6]

9. <span data-ttu-id="6775d-146">Hello telepítés befejezése után törölje a jelet **indítsa el a HPC Cluster Manager** majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="6775d-146">After hello installation completes, uncheck **Start HPC Cluster Manager** and then click **Finish**.</span></span> <span data-ttu-id="6775d-147">(A start HPC Cluster Manager egy későbbi lépésben.)</span><span class="sxs-lookup"><span data-stu-id="6775d-147">(You start HPC Cluster Manager in a later step.)</span></span>
   
    ![Befejezés][install_hpc7]

## <a name="prepare-hello-azure-subscription"></a><span data-ttu-id="6775d-149">Készítse elő a hello Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="6775d-149">Prepare hello Azure subscription</span></span>
<span data-ttu-id="6775d-150">Hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com) Azure-előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="6775d-150">Perform hello following steps in hello [Azure portal](https://portal.azure.com) with your Azure subscription.</span></span> <span data-ttu-id="6775d-151">A lépések elvégzése után telepíthet hello helyszíni átjáró-csomóponti az Azure csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="6775d-151">After completing these steps, you can deploy Azure nodes from hello on-premises head node.</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="6775d-152">Továbbá jegyezze fel a Azure-előfizetése Azonosítóját, ami később szüksége.</span><span class="sxs-lookup"><span data-stu-id="6775d-152">Also make a note of your Azure subscription ID, which you need later.</span></span> <span data-ttu-id="6775d-153">A hello azonosító található **előfizetések** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="6775d-153">Find hello ID in **Subscriptions** in hello portal.</span></span>
  > 

### <a name="upload-hello-default-management-certificate"></a><span data-ttu-id="6775d-154">Hello alapértelmezett felügyeleti tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="6775d-154">Upload hello default management certificate</span></span>
<span data-ttu-id="6775d-155">HPC Pack egy önaláírt tanúsítványt telepít hello átjárócsomópont, hello alapértelmezett Microsoft HPC Azure felügyeleti tanúsítvány, amely feltöltheti az Azure felügyeleti tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="6775d-155">HPC Pack installs a self-signed certificate on hello head node, called hello Default Microsoft HPC Azure Management certificate, that you can upload as an Azure management certificate.</span></span> <span data-ttu-id="6775d-156">Ez a tanúsítvány előírt tesztelése, és a koncepció igazolása központi telepítések toosecure hello közötti kapcsolat hello átjárócsomópont és az Azure.</span><span class="sxs-lookup"><span data-stu-id="6775d-156">This certificate is provided for testing and proof-of-concept deployments toosecure hello connection between hello head node and Azure.</span></span>

1. <span data-ttu-id="6775d-157">Hello átjárócsomópont számítógépről toohello bejelentkezés [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6775d-157">From hello head node computer, sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="6775d-158">Kattintson a **előfizetések** > *your_subscription_name*.</span><span class="sxs-lookup"><span data-stu-id="6775d-158">Click **Subscriptions** > *your_subscription_name*.</span></span>

3. <span data-ttu-id="6775d-159">Kattintson a **felügyeleti tanúsítványok** > **feltöltése**.4.</span><span class="sxs-lookup"><span data-stu-id="6775d-159">Click **Management certificates** > **Upload**.4.</span></span> <span data-ttu-id="6775d-160">Keresse meg a hello fájl C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer hello központi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="6775d-160">Browse on hello head node for hello file C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer.</span></span> <span data-ttu-id="6775d-161">Kattintson a **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="6775d-161">Then, click **Upload**.</span></span>

   
<span data-ttu-id="6775d-162">Hello **alapértelmezett HPC Azure felügyeleti** tanúsítvány megjelenik a felügyeleti tanúsítványok hello listájában.</span><span class="sxs-lookup"><span data-stu-id="6775d-162">hello **Default HPC Azure Management** certificate appears in hello list of management certificates.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="6775d-163">Azure-felhőszolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6775d-163">Create an Azure cloud service</span></span>
> [!NOTE]
> <span data-ttu-id="6775d-164">A legjobb teljesítmény érdekében hozzon létre hello felhőszolgáltatás és a storage-fiók hello (egy későbbi lépésben) hello ugyanabban a földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="6775d-164">For best performance, create hello cloud service and hello storage account (in a later step) in hello same geographic region.</span></span>
> 
> 

1. <span data-ttu-id="6775d-165">Hello portálon kattintson **Felhőszolgáltatásokhoz (klasszikus)** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6775d-165">In hello portal, click **Cloud services (classic)** > **+Add**.</span></span>

2.  <span data-ttu-id="6775d-166">Hello szolgáltatás DNS-nevet, válasszon egy erőforráscsoportot és egy helyet, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6775d-166">Type a DNS name for hello service, choose a resource group and a location, and then click **Create**.</span></span>


### <a name="create-an-azure-storage-account"></a><span data-ttu-id="6775d-167">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6775d-167">Create an Azure storage account</span></span>
1. <span data-ttu-id="6775d-168">Hello portálon kattintson **tárfiókok (klasszikus)** > **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6775d-168">In hello portal, click **Storage accounts (classic)** > **+Add**.</span></span>

2. <span data-ttu-id="6775d-169">Írja be a hello fiók nevét, majd válassza ki a hello **klasszikus** üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="6775d-169">Type a name for hello account, and select hello **Classic** deployment model.</span></span>

3. <span data-ttu-id="6775d-170">Válasszon egy erőforráscsoportot és egy helyet, és egyéb beállításokat hagyja alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="6775d-170">Choose a resource group and a location, and leave other settings at default values.</span></span> <span data-ttu-id="6775d-171">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6775d-171">Then click **Create**.</span></span>

## <a name="configure-hello-head-node"></a><span data-ttu-id="6775d-172">Átjárócsomópont hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6775d-172">Configure hello head node</span></span>
<span data-ttu-id="6775d-173">HPC Fürtkezelő toodeploy toouse Azure-csomópontok és toosubmit feladatok, először hajtsa végre néhány szükséges konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6775d-173">toouse HPC Cluster Manager toodeploy Azure nodes and toosubmit jobs, first perform some required cluster configuration steps.</span></span>

1. <span data-ttu-id="6775d-174">Indítsa el a HPC Fürtkezelő hello központi csomópontján.</span><span class="sxs-lookup"><span data-stu-id="6775d-174">On hello head node, start HPC Cluster Manager.</span></span> <span data-ttu-id="6775d-175">Ha hello **Head csomópont kiválasztása** párbeszédpanel, kattintson a **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="6775d-175">If hello **Select Head Node** dialog box appears, click **Local Computer**.</span></span> <span data-ttu-id="6775d-176">Hello **telepítési feladatlista** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6775d-176">hello **Deployment To-do List** appears.</span></span>

2. <span data-ttu-id="6775d-177">A **szükséges telepítési feladatok**, kattintson a **a hálózat konfigurálásához**.</span><span class="sxs-lookup"><span data-stu-id="6775d-177">Under **Required deployment tasks**, click **Configure your network**.</span></span>
   
    ![Hálózat konfigurálása][config_hpc2]

3. <span data-ttu-id="6775d-179">Hello hálózat beállítása varázsló, jelölje ki **minden csomópont csak a vállalati hálózaton** (topológia 5).</span><span class="sxs-lookup"><span data-stu-id="6775d-179">In hello Network Configuration Wizard, select **All nodes only on an enterprise network** (Topology 5).</span></span> <span data-ttu-id="6775d-180">Ez a hálózati konfiguráció hello legegyszerűbb bemutatási célokra.</span><span class="sxs-lookup"><span data-stu-id="6775d-180">This network configuration is hello simplest for demonstration purposes.</span></span>
   
    ![5 topológia][config_hpc3]
   
4. <span data-ttu-id="6775d-182">Kattintson a **következő** hello varázsló lapjain fennmaradó hello tooaccept alapértelmezett értékét.</span><span class="sxs-lookup"><span data-stu-id="6775d-182">Click **Next** tooaccept default values on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="6775d-183">Ezután a hello **felülvizsgálati** lapra, majd **konfigurálása** toocomplete hello hálózati konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6775d-183">Then, on hello **Review** tab, click **Configure** toocomplete hello network configuration.</span></span>

5. <span data-ttu-id="6775d-184">A hello **telepítési feladatlista**, kattintson a **telepítési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="6775d-184">In hello **Deployment To-do List**, click **Provide installation credentials**.</span></span>

6. <span data-ttu-id="6775d-185">A hello **telepítés hitelesítő adatainál** párbeszédpanelen hello tartományi fiók, amellyel tooinstall HPC Pack típus hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="6775d-185">In hello **Installation Credentials** dialog box, type hello credentials of hello domain account that you used tooinstall HPC Pack.</span></span> <span data-ttu-id="6775d-186">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6775d-186">Then click **OK**.</span></span> 
   
    ![Telepítési hitelesítő adatok][config_hpc6]
   
7. <span data-ttu-id="6775d-188">A hello **telepítési feladatlista**, kattintson a **hello elnevezési új csomópontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6775d-188">In hello **Deployment To-do List**, click **Configure hello naming of new nodes**.</span></span>

8. <span data-ttu-id="6775d-189">A hello **csomópont adja meg az adatsorozat elnevezési** párbeszédpanel mezőben fogadja el a hello alapértelmezett névhasználati a sorozat, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6775d-189">In hello **Specify Node Naming Series** dialog box, accept hello default naming series and click **OK**.</span></span> <span data-ttu-id="6775d-190">E lépés elvégzése után, annak ellenére, hogy hello ebben az oktatóanyagban hozzá Azure-csomópontok megnevezett automatikusan.</span><span class="sxs-lookup"><span data-stu-id="6775d-190">Complete this step even though hello Azure nodes you add in this tutorial are named automatically.</span></span>
   
    ![Csomópont elnevezése][config_hpc8]
   
9. <span data-ttu-id="6775d-192">A hello **telepítési feladatlista**, kattintson a **csomópont-sablon létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6775d-192">In hello **Deployment To-do List**, click **Create a node template**.</span></span> <span data-ttu-id="6775d-193">Hello oktatóanyag későbbi részében hello csomópont sablon tooadd Azure-csomópontok toohello fürtöt használ.</span><span class="sxs-lookup"><span data-stu-id="6775d-193">Later in hello tutorial, you use hello node template tooadd Azure nodes toohello cluster.</span></span>

10. <span data-ttu-id="6775d-194">A csomópont-sablon létrehozása varázsló hello, hajtsa végre a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6775d-194">In hello Create Node Template Wizard, do hello following:</span></span>
    
    <span data-ttu-id="6775d-195">a.</span><span class="sxs-lookup"><span data-stu-id="6775d-195">a.</span></span> <span data-ttu-id="6775d-196">A hello **csomópont-sablon típusának kiválasztása** kattintson **csomópontsablonhoz a Windows Azure**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6775d-196">On hello **Choose Node Template Type** page, click **Windows Azure node template**, and then click **Next**.</span></span>
    
    ![Csomópont sablon][config_hpc10]
    
    <span data-ttu-id="6775d-198">b.</span><span class="sxs-lookup"><span data-stu-id="6775d-198">b.</span></span> <span data-ttu-id="6775d-199">Kattintson a **következő** tooaccept hello alapértelmezett nevet.</span><span class="sxs-lookup"><span data-stu-id="6775d-199">Click **Next** tooaccept hello default template name.</span></span>
    
    <span data-ttu-id="6775d-200">c.</span><span class="sxs-lookup"><span data-stu-id="6775d-200">c.</span></span> <span data-ttu-id="6775d-201">A hello **előfizetés adatainak megadása** lapján adja meg az Azure előfizetés-Azonosítóval (az Azure-fiók adatainak érhető el).</span><span class="sxs-lookup"><span data-stu-id="6775d-201">On hello **Provide Subscription Information** page, enter your Azure subscription ID (available in your Azure account information).</span></span> <span data-ttu-id="6775d-202">Ezt követően a **felügyeleti tanúsítvány**, tallózással keresse meg **alapértelmezett Microsoft HPC Azure Management.**</span><span class="sxs-lookup"><span data-stu-id="6775d-202">Then, in **Management certificate**, browse for **Default Microsoft HPC Azure Management.**</span></span> <span data-ttu-id="6775d-203">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="6775d-203">Then click **Next**.</span></span>
    
    ![Csomópont sablon][config_hpc12]
    
    <span data-ttu-id="6775d-205">d.</span><span class="sxs-lookup"><span data-stu-id="6775d-205">d.</span></span> <span data-ttu-id="6775d-206">A hello **szolgáltatás adatainak megadása** lap, válassza hello felhőalapú szolgáltatás és az előző lépésben létrehozott tárfiók hello.</span><span class="sxs-lookup"><span data-stu-id="6775d-206">On hello **Provide Service Information** page, select hello cloud service and hello storage account that you created in a previous step.</span></span> <span data-ttu-id="6775d-207">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="6775d-207">Then click **Next**.</span></span>
    
    ![Csomópont sablon][config_hpc13]
    
    <span data-ttu-id="6775d-209">e.</span><span class="sxs-lookup"><span data-stu-id="6775d-209">e.</span></span> <span data-ttu-id="6775d-210">Kattintson a **következő** hello varázsló lapjain fennmaradó hello tooaccept alapértelmezett értékét.</span><span class="sxs-lookup"><span data-stu-id="6775d-210">Click **Next** tooaccept default values on hello remaining pages of hello wizard.</span></span> <span data-ttu-id="6775d-211">Ezután a hello **felülvizsgálati** lapra, majd **létrehozása** toocreate hello csomópont sablont.</span><span class="sxs-lookup"><span data-stu-id="6775d-211">Then, on hello **Review** tab, click **Create** toocreate hello node template.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="6775d-212">Alapértelmezés szerint hello Azure csomópont sablon beállításait tartalmazza (kiépíteni) toostart és stop hello csomópontok manuálisan, HPC Cluster Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="6775d-212">By default, hello Azure node template includes settings for you toostart (provision) and stop hello nodes manually, using HPC Cluster Manager.</span></span> <span data-ttu-id="6775d-213">Egy ütemezés toostart igény szerint konfigurálható és az Azure-csomópontok automatikusan hello leállítása.</span><span class="sxs-lookup"><span data-stu-id="6775d-213">You can optionally configure a schedule toostart and stop hello Azure nodes automatically.</span></span>
    > 
    > 

## <a name="add-azure-nodes-toohello-cluster"></a><span data-ttu-id="6775d-214">Azure-csomópontok toohello fürt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6775d-214">Add Azure nodes toohello cluster</span></span>
<span data-ttu-id="6775d-215">Most hello csomópont sablon tooadd Azure-csomópontok toohello fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="6775d-215">Now use hello node template tooadd Azure nodes toohello cluster.</span></span> <span data-ttu-id="6775d-216">Hello csomópontok toohello fürt tárolja a konfigurációs adatokat, így a elindíthatja (kiépíteni) hozzáadása hello felhőszolgáltatáshoz bármikor őket.</span><span class="sxs-lookup"><span data-stu-id="6775d-216">Adding hello nodes toohello cluster stores their configuration information so that you can start (provision) them at any time in hello cloud service.</span></span> <span data-ttu-id="6775d-217">Az előfizetés csak lekérdezi többletfizetésre volna szükség Azure-csomópontok hello hello felhőszolgáltatásban runbookpéldányok futása után.</span><span class="sxs-lookup"><span data-stu-id="6775d-217">Your subscription only gets charged for Azure nodes after hello instances are running in hello cloud service.</span></span>

<span data-ttu-id="6775d-218">Kövesse a lépéseket tooadd két kis csomópont.</span><span class="sxs-lookup"><span data-stu-id="6775d-218">Follow these steps tooadd two Small nodes.</span></span>

1. <span data-ttu-id="6775d-219">A HPC-kezelőben kattintson **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack aktuális verziói) > **csomópont hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6775d-219">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack) > **Add Node**.</span></span>
   
    ![Csomópont hozzáadása][add_node1]

2. <span data-ttu-id="6775d-221">A hello csomópont hozzáadása varázsló, a hello **módszer kiválasztása** kattintson **hozzáadása Windows Azure-csomópontokra**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6775d-221">In hello Add Node Wizard, on hello **Select Deployment Method** page, click **Add Windows Azure nodes**, and then click **Next**.</span></span>
   
    ![Az Azure csomópont hozzáadása][add_node1_1]

3. <span data-ttu-id="6775d-223">A hello **adja meg az új csomópontok** lapra, jelölje be hello Azure csomópontsablonhoz a korábban létrehozott (alapértelmezés szerint nevű **alapértelmezett AzureNode sablon**).</span><span class="sxs-lookup"><span data-stu-id="6775d-223">On hello **Specify New Nodes** page, select hello Azure node template you created previously (called by default **Default AzureNode Template**).</span></span> <span data-ttu-id="6775d-224">Adja meg **2** méretű csomópontok **kis**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="6775d-224">Then specify **2** nodes of size **Small**, and then click **Next**.</span></span>
   
    ![Adja meg a csomópontok][add_node2]
   
4. <span data-ttu-id="6775d-226">A hello **befejezése hello csomópont hozzáadása varázsló** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="6775d-226">On hello **Completing hello Add Node Wizard** page, click **Finish**.</span></span>
    
     <span data-ttu-id="6775d-227">Két Azure-csomópontok, nevű **AzureCN-0001** és **AzureCN-0002**, mostantól a HPC Cluster Manager jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6775d-227">Two Azure nodes, named **AzureCN-0001** and **AzureCN-0002**, now appear in HPC Cluster Manager.</span></span> <span data-ttu-id="6775d-228">Mindkettő a hello **nem telepített** állapotát.</span><span class="sxs-lookup"><span data-stu-id="6775d-228">Both are in hello **Not-Deployed** state.</span></span>
   
    ![Felvett csomópontjain][add_node3]

## <a name="start-hello-azure-nodes"></a><span data-ttu-id="6775d-230">Indítsa el az Azure csomópontok hello</span><span class="sxs-lookup"><span data-stu-id="6775d-230">Start hello Azure nodes</span></span>
<span data-ttu-id="6775d-231">Ha azt szeretné, toouse hello fürterőforrások HPC Cluster Manager toostart (kiépíteni) használja, az Azure-ban Azure-csomópontok hello és online állapotba kerüljön.</span><span class="sxs-lookup"><span data-stu-id="6775d-231">When you want toouse hello cluster resources in Azure, use HPC Cluster Manager toostart (provision) hello Azure nodes and bring them online.</span></span>

1. <span data-ttu-id="6775d-232">A HPC-kezelőben kattintson **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack aktuális verziói), és válassza ki az Azure-csomópontok hello.</span><span class="sxs-lookup"><span data-stu-id="6775d-232">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack), and select hello Azure nodes.</span></span>

2. <span data-ttu-id="6775d-233">Kattintson a **Start**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6775d-233">Click **Start**, and then click **OK**.</span></span>
   
   ![Kiinduló csomópont][add_node4]
   
    <span data-ttu-id="6775d-235">hello csomópontok átmenet toohello **kiépítési** állapotát.</span><span class="sxs-lookup"><span data-stu-id="6775d-235">hello nodes transition toohello **Provisioning** state.</span></span> <span data-ttu-id="6775d-236">Nézet hello kiépítés napló tootrack hello kiépítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="6775d-236">View hello provisioning log tootrack hello provisioning progress.</span></span>
   
    ![Kiépítés csomópontok][add_node6]

3. <span data-ttu-id="6775d-238">Néhány perc elteltével hello Azure-csomópontok üzembe helyezése és a hello **Offline** állapotát.</span><span class="sxs-lookup"><span data-stu-id="6775d-238">After a few minutes, hello Azure nodes finish provisioning and are in hello **Offline** state.</span></span> <span data-ttu-id="6775d-239">Ebben az állapotban lévő hello szerepkörpéldányokat futnak, de még nem a fürt feladatok fogadására.</span><span class="sxs-lookup"><span data-stu-id="6775d-239">In this state, hello role instances are running but cannot yet accept cluster jobs.</span></span>

4. <span data-ttu-id="6775d-240">amely a szerepkörpéldányok hello tooconfirm futnak, hello Azure-portálon, kattintson a **Felhőszolgáltatások (klasszikus)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="6775d-240">tooconfirm that hello role instances are running, in hello Azure portal, click **Cloud Services (classic)** > *your_cloud_service_name*.</span></span>
   
   <span data-ttu-id="6775d-241">Két kell megjelennie **HpcWorkerRole** hello szolgáltatásban futó példányok (csomópontok).</span><span class="sxs-lookup"><span data-stu-id="6775d-241">You should see two **HpcWorkerRole** instances (nodes) running in hello service.</span></span> <span data-ttu-id="6775d-242">HPC Pack is automatikusan telepíti a két **HpcProxy** (közepes méretű) toohandle kommunikációját hello átjárócsomópont és az Azure-példányok.</span><span class="sxs-lookup"><span data-stu-id="6775d-242">HPC Pack also automatically deploys two **HpcProxy** instances (size Medium) toohandle communication between hello head node and Azure.</span></span>

   ![Futó példányát tekintve][view_instances1]

5. <span data-ttu-id="6775d-244">toobring hello Azure-csomópontok online toorun feladatok, jelölje be hello csomópontot, kattintson a jobb gombbal, és kattintson a fürt **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="6775d-244">toobring hello Azure nodes online toorun cluster jobs, select hello nodes, right-click, and then click **Bring Online**.</span></span>
   
    ![Kapcsolat nélküli csomópontok][add_node7]
   
    <span data-ttu-id="6775d-246">HPC Fürtkezelő azt jelzi, hogy hello csomópontok a hello **Online** állapotát.</span><span class="sxs-lookup"><span data-stu-id="6775d-246">HPC Cluster Manager indicates that hello nodes are in hello **Online** state.</span></span>

## <a name="run-a-command-across-hello-cluster"></a><span data-ttu-id="6775d-247">A parancsok a hello fürtön</span><span class="sxs-lookup"><span data-stu-id="6775d-247">Run a command across hello cluster</span></span>
<span data-ttu-id="6775d-248">toocheck hello telepítése, használata hello HPC Pack **clusrun** parancs toorun parancsot vagy az alkalmazás egy vagy több fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="6775d-248">toocheck hello installation, use hello HPC Pack **clusrun** command toorun a command or application on one or more cluster nodes.</span></span> <span data-ttu-id="6775d-249">Egyszerű példaként használható **clusrun** tooget hello IP-konfigurációja, hello Azure-csomópontok.</span><span class="sxs-lookup"><span data-stu-id="6775d-249">As a simple example, use **clusrun** tooget hello IP configuration of hello Azure nodes.</span></span>

1. <span data-ttu-id="6775d-250">A hello átjárócsomópont nyisson meg egy parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6775d-250">On hello head node, open a command prompt as an administrator.</span></span>

2. <span data-ttu-id="6775d-251">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6775d-251">Type hello following command:</span></span>
   
    `clusrun /nodes:azurecn* ipconfig`

3. <span data-ttu-id="6775d-252">Ha a rendszer kéri, adja meg a fürt rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="6775d-252">If prompted, enter your cluster administrator password.</span></span> <span data-ttu-id="6775d-253">Meg kell jelennie a parancskimeneti hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="6775d-253">You should see command output similar toohello following.</span></span>
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a><span data-ttu-id="6775d-255">Egy tesztelési feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="6775d-255">Run a test job</span></span>
<span data-ttu-id="6775d-256">Most hello hibrid fürtön futó teszt feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="6775d-256">Now submit a test job that runs on hello hybrid cluster.</span></span> <span data-ttu-id="6775d-257">Ebben a példában egy egyszerű paraméteres ismétlés feladat (belsőleg párhuzamos számítási típusú).</span><span class="sxs-lookup"><span data-stu-id="6775d-257">This example is a simple parametric sweep job (a type of intrinsically parallel computation).</span></span> <span data-ttu-id="6775d-258">Ebben a példában fut, adja hozzá az egész tooitself hello segítségével résztevékenység **/a beállítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="6775d-258">This example runs subtasks that add an integer tooitself by using hello **set /a** command.</span></span> <span data-ttu-id="6775d-259">Hello fürt összes csomópontjának hello toofinishing hello résztevékenység az egész számok 1 too100 a függ.</span><span class="sxs-lookup"><span data-stu-id="6775d-259">All hello nodes in hello cluster contribute toofinishing hello subtasks for integers from 1 too100.</span></span>

1. <span data-ttu-id="6775d-260">A HPC-kezelőben kattintson **feladatkezelés** > **új paraméteres Sweep feladat**.</span><span class="sxs-lookup"><span data-stu-id="6775d-260">In HPC Cluster Manager, click **Job Management** > **New Parametric Sweep Job**.</span></span>
   
    ![Új feladat][test_job1]

2. <span data-ttu-id="6775d-262">A hello **új paraméteres Sweep feladat** párbeszédpanel **parancssori**, típus `set /a *+*` (felülírja a hello alapértelmezett parancssort, amely akkor jelenik meg).</span><span class="sxs-lookup"><span data-stu-id="6775d-262">In hello **New Parametric Sweep Job** dialog box, in **Command line**, type `set /a *+*` (overwriting hello default command line that appears).</span></span> <span data-ttu-id="6775d-263">Hagyja a fennmaradó beállításokat hello tartozó alapértelmezett értékeket, és kattintson **Submit** toosubmit hello feladat.</span><span class="sxs-lookup"><span data-stu-id="6775d-263">Leave default values for hello remaining settings, and then click **Submit** toosubmit hello job.</span></span>
   
    ![Paraméteres ismétlés][param_sweep1]

3. <span data-ttu-id="6775d-265">Ha hello feladat befejeződött, kattintson duplán a hello **saját Sweep feladat** feladat.</span><span class="sxs-lookup"><span data-stu-id="6775d-265">When hello job is finished, double-click hello **My Sweep Task** job.</span></span>

4. <span data-ttu-id="6775d-266">Kattintson a **nézet feladatai**, majd kattintson az adott részfeladatnál annak regisztrálása egy részfeladatnál annak regisztrálása tooview számított hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="6775d-266">Click **View Tasks**, and then click a subtask tooview hello calculated output of that subtask.</span></span>
   
    ![A feladat eredménye][view_job361]

5. <span data-ttu-id="6775d-268">melyik csomópontján hello számítási elvégzi a részfeladatnál annak regisztrálása, toosee kattintson **lefoglalt csomópontok**.</span><span class="sxs-lookup"><span data-stu-id="6775d-268">toosee which node performed hello calculation for that subtask, click **Allocated Nodes**.</span></span> <span data-ttu-id="6775d-269">(A fürt előfordulhat, hogy megjelenítése egy másik csomópont neve).</span><span class="sxs-lookup"><span data-stu-id="6775d-269">(Your cluster might show a different node name.)</span></span>
   
    ![A feladat eredménye][view_job362]

## <a name="stop-hello-azure-nodes"></a><span data-ttu-id="6775d-271">Állítsa le az Azure csomópontok hello</span><span class="sxs-lookup"><span data-stu-id="6775d-271">Stop hello Azure nodes</span></span>
<span data-ttu-id="6775d-272">Hello fürt kipróbálhatja, hello Azure-csomópontok tooavoid felesleges költségek tooyour fiók le.</span><span class="sxs-lookup"><span data-stu-id="6775d-272">After you try out hello cluster, stop hello Azure nodes tooavoid unnecessary charges tooyour account.</span></span> <span data-ttu-id="6775d-273">Ebben a lépésben hello felhőalapú szolgáltatás leáll, és eltávolítja a hello Azure szerepkörpéldányokat.</span><span class="sxs-lookup"><span data-stu-id="6775d-273">This step stops hello cloud service and removes hello Azure role instances.</span></span>

1. <span data-ttu-id="6775d-274">A HPC Cluster Manager, a **csomópont-felügyelet** (nevű **erőforrás-kezelés** HPC Pack korábbi verzióiban), válassza ki mindkét Azure-csomópontok.</span><span class="sxs-lookup"><span data-stu-id="6775d-274">In HPC Cluster Manager, in **Node Management** (called **Resource Management** in previous versions of HPC Pack), select both Azure nodes.</span></span> <span data-ttu-id="6775d-275">Kattintson a **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="6775d-275">Then, click **Stop**.</span></span>
   
    ![Csomópont leállítása][stop_node1]

2. <span data-ttu-id="6775d-277">A hello **állítsa le a Windows Azure-csomópontokra** párbeszédpanel, kattintson a **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="6775d-277">In hello **Stop Windows Azure nodes** dialog box, click **Stop**.</span></span> 
   
3. <span data-ttu-id="6775d-278">hello csomópontok átmenet toohello **leállítása** állapotát.</span><span class="sxs-lookup"><span data-stu-id="6775d-278">hello nodes transition toohello **Stopping** state.</span></span> <span data-ttu-id="6775d-279">Néhány perc elteltével HPC Cluster Manager azt mutatja, hogy hello csomópontok **nem telepített**.</span><span class="sxs-lookup"><span data-stu-id="6775d-279">After a few minutes, HPC Cluster Manager shows that hello nodes are **Not-Deployed**.</span></span>
   
    ![Nem telepített csomópontok][stop_node4]

4. <span data-ttu-id="6775d-281">hello szerepkörpéldányokat már nem az Azure, a futó tooconfirm hello Azure portálon kattintson **Felhőszolgáltatásokhoz (klasszikus)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="6775d-281">tooconfirm that hello role instances are no longer running in Azure, in hello Azure portal, click **Cloud services (classic)** > *your_cloud_service_name*.</span></span> <span data-ttu-id="6775d-282">Nincs példány hello éles környezetben vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="6775d-282">No instances are deployed in hello production environment.</span></span> 
   
    <span data-ttu-id="6775d-283">Ezzel befejezte a hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6775d-283">This completes hello tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6775d-284">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6775d-284">Next steps</span></span>
* <span data-ttu-id="6775d-285">Hello dokumentációja felfedezés [HPC Pack](https://technet.microsoft.com/library/cc514029).</span><span class="sxs-lookup"><span data-stu-id="6775d-285">Explore hello documentation for [HPC Pack](https://technet.microsoft.com/library/cc514029).</span></span>
* <span data-ttu-id="6775d-286">HPC Pack fürtöt tartalmazó környezetben, nagyobb léptékű hibrid mentése tooset lásd: [szerepkör Feldolgozópéldányok tooAzure Microsoft HPC Pack kapacitásnövelés](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="6775d-286">tooset up a hybrid HPC Pack cluster deployment at greater scale, see [Burst tooAzure Worker Role Instances with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
* <span data-ttu-id="6775d-287">Más módokon toocreate egy HPC Pack fürthöz az Azure-ban, beleértve az Azure Resource Manager-sablonokkal, lásd: [HPC-fürt beállítások Microsoft HPC Pack az Azure-ban](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6775d-287">For other ways toocreate an HPC Pack cluster in Azure, including using Azure Resource Manager templates, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


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
