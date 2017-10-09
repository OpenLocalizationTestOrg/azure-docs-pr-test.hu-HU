---
title: "ASP.NET MVC oktatóprogram az Azure Cosmos DB szolgáltatáshoz: webalkalmazás-fejlesztés | Microsoft Docs"
description: "ASP.NET MVC oktatóprogram toocreate egy MVC webalkalmazását az Azure Cosmos DB használatával. A JSON-fájlok tárolása és az adatok elérése az Azure-webhelyeken tárolt teendőkezelő alkalmazásból történik – ASP NET MVC oktatóprogram lépésről lépésre."
keywords: "asp.net mvc oktatóanyag, webalkalmazás fejlesztése, mvc-webalkalmazás, asp net mvc lépésről lépésre haladó oktatóanyag"
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: mimig
ms.openlocfilehash: dac2a9599b395524533e6fe14983789ff095331f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="_Toc395809351"></a>ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése az Azure Cosmos DB szolgáltatással
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

toohighlight hogyan hatékonyan kihasználja az Azure Cosmos DB toostore és lekérdezni a JSON-dokumentumok, a cikkben egy végpontok közötti körűen, bemutatja, hogyan toobuild teendőkezelő alkalmazást az Azure Cosmos DB. hello feladatok Azure Cosmos DB JSON-dokumentumokként tárolja.

![Képernyőfelvétel a hello teendőlista MVC webalkalmazás hozta létre az oktatóanyag – ASP NET MVC oktatóprogram lépésről lépésre](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image01.png)

