---
title: "A Visual Studio egy Azure-felhőszolgáltatás-projekt konfigurálása |} Microsoft Docs"
description: "További tudnivalók az Azure-felhőszolgáltatás-projekt konfigurálása a Visual Studio, a projekt követelményeitől függően."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 799715093bd1a8c8e77e6cdb7168670dc263dfa5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="e5ede-103">A Visual Studio egy Azure-felhőszolgáltatás-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5ede-103">Configure an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="e5ede-104">Egy Azure-felhőszolgáltatás-projekt, beállíthatja a projekt követelményeitől függően.</span><span class="sxs-lookup"><span data-stu-id="e5ede-104">You can configure an Azure cloud service project, depending on your requirements for that project.</span></span> <span data-ttu-id="e5ede-105">Beállítható a projekthez az alábbi kategóriákban:</span><span class="sxs-lookup"><span data-stu-id="e5ede-105">You can set properties for the project for the following categories:</span></span>

- <span data-ttu-id="e5ede-106">**Egy felhőalapú szolgáltatás közzététele az Azure-bA** -győződjön meg arról, hogy egy meglévő felhőalapú szolgáltatást, az Azure rendszerbe telepítendő nem véletlenül törli egy tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="e5ede-106">**Publish a cloud service to Azure** - You can set a property to make sure that an existing cloud service deployed to Azure is not accidentally deleted.</span></span>
- <span data-ttu-id="e5ede-107">**Futtassa, vagy egy felhőalapú szolgáltatás a helyi számítógépen debug** -kiválaszthatja, hogy a szolgáltatás konfigurációját használja, és adja meg, hogy az Azure storage emulator elindításához.</span><span class="sxs-lookup"><span data-stu-id="e5ede-107">**Run or debug a cloud service on the local computer** - You can select a service configuration to use and indicate whether you want to start the Azure storage emulator.</span></span>
- <span data-ttu-id="e5ede-108">**Ellenőrizze a cloud service-csomag létrehozásakor** -úgy is dönt, hogy hibaként figyelmeztetéseket, így biztosítható, hogy a cloud service-csomag probléma nélkül telepíti.</span><span class="sxs-lookup"><span data-stu-id="e5ede-108">**Validate a cloud service package when it is created** - You can decide to treat any warnings as errors so that you can ensure that the cloud service package deploys without any issues.</span></span> 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a><span data-ttu-id="e5ede-109">Egy Azure-felhőszolgáltatás-projekt konfigurálásának lépései</span><span class="sxs-lookup"><span data-stu-id="e5ede-109">Steps to configure an Azure cloud service project</span></span>
1. <span data-ttu-id="e5ede-110">Nyissa meg vagy egy felhőszolgáltatás-projekt létrehozása a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="e5ede-110">Open or create a cloud service project in Visual Studio</span></span>

1. <span data-ttu-id="e5ede-111">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki a helyi menüből **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e5ede-111">In **Solution Explorer**, right-click the project, and, from the context menu, select **Properties**.</span></span>
   
1. <span data-ttu-id="e5ede-112">A projekt Tulajdonságok lapján válassza a **fejlesztési** fülre.</span><span class="sxs-lookup"><span data-stu-id="e5ede-112">In the project's properties page, select the **Development** tab.</span></span>

    ![Projekt tulajdonságai menü](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. <span data-ttu-id="e5ede-114">Állítsa be **Rákérdezés a meglévő telepítés törlése előtt** való **igaz**.</span><span class="sxs-lookup"><span data-stu-id="e5ede-114">Set **Prompt before deleting an existing deployment** to **True**.</span></span> <span data-ttu-id="e5ede-115">Ez a beállítás segít biztosítani, ne véletlenül törli egy meglévő Azure-telepítés</span><span class="sxs-lookup"><span data-stu-id="e5ede-115">This setting helps to ensure you don't accidentally delete an existing deployment in Azure</span></span>

1. <span data-ttu-id="e5ede-116">Válassza ki a kívánt **szolgáltatáskonfiguráció** mely szolgáltatáskonfiguráció jelzi a futtatásakor vagy a felhőalapú szolgáltatás helyileg debug használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="e5ede-116">Select the desired **Service configuration** to indicate which service configuration you want to use when you run or debug your cloud service locally.</span></span> <span data-ttu-id="e5ede-117">A szerepkör szolgáltatáskonfiguráció módosítása további információkért lásd: [a szerepkörök az Azure-felhőszolgáltatás konfigurálása a Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="e5ede-117">For more information on how to modify a service configuration for a role, see [How to configure the roles for an Azure cloud service with Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

1. <span data-ttu-id="e5ede-118">Állítsa be **Start Azure storage emulator** való **igaz** futtatásakor vagy a felhőalapú szolgáltatás helyi hibakeresése az Azure storage emulator indításához.</span><span class="sxs-lookup"><span data-stu-id="e5ede-118">Set **Start Azure storage emulator** to **True** to start the Azure storage emulator when you run or debug your cloud service locally.</span></span>

1. <span data-ttu-id="e5ede-119">Állítsa be **figyelmeztetések hibaként** való **igaz** győződjön meg arról, nem tehető közzé, ha a csomag érvényesítési hibák vannak.</span><span class="sxs-lookup"><span data-stu-id="e5ede-119">Set **Treat warnings as errors** to **True** to make sure you cannot publish if there are package validation errors.</span></span>

1. <span data-ttu-id="e5ede-120">Állítsa be **webes projekt portok használatára** való **igaz** győződjön meg arról, hogy a webes szerepkör ugyanazt a portot használja minden egyes elindításakor azt a helyi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e5ede-120">Set **Use web project ports** to **True** to make sure that your web role uses the same port each time it starts locally in IIS Express.</span></span>

1. <span data-ttu-id="e5ede-121">Válassza ki a Visual Studio eszköztár **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e5ede-121">From the Visual Studio toolbar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5ede-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5ede-122">Next steps</span></span>
- [<span data-ttu-id="e5ede-123">Több szolgáltatáskonfiguráció használata Azure-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5ede-123">Configure an Azure project using multiple service configurations</span></span>](vs-azure-tools-multiple-services-project-configurations.md)

