---
title: "az Azure-ban az ASP.NET és az SQL-adatbázis egy REST API aaaCreate |} Microsoft Docs"
description: "Egy oktatóanyag, amely útmutatást ad meg hogyan az alkalmazások által használt toodeploy hello Visual Studio használatával ASP.NET Web API tooan Azure-webalkalmazásban."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: erikre
editor: 
ms.assetid: f4916fc0-ea08-41f7-846b-73e41bc88149
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: riande
ms.openlocfilehash: 1ef45dd1582bfda367e53c39f863164422ad678b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a>Hozzon létre egy REST-szolgáltatást az ASP.NET Web API és az SQL-adatbázis használata az Azure App Service-ben
Ez az oktatóanyag bemutatja, hogyan toodeploy az ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a Visual Studio 2013 vagy Visual Studio 2013 Community Edition hello webhely közzététele varázsló segítségével. 

Az Azure-fiók szabad megnyithatja, és ha még nem rendelkezik a Visual Studio 2013, hello SDK automatikusan telepíti a Visual Studio 2013 Express webes. Ezért nekiláthat teljes mértékben az Azure ingyenes.

Ez az oktatóanyag feltételezi, hogy rendelkezik-e nincs előzetes tapasztalata az Azure használatával kapcsolatban. Az oktatóanyag elvégzését be egy egyszerű webalkalmazást és hello felhőben futó kell.

Az oktatóanyagból a következőket sajátíthatja el:

* Hogyan tooenable Azure fejlesztési telepítésével, a gép hello Azure SDK-t.
* Hogyan toocreate a Visual Studio ASP.NET MVC 5 projektre, és tegye közzé az Azure app tooan.
* Hogyan toouse hello ASP.NET Web API tooenable Restful API hívásokat.
* Hogyan toouse egy SQL-adatbázis használati ideje toostore adatok Azure-ban.
* Hogy hogyan toopublish alkalmazás tooAzure frissíti.

Akkor lesz ASP.NET MVC 5 épülő egyszerű ismerősök listájának webalkalmazás létrehozása a, és használja az ADO.NET Entity Framework hello az adatbázis eléréséhez. a következő ábra azt mutatja be hello hello kérelem befejeződött:

