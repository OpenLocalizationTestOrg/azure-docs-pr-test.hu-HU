---
title: "Az Azure CLI parancsfájl minta – térkép webes alkalmazás egyéni tartomány |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - leképezés egy egyéni tartományt"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5ac4a680-cc73-4578-bcd6-8668c08802c2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6712be8a551731fbafd92ef19564e89399e23e76
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-web-app"></a><span data-ttu-id="2449f-103">Egyéni tartomány leképezése a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2449f-103">Map a custom domain to a web app</span></span>

<span data-ttu-id="2449f-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd leképezi `www.<yourdomain>` rá.</span><span class="sxs-lookup"><span data-stu-id="2449f-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` to it.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="2449f-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="2449f-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="2449f-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="2449f-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="2449f-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2449f-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="2449f-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="2449f-108">Sample script</span></span>

<span data-ttu-id="2449f-109">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "egy egyéni tartomány leképezése a webes alkalmazás")]</span><span class="sxs-lookup"><span data-stu-id="2449f-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a web app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="2449f-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="2449f-110">Script explanation</span></span>

<span data-ttu-id="2449f-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="2449f-111">This script uses the following commands.</span></span> <span data-ttu-id="2449f-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="2449f-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2449f-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="2449f-113">Command</span></span> | <span data-ttu-id="2449f-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2449f-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2449f-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2449f-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2449f-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2449f-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2449f-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="2449f-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="2449f-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2449f-118">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="2449f-119">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="2449f-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="2449f-120">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2449f-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="2449f-121">az alkalmazás kulcs config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2449f-121">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="2449f-122">Az egyéni tartománynév van leképezve a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2449f-122">Maps a custom domain to a web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2449f-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2449f-123">Next steps</span></span>

<span data-ttu-id="2449f-124">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2449f-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2449f-125">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="2449f-125">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
