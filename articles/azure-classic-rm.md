---
title: "Erőforrás-kezelő és a Service Management (klasszikus) üzembe helyezési mód |} Microsoft Docs"
description: "Ismerje meg a Resource Manager és klasszikus üzembe helyezési modellek közötti különbséget."
services: virtual-network
documentationcenter: 
author: telmosampaio
manager: carmonm
editor: 
tags: azure-resource-manager,azure-service-management
redirect_url: ./azure-resource-manager/resource-manager-deployment-model
ms.assetid: 18a235d8-38ac-4886-9e56-b3855c73ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: telmos
ms.openlocfilehash: 0f451efa239dd36d82229f01cc385d6df46654f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-deployment-models"></a>Azure üzembe helyezési modellek
Az Azure platformon átmeneti állapotban van.  Akár új Azure-ba, akár használja az évre, fontos megismerni néhány a kulcs végzett változások a platform során ez a változás.

Az összes Azure-erőforrások támogatja az egyik vagy mindkét a következő üzembe helyezési modellek:

* **Erőforrás-kezelő:** Ez az Azure-erőforrások legújabb telepítési modelljét. A legtöbb újabb erőforrás már támogatja a telepítési modell, és végül az összes erőforrás lesz.   
* **Klasszikus:** ebben a modellben ma legtöbb meglévő Azure-erőforrások támogatja. Az Azure-bA hozzáadott új erőforrások nem támogatja ezt a modellt.

Minden egyes melyik szolgáltatás modellek az Azure-erőforrás részletek dokumentációjában hozhatók létre.

## <a name="why-does-this-matter"></a>Miért fontos ez?
Akkor számít, a következő okok miatt:

