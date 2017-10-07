---
title: "aaaData gyári Azure másolása varázsló |} Microsoft Docs"
description: "További információk a hogyan toouse hello adatok támogatott forrásokból toosinks Data Factory Azure másolása varázsló toocopy adatait."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: 188b3ae15f937b84a58aec1b979347ac8090abf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory-copy-wizard"></a>Az Azure Data Factory másolása varázsló
hello Azure Data Factory másolása varázsló megkönnyíti a választásával dolgozhat fel adatokat, amely általában az első lépés egy végpont integrációs forgatókönyvet hello folyamatán. Amikor hello Azure Data Factory másolása varázsló keresztül haladó, nem kell toounderstand bármely JSON-definíciók társított szolgáltatások, adathalmazok és adatcsatornák. hello varázsló automatikusan hoz létre a folyamat toocopy adatok hello kijelölt adatok forrás toohello kiválasztott cél. Ezenkívül hello másolása varázsló segít toovalidate hello adatok azoknak a szerzői hello időpontban alatt okozhatnak. Ezzel időt takaríthat meg különösen ha meg vannak adatok bevitele hello az első alkalommal hello adatforrásból. toostart hello másolása varázsló, kattintson a hello **adatok másolása** hello kezdőlapján a data factory csempére.

![Másolás varázsló](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>A big data tervezett
Ez a varázsló lehetővé teszi számos forrásból toodestinations tooeasily áthelyezés adatait percben. Után hello varázsló en keresztül halad, a másolási tevékenység során a folyamat automatikusan létrejön, függő Data Factory-entitások (társított szolgáltatások és adatkészletek) együtt. További lépésekre nincs szükség toocreate hello folyamat.   

![Adatforrás kiválasztása](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Részletes útmutatás toocreate egy folyamat toocopy mintaadatokat az Azure blob tooan Azure SQL Database táblából, lásd: hello [másolása varázsló az oktatóanyag](data-factory-copy-data-wizard-tutorial.md).
>
>

hello varázsló úgy van kialakítva, a big Data típusú adatok hello Start és támogatja a különböző adatok és az Objektumtípusok szem előtt. Adat-előállító adatcsatornák, helyezze át a mappát, a fájlok vagy a táblák több száz hozhat létre. hello varázsló támogatja az automatikus adatelőnézet, séma rögzítési és leképezés és az adatok szűrése.

## <a name="automatic-data-preview"></a>Automatikus megtekintés
Megtekintheti a kijelölt adatforrás hello rendelés toovalidate hello adatok egy részét, hogy hello adatok mit toocopy. Emellett ha hello forrásadatok szövegfájl, hello másolása varázsló elemez hello szöveg toolearn hello sor és oszlop határolójel és séma automatikusan.

![Fájl formázási beállítások](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Séma rögzítési és -leképezés
a bemeneti adatok hello séma előfordulhat, hogy nem felel meg a hello sémának bizonyos esetekben a kimeneti adatok. Ebben az esetben kell hello forrás séma toocolumns hello cél séma toomap oszlopokat.

> [!TIP]
> Ha SQL Server vagy az Azure SQL Database az Azure SQL Data Warehouse, ha hello tábla nem létezik hello céltár, adat-előállító az adatok másolását támogatja adatforrás séma használatával automatikus tábla létrehozásához. A további [adatok tooand áthelyezése az Azure SQL Data Warehouse Azure Data Factory használatával](./data-factory-azure-sql-data-warehouse-connector.md).
>

A legördülő lista tooselect hello séma toomap tooa forrásoszlop oszlopának hello cél séma használja. hello másolása varázsló megpróbál toounderstand oszlopleképezés a mintát. Azonos mintát toohello rest-hello oszlopok hello vonatkozik, hogy tooselect minden hello oszlopok nem kell külön-külön toocomplete hello séma-hozzárendelése. Ha szeretné, felülbírálhatja a leképezések hello legördülő listákból toomap hello oszlopok egyenként használatával. hello mintát még pontosabb további oszlopok leképez válik. hello másolása varázsló folyamatosan frissíti a hello mintát, és végső soron eléri hello jobb mintát hello oszlopleképezés meg szeretné-e tooachieve.     

![Séma-hozzárendelése](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Adatok szűrése
Adatok tooselect csak hello forrásadatokban másolt toobe toohello fogadó adattár jeleníthetők meg. Szűrés csökkenti a hello adatok toobe másolt toohello fogadó adatokat tárolja, és ezért javítja a hello átviteli hello másolási műveletek hello mennyiségét. Lehetővé teszi egy rugalmasan toofilter adatokat egy relációs adatbázisban hello SQL lekérdezési nyelv használatával, vagy egy Azure blob mappában a fájlok [adat-előállító funkciók és változók](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Egy adatbázis adatok szűrése
hello alábbi képernyőfelvételen látható egy SQL-lekérdezést használ hello `Text.Format` függvény és `WindowStart` változó.

![Kifejezések ellenőrzése](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Az adatok Azure blob mappában szűrése
Hello mappa elérési útja toocopy adatait tartalmazó mappa, amely alapján futásidőben határozza meg a változókat is használhat [rendszerváltozók](data-factory-functions-variables.md#data-factory-system-variables). hello támogatott értékek: **{year}**, **{month}**, **{day}**, **{óra}**, **{perc}**, és **{egyéni}**. Például: inputfolder / {year} / {month} / {day}.

Tegyük fel, hogy rendelkezik-e bemeneti hello formátuma a következő mappák:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Hello kattintson **Tallózás** gombra kattint, a **fájl vagy mappa**, keresse meg a mappa tooone (például 2016 -> 03 -> 01 -> 02), és kattintson **válasszon**. Megtekintheti az `2016/03/01/02` hello szövegmezőben. Most, cserélje le **2016** rendelkező **{year}**, **03** rendelkező **{month}**, **01** rendelkező **{day}** , és **02** rendelkező **{óra}**, és nyomja le az hello **lapon** kulcs. Legördülő listák tooselect hello formátum négy változókhoz kell megjelennie:

![Rendszer változók használata](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Ahogy az alábbi képernyőfelvétel a hello, használhatja a **egyéni** változó és a [támogatott formázási karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx). egy mappa struktúra, használjon hello tooselect **Tallózás** először gombra. Ezután cserélje le a értékét **{egyéni}**, és nyomja le az hello **lapon** kulcs toosee hello mezőbe írható hello formázó karakterlánc.     

![Egyéni változóval](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Ütemezési beállítások
Futtathatja hello másolási művelet egyszer vagy ütemezés (óránként, naponta, és így tovább). Mindkét lehetőség használható hello mélysége hello összekötők környezetek, beleértve a helyszíni, a felhő és a helyi asztali példány között.

Egy egyszeri másolási művelet lehetővé teszi, hogy a forrás tooa cél az adatátvitel csak egyszer. Érvényes toodata tetszőleges méretű és bármely támogatott formátumra. ütemezett hello másolási lehetővé teszi toocopy adatok az előírt ismétlődése. Gazdag beállításaihoz (például az újra gombra, időtúllépés és riasztások) is használhat tooconfigure hello ütemezett másolása.

![Ütemezési tulajdonságok](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Következő lépések
A másolási tevékenység hello Data Factory másolása varázsló toocreate folyamat használatával gyorsan útmutatást lásd: [oktatóanyag: hozzon létre egy folyamatot, hello másolása varázsló használatával](data-factory-copy-data-wizard-tutorial.md).
