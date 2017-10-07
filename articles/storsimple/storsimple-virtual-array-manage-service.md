---
title: "StorSimple Device Manager szolgáltatás aaaDeploy |} Microsoft Docs"
description: "Azt ismerteti, hogyan toocreate és delete hello hello Azure-portálon a StorSimple eszköz kezelő szolgáltatás, és ismerteti, hogyan toomanage hello szolgáltatás regisztrációs kulcsának."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 9064053addc7b3dfcce08b47e81b38c2e0e1b559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-virtual-array"></a><span data-ttu-id="65985-103">A StorSimple virtuális tömb hello StorSimple Device Manager szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="65985-103">Deploy hello StorSimple Device Manager service for StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="65985-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="65985-104">Overview</span></span>

<span data-ttu-id="65985-105">hello StorSimple Device Manager szolgáltatás fut a Microsoft Azure-ban, és kapcsolódik toomultiple StorSimple eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="65985-105">hello StorSimple Device Manager service runs in Microsoft Azure and connects toomultiple StorSimple devices.</span></span> <span data-ttu-id="65985-106">Miután létrehozta a hello szolgáltatást, használhatja toomanage hello eszközök hello Microsoft Azure portál futtatása böngészőben.</span><span class="sxs-lookup"><span data-stu-id="65985-106">After you create hello service, you can use it toomanage hello devices from hello Microsoft Azure portal running in a browser.</span></span> <span data-ttu-id="65985-107">Ez lehetővé teszi egyetlen, központi helyről, ezáltal az adminisztratív terhek minimalizálása szolgáltatás összes hello eszköz csatlakoztatott toohello StorSimple Device Manager toomonitor.</span><span class="sxs-lookup"><span data-stu-id="65985-107">This allows you toomonitor all hello devices that are connected toohello StorSimple Device Manager service from a single, central location, thereby minimizing administrative burden.</span></span>

<span data-ttu-id="65985-108">hello gyakori feladatok kapcsolódó tooa StorSimple Device Manager szolgáltatás a következők:</span><span class="sxs-lookup"><span data-stu-id="65985-108">hello common tasks related tooa StorSimple Device Manager service are:</span></span>

* <span data-ttu-id="65985-109">Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="65985-109">Create a service</span></span>
* <span data-ttu-id="65985-110">A szolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="65985-110">Delete a service</span></span>
* <span data-ttu-id="65985-111">Hello Szolgáltatásregisztrációs kulcs lekérése</span><span class="sxs-lookup"><span data-stu-id="65985-111">Get hello service registration key</span></span>
* <span data-ttu-id="65985-112">Hello Szolgáltatásregisztrációs kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="65985-112">Regenerate hello service registration key</span></span>

<span data-ttu-id="65985-113">Ez az oktatóanyag leírja, hogyan tooperform minden a hello előző feladatok.</span><span class="sxs-lookup"><span data-stu-id="65985-113">This tutorial describes how tooperform each of hello preceding tasks.</span></span> <span data-ttu-id="65985-114">Ebben a cikkben szereplő hello információ alkalmazható csak tooStorSimple virtuális tömbök.</span><span class="sxs-lookup"><span data-stu-id="65985-114">hello information contained in this article is applicable only tooStorSimple Virtual Arrays.</span></span> <span data-ttu-id="65985-115">További információ a StorSimple 8000 Series: túl[StorSimple Manager szolgáltatás telepítése](storsimple-manage-service.md).</span><span class="sxs-lookup"><span data-stu-id="65985-115">For more information on StorSimple 8000 series, go too[deploy a StorSimple Manager service](storsimple-manage-service.md).</span></span>

## <a name="create-a-service"></a><span data-ttu-id="65985-116">Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="65985-116">Create a service</span></span>

<span data-ttu-id="65985-117">egy szolgáltatás toocreate, toohave kell:</span><span class="sxs-lookup"><span data-stu-id="65985-117">toocreate a service, you need toohave:</span></span>

* <span data-ttu-id="65985-118">Nagyvállalati szerződéssel rendelkező előfizetés</span><span class="sxs-lookup"><span data-stu-id="65985-118">A subscription with an Enterprise Agreement</span></span>
* <span data-ttu-id="65985-119">Egy aktív Microsoft Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="65985-119">An active Microsoft Azure storage account</span></span>
* <span data-ttu-id="65985-120">hello kezelési használt számlázási adatokat</span><span class="sxs-lookup"><span data-stu-id="65985-120">hello billing information that is used for access management</span></span>

<span data-ttu-id="65985-121">Másik lehetőségként toogenerate tárfiók hello szolgáltatás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="65985-121">You can also choose toogenerate a storage account when you create hello service.</span></span>