* Az Azure platformon funkcióktól két modell között különbözőek.  Például a Resource Manager használatával létrehozott erőforrások telepítési modell (vagy csak az erőforrás-kezelő) hozhatók létre [Azure Resource Manager-sablonok](azure-resource-manager/resource-group-overview.md#template-deployment), mivel a klasszikus telepítési modellel létrehozott erőforrások nem.
* Az egyes Azure-erőforrás-szolgáltatások viselkedések is térhet el a két modell között, vagy csak egy modell vagy a másik szerepel.  Például a klasszikus telepítési modellel létrehozott virtuális gépek közötti forgalom terheléselosztási van *implicit* jön létre, mert a virtuális gépek Azure-Felhőszolgáltatásban tagjai, és a rendszer automatikusan az elosztott terhelésű virtuális között egy felhőalapú szolgáltatás gépeit. Resource Manager használatával létrehozott virtuális gépek nem tagjai egy felhőalapú szolgáltatás, és egy külön Azure terheléselosztó erőforrást kell *explicit módon* létrehozott betöltése oszthatja el a forgalmat több virtuális gép között.  
* Hogyan létrehozása, konfigurálása és kezelése az Azure-erőforrások eltér két modell.
* Erőforrások egy központi telepítési modellel készült feltétlenül nem tud együttműködni a különböző telepítési modellel készült erőforrásokat. Például egy központi telepítési modellel készült Azure virtuális gépek csak csatlakozhat az Azure virtuális hálózataihoz azonos telepítési modellel készült.    

Az üzembe helyezési modellekről mindegyikének alapjául szolgáló alkalmazásprogramozási felület (API) minden erőforráshoz.  Van egy [Resource Manager API](https://msdn.microsoft.com/library/azure/dn948464.aspx) a Resource Manager telepítési modell és a [szolgáltatásfelügyeleti API](https://msdn.microsoft.com/library/azure/ee460799.aspx) a klasszikus telepítési modell. Ezen API-k együttműködhet kód írhatnak *közvetlenül*.  

Az informatikusok azonban általában interakciót ezen API-k *közvetve* webböngészőben egy grafikus portál használatával, Azure PowerShell-parancsmagok segítségével Windows rendszerű számítógépeken vagy az Azure parancssori felület (CLI) használatával vagy egy Windows , OS X vagy Linux rendszerű számítógépen. Az informatikai szakemberek által használt közvetett módszer bármelyikét három közvetlenül kommunikál az API-k. Ez azt jelenti, hogy új funkció az Azure platformon és erőforrásokhoz jelenik meg, ha mindig az API-n keresztül közvetlenül elérhető először, a közvetett módszer az új erőforrások és szolgáltatások támogatása való után az API-t szeretné elérhetővé tenni.  

Az alábbi szakaszok azt ismertetik, hogyan Azure-erőforrások a különböző üzembe helyezési modellel keresztül a három közvetett módszer használatával vannak konfigurálva.

## <a name="portals"></a>Portálok
Azure két portál rendelkezik:

* **[Azure-portálon](https://manage.windowsazure.com):** eddig használt Azure egy ideig, ha már használta ezen a portálon. Segítségével hozza létre és konfigurálja a korábbi Azure-erőforrások a klasszikus üzembe helyezési modellt támogatja. Nem használható létrehozásához, vagy konfigurálja az erőforrásokat, amelyek csak az erőforrás-kezelő támogatják. 
* **[Az Azure betekintő portál](https://azure.microsoft.com/overview/preview-portal/):** egy újabb Azure-erőforrás használata, valószínűleg használt ezen a portálon. Használat létrehozása és konfigurálása az egyes Azure-erőforrások. Végül fog tudni hozza létre és konfigurálja az összes Azure-erőforrások vele. Az egyes erőforrások két üzembe helyezési modell támogató a portál segítségével létrehozhat és konfigurálhat egy erőforrás vagy központi telepítési modell használatával. 

Bizonyos erőforrások és a szolgáltatások csak hozható létre és egy portálon vagy a másik konfigurálva. Bizonyos erőforrások vagy funkciókat (még) nem lehet létrehozni vagy bármelyik portálon konfigurálva, és csak akkor konfigurálható, a PowerShell vagy a parancssori felület. Az egyes Azure-erőforrás dokumentációjában milyen létrehozhatók az módszert részletezi. 

## <a name="powershell"></a>PowerShell
A [PowerShell](/powershell/azureps-cmdlets-docs) a parancssorból vagy a létrehozása és konfigurálása az Azure-erőforrások Windows rendszerű számítógépről parancsfájlokat hozhatnak létre.  Az egyes Azure-erőforrások rendelkeznek [Resource Manager parancsmagjainak](/powershell/azure/overview), [kezelő parancsmagok](/powershell/azure/overview?view=azuresmps-3.7.0), vagy mindkettőt.  Bizonyos erőforrások és a szolgáltatások csak hozhatók létre és/vagy a PowerShell vagy a parancssori felület használatával konfigurálhatók. Az erőforrás, attól függően, erőforrás-kezelő PowerShell-parancsmagok használata esetén előfordulhat, hogy létrehozásához és konfigurálásához az Azure-erőforrások két lehetőség közül választhat:

* **Csak a PowerShell-parancsmagok:** hozhat létre és minden Azure-erőforrás egyenként a parancsmagok használatával az egyes erőforrások konfigurálása. Ehhez a parancssorból, illetve több parancsot egy PowerShell-parancsfájlt is tárolhatja és verziót.
* **PowerShell-parancsmagok az Azure Resource Manager-sablonokban:** PowerShell hozhat létre Azure-erőforrások Azure Resource Manager-sablonnal. Sablonok mentett és rendszerverzióval ellátott lehet. További ehhez beolvassa a [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md) cikk. Több [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) által letöltött és módosított túl gyakori megoldások léteznek.

## <a name="cli"></a>parancssori felület
Hozzon létre, és konfigurálja az Azure-erőforrások a parancssori felület használatával Windows, OS X vagy Linux számítógépekről.  Olvassa el a [az Azure parancssori felület telepítése](cli-install-nodejs.md) cikk a parancssori felület telepítése az operációs rendszer választott. PowerShell, például különböző parancsok, attól függően, hogy erőforrásokat használatával hoz létre használandó nincsenek [erőforrás-kezelő](xplat-cli-azure-resource-manager.md) vagy a [klasszikus (kezelő)](virtual-machines/linux/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) üzembe helyezési modellek.

## <a name="next-steps"></a>Következő lépések
* További információ [erőforrás-kezelő](azure-resource-manager/resource-group-overview.md).
* Megértéséhez hogyan [sablonok tervezési](best-practices-resource-manager-design-templates.md).

