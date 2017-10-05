---
title: "Az Azure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti az Azure Web alkalmazásokat az új Azure Resource Manager-alapú PowerShell-parancsok segítségével."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 4231fbba-61e5-4f92-b958-531c066fb87f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: aelnably
ms.openlocfilehash: 8d574f051a327ba0409e6f25a5886af673d3d5e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Az Azure Web Apps alkalmazások kezelése az Azure Resource Manager-alapú PowerShell segítségével
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

A Microsoft Azure PowerShell 1.0.0 verzió új parancsok váltak elérhetővé, hogy a felhasználónak, Azure Resource Manager-alapú PowerShell-parancsokat használhatja a webes alkalmazásokat kezeléséhez.

Erőforráscsoportok kezelésével kapcsolatos információkért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md). 

Paraméterek és beállítások megadása a PowerShell-parancsmagok teljes listája, lásd: a [parancsmag-referencia a webes alkalmazás Azure Resource Manager-alapú PowerShell-parancsmagok teljes](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Kezelés az App Service-csomagok
### <a name="create-an-app-service-plan"></a>Az App Service-csomag létrehozása
Az app service-csomag létrehozásához használja a **New-AzureRmAppServicePlan** parancsmag.

A különböző paraméterek leírását a következők:

* **Név**: az app service-csomag neve.
* **Hely**: csomag hely szolgáltatás.
* **ResourceGroupName**: erőforráscsoport, amely tartalmazza az újonnan létrehozott app service-csomagot.
* **Réteg**: a kívánt tarifacsomagot (alapértelmezett ingyenes, a közös, Basic, Standard és Premium is.)
* **WorkerSize**: (alapértelmezett érték kisebb, ha a réteg paraméter van megadva, a Basic, Standard vagy Premium munkavállalók mérete. Is, közepes és nagy.)
* **NumberofWorkers**: szolgáltatási terv (alapértelmezett értéke 1) az alkalmazásban munkavállalók száma. 

Például, hogy használja a következő parancsmagot:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Az App Service-csomag létrehozása az App Service-környezet
Az app service environment-környezetben az app service-csomag létrehozásához használja a ugyanazzal a paranccsal **New-AzureRmAppServicePlan** parancsot a további paramétereket adja meg a ASE nevét és ASE tartozó erőforráscsoport-név.

Például, hogy használja a következő parancsmagot:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

App service Environment-környezet kapcsolatos további információkért ellenőrizze a következőket [App Service Environment bemutatása](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Lista meglévő App Service-csomagok
A meglévő app service-csomagokról kilistázhatja **Get-AzureRmAppServicePlan** parancsmag.

Az előfizetéshez tartozó összes app service csomagokban listájában, használja: 

    Get-AzureRmAppServicePlan

Egy adott erőforráscsoportba tartozó összes app service csomagokban listájában, használja:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Egy adott app service-csomag beszerzéséhez használja:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag konfigurálása
Egy meglévő app service-csomag beállításainak módosításához használja a **Set-AzureRmAppServicePlan** parancsmag. Módosíthatja a réteg, a munkavégző mérete és a dolgozók száma 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Az App Service-csomag skálázás
Egy meglévő App Service-csomag méretezéséhez használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Az App Service-csomag munkavégző méretének módosítása
Egy meglévő App Service-csomag munkavállalók méretének módosításához használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Az App Service-csomag szintjének módosítása
Egy meglévő App Service-csomag szintjének módosításához használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag törlése
Egy meglévő app service-csomag törlése, hozzárendelt webalkalmazások áthelyezését vagy először törölni kell. Ezután használja a **Remove-AzureRmAppServicePlan** parancsmag az app service-csomag törlése.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Az App Service Web Apps alkalmazások kezelése
### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
A webes alkalmazás létrehozásához használja a **New-AzureRmWebApp** parancsmag.

A különböző paraméterek leírását a következők:

* **Név**: a webalkalmazás nevét.
* **AppServicePlan**: a webalkalmazás üzemeltetéséhez használt service-csomag neve.
* **ResourceGroupName**: erőforráscsoport, amelyen az App service-csomag.
* **Hely**: a webes alkalmazás helyét.

Például, hogy használja a következő parancsmagot:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>A webalkalmazás létrehozása az App Service-környezet
A webalkalmazás létrehozása az App Service környezetben (ASE). Ugyanazon **New-AzureRmWebApp** kiegészítő paraméterekkel rendelkező parancsot adja meg a ASE nevét és az erőforráscsoport neve, amely a ASE tartozik.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

App service Environment-környezet kapcsolatos további információkért ellenőrizze a következőket [App Service Environment bemutatása](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Egy már meglévő webalkalmazás törlése
Használhat meglévő webes alkalmazás törlése a **Remove-AzureRmWebApp** parancsmag, meg kell adni a webalkalmazás nevét és az erőforráscsoport neve.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Meglévő Web Apps felsorolása
A meglévő web Apps alkalmazások listájában, használja a **Get-AzureRmWebApp** parancsmag.

Az előfizetéshez tartozó összes webes alkalmazások listájában, használja:

    Get-AzureRmWebApp

Egy adott erőforráscsoportba tartozó összes webes alkalmazások listájában, használja:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Egy adott webalkalmazás, amelyet:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Egy már meglévő webalkalmazás konfigurálása
A beállítások és egy már meglévő webalkalmazás konfiguráció módosításához használja a **Set-AzureRmWebApp** parancsmag. A paraméterek teljes listájáért tekintse meg a [parancsmag-hivatkozás](https://msdn.microsoft.com/library/mt652487.aspx)

(1). példa: Ez a parancsmag segítségével módosíthatja a kapcsolati karakterláncok

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


(3). példa: a webes alkalmazás 64 bites módban való futásra beállítása

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Egy már meglévő webalkalmazás állapotának módosítása
#### <a name="restart-a-web-app"></a>A webalkalmazás újraindítása
A webalkalmazás újraindítása, meg kell adnia a webalkalmazás nevét és az erőforrás csoportja.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>A webalkalmazás leállítása
A webalkalmazás leállítása, meg kell adnia a webalkalmazás nevét és az erőforrás csoportja.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>A webalkalmazás elindítása
A webalkalmazás elindítása, meg kell adnia a webalkalmazás nevét és az erőforrás csoportja.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Webes alkalmazás közzétételi profilok kezelése
Minden webalkalmazás a közzétételi profilt, amely segítségével az alkalmazások közzététele, akkor több művelet profilok közzététele hajtható végre.

#### <a name="get-publishing-profile"></a>Kérhető le a közzétételi profil
A közzétételi profil egy webalkalmazás, amelyet:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Ez a parancs echók a közzétételi profilhoz, amellyel a parancssorból, valamint a kimeneti a közzétételi profil egy szövegfájlba.

#### <a name="reset-publishing-profile"></a>Közzétételi profil alaphelyzetbe állítása
FTP és a webes közzétételi jelszavát alaphelyzetbe állítása mindkét egy webalkalmazást, használja a központi telepítése:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Webes alkalmazás tanúsítványok kezelése
Webes alkalmazás tanúsítványok kezelése kapcsolatos további tudnivalókért lásd: [SSL-tanúsítványok kötése PowerShell használatával](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Következő lépések
* Azure Resource Manager PowerShell-támogatással kapcsolatos információkért lásd: [Azure PowerShell használata az Azure Resource Manager eszközzel.](../powershell-azure-resource-manager.md)
* App Service Environment-környezetek kapcsolatos további tudnivalókért lásd: [App Service Environment bemutatása.](app-service-app-service-environment-intro.md)
* App Service SSL-tanúsítványok PowerShell használatával történő kezelésével kapcsolatos információkért lásd: [SSL-tanúsítványok kötése PowerShell használatával.](app-service-web-app-powershell-ssl-binding.md)
* Az Azure Resource Manager-alapú PowerShell-parancsmagok teljes listája az Azure Web Apps, lásd: [parancsmag-referencia Azure Web Apps Azure Resource Manager PowerShell-parancsmagokat.](https://msdn.microsoft.com/library/mt619237.aspx)
* * App Service parancssori felület használatának kezelésével kapcsolatos információkért lásd: [Using Azure Resource Manager-Based XPlat parancssori Felületet Azure webalkalmazás számára.](app-service-web-app-azure-resource-manager-xplat-cli.md)

