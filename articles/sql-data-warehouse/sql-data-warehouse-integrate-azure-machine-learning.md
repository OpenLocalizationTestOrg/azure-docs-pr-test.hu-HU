---
title: "az SQL Data Warehouse szolgáltatással Azure Machine Learning aaaUse |} Microsoft Docs"
description: "Oktatóanyag az Azure Machine Learning Azure SQL Data Warehouse adattárházzal történő, megoldások fejlesztésére irányuló használatához."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: fdfe8c936d2bb7a02163a0bbf6435e1ebd518d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Az Azure gépi tanulás használja az SQL Data Warehouse szolgáltatással
Az Azure Machine Learning egy olyan teljes körűen felügyelt prediktív elemzési szolgáltatás is használja toocreate prediktív modelleket SQL-adatraktár adatai alapján, és tegye közzé felhasználásához felhasználásra kész webszolgáltatásként. Ismerje meg, a prediktív elemzés hello alapjait és a gépi tanulás olvasásával [bemutatása tooMachine Azure tanulási][Introduction tooMachine Learning on Azure].  Majd megtudhatja hogyan toocreate, betanítását, pontozása és tesztelése a gépi tanulási modell használatával hello [létrehozás kísérlet oktatóanyag][Create experiment tutorial].

Ebből a cikkből megtudhatja, hogyan hello segítségével a következő toodo hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:

* Adatokat olvasni az adatbázis toocreate, betanítását és a prediktív modell pontozása
* Adatok tooyour adatbázis írása

## <a name="read-data-from-sql-data-warehouse"></a>Az SQL Data Warehouse-adatok olvasása
A Microsoft adatokat olvasni termék hello AdventureWorksDW adatbázis táblájában.

### <a name="step-1"></a>1. lépés
Új kísérlet indításához kattintson + új hello hello Machine Learning Studio ablakának alsó részén válassza ki a KÍSÉRLETET, és válassza az üres kísérlet. SELECT hello alapértelmezett neve hello vásznon a hello tetején kísérletezhet, és adjon neki toosomething értelmes, például a kerékpárgyártó árának előrejelzése.

### <a name="step-2"></a>2. lépés
Keresse meg hello olvasó modul hello paletta az adatkészletek és a modulok hello kísérletvászonra hello balra. Húzza a hello modul toohello kísérlet vászonra.
![][drag_reader]

### <a name="step-3"></a>3. lépés
Válassza ki a hello olvasó modul, és töltse ki a hello tulajdonságok panelen.

1. Az Azure SQL Database jelöli ki hello adatforrás.
2. Adatbázis-kiszolgáló nevét: hello kiszolgáló neve. Használhatja a hello [Azure-portálon] [ Azure portal] toofind ez.

![][server_name]

1. Adatbázis neve: hello kiszolgálón csak a megadott adatbázis hello nevét.
2. Felhasználói fiók kiszolgálónév: írja be a hello felhasználónevet egy olyan fiók, amely rendelkezik hozzáférési engedélyekkel hello adatbázis.
3. Kiszolgáló felhasználói fiók jelszavát: Adjon meg hello hello jelszót adott felhasználói fiókot.
4. Fogadja el a kiszolgálói tanúsítvány: használja ezt a beállítást (kevésbé biztonságos), ha azt szeretné, hogy az adatok olvasása előtt tekintse át a hello webhelytanúsítvány tooskip.
5. Adatbázis-lekérdezés: Adjon meg egy SQL-utasítást, amely leírja a tooread kívánt hello adatokat. Ebben az esetben azt az adatokat olvassa a következő lekérdezés hello segítségével termék táblából.

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>4. lépés
1. Futtassa a hello kísérlet hello kísérletvászon alatt a Futtatás gombra kattintva.
2. Hello kísérlet befejezésekor hello olvasó modul lesz egy zöld pipa tooindicate sikeresen befejeződött. Figyelje meg is hello hello jobb felső sarkában található futási állapota kész.

![][run]

1. toosee importált adatok hello hello kimeneti port hello hello autókat tartalmazó adathalmaz alsó részén kattintson, és válassza a képi megjelenítés opcióra.

## <a name="create-train-and-score-a-model"></a>Létrehozását, betanítását és pontszám modell
Most már használhatja az adatkészlethez:

* A modell létrehozása: feldolgozni az adatokat, és a jellemzők meghatározása
* Hello tanítási: kiválasztása és tanulási algoritmus alkalmazása
* Pontszám és tesztelési hello modell: új kerékpárgyártó árának előrejelzése

![][model]

arról, hogyan toocreate, betanítását, pontozása és tesztelése a gépi tanulási modell használata hello további toolearn [létrehozás kísérlet oktatóanyag][Create experiment tutorial].

## <a name="write-data-tooazure-sql-data-warehouse"></a>Az SQL Data Warehouse adatok tooAzure írása
Hello eredménytáblájában set tooProductPriceForecast hello AdventureWorksDW adatbázis írja azt.

### <a name="step-1"></a>1. lépés
Keresse meg hello író modul hello paletta az adatkészletek és a modulok hello kísérletvászonra hello balra. Húzza a hello modul toohello kísérlet vászonra.

![][drag_writer]

### <a name="step-2"></a>2. lépés
Válassza ki a hello író modul, és töltse ki a hello tulajdonságok panelen.

1. Az Azure SQL Database jelöli ki hello adatok cél.
2. Adatbázis-kiszolgáló nevét: hello kiszolgáló neve. Használhatja a hello [Azure-portálon] [ Azure portal] toofind ez.
3. Adatbázis neve: hello kiszolgálón csak a megadott adatbázis hello nevét.
4. Felhasználói fiók kiszolgálónév: hello adatbázis írási engedéllyel rendelkező fiók hello felhasználói nevét.
5. Kiszolgáló felhasználói fiók jelszavát: Adjon meg hello hello jelszót adott felhasználói fiókot.
6. Fogadja el a kiszolgálói tanúsítvány (nem biztonságos): válassza ezt a lehetőséget, ha nem szeretné, hogy tooview hello tanúsítványt.
7. Mentett oszlopok toobe vesszővel elválasztott listája: hello dataset vagy eredmény oszlopok listáját tartalmazzák, amelyet az toooutput.
8. Adatok táblanév: hello hello adattábla nevét adja meg.
9. A datatable oszlopok vesszővel tagolt listája: hello oszlop nevek toouse meg hello új tábla. hello oszlopnevek különbözhet hello néhányat a meglévők közül hello forrás adatkészletben, de kell listán azonos számú oszlopot itt a kimeneti táblához hello meghatározó hello.
10. Az SQL Azure-művelet írt sorok száma: beállíthatja a hello tooa SQL-adatbázis egy művelet írt sorok száma.

![][writer_properties]

### <a name="step-3"></a>3. lépés
1. Futtassa a hello kísérlet hello kísérletvászon alatt a Futtatás gombra kattintva.
2. Hello kísérlet befejezése után minden modul lesz egy zöld pipa tooindicate, hogy sikeresen befejeződött.

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction toomachine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
