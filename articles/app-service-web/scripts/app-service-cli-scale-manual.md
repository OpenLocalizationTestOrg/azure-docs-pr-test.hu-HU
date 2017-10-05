---
title: "Az Azure CLI parancsfájl minta - skálázási egy webalkalmazást, manuálisan az Azure CLI 2.0 használatával |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási egy webalkalmazást, manuálisan az Azure CLI 2.0 használatával"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: fe05661eb4e2d5c37aebdbfde002b34588db69e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="86d28-103">A webalkalmazás skálázása manuálisan</span><span class="sxs-lookup"><span data-stu-id="86d28-103">Scale a web app manually</span></span>

<span data-ttu-id="86d28-104">Ebben a forgatókönyvben tanulni fog létrehozni egy erőforráscsoport, az app service-csomag és a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="86d28-104">In this scenario you will learn to create a resource group, app service plan and web app.</span></span> <span data-ttu-id="86d28-105">Akkor lesz majd skálázva az App Service-csomag egyetlen példányából származó több példányára.</span><span class="sxs-lookup"><span data-stu-id="86d28-105">You will then scale the App Service Plan from a single instance to multiple instances.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="86d28-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="86d28-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="86d28-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="86d28-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="86d28-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="86d28-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="86d28-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="86d28-109">Sample script</span></span>

<span data-ttu-id="86d28-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "manuális méretezési")]</span><span class="sxs-lookup"><span data-stu-id="86d28-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="86d28-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="86d28-111">Script explanation</span></span>

<span data-ttu-id="86d28-112">A parancsfájl a következő parancsokat egy erőforráscsoport, a web app és az összes kapcsolódó erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="86d28-112">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="86d28-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="86d28-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="86d28-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="86d28-114">Command</span></span> | <span data-ttu-id="86d28-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="86d28-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="86d28-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="86d28-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="86d28-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="86d28-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="86d28-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="86d28-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="86d28-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="86d28-119">Creates an App Service plan.</span></span> <span data-ttu-id="86d28-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="86d28-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="86d28-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="86d28-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="86d28-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="86d28-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="86d28-123">az App Service-csomag frissítése</span><span class="sxs-lookup"><span data-stu-id="86d28-123">az appservice plan update</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#update) | <span data-ttu-id="86d28-124">Frissíti az App Service-csomag tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="86d28-124">Updates properties of the App Service plan.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="86d28-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86d28-125">Next steps</span></span>

<span data-ttu-id="86d28-126">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="86d28-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="86d28-127">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="86d28-127">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
