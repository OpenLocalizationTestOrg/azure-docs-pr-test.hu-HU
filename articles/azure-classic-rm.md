---
title: "aaaResource Manager és Service Management (klasszikus) üzembe helyezési mód |} Microsoft Docs"
description: "Ismerje meg a Resource Manager és klasszikus üzembe helyezési modellek hello különbségei."
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
ms.openlocfilehash: e14f815ba9d79d9dd8f83b45bda78d0eba0bec56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-deployment-models"></a>Azure üzembe helyezési modellek
hello Azure platformon átmeneti állapotban van.  A rendszer új tooAzure vagy használja az évre, hogy-e fontos toounderstand néhány főbb változását hello azt hajt toohello platform során ez a változás.

Az összes Azure-erőforrások egyik vagy mindkét hello a következő üzembe helyezési modellel támogatja:

* **Erőforrás-kezelő:** Ez az Azure-erőforrások hello legújabb telepítési modelljét. A legtöbb újabb erőforrás már támogatja a telepítési modell, és végül az összes erőforrás lesz.   
* **Klasszikus:** ebben a modellben ma legtöbb meglévő Azure-erőforrások támogatja. Új erőforrások hozzá tooAzure nem támogatják az ebben a modellben.

hello dokumentációjában minden Azure-erőforrás melyik szolgáltatás modellek akkor hozhatók létre.

## <a name="why-does-this-matter"></a>Miért fontos ez?
Akkor számít, a következő okok miatt hello:

