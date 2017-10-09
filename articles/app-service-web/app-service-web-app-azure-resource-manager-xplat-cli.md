---
title: "aaaAzure Resource Manager-alapú platformfüggetlen parancssori eszközök Azure webalkalmazás számára |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello új Azure Resource Manager-alapú platformfüggetlen parancssori eszközök toomanage az Azure Web Apps."
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
ms.openlocfilehash: 5f5e03edcb01154aef3bd220cd27358f69656ef4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a><span data-ttu-id="7fc21-103">Azure App Service az Azure Resource Manager-alapú XPlat parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="7fc21-103">Using Azure Resource Manager-Based XPlat CLI for Azure App Service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7fc21-104">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7fc21-104">Azure CLI</span></span>](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [<span data-ttu-id="7fc21-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7fc21-105">Azure PowerShell</span></span>](app-service-web-app-azure-resource-manager-powershell.md)

<span data-ttu-id="7fc21-106">A Microsoft Azure platformfüggetlen parancssori eszközök verzió 0.10.5 hello kiadással új parancsok váltak elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="7fc21-106">With hello release of Microsoft Azure Cross-platform Command-Line Tools version 0.10.5, new commands have been added.</span></span> <span data-ttu-id="7fc21-107">Ezek a parancsok hello felhasználói hello képességét toouse Azure Resource Manager-alapú PowerShell parancsok toomanage App Service biztosítják.</span><span class="sxs-lookup"><span data-stu-id="7fc21-107">These commands give hello user hello ability toouse Azure Resource Manager-based PowerShell commands toomanage App Service.</span></span>

<span data-ttu-id="7fc21-108">toolearn erőforráscsoportok, kezelésével kapcsolatban lásd: [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="7fc21-108">toolearn about managing Resource Groups, see [Use hello Azure CLI toomanage Azure resources and resource groups](../azure-resource-manager/xplat-cli-azure-resource-manager.md).</span></span> 

> [!NOTE] 
> <span data-ttu-id="7fc21-109">Emellett kipróbálásához [Azure CLI 2.0](https://github.com/Azure/azure-cli), a következő generációs CLI hello erőforrás felügyeleti telepítési modell pythonban írt.</span><span class="sxs-lookup"><span data-stu-id="7fc21-109">Also, try out [Azure CLI 2.0](https://github.com/Azure/azure-cli), a next-generation CLI written in Python for hello resource management deployment model.</span></span>
>
>

## <a name="managing-app-service-plans"></a><span data-ttu-id="7fc21-110">Kezelés az App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="7fc21-110">Managing App Service Plans</span></span>
### <a name="create-an-app-service-plan"></a><span data-ttu-id="7fc21-111">Az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fc21-111">Create an App Service Plan</span></span>
<span data-ttu-id="7fc21-112">az app service-csomag toocreate hello használata **azure appserviceplan létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-112">toocreate an app service plan, use hello **azure appserviceplan create** command.</span></span>

<span data-ttu-id="7fc21-113">Alább olvasható hello más paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="7fc21-113">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="7fc21-114">**--Erőforráscsoport**: erőforráscsoport, amely tartalmazza az újonnan létrehozott hello app service-csomag.</span><span class="sxs-lookup"><span data-stu-id="7fc21-114">**--resource-group**: resource group that includes hello newly created app service plan.</span></span>
* <span data-ttu-id="7fc21-115">**--name**: hello app service-csomag neve.</span><span class="sxs-lookup"><span data-stu-id="7fc21-115">**--name**: name of hello app service plan.</span></span>
* <span data-ttu-id="7fc21-116">**--hely**: app service-csomag hely.</span><span class="sxs-lookup"><span data-stu-id="7fc21-116">**--location**: app service plan location.</span></span>
* <span data-ttu-id="7fc21-117">**--sku**: hello szükséges árképzési termékváltozat (hello lehetőségek: F1 (ingyenes).</span><span class="sxs-lookup"><span data-stu-id="7fc21-117">**--sku**:  hello desired pricing sku (hello options are: F1 (Free).</span></span> <span data-ttu-id="7fc21-118">A D1 (megosztott).</span><span class="sxs-lookup"><span data-stu-id="7fc21-118">D1 (Shared).</span></span> <span data-ttu-id="7fc21-119">A K1 (alapvető kicsi), K2 (alapvető közepes), és B3 (Basic nagy).</span><span class="sxs-lookup"><span data-stu-id="7fc21-119">B1 (Basic Small), B2 (Basic Medium), and B3 (Basic Large).</span></span> <span data-ttu-id="7fc21-120">S1 (szabványos kicsi), S2 (szabványos közepes), és S3 (szabványos nagy).</span><span class="sxs-lookup"><span data-stu-id="7fc21-120">S1 (Standard Small), S2 (Standard Medium), and S3 (Standard Large).</span></span> <span data-ttu-id="7fc21-121">P1 (prémium szintű kicsi), P2 (prémium szintű közepes), és P3 (prémium nagy).)</span><span class="sxs-lookup"><span data-stu-id="7fc21-121">P1 (Premium Small), P2 (Premium Medium), and P3 (Premium Large).)</span></span>
* <span data-ttu-id="7fc21-122">**--példányok**: hello munkavállalók száma az hello app service-csomag (alapértelmezett értéke 1).</span><span class="sxs-lookup"><span data-stu-id="7fc21-122">**--instances**: hello number of workers in hello app service plan (Default value is 1).</span></span>

<span data-ttu-id="7fc21-123">Példa toouse ezt a parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="7fc21-123">Example toouse this cmdlet:</span></span>

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="7fc21-124">A Linux App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fc21-124">Create a Linux App Service Plan</span></span>

<span data-ttu-id="7fc21-125">Hello segítségével azonos **azure appserviceplan létrehozása** parancsot, hello kiegészítő paraméterrel **--islinux true**.</span><span class="sxs-lookup"><span data-stu-id="7fc21-125">Using hello same **azure appserviceplan create** command, with hello extra parameter **--islinux true**.</span></span> <span data-ttu-id="7fc21-126">Vegye figyelembe a hello korlátozások és a leírt régiók [bemutatása tooApp szolgáltatás Linux rendszeren](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="7fc21-126">Note hello restrictions and regions outlined in [Introduction tooApp Service on Linux](app-service-linux-intro.md)</span></span>

### <a name="list-existing-app-service-plans"></a><span data-ttu-id="7fc21-127">Lista meglévő App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="7fc21-127">List Existing App Service Plans</span></span>
<span data-ttu-id="7fc21-128">toolist hello meglévő app service-csomagokról, használjon **azure appserviceplan lista** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-128">toolist hello existing app service plans, use **azure appserviceplan list** command.</span></span>

<span data-ttu-id="7fc21-129">toolist egy adott erőforráscsoportba tartozó összes app service csomagokban használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-129">toolist all app service plans under a specific resource group, use:</span></span>

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="7fc21-130">egy adott app service-csomag tooget használja **azure appserviceplan megjelenítése** parancs:</span><span class="sxs-lookup"><span data-stu-id="7fc21-130">tooget a specific app service plan, use **azure appserviceplan show** command:</span></span>

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a><span data-ttu-id="7fc21-131">Egy meglévő App Service-csomag konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7fc21-131">Configure an existing App Service Plan</span></span>
<span data-ttu-id="7fc21-132">egy meglévő app service-csomag toochange hello beállításait használja hello **azure appserviceplan config** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-132">toochange hello settings for an existing app service plan, use hello **azure appserviceplan config** command.</span></span> <span data-ttu-id="7fc21-133">Módosíthatja a hello sku, és a dolgozók hello száma</span><span class="sxs-lookup"><span data-stu-id="7fc21-133">You can change hello sku, and hello number of workers</span></span> 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a><span data-ttu-id="7fc21-134">Az App Service-csomag skálázás</span><span class="sxs-lookup"><span data-stu-id="7fc21-134">Scaling an App Service Plan</span></span>
<span data-ttu-id="7fc21-135">egy meglévő App Service-csomag, tooscale használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-135">tooscale an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-hello-sku-of-an-app-service-plan"></a><span data-ttu-id="7fc21-136">Hello Termékváltozata egy App Service-csomag módosítása</span><span class="sxs-lookup"><span data-stu-id="7fc21-136">Changing hello SKU of an App Service Plan</span></span>
<span data-ttu-id="7fc21-137">toochange hello termékváltozata egy meglévő App Service-csomag, használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-137">toochange hello sku of an existing App Service Plan, use:</span></span>

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a><span data-ttu-id="7fc21-138">Egy meglévő App Service-csomag törlése</span><span class="sxs-lookup"><span data-stu-id="7fc21-138">Delete an existing App Service Plan</span></span>
<span data-ttu-id="7fc21-139">egy meglévő app service-csomag toodelete, az összes hozzárendelt alkalmazásokat kell toobe törölte vagy helyezte át először.</span><span class="sxs-lookup"><span data-stu-id="7fc21-139">toodelete an existing app service plan, all assigned apps need toobe moved or deleted first.</span></span> <span data-ttu-id="7fc21-140">Hello használata **azure webalkalmazás törlése** hello app service-csomag törlése paranccsal.</span><span class="sxs-lookup"><span data-stu-id="7fc21-140">Then using hello **azure webapp delete** command you can delete hello app service plan.</span></span>

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a><span data-ttu-id="7fc21-141">Az App Service-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="7fc21-141">Managing App Service apps</span></span>
### <a name="create-a-web-app"></a><span data-ttu-id="7fc21-142">Webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fc21-142">Create a web app</span></span>
<span data-ttu-id="7fc21-143">toocreate egy webalkalmazást, használja a hello **azure webalkalmazás létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-143">toocreate a web app, use hello **azure webapp create** command.</span></span>

<span data-ttu-id="7fc21-144">Alább olvasható hello más paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="7fc21-144">Following are descriptions of hello different parameters:</span></span>

* <span data-ttu-id="7fc21-145">**--name**: hello webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="7fc21-145">**--name**: name for hello web app.</span></span>
* <span data-ttu-id="7fc21-146">**--terv**: hello service-csomag használt toohost hello webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="7fc21-146">**--plan**: name for hello service plan used toohost hello web app.</span></span>
* <span data-ttu-id="7fc21-147">**--Erőforráscsoport**: erőforráscsoport, amelyen hello App service-csomag.</span><span class="sxs-lookup"><span data-stu-id="7fc21-147">**--resource-group**: resource group that hosts hello App service plan.</span></span>
* <span data-ttu-id="7fc21-148">**--hely**: hello webes alkalmazás helyét.</span><span class="sxs-lookup"><span data-stu-id="7fc21-148">**--location**: hello web app location.</span></span>

<span data-ttu-id="7fc21-149">Példa toouse ezt a parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="7fc21-149">Example toouse this cmdlet:</span></span>

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a><span data-ttu-id="7fc21-150">Egy meglévő alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="7fc21-150">Delete an existing app</span></span>
<span data-ttu-id="7fc21-151">egy meglévő alkalmazást toodelete, hello használható **azure webalkalmazás törlése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-151">toodelete an existing app, you can use hello **azure webapp delete** command.</span></span> <span data-ttu-id="7fc21-152">Hello alkalmazás toospecify hello nevét és hello erőforráscsoport-név szükséges.</span><span class="sxs-lookup"><span data-stu-id="7fc21-152">You need toospecify hello name of hello app and hello resource group name.</span></span>

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a><span data-ttu-id="7fc21-153">Meglévő alkalmazások felsorolása</span><span class="sxs-lookup"><span data-stu-id="7fc21-153">List existing apps</span></span>
<span data-ttu-id="7fc21-154">toolist hello meglévő alkalmazások esetében használja hello **azure webalkalmazás lista** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-154">toolist hello existing apps, use hello **azure webapp list** command.</span></span>

<span data-ttu-id="7fc21-155">toolist egy adott erőforráscsoportban található összes alkalmazást használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-155">toolist all apps in a specific resource group, use:</span></span>

    azure webapp list --resource-group ContosoAzureResourceGroup

<span data-ttu-id="7fc21-156">egy adott alkalmazást tooget hello használata **azure webalkalmazás megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-156">tooget a specific app, use hello **azure webapp show** command.</span></span>

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a><span data-ttu-id="7fc21-157">Meglévő alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7fc21-157">Configure an existing app</span></span>
<span data-ttu-id="7fc21-158">toochange hello beállítás és konfiguráció egy meglévő alkalmazás esetén használjon hello **azure webalkalmazás konfiguráció** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-158">toochange hello settings and configurations for an existing app, use hello **azure webapp config set** command.</span></span>

<span data-ttu-id="7fc21-159">(1). példa: hello php verzióját egy alkalmazás megváltoztatása</span><span class="sxs-lookup"><span data-stu-id="7fc21-159">Example (1): change hello php version of a app</span></span> 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

<span data-ttu-id="7fc21-160">(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="7fc21-160">Example (2): add or change app settings</span></span>

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

<span data-ttu-id="7fc21-161">milyen más konfigurációs megváltoztatására, tooknow használata hello **azure webalkalmazás konfigurációs nulla -h** parancsot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-161">tooknow what other configuration can be changed, use hello **azure webapp config set -h** command.</span></span>

### <a name="change-hello-state-of-an-existing-app"></a><span data-ttu-id="7fc21-162">Egy meglévő alkalmazást hello állapotának módosítása</span><span class="sxs-lookup"><span data-stu-id="7fc21-162">Change hello state of an existing app</span></span>
#### <a name="restart-an-app"></a><span data-ttu-id="7fc21-163">Indítsa újra az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="7fc21-163">Restart an app</span></span>
<span data-ttu-id="7fc21-164">az alkalmazás toorestart, meg kell adnia hello nevét és az erőforrás csoport hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7fc21-164">toorestart an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a><span data-ttu-id="7fc21-165">Állítsa le az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="7fc21-165">Stop an app</span></span>
<span data-ttu-id="7fc21-166">az alkalmazás toostop, meg kell adnia hello nevét és az erőforrás csoport hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7fc21-166">toostop an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a><span data-ttu-id="7fc21-167">Indítsa el az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="7fc21-167">Start an app</span></span>
<span data-ttu-id="7fc21-168">az alkalmazás toostart, meg kell adnia hello nevét és az erőforrás csoport hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7fc21-168">toostart an app, you must specify hello name and resource group of hello app.</span></span>

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a><span data-ttu-id="7fc21-169">Egy alkalmazás közzétételi profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="7fc21-169">Manage publishing profiles of an app</span></span>
<span data-ttu-id="7fc21-170">Minden alkalmazás rendelkezik egy közzétételi profilt, amely használt toopublish a kódot.</span><span class="sxs-lookup"><span data-stu-id="7fc21-170">Each app has a publishing profile that can be used toopublish your code.</span></span>

#### <a name="get-publishing-profile"></a><span data-ttu-id="7fc21-171">Kérhető le a közzétételi profil</span><span class="sxs-lookup"><span data-stu-id="7fc21-171">Get Publishing Profile</span></span>
<span data-ttu-id="7fc21-172">tooget hello közzétételi profil alkalmazások esetén használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-172">tooget hello publishing profile for a app, use:</span></span>

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

<span data-ttu-id="7fc21-173">Ez a parancs a közzétételi profil felhasználónév és jelszó toohello parancssori hello echók.</span><span class="sxs-lookup"><span data-stu-id="7fc21-173">This command echoes hello publishing profile username and password toohello command line.</span></span>

### <a name="manage-app-hostnames"></a><span data-ttu-id="7fc21-174">Alkalmazás állomásnevek kezelése</span><span class="sxs-lookup"><span data-stu-id="7fc21-174">Manage app hostnames</span></span>
<span data-ttu-id="7fc21-175">az alkalmazás toomanage állomásnévkötéseket hello használata **azure webalkalmazás config állomásnevek** parancs</span><span class="sxs-lookup"><span data-stu-id="7fc21-175">toomanage hostname bindings for your app, use hello **azure webapp config hostnames** command</span></span>  

#### <a name="list-hostname-bindings"></a><span data-ttu-id="7fc21-176">Lista állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="7fc21-176">List hostname bindings</span></span>
<span data-ttu-id="7fc21-177">tooget hello aktuális állomásnévkötéseket egy alkalmazás esetén használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-177">tooget hello current hostname bindings for an app, use:</span></span>

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a><span data-ttu-id="7fc21-178">Adja hozzá az állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="7fc21-178">Add hostname bindings</span></span>
<span data-ttu-id="7fc21-179">tooadd állomásnév kötések tooan alkalmazást, és használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-179">tooadd hostname bindings tooan app, use:</span></span>

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a><span data-ttu-id="7fc21-180">Törölje az állomásnévkötéseket</span><span class="sxs-lookup"><span data-stu-id="7fc21-180">Delete hostname bindings</span></span>
<span data-ttu-id="7fc21-181">toodelete állomásnévkötéseket, használja:</span><span class="sxs-lookup"><span data-stu-id="7fc21-181">toodelete hostname bindings, use:</span></span>

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a><span data-ttu-id="7fc21-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7fc21-182">Next Steps</span></span>
* <span data-ttu-id="7fc21-183">Azure Resource Manager CLI-támogatással kapcsolatos toolearn lásd: [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="7fc21-183">toolearn about Azure Resource Manager CLI support, see [Use hello Azure CLI toomanage Azure resources and resource groups.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)</span></span>
* <span data-ttu-id="7fc21-184">App Service powershellel, kezelésével kapcsolatos toolearn lásd: [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="7fc21-184">toolearn about managing App Service using PowerShell, see [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)</span></span>
* <span data-ttu-id="7fc21-185">toolearn: Azure App Service Linux, lásd: [bemutatása tooApp szolgáltatás Linux rendszeren](app-service-linux-intro.md)</span><span class="sxs-lookup"><span data-stu-id="7fc21-185">toolearn about Azure App Service on Linux, see [Introduction tooApp Service on Linux](app-service-linux-intro.md)</span></span>