Ez az útmutató bemutatja, hogyan toouse hello Azure Cosmos DB szolgáltatás toostore és hozzáférés az Azure rendszeren üzemeltetett ASP.NET MVC webalkalmazás adatokat. Ha egy oktatóanyag, amely csak Azure Cosmos DB keres, és nem hello ASP.NET MVC összetevőkkel, lásd: [egy Azure Cosmos DB C# Konzolalkalmazás létrehozása](documentdb-get-started.md).

> [!TIP]
> Ez az oktatóprogram feltételezi, hogy van korábbi tapasztalata az ASP.NET MVC és az Azure webhelyek használatában. Ha új tooASP.NET vagy hello [előfeltételt jelentő eszközöket](#_Toc395637760), érdemes letöltenie hello teljes mintaprojektet a [GitHub] [ GitHub] és hello utasításait követve Ez a minta. Ha felépítette, ezen cikk toogain insight hello kódja hello projekt környezetében hello tekintheti meg.
> 
> 

## <a name="_Toc395637760"></a>Az adatbázis-oktatóanyag előfeltételei
Ez a cikk hello utasításait követve, előtt győződjön meg, hogy rendelkezik-e hello következő:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 

    VAGY

    Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md).
* [A Visual Studio 2017](http://www.visualstudio.com/).  
* A Microsoft Azure SDK for .NET for Visual Studio 2017, a Visual Studio telepítő hello keresztül érhető el.

Ez a cikk összes hello képernyőképek használatával a Microsoft Visual Studio Community 2017 került sor. Ha a rendszer a lehetséges, hogy a képernyők és beállítások nem egyeznek tökéletesen, de ha megfelel a fenti előfeltételek hello ebben a megoldásban kell működnie egy másik verzió van konfigurálva.

## <a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása
Először hozzon létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik SQL (DocumentDB) fiókkal az Azure Cosmos DB vagy rendszer használata esetén ez az oktatóanyag hello Azure Cosmos DB emulátor, kihagyhatja túl[hozzon létre egy új ASP.NET MVC alkalmazást](#_Toc395637762).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

<br/>
Most végigvezetjük azon hogyan toocreate egy új ASP.NET MVC alkalmazás hello ground felfelé. 

## <a name="_Toc395637762"></a>2. lépés: Új ASP.NET MVC alkalmazás létrehozása

1. A Visual Studio, a hello **fájl** menüben mutasson túl**új**, és kattintson a **projekt**. Hello **új projekt** párbeszédpanel jelenik meg.

2. A hello **projekt típusok** ablaktáblában bontsa ki **sablonok**, **Visual C#**, **webes**, majd válassza ki **ASP.NET Webalkalmazásként való kezelése** .

      ![Képernyőfelvétel a hello új projekt párbeszédpanel a hello ASP.NET webalkalmazás projekttípus van kijelölve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. A hello **neve** mezőjébe hello projekt hello típusnév. Ez az oktatóanyag hello neve "todo" használja. Ha úgy dönt, toouse más nevet, majd bárhol ebben az oktatóanyagban beszél hello todo névtér, kell tooadjust megadott hello kód minták toouse függetlenül nevű az alkalmazás. 
4. Kattintson a **Tallózás** toonavigate toohello mappa hol kívánja toocreate hello projekt, majd kattintson a **OK**.
   
      Hello **új ASP.NET-webalkalmazás** párbeszédpanel jelenik meg.
   
    ![Képernyőfelvétel a hello MVC alkalmazássablon van kiemelve hello új ASP.NET Webalkalmazásként való kezelése párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-MVC.png)
5. Hello sablonok panelén válassza **MVC**.

6. Kattintson a **OK** és lehetővé teszik a Visual Studio kialakítsa állványok hello üres ASP.NET MVC sablonban. 

          
7. Visual Studio befejezte a hello bolierplate MVC alkalmazás létrehozása után a felhasználó egy üres ASP.NET alkalmazást, amelyet helyileg futtathat.
   
    Kihagyjuk a futó hello projekt helyileg, mert nem használom, azt is, hogy minden látott hello ASP.NET "Hello World" alkalmazást. Ugorjunk egyenes tooadding Azure Cosmos DB toothis projekt és az alkalmazás felépítésére.

## <a name="_Toc395637767"></a>3. lépés:, Adja hozzá Azure Cosmos DB tooyour MVC webalkalmazás projekthez
Most, hogy ehhez a megoldáshoz szükséges hello ASP.NET MVC bekötések nagy többségét, folytassuk toohello valódi céljával, amely ebben az oktatóanyagban Azure Cosmos DB tooour MVC webalkalmazás hozzáadása.

1. hello Azure Cosmos DB .NET SDK csomagolt és a NuGet-csomag terjesztése. tooget hello Visual Studio NuGet-csomagot, hello NuGet-Csomagkezelő használja a Visual Studióban kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** majd **NuGet-csomagok kezelése**.
   
    ![Képernyőfelvétel a hello gombbal hello webalkalmazás projekthez a Megoldáskezelőben, a Manage NuGet Packages kiemelt beállításait.](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
    Hello **NuGet-csomagok kezelése** párbeszédpanel jelenik meg.
2. A hello NuGet **Tallózás** mezőbe írja be ***Azure DocumentDB***. (hello csomag neve nem lett frissítve tooAzure Cosmos DB.)
   
    Hello eredmények közül telepítse a hello **Microsoft Microsoft.Azure.DocumentDB** csomag. Ez letölti és telepíti a hello Azure Cosmos DB csomagot, valamint az összes függőségét, például a newtonsoft.JSON elemet. Kattintson a **OK** hello a **előzetes** ablakot, és **elfogadom** hello a **licenc elfogadása** ablak toocomplete hello telepítése.
   
    ![Hello NuGet-csomagok kezelése ablakban, a Microsoft Azure DocumentDB Client Library elem van kiemelve hello képernyőfelvétele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-install-nuget.png)
   
      Másik lehetőségként hello Csomagkezelő konzol tooinstall hello csomagot is használhat. toodo Igen, a hello **eszközök** menüben kattintson a **NuGet-Csomagkezelő**, és kattintson a **Csomagkezelő konzol**. Hello parancssorba írja be a következő hello.
   
        Install-Package Microsoft.Azure.DocumentDB
        
3. Hello csomag telepítve van, a Visual Studio megoldás hello következő hivatkozásokkal két új hozzáadott hivatkozással: Microsoft.Azure.Documents.Client és Newtonsoft.Json kell hasonlítania.
   
    ![Hello két hivatkozás képernyőfelvétele toohello JSON adatprojekthez hozzáadva a Megoldáskezelőben](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-added-references.png)

## <a name="_Toc395637763"></a>4. lépés: Hello ASP.NET MVC alkalmazás beállítása
Most adjuk hozzá hello modellek, nézetekkel és vezérlőkkel toothis MVC alkalmazás:

* [Modell hozzáadása](#_Toc395637764).
* [Vezérlő hozzáadása](#_Toc395637765).
* [Nézetek hozzáadása](#_Toc395637766).

### <a name="_Toc395637764"></a>JSON adatmodell hozzáadása
Hozzuk létre hello **M** hello az mvc-ben, a modell. 

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **modellek** mappát, kattintson a **hozzáadása**, és kattintson a **osztály**.
   
      Hello **új elem hozzáadása** párbeszédpanel jelenik meg.
2. Adja az új osztálynak az **Item.cs** nevet, és kattintson az **Add** (Hozzáadás) gombra. 
3. Ebben az új **Item.cs** fájlt, adja hozzá a hello következő hello után utolsó *utasítás használatával*.
   
        using Newtonsoft.Json;
4. Most cserélje le ezt a kódot 
   
        public class Item
        {
        }
   
    a kód a következő hello.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Az Azure Cosmos Adatbázisba az összes adat hello hálózaton keresztül továbbított és JSON-ként tárolja. toocontrol hello módon az objektumok által használható JSON.NET szerializált/deszerializálása hello **JsonProperty** attribútumot, ahogyan az hello **elem** imént létrehozott osztályt. Nem **rendelkezik** toodo ez, de szeretné, hogy a tulajdonságaim követik hello JSON camelCase elnevezési konvenciói tooensure. 
   
    Nem csak vezérelheti hello hello tulajdonságnév formátumát a JSON-ba kerül, de teljesen át is nevezheti a .NET tulajdonságokat, mint ahogyan a hello **leírás** tulajdonság. 

### <a name="_Toc395637765"></a>Vezérlő hozzáadása
Amely gondoskodik hello **M**, most hozzuk létre az hello **C** az mvc-ben, a vezérlő osztályhoz.

1. A **Solution Explorer**, kattintson a jobb gombbal hello **tartományvezérlők** mappát, kattintson a **Hozzáadás**, és kattintson a **vezérlő**.
   
    Hello **hozzáadása Scaffold** párbeszédpanel jelenik meg.
2. Válassza az **MVC 5 Controller - Empty** (MVC 5 vezérlő - Üres) elemet, majd kattintson az **Add** (Hozzáadás) gombra.
   
    ![Képernyőfelvétel a hello MVC 5 vezérlő - üres beállítás kiemelt hello Scaffold hozzáadása párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)
3. Adja az **ItemController** nevet az új vezérlőnek.
   
    ![Képernyőfelvétel a hello vezérlő hozzáadása párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-controller.png)
   
    Hello-fájl létrehozása a Visual Studio megoldás kell hasonlítania hello következő hello új ItemController.cs fájllal a **Megoldáskezelőben**. hello korábban létrehozott új Item.cs fájl is látható.
   
    ![Képernyőfelvétel a hello Visual Studio megoldás - megoldáskezelő a hello új ItemController.cs és Item.cs fájl kiemelve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-new-item-solution-explorer.png)
   
    Bezárhatja az ItemController.cs, visszatérünk tooit később. 

### <a name="_Toc395637766"></a>Nézetek hozzáadása
Most hozzuk létre az hello **V** mvc, hello nézetek:

* [Elemindexnézet hozzáadása](#AddItemIndexView).
* [Új elemnézet hozzáadása](#AddNewIndexView).
* [Elemszerkesztési nézet hozzáadása](#_Toc395888515).

#### <a name="AddItemIndexView"></a>Elemindexnézet hozzáadása
1. A **Megoldáskezelőben**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal hello üres **elem** mappát, amely a Visual Studio létrehozza azt hello hozzáadásakor  **ItemController** korábbi, kattintson a **Hozzáadás**, és kattintson a **nézet**.
   
    ![Képernyőfelvétel a Megoldáskezelőben hello elem mappa, amely a Visual Studio létre hello nézet hozzáadása parancsok vannak kiemelve](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view.png)
2. A hello **nézet hozzáadása** párbeszédpanel mezőbe hello a következő:
   
   * A hello **nézetnév** mezőbe írja be ***Index***.
   * A hello **sablon** mezőben válassza ***lista***.
   * A hello **Model class** mezőben válassza ***elem (todo. Modellek)***.
   * Hello elrendezés lap mezőbe írja be ***~/Views/Shared/_Layout.cshtml***.
     
   ![Képernyőfelvétel a változásszinkronizálás ábrázoló hello nézet hozzáadása párbeszédpanel](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)
3. Amikor ezen értékek mindegyike már be van állítva, kattintson az **Add** (Hozzáadás) gombra és várja meg, hogy a Visual Studio létrehozzon egy új sablonnézetet. Ha ezzel végzett, akkor megnyílik hello létrehozott cshtml fájlt. Azt is zárja be a fájlt a Visual Studio, azt fogja térjen vissza tooit később.

#### <a name="AddNewIndexView"></a>Új elemnézet hozzáadása
Hasonló toohow létrehoztunk egy **Elemindex** nézet, most létrehozunk egy új nézetet új létrehozása **elemek**.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **elem** mappát, kattintson a **Hozzáadás**, és kattintson a **nézet**.
2. A hello **nézet hozzáadása** párbeszédpanel mezőbe hello a következő:
   
   * A hello **nézetnév** mezőbe írja be ***létrehozása***.
   * A hello **sablon** mezőben válassza ***létrehozása***.
   * A hello **Model class** mezőben válassza ***elem (todo. Modellek)***.
   * Hello elrendezés lap mezőbe írja be ***~/Views/Shared/_Layout.cshtml***.
   * Kattintson az **Add** (Hozzáadás) parancsra.
   
#### <a name="_Toc395888515"></a>Elemszerkesztési nézet hozzáadása
És végül adja hozzá egy utolsó nézetet szerkeszthető egy **elem** a hello ahogyan azt korábban.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **elem** mappát, kattintson a **Hozzáadás**, és kattintson a **nézet**.
2. A hello **nézet hozzáadása** párbeszédpanel mezőbe hello a következő:
   
   * A hello **nézetnév** mezőbe írja be ***szerkesztése***.
   * A hello **sablon** mezőben válassza ***szerkesztése***.
   * A hello **Model class** mezőben válassza ***elem (todo. Modellek)***.
   * Hello elrendezés lap mezőbe írja be ***~/Views/Shared/_Layout.cshtml***.
   * Kattintson az **Add** (Hozzáadás) parancsra.

Ha ezzel végzett, zárja be az összes hello cshtml dokumentumot a Visual Studio azt toothese nézetek később fog visszaadni.

## <a name="_Toc395637769"></a>5. lépés: Az Azure Cosmos DB csatlakoztatása
Most, hogy hello szabványos MVC elvégeztük a, adjuk tooadding hello kód Azure Cosmos DB. 

Ebben a szakaszban kód toohandle hello következő fel kell venni:

* [Hiányos elemek listázása](#_Toc395637770).
* [Elemek hozzáadása](#_Toc395637771).
* [Elemek szerkesztése](#_Toc395637772).

### <a name="_Toc395637770"></a>Hiányos elemek listázása az MVC webalkalmazásban
hello első lépésként toodo itt van vegyen fel egy osztály, amely tartalmazza az összes hello logika tooconnect tooand használata Azure Cosmos DB. Az oktatóanyag azt fogja foglalják magukban ezen logikák tooa adattár osztályt DocumentDBRepository nevű adattárba foglaljuk. 

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projekt, kattintson a **Hozzáadás**, és kattintson a **osztály**. Hello új osztály neve **DocumentDBRepository** kattintson **Hozzáadás**.
2. Az újonnan létrehozott hello **DocumentDBRepository** osztályhoz, és adja hozzá a következő hello *utasítások segítségével* fent hello *névtér* nyilatkozat
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
        using System.Net
        
    Most cserélje le ezt a kódot 
   
        public class DocumentDBRepository
        {
        }
   
    a kód a következő hello.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
    
3. Konfigurációból adunk néhány érték, ezért nyissa meg a hello **Web.config** fájlt az alkalmazás, és adja hozzá a következő sorokat a hello hello `<AppSettings>` szakasz.
   
        <add key="endpoint" value="enter hello URI from hello Keys blade of hello Azure Portal"/>
        <add key="authKey" value="enter hello PRIMARY KEY, or hello SECONDARY KEY, from hello Keys blade of hello Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Most frissítse hello értékeinek *végpont* és *authKey* hello (kulcsok) panelén hello Azure portál használatával. Hello használata **URI** hello kulcsok paneljén hello végpont beállítás, és használjon hello hello értékeként **elsődleges kulcs**, vagy **másodlagos kulcs** hello kulcsok paneljén hello hello értéke authKey beállítás értékeként.

    Hogy vesz gondot elvégeztük hello Azure Cosmos DB tárház, most adjuk hozzá az alkalmazás logikáját.

1. hello először thing szeretnénk toobe képes toodo rendelkező a teendőlista alkalmazásában toodisplay hello hiányos elemeket.  Másolja és illessze be a következő kódrészletet bárhová belül hello hello **DocumentDBRepository** osztály.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Nyissa meg hello **ItemController** azt korábban hozzáadott, és adja hozzá a következő hello *utasítások segítségével* hello névtér-deklaráció fent.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Ha a projekt neve nem "todo", akkor újra kell tooupdate használja a "todo. Modellek"; a projekt tooreflect hello nevét.
   
    Most cserélje le ezt a kódot
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    a kód a következő hello.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. Nyissa meg **Global.asax.cs** , és adja hozzá a következő sor toohello hello **Application_Start** módszer 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

Ezen a ponton a megoldás képes toobuild hiba nélkül kell lennie.

Ha hello alkalmazás most már futott, kerülne toohello **HomeController** és hello **Index** nézetébe kerülne. Ez a hello hello hello start választott MVC sablonprojekt alapértelmezett viselkedése, de mi nem ezt szeretnénk! Módosítsuk hello az Útválasztás a MVC alkalmazás tooalter ezt a viselkedést.

Nyissa meg ***App\_Start\RouteConfig.cs*** , és keresse meg a hello kezdetű sort "alapértelmezett értéke:" módosítsa azt a következő tooresemble hello.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

Ez most közli az ASP.NET MVC, ha a nem megadott érték hello URL-cím toocontrol hello helyette, amely az útválasztási viselkedés **Home**, használja **elem** hello, tartományvezérlői és felhasználói **Index** hello nézetként.

Most hello alkalmazás futtatásakor, azt fogja belülre hívni a **ItemController** amely toohello adattár osztályt hívja és hello GetItems metódus tooreturn használja minden hello hiányos elemek toohello **nézetek** \\ **Elem**\\**Index** nézet. 

Ha most felépíti és futtatja ezt a projektet, valami ilyesmit kell látnia.    

![Képernyőfelvétel a hello teendőlista webalkalmazás adatbázis-oktatóprogram során létrehozott](./media/documentdb-dotnet-application/build-and-run-the-project-now.png)

### <a name="_Toc395637771"></a>Elemek hozzáadása
Tegyünk néhány elemet az adatbázisba, hogy ne több mint egy üres táblát toolook címen.

Adjunk néhány kódot túl Azure Cosmos DBRepository és ItemController toopersist hello rekordjának Azure Cosmos DB.

1. Adja hozzá a következő metódus tooyour hello **DocumentDBRepository** osztály.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Ez a metódus egyszerűen vesz igénybe egy tooit átadott objektum, és továbbra is fennáll az Azure Cosmos Adatbázisba.
2. Nyissa meg a hello ItemController.cs fájlt, és adja hozzá a következő kódrészletet hello osztályon belül hello. Ez az ASP.NET MVC így tudja a hello milyen toodo **létrehozása** művelet. Ebben az esetben csak leképezési hello társított Create.cshtml nézetet a korábban létrehozott.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Most több kódra ebben a vezérlőben, amely elfogadja a hello hello küldésének kell **létrehozása** nézet.
3. Adja hozzá a következő kódblokkot kód toohello ItemController.cs osztályhoz, amely közli az ASP.NET MVC és az ezen vezérlőből POST űrlapművelettel milyen toodo hello.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Ez a kód a toohello DocumentDBRepository hívja, és hello CreateItemAsync metódus toopersist hello új teendő elem toohello adatbázist használ. 
   
    **Biztonsági megjegyzés**: hello **ValidateAntiForgeryToken** attribútum itt a toohelp az alkalmazást a webhelyközi kérések hamisításának megakadályozása támadások elleni védelméhez. Ez az attribútum hozzáadásánál többről további tooit, a nézetek a hamisítás elleni tokennel rendelkező toowork kell. További hello tulajdonos, és hogyan tooimplement ennek megfelelően, ellenőrizze a példák [megakadályozza a Webhelyközi kérések hamisításának megakadályozása][Preventing Cross-Site Request Forgery]. közzétett forráskódban hello [GitHub] [ GitHub] hello teljes körű rendelkezik.
   
    **Biztonsági megjegyzés**: hello is használunk **kötési** hello metódus paraméter toohelp attribútum túlküldéses támadások elleni védelem érdekében. További részletekért lásd: [Alapvető CRUD műveletek az ASP.NET MVC-ben][Basic CRUD Operations in ASP.NET MVC].

Ennyi lenne hello szükséges kód tooadd új elemek tooour adatbázis.

### <a name="_Toc395637772"></a>Elemek szerkesztése
Egyik utolsó teendő azon toodo, és ez tooadd hello képességét tooedit **elemek** hello adatbázisban és toomark őket, végezze el. hello szerkesztésre szolgáló nézet már fel van véve toohello projekt, ezért azt kell tooadd néhány kód tooour vezérlő és toohello **DocumentDBRepository** osztályból.

1. Adja hozzá a következő toohello hello **DocumentDBRepository** osztály.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    Ezek a módszerek közül első hello **GetItem** egy elemet kér le a Azure Cosmos-Adatbázisból, amelyet vissza toohello **ItemController** majd a toohello **szerkesztése** nézet.
   
    második hello módszerek hello épp most lett felvéve cserél hello **dokumentum** az Azure Cosmos Adatbázisba hello hello verziójával **dokumentum** hello átadott **ItemController**.
2. Adja hozzá a következő toohello hello **ItemController** osztály.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    hello első módszer leírók hello Http GET, amely történik, amikor hello felhasználó kattint az hello **szerkesztése** hello mutató hivatkozást **Index** nézet. Ez a módszer lekéri a [ **dokumentum** ](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) a Azure Cosmos-Adatbázisból, és átadja toohello **szerkesztése** nézet.
   
    Hello **szerkesztése** nézet tegye egy Http POST toohello **IndexController**. 
   
    hello második sikeres frissítése hello objektum tooAzure Cosmos DB toobe leírók hozzáadott metódus hello adatbázisban maradnak.

Ennyi, amire szükségünk van toorun az alkalmazás által, a lista nem teljes **elemek**, hozzáadhat új **elemek**, és szerkesztheti a **elemek**.

## <a name="_Toc395637773"></a>6. lépés: Hello alkalmazás helyileg történő futtatása
a helyi gépén, tootest hello alkalmazás hello a következő:

1. Kattintson az F5 billentyűt a Visual Studio toobuild hello alkalmazás hibakeresési módban. Ez a kell hello alkalmazás létrehozása, és elindít egy böngészőt, a hello üres rácsoldallal korábban látott:
   
    ![Képernyőfelvétel a hello teendőlista webalkalmazás adatbázis-oktatóprogram során létrehozott](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
   
     
2. Kattintson a hello **hozzon létre új** hivatkozásra, és adjon értékeket toohello **neve** és **leírás** mezők. Hagyja hello **befejezve** jelölőnégyzet nincs bejelölve egyéb hello új **elem** befejezett állapotban megjelenik, és nem jelenik meg a kiindulási lista hello.
   
    ![Képernyőfelvétel a hello nézet létrehozása](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-new-item.png)
3. Kattintson a **létrehozása** és átirányított hátsó toohello **Index** megtekintése és a **elem** hello listájában jelenik meg.
   
    ![Képernyőfelvétel a hello Index nézetről](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
   
    Érzi, hogy a szabad tooadd még néhány **elemek** tooyour teendőlistában.
    
4. Kattintson a **szerkesztése** következő tooan **elem** hello listán, és készít a toohello **szerkesztése** nézet, ahol frissítheti az objektum, például hello bármely tulajdonságát  **Befejeződött** jelzőt. Ha meg van jelölve, akkor a hello **Complete** tulajdonsággal, és kattintson a **mentése**, hello **elem** eltávolítják az hello a hiányos feladatok listájából.
   
    ![Képernyőfelvétel a hello hello befejezve mezőben be van jelölve az Index nézetről](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-completed-item.png)
5. Miután hello alkalmazás tesztelését, nyomja le az Ctrl + F5 toostop hello app hibakeresés. Most készen áll a toodeploy!

## <a name="_Toc395637774"></a>7. lépés: Hello alkalmazás tooAzure App Service telepítése 
Most, hogy hello teljes alkalmazás megfelelően működik-e az Azure Cosmos DB programot fogjuk toodeploy a webes alkalmazás tooAzure App Service.  

1. toopublish az alkalmazás összes toodo szüksége, kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** kattintson **közzététel**.
   
    ![Képernyőfelvétel a hello közzététel lehetőségről a Megoldáskezelőben](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish.png)

2. A hello **közzététel** párbeszédpanelen kattintson **Microsoft Azure App Service**, majd válassza **hozzon létre új** toocreate egy App Service profilt, vagy kattintson a **kiválasztása Meglévő** toouse egy meglévő profilt.

    ![A Visual Studio párbeszédpanel közzététele](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-publish-to-existing.png)

3. Ha egy meglévő Azure App Service-profilt, adja meg az előfizetés nevét. Használjon hello **nézet** toosort erőforráscsoport és erőforrások típus szerint szűrheti, majd válassza ki az Azure App Service. 
   
    ![A Visual Studio App Service párbeszédpanelen](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. toocreate egy új Azure App Service-profilt, kattintson a **hozzon létre új** a hello **közzététel** párbeszédpanel megnyitásához. A hello **létrehozása az App Service** párbeszédpanelen adja meg a webes alkalmazás neve és a megfelelő előfizetés, az erőforráscsoport és az App Service-csomag, majd kattintson a **létrehozása**.

    ![App Service párbeszédpanelen létrehozása a Visual Studióban](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Néhány másodpercen belül a Visual Studio befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!



## <a name="_Toc395637775"></a>Következő lépések
Gratulálunk! Ebben az esetben az első ASP.NET MVC webalkalmazását az Azure Cosmos DB használatával építve, és közzétette azt tooAzure. hello hello teljes alkalmazás, beleértve a hello részletes forráskódja és törlési szolgáltatást, ez nem foglalt oktatóanyag is letölthető vagy klónozható a [GitHub][GitHub]. Ezért ha tooyour alkalmazást érdekli, adása hello kódot, és toothis alkalmazás hozzáadása.

tooadd további funkciók tooyour alkalmazás, tekintse át hello hello elérhető API-k [Azure Cosmos .NET kódtárban](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) és érzi, hogy a szabad toocontribute toohello Azure Cosmos .NET kódtárban a [GitHub] [GitHub]. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app
