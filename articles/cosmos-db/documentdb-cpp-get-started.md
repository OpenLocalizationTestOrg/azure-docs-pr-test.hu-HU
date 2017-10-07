---
title: "Azure Cosmos DB aaaC ++ oktatóanyaga |} Microsoft Docs"
description: "Ez egy C++ oktatóanyag, amely egy C++ adatbázis és egy konzolalkalmazás létrehozását ismerteti az Azure Cosmos DB által támogatott C++ SDK használatával. Az Azure Cosmos DB egy világméretű adatbázis-szolgáltatás."
services: cosmos-db
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 12/25/2016
ms.author: aasthan
ms.openlocfilehash: 2d5eeff349b7753e39936b7eb77557ad30c5830a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-hello-documentdb-api"></a>Az Azure Cosmos DB: C++ konzol oktatóanyag a DocumentDB API hello
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js MongoDB-hez](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Üdvözli a toohello C++ útmutató hello Azure Cosmos DB DocumentDB API SDK for C++ által támogatott! Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást hozhat létre, amely Azure Cosmos DB-erőforrásokat (például C++ adatbázisokat) hoz létre és kérdez le.

Az oktatóanyag a következőket ismerteti:

* Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók
* Az alkalmazás beállítása
* C++ Azure Cosmos DB-adatbázis létrehozása
* Gyűjtemény létrehozása
* JSON-dokumentumok létrehozása
* Hello gyűjtemény lekérdezése
* Dokumentum cseréje
* Dokumentum törlése
* Hello C++ Azure Cosmos DB adatbázis törlése

Nincs elég ideje? Ne aggódjon! a teljes megoldás hello érhető [GitHub](https://github.com/stalker314314/DocumentDBCpp). Lásd: [hello teljes megoldás beszerzése](#GetSolution) gyors utasításokért.

Hello C++ az oktatóanyag befejezése után adjon használata hello szavazás gombok hello alján a lap toogive nekünk visszajelzést. 

Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude megjegyzéseit a vagy [elérhetők a toous Itt](https://www.research.net/r/8BKRJ3Z). 

Most pedig lássunk neki!

## <a name="prerequisites-for-hello-c-tutorial"></a>Hello C++ oktatóanyag előfeltételei
Győződjön meg arról, hogy a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).
* [A Visual Studio](https://www.visualstudio.com/downloads/), amelyben hello C++ nyelvi összetevő telepítve van.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. lépés: Azure Cosmos DB-fiók létrehozása
Hozzunk létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a C++-alkalmazás beállítása](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>2. lépés: A C++ alkalmazás beállítása
1. Nyissa meg a Visual Studio, majd a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**. 
2. A hello **új projekt** ablak hello **telepített** ablaktáblában bontsa ki a **Visual C++**, kattintson a **Win32**, és kattintson a  **A Win32-Konzolalkalmazás**. Hello projekt hellodocumentdb nevet és kattintson **OK**. 
   
    ![Képernyőfelvétel a hello új projekt varázsló](media/documentdb-cpp-get-started/hello.png)
3. Hello Win32 alkalmazás varázsló elindításakor kattintson **Befejezés**.
4. Hello projekt létrehozása után, nyissa meg a NuGet-Csomagkezelő hello kattintson a jobb gombbal a hello **hellodocumentdb** a projekt **Megoldáskezelőben** elemre kattintva **NuGet-csomagokkezelése**. 
   
    ![Hello Projekt menü NuGet-csomag kezelni ábrázoló képernyőfelvétel](media/documentdb-cpp-get-started/nuget.png)
5. A hello **NuGet: hellodocumentdb** lapra, majd **Tallózás**, majd keresse meg a *documentdbcpp*. Hello eredményeit válassza ki DocumentDbCPP, ahogy az alábbi képernyőfelvétel a hello. Ez a csomag telepíti hivatkozások tooC ++ REST SDK, vagyis hello DocumentDbCPP függőségei.  
   
    ![Képernyőfelvétel: hello DocumentDbCpp csomag kiemelve](media/documentdb-cpp-get-started/cpp.png)
   
    Hello csomagok tooyour projekt lettek hozzáadva, ha folyamatban, minden set toostart néhány kódot ír.   

## <a id="Config"></a>3. lépés: Kapcsolat részleteinek másolása az Azure Portalról az Azure Cosmos DB-adatbázisba
Nyithat [Azure-portálon](https://portal.azure.com) és bejárhat toohello létrehozott Azure Cosmos DB adatbázis fiókot. Fel kell hello URI és elsődleges kulcs hello hello tovább lépés tooestablish kapcsolatot az Azure portálról származó a C++ kódrészletet. 

![Az Azure Cosmos DB URI és a kulcsot hello Azure-portálon](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>4. lépés: Csatlakozás tooan Azure Cosmos DB fiók
1. Adja hozzá a következő fejlécek és névterek tooyour forráskódját, után hello `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Ezután adja hozzá a hello alábbi code tooyour fő funkciója, és cserélje hello fiók konfigurálásával és az elsődleges kulcs toomatch a 3. lépés Azure Cosmos DB beállításait. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Most, hogy hello kód tooinitialize hello documentdb-ügyfél, vessen egy pillantást a Azure Cosmos DB erőforrásokat.

## <a id="CreateDBColl"></a>5. lépés: C++ adatbázis és gyűjtemény létrehozása
Most elvégezni ezt a lépést, mielőtt ugorjunk hogyan működnek együtt a adatbázis, gyűjtemény és dokumentumok azok is, akik új tooAzure Cosmos DB keresztül. Az [adatbázisok](documentdb-resources.md#databases) a dokumentumtároló gyűjtemények között particionált logikai tárolói. A [gyűjtemény](documentdb-resources.md#collections) JSON-dokumentumok és hello kapcsolódó JavaScript-alkalmazáslogika. További információ a hello Azure Cosmos DB hierarchikus erőforrás-modellje és fogalmai terhelésekről [Azure Cosmos DB hierarchikus erőforrás-modellje és fogalmai](documentdb-resources.md).

toocreate adatbázis és a megfelelő gyűjtemény hozzáadása a következő kód toohello vége a fő funkciója hello. Ez egy "FamilyRegistry" és a hello ügyfél konfigurációs meg az előző lépésben hello deklarált FamilyCollection nevű gyűjtemény nevű adatbázist hoz létre.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>6. lépés: Dokumentum létrehozása
A [dokumentumok](documentdb-resources.md#documents) a felhasználó által megadott (tetszőleges) JSON-tartalmak. Most már beszúrhat egy dokumentumot az Azure Cosmos DB-be. Létrehozhat egy dokumentum kód hello fő funkciója hello végét a következő hello másolásával. 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

toosummarize, ez a kód létrehoz egy Azure Cosmos DB adatbázis, gyűjtemény és dokumentumok, amely Azure-portálon található Dokumentumtallózó lekérdezheti. 

![C++ útmutató – hello fiók, hello adatbázis, gyűjtemény hello és hello dokumentumok hierarchikus kapcsolatát hello ábrázoló Diagram](media/documentdb-cpp-get-started/docs.png)

## <a id="QueryDB"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése
Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md). hello alábbi mintakód bemutatja hello előző lépésben létrehozott is futtathatók hello dokumentumok SQL-szintaxis használatával létrehozott lekérdezést.

hello függvény akkor argumentumok hello egyedi azonosítója vagy erőforrás-azonosító hello adatbázis és a hello gyűjtemény hello dokumentum ügyfél együtt. Adja ezt a kódot a fő függvény elé.

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Replace"></a>8. lépés: Dokumentum cseréje
Azure Cosmos-adatbázis mutatja be a kódját a következő hello támogatja a JSON-dokumentumok cseréjét. Hello executesimplequery függvény után adja hozzá ezt a kódot.

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <a id="Delete"></a>9. lépés: Dokumentum törlése
Az Azure Cosmos DB támogatja a JSON-dokumentumok törlése, akkor ehhez kód hello replacedocument függvény után a következő, a másolás és beillesztés hello. 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="DeleteDB"></a>10. lépés: Adatbázis törlése
Törlése hello létrehozott adatbázis eltávolítja hello adatbázis és az összes gyermek-erőforrás (gyűjtemények, dokumentumok stb.).

Másolja és illessze be a következő kódrészletet (függvény tisztítás) után hello deletedocument függvény tooremove hello adatbázis és az összes hello gyermekerőforrásait hello.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>11. lépés: A teljes C++ alkalmazás futtatása!
Azt most hozzáadott kódot toocreate, lekérdezése, módosítása és törlése különböző Azure Cosmos DB erőforrások.  Ossza meg velünk most hozzá kell fűznie ez fel a fő funkciója az egyes diagnosztikai üzenetek együtt hellodocumentdb.cpp hívások toothese a különböző funkciók hozzáadásával.

Hello fő funkciója az alkalmazás helyett a kód a következő hello azt is megteheti. Az írási műveletek hello account_configuration_uri és a hello kódra a 3. lépésben másolt primary_key úgy, hogy sor másolási hello értékei vagy újból menteni hello portálról. 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

Meg kell most képes toobuild és futtathatók a kódot a Visual Studio F5 billentyű lenyomásával vagy másik lehetőségként hello terminálablakot hello alkalmazás keresése és futtatása hello a végrehajtható. 

Az első lépések alkalmazás kimenetének hello kell megjelennie. hello kimeneti meg kell felelnie a következő képernyőkép hello.

![Azure Cosmos DB C++ alkalmazás kimenete](media/documentdb-cpp-get-started/console.png)

Gratulálunk! Hello C++ oktatóprogram elvégzése után, és rendelkezik az első Azure Cosmos DB konzolalkalmazást!

## <a id="GetSolution"></a>Hello teljes C++ oktatóanyag megoldás beszerzése
toobuild hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben, következőkre lesz szüksége hello:

* [Azure Cosmos DB-fiók][create-account].
* Hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) megoldás elérhető a Githubon.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).
* A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).
* További tudnivalók a programozási modellt hello hello hello Develop szakasza [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account


