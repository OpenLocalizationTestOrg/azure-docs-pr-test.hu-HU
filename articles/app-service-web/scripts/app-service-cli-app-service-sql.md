---
title: "aaaAzure CLI parancsfájl minta - web app tooa SQL adatbázis csatlakoztatása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - web app tooa SQL adatbázis csatlakoztatása"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: adee42cd659d977b49e71d974d240324f68f67f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a>Webes alkalmazás tooa SQL adatbázis csatlakoztatása

Ebben a forgatókönyvben megtudhatja, hogyan toocreate Azure SQL-adatbázis és egy Azure webes alkalmazást. Majd hello SQL adatbázis toohello web app használatával Alkalmazásbeállítások csatolást.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, webalkalmazás, SQL-adatbázis és az összes kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | App Service-csomag létrehozása. Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára. |
| [az alkalmazás-kulcs létrehozása](https://docs.microsoft.com/cli/azure/webapp#create) | Létrehoz egy Azure-webalkalmazásban. |
| [az sql-kiszolgáló létrehozása](https://docs.microsoft.com/cli/azure/sql/server#create) | Létrehoz egy SQL adatbázis-kiszolgálót.  |
| [az sql-adatbázis létrehozása](https://docs.microsoft.com/cli/azure/sql/db#create) | Hello SQL adatbázis-kiszolgáló egy új adatbázist hoz létre. |
| [az alkalmazás kulcs appsettings konfiguráció](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás. Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).
