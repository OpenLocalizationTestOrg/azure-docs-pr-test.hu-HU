---
title: "Az Azure CLI-parancsfájlt minta - tárfiókba webes alkalmazás csatlakoztatása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - tárfiókba webes alkalmazás csatlakoztatása"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 2520eecf54b77b88d6aa1ba2e538d05e3407f3d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-storage-account"></a><span data-ttu-id="7bb72-103">Webes alkalmazás csatlakoztatása a tárolási fiók</span><span class="sxs-lookup"><span data-stu-id="7bb72-103">Connect a web app to a storage account</span></span>

<span data-ttu-id="7bb72-104">Ebben a forgatókönyvben, megtudhatja, hogyan hozzon létre egy Azure storage-fiókot és egy Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="7bb72-104">In this scenario you will learn how to create an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="7bb72-105">Majd a tárfiók összekapcsolja a web app alkalmazást beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="7bb72-105">Then you will link the storage account to the web app using app settings.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7bb72-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="7bb72-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7bb72-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="7bb72-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="7bb72-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7bb72-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="7bb72-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7bb72-109">Sample script</span></span>

<span data-ttu-id="7bb72-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]</span><span class="sxs-lookup"><span data-stu-id="7bb72-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="7bb72-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7bb72-111">Script explanation</span></span>

<span data-ttu-id="7bb72-112">A parancsfájl a következő parancsokat egy erőforráscsoport, webalkalmazás, storage-fiók és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7bb72-112">This script uses the following commands to create a resource group, web app, storage account and all related resources.</span></span> <span data-ttu-id="7bb72-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="7bb72-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7bb72-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="7bb72-114">Command</span></span> | <span data-ttu-id="7bb72-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7bb72-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7bb72-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bb72-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7bb72-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7bb72-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7bb72-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bb72-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="7bb72-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7bb72-119">Creates an App Service plan.</span></span> <span data-ttu-id="7bb72-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="7bb72-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="7bb72-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bb72-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="7bb72-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7bb72-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="7bb72-123">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bb72-123">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="7bb72-124">Tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7bb72-124">Creates a storage account.</span></span> <span data-ttu-id="7bb72-125">Ez az a statikus eszközök tárolásához.</span><span class="sxs-lookup"><span data-stu-id="7bb72-125">This is where the static assets will be stored.</span></span> |
| [<span data-ttu-id="7bb72-126">az tárolási fiók megjelenítése-kapcsolat-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7bb72-126">az storage account show-connection-string</span></span>](https://docs.microsoft.com/cli/azure/storage/account#show-connection-string) | |
| [<span data-ttu-id="7bb72-127">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7bb72-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="7bb72-128">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="7bb72-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="7bb72-129">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7bb72-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7bb72-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7bb72-130">Next steps</span></span>

<span data-ttu-id="7bb72-131">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7bb72-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7bb72-132">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7bb72-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
