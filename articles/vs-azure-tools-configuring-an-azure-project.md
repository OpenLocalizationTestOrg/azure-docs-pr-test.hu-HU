---
title: "a Visual Studio egy Azure-felhőszolgáltatás-projekt aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure az Azure felhőalapú szolgáltatás projektre a Visual Studio, a projekt követelményeitől függően."
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
ms.openlocfilehash: 40eb5eedd6ea23bf03c8707431799be28beae701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="6ae55-103">A Visual Studio egy Azure-felhőszolgáltatás-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6ae55-103">Configure an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="6ae55-104">Egy Azure-felhőszolgáltatás-projekt, beállíthatja a projekt követelményeitől függően.</span><span class="sxs-lookup"><span data-stu-id="6ae55-104">You can configure an Azure cloud service project, depending on your requirements for that project.</span></span> <span data-ttu-id="6ae55-105">A következő kategóriák hello hello projekt tulajdonságait állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="6ae55-105">You can set properties for hello project for hello following categories:</span></span>

- <span data-ttu-id="6ae55-106">**Közzétételéhez egy cloud service tooAzure** -beállíthat egy tulajdonság toomake meg arról, hogy egy meglévő felhőalapú környezetben telepített tooAzure nem véletlenül törli.</span><span class="sxs-lookup"><span data-stu-id="6ae55-106">**Publish a cloud service tooAzure** - You can set a property toomake sure that an existing cloud service deployed tooAzure is not accidentally deleted.</span></span>
- <span data-ttu-id="6ae55-107">**Futtassa, vagy egy felhőalapú szolgáltatás a helyi számítógépen hello debug** -kiválaszthatja a szolgáltatás konfigurációs toouse, és adja meg, hogy toostart hello az Azure storage emulator.</span><span class="sxs-lookup"><span data-stu-id="6ae55-107">**Run or debug a cloud service on hello local computer** - You can select a service configuration toouse and indicate whether you want toostart hello Azure storage emulator.</span></span>
- <span data-ttu-id="6ae55-108">**Ellenőrizze a cloud service-csomag létrehozásakor** -eldöntheti, így biztosíthatja, hogy hello cloud service-csomag összes figyelmeztetés hibaként telepíti probléma nélkül tootreat.</span><span class="sxs-lookup"><span data-stu-id="6ae55-108">**Validate a cloud service package when it is created** - You can decide tootreat any warnings as errors so that you can ensure that hello cloud service package deploys without any issues.</span></span> 

## <a name="steps-tooconfigure-an-azure-cloud-service-project"></a><span data-ttu-id="6ae55-109">Lépéseket tooconfigure egy Azure-felhőszolgáltatás-projekt</span><span class="sxs-lookup"><span data-stu-id="6ae55-109">Steps tooconfigure an Azure cloud service project</span></span>
1. <span data-ttu-id="6ae55-110">Nyissa meg vagy egy felhőszolgáltatás-projekt létrehozása a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="6ae55-110">Open or create a cloud service project in Visual Studio</span></span>

1. <span data-ttu-id="6ae55-111">A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="6ae55-111">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>
   
1. <span data-ttu-id="6ae55-112">Hello projekt Tulajdonságok lapján válassza hello **fejlesztési** fülre.</span><span class="sxs-lookup"><span data-stu-id="6ae55-112">In hello project's properties page, select hello **Development** tab.</span></span>

    ![Projekt tulajdonságai menü](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. <span data-ttu-id="6ae55-114">Állítsa be **Rákérdezés a meglévő telepítés törlése előtt** túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="6ae55-114">Set **Prompt before deleting an existing deployment** too**True**.</span></span> <span data-ttu-id="6ae55-115">Ez a beállítás segít tooensure véletlenül törli nem egy meglévő Azure-telepítés</span><span class="sxs-lookup"><span data-stu-id="6ae55-115">This setting helps tooensure you don't accidentally delete an existing deployment in Azure</span></span>

1. <span data-ttu-id="6ae55-116">Szükséges válassza hello **szolgáltatáskonfiguráció** mely szolgáltatáskonfiguráció kívánt toouse futtatásakor vagy a felhőalapú szolgáltatás helyileg debug tooindicate.</span><span class="sxs-lookup"><span data-stu-id="6ae55-116">Select hello desired **Service configuration** tooindicate which service configuration you want toouse when you run or debug your cloud service locally.</span></span> <span data-ttu-id="6ae55-117">További információt a toomodify szolgáltatáskonfiguráció szerepkör, lásd: [hogyan tooconfigure hello szerepkörök az Azure cloud service, a Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="6ae55-117">For more information on how toomodify a service configuration for a role, see [How tooconfigure hello roles for an Azure cloud service with Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

1. <span data-ttu-id="6ae55-118">Állítsa be **Start Azure storage emulator** túl**igaz** toostart hello az Azure storage emulator futtatásakor vagy a felhőalapú szolgáltatás helyi hibakeresése.</span><span class="sxs-lookup"><span data-stu-id="6ae55-118">Set **Start Azure storage emulator** too**True** toostart hello Azure storage emulator when you run or debug your cloud service locally.</span></span>

1. <span data-ttu-id="6ae55-119">Állítsa be **figyelmeztetések hibaként** túl**igaz** toomake meg arról, hogy Ön nem tehető közzé, ha a csomag érvényesítési hibák vannak.</span><span class="sxs-lookup"><span data-stu-id="6ae55-119">Set **Treat warnings as errors** too**True** toomake sure you cannot publish if there are package validation errors.</span></span>

1. <span data-ttu-id="6ae55-120">Állítsa be **webes projekt portok használatára** túl**igaz** toomake arról, hogy, hogy a webes szerepkör hello azonos port minden egyes elindításakor azt a helyi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6ae55-120">Set **Use web project ports** too**True** toomake sure that your web role uses hello same port each time it starts locally in IIS Express.</span></span>

1. <span data-ttu-id="6ae55-121">Hello Visual Studio eszköztárán válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6ae55-121">From hello Visual Studio toolbar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ae55-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ae55-122">Next steps</span></span>
- [<span data-ttu-id="6ae55-123">Több szolgáltatáskonfiguráció használata Azure-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6ae55-123">Configure an Azure project using multiple service configurations</span></span>](vs-azure-tools-multiple-services-project-configurations.md)

