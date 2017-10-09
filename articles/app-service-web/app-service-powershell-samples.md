---
title: "App Service - PowerShell-példák aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: b7b4a030364f797195522c56fbae5b7f530d4d1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples"></a>Az Azure PowerShell-példák

hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure PowerShell használatával készített.

| | |
|-|-|
|**Alkalmazás létrehozása**||
| [Hozzon létre egy webalkalmazást telepítése a Githubból](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Létrehoz egy Azure webalkalmazás, amely a kód lekéri a Githubról. |
| [Webalkalmazás létrehozása folyamatos üzembe helyezéssel a GitHubról](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Egy Azure webalkalmazás, amely folyamatosan telepíti a GitHub-kódot hoz létre. |
| [Hozzon létre egy webalkalmazást és FTP kód telepítése](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Fájlokat hozza létre az Azure web app és feltöltése egy helyi könyvtárból FTP használatával. |
| [Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-tárházból](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás létrehozása, és konfigurálja a helyi Git-tárház kód leküldéses. |
| [Webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás egy üzembe helyezési pont kódmódosításokat átmeneti hoz létre. |
|**Alkalmazás konfigurálása**||
| [Az egyéni tartomány tooa webalkalmazás leképezése](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure-webalkalmazás létrehozása, és egy egyéni tartomány nevét tooit társít. |
| [Egy egyéni SSL tanúsítvány tooa webalkalmazás kötése](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure-webalkalmazás létrehozása és egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve. |
|**Skála alkalmazás**||
| [Webalkalmazások manuális méretezése](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás létrehozása és méretezi a 2 példányok között. |
| [Webalkalmazások globális méretezése magas rendelkezésre állású architektúrával](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Két Azure-webalkalmazásokban két különböző földrajzi régió hoz létre, és egy végpontot, használja az Azure Traffic Manager keresztül elérhetővé válnak. |
|**Kapcsolódás app tooresources**||
| [Csatlakozás egy webes alkalmazás tooa SQL-adatbázis](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Létrehoz egy Azure web app és az SQL-adatbázis, majd hozzáadja a hello adatbázis kapcsolati karakterlánc toohello Alkalmazásbeállítások. |
| [Csatlakozás a webes alkalmazás tooa storage-fiók](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure-webalkalmazás és a storage-fiók létrehozása, majd hozzáadja a hello tárolási kapcsolati karakterlánc toohello Alkalmazásbeállítások. |
|**Alkalmazás monitorozása**||
| [Webalkalmazás figyelése a webkiszolgáló-naplókkal](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Azure-webalkalmazás létrehozása, engedélyezi a naplózást, és letölti hello naplók tooyour helyi számítógép. |
| | |
