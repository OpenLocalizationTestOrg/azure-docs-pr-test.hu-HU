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
# <a name="manage-batch-accounts-and-quotas-with-hello-batch-management-client-library-for-net"></a>Batch fiókjainak és kvótáinak hello kötegelt felügyeleti ügyféloldali kódtára a .NET-keretrendszerhez készült kezelése

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Csökkenthető karbantartási terhet az Azure Batch-alkalmazások hello segítségével [Batch Management .NET kódtárral] [ api_mgmt_net] könyvtár tooautomate Batch-fiók létrehozása, törlés, kulcskezelés és kvóta felderítés.

* **Hozzon létre vagy töröljön a Batch-fiókok** bármely régióban. Ha egy független szoftverszállító (ISV), például ad meg, amelyben mindegyik tartozik egy külön Batch-fiók számlázási okokból az ügyfelek a szolgáltatás, a fiók létrehozását és törlését képességek tooyour ügyfélportálra is hozzáadhat.
* **Beolvasni, majd újra létrehozza a kulcsait** programozott módon az összes, a Batch-fiókok. Ez segít felel meg a biztonsági házirendek, amelyeket rendszeres helyettesítő vagy kulcsait lejártát. Ha több Batch-fiókok Azure különböző régiókban, a helyettesítő folyamat automation növeli az a megoldás hatékonyságát.
* **Ellenőrizze a fiók kvóták** hajtanak végre hello próbaverzió hiba munka bizonytalanságát kívül meghatározása, hogy mely Batch-fiókok milyen korlátokkal rendelkeznek. A fiók kvótákat feladatok megkezdése előtt ellenőrzésével készletek létrehozása, vagy a számítási csomópontok hozzáadása, hol proaktív módosíthatja, vagy ha ezek a számítási erőforrások jönnek létre. Azt is meghatározhatja, hogy mely fiókok szükséges kvóta növeli az említett további erőforrások hozzárendelése előtt.
* **Más Azure-szolgáltatások jellemzői egyesítése** teljes körű felügyeleti élmény--a Batch Management .NET kódtárral [Azure Active Directory][aad_about], és hello [Azure Erőforrás-kezelő] [ resman_overview] hello kiírni ugyanahhoz az alkalmazáshoz. Ezeket a szolgáltatásokat és az API-k használatával is frictionless hitelesítési környezetet biztosítson, képes toocreate hello és erőforrás-csoportok és hello képességeket kínál, amelyek egy végpont kezelési megoldás a fent leírt törlése.