<span data-ttu-id="65985-122">Egyetlen szolgáltatás több eszközöket kezelheti.</span><span class="sxs-lookup"><span data-stu-id="65985-122">A single service can manage multiple devices.</span></span> <span data-ttu-id="65985-123">Azonban egy eszköz nem terjedhetnek ki több szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="65985-123">However, a device cannot span multiple services.</span></span> <span data-ttu-id="65985-124">A nagyvállalatok rendelkezhet több szolgáltatás példányok toowork különböző előfizetések, a szervezetek vagy még a központi telepítési helyét.</span><span class="sxs-lookup"><span data-stu-id="65985-124">A large enterprise can have multiple service instances toowork with different subscriptions, organizations, or even deployment locations.</span></span>

> [!NOTE]
> <span data-ttu-id="65985-125">StorSimple Device Manager szolgáltatás toomanage StorSimple 8000 sorozat eszközeire és a StorSimple virtuális tömbök külön példányának van szüksége.</span><span class="sxs-lookup"><span data-stu-id="65985-125">You need separate instances of StorSimple Device Manager service toomanage StorSimple 8000 series devices and StorSimple Virtual Arrays.</span></span>


<span data-ttu-id="65985-126">Hajtsa végre a következő lépéseket toocreate szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="65985-126">Perform hello following steps toocreate a service.</span></span>

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a><span data-ttu-id="65985-127">A szolgáltatás törlése</span><span class="sxs-lookup"><span data-stu-id="65985-127">Delete a service</span></span>

<span data-ttu-id="65985-128">A szolgáltatás törlése előtt győződjön meg arról, hogy nem csatlakoztatott eszközök-k használják.</span><span class="sxs-lookup"><span data-stu-id="65985-128">Before you delete a service, make sure that no connected devices are using it.</span></span> <span data-ttu-id="65985-129">Ha hello szolgáltatást használja, inaktiválása hello csatlakoztatott eszközöket.</span><span class="sxs-lookup"><span data-stu-id="65985-129">If hello service is in use, deactivate hello connected devices.</span></span> <span data-ttu-id="65985-130">hello inaktiválása művelet Server hello kapcsolatot hello eszköz és a hello szolgáltatást, de hello felhőben hello eszköz adatok megőrzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="65985-130">hello deactivate operation will sever hello connection between hello device and hello service, but preserve hello device data in hello cloud.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65985-131">Szolgáltatás törlését követően hello művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="65985-131">After a service is deleted, hello operation cannot be reversed.</span></span> <span data-ttu-id="65985-132">Minden olyan eszköz, amely hello szolgáltatás használatával lett kell toobe gyári beállításokra történő visszaállításának ahhoz, hogy egy másik szolgáltatást kell használni.</span><span class="sxs-lookup"><span data-stu-id="65985-132">Any device that was using hello service will need toobe factory reset before it can be used with another service.</span></span> <span data-ttu-id="65985-133">Ebben a forgatókönyvben hello hello eszköz, valamint hello konfigurációs helyi adatok elvesznek.</span><span class="sxs-lookup"><span data-stu-id="65985-133">In this scenario, hello local data on hello device, as well as hello configuration, will be lost.</span></span>
 

<span data-ttu-id="65985-134">Hajtsa végre a következő lépéseket toodelete szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="65985-134">Perform hello following steps toodelete a service.</span></span>

#### <a name="toodelete-a-service"></a><span data-ttu-id="65985-135">a szolgáltatás toodelete</span><span class="sxs-lookup"><span data-stu-id="65985-135">toodelete a service</span></span>

