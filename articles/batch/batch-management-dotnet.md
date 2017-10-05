---
title: "A .NET - Azure Batch fiók erőforrások az ügyféloldali kódtár kezelése |} Microsoft Docs"
description: "Létrehozása, törlése és módosítása az Azure Batch erőforrásainak a Batch Management .NET kódtárral könyvtárhoz."
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
ms.openlocfilehash: eafde9258222a2ab09ade2e366f9cc595a303dec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a><span data-ttu-id="b044b-103">Batch fiókjainak és kvótáinak a Batch Management ügyféloldali kódtára a .NET-keretrendszerhez készült kezelése</span><span class="sxs-lookup"><span data-stu-id="b044b-103">Manage Batch accounts and quotas with the Batch Management client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b044b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b044b-104">Azure portal</span></span>](batch-account-create-portal.md)
> * [<span data-ttu-id="b044b-105">Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="b044b-105">Batch Management .NET</span></span>](batch-management-dotnet.md)
> 
> 

<span data-ttu-id="b044b-106">Csökkenthető karbantartási terhet az Azure Batch-alkalmazások használatával a [Batch Management .NET kódtárral] [ api_mgmt_net] automatizálhatja a Batch-fiók létrehozása, törlés, kulcskezelés és kvóta felderítési könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b044b-106">You can lower maintenance overhead in your Azure Batch applications by using the [Batch Management .NET][api_mgmt_net] library to automate Batch account creation, deletion, key management, and quota discovery.</span></span>

* <span data-ttu-id="b044b-107">**Hozzon létre vagy töröljön a Batch-fiókok** bármely régióban.</span><span class="sxs-lookup"><span data-stu-id="b044b-107">**Create and delete Batch accounts** within any region.</span></span> <span data-ttu-id="b044b-108">Ha egy független szoftverszállító (ISV), például ad meg, amelyben mindegyik tartozik egy külön Batch-fiók számlázási okokból az ügyfelek a szolgáltatás, a fiók létrehozását és törlését képességek a felhasználói portálra is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b044b-108">If, as an independent software vendor (ISV) for example, you provide a service for your clients in which each is assigned a separate Batch account for billing purposes, you can add account creation and deletion capabilities to your customer portal.</span></span>
* <span data-ttu-id="b044b-109">**Beolvasni, majd újra létrehozza a kulcsait** programozott módon az összes, a Batch-fiókok.</span><span class="sxs-lookup"><span data-stu-id="b044b-109">**Retrieve and regenerate account keys** programmatically for any of your Batch accounts.</span></span> <span data-ttu-id="b044b-110">Ez segít felel meg a biztonsági házirendek, amelyeket rendszeres helyettesítő vagy kulcsait lejártát.</span><span class="sxs-lookup"><span data-stu-id="b044b-110">This can help you comply with security policies that enforce periodic rollover or expiry of account keys.</span></span> <span data-ttu-id="b044b-111">Ha több Batch-fiókok Azure különböző régiókban, a helyettesítő folyamat automation növeli az a megoldás hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="b044b-111">When you have several Batch accounts in various Azure regions, automation of this rollover process increases your solution's efficiency.</span></span>
* <span data-ttu-id="b044b-112">**Ellenőrizze a fiók kvóták** hajtanak végre a próba-és-hiba munka bizonytalanságát kívül meghatározása, hogy mely Batch-fiókok milyen korlátokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="b044b-112">**Check account quotas** and take the trial-and-error guesswork out of determining which Batch accounts have what limits.</span></span> <span data-ttu-id="b044b-113">A fiók kvótákat feladatok megkezdése előtt ellenőrzésével készletek létrehozása, vagy a számítási csomópontok hozzáadása, hol proaktív módosíthatja, vagy ha ezek a számítási erőforrások jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="b044b-113">By checking your account quotas before starting jobs, creating pools, or adding compute nodes, you can proactively adjust where or when these compute resources are created.</span></span> <span data-ttu-id="b044b-114">Azt is meghatározhatja, hogy mely fiókok szükséges kvóta növeli az említett további erőforrások hozzárendelése előtt.</span><span class="sxs-lookup"><span data-stu-id="b044b-114">You can determine which accounts require quota increases before allocating additional resources in those accounts.</span></span>
* <span data-ttu-id="b044b-115">**Más Azure-szolgáltatások jellemzői egyesítése** teljes körű felügyeleti élmény--a Batch Management .NET kódtárral [Azure Active Directory][aad_about], és a [Azure Erőforrás-kezelő] [ resman_overview] együtt ugyanabban az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b044b-115">**Combine features of other Azure services** for a full-featured management experience--by using Batch Management .NET, [Azure Active Directory][aad_about], and the [Azure Resource Manager][resman_overview] together in the same application.</span></span> <span data-ttu-id="b044b-116">Ezeket a szolgáltatásokat és az API-k segítségével megadhatja a frictionless felhasználói hitelesítés, létrehozása és törlése az erőforrás-csoportok és végpont kezelési megoldás a fentiekben leírt képességeit.</span><span class="sxs-lookup"><span data-stu-id="b044b-116">By using these features and their APIs, you can provide a frictionless authentication experience, the ability to create and delete resource groups, and the capabilities that are described above for an end-to-end management solution.</span></span>