![Képernyőkép a webhely][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-hello-project"></a>Hello projekt létrehozása
1. Indítsa el a Visual Studio 2013.
2. A hello **fájl** menüben kattintson **új projekt**.
3. A hello **új projekt** párbeszédpanelen bontsa ki **Visual C#** válassza **webes** , és válassza **ASP.NET Web Application**. Hello alkalmazás neve **ContactManager** kattintson **OK**.
   
    ![A New Project (Új projekt) párbeszédpanel](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. A hello **új ASP.NET projekt** párbeszédpanel megnyitásához, jelölje be hello **MVC** sablon ellenőrzése **Web API** , majd **hitelesítés módosítása**.
5. A hello **hitelesítés módosítása** párbeszédpanel, kattintson a **nem hitelesítési**, és kattintson a **OK**.
   
    ![Nincs hitelesítés](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    hello mintaalkalmazás hoz létre, nem kell a felhasználók toolog igénylő szolgáltatások. További információ tooimplement hitelesítési és engedélyezési szolgáltatásokat, lásd: hello [lépések](#nextsteps) szakasz hello Ez az oktatóanyag végén. 
6. A hello **új ASP.NET projekt** párbeszédpanelen győződjön meg arról, hogy hello **gazdagépet a felhő hello** be van jelölve, és kattintson a **OK**.

Ha korábban már nincs bejelentkezve tooAzure, a kért toosign fogja.

1. hello konfigurációs varázsló egy egyedi nevet a alapján javasol *ContactManager* (lásd az alábbi képen hello). Válasszon egy régiót a környéken. Használhat [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello legalacsonyabb késés adatközpont. 
2. Ha egy adatbázis-kiszolgálót, mielőtt még nem hozott létre, jelölje be **létrehozása új kiszolgáló**, adjon meg egy adatbázis-felhasználónevet és jelszót.
   
    ![Azure-webhely konfigurálása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

Ha egy adatbázis-kiszolgálót, használja az adott toocreate egy új adatbázist. Adatbázis-kiszolgálók értékes erőforrás, és általában kívánt toocreate hello több adatbázist ugyanarra a kiszolgálóra a tesztelés és fejlesztési létrehozása helyett adatbázis egy adatbázis-kiszolgálókhoz. Gondoskodjon arról, hogy a webhely, az adatbázis hello ugyanabban a régióban.

![Azure-webhely konfigurálása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-hello-page-header-and-footer"></a>Hello oldal fejlécében és láblécében beállítása
1. A **Megoldáskezelőben**, bontsa ki a hello *Views\Shared* mappa és a nyitott hello *_Layout.cshtml* fájlt.
   
    ![A Solution Explorer _Layout.cshtml][newapp004]
2. Cserélje le a hello hello tartalmát *Views\Shared_Layout.cshtml* hello kód a következő fájlt:

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <title>@ViewBag.Title - Contact Manager</title>
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
            <meta name="viewport" content="width=device-width" />
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
        </head>
        <body>
            <header>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p class="site-title">@Html.ActionLink("Contact Manager", "Index", "Home")</p>
                    </div>
                </div>
            </header>
            <div id="body">
                @RenderSection("featured", required: false)
                <section class="content-wrapper main-content clear-fix">
                    @RenderBody()
                </section>
            </div>
            <footer>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p>&copy; @DateTime.Now.Year - Contact Manager</p>
                    </div>
                </div>
            </footer>
            @Scripts.Render("~/bundles/jquery")
            @RenderSection("scripts", required: false)
        </body>
        </html>

hello markup fenti módosítások hello alkalmazás neve a "Saját ASP.NET-alkalmazás" túl Manager "ügyfél", és eltávolítja hello hivatkozások túl**Home**, **kapcsolatos** és **forduljon**.

### <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása
1. Nyomja le a CTRL + F5 toorun hello alkalmazás.
   hello alkalmazás kezdőlapját hello alapértelmezett böngésző jelenik meg.
    ![tooDo lista kezdőlap](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

Ez az összes toodo most toocreate hello alkalmazás tooAzure fogja telepíteni kell. Később fogja hozzáadni adatbázisokkal kapcsolatos funkciók.

## <a name="deploy-hello-application-tooazure"></a>Hello alkalmazás tooAzure telepítése
1. A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** válassza **közzététel** hello helyi menüből.
   
    ![Tegye közzé ezt a projekt helyi menü][PublishVSSolution]
   
    Hello **webhely közzététele** varázsló megnyílik.
2. Kattintson a **Publish** (Közzététel) gombra.

![Beállítások lap](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

A Visual Studio megkezdi hello eljárást másolásának hello fájlok toohello Azure-kiszolgálóval. Hello **kimeneti** ablak mutatja, milyen telepítési műveletek vették és jelentések hello központi telepítés sikeres befejezését.

1. hello alapértelmezett böngésző automatikusan megnyitja a toohello telepített hello webhely URL-CÍMÉT.
   
   hello létrehozott alkalmazás innentől kezdve fut hello felhő.
   
   ![Azure-beli tooDo lista kezdőlap][rxz2]

## <a name="add-a-database-toohello-application"></a>Adatbázis toohello alkalmazás felvétele
A következő lesz frissíti hello MVC alkalmazás tooadd hello képességét toodisplay és ügyfelek frissítése és hello adat tárolása egy adatbázis. hello alkalmazás hello Entity Framework toocreate hello adatbázison és tooread, és frissíti hello adatbázis adatait.

### <a name="add-data-model-classes-for-hello-contacts"></a>Vegye fel a modell adatosztályokat hello kapcsolattartók
Hozzon létre egy egyszerű adatmodell kódban megkezdése.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello Models mappát, kattintson a **hozzáadása**, majd **osztály**.
   
    ![Modellek mappa helyi menü osztály hozzáadása][adddb001]
2. A hello **új elem hozzáadása** párbeszédpanelen osztály fájl új nevét hello *Contact.cs*, és kattintson a **Hozzáadás**.
   
    ![Adja hozzá az új elem párbeszédpanel][adddb002]
3. Cserélje le a következő kód hello hello hello Contacts.cs fájl tartalmát.
   
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
               {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                public string Email { get; set; }
                public string Twitter { get; set; }
                public string Self
                {
                    get { return string.Format(CultureInfo.CurrentCulture,
                         "api/contacts/{0}", this.ContactId); }
                    set { }
                }
            }
        }

Hello **forduljon** osztály hello adatok, amelyek az egyes partnerek, valamint az elsődleges kulcs, ContactID hello adatbázis által igényelt tárolja azt határozza meg. Adatmodellekről további információt kaphat a hello [lépések](#nextsteps) szakasz hello Ez az oktatóanyag végén.

### <a name="create-web-pages-that-enable-app-users-toowork-with-hello-contacts"></a>Hozzon létre, amelyek lehetővé teszik az alkalmazás felhasználók toowork hello ügyfelekkel weblapok
hello ASP.NET MVC hello állványok funkció automatikusan elő tud állítani megvalósító kódot létrehozása, olvasása, frissítése és Törlés (CRUD) műveleteket.

## <a name="add-a-controller-and-a-view-for-hello-data"></a>A vezérlő és egy nézet hello adatok hozzáadása
1. A **Megoldáskezelőben**, hello tartományvezérlők mappát.
2. Hello projekt felépítéséhez **(Ctrl + Shift + B)**. (Kell létrehozni hello projekt állványok mechanizmus használata előtt.) 
3. Kattintson a jobb gombbal a hello, tartományvezérlői mappa, és kattintson a **Hozzáadás**, és kattintson a **vezérlő**.
   
    ![Vezérlő hozzáadása a tartományvezérlők mappa helyi menü][addcode001]
4. A hello **hozzáadása Scaffold** párbeszédpanelen jelölje ki **MVC-vezérlő nézetekkel, entitás-keretrendszer használatával** kattintson **Hozzáadás**.
   
   ![Vezérlő hozzáadása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. Állítsa be a hello tartományvezérlő neve túl**HomeController**. Válassza ki **forduljon** , a modell osztály. Kattintson a hello **új adatkörnyezetet** gombra, és fogadja el a hello alapértelmezett "ContactManager.Models.ContactManagerContext" hello **új adatkörnyezet-típusának**. Kattintson az **Add** (Hozzáadás) parancsra.

    A párbeszédpanel figyelmezteti: "hello nevű fájl HomeController már létezik. Szeretné, hogy tooreplace azt? ". Kattintson a **Yes** (Igen) gombra. Hello kezdőlap hello új projekt hoztak létre vezérlő másolattal azt. Használjuk hello új kezdőlap vezérlő számára a ismerőslistájába.

    A Visual Studio létrehozza a vezérlő metódusokhoz és az adatbázis-műveleteknél CRUD nézetek **forduljon** objektumok.

## <a name="enable-migrations-create-hello-database-add-sample-data-and-a-data-initializer"></a>Engedélyezze az áttelepítést, hello adatbázis létrehozása, vegye fel a mintaadatok és egy adatok inicializáló
hello következő feladata tooenable hello [Code First áttelepítést](http://curah.microsoft.com/55220) rendelés toocreate hello adatbázis funkció alapján létrehozott hello adatmodell.

1. A hello **eszközök** menü **Kódtárcsomag-kezelő** , majd **Csomagkezelő konzol**.
   
    ![Az Eszközök menü Csomagkezelő konzol][addcode008]
2. A hello **Csomagkezelő konzol** ablak, írja be a következő parancs hello:
   
        enable-migrations 
   
    Hello **enable-áttelepítések** parancs létrehoz egy *áttelepítések* abban a mappában helyezi a mappára és egy *Configuration.cs* fájlt, hogy tooconfigure áttelepítések szerkesztheti. 
3. A hello **Csomagkezelő konzol** ablak, írja be a következő parancs hello:
   
        add-migration Initial
   
    Hello **hozzáadása áttelepítés kezdeti** parancs létrehoz egy osztályt  **&lt;date_stamp&gt;kezdeti** hello adatbázist hoz létre. az első paraméter hello ( *kezdeti* ) tetszőleges és használt toocreate hello hello fájl neve. Megtekintheti a hello új osztály fájlokat **Megoldáskezelőben**.
   
    A hello **kezdeti** osztály, hello **mentése** hello kapcsolatok táblához és hello hoz létre **le** módszerét (Ha azt szeretné, hogy tooreturn toohello korábbi állapotába) esik az.
4. Nyissa meg hello *Migrations\Configuration.cs* fájlt. 
5. Adja hozzá a következő névterek hello. 
   
         using ContactManager.Models;
6. Cserélje le a hello *kezdőérték* hello kód a következő metódust:
   
        protected override void Seed(ContactManager.Models.ContactManagerContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
                   Twitter = "debra_example"
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                    Twitter = "thorsten_example"
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                    Twitter = "yuhong_example"
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                    Twitter = "jon_example"
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                    Twitter = "diliana_example"
                }
                );
        }
   
    A fenti kódot inicializálja hello adatbázis hello kapcsolattartási adatait. A beültetés hello adatbázis további információkért lásd: [hibakeresés entitás-keretrendszer (EF) adatbázisok](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).
7. A hello **Csomagkezelő konzol** hello parancsot írja be:
   
        update-database
   
    ![A Package Manager Console parancsok][addcode009]
   
    Hello **adatbázis-frissítés** futtatása hello első áttelepítésről, amely hello adatbázist hoz létre. Alapértelmezés szerint hello adatbázis egy SQL Server Express LocalDB adatbázisban jön létre.
8. Nyomja le a CTRL + F5 toorun hello alkalmazás. 

hello alkalmazás hello adatait mutatja be, Szerkesztés, adatokat és delete hivatkozásokat.

![Az adatok MVC megtekintése][rxz3]

## <a name="edit-hello-view"></a>Hello nézet szerkesztése
1. Nyissa meg hello *Views\Home\Index.cshtml* fájlt. Hello a következő lépésben azt lecseréli generált hello markup kód [jQuery](http://jquery.com/) és [Knockout.js](http://knockoutjs.com/). Az új kódot hello listájához lekéri a webes API-val, és a JSON és kötések hello forduljon adatok toohello felhasználói felület knockout.js használatával. További információkért lásd: hello [lépések](#nextsteps) szakasz hello Ez az oktatóanyag végén. 
2. Cserélje le a következő kód hello hello hello fájl tartalmát.
   
        @model IEnumerable<ContactManager.Models.Contact>
        @{
            ViewBag.Title = "Home";
        }
        @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                function ContactsViewModel() {
                    var self = this;
                    self.contacts = ko.observableArray([]);
                    self.addContact = function () {
                        $.post("api/contacts",
                            $("#addContact").serialize(),
                            function (value) {
                                self.contacts.push(value);
                            },
                            "json");
                    }
                    self.removeContact = function (contact) {
                        $.ajax({
                            type: "DELETE",
                            url: contact.Self,
                            success: function () {
                                self.contacts.remove(contact);
                            }
                        });
                    }
   
                    $.getJSON("api/contacts", function (data) {
                        self.contacts(data);
                    });
                }
                ko.applyBindings(new ContactsViewModel());    
        </script>
        }
        <ul id="contacts" data-bind="foreach: contacts">
            <li class="ui-widget-content ui-corner-all">
                <h1 data-bind="text: Name" class="ui-widget-header"></h1>
                <div><span data-bind="text: $data.Address || 'Address?'"></span></div>
                <div>
                    <span data-bind="text: $data.City || 'City?'"></span>,
                    <span data-bind="text: $data.State || 'State?'"></span>
                    <span data-bind="text: $data.Zip || 'Zip?'"></span>
                </div>
                <div data-bind="if: $data.Email"><a data-bind="attr: { href: 'mailto:' + Email }, text: Email"></a></div>
                <div data-bind="ifnot: $data.Email"><span>Email?</span></div>
                <div data-bind="if: $data.Twitter"><a data-bind="attr: { href: 'http://twitter.com/' + Twitter }, text: '@@' + Twitter"></a></div>
                <div data-bind="ifnot: $data.Twitter"><span>Twitter?</span></div>
                <p><a data-bind="attr: { href: Self }, click: $root.removeContact" class="removeContact ui-state-default ui-corner-all">Remove</a></p>
            </li>
        </ul>
        <form id="addContact" data-bind="submit: addContact">
            <fieldset>
                <legend>Add New Contact</legend>
                <ol>
                    <li>
                        <label for="Name">Name</label>
                        <input type="text" name="Name" />
                    </li>
                    <li>
                        <label for="Address">Address</label>
                        <input type="text" name="Address" >
                    </li>
                    <li>
                        <label for="City">City</label>
                        <input type="text" name="City" />
                    </li>
                    <li>
                        <label for="State">State</label>
                        <input type="text" name="State" />
                    </li>
                    <li>
                        <label for="Zip">Zip</label>
                        <input type="text" name="Zip" />
                    </li>
                    <li>
                        <label for="Email">E-mail</label>
                        <input type="text" name="Email" />
                    </li>
                    <li>
                        <label for="Twitter">Twitter</label>
                        <input type="text" name="Twitter" />
                    </li>
                </ol>
                <input type="submit" value="Add" />
            </fieldset>
        </form>
3. Kattintson a jobb gombbal a hello tartalmat tartalmazó mappa, és kattintson a **Hozzáadás**, és kattintson a **új elem...** .
   
    ![Adja hozzá a stíluslap tartalmat tartalmazó mappa helyi menü][addcode005]
4. A hello **új elem hozzáadása** párbeszédpanelen adja meg a **stílus** a hello jobb felső keresési mezőbe, és válassza ki **stíluslap**.
    ![Adja hozzá az új elem párbeszédpanel][rxStyle]
5. Nevű hello fájl *Contacts.css* kattintson **Hozzáadás**. Cserélje le a következő kód hello hello hello fájl tartalmát.
   
        .column {
            float: left;
            width: 50%;
            padding: 0;
            margin: 5px 0;
        }
        form ol {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        form li {
            padding: 1px;
            margin: 3px;
        }
        form input[type="text"] {
            width: 100%;
        }
        #addContact {
            width: 300px;
            float: left;
            width:30%;
        }
        #contacts {
            list-style-type: none;
            margin: 0;
            padding: 0;
            float:left;
            width: 70%;
        }
        #contacts li {
            margin: 3px 3px 3px 0;
            padding: 1px;
            float: left;
            width: 300px;
            text-align: center;
            background-image: none;
            background-color: #F5F5F5;
        }
        #contacts li h1
        {
            padding: 0;
            margin: 0;
            background-image: none;
            background-color: Orange;
            color: White;
            font-family: Trebuchet MS, Tahoma, Verdana, Arial, sans-serif;
        }
        .removeContact, .viewImage
        {
            padding: 3px;
            text-decoration: none;
        }
   
    A stíluslap hello elrendezés, színeit és hello kapcsolattartási manager alkalmazásban használt stílusok használjuk.
6. Nyissa meg hello *App_Start\BundleConfig.cs* fájlt.
7. Adja hozzá a következő kód tooregister hello hello [kiejtés](http://knockoutjs.com/index.html "KO") beépülő modul.
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    Ez a minta kiejtés toosimplify dinamikus JavaScript-kódot, amely kezeli a hello képernyő sablonok használatával.
8. Hello tartalma/css bejegyzés tooregister hello módosítása *contacts.css* stíluslap. A következő sor hello módosítása:
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   :
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. Hello Csomagkezelő konzol futtassa a következő parancs tooinstall kiejtés hello.
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-hello-web-api-restful-interface"></a>Hello webes API Restful interfész vezérlő hozzáadása
1. A **Megoldáskezelőben**, kattintson a jobb gombbal a tartományvezérlők, és kattintson a **Hozzáadás** , majd **vezérlő...** 
2. A hello **hozzáadása Scaffold** párbeszédpanelen adja meg a **Web API 2-es vezérlőhöz műveletekhez, entitás-keretrendszer használatával** , majd **hozzáadása**.
   
    ![API-vezérlő hozzáadása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. A hello **vezérlő hozzáadása** párbeszédpanelen adja meg a "ContactsController" a tartományvezérlő neveként. Válassza ki a "Ügyfelet (ContactManager.Models)" a hello **Model class**.  Tartsa hello alapértelmezett értéke hello **adatok környezetben osztály**. 
4. Kattintson az **Add** (Hozzáadás) parancsra.

### <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása
1. Nyomja le a CTRL + F5 toorun hello alkalmazás.
   
    ![Index lap][intro001]
2. Adja meg a partner, és kattintson a **Hozzáadás**. hello alkalmazás toohello kezdőlap adja vissza, és megjeleníti a megadott hello forduljon.
   
    ![A lista Tennivaló index lap][addwebapi004]
3. Hello böngészőben hozzáfűzése **/api/contacts** toohello URL-CÍMÉT.
   
    hello eredményül kapott URL-címet fog hasonlítani http://localhost:1234/api/Contacts címen. hello RESTful webes API, hozzáadott hello tárolt névjegyek adja vissza. A Firefox és Chrome jelenít meg hello adatot XML formátumban.
   
    ![A lista Tennivaló index lap][rxFFchrome]

    Internet Explorer tooopen kéri, vagy a hello partnerek mentése.

    ![Webes API-k mentése párbeszédpanel][addwebapi006]


    Adott vissza a névjegyeket a Jegyzettömbben vagy a böngésző hello nyithatja meg.

    A kimeneti például mobil weblap vagy az alkalmazás egy másik alkalmazás képes használni.

    ![Webes API-k mentése párbeszédpanel][addwebapi007]

    **Biztonsági figyelmeztetés**: ezen a ponton az alkalmazás nem biztonságos és sebezhető tooCSRF támadás. Hello oktatóanyag későbbi részében megszüntetjük a biztonsági rés. További információ: [akadályozza meg, hogy többhelyes kérelem hamisítására (CSRF) támadások][prevent-csrf-attacks].
## <a name="add-xsrf-protection"></a>XSRF védelem hozzáadása
Webhelyközi kérések hamisítására (más néven XSRF vagy CSRF) webes állomásokon tárolt alkalmazásokhoz, amelyek rosszindulatú webhely befolyásolhatja hello közötti interakció ügyfél böngészője és egy webhelyet, a böngésző által megbízhatónak elleni támadásoknak. Ilyen támadások lehetséges válnak, mert a böngésző fog küldeni a hitelesítési tokenek automatikusan minden kérelem tooa webhellyel. hello kanonikus példa, hogy egy hitelesítési cookie-t, például az ASP. NET a az űrlapos hitelesítés jegy. Azonban bármely állandó hitelesítési mechanizmus (például Windows-hitelesítést, a Basic, és így tovább) használó webhelyek célpontja is lehet ilyen támadások.

XSRF támadás nem egyezik a adathalász támadások. Adathalász támadások közreműködése a hello áldozata. Az egy adathalász támadások rosszindulatú webhely lesz utánozzák hello célwebhely, és hello áldozata magát van becsapni a bizalmas adatokat toohello támadó biztosítása. Az XSRF támadások nincs gyakran párbeszéd hello áldozata a szükséges. Ehelyett hello támadó hello böngésző automatikusan küldése az összes releváns cookie-k toohello cél webhely megbízható függő.

További információkért lásd: hello [biztonsági projekt megnyitása webes alkalmazás](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).

1. A **Megoldáskezelőben**, jobb **ContactManager** projektre, kattintson **hozzáadása** majd **osztály**.
2. Nevű hello fájl *ValidateHttpAntiForgeryTokenAttribute.cs* , és adja hozzá a következő kód hello:
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Helpers;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using System.Web.Mvc;
        namespace ContactManager.Filters
        {
            public class ValidateHttpAntiForgeryTokenAttribute : AuthorizationFilterAttribute
            {
                public override void OnAuthorization(HttpActionContext actionContext)
                {
                    HttpRequestMessage request = actionContext.ControllerContext.Request;
                    try
                    {
                        if (IsAjaxRequest(request))
                        {
                            ValidateRequestHeader(request);
                        }
                        else
                        {
                            AntiForgery.Validate();
                        }
                    }
                    catch (HttpAntiForgeryException e)
                    {
                        actionContext.Response = request.CreateErrorResponse(HttpStatusCode.Forbidden, e);
                    }
                }
                private bool IsAjaxRequest(HttpRequestMessage request)
                {
                    IEnumerable<string> xRequestedWithHeaders;
                    if (request.Headers.TryGetValues("X-Requested-With", out xRequestedWithHeaders))
                    {
                        string headerValue = xRequestedWithHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(headerValue))
                        {
                            return String.Equals(headerValue, "XMLHttpRequest", StringComparison.OrdinalIgnoreCase);
                        }
                    }
                    return false;
                }
                private void ValidateRequestHeader(HttpRequestMessage request)
                {
                    string cookieToken = String.Empty;
                    string formToken = String.Empty;
                    IEnumerable<string> tokenHeaders;
                    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
                    {
                        string tokenValue = tokenHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(tokenValue))
                        {
                            string[] tokens = tokenValue.Split(':');
                            if (tokens.Length == 2)
                            {
                                cookieToken = tokens[0].Trim();
                                formToken = tokens[1].Trim();
                            }
                        }
                    }
                    AntiForgery.Validate(cookieToken, formToken);
                }
            }
        }
3. Adja hozzá a következő hello *használatával* utasítás toohello szerződések vezérlő így hozzáférés toohello **[ValidateHttpAntiForgeryToken]** attribútum.
   
        using ContactManager.Filters;
4. Adja hozzá a hello **[ValidateHttpAntiForgeryToken]** toohello Post metódus a hello attribútum **ContactsController** tooprotect XSRF fenyegetések elleni védelmét. Toohello "PutContact", "PostContact" lesz hozzáadása és **DeleteContact** műveletmetódusokhoz.
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. Frissítés hello *parancsfájlok* hello szakasza *Views\Home\Index.cshtml* tooinclude kód tooget hello XSRF jogkivonatok fájlt.
   
         @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                @functions{
                   public string TokenHeaderValue()
                   {
                      string cookieToken, formToken;
                      AntiForgery.GetTokens(null, out cookieToken, out formToken);
                      return cookieToken + ":" + formToken;                
                   }
                }
   
               function ContactsViewModel() {
                  var self = this;
                  self.contacts = ko.observableArray([]);
                  self.addContact = function () {
   
                     $.ajax({
                        type: "post",
                        url: "api/contacts",
                        data: $("#addContact").serialize(),
                        dataType: "json",
                        success: function (value) {
                           self.contacts.push(value);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
                     });
   
                  }
                  self.removeContact = function (contact) {
                     $.ajax({
                        type: "DELETE",
                        url: contact.Self,
                        success: function () {
                           self.contacts.remove(contact);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
   
                     });
                  }
   
                  $.getJSON("api/contacts", function (data) {
                     self.contacts(data);
                  });
               }
               ko.applyBindings(new ContactsViewModel());
            </script>
         }

## <a name="publish-hello-application-update-tooazure-and-sql-database"></a>Hello alkalmazás frissítés tooAzure és SQL-adatbázis közzététele
toopublish hello alkalmazás, akkor ismételje meg a korábban követte hello eljárás.

1. A **Megoldáskezelőben**, hello projektben kattintson a jobb gombbal, és válassza ki **közzététel**.
   
    ![Közzététel][rxP]
2. Kattintson a hello **beállítások** fülre.
3. A **ContactsManagerContext(ContactsManagerContext)**, hello kattintson **v** ikon toochange *távoli kapcsolati karakterlánc* hello forduljon toohello kapcsolati karakterlánca az adatbázis. Kattintson a **ContactDB**.
   
    ![Beállítások](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. Hello jelölőnégyzetet a **hajtható végre Code First áttelepítést (fut az alkalmazás indítása)**.
5. Kattintson a **következő** majd **előzetes**. A Visual Studio hello fájlok hozzáadott vagy frissített listáját jeleníti meg.
6. Kattintson a **Publish** (Közzététel) gombra.
   Hello központi telepítés befejezése után hello böngésző megnyitja toohello kezdőlap hello alkalmazás.
   
    ![Index lap nincs ügyfelekkel][intro001]
   
    Visual Studio hello folyamat automatikusan konfigurált hello kapcsolati karakterlánc közzététele a telepített hello *Web.config* fájl toopoint toohello SQL-adatbázis. Azt is konfigurálni Code First áttelepítést tooautomatically frissítési hello adatbázis toohello legújabb verziója hello első hello alkalmazása hello adatbázis hozzáfér a telepítés után.
   
    Ez a konfiguráció miatt Code First létrehozott hello adatbázist hello kód futtatásával a hello **kezdeti** korábban létrehozott osztályt. Telepítés után ugyanúgy a hello első alkalommal hello próbált tooaccess hello adatbázis-alkalmazás.
7. Adja meg a partner helyileg, hello app tooverify adatbázis telepítése sikeresen futtatta hasonló módon.

Amikor megjelenik az adott meg hello elem menti, és hello kapcsolattartási manager lapján jelenik meg, tudja, hogy hello adatbázisban lett tárolt.

![Index lap ügyfelekkel][addwebapi004]

hello alkalmazás innentől kezdve fut hello felhő, használja az SQL-adatbázis toostore az adatokat. Miután befejezte a hello alkalmazás tesztelése az Azure-ban, törölje azt. hello alkalmazás nyilvános, és nem rendelkezik olyan mechanizmus toolimit hozzáféréssel.

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="next-steps"></a>Következő lépések
Egy másik módja toostore az Azure-alkalmazások adatai toouse az Azure storage, blobok és táblák hello formájában nem relációs adatok tárolásához. a következő hivatkozások hello nyújtanak további információt a Web API, az ASP.NET MVC és a Windows Azure.

* [Ismerkedés az Entity Framework MVC használatával][EFCodeFirstMVCTutorial]
* [Bevezetés tooASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Az első ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Hibakeresési WAWS](web-sites-dotnet-troubleshoot-visual-studio.md)

Ez az oktatóanyag és hello mintaalkalmazás kiírt [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) Tom Dykstra és Barry Dorrans segítséget (Twitter [ @blowdart ](https://twitter.com/blowdart)). 

Visszajelzés meghagyása mi tetszett és mi szeretné javítani, nem csak kapcsolatos hello oktatóanyag magát, hanem azt mutatja be, hogy hello termékekre vonatkozó toosee. Visszajelzése segít sorrendjének fejlesztései. Folyamatban, különösen akkor érdekli automatizálással hello folyamat konfigurálása és telepítése hello tagsági adatbázisokból nincs mennyi iránt érdeklődik, keresése. 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles toohello Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update hello Membership Database]:#ppd2
[setupdbenv]: #bkmk_setupdevenv
[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[adddb]: #bkmk_addadatabase
[addcontroller]: #bkmk_addcontroller
[addwebapi]: #bkmk_addwebapi
[deploy2]: #bkmk_deploydatabaseupdate

<!-- links -->
[EFCodeFirstMVCTutorial]: http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
[dbcontext-link]: http://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx


<!-- images-->
[rxE]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxE.png
[rxP]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxP.png
[rx22]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/
[rxb2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxb2.png
[rxz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz.png
[rxzz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxzz.png
[rxz2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz2.png
[rxz3]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz3.png
[rxStyle]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxStyle.png
[rxz4]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz4.png
[rxz44]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz44.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxPrevDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPrevDB.png
[rxOverwrite]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxOverwrite.png
[rxPWS]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPWS.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxAddApiController]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxAddApiController.png
[rxFFchrome]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxFFchrome.png
[intro001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobil-intro-finished-web-app.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxCreateWSwithDB.png
[setup007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-setup-azure-site-004.png
[setup009]: ../Media/dntutmobile-setup-azure-site-006.png
[newapp002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-002.png
[newapp004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-004.png
[firsdeploy007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-005.png
[firsdeploy009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-007.png
[adddb001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-001.png
[adddb002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-context-menu.png
[addcode002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-controller-dialog.png
[addcode004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-index-context.png
[addcode005]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-contents-context-menu.png
[addcode007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-bundleconfig-context.png
[addcode008]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-console.png
[addwebapi004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-added-contact.png
[addwebapi006]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-save-returned-contacts.png
[addwebapi007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-contacts-in-notepad.png
[Add XSRF Protection]: #xsrf
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[Add XSRF Protection]: #xsrf
[ImportPublishSettings]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishSettings.png
[ImportPublishProfile]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishProfile.png
[PublishVSSolution]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/PublishVSSolution.png
[ValidateConnection]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ValidateConnection.png
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[prevent-csrf-attacks]: http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-(csrf)-attacks

