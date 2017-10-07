---
title: "aaaAzure CLI parancsfájl minta - csatlakozás a webes alkalmazás tooa tárfiók |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - csatlakozás a webes alkalmazás tooa storage-fiók"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: affee2d39ef3f98c6043010850e08b67fb9ce54d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a>Csatlakozás a webes alkalmazás tooa storage-fiók

Ebben a forgatókönyvben megtudhatja, hogyan toocreate egy Azure storage-fiókot és egy Azure webes alkalmazást. Majd hello tárolási fiók toohello web app használatával Alkalmazásbeállítások csatolást.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, webalkalmazás, storage-fiók és minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára. |
| [az alkalmazás-kulcs létrehozása](https://docs.microsoft.com/cli/azure/webapp#create) | Létrehoz egy Azure-webalkalmazásban. |
| [az storage-fiók létrehozása](https://docs.microsoft.com/cli/azure/storage/account#create) | Tárfiók létrehozása. Ez a hello statikus eszközök tárolásához. |
| [az tárolási fiók megjelenítése-kapcsolat-karakterlánc](https://docs.microsoft.com/cli/azure/storage/account#show-connection-string) | |
| [az alkalmazás kulcs appsettings konfiguráció](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás. Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).
