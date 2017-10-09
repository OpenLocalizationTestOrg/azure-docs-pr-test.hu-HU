---
title: "aaaUse Redgate tooload tooyour az Azure data adatraktár |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Redgate adatok Platform Studio adatraktározási forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 6082390c07c8ffa73ebd8ab272ace00ba8bb1897
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a>Adatok betöltése a Redgate Data Platform Studióval
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Az oktatóanyag bemutatja, hogyan toouse [Redgate tartozó adatok Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) egy helyi SQL Server tooAzure SQL Data Warehouse toomove adatait (terjesztési pontok). Adatok Platform Studio hello legmegfelelőbb javításai vonatkozik, és leggyorsabban hello optimalizálást, ezért úgy tooget elindítani az SQL Data Warehouse szolgáltatással.

> [!NOTE]
> A [Redgate](http://www.red-gate.com) már régóta a Microsoft partnere, és különböző SQL Server-eszközöket tesz elérhetővé. A Data Platform Studio e funkciója ingyenesen elérhető kereskedelmi és nem kereskedelmi használatra.
> 
> 

## <a name="before-you-begin"></a>Előkészületek
### <a name="create-or-identify-resources"></a>Erőforrások létrehozása és azonosítása
Az oktatóanyag elindítása előtt kell toohave:

* **helyszíni SQL Server-adatbázis**: hello tooimport tooSQL adatraktár kell egy helyi SQL Server kiszolgáló toocome kívánt adatokat (2008R2 verzió vagy újabb). A Data Platform Studio nem tud közvetlenül adatokat importálni egy Azure SQL Database-ből vagy szövegfájlokból.
* **Az Azure Storage-fiók**: adatok Platform Studio hello adatokat az Azure Blob Storage előkészíti azt az SQL Data Warehouse betöltése előtt. hello tárfiók helyett hello "Klasszikus" üzembe helyezési modellel hello "erőforrás-kezelő" üzembe helyezési modellben (hello alapértelmezett) kell használnia. Ha a storage-fiók nem rendelkezik, megtudhatja, hogyan tooCreate egy tárfiókot. 
* **Az SQL Data Warehouse**: Ez az oktatóanyag helyez hello adatokat a helyszíni SQL Server tooSQL adatraktár, így toohave kell egy online adatraktárral. Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan tooCreate Azure SQL Data Warehouse.

> [!NOTE]
> A teljesítmény akkor javul, ha hello tárfiókot és hello adatraktár jöttek létre hello azonos régióban.
> 
> 

## <a name="step-1-sign-in-toodata-platform-studio-with-your-azure-account"></a>1. lépés: Bejelentkezés tooData Platform Studio az Azure-fiókjába
Nyissa meg a webböngészőt, és keresse meg a toohello [adatok Platform Studio](https://www.dataplatformstudio.com/) webhelyet. Jelentkezzen be a hello azonos Azure-fiókot adott meg használt toocreate hello tárolási fiók és az adatraktárról. Ha az e-mail címét is munkahelyi vagy iskolai fiókjával és Microsoft-fiókkal társított, hogy meg arról, hogy toochoose hello fiókkal tooyour erőforrások eléréséhez.

> [!NOTE]
> Ha az első alkalommal adatok Platform Studio segítségével, amelyek toogrant hello alkalmazás engedély toomanage kéri az Azure-erőforrások.
> 
> 

## <a name="step-2-start-hello-import-wizard"></a>2. lépés: Hello importálása varázsló elindítása
Hello terjesztési pontok fő képernyőn válassza ki hello tooAzure SQL Data Warehouse hivatkozás toostart hello importálás varázslóról.

![][1]

## <a name="step-3-install-hello-data-platform-studio-gateway"></a>3. lépés: Hello adatátjáró Platform Studio telepítése
tooconnect tooyour a helyszíni SQL Server-adatbázist, tooinstall hello terjesztési pontok átjáró szükséges. hello átjáró egy olyan ügyfélügynök, amelynek hozzáférési tooyour helyszíni környezetben, hello adatok kibontása és feltölti azt tooyour tárfiók. Az adatai soha nem haladnak keresztül a Redgate kiszolgálóin. tooinstall hello átjáró:

1. Kattintson a hello **átjáró létrehozása** hivatkozás
2. A letöltés és telepítés hello átjáró használatával hello megadott telepítő

![][2]

> [!NOTE]
> Átjáró hello bármely gépen a hálózati hozzáférés toohello forrás SQL Server-adatbázis telepíthető. Windows-hitelesítés hitelesítő adatokkal rendelkező hello hello aktuális felhasználó hello SQL Server-adatbázist használja.
> 
> 

A telepítést követően hello átjáró állapot módosítások tooConnected, és kiválaszthatja a Tovább gombra.

## <a name="step-4-identify-hello-source-database"></a>4. lépés: Hello forrásadatbázis azonosítása
A hello *kiszolgáló nevét adja meg* szövegmező, adja meg az adatbázist tároló hello kiszolgáló hello nevét, és válassza ki **következő**. Ezt követően hello legördülő menüből, válassza ki a kívánt tooimport adatait hello adatbázist.

![][3]

Terjesztési pontok megvizsgálja a kijelölt adatbázis hello táblák tooimport. Alapértelmezés szerint a terjesztési pontok hello adatbázis minden hello táblája importálja. Válassza ki, vagy kapcsolja ki a táblák összes tábla hivatkozás hello kibontásával. Válassza ki a hello Tovább gomb toomove előre.

## <a name="step-5-choose-a-storage-account-toostage-hello-data"></a>5. lépés: A tárolási fiók toostage hello adatok kiválasztásához
Terjesztési pontok egy helyen toostage hello adatokat kér. Válasszon ki egy meglévő tárfiókot az előfizetéséből, majd válassza a **Next** (Tovább) lehetőséget.

> [!NOTE]
> Terjesztési pontok létrehoz egy új blob-tároló az kiválasztott tárfiók hello és minden importálásnak egy külön mappát használja.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>6. lépés: Adattárház kiválasztása
Akkor válassza ki, az online [Azure SQL Data Warehouse](http://aka.ms/sqldw) adatbázis-tooimport hello adatokat. Miután az adatbázis kijelölt, tooenter hello hitelesítő adatok tooconnect toohello kell adatbázisról, és válassza ki **következő**.

![][5]

> [!NOTE]
> Terjesztési pontok hello forrás adattáblák egyesít hello data warehouse-bA. Terjesztési pontok figyelmezteti, ha hello táblanév írja elő hello adatraktár toooverwrite meglévő táblákat. Opcionálisan törölheti lévő hello adatraktár létező objektumok által Delete önkéntes importálás előtt minden meglévő objektumot.
> 
> 

## <a name="step-7-import-hello-data"></a>7. lépés: Hello adatok importálása
Terjesztési pontok megerősíti, hogy szeretné-e tooimport hello adatokat. Egyszerűen kattintson hello Start Importálás gomb toobegin hello adatok importálását.

![][6]

Terjesztési pontok kinyeréséhez, valamint a hello a helyszíni SQL Server és hello előrehaladását hello importálása az SQL Data Warehouse hello feltöltése hello előrehaladását mutatja a képi megjelenítés jeleníti meg.

![][7]

Hello importálás végrehajtása után az terjesztési pontok hello adatimportálás összefoglalását és hello javításai elvégzett módosítás jelentést jelenít meg.

![][8]

## <a name="next-steps"></a>Következő lépések
tooexplore SQL Data warehouse, az adatok megjelenítése elindításához:

* [Az Azure SQL Data Warehouse lekérdezése (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]
* [Adatok megjelenítése Power BI használatával][Visualize data with Power BI]

További információk a Redgate tartozó adatok Platform Studio toolearn:

* [A Microsoft hello terjesztési pontok kezdőlap](http://www.dataplatformstudio.com/)
* [Egy bemutató megtekintése a DPS-ről a Channel9-on](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Más módokon toomigrate és a betöltés áttekintése: SQL-adatraktár adatai:

* [A megoldás tooSQL adatraktár áttelepítése][Migrate your solution tooSQL Data Warehouse]
* [Adatok betöltése az Azure SQL Data Warehouse-ba](sql-data-warehouse-overview-load.md)

További fejlesztési tippek, lásd: hello [SQL Data Warehouse fejlesztői áttekintés](sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution tooSQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
