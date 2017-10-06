---
title: "aaaAzure CDN szabályok motor referencia |} Microsoft Docs"
description: "Az Azure CDN referenciadokumentációt szabályok motor egyezés feltételek és a szolgáltatások."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 4159715d15fc792afb49dcd2a30752fabcd94a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine"></a>Az Azure CDN szabálymotor
Ez a témakör részletes leírását tartalmazza hello szabad feltételek és a szolgáltatások az Azure Content Delivery Network (CDN) [szabálymotor](cdn-rules-engine.md).

hello HTTP szabálymotor tervezett toobe hello végső szolgáltató hogyan adott típusú kérelmet a dolgozza fel hello CDN.

**Gyakori használati**:

- Bírálja felül, vagy egy egyéni gyorsítótár-házirend meghatározásához.
- Biztonságos, vagy letiltja a bizalmas a tartalomhoz.
- Kérelmek átirányítása.
- Egyéni adatok tárolására.

## <a name="terminology"></a>Terminológia
Egy szabály van meghatározva hello használata [ **feltételes kifejezések**](cdn-rules-engine-reference-conditional-expressions.md), [ **megfelelő**](cdn-rules-engine-reference-match-conditions.md), és [  **szolgáltatások**](cdn-rules-engine-reference-features.md). Ezeket az elemeket a hello a következő ábrán vannak kiemelve.

 ![CDN-egyeztetés feltétel](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Szintaxis

speciális karakterek kezelni hello módon változó függően toohow egyezés feltétel vagy a szolgáltatás kezeli a szöveges értékeket. Egy egyeznek az állapot vagy a szolgáltatás értelmezhetik szöveg a következő módokon hello egyikében:

1. [**Szöveges értékek**](#literal-values) 
2. [**Helyettesítő karakteres értékek**](#wildcard-values)
3. [**A reguláris kifejezések**](#regular-expressions)

### <a name="literal-values"></a>Szöveges értékek
Szöveg, amelyet a rendszer könyvtárelválasztóként értelmezi konstansérték speciális karaktereket, hello kivétellel hello % szimbólum, kezeli a hello érték, amely egyeztetni részeként. Más szóval szövegkonstans felel meg a feltétel túl beállítása`\'*'\` csak kell teljesíteni, ha, amely pontos értékek (azaz `\'*'\`) található.
 
Százalékos szimbólum az használt tooindicate URL-kódolást (pl. `%20`).

### <a name="wildcard-values"></a>Helyettesítő karakteres értékek
Szöveg, amelyet a rendszer könyvtárelválasztóként értelmezi egy helyettesítő értéket rendeli további jelentése toospecial karaktereket. hello a következő táblázat ismerteti, hogyan kell értelmezni a következő karakterek hello.

Karakter | Leírás
----------|------------
\ | Egy fordított perjel hello karakterek egyikét sem ebben a táblázatban megadott használt tooescape. Egy fordított perjel közvetlenül hello speciális karaktert kell lennie escape-karaktersorozatot tartalmazó előtt meg kell adni.<br/>Például a következő szintaxist hello lehet kilépni csillag:`\*`
% | Százalékos szimbólum az használt tooindicate URL-kódolást (pl. `%20`).
* | A csillag karakter egy vagy több karaktert jelölő helyettesítő elemként jelen.
Lemezterület | A szóköz karakter jelzi, hogy egyezés feltétel lehet teljesíteni vagy megadott hello értékek vagy minták.
"érték" | Szimpla idézőjel nincs speciális jelentéssel. Azonban szimpla idézőjelben készlete szolgál, hogy egy érték legyen tooindicate számít konstans érték. A következő módokon hello használhatók:<br><br/>-Lehetővé teszi egy egyezés feltétel toobe hello megadott értéke megegyezik hello összehasonlítási érték bármely részének teljesül-e.  Például `'ma'` megfelelő hello a következő karakterláncok bármelyikét: <br/><br/>/Business/**ma**rathon/asset.htm<br/>**Ma**p.gif<br/>/ üzleti/sablont. **ma**p<br /><br />-Lehetővé teszi egy speciális karakter toobe literális karakter szerepel. Például adhatnak meg szövegkonstans szóköz karakter a szóköz karakter a szimpla idézőjelben belül befoglaló (azaz `' '` vagy `'sample value'`).<br/>-Lehetővé teszi egy üres érték toobe megadott. Adjon meg egy üres értéket az szimpla idézőjelben csoportja (azaz ").<br /><br/>**Fontos:**<br/>-Ha hello megadott érték nem tartalmaz helyettesítő karakter, majd azt automatikusan minősül konstans érték. Ez azt jelenti, hogy nem szükséges toospecify szimpla idézőjelben készlete.<br/>– Ha egy fordított perjel nem karaktert egy másik ebben a táblázatban, majd azt figyelmen kívül a szimpla idézőjelben belül megadott.<br/>-Más módon toospecify egy speciális karakter konstans karakterként tooescape használatával egy fordított perjel (azaz `\`).

### <a name="regular-expressions"></a>A reguláris kifejezések

A reguláris kifejezések egy mintát, amely egy szöveges értéket belül keresendő határozza meg. Reguláris kifejezés notation adott jelentését tooa különböző szimbólumok határozza meg. hello következő táblázat azt jelzi, hogyan különleges karakterek feltételek egyeznek és reguláris kifejezések támogató szolgáltatások kezeli.

Speciális karakter | Leírás
------------------|------------
\ | Egy fordított perjel Kilépés hello karakter hello azt követő. Ilyenkor az adott karakter toobe ahelyett, hogy a reguláris kifejezés jelentését konstansérték számít. Például a következő szintaxist hello lehet kilépni csillag:`\*`
% | a százalékos szimbólum a hello pontos jelentése attól függ, hogy a használatát.<br/><br/> `%{HTTPVariable}`: Ez a szintaxis egy HTTP-változó azonosítja.<br/>`%{HTTPVariable%Pattern}`: Ezt a szintaxist használja a százalékos szimbólum tooidentify változó és a elválasztó HTTP.<br />`\%`: Escape százalékos szimbólum lehetővé teszi, hogy konstans vagy tooindicate URL-kódolást használt toobe (pl. `\%20`).
* | Csillag karakter toobe nulla vagy több alkalommal egyező megelőző hello lehetővé teszi. 
Lemezterület | A szóköz karakter konstans karakterként általában rendszer kezeli. 
"érték" | Szimpla idézőjelben literális karaktereket tekintendők. Szimpla idézőjelben készlete nem rendelkezik speciális jelentéssel.


## <a name="next-steps"></a>Következő lépések
* [Szabályok motor egyezés feltételek](cdn-rules-engine-reference-match-conditions.md)
* [Szabályok motor feltételes kifejezések](cdn-rules-engine-reference-conditional-expressions.md)
* [Szabályok adatbázismotor-szolgáltatások](cdn-rules-engine-reference-features.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)
* [Az Azure CDN áttekintése](cdn-overview.md)
