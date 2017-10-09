---
title: "egy Azure Media Services-fiók használatával Aspera aaaUpload fájlok |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello fájlok feltöltése, amelyek segítségével hello Media Services-fiók van hozzárendelve a ** Aspera kiszolgáló az igény szerinti ** Azure szolgáltatást."
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 7213f016cc1b7f262b14db7b39b478a03970d1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-aspera-server-on-demand-service-on-azure"></a><span data-ttu-id="15781-103">Fájlok feltöltése a hello Aspera Server igény szerinti szolgáltatás használata az Azure Media Services-fiók</span><span class="sxs-lookup"><span data-stu-id="15781-103">Upload files into a Media Services account using hello Aspera Server On Demand service on Azure</span></span>

## <a name="overview"></a><span data-ttu-id="15781-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="15781-104">Overview</span></span>

<span data-ttu-id="15781-105">Az **Aspera** egy nagy sebességű fájlátvitelre szolgáló szoftver.</span><span class="sxs-lookup"><span data-stu-id="15781-105">**Aspera** is a high-speed file transfer software.</span></span> <span data-ttu-id="15781-106">Az Azure **Aspera Server On Demand** szolgáltatása lehetővé teszi a nagy fájlok nagy sebességű fel- és letöltését közvetlenül az Azure Blob objektumtárba.</span><span class="sxs-lookup"><span data-stu-id="15781-106">**Aspera Server On Demand** for Azure enables high-speed upload and download of large files directly into Azure Blob object storage.</span></span> <span data-ttu-id="15781-107">További információ **Aspera igény szerinti**, lásd: hello [Aspera felhő](http://cloud.asperasoft.com/) hely.</span><span class="sxs-lookup"><span data-stu-id="15781-107">For information about **Aspera On Demand**, see hello [Aspera Cloud](http://cloud.asperasoft.com/) site.</span></span> 
  
<span data-ttu-id="15781-108">**Az igény szerinti Server Aspera** Azure: hello vásárolni [Azure piactér](https://azure.microsoft.com/en-us/marketplace/).</span><span class="sxs-lookup"><span data-stu-id="15781-108">**Aspera Server On Demand** for Azure is available for purchase from hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/).</span></span> <span data-ttu-id="15781-109">A rendezés toocomplete vásárol **Aspera Server igény szerinti** az Azure-ba, jelentkezzen be Windows Live ID azonosítójával. az Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="15781-109">In order toocomplete a purchase of **Aspera Server On Demand** for Azure, please log into Azure Marketplace with your Windows Live ID.</span></span>

<span data-ttu-id="15781-110">Ez az oktatóanyag végigvezeti hello fájlok feltöltése, amelyek segítségével hello Media Services-fiók van hozzárendelve a **Aspera Server igény szerinti** Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="15781-110">This tutorial walks you through hello steps of uploading files into a storage account that is associated with a Media Services account using hello **Aspera Server On Demand** service on Azure.</span></span> 

<span data-ttu-id="15781-111">Példa bemutatja, hogyan toouse Azure functions Aspera és a Media Services található [Itt](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span><span class="sxs-lookup"><span data-stu-id="15781-111">You can find an example that shows how toouse Azure functions with Aspera and Media Services [here](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span></span>

>[!NOTE]
><span data-ttu-id="15781-112">Nincs az Azure Media Services feldolgozó media processzor (felügyeleti csomagok) támogatott korlátot toohello fájl maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="15781-112">There is a limit toohello maximum file size supported for processing with Azure Media Services media processors (MPs).</span></span> <span data-ttu-id="15781-113">Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="15781-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="15781-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15781-114">Prerequisites</span></span> 

<span data-ttu-id="15781-115">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="15781-115">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="15781-116">Egy Windows Live ID</span><span class="sxs-lookup"><span data-stu-id="15781-116">A Windows Live ID</span></span>
* <span data-ttu-id="15781-117">Egy [Azure-fiók](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="15781-117">An [Azure account](https://azure.microsoft.com).</span></span> <span data-ttu-id="15781-118">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15781-118">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="15781-119">Egy [Azure Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="15781-119">An [Azure Media Services account](media-services-portal-create-account.md).</span></span>

## <a name="purchase-aspera-on-demand-for-azure"></a><span data-ttu-id="15781-120">Az Azure-hoz készült Aspera On Demand megvásárlása</span><span class="sxs-lookup"><span data-stu-id="15781-120">Purchase Aspera On Demand for Azure</span></span>

<span data-ttu-id="15781-121">Amennyiben az Azure piactéren jelentkezett, kövesse az alábbi alapvető lépéseket toocomplete vásárlásakor Aspera igény szerinti az Azure.</span><span class="sxs-lookup"><span data-stu-id="15781-121">Once you have logged into Azure Marketplace,  follow these basic steps toocomplete your purchase of Aspera On Demand for Azure.</span></span>

1. <span data-ttu-id="15781-122">Keressen rá az Aspera kifejezésre, és válassza a „Server On Demand” lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="15781-122">Search for Aspera and select 'Server On Demand'.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. <span data-ttu-id="15781-124">Tekintse át a hello előfizetési csomag, majd kattintson a "Sign Up"</span><span class="sxs-lookup"><span data-stu-id="15781-124">Review hello subscription plans and click on 'Sign Up'</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. <span data-ttu-id="15781-126">Töltse ki hello részletekről a kiszolgáló igény szerint az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="15781-126">Fill in hello specifics for your Server on Demand subscription.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. <span data-ttu-id="15781-128">Kattintson a hello **Tarifacsomagot** és hello sub panelen jelölje ki a kívánt havi kötetre.</span><span class="sxs-lookup"><span data-stu-id="15781-128">Click on hello **Pricing Tier** and select your desired monthly volume in hello sub panel.</span></span> <span data-ttu-id="15781-129">A hello **részletek megtervezése** panelen, jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="15781-129">In hello **Plan details** panel, select **OK**.</span></span> <span data-ttu-id="15781-130">Ezt követően a hello **válassza ki a Tarifacsomagot** panelen, kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="15781-130">Then, in hello **Choose your Pricing Tier** panel, click **Select**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. <span data-ttu-id="15781-132">Kattintson a **jogi feltételeket** tooview, és fogadja el a jogi feltételeket hello hello sub panelen.</span><span class="sxs-lookup"><span data-stu-id="15781-132">Click on **Legal terms** tooview and accept hello legal terms in hello sub panel.</span></span> <span data-ttu-id="15781-133">Miután átolvasta hello jogi feltételeket, kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="15781-133">Once you have reviewed hello legal terms, click **Purchase**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. <span data-ttu-id="15781-135">Hello megvásárolhatja kattintva **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="15781-135">Complete hello purchase by clicking **Create**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. <span data-ttu-id="15781-137">hello Azure irányítópult fog értesíthetik hello szolgáltatást a(z) kiépítése.</span><span class="sxs-lookup"><span data-stu-id="15781-137">hello Azure dashboard will announce that it is provisioning hello service.</span></span>  <span data-ttu-id="15781-138">Ha szolgáltatáskiépítéssel befejeződött, megtalálhatja hello új előfizetés hello hello társított szolgáltatás neve az erőforrások megkeresése.</span><span class="sxs-lookup"><span data-stu-id="15781-138">Once it is completed with provisioning, you can find hello new subscription by searching in your resources for hello name of hello service.</span></span> <span data-ttu-id="15781-139">Miután megtalálta hello szolgáltatást, a dupla kattintás toolaunch hello Szolgáltatáskezelési portál.</span><span class="sxs-lookup"><span data-stu-id="15781-139">Once you have found hello service, double click on it toolaunch hello service management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. <span data-ttu-id="15781-141">Indítsa el a hello Aspera felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="15781-141">Launch hello Aspera management portal.</span></span> <span data-ttu-id="15781-142">Miután megtalálta az új Aspera szolgáltatást, hello szolgáltatás kattintva áttekintésével felmérheti, hozzáférési toohello felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="15781-142">Once you have found your new Aspera service, you can gain access toohello management portal, by clicking on hello service.</span></span>  <span data-ttu-id="15781-143">Ekkor egy új panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="15781-143">A new panel will be launched.</span></span> <span data-ttu-id="15781-144">A belül, hogy új panel van szüksége a hello tooclick **erőforrásnév** az új szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="15781-144">From within that new panel, you need tooclick on hello **Resource Name** of your new service.</span></span>  <span data-ttu-id="15781-145">A következő képernyőkép hello hello erőforrás neve: "AsperaTransferDemo".</span><span class="sxs-lookup"><span data-stu-id="15781-145">In hello following screenshot, hello resource name is 'AsperaTransferDemo'.</span></span> <span data-ttu-id="15781-146">Miután a hello erőforrás nevére kattint, egy másik panel nincs elindítva.</span><span class="sxs-lookup"><span data-stu-id="15781-146">Once you click on hello resource name, another panel is launched.</span></span> <span data-ttu-id="15781-147">Az újonnan megjelenő panelen látható a „Felügyelet” hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="15781-147">In that newly launched panel, you will see a 'Manage' link.</span></span> <span data-ttu-id="15781-148">Kattintson a hello "Kezelése" hivatkozást toolaunch hello Aspera felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="15781-148">Click on hello 'Manage' link toolaunch hello Aspera management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. <span data-ttu-id="15781-150">Hello kattintva kezelése hivatkozásra, elérhetővé válik a toohello regisztrációs lapjához, amely pedig szükséges tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="15781-150">By clicking on hello manage link, you will get toohello registration page, which is required tooaccess hello service.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. <span data-ttu-id="15781-152">Ezen a ponton rendelkeznie kell hozzáféréssel toohello Aspera felügyeleti portálon, ahol elérési kulcsok létrehozása, letöltheti Aspera ügyfelek és a licencek, tekintse meg és API-k hello megismerése.</span><span class="sxs-lookup"><span data-stu-id="15781-152">At this point, you should have access toohello Aspera service management portal, where you can create access keys, download Aspera clients and licenses, view usage and learn about hello APIs.</span></span>

    <span data-ttu-id="15781-153">hello alábbi képernyőfelvételen látható hello hozzáférés létrehozását.</span><span class="sxs-lookup"><span data-stu-id="15781-153">hello following screenshot shows hello access creation.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    <span data-ttu-id="15781-155">hello alábbi képernyőfelvételen látható hello használati hello portálon felületek jelentéskészítés.</span><span class="sxs-lookup"><span data-stu-id="15781-155">hello following screenshot shows hello usage reporting interfaces in hello portal.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a><span data-ttu-id="15781-157">Fájlok feltöltése az Asperával</span><span class="sxs-lookup"><span data-stu-id="15781-157">Upload files with Aspera</span></span>

1. <span data-ttu-id="15781-158">Töltse le és telepítse hello Aspera ügyfélszoftvert:</span><span class="sxs-lookup"><span data-stu-id="15781-158">Download and install hello Aspera client software:</span></span>
    
    * [<span data-ttu-id="15781-159">Böngészőbővítmény</span><span class="sxs-lookup"><span data-stu-id="15781-159">Browser plugin</span></span>](http://downloads.asperasoft.com/connect2/)
    * [<span data-ttu-id="15781-160">Funkciógazdag ügyfél</span><span class="sxs-lookup"><span data-stu-id="15781-160">Rich client</span></span>](http://downloads.asperasoft.com/en/downloads/2)

2. <span data-ttu-id="15781-161">Hajtsa végre az első átvitelt.</span><span class="sxs-lookup"><span data-stu-id="15781-161">Make your first transfer.</span></span> <span data-ttu-id="15781-162">A sorrend toouse hello Aspera ügyfél tootransfer hello Aspera adatátviteli szolgáltatás a toocomplete hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="15781-162">In order toouse hello Aspera client tootransfer with hello Aspera transfer service, you need toocomplete hello following:</span></span> 

    1. <span data-ttu-id="15781-163">Hívóbetű, hello Aspera portálon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="15781-163">Create an access key, using hello Aspera portal.</span></span>  
    2. <span data-ttu-id="15781-164">Letöltése, telepítése és licenc hello Aspera ügyfél (szoftver hello Aspera portálon találhatók).</span><span class="sxs-lookup"><span data-stu-id="15781-164">Download, install, and license hello Aspera client (software can be found in hello Aspera portal).</span></span>  

    >[!NOTE]
    ><span data-ttu-id="15781-165">Kérjük, olvassa el a hello Aspera ügyfél útmutató a konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="15781-165">Please read hello Aspera client guide for configuration information.</span></span>
    
    3. <span data-ttu-id="15781-166">Néhány adatbeolvasás a tárfiókja, amely kapcsolódik az Azure Media fiókja hello segítségével [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="15781-166">Retrieve some information of your storage account that is associated with your Azure Media Account using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="15781-167">Pontosabban, a neve, a kulcs és a hello storage blob-tároló nevének toowhich érdemes tooplace a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="15781-167">Specifically, name and key, and hello storage blob container name in toowhich you want tooplace your content.</span></span> 

        * <span data-ttu-id="15781-168">tooget hello tárolási info hello portálról: található a tárfiók, kattintson a hello elérési kulcsok és a példány hello nevét és a hello fiókja kulcsot.</span><span class="sxs-lookup"><span data-stu-id="15781-168">tooget hello storage info from hello portal: find your storage account, click on hello Access keys and copy hello name and hello key of your account.</span></span>
        * <span data-ttu-id="15781-169">tooget hello tároló neve: található a tárfiók, jelölje be **Blobok**, jelölje be a tooupload hello tartalom hello tároló hello neve.</span><span class="sxs-lookup"><span data-stu-id="15781-169">tooget hello container name: find your storage account, select **Blobs**, select hello name of hello container you want tooupload hello content into.</span></span> 

    <span data-ttu-id="15781-170">Az alábbiakban van hello képernyőfelvétel a hello Aspera ügyfél **Csatlakozáskezelő** ahol meg kell adnia hello "Azure" tárolási típusa és a hitelesítő adatokat, valamint a hello blob tároló.</span><span class="sxs-lookup"><span data-stu-id="15781-170">Below is hello screenshot of hello Aspera client **Connection Manager** where you must specify hello 'Azure' storage type and credentials as well as hello blob container.</span></span>

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a><span data-ttu-id="15781-172">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="15781-172">Resources</span></span>

<span data-ttu-id="15781-173">a következő erőforrások hello ebben a cikkben említett.</span><span class="sxs-lookup"><span data-stu-id="15781-173">hello following resources were mentioned in this article.</span></span> 

* [<span data-ttu-id="15781-174">Böngészőbővítmény csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="15781-174">Connect Browser Plugin</span></span>](http://downloads.asperasoft.com/connect2/)
* [<span data-ttu-id="15781-175">Csatlakoztatási útmutató</span><span class="sxs-lookup"><span data-stu-id="15781-175">Connect Guide</span></span>](http://downloads.asperasoft.com/en/documentation/8)
* [<span data-ttu-id="15781-176">Aspera-ügyfél</span><span class="sxs-lookup"><span data-stu-id="15781-176">Aspera Client</span></span>](http://downloads.asperasoft.com/en/downloads/2)
* [<span data-ttu-id="15781-177">Ügyfélútmutató</span><span class="sxs-lookup"><span data-stu-id="15781-177">Client Guide</span></span>](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a><span data-ttu-id="15781-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15781-178">Next steps</span></span>

<span data-ttu-id="15781-179">Most már tudja, hogyan [másolhat blobokat egy tárfiókból egy AMS-fiókba](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span><span class="sxs-lookup"><span data-stu-id="15781-179">You can now [copy blobs from a storage account into an AMS account ](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="15781-180">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="15781-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="15781-181">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="15781-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

