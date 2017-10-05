---
title: "Kiépíteni a StorSimple virtuális tömb VMware-ben |} Microsoft Docs"
description: "A StorSimple virtuális tömb telepítési sorozat második oktatóanyag magában foglalja a VMware-ben a virtuális eszköz kiépítése."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 118521a127b2e4b765efabdbdde71605440d81c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a><span data-ttu-id="a0bbc-103">A StorSimple virtuális tömb - Provision VMware-ben telepítése</span><span class="sxs-lookup"><span data-stu-id="a0bbc-103">Deploy StorSimple Virtual Array - Provision in VMware</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a><span data-ttu-id="a0bbc-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a0bbc-104">Overview</span></span>
<span data-ttu-id="a0bbc-105">Ez az oktatóanyag ismerteti, hogyan szeretnék telepíteni, és csatlakozzon a StorSimple virtuális tömb a gazdagéphez, VMware ESXi 5.5 rendszerű vagy újabb rendszeren.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-105">This tutorial describes how to provision and connect to a StorSimple Virtual Array on a host system running VMware ESXi 5.5 and above.</span></span> <span data-ttu-id="a0bbc-106">Ez a cikk a központi telepítést a StorSimple virtuális tömbök, Azure-portál és a Microsoft Azure Government felhő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-106">This article applies to the deployment of StorSimple Virtual Arrays in Azure portal and the Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="a0bbc-107">Kiépítés rendszergazdai jogosultságokat igényel, és csatlakozzon a virtuális eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-107">You need administrator privileges to provision and connect to a virtual device.</span></span> <span data-ttu-id="a0bbc-108">Az üzembe helyezési és a kezdeti telepítés befejezéséhez körülbelül 10 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-108">The provisioning and initial setup can take around 10 minutes to complete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="a0bbc-109">Telepítési előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a0bbc-109">Provisioning prerequisites</span></span>
<span data-ttu-id="a0bbc-110">VMware ESXi 5.5 rendszerű gazdagép rendszeren és újabb verzióiban a virtuális eszköz kiépítése Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-110">The prerequisites to provision a virtual device on a host system running VMware ESXi 5.5 and above, are as follows.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="a0bbc-111">A StorSimple-eszközkezelő szolgáltatás esetén</span><span class="sxs-lookup"><span data-stu-id="a0bbc-111">For the StorSimple Device Manager service</span></span>
<span data-ttu-id="a0bbc-112">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="a0bbc-113">Összes lépését végrehajtotta [készítse elő a portál a StorSimple virtuális tömb](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-113">You have completed all the steps in [Prepare the portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="a0bbc-114">A virtuális eszköz kép VMware az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-114">You have downloaded the virtual device image for VMware from the Azure portal.</span></span> <span data-ttu-id="a0bbc-115">További információkért lásd: **3. lépés: Töltse le a virtuális eszköz kép** a [készítse elő a portál a StorSimple virtuális tömb útmutató](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-115">For more information, see **Step 3: Download the virtual device image** of [Prepare the portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

### <a name="for-the-storsimple-virtual-device"></a><span data-ttu-id="a0bbc-116">A StorSimple virtuális eszköz</span><span class="sxs-lookup"><span data-stu-id="a0bbc-116">For the StorSimple virtual device</span></span>
<span data-ttu-id="a0bbc-117">A virtuális eszköz telepítése előtt győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-117">Before you deploy a virtual device, make sure that:</span></span>

* <span data-ttu-id="a0bbc-118">Rendelkezik hozzáféréssel a rendszeren futó Hyper-V gazdagéphez (2008 R2 vagy újabb), amely lehet egy kiépítéséhez használt eszköz.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-118">You have access to a host system running Hyper-V (2008 R2 or later) that can be used to a provision a device.</span></span>
* <span data-ttu-id="a0bbc-119">A gazdagép rendszere tudni jelölt ki a virtuális eszköz létrehozásához a következőket:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-119">The host system is able to dedicate the following resources to provision your virtual device:</span></span>

  * <span data-ttu-id="a0bbc-120">Legalább 4 mag.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-120">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="a0bbc-121">Legalább 8 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-121">At least 8 GB of RAM.</span></span> <span data-ttu-id="a0bbc-122">Ha szeretné konfigurálni a virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-122">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="a0bbc-123">16 GB RAM, 2 – 4 millió fájlok támogatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-123">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="a0bbc-124">Egy hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-124">One network interface.</span></span>
  * <span data-ttu-id="a0bbc-125">500 GB-os virtuális lemez Rendszeradatok.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-125">A 500 GB virtual disk for system data.</span></span>

### <a name="for-the-network-in-datacenter"></a><span data-ttu-id="a0bbc-126">Az Adatközpont hálózati</span><span class="sxs-lookup"><span data-stu-id="a0bbc-126">For the network in datacenter</span></span>
<span data-ttu-id="a0bbc-127">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-127">Before you begin, make sure that:</span></span>

* <span data-ttu-id="a0bbc-128">A StorSimple virtuális eszköz telepítése a hálózati követelmények áttekintése és konfigurálni az adatközponti hálózathoz, a követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-128">You have reviewed the networking requirements to deploy a StorSimple virtual device and configured the datacenter network as per the requirements.</span></span> 

## <a name="step-by-step-provisioning"></a><span data-ttu-id="a0bbc-129">Részletes kiépítése</span><span class="sxs-lookup"><span data-stu-id="a0bbc-129">Step-by-step provisioning</span></span>
<span data-ttu-id="a0bbc-130">Ellátásához, majd csatlakozzon a virtuális eszközhöz, hajtsa végre a következő lépéseket kell:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-130">To provision and connect to a virtual device, you need to perform the following steps:</span></span>

1. <span data-ttu-id="a0bbc-131">Győződjön meg arról, hogy a gazdagép rendszere a virtuális eszköz minimális követelmények teljesítéséhez elegendő erőforrással rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-131">Ensure that the host system has sufficient resources to meet the minimum virtual device requirements.</span></span>
2. <span data-ttu-id="a0bbc-132">A hipervizor a virtuális eszköz kiépítése.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-132">Provision a virtual device in your hypervisor.</span></span>
3. <span data-ttu-id="a0bbc-133">Indítsa el a virtuális eszköz és az IP-cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-133">Start the virtual device and get the IP address.</span></span>

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a><span data-ttu-id="a0bbc-134">1. lépés: Győződjön meg arról, gazdagép rendszere megfelel a virtuális eszköz minimális követelményei</span><span class="sxs-lookup"><span data-stu-id="a0bbc-134">Step 1: Ensure host system meets minimum virtual device requirements</span></span>
<span data-ttu-id="a0bbc-135">A virtuális eszköz létrehozásához szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-135">To create a virtual device, you will need:</span></span>

* <span data-ttu-id="a0bbc-136">Hozzáférés a gazdagéphez, VMware ESXi Server 5.5 rendszerű vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-136">Access to a host system running VMware ESXi Server 5.5 and above.</span></span>
* <span data-ttu-id="a0bbc-137">VMware vSphere ügyfél, a rendszer kezeléséhez az ESXi-állomáson.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-137">VMware vSphere client on your system to manage the ESXi host.</span></span>

  * <span data-ttu-id="a0bbc-138">Legalább 4 mag.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-138">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="a0bbc-139">Legalább 8 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-139">At least 8 GB of RAM.</span></span> <span data-ttu-id="a0bbc-140">Ha szeretné konfigurálni a virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-140">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="a0bbc-141">16 GB RAM, 2 – 4 millió fájlok támogatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-141">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="a0bbc-142">Több hálózati adapter képes az internetre irányuló forgalom útválasztási hálózathoz csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-142">One network interface connected to the network capable of routing traffic to Internet.</span></span> <span data-ttu-id="a0bbc-143">A minimális internetes sávszélességet 5 MB/s, hogy az eszköz optimális működéséhez kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-143">The minimum Internet bandwidth should be 5 Mbps to allow for optimal working of the device.</span></span>
  * <span data-ttu-id="a0bbc-144">500 GB-os virtuális lemez adatait.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-144">A 500 GB virtual disk for data.</span></span>

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a><span data-ttu-id="a0bbc-145">2. lépés: A hipervizor virtuális eszköz kiépítése</span><span class="sxs-lookup"><span data-stu-id="a0bbc-145">Step 2: Provision a virtual device in hypervisor</span></span>
<span data-ttu-id="a0bbc-146">A következő lépésekkel a hipervizor a virtuális eszköz létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-146">Perform the following steps to provision a virtual device in your hypervisor.</span></span>

1. <span data-ttu-id="a0bbc-147">Másolja át a virtuális eszköz lemezképet, a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-147">Copy the virtual device image on your system.</span></span> <span data-ttu-id="a0bbc-148">A virtuális lemezképet, az Azure portálon keresztül letöltött.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-148">You downloaded this virtual image through the Azure portal.</span></span>

   1. <span data-ttu-id="a0bbc-149">Győződjön meg arról, hogy már letöltötte a legfrissebb lemezképfájlt.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-149">Ensure that you have downloaded the latest image file.</span></span> <span data-ttu-id="a0bbc-150">Ha korábban le a lemezképet, töltse le újra a győződjön meg arról, hogy rendelkezik-e a legújabb kép.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-150">If you downloaded the image earlier, download it again to ensure you have the latest image.</span></span> <span data-ttu-id="a0bbc-151">A legújabb lemezképben van két fájlt (helyett egy).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-151">The latest image has two files (instead of one).</span></span>
   2. <span data-ttu-id="a0bbc-152">Jegyezze fel a használ a kép az eljárás későbbi szakaszában, ahová másolása a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-152">Make a note of the location where you copied the image as you are using this image later in the procedure.</span></span>

2. <span data-ttu-id="a0bbc-153">Jelentkezzen be a vSphere-ügyfélprogrammal ESXi-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-153">Log in to the ESXi server using the vSphere client.</span></span> <span data-ttu-id="a0bbc-154">Virtuális gép létrehozása rendszergazdai jogosultsággal kell.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-154">You need to have administrator privileges to create a virtual machine.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. <span data-ttu-id="a0bbc-155">A vSphere ügyfél, a bal oldali ablaktáblán, a készlet szakaszban válassza ki az ESXi-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-155">In the vSphere client, in the inventory section in the left pane, select the ESXi Server.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. <span data-ttu-id="a0bbc-156">A vmdk-fájl feltöltése az ESXi-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-156">Upload the VMDK to the ESXi server.</span></span> <span data-ttu-id="a0bbc-157">Keresse meg a **konfigurációs** fülre a jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-157">Navigate to the **Configuration** tab in the right pane.</span></span> <span data-ttu-id="a0bbc-158">A **hardver**, jelölje be **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-158">Under **Hardware**, select **Storage**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. <span data-ttu-id="a0bbc-159">A jobb oldali ablaktáblában a **Datastores**, válassza ki az adattár feltöltése a vmdk-fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-159">In the right pane, under **Datastores**, select the datastore where you want to upload the VMDK.</span></span> <span data-ttu-id="a0bbc-160">Az adattárral az operációsrendszer- és adatlemezek elegendő szabad hellyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-160">The datastore must have enough free space for the OS and data disks.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. <span data-ttu-id="a0bbc-161">Kattintson a jobb gombbal, és válassza ki **Tallózás Datastore**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-161">Right-click and select **Browse Datastore**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. <span data-ttu-id="a0bbc-162">A **Datastore böngésző** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-162">A **Datastore Browser** window appears.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. <span data-ttu-id="a0bbc-163">Kattintson az eszköztáron ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) ikonra kattintva hozzon létre egy új mappát.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-163">In the tool bar, click ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) icon to create a new folder.</span></span> <span data-ttu-id="a0bbc-164">Adja meg a mappa nevét, és jegyezze fel a azt.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-164">Specify the folder name and make a note of it.</span></span> <span data-ttu-id="a0bbc-165">A mappa neve használandó később (ajánlott az ajánlott eljárás) virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-165">You will use this folder name later when creating a virtual machine (recommended best practice).</span></span> <span data-ttu-id="a0bbc-166">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-166">Click **OK**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. <span data-ttu-id="a0bbc-167">A bal oldali ablaktábláján megjelenik az új mappába a **Datastore böngésző**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-167">The new folder appears in the left pane of the **Datastore Browser**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. <span data-ttu-id="a0bbc-168">Kattintson a Feltöltés ikonra ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) válassza **fájl feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-168">Click the Upload icon ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) and select **Upload File**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. <span data-ttu-id="a0bbc-169">Keresse meg, és a letöltött VMDK fájlok.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-169">Browse and point to the VMDK files that you downloaded.</span></span> <span data-ttu-id="a0bbc-170">Két fájl van.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-170">There are two files.</span></span> <span data-ttu-id="a0bbc-171">Válassza ki a fájlt a feltöltéshez.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-171">Select a file to upload.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. <span data-ttu-id="a0bbc-172">Kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-172">Click **Open**.</span></span> <span data-ttu-id="a0bbc-173">Megkezdődik a feltöltés, a VMDK-fájl a megadott adattárral.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-173">The upload of the VMDK file to the specified datastore starts.</span></span> <span data-ttu-id="a0bbc-174">A feltöltendő fájl több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-174">It may take several minutes for the file to upload.</span></span>
13. <span data-ttu-id="a0bbc-175">A feltöltés befejeződése után megjelenik a fájl a mappában létrehozott az adattárral.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-175">After the upload is complete, you see the file in the datastore in the folder you created.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    <span data-ttu-id="a0bbc-176">Az azonos adattárral töltsön fel a második VMDK-fájl.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-176">Now upload the second VMDK file to the same datastore.</span></span>
14. <span data-ttu-id="a0bbc-177">Térjen vissza a vSphere client ablak.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-177">Return to the vSphere client window.</span></span> <span data-ttu-id="a0bbc-178">Kijelölt ESXi-kiszolgálóval, kattintson a jobb gombbal, és válassza ki **új virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-178">With ESXi server selected, right-click and select **New Virtual Machine**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. <span data-ttu-id="a0bbc-179">A **hozzon létre új virtuális gép** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-179">A **Create New Virtual Machine** window will appear.</span></span> <span data-ttu-id="a0bbc-180">Az a **konfigurációs** lapon jelölje be a **egyéni** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-180">On the **Configuration** page, select the **Custom** option.</span></span> <span data-ttu-id="a0bbc-181">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-181">Click **Next**.</span></span>
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. <span data-ttu-id="a0bbc-182">Az a **név és hely** adja meg azokat a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-182">On the **Name and Location** page, specify the name of your virtual machine.</span></span> <span data-ttu-id="a0bbc-183">Ezt a nevet meg kell felelnie a 8. lépés korábban megadott mappanév (ajánlott).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-183">This name should match the folder name (recommended best practice) you specified earlier in Step 8.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. <span data-ttu-id="a0bbc-184">Az a **tárolási** lapon, válassza ki a virtuális gép kiépítéséhez használni kívánt adattárolót.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-184">On the **Storage** page, select a datastore you want to use to provision your VM.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. <span data-ttu-id="a0bbc-185">Az a **virtuális gép verziójának** lapon jelölje be **virtuális gép verziójának: 8**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-185">On the **Virtual Machine Version** page, select **Virtual Machine Version: 8**.</span></span> <span data-ttu-id="a0bbc-186">8 – 11 verzió használatát támogatja.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-186">Versions 8 to 11 are all supported.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. <span data-ttu-id="a0bbc-187">Az a **vendég operációs rendszer** lapon jelölje be a **vendég operációs rendszer** , **Windows**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-187">On the **Guest Operating System** page, select the **Guest Operating System** as **Windows**.</span></span> <span data-ttu-id="a0bbc-188">A **verzió**, a legördülő listából válassza ki a **Microsoft Windows Server 2012 (64 bites)**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-188">For **Version**, from the dropdown list, select **Microsoft Windows Server 2012 (64-bit)**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. <span data-ttu-id="a0bbc-189">Az a **processzorok** lapján állítsa be a **virtuális szoftvercsatornák számát** és **virtuális szoftvercsatorna magok számát** , hogy a **magok száma** 4 (vagy több).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-189">On the **CPUs** page, adjust the **Number of virtual sockets** and **Number of cores per virtual socket** so that the **Total number of cores** is 4 (or more).</span></span> <span data-ttu-id="a0bbc-190">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-190">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. <span data-ttu-id="a0bbc-191">Az a **memória** csoportjában adja meg a 8 GB (vagy több) RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-191">On the **Memory** page, specify 8 GB (or more) of RAM.</span></span> <span data-ttu-id="a0bbc-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. <span data-ttu-id="a0bbc-193">Az a **hálózati** adja meg azokat a hálózati adapterek száma.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-193">On the **Network** page, specify the number of the network interfaces.</span></span> <span data-ttu-id="a0bbc-194">A minimális mérete legalább egy hálózati illesztőt.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-194">The minimum requirement is one network interface.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. <span data-ttu-id="a0bbc-195">Az a **SCSI-vezérlő** lap, fogadja el az alapértelmezett **LSI Logic vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-195">On the **SCSI Controller** page, accept the default **LSI Logic SAS controller**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. <span data-ttu-id="a0bbc-196">Az a **válasszon ki egy lemezt** lapon, válassza ki **meglévő virtuális lemez használata**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-196">On the **Select a Disk** page, choose **Use an existing virtual disk**.</span></span> <span data-ttu-id="a0bbc-197">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. <span data-ttu-id="a0bbc-198">Az a **meglévő lemez kiválasztása** lap **lemezt fájlútvonallal**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-198">On the **Select Existing Disk** page, under **Disk File Path**, click **Browse**.</span></span> <span data-ttu-id="a0bbc-199">Ekkor megnyílik egy **Tallózás Datastores** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-199">This opens a **Browse Datastores** dialog.</span></span> <span data-ttu-id="a0bbc-200">Keresse meg a helyet, ahol a VMDK feltöltött.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-200">Navigate to the location where you uploaded the VMDK.</span></span> <span data-ttu-id="a0bbc-201">Mivel a két fájl kezdetben feltöltött egyesített ekkor megjelenik az adattárral csak egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-201">You now see only one file in the datastore as the two files that you initially uploaded have been merged.</span></span> <span data-ttu-id="a0bbc-202">Válassza ki a fájlt, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-202">Select the file and click **OK**.</span></span> <span data-ttu-id="a0bbc-203">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-203">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. <span data-ttu-id="a0bbc-204">Az a **speciális beállítások** lapon fogadja el az alapértelmezett beállításokat, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-204">On the **Advanced Options** page, accept the default and click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. <span data-ttu-id="a0bbc-205">Az a **készen áll a Complete** lapján tekintse át az új virtuális géphez társított összes beállítást.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-205">On the **Ready to Complete** page, review all the settings associated with the new virtual machine.</span></span> <span data-ttu-id="a0bbc-206">Ellenőrizze **még a befejeződése előtt a virtuális gép beállításainak szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-206">Check **Edit the virtual machine settings before completion**.</span></span> <span data-ttu-id="a0bbc-207">Kattintson a **továbbra is**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-207">Click **Continue**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. <span data-ttu-id="a0bbc-208">Az a **virtuális gépek tulajdonságai** lap a **hardver** lapján keresse meg a hardver.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-208">On the **Virtual Machines Properties** page, in the **Hardware** tab, locate the device hardware.</span></span> <span data-ttu-id="a0bbc-209">Válassza ki **új merevlemez**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-209">Select **New Hard Disk**.</span></span> <span data-ttu-id="a0bbc-210">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-210">Click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. <span data-ttu-id="a0bbc-211">Megjelenik egy **hardver hozzáadása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-211">You see a **Add Hardware** window.</span></span> <span data-ttu-id="a0bbc-212">A a **eszköztípus** lap **válassza ki a hozzáadni kívánt eszköz**, jelölje be **merevlemez**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-212">On the **Device Type** page, under **Choose the type of device you wish to add**, select **Hard Disk**, and click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. <span data-ttu-id="a0bbc-213">Az a **válasszon ki egy lemezt** lapon, válassza ki **hozzon létre egy új virtuális lemez**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-213">On the **Select a Disk** page, choose **Create a new virtual disk**.</span></span> <span data-ttu-id="a0bbc-214">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-214">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. <span data-ttu-id="a0bbc-215">A a **lemez** lapján módosíthatja a **lemezméretet** 500 GB (vagy több).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-215">On the **Create a Disk** page, change the **Disk Size** to 500 GB (or more).</span></span> <span data-ttu-id="a0bbc-216">A minimális követelmény 500 GB pedig egy nagyobb méretű lemezt mindig oszthat meg.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-216">While 500 GB is the minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="a0bbc-217">Vegye figyelembe, hogy nem kibontása vagy Zsugorítás többször kiépített lemez.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-217">Note that you cannot expand or shrink the disk once provisioned.</span></span> <span data-ttu-id="a0bbc-218">Kiépítését lemez mérete további információkért tekintse át a méretezési részt a [ajánlott eljárások a dokumentum](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-218">For more information on the size of disk to provision, review the sizing section in the [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="a0bbc-219">A **lemez kiépítés**, jelölje be **vékony létesítési**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-219">Under **Disk Provisioning**, select **Thin Provision**.</span></span> <span data-ttu-id="a0bbc-220">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-220">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. <span data-ttu-id="a0bbc-221">Az a **speciális beállítások** lap, fogadja el az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-221">On the **Advanced Options** page, accept the default.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. <span data-ttu-id="a0bbc-222">Az a **készen áll a Complete** lapján tekintse át a lemezt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-222">On the **Ready to Complete** page, review the disk options.</span></span> <span data-ttu-id="a0bbc-223">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-223">Click **Finish**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. <span data-ttu-id="a0bbc-224">A virtuális gép tulajdonságainak laphoz való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-224">Return to the Virtual Machine Properties page.</span></span> <span data-ttu-id="a0bbc-225">A virtuális gép bekerül egy új merevlemezre.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-225">A new hard disk is added to your virtual machine.</span></span> <span data-ttu-id="a0bbc-226">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-226">Click **Finish**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. <span data-ttu-id="a0bbc-227">A virtuális géppel, a jobb oldali panelen kiválasztott, navigáljon a **összegzés** fülre.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-227">With your virtual machine selected in the right pane, navigate to the **Summary** tab.</span></span> <span data-ttu-id="a0bbc-228">Tekintse át a virtuális gép beállításait.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-228">Review the settings for your virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

<span data-ttu-id="a0bbc-229">A virtuális gép ezzel ki van építve.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-229">Your virtual machine is now provisioned.</span></span> <span data-ttu-id="a0bbc-230">A következő lépésre az IP-címet, és kapcsolja be ezt a gépet.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-230">The next step is to power on this machine and get the IP address.</span></span>

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a><span data-ttu-id="a0bbc-231">3. lépés: Indítsa el a virtuális eszköz és az IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="a0bbc-231">Step 3: Start the virtual device and get the IP</span></span>
<span data-ttu-id="a0bbc-232">A következő lépésekkel indítsa el a virtuális eszköz és kapcsolódni hozzá.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-232">Perform the following steps to start your virtual device and connect to it.</span></span>

#### <a name="to-start-the-virtual-device"></a><span data-ttu-id="a0bbc-233">A virtuális eszköz elindításához</span><span class="sxs-lookup"><span data-stu-id="a0bbc-233">To start the virtual device</span></span>
1. <span data-ttu-id="a0bbc-234">Indítsa el a virtuális eszköz.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-234">Start the virtual device.</span></span> <span data-ttu-id="a0bbc-235">A vSphere Configuration Manager, a bal oldali panelen válassza ki azt az eszközt, és kattintson a jobb gombbal a helyi menüből nyithat.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-235">In the vSphere Configuration Manager, in the left pane, select your device and right-click to bring up the context menu.</span></span> <span data-ttu-id="a0bbc-236">Válassza ki **Power** majd **Bekapcsolással**.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-236">Select **Power** and then select **Power on**.</span></span> <span data-ttu-id="a0bbc-237">Ez a virtuális gépen kell kapcsolja.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-237">This should power on your virtual machine.</span></span> <span data-ttu-id="a0bbc-238">Megtekintheti a állapotát alsó **legutóbbi feladatok** a vSphere client panelén.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-238">You can view the status in the bottom **Recent Tasks** pane of the vSphere client.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. <span data-ttu-id="a0bbc-239">A beállítási feladatok befejezéséhez néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-239">The setup tasks will take a few minutes to complete.</span></span> <span data-ttu-id="a0bbc-240">Ha az eszköz fut, nyissa meg a **konzol** fülre.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-240">Once the device is running, navigate to the **Console** tab.</span></span> <span data-ttu-id="a0bbc-241">Ctrl + Alt + Delete próbál bejelentkezni az eszköz küldése.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-241">Send Ctrl+Alt+Delete to log in to the device.</span></span> <span data-ttu-id="a0bbc-242">Másik lehetőségként a kurzort a konzolablakban a, és nyomja le a Ctrl + Alt + Insert.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-242">Alternatively, you can point the cursor on the console window and press Ctrl+Alt+Insert.</span></span> <span data-ttu-id="a0bbc-243">Az alapértelmezett felhasználó *StorSimpleAdmin* és az alapértelmezett jelszó *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-243">The default user is *StorSimpleAdmin* and the default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. <span data-ttu-id="a0bbc-244">Biztonsági okokból az eszköz rendszergazdai jelszava lejár, az első bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-244">For security reasons, the device administrator password expires at the first logon.</span></span> <span data-ttu-id="a0bbc-245">Módosítsa a jelszavát kéri.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-245">You are prompted to change the password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. <span data-ttu-id="a0bbc-246">Adjon meg legalább 8 karakterből álló jelszót.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-246">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="a0bbc-247">3 kívüli 4 ezeket a követelményeket a jelszóban: nagybetű, nagybetűk, numerikus és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-247">The password must contain 3 out of 4 of these requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="a0bbc-248">Adja meg újra a jelszót a megerősítéshez.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-248">Reenter the password to confirm it.</span></span> <span data-ttu-id="a0bbc-249">Értesítést fog kapni, hogy a jelszó megváltozott.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-249">You will be notified that the password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. <span data-ttu-id="a0bbc-250">A jelszó sikeresen módosította, a virtuális eszköz előfordulhat, hogy újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-250">After the password is successfully changed, the virtual device may reboot.</span></span> <span data-ttu-id="a0bbc-251">Várjon, amíg befejeződik az újraindítás.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-251">Wait for the reboot to complete.</span></span> <span data-ttu-id="a0bbc-252">A Windows PowerShell-konzolon, az eszköz a lehetséges, hogy együtt egy folyamatjelző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-252">The Windows PowerShell console of the device may be displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. <span data-ttu-id="a0bbc-253">6 – 8. lépések csak akkor alkalmazható, ha a-DHCP környezetben rendszerindítást.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-253">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="a0bbc-254">Ha egy DHCP-környezetben, hagyja ki ezeket a lépéseket, és folytassa a 9.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-254">If you are in a DHCP environment, then skip these steps and go to step 9.</span></span> <span data-ttu-id="a0bbc-255">Ha az eszköz nem DHCP-környezet telepítése rendszerindításhoz, látni fogja a következő képernyő.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-255">If you booted up your device in non-DHCP environment, you will see the following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   <span data-ttu-id="a0bbc-256">Ezután konfigurálja a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-256">Next, configure the network.</span></span>
7. <span data-ttu-id="a0bbc-257">Használja a `Get-HcsIpAddress` paranccsal listát készíthet a hálózati csatolót engedélyezni kell a virtuális eszközön.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-257">Use the `Get-HcsIpAddress` command to list the network interfaces enabled on your virtual device.</span></span> <span data-ttu-id="a0bbc-258">Ha az eszköz egyetlen hálózati adapter engedélyezve van, az alapértelmezés szerint ez az interfész rendelt ez `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-258">If your device has a single network interface enabled, the default name assigned to this interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. <span data-ttu-id="a0bbc-259">Használja a `Set-HcsIpAddress` parancsmagot, hogy a hálózat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-259">Use the `Set-HcsIpAddress` cmdlet to configure the network.</span></span> <span data-ttu-id="a0bbc-260">Példa alatt:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-260">An example is shown below:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. <span data-ttu-id="a0bbc-261">A kezdeti telepítés befejezése után, az eszköz fel betöltődött, látni fogja az eszköz szalagcím szöveg.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-261">After the initial setup is complete and the device has booted up, you will see the device banner text.</span></span> <span data-ttu-id="a0bbc-262">Jegyezze fel az IP-cím és az URL-címet a szalagcím szövegként jelenik meg az eszköz felügyeletére.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-262">Make a note of the IP address and the URL displayed in the banner text to manage the device.</span></span> <span data-ttu-id="a0bbc-263">Az IP-cím használatával fog a webes felhasználói felület, a virtuális eszköz csatlakozhat, és fejezze be a helyi telepítését és regisztrálását.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-263">You will use this IP address to connect to the web UI of your virtual device and complete the local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. <span data-ttu-id="a0bbc-264">(Választható) Hajtsa végre ezt a lépést, csak akkor, ha az eszköz a kormányzati felhőben telepíti.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-264">(Optional) Perform this step only if you are deploying your device in the Government Cloud.</span></span> <span data-ttu-id="a0bbc-265">Most már az eszközön az Amerikai Egyesült Államokban Federal Information Processing Standard (FIPS) mód lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-265">You will now enable the United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="a0bbc-266">A FIPS 140 szabvány határozza meg a bizalmas adatok védelmének amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-266">The FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for the protection of sensitive data.</span></span>

    1. <span data-ttu-id="a0bbc-267">Ahhoz, hogy a FIPS-módban, futtassa az alábbi parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-267">To enable the FIPS mode, run the following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="a0bbc-268">Indítsa újra az eszközt, engedélyeznie kell a FIPS-módban, hogy a kriptográfiai ellenőrzések érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-268">Reboot your device after you have enabled the FIPS mode so that the cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="a0bbc-269">Engedélyezheti vagy letilthatja a FIPS-módban az eszközön.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-269">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="a0bbc-270">Váltakozó az eszköz közötti FIPS és a FIPS módban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-270">Alternating the device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="a0bbc-271">Ha az eszköz nem felel meg a minimális konfigurációs követelményeinek, látni fogja a hibát jelez a fejléc szövege (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-271">If your device does not meet the minimum configuration requirements, you will see an error in the banner text (shown below).</span></span> <span data-ttu-id="a0bbc-272">Szüksége lesz az eszköz konfigurációjának módosítása, hogy a minimális követelményeknek megfelelő erőforrások.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-272">You will need to modify the device configuration so that it has adequate resources to meet the minimum requirements.</span></span> <span data-ttu-id="a0bbc-273">Ezután indítsa újra, és csatlakozzon az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="a0bbc-273">You can then restart and connect to the device.</span></span> <span data-ttu-id="a0bbc-274">Tekintse meg a minimális konfigurációs követelményeinek [1. lépés: Győződjön meg arról, hogy a gazdagép rendszere megfelel-e a virtuális eszköz minimális követelmények](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-274">Refer to the minimum configuration requirements in [Step 1: Ensure that the host system meets minimum virtual device requirements](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

<span data-ttu-id="a0bbc-275">Ha a helyi webes felhasználói felület használata a kezdeti konfiguráció során bármilyen hiba akkor szembesülhetnek, tekintse meg a következő munkafolyamatok:</span><span class="sxs-lookup"><span data-stu-id="a0bbc-275">If you face any other error during the initial configuration using the local web UI, refer to the following workflows:</span></span>

* <span data-ttu-id="a0bbc-276">Diagnosztikai tesztek futtatása annak [webes felhasználói felületen való beállításának hibaelhárítása](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-276">Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="a0bbc-277">[Napló csomagot és naplófájlban](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="a0bbc-277">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0bbc-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0bbc-278">Next steps</span></span>
* [<span data-ttu-id="a0bbc-279">Állítsa be a StorSimple virtuális tömb fájlkiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="a0bbc-279">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="a0bbc-280">Állítsa be a StorSimple virtuális tömb iSCSI-kiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="a0bbc-280">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
