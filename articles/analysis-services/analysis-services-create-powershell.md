---
title: "az Azure Analysis Services-kiszolgáló PowerShell-lel aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Azure Analysis Services-kiszolgáló PowerShell használatával"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: 269b78983410f773d47c4cea34d6d353b19f9e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Azure Analysis Services-kiszolgáló létrehozása a PowerShell használatával

A gyors üzembe helyezés ismerteti a PowerShell használatával a hello parancssori toocreate az Azure Analysis Services-kiszolgáló a egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) az Azure-előfizetésben.

A feladathoz az Azure PowerShell-modul 4.0-s vagy újabb verziójára lesz szükség. toofind hello verzió, futtassa ` Get-Module -ListAvailable AzureRM`. tooinstall vagy frissítés, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> A kiszolgáló létrehozása új számlázható szolgáltatás létrejöttét eredményezheti. több, lásd: toolearn [Analysis Services díjszabása](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="prerequisites"></a>Előfeltételek
toocomplete a gyors üzembe helyezés szüksége:

* **Azure-előfizetés**: keresse fel [Azure ingyenes próbaverzió](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate fiókkal.
* **Azure Active Directory**: Előfizetésének egy Azure Active Directory-bérlőhöz kell tartoznia, és abban a könyvtárban kell fiókkal rendelkeznie. több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>Az AzureRm.AnalysisServices modul importálása
toocreate egy kiszolgálóhoz az előfizetését, használhat hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) összetevő modul. Hello AzureRm.AnalysisServices modul betölteni a PowerShell-munkamenetbe.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be Azure-előfizetés tooyour hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) parancsot. Kövesse hello képernyőn megjelenő utasításokat.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
 
Az [Azure-erőforráscsoport](../azure-resource-manager/resource-group-overview.md) egy olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat. A kiszolgáló létrehozásakor meg kell adnia egy erőforráscsoportot az előfizetésén belül. Ha még nem rendelkezik egy erőforráscsoport, létrehozhat egy új hello segítségével [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsot. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` hello USA nyugati régiója régióban.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>A kiszolgáló létrehozása

Hozzon létre egy új kiszolgálót hello segítségével [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) parancsot. hello alábbi példa létrehoz egy nevű myResourceGroup hello USA nyugati régiója régióban, hello D1 réteg a myServer kiszolgáló, és határozza meg philipc@adventureworks.com server rendszergazdaként.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Eltávolíthatja hello server előfizetésből hello segítségével [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) parancsot. Ha a gyűjteménybe tartozó további rövid útmutatókkal és oktatóanyagokkal szeretné folytatni, ne távolítsa el a kiszolgálót. hello következő példában eltávolítjuk hello előző lépésben létrehozott hello kiszolgáló.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Következő lépések
[Az Azure Analysis Services kezelése a PowerShell-lel](analysis-services-powershell.md)   
[Modell üzembe helyezése SSDT-ről](analysis-services-deploy.md)   
[Modell létrehozása az Azure Portalon](analysis-services-create-model-portal.md)