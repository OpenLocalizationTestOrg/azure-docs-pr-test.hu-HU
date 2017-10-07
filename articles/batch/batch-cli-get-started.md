---
title: "aaaGet Azure CLI köteg használatába |} Microsoft Docs"
description: "Helyezze a gyors bevezetés toohello kötegelt parancsok Azure CLI-t az Azure Batch szolgáltatás erőforrások kezelése"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14f28311ecb16c8097d0d304a4ad89de282a2e9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a><span data-ttu-id="85d43-103">Batch-erőforrássok kezelése az Azure CLI-vel</span><span class="sxs-lookup"><span data-stu-id="85d43-103">Manage Batch resources with Azure CLI</span></span>

<span data-ttu-id="85d43-104">hello Azure CLI 2.0 Azure új parancssori felületet Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="85d43-104">hello Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="85d43-105">A szolgáltatás macOS, Linux és Windows rendszereken használható.</span><span class="sxs-lookup"><span data-stu-id="85d43-105">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="85d43-106">Az Azure CLI 2.0 kezelése és felügyelete az Azure-erőforrások hello parancssorból van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="85d43-106">Azure CLI 2.0 is optimized for managing and administering Azure resources from hello command line.</span></span> <span data-ttu-id="85d43-107">Hello Azure CLI toomanage használhatja az Azure Batch fiókjainak és toomanage erőforrások, például a készletek, a feladatok és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="85d43-107">You can use hello Azure CLI toomanage your Azure Batch accounts and toomanage resources such as pools, jobs, and tasks.</span></span> <span data-ttu-id="85d43-108">A hello Azure parancssori felület, akkor lehet parancsprogramot futtatni a hello számos ugyanazokhoz a feladatokhoz a hajthat végre hello kötegelt API-k, az Azure-portál és a kötegelt PowerShell-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="85d43-108">With hello Azure CLI, you can script many of hello same tasks you carry out with hello Batch APIs, Azure portal, and Batch PowerShell cmdlets.</span></span>

