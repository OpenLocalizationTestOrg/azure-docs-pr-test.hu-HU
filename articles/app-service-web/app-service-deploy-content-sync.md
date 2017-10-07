---
title: "a felhő mappa tooAzure App Service aaaSync tartalmat"
description: "Ismerje meg, hogyan toodeploy az alkalmazás tooAzure App Service segítségével tartalom szinkronizálása felhő mappából."
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
ms.openlocfilehash: e1c6d53a427c36126d9cdb33cc21b4126b9d9c2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sync-content-from-a-cloud-folder-tooazure-app-service"></a><span data-ttu-id="b64c1-103">Szinkronizálási tartalmat egy felhőalapú mappa tooAzure App Service</span><span class="sxs-lookup"><span data-stu-id="b64c1-103">Sync content from a cloud folder tooAzure App Service</span></span>
<span data-ttu-id="b64c1-104">Az oktatóanyag bemutatja, hogyan toodeploy túl[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) által népszerű tárolási felhőszolgáltatások, például a Dropbox és a onedrive vállalati verzió származó tartalom szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="b64c1-104">This tutorial shows you how toodeploy too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by syncing your content from popular cloud storage services like Dropbox and OneDrive.</span></span> 

## <span data-ttu-id="b64c1-105"><a name="overview"></a>Tartalom sync telepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="b64c1-105"><a name="overview"></a>Overview of content sync deployment</span></span>
<span data-ttu-id="b64c1-106">a tárolt tartalom szinkronizálása telepítési hello hello működteti [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki) App Service szolgáltatással integrált.</span><span class="sxs-lookup"><span data-stu-id="b64c1-106">hello on-demand content sync deployment is powered by hello [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki) integrated with App Service.</span></span> <span data-ttu-id="b64c1-107">A hello [Azure Portal](https://portal.azure.com), akkor meghatározhat egy mappát a felhőbeli tárolóhelyen, az alkalmazás és a mappában található tartalom, és szinkronizálási tooApp szolgáltatás a hello kattintson gomb.</span><span class="sxs-lookup"><span data-stu-id="b64c1-107">In hello [Azure Portal](https://portal.azure.com), you can designate a folder in your cloud storage, work with your app code and content in that folder, and sync tooApp Service with hello click of a button.</span></span> <span data-ttu-id="b64c1-108">Tartalom szinkronizálási hello Kudu folyamata buildelés és üzembe helyezés használja.</span><span class="sxs-lookup"><span data-stu-id="b64c1-108">Content sync utilizes hello Kudu process for build and deployment.</span></span> 

## <span data-ttu-id="b64c1-109"><a name="contentsync"></a>Hogyan tooenable tartalom szinkronizálása a központi telepítés</span><span class="sxs-lookup"><span data-stu-id="b64c1-109"><a name="contentsync"></a>How tooenable content sync deployment</span></span>
<span data-ttu-id="b64c1-110">tartalom szinkronizálás tooenable hello [Azure Portal](https://portal.azure.com), kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b64c1-110">tooenable content sync from hello [Azure Portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="b64c1-111">A hello Azure portálra az alkalmazás paneljén kattintson **beállítások** > **központi telepítés forrásának**.</span><span class="sxs-lookup"><span data-stu-id="b64c1-111">In your app's blade in hello Azure Portal, click **Settings** > **Deployment Source**.</span></span> <span data-ttu-id="b64c1-112">Kattintson a **forrás választása**, majd jelölje be **OneDrive** vagy **Dropbox** telepítési hello forrásaként.</span><span class="sxs-lookup"><span data-stu-id="b64c1-112">Click **Choose Source**, then select **OneDrive** or **Dropbox** as hello source for deployment.</span></span> 
   
    ![Tartalom szinkronizálása](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > <span data-ttu-id="b64c1-114">Hello API-k, a mögöttes különbségek miatt **onedrive vállalati verzió** jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="b64c1-114">Because of underlying differences in hello APIs, **OneDrive for Business** is not supported at this time.</span></span> 
   > 
   > 
2. <span data-ttu-id="b64c1-115">Teljes hello engedélyezési munkafolyamat tooenable App Service tooaccess egy adott előre meghatározott kijelölt elérési út a onedrive-on vagy a Dropbox összes, az App Service tartalmának tárolására.</span><span class="sxs-lookup"><span data-stu-id="b64c1-115">Complete hello authorization workflow tooenable App Service tooaccess a specific pre-defined designated path for OneDrive or Dropbox where all of your App Service content will be stored.</span></span>  
    <span data-ttu-id="b64c1-116">Után engedélyezési hello App Service platform használatával biztosítja a hello beállítás toocreate hello tartalom mappája kijelölt tartalom elérési útját, vagy toochoose a kijelölt tartalom elérési út egy meglévő tartalom mappát.</span><span class="sxs-lookup"><span data-stu-id="b64c1-116">After authorization hello App Service platform will give you hello option toocreate a content folder under hello designated content path, or toochoose an existing content folder under this designated content path.</span></span> <span data-ttu-id="b64c1-117">kijelölt hello tartalom útvonalakat az App Service-szinkronizáláshoz használt cloud storage-fiókok hello következők tartoznak:</span><span class="sxs-lookup"><span data-stu-id="b64c1-117">hello designated content paths under your cloud storage accounts used for App Service sync are hello following:</span></span>  
   
   * <span data-ttu-id="b64c1-118">**Onedrive vállalati verzió**:`Apps\Azure Web Apps`</span><span class="sxs-lookup"><span data-stu-id="b64c1-118">**OneDrive**: `Apps\Azure Web Apps`</span></span> 
   * <span data-ttu-id="b64c1-119">**Dropbox**:`Dropbox\Apps\Azure`</span><span class="sxs-lookup"><span data-stu-id="b64c1-119">**Dropbox**: `Dropbox\Apps\Azure`</span></span>
3. <span data-ttu-id="b64c1-120">Hello után tartalom kezdeti szinkronizálás hello tartalom szinkronizálási igény szerint az Azure-portálon hello kezdeményezhető.</span><span class="sxs-lookup"><span data-stu-id="b64c1-120">After hello initial content sync hello content sync can be initiated on demand from hello Azure portal.</span></span> <span data-ttu-id="b64c1-121">Telepítési előzmények érhető el hello **központi telepítések** panelen.</span><span class="sxs-lookup"><span data-stu-id="b64c1-121">Deployment history is available with hello **Deployments** blade.</span></span>
   
    ![Telepítési előzmények](./media/app-service-deploy-content-sync/onedrive_sync.png)

<span data-ttu-id="b64c1-123">További információ a Dropbox telepítési érhető el a [központi telepítése a Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span><span class="sxs-lookup"><span data-stu-id="b64c1-123">More information for Dropbox deployment is available under [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span></span> 

