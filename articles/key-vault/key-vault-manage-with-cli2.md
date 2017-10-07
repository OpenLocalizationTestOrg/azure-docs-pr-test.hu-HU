---
title: "az Azure Key tároló parancssori felület használatával aaaManage |} Microsoft Docs"
description: "Az oktatóanyag tooautomate gyakori feladatok használata a Key Vault hello 2.0 parancssori felület használatával"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 76855c0ea09b6b307159468382a6a63627205556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli-20"></a><span data-ttu-id="abda7-103">Kezelheti a Key Vault 2.0 parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="abda7-103">Manage Key Vault using CLI 2.0</span></span>
<span data-ttu-id="abda7-104">Az Azure Key Vault a legtöbb régióban elérhető.</span><span class="sxs-lookup"><span data-stu-id="abda7-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="abda7-105">További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="abda7-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="abda7-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="abda7-106">Introduction</span></span>
<span data-ttu-id="abda7-107">Használja az oktatóanyag toohelp kap használatába az Azure Key Vault toocreate megerősített tárolókat (kulcstartókat) toostore, az Azure-ban, és kezelheti a titkosítási kulcsok és titkos az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="abda7-107">Use this tutorial toohelp you get started with Azure Key Vault toocreate a hardened container (a vault) in Azure, toostore and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="abda7-108">Az bemutatja, hogyan hello folyamatot, amely segítségével az Azure platformfüggetlen parancssori felületre toocreate egy kulcsot vagy jelszót, majd használható az Azure-alkalmazások tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="abda7-108">It walks you through hello process of using Azure Cross-Platform Command-Line Interface toocreate a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="abda7-109">Ezután bemutatja, hogyan alkalmazás használhatja adott kulcsot vagy jelszót.</span><span class="sxs-lookup"><span data-stu-id="abda7-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="abda7-110">**Becsült idő toocomplete:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="abda7-110">**Estimated time toocomplete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="abda7-111">Ez az oktatóanyag nem tartalmazza a hogyan toowrite hello Azure tartalmazó alkalmazás hello lépések egyikét, amely bemutatja, hogyan tooauthorize egy alkalmazás toouse kulcsok vagy titkos kulcs hello tároló.</span><span class="sxs-lookup"><span data-stu-id="abda7-111">This tutorial does not include instructions on how toowrite hello Azure application that one of hello steps includes, which shows how tooauthorize an application toouse a key or secret in hello key vault.</span></span>
>
> <span data-ttu-id="abda7-112">A oktatóanyag használja az Azure CLI legújabb 2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="abda7-112">This tutorial uses hello latest Azure CLI 2.0.</span></span>
>
>

