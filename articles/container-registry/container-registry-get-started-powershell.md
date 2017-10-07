---
title: "aaaAzure tároló beállításjegyzék adattárak |} Microsoft Docs"
description: "Hogyan toouse Azure tároló beállításjegyzék tárolóhelyekkel Docker lemezképek"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.openlocfilehash: 448fb812f537c9502041ce5fb372b0681a9dac4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-powershell"></a>Hozza létre a titkos Docker tároló beállításkulcs hello Azure PowerShell használatával
Parancsok használata [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate egy tároló beállításjegyzék és a beállítások kezelése a Windows rendszerű számítógépen. Is hozhat létre, és hello segítségével kezeléséhez tároló [Azure-portálon](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), vagy programozott módon hello tároló beállításjegyzék [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Háttér és fogalmak: [hello áttekintése](container-registry-intro.md)
* Támogatott parancsmagok teljes listáját lásd: [Azure tároló beállításjegyzék parancsmagokat](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).


## <a name="prerequisites"></a>Előfeltételek
* **Az Azure PowerShell**: tooinstall és Ismerkedés az Azure PowerShell, lásd: hello [telepítési utasításokat](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). Jelentkezzen be Azure-előfizetés tooyour futtatásával `Login-AzureRMAccount`. További információkért lásd: [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).
* **Erőforráscsoport**: A tárolójegyzék létrehozása előtt hozzon létre egy [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md#resource-groups), vagy használjon egy meglévő erőforráscsoportot. Ellenőrizze, hogy hello erőforráscsoport van egy helyet, ahol hello tároló beállításszolgáltatás [elérhető](https://azure.microsoft.com/regions/services/). toocreate egy erőforráscsoportot az Azure PowerShell, lásd: [PowerShell-referencia hello](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).
* **A tárfiók** (nem kötelező): hozzon létre egy szabványos Azure [tárfiók](../storage/common/storage-introduction.md) tooback hello tároló hello rendszerleíró adatbázis ugyanazon a helyen. Ha nem adja meg a storage-fiók létrehozásakor a beállításjegyzéket `New-AzureRMContainerRegistry`, hello parancs létrehoz egyet. toocreate tárolási fiók PowerShell segítségével, lásd: [PowerShell-referencia hello](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount). A Premium Storage jelenleg nem támogatott.
* **Szolgáltatás egyszerű** (nem kötelező): a beállításjegyzék a PowerShell használatával való létrehozásakor alapértelmezés szerint azt nincs beállítva a hozzáférés. Igényeitől függően rendelhet egy meglévő Azure Active Directory szolgáltatás egyszerű tooa beállításjegyzék vagy létrehozása és hozzárendelése egy újat. Másik lehetőségként engedélyezheti a hello beállításjegyzék rendszergazda felhasználói fiók. A cikk későbbi részében hello szakaszban talál. Beállításjegyzék-hozzáféréssel kapcsolatos további információkért lásd: [hitelesítés hello tároló rendszerleíró](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Tároló-beállításjegyzék létrehozása
Futtassa a hello `New-AzureRMContainerRegistry` parancs toocreate tároló beállításjegyzékbeli.

> [!TIP]
> A beállításjegyzék létrehozásakor adjon meg egy globálisan egyedi legfelső szintű tartománynevet, amely csak betűket és számokat tartalmaz. hello beállításjegyzék neve hello példákban `MyRegistry`, de helyettesítse a saját, egyedi nevét.
>
>

a következő parancsot használja hello minimális paraméterek toocreate tároló beállításjegyzék hello `MyRegistry` hello erőforráscsoportban `MyResourceGroup` hello déli középső Régiójában helyen található:

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* A(z) `-StorageAccountName` nem kötelező. Ha nem megadott, a storage-fiók létrejön a neve, amely hello beállításjegyzék neve és hello időbélyegzőnek megadott erőforráscsoport.

## <a name="assign-a-service-principal"></a>Egyszerű szolgáltatás hozzárendelése
Használjon PowerShell-parancsok tooassign egy Azure Active Directory [egyszerű](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa beállításjegyzék. hello szolgáltatás egyszerű ezekben a példákban szerepét hello tulajdonos, de rendelhet [más szerepkörök](../active-directory/role-based-access-control-configure.md) irányíthatók.

### <a name="create-a-service-principal"></a>Egyszerű szolgáltatás létrehozása
A következő parancs hello egy új szolgáltatás egyszerű jön létre. Adjon meg egy erős jelszót a hello `-Password` paraméter.

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a>Rendelje hozzá egy új vagy meglévő egyszerű
Egy új vagy egy meglévő szolgáltatás egyszerű tooa beállításjegyzék is hozzárendelhető. tooassign azt tulajdonos szerepkör hozzáférés toohello beállításjegyzék, futtassa a következő példa egy parancshoz hasonló toohello:

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-toohello-registry-with-hello-service-principal"></a>Jelentkezzen be toohello beállításjegyzéket hello egyszerű szolgáltatásnév
Miután hello szolgáltatás egyszerű toohello beállításjegyzék, bármikor beléphet hello a következő parancs használatával:

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a>Rendszergazdai hitelesítő adatok kezelése
Minden tároló-beállításjegyzékhez automatikusan létrejön egy rendszergazdai fiók, amely alapértelmezés szerint le van tiltva. hello következő példák azt szemléltetik PowerShell-parancsokat a tároló rendszerleíró toomanage hello rendszergazdai hitelesítő adataival.

### <a name="obtain-admin-user-credentials"></a>Rendszergazdai hitelesítő adatok beszerzése
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Rendszergazdai felhasználó engedélyezése meglévő beállításjegyzékhez
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Rendszergazdai felhasználó letiltása meglévő beállításjegyzékhez
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a>Következő lépések
* [Leküldéses az első kép hello Docker parancssori felület használatával](container-registry-get-started-docker-cli.md)
