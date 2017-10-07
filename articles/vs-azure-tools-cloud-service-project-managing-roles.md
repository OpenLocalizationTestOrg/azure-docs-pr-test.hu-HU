---
title: "a felhőalapú szolgáltatások a Visual Studio aaaManaging szerepkörök az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd és eltávolítása szerepkörök az Azure felhőszolgáltatások Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 131edc534d1110ba3d25cd00a3a24b643576875c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a><span data-ttu-id="d7cb5-103">Az Azure felhőszolgáltatások Visual Studio szerepkörök kezelése</span><span class="sxs-lookup"><span data-stu-id="d7cb5-103">Managing roles in Azure cloud services with Visual Studio</span></span>
<span data-ttu-id="d7cb5-104">Miután létrehozta az Azure felhőszolgáltatást, adja hozzá az új szerepkörök tooit, vagy meglévő szerepkörök eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-104">After you have created your Azure cloud service, you can add new roles tooit or remove existing roles from it.</span></span> <span data-ttu-id="d7cb5-105">Egy meglévő projektjébe importálhatja és alakítsa át a tooa szerepkör is.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-105">You can also import an existing project and convert it tooa role.</span></span> <span data-ttu-id="d7cb5-106">Például egy ASP.NET-webalkalmazás importálhatja és kijelöl egy webes szerepkörben.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-106">For example, you can import an ASP.NET web application and designate it as a web role.</span></span>

## <a name="adding-a-role-tooan-azure-cloud-service"></a><span data-ttu-id="d7cb5-107">Egy szerepkör tooan Azure-felhőszolgáltatásban hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7cb5-107">Adding a role tooan Azure cloud service</span></span>
<span data-ttu-id="d7cb5-108">a lépéseket követve hello végigvezeti Önt egy webes vagy feldolgozói szerepkör tooan Azure felhőszolgáltatás-projekt hozzáadása a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-108">hello following steps guide you through adding a web or worker role tooan Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="d7cb5-109">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-109">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="d7cb5-110">A **Megoldáskezelőben**, bontsa ki a hello projektcsomópontra</span><span class="sxs-lookup"><span data-stu-id="d7cb5-110">In **Solution Explorer**, expand hello project node</span></span>

