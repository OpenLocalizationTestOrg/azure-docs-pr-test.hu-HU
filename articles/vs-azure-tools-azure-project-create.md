---
title: "a Visual Studio egy Azure-felhőszolgáltatás-projekt aaaCreating |} Microsoft Docs"
description: "Ismerje meg, most toocreate a Visual Studio egy Azure-felhőszolgáltatás-projekt"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3c357016aa423688199a7ab3a670115e33a98fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="6cbdb-103">Egy Azure-felhőszolgáltatás-projekt létrehozása a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6cbdb-103">Creating an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="6cbdb-104">hello Azure Tools for Visual Studio biztosít a projekt sablont, amely lehetővé teszi, hogy hozzon létre egy Azure felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-104">hello Azure Tools for Visual Studio provides a project template that lets you create an Azure cloud service.</span></span> <span data-ttu-id="6cbdb-105">Hello projekt létrehozása után a Visual Studio lehetővé teszi a tooconfigure, hibakeresését és hello cloud service tooAzure telepítése.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-105">Once hello project has been created, Visual Studio enables you tooconfigure, debug, and deploy hello cloud service tooAzure.</span></span>

## <a name="steps-toocreate-an-azure-cloud-service-project-in-visual-studio"></a><span data-ttu-id="6cbdb-106">A Visual Studio egy Azure-felhőszolgáltatás-projekt lépéseket toocreate</span><span class="sxs-lookup"><span data-stu-id="6cbdb-106">Steps toocreate an Azure cloud service project in Visual Studio</span></span>
<span data-ttu-id="6cbdb-107">Ez a szakasz végigvezeti az Azure-felhőszolgáltatás-projekt létrehozása a Visual Studióban legalább egy webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-107">This section walks you through creating an Azure cloud service project in Visual Studio with one or more web roles.</span></span>  

1. <span data-ttu-id="6cbdb-108">Indítsa el a Visual Studio rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-108">Start Visual Studio as an administrator.</span></span>

1. <span data-ttu-id="6cbdb-109">A hello fő menüben válassza **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-109">On hello main menu, select **File** > **New** > **Project**.</span></span>

1. <span data-ttu-id="6cbdb-110">Válassza ki **felhő** hello Visual C# vagy a Visual Basic sablon csomópontok projektre, és válassza a **Azure Cloud Service** hello sablonok közül.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-110">Select **Cloud** from hello Visual C# or Visual Basic project template nodes, and select **Azure Cloud Service** from hello list of templates.</span></span>

    ![Új Azure-felhőszolgáltatás](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. <span data-ttu-id="6cbdb-112">Adja meg, melyik hello a projekt toouse toodevelop kívánt .NET-keretrendszer verzióját.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-112">Specify which version of hello .NET Framework you want toouse toodevelop your project.</span></span>

1. <span data-ttu-id="6cbdb-113">Adjon nevet és a projekt helyét és hello megoldás nevét.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-113">Enter a name and location for your project and a name for hello solution.</span></span> 

1. <span data-ttu-id="6cbdb-114">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-114">Select **OK**.</span></span>

1. <span data-ttu-id="6cbdb-115">A hello **új Microsoft Azure Cloud Service** párbeszédpanelen jelölje be hello szerepkörök tooadd szeretné, és válassza ki a hello jobbra mutató nyíl gombra tooadd őket tooyour megoldás.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-115">In hello **New Microsoft Azure Cloud Service** dialog, select hello roles that you want tooadd, and choose hello right arrow button tooadd them tooyour solution.</span></span>

    ![Új Azure cloud service szerepkörök kiválasztása](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. <span data-ttu-id="6cbdb-117">szerepkör hozzáadott, a hello hello szerepkör rámutatáskor toorename **új Microsoft Azure Cloud Service** párbeszédpanel, és hello helyi menüből válassza ki a **átnevezése**.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-117">toorename a role that you've added, hover on hello role in hello **New Microsoft Azure Cloud Service** dialog, and, from hello context menu, select **Rename**.</span></span> <span data-ttu-id="6cbdb-118">Egy szerepkör nevezze át a megoldás belül (a hello **Megoldáskezelőben**) után már hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-118">You can also rename a role within your solution (in hello **Solution Explorer**) after it has been added.</span></span>

    ![Azure cloud service szerepkör átnevezése](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

<span data-ttu-id="6cbdb-120">hello Visual Studio Azure projekt társítások toohello projektek hello megoldásban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-120">hello Visual Studio Azure project has associations toohello role projects in hello solution.</span></span> <span data-ttu-id="6cbdb-121">hello projekt is hello *szolgáltatásdefiníciós fájl* és *szolgáltatás konfigurációs fájlja*:</span><span class="sxs-lookup"><span data-stu-id="6cbdb-121">hello project also includes hello *service definition file* and *service configuration file*:</span></span>

- <span data-ttu-id="6cbdb-122">**Szolgáltatásdefiníciós fájl** -hello futásidejű beállításait az alkalmazás, például milyen-szerepkörök szükségesek, a végpontok és a virtuális gép méretét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-122">**Service definition file** - Defines hello runtime settings for your application, including what roles are required, endpoints, and virtual machine size.</span></span> 
- <span data-ttu-id="6cbdb-123">**Szolgáltatáskonfigurációs fájlt** -hány példánya egy futnak, és az egy szerepkörhöz definiált hello-beállítások értékeit hello konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="6cbdb-123">**Service configuration file** - Configures how many instances of a role are run and hello values of hello settings defined for a role.</span></span> 

<span data-ttu-id="6cbdb-124">További információ ezen fájlokkal kapcsolatban, tekintse meg [hello szerepkörök az Azure-felhőszolgáltatás konfigurálása a Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="6cbdb-124">For more information about these files, see [Configure hello Roles for an Azure cloud service with Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6cbdb-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6cbdb-125">Next steps</span></span>
- [<span data-ttu-id="6cbdb-126">Az Azure cloud service projektek a Visual Studio szerepkörök kezelése</span><span class="sxs-lookup"><span data-stu-id="6cbdb-126">Managing roles in Azure cloud service projects with Visual Studio</span></span>](./vs-azure-tools-cloud-service-project-managing-roles.md)