<span data-ttu-id="abda7-113">Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="abda7-113">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abda7-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="abda7-114">Prerequisites</span></span>
<span data-ttu-id="abda7-115">toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="abda7-115">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="abda7-116">Egy előfizetés tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="abda7-116">A subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="abda7-117">Ha még nem rendelkezik ilyennel, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="abda7-117">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="abda7-118">Parancssori felület 2.0-s vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="abda7-118">Command-Line Interface version 2.0 or later.</span></span> <span data-ttu-id="abda7-119">tooinstall hello legújabb verzióját, és csatlakozzon a tooyour Azure-előfizetéssel, lásd: [telepítése és konfigurálása az Azure platformfüggetlen parancssori felület 2.0 hello](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="abda7-119">tooinstall hello latest version and connect tooyour Azure subscription, see [Install and Configure hello Azure Cross-Platform Command-Line Interface 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="abda7-120">Egy alkalmazás, amely konfigurált toouse hello kulcsot vagy jelszót, amely ebben az oktatóanyagban létrehozhat lesz.</span><span class="sxs-lookup"><span data-stu-id="abda7-120">An application that will be configured toouse hello key or password that you create in this tutorial.</span></span> <span data-ttu-id="abda7-121">A mintaalkalmazás érhető el a hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="abda7-121">A sample application is available from hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="abda7-122">Útmutatásért lásd: hello kísérő információs fájlt.</span><span class="sxs-lookup"><span data-stu-id="abda7-122">For instructions, see hello accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="abda7-123">Segítség az Azure platformfüggetlen parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="abda7-123">Getting help with Azure Cross-Platform Command-Line Interface</span></span>
<span data-ttu-id="abda7-124">Ez az oktatóanyag feltételezi, hogy Ön ismeri a hello parancssori felület (Bash, terminál, parancssor)</span><span class="sxs-lookup"><span data-stu-id="abda7-124">This tutorial assumes that you are familiar with hello command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="abda7-125">hello – súgó vagy -h paraméter lehet tooview súgó használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="abda7-125">hello --help or -h parameter can be used tooview help for specific commands.</span></span> <span data-ttu-id="abda7-126">Alternatív megoldásként hello azure súgó [parancs] [kapcsolók] formátumban is használt tooreturn hello ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="abda7-126">Alternately, hello azure help [command] [options] format can also be used tooreturn hello same information.</span></span> <span data-ttu-id="abda7-127">Például a hello következő parancsokat minden visszatérési hello ugyanazokat az információkat:</span><span class="sxs-lookup"><span data-stu-id="abda7-127">For example, hello following commands all return hello same information:</span></span>

```
az account set --help
az account set -h
```

<span data-ttu-id="abda7-128">A parancs által igényelt hello paraméterekről kétségei vannak, tekintse meg a toohelp használatával – súgó, – h vagy az [parancs] segítségével.</span><span class="sxs-lookup"><span data-stu-id="abda7-128">When in doubt about hello parameters needed by a command, refer toohelp using --help, -h or az help [command].</span></span>

<span data-ttu-id="abda7-129">A következő oktatóanyagok tooget ismeri az Azure Resource Manager az Azure platformfüggetlen parancssori felület hello is olvasható:</span><span class="sxs-lookup"><span data-stu-id="abda7-129">You can also read hello following tutorials tooget familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="abda7-130">Az Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="abda7-130">Install Azure CLI</span></span>](/cli/azure/install-azure-cli)
* [<span data-ttu-id="abda7-131">Ismerkedés az Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="abda7-131">Get started with Azure CLI 2.0</span></span>](/cli/azure/get-started-with-azure-cli)

## <a name="connect-tooyour-subscriptions"></a><span data-ttu-id="abda7-132">Csatlakozás tooyour előfizetések</span><span class="sxs-lookup"><span data-stu-id="abda7-132">Connect tooyour subscriptions</span></span>
<span data-ttu-id="abda7-133">egy szervezeti fiókjával, a következő parancs használata hello toolog:</span><span class="sxs-lookup"><span data-stu-id="abda7-133">toolog in using an organizational account, use hello following command:</span></span>

```
az login -u username@domain.com -p password
```

<span data-ttu-id="abda7-134">vagy ha a kívánt toolog interaktív beírásával</span><span class="sxs-lookup"><span data-stu-id="abda7-134">or if you want toolog in by typing interactively</span></span>

```
az login
```

<span data-ttu-id="abda7-135">Ha több előfizetéssel rendelkezik, és szeretné, hogy egy adott egy toouse toospecify az Azure Key Vaulthoz, írja be a fiókhoz toosee hello előfizetések a következő hello:</span><span class="sxs-lookup"><span data-stu-id="abda7-135">If you have multiple subscriptions and want toospecify a specific one toouse for Azure Key Vault, type hello following toosee hello subscriptions for your account:</span></span>

```
az account list
```

<span data-ttu-id="abda7-136">Ezt követően toospecify hello előfizetés toouse, típus:</span><span class="sxs-lookup"><span data-stu-id="abda7-136">Then, toospecify hello subscription toouse, type:</span></span>

```
az account set --subscription <subscription name or ID>
```

<span data-ttu-id="abda7-137">Azure platformfüggetlen parancssori felületre konfigurálásával kapcsolatos további információkért lásd: [Azure CLI telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="abda7-137">For more information about configuring Azure Cross-Platform Command-Line Interface, see [Install Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-new-resource-group"></a><span data-ttu-id="abda7-138">Új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="abda7-138">Create a new resource group</span></span>
<span data-ttu-id="abda7-139">Azure Resource Manager használatakor minden kapcsolódó erőforrás egy erőforráscsoportban jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="abda7-139">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="abda7-140">Ebben az oktatóanyagban létrehozunk egy új erőforráscsoportot "ContosoResourceGroup".</span><span class="sxs-lookup"><span data-stu-id="abda7-140">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

<span data-ttu-id="abda7-141">hello első paramétere az erőforráscsoport neve, és hello második paramétere hello helyét.</span><span class="sxs-lookup"><span data-stu-id="abda7-141">hello first parameter is resource group name and hello second parameter is hello location.</span></span> <span data-ttu-id="abda7-142">A helyen, paranccsal hello `az account list-locations` tooidentify hogyan toospecify egy másodlagos helyet toohello egy ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="abda7-142">For location, use hello command `az account list-locations` tooidentify how toospecify an alternative location toohello one in this example.</span></span> <span data-ttu-id="abda7-143">Ha további tájékoztatásra van szüksége, írja be: `az account list-locations -h`.</span><span class="sxs-lookup"><span data-stu-id="abda7-143">If you need more information, type: `az account list-locations -h`.</span></span>

## <a name="register-hello-key-vault-resource-provider"></a><span data-ttu-id="abda7-144">Hello Key Vault erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="abda7-144">Register hello Key Vault resource provider</span></span>
<span data-ttu-id="abda7-145">Győződjön meg arról, hogy Key Vault erőforrás-szolgáltató regisztrálva van az előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="abda7-145">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

```
az provider register -n Microsoft.KeyVault
```

<span data-ttu-id="abda7-146">Ez csak egyszer történik előfizetésenként toobe kell.</span><span class="sxs-lookup"><span data-stu-id="abda7-146">This only needs toobe done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="abda7-147">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="abda7-147">Create a key vault</span></span>
<span data-ttu-id="abda7-148">Használjon hello `az keyvault create` parancs toocreate kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="abda7-148">Use hello `az keyvault create` command toocreate a key vault.</span></span> <span data-ttu-id="abda7-149">Ez a parancsfájl három kötelező paraméterrel rendelkezik: erőforráscsoport-nevet, a kulcstároló neve és hello földrajzi helyet.</span><span class="sxs-lookup"><span data-stu-id="abda7-149">This script has three mandatory parameters: a resource group name, a key vault name, and hello geographic location.</span></span>

<span data-ttu-id="abda7-150">Például ha hello tároló neve ContosoKeyVault, ContosoResourceGroup hello erőforráscsoport neve és helye hello Kelet-Ázsia, írja be:</span><span class="sxs-lookup"><span data-stu-id="abda7-150">For example, if you use hello vault name of ContosoKeyVault, hello resource group name of ContosoResourceGroup, and hello location of East Asia, type:</span></span>
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

<span data-ttu-id="abda7-151">a parancs kimenetének hello az újonnan létrehozott kulcstartó hello tulajdonságok láthatók.</span><span class="sxs-lookup"><span data-stu-id="abda7-151">hello output of this command shows properties of hello key vault that you've just created.</span></span> <span data-ttu-id="abda7-152">hello két legfontosabb tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="abda7-152">hello two most important properties are:</span></span>

* <span data-ttu-id="abda7-153">**név**: hello például ez az ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="abda7-153">**name**: In hello example this is ContosoKeyVault.</span></span> <span data-ttu-id="abda7-154">Más Key Vault parancsok ezt a nevet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="abda7-154">You will use this name for other Key Vault commands.</span></span>
* <span data-ttu-id="abda7-155">**vaultUri**: hello például ez az https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="abda7-155">**vaultUri**: In hello example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="abda7-156">A tárolót a REST API-ján keresztül használó alkalmazásoknak ezt az URI-t kell használniuk.</span><span class="sxs-lookup"><span data-stu-id="abda7-156">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="abda7-157">Azure-fiókja most engedélyezett tooperform bármilyen műveletet ezt a kulcsot tároló van.</span><span class="sxs-lookup"><span data-stu-id="abda7-157">Your Azure account is now authorized tooperform any operations on this key vault.</span></span> <span data-ttu-id="abda7-158">Egyelőre senki másnak nincs erre engedélye.</span><span class="sxs-lookup"><span data-stu-id="abda7-158">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-toohello-key-vault"></a><span data-ttu-id="abda7-159">Kulcs vagy titkos toohello kulcstároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="abda7-159">Add a key or secret toohello key vault</span></span>
<span data-ttu-id="abda7-160">Ha meg szeretné az Azure Key Vault toocreate szoftveres védelemmel ellátott kulcs, használja a hello `az key create` parancsot, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="abda7-160">If you want Azure Key Vault toocreate a software-protected key for you, use hello `az key create` command, and type hello following:</span></span>
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
<span data-ttu-id="abda7-161">Azonban ha meglévő kulcs egy helyi fájlba, amelyet az tooupload tooAzure Key Vault softkey.pem nevű fájlba menti a .pem fájl található, írja be a hello tooimport hello kulcsot következő hello. PEM-fájl, hello kulcs védetté hello Key Vault szolgáltatás szoftveres:</span><span class="sxs-lookup"><span data-stu-id="abda7-161">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want tooupload tooAzure Key Vault, type hello following tooimport hello key from hello .PEM file, which protects hello key by software in hello Key Vault service:</span></span>
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
<span data-ttu-id="abda7-162">Hello kulcs létrehozása vagy tooAzure Key Vaultba feltöltött az URI használatával hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="abda7-162">You can now reference hello key that you created or uploaded tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="abda7-163">Használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hello aktuális verzióra, és használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget ezt a verziót.</span><span class="sxs-lookup"><span data-stu-id="abda7-163">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways get hello current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** tooget this specific version.</span></span>

<span data-ttu-id="abda7-164">tooadd egy titkos toohello tárolóban, amely egy SQLPassword nevű jelszót, és Pa$ $w0rd tooAzure kulcstároló, a következő típus hello hello értékkel rendelkezik, amelyek:</span><span class="sxs-lookup"><span data-stu-id="abda7-164">tooadd a secret toohello vault, which is a password named SQLPassword and that has hello value of Pa$$w0rd tooAzure Key Vault, type hello following:</span></span>
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
<span data-ttu-id="abda7-165">Ezt a jelszót, hogy tooAzure Key Vault hozzáadta az URI használatával hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="abda7-165">You can now reference this password that you added tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="abda7-166">Használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hello aktuális verzióra, és használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget ezt a verziót.</span><span class="sxs-lookup"><span data-stu-id="abda7-166">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways get hello current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** tooget this specific version.</span></span>

<span data-ttu-id="abda7-167">Tekintse meg hello kulcsok vagy titkos, újonnan létrehozott:</span><span class="sxs-lookup"><span data-stu-id="abda7-167">Let's view hello key or secret that you just created:</span></span>

* <span data-ttu-id="abda7-168">tooview a, írja be:`az keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="abda7-168">tooview your key, type: `az keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="abda7-169">tooview a titkos, típus:`az keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="abda7-169">tooview your secret, type: `az keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="abda7-170">Alkalmazás regisztrálása az Azure Active Directory szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="abda7-170">Register an application with Azure Active Directory</span></span>
<span data-ttu-id="abda7-171">Ezt a lépést általában egy fejlesztő végzi egy másik számítógépről.</span><span class="sxs-lookup"><span data-stu-id="abda7-171">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="abda7-172">Nincs adott tooAzure Key Vault, de meg adva, feltétlenül teljességében.</span><span class="sxs-lookup"><span data-stu-id="abda7-172">It is not specific tooAzure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abda7-173">toocomplete hello oktatóanyag, a fiók, hello tárolóban, és ebben a lépésben regisztrálandó hello alkalmazás kell lenniük hello Azure könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="abda7-173">toocomplete hello tutorial, your account, hello vault, and hello application that you will register in this step must all be in hello same Azure directory.</span></span>
>
>

<span data-ttu-id="abda7-174">A kulcstartót használó alkalmazásoknak az Azure Active Directoryból származó jogkivonat használatával kell hitelesítést végezniük.</span><span class="sxs-lookup"><span data-stu-id="abda7-174">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="abda7-175">toodo a, hello hello alkalmazás tulajdonosának először regisztrálnia kell a hello alkalmazás az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="abda7-175">toodo this, hello owner of hello application must first register hello application in their Azure Active Directory.</span></span> <span data-ttu-id="abda7-176">A regisztrációs hello végén hello alkalmazás tulajdonosa lekérdezi a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="abda7-176">At hello end of registration, hello application owner gets hello following values:</span></span>

* <span data-ttu-id="abda7-177">Egy **Alkalmazásazonosító** (más néven Ügyfélazonosítót) és **hitelesítési kulcs** (más néven hello közös titkos kulcs).</span><span class="sxs-lookup"><span data-stu-id="abda7-177">An **Application ID** (also known as a Client ID) and **authentication key** (also known as hello shared secret).</span></span> <span data-ttu-id="abda7-178">hello alkalmazás e ezen értékek tooAzure Active Directory, a jogkivonat tooget mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="abda7-178">hello application must present both of these values tooAzure Active Directory, tooget a token.</span></span> <span data-ttu-id="abda7-179">Ez függ hello alkalmazás toodo hogyan hello alkalmazás van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="abda7-179">How hello application is configured toodo this depends on hello application.</span></span> <span data-ttu-id="abda7-180">A Key Vault mintaalkalmazás hello hello alkalmazás tulajdonosa adja meg ezeket az értékeket hello app.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="abda7-180">For hello Key Vault sample application, hello application owner sets these values in hello app.config file.</span></span>

<span data-ttu-id="abda7-181">tooregister hello alkalmazás Azure Active Directoryban:</span><span class="sxs-lookup"><span data-stu-id="abda7-181">tooregister hello application in Azure Active Directory:</span></span>

1. <span data-ttu-id="abda7-182">Bejelentkezés toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="abda7-182">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="abda7-183">Hello bal oldalon kattintson **Azure Active Directory**, majd válassza ki az alkalmazást regisztrálni hello directory.</span><span class="sxs-lookup"><span data-stu-id="abda7-183">On hello left, click **Azure Active Directory**, and then select hello directory in which you will register your application.</span></span> <br> <br> 

> [!Note] 
> <span data-ttu-id="abda7-184">Ki kell választania a hello ugyanabban a könyvtárban, amely tartalmazza a kulcstartót létrehozó Azure-előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="abda7-184">You must select hello same directory that contains hello Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="abda7-185">Ha nem tudja, melyik címtárban ez, kattintson a **beállítások**, a kulcstartót létrehozó hello előfizetés azonosítása és hello utolsó oszlopban megjelenített megjegyzés hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="abda7-185">If you do not know which directory this is, click **Settings**, identify hello subscription with which you created your key vault, and note hello name of hello directory displayed in hello last column.</span></span>

3. <span data-ttu-id="abda7-186">Kattintson az **APPLICATIONS** (ALKALMAZÁSOK) elemre.</span><span class="sxs-lookup"><span data-stu-id="abda7-186">Click **APPLICATIONS**.</span></span> <span data-ttu-id="abda7-187">Nincs alkalmazás tooyour directory lettek hozzáadva, ha ezen a lapon jelenjen-e csak hello **hozzáadhat egy alkalmazást** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="abda7-187">If no apps have been added tooyour directory, this page will show only hello **Add an App** link.</span></span> <span data-ttu-id="abda7-188">Hello hivatkozásra, vagy másik lehetőségként kattinthat hello **hozzáadása** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="abda7-188">Click hello link, or alternatively, you can click hello **ADD** on hello command bar.</span></span>
4. <span data-ttu-id="abda7-189">A hello **alkalmazás hozzáadása** hello varázsló **miről szeretne toodo?** lapján kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="abda7-189">In hello **ADD APPLICATION** wizard, on hello **What do you want toodo?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="abda7-190">A hello **adja meg azt az alkalmazás** lapon adja meg az alkalmazás nevét, és válassza **WEB APPLICATION AND/OR WEB API** (hello alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="abda7-190">On hello **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (hello default).</span></span> <span data-ttu-id="abda7-191">Kattintson a Tovább hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="abda7-191">Click hello Next icon.</span></span>
6. <span data-ttu-id="abda7-192">A hello **alkalmazás tulajdonságainak** adja meg azokat a hello **SIGN-ON URL** és **APP ID URI** a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="abda7-192">On hello **App properties** page, specify hello **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="abda7-193">Ha az alkalmazás nem rendelkezik ezekkel az értékekkel, ehhez a lépéshez nem létező értékeket is megadhat (például http://test1.contoso.com mindkét mezőhöz).</span><span class="sxs-lookup"><span data-stu-id="abda7-193">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="abda7-194">Nem számít, ha ezeken a webhelyeken létezik; a fontos, hogy hello app ID URI mindegyik alkalmazáshoz különböző alkalmazás esetében minden egyes a címtárban.</span><span class="sxs-lookup"><span data-stu-id="abda7-194">It does not matter if these sites exist; what is important is that hello app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="abda7-195">hello directory használja a karakterlánc tooidentify az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="abda7-195">hello directory uses this string tooidentify your app.</span></span>
7. <span data-ttu-id="abda7-196">Kattintson a hello teljes ikon toosave hello varázslóban a módosítások.</span><span class="sxs-lookup"><span data-stu-id="abda7-196">Click hello Complete icon toosave your changes in hello wizard.</span></span>
8. <span data-ttu-id="abda7-197">Hello gyors kezdés lapon kattintson **KONFIGURÁLÁSA**.</span><span class="sxs-lookup"><span data-stu-id="abda7-197">On hello Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="abda7-198">Görgessen toohello **kulcsok** szakasz, válassza ki a hello időtartama, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="abda7-198">Scroll toohello **keys** section, select hello duration, and then click **SAVE**.</span></span> <span data-ttu-id="abda7-199">hello oldal frissül, és most kijelzi a kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="abda7-199">hello page refreshes and now shows a key value.</span></span> <span data-ttu-id="abda7-200">Be kell állítania az alkalmazás a kulcs értékét, és a hello **ügyfél-azonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="abda7-200">You must configure your application with this key value and hello **CLIENT ID** value.</span></span> <span data-ttu-id="abda7-201">(A beállítás menete alkalmazásonként eltérő lehet.)</span><span class="sxs-lookup"><span data-stu-id="abda7-201">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="abda7-202">Ezen a lapon, mert ezzel fogja a következő lépés tooset hello engedélyei a tároló hello Ügyfélazonosító értékét másolja.</span><span class="sxs-lookup"><span data-stu-id="abda7-202">Copy hello client ID value from this page, which you will use in hello next step tooset permissions on your vault.</span></span>

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a><span data-ttu-id="abda7-203">Hello alkalmazás toouse hello kulcs vagy titkos kulcs engedélyezése</span><span class="sxs-lookup"><span data-stu-id="abda7-203">Authorize hello application toouse hello key or secret</span></span>
<span data-ttu-id="abda7-204">tooauthorize hello alkalmazás tooaccess hello kulcsok vagy titkos hello tárolóban, használjon hello `az keyvault set-policy` parancsot.</span><span class="sxs-lookup"><span data-stu-id="abda7-204">tooauthorize hello application tooaccess hello key or secret in hello vault, use hello `az keyvault set-policy` command.</span></span>

<span data-ttu-id="abda7-205">Például ha a tároló neve tooauthorize Ügyfélazonosítója 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, és ezen tooauthorize hello alkalmazás toodecrypt és bejelentkezési kulccsal rendelkező a tárolóban lévő kívánt ContosoKeyVault és hello alkalmazás, majd futtassa hello a következő:</span><span class="sxs-lookup"><span data-stu-id="abda7-205">For example, if your vault name is ContosoKeyVault and hello application you want tooauthorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want tooauthorize hello application toodecrypt and sign with keys in your vault, then run hello following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

<span data-ttu-id="abda7-206">Ha azt szeretné tooauthorize, hogy ugyanazon alkalmazás tooread titkokat a tárolóban, futtassa a hello következő:</span><span class="sxs-lookup"><span data-stu-id="abda7-206">If you want tooauthorize that same application tooread secrets in your vault, run hello following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a><span data-ttu-id="abda7-207">Ha azt szeretné, hogy toouse hardveres biztonsági modul (HSM)</span><span class="sxs-lookup"><span data-stu-id="abda7-207">If you want toouse a hardware security module (HSM)</span></span>
<span data-ttu-id="abda7-208">A nagyobb importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="abda7-208">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="abda7-209">hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="abda7-209">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="abda7-210">Ha ez a követelmény nem érvényes tooyou, hagyja ki ezt a szakaszt, és folytassa túl[hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="abda7-210">If this requirement doesn't apply tooyou, skip this section and go too[Delete hello key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="abda7-211">toocreate a HSM által védett kulcsok, a tároló-előfizetéssel kell rendelkeznie, amely támogatja a HSM által védett kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="abda7-211">toocreate these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="abda7-212">Hello keyvault létrehozásakor adja hozzá a hello "sku" paramétert:</span><span class="sxs-lookup"><span data-stu-id="abda7-212">When you create hello keyvault, add hello 'sku' parameter:</span></span>

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
<span data-ttu-id="abda7-213">Is hozzáadhat szoftveresen védett (korábban bemutatott), és HSM által védett kulcsokat toothis tároló.</span><span class="sxs-lookup"><span data-stu-id="abda7-213">You can add software-protected keys (as shown earlier) and HSM-protected keys toothis vault.</span></span> <span data-ttu-id="abda7-214">toocreate HSM által védett kulcs set hello cél paraméter too'HSM ":</span><span class="sxs-lookup"><span data-stu-id="abda7-214">toocreate an HSM-protected key, set hello Destination parameter too'HSM':</span></span>

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

<span data-ttu-id="abda7-215">A .pem fájlt a számítógépen a parancs tooimport egy kulcsot következő hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="abda7-215">You can use hello following command tooimport a key from a .pem file on your computer.</span></span> <span data-ttu-id="abda7-216">Ez a parancs hello kulcs importálja a Key Vault szolgáltatás hello a HSM-EK:</span><span class="sxs-lookup"><span data-stu-id="abda7-216">This command imports hello key into HSMs in hello Key Vault service:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
<span data-ttu-id="abda7-217">hello a következő paranccsal importálhatók, egy "állapotba hozása a saját kulcs" (BYOK) csomagot.</span><span class="sxs-lookup"><span data-stu-id="abda7-217">hello next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="abda7-218">Ez lehetővé teszi, hogy a kulcs létrehozása a helyi HSM-ben, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs továbbítható:</span><span class="sxs-lookup"><span data-stu-id="abda7-218">This lets you generate your key in your local HSM, and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
<span data-ttu-id="abda7-219">Leírja, hogyan részletes toogenerate a BYOK-csomag, lásd: [hogyan toouse HSM-Protected kulcsokat az Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="abda7-219">For more detailed instructions about how toogenerate this BYOK package, see [How toouse HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="abda7-220">Hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése</span><span class="sxs-lookup"><span data-stu-id="abda7-220">Delete hello key vault and associated keys and secrets</span></span>
<span data-ttu-id="abda7-221">Ha már nincs szüksége hello kulcstartó és hello kulcs vagy titkos kódra, törölheti hello segítségével hello kulcstároló `az keyvault delete` parancs:</span><span class="sxs-lookup"><span data-stu-id="abda7-221">If you no longer need hello key vault and hello key or secret that it contains, you can delete hello key vault by using hello `az keyvault delete` command:</span></span>

```
az keyvault delete --name 'ContosoKeyVault'
```

<span data-ttu-id="abda7-222">Vagy egy teljes Azure erőforráscsoport, ide tartozik az hello kulcstartó és abba a csoportba más erőforrások törlése:</span><span class="sxs-lookup"><span data-stu-id="abda7-222">Or, you can delete an entire Azure resource group, which includes hello key vault and any other resources that you included in that group:</span></span>

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="abda7-223">Egyéb Azure platformfüggetlen parancssori felület parancsai</span><span class="sxs-lookup"><span data-stu-id="abda7-223">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="abda7-224">Egyéb parancsok, akkor lehet hasznos, ha az Azure Key Vault kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="abda7-224">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="abda7-225">Ez a parancs táblázatos formában jeleníti meg az összes kulcsot és a kijelölt tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="abda7-225">This command lists a tabular display of all keys and selected properties:</span></span>

<span data-ttu-id="abda7-226">az keyvault kulcslista--tároló-neve "ContosoKeyVault"</span><span class="sxs-lookup"><span data-stu-id="abda7-226">az keyvault key list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="abda7-227">Ez a parancs hello megadott kulcs tulajdonságainak teljes listáját jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="abda7-227">This command displays a full list of properties for hello specified key:</span></span>

<span data-ttu-id="abda7-228">az keyvault kulcs megjelenítése--tároló-neve "ContosoKeyVault"--"ContosoFirstKey" neve</span><span class="sxs-lookup"><span data-stu-id="abda7-228">az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="abda7-229">Ez a parancs táblázatos formában jeleníti meg az összes titkos kód nevét és a kijelölt tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="abda7-229">This command lists a tabular display of all secret names and selected properties:</span></span>

<span data-ttu-id="abda7-230">az keyvault titkos lista--tároló-neve "ContosoKeyVault"</span><span class="sxs-lookup"><span data-stu-id="abda7-230">az keyvault secret list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="abda7-231">Íme egy példa bemutatja, hogyan tooremove egy adott kulcs:</span><span class="sxs-lookup"><span data-stu-id="abda7-231">Here's an example of how tooremove a specific key:</span></span>

<span data-ttu-id="abda7-232">az keyvault kulcs törlése--tároló-neve "ContosoKeyVault"--"ContosoFirstKey" neve</span><span class="sxs-lookup"><span data-stu-id="abda7-232">az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="abda7-233">Íme egy példa bemutatja, hogyan tooremove egy adott titkos kód:</span><span class="sxs-lookup"><span data-stu-id="abda7-233">Here's an example of how tooremove a specific secret:</span></span>

<span data-ttu-id="abda7-234">az keyvault titkos kulcs törlése--tároló-neve "ContosoKeyVault"--"SQLPassword" neve</span><span class="sxs-lookup"><span data-stu-id="abda7-234">az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'</span></span>


## <a name="next-steps"></a><span data-ttu-id="abda7-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="abda7-235">Next steps</span></span>
<span data-ttu-id="abda7-236">A kulcstároló parancsok teljes Azure parancssori felület referenciáért lásd: [Key Vault CLI-hivatkozás](/cli/azure/keyvault)</span><span class="sxs-lookup"><span data-stu-id="abda7-236">For complete Azure CLI reference for key vault commands, see [Key Vault CLI reference](/cli/azure/keyvault)</span></span>

<span data-ttu-id="abda7-237">Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="abda7-237">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>
