---
title: "az Azure Key tároló parancssori felület használatával aaaManage |} Microsoft Docs"
description: "Az oktatóanyag tooautomate gyakori feladatok használata a Key Vault hello parancssori felület használatával"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: 9ef506faa67e1f0db5b9e303300d63b135ddd7b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli"></a><span data-ttu-id="aaf8e-103">Kezelheti a Key Vault parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="aaf8e-103">Manage Key Vault using CLI</span></span>

<span data-ttu-id="aaf8e-104">Az Azure Key Vault a legtöbb régióban elérhető.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="aaf8e-105">További információkért lásd: hello [Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="aaf8e-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="aaf8e-106">Introduction</span></span>

<span data-ttu-id="aaf8e-107">Használja az oktatóanyag toohelp kap használatába az Azure Key Vault toocreate megerősített tárolókat (kulcstartókat) toostore, az Azure-ban, és kezelheti a titkosítási kulcsok és titkos az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-107">Use this tutorial toohelp you get started with Azure Key Vault toocreate a hardened container (a vault) in Azure, toostore and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="aaf8e-108">Az bemutatja, hogyan hello folyamatot, amely segítségével az Azure platformfüggetlen parancssori felületre toocreate egy kulcsot vagy jelszót, majd használható az Azure-alkalmazások tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-108">It walks you through hello process of using Azure Cross-Platform Command-Line Interface toocreate a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="aaf8e-109">Ezután bemutatja, hogyan alkalmazás használhatja adott kulcsot vagy jelszót.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="aaf8e-110">**Becsült idő toocomplete:** 20 perc</span><span class="sxs-lookup"><span data-stu-id="aaf8e-110">**Estimated time toocomplete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="aaf8e-111">Ez az oktatóanyag nem tartalmazza a hogyan toowrite hello Azure tartalmazó alkalmazás hello lépések egyikét, amely bemutatja, hogyan tooauthorize egy alkalmazás toouse kulcsok vagy titkos kulcs hello tároló.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-111">This tutorial does not include instructions on how toowrite hello Azure application that one of hello steps includes, which shows how tooauthorize an application toouse a key or secret in hello key vault.</span></span>
> 
> <span data-ttu-id="aaf8e-112">Jelenleg nem konfigurálhatja az Azure Key Vault hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-112">Currently, you cannot configure Azure Key Vault in hello Azure portal.</span></span> <span data-ttu-id="aaf8e-113">Ehelyett használja a platformfüggetlen parancssori felületre útmutatóhoz.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-113">Instead, use these Cross-Platform Command-Line Interface  instructions.</span></span> <span data-ttu-id="aaf8e-114">Vagy az Azure PowerShell útmutatásért lásd: [ebben az egyenértékű oktatóanyagban](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-114">Or, for Azure PowerShell instructions, see [this equivalent tutorial](key-vault-get-started.md).</span></span>
> 
> 

<span data-ttu-id="aaf8e-115">Áttekintést az Azure Key Vaultról a [What is Azure Key Vault?](key-vault-whatis.md) (Mi az Azure Key Vault?) című cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-115">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aaf8e-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aaf8e-116">Prerequisites</span></span>

<span data-ttu-id="aaf8e-117">toocomplete ebben az oktatóanyagban rendelkeznie kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-117">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="aaf8e-118">Egy előfizetés tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-118">A subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="aaf8e-119">Ha még nem rendelkezik ilyennel, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-119">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="aaf8e-120">Parancssori felület 0.9.1 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-120">Command-Line Interface version 0.9.1 or later.</span></span> <span data-ttu-id="aaf8e-121">tooinstall hello legújabb verzióját, és csatlakozzon a tooyour Azure-előfizetéssel, lásd: [telepítése és konfigurálása az Azure platformfüggetlen parancssori felületre hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-121">tooinstall hello latest version and connect tooyour Azure subscription, see [Install and Configure hello Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="aaf8e-122">Egy alkalmazás, amely konfigurált toouse hello kulcsot vagy jelszót, amely ebben az oktatóanyagban létrehozhat lesz.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-122">An application that will be configured toouse hello key or password that you create in this tutorial.</span></span> <span data-ttu-id="aaf8e-123">A mintaalkalmazás érhető el a hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-123">A sample application is available from hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="aaf8e-124">Útmutatásért lásd: hello kísérő információs fájlt.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-124">For instructions, see hello accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="aaf8e-125">Segítség az Azure platformfüggetlen parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="aaf8e-125">Getting help with Azure Cross-Platform Command-Line Interface</span></span>

<span data-ttu-id="aaf8e-126">Ez az oktatóanyag feltételezi, hogy Ön ismeri a hello parancssori felület (Bash, terminál, parancssor)</span><span class="sxs-lookup"><span data-stu-id="aaf8e-126">This tutorial assumes that you are familiar with hello command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="aaf8e-127">hello – súgó vagy -h paraméter lehet tooview súgó használt parancsok.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-127">hello --help or -h parameter can be used tooview help for specific commands.</span></span> <span data-ttu-id="aaf8e-128">Alternatív megoldásként hello azure súgó [parancs] [kapcsolók] formátumban is használt tooreturn hello ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-128">Alternately, hello azure help [command] [options] format can also be used tooreturn hello same information.</span></span> <span data-ttu-id="aaf8e-129">Például a hello következő parancsokat minden visszatérési hello ugyanazokat az információkat:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-129">For example, hello following commands all return hello same information:</span></span>

    azure account set --help

    azure account set -h

    azure help account set

<span data-ttu-id="aaf8e-130">A parancs által igényelt hello paraméterekről kétségei vannak, tekintse meg a toohelp használatával – súgó, -h vagy azure [parancs].</span><span class="sxs-lookup"><span data-stu-id="aaf8e-130">When in doubt about hello parameters needed by a command, refer toohelp using --help, -h or azure help [command].</span></span>

<span data-ttu-id="aaf8e-131">A következő oktatóanyagok tooget ismeri az Azure Resource Manager az Azure platformfüggetlen parancssori felület hello is olvasható:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-131">You can also read hello following tutorials tooget familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="aaf8e-132">Hogyan tooinstall és konfigurálása az Azure platformfüggetlen parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="aaf8e-132">How tooinstall and configure Azure Cross-Platform Command Line Interface</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="aaf8e-133">Az Azure platformfüggetlen parancssori felületre az Azure erőforrás-kezelő használatával</span><span class="sxs-lookup"><span data-stu-id="aaf8e-133">Using Azure Cross-Platform Command-Line Interface with Azure Resource Manager</span></span>](../xplat-cli-azure-resource-manager.md)

## <a name="connect-tooyour-subscriptions"></a><span data-ttu-id="aaf8e-134">Csatlakozás tooyour előfizetések</span><span class="sxs-lookup"><span data-stu-id="aaf8e-134">Connect tooyour subscriptions</span></span>

<span data-ttu-id="aaf8e-135">egy szervezeti fiókjával, a következő parancs használata hello toolog:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-135">toolog in using an organizational account, use hello following command:</span></span>

    azure login -u username -p password

<span data-ttu-id="aaf8e-136">vagy ha a kívánt toolog interaktív beírásával</span><span class="sxs-lookup"><span data-stu-id="aaf8e-136">or if you want toolog in by typing interactively</span></span>

    azure login

> [!NOTE]
> <span data-ttu-id="aaf8e-137">hello bejelentkezési metódus csak olyan szervezeti fiók működik.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-137">hello login method only works with organizational account.</span></span> <span data-ttu-id="aaf8e-138">Egy szervezeti fiók egy olyan felhasználó, a szervezet által felügyelt, és a szervezet Azure Active Directory-bérlő definiálva.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-138">An organizational account is a user that is managed by your organization, and defined in your organization's Azure Active Directory tenant.</span></span>
> 
> 

<span data-ttu-id="aaf8e-139">Ha jelenleg nem rendelkezik a szervezeti fiókkal, és tooyour Azure-előfizetés a Microsoft-fiók toolog használnak, egyszerűen létrehozhat, egyet az hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-139">If you do not currently have an organizational account, and are using a Microsoft account toolog in tooyour Azure subscription, you can easily create one using hello following steps.</span></span>

1. <span data-ttu-id="aaf8e-140">Bejelentkezési toohello bejelentkezési toohello [Azure felügyeleti portálon](https://manage.windowsazure.com/), majd kattintson az Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-140">Login toohello Login toohello [Azure Management Portal](https://manage.windowsazure.com/), and click on Active Directory.</span></span>
2. <span data-ttu-id="aaf8e-141">Ha nem a könyvtár létezik, válassza ki a könyvtár létrehozása, és hello adja meg a kért információkat.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-141">If no directory exists, select Create your directory and provide hello requested information.</span></span>
3. <span data-ttu-id="aaf8e-142">Válassza ki azt a címtárat, és új felhasználó hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-142">Select your directory and add a new user.</span></span> <span data-ttu-id="aaf8e-143">Ennek a felhasználónak egy szervezeti fiók.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-143">This new user is an organizational account.</span></span> <span data-ttu-id="aaf8e-144">Hello felhasználó hello létrehozásakor fog megadott hello felhasználó e-mail címének és ideiglenes jelszót.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-144">During hello creation of hello user, you will be supplied with both an e-mail address for hello user and a temporary password.</span></span> <span data-ttu-id="aaf8e-145">Ez az információ akkor menteni, mert használatban van egy másik lépés.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-145">Save this information as it is used in another step.</span></span>
4. <span data-ttu-id="aaf8e-146">Hello portálról jelölje be a beállításokat, és válassza a rendszergazdák.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-146">From hello portal, select Settings and then select Administrators.</span></span> <span data-ttu-id="aaf8e-147">Válassza ki a hozzáadása, és adja hozzá az új felhasználó hello társadminisztrátoraként.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-147">Select Add, and add hello new user as a co-administrator.</span></span> <span data-ttu-id="aaf8e-148">Ez lehetővé teszi, hogy hello szervezeti fiók toomanage az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-148">This allows hello organizational account toomanage your Azure subscription.</span></span>
5. <span data-ttu-id="aaf8e-149">Végezetül kijelentkezés hello Azure-portálon, és jelentkezzen be oda hello új szervezeti fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-149">Finally, log out of hello Azure portal and then log back in using hello new organizational account.</span></span> <span data-ttu-id="aaf8e-150">Ha ez a fiók első alkalommal bejelentkezik hello, felszólító toochange hello jelszó fogja.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-150">If this is hello first time logging in with this account, you will be prompted toochange hello password.</span></span>

<span data-ttu-id="aaf8e-151">A Microsoft Azure rendelkező szervezeti fiókkal kapcsolatos további információkért lásd: [iratkozzon fel a Microsoft Azure-előfizetésre](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-151">For more information about using an organizational account with Microsoft Azure, see [Sign up for Microsoft Azure as an Organization](../active-directory/sign-up-organization.md).</span></span>

<span data-ttu-id="aaf8e-152">Ha több előfizetéssel rendelkezik, és szeretné, hogy egy adott egy toouse toospecify az Azure Key Vaulthoz, írja be a fiókhoz toosee hello előfizetések a következő hello:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-152">If you have multiple subscriptions and want toospecify a specific one toouse for Azure Key Vault, type hello following toosee hello subscriptions for your account:</span></span>

    azure account list

<span data-ttu-id="aaf8e-153">Ezt követően toospecify hello előfizetés toouse, típus:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-153">Then, toospecify hello subscription toouse, type:</span></span>

    azure account set <subscription name>

<span data-ttu-id="aaf8e-154">Azure platformfüggetlen parancssori felületre konfigurálásával kapcsolatos további információkért lásd: [hogyan tooInstall és konfigurálása Azure platformfüggetlen parancssori felület](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-154">For more information about configuring Azure Cross-Platform Command-Line Interface, see [How tooInstall and Configure Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>

## <a name="switch-toousing-azure-resource-manager"></a><span data-ttu-id="aaf8e-155">Váltson Azure Resource Manager toousing</span><span class="sxs-lookup"><span data-stu-id="aaf8e-155">Switch toousing Azure Resource Manager</span></span>
<span data-ttu-id="aaf8e-156">Key Vault hello Azure Resource Manager megköveteli, tehát hello tooswitch tooAzure Resource Manager módra a következő:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-156">hello Key Vault requires Azure Resource Manager, so type hello following tooswitch tooAzure Resource Manager mode:</span></span>

    azure config mode arm

## <a name="create-a-new-resource-group"></a><span data-ttu-id="aaf8e-157">Új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaf8e-157">Create a new resource group</span></span>
<span data-ttu-id="aaf8e-158">Azure Resource Manager használatakor minden kapcsolódó erőforrás egy erőforráscsoportban jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-158">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="aaf8e-159">Ebben az oktatóanyagban létrehozunk egy új erőforráscsoportot "ContosoResourceGroup".</span><span class="sxs-lookup"><span data-stu-id="aaf8e-159">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

    azure group create 'ContosoResourceGroup' 'East Asia'

<span data-ttu-id="aaf8e-160">hello első paramétere az erőforráscsoport neve, és hello második paramétere hello helyét.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-160">hello first parameter is resource group name and hello second parameter is hello location.</span></span> <span data-ttu-id="aaf8e-161">A helyen, paranccsal hello `azure location list` tooidentify hogyan toospecify egy másodlagos helyet toohello egy ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-161">For location, use hello command `azure location list` tooidentify how toospecify an alternative location toohello one in this example.</span></span> <span data-ttu-id="aaf8e-162">Ha további tájékoztatásra van szüksége, írja be:`azure help location`</span><span class="sxs-lookup"><span data-stu-id="aaf8e-162">If you need more information, type: `azure help location`</span></span>

## <a name="register-hello-key-vault-resource-provider"></a><span data-ttu-id="aaf8e-163">Hello Key Vault erőforrás-szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="aaf8e-163">Register hello Key Vault resource provider</span></span>
<span data-ttu-id="aaf8e-164">Győződjön meg arról, hogy Key Vault erőforrás-szolgáltató regisztrálva van az előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-164">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

`azure provider register Microsoft.KeyVault`

<span data-ttu-id="aaf8e-165">Ez csak egyszer történik előfizetésenként toobe kell.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-165">This only needs toobe done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="aaf8e-166">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaf8e-166">Create a key vault</span></span>

<span data-ttu-id="aaf8e-167">Használjon hello `azure keyvault create` parancs toocreate kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-167">Use hello `azure keyvault create` command toocreate a key vault.</span></span> <span data-ttu-id="aaf8e-168">Ez a parancsfájl három kötelező paraméterrel rendelkezik: erőforráscsoport-nevet, a kulcstároló neve és hello földrajzi helyet.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-168">This script has three mandatory parameters: a resource group name, a key vault name, and hello geographic location.</span></span>

<span data-ttu-id="aaf8e-169">Például ha hello tároló neve ContosoKeyVault, ContosoResourceGroup hello erőforráscsoport neve és helye hello Kelet-Ázsia, írja be:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-169">For example, if you use hello vault name of ContosoKeyVault, hello resource group name of ContosoResourceGroup, and hello location of East Asia, type:</span></span>

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

<span data-ttu-id="aaf8e-170">a parancs kimenetének hello az újonnan létrehozott kulcstartó hello tulajdonságok láthatók.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-170">hello output of this command shows properties of hello key vault that you've just created.</span></span> <span data-ttu-id="aaf8e-171">hello két legfontosabb tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-171">hello two most important properties are:</span></span>

* <span data-ttu-id="aaf8e-172">**Név**: hello például ez az ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-172">**Name**: In hello example this is ContosoKeyVault.</span></span> <span data-ttu-id="aaf8e-173">Ezt a nevet fogja majd más Key Vault parancsmagokban is megadni.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-173">You will use this name for other Key Vault cmdlets.</span></span>
* <span data-ttu-id="aaf8e-174">**vaultUri**: hello például ez az https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-174">**vaultUri**: In hello example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="aaf8e-175">A tárolót a REST API-ján keresztül használó alkalmazásoknak ezt az URI-t kell használniuk.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-175">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="aaf8e-176">Azure-fiókja most engedélyezett tooperform bármilyen műveletet ezt a kulcsot tároló van.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-176">Your Azure account is now authorized tooperform any operations on this key vault.</span></span> <span data-ttu-id="aaf8e-177">Egyelőre senki másnak nincs erre engedélye.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-177">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-toohello-key-vault"></a><span data-ttu-id="aaf8e-178">Kulcs vagy titkos toohello kulcstároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aaf8e-178">Add a key or secret toohello key vault</span></span>

<span data-ttu-id="aaf8e-179">Ha meg szeretné az Azure Key Vault toocreate szoftveres védelemmel ellátott kulcs, használja a hello `azure key create` parancsot, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-179">If you want Azure Key Vault toocreate a software-protected key for you, use hello `azure key create` command, and type hello following:</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

<span data-ttu-id="aaf8e-180">Azonban ha meglévő kulcs egy helyi fájlba, amelyet az tooupload tooAzure Key Vault softkey.pem nevű fájlba menti a .pem fájl található, írja be a hello tooimport hello kulcsot következő hello. PEM-fájl, hello kulcs védetté hello Key Vault szolgáltatás szoftveres:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-180">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want tooupload tooAzure Key Vault, type hello following tooimport hello key from hello .PEM file, which protects hello key by software in hello Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

<span data-ttu-id="aaf8e-181">Hello kulcs létrehozása vagy tooAzure Key Vaultba feltöltött az URI használatával hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-181">You can now reference hello key that you created or uploaded tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="aaf8e-182">Használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hello aktuális verzióra, és használjon **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget ezt a verziót.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-182">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways get hello current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** tooget this specific version.</span></span>

<span data-ttu-id="aaf8e-183">tooadd egy titkos toohello tárolóban, amely egy SQLPassword nevű jelszót, és Pa$ $w0rd tooAzure kulcstároló, a következő típus hello hello értékkel rendelkezik, amelyek:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-183">tooadd a secret toohello vault, which is a password named SQLPassword and that has hello value of Pa$$w0rd tooAzure Key Vault, type hello following:</span></span>

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

<span data-ttu-id="aaf8e-184">Ezt a jelszót, hogy tooAzure Key Vault hozzáadta az URI használatával hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-184">You can now reference this password that you added tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="aaf8e-185">Használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hello aktuális verzióra, és használjon **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget ezt a verziót.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-185">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways get hello current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** tooget this specific version.</span></span>

<span data-ttu-id="aaf8e-186">Tekintse meg hello kulcsok vagy titkos, újonnan létrehozott:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-186">Let's view hello key or secret that you just created:</span></span>

* <span data-ttu-id="aaf8e-187">tooview a, írja be:`azure keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="aaf8e-187">tooview your key, type: `azure keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="aaf8e-188">tooview a titkos, típus:`azure keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="aaf8e-188">tooview your secret, type: `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="aaf8e-189">Alkalmazás regisztrálása az Azure Active Directory szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="aaf8e-189">Register an application with Azure Active Directory</span></span>

<span data-ttu-id="aaf8e-190">Ezt a lépést általában egy fejlesztő végzi egy másik számítógépről.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-190">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="aaf8e-191">Nincs adott tooAzure Key Vault, de meg adva, feltétlenül teljességében.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-191">It is not specific tooAzure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aaf8e-192">toocomplete hello oktatóanyag, a fiók, hello tárolóban, és ebben a lépésben regisztrálandó hello alkalmazás kell lenniük hello Azure könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-192">toocomplete hello tutorial, your account, hello vault, and hello application that you will register in this step must all be in hello same Azure directory.</span></span>
> 
> 

<span data-ttu-id="aaf8e-193">A kulcstartót használó alkalmazásoknak az Azure Active Directoryból származó jogkivonat használatával kell hitelesítést végezniük.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-193">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="aaf8e-194">toodo a, hello hello alkalmazás tulajdonosának először regisztrálnia kell a hello alkalmazás az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-194">toodo this, hello owner of hello application must first register hello application in their Azure Active Directory.</span></span> <span data-ttu-id="aaf8e-195">A regisztrációs hello végén hello alkalmazás tulajdonosa lekérdezi a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-195">At hello end of registration, hello application owner gets hello following values:</span></span>

* <span data-ttu-id="aaf8e-196">Egy **Alkalmazásazonosító** (más néven Ügyfélazonosítót) és **hitelesítési kulcs** (más néven hello közös titkos kulcs).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-196">An **Application ID** (also known as a Client ID) and **authentication key** (also known as hello shared secret).</span></span> <span data-ttu-id="aaf8e-197">hello alkalmazás e ezen értékek tooAzure Active Directory, a jogkivonat tooget mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-197">hello application must present both of these values tooAzure Active Directory, tooget a token.</span></span> <span data-ttu-id="aaf8e-198">Ez függ hello alkalmazás toodo hogyan hello alkalmazás van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-198">How hello application is configured toodo this depends on hello application.</span></span> <span data-ttu-id="aaf8e-199">A Key Vault mintaalkalmazás hello hello alkalmazás tulajdonosa adja meg ezeket az értékeket hello app.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-199">For hello Key Vault sample application, hello application owner sets these values in hello app.config file.</span></span>

<span data-ttu-id="aaf8e-200">tooregister hello alkalmazás Azure Active Directoryban:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-200">tooregister hello application in Azure Active Directory:</span></span>

1. <span data-ttu-id="aaf8e-201">Bejelentkezés toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-201">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="aaf8e-202">Hello bal oldalon kattintson **Active Directory**, majd válassza ki az alkalmazást regisztrálni hello directory.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-202">On hello left, click **Active Directory**, and then select hello directory in which you will register your application.</span></span> <br> <br> 

>[!NOTE] 
> <span data-ttu-id="aaf8e-203">Ki kell választania a hello ugyanabban a könyvtárban, amely tartalmazza a kulcstartót létrehozó Azure-előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-203">You must select hello same directory that contains hello Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="aaf8e-204">Ha nem tudja, melyik címtárban ez, kattintson a **beállítások**, a kulcstartót létrehozó hello előfizetés azonosítása és hello utolsó oszlopban megjelenített megjegyzés hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-204">If you do not know which directory this is, click **Settings**, identify hello subscription with which you created your key vault, and note hello name of hello directory displayed in hello last column.</span></span>

3. <span data-ttu-id="aaf8e-205">Kattintson az **APPLICATIONS** (ALKALMAZÁSOK) elemre.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-205">Click **APPLICATIONS**.</span></span> <span data-ttu-id="aaf8e-206">Nincs alkalmazás tooyour directory lettek hozzáadva, ha ezen a lapon jelenjen-e csak hello **hozzáadhat egy alkalmazást** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-206">If no apps have been added tooyour directory, this page will show only hello **Add an App** link.</span></span> <span data-ttu-id="aaf8e-207">Hello hivatkozásra, vagy másik lehetőségként kattinthat hello **hozzáadása** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-207">Click hello link, or alternatively, you can click hello **ADD** on hello command bar.</span></span>
4. <span data-ttu-id="aaf8e-208">A hello **alkalmazás hozzáadása** hello varázsló **miről szeretne toodo?** lapján kattintson **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-208">In hello **ADD APPLICATION** wizard, on hello **What do you want toodo?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="aaf8e-209">A hello **adja meg azt az alkalmazás** lapon adja meg az alkalmazás nevét, és válassza **WEB APPLICATION AND/OR WEB API** (hello alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-209">On hello **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (hello default).</span></span> <span data-ttu-id="aaf8e-210">Kattintson a Tovább hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-210">Click hello Next icon.</span></span>
6. <span data-ttu-id="aaf8e-211">A hello **alkalmazás tulajdonságainak** adja meg azokat a hello **SIGN-ON URL** és **APP ID URI** a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-211">On hello **App properties** page, specify hello **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="aaf8e-212">Ha az alkalmazás nem rendelkezik ezekkel az értékekkel, ehhez a lépéshez nem létező értékeket is megadhat (például http://test1.contoso.com mindkét mezőhöz).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-212">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="aaf8e-213">Nem számít, ha ezeken a webhelyeken létezik; a fontos, hogy hello app ID URI mindegyik alkalmazáshoz különböző alkalmazás esetében minden egyes a címtárban.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-213">It does not matter if these sites exist; what is important is that hello app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="aaf8e-214">hello directory használja a karakterlánc tooidentify az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-214">hello directory uses this string tooidentify your app.</span></span>
7. <span data-ttu-id="aaf8e-215">Kattintson a hello teljes ikon toosave hello varázslóban a módosítások.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-215">Click hello Complete icon toosave your changes in hello wizard.</span></span>
8. <span data-ttu-id="aaf8e-216">Hello gyors kezdés lapon kattintson **KONFIGURÁLÁSA**.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-216">On hello Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="aaf8e-217">Görgessen toohello **kulcsok** szakasz, válassza ki a hello időtartama, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-217">Scroll toohello **keys** section, select hello duration, and then click **SAVE**.</span></span> <span data-ttu-id="aaf8e-218">hello oldal frissül, és most kijelzi a kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-218">hello page refreshes and now shows a key value.</span></span> <span data-ttu-id="aaf8e-219">Be kell állítania az alkalmazás a kulcs értékét, és a hello **ügyfél-azonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-219">You must configure your application with this key value and hello **CLIENT ID** value.</span></span> <span data-ttu-id="aaf8e-220">(A beállítás menete alkalmazásonként eltérő lehet.)</span><span class="sxs-lookup"><span data-stu-id="aaf8e-220">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="aaf8e-221">Ezen a lapon, mert ezzel fogja a következő lépés tooset hello engedélyei a tároló hello Ügyfélazonosító értékét másolja.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-221">Copy hello client ID value from this page, which you will use in hello next step tooset permissions on your vault.</span></span>

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a><span data-ttu-id="aaf8e-222">Hello alkalmazás toouse hello kulcs vagy titkos kulcs engedélyezése</span><span class="sxs-lookup"><span data-stu-id="aaf8e-222">Authorize hello application toouse hello key or secret</span></span>
<span data-ttu-id="aaf8e-223">tooauthorize hello alkalmazás tooaccess hello kulcsok vagy titkos hello tárolóban, használjon hello `azure keyvault set-policy` parancsot.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-223">tooauthorize hello application tooaccess hello key or secret in hello vault, use hello `azure keyvault set-policy` command.</span></span>

<span data-ttu-id="aaf8e-224">Például ha a tároló neve tooauthorize Ügyfélazonosítója 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, és ezen tooauthorize hello alkalmazás toodecrypt és bejelentkezési kulccsal rendelkező a tárolóban lévő kívánt ContosoKeyVault és hello alkalmazás, majd futtassa hello a következő:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-224">For example, if your vault name is ContosoKeyVault and hello application you want tooauthorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want tooauthorize hello application toodecrypt and sign with keys in your vault, then run hello following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> <span data-ttu-id="aaf8e-225">Ha futtatja a Windows parancssori ablakba, inkább szimpla idézőjelet cserélje le, és is escape-hello belső dupla idézőjelek között.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-225">If you are running on Windows command prompt, you should replace single quotes with double quotes, and also escape hello internal double quotes.</span></span> <span data-ttu-id="aaf8e-226">Például: "[\"visszafejtéséhez\",\"bejelentkezési\"]".</span><span class="sxs-lookup"><span data-stu-id="aaf8e-226">For example: "[\"decrypt\",\"sign\"]".</span></span>
> 
> 

<span data-ttu-id="aaf8e-227">Ha azt szeretné tooauthorize, hogy ugyanazon alkalmazás tooread titkokat a tárolóban, futtassa a hello következő:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-227">If you want tooauthorize that same application tooread secrets in your vault, run hello following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a><span data-ttu-id="aaf8e-228">Ha azt szeretné, hogy toouse hardveres biztonsági modul (HSM)</span><span class="sxs-lookup"><span data-stu-id="aaf8e-228">If you want toouse a hardware security module (HSM)</span></span>
<span data-ttu-id="aaf8e-229">A nagyobb importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-229">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="aaf8e-230">hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-230">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="aaf8e-231">Ha ez a követelmény nem érvényes tooyou, hagyja ki ezt a szakaszt, és folytassa túl[hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-231">If this requirement doesn't apply tooyou, skip this section and go too[Delete hello key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="aaf8e-232">toocreate a HSM által védett kulcsok, a tároló-előfizetéssel kell rendelkeznie, amely támogatja a HSM által védett kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-232">toocreate these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="aaf8e-233">Hello keyvault létrehozásakor adja hozzá a hello "sku" paramétert:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-233">When you create hello keyvault, add hello 'sku' parameter:</span></span>

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

<span data-ttu-id="aaf8e-234">Is hozzáadhat szoftveresen védett (korábban bemutatott), és HSM által védett kulcsokat toothis tároló.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-234">You can add software-protected keys (as shown earlier) and HSM-protected keys toothis vault.</span></span> <span data-ttu-id="aaf8e-235">toocreate HSM által védett kulcs set hello cél paraméter too'HSM ":</span><span class="sxs-lookup"><span data-stu-id="aaf8e-235">toocreate an HSM-protected key, set hello Destination parameter too'HSM':</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

<span data-ttu-id="aaf8e-236">A .pem fájlt a számítógépen a parancs tooimport egy kulcsot következő hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-236">You can use hello following command tooimport a key from a .pem file on your computer.</span></span> <span data-ttu-id="aaf8e-237">Ez a parancs hello kulcs importálja a Key Vault szolgáltatás hello a HSM-EK:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-237">This command imports hello key into HSMs in hello Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

<span data-ttu-id="aaf8e-238">hello a következő paranccsal importálhatók, egy "állapotba hozása a saját kulcs" (BYOK) csomagot.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-238">hello next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="aaf8e-239">Ez lehetővé teszi, hogy a kulcs létrehozása a helyi HSM-ben, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs továbbítható:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-239">This lets you generate your key in your local HSM, and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

<span data-ttu-id="aaf8e-240">Leírja, hogyan részletes toogenerate a BYOK-csomag, lásd: [hogyan toouse HSM-Protected kulcsokat az Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-240">For more detailed instructions about how toogenerate this BYOK package, see [How toouse HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="aaf8e-241">Hello kulcstartó és a hozzá tartozó kulcsok és titkos kulcsok törlése</span><span class="sxs-lookup"><span data-stu-id="aaf8e-241">Delete hello key vault and associated keys and secrets</span></span>
<span data-ttu-id="aaf8e-242">Ha már nincs szüksége hello kulcstartó és hello kulcs vagy titkos kódra, törölheti a hello kulcstároló hello azure keyvault delete parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-242">If you no longer need hello key vault and hello key or secret that it contains, you can delete hello key vault by using hello azure keyvault delete command:</span></span>

    azure keyvault delete --vault-name 'ContosoKeyVault'

<span data-ttu-id="aaf8e-243">Vagy egy teljes Azure erőforráscsoport, ide tartozik az hello kulcstartó és abba a csoportba más erőforrások törlése:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-243">Or, you can delete an entire Azure resource group, which includes hello key vault and any other resources that you included in that group:</span></span>

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="aaf8e-244">Egyéb Azure platformfüggetlen parancssori felület parancsai</span><span class="sxs-lookup"><span data-stu-id="aaf8e-244">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="aaf8e-245">Egyéb parancsok, akkor lehet hasznos, ha az Azure Key Vault kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="aaf8e-245">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="aaf8e-246">Ez a parancs táblázatos formában jeleníti meg az összes kulcsot és a kijelölt tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-246">This command lists a tabular display of all keys and selected properties:</span></span>

    azure keyvault key list --vault-name 'ContosoKeyVault'

<span data-ttu-id="aaf8e-247">Ez a parancs hello megadott kulcs tulajdonságainak teljes listáját jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-247">This command displays a full list of properties for hello specified key:</span></span>

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="aaf8e-248">Ez a parancs táblázatos formában jeleníti meg az összes titkos kód nevét és a kijelölt tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-248">This command lists a tabular display of all secret names and selected properties:</span></span>

    azure keyvault secret list --vault-name 'ContosoKeyVault'

<span data-ttu-id="aaf8e-249">Íme egy példa bemutatja, hogyan tooremove egy adott kulcs:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-249">Here's an example of how tooremove a specific key:</span></span>

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="aaf8e-250">Íme egy példa bemutatja, hogyan tooremove egy adott titkos kód:</span><span class="sxs-lookup"><span data-stu-id="aaf8e-250">Here's an example of how tooremove a specific secret:</span></span>

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a><span data-ttu-id="aaf8e-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aaf8e-251">Next steps</span></span>
<span data-ttu-id="aaf8e-252">Programozási hivatkozások: [hello Azure Key Vault fejlesztői útmutatója](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="aaf8e-252">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

