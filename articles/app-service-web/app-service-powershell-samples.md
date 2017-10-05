---
title: "Az Azure PowerShell minták – az App Service |} Microsoft Docs"
description: "Az Azure PowerShell minták – az App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 3254fdd57cfcd170f22374c1e3b058e6081d8e8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples"></a>Az Azure PowerShell-példák

A következő táblázat a bash parancsfájlok az Azure PowerShell használatával készített mutató hivatkozásokat tartalmaz.

| | |
|-|-|
|**Alkalmazás létrehozása**||
| [Hozzon létre egy webalkalmazást telepítése a Githubból](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Létrehoz egy Azure webalkalmazás, amely a kód lekéri a Githubról. |
| [Webalkalmazás létrehozása folyamatos üzembe helyezéssel a GitHubról](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Egy Azure webalkalmazás, amely folyamatosan telepíti a GitHub-kódot hoz létre. |
| [Hozzon létre egy webalkalmazást és FTP kód telepítése](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Fájlokat hozza létre az Azure web app és feltöltése egy helyi könyvtárból FTP használatával. |
| [Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-tárházból](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás létrehozása, és konfigurálja a helyi Git-tárház kód leküldéses. |
| [Webalkalmazás létrehozása és kód üzembe helyezése átmeneti környezetbe](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás egy üzembe helyezési pont kódmódosításokat átmeneti hoz létre. |
|**Alkalmazás konfigurálása**||
| [Egyéni tartomány leképezése egy webalkalmazásra](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure-webalkalmazás létrehozása és egy egyéni tartománynevet rendel. |
| [Egyéni SSL-tanúsítvány kötése a webes alkalmazás](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure-webalkalmazás létrehozása, és az SSL-tanúsítvány egy egyéni tartománynév kötődik. |
|**Skála alkalmazás**||
| [Webalkalmazások manuális méretezése](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás létrehozása és méretezi a 2 példányok között. |
| [Webalkalmazások globális méretezése magas rendelkezésre állású architektúrával](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Két Azure-webalkalmazásokban két különböző földrajzi régió hoz létre, és egy végpontot, használja az Azure Traffic Manager keresztül elérhetővé válnak. |
|**Alkalmazás kapcsolódni tudjon erőforrásokhoz**||
| [Webes alkalmazás csatlakoztatása SQL-adatbázishoz](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Létrehoz egy Azure web app és az SQL-adatbázis, majd az adatbázis-kapcsolati karakterláncot ad hozzá az alkalmazás beállításaiban. |
| [Webalkalmazás csatlakoztatása tárfiókhoz](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure-webalkalmazás és a storage-fiók létrehozása, majd hozzáadja a tárolási kapcsolati karakterlánc az alkalmazás beállításaiban. |
|**Alkalmazás monitorozása**||
| [Webalkalmazás figyelése a webkiszolgáló-naplókkal](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás létrehozása, engedélyezi a naplózást, és a naplók letölti a helyi számítógépre. |
| | |
