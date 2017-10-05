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
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Azure App Service az Azure Resource Manager-alapú XPlat parancssori felület használatával
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

A Microsoft Azure platformfüggetlen parancssori eszközök verzió 0.10.5 a kiadástól kezdve új parancsok váltak elérhetővé. Ezek a parancsok a felhasználónak, Azure Resource Manager-alapú PowerShell-parancsokat használhatja az App Service kezeléséhez.

Erőforráscsoportok kezelésével kapcsolatos információkért lásd: [Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Emellett kipróbálásához [Azure CLI 2.0](https://github.com/Azure/azure-cli), a következő generációs CLI erőforrás felügyeleti telepítési modell pythonban írt.
>
>

## <a name="managing-app-service-plans"></a>Kezelés az App Service-csomagok
### <a name="create-an-app-service-plan"></a>Az App Service-csomag létrehozása
Az app service-csomag létrehozásához használja a **azure appserviceplan létrehozása** parancsot.

A különböző paraméterek leírását a következők:

* **--Erőforráscsoport**: erőforráscsoport, amely tartalmazza az újonnan létrehozott app service-csomagot.
* **--name**: az app service-csomag neve.
* **--hely**: app service-csomag hely.
* **--sku**: a kívánt tarifacsomagot termékváltozat (a lehetőségek a következők: F1 (ingyenes). A D1 (megosztott). A K1 (alapvető kicsi), K2 (alapvető közepes), és B3 (Basic nagy). S1 (szabványos kicsi), S2 (szabványos közepes), és S3 (szabványos nagy). P1 (prémium szintű kicsi), P2 (prémium szintű közepes), és P3 (prémium nagy).)
* **--példányok**: szolgáltatási terv (alapértelmezett értéke 1) az alkalmazásban munkavállalók száma.

Például, hogy használja a következő parancsmagot:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>A Linux App Service-csomag létrehozása

Azonos **azure appserviceplan létrehozása** parancsot, a kiegészítő paraméterrel **--islinux true**. Jegyezze fel a korlátozások és a leírt régiók [App Service Linux bemutatása](app-service-linux-intro.md)

### <a name="list-existing-app-service-plans"></a>Lista meglévő App Service-csomagok
A meglévő app service-csomagokról kilistázhatja **azure appserviceplan lista** parancsot.

Egy adott erőforráscsoportba tartozó összes app service csomagokban listájában, használja:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Egy adott app service-csomag használatához **azure appserviceplan megjelenítése** parancs:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag konfigurálása
Egy meglévő app service-csomag beállításainak módosításához használja a **azure appserviceplan config** parancsot. Módosíthatja a termékváltozat, és a dolgozók száma 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Az App Service-csomag skálázás
Egy meglévő App Service-csomag méretezéséhez használja:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Az App Service-csomag Termékváltozata módosítása
Egy meglévő App Service-csomag termékváltozata módosításához használja:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag törlése
Egy meglévő app service-csomag törlése, minden hozzárendelt alkalmazás kell helyezni vagy először törölni. Ezután használja a **azure webalkalmazás törlése** parancs segítségével törölheti az app service-csomag.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Az App Service-alkalmazások kezelése
### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
A webes alkalmazás létrehozásához használja a **azure webalkalmazás létrehozása** parancsot.

A különböző paraméterek leírását a következők:

* **--name**: a webalkalmazás nevét.
* **--terv**: a webalkalmazás üzemeltetéséhez használt service-csomag neve.
* **--Erőforráscsoport**: erőforráscsoport, amelyen az App service-csomag.
* **--hely**: a webes alkalmazás helyét.

Például, hogy használja a következő parancsmagot:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Egy meglévő alkalmazás törlése
Meglévő alkalmazás törléséhez használja a **azure webalkalmazás törlése** parancsot. Meg kell adnia az alkalmazás nevét és az erőforráscsoport neve.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Meglévő alkalmazások felsorolása
A meglévő alkalmazások listájában, használja a **azure webalkalmazás lista** parancsot.

Egy adott erőforráscsoportban található összes alkalmazások listájában, használja:

    azure webapp list --resource-group ContosoAzureResourceGroup

Ahhoz, hogy egy adott alkalmazást, használja a **azure webalkalmazás megjelenítése** parancsot.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Meglévő alkalmazás konfigurálása
A beállítások és a meglévő alkalmazás konfiguráció módosításához használja a **azure webalkalmazás konfiguráció** parancsot.

(1). példa: a php verzióját egy alkalmazás megváltoztatása 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Tudni, hogy milyen más konfigurációs lehet módosítani, használja a **azure webalkalmazás konfigurációs nulla -h** parancsot.

### <a name="change-the-state-of-an-existing-app"></a>A meglévő alkalmazás állapotának módosítása
#### <a name="restart-an-app"></a>Indítsa újra az alkalmazást
Indítsa újra az alkalmazást, meg kell adnia az alkalmazás nevét és az erőforrás csoportja.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Állítsa le az alkalmazást
Állítsa le az alkalmazást, meg kell adnia az alkalmazás nevét és az erőforrás csoportja.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Indítsa el az alkalmazást
Indítsa el az alkalmazást, meg kell adnia az alkalmazás nevét és az erőforrás csoportja.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Egy alkalmazás közzétételi profilok kezelése
Minden alkalmazás rendelkezik egy közzétételi profilt, amely segítségével a kód közzététele.

#### <a name="get-publishing-profile"></a>Kérhető le a közzétételi profil
A közzétételi profil alkalmazások használatához:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Ez a parancs echók a közzétételi profil felhasználónevet és jelszót a parancssorba.

### <a name="manage-app-hostnames"></a>Alkalmazás állomásnevek kezelése
Az alkalmazás állomásnévkötéseket kezeléséhez használja a **azure webalkalmazás config állomásnevek** parancs  

#### <a name="list-hostname-bindings"></a>Lista állomásnévkötéseket
Az alkalmazás aktuális állomásnévkötéseket használatához:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Adja hozzá az állomásnévkötéseket
Állomásnévkötéseket alkalmazás hozzáadásához használja:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Törölje az állomásnévkötéseket
Állomásnévkötéseket törléséhez használja:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Következő lépések
* Azure Resource Manager CLI-támogatással kapcsolatos információkért lásd: [Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)
* App Service PowerShell-lel kezelésével kapcsolatos információkért lásd: [Using Azure Resource Manager-Based PowerShell kezelése Azure Web Apps használatával.](app-service-web-app-azure-resource-manager-powershell.md)
* Azure App Service Linux rendszeren, lásd: [App Service Linux bemutatása](app-service-linux-intro.md)
