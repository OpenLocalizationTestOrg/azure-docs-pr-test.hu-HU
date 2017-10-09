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
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a><span data-ttu-id="82942-103">Hozzon létre egy REST-szolgáltatást az ASP.NET Web API és az SQL-adatbázis használata az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="82942-103">Create a REST service using ASP.NET Web API and SQL Database in Azure App Service</span></span>
<span data-ttu-id="82942-104">Ez az oktatóanyag bemutatja, hogyan toodeploy az ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a Visual Studio 2013 vagy Visual Studio 2013 Community Edition hello webhely közzététele varázsló segítségével.</span><span class="sxs-lookup"><span data-stu-id="82942-104">This tutorial shows how toodeploy an ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by using hello Publish Web wizard in Visual Studio 2013 or Visual Studio 2013 Community Edition.</span></span> 

<span data-ttu-id="82942-105">Az Azure-fiók szabad megnyithatja, és ha még nem rendelkezik a Visual Studio 2013, hello SDK automatikusan telepíti a Visual Studio 2013 Express webes.</span><span class="sxs-lookup"><span data-stu-id="82942-105">You can open an Azure account for free, and if you don't already have Visual Studio 2013, hello SDK automatically installs Visual Studio 2013 for Web Express.</span></span> <span data-ttu-id="82942-106">Ezért nekiláthat teljes mértékben az Azure ingyenes.</span><span class="sxs-lookup"><span data-stu-id="82942-106">So you can start developing for Azure entirely for free.</span></span>

<span data-ttu-id="82942-107">Ez az oktatóanyag feltételezi, hogy rendelkezik-e nincs előzetes tapasztalata az Azure használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="82942-107">This tutorial assumes that you have no prior experience using Azure.</span></span> <span data-ttu-id="82942-108">Az oktatóanyag elvégzését be egy egyszerű webalkalmazást és hello felhőben futó kell.</span><span class="sxs-lookup"><span data-stu-id="82942-108">On completing this tutorial, you'll have a simple web app up and running in hello cloud.</span></span>

<span data-ttu-id="82942-109">Az oktatóanyagból a következőket sajátíthatja el:</span><span class="sxs-lookup"><span data-stu-id="82942-109">You'll learn:</span></span>

* <span data-ttu-id="82942-110">Hogyan tooenable Azure fejlesztési telepítésével, a gép hello Azure SDK-t.</span><span class="sxs-lookup"><span data-stu-id="82942-110">How tooenable your machine for Azure development by installing hello Azure SDK.</span></span>
* <span data-ttu-id="82942-111">Hogyan toocreate a Visual Studio ASP.NET MVC 5 projektre, és tegye közzé az Azure app tooan.</span><span class="sxs-lookup"><span data-stu-id="82942-111">How toocreate a Visual Studio ASP.NET MVC 5 project and publish it tooan Azure app.</span></span>
* <span data-ttu-id="82942-112">Hogyan toouse hello ASP.NET Web API tooenable Restful API hívásokat.</span><span class="sxs-lookup"><span data-stu-id="82942-112">How toouse hello ASP.NET Web API tooenable Restful API calls.</span></span>
* <span data-ttu-id="82942-113">Hogyan toouse egy SQL-adatbázis használati ideje toostore adatok Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="82942-113">How toouse a SQL database toostore data in Azure.</span></span>
* <span data-ttu-id="82942-114">Hogy hogyan toopublish alkalmazás tooAzure frissíti.</span><span class="sxs-lookup"><span data-stu-id="82942-114">How toopublish application updates tooAzure.</span></span>

<span data-ttu-id="82942-115">Akkor lesz ASP.NET MVC 5 épülő egyszerű ismerősök listájának webalkalmazás létrehozása a, és használja az ADO.NET Entity Framework hello az adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="82942-115">You'll build a simple contact list web application that is built on ASP.NET MVC 5 and uses hello ADO.NET Entity Framework for database access.</span></span> <span data-ttu-id="82942-116">a következő ábra azt mutatja be hello hello kérelem befejeződött:</span><span class="sxs-lookup"><span data-stu-id="82942-116">hello following illustration shows hello completed application:</span></span>

