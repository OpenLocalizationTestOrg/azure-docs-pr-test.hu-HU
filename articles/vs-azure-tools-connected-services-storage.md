---
title: "Adja hozzá az Azure Storage szolgáltatások csatlakoztatva a Visual Studio használatával |} Microsoft Docs"
description: "Azure Storage hozzáadása az alkalmazáshoz a Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 35638083cd75e1b751d00a9c8163a3bc7480f0cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a><span data-ttu-id="5cf13-103">Az Azure storage hozzáadása a Visual Studio kapcsolódó szolgáltatások használatával</span><span class="sxs-lookup"><span data-stu-id="5cf13-103">Adding Azure storage by using Visual Studio Connected Services</span></span>
<span data-ttu-id="5cf13-104">A Visual Studio kapcsolódás a következő Azure Storage használatával a **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="5cf13-104">With Visual Studio, you can connect any of the following to Azure Storage by using the **Add Connected Services** dialog:</span></span>

- <span data-ttu-id="5cf13-105">C#-felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5cf13-105">C# cloud service</span></span>
- <span data-ttu-id="5cf13-106">.NET-háttérrendszer mobilszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5cf13-106">.NET backend mobile service</span></span>
- <span data-ttu-id="5cf13-107">ASP.NET-webhely vagy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5cf13-107">ASP.NET website or service</span></span>
- <span data-ttu-id="5cf13-108">Az ASP.NET Core szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5cf13-108">ASP.NET Core service</span></span>
- <span data-ttu-id="5cf13-109">Azure webjobs-feladat szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5cf13-109">Azure WebJob service</span></span> 

<span data-ttu-id="5cf13-110">A csatlakoztatott szolgáltatás funkcióit a szükséges hivatkozásokat és a kapcsolat kód hozzáadása a projekthez, és megfelelően módosítja a konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="5cf13-110">The connected service functionality adds all the needed references and connection code to your project, and modifies your configuration files appropriately.</span></span> 

<span data-ttu-id="5cf13-111">A művelet befejezését követően a **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel automatikusan megjeleníti a dokumentációját, és részletesen leírja a dolgozni a blob-tároló, várólisták, szükséges lépéseket és táblázatot.</span><span class="sxs-lookup"><span data-stu-id="5cf13-111">After completion, the **Add Connected Services** dialog automatically displays documentation detailing the steps required to start working with blob storage, queues, and tables.</span></span>

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a><span data-ttu-id="5cf13-112">Csatlakozás Azure Storage a kapcsolódó szolgáltatások párbeszédpanelen</span><span class="sxs-lookup"><span data-stu-id="5cf13-112">Connect to Azure Storage using the Connected Services dialog</span></span>
1. <span data-ttu-id="5cf13-113">Nyissa meg a projektet a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5cf13-113">Open your project in Visual Studio</span></span>

