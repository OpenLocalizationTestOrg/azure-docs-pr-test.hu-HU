---
title: "Az Azure App Service egy felhőalapú mappából a szinkronizálási tartalom"
description: "Megtudhatja, hogyan telepítse az alkalmazást az Azure App Service tartalom szinkronizálási keresztül egy felhőalapú mappából."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 010e7dc492abefaa3afe814c0322af9f6fe5acd2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a><span data-ttu-id="de3b8-103">Az Azure App Service egy felhőalapú mappából a szinkronizálási tartalom</span><span class="sxs-lookup"><span data-stu-id="de3b8-103">Sync content from a cloud folder to Azure App Service</span></span>
<span data-ttu-id="de3b8-104">Az oktatóanyag bemutatja, hogyan lehet telepíteni [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) által népszerű tárolási felhőszolgáltatások, például a Dropbox és a onedrive vállalati verzió származó tartalom szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="de3b8-104">This tutorial shows you how to deploy to [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by syncing your content from popular cloud storage services like Dropbox and OneDrive.</span></span> 

## <span data-ttu-id="de3b8-105"><a name="overview"></a>Tartalom sync telepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="de3b8-105"><a name="overview"></a>Overview of content sync deployment</span></span>
<span data-ttu-id="de3b8-106">Az igény szerinti tartalom sync telepítési működteti a [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki) App Service szolgáltatással integrált.</span><span class="sxs-lookup"><span data-stu-id="de3b8-106">The on-demand content sync deployment is powered by the [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki) integrated with App Service.</span></span> <span data-ttu-id="de3b8-107">Az a [Azure Portal](https://portal.azure.com), meghatározhat egy mappát a felhőbeli tárolóhelyen, az alkalmazás és a mappában található tartalom, és az App Service szinkronizálhat egy kattintással.</span><span class="sxs-lookup"><span data-stu-id="de3b8-107">In the [Azure Portal](https://portal.azure.com), you can designate a folder in your cloud storage, work with your app code and content in that folder, and sync to App Service with the click of a button.</span></span> <span data-ttu-id="de3b8-108">Tartalom szinkronizálása a Kudu folyamata buildelés és üzembe helyezés használja.</span><span class="sxs-lookup"><span data-stu-id="de3b8-108">Content sync utilizes the Kudu process for build and deployment.</span></span> 

## <span data-ttu-id="de3b8-109"><a name="contentsync"></a>Tartalom szinkronizálási központi telepítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="de3b8-109"><a name="contentsync"></a>How to enable content sync deployment</span></span>
<span data-ttu-id="de3b8-110">Tartalom szinkronizálás engedélyezése a [Azure Portal](https://portal.azure.com), kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="de3b8-110">To enable content sync from the [Azure Portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="de3b8-111">Az Azure portálon az alkalmazás paneljén kattintson **beállítások** > **központi telepítés forrásának**.</span><span class="sxs-lookup"><span data-stu-id="de3b8-111">In your app's blade in the Azure Portal, click **Settings** > **Deployment Source**.</span></span> <span data-ttu-id="de3b8-112">Kattintson a **forrás választása**, majd jelölje be **OneDrive** vagy **Dropbox** telepítési forrásaként.</span><span class="sxs-lookup"><span data-stu-id="de3b8-112">Click **Choose Source**, then select **OneDrive** or **Dropbox** as the source for deployment.</span></span> 
   
    ![Tartalom szinkronizálása](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > <span data-ttu-id="de3b8-114">Az API-k, a mögöttes különbségek miatt **onedrive vállalati verzió** jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="de3b8-114">Because of underlying differences in the APIs, **OneDrive for Business** is not supported at this time.</span></span> 
   > 
   > 
2. <span data-ttu-id="de3b8-115">Végezze el az engedélyezési munkafolyamat ahhoz, hogy az App Service egy adott előre definiált megadott elérési úthoz való hozzáférésre a onedrive-on vagy a Dropbox összes, az App Service tartalmának tárolására.</span><span class="sxs-lookup"><span data-stu-id="de3b8-115">Complete the authorization workflow to enable App Service to access a specific pre-defined designated path for OneDrive or Dropbox where all of your App Service content will be stored.</span></span>  
    <span data-ttu-id="de3b8-116">Az App Service engedélyezést követően platform Erre azért van szükség a beállítás a kijelölt tartalom elérési úton található tartalom mappa létrehozásához, vagy kiválaszthat egy meglévő tartalom mappát a kijelölt tartalom elérési útját.</span><span class="sxs-lookup"><span data-stu-id="de3b8-116">After authorization the App Service platform will give you the option to create a content folder under the designated content path, or to choose an existing content folder under this designated content path.</span></span> <span data-ttu-id="de3b8-117">A kijelölt tartalom útvonalakat az App Service-szinkronizáláshoz használt cloud storage-fiókok a következők:</span><span class="sxs-lookup"><span data-stu-id="de3b8-117">The designated content paths under your cloud storage accounts used for App Service sync are the following:</span></span>  
   
   * <span data-ttu-id="de3b8-118">**Onedrive vállalati verzió**:`Apps\Azure Web Apps`</span><span class="sxs-lookup"><span data-stu-id="de3b8-118">**OneDrive**: `Apps\Azure Web Apps`</span></span> 
   * <span data-ttu-id="de3b8-119">**Dropbox**:`Dropbox\Apps\Azure`</span><span class="sxs-lookup"><span data-stu-id="de3b8-119">**Dropbox**: `Dropbox\Apps\Azure`</span></span>
3. <span data-ttu-id="de3b8-120">A kezdeti tartalom szinkronizálása után a tartalom szinkronizálása az Azure-portálról igény szerinti kezdeményezhető.</span><span class="sxs-lookup"><span data-stu-id="de3b8-120">After the initial content sync the content sync can be initiated on demand from the Azure portal.</span></span> <span data-ttu-id="de3b8-121">Telepítési előzmények esetén érhető el a **központi telepítések** panelen.</span><span class="sxs-lookup"><span data-stu-id="de3b8-121">Deployment history is available with the **Deployments** blade.</span></span>
   
    ![Telepítési előzmények](./media/app-service-deploy-content-sync/onedrive_sync.png)

<span data-ttu-id="de3b8-123">További információ a Dropbox telepítési érhető el a [központi telepítése a Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span><span class="sxs-lookup"><span data-stu-id="de3b8-123">More information for Dropbox deployment is available under [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span></span> 

