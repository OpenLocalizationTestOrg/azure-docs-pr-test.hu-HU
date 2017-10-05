---
title: "A közzétett Hibakeresés az Azure felhőalapú szolgáltatás a Visual Studio és az IntelliTrace |} Microsoft Docs"
description: "Ismerje meg, egy felhőalapú szolgáltatás, a Visual Studio és az IntelliTrace hibakeresése"
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
ms.openlocfilehash: 7905dfb97cbd7578a8422567fe674839d00c21ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a><span data-ttu-id="19b02-103">A közzétett Azure-felhőszolgáltatásban a Visual Studio és az IntelliTrace-hibakeresés</span><span class="sxs-lookup"><span data-stu-id="19b02-103">Debugging a published Azure cloud service with Visual Studio and IntelliTrace</span></span>
<span data-ttu-id="19b02-104">Intellitrace bejelentkezhet a szerepkör példánya nagy mennyiségű hibakeresési adatok az Azure-ban futtatott.</span><span class="sxs-lookup"><span data-stu-id="19b02-104">With IntelliTrace, you can log extensive debugging information for a role instance when it runs in Azure.</span></span> <span data-ttu-id="19b02-105">Ha a probléma okát van szüksége, az IntelliTrace-naplók segítségével végighaladhat a kódot a Visual Studio eszközből, mintha csak az Azure-ban futna.</span><span class="sxs-lookup"><span data-stu-id="19b02-105">If you need to find the cause of a problem, you can use the IntelliTrace logs to step through your code from Visual Studio as if it were running in Azure.</span></span> <span data-ttu-id="19b02-106">Gyakorlatilag IntelliTrace kulccsal rekordok programkód és környezeti adatok az Azure alkalmazás az Azure-ban felhőszolgáltatásként fut, és beállíthatja, a Visual Studio eszközből a rögzített adatokat.</span><span class="sxs-lookup"><span data-stu-id="19b02-106">In effect, IntelliTrace records key code execution and environment data when your Azure application is running as a cloud service in Azure, and lets you replay the recorded data from Visual Studio.</span></span> 

<span data-ttu-id="19b02-107">Ha rendelkezik telepített Visual Studio Enterprise IntelliTrace és az Azure-alkalmazásokban célok .NET-keretrendszer 4 vagy újabb verzió használható.</span><span class="sxs-lookup"><span data-stu-id="19b02-107">You can use IntelliTrace if you have Visual Studio Enterprise installed and your Azure application targets .NET Framework 4 or a later version.</span></span> <span data-ttu-id="19b02-108">IntelliTrace az Azure-szerepkörök adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="19b02-108">IntelliTrace collects information for your Azure roles.</span></span> <span data-ttu-id="19b02-109">Ezeket a szerepköröket mindig a 64 bites operációs rendszereket futtató virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="19b02-109">The virtual machines for these roles always run 64-bit operating systems.</span></span>

