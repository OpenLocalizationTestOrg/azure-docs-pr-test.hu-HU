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
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a><span data-ttu-id="e747d-103">Egy egyéni SSL-tanúsítvány kötését függvény alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="e747d-103">Bind a custom SSL certificate to a function app</span></span>

<span data-ttu-id="e747d-104">Ez a parancsfájlpélda hoz létre egy függvény alkalmazást az App Service azok kapcsolódó erőforrásait, majd az SSL-tanúsítvány egy egyéni tartománynév kötődik.</span><span class="sxs-lookup"><span data-stu-id="e747d-104">This sample script creates a function app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> <span data-ttu-id="e747d-105">Ez a minta van szüksége:</span><span class="sxs-lookup"><span data-stu-id="e747d-105">For this sample, you need:</span></span>

* <span data-ttu-id="e747d-106">A tartományregisztráló DNS konfigurációs laphoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="e747d-106">Access to your domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="e747d-107">Egy érvényes. PFX-fájlt és a jelszót, az SSL-tanúsítvány feltöltése és kötni kívánt.</span><span class="sxs-lookup"><span data-stu-id="e747d-107">A valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

<span data-ttu-id="e747d-108">Egy SSL-tanúsítványt kötni, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben.</span><span class="sxs-lookup"><span data-stu-id="e747d-108">To bind an SSL certificate, your function app must be created in an App Service plan and not in a consumption plan.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e747d-109">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="e747d-109">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e747d-110">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="e747d-110">Run `az --version` to find the version.</span></span> <span data-ttu-id="e747d-111">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e747d-111">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e747d-112">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="e747d-112">Sample script</span></span>

<span data-ttu-id="e747d-113">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "egyéni SSL-tanúsítvány kötése a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="e747d-113">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="e747d-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e747d-114">Script explanation</span></span>

<span data-ttu-id="e747d-115">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="e747d-115">This script uses the following commands.</span></span> <span data-ttu-id="e747d-116">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="e747d-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e747d-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="e747d-117">Command</span></span> | <span data-ttu-id="e747d-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e747d-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e747d-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e747d-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e747d-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e747d-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e747d-121">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="e747d-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="e747d-122">Létrehoz egy App Service-csomag szükséges SSL-tanúsítványok kötése.</span><span class="sxs-lookup"><span data-stu-id="e747d-122">Creates an App Service plan required to bind SSL certificates.</span></span> |
| [<span data-ttu-id="e747d-123">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="e747d-123">az functionapp create</span></span>]() | <span data-ttu-id="e747d-124">Létrehoz egy függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e747d-124">Creates a function app.</span></span> |
| [<span data-ttu-id="e747d-125">az App Service web config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e747d-125">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="e747d-126">Az egyéni tartománynév van leképezve a függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e747d-126">Maps a custom domain to the function app.</span></span> |
| [<span data-ttu-id="e747d-127">az App Service web config ssl feltöltése</span><span class="sxs-lookup"><span data-stu-id="e747d-127">az appservice web config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | <span data-ttu-id="e747d-128">Az SSL-tanúsítvány feltöltése függvény-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="e747d-128">Uploads an SSL certificate to a function app.</span></span> |
| [<span data-ttu-id="e747d-129">az App Service web config ssl-kötés</span><span class="sxs-lookup"><span data-stu-id="e747d-129">az appservice web config ssl bind</span></span>](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | <span data-ttu-id="e747d-130">Egy függvény app feltöltött SSL-tanúsítvány van kötve.</span><span class="sxs-lookup"><span data-stu-id="e747d-130">Binds an uploaded SSL certificate to a function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e747d-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e747d-131">Next steps</span></span>

<span data-ttu-id="e747d-132">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e747d-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e747d-133">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció]().</span><span class="sxs-lookup"><span data-stu-id="e747d-133">Additional App Service CLI script samples can be found in the [Azure App Service documentation]().</span></span>
