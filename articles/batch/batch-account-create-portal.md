---
title: "Batch-fiók létrehozása az Azure Portalon | Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre Azure Batch-fiókot az Azure Portalon nagyméretű párhuzamos számítási feladatok futtatásához a felhőben"
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
ms.openlocfilehash: 520d1d42d35b25db1a35d4317e9eb616cf5de565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-batch-account-with-the-azure-portal"></a><span data-ttu-id="5a275-103">Batch-fiók létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="5a275-103">Create a Batch account with the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5a275-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5a275-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="5a275-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="5a275-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
>
>

<span data-ttu-id="5a275-106">Megtudhatja, hogyan hozhat létre Azure Batch-fiókot az [Azure Portalon][azure_portal], és miként választhatja ki a számítási forgatókönyvének legmegfelelőbb fióktulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="5a275-106">Learn how to create an Azure Batch account in the [Azure portal][azure_portal], and choose the account properties that fit your compute scenario.</span></span> <span data-ttu-id="5a275-107">Megtudhatja, hol találja a fontos fióktulajdonságokat, például a hozzáférési kulcsokat és a fiókok URL-címeit.</span><span class="sxs-lookup"><span data-stu-id="5a275-107">Learn where to find important account properties like access keys and account URLs.</span></span>

<span data-ttu-id="5a275-108">További ismereteket a Batch-fiókokról és -forgatókönyvekről a [funkciók áttekintésében](batch-api-basics.md) találhat.</span><span class="sxs-lookup"><span data-stu-id="5a275-108">For background about Batch accounts and scenarios, see the [feature overview](batch-api-basics.md).</span></span>



## <a name="create-a-batch-account"></a><span data-ttu-id="5a275-109">Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a275-109">Create a Batch account</span></span>