> [!NOTE]
> Amíg ez a cikk foglalkozik a hello programozott felügyeleti a kötegelt fiókok, a kulcsok és a kvóták, hajthat végre ezeket a tevékenységeket számos hello segítségével [Azure-portálon][azure_portal]. További információkért lásd: [hello Azure portál használata az Azure Batch-fiók létrehozása](batch-account-create-portal.md) és [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Hozzon létre vagy töröljön a Batch-fiókok
Ahogy azt korábban említettük, egy elsődleges szolgáltatásai hello hello kötegelt felügyeleti API toocreate, és törölje a Batch-fiókok Azure-régióban. Igen, használjon toodo [BatchManagementClient.Account.CreateAsync] [ net_create] és [DeleteAsync][net_delete], vagy mint a szinkron.

hello következő kódrészletet fiókot hoz létre, újonnan létrehozott fiókot a Batch szolgáltatás hello hello beolvassa és törli őket. Az e részlet és mások ebben a cikkben hello `batchManagementClient` teljesen inicializált példánya [BatchManagementClient][net_mgmt_client].

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
> Hello Batch Management .NET kódtárral kódtár és BatchManagementClient osztálya használó alkalmazásoknak **szolgáltatás-rendszergazda** vagy **coadministrator** toohello előfizetés hello birtokló eléréséhez A Batch-fiók toobe felügyelt. További információkért lásd: hello [Azure Active Directory](#azure-active-directory) szakasz és hello [AccountManagement] [ acct_mgmt_sample] kódminta.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Kérje le, majd fiók kulcsok újragenerálása
Elsődleges és másodlagos kulcsok beszerzése az előfizetésen belül a Batch-fiókból való használatával [ListKeysAsync][net_list_keys]. Ezeknek a kulcsoknak használatával állíthatja helyre [RegenerateKeyAsync][net_regenerate_keys].

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
> A felügyeleti alkalmazások zökkenőmentes kapcsolódási munkafolyamatot hozhat létre. Először szerezze be a fiókkulcs hello Batch-fiókhoz való toomanage kívánja [ListKeysAsync][net_list_keys]. Ezt követően használja ezt a kulcsot, hello Batch .NET kódtár inicializálása közben [BatchSharedKeyCredentials] [ net_sharedkeycred] osztály, amely a inicializálása [BatchClient] [net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Ellenőrizze az Azure-előfizetés és a Batch-fiók kvóták
Azure-előfizetések és hello egyes Azure-szolgáltatásokkal, mint a Batch-összes rendelkezik alapértelmezett kvóták korlátozó bizonyos entitások bennük hello száma. Azure-előfizetések hello alapértelmezett kvótái, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md). Hello alapértelmezett hello Batch szolgáltatás kvótái, lásd: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md). Hello Batch Management .NET kódtárral könyvtár használatával ellenőrizheti az alkalmazások ezek mely százalékértékénél kéri. Ez lehetővé teszi toomake foglalási döntések fiókok hozzáadása, vagy a számítási erőforrásokhoz, mint a gyűjtők és számítási csomópontok előtt.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Ellenőrizze a kötegelt fiók kvóták az Azure-előfizetés
Batch-fiók létrehozása a régióban, előtt ellenőrizheti az Azure-előfizetés toosee képes tooadd egy fiókot az adott régióban van.

Hello kódrészletben, először használjuk [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] tooget összes Batch-fiókot, amely egy előfizetésen belül gyűjteménye. Amint azt korábban beszerzett ehhez a gyűjteményhez, azt határozza meg, hány fiókhoz hello cél területen vannak. Akkor használjuk [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] tooobtain hello Batch-fiókkvótája, és határozza meg, hány fiókhoz (ha vannak) az adott régióban is létrehozható.

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

A fenti, hello részlet `creds` példánya [TokenCloudCredentials][azure_tokencreds]. toosee létrehozná ezt az objektumot, például lásd: hello [AccountManagement] [ acct_mgmt_sample] kód mintát a Githubon.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Ellenőrizze a számítási erőforrás kvóták Batch-fiók
A kötegelt megoldásban számítási erőforrások növelése, előtt tooensure hello erőforrások kívánt tooallocate nem haladhatja meg a hello fiók kvóták ellenőrizheti. Hello kódrészletben szereplő, jelenleg nyomtatása hello kvótaadatok hello Batch-fiók `mybatchaccount`. Saját alkalmazás használhatja az ilyen információk toodetermine, hogy hello fiók kezelni tud a hello további erőforrások toobe létrehozott.

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
> Miközben alapértelmezett kvóták Azure-előfizetések és a szolgáltatások, a működés felső korlátjának számos küld a hello emelhető [Azure-portálon][azure_portal]. Lásd például: [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md) kapcsolatos utasításokat a Batch-fiók kvótákat növelését.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Használhatja az Azure Active Directory Batch Management .NET

hello Batch Management .NET könyvtár az Azure-erőforrás-szolgáltató ügyfél, és együtt használni [Azure Resource Manager] [ resman_overview] toomanage fiók erőforrások programozott módon. Az Azure AD szükség tooauthenticate kérelmet bármely Azure-erőforrás szolgáltató ügyfél hello Batch Management .NET kódtárral dokumentumtár, beleértve és révén [Azure Resource Manager][resman_overview]. További információ az Azure AD hello Batch Management .NET kódtárral könyvtárhoz: [Azure Active Directory használata tooauthenticate kötegelt megoldások](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>Mintaprojektet a Githubon

toosee a Batch Management .NET kódtárral műveletben, tekintse meg a hello [AccountManagment] [ acct_mgmt_sample] mintaprojektet a Githubon. hello AccountManagment mintaalkalmazást a következő műveletek hello mutatja be:

1. Szerezzen be egy biztonsági jogkivonatot az Azure AD használatával [ADAL][aad_adal]. Ha hello felhasználó már nem jelentkezett be, meg kell a Azure hitelesítő adatait.
2. Az Azure AD-ből kapott az hello biztonsági jogkivonatot, hozzon létre egy [SubscriptionClient] [ resman_subclient] tooquery Azure hello fiókhoz tartozó előfizetések listáját. hello felhasználói is kiválaszthat hello listájából egynél több előfizetésnek tartalmaz.
3. Kijelölt hello előfizetéshez tartozó hitelesítő adatokat lekérni.
4. Hozzon létre egy [ResourceManagementClient] [ resman_client] objektum hello hitelesítő adatok használatával.
5. Használja a [ResourceManagementClient] [ resman_client] objektum toocreate egy erőforráscsoportot.
6. Használja a [BatchManagementClient] [ net_mgmt_client] objektum tooperform több fiók kötegműveletek:
   * Batch-fiók létrehozása hello új erőforráscsoportban.
   * Újonnan létrehozott fiókot a Batch szolgáltatás hello hello beolvasása.
   * Nyomtatási hello kulcsait hello új fiókot.
   * Hello fiók egy új elsődleges kulcs újragenerálása.
   * Nyomtatási hello kvótaadatok hello fiók.
   * Nyomtatási hello kvótaadatok hello előfizetés.
   * Hello előfizetésen belüli összes fiók nyomtatása.
   * Törölje az újonnan létrehozott fiók.
7. Hello csoport törléséhez.

Az újonnan létrehozott hello kötegelt fiók- és erőforrás-csoport törlése, mielőtt megtekinthetné őket a hello [Azure-portálon][azure_portal]:

toorun hello mintaalkalmazás sikeresen, akkor először regisztrálnia kell azt az Azure AD-bérlő a hello Azure portál és a grant engedéllyel toohello Azure Resource Manager API. A megadott hello lépésekkel [hitelesítéséhez kötegelt megoldásokat az Active Directory](batch-aad-auth-management.md).


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