* hello Azure platformon funkcióktól két modell között különbözőek.  Például hello Resource Manager üzembe helyezési modellben (vagy csak az erőforrás-kezelő) használatával létrehozott erőforrásokat lehet létrehozni a [Azure Resource Manager-sablonok](azure-resource-manager/resource-group-overview.md#template-deployment), mivel hello klasszikus telepítési modellel létrehozott erőforrások nem.
* az egyes Azure-erőforrás szolgáltatások hello vagy viselkedés különböző hello két modell között, és csak egy adott modellen vagy más hello szerepel.  Például terheléselosztásának forgalom hello klasszikus telepítési modellel létrehozott virtuális gépeken van *implicit* jön létre, mert a virtuális gépek Azure-Felhőszolgáltatásban tagjai, és a rendszer automatikusan az elosztott terhelésű virtuális között egy felhőalapú szolgáltatás gépeit. Resource Manager használatával létrehozott virtuális gépek nem tagjai egy felhőalapú szolgáltatás, és egy külön Azure Load Balancer erőforrás kell *explicit módon* létrehozott tooload oszthatja el a forgalmat több virtuális gép között.  
* Hogyan létrehozása, konfigurálása és kezelése az Azure-erőforrások eltér két modell.
* Erőforrások egy központi telepítési modellel készült feltétlenül nem tud együttműködni a különböző telepítési modellel készült erőforrásokat. Például egy központi telepítési modellel készült Azure virtuális gépek csak lehet csatlakoztatott tooAzure hello használatával létrehozott virtuális hálózatokat megegyező telepítési modellben.    

Egyes hello üzembe helyezési modellel alapjául szolgáló alkalmazásprogramozási felület (API) minden erőforráshoz.  Van egy [Resource Manager API](https://msdn.microsoft.com/library/azure/dn948464.aspx) hello erőforrás-kezelő telepítési modell és a [szolgáltatásfelügyeleti API](https://msdn.microsoft.com/library/azure/ee460799.aspx) hello klasszikus telepítési modell. A fejlesztők írhat kódot toointeract ezen API-khoz *közvetlenül*.  

Az informatikusok azonban általában interakciót ezen API-k *közvetve* grafikus a portál használatával egy webböngészőben, Azure PowerShell használatával parancsmagok a Windows rendszerű számítógépeken, vagy a hello Azure parancssori felület (CLI) akár egy Windows, OS X vagy Linux számítógépen. Közvetett módszer bármelyikét hello informatikai szakemberek által használt három közvetlenül kommunikálni hello API-k. Ez azt jelenti, hogy ha új funkciókat bevezetett toohello Azure platform vagy erőforrásokat, mindig hello API keresztül közvetlenül elérhető először, hello közvetett módszer való hello támogatása új erőforrások és szolgáltatások után hello API szeretné elérhetővé tenni.  

az alábbi hello szakaszok azt ismertetik, hogyan Azure-erőforrások közvetett hello három módszerrel hello különböző üzembe helyezési modellel használatával vannak konfigurálva.

## <a name="portals"></a>Portálok
Azure két portál rendelkezik:

* **[Azure-portálon](https://manage.windowsazure.com):** eddig használt Azure egy ideig, ha már használta ezen a portálon. Használt toocreate, és konfigurálja a régebbi Azure-erőforrások hello klasszikus üzembe helyezési modellt támogatja. Nem használhatja toocreate és erőforrásokat, amelyek csak támogatják az erőforrás-kezelő konfigurálása. 
* **[Az Azure betekintő portál](https://azure.microsoft.com/overview/preview-portal/):** egy újabb Azure-erőforrás használata, valószínűleg használt ezen a portálon. Azt is használt toocreate és néhány Azure-erőforrások konfigurálása. Lesz végül lenniük képes toocreate, és az összes Azure-erőforrások konfigurálása vele. Az egyes erőforrásokat, amelyek támogatják a két üzembe helyezési modell ezen a portálon használt toocreate legyen, és egy erőforrás vagy központi telepítési modell segítségével konfigurálja. 

Bizonyos erőforrások és a szolgáltatások csak hozható létre és egy portálon vagy más hello konfigurálva. Bizonyos erőforrások vagy funkciókat (még) nem hozható létre vagy konfigurálva vagy a portál és csak a Powershellel konfigurálható, hello CLI, vagy mindkettőt. minden Azure-erőforrás hello dokumentációja részletesen milyen létrehozhatók a módszert. 

## <a name="powershell"></a>PowerShell
A [PowerShell](/powershell/azureps-cmdlets-docs) használhatja a parancssorban vagy a szerző parancsfájlok toocreate és konfigurálása az Azure-erőforrások Windows rendszerű számítógépről.  Az egyes Azure-erőforrások rendelkeznek [Resource Manager parancsmagjainak](/powershell/azure/overview), [kezelő parancsmagok](/powershell/azure/overview?view=azuresmps-3.7.0), vagy mindkettőt.  Bizonyos erőforrások és a szolgáltatások csak hozhatók létre és/vagy a PowerShell vagy a hello CLI segítségével konfigurálható. Hello erőforrás, attól függően, erőforrás-kezelő PowerShell-parancsmagok használata esetén előfordulhat, hogy létrehozásához és konfigurálásához az Azure-erőforrások két lehetőség közül választhat:

* **Csak a PowerShell-parancsmagok:** hozhat létre és minden Azure-erőforrás külön-külön az egyes erőforrások a hello parancsmagok segítségével konfigurálhatja. Ehhez a parancssorból, illetve több parancsot egy PowerShell-parancsfájlt is tárolhatja és verziót.
* **PowerShell-parancsmagok az Azure Resource Manager-sablonokban:** használhatja az PowerShell toocreate Azure erőforrásokat az Azure Resource Manager-sablon használatával. Sablonok mentett és rendszerverzióval ellátott lehet. További hello olvasásával [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md) cikk. Több [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) által letöltött és módosított túl gyakori megoldások léteznek.

## <a name="cli"></a>parancssori felület
Hozzon létre, és konfigurálja az Azure-erőforrások használata hello CLI Windows, OS X vagy Linux számítógépekről.  Olvasási hello [telepítés hello Azure CLI](cli-install-nodejs.md) cikk tooinstall hello CLI a választott operációs rendszerre. PowerShell, például különböző parancsok, attól függően, hogy erőforrásokat használatával hoz létre használandó nincsenek [erőforrás-kezelő](xplat-cli-azure-resource-manager.md) vagy hello [klasszikus (kezelő)](virtual-machines/linux/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) üzembe helyezési modellek.

## <a name="next-steps"></a>Következő lépések
* További információ [erőforrás-kezelő](azure-resource-manager/resource-group-overview.md).
* Hogyan túl megértéséhez[sablonok tervezési](best-practices-resource-manager-design-templates.md).

