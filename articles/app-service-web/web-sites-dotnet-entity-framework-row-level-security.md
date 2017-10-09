---
title: "Oktatóprogram: Egy több-bérlős adatbázist Entity Framework és sorszintű biztonsággal rendelkező webalkalmazás"
description: "Ismerje meg, hogyan toodevelop egy ASP.NET MVC 5 webalkalmazás egy több-bérlős SQL adatbázis backent, Entity Framework és sorszintű biztonsággal rendelkező."
metakeywords: azure asp.net mvc entity framework multi tenant row level security rls sql database
services: app-service\web
documentationcenter: .net
manager: jeffreyg
author: tmullaney
ms.assetid: 8fdc47a5-6fc3-4d29-ab6a-33e79f50699f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/25/2016
ms.author: thmullan
ms.openlocfilehash: 1b715e01807032c3f6497c374ce427dd762af141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a><span data-ttu-id="ff545-103">Oktatóprogram: Egy több-bérlős adatbázist Entity Framework és sorszintű biztonsággal rendelkező webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="ff545-103">Tutorial: Web app with a multi-tenant database using Entity Framework and Row-Level Security</span></span>
<span data-ttu-id="ff545-104">Ez az oktatóanyag bemutatja, hogyan toobuild egy több-bérlős webes alkalmazást egy "[megosztott adatbázis, a közös séma](https://msdn.microsoft.com/library/aa479086.aspx)" bérlős modell használatával az Entity Framework és [sorszintű biztonság](https://msdn.microsoft.com/library/dn765131.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff545-104">This tutorial shows how toobuild a multi-tenant web app with a "[shared database, shared schema](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy model, using Entity Framework and [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx).</span></span> <span data-ttu-id="ff545-105">Ebben a modellben egy adatbázis több bérlő adatait tartalmazza, és minden tábla minden sorára társítva van egy "bérlői azonosító".</span><span class="sxs-lookup"><span data-stu-id="ff545-105">In this model, a single database contains data for many tenants, and each row in each table is associated with a "Tenant ID."</span></span> <span data-ttu-id="ff545-106">A sorszintű biztonságot (RLS), egy új funkció az Azure SQL Database esetén használt tooprevent bérlők egymás adatok hozzáférése.</span><span class="sxs-lookup"><span data-stu-id="ff545-106">Row-Level Security (RLS), a new feature for Azure SQL Database, is used tooprevent tenants from accessing each other's data.</span></span> <span data-ttu-id="ff545-107">Ehhez csak egyetlen, kisebb módosítást toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ff545-107">This requires just a single, small change toohello application.</span></span> <span data-ttu-id="ff545-108">Központosítása hello bérlői hozzáférési logika hello adatbázis magát RLS hello alkalmazáskód leegyszerűsíti, és csökkenti a bérlők közötti adatok véletlen kiszivárgásának kockázata hello.</span><span class="sxs-lookup"><span data-stu-id="ff545-108">By centralizing hello tenant access logic within hello database itself, RLS simplifies hello application code and reduces hello risk of accidental data leakage between tenants.</span></span>

<span data-ttu-id="ff545-109">Kezdjük hello egyszerű kapcsolatkezelő alkalmazás [ASP.NET MVP-alkalmazás létrehozása a hitelesítés és SQL-adatbázis, és telepítheti az App Service tooAzure](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ff545-109">Let's start with hello simple Contact Manager application from [Create an ASP.NET MVP app with auth and SQL DB and deploy tooAzure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span> <span data-ttu-id="ff545-110">Jobb most hello alkalmazás lehetővé teszi, hogy minden felhasználó (bérlő) toosee összes névjegy:</span><span class="sxs-lookup"><span data-stu-id="ff545-110">Right now, hello application allows all users (tenants) toosee all contacts:</span></span>

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

<span data-ttu-id="ff545-112">Néhány kis módosításainak köszönhetően azt támogatni fogják a több-bérlős, ezáltal a felhasználók képesek toosee csak hello ügyfelek toothem tartoznak.</span><span class="sxs-lookup"><span data-stu-id="ff545-112">With just a few small changes, we will add support for multi-tenancy, so that users are able toosee only hello contacts that belong toothem.</span></span>

## <a name="step-1-add-an-interceptor-class-in-hello-application-tooset-hello-sessioncontext"></a><span data-ttu-id="ff545-113">1. lépés: Az elfogó osztály hozzáadása a hello alkalmazás tooset hello SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="ff545-113">Step 1: Add an Interceptor class in hello application tooset hello SESSION_CONTEXT</span></span>
<span data-ttu-id="ff545-114">Egy alkalmazás változás toomake van szükségünk van.</span><span class="sxs-lookup"><span data-stu-id="ff545-114">There is one application change we need toomake.</span></span> <span data-ttu-id="ff545-115">Minden alkalmazás felhasználóinak csatlakozás toohello adatbázis használatával hello ugyanazt a kapcsolati karakterláncot (azaz azonos SQL-bejelentkezési), jelenleg nincs mód az RLS házirend tooknow felhasználói azt kell szűrhet az.</span><span class="sxs-lookup"><span data-stu-id="ff545-115">Because all application users connect toohello database using hello same connection string (i.e. same SQL login), there is currently no way for an RLS policy tooknow which user it should filter for.</span></span> <span data-ttu-id="ff545-116">Ez a megközelítés nem nagyon gyakori a webes alkalmazások, mert lehetővé teszi, hogy hatékony kapcsolatkészlet, de az azt jelenti, hogy egy másik módja tooidentify hello aktuális alkalmazás felhasználójának hello adatbázison belül kell.</span><span class="sxs-lookup"><span data-stu-id="ff545-116">This approach is very common in web applications because it enables efficient connection pooling, but it means we need another way tooidentify hello current application user within hello database.</span></span> <span data-ttu-id="ff545-117">hello megoldás toohave hello alkalmazás beállítása egy kulcs-érték párt a hello aktuális UserId hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) után azonnal kapcsolat létesítése előtt végrehajtása lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="ff545-117">hello solution is toohave hello application set a key-value pair for hello current UserId in hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) immediately after opening a connection, before it executes any queries.</span></span> <span data-ttu-id="ff545-118">SESSION_CONTEXT egy munkamenet-hatókörű kulcs-érték tárolóban, és az RLS házirend fogja használni a felhasználói azonosítóját a benne tárolt hello tooidentify hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ff545-118">SESSION_CONTEXT is a session-scoped key-value store, and our RLS policy will use hello UserId stored in it tooidentify hello current user.</span></span>

