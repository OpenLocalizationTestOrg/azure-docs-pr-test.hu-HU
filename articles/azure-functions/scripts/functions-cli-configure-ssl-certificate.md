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
# <a name="bind-a-custom-ssl-certificate-tooa-function-app"></a><span data-ttu-id="f0e32-103">A kötés SSL tanúsítvány tooa függvény egyéni alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0e32-103">Bind a custom SSL certificate tooa function app</span></span>

<span data-ttu-id="f0e32-104">Ez a parancsfájlpélda hoz létre egy függvény alkalmazást az App Service azok kapcsolódó erőforrásait, majd egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve.</span><span class="sxs-lookup"><span data-stu-id="f0e32-104">This sample script creates a function app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> <span data-ttu-id="f0e32-105">Ez a minta van szüksége:</span><span class="sxs-lookup"><span data-stu-id="f0e32-105">For this sample, you need:</span></span>

* <span data-ttu-id="f0e32-106">Tooyour tartomány regisztráló DNS konfigurációs lapon érhető el.</span><span class="sxs-lookup"><span data-stu-id="f0e32-106">Access tooyour domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="f0e32-107">Egy érvényes. PFX-fájlt és a jelszó hello SSL tanúsítvány tooupload szeretné, majd kötést.</span><span class="sxs-lookup"><span data-stu-id="f0e32-107">A valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

<span data-ttu-id="f0e32-108">toobind SSL-tanúsítvány, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben.</span><span class="sxs-lookup"><span data-stu-id="f0e32-108">toobind an SSL certificate, your function app must be created in an App Service plan and not in a consumption plan.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f0e32-109">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f0e32-109">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f0e32-110">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="f0e32-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f0e32-111">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f0e32-111">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f0e32-112">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f0e32-112">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f0e32-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f0e32-113">Script explanation</span></span>

<span data-ttu-id="f0e32-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="f0e32-114">This script uses hello following commands.</span></span> <span data-ttu-id="f0e32-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="f0e32-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f0e32-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="f0e32-116">Command</span></span> | <span data-ttu-id="f0e32-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f0e32-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f0e32-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0e32-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f0e32-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f0e32-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f0e32-120">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0e32-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="f0e32-121">Egy App Service-csomag szükséges toobind SSL-tanúsítványokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f0e32-121">Creates an App Service plan required toobind SSL certificates.</span></span> |
| [<span data-ttu-id="f0e32-122">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0e32-122">az functionapp create</span></span>]() | <span data-ttu-id="f0e32-123">Létrehoz egy függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f0e32-123">Creates a function app.</span></span> |
| [<span data-ttu-id="f0e32-124">az App Service web config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f0e32-124">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="f0e32-125">A Maps egy egyéni tartomány toohello függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f0e32-125">Maps a custom domain toohello function app.</span></span> |
| [<span data-ttu-id="f0e32-126">az App Service web config ssl feltöltése</span><span class="sxs-lookup"><span data-stu-id="f0e32-126">az appservice web config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | <span data-ttu-id="f0e32-127">Feltölt egy SSL tanúsítvány tooa függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f0e32-127">Uploads an SSL certificate tooa function app.</span></span> |
| [<span data-ttu-id="f0e32-128">az App Service web config ssl-kötés</span><span class="sxs-lookup"><span data-stu-id="f0e32-128">az appservice web config ssl bind</span></span>](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | <span data-ttu-id="f0e32-129">SSL tanúsítvány tooa függvény feltöltött alkalmazás van kötve.</span><span class="sxs-lookup"><span data-stu-id="f0e32-129">Binds an uploaded SSL certificate tooa function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f0e32-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0e32-130">Next steps</span></span>

<span data-ttu-id="f0e32-131">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f0e32-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f0e32-132">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció]().</span><span class="sxs-lookup"><span data-stu-id="f0e32-132">Additional App Service CLI script samples can be found in hello [Azure App Service documentation]().</span></span>