<span data-ttu-id="5a275-110">A portálon a két *készletfelosztási mód* egyikének használatával hozhat létre Batch-fiókot: a **Batch szolgáltatás** módban vagy az újabb, **felhasználói előfizetés** módban, amely több konfigurálást igényel.</span><span class="sxs-lookup"><span data-stu-id="5a275-110">Use the portal to create a Batch account in one of the two *pool allocation modes*: **Batch service** mode or the newer **user subscription** mode, which requires more configuration.</span></span> <span data-ttu-id="5a275-111">További információt a két módról a [funkciók áttekintésében](batch-api-basics.md#account) találhat.</span><span class="sxs-lookup"><span data-stu-id="5a275-111">For information about these two modes, see the [feature overview](batch-api-basics.md#account).</span></span> <span data-ttu-id="5a275-112">A felhasználói előfizetés mód funkcióiról a [blogbejegyzésben](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/) is olvashat.</span><span class="sxs-lookup"><span data-stu-id="5a275-112">For features of the user subscription mode, see also the [blog post](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).</span></span>

## <a name="batch-service-mode"></a><span data-ttu-id="5a275-113">Batch szolgáltatás mód</span><span class="sxs-lookup"><span data-stu-id="5a275-113">Batch service mode</span></span>



1. <span data-ttu-id="5a275-114">Jelentkezzen be az [Azure Portalra][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="5a275-114">Sign in to the [Azure portal][azure_portal].</span></span>
2. <span data-ttu-id="5a275-115">Kattintson az **Új** > **Számítás** > **Batch-szolgáltatás** elemre.</span><span class="sxs-lookup"><span data-stu-id="5a275-115">Click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch a Piactéren][marketplace_portal]
3. <span data-ttu-id="5a275-117">Megjelenik az **Új Batch-fiók** panel.</span><span class="sxs-lookup"><span data-stu-id="5a275-117">The **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="5a275-118">Az egyes panelelemek leírását alább találja.</span><span class="sxs-lookup"><span data-stu-id="5a275-118">See the descriptions below of each blade element.</span></span>

    ![Batch-fiók létrehozása][account_portal]

    <span data-ttu-id="5a275-120">a.</span><span class="sxs-lookup"><span data-stu-id="5a275-120">a.</span></span> <span data-ttu-id="5a275-121">**Fióknév**: A választott Batch-fióknévnek egyedinek kell lennie az Azure-régióban, amelyben az új fiókot létrehozza (lásd **Hely** alább).</span><span class="sxs-lookup"><span data-stu-id="5a275-121">**Account name**: The Batch account name you choose must be unique within the Azure region where the account is created (see **Location** below).</span></span> <span data-ttu-id="5a275-122">A fiók neve csak kisbetűket vagy számokat tartalmazhat, és 3–24 karakter hosszúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5a275-122">The account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="5a275-123">b.</span><span class="sxs-lookup"><span data-stu-id="5a275-123">b.</span></span> <span data-ttu-id="5a275-124">**Előfizetés**: A Batch-fiók létrehozására szolgáló előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5a275-124">**Subscription**: The subscription in which to create the Batch account.</span></span> <span data-ttu-id="5a275-125">Ha csak egy előfizetéssel rendelkezik, ez alapértelmezés szerint be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="5a275-125">If you have only one subscription, it is selected by default.</span></span>

    <span data-ttu-id="5a275-126">c.</span><span class="sxs-lookup"><span data-stu-id="5a275-126">c.</span></span> <span data-ttu-id="5a275-127">**Készletfelosztási mód**: Válassza a **Batch szolgáltatás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5a275-127">**Pool allocation mode**: Select **Batch service**.</span></span>

    <span data-ttu-id="5a275-128">c.</span><span class="sxs-lookup"><span data-stu-id="5a275-128">c.</span></span> <span data-ttu-id="5a275-129">**Erőforráscsoport**: Kiválaszthat egy meglévő erőforráscsoportot az új Batch-fiókhoz, vagy újat is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5a275-129">**Resource group**: Select an existing resource group for your new Batch account, or optionally create a new one.</span></span>

    <span data-ttu-id="5a275-130">d.</span><span class="sxs-lookup"><span data-stu-id="5a275-130">d.</span></span> <span data-ttu-id="5a275-131">**Hely**: Az az Azure-régió, amelyben a Batch-fiókot létrehozza.</span><span class="sxs-lookup"><span data-stu-id="5a275-131">**Location**: The Azure region in which to create the Batch account.</span></span> <span data-ttu-id="5a275-132">Csak az előfizetése és az erőforráscsoportja által támogatott régiók jelennek meg lehetőségként.</span><span class="sxs-lookup"><span data-stu-id="5a275-132">Only the regions supported by your subscription and resource group are displayed as options.</span></span>

    <span data-ttu-id="5a275-133">e.</span><span class="sxs-lookup"><span data-stu-id="5a275-133">e.</span></span> <span data-ttu-id="5a275-134">**Tárfiók** (nem kötelező): Olyan általános célú Azure Storage-fiók, amelyet a Batch-fiókhoz társít.</span><span class="sxs-lookup"><span data-stu-id="5a275-134">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="5a275-135">A legtöbb Batch-fiókhoz ajánlott a használata.</span><span class="sxs-lookup"><span data-stu-id="5a275-135">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="5a275-136">További részletekért tekintse meg a lentebb található [Társított Azure Storage-fiók](#linked-azure-storage-account) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="5a275-136">See [Linked Azure Storage account](#linked-azure-storage-account) later in this article for more details.</span></span>

4. <span data-ttu-id="5a275-137">A fiók létrehozásához kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a275-137">Click **Create** to create the account.</span></span>

   <span data-ttu-id="5a275-138">A portál jelzi a folyamatban lévő üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="5a275-138">The portal indicates deployment is in progress.</span></span> <span data-ttu-id="5a275-139">A befejezéskor **Az üzembe helyezések sikerültek** értesítés jelenik meg az **Értesítések** területen.</span><span class="sxs-lookup"><span data-stu-id="5a275-139">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>

## <a name="user-subscription-mode"></a><span data-ttu-id="5a275-140">Felhasználói előfizetés mód</span><span class="sxs-lookup"><span data-stu-id="5a275-140">User subscription mode</span></span>

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a><span data-ttu-id="5a275-141">Az előfizetés elérésének engedélyezése az Azure Batch számára (egyszeri művelet)</span><span class="sxs-lookup"><span data-stu-id="5a275-141">Allow Azure Batch to access the subscription (one-time operation)</span></span>
<span data-ttu-id="5a275-142">Amikor először hoz létre Batch-fiókot felhasználói előfizetés módban, az alábbi lépéseket követve regisztrálja az előfizetését a Batch szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="5a275-142">When creating your first Batch account in user subscription mode, perform the following steps to register your subscription with Batch.</span></span> <span data-ttu-id="5a275-143">(Ha korábban már regisztrált, ugorjon a következő szakaszra.)</span><span class="sxs-lookup"><span data-stu-id="5a275-143">(If you previously did this, skip to the next section.)</span></span>

1. <span data-ttu-id="5a275-144">Jelentkezzen be az [Azure Portalra][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="5a275-144">Sign in to the [Azure portal][azure_portal].</span></span>

2. <span data-ttu-id="5a275-145">Kattintson a **További szolgáltatások** > **Előfizetések** elemre, majd a Batch-fiókhoz használni kívánt előfizetésre.</span><span class="sxs-lookup"><span data-stu-id="5a275-145">Click **More Services** > **Subscriptions**, and click the subscription you want to use for the Batch account.</span></span>

3. <span data-ttu-id="5a275-146">Az **Előfizetés** panelen kattintson a **Hozzáférés-vezérlés (IAM)** > **Hozzáadás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="5a275-146">In the **Subscription** blade, click **Access control (IAM)** > **Add**.</span></span>

    ![Az előfizetéshez való hozzáférés vezérlése][subscription_access]

4. <span data-ttu-id="5a275-148">Az **Engedélyek hozzáadása** panelen válassza a **Közreműködő** szerepkört, és keressen rá a Batch API kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="5a275-148">On the **Add permissions** blade, select the **Contributor** role, search for the Batch API.</span></span> <span data-ttu-id="5a275-149">Keressen rá az egyes karakterláncokra, addig amíg meg nem találja az API-t:</span><span class="sxs-lookup"><span data-stu-id="5a275-149">Search for each of these strings until you find the API:</span></span>
    1. <span data-ttu-id="5a275-150">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="5a275-150">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="5a275-151">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="5a275-151">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="5a275-152">Újabb Azure AD-bérlők ezt a nevet használhatják.</span><span class="sxs-lookup"><span data-stu-id="5a275-152">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="5a275-153">A **ddbf3205-c6bd-46ae-8127-60eb93363864** a Batch API azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5a275-153">**ddbf3205-c6bd-46ae-8127-60eb93363864** is the ID for the Batch API.</span></span> 

5. <span data-ttu-id="5a275-154">Miután megtalálta a Batch API-t, jelölje ki, majd kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a275-154">Once you find the Batch API, select it and click **Save**.</span></span>

    ![Batch-engedélyek hozzáadása][add_permission]

### <a name="create-a-key-vault"></a><span data-ttu-id="5a275-156">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a275-156">Create a key vault</span></span>
<span data-ttu-id="5a275-157">Felhasználói előfizetés módban olyan Azure Key Vault szükséges, amely ugyanahhoz az erőforráscsoporthoz tartozik, mint a létrehozandó Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="5a275-157">In user subscription mode, an Azure key vault is required that belongs to the same resource group as the Batch account to be created.</span></span> <span data-ttu-id="5a275-158">Győződjön meg arról, hogy az erőforráscsoport olyan régióban található, amelyben [elérhető](https://azure.microsoft.com/regions/services/) a Batch szolgáltatás, és amelyet támogat az előfizetése.</span><span class="sxs-lookup"><span data-stu-id="5a275-158">Make sure the resource group is in a region where Batch is [available](https://azure.microsoft.com/regions/services/) and which your subscription supports.</span></span>

1. <span data-ttu-id="5a275-159">Az [Azure Portalon][azure_portal] kattintson az **Új** > **Biztonság és identitás** > **Kulcstartó** elemre.</span><span class="sxs-lookup"><span data-stu-id="5a275-159">In the [Azure portal][azure_portal], click **New** > **Security + Identity** > **Key Vault**.</span></span>

2. <span data-ttu-id="5a275-160">A **Kulcstartó létrehozása** panelen adja meg a kulcstartó nevét, és hozzon létre a régióban egy erőforráscsoportot a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="5a275-160">In the **Create Key Vault** blade, enter a name for the key vault, and create a resource group in the region you want for your Batch account.</span></span> <span data-ttu-id="5a275-161">A többi beállításnál hagyja meg az alapértelmezett értéket, majd kattintson a **Létrehozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="5a275-161">Leave the remaining settings at default values, then click **Create**.</span></span>

### <a name="create-a-batch-account"></a><span data-ttu-id="5a275-162">Batch-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a275-162">Create a Batch account</span></span>

1. <span data-ttu-id="5a275-163">Az [Azure Portalon][azure_portal] kattintson az **Új** > **Számítás** > **Batch szolgáltatás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="5a275-163">In the [Azure portal][azure_portal], click **New** > **Compute** > **Batch Service**.</span></span>

    ![Batch a Piactéren][marketplace_portal]
3. <span data-ttu-id="5a275-165">Megjelenik az **Új Batch-fiók** panel.</span><span class="sxs-lookup"><span data-stu-id="5a275-165">The **New Batch Account** blade is displayed.</span></span> <span data-ttu-id="5a275-166">Az egyes panelelemek leírását alább találja.</span><span class="sxs-lookup"><span data-stu-id="5a275-166">See the descriptions below of each blade element.</span></span>

    ![Batch-fiók létrehozása][account_portal_byos]

    <span data-ttu-id="5a275-168">a.</span><span class="sxs-lookup"><span data-stu-id="5a275-168">a.</span></span> <span data-ttu-id="5a275-169">**Fióknév**: A választott Batch-fióknévnek egyedinek kell lennie az Azure-régióban, amelyben az új fiókot létrehozza (lásd **Hely** alább).</span><span class="sxs-lookup"><span data-stu-id="5a275-169">**Account name**: The Batch account name you choose must be unique within the Azure region where the account is created (see **Location** below).</span></span> <span data-ttu-id="5a275-170">A fiók neve csak kisbetűket vagy számokat tartalmazhat, és 3–24 karakter hosszúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5a275-170">The account name may contain only lowercase characters or numbers, and must be 3-24 characters in length.</span></span>

    <span data-ttu-id="5a275-171">b.</span><span class="sxs-lookup"><span data-stu-id="5a275-171">b.</span></span> <span data-ttu-id="5a275-172">**Előfizetés**: Ha egynél több előfizetéssel rendelkezik, akkor válassza ki azt, amelyet a Batch szolgáltatáshoz regisztrált.</span><span class="sxs-lookup"><span data-stu-id="5a275-172">**Subscription**: If you have more than one subscription, select the subscription that you registered with the Batch service.</span></span>

    <span data-ttu-id="5a275-173">c.</span><span class="sxs-lookup"><span data-stu-id="5a275-173">c.</span></span> <span data-ttu-id="5a275-174">**Készletfeldolgozási mód**: Válassza a **Felhasználói előfizetés** módot.</span><span class="sxs-lookup"><span data-stu-id="5a275-174">**Pool allocation mode**: Select **User subscription**.</span></span>

    <span data-ttu-id="5a275-175">d.</span><span class="sxs-lookup"><span data-stu-id="5a275-175">d.</span></span> <span data-ttu-id="5a275-176">**Kulcstartó**: válassza ki azt a kulcstartót, amelyet az előző szakaszban leírtak szerint hozott létre a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="5a275-176">**Key vault**: Select the key vault you created for your Batch account in the previous section.</span></span> <span data-ttu-id="5a275-177">Szükség esetén hozzon létre egy új kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="5a275-177">Optionally, create a new key vault.</span></span> <span data-ttu-id="5a275-178">A kulcstartó kiválasztása után a jelölőnégyzet bejelölésével engedélyezze az Azure Batch számára a kulcstartó elérését.</span><span class="sxs-lookup"><span data-stu-id="5a275-178">After selecting the vault, select the checkbox to grant Azure Batch access to the key vault.</span></span>

    <span data-ttu-id="5a275-179">c.</span><span class="sxs-lookup"><span data-stu-id="5a275-179">c.</span></span> <span data-ttu-id="5a275-180">**Erőforráscsoport**: Válassza ki azt az erőforráscsoportot, amelyben létrehozta a kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="5a275-180">**Resource group**: Select the resource group in which you  created the key vault.</span></span>

    <span data-ttu-id="5a275-181">d.</span><span class="sxs-lookup"><span data-stu-id="5a275-181">d.</span></span> <span data-ttu-id="5a275-182">**Hely**: Az az Azure-régió, amelyben létrehozta a Batch-fiókhoz tartozó kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="5a275-182">**Location**: The Azure region in which you created the key vault for the Batch account.</span></span>

    <span data-ttu-id="5a275-183">e.</span><span class="sxs-lookup"><span data-stu-id="5a275-183">e.</span></span> <span data-ttu-id="5a275-184">**Tárfiók** (nem kötelező): Olyan általános célú Azure Storage-fiók, amelyet a Batch-fiókhoz társít.</span><span class="sxs-lookup"><span data-stu-id="5a275-184">**Storage account** (optional): A general-purpose Azure Storage account that you associate with your Batch account.</span></span> <span data-ttu-id="5a275-185">A legtöbb Batch-fiókhoz ajánlott a használata.</span><span class="sxs-lookup"><span data-stu-id="5a275-185">This is recommended for most Batch accounts.</span></span> <span data-ttu-id="5a275-186">További részletekért tekintse meg az alábbi, [Társított Azure Storage-fiók](#linked-azure-storage-account) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="5a275-186">See [Linked Azure Storage account](#linked-azure-storage-account) below for more details.</span></span>

4. <span data-ttu-id="5a275-187">A fiók létrehozásához kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a275-187">Click **Create** to create the account.</span></span>

   <span data-ttu-id="5a275-188">A portál jelzi a folyamatban lévő üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="5a275-188">The portal indicates deployment is in progress.</span></span> <span data-ttu-id="5a275-189">A befejezéskor **Az üzembe helyezések sikerültek** értesítés jelenik meg az **Értesítések** területen.</span><span class="sxs-lookup"><span data-stu-id="5a275-189">Upon completion, a **Deployments succeeded** notification appears in **Notifications**.</span></span>



## <a name="view-batch-account-properties"></a><span data-ttu-id="5a275-190">Batch-fiók tulajdonságainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="5a275-190">View Batch account properties</span></span>
<span data-ttu-id="5a275-191">A fiók létrehozása után megnyithatja a **Batch-fiók panelt** a beállítások és tulajdonságok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5a275-191">Once the account has been created, you can open the **Batch account blade** to access its settings and properties.</span></span> <span data-ttu-id="5a275-192">Az összes fiókbeállítást és tulajdonságot elérheti a Batch-fiók panelének bal oldali menüjéből.</span><span class="sxs-lookup"><span data-stu-id="5a275-192">You can access all account settings and properties by using the left menu of the Batch account blade.</span></span>

![A Batch-fiók panel az Azure Portalon][account_blade]

* <span data-ttu-id="5a275-194">**Batch-fiók URL-cím**: Amikor egy alkalmazást fejleszt a [Batch API-kkal](batch-apis-tools.md#azure-accounts-for-batch-development), szüksége lesz egy fiók URL-címére a Batch-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5a275-194">**Batch account URL**: When you develop an application with the [Batch APIs](batch-apis-tools.md#azure-accounts-for-batch-development), you'll need an account URL to access your Batch resources.</span></span> <span data-ttu-id="5a275-195">A Batch-fiók URL-címének formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="5a275-195">A Batch account URL has the following format:</span></span>

    `https://<account_name>.<region>.batch.azure.com`

![A Batch-fiók URL-címe a portálon][account_url]

* <span data-ttu-id="5a275-197">**Hozzáférési kulcsok** (Batch szolgáltatás mód): Ahhoz, hogy hozzáférjen a Batch-fiókhoz az alkalmazásból, szüksége lesz a fiók hozzáférési kulcsára.</span><span class="sxs-lookup"><span data-stu-id="5a275-197">**Access keys** (Batch service mode): To authenticate access to your Batch account from your application, you'll need an account access key.</span></span> <span data-ttu-id="5a275-198">(Ez a beállítás nem áll rendelkezésre felhasználói előfizetés módban, amelyben Azure Active Directory-hitelesítést használ.)</span><span class="sxs-lookup"><span data-stu-id="5a275-198">(This setting is not available in user subscription mode, where you use Azure Active Directory authentication.)</span></span>

    <span data-ttu-id="5a275-199">A Batch-fiók hozzáférési kulcsainak megtekintéséhez vagy újbóli létrehozásához írja be a `keys` kifejezést a bal oldali menü **Keresés** mezőjébe a Batch-fiók panelen, majd válassza a **Kulcsok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5a275-199">To view or regenerate your Batch account's access keys, enter `keys` in the left menu **Search** box on the Batch account blade, then select **Keys**.</span></span>

    ![A Batch-fiók kulcsai az Azure Portalon][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a><span data-ttu-id="5a275-201">Társított Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="5a275-201">Linked Azure Storage account</span></span>

<span data-ttu-id="5a275-202">Dönthet úgy, hogy általános célú Azure Storage-fiókot csatol a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="5a275-202">You can optionally link a general-purpose Azure Storage account to your Batch account.</span></span> <span data-ttu-id="5a275-203">A Batch [alkalmazáscsomagok](batch-application-packages.md) funkciója Azure Blob Storage-ot használ, ahogyan a [Batch File Conventions .NET](batch-task-output.md) könyvtár is.</span><span class="sxs-lookup"><span data-stu-id="5a275-203">The [application packages](batch-application-packages.md) feature of Batch uses Azure Blob storage, as does the [Batch File Conventions .NET](batch-task-output.md) library.</span></span> <span data-ttu-id="5a275-204">Ezek a választható funkciók segítik a Batch-feladatok által futtatott alkalmazások üzembe helyezését és az általuk létrehozott adatok megőrzését.</span><span class="sxs-lookup"><span data-stu-id="5a275-204">These optional features assist you in deploying the applications that your Batch tasks run, and persisting the data they produce.</span></span>

<span data-ttu-id="5a275-205">Érdemes létrehozni egy új Storage-fiókot kifejezetten a Batch-fiók általi használatra.</span><span class="sxs-lookup"><span data-stu-id="5a275-205">We recommend that you create a new Storage account exclusively for use by your Batch account.</span></span>

![Általános célú tárfiók létrehozása][storage_account]

> [!NOTE]
> <span data-ttu-id="5a275-207">Az Azure Batch jelenleg kizárólag az általános célú Storage-fiók típusát támogatja.</span><span class="sxs-lookup"><span data-stu-id="5a275-207">Azure Batch currently supports only the general-purpose Storage account type.</span></span> <span data-ttu-id="5a275-208">Erről a fióktípusról a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) oldal 5. lépésében [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account) talál leírást.</span><span class="sxs-lookup"><span data-stu-id="5a275-208">This account type is described in step 5, [Create a storage account] (../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

> [!WARNING]
> <span data-ttu-id="5a275-209">Körültekintően járjon el, amikor újból létrehozza a társított Storage-fiók hozzáférési kulcsait.</span><span class="sxs-lookup"><span data-stu-id="5a275-209">Be careful when regenerating the access keys of a linked Storage account.</span></span> <span data-ttu-id="5a275-210">A Storage-fiókhoz csak egy hozzáférési kulcsot hozzon létre ismét, és kattintson a társított tárfiók panelén a **Kulcsok szinkronizálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a275-210">Regenerate only one Storage account key and click **Sync Keys** on the linked Storage account blade.</span></span> <span data-ttu-id="5a275-211">Várjon öt percet, hogy a rendszer propagálja a hozzáférési kulcsokat a készletekben található számítási csomópontokra, majd szükség esetén hozza létre újra és szinkronizálja a másik hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="5a275-211">Wait five minutes to allow the keys to propagate to the compute nodes in your pools, then regenerate and synchronize the other key if necessary.</span></span> <span data-ttu-id="5a275-212">Ha mindkét hozzáférési kulcsot egyszerre hozza létre újra, a számítási csomópontok nem tudják szinkronizálni egyiket sem, és elveszítik a Storage-fiókhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="5a275-212">If you regenerate both keys at the same time, your compute nodes will not be able to synchronize either key, and they will lose access to the Storage account.</span></span>
>
>

<span data-ttu-id="5a275-213">![A tárfiók hozzáférési kulcsainak ismételt létrehozása][4]</span><span class="sxs-lookup"><span data-stu-id="5a275-213">![Regenerating storage account keys][4]</span></span>

## <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="5a275-214">A Bach szolgáltatás kvótái és korlátozásai</span><span class="sxs-lookup"><span data-stu-id="5a275-214">Batch service quotas and limits</span></span>
<span data-ttu-id="5a275-215">Vegye figyelembe, hogy az Azure-előfizetéshez és más Azure-szolgáltatásokhoz hasonlóan a Batch-fiókokra is bizonyos [kvóták és korlátozások](batch-quota-limit.md) érvényesek.</span><span class="sxs-lookup"><span data-stu-id="5a275-215">Please be aware that as with your Azure subscription and other Azure services, certain [quotas and limits](batch-quota-limit.md) apply to Batch accounts.</span></span> <span data-ttu-id="5a275-216">A Batch-fiókok aktuális kvótái a **Tulajdonságok** fiókban jelennek meg a portálon.</span><span class="sxs-lookup"><span data-stu-id="5a275-216">Current quotas for a Batch account appear in the portal in the account **Properties**.</span></span>

![Batch-fiókra vonatkozó kvóták az Azure Portalon][quotas]



<span data-ttu-id="5a275-218">Ezenkívül, sok kvóta egyszerűen növelhető az Azure Portalra elküldött ingyenes terméktámogatási kéréssel.</span><span class="sxs-lookup"><span data-stu-id="5a275-218">Additionally, many of these quotas can be increased simply with a free product support request submitted in the Azure portal.</span></span> <span data-ttu-id="5a275-219">A kvótanövelések kéréséről részletekért lásd: [Quotas and limits for the Azure Batch service](batch-quota-limit.md) (Az Azure Batch szolgáltatás kvótái és korlátai).</span><span class="sxs-lookup"><span data-stu-id="5a275-219">See [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for details on requesting quota increases.</span></span>

## <a name="other-batch-account-management-options"></a><span data-ttu-id="5a275-220">Egyéb Batch-fiókkezelési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="5a275-220">Other Batch account management options</span></span>
<span data-ttu-id="5a275-221">Az Azure Portal használata mellett a következőkkel is létrehozhat és kezelhet Batch-fiókokat:</span><span class="sxs-lookup"><span data-stu-id="5a275-221">In addition to using the Azure portal, you can also create and manage Batch accounts with the following:</span></span>

* [<span data-ttu-id="5a275-222">Batch – PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="5a275-222">Batch PowerShell cmdlets</span></span>](batch-powershell-cmdlets-get-started.md)
* [<span data-ttu-id="5a275-223">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5a275-223">Azure CLI</span></span>](batch-cli-get-started.md)
* [<span data-ttu-id="5a275-224">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="5a275-224">Batch Management .NET</span></span>](batch-management-dotnet.md)

## <a name="next-steps"></a><span data-ttu-id="5a275-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a275-225">Next steps</span></span>
* <span data-ttu-id="5a275-226">A Batch szolgáltatás fogalmairól és funkcióiról további információt [a Batch funkcióinak áttekintésében](batch-api-basics.md) talál.</span><span class="sxs-lookup"><span data-stu-id="5a275-226">See the [Batch feature overview](batch-api-basics.md) to learn more about Batch service concepts and features.</span></span> <span data-ttu-id="5a275-227">A cikk az elsődleges Batch-erőforrásokat tárgyalja, például a készleteket, a számítási csomópontokat, a feladatokat, a tevékenységeket és a szolgáltatás olyan funkcióinak áttekintését nyújtja, amely lehetővé teszi a nagy méretű számítási feladatok futtatását.</span><span class="sxs-lookup"><span data-stu-id="5a275-227">The article discusses the primary Batch resources such as pools, compute nodes, jobs, and tasks, and provides an overview of the service's features that enable large-scale compute workload execution.</span></span>
* <span data-ttu-id="5a275-228">Megismerheti a Batch-kompatibilis alkalmazások [Batch .NET ügyfélkönyvtárral](batch-dotnet-get-started.md) vagy [Python](batch-python-tutorial.md) segítségével való fejlesztésének alapjait.</span><span class="sxs-lookup"><span data-stu-id="5a275-228">Learn the basics of developing a Batch-enabled application using the [Batch .NET client library](batch-dotnet-get-started.md) or [Python](batch-python-tutorial.md).</span></span> <span data-ttu-id="5a275-229">Ezek a bevezető cikkek végigvezetik egy működő alkalmazáson, amely a Batch szolgáltatással futtat egy számítási feladatot több számítási csomóponton, és az Azure Storage szolgáltatást is használja a számítási feladatok fájljainak előkészítéséhez és lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="5a275-229">These introductory articles guide you through a working application that uses the Batch service to execute a workload on multiple compute nodes, and includes using Azure Storage for workload file staging and retrieval.</span></span>

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