1. <span data-ttu-id="65985-136">Nyissa meg túl**összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="65985-136">Go too**All resources**.</span></span> <span data-ttu-id="65985-137">Keresse meg a StorSimple eszköz Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="65985-137">Search for your StorSimple Device Manager service.</span></span> <span data-ttu-id="65985-138">Válassza ki, hogy kívánja-e toodelete hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="65985-138">Select hello service that you wish toodelete.</span></span>
   
    ![Válassza ki a szolgáltatás toodelete](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. <span data-ttu-id="65985-140">Nyissa meg tooyour szolgáltatás irányítópult tooensure nincsenek egyetlen eszköz sem kapcsolódó toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="65985-140">Go tooyour service dashboard tooensure there are no devices connected toohello service.</span></span> <span data-ttu-id="65985-141">Ha nincsenek a szolgáltatáshoz regisztrált eszközök, ezenkívül megjelenik egy szalagcím üzenet toohello hatást.</span><span class="sxs-lookup"><span data-stu-id="65985-141">If there are no devices registered with this service, you will also see a banner message toohello effect.</span></span> <span data-ttu-id="65985-142">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="65985-142">Click **Delete**.</span></span>
   
    ![Szolgáltatás törlése](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. <span data-ttu-id="65985-144">Amikor felszólítja a megerősítésre, kattintson **Igen** hello megerősítési értesítés.</span><span class="sxs-lookup"><span data-stu-id="65985-144">When prompted for confirmation, click **Yes** in hello confirmation notification.</span></span> 
   
    ![Szolgáltatás törlésének megerősítése](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. <span data-ttu-id="65985-146">Hello szolgáltatás toobe törölt néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="65985-146">It may take a few minutes for hello service toobe deleted.</span></span> <span data-ttu-id="65985-147">Után hello szolgáltatás sikeresen törlődik, értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="65985-147">After hello service is successfully deleted, you will be notified.</span></span>
   
    ![Sikeres szolgáltatás törlése](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

<span data-ttu-id="65985-149">szolgáltatások hello listája frissítve lesz.</span><span class="sxs-lookup"><span data-stu-id="65985-149">hello list of services will be refreshed.</span></span>

 ![Szolgáltatás frissített listája](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-hello-service-registration-key"></a><span data-ttu-id="65985-151">Hello Szolgáltatásregisztrációs kulcs lekérése</span><span class="sxs-lookup"><span data-stu-id="65985-151">Get hello service registration key</span></span>
<span data-ttu-id="65985-152">Sikeresen létrehozott egy szolgáltatást, akkor a StorSimple eszköz hello szolgáltatásban tooregister.</span><span class="sxs-lookup"><span data-stu-id="65985-152">After you have successfully created a service, you will need tooregister your StorSimple device with hello service.</span></span> <span data-ttu-id="65985-153">tooregister az első StorSimple eszköz meg fogja kell hello szolgáltatás regisztrációs kulcsának.</span><span class="sxs-lookup"><span data-stu-id="65985-153">tooregister your first StorSimple device, you will need hello service registration key.</span></span> <span data-ttu-id="65985-154">egy meglévő StorSimple szolgáltatás további eszközök tooregister, szüksége lesz hello regisztrációs kulcs és a hello szolgáltatásadat-titkosítási kulcs (amely a regisztráció során van hello első eszközön létre).</span><span class="sxs-lookup"><span data-stu-id="65985-154">tooregister additional devices with an existing StorSimple service, you will need both hello registration key and hello service data encryption key (which is generated on hello first device during registration).</span></span> <span data-ttu-id="65985-155">További információ a szolgáltatásadat-titkosítási kulcs hello: [StorSimple biztonsági](storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="65985-155">For more information about hello service data encryption key, see [StorSimple security](storsimple-security.md).</span></span> <span data-ttu-id="65985-156">Hello regisztrációs kulcs kaphat hello elérésével **kulcsok** panel a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="65985-156">You can get hello registration key by accessing hello **Keys** blade for your service.</span></span>

<span data-ttu-id="65985-157">Hajtsa végre a következő lépéseket tooget hello szolgáltatás regisztrációs kulcsának hello.</span><span class="sxs-lookup"><span data-stu-id="65985-157">Perform hello following steps tooget hello service registration key.</span></span>

#### <a name="tooget-hello-service-registration-key"></a><span data-ttu-id="65985-158">tooget hello Szolgáltatásregisztrációs kulcs</span><span class="sxs-lookup"><span data-stu-id="65985-158">tooget hello service registration key</span></span>
1. <span data-ttu-id="65985-159">A hello **StorSimple Device Manager** paneljén lépjen túl**felügyeleti &gt;**  **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="65985-159">In hello **StorSimple Device Manager** blade, go too**Management &gt;** **Keys**.</span></span>
   
   ![Kulcsok panel](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="65985-161">A hello **kulcsok** panelen, a szolgáltatás regisztrációs kulcsának jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="65985-161">In hello **Keys** blade, a service registration key appears.</span></span> <span data-ttu-id="65985-162">Hello másolás ikon használata hello regisztrációs kulcs másolása.</span><span class="sxs-lookup"><span data-stu-id="65985-162">Copy hello registration key using hello copy icon.</span></span> 

<span data-ttu-id="65985-163">Hello szolgáltatás regisztrációs kulcsának tartsa biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="65985-163">Keep hello service registration key in a safe location.</span></span> <span data-ttu-id="65985-164">Szüksége lesz a kulcs, valamint hello szolgáltatásadat-titkosítási kulcs, tooregister további eszközöket a szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="65985-164">You will need this key, as well as hello service data encryption key, tooregister additional devices with this service.</span></span> <span data-ttu-id="65985-165">Miután beszerezte a hello szolgáltatás regisztrációs kulcsának, szüksége lesz tooconfigure hello Windows PowerShell az eszköz StorSimple felületet.</span><span class="sxs-lookup"><span data-stu-id="65985-165">After obtaining hello service registration key, you will need tooconfigure your device through hello Windows PowerShell for StorSimple interface.</span></span>

## <a name="regenerate-hello-service-registration-key"></a><span data-ttu-id="65985-166">Hello Szolgáltatásregisztrációs kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="65985-166">Regenerate hello service registration key</span></span>
<span data-ttu-id="65985-167">Szüksége lesz a szolgáltatás regisztrációs kulcsának tooregenerate szükséges tooperform kulcs elforgatás vagy ha a szolgáltatás-rendszergazdák hello listája megváltozott-e.</span><span class="sxs-lookup"><span data-stu-id="65985-167">You will need tooregenerate a service registration key if you are required tooperform key rotation or if hello list of service administrators has changed.</span></span> <span data-ttu-id="65985-168">Amikor újragenerálja hello kulcsot, hello új kulccsal csak a következő eszközök regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="65985-168">When you regenerate hello key, hello new key is used only for registering subsequent devices.</span></span> <span data-ttu-id="65985-169">már regisztrált hello eszközöket nem érinti ez a folyamat.</span><span class="sxs-lookup"><span data-stu-id="65985-169">hello devices that were already registered are unaffected by this process.</span></span>

<span data-ttu-id="65985-170">Hajtsa végre a következő lépéseket tooregenerate a szolgáltatás regisztrációs kulcsának hello.</span><span class="sxs-lookup"><span data-stu-id="65985-170">Perform hello following steps tooregenerate a service registration key.</span></span>

#### <a name="tooregenerate-hello-service-registration-key"></a><span data-ttu-id="65985-171">tooregenerate hello Szolgáltatásregisztrációs kulcs</span><span class="sxs-lookup"><span data-stu-id="65985-171">tooregenerate hello service registration key</span></span>
1. <span data-ttu-id="65985-172">A hello **StorSimple Device Manager** paneljén lépjen túl**felügyeleti &gt;**  **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="65985-172">In hello **StorSimple Device Manager** blade, go too**Management &gt;** **Keys**.</span></span>
   
   ![Kulcsok panel](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. <span data-ttu-id="65985-174">A hello **kulcsok** panelen kattintson a **újragenerálja**.</span><span class="sxs-lookup"><span data-stu-id="65985-174">In hello **Keys** blade, click **Regenerate**.</span></span>
   
   ![Kattintson a újragenerálása](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. <span data-ttu-id="65985-176">A hello **Szolgáltatásregisztrációs kulcs újragenerálása** panelt, tekintse át a hello művelet kötelező, ha a hello kulcsok újragenerálása vannak.</span><span class="sxs-lookup"><span data-stu-id="65985-176">In hello **Regenerate service registration key** blade, review hello action required when hello keys are regenerated.</span></span> <span data-ttu-id="65985-177">A szolgáltatáshoz regisztrált összes hello további eszközök hello új regisztrációs kulcsot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="65985-177">All hello subsequent devices that are registered with this service will use hello new registration key.</span></span> <span data-ttu-id="65985-178">Kattintson a **újragenerálja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="65985-178">Click **Regenerate** tooconfirm.</span></span> <span data-ttu-id="65985-179">Értesítést fog kapni hello regisztráció befejezése után.</span><span class="sxs-lookup"><span data-stu-id="65985-179">You will be notified after hello registration is complete.</span></span>
   
   ![Ellenőrizze a kulcs újragenerálása](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. <span data-ttu-id="65985-181">Egy új Szolgáltatásregisztrációs kulcs jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="65985-181">A new service registration key will appear.</span></span>
   
    ![Ellenőrizze a kulcs újragenerálása](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   <span data-ttu-id="65985-183">Másolja ki ezt a kulcsot, és mentse az új eszköz regisztrálása ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="65985-183">Copy this key and save it for registering any new devices with this service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65985-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65985-184">Next steps</span></span>
* <span data-ttu-id="65985-185">Ismerje meg, hogyan túl[Ismerkedés](storsimple-virtual-array-deploy1-portal-prep.md) egy StorSimple virtuális tömbbel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="65985-185">Learn how too[get started](storsimple-virtual-array-deploy1-portal-prep.md) with a StorSimple Virtual Array.</span></span>
* <span data-ttu-id="65985-186">Ismerje meg, hogyan túl[felügyelete a StorSimple eszköz](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="65985-186">Learn how too[administer your StorSimple device](storsimple-ova-web-ui-admin.md).</span></span>

