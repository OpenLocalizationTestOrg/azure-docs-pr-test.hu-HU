---
title: "a Batch-fiók hello Azure-portálon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Azure Batch fiókként hello Azure portál toorun nagyméretű párhuzamos munkaterhelések hello felhőben"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2176f88ba0a1a3298023de8f520d46ef28a664b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-portal"></a><span data-ttu-id="80f2f-103">Az Azure-portálon hello Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="80f2f-103">Create a Batch account with hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="80f2f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="80f2f-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="80f2f-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="80f2f-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
>
>

<span data-ttu-id="80f2f-106">Megtudhatja, hogyan toocreate az Azure Batch hello fiókként [Azure-portálon][azure_portal], és válassza ki, amelyek felelnek meg a számítási forgatókönyv hello fiók tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="80f2f-106">Learn how toocreate an Azure Batch account in hello [Azure portal][azure_portal], and choose hello account properties that fit your compute scenario.</span></span> <span data-ttu-id="80f2f-107">Ismerje meg, ahol toofind fontos fiók tulajdonságait, például tárelérési kulcsok és a fiók URL-címek.</span><span class="sxs-lookup"><span data-stu-id="80f2f-107">Learn where toofind important account properties like access keys and account URLs.</span></span>

<span data-ttu-id="80f2f-108">Batch-fiókok és forgatókönyvek kapcsolatos háttér, lásd: hello [szolgáltatás áttekintése](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="80f2f-108">For background about Batch accounts and scenarios, see hello [feature overview](batch-api-basics.md).</span></span>



## <a name="create-a-batch-account"></a><span data-ttu-id="80f2f-109">Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="80f2f-109">Create a Batch account</span></span>

<span data-ttu-id="80f2f-110">Hello portál toocreate Batch-fiók használata egy hello két *tárolókészlet foglalási mód*: **a Batch szolgáltatás** módban vagy újabb hello **felhasználói előfizetési** módját, amely több helyre van szüksége konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="80f2f-110">Use hello portal toocreate a Batch account in one of hello two *pool allocation modes*: **Batch service** mode or hello newer **user subscription** mode, which requires more configuration.</span></span> <span data-ttu-id="80f2f-111">E két mód kapcsolatos információkért lásd: hello [szolgáltatás áttekintése](batch-api-basics.md#account).</span><span class="sxs-lookup"><span data-stu-id="80f2f-111">For information about these two modes, see hello [feature overview](batch-api-basics.md#account).</span></span> <span data-ttu-id="80f2f-112">A hello felhasználói előfizetési mód, lásd is hello [blogbejegyzés](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span><span class="sxs-lookup"><span data-stu-id="80f2f-112">For features of hello user subscription mode, see also hello [blog post](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span></span>

## <a name="batch-service-mode"></a><span data-ttu-id="80f2f-113">Batch szolgáltatás mód</span><span class="sxs-lookup"><span data-stu-id="80f2f-113">Batch service mode</span></span>



1. <span data-ttu-id="80f2f-114">Jelentkezzen be toohello [Azure-portálon][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="80f2f-114">Sign in toohello [Azure portal][azure_portal].</span></span>
2. <span data-ttu-id="80f2f-115">Kattintson az **Új** > **Számítás** > **Batch-szolgáltatás** elemre.</span><span class="sxs-lookup"><span data-stu-id="80f2f-115">Click **New** > **Compute** > **Batch Service**.</span></span>

    ![Köteg hello piactér][marketplace_portal]
3. <span data-ttu-id="80f2f-117">Hello **új Batch-fiók** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="80f2f-117">hello **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="80f2f-118">Lásd: hello leírását alább minden panel elemet.</span><span class="sxs-lookup"><span data-stu-id="80f2f-118">See hello descriptions below of each blade element.</span></span>

    ![Batch-fiók létrehozása][account_portal]

    <span data-ttu-id="80f2f-120">a.</span><span class="sxs-lookup"><span data-stu-id="80f2f-120">a.</span></span> <span data-ttu-id="80f2f-121">**Fióknév**: hello Batch-fiók neve mellett dönt hello Azure-régió, ahol hello-fiók létrehozása belül egyedinek kell lennie (lásd: **hely** alatt).</span><span class="sxs-lookup"><span data-stu-id="80f2f-121">**Account name**: hello Batch account name you choose must be unique within hello Azure region where hello account is created (see **Location** below).</span></span> <span data-ttu-id="80f2f-122">hello fiók neve csak kisbetűket és számokat tartalmazhat, és 3 – 24 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="80f2f-122">hello account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="80f2f-123">b.</span><span class="sxs-lookup"><span data-stu-id="80f2f-123">b.</span></span> <span data-ttu-id="80f2f-124">**Előfizetés**: hello mely toocreate hello Batch-fiókhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="80f2f-124">**Subscription**: hello subscription in which toocreate hello Batch account.</span></span> <span data-ttu-id="80f2f-125">Ha csak egy előfizetéssel rendelkezik, ez alapértelmezés szerint be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="80f2f-125">If you have only one subscription, it is selected by default.</span></span>

    <span data-ttu-id="80f2f-126">c.</span><span class="sxs-lookup"><span data-stu-id="80f2f-126">c.</span></span> <span data-ttu-id="80f2f-127">**Készletfelosztási mód**: Válassza a **Batch szolgáltatás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="80f2f-127">**Pool allocation mode**: Select **Batch service**.</span></span>

    <span data-ttu-id="80f2f-128">c.</span><span class="sxs-lookup"><span data-stu-id="80f2f-128">c.</span></span> <span data-ttu-id="80f2f-129">**Erőforráscsoport**: Kiválaszthat egy meglévő erőforráscsoportot az új Batch-fiókhoz, vagy újat is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="80f2f-129">**Resource group**: Select an existing resource group for your new Batch account, or optionally create a new one.</span></span>

    <span data-ttu-id="80f2f-130">d.</span><span class="sxs-lookup"><span data-stu-id="80f2f-130">d.</span></span> <span data-ttu-id="80f2f-131">**Hely**: hello Azure-régiót mely toocreate hello Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="80f2f-131">**Location**: hello Azure region in which toocreate hello Batch account.</span></span> <span data-ttu-id="80f2f-132">Beállítások, csak az előfizetés és az erőforráscsoport által támogatott hello régiók jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="80f2f-132">Only hello regions supported by your subscription and resource group are displayed as options.</span></span>

    <span data-ttu-id="80f2f-133">e.</span><span class="sxs-lookup"><span data-stu-id="80f2f-133">e.</span></span> <span data-ttu-id="80f2f-134">**Tárfiók** (nem kötelező): Olyan általános célú Azure Storage-fiók, amelyet a Batch-fiókhoz társít.</span><span class="sxs-lookup"><span data-stu-id="80f2f-134">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="80f2f-135">A legtöbb Batch-fiókhoz ajánlott a használata.</span><span class="sxs-lookup"><span data-stu-id="80f2f-135">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="80f2f-136">További részletekért tekintse meg a lentebb található [Társított Azure Storage-fiók](#linked-azure-storage-account) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="80f2f-136">See [Linked Azure Storage account](#linked-azure-storage-account) later in this article for more details.</span></span>

4. <span data-ttu-id="80f2f-137">Kattintson a **létrehozása** toocreate hello fiók.</span><span class="sxs-lookup"><span data-stu-id="80f2f-137">Click **Create** toocreate hello account.</span></span>

   <span data-ttu-id="80f2f-138">hello portál jelzi, hogy központi telepítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="80f2f-138">hello portal indicates deployment is in progress.</span></span> <span data-ttu-id="80f2f-139">A befejezéskor **Az üzembe helyezések sikerültek** értesítés jelenik meg az **Értesítések** területen.</span><span class="sxs-lookup"><span data-stu-id="80f2f-139">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>

## <a name="user-subscription-mode"></a><span data-ttu-id="80f2f-140">Felhasználói előfizetés mód</span><span class="sxs-lookup"><span data-stu-id="80f2f-140">User subscription mode</span></span>

### <a name="allow-azure-batch-tooaccess-hello-subscription-one-time-operation"></a><span data-ttu-id="80f2f-141">Engedélyezi az Azure Batch tooaccess hello előfizetés (egyszeri művelet)</span><span class="sxs-lookup"><span data-stu-id="80f2f-141">Allow Azure Batch tooaccess hello subscription (one-time operation)</span></span>
<span data-ttu-id="80f2f-142">Az első Batch-fiók felhasználói előfizetési módban létrehozásakor hajtsa végre a következő lépéseket tooregister hello kötegelt előfizetés.</span><span class="sxs-lookup"><span data-stu-id="80f2f-142">When creating your first Batch account in user subscription mode, perform hello following steps tooregister your subscription with Batch.</span></span> <span data-ttu-id="80f2f-143">(Ha korábban már elvégezte ezt, hagyja ki toohello a következő szakaszt.)</span><span class="sxs-lookup"><span data-stu-id="80f2f-143">(If you previously did this, skip toohello next section.)</span></span>

1. <span data-ttu-id="80f2f-144">Jelentkezzen be toohello [Azure-portálon][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="80f2f-144">Sign in toohello [Azure portal][azure_portal].</span></span>

2. <span data-ttu-id="80f2f-145">Kattintson a **több szolgáltatások** > **előfizetések**, és hello az előfizetés Batch-fiók hello toouse használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="80f2f-145">Click **More Services** > **Subscriptions**, and click hello subscription you want toouse for hello Batch account.</span></span>

3. <span data-ttu-id="80f2f-146">A hello **előfizetés** panelen kattintson a **hozzáférés-vezérlés (IAM)** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-146">In hello **Subscription** blade, click **Access control (IAM)** > **Add**.</span></span>

    ![Az előfizetéshez való hozzáférés vezérlése][subscription_access]

4. <span data-ttu-id="80f2f-148">A hello **engedélyek hozzáadása** panelen, jelölje be hello **közreműködő** szerepkör, keressen a hello kötegelt API.</span><span class="sxs-lookup"><span data-stu-id="80f2f-148">On hello **Add permissions** blade, select hello **Contributor** role, search for hello Batch API.</span></span> <span data-ttu-id="80f2f-149">Ezek a karakterláncok, amíg meg nem látja hello API mindegyikének keresése:</span><span class="sxs-lookup"><span data-stu-id="80f2f-149">Search for each of these strings until you find hello API:</span></span>
    1. <span data-ttu-id="80f2f-150">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-150">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="80f2f-151">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-151">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="80f2f-152">Újabb Azure AD-bérlők ezt a nevet használhatják.</span><span class="sxs-lookup"><span data-stu-id="80f2f-152">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="80f2f-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** hello azonosítója hello kötegelt API.</span><span class="sxs-lookup"><span data-stu-id="80f2f-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** is hello ID for hello Batch API.</span></span> 

5. <span data-ttu-id="80f2f-154">Miután megtalálta hello kötegelt API-t, jelölje ki, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-154">Once you find hello Batch API, select it and click **Save**.</span></span>

    ![Batch-engedélyek hozzáadása][add_permission]

### <a name="create-a-key-vault"></a><span data-ttu-id="80f2f-156">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="80f2f-156">Create a key vault</span></span>
<span data-ttu-id="80f2f-157">Felhasználói előfizetési módban az az Azure key vault kell toothe tartozik, amely ugyanabban az erőforráscsoportban, hello Batch-fiók toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="80f2f-157">In user subscription mode, an Azure key vault is required that belongs toothe same resource group as hello Batch account toobe created.</span></span> <span data-ttu-id="80f2f-158">Ellenőrizze, hogy a terület, ahol kötegelt hello erőforráscsoport van [elérhető](https://azure.microsoft.com/regions/services/) , és amely az előfizetés támogatja.</span><span class="sxs-lookup"><span data-stu-id="80f2f-158">Make sure hello resource group is in a region where Batch is [available](https://azure.microsoft.com/regions/services/) and which your subscription supports.</span></span>

1. <span data-ttu-id="80f2f-159">A hello [Azure-portálon][azure_portal], kattintson a **új** > **biztonság + identitás szakaszában** > **Key Vault** .</span><span class="sxs-lookup"><span data-stu-id="80f2f-159">In hello [Azure portal][azure_portal], click **New** > **Security + Identity** > **Key Vault**.</span></span>

2. <span data-ttu-id="80f2f-160">A hello **kulcstároló létrehozása** panelen adjon meg egy nevet hello kulcstároló, és hozzon létre egy erőforráscsoportot a Batch-fiókhoz használni kívánt hello régióban.</span><span class="sxs-lookup"><span data-stu-id="80f2f-160">In hello **Create Key Vault** blade, enter a name for hello key vault, and create a resource group in hello region you want for your Batch account.</span></span> <span data-ttu-id="80f2f-161">Hagyja hello fennmaradó alapértelmezés szerinti beállításokat, majd kattintson az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-161">Leave hello remaining settings at default values, then click **Create**.</span></span>

### <a name="create-a-batch-account"></a><span data-ttu-id="80f2f-162">Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="80f2f-162">Create a Batch account</span></span>

1. <span data-ttu-id="80f2f-163">A hello [Azure-portálon][azure_portal], kattintson a **új** > **számítási** > **Batch szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-163">In hello [Azure portal][azure_portal], click **New** > **Compute** > **Batch Service**.</span></span>

    ![Köteg hello piactér][marketplace_portal]
3. <span data-ttu-id="80f2f-165">Hello **új Batch-fiók** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="80f2f-165">hello **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="80f2f-166">Lásd: hello leírását alább minden panel elemet.</span><span class="sxs-lookup"><span data-stu-id="80f2f-166">See hello descriptions below of each blade element.</span></span>

    ![Batch-fiók létrehozása][account_portal_byos]

    <span data-ttu-id="80f2f-168">a.</span><span class="sxs-lookup"><span data-stu-id="80f2f-168">a.</span></span> <span data-ttu-id="80f2f-169">**Fióknév**: hello Batch-fiók neve mellett dönt hello Azure-régió, ahol hello-fiók létrehozása belül egyedinek kell lennie (lásd: **hely** alatt).</span><span class="sxs-lookup"><span data-stu-id="80f2f-169">**Account name**: hello Batch account name you choose must be unique within hello Azure region where hello account is created (see **Location** below).</span></span> <span data-ttu-id="80f2f-170">hello fiók neve csak kisbetűket és számokat tartalmazhat, és 3 – 24 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="80f2f-170">hello account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="80f2f-171">b.</span><span class="sxs-lookup"><span data-stu-id="80f2f-171">b.</span></span> <span data-ttu-id="80f2f-172">**Előfizetés**: Ha több előfizetéssel rendelkezik, válassza ki a Batch szolgáltatás hello regisztrált hello-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="80f2f-172">**Subscription**: If you have more than one subscription, select hello subscription that you registered with hello Batch service.</span></span>

    <span data-ttu-id="80f2f-173">c.</span><span class="sxs-lookup"><span data-stu-id="80f2f-173">c.</span></span> <span data-ttu-id="80f2f-174">**Készletfeldolgozási mód**: Válassza a **Felhasználói előfizetés** módot.</span><span class="sxs-lookup"><span data-stu-id="80f2f-174">**Pool allocation mode**: Select **User subscription**.</span></span>

    <span data-ttu-id="80f2f-175">d.</span><span class="sxs-lookup"><span data-stu-id="80f2f-175">d.</span></span> <span data-ttu-id="80f2f-176">**Kulcstároló**: a Batch-fiókhoz hello előző szakaszban létrehozott válassza hello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="80f2f-176">**Key vault**: Select hello key vault you created for your Batch account in hello previous section.</span></span> <span data-ttu-id="80f2f-177">Szükség esetén hozzon létre egy új kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="80f2f-177">Optionally, create a new key vault.</span></span> <span data-ttu-id="80f2f-178">Miután kijelölte a hello tárolóban, jelölje ki a hello jelölőnégyzet toogrant Azure Batch hozzáférés toohello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="80f2f-178">After selecting hello vault, select hello checkbox toogrant Azure Batch access toohello key vault.</span></span>

    <span data-ttu-id="80f2f-179">c.</span><span class="sxs-lookup"><span data-stu-id="80f2f-179">c.</span></span> <span data-ttu-id="80f2f-180">**Erőforráscsoport**: hello kulcstároló létrehozására használt válassza hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="80f2f-180">**Resource group**: Select hello resource group in which you  created hello key vault.</span></span>

    <span data-ttu-id="80f2f-181">d.</span><span class="sxs-lookup"><span data-stu-id="80f2f-181">d.</span></span> <span data-ttu-id="80f2f-182">**Hely**: hello hello kulcstároló hello Batch-fiók létrehozására használt Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="80f2f-182">**Location**: hello Azure region in which you created hello key vault for hello Batch account.</span></span>

    <span data-ttu-id="80f2f-183">e.</span><span class="sxs-lookup"><span data-stu-id="80f2f-183">e.</span></span> <span data-ttu-id="80f2f-184">**Tárfiók** (nem kötelező): Olyan általános célú Azure Storage-fiók, amelyet a Batch-fiókhoz társít.</span><span class="sxs-lookup"><span data-stu-id="80f2f-184">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="80f2f-185">A legtöbb Batch-fiókhoz ajánlott a használata.</span><span class="sxs-lookup"><span data-stu-id="80f2f-185">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="80f2f-186">További részletekért tekintse meg az alábbi, [Társított Azure Storage-fiók](#linked-azure-storage-account) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="80f2f-186">See [Linked Azure Storage account](#linked-azure-storage-account) below for more details.</span></span>

4. <span data-ttu-id="80f2f-187">Kattintson a **létrehozása** toocreate hello fiók.</span><span class="sxs-lookup"><span data-stu-id="80f2f-187">Click **Create** toocreate hello account.</span></span>

   <span data-ttu-id="80f2f-188">hello portál jelzi, hogy központi telepítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="80f2f-188">hello portal indicates deployment is in progress.</span></span> <span data-ttu-id="80f2f-189">A befejezéskor **Az üzembe helyezések sikerültek** értesítés jelenik meg az **Értesítések** területen.</span><span class="sxs-lookup"><span data-stu-id="80f2f-189">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>



## <a name="view-batch-account-properties"></a><span data-ttu-id="80f2f-190">Batch-fiók tulajdonságainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="80f2f-190">View Batch account properties</span></span>
<span data-ttu-id="80f2f-191">Hello fiók létrehozása után megnyithatja hello **Batch-fiók panelen** tooaccess a beállítások és tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="80f2f-191">Once hello account has been created, you can open hello **Batch account blade** tooaccess its settings and properties.</span></span> <span data-ttu-id="80f2f-192">A bal oldali menüjében hello hello Batch-fiók panelen elérheti azokat fiókbeállítások és tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="80f2f-192">You can access all account settings and properties by using hello left menu of hello Batch account blade.</span></span>

![A Batch-fiók panel az Azure Portalon][account_blade]

* <span data-ttu-id="80f2f-194">**A Batch-fiók URL-címe**: Ha a hello alkalmazást fejleszt [kötegelt API-k](batch-apis-tools.md#azure-accounts-for-batch-development), szüksége lesz egy fiók URL-cím tooaccess a kötegelt erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="80f2f-194">**Batch account URL**: When you develop an application with hello [Batch APIs](batch-apis-tools.md#azure-accounts-for-batch-development), you'll need an account URL tooaccess your Batch resources.</span></span> <span data-ttu-id="80f2f-195">A Batch-fiók URL-címe van hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="80f2f-195">A Batch account URL has hello following format:</span></span>

    `https://<account_name>.<region>.batch.azure.com`

![A Batch-fiók URL-címe a portálon][account_url]

* <span data-ttu-id="80f2f-197">**Hívóbetűk** (a Batch szolgáltatás mód): tooauthenticate hozzáférés tooyour Batch-fiókhoz az alkalmazásról, szüksége lesz egy fiók hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="80f2f-197">**Access keys** (Batch service mode): tooauthenticate access tooyour Batch account from your application, you'll need an account access key.</span></span> <span data-ttu-id="80f2f-198">(Ez a beállítás nem áll rendelkezésre felhasználói előfizetés módban, amelyben Azure Active Directory-hitelesítést használ.)</span><span class="sxs-lookup"><span data-stu-id="80f2f-198">(This setting is not available in user subscription mode, where you use Azure Active Directory authentication.)</span></span>

    <span data-ttu-id="80f2f-199">tooview vagy újragenerálása a Batch-fiók hozzáférési kulcsait, adja meg `keys` hello bal oldali menüben **keresési** hello Batch-fiók panelen lévő mezőbe, majd válasszon **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-199">tooview or regenerate your Batch account's access keys, enter `keys` in hello left menu **Search** box on hello Batch account blade, then select **Keys**.</span></span>

    ![A Batch-fiók kulcsai az Azure Portalon][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a><span data-ttu-id="80f2f-201">Társított Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="80f2f-201">Linked Azure Storage account</span></span>

<span data-ttu-id="80f2f-202">Opcionálisan hozzákapcsolhatja egy általános célú Azure Storage-fiók tooyour Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="80f2f-202">You can optionally link a general-purpose Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="80f2f-203">Hello [alkalmazáscsomagok](batch-application-packages.md) a Batch szolgáltatás használja Azure Blob Storage tárolóban hello [kötegelt fájl egyezmények .NET](batch-task-output.md) könyvtár.</span><span class="sxs-lookup"><span data-stu-id="80f2f-203">hello [application packages](batch-application-packages.md) feature of Batch uses Azure Blob storage, as does hello [Batch File Conventions .NET](batch-task-output.md) library.</span></span> <span data-ttu-id="80f2f-204">Ilyen választható szolgáltatások segítséget nyújt a rendszerbe állítása a kötegelt feladatok futtatása hello alkalmazásokat és általuk előállított hello adatait.</span><span class="sxs-lookup"><span data-stu-id="80f2f-204">These optional features assist you in deploying hello applications that your Batch tasks run, and persisting hello data they produce.</span></span>

<span data-ttu-id="80f2f-205">Érdemes létrehozni egy új Storage-fiókot kifejezetten a Batch-fiók általi használatra.</span><span class="sxs-lookup"><span data-stu-id="80f2f-205">We recommend that you create a new Storage account exclusively for use by your Batch account.</span></span>

![Általános célú tárfiók létrehozása][storage_account]

> [!NOTE]
> <span data-ttu-id="80f2f-207">Az Azure Batch jelenleg csak hello általános célú Tárfiók típusa.</span><span class="sxs-lookup"><span data-stu-id="80f2f-207">Azure Batch currently supports only hello general-purpose Storage account type.</span></span> <span data-ttu-id="80f2f-208">Erről a fióktípusról a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) oldal 5. lépésében [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account) talál leírást.</span><span class="sxs-lookup"><span data-stu-id="80f2f-208">This account type is described in step 5, [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

> [!WARNING]
> <span data-ttu-id="80f2f-209">Legyen körültekintő, amikor egy kapcsolódó tárfiók hello tárelérési kulcsok újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="80f2f-209">Be careful when regenerating hello access keys of a linked Storage account.</span></span> <span data-ttu-id="80f2f-210">Csak egy Tárfiók kulcsának újragenerálása, és kattintson a **szinkronizálási kulcsok** hello a kapcsolódó Storage-fiók panelen.</span><span class="sxs-lookup"><span data-stu-id="80f2f-210">Regenerate only one Storage account key and click **Sync Keys** on hello linked Storage account blade.</span></span> <span data-ttu-id="80f2f-211">Várjon, amíg tooallow hello kulcsok toopropagate toohello számítási csomópontjain a készletek, majd újbóli létrehozása és szinkronizálása öt percen belül hello más kulcsot, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="80f2f-211">Wait five minutes tooallow hello keys toopropagate toohello compute nodes in your pools, then regenerate and synchronize hello other key if necessary.</span></span> <span data-ttu-id="80f2f-212">Ha mindkét újragenerálja a kulcsok: hello azonos időben, a számítási csomópontok nem lesz képes toosynchronize mindkét kulcsot, és elvesztik a hozzáférést toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="80f2f-212">If you regenerate both keys at hello same time, your compute nodes will not be able toosynchronize either key, and they will lose access toohello Storage account.</span></span>
>
>

<span data-ttu-id="80f2f-213">![A tárfiók hozzáférési kulcsainak ismételt létrehozása][4]</span><span class="sxs-lookup"><span data-stu-id="80f2f-213">![Regenerating storage account keys][4]</span></span>

## <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="80f2f-214">A Bach szolgáltatás kvótái és korlátozásai</span><span class="sxs-lookup"><span data-stu-id="80f2f-214">Batch service quotas and limits</span></span>
<span data-ttu-id="80f2f-215">Felhívjuk a vegye figyelembe, hogy, az Azure-előfizetés és más Azure-szolgáltatások, bizonyos [kvótái és korlátai](batch-quota-limit.md) tooBatch alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="80f2f-215">Please be aware that as with your Azure subscription and other Azure services, certain [quotas and limits](batch-quota-limit.md) apply tooBatch accounts.</span></span> <span data-ttu-id="80f2f-216">Batch-fiók aktuális kvótái hello fiók hello portálon jelennek meg **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="80f2f-216">Current quotas for a Batch account appear in hello portal in hello account **Properties**.</span></span>

![Batch-fiókra vonatkozó kvóták az Azure Portalon][quotas]



<span data-ttu-id="80f2f-218">Ezenkívül ezek mely százalékértékénél kéri számos növelhető egyszerűen az elküldött hello Azure-portálon a termék ingyenes támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="80f2f-218">Additionally, many of these quotas can be increased simply with a free product support request submitted in hello Azure portal.</span></span> <span data-ttu-id="80f2f-219">Lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) talál részletes információt kérő kvóta növekszik.</span><span class="sxs-lookup"><span data-stu-id="80f2f-219">See [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for details on requesting quota increases.</span></span>

## <a name="other-batch-account-management-options"></a><span data-ttu-id="80f2f-220">Egyéb Batch-fiókkezelési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="80f2f-220">Other Batch account management options</span></span>
<span data-ttu-id="80f2f-221">Ezenkívül toousing hello Azure-portálon, is létrehozása és kezelése a Batch-fiókok hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="80f2f-221">In addition toousing hello Azure portal, you can also create and manage Batch accounts with hello following:</span></span>

* [<span data-ttu-id="80f2f-222">Batch – PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="80f2f-222">Batch PowerShell cmdlets</span></span>](batch-powershell-cmdlets-get-started.md)
* [<span data-ttu-id="80f2f-223">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80f2f-223">Azure CLI</span></span>](batch-cli-get-started.md)
* [<span data-ttu-id="80f2f-224">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="80f2f-224">Batch Management .NET</span></span>](batch-management-dotnet.md)

## <a name="next-steps"></a><span data-ttu-id="80f2f-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80f2f-225">Next steps</span></span>
* <span data-ttu-id="80f2f-226">Lásd: hello [Batch funkcióinak áttekintése](batch-api-basics.md) toolearn további kötegelt szolgáltatással kapcsolatos fogalmak és szolgáltatásokkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="80f2f-226">See hello [Batch feature overview](batch-api-basics.md) toolearn more about Batch service concepts and features.</span></span> <span data-ttu-id="80f2f-227">hello cikk ismerteti hello elsődleges kötegelt erőforrások, például a készletek, a számítási csomópontokat, a feladatok és a feladatokat, és hello áttekintése, amelyek lehetővé teszik a nagyméretű számítási munkaterhelés végrehajtási szolgáltatás szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="80f2f-227">hello article discusses hello primary Batch resources such as pools, compute nodes, jobs, and tasks, and provides an overview of hello service's features that enable large-scale compute workload execution.</span></span>
* <span data-ttu-id="80f2f-228">A Batch-kompatibilis alkalmazások hello használata fejlődő hello alapvető [Batch .NET ügyféloldali kódtár](batch-dotnet-get-started.md) vagy [Python](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="80f2f-228">Learn hello basics of developing a Batch-enabled application using hello [Batch .NET client library](batch-dotnet-get-started.md) or [Python](batch-python-tutorial.md).</span></span> <span data-ttu-id="80f2f-229">A bevezető cikkeiben végigvezeti egy működő alkalmazást, amely hello Batch szolgáltatás tooexecute a munkaterhelés használ több számítási csomóponton, és a munkaterhelés fájl átmeneti és lekérése az Azure Storage használatát magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="80f2f-229">These introductory articles guide you through a working application that uses hello Batch service tooexecute a workload on multiple compute nodes, and includes using Azure Storage for workload file staging and retrieval.</span></span>

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "A tárfiók hozzáférési kulcsainak ismételt létrehozása"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
