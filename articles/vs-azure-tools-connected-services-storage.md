---
title: "Kapcsolódó szolgáltatások a Visual Studio használatával Azure Storage aaaAdd |} Microsoft Docs"
description: "A Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen hello Azure Storage tooyour alkalmazás hozzáadása"
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
ms.openlocfilehash: 56b42063d86510b330e405108e28d50e6ba4da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a><span data-ttu-id="c4f45-103">Az Azure storage hozzáadása a Visual Studio kapcsolódó szolgáltatások használatával</span><span class="sxs-lookup"><span data-stu-id="c4f45-103">Adding Azure storage by using Visual Studio Connected Services</span></span>
<span data-ttu-id="c4f45-104">A Visual Studio kapcsolódhatnak tooAzure tárolási hello segítségével a következő hello bármelyikét **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="c4f45-104">With Visual Studio, you can connect any of hello following tooAzure Storage by using hello **Add Connected Services** dialog:</span></span>

- <span data-ttu-id="c4f45-105">C#-felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c4f45-105">C# cloud service</span></span>
- <span data-ttu-id="c4f45-106">.NET-háttérrendszer mobilszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c4f45-106">.NET backend mobile service</span></span>
- <span data-ttu-id="c4f45-107">ASP.NET-webhely vagy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c4f45-107">ASP.NET website or service</span></span>
- <span data-ttu-id="c4f45-108">Az ASP.NET Core szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c4f45-108">ASP.NET Core service</span></span>
- <span data-ttu-id="c4f45-109">Azure webjobs-feladat szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c4f45-109">Azure WebJob service</span></span> 

<span data-ttu-id="c4f45-110">hello csatlakoztatott működtetéséhez szükséges hello hivatkozások és a kapcsolat kód tooyour projekt hozzáadása, és megfelelően módosítja a konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="c4f45-110">hello connected service functionality adds all hello needed references and connection code tooyour project, and modifies your configuration files appropriately.</span></span> 

<span data-ttu-id="c4f45-111">A művelet befejezését követően hello **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel automatikusan dokumentációját, és részletesen leírja a hello lépéseket szükséges toostart blob-tároló, üzenetsorok és táblák használata jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c4f45-111">After completion, hello **Add Connected Services** dialog automatically displays documentation detailing hello steps required toostart working with blob storage, queues, and tables.</span></span>

## <a name="connect-tooazure-storage-using-hello-connected-services-dialog"></a><span data-ttu-id="c4f45-112">Csatlakozás tooAzure tárolóhely-hello kapcsolódó szolgáltatások párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="c4f45-112">Connect tooAzure Storage using hello Connected Services dialog</span></span>
1. <span data-ttu-id="c4f45-113">Nyissa meg a projektet a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4f45-113">Open your project in Visual Studio</span></span>

