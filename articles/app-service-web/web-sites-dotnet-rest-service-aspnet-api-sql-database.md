---
title: "Hozzon létre egy REST API-t az Azure-ban az ASP.NET és az SQL DB |} Microsoft Docs"
description: "Ez az oktatóanyag útmutatást ad meg, hogy az ASP.NET Web API az Azure-webalkalmazás használ a Visual Studio használatával alkalmazások központi telepítése."
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
ms.openlocfilehash: 64c18f2cfabbb7af6ffd89b4c2a9095fca1cf799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a>Hozzon létre egy REST-szolgáltatást az ASP.NET Web API és az SQL-adatbázis használata az Azure App Service-ben
Ez az oktatóanyag bemutatja az ASP.NET webalkalmazás telepítése egy [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a Visual Studio 2013 vagy Visual Studio 2013 Community Edition webhely közzététele varázsló használatával. 

Az Azure-fiók szabad megnyithatja, és ha még nem rendelkezik a Visual Studio 2013, az SDK automatikusan telepíti a Visual Studio 2013 Express webes. Ezért nekiláthat teljes mértékben az Azure ingyenes.

Ez az oktatóanyag feltételezi, hogy rendelkezik-e nincs előzetes tapasztalata az Azure használatával kapcsolatban. Az oktatóanyag elvégzését be egy egyszerű webalkalmazást és a felhőben futó kell.

Az oktatóanyagból a következőket sajátíthatja el:

* A gép alkalmassá tétele az Azure-alapú fejlesztésre az Azure SDK telepítésével.
* Hozzon létre egy Visual Studio ASP.NET MVC 5 projektet, és tegye közzé az Azure alkalmazás módjáról.
* Hogyan lehet Restful API-hívások engedélyezése az ASP.NET Web API segítségével.
* Hogyan használható az SQL-adatbázis adatok tárolásához Azure-ban.
* Alkalmazás közzététele – útmutató frissíti az Azure-bA.

ASP.NET MVC 5 épül, és használja az ADO.NET Entity Framework adatbázis hozzáférés a ismerőslistájába egyszerű webalkalmazás lesz létrehozása. Az alábbi ábrán a kész alkalmazás látható:

![Képernyőkép a webhely][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-the-project"></a>A projekt létrehozása
1. Indítsa el a Visual Studio 2013.
2. Az a **fájl** menüben kattintson **új projekt**.
3. Az a **új projekt** párbeszédpanelen bontsa ki **Visual C#** válassza **webes** , és válassza **ASP.NET Web Application**. Adjon nevet az alkalmazásnak **ContactManager** kattintson **OK**.
   
    ![A New Project (Új projekt) párbeszédpanel](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. A a **új ASP.NET-projekt** párbeszédpanelen jelölje ki a **MVC** sablon, a jelölőnégyzet **Web API** , majd **hitelesítés módosítása**.
5. A **Change Authentication** (Hitelesítés módosítása) párbeszédpanelen kattintson a **No Authentication** (Nincs hitelesítés) elemre, majd az **OK** gombra.
   
    ![Nincs hitelesítés](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    A mintaalkalmazás hoz létre, nem kell funkciókra, a felhasználóknak a bejelentkezéshez. Hitelesítési és engedélyezési szolgáltatásokat megvalósításához kapcsolatos információkért lásd: a [lépések](#nextsteps) szakasz Ez az oktatóanyag végén. 
6. Az a **új ASP.NET projekt** párbeszédpanelen győződjön meg arról, hogy a **a felhőben lévő gazdagéphez** be van jelölve, és kattintson a **OK**.

Ha nem korábban bejelentkezett az Azure-ba, kérni fogja a bejelentkezéshez.

1. A konfiguráció varázsló egy egyedi nevet a alapján javasol *ContactManager* (lásd az alábbi képen). Válasszon egy régiót a környéken. Használhat [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") található a legkisebb mértékű késés adatközpont. 
2. Ha egy adatbázis-kiszolgálót, mielőtt még nem hozott létre, jelölje be **létrehozása új kiszolgáló**, adjon meg egy adatbázis-felhasználónevet és jelszót.
   
    ![Azure-webhely konfigurálása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

Ha egy adatbázis-kiszolgálót, amelyek segítségével hozzon létre egy új adatbázist. Adatbázis-kiszolgálók értékes erőforrás, és általában létrehozásához több adatbázis ugyanazon a kiszolgálón, tesztelése és fejlesztési helyett az adatbázis egy adatbázis-kiszolgáló létrehozása. Győződjön meg arról, hogy a webhely, az adatbázis ugyanabban a régióban.

![Azure-webhely konfigurálása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-the-page-header-and-footer"></a>Az oldal fejlécében és láblécében beállítása
1. A **Megoldáskezelőben**, bontsa ki a *Views\Shared* mappa, és nyissa meg a *_Layout.cshtml* fájlt.
   
    ![A Solution Explorer _Layout.cshtml][newapp004]
2. Cserélje le a tartalmát a *Views\Shared_Layout.cshtml* fájl a következő kóddal:

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

A fenti kód módosításai az alkalmazás nevére "My ASP.NET alkalmazás" Manager"ügyfél", és eltávolítja a hivatkozásokat **Home**, **kapcsolatos** és **forduljon**.

### <a name="run-the-application-locally"></a>Az alkalmazás helyi futtatása
1. Az alkalmazás futtatásához nyomja le a Ctrl+F5 billentyűkombinációt.
   Az alkalmazás kezdőlapját az alapértelmezett böngésző jelenik meg.
    ![Tennivalók listája kezdőlapjára](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

Ez az összes szükséges most az alkalmazást, amely az Azure-bA telepítésekor létrehozásához. Később fogja hozzáadni adatbázisokkal kapcsolatos funkciók.

## <a name="deploy-the-application-to-azure"></a>Az alkalmazás központi telepítése az Azure-ban
1. A Visual Studióban, kattintson a jobb gombbal a projektre a **Megoldáskezelőben** válassza **közzététel** a helyi menüből.
   
    ![Tegye közzé ezt a projekt helyi menü][PublishVSSolution]
   
    A **webhely közzététele** varázsló megnyílik.
2. Kattintson a **Publish** (Közzététel) gombra.

![Beállítások lap](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

A Visual Studio megkezdi a fájlok másolása az Azure-kiszolgálóval. A **kimeneti** ablak mutatja, milyen telepítési műveletek vették és sikeres telepítésről szóló jelentéseket.

1. Az alapértelmezett böngésző automatikusan megnyitja a telepített hely URL-címét.
   
   A létrehozott alkalmazás innentől kezdve fut a felhőben.
   
   ![Azure-beli teendőlista kezdőlapjára][rxz2]

## <a name="add-a-database-to-the-application"></a>Egy adatbázis hozzáadni az alkalmazáshoz
Ezt követően frissíteni fogja az MVC alkalmazás hozzáadása megjelenítéséhez és frissítése a névjegyeket és képes tárolni az adatokat adatbázisban. Az alkalmazás használja az Entity Framework létrehozni az adatbázist, és hogy olvassa és frissítse az adatbázis adatai.

### <a name="add-data-model-classes-for-the-contacts"></a>Adja hozzá a névjegyek adatosztályokat modell
Hozzon létre egy egyszerű adatmodell kódban megkezdése.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a Models mappát, kattintson a **hozzáadása**, majd **osztály**.
   
    ![Modellek mappa helyi menü osztály hozzáadása][adddb001]
2. Az a **új elem hozzáadása** párbeszédablakban nevezze el az új osztály fájl *Contact.cs*, és kattintson a **Hozzáadás**.
   
    ![Adja hozzá az új elem párbeszédpanel][adddb002]
3. Cserélje le a Contacts.cs fájl tartalmát az alábbira.
   
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

A **forduljon** osztály határozza meg az adatokat, amelyek az egyes partnerek, valamint az elsődleges kulcs, ContactID, az adatbázis által igényelt tárolására. További információ a modellek adatokat kaphat a [lépések](#nextsteps) szakasz Ez az oktatóanyag végén.

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a>Hozzon létre, amelyek lehetővé teszik a felhasználók számára a névjegyek együttműködve weblapok
Az ASP.NET MVC a állványok funkció automatikusan elő tud állítani megvalósító kódot létrehozása, olvasása, frissítése és Törlés (CRUD) műveleteket.

## <a name="add-a-controller-and-a-view-for-the-data"></a>Egy tartományvezérlő és az adatok egy nézet hozzáadása
1. A **Megoldáskezelőben**, bontsa ki a tartományvezérlők mappát.
2. A projekt felépítéséhez **(Ctrl + Shift + B)**. (Kell létrehozni a projekt állványok mechanizmus használata előtt.) 
3. Kattintson a jobb gombbal a tartományvezérlők mappát, és kattintson a **Hozzáadás**, és kattintson a **vezérlő**.
   
    ![Vezérlő hozzáadása a tartományvezérlők mappa helyi menü][addcode001]
4. Az a **hozzáadása Scaffold** párbeszédpanelen jelölje ki **MVC-vezérlő nézetekkel, entitás-keretrendszer használatával** kattintson **Hozzáadás**.
   
   ![Vezérlő hozzáadása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. A vezérlő neve **HomeController**. Válassza ki **forduljon** , a modell osztály. Kattintson a **új adatkörnyezetet** gombra, és fogadja el az alapértelmezett "ContactManager.Models.ContactManagerContext" számára a **új adatkörnyezet-típusának**. Kattintson az **Add** (Hozzáadás) parancsra.

    A párbeszédpanel figyelmezteti: "a nevű fájl HomeController már létezik. Szeretné lecserélni? ". Kattintson a **Yes** (Igen) gombra. A kezdőlap vezérlő hoztak létre az új projekt másolattal azt. Az új kezdőlap tartományvezérlő a ismerőslistájába azt fogja használni.

    A Visual Studio létrehozza a vezérlő metódusokhoz és az adatbázis-műveleteknél CRUD nézetek **forduljon** objektumok.

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a>Áttelepítések engedélyezése, hozza létre az adatbázist, mintaadatokat és egy adatok inicializáló hozzáadása
A következő feladat, hogy engedélyezze a [Code First áttelepítést](http://curah.microsoft.com/55220) létrehozott adatok minta alapján szolgáltatás az adatbázis létrehozásához.

1. Az a **eszközök** menü **Kódtárcsomag-kezelő** , majd **Csomagkezelő konzol**.
   
    ![Az Eszközök menü Csomagkezelő konzol][addcode008]
2. Az a **Csomagkezelő konzol** ablak, írja be a következő parancsot:
   
        enable-migrations 
   
    A **enable-áttelepítések** parancs létrehoz egy *áttelepítések* abban a mappában helyezi a mappára és egy *Configuration.cs* fájlt, amely az áttelepítés konfigurálása szerkesztheti. 
3. Az a **Csomagkezelő konzol** ablak, írja be a következő parancsot:
   
        add-migration Initial
   
    A **hozzáadása áttelepítés kezdeti** parancs létrehoz egy osztályt  **&lt;date_stamp&gt;kezdeti** , amely létrehozza az adatbázist. Az első paraméter ( *kezdeti* ) tetszőleges és a név a fájl létrehozásához használt. Megtekintheti az új osztály fájlokat **Megoldáskezelőben**.
   
    A a **kezdeti** osztály, a **mentése** hoz létre a névjegyek tábla és a **le** (Ha azt szeretné, a korábbi állapotának visszaállítására használt) módszer esik azt.
4. Nyissa meg a *Migrations\Configuration.cs* fájlt. 
5. A következő névterek. 
   
         using ContactManager.Models;
6. Cserélje le a *kezdőérték* metódus a következő kóddal:
   
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
   
    A fenti kódot inicializálja a kapcsolattartási adatokat az adatbázisba. Az adatbázis összehangolása a további információkért lásd: [hibakeresés entitás-keretrendszer (EF) adatbázisok](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).
7. Az a **Csomagkezelő konzol** adja meg a parancsot:
   
        update-database
   
    ![A Package Manager Console parancsok][addcode009]
   
    A **adatbázis-frissítés** futtatja az első áttelepítésről, amely létrehozza az adatbázist. Alapértelmezés szerint az adatbázist egy SQL Server Express LocalDB adatbázisban jön létre.
8. Az alkalmazás futtatásához nyomja le a Ctrl+F5 billentyűkombinációt. 

Az alkalmazás kezdőérték adatainak megjelenítése és szerkesztése, a részletek és a delete hivatkozásokat tartalmaz.

![Az adatok MVC megtekintése][rxz3]

## <a name="edit-the-view"></a>A nézet szerkesztése
1. Nyissa meg a *Views\Home\Index.cshtml* fájlt. A következő lépésben azt lecseréli a generált kód kód [jQuery](http://jquery.com/) és [Knockout.js](http://knockoutjs.com/). Az új kódot névjegyek listájának beolvasása a webes API és a JSON és majd a felhasználói felület használatával knockout.js van kötve a kapcsolattartási adatok. További információkért lásd: a [lépések](#nextsteps) szakasz Ez az oktatóanyag végén. 
2. Cserélje le a fájl tartalmát az alábbira.
   
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
3. Kattintson a jobb gombbal a tartalmat tartalmazó mappa, és kattintson a **Hozzáadás**, és kattintson a **új elem...** .
   
    ![Adja hozzá a stíluslap tartalmat tartalmazó mappa helyi menü][addcode005]
4. Az a **új elem hozzáadása** párbeszédpanelen adja meg a **stílus** jobb felső a keresési mezőbe, és válassza **stíluslap**.
    ![Adja hozzá az új elem párbeszédpanel][rxStyle]
5. A fájl neve *Contacts.css* kattintson **Hozzáadás**. Cserélje le a fájl tartalmát az alábbira.
   
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
   
    A stíluslap elrendezés, színek és a kapcsolattartási alkalmazással használt stílusok használjuk.
6. Nyissa meg a *App_Start\BundleConfig.cs* fájlt.
7. Adja hozzá a következő kódot a [kiejtés](http://knockoutjs.com/index.html "KO") beépülő modul.
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    Ez a minta kiejtés használatával egyszerűbbé teheti a dinamikus JavaScript-kódot, amely kezeli a képernyő-sablonokat.
8. A tartalom/css bejegyzés regisztrálni a *contacts.css* stíluslap. Módosítsa a következő sort:
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   :
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. A Package Manager Console kiejtés telepítéséhez a következő parancsot.
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-the-web-api-restful-interface"></a>A webes API Restful interfész vezérlő hozzáadása
1. A **Megoldáskezelőben**, kattintson a jobb gombbal a tartományvezérlők, és kattintson a **Hozzáadás** , majd **vezérlő...** 
2. A a **hozzáadása Scaffold** párbeszédpanelen adja meg a **Web API 2-es vezérlőhöz műveletekhez, entitás-keretrendszer használatával** , majd **Hozzáadás**.
   
    ![API-vezérlő hozzáadása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. Az a **adja hozzá a tartományvezérlő** párbeszédpanelen adja meg a "ContactsController", a tartományvezérlő neve. "Forduljon (ContactManager.Models)" Válassza ki a **Model class**.  Tartsa meg az alapértelmezett értéket a **adatok környezetben osztály**. 
4. Kattintson az **Add** (Hozzáadás) parancsra.

### <a name="run-the-application-locally"></a>Az alkalmazás helyi futtatása
1. Az alkalmazás futtatásához nyomja le a Ctrl+F5 billentyűkombinációt.
   
    ![Index lap][intro001]
2. Adja meg a partner, és kattintson a **Hozzáadás**. Az alkalmazás adja vissza a kezdőlapra, és megjeleníti a megadott forduljon.
   
    ![A lista Tennivaló index lap][addwebapi004]
3. A böngészőben hozzáfűzése **/api/contacts** URL-címét.
   
    Az eredményül kapott URL-címet fog hasonlítani http://localhost:1234/api/Contacts címen. A RESTful webes API, hozzáadott a tárolt névjegyek adja vissza. A Firefox és Chrome XML formátumban fogja megjeleníteni az adatokat.
   
    ![A lista Tennivaló index lap][rxFFchrome]

    IE kérni fogja megnyitni vagy menteni a névjegyeket.

    ![Webes API-k mentése párbeszédpanel][addwebapi006]


    A visszaadott kapcsolatba lép a Jegyzettömbben vagy a böngésző nyithatja meg.

    A kimeneti például mobil weblap vagy az alkalmazás egy másik alkalmazás képes használni.

    ![Webes API-k mentése párbeszédpanel][addwebapi007]

    **Biztonsági figyelmeztetés**: ezen a ponton az alkalmazás pedig a nem biztonságos téve CSRF támadásnak. Az oktatóanyag későbbi részében megszüntetjük a biztonsági rés. További információ: [akadályozza meg, hogy többhelyes kérelem hamisítására (CSRF) támadások][prevent-csrf-attacks].
## <a name="add-xsrf-protection"></a>XSRF védelem hozzáadása
Webhelyközi kérések hamisítására (más néven XSRF vagy CSRF) webes állomásokon tárolt alkalmazásokhoz, amelyek rosszindulatú webhely befolyásolhatja az ügyfél böngészője, valamint egy webhely, a böngésző által megbízhatónak közötti interakció elleni támadásoknak. Ilyen támadások lehetséges válnak, mert a böngészők által küldött kérelmek automatikusan a hitelesítési tokenek webhely. A kanonikus példája egy hitelesítési cookie-t, például az ASP. NET a az űrlapos hitelesítés jegy. Azonban bármely állandó hitelesítési mechanizmus (például Windows-hitelesítést, a Basic, és így tovább) használó webhelyek célpontja is lehet ilyen támadások.

XSRF támadás nem egyezik a adathalász támadások. Adathalász támadások igényelnek az áldozat a beavatkozást. Az egy adathalász támadások rosszindulatú webhely lesz utánozzák a webhely, és az áldozat magát van becsapni a bizalmas adatokat biztosít a támadónak. Az XSRF támadások nincs gyakran párbeszéd az áldozat a szükséges. Ehelyett a támadó a böngésző automatikusan megfelelő cookie-k küld a cél webhely megbízható függő.

További információkért lásd: a [biztonsági projekt megnyitása webes alkalmazás](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).

1. A **Megoldáskezelőben**, jobb **ContactManager** projektre, kattintson **hozzáadása** majd **osztály**.
2. A fájl neve *ValidateHttpAntiForgeryTokenAttribute.cs* és adja hozzá a következő kódot:
   
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
3. Adja hozzá a következő *használatával* nyilatkozatot, így a szerződések vezérlő így hozzáférhet a **[ValidateHttpAntiForgeryToken]** attribútum.
   
        using ContactManager.Filters;
4. Adja hozzá a **[ValidateHttpAntiForgeryToken]** a Post metódus az attribútumot a **ContactsController** XSRF fenyegetésekkel szembeni hatékony védelmét. Hozzáadja a "PutContact", "PostContact" és **DeleteContact** műveletmetódusokhoz.
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. Frissítés a *parancsfájlok* szakasza a *Views\Home\Index.cshtml* fájlt a kód segítségével kérheti le a XSRF jogkivonatokat.
   
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

## <a name="publish-the-application-update-to-azure-and-sql-database"></a>A frissítés közzétételéhez Azure és az SQL-adatbázis
Az alkalmazás közzététele, ismételje meg a korábban követte eljárást.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a projektre, és válassza ki **közzététel**.
   
    ![Közzététel][rxP]
2. Kattintson a **Beállítások** fülre.
3. A **ContactsManagerContext(ContactsManagerContext)**, kattintson a **v** módosítása ikon *távoli kapcsolati karakterlánc* kapcsolattartási adatbázis a kapcsolati karakterlánc módosításait. Kattintson a **ContactDB**.
   
    ![Beállítások](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. Jelölje be a **hajtható végre Code First áttelepítést (fut az alkalmazás indítása)**.
5. Kattintson a **következő** majd **előzetes**. A Visual Studio a hozzáadott vagy frissített fájlok listáját jeleníti meg.
6. Kattintson a **Publish** (Közzététel) gombra.
   A telepítés befejezése után a böngésző megnyitja az alkalmazás kezdőlapja.
   
    ![Index lap nincs ügyfelekkel][intro001]
   
    A Visual Studio közzétételi folyamat automatikusan konfigurálva a kapcsolati karakterlánc az alkalmazott *Web.config* fájlt úgy, hogy az SQL-adatbázis mutasson. Azt is konfigurálni Code First áttelepítést automatikusan az adatbázis frissítése a legújabb verzióra először az alkalmazás telepítése után az adatbázis fér hozzá.
   
    Ez a konfiguráció miatt Code First hozta létre az adatbázist a kód futtatásával a **kezdeti** korábban létrehozott osztályt. Ez ugyanúgy az alkalmazás telepítése után az adatbázis eléréséhez próbált meg először.
7. Adja meg a forduljon, úgy, ahogy az adatbázis telepítése sikeres volt, hogy helyileg, az alkalmazás futtatásakor.

Ha látja, hogy az elem meg menti, és kapcsolattartási manager lapján jelenik meg, azt jelzi, hogy az adatbázis már tárolt.

![Index lap ügyfelekkel][addwebapi004]

Az alkalmazás most már fut a felhőben, SQL-adatbázis segítségével tárolja az adatokat. Ha befejezte az alkalmazás tesztelése az Azure-ban, törölje azt. Az alkalmazás nyilvános, és nem rendelkezik egy olyan mechanizmus a hozzáférés korlátozásához.

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="next-steps"></a>Következő lépések
Egy másik adat tárolása az Azure-alkalmazások módja az Azure storage használatához nem relációs adatok tárolási BLOB és táblák formájában nyújt. Az alábbi hivatkozások nyújtanak további információt a Web API, az ASP.NET MVC és a Windows Azure.

* [Ismerkedés az Entity Framework MVC használatával][EFCodeFirstMVCTutorial]
* [Bevezetés az ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Az első ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Hibakeresési WAWS](web-sites-dotnet-troubleshoot-visual-studio.md)

Ez az oktatóanyag és a mintaalkalmazás kiírt [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) Tom Dykstra és Barry Dorrans segítséget (Twitter [ @blowdart ](https://twitter.com/blowdart)). 

Adjon hagyja visszajelzés mi tetszett és mi kíváncsi rá továbbfejlesztett, nem csak az oktatóanyag maga kapcsolatos, hanem a termékekről, amely azt mutatja be. Visszajelzése segít sorrendjének fejlesztései. Folyamatban, különösen szeretné tudni, hogy mennyi érdeklődési automatizálással folyamat konfigurálása és telepítése a tagsági adatbázisokból van. 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles to the Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2
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

