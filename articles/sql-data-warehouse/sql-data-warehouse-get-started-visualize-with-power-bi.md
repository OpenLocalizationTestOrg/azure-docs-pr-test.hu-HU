---
title: a Power BI Microsoft Azure SQL Data Warehouse adatainak aaaVisualize
description: "SQL-adatraktár adatainak megjelenítése Power BI használatával"
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: 0425cf5abe7bc001b2a41df4d09bf5f2e42527e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-data-with-power-bi"></a>Adatok megjelenítése Power BI használatával
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Az oktatóanyag bemutatja, hogyan toouse Power BI tooconnect tooSQL Data warehouse-ba, és néhány alapvető képi megjelenítéseket készíthet.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Előfeltételek
az oktatóanyag teljesítéséhez toostep, lesz szüksége:

* SQL Data Warehouse előzetes betöltését hello AdventureWorksDW adatbázishoz. tooprovision a, lásd: [SQL Data Warehouse létrehozása] [ Create a SQL Data Warehouse] és tooload hello minta adatok kiválasztásához. Ha már rendelkezik egy adatraktárral, de nincsenek mintaadatai, [a mintaadatokat manuálisan is betöltheti][load sample data manually].

## <a name="1-connect-tooyour-database"></a>1. Csatlakozás tooyour adatbázis
a Power BI tooopen, és csatlakozzon a tooyour AdventureWorksDW adatbázishoz:

1. Jelentkezzen be a hello [Azure-portálon][Azure portal].
2. Kattintson az **SQL adatbázisok** elemre, és válassza ki az AdventureWorks nevű SQL Data Warehouse-adatbázist.
   
    ![Adatbázis keresése][1]
3. Hello "Megnyitás Power BI-ban" gombra.
   
    ![Power BI gomb][2]
4. Meg kell jelennie hello SQL Data Warehouse csatlakozási oldalán az adatbázis webcímének. Kattintson a Tovább gombra.
   
    ![Power BI-csatlakozás][3]
5. Adja meg az Azure SQL server-felhasználónevét és jelszavát, és nem fogja teljesen csatlakoztatott tooyour SQL Data Warehouse-adatbázis.
   
    ![Power BI-bejelentkezés][4]
6. Miután bejelentkezett a Power bi-ban, kattintson a hello AdventureWorksDW adatkészletre a bal oldali panelen hello. Ekkor megnyílik hello adatbázis.
   
    ![Power BI, AdventureWorksDW megnyitása][5]

## <a name="2-create-a-report"></a>2. Jelentés létrehozása
Meg vannak a Power BI tooanalyze toouse most már készen áll az AdventureWorksDW-mintaadatok. tooperform hello elemzést követően az AdventureWorksDW aggregatesales nevű nézete rendelkezik. Ebben a nézetben megtalálhatóak hello alapvető metrikákat hello hello vállalati értékesítés elemzéséhez.

1. az értékesítési összegeket toopostal kód hello jobb oldali ablaktáblában szerint térképre toocreate kattintson hello AggregateSales nézet tooexpand azt. Kattintson a PostalCode és a SalesAmount hello oszlopok tooselect őket.
   
    ![Power BI, AggregateSales kijelölése][6]
   
    A Power BI automatikusan felismeri, hogy ezek földrajzi adatok, és egy térképen jeleníti meg azokat.
   
    ![Power BI, térkép][7]
2. Ez a lépés egy oszlopdiagramot hoz létre, amely az értékesítési összegeket az ügyfélbevételek szerint jeleníti meg. toocreate a lépjen toohello kibontott AggregateSales nézetre. Kattintson a SalesAmount mezőre hello. Húzza a hello Customerincome mezőt toohello balra, és helyezze a tengely alá.
   
    ![Power BI, tengely kijelölése][8]
   
    Hello sávdiagram hello balra helyeztük.
   
    ![Power BI, oszlop][9]
3. Ez a lépés egy vonaldiagramot hoz létre, amely az értékesítési összegeket a megrendelési dátumok szerint mutatja. toocreate a lépjen toohello kibontott AggregateSales nézetre. Kattintson a SalesAmount és az OrderDate oszlopra. Hello képi megjelenítések oszlopban kattintson a vonaldiagram ikonjára hello; Ez az első ikon hello hello a második sorban a megjelenítések alatt.
   
    ![Power BI, vonaldiagram kiválasztása][10]
   
    Most már rendelkezik egy jelentést, amely hello adatok három különböző megjelenítését.
   
    ![Power BI, vonal][11]

A folyamatot bármikor mentheti a **Fájl** gombra kattintva, majd a **Mentés** lehetőséget választva.

## <a name="next-steps"></a>Következő lépések
Most, hogy ízelítőt kapott bizonyos idő toowarm hello mintaadatokkal, lásd: hogyan túl[fejlesztése][develop], [betölteni][load], vagy [ Telepítse át][migrate]. Vessen egy pillantást hello vagy [Power BI webhelyén][Power BI website].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting tooSQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
