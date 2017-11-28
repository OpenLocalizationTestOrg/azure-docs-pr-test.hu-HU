---
title: "Az Azure CLI parancsfájl minta - kötés egy egyéni az SSL-tanúsítványt egy |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötés egy egyéni SSL-tanúsítvány"
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
ms.openlocfilehash: d4fab3fb2c297bf5f498b63bee46692febb9180b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a><span data-ttu-id="f77ca-103">Egyéni SSL-tanúsítvány kötése a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f77ca-103">Bind a custom SSL certificate to a web app</span></span>

<span data-ttu-id="f77ca-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, majd az SSL-tanúsítvány egy egyéni tartománynév kötődik.</span><span class="sxs-lookup"><span data-stu-id="f77ca-104">This sample script creates a web app in App Service with its related resources, then binds the SSL certificate of a custom domain name to it.</span></span> <span data-ttu-id="f77ca-105">Az ebben a példában a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f77ca-105">For this sample, you will need:</span></span>

* <span data-ttu-id="f77ca-106">A tartományregisztráló DNS konfigurációs laphoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="f77ca-106">Access to your domain registrar's DNS configuration page.</span></span>
* <span data-ttu-id="f77ca-107">Egy érvényes. PFX-fájlt és a jelszót, az SSL-tanúsítvány feltöltése és kötni kívánt.</span><span class="sxs-lookup"><span data-stu-id="f77ca-107">A valid .PFX file and its password for the SSL certificate you want to upload and bind.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f77ca-108">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="f77ca-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f77ca-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f77ca-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="f77ca-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f77ca-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="f77ca-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f77ca-111">Sample script</span></span>

<span data-ttu-id="f77ca-112">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "egyéni SSL-tanúsítvány kötése a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="f77ca-112">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f77ca-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f77ca-113">Script explanation</span></span>

<span data-ttu-id="f77ca-114">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="f77ca-114">This script uses the following commands.</span></span> <span data-ttu-id="f77ca-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="f77ca-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f77ca-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="f77ca-116">Command</span></span> | <span data-ttu-id="f77ca-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f77ca-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f77ca-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f77ca-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f77ca-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f77ca-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f77ca-120">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="f77ca-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="f77ca-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f77ca-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="f77ca-122">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="f77ca-122">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="f77ca-123">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f77ca-123">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="f77ca-124">az alkalmazás kulcs config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f77ca-124">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="f77ca-125">Az egyéni tartománynév van leképezve a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f77ca-125">Maps a custom domain to a web app.</span></span> |
| [<span data-ttu-id="f77ca-126">az alkalmazás kulcs config ssl feltöltése</span><span class="sxs-lookup"><span data-stu-id="f77ca-126">az webapp config ssl upload</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | <span data-ttu-id="f77ca-127">Az SSL-tanúsítvány feltölt egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f77ca-127">Uploads an SSL certificate to a web app.</span></span> |
| [<span data-ttu-id="f77ca-128">az alkalmazás kulcs config ssl-kötés</span><span class="sxs-lookup"><span data-stu-id="f77ca-128">az webapp config ssl bind</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | <span data-ttu-id="f77ca-129">A webes alkalmazás feltöltött SSL-tanúsítvány van kötve.</span><span class="sxs-lookup"><span data-stu-id="f77ca-129">Binds an uploaded SSL certificate to a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f77ca-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f77ca-130">Next steps</span></span>

<span data-ttu-id="f77ca-131">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f77ca-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f77ca-132">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f77ca-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