1. <span data-ttu-id="5cf13-114">A **Megoldáskezelőben**, kattintson a jobb gombbal a **kapcsolódó szolgáltatások** csomópont, és a helyi menüt, majd válassza a **kapcsolódó szolgáltatás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5cf13-114">In **Solution Explorer**, right-click the **Connected Services** node, and, from the context menu, and select **Add Connected Service**.</span></span>
   
    ![Adja hozzá az Azure szolgáltatás csatlakoztatva](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. <span data-ttu-id="5cf13-116">Az a **kapcsolódó szolgáltatások** lapon jelölje be **felhőalapú tárolás az Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="5cf13-116">In the **Connected Services** page, select **Cloud Storage with Azure Storage**.</span></span>
   
    ![Az Azure Storage hozzáadása](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. <span data-ttu-id="5cf13-118">Az a **Azure Storage** párbeszédpanel, válasszon egy meglévő tárfiókot használ, válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5cf13-118">In the **Azure Storage** dialog, select an existing storage account, and select **Add**.</span></span>
   
    <span data-ttu-id="5cf13-119">Ha létrehoz egy tárfiókot van szüksége, nyissa meg a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="5cf13-119">If you need to create a storage account, go to the next step.</span></span> <span data-ttu-id="5cf13-120">Egyéb esetben folytassa a 6. lépés.</span><span class="sxs-lookup"><span data-stu-id="5cf13-120">Otherwise, skip to step 6.</span></span>
    
    ![Meglévő tárfiók hozzáadása a projekthez](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. <span data-ttu-id="5cf13-122">A storage-fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="5cf13-122">To create a storage account:</span></span> 
   
   1. <span data-ttu-id="5cf13-123">Válassza ki **hozzon létre egy új Tárfiókot** a párbeszédpanel alján.</span><span class="sxs-lookup"><span data-stu-id="5cf13-123">Select **Create a New Storage Account** at the bottom of the dialog.</span></span>

   1. <span data-ttu-id="5cf13-124">Töltse ki a **Storage-fiók létrehozása** párbeszédpanel, válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5cf13-124">Fill out the **Create Storage Account** dialog, and select **Create**.</span></span>
      
       ![Új Azure storage-fiók](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. <span data-ttu-id="5cf13-126">Ha a **Azure Storage** párbeszédpanel jelenik meg, az új tárfiók listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5cf13-126">When the **Azure Storage** dialog is displayed, the new storage account appears in the list.</span></span> <span data-ttu-id="5cf13-127">A listában jelölje ki az új tárfiókot, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5cf13-127">Select the new storage account in the list, and select **Add**.</span></span>

1. <span data-ttu-id="5cf13-128">A tárolási csatlakoztatott szolgáltatás alatt jelenik meg a **szolgáltatási hivatkozást lekérni** a projekt csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="5cf13-128">The storage connected service appears under the **Service References** node of your project.</span></span>
   
## <a name="how-your-project-is-modified"></a><span data-ttu-id="5cf13-129">A projekt módosítása hogyan</span><span class="sxs-lookup"><span data-stu-id="5cf13-129">How your project is modified</span></span>
<span data-ttu-id="5cf13-130">Ha befejezte a párbeszédpanelen, a Visual Studio hivatkozásokat ad, és módosítja a bizonyos konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="5cf13-130">When you finish the dialog, Visual Studio adds references and modifies certain configuration files.</span></span> <span data-ttu-id="5cf13-131">Az adott módosítások a projekt típusától függ:</span><span class="sxs-lookup"><span data-stu-id="5cf13-131">The specific changes depend on the project type:</span></span> 

- <span data-ttu-id="5cf13-132">ASP.NET-projekt - [Mi történt – ASP.NET-projektek](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span><span class="sxs-lookup"><span data-stu-id="5cf13-132">ASP.NET project - [What happened – ASP.NET Projects](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span></span>
- <span data-ttu-id="5cf13-133">Az ASP.NET Core projekt - [Mi történt – ASP.NET 5 projektek](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span><span class="sxs-lookup"><span data-stu-id="5cf13-133">ASP.NET Core project - [What happened – ASP.NET 5 Projects](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span></span> 
- <span data-ttu-id="5cf13-134">Felhőszolgáltatás-projekt (webes és feldolgozói szerepkörök) - [Mi történt – Cloud Service projektek](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span><span class="sxs-lookup"><span data-stu-id="5cf13-134">Cloud service project (web roles and worker roles) - [What happened – Cloud Service projects](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span></span>
- <span data-ttu-id="5cf13-135">Webjobs-feladat projekt - [Mi történt – a webjobs-feladat projektek](visual-studio/vs-storage-webjobs-what-happened.md)</span><span class="sxs-lookup"><span data-stu-id="5cf13-135">WebJob project - [What happened - WebJob projects](visual-studio/vs-storage-webjobs-what-happened.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5cf13-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5cf13-136">Next steps</span></span>
- [<span data-ttu-id="5cf13-137">MSDN fórum: Az Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5cf13-137">MSDN Forum: Azure Storage</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [<span data-ttu-id="5cf13-138">A Microsoft Azure tárolás fejlesztői Blog</span><span class="sxs-lookup"><span data-stu-id="5cf13-138">Microsoft Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
- [<span data-ttu-id="5cf13-139">Az Azure Storage-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="5cf13-139">Azure Storage documentation</span></span>](https://docs.microsoft.com/azure/storage/)