![Képernyőkép a webhely][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-hello-project"></a><span data-ttu-id="82942-118">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="82942-118">Create hello project</span></span>
1. <span data-ttu-id="82942-119">Indítsa el a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="82942-119">Start Visual Studio 2013.</span></span>
2. <span data-ttu-id="82942-120">A hello **fájl** menüben kattintson **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="82942-120">From hello **File** menu click **New Project**.</span></span>
3. <span data-ttu-id="82942-121">A hello **új projekt** párbeszédpanelen bontsa ki **Visual C#** válassza **webes** , és válassza **ASP.NET Web Application**.</span><span class="sxs-lookup"><span data-stu-id="82942-121">In hello **New Project** dialog box, expand **Visual C#** and select **Web**  and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="82942-122">Hello alkalmazás neve **ContactManager** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="82942-122">Name hello application **ContactManager** and click **OK**.</span></span>
   
    ![A New Project (Új projekt) párbeszédpanel](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. <span data-ttu-id="82942-124">A hello **új ASP.NET projekt** párbeszédpanel megnyitásához, jelölje be hello **MVC** sablon ellenőrzése **Web API** , majd **hitelesítés módosítása**.</span><span class="sxs-lookup"><span data-stu-id="82942-124">In hello **New ASP.NET Project** dialog box, select hello **MVC** template, check **Web API** and then click **Change Authentication**.</span></span>
5. <span data-ttu-id="82942-125">A hello **hitelesítés módosítása** párbeszédpanel, kattintson a **nem hitelesítési**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="82942-125">In hello **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span>
   
    ![Nincs hitelesítés](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    <span data-ttu-id="82942-127">hello mintaalkalmazás hoz létre, nem kell a felhasználók toolog igénylő szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="82942-127">hello sample application you're creating won't have features that require users toolog in.</span></span> <span data-ttu-id="82942-128">További információ tooimplement hitelesítési és engedélyezési szolgáltatásokat, lásd: hello [lépések](#nextsteps) szakasz hello Ez az oktatóanyag végén.</span><span class="sxs-lookup"><span data-stu-id="82942-128">For information about how tooimplement authentication and authorization features, see hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span> 
6. <span data-ttu-id="82942-129">A hello **új ASP.NET projekt** párbeszédpanelen győződjön meg arról, hogy hello **gazdagépet a felhő hello** be van jelölve, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="82942-129">In hello **New ASP.NET Project** dialog box, make sure hello **Host in hello Cloud** is checked and click **OK**.</span></span>

<span data-ttu-id="82942-130">Ha korábban már nincs bejelentkezve tooAzure, a kért toosign fogja.</span><span class="sxs-lookup"><span data-stu-id="82942-130">If you have not previously signed in tooAzure, you will be prompted toosign in.</span></span>

1. <span data-ttu-id="82942-131">hello konfigurációs varázsló egy egyedi nevet a alapján javasol *ContactManager* (lásd az alábbi képen hello).</span><span class="sxs-lookup"><span data-stu-id="82942-131">hello configuration wizard will suggest a unique name based on *ContactManager* (see hello image below).</span></span> <span data-ttu-id="82942-132">Válasszon egy régiót a környéken.</span><span class="sxs-lookup"><span data-stu-id="82942-132">Select a region near you.</span></span> <span data-ttu-id="82942-133">Használhat [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello legalacsonyabb késés adatközpont.</span><span class="sxs-lookup"><span data-stu-id="82942-133">You can use [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello lowest latency data center.</span></span> 
2. <span data-ttu-id="82942-134">Ha egy adatbázis-kiszolgálót, mielőtt még nem hozott létre, jelölje be **létrehozása új kiszolgáló**, adjon meg egy adatbázis-felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="82942-134">If you haven't created a database server before, select **Create new server**, enter a database user name and password.</span></span>
   
    ![Azure-webhely konfigurálása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

<span data-ttu-id="82942-136">Ha egy adatbázis-kiszolgálót, használja az adott toocreate egy új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="82942-136">If you have a database server, use that toocreate a new database.</span></span> <span data-ttu-id="82942-137">Adatbázis-kiszolgálók értékes erőforrás, és általában kívánt toocreate hello több adatbázist ugyanarra a kiszolgálóra a tesztelés és fejlesztési létrehozása helyett adatbázis egy adatbázis-kiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="82942-137">Database servers are a precious resource, and you generally want toocreate multiple databases on hello same server for testing and development rather than creating a database server per database.</span></span> <span data-ttu-id="82942-138">Gondoskodjon arról, hogy a webhely, az adatbázis hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="82942-138">Make sure your web site and database are in hello same region.</span></span>

![Azure-webhely konfigurálása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-hello-page-header-and-footer"></a><span data-ttu-id="82942-140">Hello oldal fejlécében és láblécében beállítása</span><span class="sxs-lookup"><span data-stu-id="82942-140">Set hello page header and footer</span></span>
1. <span data-ttu-id="82942-141">A **Megoldáskezelőben**, bontsa ki a hello *Views\Shared* mappa és a nyitott hello *_Layout.cshtml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="82942-141">In **Solution Explorer**, expand hello *Views\Shared* folder and open hello *_Layout.cshtml* file.</span></span>
   
    ![A Solution Explorer _Layout.cshtml][newapp004]
2. <span data-ttu-id="82942-143">Cserélje le a hello hello tartalmát *Views\Shared_Layout.cshtml* hello kód a következő fájlt:</span><span class="sxs-lookup"><span data-stu-id="82942-143">Replace hello contents of hello *Views\Shared_Layout.cshtml* file with hello following code:</span></span>

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

<span data-ttu-id="82942-144">hello markup fenti módosítások hello alkalmazás neve a "Saját ASP.NET-alkalmazás" túl Manager "ügyfél", és eltávolítja hello hivatkozások túl**Home**, **kapcsolatos** és **forduljon**.</span><span class="sxs-lookup"><span data-stu-id="82942-144">hello markup above changes hello app name from "My ASP.NET App" too"Contact Manager", and it removes hello links too**Home**, **About** and **Contact**.</span></span>

### <a name="run-hello-application-locally"></a><span data-ttu-id="82942-145">Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="82942-145">Run hello application locally</span></span>
1. <span data-ttu-id="82942-146">Nyomja le a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="82942-146">Press CTRL+F5 toorun hello application.</span></span>
   <span data-ttu-id="82942-147">hello alkalmazás kezdőlapját hello alapértelmezett böngésző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="82942-147">hello application home page appears in hello default browser.</span></span>
    <span data-ttu-id="82942-148">![tooDo lista kezdőlap](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span><span class="sxs-lookup"><span data-stu-id="82942-148">![tooDo List home page](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)</span></span>

<span data-ttu-id="82942-149">Ez az összes toodo most toocreate hello alkalmazás tooAzure fogja telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="82942-149">This is all you need toodo for now toocreate hello application that you'll deploy tooAzure.</span></span> <span data-ttu-id="82942-150">Később fogja hozzáadni adatbázisokkal kapcsolatos funkciók.</span><span class="sxs-lookup"><span data-stu-id="82942-150">Later you'll add database functionality.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="82942-151">Hello alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="82942-151">Deploy hello application tooAzure</span></span>
1. <span data-ttu-id="82942-152">A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** válassza **közzététel** hello helyi menüből.</span><span class="sxs-lookup"><span data-stu-id="82942-152">In Visual Studio, right-click hello project in **Solution Explorer** and select **Publish** from hello context menu.</span></span>
   
    ![Tegye közzé ezt a projekt helyi menü][PublishVSSolution]
   
    <span data-ttu-id="82942-154">Hello **webhely közzététele** varázsló megnyílik.</span><span class="sxs-lookup"><span data-stu-id="82942-154">hello **Publish Web** wizard opens.</span></span>
2. <span data-ttu-id="82942-155">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="82942-155">Click **Publish**.</span></span>

![Beállítások lap](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

<span data-ttu-id="82942-157">A Visual Studio megkezdi hello eljárást másolásának hello fájlok toohello Azure-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="82942-157">Visual Studio begins hello process of copying hello files toohello Azure server.</span></span> <span data-ttu-id="82942-158">Hello **kimeneti** ablak mutatja, milyen telepítési műveletek vették és jelentések hello központi telepítés sikeres befejezését.</span><span class="sxs-lookup"><span data-stu-id="82942-158">hello **Output** window shows what deployment actions were taken and reports successful completion of hello deployment.</span></span>

1. <span data-ttu-id="82942-159">hello alapértelmezett böngésző automatikusan megnyitja a toohello telepített hello webhely URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="82942-159">hello default browser automatically opens toohello URL of hello deployed site.</span></span>
   
   <span data-ttu-id="82942-160">hello létrehozott alkalmazás innentől kezdve fut hello felhő.</span><span class="sxs-lookup"><span data-stu-id="82942-160">hello application you created is now running in hello cloud.</span></span>
   
   ![Azure-beli tooDo lista kezdőlap][rxz2]

## <a name="add-a-database-toohello-application"></a><span data-ttu-id="82942-162">Adatbázis toohello alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="82942-162">Add a database toohello application</span></span>
<span data-ttu-id="82942-163">A következő lesz frissíti hello MVC alkalmazás tooadd hello képességét toodisplay és ügyfelek frissítése és hello adat tárolása egy adatbázis.</span><span class="sxs-lookup"><span data-stu-id="82942-163">Next, you'll update hello MVC application tooadd hello ability toodisplay and update contacts and store hello data in a database.</span></span> <span data-ttu-id="82942-164">hello alkalmazás hello Entity Framework toocreate hello adatbázison és tooread, és frissíti hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="82942-164">hello application will use hello Entity Framework toocreate hello database and tooread and update data in hello database.</span></span>

### <a name="add-data-model-classes-for-hello-contacts"></a><span data-ttu-id="82942-165">Vegye fel a modell adatosztályokat hello kapcsolattartók</span><span class="sxs-lookup"><span data-stu-id="82942-165">Add data model classes for hello contacts</span></span>
<span data-ttu-id="82942-166">Hozzon létre egy egyszerű adatmodell kódban megkezdése.</span><span class="sxs-lookup"><span data-stu-id="82942-166">You begin by creating a simple data model in code.</span></span>

1. <span data-ttu-id="82942-167">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello Models mappát, kattintson a **hozzáadása**, majd **osztály**.</span><span class="sxs-lookup"><span data-stu-id="82942-167">In **Solution Explorer**, right-click hello Models folder, click **Add**, and then **Class**.</span></span>
   
    ![Modellek mappa helyi menü osztály hozzáadása][adddb001]
2. <span data-ttu-id="82942-169">A hello **új elem hozzáadása** párbeszédpanelen osztály fájl új nevét hello *Contact.cs*, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="82942-169">In hello **Add New Item** dialog box, name hello new class file *Contact.cs*, and then click **Add**.</span></span>
   
    ![Adja hozzá az új elem párbeszédpanel][adddb002]
3. <span data-ttu-id="82942-171">Cserélje le a következő kód hello hello hello Contacts.cs fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="82942-171">Replace hello contents of hello Contacts.cs file with hello following code.</span></span>
   
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

<span data-ttu-id="82942-172">Hello **forduljon** osztály hello adatok, amelyek az egyes partnerek, valamint az elsődleges kulcs, ContactID hello adatbázis által igényelt tárolja azt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="82942-172">hello **Contact** class defines hello data that you will store for each contact, plus a primary key, ContactID, that is needed by hello database.</span></span> <span data-ttu-id="82942-173">Adatmodellekről további információt kaphat a hello [lépések](#nextsteps) szakasz hello Ez az oktatóanyag végén.</span><span class="sxs-lookup"><span data-stu-id="82942-173">You can get more information about data models in hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span>

### <a name="create-web-pages-that-enable-app-users-toowork-with-hello-contacts"></a><span data-ttu-id="82942-174">Hozzon létre, amelyek lehetővé teszik az alkalmazás felhasználók toowork hello ügyfelekkel weblapok</span><span class="sxs-lookup"><span data-stu-id="82942-174">Create web pages that enable app users toowork with hello contacts</span></span>
<span data-ttu-id="82942-175">hello ASP.NET MVC hello állványok funkció automatikusan elő tud állítani megvalósító kódot létrehozása, olvasása, frissítése és Törlés (CRUD) műveleteket.</span><span class="sxs-lookup"><span data-stu-id="82942-175">hello ASP.NET MVC hello scaffolding feature can automatically generate code that performs create, read, update, and delete (CRUD) actions.</span></span>

## <a name="add-a-controller-and-a-view-for-hello-data"></a><span data-ttu-id="82942-176">A vezérlő és egy nézet hello adatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="82942-176">Add a Controller and a view for hello data</span></span>
1. <span data-ttu-id="82942-177">A **Megoldáskezelőben**, hello tartományvezérlők mappát.</span><span class="sxs-lookup"><span data-stu-id="82942-177">In **Solution Explorer**, expand hello Controllers folder.</span></span>
2. <span data-ttu-id="82942-178">Hello projekt felépítéséhez **(Ctrl + Shift + B)**.</span><span class="sxs-lookup"><span data-stu-id="82942-178">Build hello project **(Ctrl+Shift+B)**.</span></span> <span data-ttu-id="82942-179">(Kell létrehozni hello projekt állványok mechanizmus használata előtt.)</span><span class="sxs-lookup"><span data-stu-id="82942-179">(You must build hello project before using scaffolding mechanism.)</span></span> 
3. <span data-ttu-id="82942-180">Kattintson a jobb gombbal a hello, tartományvezérlői mappa, és kattintson a **Hozzáadás**, és kattintson a **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="82942-180">Right-click hello Controllers folder and click **Add**, and then click **Controller**.</span></span>
   
    ![Vezérlő hozzáadása a tartományvezérlők mappa helyi menü][addcode001]
4. <span data-ttu-id="82942-182">A hello **hozzáadása Scaffold** párbeszédpanelen jelölje ki **MVC-vezérlő nézetekkel, entitás-keretrendszer használatával** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="82942-182">In hello **Add Scaffold** dialog box, select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
   
   ![Vezérlő hozzáadása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. <span data-ttu-id="82942-184">Állítsa be a hello tartományvezérlő neve túl**HomeController**.</span><span class="sxs-lookup"><span data-stu-id="82942-184">Set hello controller name too**HomeController**.</span></span> <span data-ttu-id="82942-185">Válassza ki **forduljon** , a modell osztály.</span><span class="sxs-lookup"><span data-stu-id="82942-185">Select **Contact** as your model class.</span></span> <span data-ttu-id="82942-186">Kattintson a hello **új adatkörnyezetet** gombra, és fogadja el a hello alapértelmezett "ContactManager.Models.ContactManagerContext" hello **új adatkörnyezet-típusának**.</span><span class="sxs-lookup"><span data-stu-id="82942-186">Click hello **New data context** button and accept hello default "ContactManager.Models.ContactManagerContext" for hello **New data context type**.</span></span> <span data-ttu-id="82942-187">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="82942-187">Click **Add**.</span></span>

    <span data-ttu-id="82942-188">A párbeszédpanel figyelmezteti: "hello nevű fájl HomeController már létezik.</span><span class="sxs-lookup"><span data-stu-id="82942-188">A dialog box will prompt you: "A file with hello name HomeController already exits.</span></span> <span data-ttu-id="82942-189">Szeretné, hogy tooreplace azt? ".</span><span class="sxs-lookup"><span data-stu-id="82942-189">Do you want tooreplace it?".</span></span> <span data-ttu-id="82942-190">Kattintson a **Yes** (Igen) gombra.</span><span class="sxs-lookup"><span data-stu-id="82942-190">Click **Yes**.</span></span> <span data-ttu-id="82942-191">Hello kezdőlap hello új projekt hoztak létre vezérlő másolattal azt.</span><span class="sxs-lookup"><span data-stu-id="82942-191">We are overwriting hello Home Controller that was created with hello new project.</span></span> <span data-ttu-id="82942-192">Használjuk hello új kezdőlap vezérlő számára a ismerőslistájába.</span><span class="sxs-lookup"><span data-stu-id="82942-192">We will use hello new Home Controller for our contact list.</span></span>

    <span data-ttu-id="82942-193">A Visual Studio létrehozza a vezérlő metódusokhoz és az adatbázis-műveleteknél CRUD nézetek **forduljon** objektumok.</span><span class="sxs-lookup"><span data-stu-id="82942-193">Visual Studio creates controller methods and views for CRUD database operations for **Contact** objects.</span></span>

## <a name="enable-migrations-create-hello-database-add-sample-data-and-a-data-initializer"></a><span data-ttu-id="82942-194">Engedélyezze az áttelepítést, hello adatbázis létrehozása, vegye fel a mintaadatok és egy adatok inicializáló</span><span class="sxs-lookup"><span data-stu-id="82942-194">Enable Migrations, create hello database, add sample data and a data initializer</span></span>
<span data-ttu-id="82942-195">hello következő feladata tooenable hello [Code First áttelepítést](http://curah.microsoft.com/55220) rendelés toocreate hello adatbázis funkció alapján létrehozott hello adatmodell.</span><span class="sxs-lookup"><span data-stu-id="82942-195">hello next task is tooenable hello [Code First Migrations](http://curah.microsoft.com/55220) feature in order toocreate hello database based on hello data model you created.</span></span>

1. <span data-ttu-id="82942-196">A hello **eszközök** menü **Kódtárcsomag-kezelő** , majd **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="82942-196">In hello **Tools** menu, select **Library Package Manager** and then **Package Manager Console**.</span></span>
   
    ![Az Eszközök menü Csomagkezelő konzol][addcode008]
2. <span data-ttu-id="82942-198">A hello **Csomagkezelő konzol** ablak, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="82942-198">In hello **Package Manager Console** window, enter hello following command:</span></span>
   
        enable-migrations 
   
    <span data-ttu-id="82942-199">Hello **enable-áttelepítések** parancs létrehoz egy *áttelepítések* abban a mappában helyezi a mappára és egy *Configuration.cs* fájlt, hogy tooconfigure áttelepítések szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="82942-199">hello **enable-migrations** command creates a *Migrations* folder and it puts in that folder a *Configuration.cs* file that you can edit tooconfigure Migrations.</span></span> 
3. <span data-ttu-id="82942-200">A hello **Csomagkezelő konzol** ablak, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="82942-200">In hello **Package Manager Console** window, enter hello following command:</span></span>
   
        add-migration Initial
   
    <span data-ttu-id="82942-201">Hello **hozzáadása áttelepítés kezdeti** parancs létrehoz egy osztályt  **&lt;date_stamp&gt;kezdeti** hello adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="82942-201">hello **add-migration Initial** command generates a class named **&lt;date_stamp&gt;Initial** that creates hello database.</span></span> <span data-ttu-id="82942-202">az első paraméter hello ( *kezdeti* ) tetszőleges és használt toocreate hello hello fájl neve.</span><span class="sxs-lookup"><span data-stu-id="82942-202">hello first parameter ( *Initial* ) is arbitrary and used toocreate hello name of hello file.</span></span> <span data-ttu-id="82942-203">Megtekintheti a hello új osztály fájlokat **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="82942-203">You can see hello new class files in **Solution Explorer**.</span></span>
   
    <span data-ttu-id="82942-204">A hello **kezdeti** osztály, hello **mentése** hello kapcsolatok táblához és hello hoz létre **le** módszerét (Ha azt szeretné, hogy tooreturn toohello korábbi állapotába) esik az.</span><span class="sxs-lookup"><span data-stu-id="82942-204">In hello **Initial** class, hello **Up** method creates hello Contacts table, and hello **Down** method (used when you want tooreturn toohello previous state) drops it.</span></span>
4. <span data-ttu-id="82942-205">Nyissa meg hello *Migrations\Configuration.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="82942-205">Open hello *Migrations\Configuration.cs* file.</span></span> 
5. <span data-ttu-id="82942-206">Adja hozzá a következő névterek hello.</span><span class="sxs-lookup"><span data-stu-id="82942-206">Add hello following namespaces.</span></span> 
   
         using ContactManager.Models;
6. <span data-ttu-id="82942-207">Cserélje le a hello *kezdőérték* hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="82942-207">Replace hello *Seed* method with hello following code:</span></span>
   
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
   
    <span data-ttu-id="82942-208">A fenti kódot inicializálja hello adatbázis hello kapcsolattartási adatait.</span><span class="sxs-lookup"><span data-stu-id="82942-208">This code above will initialize hello database with hello contact information.</span></span> <span data-ttu-id="82942-209">A beültetés hello adatbázis további információkért lásd: [hibakeresés entitás-keretrendszer (EF) adatbázisok](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span><span class="sxs-lookup"><span data-stu-id="82942-209">For more information on seeding hello database, see [Debugging Entity Framework (EF) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).</span></span>
7. <span data-ttu-id="82942-210">A hello **Csomagkezelő konzol** hello parancsot írja be:</span><span class="sxs-lookup"><span data-stu-id="82942-210">In hello **Package Manager Console** enter hello command:</span></span>
   
        update-database
   
    ![A Package Manager Console parancsok][addcode009]
   
    <span data-ttu-id="82942-212">Hello **adatbázis-frissítés** futtatása hello első áttelepítésről, amely hello adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="82942-212">hello **update-database** runs hello first migration which creates hello database.</span></span> <span data-ttu-id="82942-213">Alapértelmezés szerint hello adatbázis egy SQL Server Express LocalDB adatbázisban jön létre.</span><span class="sxs-lookup"><span data-stu-id="82942-213">By default, hello database is created as a SQL Server Express LocalDB database.</span></span>
8. <span data-ttu-id="82942-214">Nyomja le a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="82942-214">Press CTRL+F5 toorun hello application.</span></span> 

<span data-ttu-id="82942-215">hello alkalmazás hello adatait mutatja be, Szerkesztés, adatokat és delete hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="82942-215">hello application shows hello seed data and provides edit, details and delete links.</span></span>

![Az adatok MVC megtekintése][rxz3]

## <a name="edit-hello-view"></a><span data-ttu-id="82942-217">Hello nézet szerkesztése</span><span class="sxs-lookup"><span data-stu-id="82942-217">Edit hello View</span></span>
1. <span data-ttu-id="82942-218">Nyissa meg hello *Views\Home\Index.cshtml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="82942-218">Open hello *Views\Home\Index.cshtml* file.</span></span> <span data-ttu-id="82942-219">Hello a következő lépésben azt lecseréli generált hello markup kód [jQuery](http://jquery.com/) és [Knockout.js](http://knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="82942-219">In hello next step, we will replace hello generated markup with code that uses [jQuery](http://jquery.com/) and [Knockout.js](http://knockoutjs.com/).</span></span> <span data-ttu-id="82942-220">Az új kódot hello listájához lekéri a webes API-val, és a JSON és kötések hello forduljon adatok toohello felhasználói felület knockout.js használatával.</span><span class="sxs-lookup"><span data-stu-id="82942-220">This new code retrieves hello list of contacts from using web API and JSON and then binds hello contact data toohello UI using knockout.js.</span></span> <span data-ttu-id="82942-221">További információkért lásd: hello [lépések](#nextsteps) szakasz hello Ez az oktatóanyag végén.</span><span class="sxs-lookup"><span data-stu-id="82942-221">For more information, see hello [Next Steps](#nextsteps) section at hello end of this tutorial.</span></span> 
2. <span data-ttu-id="82942-222">Cserélje le a következő kód hello hello hello fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="82942-222">Replace hello contents of hello file with hello following code.</span></span>
   
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
3. <span data-ttu-id="82942-223">Kattintson a jobb gombbal a hello tartalmat tartalmazó mappa, és kattintson a **Hozzáadás**, és kattintson a **új elem...** .</span><span class="sxs-lookup"><span data-stu-id="82942-223">Right-click hello Content folder and click **Add**, and then click **New Item...**.</span></span>
   
    ![Adja hozzá a stíluslap tartalmat tartalmazó mappa helyi menü][addcode005]
4. <span data-ttu-id="82942-225">A hello **új elem hozzáadása** párbeszédpanelen adja meg a **stílus** a hello jobb felső keresési mezőbe, és válassza ki **stíluslap**.</span><span class="sxs-lookup"><span data-stu-id="82942-225">In hello **Add New Item** dialog box, enter **Style** in hello upper right search box and then select **Style Sheet**.</span></span>
    <span data-ttu-id="82942-226">![Adja hozzá az új elem párbeszédpanel][rxStyle]</span><span class="sxs-lookup"><span data-stu-id="82942-226">![Add New Item dialog box][rxStyle]</span></span>
5. <span data-ttu-id="82942-227">Nevű hello fájl *Contacts.css* kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="82942-227">Name hello file *Contacts.css* and click **Add**.</span></span> <span data-ttu-id="82942-228">Cserélje le a következő kód hello hello hello fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="82942-228">Replace hello contents of hello file with hello following code.</span></span>
   
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
   
    <span data-ttu-id="82942-229">A stíluslap hello elrendezés, színeit és hello kapcsolattartási manager alkalmazásban használt stílusok használjuk.</span><span class="sxs-lookup"><span data-stu-id="82942-229">We will use this style sheet for hello layout, colors and styles used in hello contact manager app.</span></span>
6. <span data-ttu-id="82942-230">Nyissa meg hello *App_Start\BundleConfig.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="82942-230">Open hello *App_Start\BundleConfig.cs* file.</span></span>
7. <span data-ttu-id="82942-231">Adja hozzá a következő kód tooregister hello hello [kiejtés](http://knockoutjs.com/index.html "KO") beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="82942-231">Add hello following code tooregister hello [Knockout](http://knockoutjs.com/index.html "KO") plugin.</span></span>
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    <span data-ttu-id="82942-232">Ez a minta kiejtés toosimplify dinamikus JavaScript-kódot, amely kezeli a hello képernyő sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="82942-232">This sample using knockout toosimplify dynamic JavaScript code that handles hello screen templates.</span></span>
8. <span data-ttu-id="82942-233">Hello tartalma/css bejegyzés tooregister hello módosítása *contacts.css* stíluslap.</span><span class="sxs-lookup"><span data-stu-id="82942-233">Modify hello contents/css entry tooregister hello *contacts.css* style sheet.</span></span> <span data-ttu-id="82942-234">A következő sor hello módosítása:</span><span class="sxs-lookup"><span data-stu-id="82942-234">Change hello following line:</span></span>
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   <span data-ttu-id="82942-235">:</span><span class="sxs-lookup"><span data-stu-id="82942-235">To:</span></span>
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. <span data-ttu-id="82942-236">Hello Csomagkezelő konzol futtassa a következő parancs tooinstall kiejtés hello.</span><span class="sxs-lookup"><span data-stu-id="82942-236">In hello Package Manager Console, run hello following command tooinstall Knockout.</span></span>
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-hello-web-api-restful-interface"></a><span data-ttu-id="82942-237">Hello webes API Restful interfész vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="82942-237">Add a controller for hello Web API Restful interface</span></span>
1. <span data-ttu-id="82942-238">A **Megoldáskezelőben**, kattintson a jobb gombbal a tartományvezérlők, és kattintson a **Hozzáadás** , majd **vezérlő...**</span><span class="sxs-lookup"><span data-stu-id="82942-238">In **Solution Explorer**, right-click Controllers and click **Add** and then **Controller....**</span></span> 
2. <span data-ttu-id="82942-239">A hello **hozzáadása Scaffold** párbeszédpanelen adja meg a **Web API 2-es vezérlőhöz műveletekhez, entitás-keretrendszer használatával** , majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="82942-239">In hello **Add Scaffold** dialog box, enter **Web API 2 Controller with actions, using Entity Framework** and then click **Add**.</span></span>
   
    ![API-vezérlő hozzáadása](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. <span data-ttu-id="82942-241">A hello **vezérlő hozzáadása** párbeszédpanelen adja meg a "ContactsController" a tartományvezérlő neveként.</span><span class="sxs-lookup"><span data-stu-id="82942-241">In hello **Add Controller** dialog box, enter "ContactsController" as your controller name.</span></span> <span data-ttu-id="82942-242">Válassza ki a "Ügyfelet (ContactManager.Models)" a hello **Model class**.</span><span class="sxs-lookup"><span data-stu-id="82942-242">Select "Contact (ContactManager.Models)" for hello **Model class**.</span></span>  <span data-ttu-id="82942-243">Tartsa hello alapértelmezett értéke hello **adatok környezetben osztály**.</span><span class="sxs-lookup"><span data-stu-id="82942-243">Keep hello default value for hello **Data context class**.</span></span> 
4. <span data-ttu-id="82942-244">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="82942-244">Click **Add**.</span></span>

### <a name="run-hello-application-locally"></a><span data-ttu-id="82942-245">Hello alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="82942-245">Run hello application locally</span></span>
1. <span data-ttu-id="82942-246">Nyomja le a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="82942-246">Press CTRL+F5 toorun hello application.</span></span>
   
    ![Index lap][intro001]
2. <span data-ttu-id="82942-248">Adja meg a partner, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="82942-248">Enter a contact and click **Add**.</span></span> <span data-ttu-id="82942-249">hello alkalmazás toohello kezdőlap adja vissza, és megjeleníti a megadott hello forduljon.</span><span class="sxs-lookup"><span data-stu-id="82942-249">hello app returns toohello home page and displays hello contact you entered.</span></span>
   
    ![A lista Tennivaló index lap][addwebapi004]
3. <span data-ttu-id="82942-251">Hello böngészőben hozzáfűzése **/api/contacts** toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="82942-251">In hello browser, append **/api/contacts** toohello URL.</span></span>
   
    <span data-ttu-id="82942-252">hello eredményül kapott URL-címet fog hasonlítani http://localhost:1234/api/Contacts címen.</span><span class="sxs-lookup"><span data-stu-id="82942-252">hello resulting URL will resemble http://localhost:1234/api/contacts.</span></span> <span data-ttu-id="82942-253">hello RESTful webes API, hozzáadott hello tárolt névjegyek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="82942-253">hello RESTful web API you added returns hello stored contacts.</span></span> <span data-ttu-id="82942-254">A Firefox és Chrome jelenít meg hello adatot XML formátumban.</span><span class="sxs-lookup"><span data-stu-id="82942-254">Firefox and Chrome will display hello data in XML format.</span></span>
   
    ![A lista Tennivaló index lap][rxFFchrome]

    <span data-ttu-id="82942-256">Internet Explorer tooopen kéri, vagy a hello partnerek mentése.</span><span class="sxs-lookup"><span data-stu-id="82942-256">IE will prompt you tooopen or save hello contacts.</span></span>

    ![Webes API-k mentése párbeszédpanel][addwebapi006]


    <span data-ttu-id="82942-258">Adott vissza a névjegyeket a Jegyzettömbben vagy a böngésző hello nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="82942-258">You can open hello returned contacts in notepad or a browser.</span></span>

    <span data-ttu-id="82942-259">A kimeneti például mobil weblap vagy az alkalmazás egy másik alkalmazás képes használni.</span><span class="sxs-lookup"><span data-stu-id="82942-259">This output can be consumed by another application such as mobile web page or application.</span></span>

    ![Webes API-k mentése párbeszédpanel][addwebapi007]

    <span data-ttu-id="82942-261">**Biztonsági figyelmeztetés**: ezen a ponton az alkalmazás nem biztonságos és sebezhető tooCSRF támadás.</span><span class="sxs-lookup"><span data-stu-id="82942-261">**Security Warning**: At this point, your application is insecure and vulnerable tooCSRF attack.</span></span> <span data-ttu-id="82942-262">Hello oktatóanyag későbbi részében megszüntetjük a biztonsági rés.</span><span class="sxs-lookup"><span data-stu-id="82942-262">Later in hello tutorial we will remove this vulnerability.</span></span> <span data-ttu-id="82942-263">További információ: [akadályozza meg, hogy többhelyes kérelem hamisítására (CSRF) támadások][prevent-csrf-attacks].</span><span class="sxs-lookup"><span data-stu-id="82942-263">For more information see [Preventing Cross-Site Request Forgery (CSRF) Attacks][prevent-csrf-attacks].</span></span>
## <a name="add-xsrf-protection"></a><span data-ttu-id="82942-264">XSRF védelem hozzáadása</span><span class="sxs-lookup"><span data-stu-id="82942-264">Add XSRF Protection</span></span>
<span data-ttu-id="82942-265">Webhelyközi kérések hamisítására (más néven XSRF vagy CSRF) webes állomásokon tárolt alkalmazásokhoz, amelyek rosszindulatú webhely befolyásolhatja hello közötti interakció ügyfél böngészője és egy webhelyet, a böngésző által megbízhatónak elleni támadásoknak.</span><span class="sxs-lookup"><span data-stu-id="82942-265">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted applications whereby a malicious website can influence hello interaction between a client browser and a website trusted by that browser.</span></span> <span data-ttu-id="82942-266">Ilyen támadások lehetséges válnak, mert a böngésző fog küldeni a hitelesítési tokenek automatikusan minden kérelem tooa webhellyel.</span><span class="sxs-lookup"><span data-stu-id="82942-266">These attacks are made possible because web browsers will send authentication tokens automatically with every request tooa website.</span></span> <span data-ttu-id="82942-267">hello kanonikus példa, hogy egy hitelesítési cookie-t, például az ASP. NET a az űrlapos hitelesítés jegy.</span><span class="sxs-lookup"><span data-stu-id="82942-267">hello canonical example is an authentication cookie, such as ASP.NET's Forms Authentication ticket.</span></span> <span data-ttu-id="82942-268">Azonban bármely állandó hitelesítési mechanizmus (például Windows-hitelesítést, a Basic, és így tovább) használó webhelyek célpontja is lehet ilyen támadások.</span><span class="sxs-lookup"><span data-stu-id="82942-268">However, websites which use any persistent authentication mechanism (such as Windows Authentication, Basic, and so forth) can be targeted by these attacks.</span></span>

<span data-ttu-id="82942-269">XSRF támadás nem egyezik a adathalász támadások.</span><span class="sxs-lookup"><span data-stu-id="82942-269">An XSRF attack is distinct from a phishing attack.</span></span> <span data-ttu-id="82942-270">Adathalász támadások közreműködése a hello áldozata.</span><span class="sxs-lookup"><span data-stu-id="82942-270">Phishing attacks require interaction from hello victim.</span></span> <span data-ttu-id="82942-271">Az egy adathalász támadások rosszindulatú webhely lesz utánozzák hello célwebhely, és hello áldozata magát van becsapni a bizalmas adatokat toohello támadó biztosítása.</span><span class="sxs-lookup"><span data-stu-id="82942-271">In a phishing attack, a malicious website will mimic hello target website, and hello victim is fooled into providing sensitive information toohello attacker.</span></span> <span data-ttu-id="82942-272">Az XSRF támadások nincs gyakran párbeszéd hello áldozata a szükséges.</span><span class="sxs-lookup"><span data-stu-id="82942-272">In an XSRF attack, there is often no interaction necessary from hello victim.</span></span> <span data-ttu-id="82942-273">Ehelyett hello támadó hello böngésző automatikusan küldése az összes releváns cookie-k toohello cél webhely megbízható függő.</span><span class="sxs-lookup"><span data-stu-id="82942-273">Rather, hello attacker is relying on hello browser automatically sending all relevant cookies toohello destination website.</span></span>

<span data-ttu-id="82942-274">További információkért lásd: hello [biztonsági projekt megnyitása webes alkalmazás](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span><span class="sxs-lookup"><span data-stu-id="82942-274">For more information, see hello [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).</span></span>

1. <span data-ttu-id="82942-275">A **Megoldáskezelőben**, jobb **ContactManager** projektre, kattintson **hozzáadása** majd **osztály**.</span><span class="sxs-lookup"><span data-stu-id="82942-275">In **Solution Explorer**, right **ContactManager** project and click **Add** and then click **Class**.</span></span>
2. <span data-ttu-id="82942-276">Nevű hello fájl *ValidateHttpAntiForgeryTokenAttribute.cs* , és adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="82942-276">Name hello file *ValidateHttpAntiForgeryTokenAttribute.cs* and add hello following code:</span></span>
   
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
3. <span data-ttu-id="82942-277">Adja hozzá a következő hello *használatával* utasítás toohello szerződések vezérlő így hozzáférés toohello **[ValidateHttpAntiForgeryToken]** attribútum.</span><span class="sxs-lookup"><span data-stu-id="82942-277">Add hello following *using* statement toohello contracts controller so you have access toohello **[ValidateHttpAntiForgeryToken]** attribute.</span></span>
   
        using ContactManager.Filters;
4. <span data-ttu-id="82942-278">Adja hozzá a hello **[ValidateHttpAntiForgeryToken]** toohello Post metódus a hello attribútum **ContactsController** tooprotect XSRF fenyegetések elleni védelmét.</span><span class="sxs-lookup"><span data-stu-id="82942-278">Add hello **[ValidateHttpAntiForgeryToken]** attribute toohello Post methods of hello **ContactsController** tooprotect it from XSRF threats.</span></span> <span data-ttu-id="82942-279">Toohello "PutContact", "PostContact" lesz hozzáadása és **DeleteContact** műveletmetódusokhoz.</span><span class="sxs-lookup"><span data-stu-id="82942-279">You will add it toohello "PutContact",  "PostContact" and **DeleteContact** action methods.</span></span>
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. <span data-ttu-id="82942-280">Frissítés hello *parancsfájlok* hello szakasza *Views\Home\Index.cshtml* tooinclude kód tooget hello XSRF jogkivonatok fájlt.</span><span class="sxs-lookup"><span data-stu-id="82942-280">Update hello *Scripts* section of hello *Views\Home\Index.cshtml* file tooinclude code tooget hello XSRF tokens.</span></span>
   
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

## <a name="publish-hello-application-update-tooazure-and-sql-database"></a><span data-ttu-id="82942-281">Hello alkalmazás frissítés tooAzure és SQL-adatbázis közzététele</span><span class="sxs-lookup"><span data-stu-id="82942-281">Publish hello application update tooAzure and SQL Database</span></span>
<span data-ttu-id="82942-282">toopublish hello alkalmazás, akkor ismételje meg a korábban követte hello eljárás.</span><span class="sxs-lookup"><span data-stu-id="82942-282">toopublish hello application, you repeat hello procedure you followed earlier.</span></span>

1. <span data-ttu-id="82942-283">A **Megoldáskezelőben**, hello projektben kattintson a jobb gombbal, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="82942-283">In **Solution Explorer**, right click hello project and select **Publish**.</span></span>
   
    ![Közzététel][rxP]
2. <span data-ttu-id="82942-285">Kattintson a hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="82942-285">Click hello **Settings** tab.</span></span>
3. <span data-ttu-id="82942-286">A **ContactsManagerContext(ContactsManagerContext)**, hello kattintson **v** ikon toochange *távoli kapcsolati karakterlánc* hello forduljon toohello kapcsolati karakterlánca az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="82942-286">Under **ContactsManagerContext(ContactsManagerContext)**, click hello **v** icon toochange *Remote connection string* toohello connection string for hello contact database.</span></span> <span data-ttu-id="82942-287">Kattintson a **ContactDB**.</span><span class="sxs-lookup"><span data-stu-id="82942-287">Click **ContactDB**.</span></span>
   
    ![Beállítások](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. <span data-ttu-id="82942-289">Hello jelölőnégyzetet a **hajtható végre Code First áttelepítést (fut az alkalmazás indítása)**.</span><span class="sxs-lookup"><span data-stu-id="82942-289">Check hello box for **Execute Code First Migrations (runs on application start)**.</span></span>
5. <span data-ttu-id="82942-290">Kattintson a **következő** majd **előzetes**.</span><span class="sxs-lookup"><span data-stu-id="82942-290">Click **Next** and then click **Preview**.</span></span> <span data-ttu-id="82942-291">A Visual Studio hello fájlok hozzáadott vagy frissített listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="82942-291">Visual Studio displays a list of hello files that will be added or updated.</span></span>
6. <span data-ttu-id="82942-292">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="82942-292">Click **Publish**.</span></span>
   <span data-ttu-id="82942-293">Hello központi telepítés befejezése után hello böngésző megnyitja toohello kezdőlap hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="82942-293">After hello deployment completes, hello browser opens toohello home page of hello application.</span></span>
   
    ![Index lap nincs ügyfelekkel][intro001]
   
    <span data-ttu-id="82942-295">Visual Studio hello folyamat automatikusan konfigurált hello kapcsolati karakterlánc közzététele a telepített hello *Web.config* fájl toopoint toohello SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="82942-295">hello Visual Studio publish process automatically configured hello connection string in hello deployed *Web.config* file toopoint toohello SQL database.</span></span> <span data-ttu-id="82942-296">Azt is konfigurálni Code First áttelepítést tooautomatically frissítési hello adatbázis toohello legújabb verziója hello első hello alkalmazása hello adatbázis hozzáfér a telepítés után.</span><span class="sxs-lookup"><span data-stu-id="82942-296">It also configured Code First Migrations tooautomatically upgrade hello database toohello latest version hello first time hello application accesses hello database after deployment.</span></span>
   
    <span data-ttu-id="82942-297">Ez a konfiguráció miatt Code First létrehozott hello adatbázist hello kód futtatásával a hello **kezdeti** korábban létrehozott osztályt.</span><span class="sxs-lookup"><span data-stu-id="82942-297">As a result of this configuration, Code First created hello database by running hello code in hello **Initial** class that you created earlier.</span></span> <span data-ttu-id="82942-298">Telepítés után ugyanúgy a hello első alkalommal hello próbált tooaccess hello adatbázis-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="82942-298">It did this hello first time hello application tried tooaccess hello database after deployment.</span></span>
7. <span data-ttu-id="82942-299">Adja meg a partner helyileg, hello app tooverify adatbázis telepítése sikeresen futtatta hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="82942-299">Enter a contact as you did when you ran hello app locally, tooverify that database deployment succeeded.</span></span>

<span data-ttu-id="82942-300">Amikor megjelenik az adott meg hello elem menti, és hello kapcsolattartási manager lapján jelenik meg, tudja, hogy hello adatbázisban lett tárolt.</span><span class="sxs-lookup"><span data-stu-id="82942-300">When you see that hello item you enter is saved and appears on hello contact manager page, you know that it has been stored in hello database.</span></span>

![Index lap ügyfelekkel][addwebapi004]

<span data-ttu-id="82942-302">hello alkalmazás innentől kezdve fut hello felhő, használja az SQL-adatbázis toostore az adatokat.</span><span class="sxs-lookup"><span data-stu-id="82942-302">hello application is now running in hello cloud, using SQL Database toostore its data.</span></span> <span data-ttu-id="82942-303">Miután befejezte a hello alkalmazás tesztelése az Azure-ban, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="82942-303">After you finish testing hello application in Azure, delete it.</span></span> <span data-ttu-id="82942-304">hello alkalmazás nyilvános, és nem rendelkezik olyan mechanizmus toolimit hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="82942-304">hello application is public and doesn't have a mechanism toolimit access.</span></span>

> [!NOTE]
> <span data-ttu-id="82942-305">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="82942-305">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="82942-306">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="82942-306">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="82942-307">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="82942-307">Next Steps</span></span>
<span data-ttu-id="82942-308">Egy másik módja toostore az Azure-alkalmazások adatai toouse az Azure storage, blobok és táblák hello formájában nem relációs adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="82942-308">Another way toostore data in an Azure application is toouse Azure storage, which provide non-relational data storage in hello form of blobs and tables.</span></span> <span data-ttu-id="82942-309">a következő hivatkozások hello nyújtanak további információt a Web API, az ASP.NET MVC és a Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="82942-309">hello following links provide more information on Web API, ASP.NET MVC and Window Azure.</span></span>

* <span data-ttu-id="82942-310">[Ismerkedés az Entity Framework MVC használatával][EFCodeFirstMVCTutorial]</span><span class="sxs-lookup"><span data-stu-id="82942-310">[Getting Started with Entity Framework using MVC][EFCodeFirstMVCTutorial]</span></span>
* [<span data-ttu-id="82942-311">Bevezetés tooASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="82942-311">Intro tooASP.NET MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="82942-312">Az első ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="82942-312">Your First ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [<span data-ttu-id="82942-313">Hibakeresési WAWS</span><span class="sxs-lookup"><span data-stu-id="82942-313">Debugging WAWS</span></span>](web-sites-dotnet-troubleshoot-visual-studio.md)

<span data-ttu-id="82942-314">Ez az oktatóanyag és hello mintaalkalmazás kiírt [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) Tom Dykstra és Barry Dorrans segítséget (Twitter [ @blowdart ](https://twitter.com/blowdart)).</span><span class="sxs-lookup"><span data-stu-id="82942-314">This tutorial and hello sample application was written by [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) with assistance from Tom Dykstra and Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)).</span></span> 

<span data-ttu-id="82942-315">Visszajelzés meghagyása mi tetszett és mi szeretné javítani, nem csak kapcsolatos hello oktatóanyag magát, hanem azt mutatja be, hogy hello termékekre vonatkozó toosee.</span><span class="sxs-lookup"><span data-stu-id="82942-315">Please leave feedback on what you liked or what you would like toosee improved, not only about hello tutorial itself but also about hello products that it demonstrates.</span></span> <span data-ttu-id="82942-316">Visszajelzése segít sorrendjének fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="82942-316">Your feedback will help us prioritize improvements.</span></span> <span data-ttu-id="82942-317">Folyamatban, különösen akkor érdekli automatizálással hello folyamat konfigurálása és telepítése hello tagsági adatbázisokból nincs mennyi iránt érdeklődik, keresése.</span><span class="sxs-lookup"><span data-stu-id="82942-317">We are especially interested in finding out how much interest there is in more automation for hello process of configuring and deploying hello membership database.</span></span> 

## <a name="whats-changed"></a><span data-ttu-id="82942-318">A változások</span><span class="sxs-lookup"><span data-stu-id="82942-318">What's changed</span></span>
* <span data-ttu-id="82942-319">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="82942-319">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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

