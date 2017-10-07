---
title: "aaaAzure CLI parancsfájl minta - webalkalmazás a webkiszolgáló naplóinak figyelése |} Microsoft Docs"
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
ms.openlocfilehash: 012b800c03af77387bed3d098fae3c96d82aee29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-web-app-with-web-server-logs"></a><span data-ttu-id="ffd9a-103">A webkiszolgáló naplóinak webes alkalmazás figyelése</span><span class="sxs-lookup"><span data-stu-id="ffd9a-103">Monitor a web app with web server logs</span></span>

<span data-ttu-id="ffd9a-104">Ebben a forgatókönyvben létrehoz egy erőforráscsoport, az app service-csomag, a web app és hello web app tooenable webkiszolgáló naplóinak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-104">In this scenario you will create a resource group, app service plan, web app and configure hello web app tooenable web server logs.</span></span> <span data-ttu-id="ffd9a-105">Tekintse át a hello naplófájlokban majd letölti.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-105">You will then download hello log files for review.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ffd9a-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ffd9a-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ffd9a-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ffd9a-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ffd9a-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ffd9a-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ffd9a-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ffd9a-110">Script explanation</span></span>

<span data-ttu-id="ffd9a-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a web app és az összes kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-111">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="ffd9a-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ffd9a-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="ffd9a-113">Command</span></span> | <span data-ttu-id="ffd9a-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ffd9a-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ffd9a-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffd9a-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ffd9a-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ffd9a-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffd9a-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="ffd9a-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-118">Creates an App Service plan.</span></span> <span data-ttu-id="ffd9a-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="ffd9a-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffd9a-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="ffd9a-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="ffd9a-122">az alkalmazás kulcs naplózási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="ffd9a-122">az webapp log config</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#config) | <span data-ttu-id="ffd9a-123">Mely Azure-webalkalmazás egészen addig megmarad naplók konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-123">Configures which logs an Azure web app will persist.</span></span> |
| [<span data-ttu-id="ffd9a-124">az alkalmazás kulcs naplófájl letöltése</span><span class="sxs-lookup"><span data-stu-id="ffd9a-124">az webapp log download</span></span>](https://docs.microsoft.com/cli/azure/webapp/log#download) | <span data-ttu-id="ffd9a-125">Letöltések hello kijelentkezik a hello egy Azure web app tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="ffd9a-125">Downloads hello logs of hello an Azure web app tooyour local machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ffd9a-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ffd9a-126">Next steps</span></span>

<span data-ttu-id="ffd9a-127">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ffd9a-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ffd9a-128">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ffd9a-128">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
