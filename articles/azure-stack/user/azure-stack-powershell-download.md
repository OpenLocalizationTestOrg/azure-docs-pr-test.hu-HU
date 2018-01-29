---
title: "Töltse le az Azure-verem eszközök a Githubról |} Microsoft Docs"
description: "Útmutató az Azure veremnek megfelelő működéséhez szükséges eszközök."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 28F360AD-789A-488D-965F-FC6E6CCF3329
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: mabrigg
ms.openlocfilehash: d4f8a8d73f8e2ea321cb6cc1deda2301033b249d
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="download-azure-stack-tools-from-github"></a>Töltse le az Azure-verem eszközök a Githubról

AzureStack-eszközök kezelésének és központi telepítésének erőforrások Azure verem használható PowerShell-modulok üzemeltető GitHub-tárházban. Töltse le, és a PowerShell-modulok az Azure verem szoftverfejlesztői készlet, vagy a windows-alapú külső ügyfél használja, ha azt tervezi, hogy a VPN-kapcsolatot létrehozni. Ezek az eszközök klónozza a GitHub-tárházban, vagy töltse le a AzureStack-eszközök mappa a következő parancsfájl futtatásával:

```PowerShell
# Change directory to the root directory 
cd \

# Download the tools archive
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory
cd AzureStack-Tools-master

```

## <a name="functionalities-provided-by-the-modules"></a>A modulok által nyújtott funkciók

A AzureStack-eszközök tárház, amely támogatja a következő funkciók verem Azure PowerShell-modulok tartalmazza:  

| Funkció | Leírás | Ez a modul ki tudja használni? |
| --- | --- | --- |
| [Felhőalapú képességek](azure-stack-validate-templates.md) | Ez a modul segítségével felhő felhőalapú szolgáltatásokkal. A felhőalapú képességek, például az API-verzió, az Azure Resource Managerhez tartozó erőforrások, a Virtuálisgép-bővítmények stb. például Azure verem, és ez a modul Azure felhők kaphat. | Felhő rendszergazdák és felhasználók. |
| [Erőforrás-kezelő házirend Azure verem](azure-stack-policy-module.md) | Ez a modul segítségével konfigurálása Azure-előfizetés vagy egy Azure-erőforráscsoportot a azonos versioning és a szolgáltatás rendelkezésre állás mint Azure verem. | Felhő rendszergazdák és felhasználók |
| [Kapcsolódás Azure verem](azure-stack-connect-azure-stack.md) | Ez a modul használja, egy Azure veremben példányra Powershellen keresztül csatlakozni, és Azure verem VPN-kapcsolat konfigurálása. | Felhő rendszergazdák és felhasználók |
| [Sablon érvényesítő](azure-stack-validate-templates.md) | Ez a modul segítségével ellenőrizheti, ha egy meglévő vagy új sablon Azure verem számára telepíthető. | Felhő rendszergazdák és felhasználók |


## <a name="next-steps"></a>Következő lépések
* [Az Azure-verem felhasználói PowerShell környezet konfigurálása](azure-stack-powershell-configure-user.md)   
* [Csatlakozás Azure verem szoftverfejlesztői készlet egy VPN-kapcsolaton keresztül](azure-stack-connect-azure-stack.md)  
