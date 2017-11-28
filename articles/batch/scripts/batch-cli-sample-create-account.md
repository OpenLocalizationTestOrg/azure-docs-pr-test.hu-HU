---
title: "parancssori felület parancsfájl minta - aaaAzure Batch-fiók létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - Batch-fiók létrehozása"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: 62b640eebbafdd1081822a638fd0720121ef067a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-cli"></a><span data-ttu-id="c4be7-103">Az Azure CLI hello Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4be7-103">Create a Batch account with hello Azure CLI</span></span>

<span data-ttu-id="c4be7-104">Ez a parancsfájl létrehoz egy Azure Batch-fiókot, és bemutatja, hogyan hello fiók tulajdonságait kérdezhetők le, és frissíti.</span><span class="sxs-lookup"><span data-stu-id="c4be7-104">This script creates an Azure Batch account and shows how various properties of hello account can be queried and updated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4be7-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c4be7-105">Prerequisites</span></span>

<span data-ttu-id="c4be7-106">Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="c4be7-106">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>

## <a name="batch-account-sample-script"></a><span data-ttu-id="c4be7-107">Batch-fiók parancsfájlpéldát</span><span class="sxs-lookup"><span data-stu-id="c4be7-107">Batch account sample script</span></span>

<span data-ttu-id="c4be7-108">Batch-fiók létrehozásakor alapértelmezés szerint a számítási csomópontok belső rendeli hozzá hello Batch-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c4be7-108">When you create a Batch account, by default its compute nodes are assigned internally by hello Batch service.</span></span> <span data-ttu-id="c4be7-109">Lefoglalt számítási csomópontok tulajdonos tooa külön core kvóta és hello fiók hitelesítése keresztül megosztott kulcs hitelesítő adatai vagy az Azure Active Dirctory tokent.</span><span class="sxs-lookup"><span data-stu-id="c4be7-109">Allocated compute nodes will be subject tooa separate core quota and hello account can be authenticated either via Shared Key credentials or an Azure Active Dirctory token.</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a><span data-ttu-id="c4be7-110">Batch-fiók felhasználói előfizetés minta parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="c4be7-110">Batch account using user subscription sample script</span></span>

<span data-ttu-id="c4be7-111">Is toohave Batch számítási csomópontjainak létrehozása a saját Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="c4be7-111">You can also opt toohave Batch create its compute nodes in your own Azure subscription.</span></span>
<span data-ttu-id="c4be7-112">Fiókok kialakítása, amelyek számítási csomópontok a előfizetéssé hitelesíteni kell az Azure Active Directory-tokent keresztül, és hello számítási csomópontok lefoglalt az előfizetési kvóta is beleszámít.</span><span class="sxs-lookup"><span data-stu-id="c4be7-112">Accounts that allocate compute nodes into your subscription must be authenticated via an Azure Active Directory token and hello compute nodes allocated will count towards your subscription quota.</span></span> <span data-ttu-id="c4be7-113">toocreate ebben a módban egy fiókot, hello fiók létrehozásakor egy kell adnia a Key Vault hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="c4be7-113">toocreate an account in this mode, one must specify a Key Vault reference when creating hello account.</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a><span data-ttu-id="c4be7-114">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c4be7-114">Clean up deployment</span></span>

<span data-ttu-id="c4be7-115">Miután lefuttatta a fent mintaparancsfájlok hello valamelyikét, futtassa a következő parancs tooremove hello az erőforráscsoport és az összes kapcsolódó erőforrások (beleértve a Batch-fiókok, Azure Storage-fiókokat és Azure kulcstárolójának).</span><span class="sxs-lookup"><span data-stu-id="c4be7-115">After you run either of hello above sample scripts, run hello following command tooremove the resource group and all related resources (including Batch accounts, Azure Storage accounts and Azure key vaults).</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="c4be7-116">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c4be7-116">Script explanation</span></span>

<span data-ttu-id="c4be7-117">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a Batch-fiók és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="c4be7-117">This script uses hello following commands toocreate a resource group, Batch account, and all related resources.</span></span> <span data-ttu-id="c4be7-118">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="c4be7-118">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="c4be7-119">Parancs</span><span class="sxs-lookup"><span data-stu-id="c4be7-119">Command</span></span> | <span data-ttu-id="c4be7-120">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c4be7-120">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c4be7-121">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4be7-121">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c4be7-122">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c4be7-122">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c4be7-123">az a batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4be7-123">az batch account create</span></span>](https://docs.microsoft.com/cli/azure/batch/account#create) | <span data-ttu-id="c4be7-124">Hello Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4be7-124">Creates hello Batch account.</span></span>  |
| [<span data-ttu-id="c4be7-125">az batch-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="c4be7-125">az batch account set</span></span>](https://docs.microsoft.com/cli/azure/batch/account#set) | <span data-ttu-id="c4be7-126">Hello Batch-fiók tulajdonságainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="c4be7-126">Updates properties of hello Batch account.</span></span>  |
| [<span data-ttu-id="c4be7-127">az batch-fiók megjelenítése</span><span class="sxs-lookup"><span data-stu-id="c4be7-127">az batch account show</span></span>](https://docs.microsoft.com/cli/azure/batch/account#show) | <span data-ttu-id="c4be7-128">Hello részleteit kérdezi le a Batch-fiókhoz megadott.</span><span class="sxs-lookup"><span data-stu-id="c4be7-128">Retrieves details of hello specified Batch account.</span></span>  |
| [<span data-ttu-id="c4be7-129">az kötegelt fióklista kulcsok</span><span class="sxs-lookup"><span data-stu-id="c4be7-129">az batch account keys list</span></span>](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | <span data-ttu-id="c4be7-130">Lekéri hello hívóbetűk hello a Batch-fiókhoz megadott.</span><span class="sxs-lookup"><span data-stu-id="c4be7-130">Retrieves hello access keys of hello specified Batch account.</span></span>  |
| [<span data-ttu-id="c4be7-131">az batch-fiók bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="c4be7-131">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="c4be7-132">Megadott hello azonosítja a Batch-fiók a további parancssori felület interakció.</span><span class="sxs-lookup"><span data-stu-id="c4be7-132">Authenticates against hello specified Batch account for further CLI interaction.</span></span>  |
| [<span data-ttu-id="c4be7-133">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4be7-133">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="c4be7-134">Tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c4be7-134">Creates a storage account.</span></span> |
| [<span data-ttu-id="c4be7-135">az keyvault létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4be7-135">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="c4be7-136">Kulcstároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c4be7-136">Creates a key vault.</span></span> |
| [<span data-ttu-id="c4be7-137">az keyvault-szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="c4be7-137">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="c4be7-138">A megadott kulcstároló hello hello biztonsági házirend frissítése.</span><span class="sxs-lookup"><span data-stu-id="c4be7-138">Update hello security policy of hello specified key vault.</span></span> |
| [<span data-ttu-id="c4be7-139">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="c4be7-139">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="c4be7-140">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="c4be7-140">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c4be7-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4be7-141">Next steps</span></span>

<span data-ttu-id="c4be7-142">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c4be7-142">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c4be7-143">További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c4be7-143">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
