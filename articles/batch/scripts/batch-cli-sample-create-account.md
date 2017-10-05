---
title: "Az Azure CLI-parancsfájlt minta - Batch-fiók létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: 698978fd717091c49a1375e222f46f4325431223
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-batch-account-with-the-azure-cli"></a><span data-ttu-id="66d8d-103">Batch-fiók létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="66d8d-103">Create a Batch account with the Azure CLI</span></span>

<span data-ttu-id="66d8d-104">Ezt a parancsfájlt hoz létre az Azure Batch-fiók, és megjeleníti a fiók a különféle tulajdonságainak lekérdezése, és frissíti.</span><span class="sxs-lookup"><span data-stu-id="66d8d-104">This script creates an Azure Batch account and shows how various properties of the account can be queried and updated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66d8d-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66d8d-105">Prerequisites</span></span>

<span data-ttu-id="66d8d-106">Telepítse az Azure parancssori felület használatával a utasításokat a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="66d8d-106">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>

## <a name="batch-account-sample-script"></a><span data-ttu-id="66d8d-107">Batch-fiók parancsfájlpéldát</span><span class="sxs-lookup"><span data-stu-id="66d8d-107">Batch account sample script</span></span>

<span data-ttu-id="66d8d-108">Batch-fiók létrehozásakor alapértelmezés szerint a számítási csomópontok belső rendeli hozzá a Batch szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="66d8d-108">When you create a Batch account, by default its compute nodes are assigned internally by the Batch service.</span></span> <span data-ttu-id="66d8d-109">Lefoglalt számítási csomópontok részt vesznek egy külön core kvótát, és a fiók hitelesítése keresztül megosztott kulcs hitelesítő adatai vagy az Azure Active Dirctory tokent.</span><span class="sxs-lookup"><span data-stu-id="66d8d-109">Allocated compute nodes will be subject to a separate core quota and the account can be authenticated either via Shared Key credentials or an Azure Active Dirctory token.</span></span>

<span data-ttu-id="66d8d-110">[!code-azurecli[fő](../../../cli_scripts/batch/create-account/create-account.sh "fiók létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="66d8d-110">[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]</span></span>

## <a name="batch-account-using-user-subscription-sample-script"></a><span data-ttu-id="66d8d-111">Batch-fiók felhasználói előfizetés minta parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="66d8d-111">Batch account using user subscription sample script</span></span>

<span data-ttu-id="66d8d-112">Úgy is dönthet, hogy a Batch számítási csomópontjainak létrehozása a saját Azure-előfizetésben rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="66d8d-112">You can also opt to have Batch create its compute nodes in your own Azure subscription.</span></span>
<span data-ttu-id="66d8d-113">Fiókok kialakítása, amelyek számítási csomópontok a előfizetéssé hitelesíteni kell az Azure Active Directory-tokent keresztül, és a számítási csomópontok lefoglalt az előfizetési kvóta is beleszámít.</span><span class="sxs-lookup"><span data-stu-id="66d8d-113">Accounts that allocate compute nodes into your subscription must be authenticated via an Azure Active Directory token and the compute nodes allocated will count towards your subscription quota.</span></span> <span data-ttu-id="66d8d-114">Fiók létrehozása az ebben a módban, a Key Vault hivatkozás egy kell adnia a fiók létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="66d8d-114">To create an account in this mode, one must specify a Key Vault reference when creating the account.</span></span>

<span data-ttu-id="66d8d-115">[!code-azurecli[fő](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "előfizetést felhasználói fiók létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="66d8d-115">[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="66d8d-116">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="66d8d-116">Clean up deployment</span></span>

<span data-ttu-id="66d8d-117">Miután a fenti minta parancsfájlok valamelyike, futtassa a következő parancs futtatásával távolítsa el az erőforráscsoportot, és az összes kapcsolódó erőforrások (beleértve a Batch-fiókok, Azure Storage-fiókokat és Azure kulcstárolójának).</span><span class="sxs-lookup"><span data-stu-id="66d8d-117">After you run either of the above sample scripts, run the following command to remove the resource group and all related resources (including Batch accounts, Azure Storage accounts and Azure key vaults).</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="66d8d-118">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="66d8d-118">Script explanation</span></span>

<span data-ttu-id="66d8d-119">A parancsfájl a következő parancsokat egy erőforráscsoport, a Batch-fiók és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="66d8d-119">This script uses the following commands to create a resource group, Batch account, and all related resources.</span></span> <span data-ttu-id="66d8d-120">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="66d8d-120">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="66d8d-121">Parancs</span><span class="sxs-lookup"><span data-stu-id="66d8d-121">Command</span></span> | <span data-ttu-id="66d8d-122">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="66d8d-122">Notes</span></span> |
|---|---|
| [<span data-ttu-id="66d8d-123">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="66d8d-123">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="66d8d-124">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="66d8d-124">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="66d8d-125">az a batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="66d8d-125">az batch account create</span></span>](https://docs.microsoft.com/cli/azure/batch/account#create) | <span data-ttu-id="66d8d-126">A Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="66d8d-126">Creates the Batch account.</span></span>  |
| [<span data-ttu-id="66d8d-127">az batch-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="66d8d-127">az batch account set</span></span>](https://docs.microsoft.com/cli/azure/batch/account#set) | <span data-ttu-id="66d8d-128">A Batch-fiók tulajdonságainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="66d8d-128">Updates properties of the Batch account.</span></span>  |
| [<span data-ttu-id="66d8d-129">az batch-fiók megjelenítése</span><span class="sxs-lookup"><span data-stu-id="66d8d-129">az batch account show</span></span>](https://docs.microsoft.com/cli/azure/batch/account#show) | <span data-ttu-id="66d8d-130">Lekérdezi a megadott Batch-fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="66d8d-130">Retrieves details of the specified Batch account.</span></span>  |
| [<span data-ttu-id="66d8d-131">az kötegelt fióklista kulcsok</span><span class="sxs-lookup"><span data-stu-id="66d8d-131">az batch account keys list</span></span>](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | <span data-ttu-id="66d8d-132">Lekérdezi a megadott Batch-fiók hozzáférési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="66d8d-132">Retrieves the access keys of the specified Batch account.</span></span>  |
| [<span data-ttu-id="66d8d-133">az batch-fiók bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="66d8d-133">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="66d8d-134">A megadott Batch-fiók további CLI interakció azonosítja.</span><span class="sxs-lookup"><span data-stu-id="66d8d-134">Authenticates against the specified Batch account for further CLI interaction.</span></span>  |
| [<span data-ttu-id="66d8d-135">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="66d8d-135">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="66d8d-136">Tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="66d8d-136">Creates a storage account.</span></span> |
| [<span data-ttu-id="66d8d-137">az keyvault létrehozása</span><span class="sxs-lookup"><span data-stu-id="66d8d-137">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="66d8d-138">Kulcstároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="66d8d-138">Creates a key vault.</span></span> |
| [<span data-ttu-id="66d8d-139">az keyvault-szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="66d8d-139">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="66d8d-140">A biztonsági házirend a megadott kulcstároló frissítése.</span><span class="sxs-lookup"><span data-stu-id="66d8d-140">Update the security policy of the specified key vault.</span></span> |
| [<span data-ttu-id="66d8d-141">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="66d8d-141">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="66d8d-142">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="66d8d-142">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="66d8d-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66d8d-143">Next steps</span></span>

<span data-ttu-id="66d8d-144">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="66d8d-144">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="66d8d-145">További kötegelt CLI parancsfájl minták megtalálhatók a [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="66d8d-145">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