1. <span data-ttu-id="d7cb5-111">Kattintson a jobb gombbal hello **szerepkörök** csomópont toodisplay hello helyi menü.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-111">Right-click hello **Roles** node toodisplay hello context menu.</span></span> <span data-ttu-id="d7cb5-112">Hello helyi menüből válassza ki a **Hozzáadás**, majd válasszon egy meglévő webes szerepkör vagy a feldolgozói szerepkör hello aktuális megoldás, vagy hozzon létre egy webes vagy feldolgozói szerepkör projekt.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-112">From hello context menu, select **Add**, then select an existing web role or worker role from hello current solution, or create a web or worker role project.</span></span> <span data-ttu-id="d7cb5-113">Is válassza ki a megfelelő projektben, például egy ASP.NET webalkalmazás projekthez, és társíthatja egy szerepkör-projektet.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-113">You can also select an appropriate project, such as an ASP.NET web application project, and associate it with a role project.</span></span>

    ![Menü Beállítások tooadd egy szerepkör tooan Azure felhőszolgáltatás-projekt](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a><span data-ttu-id="d7cb5-115">A szerepkör eltávolítását Azure-felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d7cb5-115">Removing a role from an Azure cloud service</span></span>
<span data-ttu-id="d7cb5-116">hello következő lépések végigvezetik a Visual Studio egy Azure-felhőszolgáltatás-projekt egy webes vagy feldolgozói szerepkör eltávolítását.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-116">hello following steps guide you through removing a web or worker role from an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="d7cb5-117">Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-117">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="d7cb5-118">A **Megoldáskezelőben**, bontsa ki a hello projektcsomópontra</span><span class="sxs-lookup"><span data-stu-id="d7cb5-118">In **Solution Explorer**, expand hello project node</span></span>

1. <span data-ttu-id="d7cb5-119">Bontsa ki a hello **szerepkörök** csomópont.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-119">Expand hello **Roles** node.</span></span>

1. <span data-ttu-id="d7cb5-120">Kattintson a jobb gombbal hello csomópont tooremove, és, hello helyi menüből jelöljük ki **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-120">Right-click hello node you want tooremove, and, from hello context menu, select **Remove**.</span></span> 

    ![Menü Beállítások tooadd egy szerepkör tooan Azure-felhőszolgáltatás](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-tooan-azure-cloud-service-project"></a><span data-ttu-id="d7cb5-122">Olvasása a következő egy szerepkör tooan Azure felhőszolgáltatás-projekt</span><span class="sxs-lookup"><span data-stu-id="d7cb5-122">Readding a role tooan Azure cloud service project</span></span>
<span data-ttu-id="d7cb5-123">Ha a szerepkör eltávolítása a felhőszolgáltatás-projekt, de később úgy dönt, tooadd hello biztonsági szerepkör toohello projekt, csak hello szerepkör nyilatkozat és alapvető attribútumait, például a végpontok és diagnosztikai információk, kerülnek.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-123">If you remove a role from your cloud service project but later decide tooadd hello role back toohello project, only hello role declaration and basic attributes, such as endpoints and diagnostics information, are added.</span></span> <span data-ttu-id="d7cb5-124">További erőforrások vagy hivatkozások kerülnek toohello `ServiceDefinition.csdef` fájl vagy toohello `ServiceConfiguration.cscfg` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-124">No additional resources or references are added toohello `ServiceDefinition.csdef` file or toohello `ServiceConfiguration.cscfg` file.</span></span> <span data-ttu-id="d7cb5-125">Ha azt szeretné, tooadd ezt az információt, toomanually kell adja hozzá ezeket a fájlokat programba.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-125">If you want tooadd this information, you need toomanually add it back into these files.</span></span>

<span data-ttu-id="d7cb5-126">Például előfordulhat, hogy távolítsa el a webes szerepkör-szolgáltatás, és később mellett dönt a szerepkör biztonsági tooadd megoldásba történő.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-126">For example, you might remove a web service role and later you decide tooadd this role back into your solution.</span></span> <span data-ttu-id="d7cb5-127">Ha így tesz, akkor hiba történik.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-127">If you do this, an error occurs.</span></span> <span data-ttu-id="d7cb5-128">tooprevent Ez a hiba tooadd hello rendelkezik `<LocalResources>` elem látható a következő hello XML vissza az hello `ServiceDefinition.csdef` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-128">tooprevent this error, you have tooadd hello `<LocalResources>` element shown in hello following XML back into hello `ServiceDefinition.csdef` file.</span></span> <span data-ttu-id="d7cb5-129">Használjon hello neve hello webes szerepkör-szolgáltatás hello hello name attribútum részeként vissza hello projektbe hozzáadott  **<LocalStorage>**  elemet.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-129">Use hello name of hello web service role that you added back into hello project as part of hello name attribute for hello **<LocalStorage>** element.</span></span> <span data-ttu-id="d7cb5-130">Ebben a példában hello webszolgáltatási szerepkörének hello neve nem **WCFServiceWebRole1**.</span><span class="sxs-lookup"><span data-stu-id="d7cb5-130">In this example, hello name of hello web service role is **WCFServiceWebRole1**.</span></span>

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a><span data-ttu-id="d7cb5-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7cb5-131">Next steps</span></span>
- [<span data-ttu-id="d7cb5-132">A Visual Studio hello szerepkörök az Azure-felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7cb5-132">Configure hello Roles for an Azure cloud service with Visual Studio</span></span>](vs-azure-tools-configure-roles-for-cloud-service.md)
