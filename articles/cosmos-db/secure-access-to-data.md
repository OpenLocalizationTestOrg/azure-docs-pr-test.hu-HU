---
title: "Megtudhatja, hogyan biztosíthat biztonságos hozzáférést a adatokat az Adatbázisba az Azure Cosmos |} Microsoft Docs"
description: "További tudnivalók access control jellemzői Azure Cosmos DB, beleértve a főkulcs, csak olvasható kulcsok, felhasználók és engedélyek."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: 383e04f91eec2f465b381ce30f2d6d24c488b731
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="securing-access-to-azure-cosmos-db-data"></a><span data-ttu-id="4624c-103">Azure Cosmos DB adatokhoz való hozzáférés biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="4624c-103">Securing access to Azure Cosmos DB data</span></span>
<span data-ttu-id="4624c-104">Ez a cikk áttekintést a adataihoz való hozzáférés biztosítása [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="4624c-104">This article provides an overview of securing access to data stored in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

<span data-ttu-id="4624c-105">Azure Cosmos-adatbázis kétféle kulcsok segítségével hitelesíti a felhasználókat, és az adatok és erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4624c-105">Azure Cosmos DB uses two types of keys to authenticate users and provide access to its data and resources.</span></span> 

|<span data-ttu-id="4624c-106">kulcs típusa</span><span class="sxs-lookup"><span data-stu-id="4624c-106">Key type</span></span>|<span data-ttu-id="4624c-107">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="4624c-107">Resources</span></span>|
|---|---|
|[<span data-ttu-id="4624c-108">Főkulcsok</span><span class="sxs-lookup"><span data-stu-id="4624c-108">Master keys</span></span>](#master-keys) |<span data-ttu-id="4624c-109">Felügyeleti erőforrások használt: adatbázis-fiókok, adatbázisok, felhasználók és engedélyek</span><span class="sxs-lookup"><span data-stu-id="4624c-109">Used for administrative resources: database accounts, databases, users, and permissions</span></span>|
|[<span data-ttu-id="4624c-110">Erőforrás-tokenek</span><span class="sxs-lookup"><span data-stu-id="4624c-110">Resource tokens</span></span>](#resource-tokens)|<span data-ttu-id="4624c-111">Alkalmazás-erőforrásokat használt: gyűjtemények, dokumentumok, a mellékletek, tárolt eljárások, eseményindítók és felhasználó által megadott függvények</span><span class="sxs-lookup"><span data-stu-id="4624c-111">Used for application resources: collections, documents, attachments, stored procedures, triggers, and UDFs</span></span>|

<a id="master-keys"></a>

## <a name="master-keys"></a><span data-ttu-id="4624c-112">Főkulcsok</span><span class="sxs-lookup"><span data-stu-id="4624c-112">Master keys</span></span> 

<span data-ttu-id="4624c-113">Főkulcsait az összes felügyeleti erőforrást az adatbázis fiókjához tartozó hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="4624c-113">Master keys provide access to the all the administrative resources for the database account.</span></span> <span data-ttu-id="4624c-114">Főkulcsok:</span><span class="sxs-lookup"><span data-stu-id="4624c-114">Master keys:</span></span>  
- <span data-ttu-id="4624c-115">Fiókok, adatbázisok, felhasználók és engedélyek hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="4624c-115">Provide access to accounts, databases, users, and permissions.</span></span> 
- <span data-ttu-id="4624c-116">Nem használható a részletes hozzáférést biztosít a gyűjtemények és dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="4624c-116">Cannot be used to provide granular access to collections and documents.</span></span>
- <span data-ttu-id="4624c-117">A fiók létrehozása során jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="4624c-117">Are created during the creation of an account.</span></span>
- <span data-ttu-id="4624c-118">Bármikor helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="4624c-118">Can be regenerated at any time.</span></span>

<span data-ttu-id="4624c-119">Minden felhasználói fiókhoz két főkulcsok áll: egy elsődleges és másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4624c-119">Each account consists of two Master keys: a primary key and secondary key.</span></span> <span data-ttu-id="4624c-120">Kettős kulcsok célja, hogy generálni, vagy állítsa a kulcsokat, a fiók és az adatok folyamatos hozzáférést biztosító.</span><span class="sxs-lookup"><span data-stu-id="4624c-120">The purpose of dual keys is so that you can regenerate, or roll keys, providing continuous access to your account and data.</span></span> 

<span data-ttu-id="4624c-121">A Cosmos DB fiók két fő kulcsot kívül két írásvédett kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4624c-121">In addition to the two master keys for the Cosmos DB account, there are two read-only keys.</span></span> <span data-ttu-id="4624c-122">A csak olvasható kulcsokat engedélyezése csak a fiók az olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="4624c-122">These read-only keys only allow read operations on the account.</span></span> <span data-ttu-id="4624c-123">Csak olvasható kulcsok nem biztosítanak engedélyeket erőforrások olvasási hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="4624c-123">Read-only keys do not provide access to read permissions resources.</span></span>

<span data-ttu-id="4624c-124">Elsődleges, másodlagos csak olvasható és írható-olvasható főkulcsok lekérhető, és újra létrehozza a rendszer az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="4624c-124">Primary, secondary, read only, and read-write master keys can be retrieved and regenerated using the Azure portal.</span></span> <span data-ttu-id="4624c-125">Útmutatásért lásd: [megtekintése, másolása és újragenerálása hívóbetűk](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="4624c-125">For instructions, see [View, copy, and regenerate access keys](manage-account.md#keys).</span></span>

![Az Azure portál – bemutatásához NoSQL-adatbázis biztonsági hozzáférés-vezérlés (IAM)](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

<span data-ttu-id="4624c-127">A főkulcs elforgatása folyamata felettébb egyszerű.</span><span class="sxs-lookup"><span data-stu-id="4624c-127">The process of rotating your master key is simple.</span></span> <span data-ttu-id="4624c-128">Keresse meg a másodlagos kulcs lekérését, majd cserélje le az elsődleges kulcsra a másodlagos kulcs az alkalmazás az Azure portálon, majd forgassa el az elsődleges kulcsot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4624c-128">Navigate to the Azure portal to retrieve your secondary key, then replace your primary key with your secondary key in your application, then rotate the primary key in the Azure portal.</span></span>

![Az Azure portál – NoSQL-adatbázis biztonsági, amely tartalmazza a főkulcs Elforgatás](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a><span data-ttu-id="4624c-130">A főkulcs használandó kódminta</span><span class="sxs-lookup"><span data-stu-id="4624c-130">Code sample to use a master key</span></span>

<span data-ttu-id="4624c-131">A következő példakód azt ábrázolja, hogyan Cosmos DB fiók végpontját és főkulcs használatával hozható létre egy DocumentClient, és hozzon létre egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="4624c-131">The following code sample illustrates how to use a Cosmos DB account endpoint and master key to instantiate a DocumentClient and create a database.</span></span> 

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access to your DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a><span data-ttu-id="4624c-132">Erőforrás-tokenek</span><span class="sxs-lookup"><span data-stu-id="4624c-132">Resource tokens</span></span>

<span data-ttu-id="4624c-133">Erőforrás-jogkivonatokat biztosítanak hozzáférést az adatbázisban lévő alkalmazás-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4624c-133">Resource tokens provide access to the application resources within a database.</span></span> <span data-ttu-id="4624c-134">Erőforrás-jogkivonatokat:</span><span class="sxs-lookup"><span data-stu-id="4624c-134">Resource tokens:</span></span>
- <span data-ttu-id="4624c-135">Adott gyűjteményekre, partíciós kulcsok, dokumentumok, a mellékletek, tárolt eljárások, eseményindítók és felhasználó által megadott függvények hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="4624c-135">Provide access to specific collections, partition keys, documents, attachments, stored procedures, triggers, and UDFs.</span></span>
- <span data-ttu-id="4624c-136">Jönnek létre, amikor egy [felhasználói](#users) kap [engedélyek](#permissions) adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="4624c-136">Are created when a [user](#users) is granted [permissions](#permissions) to a specific resource.</span></span>
- <span data-ttu-id="4624c-137">Jönnek létre újból, akkor egy engedély erőforrás van a FELADÁS egy vagy több, a GET vagy a PUT hívás.</span><span class="sxs-lookup"><span data-stu-id="4624c-137">Are recreated when a permission resource is acted upon on by POST, GET, or PUT call.</span></span>
- <span data-ttu-id="4624c-138">A felhasználói, erőforrás és engedélye kifejezetten összeállított kivonatoló erőforrás jogkivonatot használja.</span><span class="sxs-lookup"><span data-stu-id="4624c-138">Use a hash resource token specifically constructed for the user, resource, and permission.</span></span>
- <span data-ttu-id="4624c-139">Testre szabható érvényességi időtartam kötött megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="4624c-139">Are time bound with a customizable validity period.</span></span> <span data-ttu-id="4624c-140">Az alapértelmezett érvényes timespan egy óra.</span><span class="sxs-lookup"><span data-stu-id="4624c-140">The default valid timespan is one hour.</span></span> <span data-ttu-id="4624c-141">A jogkivonatok élettartama, azonban előfordulhat, hogy explicit módon megadni, legfeljebb öt órát.</span><span class="sxs-lookup"><span data-stu-id="4624c-141">Token lifetime, however, may be explicitly specified, up to a maximum of five hours.</span></span>
- <span data-ttu-id="4624c-142">Adjon meg egy biztonságos helyett a főkulcs kiadása.</span><span class="sxs-lookup"><span data-stu-id="4624c-142">Provide a safe alternative to giving out the master key.</span></span> 
- <span data-ttu-id="4624c-143">Engedélyezi az ügyfelek számára olvasási, írási és törli az erőforrást a Cosmos DB fiók megfelelően korábban megadott engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="4624c-143">Enable clients to read, write, and delete resources in the Cosmos DB account according to the permissions they've been granted.</span></span>

<span data-ttu-id="4624c-144">Egy erőforrás-jogkivonat (létrehozásával Cosmos DB felhasználókat és engedélyeket) is használhatja az Cosmos DB-fiókban lévő erőforrásokhoz való hozzáférés biztosításához ügyfél, amely nem adható meg megbízhatóként a főkulcs a kívánt.</span><span class="sxs-lookup"><span data-stu-id="4624c-144">You can use a resource token (by creating Cosmos DB users and permissions) when you want to provide access to resources in your Cosmos DB account to a client that cannot be trusted with the master key.</span></span>  

<span data-ttu-id="4624c-145">Cosmos DB erőforrás jogkivonatokat adjon meg egy biztonságos megoldás, amely lehetővé teszi az ügyfelek számára olvasási, írási és törlési a jogosultságaitól megfelelően, és egy fő vagy a kulcs csak olvasható nélkül a Cosmos DB fiók erőforrásai.</span><span class="sxs-lookup"><span data-stu-id="4624c-145">Cosmos DB resource tokens provide a safe alternative that enables clients to read, write, and delete resources in your Cosmos DB account according to the permissions you've granted, and without need for either a master or read only key.</span></span>

<span data-ttu-id="4624c-146">Íme egy tipikus kialakításban, amellyel erőforrás jogkivonatok kérhetők, jön létre, és ügyfelek felé:</span><span class="sxs-lookup"><span data-stu-id="4624c-146">Here is a typical design pattern whereby resource tokens may be requested, generated, and delivered to clients:</span></span>

1. <span data-ttu-id="4624c-147">Egy közepes réteg szolgáltatás megosztásához a felhasználó fényképeihez mobilalkalmazás kiszolgálására van beállítva.</span><span class="sxs-lookup"><span data-stu-id="4624c-147">A mid-tier service is set up to serve a mobile application to share user photos.</span></span> 
2. <span data-ttu-id="4624c-148">A közepes réteg szolgáltatás a fő oszlopkulcs Cosmos DB fiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4624c-148">The mid-tier service possesses the master key of the Cosmos DB account.</span></span>
3. <span data-ttu-id="4624c-149">Az fényképező alkalmazás telepítve van a végfelhasználói mobileszközökön.</span><span class="sxs-lookup"><span data-stu-id="4624c-149">The photo app is installed on end-user mobile devices.</span></span> 
4. <span data-ttu-id="4624c-150">A bejelentkezés a fényképező alkalmazás létrehozza a felhasználó a közepes réteg szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="4624c-150">On login, the photo app establishes the identity of the user with the mid-tier service.</span></span> <span data-ttu-id="4624c-151">A mechanizmus identitás létesítmény kizárólag az alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="4624c-151">This mechanism of identity establishment is purely up to the application.</span></span>
5. <span data-ttu-id="4624c-152">Miután az identitása létrejött, a közepes réteg a szolgáltatáskérések engedélyek személyazonossága alapján.</span><span class="sxs-lookup"><span data-stu-id="4624c-152">Once the identity is established, the mid-tier service requests permissions based on the identity.</span></span>
6. <span data-ttu-id="4624c-153">A közepes réteg szolgáltatás elküldi egy erőforrás-jogkivonat a telefonalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4624c-153">The mid-tier service sends a resource token back to the phone app.</span></span>
7. <span data-ttu-id="4624c-154">A telefonalkalmazás továbbra is használhatja az erőforrás-jogkivonat erőforrásokhoz való közvetlen hozzáféréshez Cosmos DB definiálva az erőforrás-jogkivonat és a alatt, az erőforrás-jogkivonat által engedélyezett engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="4624c-154">The phone app can continue to use the resource token to directly access Cosmos DB resources with the permissions defined by the resource token and for the interval allowed by the resource token.</span></span> 
8. <span data-ttu-id="4624c-155">Amikor az erőforrás-jogkivonat lejár, a későbbi kérelmek kapnak a 401-es jogosulatlan kivételt.</span><span class="sxs-lookup"><span data-stu-id="4624c-155">When the resource token expires, subsequent requests receive a 401 unauthorized exception.</span></span>  <span data-ttu-id="4624c-156">Ezen a ponton a telefonalkalmazás újból létrehozza az identitás, és egy új erőforrás-jogkivonat kéri.</span><span class="sxs-lookup"><span data-stu-id="4624c-156">At this point, the phone app re-establishes the identity and requests a new resource token.</span></span>

    ![Az Azure Cosmos DB erőforrás jogkivonatok munkafolyamat](./media/secure-access-to-data/resourcekeyworkflow.png)

<span data-ttu-id="4624c-158">Erőforrás-token létrehozása és kezelése végzi el a natív Cosmos DB ügyfél könyvtárakat. REST használatakor a kérelem/hitelesítési fejlécek kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4624c-158">Resource token generation and management is handled by the native Cosmos DB client libraries; however, if you use REST you must construct the request/authentication headers.</span></span> <span data-ttu-id="4624c-159">A többi hitelesítési fejlécek létrehozásáról további információk: [Cosmos DB erőforrások hozzáférés-vezérlés](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) vagy a [forráskód az SDK-k](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span><span class="sxs-lookup"><span data-stu-id="4624c-159">For more information on creating authentication headers for REST, see [Access Control on Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) or the [source code for our SDKs](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span></span>

<span data-ttu-id="4624c-160">A középső réteg szolgáltatás létrehozásához vagy replikaszervező erőforrás jogkivonatok használt példáért lásd: a [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span><span class="sxs-lookup"><span data-stu-id="4624c-160">For an example of a middle tier service used to generate or broker resource tokens, see the [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span></span>

<a id="users"></a>

## <a name="users"></a><span data-ttu-id="4624c-161">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="4624c-161">Users</span></span>
<span data-ttu-id="4624c-162">Cosmos DB felhasználók hozzárendelve egy Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="4624c-162">Cosmos DB users are associated with a Cosmos DB database.</span></span>  <span data-ttu-id="4624c-163">Az egyes adatbázisok nulla vagy több Cosmos DB felhasználókat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="4624c-163">Each database can contain zero or more Cosmos DB users.</span></span>  <span data-ttu-id="4624c-164">A következő példakód bemutatja, hogyan hozzon létre egy Cosmos DB felhasználói erőforrást.</span><span class="sxs-lookup"><span data-stu-id="4624c-164">The following code sample shows how to create a Cosmos DB user resource.</span></span>

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> <span data-ttu-id="4624c-165">Minden egyes Cosmos DB felhasználói segítségével listájának beolvasása PermissionsLink tulajdonsággal rendelkezik [engedélyek](#permissions) a felhasználóhoz társított.</span><span class="sxs-lookup"><span data-stu-id="4624c-165">Each Cosmos DB user has a PermissionsLink property that can be used to retrieve the list of [permissions](#permissions) associated with the user.</span></span>
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a><span data-ttu-id="4624c-166">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="4624c-166">Permissions</span></span>
<span data-ttu-id="4624c-167">A Cosmos DB engedély erőforrás társítva egy Cosmos DB felhasználói.</span><span class="sxs-lookup"><span data-stu-id="4624c-167">A Cosmos DB permission resource is associated with a Cosmos DB user.</span></span>  <span data-ttu-id="4624c-168">Minden felhasználó nulla vagy több Cosmos DB engedélyeket is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="4624c-168">Each user may contain zero or more Cosmos DB permissions.</span></span>  <span data-ttu-id="4624c-169">Engedély erőforrás egy biztonsági jogkivonatot, amely a felhasználó számára szükséges egy adott alkalmazás erőforrás elérésére tett kísérlet során hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="4624c-169">A permission resource provides access to a security token that the user needs when trying to access a specific application resource.</span></span>
<span data-ttu-id="4624c-170">Van engedélye az erőforrás által is megadható két rendelkezésre álló hozzáférési szintek:</span><span class="sxs-lookup"><span data-stu-id="4624c-170">There are two available access levels that may be provided by a permission resource:</span></span>

* <span data-ttu-id="4624c-171">Összes: A felhasználó teljes körű hozzáféréssel rendelkezik az erőforráson.</span><span class="sxs-lookup"><span data-stu-id="4624c-171">All: The user has full permission on the resource.</span></span>
* <span data-ttu-id="4624c-172">Olvassa el: A felhasználó csak olvashatók a tartalmát az erőforrás, de nem hajtható végre, írási, update vagy delete művelethez az erőforráson.</span><span class="sxs-lookup"><span data-stu-id="4624c-172">Read: The user can only read the contents of the resource but cannot perform write, update, or delete operations on the resource.</span></span>

> [!NOTE]
> <span data-ttu-id="4624c-173">Cosmos DB futtatásához tárolt eljárások a felhasználónak kell rendelkeznie az All engedély a gyűjteményt, amelyben a tárolt eljárás fog futni.</span><span class="sxs-lookup"><span data-stu-id="4624c-173">In order to run Cosmos DB stored procedures the user must have the All permission on the collection in which the stored procedure will be run.</span></span>
> 
> 

### <a name="code-sample-to-create-permission"></a><span data-ttu-id="4624c-174">Kódminta engedély létrehozása</span><span class="sxs-lookup"><span data-stu-id="4624c-174">Code sample to create permission</span></span>

<span data-ttu-id="4624c-175">A következő példakód bemutatja, hogyan hozzon létre egy engedély erőforrást, olvassa el az erőforrás-jogkivonat a engedély erőforrás, és rendelje hozzá a az engedélyeket a [felhasználói](#users) a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4624c-175">The following code sample shows how to create a permission resource, read the resource token of the permission resource, and associate the permissions with the [user](#users) created above.</span></span>

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = documentCollection.SelfLink,
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

<span data-ttu-id="4624c-176">Ha a gyűjtemény adott meg partíciókulcsot, az engedély a gyűjtemény, a dokumentum és a melléklet erőforrások tartalmaznia mellett a ResourceLink ResourcePartitionKey.</span><span class="sxs-lookup"><span data-stu-id="4624c-176">If you have specified a partition key for your collection, then the permission for collection, document, and attachment resources must also include the ResourcePartitionKey in addition to the ResourceLink.</span></span>

### <a name="code-sample-to-read-permissions-for-user"></a><span data-ttu-id="4624c-177">Olvassa el a felhasználó engedélyeit kódminta</span><span class="sxs-lookup"><span data-stu-id="4624c-177">Code sample to read permissions for user</span></span>

<span data-ttu-id="4624c-178">Könnyen beszerzése az all engedély erőforrásokat egy adott felhasználóhoz társított, Cosmos DB teszi elérhetővé minden felhasználói objektum hírcsatorna engedély.</span><span class="sxs-lookup"><span data-stu-id="4624c-178">To easily obtain all permission resources associated with a particular user, Cosmos DB makes available a permission feed for each user object.</span></span>  <span data-ttu-id="4624c-179">A következő kódrészletet bemutatja, hogyan beolvasni az előbb létrehozott felhasználói rendelt engedélynél, engedély listáját, és hozható létre egy új documentclient-ügyfelet a felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="4624c-179">The following code snippet shows how to retrieve the permission associated with the user created above, construct a permission list, and instantiate a new DocumentClient on behalf of the user.</span></span>

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a><span data-ttu-id="4624c-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4624c-180">Next steps</span></span>
* <span data-ttu-id="4624c-181">Cosmos-adatbázis adatbázis-biztonsággal kapcsolatos további tudnivalókért lásd: [Cosmos DB: adatbázis-biztonsági](database-security.md).</span><span class="sxs-lookup"><span data-stu-id="4624c-181">To learn more about Cosmos DB database security, see [Cosmos DB: Database security](database-security.md).</span></span>
* <span data-ttu-id="4624c-182">Fő és a csak olvasható kezelésével kapcsolatos információkért lásd: [Azure Cosmos DB fiók kezelése](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="4624c-182">To learn about managing master and read-only keys, see [How to manage an Azure Cosmos DB account](manage-account.md#keys).</span></span>
* <span data-ttu-id="4624c-183">Azure Cosmos DB engedélyezési jogkivonatok létrehozásához, lásd: [Azure Cosmos DB erőforrások hozzáférés-vezérlés](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span><span class="sxs-lookup"><span data-stu-id="4624c-183">To learn how to construct Azure Cosmos DB authorization tokens, see [Access Control on Azure Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span></span>
