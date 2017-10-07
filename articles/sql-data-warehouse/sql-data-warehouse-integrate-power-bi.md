---
title: "az SQL Data Warehouse szolgáltatással a Power BI aaaUse |} Microsoft Docs"
description: "Tippek a Power BI használatával az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a3a347493d07af6824a561567f05894cfe3c0471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a>A Power BI használja az SQL Data Warehouse szolgáltatással
Az Azure SQL Database, a SQL Data Warehouse közvetlen kapcsolódás lehetővé teszi a felhasználó tooleverage hatékony logikai leküldéses közzététele mellett hello analitikai funkciók a Power bi-ban.  Közvetlen csatlakozás esetén a lekérdezéseket a rendszer küldi vissza tooyour Azure SQL Data Warehouse valós idejű hello adatokba.  Ez, kombinálva hello méretezési az SQL Data Warehouse lehetővé teszi a számára toocreate dinamikus jelentéseket elleni terabájtos adatkészleteket percben.  Ezenkívül hello bevezetése hello nyissa meg a Power BI gomb lehetővé teszi a felhasználók toodirectly csatlakozni a Power BI tootheir SQL Data Warehouse Azure egyéb részeitől információk gyűjtése nélkül.

Ha közvetlen kapcsolódás használatával adjon Megjegyzés:

* Adja meg a hello teljes kiszolgálónév (lásd alább olvashat) csatlakozáskor
* Győződjön meg arról, a tűzfalszabályok hello adatbázis vannak beállítva túl "hozzáférési tooAzure-szolgáltatások engedélyezése".
* Minden művelet, például jelöljön ki egy oszlopot, vagy hozzáad egy szűrőt közvetlenül fogja kérdezni hello data warehouse-bA
* Csempék frissítése körülbelül 15 percenként (a frissítés nem kell ütemezett toobe)
* A Q & A nincs közvetlen kapcsolódás adatkészletek
* Sémaváltozások nem átveszik automatikusan
* Minden közvetlen kapcsolódás lekérdezések időtúllépést okoz 2 perc múlva

E korlátozások és a megjegyzések tooimprove hello lép folyamatos módosíthatja. hello lépéseket tooconnect részleteket lejjebb olvashatja.  

## <a name="using-hello-open-in-power-bi-button"></a>A hello "Megnyitás Power BI-ban" gombra kattintva
hello legegyszerűbb módja toomove a SQL Data Warehouse között a Power BI hello nyissa meg a Power BI gomb jelenti. Ez a gomb lehetővé teszi a tooseamlessly készítését új irányítópultok a Power bi-ban.  

1. lépések tooget tooyour SQL Data Warehouse-példányhoz a klasszikus Azure portál hello lépjen.
2. Hello "Megnyitás Power BI-ban" gombra.
3. Ha nem tudja toosign dolgozunk a közvetlenül, vagy ha nem rendelkezik Power BI-fiókja, szüksége lesz toosign a.  
4. A rendszer átirányítja az SQL Data Warehouse csatlakozási oldalán, az SQL Data Warehouse hello adataival toohello előre feltöltve.
5. A hitelesítő adatok megadása után a teljes csatlakoztatott tooyour SQL Data Warehouse lesz.

## <a name="connecting-through-hello-power-bi-portal"></a>Hello Power BI portálon keresztül csatlakozó
Hozzáadása toousing hello nyissa meg a Power BI gomb, a felhasználók is kapcsolódhatnak az SQL Data Warehouse tootheir hello Power BI-portál.

1. Kattintson az "Adatok beolvasása" hello navigációs ablaktábla hello alján.
2. Válassza ki a "Adatbázisokat".
3. Egyszer hello adatbázisok lapon válassza az "Azure SQL Data Warehouse", és kattintson a "Csatlakozás".
4. Adja meg a hello szükséges csatlakozási adatait.  A kiszolgáló és adatbázis nevét a hello Azure portálon található.
5. A rendszer átirányítja vissza toohello főoldala a Power bi-ban, és ha a kapcsolat létrejött egy új "Adatkészletek" bejegyzést a példány hello nevű fog megjelenni.  
6. Rákattinthat a hello új adatkészlet tooexplore összes hello táblák és nézetek az adatbázisban. Jelöljön ki egy oszlopot elküld egy lekérdezés hátsó toohello forrást, dinamikusan létrehozása a visual. Ezek a látványelemek egy új jelentést lehessen menteni, és vissza tooyour irányítópulton rögzítette.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
