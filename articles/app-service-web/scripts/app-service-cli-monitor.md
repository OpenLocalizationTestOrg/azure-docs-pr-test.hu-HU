---
title: "Az Azure CLI parancsfájl minta - figyelő a webes alkalmazás a webkiszolgáló naplóinak |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - figyelő a webes alkalmazás a webkiszolgáló naplóinak"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: df4ca5b1270ada849e231ad9608a5b1d2edda8be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="fa59d-103">A webkiszolgáló naplóinak webes alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="fa59d-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="fa59d-104">Ebben a forgatókönyvben létrehoz egy erőforráscsoport, az app service-csomag, a webes alkalmazás, és konfigurálja a webalkalmazás a webkiszolgáló naplóinak engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fa59d-104">In this scenario you will create a resource group, app service plan, web app and configure the web app to enable web server logs.</span></span> <span data-ttu-id="fa59d-105">Tekintse át a naplófájlokat, majd letölti azokat.</span><span class="sxs-lookup"><span data-stu-id="fa59d-105">You will then download the log files for review.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fa59d-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="fa59d-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="fa59d-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="fa59d-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="fa59d-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fa59d-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="fa59d-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="fa59d-109">Sample script</span></span>

<span data-ttu-id="fa59d-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "figyelő naplók")]</span><span class="sxs-lookup"><span data-stu-id="fa59d-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="fa59d-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="fa59d-111">Script explanation</span></span>

<span data-ttu-id="fa59d-112">A parancsfájl a következő parancsokat egy erőforráscsoport, a web app és az összes kapcsolódó erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fa59d-112">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="fa59d-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="fa59d-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="fa59d-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="fa59d-114">Command</span></span> | <span data-ttu-id="fa59d-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fa59d-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fa59d-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa59d-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="fa59d-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fa59d-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fa59d-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa59d-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="fa59d-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fa59d-119">Creates an App Service plan.</span></span> <span data-ttu-id="fa59d-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="fa59d-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="fa59d-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa59d-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="fa59d-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fa59d-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="fa59d-123">az alkalmazás kulcs naplózási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="fa59d-123">az webapp log config</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#config) | <span data-ttu-id="fa59d-124">Mely Azure-webalkalmazás egészen addig megmarad naplók konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="fa59d-124">Configures which logs an Azure web app will persist.</span></span> |
| [<span data-ttu-id="fa59d-125">az alkalmazás kulcs naplófájl letöltése</span><span class="sxs-lookup"><span data-stu-id="fa59d-125">az webapp log download</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#download) | <span data-ttu-id="fa59d-126">Letölti a naplók az Azure-webalkalmazás a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="fa59d-126">Downloads the logs of the an Azure web app to your local machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fa59d-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa59d-127">Next steps</span></span>

<span data-ttu-id="fa59d-128">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fa59d-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="fa59d-129">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fa59d-129">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
