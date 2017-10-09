---
title: "a Visual Studio Azure felhők titkos aaaAccessing |} Microsoft Docs"
description: "Ismerje meg, hogyan tooaccess magánfelhő erőforrások Visual Studio használatával."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: 5cfd6439afdcf98c6f7d7f29ab6c4256ed02533a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a><span data-ttu-id="922d9-103">A Visual Studio Azure magánfelhőkben elérése</span><span class="sxs-lookup"><span data-stu-id="922d9-103">Accessing private Azure clouds with Visual Studio</span></span>
<span data-ttu-id="922d9-104">Alapértelmezés szerint a Visual Studio támogatja a nyilvános Azure-felhőbe REST-végpontok.</span><span class="sxs-lookup"><span data-stu-id="922d9-104">By default, Visual Studio supports public Azure cloud REST endpoints.</span></span> <span data-ttu-id="922d9-105">Ebben a témakörben elsajátíthatja, hogyan toouse a magánfelhő tartozó tanúsítvány tooaccess - és kommunikálhat - hello magánfelhő a Visual Studio eszközből.</span><span class="sxs-lookup"><span data-stu-id="922d9-105">In this topic, you learn how toouse your private cloud's certificate tooaccess - and interact with - hello private cloud from Visual Studio.</span></span>

## <a name="tooaccess-a-private-azure-cloud-in-visual-studio"></a><span data-ttu-id="922d9-106">a Visual Studio cloud tooaccess titkos Azure</span><span class="sxs-lookup"><span data-stu-id="922d9-106">tooaccess a private Azure cloud in Visual Studio</span></span>
1. <span data-ttu-id="922d9-107">A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885) hello magánfelhő, töltse le a közzétételi-beállítások fájlt, vagy forduljon a rendszergazdához közzététele beállításfájl.</span><span class="sxs-lookup"><span data-stu-id="922d9-107">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) for hello private cloud, download your publish-settings file, or contact your administrator for a publish-settings file.</span></span> <span data-ttu-id="922d9-108">Azure verzióján nyilvános hello, ez a hivatkozás toodownload hello [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span><span class="sxs-lookup"><span data-stu-id="922d9-108">On hello public version of Azure, hello link toodownload this is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span></span> <span data-ttu-id="922d9-109">(hello letöltött fájl kiterjesztése kell `.publishsettings`)</span><span class="sxs-lookup"><span data-stu-id="922d9-109">(hello downloaded file should have an extension of `.publishsettings`)</span></span>

1. <span data-ttu-id="922d9-110">Nyissa meg a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="922d9-110">Open Visual Studio</span></span>

1. <span data-ttu-id="922d9-111">A **Server Explorer**, kattintson a jobb gombbal hello **Azure** csomópont és hello helyi menüből válassza ki a **kezelése és a szűrő előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="922d9-111">In **Server Explorer**, right-click hello **Azure** node and, from hello context menu, select **Manage and Filter Subscriptions**.</span></span>
   
    ![Előfizetések parancs kezelése](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. <span data-ttu-id="922d9-113">A hello **kezelése a Microsoft Azure-előfizetések** párbeszédpanelen jelölje be hello **tanúsítványok** lapra, majd válassza ki **importálási**.</span><span class="sxs-lookup"><span data-stu-id="922d9-113">In hello **Manage Microsoft Azure Subscriptions** dialog, select hello **Certificates** tab, and then select **Import**.</span></span>
   
    ![Az Azure tanúsítványok importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. <span data-ttu-id="922d9-115">A hello **importálása a Microsoft Azure-előfizetések** párbeszédablakban válassza **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="922d9-115">In hello **Import Microsoft Azure Subscriptions** dialog, select **Browse**.</span></span>

    ![Tallózás gomb hello importálása a Microsoft Azure-előfizetések párbeszédpanelen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. <span data-ttu-id="922d9-117">A hello **nyitott** párbeszédpanelen Tallózás toohello könyvtárába, ahol Ön mentett hello közzétételi beállítási fájl, jelölje be hello fájl, és adja **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="922d9-117">In hello **Open** dialog, browse toohello directory where you saved hello publish-settings file, select hello file, and then select **Open**.</span></span>

    ![Hello közzétételi beállítási fájl kiválasztása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. <span data-ttu-id="922d9-119">Amikor toohello visszaadott **importálása a Microsoft Azure-előfizetések** párbeszédablakban válassza **importálási**.</span><span class="sxs-lookup"><span data-stu-id="922d9-119">When returned toohello **Import Microsoft Azure Subscriptions** dialog, select **Import**.</span></span>

    ![Hello közzétételi beállítási fájl importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    <span data-ttu-id="922d9-121">hello tanúsítványok hello közzétételi beállítások fájlját a rendszer importálja a parancsmagmodulokat a Visual Studio, és most kezelheti a magánfelhő-alapú erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="922d9-121">hello certificates are imported from hello publish-settings file into Visual Studio, and you can now interact with your private cloud resources.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="922d9-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="922d9-122">Next steps</span></span>
- [<span data-ttu-id="922d9-123">Közzétételi tooan Azure Cloud Service, a Visual Studio eszközből</span><span class="sxs-lookup"><span data-stu-id="922d9-123">Publishing tooan Azure Cloud Service from Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- <span data-ttu-id="922d9-124">[Hogyan: letöltése és importálása beállítások és az előfizetési adatok közzététele](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span><span class="sxs-lookup"><span data-stu-id="922d9-124">[How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span></span>
