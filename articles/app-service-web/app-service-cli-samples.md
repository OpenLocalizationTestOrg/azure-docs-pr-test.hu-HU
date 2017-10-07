---
title: "aaaAzure CLI minták – az App Service |} Microsoft Docs"
description: "Az Azure CLI minták – az App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a943ccffb59c5d30a44cf1ce513fd2eac46101f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples"></a>Az Azure CLI-minták

hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure parancssori felület használatával készített.

| | |
|-|-|
|**Alkalmazás létrehozása**||
| [Webalkalmazás létrehozása és kód üzembe helyezése a GitHubról](./scripts/app-service-cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Azure-webalkalmazás létrehozza, és egy nyilvános GitHub-tárházban lévő kód telepíti. |
| [Webalkalmazás létrehozása folyamatos üzembe helyezéssel a GitHubról](./scripts/app-service-cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Saját GitHub-tárházban folyamatos közzététel hoz létre egy Azure-webalkalmazásban. |
| [Webalkalmazás létrehozása és kód üzembe helyezése helyi Git-tárházból](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Azure-webalkalmazás létrehozása, és konfigurálja a helyi Git-tárház kód leküldéses. |
| [Webalkalmazás létrehozása és központi telepítése átmeneti környezet kód tooa](./scripts/app-service-cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Azure-webalkalmazás egy üzembe helyezési pont kódmódosításokat átmeneti hoz létre. |
| [ASP.NET-magalapú webalkalmazás létrehozása Docker-tárolóban](./scripts/app-service-cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Azure-webalkalmazás létrehozása a Linux és a Docker-lemezkép betölti a Docker Hub. |
|**Alkalmazás konfigurálása**||
| [Az egyéni tartomány tooa webalkalmazás leképezése](./scripts/app-service-cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Azure-webalkalmazás létrehozása, és egy egyéni tartomány nevét tooit társít. |
| [Egy egyéni SSL tanúsítvány tooa webalkalmazás kötése](./scripts/app-service-cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Azure-webalkalmazás létrehozása és egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve. |
|**Skála alkalmazás**||
| [Webalkalmazások manuális méretezése](./scripts/app-service-cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Azure-webalkalmazás létrehozása és méretezi a 2 példányok között. |
| [Webalkalmazások globális méretezése magas rendelkezésre állású architektúrával](./scripts/app-service-cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Két Azure-webalkalmazásokban két különböző földrajzi régió hoz létre, és egy végpontot, használja az Azure Traffic Manager keresztül elérhetővé válnak. |
|**Kapcsolódás app tooresources**||
| [Csatlakozás egy webes alkalmazás tooa SQL-adatbázis](./scripts/app-service-cli-app-service-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Létrehoz egy Azure web app és az SQL-adatbázis, majd hozzáadja a hello adatbázis kapcsolati karakterlánc toohello Alkalmazásbeállítások. |
| [Csatlakozás a webes alkalmazás tooa storage-fiók](./scripts/app-service-cli-app-service-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Azure-webalkalmazás és a storage-fiók létrehozása, majd hozzáadja a hello tárolási kapcsolati karakterlánc toohello Alkalmazásbeállítások. |
| [Csatlakozás a webes alkalmazás tooa redis gyorsítótár](./scripts/app-service-cli-app-service-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Azure-webalkalmazás és a redis gyorsítótár hoz létre, majd hozzáadja hello redis kapcsolódási részletek toohello Alkalmazásbeállítások.) |
| [Csatlakozás egy webes alkalmazás tooCosmos DB](./scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy Azure webalkalmazás és egy Cosmos DB, majd hozzáadja a hello Cosmos DB kapcsolat részletek toohello Alkalmazásbeállítások. |
|**Alkalmazás monitorozása**||
| [Webalkalmazás figyelése a webkiszolgáló-naplókkal](./scripts/app-service-cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Azure-webalkalmazás létrehozása, engedélyezi a naplózást, és letölti hello naplók tooyour helyi számítógép. |
| | |