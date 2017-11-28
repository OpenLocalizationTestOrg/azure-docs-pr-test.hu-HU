---
title: "egy Azure közzétett aaaDebugging felhőalapú szolgáltatás, a Visual Studio és az IntelliTrace |} Microsoft Docs"
description: "Ismerje meg, hogyan toodebug egy felhőalapú szolgáltatás, a Visual Studio és az IntelliTrace"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 60734a71d5499de72e1ca81a3ab0ab9f34bc303a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a><span data-ttu-id="a6859-103">A közzétett Azure-felhőszolgáltatásban a Visual Studio és az IntelliTrace-hibakeresés</span><span class="sxs-lookup"><span data-stu-id="a6859-103">Debugging a published Azure cloud service with Visual Studio and IntelliTrace</span></span>
<span data-ttu-id="a6859-104">Intellitrace bejelentkezhet a szerepkör példánya nagy mennyiségű hibakeresési adatok az Azure-ban futtatott.</span><span class="sxs-lookup"><span data-stu-id="a6859-104">With IntelliTrace, you can log extensive debugging information for a role instance when it runs in Azure.</span></span> <span data-ttu-id="a6859-105">Ha a probléma okát toofind hello van szüksége, használhatja hello IntelliTrace-naplók toostep a kód a Visual Studio eszközből, mintha csak az Azure-ban futó.</span><span class="sxs-lookup"><span data-stu-id="a6859-105">If you need toofind hello cause of a problem, you can use hello IntelliTrace logs toostep through your code from Visual Studio as if it were running in Azure.</span></span> <span data-ttu-id="a6859-106">IntelliTrace kulcs kód végrehajtása és a környezet adatait rögzíti, amikor az Azure-alkalmazásokban az Azure-ban felhőszolgáltatásként fut, és lehetővé teszi, hogy érvényben, a Visual Studio eszközből hello rögzített adatok visszajátszásos.</span><span class="sxs-lookup"><span data-stu-id="a6859-106">In effect, IntelliTrace records key code execution and environment data when your Azure application is running as a cloud service in Azure, and lets you replay hello recorded data from Visual Studio.</span></span> 

<span data-ttu-id="a6859-107">Ha rendelkezik telepített Visual Studio Enterprise IntelliTrace és az Azure-alkalmazásokban célok .NET-keretrendszer 4 vagy újabb verzió használható.</span><span class="sxs-lookup"><span data-stu-id="a6859-107">You can use IntelliTrace if you have Visual Studio Enterprise installed and your Azure application targets .NET Framework 4 or a later version.</span></span> <span data-ttu-id="a6859-108">IntelliTrace az Azure-szerepkörök adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="a6859-108">IntelliTrace collects information for your Azure roles.</span></span> <span data-ttu-id="a6859-109">Ezeket a szerepköröket mindig a 64 bites operációs rendszereket futtató virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="a6859-109">hello virtual machines for these roles always run 64-bit operating systems.</span></span>