<span data-ttu-id="85d43-109">Ez a cikk betekintést nyújt az [Azure CLI 2.0-ás verziójának](https://docs.microsoft.com/cli/azure/overview) Batch-csel történő használatába.</span><span class="sxs-lookup"><span data-stu-id="85d43-109">This article provides an overview of using [Azure CLI version 2.0](https://docs.microsoft.com/cli/azure/overview) with Batch.</span></span> <span data-ttu-id="85d43-110">Lásd: [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) hello CLI használata az Azure-ral áttekintését.</span><span class="sxs-lookup"><span data-stu-id="85d43-110">See [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) for an overview of using hello CLI with Azure.</span></span>

<span data-ttu-id="85d43-111">A Microsoft azt javasolja, hello hello 2.0-s verzióját, az Azure parancssori felület legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="85d43-111">Microsoft recommends using hello latest version of hello Azure CLI, version 2.0.</span></span> <span data-ttu-id="85d43-112">A 2.0-ás verzióval kapcsolatban további információt az [Mostantól általánosan elérhető az Azure parancssori felületének 2.0-s verziója](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/) című blogbejegyzésben talál.</span><span class="sxs-lookup"><span data-stu-id="85d43-112">For more information about version 2.0, see [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).</span></span>

## <a name="set-up-hello-azure-cli"></a><span data-ttu-id="85d43-113">Hello Azure CLI beállítása</span><span class="sxs-lookup"><span data-stu-id="85d43-113">Set up hello Azure CLI</span></span>

<span data-ttu-id="85d43-114">tooinstall hello Azure CLI lépésekkel hello leírt [telepítés hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="85d43-114">tooinstall hello Azure CLI, follow hello steps outlined in [Install hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).</span></span>

> [!TIP]
> <span data-ttu-id="85d43-115">Azt javasoljuk, hogy a frissíteni az Azure parancssori felület telepítése gyakran tootake előnyeit szolgáltatás frissítéseket és fejlesztéseket.</span><span class="sxs-lookup"><span data-stu-id="85d43-115">We recommend that you update your Azure CLI installation frequently tootake advantage of service updates and enhancements.</span></span>
> 
> 

## <a name="command-help"></a><span data-ttu-id="85d43-116">Segítség a parancsokhoz</span><span class="sxs-lookup"><span data-stu-id="85d43-116">Command help</span></span>

<span data-ttu-id="85d43-117">Jeleníthet meg minden parancs súgószöveg hello Azure CLI hozzáfűzésével `-h` toohello parancsot.</span><span class="sxs-lookup"><span data-stu-id="85d43-117">You can display help text for every command in hello Azure CLI by appending `-h` toohello command.</span></span> <span data-ttu-id="85d43-118">Az egyéb beállításokat hagyja változatlanul.</span><span class="sxs-lookup"><span data-stu-id="85d43-118">Omit any other options.</span></span> <span data-ttu-id="85d43-119">Példa:</span><span class="sxs-lookup"><span data-stu-id="85d43-119">For example:</span></span>

* <span data-ttu-id="85d43-120">hello tooget súgóját `az` parancsot, írja be:`az -h`</span><span class="sxs-lookup"><span data-stu-id="85d43-120">tooget help for hello `az` command, enter: `az -h`</span></span>
* <span data-ttu-id="85d43-121">használja a tooget hello CLI, az összes kötegelt parancsok listája:`az batch -h`</span><span class="sxs-lookup"><span data-stu-id="85d43-121">tooget a list of all Batch commands in hello CLI, use: `az batch -h`</span></span>
* <span data-ttu-id="85d43-122">Adja meg a Batch-fiók létrehozásával tooget súgó:`az batch account create -h`</span><span class="sxs-lookup"><span data-stu-id="85d43-122">tooget help on creating a Batch account, enter: `az batch account create -h`</span></span>

<span data-ttu-id="85d43-123">A kétséges, használja a hello `-h` bármely Azure CLI parancs parancssori kapcsoló tooget súgóját.</span><span class="sxs-lookup"><span data-stu-id="85d43-123">When in doubt, use hello `-h` command-line option tooget help on any Azure CLI command.</span></span>

> [!NOTE]
> <span data-ttu-id="85d43-124">Korábbi verzióiban használt Azure CLI hello `azure` toopreface CLI parancsot.</span><span class="sxs-lookup"><span data-stu-id="85d43-124">Earlier versions of hello Azure CLI used `azure` toopreface a CLI command.</span></span> <span data-ttu-id="85d43-125">A 2.0-ás verzióban minden parancs az `az` előtagot használja.</span><span class="sxs-lookup"><span data-stu-id="85d43-125">In version 2.0, all commands are now prefaced with `az`.</span></span> <span data-ttu-id="85d43-126">Lehet, hogy tooupdate a parancsfájlok toouse hello új szintaxist a 2.0-s verziójában.</span><span class="sxs-lookup"><span data-stu-id="85d43-126">Be sure tooupdate your scripts toouse hello new syntax with version 2.0.</span></span>
>
>  

<span data-ttu-id="85d43-127">Emellett tekintse meg a toohello Azure CLI hivatkozás dokumentációjában kapcsolatos [Azure parancssori felület parancsait köteg](https://docs.microsoft.com/cli/azure/batch).</span><span class="sxs-lookup"><span data-stu-id="85d43-127">Additionally, refer toohello Azure CLI reference documentation for details about [Azure CLI commands for Batch](https://docs.microsoft.com/cli/azure/batch).</span></span> 

## <a name="log-in-and-authenticate"></a><span data-ttu-id="85d43-128">Bejelentkezés és hitelesítés</span><span class="sxs-lookup"><span data-stu-id="85d43-128">Log in and authenticate</span></span>

<span data-ttu-id="85d43-129">toouse hello Azure CLI-es, a toolog kell, és hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="85d43-129">toouse hello Azure CLI with Batch, you need toolog in and authenticate.</span></span> <span data-ttu-id="85d43-130">Két egyszerű lépéseket toofollow van:</span><span class="sxs-lookup"><span data-stu-id="85d43-130">There are two simple steps toofollow:</span></span>

1. <span data-ttu-id="85d43-131">**Bejelentkezés az Azure-ba.**</span><span class="sxs-lookup"><span data-stu-id="85d43-131">**Log into Azure.**</span></span> <span data-ttu-id="85d43-132">Bejelentkezés az Azure által biztosított van tooAzure erőforrás-kezelő parancsokat, beleértve a hozzáférési [kötegelt felügyeleti szolgáltatás](batch-management-dotnet.md) parancsok.</span><span class="sxs-lookup"><span data-stu-id="85d43-132">Logging into Azure gives you access tooAzure Resource Manager commands, including [Batch Management service](batch-management-dotnet.md) commands.</span></span>  
2. <span data-ttu-id="85d43-133">**Bejelentkezés a Batch-fiókjába.**</span><span class="sxs-lookup"><span data-stu-id="85d43-133">**Log into your Batch account.**</span></span> <span data-ttu-id="85d43-134">A kötegelt biztosít való bejelentkezés tooBatch szolgáltatás parancsok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="85d43-134">Logging into your Batch account gives you access tooBatch service commands.</span></span>   

### <a name="log-in-tooazure"></a><span data-ttu-id="85d43-135">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="85d43-135">Log in tooAzure</span></span>

<span data-ttu-id="85d43-136">Van néhány különböző módokon toolog az Azure, a részletes leírását lásd [jelentkezzen be Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):</span><span class="sxs-lookup"><span data-stu-id="85d43-136">There are a few different ways toolog into Azure, described in detail in [Log in with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):</span></span>

1. <span data-ttu-id="85d43-137">[Interaktív bejelentkezés](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in).</span><span class="sxs-lookup"><span data-stu-id="85d43-137">[Log in interactively](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in).</span></span> <span data-ttu-id="85d43-138">Jelentkezzen be párbeszédes formában történő futtatásakor Azure parancssori felület parancsait saját kezűleg hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="85d43-138">Log in interactively when you are running Azure CLI commands yourself from hello command line.</span></span>
2. <span data-ttu-id="85d43-139">[Bejelentkezés szolgáltatásnévvel](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="85d43-139">[Log in with a service principal](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal).</span></span> <span data-ttu-id="85d43-140">Jelentkezzen be szolgáltatásnévvel, ha szkript vagy alkalmazás használatával kíván Azure CLI-parancsokat futtatni.</span><span class="sxs-lookup"><span data-stu-id="85d43-140">Log in with a service principal when you are running Azure CLI commands from a script or an application.</span></span>

<span data-ttu-id="85d43-141">Ez a cikk hello célokra megmutatjuk, hogyan toolog az Azure interaktív módon.</span><span class="sxs-lookup"><span data-stu-id="85d43-141">For hello purposes of this article, we show how toolog into Azure interactively.</span></span> <span data-ttu-id="85d43-142">Típus [az bejelentkezési](https://docs.microsoft.com/cli/azure/#login) hello parancssorban:</span><span class="sxs-lookup"><span data-stu-id="85d43-142">Type [az login](https://docs.microsoft.com/cli/azure/#login) on hello command line:</span></span>

```azurecli
# Log in tooAzure and authenticate interactively.
az login
```

<span data-ttu-id="85d43-143">Hello `az login` token használható tooauthenticate, ahogy az itt látható a parancs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="85d43-143">hello `az login` command returns a token that you can use tooauthenticate, as shown here.</span></span> <span data-ttu-id="85d43-144">Hajtsa végre a hello utasításokat tooopen egy weblap, és küldje el a hello token tooAzure:</span><span class="sxs-lookup"><span data-stu-id="85d43-144">Follow hello instructions provided tooopen a web page and submit hello token tooAzure:</span></span>

![Jelentkezzen be tooAzure](./media/batch-cli-get-started/az-login.png)

<span data-ttu-id="85d43-146">hello példákban szereplő hello [rendszerhéj-parancsfájlok minta](#sample-shell-scripts) . szakasz is megjelenítése hogyan toostart interaktívan jelentkezik be Azure által az Azure CLI munkamenet.</span><span class="sxs-lookup"><span data-stu-id="85d43-146">hello examples listed in hello [Sample shell scripts](#sample-shell-scripts) section also show how toostart your Azure CLI session by logging into Azure interactively.</span></span> <span data-ttu-id="85d43-147">Ha már bejelentkezett, hívása parancsok toowork Batch-fiókok, kulcsokkal, alkalmazáscsomagok és kvóták kezelését az erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="85d43-147">Once you have logged in, you can call commands toowork with Batch Management resources, including Batch accounts, keys, application packages, and quotas.</span></span>  

### <a name="log-in-tooyour-batch-account"></a><span data-ttu-id="85d43-148">Jelentkezzen be tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="85d43-148">Log in tooyour Batch account</span></span>

<span data-ttu-id="85d43-149">toouse hello Azure CLI toomanage kötegelt erőforrások, például-készletek, feladatok, és feladatokat, a Batch-fiók toolog igénylő, és a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="85d43-149">toouse hello Azure CLI toomanage Batch resources, such as pools, jobs, and tasks, you need toolog into your Batch account and authenticate.</span></span> <span data-ttu-id="85d43-150">a Batch szolgáltatás toohello toolog hello használata [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) parancsot.</span><span class="sxs-lookup"><span data-stu-id="85d43-150">toolog in toohello Batch service, use hello [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) command.</span></span> 

<span data-ttu-id="85d43-151">A Batch-fiók hitelesítését két módon is elvégezheti:</span><span class="sxs-lookup"><span data-stu-id="85d43-151">You have two options for authenticating against your Batch account:</span></span>

- <span data-ttu-id="85d43-152">**Hitelesítés az Azure Active Directory (Azure AD) használatával.**</span><span class="sxs-lookup"><span data-stu-id="85d43-152">**By using Azure Active Directory (Azure AD) authentication.**</span></span> 

    <span data-ttu-id="85d43-153">Az Azure AD hitelesítő hello alapértelmezett hello Azure CLI-es használatakor, és a legtöbb esetben ajánlott.</span><span class="sxs-lookup"><span data-stu-id="85d43-153">Authenticating with Azure AD is hello default when you use hello Azure CLI with Batch, and recommended for most scenarios.</span></span> 
    
    <span data-ttu-id="85d43-154">Bejelentkezéskor tooAzure interaktív módon, hello előző szakaszban leírtak szerint, a hitelesítőadatok gyorsítótárazva lettek, így hello Azure parancssori felület is bejelentkezés tooyour e ugyanazokat a hitelesítő adatokat használ a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="85d43-154">When you log in tooAzure interactively, as described in hello previous section, your credentials are cached, so hello Azure CLI can log you in tooyour Batch account using those same credentials.</span></span> <span data-ttu-id="85d43-155">Ha jelentkezik be egy egyszerű szolgáltatás használatával tooAzure, ezeket a hitelesítő adatokat egyaránt használt toolog a tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="85d43-155">If you log in tooAzure using a service principal, those credentials are also used toolog in tooyour Batch account.</span></span>

    <span data-ttu-id="85d43-156">Az Azure AD előnye a szerepköralapú hozzáférés-vezérlés (RBAC) használatában rejlik.</span><span class="sxs-lookup"><span data-stu-id="85d43-156">An advantage of Azure AD is that it offers role-based access control (RBAC).</span></span> <span data-ttu-id="85d43-157">Az RBAC a felhasználó hozzáférési függ hozzájuk rendelt szerepkör helyett hello kulcsait rendelkeznek-e.</span><span class="sxs-lookup"><span data-stu-id="85d43-157">With RBAC, a user's access depends on their assigned role, rather than whether or not they possess hello account keys.</span></span> <span data-ttu-id="85d43-158">Így ahelyett, hogy hozzáférési kulcsokat kellene kezelnie, elég ha a szerepköröket kezeli, a hozzáférést és a hitelesítést pedig az Azure AD-ra bízhatja.</span><span class="sxs-lookup"><span data-stu-id="85d43-158">Instead of managing account keys, you can manage RBAC roles, and let Azure AD handle access and authentication.</span></span>  

    <span data-ttu-id="85d43-159">Az Azure AD hitelesítő szükség, ha létrehozta az Azure Batch-fiókhoz, a tárolókészlet foglalási mód beállítása too'User előfizetés ".</span><span class="sxs-lookup"><span data-stu-id="85d43-159">Authenticating with Azure AD is required if you created your Azure Batch account with its pool allocation mode set too'User Subscription'.</span></span> 

    <span data-ttu-id="85d43-160">a kötegelt tooyour toolog fiók az Azure AD segítségével, hello hívás [az batch-fiók bejelentkezési](https://docs.microsoft.com/cli/azure/batch/account#login) parancs:</span><span class="sxs-lookup"><span data-stu-id="85d43-160">toolog in tooyour Batch account using Azure AD, call hello [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) command:</span></span> 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- <span data-ttu-id="85d43-161">**Megosztott kulcsos hitelesítés.**</span><span class="sxs-lookup"><span data-stu-id="85d43-161">**By using Shared Key authentication.**</span></span>

    <span data-ttu-id="85d43-162">[Megosztott kulcsos hitelesítést](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) használ, a fiók hozzáférési kulcsok tooauthenticate Azure parancssori felület parancsait hello a Batch-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="85d43-162">[Shared Key authentication](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) uses your account access keys tooauthenticate Azure CLI commands for hello Batch service.</span></span>

    <span data-ttu-id="85d43-163">Létrehozásakor az Azure parancssori felület parancsfájlok tooautomate hívó kötegelt parancsok, megosztott kulcsos hitelesítést, vagy az Azure AD szolgáltatás egyszerű is használhatja.</span><span class="sxs-lookup"><span data-stu-id="85d43-163">If you are creating Azure CLI scripts tooautomate calling Batch commands, you can use either Shared Key authentication, or an Azure AD service principal.</span></span> <span data-ttu-id="85d43-164">Bizonyos esetekben viszont a megosztott kulcsos hitelesítés használata egyszerűbb lehet, mint létrehozni egy szolgáltatásnevet.</span><span class="sxs-lookup"><span data-stu-id="85d43-164">In some scenarios, using Shared Key authentication may be simpler than creating a service principal.</span></span>  

    <span data-ttu-id="85d43-165">a megosztott kulcsos hitelesítést használó toolog tartalmaznak hello `--shared-key-auth` hello parancssori beállítást:</span><span class="sxs-lookup"><span data-stu-id="85d43-165">toolog in using Shared Key authentication, include hello `--shared-key-auth` option on hello command line:</span></span>

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

<span data-ttu-id="85d43-166">hello példákban szereplő hello [rendszerhéj-parancsfájlok minta](#sample-shell-scripts) szakasz megjelenítése hogyan toolog a kötegelt fiókjába a hello szolgáltatást is használja az Azure parancssori felület az Azure AD és a megosztott kulcsot.</span><span class="sxs-lookup"><span data-stu-id="85d43-166">hello examples listed in hello [Sample shell scripts](#sample-shell-scripts) section show how toolog into your Batch account with hello Azure CLI using both Azure AD and Shared Key.</span></span>

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="85d43-167">Az Azure Batch parancssori felületi sablonjainak és fájlátviteli funkciójának (előzetes verzió) használata</span><span class="sxs-lookup"><span data-stu-id="85d43-167">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="85d43-168">Kód írása nélkül hello Azure CLI toorun kötegelt feladatok-végpontok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="85d43-168">You can use hello Azure CLI toorun Batch jobs end-to-end without writing code.</span></span> <span data-ttu-id="85d43-169">Sablon parancsfájlokat létrehozása készletek, feladatok és az Azure CLI hello feladatok támogatja.</span><span class="sxs-lookup"><span data-stu-id="85d43-169">Batch template files support creating pools, jobs, and tasks with hello Azure CLI.</span></span> <span data-ttu-id="85d43-170">Is használhatja a hello Azure CLI tooupload feladat bemeneti fájlok toohello hello társított Azure Storage-fiók a Batch-fiók, és a feladat kimeneti fájlok letöltését.</span><span class="sxs-lookup"><span data-stu-id="85d43-170">You can also use hello Azure CLI tooupload job input files toohello Azure Storage account associated with hello Batch account, and download job output files from it.</span></span> <span data-ttu-id="85d43-171">További információk: [Az Azure Batch parancssori felületi sablonjainak és fájlátviteli funkciójának (előzetes verzió) használata](batch-cli-templates.md).</span><span class="sxs-lookup"><span data-stu-id="85d43-171">For more information, see [Use Azure Batch CLI Templates and File Transfer (Preview)](batch-cli-templates.md).</span></span>

## <a name="sample-shell-scripts"></a><span data-ttu-id="85d43-172">Shell-szkript minták</span><span class="sxs-lookup"><span data-stu-id="85d43-172">Sample shell scripts</span></span>

<span data-ttu-id="85d43-173">hello mintaparancsfájlok szerepel a következő táblázat megjelenítése hello hogyan toouse Azure parancssori felület parancsai hello Batch szolgáltatás és a Batch Management szolgáltatás tooaccomplish gyakori feladatokat.</span><span class="sxs-lookup"><span data-stu-id="85d43-173">hello sample scripts listed in hello following table show how toouse Azure CLI commands with hello Batch service and Batch Management service tooaccomplish common tasks.</span></span> <span data-ttu-id="85d43-174">A minta parancsfájlokat hello parancsok hello Azure CLI köteg elérhető számos foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="85d43-174">These sample scripts cover many of hello commands available in hello Azure CLI for Batch.</span></span> 

| <span data-ttu-id="85d43-175">Szkript</span><span class="sxs-lookup"><span data-stu-id="85d43-175">Script</span></span> | <span data-ttu-id="85d43-176">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="85d43-176">Notes</span></span> |
|---|---|
| [<span data-ttu-id="85d43-177">Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="85d43-177">Create a Batch account</span></span>](./scripts/batch-cli-sample-create-account.md) | <span data-ttu-id="85d43-178">Létrehoz egy Batch-fiókot és hozzárendeli egy tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="85d43-178">Creates a Batch account and associates it with a storage account.</span></span> |
| [<span data-ttu-id="85d43-179">Alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="85d43-179">Add an application</span></span>](./scripts/batch-cli-sample-add-application.md) | <span data-ttu-id="85d43-180">Hozzáad egy alkalmazást és feltölti a csomagolt bináris fájlokat.</span><span class="sxs-lookup"><span data-stu-id="85d43-180">Adds an application and uploads packaged binaries.</span></span>|
| [<span data-ttu-id="85d43-181">Batch-készletek kezelése</span><span class="sxs-lookup"><span data-stu-id="85d43-181">Manage Batch pools</span></span>](./scripts/batch-cli-sample-manage-pool.md) | <span data-ttu-id="85d43-182">Bemutatja a készletek létrehozását, átméretezését és kezelését.</span><span class="sxs-lookup"><span data-stu-id="85d43-182">Demonstrates creating, resizing, and managing pools.</span></span> |
| [<span data-ttu-id="85d43-183">Feladatok és tevékenységek futtatása a Batch-csel</span><span class="sxs-lookup"><span data-stu-id="85d43-183">Run a job and tasks with Batch</span></span>](./scripts/batch-cli-sample-run-job.md) | <span data-ttu-id="85d43-184">Bemutatja a feladatok futtatását és a tevékenységek hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="85d43-184">Demonstrates running a job and adding tasks.</span></span> |

## <a name="json-files-for-resource-creation"></a><span data-ttu-id="85d43-185">Erőforrás létrehozása JSON-fájlok használatával</span><span class="sxs-lookup"><span data-stu-id="85d43-185">JSON files for resource creation</span></span>

<span data-ttu-id="85d43-186">Kötegelt erőforrásokhoz, mint a készletek és a feladatok létrehozásakor egy JSON-fájlt tartalmazó hello új erőforrás konfigurációs helyett a paraméterek átadása, parancssori kapcsolókat is megadhat.</span><span class="sxs-lookup"><span data-stu-id="85d43-186">When you create Batch resources like pools and jobs, you can specify a JSON file containing hello new resource's configuration instead of passing its parameters as command-line options.</span></span> <span data-ttu-id="85d43-187">Példa:</span><span class="sxs-lookup"><span data-stu-id="85d43-187">For example:</span></span>

```azurecli
az batch pool create my_batch_pool.json
```

<span data-ttu-id="85d43-188">Legtöbb kötegelt erőforrások csak parancssori kapcsolók használatával hozhat létre, míg egyes funkciók szükségesek, egy hello erőforrás részleteit tartalmazó JSON-formátumú fájlt ad meg.</span><span class="sxs-lookup"><span data-stu-id="85d43-188">While you can create most Batch resources using only command-line options, some features require that you specify a JSON-formatted file containing hello resource details.</span></span> <span data-ttu-id="85d43-189">Például egy JSON-fájlt kell használnia, ha azt szeretné, hogy toospecify Erőforrásfájlok kezdő tevékenység.</span><span class="sxs-lookup"><span data-stu-id="85d43-189">For example, you must use a JSON file if you want toospecify resource files for a start task.</span></span>

<span data-ttu-id="85d43-190">JSON-szintaxis toosee hello szükséges toocreate erőforrás, tekintse meg a toohello [Batch REST API-referenciában] [ rest_api] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="85d43-190">toosee hello JSON syntax required toocreate a resource, refer toohello [Batch REST API reference][rest_api] documentation.</span></span> <span data-ttu-id="85d43-191">Minden "Add *erőforrástípus*" hello REST API-referenciában témakör JSON mintaparancsfájlok erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="85d43-191">Each "Add *resource type*" topic in hello REST API reference contains sample JSON scripts for creating that resource.</span></span> <span data-ttu-id="85d43-192">Használható a minta JSON-parancsfájlok sablonként hello Azure parancssori Felülettel rendelkező JSON-fájlok toouse.</span><span class="sxs-lookup"><span data-stu-id="85d43-192">You can use those sample JSON scripts as templates for JSON files toouse with hello Azure CLI.</span></span> <span data-ttu-id="85d43-193">Például toosee hello JSON-szintaxis készlet létrehozását, tekintse meg a túl[tooan alkalmazáskészlet-fiók hozzáadása][rest_add_pool].</span><span class="sxs-lookup"><span data-stu-id="85d43-193">For example, toosee hello JSON syntax for pool creation, refer too[Add a pool tooan account][rest_add_pool].</span></span>

<span data-ttu-id="85d43-194">JSON-fájlra hivatkozó szkriptre példát a [Feladatok és tevékenységek futtatása a Batch-csel](./scripts/batch-cli-sample-run-job.md) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="85d43-194">For a sample script that specifies a JSON file, see [Run a job and tasks with Batch](./scripts/batch-cli-sample-run-job.md).</span></span>

> [!NOTE]
> <span data-ttu-id="85d43-195">Erőforrás létrehozásakor megad egy JSON-fájlt, ha a rendszer figyelmen kívül hagyja az adott erőforrás hello parancssori ad meg más paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="85d43-195">If you specify a JSON file when you create a resource, any other parameters that you specify on hello command line for that resource are ignored.</span></span>
> 
> 

## <a name="efficient-queries-for-batch-resources"></a><span data-ttu-id="85d43-196">Hatékony lekérdezések Batch-erőforrásokhoz</span><span class="sxs-lookup"><span data-stu-id="85d43-196">Efficient queries for Batch resources</span></span>

<span data-ttu-id="85d43-197">Minden Batch erőforrástípus támogat egy `list` parancsot, amely lekérdezi a Batch-fiókját, és listázza az adott típusú erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="85d43-197">Each Batch resource type supports a `list` command that queries your Batch account and lists resources of that type.</span></span> <span data-ttu-id="85d43-198">Például a fiók és a hello feladatokat egy feladat hello készletek is listázhatja:</span><span class="sxs-lookup"><span data-stu-id="85d43-198">For example, you can list hello pools in your account and hello tasks in a job:</span></span>

```azurecli
az batch pool list
az batch task list --job-id job001
```

<span data-ttu-id="85d43-199">Ha lekérdezést hajt végre hello Batch szolgáltatást, amely egy `list` művelet, megadhat egy OData záradék toolimit hello mennyiségű adatot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="85d43-199">When you query hello Batch service with a `list` operation, you can specify an OData clause toolimit hello amount of data returned.</span></span> <span data-ttu-id="85d43-200">Kiszolgálóoldali összes szűrés következik be, mert csak hello adatokat kér le áthalad hello keresztülhaladnak a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="85d43-200">Because all filtering occurs server-side, only hello data you request crosses hello wire.</span></span> <span data-ttu-id="85d43-201">Ezek záradékok toosave sávszélességet (és időpontot, ezért) lista műveletek végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="85d43-201">Use these clauses toosave bandwidth (and therefore time) when you perform list operations.</span></span>

<span data-ttu-id="85d43-202">hello következő táblázatban hello OData záradékok hello Batch szolgáltatás által támogatott.</span><span class="sxs-lookup"><span data-stu-id="85d43-202">hello following table describes hello OData clauses supported by hello Batch service:</span></span>

| <span data-ttu-id="85d43-203">Záradék</span><span class="sxs-lookup"><span data-stu-id="85d43-203">Clause</span></span> | <span data-ttu-id="85d43-204">Leírás</span><span class="sxs-lookup"><span data-stu-id="85d43-204">Description</span></span> |
|---|---|
| `--select-clause [select-clause]` | <span data-ttu-id="85d43-205">A tulajdonságok egy részét adja vissza minden entitás esetében.</span><span class="sxs-lookup"><span data-stu-id="85d43-205">Returns a subset of properties for each entity.</span></span> |
| `--filter-clause [filter-clause]` | <span data-ttu-id="85d43-206">Csak entitások értéket ad vissza, amelyek megfelelnek a hello megadott OData kifejezés.</span><span class="sxs-lookup"><span data-stu-id="85d43-206">Returns only entities that match hello specified OData expression.</span></span> |
| `--expand-clause [expand-clause]` | <span data-ttu-id="85d43-207">Beolvassa a hello entitás adatokat egyetlen alapul szolgáló REST-hívást.</span><span class="sxs-lookup"><span data-stu-id="85d43-207">Obtains hello entity information in a single underlying REST call.</span></span> <span data-ttu-id="85d43-208">hello bontsa ki a záradékot jelenleg csak hello támogatja `stats` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="85d43-208">hello expand clause currently supports only hello `stats` property.</span></span> |

<span data-ttu-id="85d43-209">A minta parancsfájl, hogy bemutatja, hogyan toouse egy OData záradék: [futtatni egy feladatot, és a feladatok kötegelt](./scripts/batch-cli-sample-run-job.md).</span><span class="sxs-lookup"><span data-stu-id="85d43-209">For a sample script that shows how toouse an OData clause, see [Run a job and tasks with Batch](./scripts/batch-cli-sample-run-job.md).</span></span>

<span data-ttu-id="85d43-210">Az OData záradékot tartalmazó hatékony lista lekérdezések végrehajtásáról további információkért lásd: [hello Azure Batch szolgáltatás hatékony lekérdezéséhez](batch-efficient-list-queries.md).</span><span class="sxs-lookup"><span data-stu-id="85d43-210">For more information on performing efficient list queries with OData clauses, see [Query hello Azure Batch service efficiently](batch-efficient-list-queries.md).</span></span>

## <a name="troubleshooting-tips"></a><span data-ttu-id="85d43-211">Hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="85d43-211">Troubleshooting tips</span></span>

<span data-ttu-id="85d43-212">hello alábbi tippek segíthetnek az Azure parancssori felület problémák elhárításakor:</span><span class="sxs-lookup"><span data-stu-id="85d43-212">hello following tips may help when you are troubleshooting Azure CLI issues:</span></span>

* <span data-ttu-id="85d43-213">Használjon `-h` tooget **súgószöveg** bármely CLI parancs esetében</span><span class="sxs-lookup"><span data-stu-id="85d43-213">Use `-h` tooget **help text** for any CLI command</span></span>
* <span data-ttu-id="85d43-214">Használjon `-v` és `-vv` toodisplay **részletes** kimeneti parancsot.</span><span class="sxs-lookup"><span data-stu-id="85d43-214">Use `-v` and `-vv` toodisplay **verbose** command output.</span></span> <span data-ttu-id="85d43-215">Ha hello `-vv` jelző megtalálható, hello Azure parancssori felület megjeleníti hello tényleges REST kérelmeit és válaszait.</span><span class="sxs-lookup"><span data-stu-id="85d43-215">When hello `-vv` flag is included, hello Azure CLI displays hello actual REST requests and responses.</span></span> <span data-ttu-id="85d43-216">Ezek a kapcsolók jól jönnek a teljes hibakimenet megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="85d43-216">These switches are handy for displaying full error output.</span></span>
* <span data-ttu-id="85d43-217">Megtekintheti **parancsot a kimeneti adatok JSON** a hello `--json` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="85d43-217">You can view **command output as JSON** with hello `--json` option.</span></span> <span data-ttu-id="85d43-218">Például az `az batch pool show pool001 --json` JSON-formátumban jeleníti meg a pool001 tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="85d43-218">For example, `az batch pool show pool001 --json` displays pool001's properties in JSON format.</span></span> <span data-ttu-id="85d43-219">Ezután másolja, és módosítsa ezt a kimeneti toouse egy `--json-file` (lásd: [JSON-fájlok](#json-files) korábbi ebben a cikkben).</span><span class="sxs-lookup"><span data-stu-id="85d43-219">You can then copy and modify this output toouse in a `--json-file` (see [JSON files](#json-files) earlier in this article).</span></span>
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting tooany location.--->
* <span data-ttu-id="85d43-220">Hello [Batch fórum] [ batch_forum] figyelt kötegelt csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="85d43-220">hello [Batch forum][batch_forum] is monitored by Batch team members.</span></span> <span data-ttu-id="85d43-221">Ott felteheti kérdéseit, ha problémákba ütközne, vagy segítségre lenne szüksége egy adott művelethez.</span><span class="sxs-lookup"><span data-stu-id="85d43-221">You can post your questions there if you run into issues or would like help with a specific operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85d43-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85d43-222">Next steps</span></span>

* <span data-ttu-id="85d43-223">Hello Azure parancssori Felülettel kapcsolatos további információkért lásd: hello [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85d43-223">For more information about hello Azure CLI, see hello [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="85d43-224">További információt a Batch-erőforrásokkal kapcsolatban a [Az Azure Batch áttekintése fejlesztők számára](batch-api-basics.md) című cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="85d43-224">For more information about Batch resources, see [Overview of Azure Batch for developers](batch-api-basics.md).</span></span>
* <span data-ttu-id="85d43-225">Kód írása nélkül kötegelt sablonok toocreate készletek, feladatok és feladatok használatával kapcsolatos további információkért lásd: [használata Azure Batch CLI sablonok és a File Transfer (előzetes verzió)](batch-cli-templates.md).</span><span class="sxs-lookup"><span data-stu-id="85d43-225">For more information about using Batch templates toocreate pools, jobs, and tasks without writing code, see [Use Azure Batch CLI Templates and File Transfer (Preview)](batch-cli-templates.md).</span></span>

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
