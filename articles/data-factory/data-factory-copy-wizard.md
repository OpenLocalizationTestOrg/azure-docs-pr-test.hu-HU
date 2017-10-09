---
title: "könnyen másolása varázsló – Azure aaaCopy adatok |} Microsoft Docs"
description: "További információk a hogyan toouse hello adatok támogatott forrásokból toosinks Data Factory másolása varázsló toocopy adatait."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 99437ec16facf3b94c8be18487ec89e9f13acc9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Átmásolnia vagy áthelyeznie az adatokat könnyen az Azure Data Factory másolása varázsló
hello Azure Data Factory másolása varázsló az tooease hello folyamat választásával dolgozhat fel adatokat, amely általában az első lépés egy végpont integrációs forgatókönyvet. Amikor hello Azure Data Factory másolása varázsló keresztül haladó, nem kell toounderstand bármely JSON-definíciók társított szolgáltatások, adathalmazok és adatcsatornák. Azonban hello lépéseket a hello varázsló befejezése után hello varázsló automatikusan hoz létre a folyamat toocopy adatok hello kijelölt adatok forrás toohello kiválasztott cél. Emellett hello másolása varázsló segít toovalidate hello adat hello időpontban azoknak a szerzői, éppen okozhatnak, ami nagy részét a idő menti, különösen ha meg vannak adatok bevitele hello az első alkalommal hello adatforrásból. toostart hello másolása varázsló, kattintson a hello **adatok másolása** hello kezdőlapján a data factory csempére.

![Másolás varázsló](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Adatok másolása egy egyszerűen elsajátítható varázslót
Ez a varázsló lehetővé teszi számos forrásból toodestinations tooeasily áthelyezés adatait percben. Elvégzése után hello varázsló, a másolási tevékenység során a folyamat automatikusan létrejön függő Data Factory entitások (társított szolgáltatások és adatkészletek) együtt. További lépésekre nincs szükség toocreate hello folyamat.   

![Adatforrás kiválasztása](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Lásd: [másolása varázsló az oktatóanyag](data-factory-copy-data-wizard-tutorial.md) részletesen toocreate a cikk egy folyamat toocopy mintaadatokat az Azure blob tooan Azure SQL Database táblából. 
> 
> 

hello varázsló úgy van kialakítva, a big Data típusú adatok hello indítás szem előtt. Egyszerű és hatékony tooauthor adat-előállító adatcsatornák, helyezze át a mappákat, fájlokat vagy hello adatok másolása varázslóval táblák akár több százszor is. hello varázsló támogatja a következő három szolgáltatás hello: automatikus adatelőnézet, séma rögzítési és leképezés és az adatok szűrése. 

## <a name="automatic-data-preview"></a>Automatikus megtekintés
hello másolása varázsló lehetővé teszi, hogy tooreview hello hello adatait kijelölve adatforrás meg toovalidate e hello hello adatok jobb toocopy kívánt adatokat. Emellett ha hello forrásadatok szövegfájl, hello másolása varázsló elemez hello szöveg toolearn sor és oszlop elválasztókat és séma automatikusan. 

![Fájl formázási beállítások](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Séma rögzítési és -leképezés
a bemeneti adatok hello séma előfordulhat, hogy nem felel meg a hello sémának bizonyos esetekben a kimeneti adatok. Ebben az esetben kell hello forrás séma toocolumns hello cél séma toomap oszlopokat. 

hello másolása varázsló automatikusan hozzárendeli a hello forrás séma toocolumns hello cél sémában oszlopai. Ön felülbírálhatja hello hozzárendelések hello legördülő listák segítségével (vagy) adja meg, hogy egy oszlopot kell-e toobe hello az adatok másolásának kihagyta.   

![Séma-hozzárendelése](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Adatok szűrése
hello varázsló lehetővé teszi toofilter adatok tooselect csak hello forrásadatokban másolt toobe toohello cél/fogadó adattár. Szűrés csökkenti a hello adatok toobe másolt toohello fogadó adatokat tárolja, és ezért javítja a hello átviteli hello másolási műveletek hello mennyiségét. Azt adja meg egy rugalmasan toofilter adatait egy relációs adatbázisban használatával SQL lekérdezési nyelv (vagy) fájlok az Azure blob mappában [adat-előállító funkciók és változók](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Egy adatbázis adatok szűrése
Hello példában hello SQL-lekérdezést használ hello `Text.Format` függvény és `WindowStart` változó. 

![Kifejezések ellenőrzése](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Az adatok Azure blob mappában szűrése
Hello mappa elérési útja toocopy adatait tartalmazó mappa, amely alapján futásidőben határozza meg a változókat is használhat [rendszerváltozók](data-factory-functions-variables.md#data-factory-system-variables). hello támogatott értékek: **{year}**, **{month}**, **{day}**, **{óra}**, **{perc}**, és **{egyéni}**. Példa: inputfolder / {year} / {month} / {day}.

Tegyük fel, hogy rendelkezik-e bemeneti hello formátuma a következő mappák:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Hello kattintson **Tallózás** gombra kattint, a **fájl vagy mappa**, keresse meg a mappa tooone (például 2016 -> 03 -> 01 -> 02), és kattintson **válasszon**. Megtekintheti az `2016/03/01/02` hello szövegmezőben. Cserélje le **2016** rendelkező **{year}**, **03** rendelkező **{month}**, **01** rendelkező **{day}**, és **02** rendelkező **{óra}**, és nyomja meg a Tab. Legördülő listák tooselect hello formátum négy változókhoz kell megjelennie:

![Rendszer változók használata](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Ahogy az alábbi képernyőfelvétel a hello, használhatja a **egyéni** változó és a [támogatott formázási karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx). egy mappa struktúra, használjon hello tooselect **Tallózás** először gombra. Ezután cserélje le a értékét **{egyéni}**, és nyomja le az ENTER lapon toosee hello szövegmező írható hello formázó karakterlánc.     

![Egyéni változóval](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>A különböző adatok és objektumtípusok támogatása
Hello másolása varázsló segítségével, akár több százszor is mappákat, fájlokat vagy táblák hatékonyan mozgatják.

![Válassza ki a tábla mely toocopy adatokból](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Ütemezési beállítások
Futtathatja hello másolási művelet egyszer vagy ütemezés (óránként, naponta, és így tovább). Mindkét lehetőség használható hello összekötők hello szélessége a helyszínen, a felhő és a helyi asztali példány között.

Egy egyszeri másolási művelet lehetővé teszi, hogy a forrás tooa cél az adatátvitel csak egyszer. Érvényes toodata tetszőleges méretű és bármely támogatott formátumra. ütemezett hello másolási lehetővé teszi toocopy adatok az előírt ismétlődése. Gazdag beállításaihoz (például az újra gombra, időtúllépés és riasztások) is használhat tooconfigure hello ütemezett másolása.

![Ütemezési tulajdonságok](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Következő lépések
A másolási tevékenység hello Data Factory másolása varázsló toocreate folyamat használatával gyorsan útmutatást lásd: [oktatóanyag: hozzon létre egy folyamatot, hello másolása varázsló használatával](data-factory-copy-data-wizard-tutorial.md).

