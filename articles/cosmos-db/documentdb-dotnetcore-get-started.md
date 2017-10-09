---
title: "Azure Cosmos DB: DocumentDB API bevezetés .NET Core oktatóanyaggal | Microsoft Docs"
description: "Ez az oktatóanyag létrehoz egy online adatbázist és a C# konzolalkalmazást hello Azure Cosmos DB DocumentDB API .NET Core SDK használatával."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 5e7efb327252e9e73e9b2a340820eeecc6937fa5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-getting-started-with-hello-documentdb-api-and-net-core"></a>Az Azure Cosmos DB: Hello DocumentDB API és a .NET Core első lépések
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js MongoDB-hez](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

A DocumentDB API toohello Azure Cosmos DB Ismerkedés a .NET Core-oktatóanyag – Üdvözöljük! Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.

Az oktatóanyag a következőket ismerteti:

* Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók
* A Visual Studio megoldás konfigurálása
* Online adatbázis létrehozása
* Gyűjtemény létrehozása
* JSON-dokumentumok létrehozása
* Hello gyűjtemény lekérdezése
* Dokumentum cseréje
* Dokumentum törlése
* Hello adatbázis törlése

Nincs elég ideje? Ne aggódjon! a teljes megoldás hello érhető [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). Ugrás a toohello [hello teljes megoldás szakaszának beolvasása](#GetSolution) gyors utasításokért.

Toobuild szeretné, hogy a Xamarin iOS, Android vagy űrlapok DocumentDB API és a .NET Core SDK használata alkalmazás hello? Lásd: [használó alkalmazások mobil a Xamarinnal és Azure Cosmos DB](mobile-apps-with-xamarin.md).

> [!NOTE]
> hello Azure Cosmos DB .NET Core SDK ebben az oktatóanyagban használt még nem kompatibilis az univerzális Windows Platform (UWP-) alkalmazásokat. A .NET Core SDK-t támogató UWP-alkalmazások hello előzetes verzióját, e-mailek küldése túl[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

Most pedig lássunk neki!

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/). 
    * Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.
* [Visual Studio 2017](https://www.visualstudio.com/vs/) 
    * Ha MacOS vagy Linux dolgozik, parancssori hello .NET Core alkalmazásokat fejleszthet hello telepítésével [.NET Core SDK](https://www.microsoft.com/net/core#macos) hello platform az Ön által választott. 
    * Ha a Windows dolgozik, parancssori hello .NET Core alkalmazásokat fejleszthet hello telepítésével [.NET Core SDK](https://www.microsoft.com/net/core#windows). 
    * Használhat saját szerkesztőt is, vagy letöltheti az ingyenes [Visual Studio Code](https://code.visualstudio.com/) alkalmazást, amely Windows, Linux és MacOS rendszeren egyaránt működik. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. lépés: Azure Cosmos DB-fiók létrehozása
Hozzunk létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS). Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>2. lépés: A Visual Studio megoldás beállítása
1. Nyissa meg a **Visual Studio 2017-et** a számítógépén.
2. A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.
3. A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **.NET Core** / **Console Application (a .NET Core)**, a projekt elnevezése **DocumentDBGettingStarted**, és kattintson a **OK**.

   ![Képernyőfelvétel a hello új projekt ablakról](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. A hello **Megoldáskezelőben**, kattintson a jobb gombbal **DocumentDBGettingStarted**.
5. Ne maradjanak hello menüben, kattintson a **NuGet-csomagok kezelése...** .

   ![Képernyőfelvétel a hello jobbra a hello projekt helyi](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. A hello **NuGet** lapra, majd **Tallózás** hello és írja be a hello tetején **az azure documentdb** hello Keresés mezőbe.
7. Hello eredmények belül található **Microsoft.Azure.DocumentDB.Core** kattintson **telepítése**.
   a DocumentDB ügyféloldali kódtára a .NET Core hello hello csomag azonosítója [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Ha a .NET-keretrendszer olyan verzióját célozza meg (például net461), amelyet ez a .NET Core NuGet csomag nem támogat, használja a [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)-t, amely a .NET-keretrendszer 4.5-ös verziójától kezdődően minden verziót támogat.
8. Hello kér, fogadja el a hello NuGet csomag telepítése és hello licencszerződést.

Remek! Most, hogy befejeztük hello beállítása, először néhány kódot ír. A [GitHubon](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) megtalálhatja az oktatóanyagban szereplő kódprojekt befejezett változatát.

## <a id="Connect"></a>3. lépés: Csatlakozás tooan Azure Cosmos DB fiók
Először adja hozzá ezeket a C#-alkalmazás, hello Program.cs fájl elejéhez toohello hivatkozik:

```csharp
using System;

// ADD THIS PART tooYOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

> [!IMPORTANT]
> A rendezés toocomplete ebben az oktatóanyagban, mindenképpen adja hozzá a hello függőségeket.

Most adja hozzá ezt a két állandót és az *ügyfél* változót a *Program* nyilvános osztály alatt.

```csharp
class Program
{
    // ADD THIS PART tooYOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

A következő head toohello [Azure Portal](https://portal.azure.com) tooretrieve URI és elsődleges kulcs. hello Azure Cosmos DB URI és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.

A hello Azure portál, keresse meg a tooyour Azure Cosmos DB fiókot, és kattintson **kulcsok**.

Hello portálról hello URI másolja és illessze be azt `<your endpoint URI>` hello program.cs fájlban. Ezután másolási elsődleges kulcs hello hello portálról, és illessze be azt `<your key>`. Hello Azure Cosmos DB emulátor használata `https://localhost:8081` hello végpont és hello jól meghatározott hitelesítési kulcsát, [hogyan toodevelop használatával hello Azure Cosmos DB emulátor](local-emulator.md). Győződjön meg arról, hogy tooremove hello < és >, de hagyjon hello a végpont és a kulcs dupla idézőjelbe.

![Képernyőfelvétel a hello hello NoSQL-oktatóanyag toocreate a C# Konzolalkalmazás által használt Azure-portálról. Egy Azure Cosmos DB megjeleníti a fiók, hello ACTIVE központ, a hello hello Azure Cosmos DB fiók panelén lévő KEYS gomb és hello URI, elsődleges és másodlagos kulcsot értékek kiemelésével a hello (kulcsok) panelén][keys]

Hello első lépések alkalmazáshoz hozzon létre egy új példányát hello először foglalkozunk **DocumentClient**.

Alább hello **fő** módszer, vegye fel a elnevezésű új aszinkron feladatot **GetStartedDemo**, amely létrehozza nekünk az új **DocumentClient**.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART tooYOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Hello következő kódot toorun az aszinkron feladat hozzáadása a **fő** metódust. Hello **fő** metódus kivételeket, és azt toohello konzol írja azokat.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART tooYOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key tooexit.");
                Console.ReadKey();
        }
```

Nyomja le az hello **DocumentDBGettingStarted** toobuild gombra, és hello alkalmazás futtatásához.

Gratulálunk! Sikeresen csatlakozott tooan Azure Cosmos DB fiók, most vessünk egy pillantást a Azure Cosmos DB erőforrásokat.  

## <a name="step-4-create-a-database"></a>4. lépés: Adatbázis létrehozása
Az adatbázis létrehozásához hello kód hozzáadása előtt adjon hozzá egy segédmetódust toohello konzol írásához.

Másolja és illessze be a hello **WriteToConsoleAndPromptToContinue** hello metódust **GetStartedDemo** metódust.

```csharp
// ADD THIS PART tooYOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

Az Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) hello metódusában **DocumentClient** osztály. Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.

Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello ügyfél létrehozása alatt. Ezzel létrehoz egy *FamilyDB* elnevezésű adatbázist.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.  

## <a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása
> [!WARNING]
> A **CreateDocumentCollectionAsync** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után. További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).

A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) hello metódusában **DocumentClient** osztály. A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.

Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello adatbázis létrehozása alatt. Ezzel létrehoz egy *FamilyCollection_oa* elnevezésű dokumentumgyűjteményt.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeresen létrehozott egy Azure Cosmos DB-dokumentumgyűjteményt.  

## <a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása
A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) hello metódusában **DocumentClient** osztály. A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak. Most már beilleszthetünk egy vagy több dokumentumot. Ha van olyan adat, milyen toostore az adatbázisban, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md).

Először létre kell toocreate egy **termékcsalád** osztály, amely a mintában Azure Cosmos DB tárolt objektumokat képviseli. Létrehozunk még egy **Szülő**, **Gyermek**, **Háziállat** és **Cím** alosztályt is a **Család** osztályban való használatra. Ne feledje, hogy a dokumentumoknak rendelkezniük kell egy **Azonosító** tulajdonsággal, amely a JSON-fájlban **id**-ként van szerializálva. Az osztályok létrehozásához adja hozzá a következő belső alosztályok után hello hello **GetStartedDemo** metódust.

Másolja és illessze be a hello **termékcsalád**, **szülő**, **gyermek**, **háziállat**, és **cím** osztályokat Hello **WriteToConsoleAndPromptToContinue** metódust.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key toocontinue ...");
    Console.ReadKey();
}

// ADD THIS PART tooYOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

Másolja és illessze be a hello **CreateFamilyDocumentIfNotExists** metódust a **CreateDocumentCollectionIfNotExists** metódust.

```csharp
// ADD THIS PART tooYOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

Majd szúrja be két dokumentumot, egyet mindegyik hello Andersen családhoz, majd hello Wakefield családhoz.

Másolja és illessze be a következő hello kód `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** metódusba hello dokumentumgyűjtemény létrehozása alatt.

```csharp
await this.CreateDatabaseIfNotExists("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeresen létrehozott két Azure Cosmos DB-dokumentumot.  

![Diagram szemléltető hello hierarchikus kapcsolatát hello fiók hello online adatbázis, gyűjtemény hello és hello NoSQL-oktatóanyag toocreate által használt hello dokumentumok a C# Konzolalkalmazás](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése
Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).  hello következő mintakód bemutatja, több olyan lekérdezést, mert mindkét Azure Cosmos DB SQL használatával szintaxisát, valamint a LINQ -, amely azt is futtathatók hello azt hello előző lépésben beszúrt dokumentumokat.

Másolja és illessze be a hello **ExecuteSimpleQuery** metódust a **CreateFamilyDocumentIfNotExists** metódust.

```csharp
// ADD THIS PART tooYOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find hello Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // hello query is executed synchronously here, but can also be executed asynchronously via hello IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute hello same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello második dokumentum létrehozása alatt.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART tooYOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeres lekérdezést végzett egy Azure Cosmos DB-gyűjteményen.

hello következő diagram azt ábrázolja, hogyan hello Azure Cosmos adatbázis SQL-lekérdezés szintaxisa hívást hello gyűjtemény létrehozása, és hello ugyanez a logika vonatkozik toohello LINQ-lekérdezésekre is.

![A C# Konzolalkalmazás hello NoSQL-oktatóanyag toocreate használja és hello lekérdezés jelentését hello hatókör ábrázoló diagram](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

Hello [FROM](documentdb-sql-query.md#FromClause) kulcsszó nem kötelező, hello lekérdezésben, mert Azure Cosmos DB lekérdezések már hatókörön belüli tooa egyetlen gyűjtemény. Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre. A DocumentDB következtethető ki, hogy családok, legfelső szintű vagy hello változó neve választja, hivatkozás hello aktuális gyűjtemény alapértelmezés szerint.

## <a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje
Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.  

Másolja és illessze be a hello **ReplaceFamilyDocument** metódust a **ExecuteSimpleQuery** metódust.

```csharp
// ADD THIS PART tooYOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    try
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
        this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello lekérdezés végrehajtása alá. Miután kicserélte hello dokumentum, ez fog futni hello azonos újra lekérdezése tooview hello megváltozott dokumentum.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
// Update hello Grade of hello Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.

## <a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése
Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.  

Másolja és illessze be a hello **DeleteFamilyDocument** metódust a **ReplaceFamilyDocument** metódust.

```csharp
// ADD THIS PART tooYOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    try
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine("Deleted Family {0}", documentName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello második lekérdezés végrehajtása alá.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooCODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeresen törölt egy Azure Cosmos DB-dokumentumot.

## <a id="DeleteDatabase"></a>10. lépés: Hello adatbázis törlése
A létrehozott adatbázis törlésével hello eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).

Másolás és beillesztés hello következő code tooyour **GetStartedDemo** hello dokumentum metódust toodelete hello adatbázis és az összes gyermek-erőforrás törlése.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART tooCODE
// Clean up/delete hello database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.

Gratulálunk! Sikeresen törölt egy Azure Cosmos DB-adatbázist.

## <a id="Run"></a>11. lépés: Futtassa a teljes C# konzolalkalmazást!
Nyomja le az hello **DocumentDBGettingStarted** gombra a Visual Studio toobuild hello alkalmazás hibakeresési módban.

Meg kell jelennie az első lépések alkalmazás hello konzolablakban hello kimenetét. hello kimenet hello hello eredményét megjeleníti azt hozzá, és meg kell felelnie az alábbi hello mintaszöveggel lekérdezések.

```
Created FamilyDB_oa
Press any key toocontinue ...
Created FamilyCollection_oa
Press any key toocontinue ...
Created Family Andersen.1
Press any key toocontinue ...
Created Family Wakefield.7
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key tooexit.
```

Gratulálunk! Hello az oktatóanyag elvégzése után, és van egy működő C# konzolalkalmazást!

## <a id="GetSolution"></a>Hello teljes oktatóanyag megoldás beszerzése
toobuild hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben, szüksége lesz a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).
* Egy [Azure Cosmos DB fiók][create-documentdb-dotnet.md#create-account].
* Hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) megoldás elérhető a Githubon.

toorestore hello hivatkozások toohello DocumentDB API for Azure Cosmos DB .NET Core SDK-t a Visual Studióban, kattintson a jobb gombbal a hello **GetStarted** Megoldáskezelőben, majd megoldás **engedélyezése NuGet-csomagok visszaállításának**. Következő lépésként hello Program.cs fájlban frissítse hello végponti URL-cím és az AuthorizationKey értékeket a [tooan Azure Cosmos DB fiók csatlakozás](#Connect).

## <a name="next-steps"></a>Következő lépések
* Összetettebb ASP.NET MVC-oktatóanyagot szeretne? Lásd: [ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-dotnet-application.md).
* Toodevelop szeretné, hogy a Xamarin iOS, Android vagy űrlapok DocumentDB API-t használó hello Azure Cosmos DB .NET Core SDK-t? Lásd: [használó alkalmazások mobil a Xamarinnal és Azure Cosmos DB](mobile-apps-with-xamarin.md).
* Szeretné, hogy tooperform méretezés és teljesítmény Azure Cosmos DB tesztelték? Lásd: [teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése](performance-testing.md)
* Ismerje meg, hogyan túl[figyelő Azure Cosmos DB kéréseket, a használati és a tárolási](monitor-accounts.md).
* A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).
* toolearn hello programozási modell kapcsolatos további információkért lásd: [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

[create-documentdb-dotnet.md#create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png
