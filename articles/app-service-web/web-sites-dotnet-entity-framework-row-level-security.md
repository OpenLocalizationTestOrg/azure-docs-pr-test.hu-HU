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
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Oktatóprogram: Egy több-bérlős adatbázist Entity Framework és sorszintű biztonsággal rendelkező webalkalmazás
Ez az oktatóanyag bemutatja, hogyan toobuild egy több-bérlős webes alkalmazást egy "[megosztott adatbázis, a közös séma](https://msdn.microsoft.com/library/aa479086.aspx)" bérlős modell használatával az Entity Framework és [sorszintű biztonság](https://msdn.microsoft.com/library/dn765131.aspx). Ebben a modellben egy adatbázis több bérlő adatait tartalmazza, és minden tábla minden sorára társítva van egy "bérlői azonosító". A sorszintű biztonságot (RLS), egy új funkció az Azure SQL Database esetén használt tooprevent bérlők egymás adatok hozzáférése. Ehhez csak egyetlen, kisebb módosítást toohello alkalmazást. Központosítása hello bérlői hozzáférési logika hello adatbázis magát RLS hello alkalmazáskód leegyszerűsíti, és csökkenti a bérlők közötti adatok véletlen kiszivárgásának kockázata hello.

Kezdjük hello egyszerű kapcsolatkezelő alkalmazás [ASP.NET MVP-alkalmazás létrehozása a hitelesítés és SQL-adatbázis, és telepítheti az App Service tooAzure](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Jobb most hello alkalmazás lehetővé teszi, hogy minden felhasználó (bérlő) toosee összes névjegy:

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Néhány kis módosításainak köszönhetően azt támogatni fogják a több-bérlős, ezáltal a felhasználók képesek toosee csak hello ügyfelek toothem tartoznak.

## <a name="step-1-add-an-interceptor-class-in-hello-application-tooset-hello-sessioncontext"></a>1. lépés: Az elfogó osztály hozzáadása a hello alkalmazás tooset hello SESSION_CONTEXT
Egy alkalmazás változás toomake van szükségünk van. Minden alkalmazás felhasználóinak csatlakozás toohello adatbázis használatával hello ugyanazt a kapcsolati karakterláncot (azaz azonos SQL-bejelentkezési), jelenleg nincs mód az RLS házirend tooknow felhasználói azt kell szűrhet az. Ez a megközelítés nem nagyon gyakori a webes alkalmazások, mert lehetővé teszi, hogy hatékony kapcsolatkészlet, de az azt jelenti, hogy egy másik módja tooidentify hello aktuális alkalmazás felhasználójának hello adatbázison belül kell. hello megoldás toohave hello alkalmazás beállítása egy kulcs-érték párt a hello aktuális UserId hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) után azonnal kapcsolat létesítése előtt végrehajtása lekérdezéseket. SESSION_CONTEXT egy munkamenet-hatókörű kulcs-érték tárolóban, és az RLS házirend fogja használni a felhasználói azonosítóját a benne tárolt hello tooidentify hello aktuális felhasználó.

Adunk hozzá egy [elfogó](https://msdn.microsoft.com/data/dn469464.aspx) (különösen a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), egy új szolgáltatás a Entity Framework (EF) 6, tooautomatically set hello hello SESSION_CONTEXT az aktuális felhasználói azonosítóját a következő futtatásával egy T-SQL utasítást, amikor EF megnyit egy kapcsolatot.

1. Nyissa meg a hello ContactManager projektet a Visual Studióban.
2. Kattintson a jobb gombbal a Solution Explorer hello hello Models mappát, és válassza a Hozzáadás > osztály.
3. Új osztály hello "SessionContextInterceptor.cs" nevet, és kattintson a Hozzáadás gombra.
4. Cserélje le a következő kód hello SessionContextInterceptor.cs hello tartalmát.

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

Ez az hello egyetlen alkalmazás módosítása kötelező. Lépjen tovább, és hozza létre, és hello alkalmazás közzététele.

## <a name="step-2-add-a-userid-column-toohello-database-schema"></a>2. lépés: UserId oszlop toohello adatbázisséma hozzáadása
A következő igazolnia kell a UserId oszlop toohello névjegyek tábla tooassociate tooadd minden egyes sorára a felhasználó (bérlő). Közvetlenül a hello adatbázis hello séma azt módosítja, így nem tudunk tooinclude ebben a mezőben az EF adatmodell.

Toohello adatbázis közvetlen csatlakozás SQL Server Management Studio vagy Visual Studio használatával, és majd hajtsa végre a következő T-SQL hello:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Ez biztosítja a UserId oszlop toohello névjegyek tábla. Hello nvarchar(128) adatok típusa toomatch hello UserIds hello AspNetUsers táblában tárolt használjuk, és azt, amely az újonnan behelyezett sorok toobe hello SESSION_CONTEXT jelenleg tárolt UserId UserId hello automatikusan be alapértelmezett megkötés létrehozása.

Most hello tábla néz ki:

![SSMS kapcsolatok táblához](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Új partnerek létrehozásakor ezeket lesz automatikusan hozzárendeli a hello javítsa ki a felhasználói azonosítóját. Bemutató céljára azonban nézzük rendelje hozzá a meglévő ügyfelek tooan felhasználó meglévő néhány.

Ha létrehozta az alkalmazás már hello néhány felhasználó (pl. használatával helyi, Google vagy Facebook fiókok), hello AspNetUsers táblázatban láthatja. Hello a képernyőfelvételen látható az alábbi nincs, amennyiben a csak egy felhasználó.

![SSMS AspNetUsers tábla](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Másolás hello azonosítója a user1@contoso.com, és illessze be az alábbi hello T-SQL-utasításban. Ez a felhasználóazonosító hello kapcsolattartás utasítás tooassociate három hajtható végre.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-hello-database"></a>3. lépés: A sorszintű biztonsági szabályzat létrehozása a hello adatbázis
utolsó lépésként hello toocreate hello UserId SESSION_CONTEXT tooautomatically szűrő hello eredmények lekérdezések által visszaadott használó biztonsági házirend.

Közben továbbra is csatlakoztatott toohello adatbázis hajtható végre a következő T-SQL hello:

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

Ez a kód három dolgot végzi. Először hoz létre egy új séma központosítását és hozzáférési toohello RLS objektumok korlátozza az ajánlott eljárás. Ezután létrehoz egy predikátum függvény, amely egy sor UserId hello megegyezik a SESSION_CONTEXT UserId hello visszaadható "1". Végül létrehoz egy olyan biztonsági házirendet, amely felveszi ezt a funkciót egy szűrési és a blokk predikátum hello névjegyek táblán. hello szűrőpredikátum hatására a lekérdezések tooreturn csak azon sorait, amelyek toohello aktuális felhasználó tartozik, és hello blokkpredikátumok úgy működik, mint a biztonságos működés érdekében tooprevent hello alkalmazást legalább egyszer véletlenül beszúrni egy sort a megfelelő felhasználói hello.

Most futtassa hello alkalmazást, és jelentkezzen be a user1@contoso.com. Ez most felhasználónál csak UserId toothis korábban hozzárendelt igazolnia hello ügyfelek:

![Ahhoz, hogy a RLS kapcsolatkezelő alkalmazás](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

toovalidate ez további, próbálkozzon egy új felhasználó regisztrálása. Nincs a névjegyek, mert nincs hozzárendelt toothem látnak. Ha hoznak létre új ügyfél, a rendszer hozzárendel toothem, és csak akkor lesz képes toosee azt.

## <a name="next-steps"></a>Következő lépések
Ennyi az egész! egy több-bérlős, ahol minden felhasználó rendelkezik-e a saját a ismerőslistájába egy egyszerű forduljon Manager webalkalmazás hello lett alakítva. Használ sorszintű biztonsággal, azt is elkerülhető bérlői access programot az alkalmazás kódjában érvényesítési hello összetettségét. Az átláthatóság hello alkalmazás toofocus lehetővé teszi az elvégzendő hello valódi üzleti probléma, és csökkenti véletlenül megakadályozására adatok, mint hello alkalmazás csomagazonosítóját kódbázis hello kockázatát növekszik.

Ez az oktatóanyag csak karcolva hello felületét Mi az az RLS lehetséges rendelkezik. Például lehetséges toohave kifinomultabb vagy részletes access programot, és azok lehetséges toostore csupán hello hello SESSION_CONTEXT az aktuális felhasználói azonosítóját. Lehetőség arra is túl[RLS integrálása hello rugalmas adatbázis eszközök ügyféloldali kódtáraknál](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport több-bérlős szilánkok a kibővített adatok rétegben.

Ezekkel a lehetőségekkel, túl toomake még élvezetesebbé RLS is dolgozunk. Ha kérdése van, ötleteket vagy toosee, milyen dolgot tudassa velünk hello megjegyzések. Köszönjük visszajelzését!