<span data-ttu-id="a6859-110">Alternatív megoldásként használható [távoli hibakeresés](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach közvetlenül tooa felhőalapú szolgáltatás, hogy fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a6859-110">As an alternative, you can use [remote debugging](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach directly tooa cloud service that's running in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6859-111">IntelliTrace csak hibakeresési forgatókönyvekhez készült, és nem használható éles környezet.</span><span class="sxs-lookup"><span data-stu-id="a6859-111">IntelliTrace is intended for debug scenarios only, and should not be used for a production deployment.</span></span>
> 

## <a name="configure-an-azure-application-for-intellitrace"></a><span data-ttu-id="a6859-112">Az Azure-alkalmazások konfigurálása az IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="a6859-112">Configure an Azure application for IntelliTrace</span></span>
<span data-ttu-id="a6859-113">tooenable IntelliTrace Azure alkalmazás esetén létre kell hoznia és tegye közzé a Visual Studio Azure projektből hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a6859-113">tooenable IntelliTrace for an Azure application, you must create and publish hello application from a Visual Studio Azure project.</span></span> <span data-ttu-id="a6859-114">Közzététel előtt tooAzure IntelliTrace kell konfigurálása az Azure-alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="a6859-114">You must configure IntelliTrace for your Azure application before you publish it tooAzure.</span></span> <span data-ttu-id="a6859-115">Ha az alkalmazás közzétételére IntelliTrace beállítása nélkül, toorepublish hello projekt kell.</span><span class="sxs-lookup"><span data-stu-id="a6859-115">If you publish your application without configuring IntelliTrace, you need toorepublish hello project.</span></span> <span data-ttu-id="a6859-116">További információkért lásd: [közzététele egy Azure cloud services projekteket a Visual Studio használatával](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span><span class="sxs-lookup"><span data-stu-id="a6859-116">For more information, see [Publishing an Azure cloud services projects using Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span></span>

1. <span data-ttu-id="a6859-117">Amikor készen áll a toodeploy az Azure-alkalmazásában, győződjön meg arról, hogy túl van-e beállítva a project build célkitűzések**Debug**.</span><span class="sxs-lookup"><span data-stu-id="a6859-117">When you are ready toodeploy your Azure application, verify that your project build targets are set too**Debug**.</span></span>

1. <span data-ttu-id="a6859-118">A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt, és hello helyi menüből válassza ki a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="a6859-118">In **Solution Explorer**, right-click project, and, from hello context menu, select **Publish**.</span></span>
   
1. <span data-ttu-id="a6859-119">A hello **Azure-alkalmazás közzététele** párbeszédpanel, jelölje be hello Azure-előfizetéssel, válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="a6859-119">In hello **Publish Azure Application** dialog, select hello Azure subscription, and select **Next**.</span></span>

1. <span data-ttu-id="a6859-120">A hello **beállítások** lapra, jelölje be hello **speciális beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="a6859-120">In hello **Settings** page, select hello **Advanced Settings** tab.</span></span>

1. <span data-ttu-id="a6859-121">Kapcsolja be a hello **engedélyezése IntelliTrace** toocollect IntelliTrace-naplók az alkalmazás lehetőséget hello felhőben van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="a6859-121">Turn on hello **Enable IntelliTrace** option toocollect IntelliTrace logs for your application when it is published in hello cloud.</span></span>
   
1. <span data-ttu-id="a6859-122">toocustomize hello IntelliTrace alapkonfiguráció, jelölje be **beállítások** következő túl**engedélyezése IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="a6859-122">toocustomize hello basic IntelliTrace configuration, select **Settings** next too**Enable IntelliTrace**.</span></span>

    ![IntelliTrace beállítások hivatkozására](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. <span data-ttu-id="a6859-124">A hello **IntelliTrace beállítások** párbeszédpanelen adhat meg, milyen események toolog, e toocollect hívja fel információt, mely modulok és a folyamatok toocollect naplóit, és mekkora terület tooallocate toohello rögzítése.</span><span class="sxs-lookup"><span data-stu-id="a6859-124">In hello **IntelliTrace Settings** dialog, you can specify which events toolog, whether toocollect call information, which modules and processes toocollect logs for, and how much space tooallocate toohello recording.</span></span> <span data-ttu-id="a6859-125">Intellitrace alkalmazással kapcsolatban további információkért lásd: [intellitrace-hibakeresés](http://go.microsoft.com/fwlink/?LinkId=214468).</span><span class="sxs-lookup"><span data-stu-id="a6859-125">For more information about IntelliTrace, see [Debugging with IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span></span>
   
    ![IntelliTrace-beállítások](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

<span data-ttu-id="a6859-127">hello IntelliTrace-naplót hello megadott hello IntelliTrace beállítások (hello alapértelmezett mérete 250 MB) maximális méretének körkörös naplófájl.</span><span class="sxs-lookup"><span data-stu-id="a6859-127">hello IntelliTrace log is a circular log file of hello maximum size specified in hello IntelliTrace settings (hello default size is 250 MB).</span></span> <span data-ttu-id="a6859-128">IntelliTrace-naplók összegyűjtött tooa fájl hello fájlrendszer hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a6859-128">IntelliTrace logs are collected tooa file in hello file system of hello virtual machine.</span></span> <span data-ttu-id="a6859-129">Hello naplók igénylésekor pillanatkép ezen a ponton az időben lesz végrehajtva, és letöltötte a helyi számítógép tooyour.</span><span class="sxs-lookup"><span data-stu-id="a6859-129">When you request hello logs, a snapshot is taken at that point in time and downloaded tooyour local computer.</span></span>

<span data-ttu-id="a6859-130">Azure-felhőszolgáltatásban hello közzétett tooAzure után azt is meghatározhatja, ha az IntelliTrace a hello Azure engedélyezett-e a csomópont **Server Explorer**, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="a6859-130">After hello Azure cloud service has been published tooAzure, you can determine if IntelliTrace has been enabled from hello Azure node in **Server Explorer**, as shown in hello following image:</span></span>

![Server Explorer - IntelliTrace engedélyezve](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a><span data-ttu-id="a6859-132">Töltse le az IntelliTrace-naplókat a szerepkörpéldányhoz</span><span class="sxs-lookup"><span data-stu-id="a6859-132">Download IntelliTrace logs for a role instance</span></span>
<span data-ttu-id="a6859-133">Visual Studio használatával, letöltheti a IntelliTrace-naplókat a szerepkör példánya a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="a6859-133">Using Visual Studio, you can download IntelliTrace logs for a role instance by following these steps:</span></span>

1. <span data-ttu-id="a6859-134">A **Server Explorer**, bontsa ki a hello **Felhőszolgáltatások** csomópont, és keresse meg a szerepkörpéldányt, amelynek a naplói toodownload kívánja.</span><span class="sxs-lookup"><span data-stu-id="a6859-134">In **Server Explorer**, expand hello **Cloud Services** node, and locate role instance whose logs you wish toodownload.</span></span> 

1. <span data-ttu-id="a6859-135">Kattintson a jobb gombbal a hello szerepkörpéldányt, és hello s helyi menüben válassza a **IntelliTrace-naplók megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="a6859-135">Right-click hello role instance, and from hello s context menu, select **View IntelliTrace Logs**.</span></span> 

    ![IntelliTrace-naplók menüpont megjelenítése](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. <span data-ttu-id="a6859-137">hello IntelliTrace-naplók letöltött tooa fájl a könyvtárban található a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a6859-137">hello IntelliTrace logs are downloaded tooa file in a directory on your local computer.</span></span> <span data-ttu-id="a6859-138">Minden alkalommal, amikor hello IntelliTrace-naplók kérnie egy új pillanatkép jön létre.</span><span class="sxs-lookup"><span data-stu-id="a6859-138">Each time that you request hello IntelliTrace logs, a new snapshot is created.</span></span> <span data-ttu-id="a6859-139">Hello naplók letöltése folyamatban van, amíg a Visual Studio megjeleníti hello művelet előrehaladását hello hello **Azure tevékenységnapló** ablak.</span><span class="sxs-lookup"><span data-stu-id="a6859-139">While hello logs are being downloaded, Visual Studio displays hello progress of hello operation in hello **Azure Activity Log** window.</span></span> <span data-ttu-id="a6859-140">Ahogy az ábra a következő hello, bővítheti hello sor eleme hello művelet toosee további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="a6859-140">As shown in hello following figure, you can expand hello line item for hello operation toosee more detail.</span></span>

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

<span data-ttu-id="a6859-142">A Visual Studio toowork folytatható, amíg hello IntelliTrace-naplók letöltése.</span><span class="sxs-lookup"><span data-stu-id="a6859-142">You can continue toowork in Visual Studio while hello IntelliTrace logs are downloading.</span></span> <span data-ttu-id="a6859-143">Amikor hello napló letöltése befejeződött, a Visual Studio nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a6859-143">When hello log has finished downloading, it opens in Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="a6859-144">hello IntelliTrace-naplók tartalmazhat kivételek adott hello keretrendszer hoz létre, és ezt követően kezeli.</span><span class="sxs-lookup"><span data-stu-id="a6859-144">hello IntelliTrace logs might contain exceptions that hello framework generates and handles afterwards.</span></span> <span data-ttu-id="a6859-145">Belső keretrendszer kódot állít elő, ezeket a kivételeket, így biztonságosan figyelmen kívül hagyhatja őket egy szerepkör indítása normál részeként.</span><span class="sxs-lookup"><span data-stu-id="a6859-145">Internal framework code generates these exceptions as a normal part of starting up a role, so you may safely ignore them.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="a6859-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6859-146">Next steps</span></span>
- [<span data-ttu-id="a6859-147">Azure felhőszolgáltatások hibakeresési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="a6859-147">Options for debugging Azure cloud services</span></span>](vs-azure-tools-debugging-cloud-services-overview.md)
- [<span data-ttu-id="a6859-148">Egy Visual Studio használatával Azure-felhőszolgáltatásban közzététele</span><span class="sxs-lookup"><span data-stu-id="a6859-148">Publishing an Azure cloud service using Visual Studio</span></span>](vs-azure-tools-publishing-a-cloud-service.md)