> [!NOTE]
> <span data-ttu-id="b044b-117">Amíg ez a cikk foglalkozik a programozott felügyeleti a kötegelt fiókok, a kulcsok és a kvóták, hajthat végre ezeket a tevékenységeket számos használatával a [Azure-portálon][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="b044b-117">While this article focuses on the programmatic management of your Batch accounts, keys, and quotas, you can perform many of these activities by using the [Azure portal][azure_portal].</span></span> <span data-ttu-id="b044b-118">További információkért lásd: [az Azure portál használata az Azure Batch-fiók létrehozása](batch-account-create-portal.md) és [kvótái és korlátai az Azure Batch szolgáltatás](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="b044b-118">For more information, see [Create an Azure Batch account using the Azure portal](batch-account-create-portal.md) and [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span>
> 
> 

## <a name="create-and-delete-batch-accounts"></a><span data-ttu-id="b044b-119">Hozzon létre vagy töröljön a Batch-fiókok</span><span class="sxs-lookup"><span data-stu-id="b044b-119">Create and delete Batch accounts</span></span>
<span data-ttu-id="b044b-120">Ahogy azt korábban említettük, a kötegelt API elsődleges szolgáltatásai egyik hozzon létre vagy töröljön a Batch-fiókok Azure-régióban.</span><span class="sxs-lookup"><span data-stu-id="b044b-120">As mentioned, one of the primary features of the Batch Management API is to create and delete Batch accounts in an Azure region.</span></span> <span data-ttu-id="b044b-121">Ehhez használja [BatchManagementClient.Account.CreateAsync] [ net_create] és [DeleteAsync][net_delete], vagy mint a szinkron.</span><span class="sxs-lookup"><span data-stu-id="b044b-121">To do so, use [BatchManagementClient.Account.CreateAsync][net_create] and [DeleteAsync][net_delete], or their synchronous counterparts.</span></span>

<span data-ttu-id="b044b-122">A következő kódrészletet fiókot hoz létre, az újonnan létrehozott fiókhoz kapja a Batch szolgáltatás, és törli őket.</span><span class="sxs-lookup"><span data-stu-id="b044b-122">The following code snippet creates an account, obtains the newly created account from the Batch service, and then deletes it.</span></span> <span data-ttu-id="b044b-123">Ezt a kódrészletet, és a többi ebben a cikkben `batchManagementClient` teljesen inicializált példánya [BatchManagementClient][net_mgmt_client].</span><span class="sxs-lookup"><span data-stu-id="b044b-123">In this snippet and the others in this article, `batchManagementClient` is a fully initialized instance of [BatchManagementClient][net_mgmt_client].</span></span>

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> <span data-ttu-id="b044b-124">A Batch Management .NET kódtárral és BatchManagementClient osztálya használó alkalmazásoknak **szolgáltatás-rendszergazda** vagy **coadministrator** hozzáférése az előfizetéshez, amely a kötegelt tulajdonosa fiókot felügyelhető.</span><span class="sxs-lookup"><span data-stu-id="b044b-124">Applications that use the Batch Management .NET library and its BatchManagementClient class require **service administrator** or **coadministrator** access to the subscription that owns the Batch account to be managed.</span></span> <span data-ttu-id="b044b-125">További információkért lásd: a [Azure Active Directory](#azure-active-directory) szakasz és a [AccountManagement] [ acct_mgmt_sample] kódminta.</span><span class="sxs-lookup"><span data-stu-id="b044b-125">For more information, see the [Azure Active Directory](#azure-active-directory) section and the [AccountManagement][acct_mgmt_sample] code sample.</span></span>
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a><span data-ttu-id="b044b-126">Kérje le, majd fiók kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="b044b-126">Retrieve and regenerate account keys</span></span>
<span data-ttu-id="b044b-127">Elsődleges és másodlagos kulcsok beszerzése az előfizetésen belül a Batch-fiókból való használatával [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="b044b-127">Obtain primary and secondary account keys from any Batch account within your subscription by using [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="b044b-128">Ezeknek a kulcsoknak használatával állíthatja helyre [RegenerateKeyAsync][net_regenerate_keys].</span><span class="sxs-lookup"><span data-stu-id="b044b-128">You can regenerate those keys by using [RegenerateKeyAsync][net_regenerate_keys].</span></span>

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> <span data-ttu-id="b044b-129">A felügyeleti alkalmazások zökkenőmentes kapcsolódási munkafolyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b044b-129">You can create a streamlined connection workflow for your management applications.</span></span> <span data-ttu-id="b044b-130">Először szerezze be a Batch-fiókhoz a felügyelni kívánt fiókkulcs [ListKeysAsync][net_list_keys].</span><span class="sxs-lookup"><span data-stu-id="b044b-130">First, obtain an account key for the Batch account you wish to manage with [ListKeysAsync][net_list_keys].</span></span> <span data-ttu-id="b044b-131">Ezt követően használja ezt a kulcsot, a Batch .NET kódtár inicializálása közben [BatchSharedKeyCredentials] [ net_sharedkeycred] osztály, amely a inicializálása [BatchClient] [ net_batch_client].</span><span class="sxs-lookup"><span data-stu-id="b044b-131">Then, use this key when initializing the Batch .NET library's [BatchSharedKeyCredentials][net_sharedkeycred] class, which is used when initializing [BatchClient][net_batch_client].</span></span>
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a><span data-ttu-id="b044b-132">Ellenőrizze az Azure-előfizetés és a Batch-fiók kvóták</span><span class="sxs-lookup"><span data-stu-id="b044b-132">Check Azure subscription and Batch account quotas</span></span>
<span data-ttu-id="b044b-133">Azure-előfizetések és az egyes Azure-szolgáltatásokkal, mint a Batch-összes rendelkezik alapértelmezett kvótákat, amelyek a bennük lévő egyes entitások számának korlátozásához.</span><span class="sxs-lookup"><span data-stu-id="b044b-133">Azure subscriptions and the individual Azure services like Batch all have default quotas that limit the number of certain entities within them.</span></span> <span data-ttu-id="b044b-134">További Azure-előfizetések alapértelmezett kvótái: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="b044b-134">For the default quotas for Azure subscriptions, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="b044b-135">A Batch szolgáltatás alapértelmezett kvóták, lásd: [kvótái és korlátai az Azure Batch szolgáltatás](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="b044b-135">For the default quotas of the Batch service, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="b044b-136">A Batch Management .NET kódtárral könyvtár használatával ellenőrizheti az alkalmazások ezek mely százalékértékénél kéri.</span><span class="sxs-lookup"><span data-stu-id="b044b-136">By using the Batch Management .NET library, you can check these quotas in your applications.</span></span> <span data-ttu-id="b044b-137">Ez lehetővé teszi, hogy foglalási döntéseket fiókok hozzáadása, vagy a számítási erőforrásokhoz, mint a gyűjtők és számítási csomópontok előtt.</span><span class="sxs-lookup"><span data-stu-id="b044b-137">This enables you to make allocation decisions before you add accounts or compute resources like pools and compute nodes.</span></span>

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a><span data-ttu-id="b044b-138">Ellenőrizze a kötegelt fiók kvóták az Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="b044b-138">Check an Azure subscription for Batch account quotas</span></span>
<span data-ttu-id="b044b-139">Batch-fiók létrehozása a régióban, előtt ellenőrizheti, hogy meg vannak-e fiók hozzáadása az adott régióban tudni az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b044b-139">Before creating a Batch account in a region, you can check your Azure subscription to see whether you are able to add an account in that region.</span></span>

<span data-ttu-id="b044b-140">Az alábbi kódrészletet, először használjuk [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] gyűjteménye, amelyek egy előfizetésen belüli összes Batch-fiókok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b044b-140">In the code snippet below, we first use [BatchManagementClient.Account.ListAsync][net_mgmt_listaccounts] to get a collection of all Batch accounts that are within a subscription.</span></span> <span data-ttu-id="b044b-141">Amint azt korábban beszerzett ehhez a gyűjteményhez, azt határozza meg, hány fiókhoz cél területen vannak.</span><span class="sxs-lookup"><span data-stu-id="b044b-141">Once we've obtained this collection, we determine how many accounts are in the target region.</span></span> <span data-ttu-id="b044b-142">Akkor használjuk [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] beszerzése a Batch-fiókkvótája, és határozza meg, hány fiókhoz (ha vannak) az adott régióban is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="b044b-142">Then we use [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] to obtain the Batch account quota and determine how many accounts (if any) can be created in that region.</span></span>

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

<span data-ttu-id="b044b-143">A fenti, a részlet `creds` példánya [TokenCloudCredentials][azure_tokencreds].</span><span class="sxs-lookup"><span data-stu-id="b044b-143">In the snippet above, `creds` is an instance of [TokenCloudCredentials][azure_tokencreds].</span></span> <span data-ttu-id="b044b-144">Ez az objektum létrehozására láthat példát, olvassa el a [AccountManagement] [ acct_mgmt_sample] kód mintát a Githubon.</span><span class="sxs-lookup"><span data-stu-id="b044b-144">To see an example of creating this object, see the [AccountManagement][acct_mgmt_sample] code sample on GitHub.</span></span>

### <a name="check-a-batch-account-for-compute-resource-quotas"></a><span data-ttu-id="b044b-145">Ellenőrizze a számítási erőforrás kvóták Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="b044b-145">Check a Batch account for compute resource quotas</span></span>
<span data-ttu-id="b044b-146">A kötegelt megoldásban számítási erőforrások növelése, előtt ellenőrizheti a lefoglalni kívánt erőforrásokat biztosításához nem haladhatja meg a fiók a kvóták.</span><span class="sxs-lookup"><span data-stu-id="b044b-146">Before increasing compute resources in your Batch solution, you can check to ensure the resources you want to allocate won't exceed the account's quotas.</span></span> <span data-ttu-id="b044b-147">Az alábbi kódrészletben, azt nyomtassa ki a Batch-fiók a kvótaadatok `mybatchaccount`.</span><span class="sxs-lookup"><span data-stu-id="b044b-147">In the code snippet below, we print the quota information for the Batch account named `mybatchaccount`.</span></span> <span data-ttu-id="b044b-148">A saját alkalmazásban használhatja ezeket az információkat annak meghatározásához, hogy a fiók kezelni tud létrehozni a további erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b044b-148">In your own application, you could use such information to determine whether the account can handle the additional resources to be created.</span></span>

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> <span data-ttu-id="b044b-149">Miközben alapértelmezett kvóták Azure-előfizetések és a szolgáltatások, a működés felső korlátjának számos küld a emelhető a [Azure-portálon][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="b044b-149">While there are default quotas for Azure subscriptions and services, many of these limits can be raised by issuing a request in the [Azure portal][azure_portal].</span></span> <span data-ttu-id="b044b-150">Lásd például: [kvótái és korlátai az Azure Batch szolgáltatás](batch-quota-limit.md) kapcsolatos utasításokat a Batch-fiók kvótákat növelését.</span><span class="sxs-lookup"><span data-stu-id="b044b-150">For example, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for instructions on increasing your Batch account quotas.</span></span>
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a><span data-ttu-id="b044b-151">Használhatja az Azure Active Directory Batch Management .NET</span><span class="sxs-lookup"><span data-stu-id="b044b-151">Use Azure AD with Batch Management .NET</span></span>

<span data-ttu-id="b044b-152">A Batch Management .NET könyvtár az Azure-erőforrás-szolgáltató ügyfél, és együtt használni [Azure Resource Manager] [ resman_overview] programozott módon fiók-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b044b-152">The Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] to manage account resources programmatically.</span></span> <span data-ttu-id="b044b-153">Az Azure AD bármely Azure-erőforrás szolgáltató ügyfél, beleértve a Batch Management .NET kódtár és révén kérelmek hitelesítéséhez szükséges [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="b044b-153">Azure AD is required to authenticate requests made through any Azure resource provider client, including the Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span> <span data-ttu-id="b044b-154">A Batch Management .NET könyvtár az Azure AD használatával kapcsolatban további információkért lásd: [használata Azure Active Directory kötegelt megoldások hitelesítéséhez](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="b044b-154">For information about using Azure AD with the Batch Management .NET library, see [Use Azure Active Directory to authenticate Batch solutions](batch-aad-auth.md).</span></span> 

## <a name="sample-project-on-github"></a><span data-ttu-id="b044b-155">Mintaprojektet a Githubon</span><span class="sxs-lookup"><span data-stu-id="b044b-155">Sample project on GitHub</span></span>

<span data-ttu-id="b044b-156">A művelet a Batch Management .NET kódtárral megtekintéséhez tekintse meg a [AccountManagment] [ acct_mgmt_sample] mintaprojektet a Githubon.</span><span class="sxs-lookup"><span data-stu-id="b044b-156">To see Batch Management .NET in action, check out the [AccountManagment][acct_mgmt_sample] sample project on GitHub.</span></span> <span data-ttu-id="b044b-157">A AccountManagment mintaalkalmazás mutatja be a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="b044b-157">The AccountManagment sample application demonstrates the following operations:</span></span>

1. <span data-ttu-id="b044b-158">Szerezzen be egy biztonsági jogkivonatot az Azure AD használatával [ADAL][aad_adal].</span><span class="sxs-lookup"><span data-stu-id="b044b-158">Acquire a security token from Azure AD by using [ADAL][aad_adal].</span></span> <span data-ttu-id="b044b-159">Ha a felhasználó már nem jelentkezett be, meg kell a Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b044b-159">If the user is not already signed in, they are prompted for their Azure credentials.</span></span>
2. <span data-ttu-id="b044b-160">Az az Azure AD-ből kapott biztonsági jogkivonatot, hozzon létre egy [SubscriptionClient] [ resman_subclient] Azure lekérdezni a fiókhoz tartozó előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="b044b-160">With the security token obtained from Azure AD, create a [SubscriptionClient][resman_subclient] to query Azure for a list of subscriptions associated with the account.</span></span> <span data-ttu-id="b044b-161">A felhasználó is kiválaszthat a listából, ha egynél több előfizetésnek tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b044b-161">The user can select a subscription from the list if it contains more than one subscription.</span></span>
3. <span data-ttu-id="b044b-162">A kijelölt előfizetéshez tartozó hitelesítő adatokat lekérni.</span><span class="sxs-lookup"><span data-stu-id="b044b-162">Get credentials associated with the selected subscription.</span></span>
4. <span data-ttu-id="b044b-163">Hozzon létre egy [ResourceManagementClient] [ resman_client] objektum a hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="b044b-163">Create a [ResourceManagementClient][resman_client] object by using the credentials.</span></span>
5. <span data-ttu-id="b044b-164">Használja a [ResourceManagementClient] [ resman_client] objektumot hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b044b-164">Use a [ResourceManagementClient][resman_client] object to create a resource group.</span></span>
6. <span data-ttu-id="b044b-165">Használja a [BatchManagementClient] [ net_mgmt_client] objektum több Batch-fiók műveletek végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="b044b-165">Use a [BatchManagementClient][net_mgmt_client] object to perform several Batch account operations:</span></span>
   * <span data-ttu-id="b044b-166">Batch-fiók létrehozása az új erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b044b-166">Create a Batch account in the new resource group.</span></span>
   * <span data-ttu-id="b044b-167">Az újonnan létrehozott fiókhoz beszerzése a Batch szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b044b-167">Get the newly created account from the Batch service.</span></span>
   * <span data-ttu-id="b044b-168">Nyomtassa ki az új fiók a kulcsait.</span><span class="sxs-lookup"><span data-stu-id="b044b-168">Print the account keys for the new account.</span></span>
   * <span data-ttu-id="b044b-169">A fiók egy új elsődleges kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="b044b-169">Regenerate a new primary key for the account.</span></span>
   * <span data-ttu-id="b044b-170">A fiók kvótájának nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="b044b-170">Print the quota information for the account.</span></span>
   * <span data-ttu-id="b044b-171">Az előfizetéshez tartozó kvóta információt kinyomtatni.</span><span class="sxs-lookup"><span data-stu-id="b044b-171">Print the quota information for the subscription.</span></span>
   * <span data-ttu-id="b044b-172">Nyomtassa ki az előfizetésen belüli összes fiók.</span><span class="sxs-lookup"><span data-stu-id="b044b-172">Print all accounts within the subscription.</span></span>
   * <span data-ttu-id="b044b-173">Törölje az újonnan létrehozott fiók.</span><span class="sxs-lookup"><span data-stu-id="b044b-173">Delete newly created account.</span></span>
7. <span data-ttu-id="b044b-174">Törölje a csoportot.</span><span class="sxs-lookup"><span data-stu-id="b044b-174">Delete the resource group.</span></span>

<span data-ttu-id="b044b-175">Az újonnan létrehozott kötegelt fiók- és erőforrás csoportot töröl, mielőtt megtekinthetné őket a a [Azure-portálon][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="b044b-175">Before deleting the newly created Batch account and resource group, you can view them in the [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="b044b-176">A mintaalkalmazás sikeresen futtatni, először regisztrálja az Azure AD-bérlő az Azure portálon, és a hozzáférési jogot az Azure Resource Manager API-t.</span><span class="sxs-lookup"><span data-stu-id="b044b-176">To run the sample application successfully, you must first register it with your Azure AD tenant in the Azure portal and grant permissions to the Azure Resource Manager API.</span></span> <span data-ttu-id="b044b-177">Kövesse az itt ismertetett lépéseket: [hitelesítéséhez kötegelt megoldásokat az Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="b044b-177">Follow the steps provided in [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span>


<span data-ttu-id="b044b-178">[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="b044b-178">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="b044b-179">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"</span><span class="sxs-lookup"><span data-stu-id="b044b-179">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="b044b-180">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"</span><span class="sxs-lookup"><span data-stu-id="b044b-180">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
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
