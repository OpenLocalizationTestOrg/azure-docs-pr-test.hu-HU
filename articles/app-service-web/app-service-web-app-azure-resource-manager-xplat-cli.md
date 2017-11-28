---
title: "Az Azure Resource Manager-alapú platformfüggetlen parancssori eszközök, az Azure Web App |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti az Azure Web alkalmazásokat az új Azure Resource Manager-alapú platformfüggetlen parancssori eszközök segítségével."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: d415b195-4262-416f-b59f-7e1aef200054
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: 7a03e1417617453c43edcc3787da10d171359757
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a><span data-ttu-id="4fbaa-103">Azure App Service az Azure Resource Manager-alapú XPlat parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="4fbaa-103">Using Azure Resource Manager-Based XPlat CLI for Azure App Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4fbaa-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4fbaa-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="4fbaa-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4fbaa-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)

<span data-ttu-id="4fbaa-106">A Microsoft Azure platformfüggetlen parancssori eszközök verzió 0.10.5 a kiadástól kezdve új parancsok váltak elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-106">With the release of Microsoft Azure Cross-platform Command-Line Tools version 0.10.5, new commands have been added.</span></span> <span data-ttu-id="4fbaa-107">Ezek a parancsok a felhasználónak, Azure Resource Manager-alapú PowerShell-parancsokat használhatja az App Service kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-107">These commands give the user the ability to use Azure Resource Manager-based PowerShell commands to manage App Service.</span></span>

