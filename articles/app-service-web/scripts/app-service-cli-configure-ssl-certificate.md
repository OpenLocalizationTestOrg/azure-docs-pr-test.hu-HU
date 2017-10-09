---
title: "aaaAzure CLI parancsfájl minta - kötés egy egyéni SSL tanúsítvány tooa webalkalmazás |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötés egy egyéni SSL tanúsítvány tooa webalkalmazás"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2fec2db84a2007fa6b005776c84d4f8cba392b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a><span data-ttu-id="bf0d3-103">Egy egyéni SSL tanúsítvány tooa webalkalmazás kötése</span><span class="sxs-lookup"><span data-stu-id="bf0d3-103">Bind a custom SSL certificate tooa web app</span></span>

<span data-ttu-id="bf0d3-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, majd egy egyéni tartomány nevét tooit hello SSL-tanúsítvány van kötve.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-104">This sample script creates a web app in App Service with its related resources, then binds hello SSL certificate of a custom domain name tooit.</span></span> <span data-ttu-id="bf0d3-105">Az ebben a példában a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="bf0d3-105">For this sample, you will need:</span></span>

* <span data-ttu-id="bf0d3-106">Tooyour tartomány regisztráló DNS konfigurációs lapon érhető el.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-106">Access tooyour domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="bf0d3-107">Egy érvényes. PFX-fájlt és a jelszó hello SSL tanúsítvány tooupload szeretné, majd kötést.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-107">A valid .PFX file and its password for hello SSL certificate you want tooupload and bind.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bf0d3-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="bf0d3-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bf0d3-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bf0d3-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="bf0d3-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="bf0d3-111">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="bf0d3-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="bf0d3-112">Script explanation</span></span>

<span data-ttu-id="bf0d3-113">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-113">This script uses hello following commands.</span></span> <span data-ttu-id="bf0d3-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bf0d3-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="bf0d3-115">Command</span></span> | <span data-ttu-id="bf0d3-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bf0d3-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bf0d3-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf0d3-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bf0d3-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bf0d3-119">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf0d3-119">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="bf0d3-120">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-120">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="bf0d3-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf0d3-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="bf0d3-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="bf0d3-123">az alkalmazás kulcs config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf0d3-123">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="bf0d3-124">Az egyéni tartomány tooa webalkalmazás rendeli.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-124">Maps a custom domain tooa web app.</span></span> |
| [<span data-ttu-id="bf0d3-125">az alkalmazás kulcs config ssl feltöltése</span><span class="sxs-lookup"><span data-stu-id="bf0d3-125">az webapp config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | <span data-ttu-id="bf0d3-126">Egy SSL-tanúsítvány tooa webalkalmazás feltöltése.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-126">Uploads an SSL certificate tooa web app.</span></span> |
| [<span data-ttu-id="bf0d3-127">az alkalmazás kulcs config ssl-kötés</span><span class="sxs-lookup"><span data-stu-id="bf0d3-127">az webapp config ssl bind</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | <span data-ttu-id="bf0d3-128">Egy feltöltött SSL tanúsítvány tooa webalkalmazás van kötve.</span><span class="sxs-lookup"><span data-stu-id="bf0d3-128">Binds an uploaded SSL certificate tooa web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bf0d3-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf0d3-129">Next steps</span></span>

<span data-ttu-id="bf0d3-130">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bf0d3-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bf0d3-131">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bf0d3-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
