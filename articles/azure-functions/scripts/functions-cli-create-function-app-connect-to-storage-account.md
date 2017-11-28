---
title: "egy Azure-függvény, amely összeköti az Azure Storage tooan aaaCreate |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták –, amely a tooan Azure Storage Azure-függvény létrehozása"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: a51a2c17149478eb2d3d0d4034400ed00cd8416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a><span data-ttu-id="21bb4-103">Függvény alkalmazás integrálja az Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="21bb4-103">Integrate Function App into Azure Storage Account</span></span>

<span data-ttu-id="21bb4-104">Ez a parancsfájlpélda hoz létre, a függvény alkalmazás és a Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="21bb4-104">This sample script creates a Function App and Storage Account.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="21bb4-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="21bb4-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="21bb4-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="21bb4-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="21bb4-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="21bb4-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="21bb4-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="21bb4-108">Sample script</span></span>

<span data-ttu-id="21bb4-109">Ez a minta egy Azure-függvény alkalmazást hoz létre, és hozzáadja a hello tárolási kapcsolati karakterlánc tooan alkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="21bb4-109">This sample creates an Azure Function app and adds hello storage connection string tooan app setting.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a><span data-ttu-id="21bb4-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="21bb4-110">Clean up deployment</span></span>

<span data-ttu-id="21bb4-111">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, az App Service alkalmazás és minden kapcsolódó erőforrások:</span><span class="sxs-lookup"><span data-stu-id="21bb4-111">After hello script sample has been run, hello following command can be used tooremove hello resource group, App Service app, and all related resources:</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="21bb4-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="21bb4-112">Script explanation</span></span>

<span data-ttu-id="21bb4-113">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="21bb4-113">This script uses hello following commands.</span></span> <span data-ttu-id="21bb4-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="21bb4-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="21bb4-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="21bb4-115">Command</span></span> | <span data-ttu-id="21bb4-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="21bb4-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="21bb4-117">az bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="21bb4-117">az login</span></span>](https://docs.microsoft.com/cli/azure/#login) | <span data-ttu-id="21bb4-118">Bejelentkezési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="21bb4-118">Login tooAzure.</span></span> |
| [<span data-ttu-id="21bb4-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="21bb4-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="21bb4-120">Hozzon létre egy erőforráscsoportot, amelynek a helye</span><span class="sxs-lookup"><span data-stu-id="21bb4-120">Create a resource group with location</span></span> |
| [<span data-ttu-id="21bb4-121">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="21bb4-121">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account) | <span data-ttu-id="21bb4-122">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="21bb4-122">Create a storage account</span></span> |
| [<span data-ttu-id="21bb4-123">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="21bb4-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="21bb4-124">Hozzon létre egy új funkció alkalmazást</span><span class="sxs-lookup"><span data-stu-id="21bb4-124">Create a new function app</span></span> |
| [<span data-ttu-id="21bb4-125">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="21bb4-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="21bb4-126">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="21bb4-126">Clean up</span></span> |

## <a name="next-steps"></a><span data-ttu-id="21bb4-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21bb4-127">Next steps</span></span>

<span data-ttu-id="21bb4-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="21bb4-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="21bb4-129">További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="21bb4-129">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
