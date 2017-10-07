---
title: "aaaManage kötegelt fiók erőforrások hello ügyféloldali kódtára a .NET - Azure |} Microsoft Docs"
description: "Létrehozása, törlése és módosítása az Azure Batch erőforrásainak hello Batch Management .NET kódtárral könyvtárhoz."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 638d8129f3841ffcfb2e10f5d531a319bcbb7701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-accounts-and-quotas-with-hello-batch-management-client-library-for-net"></a><span data-ttu-id="2ee92-103">Batch fiókjainak és kvótáinak hello kötegelt felügyeleti ügyféloldali kódtára a .NET-keretrendszerhez készült kezelése</span><span class="sxs-lookup"><span data-stu-id="2ee92-103">Manage Batch accounts and quotas with hello Batch Management client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ee92-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2ee92-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="2ee92-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="2ee92-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
> 
> 

<span data-ttu-id="2ee92-106">Csökkenthető karbantartási terhet az Azure Batch-alkalmazások hello segítségével [Batch Management .NET kódtárral] [ api_mgmt_net] könyvtár tooautomate Batch-fiók létrehozása, törlés, kulcskezelés és kvóta felderítés.</span><span class="sxs-lookup"><span data-stu-id="2ee92-106">You can lower maintenance overhead in your Azure Batch applications by using hello [Batch Management .NET][api_mgmt_net] library tooautomate Batch account creation, deletion, key management, and quota discovery.</span></span>

* <span data-ttu-id="2ee92-107">**Hozzon létre vagy töröljön a Batch-fiókok** bármely régióban.</span><span class="sxs-lookup"><span data-stu-id="2ee92-107">**Create and delete Batch accounts** within any region.</span></span> <span data-ttu-id="2ee92-108">Ha egy független szoftverszállító (ISV), például ad meg, amelyben mindegyik tartozik egy külön Batch-fiók számlázási okokból az ügyfelek a szolgáltatás, a fiók létrehozását és törlését képességek tooyour ügyfélportálra is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="2ee92-108">If, as an independent software vendor (ISV) for example, you provide a service for your clients in which each is assigned a separate Batch account for billing purposes, you can add account creation and deletion capabilities tooyour customer portal.</span></span>
* <span data-ttu-id="2ee92-109">**Beolvasni, majd újra létrehozza a kulcsait** programozott módon az összes, a Batch-fiókok.</span><span class="sxs-lookup"><span data-stu-id="2ee92-109">**Retrieve and regenerate account keys** programmatically for any of your Batch accounts.</span></span> <span data-ttu-id="2ee92-110">Ez segít felel meg a biztonsági házirendek, amelyeket rendszeres helyettesítő vagy kulcsait lejártát.</span><span class="sxs-lookup"><span data-stu-id="2ee92-110">This can help you comply with security policies that enforce periodic rollover or expiry of account keys.</span></span> <span data-ttu-id="2ee92-111">Ha több Batch-fiókok Azure különböző régiókban, a helyettesítő folyamat automation növeli az a megoldás hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="2ee92-111">When you have several Batch accounts in various Azure regions, automation of this rollover process increases your solution's efficiency.</span></span>
* <span data-ttu-id="2ee92-112">**Ellenőrizze a fiók kvóták** hajtanak végre hello próbaverzió hiba munka bizonytalanságát kívül meghatározása, hogy mely Batch-fiókok milyen korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="2ee92-112">**Check account quotas** and take hello trial-and-error guesswork out of determining which Batch accounts have what limits.</span></span> <span data-ttu-id="2ee92-113">A fiók kvótákat feladatok megkezdése előtt ellenőrzésével készletek létrehozása, vagy a számítási csomópontok hozzáadása, hol proaktív módosíthatja, vagy ha ezek a számítási erőforrások jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2ee92-113">By checking your account quotas before starting jobs, creating pools, or adding compute nodes, you can proactively adjust where or when these compute resources are created.</span></span> <span data-ttu-id="2ee92-114">Azt is meghatározhatja, hogy mely fiókok szükséges kvóta növeli az említett további erőforrások hozzárendelése előtt.</span><span class="sxs-lookup"><span data-stu-id="2ee92-114">You can determine which accounts require quota increases before allocating additional resources in those accounts.</span></span>
* <span data-ttu-id="2ee92-115">**Más Azure-szolgáltatások jellemzői egyesítése** teljes körű felügyeleti élmény--a Batch Management .NET kódtárral [Azure Active Directory][aad_about], és hello [Azure Erőforrás-kezelő] [ resman_overview] hello kiírni ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2ee92-115">**Combine features of other Azure services** for a full-featured management experience--by using Batch Management .NET, [Azure Active Directory][aad_about], and hello [Azure Resource Manager][resman_overview] together in hello same application.</span></span> <span data-ttu-id="2ee92-116">Ezeket a szolgáltatásokat és az API-k használatával is frictionless hitelesítési környezetet biztosítson, képes toocreate hello és erőforrás-csoportok és hello képességeket kínál, amelyek egy végpont kezelési megoldás a fent leírt törlése.</span><span class="sxs-lookup"><span data-stu-id="2ee92-116">By using these features and their APIs, you can provide a frictionless authentication experience, hello ability toocreate and delete resource groups, and hello capabilities that are described above for an end-to-end management solution.</span></span>

