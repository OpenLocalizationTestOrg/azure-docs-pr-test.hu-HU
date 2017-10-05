---
title: "Oktatóprogram: Egy több-bérlős adatbázist Entity Framework és sorszintű biztonsággal rendelkező webalkalmazás"
description: "Ismerje meg, hogyan fejleszthet egy ASP.NET MVC 5 webalkalmazásnál egy több-bérlős SQL adatbázis backent, Entity Framework és a sorszintű biztonság."
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
ms.openlocfilehash: ba1bb3d84b462dfebbb2564569517d7336bf54fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a><span data-ttu-id="909a2-103">Oktatóprogram: Egy több-bérlős adatbázist Entity Framework és sorszintű biztonsággal rendelkező webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="909a2-103">Tutorial: Web app with a multi-tenant database using Entity Framework and Row-Level Security</span></span>
<span data-ttu-id="909a2-104">Ez az oktatóanyag bemutatja, hogyan hozhat létre egy több-bérlős webes alkalmazást egy "[megosztott adatbázis, a közös séma](https://msdn.microsoft.com/library/aa479086.aspx)" bérlős modell használatával az Entity Framework és [sorszintű biztonság](https://msdn.microsoft.com/library/dn765131.aspx).</span><span class="sxs-lookup"><span data-stu-id="909a2-104">This tutorial shows how to build a multi-tenant web app with a "[shared database, shared schema](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy model, using Entity Framework and [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx).</span></span> <span data-ttu-id="909a2-105">Ebben a modellben egy adatbázis több bérlő adatait tartalmazza, és minden tábla minden sorára társítva van egy "bérlői azonosító".</span><span class="sxs-lookup"><span data-stu-id="909a2-105">In this model, a single database contains data for many tenants, and each row in each table is associated with a "Tenant ID."</span></span> <span data-ttu-id="909a2-106">A sorszintű biztonságot (RLS), egy új funkció az Azure SQL Database, ezáltal megakadályozhatja, hogy a bérlők egymás adatokhoz hozzáférő szolgál.</span><span class="sxs-lookup"><span data-stu-id="909a2-106">Row-Level Security (RLS), a new feature for Azure SQL Database, is used to prevent tenants from accessing each other's data.</span></span> <span data-ttu-id="909a2-107">Ehhez csak egyetlen, kisebb módosítást az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="909a2-107">This requires just a single, small change to the application.</span></span> <span data-ttu-id="909a2-108">A bérlői access programot belül maga az adatbázis központosításával, RLS egyszerűbbé teszi az alkalmazás kódjában, és csökkenti a bérlők közötti adatok véletlen kiszivárgásának kockázata.</span><span class="sxs-lookup"><span data-stu-id="909a2-108">By centralizing the tenant access logic within the database itself, RLS simplifies the application code and reduces the risk of accidental data leakage between tenants.</span></span>

<span data-ttu-id="909a2-109">Kezdjük az egyszerű kapcsolatkezelő alkalmazás [a hitelesítés és SQL-adatbázis a ASP.NET MVP-alkalmazás létrehozása és telepítése az Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="909a2-109">Let's start with the simple Contact Manager application from [Create an ASP.NET MVP app with auth and SQL DB and deploy to Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span> <span data-ttu-id="909a2-110">Jobb, az alkalmazás lehetővé teszi, hogy minden felhasználó (bérlő) található összes névjegyeket:</span><span class="sxs-lookup"><span data-stu-id="909a2-110">Right now, the application allows all users (tenants) to see all contacts:</span></span>

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

<span data-ttu-id="909a2-112">Néhány kis módosításainak köszönhetően azt támogatni fogják a több-bérlős, ezáltal a felhasználók csak a hozzájuk tartozó ügyfelek láthatók.</span><span class="sxs-lookup"><span data-stu-id="909a2-112">With just a few small changes, we will add support for multi-tenancy, so that users are able to see only the contacts that belong to them.</span></span>

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a><span data-ttu-id="909a2-113">1. lépés: Az elfogó osztály hozzáadása az alkalmazásban a SESSION_CONTEXT beállítása</span><span class="sxs-lookup"><span data-stu-id="909a2-113">Step 1: Add an Interceptor class in the application to set the SESSION_CONTEXT</span></span>
<span data-ttu-id="909a2-114">Meg kell győződnünk egy alkalmazás változás van.</span><span class="sxs-lookup"><span data-stu-id="909a2-114">There is one application change we need to make.</span></span> <span data-ttu-id="909a2-115">Minden alkalmazás felhasználóinak kapcsolódni az adatbázishoz, ugyanazt a kapcsolati karakterláncot (azaz azonos SQL-bejelentkezési) használatával, mert jelenleg nem úgy RLS házirend tudni, hogy mely felhasználói szűrhet kell.</span><span class="sxs-lookup"><span data-stu-id="909a2-115">Because all application users connect to the database using the same connection string (i.e. same SQL login), there is currently no way for an RLS policy to know which user it should filter for.</span></span> <span data-ttu-id="909a2-116">Ez a megközelítés nem nagyon gyakori a webes alkalmazások, mert lehetővé teszi, hogy hatékony kapcsolatkészlet, de az azt jelenti, hogy úgy is azonosíthatja az adatbázisból az aktuális alkalmazás felhasználójának kell.</span><span class="sxs-lookup"><span data-stu-id="909a2-116">This approach is very common in web applications because it enables efficient connection pooling, but it means we need another way to identify the current application user within the database.</span></span> <span data-ttu-id="909a2-117">A megoldás az, hogy az alkalmazás a jelenlegi felhasználói azonosítóját, a kulcs-érték párból beállítani a [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) után azonnal kapcsolat létesítése előtt végrehajtása lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="909a2-117">The solution is to have the application set a key-value pair for the current UserId in the [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) immediately after opening a connection, before it executes any queries.</span></span> <span data-ttu-id="909a2-118">SESSION_CONTEXT egy munkamenet-hatókörű kulcs-érték tárolóban, és az RLS-házirend a benne tárolt felhasználói azonosítóját fogja használni a jelenlegi felhasználó azonosítására.</span><span class="sxs-lookup"><span data-stu-id="909a2-118">SESSION_CONTEXT is a session-scoped key-value store, and our RLS policy will use the UserId stored in it to identify the current user.</span></span>

<span data-ttu-id="909a2-119">Adunk hozzá egy [elfogó](https://msdn.microsoft.com/data/dn469464.aspx) (különösen a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), egy új szolgáltatás a Entity Framework (EF) 6, automatikusan beállítani a jelenlegi felhasználói azonosítóját a SESSION_CONTEXT egy T-SQL-utasítás végrehajtásával, amikor az EF megnyit egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="909a2-119">We will add an [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (in particular, a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), a new feature in Entity Framework (EF) 6, to automatically set the current UserId in the SESSION_CONTEXT by executing a T-SQL statement whenever EF opens a connection.</span></span>

1. <span data-ttu-id="909a2-120">Nyissa meg a ContactManager projektet a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="909a2-120">Open the ContactManager project in Visual Studio.</span></span>
2. <span data-ttu-id="909a2-121">Kattintson a jobb gombbal a Solution Explorer modellek mappájára, és válassza a Hozzáadás > osztály.</span><span class="sxs-lookup"><span data-stu-id="909a2-121">Right-click on the Models folder in the Solution Explorer, and choose Add > Class.</span></span>
3. <span data-ttu-id="909a2-122">Az új osztály "SessionContextInterceptor.cs" nevet, és kattintson a Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="909a2-122">Name the new class "SessionContextInterceptor.cs" and click Add.</span></span>
4. <span data-ttu-id="909a2-123">Cserélje le a SessionContextInterceptor.cs tartalmát az alábbira.</span><span class="sxs-lookup"><span data-stu-id="909a2-123">Replace the contents of SessionContextInterceptor.cs with the following code.</span></span>

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
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
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

<span data-ttu-id="909a2-124">Ez az egyetlen alkalmazás módosítása kötelező.</span><span class="sxs-lookup"><span data-stu-id="909a2-124">That's the only application change required.</span></span> <span data-ttu-id="909a2-125">Lépjen tovább, és hozza létre, és tegye közzé az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="909a2-125">Go ahead and build and publish the application.</span></span>

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a><span data-ttu-id="909a2-126">2. lépés: Az adatbázis-séma UserId oszlop hozzáadása</span><span class="sxs-lookup"><span data-stu-id="909a2-126">Step 2: Add a UserId column to the database schema</span></span>
<span data-ttu-id="909a2-127">A következő igazolnia kell a UserId oszlop hozzáadása a névjegyek tábla minden egyes sorára társítja a felhasználó (bérlő).</span><span class="sxs-lookup"><span data-stu-id="909a2-127">Next, we need to add a UserId column to the Contacts table to associate each row with a user (tenant).</span></span> <span data-ttu-id="909a2-128">A séma közvetlenül az adatbázisban, azt módosítja, így azt nem kell ahhoz, hogy ez a mező szerepeljen az EF adatmodell.</span><span class="sxs-lookup"><span data-stu-id="909a2-128">We will alter the schema directly in the database, so that we don't have to include this field in our EF data model.</span></span>

<span data-ttu-id="909a2-129">Kapcsolódni az adatbázishoz közvetlenül, SQL Server Management Studio vagy Visual Studio használatával, és hajthat végre a következő T-SQL:</span><span class="sxs-lookup"><span data-stu-id="909a2-129">Connect to the database directly, using either SQL Server Management Studio or Visual Studio, and then execute the following T-SQL:</span></span>

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

<span data-ttu-id="909a2-130">A UserId oszlop hozzáadása a kapcsolatok táblához.</span><span class="sxs-lookup"><span data-stu-id="909a2-130">This adds a UserId column to the Contacts table.</span></span> <span data-ttu-id="909a2-131">Nvarchar(128) adattípus használatával felel meg a UserIds a AspNetUsers tábla tárolja, és azt, amely automatikusan be kell lennie a UserId SESSION_CONTEXT jelenleg tárolt újonnan beszúrt sorai a UserId alapértelmezett megkötés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="909a2-131">We use the nvarchar(128) data type to match the UserIds stored in the AspNetUsers table, and we create a DEFAULT constraint that will automatically set the UserId for newly inserted rows to be the UserId currently stored in SESSION_CONTEXT.</span></span>

<span data-ttu-id="909a2-132">Most már a tábla néz ki:</span><span class="sxs-lookup"><span data-stu-id="909a2-132">Now the table looks like this:</span></span>

![SSMS kapcsolatok táblához](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

<span data-ttu-id="909a2-134">Új partnerek létrehozásakor ezeket lesz automatikusan hozzárendeli a megfelelő felhasználói azonosítót.</span><span class="sxs-lookup"><span data-stu-id="909a2-134">When new contacts are created, they'll automatically be assigned the correct UserId.</span></span> <span data-ttu-id="909a2-135">Bemutató céljára azonban nézzük hozzárendelése néhány a meglévő ügyfeleket egy meglévő felhasználó.</span><span class="sxs-lookup"><span data-stu-id="909a2-135">For demo purposes, however, let's assign a few of these existing contacts to an existing user.</span></span>

<span data-ttu-id="909a2-136">Ha létrehozta az alkalmazás már néhány felhasználó (pl. használatával helyi, Google vagy Facebook fiókok), a AspNetUsers táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="909a2-136">If you've created a few users in the application already (e.g., using local, Google, or Facebook accounts), you'll see them in the AspNetUsers table.</span></span> <span data-ttu-id="909a2-137">Az alábbi képernyőképen látható nincs, amennyiben a csak egy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="909a2-137">In the screenshot below, there is only one user so far.</span></span>

![SSMS AspNetUsers tábla](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

<span data-ttu-id="909a2-139">Másolja le az azonosítót user1@contoso.com, majd illessze be az alábbi T-SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="909a2-139">Copy the Id for user1@contoso.com, and paste it into the T-SQL statement below.</span></span> <span data-ttu-id="909a2-140">Rendelje hozzá a három ügyfeleket a felhasználói azonosítóját e utasítás végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="909a2-140">Execute this statement to associate three of the Contacts with this UserId.</span></span>

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a><span data-ttu-id="909a2-141">3. lépés: A sorszintű biztonsági szabályzat létrehozása az adatbázisban</span><span class="sxs-lookup"><span data-stu-id="909a2-141">Step 3: Create a Row-Level Security policy in the database</span></span>
<span data-ttu-id="909a2-142">Az utolsó lépést, ha egy olyan biztonsági házirendet, amely a felhasználói azonosítóját használja a SESSION_CONTEXT automatikusan a lekérdezések által visszaadott eredmények szűrésére.</span><span class="sxs-lookup"><span data-stu-id="909a2-142">The final step is to create a security policy that uses the UserId in SESSION_CONTEXT to automatically filter the results returned by queries.</span></span>

<span data-ttu-id="909a2-143">Amíg az adatbázis továbbra is kapcsolódik, hajtsa végre a következő T-SQL:</span><span class="sxs-lookup"><span data-stu-id="909a2-143">While still connected to the database, execute the following T-SQL:</span></span>

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

<span data-ttu-id="909a2-144">Ez a kód három dolgot végzi.</span><span class="sxs-lookup"><span data-stu-id="909a2-144">This code does three things.</span></span> <span data-ttu-id="909a2-145">Először hoz létre egy új séma központosítása és az RLS-objektumokhoz való hozzáférés korlátozása az ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="909a2-145">First, it creates a new schema as a best practice for centralizing and limiting access to the RLS objects.</span></span> <span data-ttu-id="909a2-146">Ezt követően az alkalmazás létrehozza a predikátum függvény, amely egy sor UserId megegyezik a SESSION_CONTEXT UserId visszaadható "1".</span><span class="sxs-lookup"><span data-stu-id="909a2-146">Next, it creates a predicate function that will return '1' when the UserId of a row matches the UserId in SESSION_CONTEXT.</span></span> <span data-ttu-id="909a2-147">Végül létrehoz egy olyan biztonsági házirendet, amely ezt a funkciót, az ügyfelek táblán egy szűrési és a blokk predikátum.</span><span class="sxs-lookup"><span data-stu-id="909a2-147">Finally, it creates a security policy that adds this function as both a filter and block predicate on the Contacts table.</span></span> <span data-ttu-id="909a2-148">A szűrő predikátuma hatására a lekérdezések vissza csak azok a sorok, amelyekre az aktuális felhasználóhoz tartozik, és a blokkpredikátumok úgy működik, mint a biztonságos működés érdekében megakadályozhatja, hogy a véletlenül legalább egyszer beszúrni egy sort a megfelelő felhasználói alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="909a2-148">The filter predicate causes queries to return only rows that belong to the current user, and the block predicate acts as a safeguard to prevent the application from ever accidentally inserting a row for the wrong user.</span></span>

<span data-ttu-id="909a2-149">Most futtassa az alkalmazást, és jelentkezzen be a user1@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="909a2-149">Now run the application, and sign in as user1@contoso.com.</span></span> <span data-ttu-id="909a2-150">Ezt a felhasználót most csak azt hozzárendelt ügyfeleket a felhasználóazonosító korábbi látja:</span><span class="sxs-lookup"><span data-stu-id="909a2-150">This user now sees only the Contacts we assigned to this UserId earlier:</span></span>

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

<span data-ttu-id="909a2-152">Ez további, próbálkozzon egy új felhasználó regisztrálása érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="909a2-152">To validate this further, try registering a new user.</span></span> <span data-ttu-id="909a2-153">Nincs a névjegyek, mert nincs hozzárendelt őket látnak.</span><span class="sxs-lookup"><span data-stu-id="909a2-153">They will see no contacts, because none have been assigned to them.</span></span> <span data-ttu-id="909a2-154">Akkor hozzon létre egy új ügyfelet, ha rendeli hozzá őket, és csak akkor lesz látható legyen.</span><span class="sxs-lookup"><span data-stu-id="909a2-154">If they create a new contact, it will be assigned to them, and only they will be able to see it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="909a2-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="909a2-155">Next steps</span></span>
<span data-ttu-id="909a2-156">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="909a2-156">That's it!</span></span> <span data-ttu-id="909a2-157">A Contact Manager egyszerű webalkalmazást egy több-bérlős, ahol minden felhasználó rendelkezik-e a saját a ismerőslistájába egyik lett alakítva.</span><span class="sxs-lookup"><span data-stu-id="909a2-157">The simple Contact Manager web app has been converted into a multi-tenant one where each user has its own contact list.</span></span> <span data-ttu-id="909a2-158">A sorszintű biztonság, azt már elkerülhető a bérlői access programot az alkalmazás kódjában érvényesítési összetettségét.</span><span class="sxs-lookup"><span data-stu-id="909a2-158">By using Row-Level Security, we've avoided the complexity of enforcing tenant access logic in our application code.</span></span> <span data-ttu-id="909a2-159">Az átláthatóság lehetővé teszi, hogy az alkalmazás a valódi üzleti probléma kezelésére összpontosítson, és csökkenti a kockázata, hogy véletlenül megakadályozására adatok, mint az alkalmazás csomagazonosítóját kódbázis növekszik.</span><span class="sxs-lookup"><span data-stu-id="909a2-159">This transparency allows the application to focus on the real business problem at hand, and it also reduces the risk of accidentally leaking data as the application's codebase grows.</span></span>

<span data-ttu-id="909a2-160">Ez az oktatóanyag rendelkezik mondtam, hogy mit az RLS lehetséges.</span><span class="sxs-lookup"><span data-stu-id="909a2-160">This tutorial has only scratched the surface of what's possible with RLS.</span></span> <span data-ttu-id="909a2-161">Például lehet rendelkezik több kifinomult vagy részletes access programot, és lehet tárolni a SESSION_CONTEXT nem csak az aktuális felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="909a2-161">For instance, it's possible to have more sophisticated or granular access logic, and it's possible to store more than just the current UserId in the SESSION_CONTEXT.</span></span> <span data-ttu-id="909a2-162">Akkor is lehet [RLS integrálása a rugalmas adatbázis eszközök klienskódtárak segítségével](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) egy kibővített adatrétegbeli több-bérlős szilánkok támogatásához.</span><span class="sxs-lookup"><span data-stu-id="909a2-162">It's also possible to [integrate RLS with the elastic database tools client libraries](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) to support multi-tenant shards in a scale-out data tier.</span></span>

<span data-ttu-id="909a2-163">Ezekkel a lehetőségekkel, túl is dolgozunk RLS még továbbfejlesztésében.</span><span class="sxs-lookup"><span data-stu-id="909a2-163">Beyond these possibilities, we're also working to make RLS even better.</span></span> <span data-ttu-id="909a2-164">Ha kérdése van, ötleteket vagy kíváncsi rá, dolgot tudassa velünk, a megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="909a2-164">If you have any questions, ideas, or things you'd like to see, please let us know in the comments.</span></span> <span data-ttu-id="909a2-165">Köszönjük visszajelzését!</span><span class="sxs-lookup"><span data-stu-id="909a2-165">We appreciate your feedback!</span></span>

