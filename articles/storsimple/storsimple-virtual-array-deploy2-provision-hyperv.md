---
title: "A Hyper-v-ben a StorSimple virtuális tömb kiépítése |} Microsoft Docs"
description: "A StorSimple virtuális tömb telepítési második oktatóanyag magában foglalja a kiépítés a Hyper-V egy virtuális tömb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bad431c8958f7d381bb9c0410caa3a57c6e75c19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a><span data-ttu-id="2fb94-103">A StorSimple virtuális tömb - Provision a Hyper-V telepítése</span><span class="sxs-lookup"><span data-stu-id="2fb94-103">Deploy StorSimple Virtual Array - Provision in Hyper-V</span></span>
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a><span data-ttu-id="2fb94-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2fb94-104">Overview</span></span>
<span data-ttu-id="2fb94-105">Ez az oktatóanyag ismerteti, hogyan lehet kiépíteni a StorSimple virtuális tömb állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2fb94-105">This tutorial describes how to provision a StorSimple Virtual Array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span> <span data-ttu-id="2fb94-106">Ez a cikk a központi telepítést a StorSimple virtuális tömbök, Azure-portál és a Microsoft Azure Government felhő vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2fb94-106">This article applies to the deployment of StorSimple Virtual Arrays in Azure portal and Microsoft Azure Government Cloud.</span></span>

<span data-ttu-id="2fb94-107">Kiépítés rendszergazdai jogosultságokat igényel, és konfigurálja a virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="2fb94-107">You need administrator privileges to provision and configure a virtual array.</span></span> <span data-ttu-id="2fb94-108">Az üzembe helyezési és a kezdeti telepítés befejezéséhez körülbelül 10 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="2fb94-108">The provisioning and initial setup can take around 10 minutes to complete.</span></span>

## <a name="provisioning-prerequisites"></a><span data-ttu-id="2fb94-109">Telepítési előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2fb94-109">Provisioning prerequisites</span></span>
<span data-ttu-id="2fb94-110">Itt megtalálja az előfeltételek telepítéséhez egy virtuális tömb állomás operációs rendszert futtató Hyper-V a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2fb94-110">Here you will find the prerequisites to provision a virtual array on a host system running Hyper-V on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2.</span></span>

### <a name="for-the-storsimple-device-manager-service"></a><span data-ttu-id="2fb94-111">A StorSimple-eszközkezelő szolgáltatás esetén</span><span class="sxs-lookup"><span data-stu-id="2fb94-111">For the StorSimple Device Manager service</span></span>
<span data-ttu-id="2fb94-112">Mielőtt hozzákezd, győződjön meg az alábbiakról:</span><span class="sxs-lookup"><span data-stu-id="2fb94-112">Before you begin, make sure that:</span></span>