1. <span data-ttu-id="c4f45-114">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **kapcsolódó szolgáltatások** csomópont, és hello helyi menüt, majd válassza a **kapcsolódó szolgáltatás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c4f45-114">In **Solution Explorer**, right-click hello **Connected Services** node, and, from hello context menu, and select **Add Connected Service**.</span></span>
   
    ![Adja hozzá az Azure szolgáltatás csatlakoztatva](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. <span data-ttu-id="c4f45-116">A hello **kapcsolódó szolgáltatások** lapon jelölje be **felhőalapú tárolás az Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="c4f45-116">In hello **Connected Services** page, select **Cloud Storage with Azure Storage**.</span></span>
   
    ![Az Azure Storage hozzáadása](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. <span data-ttu-id="c4f45-118">A hello **Azure Storage** párbeszédpanel, válasszon egy meglévő tárfiókot használ, válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c4f45-118">In hello **Azure Storage** dialog, select an existing storage account, and select **Add**.</span></span>
   
    <span data-ttu-id="c4f45-119">Ha toocreate a storage-fiókra van szüksége, lépjen a toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="c4f45-119">If you need toocreate a storage account, go toohello next step.</span></span> <span data-ttu-id="c4f45-120">Egyéb esetben folytassa a toostep 6.</span><span class="sxs-lookup"><span data-stu-id="c4f45-120">Otherwise, skip toostep 6.</span></span>
    
    ![Adja hozzá a meglévő tárolási fiók tooproject](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. <span data-ttu-id="c4f45-122">a tárfiók toocreate:</span><span class="sxs-lookup"><span data-stu-id="c4f45-122">toocreate a storage account:</span></span> 
   
   1. <span data-ttu-id="c4f45-123">Válassza ki **hozzon létre egy új Tárfiókot** hello hello párbeszédpanel alsó részén.</span><span class="sxs-lookup"><span data-stu-id="c4f45-123">Select **Create a New Storage Account** at hello bottom of hello dialog.</span></span>

   1. <span data-ttu-id="c4f45-124">Töltse ki a hello **Storage-fiók létrehozása** párbeszédpanel, válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c4f45-124">Fill out hello **Create Storage Account** dialog, and select **Create**.</span></span>
      
       ![Új Azure storage-fiók](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. <span data-ttu-id="c4f45-126">Ha hello **Azure Storage** párbeszédpanel jelenik meg, az új tárfiók hello hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c4f45-126">When hello **Azure Storage** dialog is displayed, hello new storage account appears in hello list.</span></span> <span data-ttu-id="c4f45-127">Új tárfiók hello hello listában válassza ki és **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c4f45-127">Select hello new storage account in hello list, and select **Add**.</span></span>

1. <span data-ttu-id="c4f45-128">csatlakoztatott szolgáltatás hello alatt jelenik meg tárolási hello **Szolgáltatáshivatkozások** a projekt csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="c4f45-128">hello storage connected service appears under hello **Service References** node of your project.</span></span>
   
## <a name="how-your-project-is-modified"></a><span data-ttu-id="c4f45-129">A projekt módosítása hogyan</span><span class="sxs-lookup"><span data-stu-id="c4f45-129">How your project is modified</span></span>
<span data-ttu-id="c4f45-130">Hello párbeszédpanel befejezése után a Visual Studio hivatkozásokat ad, és módosítja az egyes konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="c4f45-130">When you finish hello dialog, Visual Studio adds references and modifies certain configuration files.</span></span> <span data-ttu-id="c4f45-131">hello adott módosítások hello projekt típusától függ:</span><span class="sxs-lookup"><span data-stu-id="c4f45-131">hello specific changes depend on hello project type:</span></span> 

- <span data-ttu-id="c4f45-132">ASP.NET-projekt - [Mi történt – ASP.NET-projektek](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span><span class="sxs-lookup"><span data-stu-id="c4f45-132">ASP.NET project - [What happened – ASP.NET Projects](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span></span>
- <span data-ttu-id="c4f45-133">Az ASP.NET Core projekt - [Mi történt – ASP.NET 5 projektek](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span><span class="sxs-lookup"><span data-stu-id="c4f45-133">ASP.NET Core project - [What happened – ASP.NET 5 Projects](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span></span> 
- <span data-ttu-id="c4f45-134">Felhőszolgáltatás-projekt (webes és feldolgozói szerepkörök) - [Mi történt – Cloud Service projektek](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span><span class="sxs-lookup"><span data-stu-id="c4f45-134">Cloud service project (web roles and worker roles) - [What happened – Cloud Service projects](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span></span>
- <span data-ttu-id="c4f45-135">Webjobs-feladat projekt - [Mi történt – a webjobs-feladat projektek](visual-studio/vs-storage-webjobs-what-happened.md)</span><span class="sxs-lookup"><span data-stu-id="c4f45-135">WebJob project - [What happened - WebJob projects](visual-studio/vs-storage-webjobs-what-happened.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4f45-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4f45-136">Next steps</span></span>
- [<span data-ttu-id="c4f45-137">MSDN fórum: Az Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c4f45-137">MSDN Forum: Azure Storage</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [<span data-ttu-id="c4f45-138">A Microsoft Azure tárolás fejlesztői Blog</span><span class="sxs-lookup"><span data-stu-id="c4f45-138">Microsoft Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
- [<span data-ttu-id="c4f45-139">Az Azure Storage-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="c4f45-139">Azure Storage documentation</span></span>](https://docs.microsoft.com/azure/storage/)
