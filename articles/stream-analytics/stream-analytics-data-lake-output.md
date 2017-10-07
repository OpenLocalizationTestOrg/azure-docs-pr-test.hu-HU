---
title: aaaStream Analytics Data Lake Store kimeneti |} Microsoft Docs
description: "Hitelesítési és engedélyezési egy Azure Data Lake store a Stream Analytics-feladatok konfigurálása"
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 183cf51edb2e49ac3e42257e67a8077b95777258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-data-lake-store-output"></a>Stream Analytics Data Lake Store kimeneti
Stream Analytics-feladatok támogatja több kimeneti módszerek közül az egyik egy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Az Azure Data Lake Store egy vállalati szintű, nagy kapacitású adattár a big data koncepción alapuló adatelemzési célokra. Data Lake Store lehetővé teszi a műveleti és felderítési jellegű bármilyen méretű, típusú és feldolgozási sebességű toostore adatok.

## <a name="authorize-a-data-lake-store-account"></a>Data Lake Store-fiók engedélyezése
1. Data Lake Store az Azure-portálon hello kimenetként kiválasztásakor kérni fogja a meglévő Data Lake Store vagy toorequest tooauthorize használata toohello Data Lake Store elérése hello klasszikus portál eléréséhez.
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. Ha már rendelkezik tooData Lake áruház eléréséhez kattintson a "Azonnali engedélyezése" és egy rövid ideig lap jelenik meg, amely jelzi, "Átirányítása tooauthorization". hello lap automatikusan bezárul, és lehetővé teszi tooconfigure hello Data Lake Store kimeneti hello lapok választhat.

Nem feliratkozott a Data Lake Store, esetén hajtsa végre a hello "Feliratkozás most" hivatkozás tooinitiate hello kérelem, vagy hajtsa végre a hello [elindított útmutatást](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-hello-data-lake-store-output-properties"></a>Hello Data Lake Store kimeneti tulajdonságainak konfigurálása
Miután hitelesített hello Data Lake Store-fiók, a Data Lake Store kimeneti állíthatja be a hello tulajdonságait. az alábbi táblázat hello tulajdonságnevek és azok leírása tooconfigure a Data Lake Store kimeneti hello listája.

<table>
<tbody>
<tr>
<td><B>TULAJDONSÁG NEVE</B></td>
<td><B>LEÍRÁS</B></td>
</tr>
<tr>
<td>A kimeneti Alias</td>
<td>Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Data Lake Store.</td>
</tr>
<tr>
<td>Data Lake Store-fiók</td>
<td>ahol a kimeneti küldi hello tárfiók hello neve. Választhat hello bejelentkezett felhasználó hozzáféréssel rendelkezik Data Lake Store-fiókok listájával.</td>
</tr>
<tr>
<td>Elérési út előtag mintája [<I>választható</I>]</td>
<td>fájl elérési útja toowrite hello hello található a fájl megadott Data Lake Store-fiók. <BR>a {date}, {time}<BR>1. példa: mappa1/logs / {date} / {time}<BR>2. példa: mappa1/logs / {date}</td>
</tr>
<tr>
<td>Dátum formátumban [<I>választható</I>]</td>
<td>Hello dátumtokent hello előtag elérési út használata esetén, amelyben a fájlok vannak rendszerezve hello dátumformátum választhatja meg. . Példa: Éééé/hh/nn</td>
</tr>
<tr>
<td>Idő formátumot [<I>választható</I>]</td>
<td>Ha hello idő jogkivonat hello előtag elérési útja, adja meg a hello idő formátumban, amelyben a fájlok vannak rendezve. Jelenleg csak a támogatott hello értéke HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>A kimeneti adatok szerializálási formátum. JSON, CSV és az avro-hoz támogatott.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Ha a fürt megosztott kötetei szolgáltatás- vagy JSON formátumú, kódolással meg kell adni. Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg.</td>
</tr>
<tr>
<td>Elválasztó</td>
<td>Csak a fürt megosztott kötetei szolgáltatás szerializálási alkalmazható. A Stream Analytics számos általánosan használt elválasztó karaktert támogatja a CSV-adatok szerializálása során. Támogatott értékei vesszővel, a pontosvesszővel válassza el, a terület, a lapon és a függőleges vonal.</td>
</tr>
<tr>
<td>Formátumban</td>
<td>Csak a JSON-szerializálás alkalmazható. Sorral elválasztott beállítás megadja, hogy hello kimeneti azzal, hogy minden JSON-objektum sortöréssel elválasztva kell formázni. A tömb határozza meg, hogy hello kimeneti lesznek formázva, egy JSON-objektumok tömbjét.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Újítsa meg a Data Lake Store engedélyezési
Jelenleg egy korlátozás amikor a hello hitelesítésére szolgáló jogkivonat toobe vonatkozó minden Data Lake Store kimenet 90 naponta manuálisan frissíteni kell. Konfigurálnia kell toore-hitelesítéséhez a Data Lake Store-fiókot, ha sikeresen módosította jelszavát, mert a feladat meg lett létrehozva, vagy utolsó hitelesítése. A probléma tünete nem feladatkiemenetét és újbóli engedélyezése szükségességét jelző hello műveletnaplók a hiba.

tooresolve probléma leállítja a futó feladatot, és nyissa meg a kimeneti tooyour Data Lake Store. Hello "Megújítási engedélyezési" hivatkozásra, és egy rövid ideig lap jelenik meg, amely jelzi, "Átirányítása tooauthorization...". hello lap automatikusan bezárul, és ha sikeres, fogja jelezni, "Engedélyezési sikeresen megújítva". Ezután alján hello hello tooclick "Mentés" kell, és indítsa újra a feladatot hello feladat utolsó befejezési időpontja tooavoid adatvesztés lépne.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

