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
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Oktatóprogram: Egy több-bérlős adatbázist Entity Framework és sorszintű biztonsággal rendelkező webalkalmazás
Ez az oktatóanyag bemutatja, hogyan hozhat létre egy több-bérlős webes alkalmazást egy "[megosztott adatbázis, a közös séma](https://msdn.microsoft.com/library/aa479086.aspx)" bérlős modell használatával az Entity Framework és [sorszintű biztonság](https://msdn.microsoft.com/library/dn765131.aspx). Ebben a modellben egy adatbázis több bérlő adatait tartalmazza, és minden tábla minden sorára társítva van egy "bérlői azonosító". A sorszintű biztonságot (RLS), egy új funkció az Azure SQL Database, ezáltal megakadályozhatja, hogy a bérlők egymás adatokhoz hozzáférő szolgál. Ehhez csak egyetlen, kisebb módosítást az alkalmazáshoz. A bérlői access programot belül maga az adatbázis központosításával, RLS egyszerűbbé teszi az alkalmazás kódjában, és csökkenti a bérlők közötti adatok véletlen kiszivárgásának kockázata.

Kezdjük az egyszerű kapcsolatkezelő alkalmazás [a hitelesítés és SQL-adatbázis a ASP.NET MVP-alkalmazás létrehozása és telepítése az Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Jobb, az alkalmazás lehetővé teszi, hogy minden felhasználó (bérlő) található összes névjegyeket:

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Néhány kis módosításainak köszönhetően azt támogatni fogják a több-bérlős, ezáltal a felhasználók csak a hozzájuk tartozó ügyfelek láthatók.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>1. lépés: Az elfogó osztály hozzáadása az alkalmazásban a SESSION_CONTEXT beállítása
Meg kell győződnünk egy alkalmazás változás van. Minden alkalmazás felhasználóinak kapcsolódni az adatbázishoz, ugyanazt a kapcsolati karakterláncot (azaz azonos SQL-bejelentkezési) használatával, mert jelenleg nem úgy RLS házirend tudni, hogy mely felhasználói szűrhet kell. Ez a megközelítés nem nagyon gyakori a webes alkalmazások, mert lehetővé teszi, hogy hatékony kapcsolatkészlet, de az azt jelenti, hogy úgy is azonosíthatja az adatbázisból az aktuális alkalmazás felhasználójának kell. A megoldás az, hogy az alkalmazás a jelenlegi felhasználói azonosítóját, a kulcs-érték párból beállítani a [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) után azonnal kapcsolat létesítése előtt végrehajtása lekérdezéseket. SESSION_CONTEXT egy munkamenet-hatókörű kulcs-érték tárolóban, és az RLS-házirend a benne tárolt felhasználói azonosítóját fogja használni a jelenlegi felhasználó azonosítására.

Adunk hozzá egy [elfogó](https://msdn.microsoft.com/data/dn469464.aspx) (különösen a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), egy új szolgáltatás a Entity Framework (EF) 6, automatikusan beállítani a jelenlegi felhasználói azonosítóját a SESSION_CONTEXT egy T-SQL-utasítás végrehajtásával, amikor az EF megnyit egy kapcsolatot.

1. Nyissa meg a ContactManager projektet a Visual Studióban.
2. Kattintson a jobb gombbal a Solution Explorer modellek mappájára, és válassza a Hozzáadás > osztály.
3. Az új osztály "SessionContextInterceptor.cs" nevet, és kattintson a Hozzáadás gombra.
4. Cserélje le a SessionContextInterceptor.cs tartalmát az alábbira.

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

Ez az egyetlen alkalmazás módosítása kötelező. Lépjen tovább, és hozza létre, és tegye közzé az alkalmazást.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>2. lépés: Az adatbázis-séma UserId oszlop hozzáadása
A következő igazolnia kell a UserId oszlop hozzáadása a névjegyek tábla minden egyes sorára társítja a felhasználó (bérlő). A séma közvetlenül az adatbázisban, azt módosítja, így azt nem kell ahhoz, hogy ez a mező szerepeljen az EF adatmodell.

Kapcsolódni az adatbázishoz közvetlenül, SQL Server Management Studio vagy Visual Studio használatával, és hajthat végre a következő T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

A UserId oszlop hozzáadása a kapcsolatok táblához. Nvarchar(128) adattípus használatával felel meg a UserIds a AspNetUsers tábla tárolja, és azt, amely automatikusan be kell lennie a UserId SESSION_CONTEXT jelenleg tárolt újonnan beszúrt sorai a UserId alapértelmezett megkötés létrehozása.

Most már a tábla néz ki:

![SSMS kapcsolatok táblához](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Új partnerek létrehozásakor ezeket lesz automatikusan hozzárendeli a megfelelő felhasználói azonosítót. Bemutató céljára azonban nézzük hozzárendelése néhány a meglévő ügyfeleket egy meglévő felhasználó.

Ha létrehozta az alkalmazás már néhány felhasználó (pl. használatával helyi, Google vagy Facebook fiókok), a AspNetUsers táblázatban láthatja. Az alábbi képernyőképen látható nincs, amennyiben a csak egy felhasználó.

![SSMS AspNetUsers tábla](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Másolja le az azonosítót user1@contoso.com, majd illessze be az alábbi T-SQL-utasításban. Rendelje hozzá a három ügyfeleket a felhasználói azonosítóját e utasítás végrehajtásához.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>3. lépés: A sorszintű biztonsági szabályzat létrehozása az adatbázisban
Az utolsó lépést, ha egy olyan biztonsági házirendet, amely a felhasználói azonosítóját használja a SESSION_CONTEXT automatikusan a lekérdezések által visszaadott eredmények szűrésére.

Amíg az adatbázis továbbra is kapcsolódik, hajtsa végre a következő T-SQL:

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

Ez a kód három dolgot végzi. Először hoz létre egy új séma központosítása és az RLS-objektumokhoz való hozzáférés korlátozása az ajánlott eljárás. Ezt követően az alkalmazás létrehozza a predikátum függvény, amely egy sor UserId megegyezik a SESSION_CONTEXT UserId visszaadható "1". Végül létrehoz egy olyan biztonsági házirendet, amely ezt a funkciót, az ügyfelek táblán egy szűrési és a blokk predikátum. A szűrő predikátuma hatására a lekérdezések vissza csak azok a sorok, amelyekre az aktuális felhasználóhoz tartozik, és a blokkpredikátumok úgy működik, mint a biztonságos működés érdekében megakadályozhatja, hogy a véletlenül legalább egyszer beszúrni egy sort a megfelelő felhasználói alkalmazás.

Most futtassa az alkalmazást, és jelentkezzen be a user1@contoso.com. Ezt a felhasználót most csak azt hozzárendelt ügyfeleket a felhasználóazonosító korábbi látja:

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Ez további, próbálkozzon egy új felhasználó regisztrálása érvényesítéséhez. Nincs a névjegyek, mert nincs hozzárendelt őket látnak. Akkor hozzon létre egy új ügyfelet, ha rendeli hozzá őket, és csak akkor lesz látható legyen.

## <a name="next-steps"></a>Következő lépések
Ennyi az egész! A Contact Manager egyszerű webalkalmazást egy több-bérlős, ahol minden felhasználó rendelkezik-e a saját a ismerőslistájába egyik lett alakítva. A sorszintű biztonság, azt már elkerülhető a bérlői access programot az alkalmazás kódjában érvényesítési összetettségét. Az átláthatóság lehetővé teszi, hogy az alkalmazás a valódi üzleti probléma kezelésére összpontosítson, és csökkenti a kockázata, hogy véletlenül megakadályozására adatok, mint az alkalmazás csomagazonosítóját kódbázis növekszik.

Ez az oktatóanyag rendelkezik mondtam, hogy mit az RLS lehetséges. Például lehet rendelkezik több kifinomult vagy részletes access programot, és lehet tárolni a SESSION_CONTEXT nem csak az aktuális felhasználói azonosítóját. Akkor is lehet [RLS integrálása a rugalmas adatbázis eszközök klienskódtárak segítségével](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) egy kibővített adatrétegbeli több-bérlős szilánkok támogatásához.

Ezekkel a lehetőségekkel, túl is dolgozunk RLS még továbbfejlesztésében. Ha kérdése van, ötleteket vagy kíváncsi rá, dolgot tudassa velünk, a megjegyzések. Köszönjük visszajelzését!