> [!NOTE]
> <span data-ttu-id="2ee92-117">Amíg ez a cikk foglalkozik a hello programozott felügyeleti a kötegelt fiókok, a kulcsok és a kvóták, hajthat végre ezeket a tevékenységeket számos hello segítségével [Azure-portálon][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="2ee92-117">While this article focuses on hello programmatic management of your Batch accounts, keys, and quotas, you can perform many of these activities by using hello [Azure portal][azure_portal].</span></span> <span data-ttu-id="2ee92-118">További információkért lásd: [hello Azure portál használata az Azure Batch-fiók létrehozása](batch-account-create-portal.md) és [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="2ee92-118">For more information, see [Create an Azure Batch account using hello Azure portal](batch-account-create-portal.md) and [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span>
> 
> 

## <a name="create-and-delete-batch-accounts"></a><span data-ttu-id="2ee92-119">Hozzon létre vagy töröljön a Batch-fiókok</span><span class="sxs-lookup"><span data-stu-id="2ee92-119">Create and delete Batch accounts</span></span>
<span data-ttu-id="2ee92-120">Ahogy azt korábban említettük, egy elsődleges szolgáltatásai hello hello kötegelt felügyeleti API toocreate, és törölje a Batch-fiókok Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="2ee92-120">As mentioned, one of hello primary features of hello Batch Management API is toocreate and delete Batch accounts in an Azure region.</span></span> <span data-ttu-id="2ee92-121">Igen, használjon toodo [BatchManagementClient.Account.CreateAsync] [ net_create] és [DeleteAsync][net_delete], vagy mint a szinkron.</span><span class="sxs-lookup"><span data-stu-id="2ee92-121">toodo so, use [BatchManagementClient.Account.CreateAsync][net_create] and [DeleteAsync][net_delete], or their synchronous counterparts.</span></span>

<span data-ttu-id="2ee92-122">hello következő kódrészletet fiókot hoz létre, újonnan létrehozott fiókot a Batch szolgáltatás hello hello beolvassa és törli őket.</span><span class="sxs-lookup"><span data-stu-id="2ee92-122">hello following code snippet creates an account, obtains hello newly created account from hello Batch service, and then deletes it.</span></span> <span data-ttu-id="2ee92-123">Az e részlet és mások ebben a cikkben hello `batchManagementClient` teljesen inicializált példánya [BatchManagementClient][net_mgmt_client].</span><span class="sxs-lookup"><span data-stu-id="2ee92-123">In this snippet and hello others in this article, `batchManagementClient` is a fully initialized instance of [BatchManagementClient][net_mgmt_client].</span></span>

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get hello new account from hello Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete hello account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> <span data-ttu-id="2ee92-124">Hello Batch Management .NET kódtárral kódtár és BatchManagementClient osztálya használó alkalmazásoknak **szolgáltatás-rendszergazda** vagy **coadministrator** toohello előfizetés hello birtokló eléréséhez A Batch-fiók toobe felügyelt.</span><span class="sxs-lookup"><span data-stu-id="2ee92-124">Applications that use hello Batch Management .NET library and its BatchManagementClient class require **service administrator** or **coadministrator** access toohello subscription that owns hello Batch account toobe managed.</span></span> <span data-ttu-id="2ee92-125">További információkért lásd: hello [Azure Active Directory](#azure-active-directory) szakasz és hello [AccountManagement] [ acct_mgmt_sample] kódminta.</span><span class="sxs-lookup"><span data-stu-id="2ee92-125">For more information, see hello [Azure Active Directory](#azure-active-directory) section and hello [AccountManagement][acct_mgmt_sample] code sample.</span></span>
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a><span data-ttu-id="2ee92-126">Kérje le, majd fiók kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="2ee92-126">Retrieve and regenerate account keys</span></span>
<span data-ttu-id="2ee92-127">Elsődleges és másodlagos kulcsok beszerzése az előfizetésen belül a Batch-fiókból való használatával [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="2ee92-127">Obtain primary and secondary account keys from any Batch account within your subscription by using [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="2ee92-128">Ezeknek a kulcsoknak használatával állíthatja helyre [RegenerateKeyAsync][net_regenerate_keys].</span><span class="sxs-lookup"><span data-stu-id="2ee92-128">You can regenerate those keys by using [RegenerateKeyAsync][net_regenerate_keys].</span></span>

```csharp
// Get and print hello primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate hello primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> <span data-ttu-id="2ee92-129">A felügyeleti alkalmazások zökkenőmentes kapcsolódási munkafolyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2ee92-129">You can create a streamlined connection workflow for your management applications.</span></span> <span data-ttu-id="2ee92-130">Először szerezze be a fiókkulcs hello Batch-fiókhoz való toomanage kívánja [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="2ee92-130">First, obtain an account key for hello Batch account you wish toomanage with [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="2ee92-131">Ezt követően használja ezt a kulcsot, hello Batch .NET kódtár inicializálása közben [BatchSharedKeyCredentials] [ net_sharedkeycred] osztály, amely a inicializálása [BatchClient] [net_batch_client].</span><span class="sxs-lookup"><span data-stu-id="2ee92-131">Then, use this key when initializing hello Batch .NET library's [BatchSharedKeyCredentials][net_sharedkeycred] class, which is used when initializing [BatchClient][net_batch_client].</span></span>
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a><span data-ttu-id="2ee92-132">Ellenőrizze az Azure-előfizetés és a Batch-fiók kvóták</span><span class="sxs-lookup"><span data-stu-id="2ee92-132">Check Azure subscription and Batch account quotas</span></span>
<span data-ttu-id="2ee92-133">Azure-előfizetések és hello egyes Azure-szolgáltatásokkal, mint a Batch-összes rendelkezik alapértelmezett kvóták korlátozó bizonyos entitások bennük hello száma.</span><span class="sxs-lookup"><span data-stu-id="2ee92-133">Azure subscriptions and hello individual Azure services like Batch all have default quotas that limit hello number of certain entities within them.</span></span> <span data-ttu-id="2ee92-134">Azure-előfizetések hello alapértelmezett kvótái, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="2ee92-134">For hello default quotas for Azure subscriptions, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="2ee92-135">Hello alapértelmezett hello Batch szolgáltatás kvótái, lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="2ee92-135">For hello default quotas of hello Batch service, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="2ee92-136">Hello Batch Management .NET kódtárral könyvtár használatával ellenőrizheti az alkalmazások ezek mely százalékértékénél kéri.</span><span class="sxs-lookup"><span data-stu-id="2ee92-136">By using hello Batch Management .NET library, you can check these quotas in your applications.</span></span> <span data-ttu-id="2ee92-137">Ez lehetővé teszi toomake foglalási döntések fiókok hozzáadása, vagy a számítási erőforrásokhoz, mint a gyűjtők és számítási csomópontok előtt.</span><span class="sxs-lookup"><span data-stu-id="2ee92-137">This enables you toomake allocation decisions before you add accounts or compute resources like pools and compute nodes.</span></span>

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a><span data-ttu-id="2ee92-138">Ellenőrizze a kötegelt fiók kvóták az Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="2ee92-138">Check an Azure subscription for Batch account quotas</span></span>
<span data-ttu-id="2ee92-139">Batch-fiók létrehozása a régióban, előtt ellenőrizheti az Azure-előfizetés toosee képes tooadd egy fiókot az adott régióban van.</span><span class="sxs-lookup"><span data-stu-id="2ee92-139">Before creating a Batch account in a region, you can check your Azure subscription toosee whether you are able tooadd an account in that region.</span></span>

<span data-ttu-id="2ee92-140">Hello kódrészletben, először használjuk [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] tooget összes Batch-fiókot, amely egy előfizetésen belül gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="2ee92-140">In hello code snippet below, we first use [BatchManagementClient.Account.ListAsync][net_mgmt_listaccounts] tooget a collection of all Batch accounts that are within a subscription.</span></span> <span data-ttu-id="2ee92-141">Amint azt korábban beszerzett ehhez a gyűjteményhez, azt határozza meg, hány fiókhoz hello cél területen vannak.</span><span class="sxs-lookup"><span data-stu-id="2ee92-141">Once we've obtained this collection, we determine how many accounts are in hello target region.</span></span> <span data-ttu-id="2ee92-142">Akkor használjuk [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] tooobtain hello Batch-fiókkvótája, és határozza meg, hány fiókhoz (ha vannak) az adott régióban is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="2ee92-142">Then we use [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] tooobtain hello Batch account quota and determine how many accounts (if any) can be created in that region.</span></span>

```csharp
// Get a collection of all Batch accounts within hello subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within hello target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get hello account quota for hello specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in hello target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in hello {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

<span data-ttu-id="2ee92-143">A fenti, hello részlet `creds` példánya [TokenCloudCredentials][azure_tokencreds].</span><span class="sxs-lookup"><span data-stu-id="2ee92-143">In hello snippet above, `creds` is an instance of [TokenCloudCredentials][azure_tokencreds].</span></span> <span data-ttu-id="2ee92-144">toosee létrehozná ezt az objektumot, például lásd: hello [AccountManagement] [ acct_mgmt_sample] kód mintát a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2ee92-144">toosee an example of creating this object, see hello [AccountManagement][acct_mgmt_sample] code sample on GitHub.</span></span>

### <a name="check-a-batch-account-for-compute-resource-quotas"></a><span data-ttu-id="2ee92-145">Ellenőrizze a számítási erőforrás kvóták Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="2ee92-145">Check a Batch account for compute resource quotas</span></span>
<span data-ttu-id="2ee92-146">A kötegelt megoldásban számítási erőforrások növelése, előtt tooensure hello erőforrások kívánt tooallocate nem haladhatja meg a hello fiók kvóták ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="2ee92-146">Before increasing compute resources in your Batch solution, you can check tooensure hello resources you want tooallocate won't exceed hello account's quotas.</span></span> <span data-ttu-id="2ee92-147">Hello kódrészletben szereplő, jelenleg nyomtatása hello kvótaadatok hello Batch-fiók `mybatchaccount`.</span><span class="sxs-lookup"><span data-stu-id="2ee92-147">In hello code snippet below, we print hello quota information for hello Batch account named `mybatchaccount`.</span></span> <span data-ttu-id="2ee92-148">Saját alkalmazás használhatja az ilyen információk toodetermine, hogy hello fiók kezelni tud a hello további erőforrások toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2ee92-148">In your own application, you could use such information toodetermine whether hello account can handle hello additional resources toobe created.</span></span>

```csharp
// First obtain hello Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print hello compute resource quotas for hello account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> <span data-ttu-id="2ee92-149">Miközben alapértelmezett kvóták Azure-előfizetések és a szolgáltatások, a működés felső korlátjának számos küld a hello emelhető [Azure-portálon][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="2ee92-149">While there are default quotas for Azure subscriptions and services, many of these limits can be raised by issuing a request in hello [Azure portal][azure_portal].</span></span> <span data-ttu-id="2ee92-150">Lásd például: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) kapcsolatos utasításokat a Batch-fiók kvótákat növelését.</span><span class="sxs-lookup"><span data-stu-id="2ee92-150">For example, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for instructions on increasing your Batch account quotas.</span></span>
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a><span data-ttu-id="2ee92-151">Használhatja az Azure Active Directory Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="2ee92-151">Use Azure AD with Batch Management .NET</span></span>

<span data-ttu-id="2ee92-152">hello Batch Management .NET könyvtár az Azure-erőforrás-szolgáltató ügyfél, és együtt használni [Azure Resource Manager] [ resman_overview] toomanage fiók erőforrások programozott módon.</span><span class="sxs-lookup"><span data-stu-id="2ee92-152">hello Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] toomanage account resources programmatically.</span></span> <span data-ttu-id="2ee92-153">Az Azure AD szükség tooauthenticate kérelmet bármely Azure-erőforrás szolgáltató ügyfél hello Batch Management .NET kódtárral dokumentumtár, beleértve és révén [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="2ee92-153">Azure AD is required tooauthenticate requests made through any Azure resource provider client, including hello Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span> <span data-ttu-id="2ee92-154">További információ az Azure AD hello Batch Management .NET kódtárral könyvtárhoz: [Azure Active Directory használata tooauthenticate kötegelt megoldások](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="2ee92-154">For information about using Azure AD with hello Batch Management .NET library, see [Use Azure Active Directory tooauthenticate Batch solutions](batch-aad-auth.md).</span></span> 

## <a name="sample-project-on-github"></a><span data-ttu-id="2ee92-155">Mintaprojektet a Githubon</span><span class="sxs-lookup"><span data-stu-id="2ee92-155">Sample project on GitHub</span></span>

<span data-ttu-id="2ee92-156">toosee a Batch Management .NET kódtárral műveletben, tekintse meg a hello [AccountManagment] [ acct_mgmt_sample] mintaprojektet a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2ee92-156">toosee Batch Management .NET in action, check out hello [AccountManagment][acct_mgmt_sample] sample project on GitHub.</span></span> <span data-ttu-id="2ee92-157">hello AccountManagment mintaalkalmazást a következő műveletek hello mutatja be:</span><span class="sxs-lookup"><span data-stu-id="2ee92-157">hello AccountManagment sample application demonstrates hello following operations:</span></span>

1. <span data-ttu-id="2ee92-158">Szerezzen be egy biztonsági jogkivonatot az Azure AD használatával [ADAL][aad_adal].</span><span class="sxs-lookup"><span data-stu-id="2ee92-158">Acquire a security token from Azure AD by using [ADAL][aad_adal].</span></span> <span data-ttu-id="2ee92-159">Ha hello felhasználó már nem jelentkezett be, meg kell a Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2ee92-159">If hello user is not already signed in, they are prompted for their Azure credentials.</span></span>
2. <span data-ttu-id="2ee92-160">Az Azure AD-ből kapott az hello biztonsági jogkivonatot, hozzon létre egy [SubscriptionClient] [ resman_subclient] tooquery Azure hello fiókhoz tartozó előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="2ee92-160">With hello security token obtained from Azure AD, create a [SubscriptionClient][resman_subclient] tooquery Azure for a list of subscriptions associated with hello account.</span></span> <span data-ttu-id="2ee92-161">hello felhasználói is kiválaszthat hello listájából egynél több előfizetésnek tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2ee92-161">hello user can select a subscription from hello list if it contains more than one subscription.</span></span>
3. <span data-ttu-id="2ee92-162">Kijelölt hello előfizetéshez tartozó hitelesítő adatokat lekérni.</span><span class="sxs-lookup"><span data-stu-id="2ee92-162">Get credentials associated with hello selected subscription.</span></span>
4. <span data-ttu-id="2ee92-163">Hozzon létre egy [ResourceManagementClient] [ resman_client] objektum hello hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="2ee92-163">Create a [ResourceManagementClient][resman_client] object by using hello credentials.</span></span>
5. <span data-ttu-id="2ee92-164">Használja a [ResourceManagementClient] [ resman_client] objektum toocreate egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2ee92-164">Use a [ResourceManagementClient][resman_client] object toocreate a resource group.</span></span>
6. <span data-ttu-id="2ee92-165">Használja a [BatchManagementClient] [ net_mgmt_client] objektum tooperform több fiók kötegműveletek:</span><span class="sxs-lookup"><span data-stu-id="2ee92-165">Use a [BatchManagementClient][net_mgmt_client] object tooperform several Batch account operations:</span></span>
   * <span data-ttu-id="2ee92-166">Batch-fiók létrehozása hello új erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="2ee92-166">Create a Batch account in hello new resource group.</span></span>
   * <span data-ttu-id="2ee92-167">Újonnan létrehozott fiókot a Batch szolgáltatás hello hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2ee92-167">Get hello newly created account from hello Batch service.</span></span>
   * <span data-ttu-id="2ee92-168">Nyomtatási hello kulcsait hello új fiókot.</span><span class="sxs-lookup"><span data-stu-id="2ee92-168">Print hello account keys for hello new account.</span></span>
   * <span data-ttu-id="2ee92-169">Hello fiók egy új elsődleges kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="2ee92-169">Regenerate a new primary key for hello account.</span></span>
   * <span data-ttu-id="2ee92-170">Nyomtatási hello kvótaadatok hello fiók.</span><span class="sxs-lookup"><span data-stu-id="2ee92-170">Print hello quota information for hello account.</span></span>
   * <span data-ttu-id="2ee92-171">Nyomtatási hello kvótaadatok hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2ee92-171">Print hello quota information for hello subscription.</span></span>
   * <span data-ttu-id="2ee92-172">Hello előfizetésen belüli összes fiók nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="2ee92-172">Print all accounts within hello subscription.</span></span>
   * <span data-ttu-id="2ee92-173">Törölje az újonnan létrehozott fiók.</span><span class="sxs-lookup"><span data-stu-id="2ee92-173">Delete newly created account.</span></span>
7. <span data-ttu-id="2ee92-174">Hello csoport törléséhez.</span><span class="sxs-lookup"><span data-stu-id="2ee92-174">Delete hello resource group.</span></span>

<span data-ttu-id="2ee92-175">Az újonnan létrehozott hello kötegelt fiók- és erőforrás-csoport törlése, mielőtt megtekinthetné őket a hello [Azure-portálon][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="2ee92-175">Before deleting hello newly created Batch account and resource group, you can view them in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="2ee92-176">toorun hello mintaalkalmazás sikeresen, akkor először regisztrálnia kell azt az Azure AD-bérlő a hello Azure portál és a grant engedéllyel toohello Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="2ee92-176">toorun hello sample application successfully, you must first register it with your Azure AD tenant in hello Azure portal and grant permissions toohello Azure Resource Manager API.</span></span> <span data-ttu-id="2ee92-177">A megadott hello lépésekkel [hitelesítéséhez kötegelt megoldásokat az Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="2ee92-177">Follow hello steps provided in [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span>


[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
