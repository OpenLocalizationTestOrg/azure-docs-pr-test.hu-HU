---
title: "Az Azure CLI parancsfájl minta - kötési egy egyéni az SSL-tanúsítványt függvény alkalmazásokhoz |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötés egy egyéni SSL-tanúsítvány függvény alkalmazásokhoz az Azure-ban"
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: ddabb701d7d5615232d1f6163aa6fb166efe5cb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Egy egyéni SSL-tanúsítvány kötését függvény alkalmazásokhoz

Ez a parancsfájlpélda hoz létre egy függvény alkalmazást az App Service azok kapcsolódó erőforrásait, majd az SSL-tanúsítvány egy egyéni tartománynév kötődik. Ez a minta van szüksége:

* A tartományregisztráló DNS konfigurációs laphoz való hozzáférés.
* Egy érvényes. PFX-fájlt és a jelszót, az SSL-tanúsítvány feltöltése és kötni kívánt.

Egy SSL-tanúsítványt kötni, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "egyéni SSL-tanúsítvány kötése a webes alkalmazás")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Létrehoz egy App Service-csomag szükséges SSL-tanúsítványok kötése. |
| [az functionapp létrehozása]() | Létrehoz egy függvény alkalmazást. |
| [az App Service web config állomásnév hozzáadása](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Az egyéni tartománynév van leképezve a függvény alkalmazást. |
| [az App Service web config ssl feltöltése](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | Az SSL-tanúsítvány feltöltése függvény-alkalmazásokhoz. |
| [az App Service web config ssl-kötés](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | Egy függvény app feltöltött SSL-tanúsítvány van kötve. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció]().
