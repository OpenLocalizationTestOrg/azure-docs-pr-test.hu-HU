---
title: "A Visual Studio Azure magánfelhőkben elérése |} Microsoft Docs"
description: "Útmutató a magánfelhő-alapú erőforrások eléréséhez a Visual Studio használatával."
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
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a><span data-ttu-id="5cfce-103">A Visual Studio Azure magánfelhőkben elérése</span><span class="sxs-lookup"><span data-stu-id="5cfce-103">Accessing private Azure clouds with Visual Studio</span></span>
<span data-ttu-id="5cfce-104">Alapértelmezés szerint a Visual Studio támogatja a nyilvános Azure-felhőbe REST-végpontok.</span><span class="sxs-lookup"><span data-stu-id="5cfce-104">By default, Visual Studio supports public Azure cloud REST endpoints.</span></span> <span data-ttu-id="5cfce-105">Ebben a témakörben elsajátíthatja, hogyan saját magánfelhő-alapú tanúsítvány használatára eléri - és a - használhatja a Visual Studio eszközből a magánfelhő.</span><span class="sxs-lookup"><span data-stu-id="5cfce-105">In this topic, you learn how to use your private cloud's certificate to access - and interact with - the private cloud from Visual Studio.</span></span>

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a><span data-ttu-id="5cfce-106">A saját Azure eléréséhez a Visual Studio felhő</span><span class="sxs-lookup"><span data-stu-id="5cfce-106">To access a private Azure cloud in Visual Studio</span></span>
1. <span data-ttu-id="5cfce-107">Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885) megadása a magánfelhőhöz, töltse le a közzétételi-beállítások fájlt, vagy forduljon a rendszergazdához közzététele beállításfájl.</span><span class="sxs-lookup"><span data-stu-id="5cfce-107">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) for the private cloud, download your publish-settings file, or contact your administrator for a publish-settings file.</span></span> <span data-ttu-id="5cfce-108">A nyilvános Azure verzión a hivatkozásra kattintva töltse le a rendszer [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span><span class="sxs-lookup"><span data-stu-id="5cfce-108">On the public version of Azure, the link to download this is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span></span> <span data-ttu-id="5cfce-109">(A letöltött fájl kiterjesztése kell `.publishsettings`)</span><span class="sxs-lookup"><span data-stu-id="5cfce-109">(The downloaded file should have an extension of `.publishsettings`)</span></span>

1. <span data-ttu-id="5cfce-110">Nyissa meg a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5cfce-110">Open Visual Studio</span></span>

1. <span data-ttu-id="5cfce-111">A **Server Explorer**, kattintson a jobb gombbal a **Azure** csomópont, és válassza a helyi menüből **kezelése és a szűrő előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="5cfce-111">In **Server Explorer**, right-click the **Azure** node and, from the context menu, select **Manage and Filter Subscriptions**.</span></span>
   
    ![Előfizetések parancs kezelése](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. <span data-ttu-id="5cfce-113">Az a **kezelése a Microsoft Azure-előfizetések** párbeszédpanelen válassza a **tanúsítványok** lapra, majd válassza ki **importálási**.</span><span class="sxs-lookup"><span data-stu-id="5cfce-113">In the **Manage Microsoft Azure Subscriptions** dialog, select the **Certificates** tab, and then select **Import**.</span></span>
   
    ![Az Azure tanúsítványok importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. <span data-ttu-id="5cfce-115">Az a **importálása a Microsoft Azure-előfizetések** párbeszédablakban válassza **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="5cfce-115">In the **Import Microsoft Azure Subscriptions** dialog, select **Browse**.</span></span>

    ![Tallózás gombra a Microsoft Azure-előfizetések importálása párbeszédpanel](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. <span data-ttu-id="5cfce-117">Az a **nyitott** párbeszédpanelen keresse meg a könyvtárat, ahová mentette az közzététele beállításfájl válassza ki a fájlt, és válassza **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="5cfce-117">In the **Open** dialog, browse to the directory where you saved the publish-settings file, select the file, and then select **Open**.</span></span>

    ![A publish-fájl kiválasztása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. <span data-ttu-id="5cfce-119">Amikor visszatér a **importálása a Microsoft Azure-előfizetések** párbeszédablakban válassza **importálási**.</span><span class="sxs-lookup"><span data-stu-id="5cfce-119">When returned to the **Import Microsoft Azure Subscriptions** dialog, select **Import**.</span></span>

    ![Közzététele beállításfájl importálása](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    <span data-ttu-id="5cfce-121">A tanúsítványok importálására a publish-fájl a Visual Studióhoz, és most kezelheti a magánfelhő-alapú erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5cfce-121">The certificates are imported from the publish-settings file into Visual Studio, and you can now interact with your private cloud resources.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="5cfce-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5cfce-122">Next steps</span></span>
- [<span data-ttu-id="5cfce-123">A Visual Studio eszközből közzétételéhez Azure-Felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5cfce-123">Publishing to an Azure Cloud Service from Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- <span data-ttu-id="5cfce-124">[Hogyan: letöltése és importálása beállítások és az előfizetési adatok közzététele](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span><span class="sxs-lookup"><span data-stu-id="5cfce-124">[How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span></span>