<span data-ttu-id="ff545-119">Adunk hozzá egy [elfogó](https://msdn.microsoft.com/data/dn469464.aspx) (különösen a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), egy új szolgáltatás a Entity Framework (EF) 6, tooautomatically set hello hello SESSION_CONTEXT az aktuális felhasználói azonosítóját a következő futtatásával egy T-SQL utasítást, amikor EF megnyit egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ff545-119">We will add an [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (in particular, a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), a new feature in Entity Framework (EF) 6, tooautomatically set hello current UserId in hello SESSION_CONTEXT by executing a T-SQL statement whenever EF opens a connection.</span></span>

1. <span data-ttu-id="ff545-120">Nyissa meg a hello ContactManager projektet a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ff545-120">Open hello ContactManager project in Visual Studio.</span></span>
2. <span data-ttu-id="ff545-121">Kattintson a jobb gombbal a Solution Explorer hello hello Models mappát, és válassza a Hozzáadás > osztály.</span><span class="sxs-lookup"><span data-stu-id="ff545-121">Right-click on hello Models folder in hello Solution Explorer, and choose Add > Class.</span></span>
3. <span data-ttu-id="ff545-122">Új osztály hello "SessionContextInterceptor.cs" nevet, és kattintson a Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="ff545-122">Name hello new class "SessionContextInterceptor.cs" and click Add.</span></span>
4. <span data-ttu-id="ff545-123">Cserélje le a következő kód hello SessionContextInterceptor.cs hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="ff545-123">Replace hello contents of SessionContextInterceptor.cs with hello following code.</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT toocurrent UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }

        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

