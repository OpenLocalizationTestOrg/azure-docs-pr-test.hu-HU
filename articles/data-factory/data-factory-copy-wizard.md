---
title: "Másolja az adatokat könnyen másolása varázsló – Azure |} Microsoft Docs"
description: "Ismerje meg az adatokat másolni a támogatott adatforrások mosdók a Data Factory másolása varázsló használatával kapcsolatban."
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
ms.openlocfilehash: 282cb4484f8209e6bb36f2a02d7a897f1ba0aa8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Átmásolnia vagy áthelyeznie az adatokat könnyen az Azure Data Factory másolása varázsló
Az Azure Data Factory másolása varázsló választásával dolgozhat fel adatokat, amely általában az első lépés egy végpont integrációs forgatókönyvet folyamatának megkönnyítése érdekében. Az Azure Data Factory másolása varázsló áthaladás, ha nem kell megérteni az összes társított szolgáltatások, adathalmazok és adatcsatornák a JSON-definíciót. Miután végzett a varázsló utasításait, az a varázsló automatikusan létrehozza a kijelölt adatforrás adatainak másolása a kiválasztott cél csővezeték. Emellett a varázsló segít ellenőrzése a szerzői, időpontjában alatt okozhatnak adatait, amely menti a idő jelentős részét, különösen ha meg vannak adatok bevitele először az adatforrásból. A varázsló elindításához kattintson a **adatok másolása** csempét a data factory kezdőlapján.

![Másolás varázsló](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Adatok másolása egy egyszerűen elsajátítható varázslót
Ez a varázsló lehetővé teszi, hogy könnyedén helyezhetik át adatokat különböző forrásokból célhelyekre perc múlva. Elvégzése után a varázsló, a másolási tevékenység során a folyamat automatikusan létrejön függő Data Factory entitások (társított szolgáltatások és adatkészletek) együtt. Nincsenek további lépéseket kell végrehajtani a folyamatot létrehozni.   

![Adatforrás kiválasztása](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Lásd: [másolása varázsló az oktatóanyag](data-factory-copy-data-wizard-tutorial.md) cikk lépéseit másolása minta folyamatokat létrehozni adatait az Azure blob-Azure SQL Database táblához. 
> 
> 

A varázsló a big Data típusú adatok szem előtt a kezdetektől úgy van kialakítva. Egyszerű és hatékony hozhatnak létre, amelyek több száz mappákat, fájlokat vagy az adatok másolása varázslóval táblák áthelyezése adat-előállító adatcsatornák. A varázsló támogatja a következő három szolgáltatás: automatikus adatelőnézet, séma rögzítési és leképezés és az adatok szűrése. 

## <a name="automatic-data-preview"></a>Automatikus megtekintés
A varázsló lehetővé teszi, hogy tekintse át a választott adatforrással kapcsolatosan, hogy ellenőrizze, hogy az adatok a másolni kívánt adatokat az adatok egy részét. Ezenkívül ha az adatok szövegfájlba, másolása varázsló kijelölt szöveg sor és oszlop elválasztókat és séma automatikusan további. 

![Fájl formázási beállítások](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Séma rögzítési és -leképezés
A séma, bemeneti adatokat előfordulhat, hogy a kimeneti adatokat egyes esetekben sémája nem egyezik meg. Ebben a forgatókönyvben kell hozzárendelni a cél séma oszlopok a forrás séma oszlopokat. 

Másolása varázsló automatikusan leképezi a forrás sémában oszlopok oszlopok a cél sémában. A leképezések felülírása a legördülő listák segítségével (de) adja meg, hogy egy oszlopot kell figyelmen kívül hagyja az adatok másolásakor.   

![Séma-hozzárendelése](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Adatok szűrése
A varázsló lehetővé teszi szűrése forrásadatok csak, amelyet a cél/fogadó adattárba másolni kívánt adatok kiválasztásához. Szűrés csökkenti a fogadó adattárba másolandó adatok mennyiségét, és ezért javítja a teljesítményt, a másolási művelet. Biztosít egy relációs adatbázisban lévő adatok szűrése rugalmasan használatával SQL lekérdezési nyelv (vagy) fájlok az Azure blob mappában használatával [adat-előállító funkciók és változók](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Egy adatbázis adatok szűrése
A példában az SQL-lekérdezést használ a `Text.Format` függvény és `WindowStart` változó. 

![Kifejezések ellenőrzése](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Az adatok Azure blob mappában szűrése
Adatok másolása egy mappába, amely alapján futásidőben határozza meg a mappa elérési változók is használhatja [rendszerváltozók](data-factory-functions-variables.md#data-factory-system-variables). A támogatott értékek: **{year}**, **{month}**, **{day}**, **{óra}**, **{perc}**, és **{egyéni}**. Példa: inputfolder / {year} / {month} / {day}.

Tegyük fel, hogy rendelkezik-e bemeneti mappák a következő formátumban:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Kattintson a **Tallózás** gombra kattint, a **fájl vagy mappa**, keresse meg az egyik mappát (például 2016 -> 03 -> 01 -> 02), és kattintson a **válasszon**. Megtekintheti az `2016/03/01/02` a szövegmezőben. Cserélje le **2016** rendelkező **{year}**, **03** rendelkező **{month}**, **01** rendelkező **{day}**, és **02** rendelkező **{óra}**, és nyomja meg a Tab. Legördülő lista használatával válassza ki a formátumot az alábbi négy változók kell megjelennie:

![Rendszer változók használata](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Ahogy az az alábbi képernyőfelvételen, használhatja a **egyéni** változó, és bármilyen [támogatott formázási karakterláncok](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Válasszon egy mappát a struktúra, használja a **Tallózás** először gombra. Ezután cserélje le a értékét **{egyéni}**, és nyomja le az ENTER lapon, láthatja a szövegmezőben, ahová beírhatja a Formátum-karakterlánc.     

![Egyéni változóval](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>A különböző adatok és objektumtípusok támogatása
A varázsló segítségével, akár több százszor is mappákat, fájlokat vagy táblák hatékonyan mozgatják.

![Válassza ki azokat a táblákat, amelyből adatok másolása](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Ütemezési beállítások
Futtathatja a másolási művelet egyszer vagy ütemezés (óránként, naponta, és így tovább). Mindkét lehetőség használható az összekötők a hardverekről a helyszínen, a felhő és a helyi asztali példány között.

Egy egyszeri másolási művelet lehetővé teszi, hogy egy célra forrásból adatmozgás csak egyszer. Érvényes adatok bármilyen méretű és bármely támogatott formátumra. Az ütemezett másolatát lehetővé teszi, hogy az előírt ismétlődése adatokat másolhat. Gazdag beállításaihoz (például az újra gombra, időtúllépés és riasztások) segítségével konfigurálhatja az ütemezett másolatát.

![Ütemezési tulajdonságok](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Következő lépések
A Data Factory másolása varázsló segítségével hozzon létre egy folyamatot másolási tevékenység az első útmutatást lásd: [oktatóanyag: hozzon létre egy folyamatot, a másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).

