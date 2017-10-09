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
# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-app-service"></a>Azure App Service az Azure Resource Manager-alapú XPlat parancssori felület használatával
> [!div class="op_single_selector"]
> * [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
> * [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

A Microsoft Azure platformfüggetlen parancssori eszközök verzió 0.10.5 hello kiadással új parancsok váltak elérhetővé. Ezek a parancsok hello felhasználói hello képességét toouse Azure Resource Manager-alapú PowerShell parancsok toomanage App Service biztosítják.

toolearn erőforráscsoportok, kezelésével kapcsolatban lásd: [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások](../azure-resource-manager/xplat-cli-azure-resource-manager.md). 

> [!NOTE] 
> Emellett kipróbálásához [Azure CLI 2.0](https://github.com/Azure/azure-cli), a következő generációs CLI hello erőforrás felügyeleti telepítési modell pythonban írt.
>
>

## <a name="managing-app-service-plans"></a>Kezelés az App Service-csomagok
### <a name="create-an-app-service-plan"></a>Az App Service-csomag létrehozása
az app service-csomag toocreate hello használata **azure appserviceplan létrehozása** parancsot.

Alább olvasható hello más paraméterekkel:

* **--Erőforráscsoport**: erőforráscsoport, amely tartalmazza az újonnan létrehozott hello app service-csomag.
* **--name**: hello app service-csomag neve.
* **--hely**: app service-csomag hely.
* **--sku**: hello szükséges árképzési termékváltozat (hello lehetőségek: F1 (ingyenes). A D1 (megosztott). A K1 (alapvető kicsi), K2 (alapvető közepes), és B3 (Basic nagy). S1 (szabványos kicsi), S2 (szabványos közepes), és S3 (szabványos nagy). P1 (prémium szintű kicsi), P2 (prémium szintű közepes), és P3 (prémium nagy).)
* **--példányok**: hello munkavállalók száma az hello app service-csomag (alapértelmezett értéke 1).

Példa toouse ezt a parancsmagot:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

#### <a name="create-a-linux-app-service-plan"></a>A Linux App Service-csomag létrehozása

Hello segítségével azonos **azure appserviceplan létrehozása** parancsot, hello kiegészítő paraméterrel **--islinux true**. Vegye figyelembe a hello korlátozások és a leírt régiók [bemutatása tooApp szolgáltatás Linux rendszeren](app-service-linux-intro.md)

### <a name="list-existing-app-service-plans"></a>Lista meglévő App Service-csomagok
toolist hello meglévő app service-csomagokról, használjon **azure appserviceplan lista** parancsot.

toolist egy adott erőforráscsoportba tartozó összes app service csomagokban használja:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

egy adott app service-csomag tooget használja **azure appserviceplan megjelenítése** parancs:

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag konfigurálása
egy meglévő app service-csomag toochange hello beállításait használja hello **azure appserviceplan config** parancsot. Módosíthatja a hello sku, és a dolgozók hello száma 

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Az App Service-csomag skálázás
egy meglévő App Service-csomag, tooscale használja:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-hello-sku-of-an-app-service-plan"></a>Hello Termékváltozata egy App Service-csomag módosítása
toochange hello termékváltozata egy meglévő App Service-csomag, használja:

    azure appserviceplan config --name ContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Egy meglévő App Service-csomag törlése
egy meglévő app service-csomag toodelete, az összes hozzárendelt alkalmazásokat kell toobe törölte vagy helyezte át először. Hello használata **azure webalkalmazás törlése** hello app service-csomag törlése paranccsal.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-apps"></a>Az App Service-alkalmazások kezelése
### <a name="create-a-web-app"></a>Webalkalmazás létrehozása
toocreate egy webalkalmazást, használja a hello **azure webalkalmazás létrehozása** parancsot.

Alább olvasható hello más paraméterekkel:

* **--name**: hello webalkalmazás nevét.
* **--terv**: hello service-csomag használt toohost hello webes alkalmazás neve.
* **--Erőforráscsoport**: erőforráscsoport, amelyen hello App service-csomag.
* **--hely**: hello webes alkalmazás helyét.

Példa toouse ezt a parancsmagot:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-app"></a>Egy meglévő alkalmazás törlése
egy meglévő alkalmazást toodelete, hello használható **azure webalkalmazás törlése** parancsot. Hello alkalmazás toospecify hello nevét és hello erőforráscsoport-név szükséges.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-apps"></a>Meglévő alkalmazások felsorolása
toolist hello meglévő alkalmazások esetében használja hello **azure webalkalmazás lista** parancsot.

toolist egy adott erőforráscsoportban található összes alkalmazást használja:

    azure webapp list --resource-group ContosoAzureResourceGroup

egy adott alkalmazást tooget hello használata **azure webalkalmazás megjelenítése** parancsot.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-app"></a>Meglévő alkalmazás konfigurálása
toochange hello beállítás és konfiguráció egy meglévő alkalmazás esetén használjon hello **azure webalkalmazás konfiguráció** parancsot.

(1). példa: hello php verzióját egy alkalmazás megváltoztatása 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

(2). példa: hozzáadása vagy alkalmazás beállításainak módosítása

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

milyen más konfigurációs megváltoztatására, tooknow használata hello **azure webalkalmazás konfigurációs nulla -h** parancsot.

### <a name="change-hello-state-of-an-existing-app"></a>Egy meglévő alkalmazást hello állapotának módosítása
#### <a name="restart-an-app"></a>Indítsa újra az alkalmazást
az alkalmazás toorestart, meg kell adnia hello nevét és az erőforrás csoport hello alkalmazás.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-an-app"></a>Állítsa le az alkalmazást
az alkalmazás toostop, meg kell adnia hello nevét és az erőforrás csoport hello alkalmazás.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-an-app"></a>Indítsa el az alkalmazást
az alkalmazás toostart, meg kell adnia hello nevét és az erőforrás csoport hello alkalmazás.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-publishing-profiles-of-an-app"></a>Egy alkalmazás közzétételi profilok kezelése
Minden alkalmazás rendelkezik egy közzétételi profilt, amely használt toopublish a kódot.

#### <a name="get-publishing-profile"></a>Kérhető le a közzétételi profil
tooget hello közzétételi profil alkalmazások esetén használja:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Ez a parancs a közzétételi profil felhasználónév és jelszó toohello parancssori hello echók.

### <a name="manage-app-hostnames"></a>Alkalmazás állomásnevek kezelése
az alkalmazás toomanage állomásnévkötéseket hello használata **azure webalkalmazás config állomásnevek** parancs  

#### <a name="list-hostname-bindings"></a>Lista állomásnévkötéseket
tooget hello aktuális állomásnévkötéseket egy alkalmazás esetén használja:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Adja hozzá az állomásnévkötéseket
tooadd állomásnév kötések tooan alkalmazást, és használja:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Törölje az állomásnévkötéseket
toodelete állomásnévkötéseket, használja:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

## <a name="next-steps"></a>Következő lépések
* Azure Resource Manager CLI-támogatással kapcsolatos toolearn lásd: [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások.](../azure-resource-manager/xplat-cli-azure-resource-manager.md)
* App Service powershellel, kezelésével kapcsolatos toolearn lásd: [Using Azure Resource Manager-Based PowerShell tooManage Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
* toolearn: Azure App Service Linux, lásd: [bemutatása tooApp szolgáltatás Linux rendszeren](app-service-linux-intro.md)