<span data-ttu-id="ff545-124">Ez az hello egyetlen alkalmazás módosítása kötelező.</span><span class="sxs-lookup"><span data-stu-id="ff545-124">That's hello only application change required.</span></span> <span data-ttu-id="ff545-125">Lépjen tovább, és hozza létre, és hello alkalmazás közzététele.</span><span class="sxs-lookup"><span data-stu-id="ff545-125">Go ahead and build and publish hello application.</span></span>

## <a name="step-2-add-a-userid-column-toohello-database-schema"></a><span data-ttu-id="ff545-126">2. lépés: UserId oszlop toohello adatbázisséma hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ff545-126">Step 2: Add a UserId column toohello database schema</span></span>
<span data-ttu-id="ff545-127">A következő igazolnia kell a UserId oszlop toohello névjegyek tábla tooassociate tooadd minden egyes sorára a felhasználó (bérlő).</span><span class="sxs-lookup"><span data-stu-id="ff545-127">Next, we need tooadd a UserId column toohello Contacts table tooassociate each row with a user (tenant).</span></span> <span data-ttu-id="ff545-128">Közvetlenül a hello adatbázis hello séma azt módosítja, így nem tudunk tooinclude ebben a mezőben az EF adatmodell.</span><span class="sxs-lookup"><span data-stu-id="ff545-128">We will alter hello schema directly in hello database, so that we don't have tooinclude this field in our EF data model.</span></span>

<span data-ttu-id="ff545-129">Toohello adatbázis közvetlen csatlakozás SQL Server Management Studio vagy Visual Studio használatával, és majd hajtsa végre a következő T-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="ff545-129">Connect toohello database directly, using either SQL Server Management Studio or Visual Studio, and then execute hello following T-SQL:</span></span>

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

<span data-ttu-id="ff545-130">Ez biztosítja a UserId oszlop toohello névjegyek tábla.</span><span class="sxs-lookup"><span data-stu-id="ff545-130">This adds a UserId column toohello Contacts table.</span></span> <span data-ttu-id="ff545-131">Hello nvarchar(128) adatok típusa toomatch hello UserIds hello AspNetUsers táblában tárolt használjuk, és azt, amely az újonnan behelyezett sorok toobe hello SESSION_CONTEXT jelenleg tárolt UserId UserId hello automatikusan be alapértelmezett megkötés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ff545-131">We use hello nvarchar(128) data type toomatch hello UserIds stored in hello AspNetUsers table, and we create a DEFAULT constraint that will automatically set hello UserId for newly inserted rows toobe hello UserId currently stored in SESSION_CONTEXT.</span></span>

<span data-ttu-id="ff545-132">Most hello tábla néz ki:</span><span class="sxs-lookup"><span data-stu-id="ff545-132">Now hello table looks like this:</span></span>