* <span data-ttu-id="2fb94-113">Összes lépését végrehajtotta [készítse elő a portál a StorSimple virtuális tömb](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="2fb94-113">You have completed all the steps in [Prepare the portal for StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>
* <span data-ttu-id="2fb94-114">A Hyper-V virtuális tömb kép már letöltötte az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="2fb94-114">You have downloaded the virtual array image for Hyper-V from the Azure portal.</span></span> <span data-ttu-id="2fb94-115">További információkért lásd: **3. lépés: Töltse le a virtuális tömb kép** a [készítse elő a portál a StorSimple virtuális tömb útmutató](storsimple-virtual-array-deploy1-portal-prep.md).</span><span class="sxs-lookup"><span data-stu-id="2fb94-115">For more information, see **Step 3: Download the virtual array image** of [Prepare the portal for StorSimple Virtual Array guide](storsimple-virtual-array-deploy1-portal-prep.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="2fb94-116">A StorSimple virtuális tömb futó szoftver csak a StorSimple Device Manager szolgáltatás is használhatók.</span><span class="sxs-lookup"><span data-stu-id="2fb94-116">The software running on the StorSimple Virtual Array may only be used with the StorSimple Device Manager service.</span></span>
  >
  >

### <a name="for-the-storsimple-virtual-array"></a><span data-ttu-id="2fb94-117">A StorSimple virtuális tömb</span><span class="sxs-lookup"><span data-stu-id="2fb94-117">For the StorSimple Virtual Array</span></span>
<span data-ttu-id="2fb94-118">Egy virtuális tömb telepítése előtt győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="2fb94-118">Before you deploy a virtual array, make sure that:</span></span>

* <span data-ttu-id="2fb94-119">Gazdagép operációs rendszert futtató Hyper-V a Windows Server 2008 R2 vagy újabb, amely hozzáféréssel rendelkezik a kiépítését használt eszköz.</span><span class="sxs-lookup"><span data-stu-id="2fb94-119">You have access to a host system running Hyper-V on Windows Server 2008 R2 or later that can be used to a provision a device.</span></span>
* <span data-ttu-id="2fb94-120">A gazdagép rendszere tudni jelölt ki a virtuális tömb telepítéséhez a következőket:</span><span class="sxs-lookup"><span data-stu-id="2fb94-120">The host system is able to dedicate the following resources to provision your virtual array:</span></span>

  * <span data-ttu-id="2fb94-121">Legalább 4 mag.</span><span class="sxs-lookup"><span data-stu-id="2fb94-121">A minimum of 4 cores.</span></span>
  * <span data-ttu-id="2fb94-122">Legalább 8 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="2fb94-122">At least 8 GB of RAM.</span></span> <span data-ttu-id="2fb94-123">Ha szeretné konfigurálni a virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="2fb94-123">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="2fb94-124">16 GB RAM, 2 – 4 millió fájlok támogatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2fb94-124">You need 16 GB RAM to support 2 - 4 million files.</span></span>
  * <span data-ttu-id="2fb94-125">Egy hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="2fb94-125">One network interface.</span></span>
  * <span data-ttu-id="2fb94-126">500 GB-os virtuális lemez adatait.</span><span class="sxs-lookup"><span data-stu-id="2fb94-126">A 500 GB virtual disk for data.</span></span>

### <a name="for-the-network-in-the-datacenter"></a><span data-ttu-id="2fb94-127">Az adatközpont hálózata esetén</span><span class="sxs-lookup"><span data-stu-id="2fb94-127">For the network in the datacenter</span></span>
<span data-ttu-id="2fb94-128">Mielőtt elkezdené, tekintse át a StorSimple virtuális tömb telepítéséhez, és az Adatközpont-hálózat megfelelő konfigurálása a hálózati követelmények.</span><span class="sxs-lookup"><span data-stu-id="2fb94-128">Before you begin, review the networking requirements to deploy a StorSimple Virtual Array and configure the datacenter network appropriately.</span></span> <span data-ttu-id="2fb94-129">További információkért lásd: [StorSimple virtuális tömb hálózati követelményei](storsimple-ova-system-requirements.md#networking-requirements).</span><span class="sxs-lookup"><span data-stu-id="2fb94-129">For more information, see [StorSimple Virtual Array networking requirements](storsimple-ova-system-requirements.md#networking-requirements).</span></span>

## <a name="step-by-step-provisioning"></a><span data-ttu-id="2fb94-130">Részletes kiépítése</span><span class="sxs-lookup"><span data-stu-id="2fb94-130">Step-by-step provisioning</span></span>
<span data-ttu-id="2fb94-131">A kiépítése, és kapcsolódjon a virtuális tömb, hajtsa végre a következő lépéseket kell:</span><span class="sxs-lookup"><span data-stu-id="2fb94-131">To provision and connect to a virtual array, you need to perform the following steps:</span></span>

1. <span data-ttu-id="2fb94-132">Győződjön meg arról, hogy a gazdagép rendszere a minimális virtuális tömb követelményeinek megfelelően elegendő erőforrással rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="2fb94-132">Ensure that the host system has sufficient resources to meet the minimum virtual array requirements.</span></span>
2. <span data-ttu-id="2fb94-133">Egy virtuális tömbhöz a hipervizor kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-133">Provision a virtual array in your hypervisor.</span></span>
3. <span data-ttu-id="2fb94-134">Indítsa el a virtuális tömb, és az IP-cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2fb94-134">Start the virtual array and get the IP address.</span></span>

<span data-ttu-id="2fb94-135">A lépések esetén, tekintse meg a következő szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="2fb94-135">Each of these steps is explained in the following sections.</span></span>

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a><span data-ttu-id="2fb94-136">1. lépés: Győződjön meg arról, hogy a gazdagép rendszere megfelel-e a virtuális tömb minimális követelményeknek</span><span class="sxs-lookup"><span data-stu-id="2fb94-136">Step 1: Ensure that the host system meets minimum virtual array requirements</span></span>
<span data-ttu-id="2fb94-137">Hozzon létre egy virtuális tömb, az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="2fb94-137">To create a virtual array, you need:</span></span>

* <span data-ttu-id="2fb94-138">A Hyper-V szerepkör telepítése a Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="2fb94-138">The Hyper-V role installed on Windows Server 2012 R2, Windows Server 2012, or Windows Server 2008 R2 SP1.</span></span>
* <span data-ttu-id="2fb94-139">Microsoft Hyper-V kezelőjével, a Microsoft Windows-ügyfél kapcsolódik a gazdagéphez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-139">Microsoft Hyper-V Manager on a Microsoft Windows client connected to the host.</span></span>

<span data-ttu-id="2fb94-140">Gondoskodjon arról, hogy a mögöttes hardver (gazdagép rendszere), amely létrehozza a virtuális tömb tudni jelölt ki a következő források a virtuális tömbhöz:</span><span class="sxs-lookup"><span data-stu-id="2fb94-140">Make sure that the underlying hardware (host system) on which you are creating the virtual array is able to dedicate the following resources to your virtual array:</span></span>

* <span data-ttu-id="2fb94-141">Legalább 4 mag.</span><span class="sxs-lookup"><span data-stu-id="2fb94-141">A minimum of 4 cores.</span></span>
* <span data-ttu-id="2fb94-142">Legalább 8 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="2fb94-142">At least 8 GB of RAM.</span></span> <span data-ttu-id="2fb94-143">Ha szeretné konfigurálni a virtuális tömb fájlkiszolgálóként, a 8 GB legalább 2 millió fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="2fb94-143">If you plan to configure the virtual array as file server, 8 GB supports less than 2 million files.</span></span> <span data-ttu-id="2fb94-144">16 GB RAM, 2 – 4 millió fájlok támogatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2fb94-144">You need 16 GB RAM to support 2 - 4 million files.</span></span>
* <span data-ttu-id="2fb94-145">Egy hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="2fb94-145">One network interface.</span></span>
* <span data-ttu-id="2fb94-146">500 GB-os virtuális lemez Rendszeradatok.</span><span class="sxs-lookup"><span data-stu-id="2fb94-146">A 500 GB virtual disk for system data.</span></span>

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a><span data-ttu-id="2fb94-147">2. lépés: A hipervizor egy virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="2fb94-147">Step 2: Provision a virtual array in hypervisor</span></span>
<span data-ttu-id="2fb94-148">A következő lépésekkel egy eszközt a hipervizor kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-148">Perform the following steps to provision a device in your hypervisor.</span></span>

#### <a name="to-provision-a-virtual-array"></a><span data-ttu-id="2fb94-149">Egy virtuális tömb kiépítése</span><span class="sxs-lookup"><span data-stu-id="2fb94-149">To provision a virtual array</span></span>
1. <span data-ttu-id="2fb94-150">A Windows Server-állomáson másolja át a virtuális tömb lemezképet egy helyi meghajtóra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-150">On your Windows Server host, copy the virtual array image to a local drive.</span></span> <span data-ttu-id="2fb94-151">Ez a rendszerkép (VHD- vagy VHDX-) letöltötte az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="2fb94-151">You downloaded this image (VHD or VHDX) through the Azure portal.</span></span> <span data-ttu-id="2fb94-152">Jegyezze fel a használ a kép az eljárás későbbi szakaszában, ahová másolása a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="2fb94-152">Make a note of the location where you copied the image as you are using this image later in the procedure.</span></span>
2. <span data-ttu-id="2fb94-153">Nyissa meg **Kiszolgálókezelő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-153">Open **Server Manager**.</span></span> <span data-ttu-id="2fb94-154">Kattintson a jobb felső sarokban, **eszközök** válassza **Hyper-V kezelője**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-154">In the top right corner, click **Tools** and select **Hyper-V Manager**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   <span data-ttu-id="2fb94-155">Ha Windows Server 2008 R2 fut, nyissa meg a Hyper-V kezelőt.</span><span class="sxs-lookup"><span data-stu-id="2fb94-155">If you are running Windows Server 2008 R2, open the Hyper-V Manager.</span></span> <span data-ttu-id="2fb94-156">A Kiszolgálókezelőben kattintson **szerepkörök > Hyper-V > Hyper-V kezelője**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-156">In Server Manager, click **Roles > Hyper-V > Hyper-V Manager**.</span></span>
3. <span data-ttu-id="2fb94-157">A **Hyper-V kezelője**, a hatókör ablaktáblán kattintson a jobb gombbal a csomópont a helyi menü megnyitásához, és kattintson a **új** > **virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-157">In **Hyper-V Manager**, in the scope pane, right-click your system node to open the context menu, and then click **New** > **Virtual Machine**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. <span data-ttu-id="2fb94-158">Az a **megkezdése előtt** lapon kattintson az új virtuális gép varázsló **következő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-158">On the **Before you begin** page of the New Virtual Machine Wizard, click **Next**.</span></span>
5. <span data-ttu-id="2fb94-159">Az a **név és hely megadása** lapján adjon meg egy **neve** a virtuális tömbhöz.</span><span class="sxs-lookup"><span data-stu-id="2fb94-159">On the **Specify name and location** page, provide a **Name** for your virtual array.</span></span> <span data-ttu-id="2fb94-160">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-160">Click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. <span data-ttu-id="2fb94-161">Az a **adnia generációját** lapon, válassza ki az eszköz kép típusát, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-161">On the **Specify generation** page, choose the device image type, and then click **Next**.</span></span> <span data-ttu-id="2fb94-162">Ez a lap nem jelenik meg, a Windows Server 2008 R2 használata esetén.</span><span class="sxs-lookup"><span data-stu-id="2fb94-162">This page doesn't appear if you're using Windows Server 2008 R2.</span></span>

   * <span data-ttu-id="2fb94-163">Válasszon **2. generációs** Ha letöltötte a .vhdx-lemezkép a Windows Server 2012 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2fb94-163">Choose **Generation 2** if you downloaded a .vhdx image for Windows Server 2012 or later.</span></span>
   * <span data-ttu-id="2fb94-164">Válasszon **1. generáció** Ha letöltötte a .vhd lemezképek a Windows Server 2008 R2 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2fb94-164">Choose **Generation 1** if you downloaded a .vhd image for Windows Server 2008 R2 or later.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. <span data-ttu-id="2fb94-165">Az a **memóriát rendelnek** adja meg azokat a **indítási memória** a legalább **8192 MB**, ne engedélyezze a dinamikus memóriát, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-165">On the **Assign memory** page, specify a **Startup memory** of at least **8192 MB**, don't enable dynamic memory, and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. <span data-ttu-id="2fb94-166">Az a **Hálózatkezelés beállítása** lapon, adja meg a virtuális kapcsoló, amely kapcsolódik az internethez, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-166">On the **Configure networking** page, specify the virtual switch that is connected to the Internet and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. <span data-ttu-id="2fb94-167">Az a **virtuális merevlemez csatlakoztatása** lapon, válassza ki **meglévő virtuális merevlemez használata**, adja meg a helyet a virtuális tömb kép (.vhdx vagy .vhd), és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-167">On the **Connect virtual hard disk** page, choose **Use an existing virtual hard disk**, specify the location of the virtual array image (.vhdx or .vhd), and then click **Next**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. <span data-ttu-id="2fb94-168">Tekintse át a **összegzés** majd **Befejezés** a virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2fb94-168">Review the **Summary** and then click **Finish** to create the virtual machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. <span data-ttu-id="2fb94-169">A minimális követelményeknek, 4 mag van szükség.</span><span class="sxs-lookup"><span data-stu-id="2fb94-169">To meet the minimum requirements, you need 4 cores.</span></span> <span data-ttu-id="2fb94-170">4 virtuális processzor hozzáadásához válassza a gazdagép rendszere a a **Hyper-V kezelője** ablak.</span><span class="sxs-lookup"><span data-stu-id="2fb94-170">To add 4 virtual processors, select your host system in the **Hyper-V Manager** window.</span></span> <span data-ttu-id="2fb94-171">A jobb oldali ablaktáblában listája alatt **virtuális gépek**, keresse meg az imént létrehozott virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2fb94-171">In the right-pane under the list of **Virtual Machines**, locate the virtual machine you just created.</span></span> <span data-ttu-id="2fb94-172">Válassza ki, és kattintson a jobb gombbal a számítógép nevét, majd **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-172">Select and right-click the machine name and select **Settings**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. <span data-ttu-id="2fb94-173">Az a **beállítások** oldalon, a bal oldali ablaktáblán, kattintson a **processzor**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-173">On the **Settings** page, in the left-pane, click **Processor**.</span></span> <span data-ttu-id="2fb94-174">A jobb oldali ablaktáblában, állítson be **a virtuális processzorok száma** 4 (vagy több).</span><span class="sxs-lookup"><span data-stu-id="2fb94-174">In the right-pane, set **number of virtual processors** to 4 (or more).</span></span> <span data-ttu-id="2fb94-175">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-175">Click **Apply**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. <span data-ttu-id="2fb94-176">A minimális követelmények teljesítéséhez is kell hozzáadnia 500 GB-os virtuális adatlemezt.</span><span class="sxs-lookup"><span data-stu-id="2fb94-176">To meet the minimum requirements, you also need to add a 500 GB virtual data disk.</span></span> <span data-ttu-id="2fb94-177">Az a **beállítások** lap:</span><span class="sxs-lookup"><span data-stu-id="2fb94-177">In the **Settings** page:</span></span>

    1. <span data-ttu-id="2fb94-178">A bal oldali panelen válassza ki a **SCSI-vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-178">In the left pane, select **SCSI Controller**.</span></span>
    2. <span data-ttu-id="2fb94-179">A jobb oldali ablaktáblában jelölje ki a **merevlemez,** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-179">In the right pane, select **Hard Drive,** and click **Add**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. <span data-ttu-id="2fb94-180">Az a **merevlemez-meghajtóról** lapon jelölje be a **virtuális merevlemez** lehetőséget, majd kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-180">On the **Hard drive** page, select the **Virtual hard disk** option and click **New**.</span></span> <span data-ttu-id="2fb94-181">A **új virtuális merevlemez varázsló** kezdődik.</span><span class="sxs-lookup"><span data-stu-id="2fb94-181">The **New Virtual Hard Disk Wizard** starts.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. <span data-ttu-id="2fb94-182">Az a **megkezdése előtt** lapon kattintson az új virtuális merevlemez varázsló **következő**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-182">On the **Before you begin** page of the New Virtual Hard Disk Wizard, click **Next**.</span></span>
16. <span data-ttu-id="2fb94-183">Az a **lemezformátum lap**, fogadja el az alapértelmezett beállítás a **VHDX** formátumban.</span><span class="sxs-lookup"><span data-stu-id="2fb94-183">On the **Choose Disk Format page**, accept the default option of **VHDX** format.</span></span> <span data-ttu-id="2fb94-184">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-184">Click **Next**.</span></span> <span data-ttu-id="2fb94-185">Ez a képernyő nem jelenik meg, ha a Windows Server 2008 R2 rendszerű.</span><span class="sxs-lookup"><span data-stu-id="2fb94-185">This screen is not presented if running Windows Server 2008 R2.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. <span data-ttu-id="2fb94-186">Az a **lemeztípus lap**, állítsa be a virtuális merevlemez típusát, **dinamikusan bővülő** (ajánlott).</span><span class="sxs-lookup"><span data-stu-id="2fb94-186">On the **Choose Disk Type page**, set virtual hard disk type as **Dynamically expanding** (recommended).</span></span> <span data-ttu-id="2fb94-187">**Rögzített méretű** lemez működik, de előfordulhat, hogy hosszú ideig várjon.</span><span class="sxs-lookup"><span data-stu-id="2fb94-187">**Fixed size** disk would work but you may need to wait a long time.</span></span> <span data-ttu-id="2fb94-188">Azt javasoljuk, hogy nem használja a **Differencing** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2fb94-188">We recommend that you do not use the **Differencing** option.</span></span> <span data-ttu-id="2fb94-189">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-189">Click **Next**.</span></span> <span data-ttu-id="2fb94-190">A Windows Server 2012 R2 és Windows Server 2012-ben **dinamikusan bővülő** lesz az alapértelmezett beállítás, mivel a Windows Server 2008 R2, az alapértelmezett érték **mérete rögzített**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-190">In Windows Server 2012 R2 and Windows Server 2012, **Dynamically expanding** is the default option whereas in Windows Server 2008 R2, the default is **Fixed size**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. <span data-ttu-id="2fb94-191">A a **név és hely megadása** lapján adjon meg egy **neve** , valamint **hely** (tallózással megkereshet egy) az adatok lemez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-191">On the **Specify Name and Location** page, provide a **name** as well as **location** (you can browse to one) for the data disk.</span></span> <span data-ttu-id="2fb94-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-192">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. <span data-ttu-id="2fb94-193">Az a **lemez beállítása** lapon, a beállításnak a **hozzon létre egy új üres virtuális merevlemez** , és adja meg a méret a **500 GB** (vagy több).</span><span class="sxs-lookup"><span data-stu-id="2fb94-193">On the **Configure Disk** page, select the option **Create a new blank virtual hard disk** and specify the size as **500 GB** (or more).</span></span> <span data-ttu-id="2fb94-194">A minimális követelmény 500 GB pedig egy nagyobb méretű lemezt mindig oszthat meg.</span><span class="sxs-lookup"><span data-stu-id="2fb94-194">While 500 GB is the minimum requirement, you can always provision a larger disk.</span></span> <span data-ttu-id="2fb94-195">Vegye figyelembe, hogy nem kibontása vagy Zsugorítás többször kiépített lemez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-195">Note that you cannot expand or shrink the disk once provisioned.</span></span> <span data-ttu-id="2fb94-196">Kiépítését lemez mérete további információkért tekintse át a méretezési részt a [ajánlott eljárások a dokumentum](storsimple-ova-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="2fb94-196">For more information on the size of disk to provision, review the sizing section in the [best practices document](storsimple-ova-best-practices.md).</span></span> <span data-ttu-id="2fb94-197">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2fb94-197">Click **Next**.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. <span data-ttu-id="2fb94-198">Az a **összegzés** lapon ellenőrizze a virtuális adatlemez részleteit, és ha mindent megfelelőnek talált, kattintson a **Befejezés** hozhat létre a lemezt.</span><span class="sxs-lookup"><span data-stu-id="2fb94-198">On the **Summary** page, review the details of your virtual data disk and if satisfied, click **Finish** to create the disk.</span></span> <span data-ttu-id="2fb94-199">A varázsló bezárása után, és a virtuális merevlemez ad hozzá a számítógépet.</span><span class="sxs-lookup"><span data-stu-id="2fb94-199">The wizard closes and a virtual hard disk is added to your machine.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. <span data-ttu-id="2fb94-200">Lépjen vissza a **beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="2fb94-200">Return to the **Settings** page.</span></span> <span data-ttu-id="2fb94-201">Kattintson a **OK** bezárásához a **beállítások** lapon, és visszatérhet a Hyper-V Manager ablakát.</span><span class="sxs-lookup"><span data-stu-id="2fb94-201">Click **OK** to close the **Settings** page and return to Hyper-V Manager window.</span></span>

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a><span data-ttu-id="2fb94-202">3. lépés: Indítsa el a virtuális tömb, és az IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="2fb94-202">Step 3: Start the virtual array and get the IP</span></span>
<span data-ttu-id="2fb94-203">A következő lépésekkel indítsa el a virtuális tömb és kapcsolódni hozzá.</span><span class="sxs-lookup"><span data-stu-id="2fb94-203">Perform the following steps to start your virtual array and connect to it.</span></span>

#### <a name="to-start-the-virtual-array"></a><span data-ttu-id="2fb94-204">A virtuális tömb elejéig</span><span class="sxs-lookup"><span data-stu-id="2fb94-204">To start the virtual array</span></span>
1. <span data-ttu-id="2fb94-205">Indítsa el a virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="2fb94-205">Start the virtual array.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. <span data-ttu-id="2fb94-206">Miután az eszköz fut, válassza ki az eszközt, kattintson a jobb gombbal és válassza ki **Connect**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-206">After the device is running, select the device, right click, and select **Connect**.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. <span data-ttu-id="2fb94-207">Előfordulhat, hogy készen áll arra, hogy az eszköz 5 – 10 percig is várakoznia.</span><span class="sxs-lookup"><span data-stu-id="2fb94-207">You may have to wait 5-10 minutes for the device to be ready.</span></span> <span data-ttu-id="2fb94-208">Állapotüzenet jelenik meg a konzol jelzik, hogy.</span><span class="sxs-lookup"><span data-stu-id="2fb94-208">A status message is displayed on the console to indicate the progress.</span></span> <span data-ttu-id="2fb94-209">Miután az eszköz készen áll, lépjen **művelet**.</span><span class="sxs-lookup"><span data-stu-id="2fb94-209">After the device is ready, go to **Action**.</span></span> <span data-ttu-id="2fb94-210">Nyomja le az `Ctrl + Alt + Delete` bejelentkezni a virtuális tömb.</span><span class="sxs-lookup"><span data-stu-id="2fb94-210">Press `Ctrl + Alt + Delete` to log in to the virtual array.</span></span> <span data-ttu-id="2fb94-211">Az alapértelmezett felhasználó *StorSimpleAdmin* és az alapértelmezett jelszó *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="2fb94-211">The default user is *StorSimpleAdmin* and the default password is *Password1*.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. <span data-ttu-id="2fb94-212">Biztonsági okokból az eszköz rendszergazdai jelszava lejár, az első bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="2fb94-212">For security reasons, the device administrator password expires at the first logon.</span></span> <span data-ttu-id="2fb94-213">Módosítsa a jelszavát kéri.</span><span class="sxs-lookup"><span data-stu-id="2fb94-213">You are prompted to change the password.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   <span data-ttu-id="2fb94-214">Adjon meg legalább 8 karakterből álló jelszót.</span><span class="sxs-lookup"><span data-stu-id="2fb94-214">Enter a password that contains at least 8 characters.</span></span> <span data-ttu-id="2fb94-215">A jelszót meg kell felelnie legalább 3 kívül az alábbi 4 követelményeknek: nagybetűk, nagybetűk, numerikus és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="2fb94-215">The password must satisfy at least 3 out of the following 4 requirements: uppercase, lowercase, numeric, and special characters.</span></span> <span data-ttu-id="2fb94-216">Adja meg újra a jelszót a megerősítéshez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-216">Reenter the password to confirm it.</span></span> <span data-ttu-id="2fb94-217">Értesítés jelenik meg, hogy a jelszó megváltozott.</span><span class="sxs-lookup"><span data-stu-id="2fb94-217">You are notified that the password has changed.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. <span data-ttu-id="2fb94-218">A jelszó sikeresen módosítását követően a virtuális tömb újraindulhat.</span><span class="sxs-lookup"><span data-stu-id="2fb94-218">After the password is successfully changed, the virtual array may restart.</span></span> <span data-ttu-id="2fb94-219">Várjon, amíg az eszköz elindításához.</span><span class="sxs-lookup"><span data-stu-id="2fb94-219">Wait for the device to start.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    <span data-ttu-id="2fb94-220">A Windows PowerShell-konzolon, az eszköz együtt egy folyamatjelző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2fb94-220">The Windows PowerShell console of the device is displayed along with a progress bar.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. <span data-ttu-id="2fb94-221">6 – 8. lépések csak akkor alkalmazható, ha a-DHCP környezetben rendszerindítást.</span><span class="sxs-lookup"><span data-stu-id="2fb94-221">Steps 6-8 only apply when booting up in a non-DHCP environment.</span></span> <span data-ttu-id="2fb94-222">Ha egy DHCP-környezetben, hagyja ki ezeket a lépéseket, és folytassa a 9.</span><span class="sxs-lookup"><span data-stu-id="2fb94-222">If you are in a DHCP environment, then skip these steps and go to step 9.</span></span> <span data-ttu-id="2fb94-223">Ha az eszköz nem DHCP-környezet telepítése rendszerindításhoz, látni fogja a következő képernyő.</span><span class="sxs-lookup"><span data-stu-id="2fb94-223">If you booted up your device in non-DHCP environment, you will see the following screen.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    <span data-ttu-id="2fb94-224">Ezután konfigurálja a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="2fb94-224">Next, configure the network.</span></span>
7. <span data-ttu-id="2fb94-225">Használja a `Get-HcsIpAddress` paranccsal listát készíthet a hálózati csatolót engedélyezni kell a virtuális tömbben.</span><span class="sxs-lookup"><span data-stu-id="2fb94-225">Use the `Get-HcsIpAddress` command to list the network interfaces enabled on your virtual array.</span></span> <span data-ttu-id="2fb94-226">Ha az eszköz egyetlen hálózati adapter engedélyezve van, az alapértelmezés szerint ez az interfész rendelt ez `Ethernet`.</span><span class="sxs-lookup"><span data-stu-id="2fb94-226">If your device has a single network interface enabled, the default name assigned to this interface is `Ethernet`.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. <span data-ttu-id="2fb94-227">Használja a `Set-HcsIpAddress` parancsmagot, hogy a hálózat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2fb94-227">Use the `Set-HcsIpAddress` cmdlet to configure the network.</span></span> <span data-ttu-id="2fb94-228">Tekintse meg a következő példát:</span><span class="sxs-lookup"><span data-stu-id="2fb94-228">See the following example:</span></span>

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. <span data-ttu-id="2fb94-229">A kezdeti telepítés befejezése után, az eszköz fel betöltődött, látni fogja az eszköz szalagcím szöveg.</span><span class="sxs-lookup"><span data-stu-id="2fb94-229">After the initial setup is complete and the device has booted up, you will see the device banner text.</span></span> <span data-ttu-id="2fb94-230">Jegyezze fel az IP-cím és az URL-címet a szalagcím szövegként jelenik meg az eszköz felügyeletére.</span><span class="sxs-lookup"><span data-stu-id="2fb94-230">Make a note of the IP address and the URL displayed in the banner text to manage the device.</span></span> <span data-ttu-id="2fb94-231">Az IP-cím segítségével csatlakozzon a webes felhasználói felület, a virtuális tömb, és fejezze be a helyi telepítését és regisztrálását.</span><span class="sxs-lookup"><span data-stu-id="2fb94-231">Use this IP address to connect to the web UI of your virtual array and complete the local setup and registration.</span></span>

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. <span data-ttu-id="2fb94-232">(Választható) Hajtsa végre ezt a lépést, csak akkor, ha az eszköz a kormányzati felhőben telepíti.</span><span class="sxs-lookup"><span data-stu-id="2fb94-232">(Optional) Perform this step only if you are deploying your device in the Government Cloud.</span></span> <span data-ttu-id="2fb94-233">Most már az eszközön az Amerikai Egyesült Államokban Federal Information Processing Standard (FIPS) mód lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="2fb94-233">You will now enable the United States Federal Information Processing Standard (FIPS) mode on your device.</span></span> <span data-ttu-id="2fb94-234">A FIPS 140 szabvány határozza meg a bizalmas adatok védelmének amerikai szövetségi kormányzati számítógépes rendszerek által jóváhagyott titkosítási algoritmusokat.</span><span class="sxs-lookup"><span data-stu-id="2fb94-234">The FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for the protection of sensitive data.</span></span>

    1. <span data-ttu-id="2fb94-235">Ahhoz, hogy a FIPS-módban, futtassa az alábbi parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="2fb94-235">To enable the FIPS mode, run the following cmdlet:</span></span>

        `Enable-HcsFIPSMode`
    2. <span data-ttu-id="2fb94-236">Indítsa újra az eszközt, engedélyeznie kell a FIPS-módban, hogy a kriptográfiai ellenőrzések érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="2fb94-236">Reboot your device after you have enabled the FIPS mode so that the cryptographic validations take effect.</span></span>

       > [!NOTE]
       > <span data-ttu-id="2fb94-237">Engedélyezheti vagy letilthatja a FIPS-módban az eszközön.</span><span class="sxs-lookup"><span data-stu-id="2fb94-237">You can either enable or disable FIPS mode on your device.</span></span> <span data-ttu-id="2fb94-238">Váltakozó az eszköz közötti FIPS és a FIPS módban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2fb94-238">Alternating the device between FIPS and non-FIPS mode is not supported.</span></span>
       >
       >

<span data-ttu-id="2fb94-239">Ha az eszköz nem felel meg a minimális konfigurációs követelményeinek, tekintse meg a következő hiba a szalagcím szöveg (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="2fb94-239">If your device does not meet the minimum configuration requirements, you see the following error in the banner text (shown below).</span></span> <span data-ttu-id="2fb94-240">Módosítsa az eszközök konfigurációját úgy, hogy a gép meg a minimális követelményeknek megfelelő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2fb94-240">Modify the device configuration so that the machine has adequate resources to meet the minimum requirements.</span></span> <span data-ttu-id="2fb94-241">Ezután indítsa újra, és csatlakozzon az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="2fb94-241">You can then restart and connect to the device.</span></span> <span data-ttu-id="2fb94-242">Tekintse meg a minimális konfigurációs követelményeinek [1. lépés: Győződjön meg arról, hogy a gazdagép rendszere megfelel-e a minimális virtuális tömb követelmények](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span><span class="sxs-lookup"><span data-stu-id="2fb94-242">Refer to the minimum configuration requirements in [Step 1: Ensure that the host system meets minimum virtual array requirements](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).</span></span>

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

<span data-ttu-id="2fb94-243">Ha a helyi webes felhasználói felület használata a kezdeti konfiguráció során bármilyen hiba akkor szembesülhetnek, tekintse meg a következő munkafolyamatok:</span><span class="sxs-lookup"><span data-stu-id="2fb94-243">If you face any other error during the initial configuration using the local web UI, refer to the following workflows:</span></span>

* <span data-ttu-id="2fb94-244">Diagnosztikai tesztek futtatása annak [webes felhasználói felületen való beállításának hibaelhárítása](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span><span class="sxs-lookup"><span data-stu-id="2fb94-244">Run diagnostic tests to [troubleshoot web UI setup](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).</span></span>
* <span data-ttu-id="2fb94-245">[Napló csomagot és naplófájlban](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="2fb94-245">[Generate log package and view log files](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fb94-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2fb94-246">Next steps</span></span>
* [<span data-ttu-id="2fb94-247">Állítsa be a StorSimple virtuális tömb fájlkiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="2fb94-247">Set up your StorSimple Virtual Array as a file server</span></span>](storsimple-virtual-array-deploy3-fs-setup.md)
* [<span data-ttu-id="2fb94-248">Állítsa be a StorSimple virtuális tömb iSCSI-kiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="2fb94-248">Set up your StorSimple Virtual Array as an iSCSI server</span></span>](storsimple-virtual-array-deploy3-iscsi-setup.md)
