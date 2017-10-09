---
title: "VS Code: Csatlakozás és adatok lekérdezése Azure SQL Database-adatbázisokból | Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooSQL adatbázis Azure Visual Studio Code használatával. Tooquery és Szerkesztés adatokat, majd futtassa a Transact-SQL (T-SQL) utasítás."
metacanonical: 
keywords: "Csatlakozás toosql adatbázis"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/20/2017
ms.author: carlrab
ms.openlocfilehash: ed8bdbfc3271b463a21cde5ff6b5f05fd0ff51d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-use-visual-studio-code-tooconnect-and-query-data"></a>Az Azure SQL Database: Használja a Visual Studio Code tooconnect és lekérdezési adatok

[A Visual Studio Code](https://code.visualstudio.com/docs) egy grafikus kód szerkesztő Linux, macOS, és, amely támogatja a kiterjesztések, beleértve a Windows hello [mssql bővítmény](https://aka.ms/mssql-marketplace) Microsoft SQL Server, az Azure SQL Database és az SQL Data Warehouse lekérdezése. A gyors üzembe helyezési bemutatja, hogyan toouse Visual Studio Code tooconnect tooan Azure SQL-adatbázist, és használja a Transact-SQL utasítás tooquery, beszúrási, frissítési és törlési hello adatbázis adatait.

## <a name="prerequisites"></a>Előfeltételek

A gyors üzembe helyezési használja, mint a kiindulási pont hoznak létre az egyik a gyors üzembe helyezések hello erőforrások:

- [DB létrehozása – portál](sql-database-get-started-portal.md)
- [DB létrehozása – CLI](sql-database-get-started-cli.md)
- [DB létrehozása – PowerShell](sql-database-get-started-powershell.md)

Mielőtt elkezdené, győződjön meg arról, hello legújabb verziója van telepítve [Visual Studio Code](https://code.visualstudio.com/Download) és betöltött hello [mssql bővítmény](https://aka.ms/mssql-marketplace). Hello mssql bővítmény telepítési útmutatásért lásd: [Visual STUDIO Code telepítése](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) és [a Visual Studio Code mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql). 

## <a name="configure-vs-code"></a>A VS Code konfigurálása 

### <a name="mac-os"></a>**Mac OS**
MacOS kell tooinstall OpenSSL, amely egy előfeltétel, hogy mssql kiterjesztés DotNet Core célokra. Nyissa meg a terminált, és írja be a következő parancsok tooinstall hello **brew** és **OpenSSL**. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Nincs szükség különleges konfigurációra.

### <a name="windows"></a>**Windows**

Nincs szükség különleges konfigurációra.

## <a name="sql-server-connection-information"></a>Az SQL-kiszolgáló kapcsolatadatai

Hello kapcsolat szükséges információkat tooconnect toohello Azure SQL adatbázis beolvasása. Hello teljes kiszolgálónév, az adatbázisnév és a bejelentkezési adatok a következő eljárások hello kell.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap. 
3. A hello **áttekintése** lap az adatbázishoz, tekintse át hello teljesen minősített kiszolgáló neve, ahogy az a következő kép hello. Hello server name toobring hello másolatot is mutat **toocopy kattintson** lehetőséget.

   ![kapcsolatadatok](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Ha elfelejtette a hello bejelentkezési adatok az Azure SQL Database-kiszolgáló, keresse meg a toohello SQL adatbázis-kiszolgáló lapon tooview hello server admin neve és, ha szükséges, állítsa vissza a hello jelszót. 

## <a name="set-language-mode-toosql"></a>Set language mód tooSQL

Hello nyelvi mód beállítása értéke túl**SQL** a Visual Studio Code tooenable mssql parancsok és a T-SQL IntelliSense.

1. Nyisson meg egy új Visual Studio Code-ablakot. 

2. Kattintson a **egyszerű szöveges** a hello jobb alsó sarkában hello állapotsor.
3. A hello **nyelv kijelölését mód** megnyíló legördülő menü, írja be **SQL**, majd nyomja le az **ENTER** tooset hello nyelvi mód tooSQL. 

   ![SQL nyelvmód](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-tooyour-database"></a>Csatlakozás tooyour adatbázis

Visual Studio Code tooestablish kapcsolat tooyour Azure SQL adatbázis-kiszolgálót használja.

> [!IMPORTANT]
> A folytatás előtt győződjön meg arról, hogy rendelkezésére állnak a kiszolgálóval és az adatbázissal kapcsolatos, valamint a bejelentkezési adatok. Kezdés belépés hello kapcsolati profil adatait, ha módosítja a fókusz a Visual Studio Code követően toorestart hello kapcsolati profil létrehozása.
>

1. A Visual STUDIO Code, nyomja le a **CTRL + SHIFT + P** (vagy **F1**) tooopen hello parancs palettát.

2. Írja be az **sqlcon** parancsot, és nyomja le az **ENTER** billentyűt.

3. Nyomja le az **ENTER** tooselect **kapcsolatprofil létrehozása**. Ezzel létrehoz egy kapcsolati profilt az SQL Server-példányhoz.

4. Hajtsa végre a hello kér toospecify hello kapcsolat tulajdonságai hello új kapcsolati profil számára. Adja meg az egyes értékek, nyomja le az **ENTER** toocontinue. 

   | Beállítás       | Ajánlott érték | Leírás |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Kiszolgálónév | hello teljes kiszolgálónév | hello neve legyen például ehhez hasonló: **mynewserver20170313.database.windows.net**. |
   | **Adatbázis neve** | mySampleDatabase | hello adatbázis toowhich tooconnect hello neve. |
   | **Hitelesítés** | SQL-bejelentkezés| SQL-hitelesítés ebben az esetben az oktatóanyag hello egyetlen hitelesítési típus. |
   | **Felhasználónév** | hello server rendszergazdai fiók | Ez az hello kiszolgáló létrehozásakor megadott hello fiókhoz. |
   | **Jelszó (SQL-bejelentkezés)** | a kiszolgáló rendszergazdai fiókjának hello jelszó | Ez a hello hello kiszolgáló létrehozásakor megadott jelszót. |
   | **Menti a jelszót?** | Igen vagy Nem | Ha nem szeretné, hogy tooenter hello jelszó minden alkalommal, amikor, válassza az Igen gombra. |
   | **Adja meg a profil kívánt nevét** | Egy profilnév, például: **mySampleDatabase** | A mentett profilnév felgyorsítja a csatlakozást a későbbi bejelentkezések során. | 

5. Nyomja le az hello **ESC** kulcs tooclose hello tájékoztató üzenet értesíti arról, hogy hello-profil létrehozása és csatlakoztatva van.

6. A kapcsolat ellenőrzése hello állapotsorban.

   ![A kapcsolat állapota](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a>Adatok lekérdezése

Használjon hello következő hello első 20 termékeknél tooquery kódot hello segítségével kategória szerint [válasszon](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL-utasításban.

1. A hello **szerkesztő** ablak, írja be a következő lekérdezés hello üres lekérdezési ablakban hello:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Nyomja le az **CTRL + SHIFT + E** hello termék- és a ProductCategory táblázatok tooretrieve adatait.

    ![Lekérdezés](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a>Adat beszúrása

Használjon hello következő kódot tooinsert egy új terméken hello segítségével hello SalesLT.Product táblába [BESZÚRÁSA](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL-utasításban.

1. A hello **szerkesztő** ablakban hello előző lekérdezés törlése, és írja be a következő lekérdezés hello:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Nyomja le az **CTRL + SHIFT + E** tooinsert hello termék tábla új sort.

## <a name="update-data"></a>Adatok frissítése

Használjon hello következő tooupdate hello új terméket, hogy korábban hozzáadott hello segítségével kódot [frissítés](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL-utasításban.

1.  A hello **szerkesztő** ablakban hello előző lekérdezés törlése, és írja be a következő lekérdezés hello:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Nyomja le az **CTRL + SHIFT + E** tooupdate hello a megadott sor hello termék táblában.

## <a name="delete-data"></a>Adat törlése

Használjon hello következő toodelete hello új terméket, hogy korábban hozzáadott hello segítségével kódot [törlése](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL-utasításban.

1. A hello **szerkesztő** ablakban hello előző lekérdezés törlése, és írja be a következő lekérdezés hello:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Nyomja le az **CTRL + SHIFT + E** toodelete hello a megadott sor hello termék táblában.

## <a name="next-steps"></a>Következő lépések

- tooconnect és az SQL Server Management Studio használatával lekérdezés [kapcsolódás és lekérdezés ssms alkalmazásával](sql-database-connect-query-ssms.md).
- Az MSDN magazin Visual Studio Code használatáról szóló cikkéhez lásd az [Adatbázis IDE létrehozása az MSSQL bővítménnyel blogbejegyzést](https://msdn.microsoft.com/magazine/mt809115).