![SSMS kapcsolatok táblához](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

<span data-ttu-id="ff545-134">Új partnerek létrehozásakor ezeket lesz automatikusan hozzárendeli a hello javítsa ki a felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ff545-134">When new contacts are created, they'll automatically be assigned hello correct UserId.</span></span> <span data-ttu-id="ff545-135">Bemutató céljára azonban nézzük rendelje hozzá a meglévő ügyfelek tooan felhasználó meglévő néhány.</span><span class="sxs-lookup"><span data-stu-id="ff545-135">For demo purposes, however, let's assign a few of these existing contacts tooan existing user.</span></span>

<span data-ttu-id="ff545-136">Ha létrehozta az alkalmazás már hello néhány felhasználó (pl. használatával helyi, Google vagy Facebook fiókok), hello AspNetUsers táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="ff545-136">If you've created a few users in hello application already (e.g., using local, Google, or Facebook accounts), you'll see them in hello AspNetUsers table.</span></span> <span data-ttu-id="ff545-137">Hello a képernyőfelvételen látható az alábbi nincs, amennyiben a csak egy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ff545-137">In hello screenshot below, there is only one user so far.</span></span>

![SSMS AspNetUsers tábla](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

<span data-ttu-id="ff545-139">Másolás hello azonosítója a user1@contoso.com, és illessze be az alábbi hello T-SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="ff545-139">Copy hello Id for user1@contoso.com, and paste it into hello T-SQL statement below.</span></span> <span data-ttu-id="ff545-140">Ez a felhasználóazonosító hello kapcsolattartás utasítás tooassociate három hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="ff545-140">Execute this statement tooassociate three of hello Contacts with this UserId.</span></span>

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-hello-database"></a><span data-ttu-id="ff545-141">3. lépés: A sorszintű biztonsági szabályzat létrehozása a hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="ff545-141">Step 3: Create a Row-Level Security policy in hello database</span></span>
<span data-ttu-id="ff545-142">utolsó lépésként hello toocreate hello UserId SESSION_CONTEXT tooautomatically szűrő hello eredmények lekérdezések által visszaadott használó biztonsági házirend.</span><span class="sxs-lookup"><span data-stu-id="ff545-142">hello final step is toocreate a security policy that uses hello UserId in SESSION_CONTEXT tooautomatically filter hello results returned by queries.</span></span>

<span data-ttu-id="ff545-143">Közben továbbra is csatlakoztatott toohello adatbázis hajtható végre a következő T-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="ff545-143">While still connected toohello database, execute hello following T-SQL:</span></span>

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

<span data-ttu-id="ff545-144">Ez a kód három dolgot végzi.</span><span class="sxs-lookup"><span data-stu-id="ff545-144">This code does three things.</span></span> <span data-ttu-id="ff545-145">Először hoz létre egy új séma központosítását és hozzáférési toohello RLS objektumok korlátozza az ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="ff545-145">First, it creates a new schema as a best practice for centralizing and limiting access toohello RLS objects.</span></span> <span data-ttu-id="ff545-146">Ezután létrehoz egy predikátum függvény, amely egy sor UserId hello megegyezik a SESSION_CONTEXT UserId hello visszaadható "1".</span><span class="sxs-lookup"><span data-stu-id="ff545-146">Next, it creates a predicate function that will return '1' when hello UserId of a row matches hello UserId in SESSION_CONTEXT.</span></span> <span data-ttu-id="ff545-147">Végül létrehoz egy olyan biztonsági házirendet, amely felveszi ezt a funkciót egy szűrési és a blokk predikátum hello névjegyek táblán.</span><span class="sxs-lookup"><span data-stu-id="ff545-147">Finally, it creates a security policy that adds this function as both a filter and block predicate on hello Contacts table.</span></span> <span data-ttu-id="ff545-148">hello szűrőpredikátum hatására a lekérdezések tooreturn csak azon sorait, amelyek toohello aktuális felhasználó tartozik, és hello blokkpredikátumok úgy működik, mint a biztonságos működés érdekében tooprevent hello alkalmazást legalább egyszer véletlenül beszúrni egy sort a megfelelő felhasználói hello.</span><span class="sxs-lookup"><span data-stu-id="ff545-148">hello filter predicate causes queries tooreturn only rows that belong toohello current user, and hello block predicate acts as a safeguard tooprevent hello application from ever accidentally inserting a row for hello wrong user.</span></span>

<span data-ttu-id="ff545-149">Most futtassa hello alkalmazást, és jelentkezzen be a user1@contoso.com. Ez most felhasználónál csak UserId toothis korábban hozzárendelt igazolnia hello ügyfelek:</span><span class="sxs-lookup"><span data-stu-id="ff545-149">Now run hello application, and sign in as user1@contoso.com. This user now sees only hello Contacts we assigned toothis UserId earlier:</span></span>

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

<span data-ttu-id="ff545-151">toovalidate ez további, próbálkozzon egy új felhasználó regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="ff545-151">toovalidate this further, try registering a new user.</span></span> <span data-ttu-id="ff545-152">Nincs a névjegyek, mert nincs hozzárendelt toothem látnak.</span><span class="sxs-lookup"><span data-stu-id="ff545-152">They will see no contacts, because none have been assigned toothem.</span></span> <span data-ttu-id="ff545-153">Ha hoznak létre új ügyfél, a rendszer hozzárendel toothem, és csak akkor lesz képes toosee azt.</span><span class="sxs-lookup"><span data-stu-id="ff545-153">If they create a new contact, it will be assigned toothem, and only they will be able toosee it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff545-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff545-154">Next steps</span></span>
<span data-ttu-id="ff545-155">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="ff545-155">That's it!</span></span> <span data-ttu-id="ff545-156">egy több-bérlős, ahol minden felhasználó rendelkezik-e a saját a ismerőslistájába egy egyszerű forduljon Manager webalkalmazás hello lett alakítva.</span><span class="sxs-lookup"><span data-stu-id="ff545-156">hello simple Contact Manager web app has been converted into a multi-tenant one where each user has its own contact list.</span></span> <span data-ttu-id="ff545-157">Használ sorszintű biztonsággal, azt is elkerülhető bérlői access programot az alkalmazás kódjában érvényesítési hello összetettségét.</span><span class="sxs-lookup"><span data-stu-id="ff545-157">By using Row-Level Security, we've avoided hello complexity of enforcing tenant access logic in our application code.</span></span> <span data-ttu-id="ff545-158">Az átláthatóság hello alkalmazás toofocus lehetővé teszi az elvégzendő hello valódi üzleti probléma, és csökkenti véletlenül megakadályozására adatok, mint hello alkalmazás csomagazonosítóját kódbázis hello kockázatát növekszik.</span><span class="sxs-lookup"><span data-stu-id="ff545-158">This transparency allows hello application toofocus on hello real business problem at hand, and it also reduces hello risk of accidentally leaking data as hello application's codebase grows.</span></span>

<span data-ttu-id="ff545-159">Ez az oktatóanyag csak karcolva hello felületét Mi az az RLS lehetséges rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ff545-159">This tutorial has only scratched hello surface of what's possible with RLS.</span></span> <span data-ttu-id="ff545-160">Például lehetséges toohave kifinomultabb vagy részletes access programot, és azok lehetséges toostore csupán hello hello SESSION_CONTEXT az aktuális felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ff545-160">For instance, it's possible toohave more sophisticated or granular access logic, and it's possible toostore more than just hello current UserId in hello SESSION_CONTEXT.</span></span> <span data-ttu-id="ff545-161">Lehetőség arra is túl[RLS integrálása hello rugalmas adatbázis eszközök ügyféloldali kódtáraknál](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport több-bérlős szilánkok a kibővített adatok rétegben.</span><span class="sxs-lookup"><span data-stu-id="ff545-161">It's also possible too[integrate RLS with hello elastic database tools client libraries](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport multi-tenant shards in a scale-out data tier.</span></span>

<span data-ttu-id="ff545-162">Ezekkel a lehetőségekkel, túl toomake még élvezetesebbé RLS is dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="ff545-162">Beyond these possibilities, we're also working toomake RLS even better.</span></span> <span data-ttu-id="ff545-163">Ha kérdése van, ötleteket vagy toosee, milyen dolgot tudassa velünk hello megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="ff545-163">If you have any questions, ideas, or things you'd like toosee, please let us know in hello comments.</span></span> <span data-ttu-id="ff545-164">Köszönjük visszajelzését!</span><span class="sxs-lookup"><span data-stu-id="ff545-164">We appreciate your feedback!</span></span>

