---
title: "aaaAzure Resource Manager-alapú PowerShell-parancsok Azure webalkalmazás számára |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello új Azure Resource Manager-alapú PowerShell-parancsok toomanage az Azure Web Apps."
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
ms.openlocfilehash: bbb821e89daa315280436e84e11316217bb644d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-based-powershell-toomanage-azure-web-apps"></a>Azure Resource Manager-Based PowerShell tooManage Azure Web Apps használatával
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
> 
> 

A Microsoft Azure PowerShell verzió 1.0.0 bővült, hello felhasználói hello képességét toouse Azure Resource Manager-alapú PowerShell parancsok toomanage Web Apps biztosítják, hogy új parancsok.

toolearn erőforráscsoportok, kezelésével kapcsolatban lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md). 

toolearn paraméterek és beállítások hello PowerShell-parancsmagok teljes listája hello kapcsolatban lásd: hello [parancsmag-referencia a webes alkalmazás Azure Resource Manager-alapú PowerShell-parancsmagok teljes](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Kezelés az App Service-csomagok
### <a name="create-an-app-service-plan"></a>Az App Service-csomag létrehozása
az app service-csomag toocreate hello használata **New-AzureRmAppServicePlan** parancsmag.

Alább olvasható hello más paraméterekkel:

* **Név**: hello app service-csomag neve.
* **Hely**: csomag hely szolgáltatás.
* **ResourceGroupName**: erőforráscsoport, amely tartalmazza az újonnan létrehozott hello app service-csomag.
* **Réteg**: hello szükséges tarifacsomag (alapértelmezett ingyenes, a közös, Basic, Standard és Premium is.)
* **WorkerSize**: hello mérete munkavállalók (az alapértelmezett érték kisebb, ha hello réteg paraméter van megadva, a Basic, Standard vagy prémium. Is, közepes és nagy.)
* **NumberofWorkers**: hello munkavállalók száma az hello app service-csomag (alapértelmezett értéke 1). 

Példa toouse ezt a parancsmagot:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Az App Service-csomag létrehozása az App Service-környezet
toocreate egy app service-csomag egy app service environment-környezetben, használja az azonos hello parancs **New-AzureRmAppServicePlan** parancsot kiegészítő paraméterekkel toospecify hello ASE tartozó név és ASE tartozó erőforráscsoport-név.

Példa toouse ezt a parancsmagot:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

További információk az app service Environment-környezet, ellenőrzés toolearn [bemutatása tooApp Service-környezet](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Lista meglévő App Service-csomagok
toolist hello meglévő app service-csomagokról, használjon **Get-AzureRmAppServicePlan** parancsmag.

toolist az előfizetéshez tartozó összes app service csomagokban használja: 

    Get-AzureRmAppServicePlan

toolist egy adott erőforráscsoportba tartozó összes app service csomagokban használja:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

egy adott app service-csomag tooget használja:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag konfigurálása
egy meglévő app service-csomag toochange hello beállításait használja hello **Set-AzureRmAppServicePlan** parancsmag. Módosíthatja a hello réteg, a munkavégző méretének és a dolgozók hello száma 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Az App Service-csomag skálázás
egy meglévő App Service-csomag, tooscale használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-hello-worker-size-of-an-app-service-plan"></a>Az App Service-csomag hello munkavégző méretének módosítása
egy meglévő App Service-csomag, használja a munkavállalók toochange hello mérete:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-hello-tier-of-an-app-service-plan"></a>Hello egy App Service-csomag szintjének módosítása
toochange hello szintjének egy meglévő App Service-csomag, használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag törlése
egy meglévő app service-csomag toodelete, az összes hozzárendelt web apps kell toobe törölte vagy helyezte át először. Hello használata **Remove-AzureRmAppServicePlan** parancsmag hello app service-csomag törlése.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Az App Service Web Apps alkalmazások kezelése
### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
toocreate egy webalkalmazást, használja a hello **New-AzureRmWebApp** parancsmag.

Alább olvasható hello más paraméterekkel:

* **Név**: hello webalkalmazás nevét.
* **AppServicePlan**: hello service-csomag használt toohost hello webes alkalmazás neve.
* **ResourceGroupName**: erőforráscsoport, amelyen hello App service-csomag.
* **Hely**: hello webes alkalmazás helyét.

Példa toouse ezt a parancsmagot:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>A webalkalmazás létrehozása az App Service-környezet
toocreate egy webalkalmazást az App Service környezetben (ASE). Használja az azonos hello **New-AzureRmWebApp** kiegészítő paraméterekkel toospecify hello ASE nevét és hello az erőforráscsoport neve, amely hello ASE tartozik parancsot.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

További információk az app service Environment-környezet, ellenőrzés toolearn [bemutatása tooApp Service-környezet](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Egy már meglévő webalkalmazás törlése
egy létező webalkalmazása, használhatja a hello toodelete **Remove-AzureRmWebApp** parancsmag, toospecify hello neve hello web app és hello az erőforráscsoport neve van szükség.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Meglévő Web Apps felsorolása
toolist hello meglévő, használó webalkalmazások hello **Get-AzureRmWebApp** parancsmag.

toolist az előfizetéshez tartozó összes webalkalmazások használja:

    Get-AzureRmWebApp

toolist egy adott erőforráscsoportba tartozó összes webalkalmazások használja:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

egy adott webalkalmazás tooget használja:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Egy már meglévő webalkalmazás konfigurálása
toochange hello beállítás és konfiguráció egy már meglévő webalkalmazás használja hello **Set-AzureRmWebApp** parancsmag. A paraméterek teljes listájáért tekintse meg hello [parancsmag-hivatkozás](https://msdn.microsoft.com/library/mt652487.aspx)

(1). példa: Ez a parancsmag toochange kapcsolati karakterláncok használata

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


(3). példa: készlet hello web app toorun 64 bites üzemmódban

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-hello-state-of-an-existing-web-app"></a>Egy már meglévő webalkalmazás hello állapotának módosítása
#### <a name="restart-a-web-app"></a>A webalkalmazás újraindítása
a webes alkalmazás toorestart, meg kell adnia hello nevét és az erőforrás csoport hello webalkalmazás.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>A webalkalmazás leállítása
a webes alkalmazás toostop, meg kell adnia hello nevét és az erőforrás csoport hello webalkalmazás.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>A webalkalmazás elindítása
a webes alkalmazás toostart, meg kell adnia hello nevét és az erőforrás csoport hello webalkalmazás.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Webes alkalmazás közzétételi profilok kezelése
Minden webalkalmazás a közzétételi profilt, amely lehet használt toopublish az alkalmazások, a több művelet profilok közzététele hajtható végre.

#### <a name="get-publishing-profile"></a>Kérhető le a közzétételi profil
Közzétételi profil egy webalkalmazás, használja a tooget hello:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Ez a parancs echók hello közzétételi profil toohello parancssor, valamint a kimeneti hello közzétételi profil tooa szövegfájl.

#### <a name="reset-publishing-profile"></a>Közzétételi profil alaphelyzetbe állítása
tooreset mindkét hello közzétételi jelszót az FTP és a webes központi telepítése a webes alkalmazások esetén használja:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Webes alkalmazás tanúsítványok kezelése
Hogyan toomanage web app tanúsítványok kapcsolatos toolearn lásd: [SSL-tanúsítványok kötése PowerShell használatával](app-service-web-app-powershell-ssl-binding.md)

### <a name="next-steps"></a>Következő lépések
* Azure Resource Manager PowerShell-támogatással kapcsolatos toolearn lásd [Azure PowerShell használata az Azure Resource Manager eszközzel.](../powershell-azure-resource-manager.md)
* App Service-környezetek kapcsolatos toolearn lásd [bemutatása tooApp Service-környezet.](app-service-app-service-environment-intro.md)
* App Service SSL-tanúsítványok PowerShell, történő kezelésével kapcsolatos toolearn lásd [SSL-tanúsítványok kötése PowerShell használatával.](app-service-web-app-powershell-ssl-binding.md)
* toolearn kapcsolatos hello Azure Web Apps, Azure Resource Manager-alapú PowerShell-parancsmagok teljes listáját lásd: [parancsmag-referencia Azure Web Apps Azure Resource Manager PowerShell-parancsmagokat.](https://msdn.microsoft.com/library/mt619237.aspx)
* * App Service CLI-vel, kezelésével kapcsolatos toolearn lásd: [Using Azure Resource Manager-Based XPlat parancssori Felületet Azure webalkalmazás számára.](app-service-web-app-azure-resource-manager-xplat-cli.md)

