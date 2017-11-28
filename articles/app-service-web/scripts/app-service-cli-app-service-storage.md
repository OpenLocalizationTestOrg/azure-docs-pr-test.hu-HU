---
title: "aaaAzure CLI parancsfájl minta - csatlakozás a webes alkalmazás tooa tárfiók |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - csatlakozás a webes alkalmazás tooa storage-fiók"
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
ms.openlocfilehash: affee2d39ef3f98c6043010850e08b67fb9ce54d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a><span data-ttu-id="88090-103">Csatlakozás a webes alkalmazás tooa storage-fiók</span><span class="sxs-lookup"><span data-stu-id="88090-103">Connect a web app tooa storage account</span></span>

<span data-ttu-id="88090-104">Ebben a forgatókönyvben megtudhatja, hogyan toocreate egy Azure storage-fiókot és egy Azure webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="88090-104">In this scenario you will learn how toocreate an Azure storage account and an Azure web app.</span></span> <span data-ttu-id="88090-105">Majd hello tárolási fiók toohello web app használatával Alkalmazásbeállítások csatolást.</span><span class="sxs-lookup"><span data-stu-id="88090-105">Then you will link hello storage account toohello web app using app settings.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="88090-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="88090-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="88090-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="88090-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="88090-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="88090-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="88090-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="88090-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="88090-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="88090-110">Script explanation</span></span>

<span data-ttu-id="88090-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, webalkalmazás, storage-fiók és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="88090-111">This script uses hello following commands toocreate a resource group, web app, storage account and all related resources.</span></span> <span data-ttu-id="88090-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="88090-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="88090-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="88090-113">Command</span></span> | <span data-ttu-id="88090-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="88090-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="88090-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="88090-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="88090-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="88090-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="88090-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="88090-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="88090-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="88090-118">Creates an App Service plan.</span></span> <span data-ttu-id="88090-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="88090-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="88090-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="88090-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="88090-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="88090-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="88090-122">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="88090-122">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="88090-123">Tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="88090-123">Creates a storage account.</span></span> <span data-ttu-id="88090-124">Ez a hello statikus eszközök tárolásához.</span><span class="sxs-lookup"><span data-stu-id="88090-124">This is where hello static assets will be stored.</span></span> |
| [<span data-ttu-id="88090-125">az tárolási fiók megjelenítése-kapcsolat-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="88090-125">az storage account show-connection-string</span></span>](https://docs.microsoft.com/cli/azure/storage/account#show-connection-string) | |
| [<span data-ttu-id="88090-126">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="88090-126">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="88090-127">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="88090-127">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="88090-128">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="88090-128">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="88090-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88090-129">Next steps</span></span>

<span data-ttu-id="88090-130">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="88090-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="88090-131">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="88090-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
