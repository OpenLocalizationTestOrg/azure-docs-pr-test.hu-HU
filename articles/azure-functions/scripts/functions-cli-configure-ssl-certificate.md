---
title: "aaaAzure CLI parancsfájl minta - kötés SSL tanúsítvány tooa függvény egyéni alkalmazás |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötési egyéni SSL tanúsítvány tooa függvény alkalmazás az Azure-ban"
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
ms.openlocfilehash: 692dbc03583f2978131823083f1bfd257882664c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-function-app"></a>A kötés SSL tanúsítvány tooa függvény egyéni alkalmazás

Ez a parancsfájlpélda hoz létre egy függvény alkalmazást az App Service azok kapcsolódó erőforrásait, majd egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve. Ez a minta van szüksége:

* Tooyour tartomány regisztráló DNS konfigurációs lapon érhető el.
* Egy érvényes. PFX-fájlt és a jelszó hello SSL tanúsítvány tooupload szeretné, majd kötést.

toobind SSL-tanúsítvány, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az App Service-csomag létrehozása](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Egy App Service-csomag szükséges toobind SSL-tanúsítványokat hoz létre. |
| [az functionapp létrehozása]() | Létrehoz egy függvény alkalmazást. |
| [az App Service web config állomásnév hozzáadása](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | A Maps egy egyéni tartomány toohello függvény alkalmazást. |
| [az App Service web config ssl feltöltése](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | Feltölt egy SSL tanúsítvány tooa függvény alkalmazást. |
| [az App Service web config ssl-kötés](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | SSL tanúsítvány tooa függvény feltöltött alkalmazás van kötve. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció]().