<span data-ttu-id="19b02-110">Alternatív megoldásként használható [távoli hibakeresés](http://go.microsoft.com/fwlink/p/?LinkId=623041) közvetlenül egy felhőszolgáltatás, amely az Azure-ban futó csatolni.</span><span class="sxs-lookup"><span data-stu-id="19b02-110">As an alternative, you can use [remote debugging](http://go.microsoft.com/fwlink/p/?LinkId=623041) to attach directly to a cloud service that's running in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19b02-111">IntelliTrace csak hibakeresési forgatókönyvekhez készült, és nem használható éles környezet.</span><span class="sxs-lookup"><span data-stu-id="19b02-111">IntelliTrace is intended for debug scenarios only, and should not be used for a production deployment.</span></span>
> 

## <a name="configure-an-azure-application-for-intellitrace"></a><span data-ttu-id="19b02-112">Az Azure-alkalmazások konfigurálása az IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="19b02-112">Configure an Azure application for IntelliTrace</span></span>
<span data-ttu-id="19b02-113">Az Azure-alkalmazások IntelliTrace engedélyezéséhez hozzon létre, és tegye közzé az alkalmazást a Visual Studio Azure projektből.</span><span class="sxs-lookup"><span data-stu-id="19b02-113">To enable IntelliTrace for an Azure application, you must create and publish the application from a Visual Studio Azure project.</span></span> <span data-ttu-id="19b02-114">Az Azure-ba való közzététele előtt IntelliTrace kell konfigurálása az Azure-alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="19b02-114">You must configure IntelliTrace for your Azure application before you publish it to Azure.</span></span> <span data-ttu-id="19b02-115">Ha az alkalmazás közzétételére IntelliTrace beállítása nélkül, akkor a projekt közzé kell.</span><span class="sxs-lookup"><span data-stu-id="19b02-115">If you publish your application without configuring IntelliTrace, you need to republish the project.</span></span> <span data-ttu-id="19b02-116">További információkért lásd: [közzététele egy Azure cloud services projekteket a Visual Studio használatával](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span><span class="sxs-lookup"><span data-stu-id="19b02-116">For more information, see [Publishing an Azure cloud services projects using Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span></span>

1. <span data-ttu-id="19b02-117">Ha készen áll az Azure alkalmazás közzétételéhez, győződjön meg arról, hogy a project build célkitűzések vannak-e beállítva **Debug**.</span><span class="sxs-lookup"><span data-stu-id="19b02-117">When you are ready to deploy your Azure application, verify that your project build targets are set to **Debug**.</span></span>

1. <span data-ttu-id="19b02-118">A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt, és válassza ki a helyi menüből **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="19b02-118">In **Solution Explorer**, right-click project, and, from the context menu, select **Publish**.</span></span>
   
1. <span data-ttu-id="19b02-119">Az a **Azure-alkalmazás közzététele** párbeszédpanel, válassza ki az Azure-előfizetés, válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="19b02-119">In the **Publish Azure Application** dialog, select the Azure subscription, and select **Next**.</span></span>

1. <span data-ttu-id="19b02-120">Az a **beállítások** lapon jelölje be a **speciális beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="19b02-120">In the **Settings** page, select the **Advanced Settings** tab.</span></span>

1. <span data-ttu-id="19b02-121">Kapcsolja be a **engedélyezése IntelliTrace** beállítás gyűjtése az IntelliTrace-naplók az alkalmazáshoz, ha a felhőben van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="19b02-121">Turn on the **Enable IntelliTrace** option to collect IntelliTrace logs for your application when it is published in the cloud.</span></span>
   
1. <span data-ttu-id="19b02-122">Az IntelliTrace-alapkonfiguráció testreszabásához jelölje be **beállítások** melletti **engedélyezése IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="19b02-122">To customize the basic IntelliTrace configuration, select **Settings** next to **Enable IntelliTrace**.</span></span>

    ![IntelliTrace beállítások hivatkozására](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. <span data-ttu-id="19b02-124">Az a **IntelliTrace beállítások** párbeszédpanelen, a napló, e hívás kapcsolatos információk összegyűjtéséhez, mely modulok és a folyamatok gyűjtéséhez naplóit, és mekkora lemezterület lefoglalása a felvétel számára események megadhat.</span><span class="sxs-lookup"><span data-stu-id="19b02-124">In the **IntelliTrace Settings** dialog, you can specify which events to log, whether to collect call information, which modules and processes to collect logs for, and how much space to allocate to the recording.</span></span> <span data-ttu-id="19b02-125">Intellitrace alkalmazással kapcsolatban további információkért lásd: [intellitrace-hibakeresés](http://go.microsoft.com/fwlink/?LinkId=214468).</span><span class="sxs-lookup"><span data-stu-id="19b02-125">For more information about IntelliTrace, see [Debugging with IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span></span>
   
    ![IntelliTrace-beállítások](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

<span data-ttu-id="19b02-127">Az IntelliTrace-naplót, az IntelliTrace-beállításokat (az alapértelmezett mérete 250 MB) a megadott maximális méret körkörös naplófájl.</span><span class="sxs-lookup"><span data-stu-id="19b02-127">The IntelliTrace log is a circular log file of the maximum size specified in the IntelliTrace settings (the default size is 250 MB).</span></span> <span data-ttu-id="19b02-128">IntelliTrace-naplókat a rendszer egy fájlba, a fájlrendszer, a virtuális gép gyűjti.</span><span class="sxs-lookup"><span data-stu-id="19b02-128">IntelliTrace logs are collected to a file in the file system of the virtual machine.</span></span> <span data-ttu-id="19b02-129">A naplók igénylésekor pillanatkép hajtja végre, ezen a ponton idő, és letölti a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="19b02-129">When you request the logs, a snapshot is taken at that point in time and downloaded to your local computer.</span></span>

<span data-ttu-id="19b02-130">Miután az Azure felhőszolgáltatást közzé lett téve az Azure-ba, azt is meghatározhatja, ha a IntelliTrace a Azure csomópontján engedélyezve van-e **Server Explorer**, a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="19b02-130">After the Azure cloud service has been published to Azure, you can determine if IntelliTrace has been enabled from the Azure node in **Server Explorer**, as shown in the following image:</span></span>

![Server Explorer - IntelliTrace engedélyezve](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a><span data-ttu-id="19b02-132">Töltse le az IntelliTrace-naplókat a szerepkörpéldányhoz</span><span class="sxs-lookup"><span data-stu-id="19b02-132">Download IntelliTrace logs for a role instance</span></span>
<span data-ttu-id="19b02-133">Visual Studio használatával, letöltheti a IntelliTrace-naplókat a szerepkör példánya a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="19b02-133">Using Visual Studio, you can download IntelliTrace logs for a role instance by following these steps:</span></span>

1. <span data-ttu-id="19b02-134">A **Server Explorer**, bontsa ki a **Felhőszolgáltatások** csomópont, és keresse meg a szerepkörpéldányt, amelynek a naplói, töltse le kívánja.</span><span class="sxs-lookup"><span data-stu-id="19b02-134">In **Server Explorer**, expand the **Cloud Services** node, and locate role instance whose logs you wish to download.</span></span> 

1. <span data-ttu-id="19b02-135">Kattintson a jobb gombbal a szerepkörpéldányt, és a s helyi menüben válassza a **IntelliTrace-naplók megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="19b02-135">Right-click the role instance, and from the s context menu, select **View IntelliTrace Logs**.</span></span> 

    ![IntelliTrace-naplók menüpont megjelenítése](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. <span data-ttu-id="19b02-137">Az IntelliTrace-naplók letölti a fájl a könyvtárban található a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="19b02-137">The IntelliTrace logs are downloaded to a file in a directory on your local computer.</span></span> <span data-ttu-id="19b02-138">Minden alkalommal, amikor kérnie az IntelliTrace-naplók, egy új pillanatkép jön létre.</span><span class="sxs-lookup"><span data-stu-id="19b02-138">Each time that you request the IntelliTrace logs, a new snapshot is created.</span></span> <span data-ttu-id="19b02-139">A naplók letöltése folyamatban van, amíg a Visual Studio megjeleníti a műveletnek az előrehaladását a **Azure tevékenységnapló** ablak.</span><span class="sxs-lookup"><span data-stu-id="19b02-139">While the logs are being downloaded, Visual Studio displays the progress of the operation in the **Azure Activity Log** window.</span></span> <span data-ttu-id="19b02-140">A következő ábrán látható, a részletek megtekintéséhez művelet sortételt bővítheti.</span><span class="sxs-lookup"><span data-stu-id="19b02-140">As shown in the following figure, you can expand the line item for the operation to see more detail.</span></span>

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

<span data-ttu-id="19b02-142">Továbbra is az IntelliTrace-naplók letöltése a Visual Studio működne.</span><span class="sxs-lookup"><span data-stu-id="19b02-142">You can continue to work in Visual Studio while the IntelliTrace logs are downloading.</span></span> <span data-ttu-id="19b02-143">Ha a napló letöltése befejeződött, a Visual Studio nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="19b02-143">When the log has finished downloading, it opens in Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="19b02-144">Az IntelliTrace-naplókat is tartalmazhatnak, kivételek fellépése, amelyek a keretrendszer hoz létre, és ezt követően kezeli.</span><span class="sxs-lookup"><span data-stu-id="19b02-144">The IntelliTrace logs might contain exceptions that the framework generates and handles afterwards.</span></span> <span data-ttu-id="19b02-145">Belső keretrendszer kódot állít elő, ezeket a kivételeket, így biztonságosan figyelmen kívül hagyhatja őket egy szerepkör indítása normál részeként.</span><span class="sxs-lookup"><span data-stu-id="19b02-145">Internal framework code generates these exceptions as a normal part of starting up a role, so you may safely ignore them.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="19b02-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19b02-146">Next steps</span></span>
- [<span data-ttu-id="19b02-147">Azure felhőszolgáltatások hibakeresési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="19b02-147">Options for debugging Azure cloud services</span></span>](vs-azure-tools-debugging-cloud-services-overview.md)
- [<span data-ttu-id="19b02-148">Egy Visual Studio használatával Azure-felhőszolgáltatásban közzététele</span><span class="sxs-lookup"><span data-stu-id="19b02-148">Publishing an Azure cloud service using Visual Studio</span></span>](vs-azure-tools-publishing-a-cloud-service.md)