<span data-ttu-id="4fbaa-108">Erőforráscsoportok kezelésével kapcsolatos információkért lásd: [Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="4fbaa-108">To learn about managing Resource Groups, see [Use the Azure CLI to manage Azure resources and resource groups](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span></span> 

> [!NOTE] 
> <span data-ttu-id="4fbaa-109">Emellett kipróbálásához [Azure CLI 2.0](https://github.com/Azure/azure-cli), a következő generációs CLI erőforrás felügyeleti telepítési modell pythonban írt.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-109">Also, try out [Azure CLI 2.0](https://github.com/Azure/azure-cli), a next-generation CLI written in Python for the resource management deployment model.</span></span>
>
>

## <a name="managing-app-service-plans"></a><span data-ttu-id="4fbaa-110">Kezelés az App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="4fbaa-110">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="4fbaa-111">Az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-111">Create an App Service Plan</span></span>
<span data-ttu-id="4fbaa-112">Az app service-csomag létrehozásához használja a **azure appserviceplan létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-112">To create an app service plan, use the **azure appserviceplan create** command.</span></span>

<span data-ttu-id="4fbaa-113">A különböző paraméterek leírását a következők:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-113">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="4fbaa-114">**--Erőforráscsoport**: erőforráscsoport, amely tartalmazza az újonnan létrehozott app service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-114">**--resource-group**: resource group that includes the newly created app service plan.</span></span>
* <span data-ttu-id="4fbaa-115">**--name**: az app service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-115">**--name**: name of the app service plan.</span></span>
* <span data-ttu-id="4fbaa-116">**--hely**: app service-csomag hely.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-116">**--location**: app service plan location.</span></span>
* <span data-ttu-id="4fbaa-117">**--sku**: a kívánt tarifacsomagot termékváltozat (a lehetőségek a következők: F1 (ingyenes).</span><span class="sxs-lookup"><span data-stu-id="4fbaa-117">**--sku**:  the desired pricing sku (The options are: F1 (Free).</span></span> <span data-ttu-id="4fbaa-118">A D1 (megosztott).</span><span class="sxs-lookup"><span data-stu-id="4fbaa-118">D1 (Shared).</span></span> <span data-ttu-id="4fbaa-119">A K1 (alapvető kicsi), K2 (alapvető közepes), és B3 (Basic nagy).</span><span class="sxs-lookup"><span data-stu-id="4fbaa-119">B1 (Basic Small), B2 (Basic Medium), and B3 (Basic Large).</span></span> <span data-ttu-id="4fbaa-120">S1 (szabványos kicsi), S2 (szabványos közepes), és S3 (szabványos nagy).</span><span class="sxs-lookup"><span data-stu-id="4fbaa-120">S1 (Standard Small), S2 (Standard Medium), and S3 (Standard Large).</span></span> <span data-ttu-id="4fbaa-121">P1 (prémium szintű kicsi), P2 (prémium szintű közepes), és P3 (prémium nagy).)</span><span class="sxs-lookup"><span data-stu-id="4fbaa-121">P1 (Premium Small), P2 (Premium Medium), and P3 (Premium Large).)</span></span>
* <span data-ttu-id="4fbaa-122">**--példányok**: szolgáltatási terv (alapértelmezett értéke 1) az alkalmazásban munkavállalók száma.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-122">**--instances**: the number of workers in the app service plan (Default value is 1).</span></span>

<span data-ttu-id="4fbaa-123">Például, hogy használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-123">Example to use this cmdlet:</span></span>

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="4fbaa-124">A Linux App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-124">Create a Linux App Service Plan</span></span>

<span data-ttu-id="4fbaa-125">Azonos **azure appserviceplan létrehozása** parancsot, a kiegészítő paraméterrel **--islinux true**.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-125">Using the same **azure appserviceplan create** command, with the extra parameter **--islinux true**.</span></span> <span data-ttu-id="4fbaa-126">Jegyezze fel a korlátozások és a leírt régiók [App Service Linux bemutatása](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="4fbaa-126">Note the restrictions and regions outlined in [Introduction to App Service on Linux](app-service-linux-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="4fbaa-127">Lista meglévő App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="4fbaa-127">List Existing App Service Plans</span></span>
<span data-ttu-id="4fbaa-128">A meglévő app service-csomagokról kilistázhatja **azure appserviceplan lista** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-128">To list the existing app service plans, use **azure appserviceplan list** command.</span></span>

<span data-ttu-id="4fbaa-129">Egy adott erőforráscsoportba tartozó összes app service csomagokban listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-129">To list all app service plans under a specific resource group, use:</span></span>

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="4fbaa-130">Egy adott app service-csomag használatához **azure appserviceplan megjelenítése** parancs:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-130">To get a specific app service plan, use **azure appserviceplan show** command:</span></span>

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="4fbaa-131">Egy meglévő App Service-csomag konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-131">Configure an existing App Service Plan</span></span>
<span data-ttu-id="4fbaa-132">Egy meglévő app service-csomag beállításainak módosításához használja a **azure appserviceplan config** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-132">To change the settings for an existing app service plan, use the **azure appserviceplan config** command.</span></span> <span data-ttu-id="4fbaa-133">Módosíthatja a termékváltozat, és a dolgozók száma</span><span class="sxs-lookup"><span data-stu-id="4fbaa-133">You can change the sku, and the number of workers</span></span> 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="4fbaa-134">Az App Service-csomag skálázás</span><span class="sxs-lookup"><span data-stu-id="4fbaa-134">Scaling an App Service Plan</span></span>
<span data-ttu-id="4fbaa-135">Egy meglévő App Service-csomag méretezéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-135">To scale an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a><span data-ttu-id="4fbaa-136">Az App Service-csomag Termékváltozata módosítása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-136">Changing the SKU of an App Service Plan</span></span>
<span data-ttu-id="4fbaa-137">Egy meglévő App Service-csomag termékváltozata módosításához használja:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-137">To change the sku of an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="4fbaa-138">Egy meglévő App Service-csomag törlése</span><span class="sxs-lookup"><span data-stu-id="4fbaa-138">Delete an existing App Service Plan</span></span>
<span data-ttu-id="4fbaa-139">Egy meglévő app service-csomag törlése, minden hozzárendelt alkalmazás kell helyezni vagy először törölni.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-139">To delete an existing app service plan, all assigned apps need to be moved or deleted first.</span></span> <span data-ttu-id="4fbaa-140">Ezután használja a **azure webalkalmazás törlése** parancs segítségével törölheti az app service-csomag.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-140">Then using the **azure webapp delete** command you can delete the app service plan.</span></span>

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a><span data-ttu-id="4fbaa-141">Az App Service-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="4fbaa-141">Managing App Service apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="4fbaa-142">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-142">Create a web app</span></span>
<span data-ttu-id="4fbaa-143">A webes alkalmazás létrehozásához használja a **azure webalkalmazás létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-143">To create a web app, use the **azure webapp create** command.</span></span>

<span data-ttu-id="4fbaa-144">A különböző paraméterek leírását a következők:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-144">Following are descriptions of the different parameters:</span></span>

* <span data-ttu-id="4fbaa-145">**--name**: a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-145">**--name**: name for the web app.</span></span>
* <span data-ttu-id="4fbaa-146">**--terv**: a webalkalmazás üzemeltetéséhez használt service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-146">**--plan**: name for the service plan used to host the web app.</span></span>
* <span data-ttu-id="4fbaa-147">**--Erőforráscsoport**: erőforráscsoport, amelyen az App service-csomag.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-147">**--resource-group**: resource group that hosts the App service plan.</span></span>
* <span data-ttu-id="4fbaa-148">**--hely**: a webes alkalmazás helyét.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-148">**--location**: the web app location.</span></span>

<span data-ttu-id="4fbaa-149">Például, hogy használja a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-149">Example to use this cmdlet:</span></span>

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a><span data-ttu-id="4fbaa-150">Egy meglévő alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="4fbaa-150">Delete an existing app</span></span>
<span data-ttu-id="4fbaa-151">Meglévő alkalmazás törléséhez használja a **azure webalkalmazás törlése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-151">To delete an existing app, you can use the **azure webapp delete** command.</span></span> <span data-ttu-id="4fbaa-152">Meg kell adnia az alkalmazás nevét és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-152">You need to specify the name of the app and the resource group name.</span></span>

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a><span data-ttu-id="4fbaa-153">Meglévő alkalmazások felsorolása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-153">List existing apps</span></span>
<span data-ttu-id="4fbaa-154">A meglévő alkalmazások listájában, használja a **azure webalkalmazás lista** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-154">To list the existing apps, use the **azure webapp list** command.</span></span>

<span data-ttu-id="4fbaa-155">Egy adott erőforráscsoportban található összes alkalmazások listájában, használja:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-155">To list all apps in a specific resource group, use:</span></span>

    azure webapp list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="4fbaa-156">Ahhoz, hogy egy adott alkalmazást, használja a **azure webalkalmazás megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-156">To get a specific app, use the **azure webapp show** command.</span></span>

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a><span data-ttu-id="4fbaa-157">Meglévő alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-157">Configure an existing app</span></span>
<span data-ttu-id="4fbaa-158">A beállítások és a meglévő alkalmazás konfiguráció módosításához használja a **azure webalkalmazás konfiguráció** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-158">To change the settings and configurations for an existing app, use the **azure webapp config set** command.</span></span>

<span data-ttu-id="4fbaa-159">(1). példa: a php verzióját egy alkalmazás megváltoztatása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-159">Example (1): change the php version of a app</span></span> 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

<span data-ttu-id="4fbaa-160">(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-160">Example (2): add or change app settings</span></span>

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

<span data-ttu-id="4fbaa-161">Tudni, hogy milyen más konfigurációs lehet módosítani, használja a **azure webalkalmazás konfigurációs nulla -h** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-161">To know what other configuration can be changed, use the **azure webapp config set -h** command.</span></span>

### <a name="change-the-state-of-an-existing-app"></a><span data-ttu-id="4fbaa-162">A meglévő alkalmazás állapotának módosítása</span><span class="sxs-lookup"><span data-stu-id="4fbaa-162">Change the state of an existing app</span></span>
#### <a name="restart-an-app"></a><span data-ttu-id="4fbaa-163">Indítsa újra az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="4fbaa-163">Restart an app</span></span>
<span data-ttu-id="4fbaa-164">Indítsa újra az alkalmazást, meg kell adnia az alkalmazás nevét és az erőforrás csoportja.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-164">To restart an app, you must specify the name and resource group of the app.</span></span>

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a><span data-ttu-id="4fbaa-165">Állítsa le az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="4fbaa-165">Stop an app</span></span>
<span data-ttu-id="4fbaa-166">Állítsa le az alkalmazást, meg kell adnia az alkalmazás nevét és az erőforrás csoportja.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-166">To stop an app, you must specify the name and resource group of the app.</span></span>

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a><span data-ttu-id="4fbaa-167">Indítsa el az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="4fbaa-167">Start an app</span></span>
<span data-ttu-id="4fbaa-168">Indítsa el az alkalmazást, meg kell adnia az alkalmazás nevét és az erőforrás csoportja.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-168">To start an app, you must specify the name and resource group of the app.</span></span>

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a><span data-ttu-id="4fbaa-169">Egy alkalmazás közzétételi profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="4fbaa-169">Manage publishing profiles of an app</span></span>
<span data-ttu-id="4fbaa-170">Minden alkalmazás rendelkezik egy közzétételi profilt, amely segítségével a kód közzététele.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-170">Each app has a publishing profile that can be used to publish your code.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="4fbaa-171">Kérhető le a közzétételi profil</span><span class="sxs-lookup"><span data-stu-id="4fbaa-171">Get Publishing Profile</span></span>
<span data-ttu-id="4fbaa-172">A közzétételi profil alkalmazások használatához:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-172">To get the publishing profile for a app, use:</span></span>

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

<span data-ttu-id="4fbaa-173">Ez a parancs echók a közzétételi profil felhasználónevet és jelszót a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="4fbaa-173">This command echoes the publishing profile username and password to the command line.</span></span>

### <a name="manage-app-hostnames"></a><span data-ttu-id="4fbaa-174">Alkalmazás állomásnevek kezelése</span><span class="sxs-lookup"><span data-stu-id="4fbaa-174">Manage app hostnames</span></span>
<span data-ttu-id="4fbaa-175">Az alkalmazás állomásnévkötéseket kezeléséhez használja a **azure webalkalmazás config állomásnevek** parancs</span><span class="sxs-lookup"><span data-stu-id="4fbaa-175">To manage hostname bindings for your app, use the **azure webapp config hostnames** command</span></span>  

#### <a name="list-hostname-bindings"></a><span data-ttu-id="4fbaa-176">Lista állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="4fbaa-176">List hostname bindings</span></span>
<span data-ttu-id="4fbaa-177">Az alkalmazás aktuális állomásnévkötéseket használatához:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-177">To get the current hostname bindings for an app, use:</span></span>

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a><span data-ttu-id="4fbaa-178">Adja hozzá az állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="4fbaa-178">Add hostname bindings</span></span>
<span data-ttu-id="4fbaa-179">Állomásnévkötéseket alkalmazás hozzáadásához használja:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-179">To add hostname bindings to an app, use:</span></span>

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a><span data-ttu-id="4fbaa-180">Törölje az állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="4fbaa-180">Delete hostname bindings</span></span>
<span data-ttu-id="4fbaa-181">Állomásnévkötéseket törléséhez használja:</span><span class="sxs-lookup"><span data-stu-id="4fbaa-181">To delete hostname bindings, use:</span></span>

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a><span data-ttu-id="4fbaa-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fbaa-182">Next Steps</span></span>
* <span data-ttu-id="4fbaa-183">Azure Resource Manager CLI-támogatással kapcsolatos információkért lásd: [Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="4fbaa-183">To learn about Azure Resource Manager CLI support, see [Use the Azure CLI to manage Azure resources and resource groups.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span></span>
* <span data-ttu-id="4fbaa-184">App Service PowerShell-lel kezelésével kapcsolatos információkért lásd: [Using Azure Resource Manager-Based PowerShell kezelése Azure Web Apps használatával.](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="4fbaa-184">To learn about managing App Service using PowerShell, see [Using Azure Resource Manager-Based PowerShell to Manage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span></span>
* <span data-ttu-id="4fbaa-185">Azure App Service Linux rendszeren, lásd: [App Service Linux bemutatása](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="4fbaa-185">To learn about Azure App Service on Linux, see [Introduction to App Service on Linux](app-service-linux-intro.md)</span></span>
