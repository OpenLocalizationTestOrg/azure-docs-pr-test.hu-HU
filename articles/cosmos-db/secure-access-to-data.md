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
# <a name="securing-access-tooazure-cosmos-db-data"></a>Hozzáférés tooAzure Cosmos DB adatok védelme
Ez a cikk áttekintést biztonságossá tétele a hozzáférés toodata tárolt [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB kétféle kulcsok tooauthenticate felhasználók használ, és adja meg a hozzáférés tooits adatokat és erőforrásokat. 

|kulcs típusa|Erőforrások|
|---|---|
|[Főkulcsok](#master-keys) |Felügyeleti erőforrások használt: adatbázis-fiókok, adatbázisok, felhasználók és engedélyek|
|[Erőforrás-tokenek](#resource-tokens)|Alkalmazás-erőforrásokat használt: gyűjtemények, dokumentumok, a mellékletek, tárolt eljárások, eseményindítók és felhasználó által megadott függvények|

<a id="master-keys"></a>

## <a name="master-keys"></a>Főkulcsok 

Főkulcsok hozzáférés toohello hello felügyeleti erőforrások hello adatbázis fiók megadása. Főkulcsok:  
- Adja meg a hozzáférés tooaccounts, adatbázisok, felhasználók és engedélyek. 
- Nem lehet használt tooprovide részletes hozzáférés toocollections és dokumentumok.
- Egy olyan fiók hello létrehozása során jönnek létre.
- Bármikor helyreállíthatja.

Minden felhasználói fiókhoz két főkulcsok áll: egy elsődleges és másodlagos kulcsot. hello kettős kulcsok célja, hogy generálja újra, vagy állítsa a kulcsokat, biztosít a folyamatos hozzáférés tooyour fiókot és az adatok. 

Továbbá toohello két fő kulcsok hello Cosmos DB fiók csak olvasható két kulcs van. A csak olvasható kulcsokat csak hello fiók az olvasási műveletek engedélyezése. Csak olvasható kulcsok nem biztosít hozzáférést tooread engedélyek erőforrásokat.

Elsődleges, másodlagos csak olvasható és írható-olvasható főkulcsok lekérhető, és újra létrehozza a rendszer hello Azure-portál használatával. Útmutatásért lásd: [megtekintése, másolása és újragenerálása hívóbetűk](manage-account.md#keys).

![Hello Azure portál – bemutatásához NoSQL-adatbázis biztonsági hozzáférés-vezérlés (IAM)](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

hello az elforgatás a főkulcs nem egyszerű. Keresse meg az Azure portál tooretrieve toohello a másodlagos kulcsot, majd cserélje le az elsődleges kulcs az alkalmazásban a másodlagos kulcs elforgatása hello elsődleges kulcs a következő hello Azure-portálon.

![Hello Azure portál – NoSQL-adatbázis biztonsági, amely tartalmazza a főkulcs Elforgatás](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-toouse-a-master-key"></a>A minta toouse főkulcs kód

hello alábbi kódminta bemutatja, hogyan toouse egy Cosmos DB fiók végpont és a főkulcs tooinstantiate egy DocumentClient, és hozzon létre egy adatbázist. 

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

## <a name="resource-tokens"></a>Erőforrás-tokenek

Erőforrás-jogkivonatokat adatbázisban lévő alkalmazás-erőforrásokat toohello hozzáférést biztosítanak. Erőforrás-jogkivonatokat:
- Adja meg a hozzáférés toospecific gyűjtemények, partíciós kulcsok, dokumentumok, mellékleteket, tárolt eljárások, eseményindítók és felhasználó által megadott függvények.
- Jönnek létre, amikor egy [felhasználói](#users) kap [engedélyek](#permissions) tooa adott erőforrás.
- Jönnek létre újból, akkor egy engedély erőforrás van a FELADÁS egy vagy több, a GET vagy a PUT hívás.
- Hello felhasználói, erőforrás és engedélye kifejezetten összeállított kivonatoló erőforrás jogkivonatot használja.
- Testre szabható érvényességi időtartam kötött megnyitásakor. hello alapértelmezett érvényes timespan egy óra. A jogkivonatok élettartama, azonban explicit módon megadható tooa legfeljebb öt órát fel.
- Adjon meg egy biztonságos alternatív toogiving hello főkulcs ki. 
- Engedélyezi az ügyfelek tooread, írási és törlési erőforrások hello Cosmos DB fiók már kapott toohello engedélyek alapján történik.

Egy erőforrás-jogkivonat használhatja (létrehozásával Cosmos DB felhasználókat és engedélyeket) kívánt tooprovide hozzáférés tooresources a Cosmos DB a fiók tooa ügyfél, amely nem adható meg megbízhatóként a hello főkulcs.  

Cosmos DB erőforrás jogkivonatokat adjon meg egy biztonságos megoldás, amely lehetővé teszi az ügyfelek tooread, írási és törlési erőforrások alapján történik, hogy adott toohello engedélyek Cosmos DB fiókjában, és vagy a fő nélkül, vagy csak kulcs olvasásakor.

Íme egy tipikus kialakítási mintája, amellyel erőforrás jogkivonatok kérhetők, jön létre, és tooclients kézbesíteni:

1. Egy közepes réteg szolgáltatás tooserve be van állítva egy mobilalkalmazás tooshare felhasználó fényképeihez. 
2. hello közepes réteg szolgáltatás hello főkulcs a hello Cosmos DB fiók rendelkezik.
3. hello fényképező alkalmazás telepítve van a végfelhasználói mobileszközökön. 
4. A bejelentkezés hello fényképező alkalmazás hello felhasználó identitásával az hello hello közepes réteg szolgáltatással hoz létre. A mechanizmus identitás létesítmény tisztán toohello alkalmazás működik.
5. Miután hello identitása létrejött, hello közepes réteg a szolgáltatáskérések engedélyek hello identitása alapján.
6. hello közepes réteg szolgáltatás egy erőforrás-jogkivonat hátsó toohello telefonalkalmazás küld.
7. hello telefonalkalmazás hello erőforrás-jogkivonat és erőforrás-jogkivonat hello által engedélyezett hello időköz definiált hello engedélyek folytatható toouse hello erőforrás token toodirectly Cosmos DB hozzáférését az erőforrásokhoz. 
8. Amikor hello erőforrás-token érvényessége lejár, a későbbi kérelmeket kapnak a 401-es jogosulatlan kivételt.  Ezen a ponton hello telefonalkalmazás hello identitás újból létrehozza, és egy új erőforrás-jogkivonat kéri.

    ![Az Azure Cosmos DB erőforrás jogkivonatok munkafolyamat](./media/secure-access-to-data/resourcekeyworkflow.png)

Erőforrás-token létrehozása és kezelése kezeli hello natív Cosmos DB ügyfél könyvtárakat. Ha REST hello kérelem/hitelesítési fejlécek kell összeállítani. A többi hitelesítési fejlécek létrehozásáról további információk: [Cosmos DB erőforrások hozzáférés-vezérlés](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) vagy hello [forráskód az SDK-k](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Példa a középső réteg szolgáltatás toogenerate vagy broker erőforrás jogkivonatokat használ, a következő témakörben: hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Felhasználók
Cosmos DB felhasználók hozzárendelve egy Cosmos DB adatbázisban.  Az egyes adatbázisok nulla vagy több Cosmos DB felhasználókat is tartalmazhat.  a következő kód a minta bemutatja hogyan hello toocreate egy Cosmos DB felhasználói erőforrás.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Minden egyes Cosmos DB felhasználói lehet használt tooretrieve hello listája PermissionsLink tulajdonsággal rendelkezik [engedélyek](#permissions) hello felhasználóhoz társított.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>Engedélyek
A Cosmos DB engedély erőforrás társítva egy Cosmos DB felhasználói.  Minden felhasználó nulla vagy több Cosmos DB engedélyeket is tartalmazhat.  Engedély erőforrás biztosít a hozzáférési tooa biztonsági jogkivonatot, amely hello szükségleteinek tooaccess közben egy adott alkalmazás erőforrást.
Van engedélye az erőforrás által is megadható két rendelkezésre álló hozzáférési szintek:

* Összes: hello felhasználó teljes körű hozzáféréssel rendelkezik a hello erőforrás.
* Olvassa el: hello felhasználó hello erőforrás hello tartalmát csak olvashatja, de nem hajtható végre, írási, update vagy delete művelethez hello erőforráson.

> [!NOTE]
> A sorrend toorun Cosmos DB tárolt eljárások hello felhasználói engedéllyel kell rendelkeznie hello összes hello gyűjteményen, mely hello a tárolt eljárás fog futni.
> 
> 

### <a name="code-sample-toocreate-permission"></a>Kód a minta toocreate engedély

hello következő mintakód bemutatja, hogyan toocreate engedély erőforrás, olvassa el erőforrás-jogkivonat hello engedély erőforrás hello, és hello engedélyek társítása hello [felhasználói](#users) a fenti létrehozott.

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

Ha a gyűjtemény, melléklet, gyűjtemény és dokumentum erőforrások majd hello engedély partíciókulcsot is magában kell foglalnia megadott hello ResourcePartitionKey hozzáadása toohello ResourceLink a.

### <a name="code-sample-tooread-permissions-for-user"></a>A minta tooread kódengedélyek felhasználó

tooeasily egy adott felhasználóhoz társított összes engedély erőforrások megszerzéséhez Cosmos DB elérhetővé engedélyt minden felhasználói objektum hírcsatorna.  hello következő kódrészletet bemutatja, hogyan tooretrieve hello engedéllyel, a fenti létrehozott hello felhasználó társított egy engedélylistában összeállítani, és hozható létre egy új DocumentClient hello felhasználó nevében.

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

## <a name="next-steps"></a>Következő lépések
* További információ az adatbázis biztonsági Cosmos DB, toolearn lásd [Cosmos DB: adatbázis-biztonsági](database-security.md).
* toolearn fő és a csak olvasható kulcsok kezelésével kapcsolatban lásd: [hogyan toomanage Azure Cosmos DB fiók](manage-account.md#keys).
* Hogyan tooconstruct Azure Cosmos DB engedélyezési jogkivonatokat: toolearn [Azure Cosmos DB erőforrások hozzáférés-vezérlés](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).
