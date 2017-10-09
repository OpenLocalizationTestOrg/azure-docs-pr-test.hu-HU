---
title: "aaaLearn toosecure hogyan érik el az Azure Cosmos Adatbázisba toodata |} Microsoft Docs"
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
ms.openlocfilehash: fef7f8e14b488f6ceab0f2aa279a1e99d4416f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-cosmos-db-data"></a><span data-ttu-id="18ccd-103">Hozzáférés tooAzure Cosmos DB adatok védelme</span><span class="sxs-lookup"><span data-stu-id="18ccd-103">Securing access tooAzure Cosmos DB data</span></span>
<span data-ttu-id="18ccd-104">Ez a cikk áttekintést biztonságossá tétele a hozzáférés toodata tárolt [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="18ccd-104">This article provides an overview of securing access toodata stored in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

<span data-ttu-id="18ccd-105">Azure Cosmos DB kétféle kulcsok tooauthenticate felhasználók használ, és adja meg a hozzáférés tooits adatokat és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="18ccd-105">Azure Cosmos DB uses two types of keys tooauthenticate users and provide access tooits data and resources.</span></span> 

|<span data-ttu-id="18ccd-106">kulcs típusa</span><span class="sxs-lookup"><span data-stu-id="18ccd-106">Key type</span></span>|<span data-ttu-id="18ccd-107">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="18ccd-107">Resources</span></span>|
|---|---|
|[<span data-ttu-id="18ccd-108">Főkulcsok</span><span class="sxs-lookup"><span data-stu-id="18ccd-108">Master keys</span></span>](#master-keys) |<span data-ttu-id="18ccd-109">Felügyeleti erőforrások használt: adatbázis-fiókok, adatbázisok, felhasználók és engedélyek</span><span class="sxs-lookup"><span data-stu-id="18ccd-109">Used for administrative resources: database accounts, databases, users, and permissions</span></span>|
|[<span data-ttu-id="18ccd-110">Erőforrás-tokenek</span><span class="sxs-lookup"><span data-stu-id="18ccd-110">Resource tokens</span></span>](#resource-tokens)|<span data-ttu-id="18ccd-111">Alkalmazás-erőforrásokat használt: gyűjtemények, dokumentumok, a mellékletek, tárolt eljárások, eseményindítók és felhasználó által megadott függvények</span><span class="sxs-lookup"><span data-stu-id="18ccd-111">Used for application resources: collections, documents, attachments, stored procedures, triggers, and UDFs</span></span>|

<a id="master-keys"></a>

## <a name="master-keys"></a><span data-ttu-id="18ccd-112">Főkulcsok</span><span class="sxs-lookup"><span data-stu-id="18ccd-112">Master keys</span></span> 

<span data-ttu-id="18ccd-113">Főkulcsok hozzáférés toohello hello felügyeleti erőforrások hello adatbázis fiók megadása.</span><span class="sxs-lookup"><span data-stu-id="18ccd-113">Master keys provide access toohello all hello administrative resources for hello database account.</span></span> <span data-ttu-id="18ccd-114">Főkulcsok:</span><span class="sxs-lookup"><span data-stu-id="18ccd-114">Master keys:</span></span>  
- <span data-ttu-id="18ccd-115">Adja meg a hozzáférés tooaccounts, adatbázisok, felhasználók és engedélyek.</span><span class="sxs-lookup"><span data-stu-id="18ccd-115">Provide access tooaccounts, databases, users, and permissions.</span></span> 
- <span data-ttu-id="18ccd-116">Nem lehet használt tooprovide részletes hozzáférés toocollections és dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="18ccd-116">Cannot be used tooprovide granular access toocollections and documents.</span></span>
- <span data-ttu-id="18ccd-117">Egy olyan fiók hello létrehozása során jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="18ccd-117">Are created during hello creation of an account.</span></span>
- <span data-ttu-id="18ccd-118">Bármikor helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="18ccd-118">Can be regenerated at any time.</span></span>

<span data-ttu-id="18ccd-119">Minden felhasználói fiókhoz két főkulcsok áll: egy elsődleges és másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="18ccd-119">Each account consists of two Master keys: a primary key and secondary key.</span></span> <span data-ttu-id="18ccd-120">hello kettős kulcsok célja, hogy generálja újra, vagy állítsa a kulcsokat, biztosít a folyamatos hozzáférés tooyour fiókot és az adatok.</span><span class="sxs-lookup"><span data-stu-id="18ccd-120">hello purpose of dual keys is so that you can regenerate, or roll keys, providing continuous access tooyour account and data.</span></span> 

<span data-ttu-id="18ccd-121">Továbbá toohello két fő kulcsok hello Cosmos DB fiók csak olvasható két kulcs van.</span><span class="sxs-lookup"><span data-stu-id="18ccd-121">In addition toohello two master keys for hello Cosmos DB account, there are two read-only keys.</span></span> <span data-ttu-id="18ccd-122">A csak olvasható kulcsokat csak hello fiók az olvasási műveletek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="18ccd-122">These read-only keys only allow read operations on hello account.</span></span> <span data-ttu-id="18ccd-123">Csak olvasható kulcsok nem biztosít hozzáférést tooread engedélyek erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="18ccd-123">Read-only keys do not provide access tooread permissions resources.</span></span>

<span data-ttu-id="18ccd-124">Elsődleges, másodlagos csak olvasható és írható-olvasható főkulcsok lekérhető, és újra létrehozza a rendszer hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="18ccd-124">Primary, secondary, read only, and read-write master keys can be retrieved and regenerated using hello Azure portal.</span></span> <span data-ttu-id="18ccd-125">Útmutatásért lásd: [megtekintése, másolása és újragenerálása hívóbetűk](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="18ccd-125">For instructions, see [View, copy, and regenerate access keys](manage-account.md#keys).</span></span>

![Hello Azure portál – bemutatásához NoSQL-adatbázis biztonsági hozzáférés-vezérlés (IAM)](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

<span data-ttu-id="18ccd-127">hello az elforgatás a főkulcs nem egyszerű.</span><span class="sxs-lookup"><span data-stu-id="18ccd-127">hello process of rotating your master key is simple.</span></span> <span data-ttu-id="18ccd-128">Keresse meg az Azure portál tooretrieve toohello a másodlagos kulcsot, majd cserélje le az elsődleges kulcs az alkalmazásban a másodlagos kulcs elforgatása hello elsődleges kulcs a következő hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="18ccd-128">Navigate toohello Azure portal tooretrieve your secondary key, then replace your primary key with your secondary key in your application, then rotate hello primary key in hello Azure portal.</span></span>

![Hello Azure portál – NoSQL-adatbázis biztonsági, amely tartalmazza a főkulcs Elforgatás](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-toouse-a-master-key"></a><span data-ttu-id="18ccd-130">A minta toouse főkulcs kód</span><span class="sxs-lookup"><span data-stu-id="18ccd-130">Code sample toouse a master key</span></span>

<span data-ttu-id="18ccd-131">hello alábbi kódminta bemutatja, hogyan toouse egy Cosmos DB fiók végpont és a főkulcs tooinstantiate egy DocumentClient, és hozzon létre egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="18ccd-131">hello following code sample illustrates how toouse a Cosmos DB account endpoint and master key tooinstantiate a DocumentClient and create a database.</span></span> 

```csharp
//Read hello Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from hello Azure portal on hello Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access tooyour DocDB account.

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

## <a name="resource-tokens"></a><span data-ttu-id="18ccd-132">Erőforrás-tokenek</span><span class="sxs-lookup"><span data-stu-id="18ccd-132">Resource tokens</span></span>

<span data-ttu-id="18ccd-133">Erőforrás-jogkivonatokat adatbázisban lévő alkalmazás-erőforrásokat toohello hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="18ccd-133">Resource tokens provide access toohello application resources within a database.</span></span> <span data-ttu-id="18ccd-134">Erőforrás-jogkivonatokat:</span><span class="sxs-lookup"><span data-stu-id="18ccd-134">Resource tokens:</span></span>
- <span data-ttu-id="18ccd-135">Adja meg a hozzáférés toospecific gyűjtemények, partíciós kulcsok, dokumentumok, mellékleteket, tárolt eljárások, eseményindítók és felhasználó által megadott függvények.</span><span class="sxs-lookup"><span data-stu-id="18ccd-135">Provide access toospecific collections, partition keys, documents, attachments, stored procedures, triggers, and UDFs.</span></span>
- <span data-ttu-id="18ccd-136">Jönnek létre, amikor egy [felhasználói](#users) kap [engedélyek](#permissions) tooa adott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="18ccd-136">Are created when a [user](#users) is granted [permissions](#permissions) tooa specific resource.</span></span>
- <span data-ttu-id="18ccd-137">Jönnek létre újból, akkor egy engedély erőforrás van a FELADÁS egy vagy több, a GET vagy a PUT hívás.</span><span class="sxs-lookup"><span data-stu-id="18ccd-137">Are recreated when a permission resource is acted upon on by POST, GET, or PUT call.</span></span>
- <span data-ttu-id="18ccd-138">Hello felhasználói, erőforrás és engedélye kifejezetten összeállított kivonatoló erőforrás jogkivonatot használja.</span><span class="sxs-lookup"><span data-stu-id="18ccd-138">Use a hash resource token specifically constructed for hello user, resource, and permission.</span></span>
- <span data-ttu-id="18ccd-139">Testre szabható érvényességi időtartam kötött megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="18ccd-139">Are time bound with a customizable validity period.</span></span> <span data-ttu-id="18ccd-140">hello alapértelmezett érvényes timespan egy óra.</span><span class="sxs-lookup"><span data-stu-id="18ccd-140">hello default valid timespan is one hour.</span></span> <span data-ttu-id="18ccd-141">A jogkivonatok élettartama, azonban explicit módon megadható tooa legfeljebb öt órát fel.</span><span class="sxs-lookup"><span data-stu-id="18ccd-141">Token lifetime, however, may be explicitly specified, up tooa maximum of five hours.</span></span>
- <span data-ttu-id="18ccd-142">Adjon meg egy biztonságos alternatív toogiving hello főkulcs ki.</span><span class="sxs-lookup"><span data-stu-id="18ccd-142">Provide a safe alternative toogiving out hello master key.</span></span> 
- <span data-ttu-id="18ccd-143">Engedélyezi az ügyfelek tooread, írási és törlési erőforrások hello Cosmos DB fiók már kapott toohello engedélyek alapján történik.</span><span class="sxs-lookup"><span data-stu-id="18ccd-143">Enable clients tooread, write, and delete resources in hello Cosmos DB account according toohello permissions they've been granted.</span></span>

<span data-ttu-id="18ccd-144">Egy erőforrás-jogkivonat használhatja (létrehozásával Cosmos DB felhasználókat és engedélyeket) kívánt tooprovide hozzáférés tooresources a Cosmos DB a fiók tooa ügyfél, amely nem adható meg megbízhatóként a hello főkulcs.</span><span class="sxs-lookup"><span data-stu-id="18ccd-144">You can use a resource token (by creating Cosmos DB users and permissions) when you want tooprovide access tooresources in your Cosmos DB account tooa client that cannot be trusted with hello master key.</span></span>  

<span data-ttu-id="18ccd-145">Cosmos DB erőforrás jogkivonatokat adjon meg egy biztonságos megoldás, amely lehetővé teszi az ügyfelek tooread, írási és törlési erőforrások alapján történik, hogy adott toohello engedélyek Cosmos DB fiókjában, és vagy a fő nélkül, vagy csak kulcs olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="18ccd-145">Cosmos DB resource tokens provide a safe alternative that enables clients tooread, write, and delete resources in your Cosmos DB account according toohello permissions you've granted, and without need for either a master or read only key.</span></span>

<span data-ttu-id="18ccd-146">Íme egy tipikus kialakítási mintája, amellyel erőforrás jogkivonatok kérhetők, jön létre, és tooclients kézbesíteni:</span><span class="sxs-lookup"><span data-stu-id="18ccd-146">Here is a typical design pattern whereby resource tokens may be requested, generated, and delivered tooclients:</span></span>

1. <span data-ttu-id="18ccd-147">Egy közepes réteg szolgáltatás tooserve be van állítva egy mobilalkalmazás tooshare felhasználó fényképeihez.</span><span class="sxs-lookup"><span data-stu-id="18ccd-147">A mid-tier service is set up tooserve a mobile application tooshare user photos.</span></span> 
2. <span data-ttu-id="18ccd-148">hello közepes réteg szolgáltatás hello főkulcs a hello Cosmos DB fiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="18ccd-148">hello mid-tier service possesses hello master key of hello Cosmos DB account.</span></span>
3. <span data-ttu-id="18ccd-149">hello fényképező alkalmazás telepítve van a végfelhasználói mobileszközökön.</span><span class="sxs-lookup"><span data-stu-id="18ccd-149">hello photo app is installed on end-user mobile devices.</span></span> 
4. <span data-ttu-id="18ccd-150">A bejelentkezés hello fényképező alkalmazás hello felhasználó identitásával az hello hello közepes réteg szolgáltatással hoz létre.</span><span class="sxs-lookup"><span data-stu-id="18ccd-150">On login, hello photo app establishes hello identity of hello user with hello mid-tier service.</span></span> <span data-ttu-id="18ccd-151">A mechanizmus identitás létesítmény tisztán toohello alkalmazás működik.</span><span class="sxs-lookup"><span data-stu-id="18ccd-151">This mechanism of identity establishment is purely up toohello application.</span></span>
5. <span data-ttu-id="18ccd-152">Miután hello identitása létrejött, hello közepes réteg a szolgáltatáskérések engedélyek hello identitása alapján.</span><span class="sxs-lookup"><span data-stu-id="18ccd-152">Once hello identity is established, hello mid-tier service requests permissions based on hello identity.</span></span>
6. <span data-ttu-id="18ccd-153">hello közepes réteg szolgáltatás egy erőforrás-jogkivonat hátsó toohello telefonalkalmazás küld.</span><span class="sxs-lookup"><span data-stu-id="18ccd-153">hello mid-tier service sends a resource token back toohello phone app.</span></span>
7. <span data-ttu-id="18ccd-154">hello telefonalkalmazás hello erőforrás-jogkivonat és erőforrás-jogkivonat hello által engedélyezett hello időköz definiált hello engedélyek folytatható toouse hello erőforrás token toodirectly Cosmos DB hozzáférését az erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="18ccd-154">hello phone app can continue toouse hello resource token toodirectly access Cosmos DB resources with hello permissions defined by hello resource token and for hello interval allowed by hello resource token.</span></span> 
8. <span data-ttu-id="18ccd-155">Amikor hello erőforrás-token érvényessége lejár, a későbbi kérelmeket kapnak a 401-es jogosulatlan kivételt.</span><span class="sxs-lookup"><span data-stu-id="18ccd-155">When hello resource token expires, subsequent requests receive a 401 unauthorized exception.</span></span>  <span data-ttu-id="18ccd-156">Ezen a ponton hello telefonalkalmazás hello identitás újból létrehozza, és egy új erőforrás-jogkivonat kéri.</span><span class="sxs-lookup"><span data-stu-id="18ccd-156">At this point, hello phone app re-establishes hello identity and requests a new resource token.</span></span>

    ![Az Azure Cosmos DB erőforrás jogkivonatok munkafolyamat](./media/secure-access-to-data/resourcekeyworkflow.png)

<span data-ttu-id="18ccd-158">Erőforrás-token létrehozása és kezelése kezeli hello natív Cosmos DB ügyfél könyvtárakat. Ha REST hello kérelem/hitelesítési fejlécek kell összeállítani.</span><span class="sxs-lookup"><span data-stu-id="18ccd-158">Resource token generation and management is handled by hello native Cosmos DB client libraries; however, if you use REST you must construct hello request/authentication headers.</span></span> <span data-ttu-id="18ccd-159">A többi hitelesítési fejlécek létrehozásáról további információk: [Cosmos DB erőforrások hozzáférés-vezérlés](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) vagy hello [forráskód az SDK-k](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span><span class="sxs-lookup"><span data-stu-id="18ccd-159">For more information on creating authentication headers for REST, see [Access Control on Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) or hello [source code for our SDKs](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span></span>

<span data-ttu-id="18ccd-160">Példa a középső réteg szolgáltatás toogenerate vagy broker erőforrás jogkivonatokat használ, a következő témakörben: hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span><span class="sxs-lookup"><span data-stu-id="18ccd-160">For an example of a middle tier service used toogenerate or broker resource tokens, see hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span></span>

<a id="users"></a>

## <a name="users"></a><span data-ttu-id="18ccd-161">Felhasználók</span><span class="sxs-lookup"><span data-stu-id="18ccd-161">Users</span></span>
<span data-ttu-id="18ccd-162">Cosmos DB felhasználók hozzárendelve egy Cosmos DB adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="18ccd-162">Cosmos DB users are associated with a Cosmos DB database.</span></span>  <span data-ttu-id="18ccd-163">Az egyes adatbázisok nulla vagy több Cosmos DB felhasználókat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="18ccd-163">Each database can contain zero or more Cosmos DB users.</span></span>  <span data-ttu-id="18ccd-164">a következő kód a minta bemutatja hogyan hello toocreate egy Cosmos DB felhasználói erőforrás.</span><span class="sxs-lookup"><span data-stu-id="18ccd-164">hello following code sample shows how toocreate a Cosmos DB user resource.</span></span>

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> <span data-ttu-id="18ccd-165">Minden egyes Cosmos DB felhasználói lehet használt tooretrieve hello listája PermissionsLink tulajdonsággal rendelkezik [engedélyek](#permissions) hello felhasználóhoz társított.</span><span class="sxs-lookup"><span data-stu-id="18ccd-165">Each Cosmos DB user has a PermissionsLink property that can be used tooretrieve hello list of [permissions](#permissions) associated with hello user.</span></span>
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a><span data-ttu-id="18ccd-166">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="18ccd-166">Permissions</span></span>
<span data-ttu-id="18ccd-167">A Cosmos DB engedély erőforrás társítva egy Cosmos DB felhasználói.</span><span class="sxs-lookup"><span data-stu-id="18ccd-167">A Cosmos DB permission resource is associated with a Cosmos DB user.</span></span>  <span data-ttu-id="18ccd-168">Minden felhasználó nulla vagy több Cosmos DB engedélyeket is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="18ccd-168">Each user may contain zero or more Cosmos DB permissions.</span></span>  <span data-ttu-id="18ccd-169">Engedély erőforrás biztosít a hozzáférési tooa biztonsági jogkivonatot, amely hello szükségleteinek tooaccess közben egy adott alkalmazás erőforrást.</span><span class="sxs-lookup"><span data-stu-id="18ccd-169">A permission resource provides access tooa security token that hello user needs when trying tooaccess a specific application resource.</span></span>
<span data-ttu-id="18ccd-170">Van engedélye az erőforrás által is megadható két rendelkezésre álló hozzáférési szintek:</span><span class="sxs-lookup"><span data-stu-id="18ccd-170">There are two available access levels that may be provided by a permission resource:</span></span>

* <span data-ttu-id="18ccd-171">Összes: hello felhasználó teljes körű hozzáféréssel rendelkezik a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="18ccd-171">All: hello user has full permission on hello resource.</span></span>
* <span data-ttu-id="18ccd-172">Olvassa el: hello felhasználó hello erőforrás hello tartalmát csak olvashatja, de nem hajtható végre, írási, update vagy delete művelethez hello erőforráson.</span><span class="sxs-lookup"><span data-stu-id="18ccd-172">Read: hello user can only read hello contents of hello resource but cannot perform write, update, or delete operations on hello resource.</span></span>

> [!NOTE]
> <span data-ttu-id="18ccd-173">A sorrend toorun Cosmos DB tárolt eljárások hello felhasználói engedéllyel kell rendelkeznie hello összes hello gyűjteményen, mely hello a tárolt eljárás fog futni.</span><span class="sxs-lookup"><span data-stu-id="18ccd-173">In order toorun Cosmos DB stored procedures hello user must have hello All permission on hello collection in which hello stored procedure will be run.</span></span>
> 
> 

### <a name="code-sample-toocreate-permission"></a><span data-ttu-id="18ccd-174">Kód a minta toocreate engedély</span><span class="sxs-lookup"><span data-stu-id="18ccd-174">Code sample toocreate permission</span></span>

<span data-ttu-id="18ccd-175">hello következő mintakód bemutatja, hogyan toocreate engedély erőforrás, olvassa el erőforrás-jogkivonat hello engedély erőforrás hello, és hello engedélyek társítása hello [felhasználói](#users) a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="18ccd-175">hello following code sample shows how toocreate a permission resource, read hello resource token of hello permission resource, and associate hello permissions with hello [user](#users) created above.</span></span>

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

<span data-ttu-id="18ccd-176">Ha a gyűjtemény, melléklet, gyűjtemény és dokumentum erőforrások majd hello engedély partíciókulcsot is magában kell foglalnia megadott hello ResourcePartitionKey hozzáadása toohello ResourceLink a.</span><span class="sxs-lookup"><span data-stu-id="18ccd-176">If you have specified a partition key for your collection, then hello permission for collection, document, and attachment resources must also include hello ResourcePartitionKey in addition toohello ResourceLink.</span></span>

### <a name="code-sample-tooread-permissions-for-user"></a><span data-ttu-id="18ccd-177">A minta tooread kódengedélyek felhasználó</span><span class="sxs-lookup"><span data-stu-id="18ccd-177">Code sample tooread permissions for user</span></span>

<span data-ttu-id="18ccd-178">tooeasily egy adott felhasználóhoz társított összes engedély erőforrások megszerzéséhez Cosmos DB elérhetővé engedélyt minden felhasználói objektum hírcsatorna.</span><span class="sxs-lookup"><span data-stu-id="18ccd-178">tooeasily obtain all permission resources associated with a particular user, Cosmos DB makes available a permission feed for each user object.</span></span>  <span data-ttu-id="18ccd-179">hello következő kódrészletet bemutatja, hogyan tooretrieve hello engedéllyel, a fenti létrehozott hello felhasználó társított egy engedélylistában összeállítani, és hozható létre egy új DocumentClient hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="18ccd-179">hello following code snippet shows how tooretrieve hello permission associated with hello user created above, construct a permission list, and instantiate a new DocumentClient on behalf of hello user.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="18ccd-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18ccd-180">Next steps</span></span>
* <span data-ttu-id="18ccd-181">További információ az adatbázis biztonsági Cosmos DB, toolearn lásd [Cosmos DB: adatbázis-biztonsági](database-security.md).</span><span class="sxs-lookup"><span data-stu-id="18ccd-181">toolearn more about Cosmos DB database security, see [Cosmos DB: Database security](database-security.md).</span></span>
* <span data-ttu-id="18ccd-182">toolearn fő és a csak olvasható kulcsok kezelésével kapcsolatban lásd: [hogyan toomanage Azure Cosmos DB fiók](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="18ccd-182">toolearn about managing master and read-only keys, see [How toomanage an Azure Cosmos DB account](manage-account.md#keys).</span></span>
* <span data-ttu-id="18ccd-183">Hogyan tooconstruct Azure Cosmos DB engedélyezési jogkivonatokat: toolearn [Azure Cosmos DB erőforrások hozzáférés-vezérlés](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span><span class="sxs-lookup"><span data-stu-id="18ccd-183">toolearn how tooconstruct Azure Cosmos DB authorization tokens, see [Access Control on Azure Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span></span>
