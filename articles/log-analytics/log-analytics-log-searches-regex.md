---
title: "az OMS szolgáltatáshoz aaaRegular kifejezések keresések jelentkezzen |} Microsoft Docs"
description: "Naplóelemzési napló keresések toohello szűrő hello eredmények tooa reguláris kifejezés szerinti hello RegEx kulcsszó is használhatja.  Ez a cikk hello szintaxis biztosít néhány példa a kifejezést."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 3033593dac2c50e911fc69054947d40d4a74369b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-regular-expressions-toofilter-log-searches-in-log-analytics"></a>Napló keres a reguláris kifejezések toofilter használatával Naplóelemzési

[A keresések jelentkezzen](log-analytics-log-searches.md) adattárból hello Naplóelemzési tooextract információk lehetővé teszik.  [Szűrési kifejezésekben](log-analytics-search-reference.md#filter-expressions) lehetővé teszik toofilter hello találatokat hello toospecific feltételek szerint.  Hello **RegEx** kulcsszó lehetővé teszi egy reguláris kifejezést a szűrő toospecify.  

Ez a cikk részletesen hello Naplóelemzési által használt reguláris kifejezés szintaxisa adható meg.

> [!NOTE]
> Csak a kereshető mezők RegEx tudja használni.  További információ a kereshető mezők: **mezőtípusok** a [található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx kulcsszó

A következő szintaxist toouse hello használata hello **RegEx** naplófájl-keresési kulcsszót.  Használhat más hello reguláris kifejezés ebben a cikkben toodetermine hello szintaxis szakasza hello.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

A reguláris kifejezés tooreturn riasztások toouse típussal rendelkező rögzíti, például *figyelmeztetés* vagy *hiba*, a következő naplófájl-keresési hello szeretné használni.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Részleges egyezések
Vegye figyelembe, hogy a hello reguláris kifejezésnek egyeznie kell a teljes szöveges hello hello tulajdonság.  Részleges egyezéseket nem ad vissza rekordok.  Például, ha meg az elérni próbált tooreturn bejegyzéseit srv01.contoso.com nevű számítógépen, hello napló keresése a következő lenne **nem** vissza rekordot.

    Computer=RegEx("srv..")

Ennek oka az, csak hello első név egy részének hello hello reguláris kifejezésre illeszkedik.  hello következő két naplófájl keresések alakítanák vissza rekordok erről a számítógépről, mert azok felel meg a hello teljes nevét.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Karakter
Adjon meg másik karaktereket.

| Karakter | Leírás | Példa | A minta megfelel |
|:--|:--|:--|:--|
| egy | Hello karakter egy előfordulását. | Computer=Regex("Srv01.contoso.com") | Srv01.contoso.com |
| . | Bármely egy karakter. | Computer=Regex("Srv...contoso.com") | Srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| egy? | Hello karakter nulla vagy egy előfordulását. | Számítógép = RegEx ("Szerv01?. "contoso.com") | srv0.contoso.com<br>Srv01.contoso.com |
| a * | Hello karakter nulla vagy több előfordulását. | Computer=Regex("Srv01*.contoso.com") | srv0.contoso.com<br>Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| a + | Egy vagy több események hello karakter. | Computer=Regex("Srv01+.contoso.com") | Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Hello zárójelben egyetlen karakter | Computer=Regex("srv0[123].contoso.com") | Srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [*egy*-*z*] | Hello közé egyetlen karakternek felel meg.  Több tartomány is tartalmazza. | Computer=Regex("srv0[1-3].contoso.com") | Srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Hello zárójelben hello karakterek egyike sem | Computer=Regex("srv0[^123].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [^*egy*-*z*] | Hello közé hello karakterek egyike sem. | Computer=Regex("srv0[^1-3].contoso.com") | srv05.contoso.com<br>srv06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Megfelelő numerikus karakterek tartományát. | Computer=Regex("SRV[01-03].contoso.com") | Srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |
| @ | Bármilyen karakterlánc. | Számítógép = RegEx ("srv@.contoso.com") | Srv01.contoso.com<br>srv02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Több előfordulásának karakter
Adjon meg egy adott karaktereket szoftver többször is előfordul.

| Karakter | Leírás | Példa | A minta megfelel |
|:--|:--|:--|:--|
| {n} |  *n*hello karakter előfordulását. | Computer=Regex("BW-Win-sc01{3}.bwren.Lab") | sávszélesség-Windows-sc0111.bwren.lab |
| a {n} |  *n*vagy több példányának hello karakter. | Computer=Regex("BW-Win-sc01{3,}.bwren.Lab") | sávszélesség-Windows-sc0111.bwren.lab<br>sávszélesség-Windows-sc01111.bwren.lab<br>sávszélesség-Windows-sc011111.bwren.lab<br>sávszélesség-Windows-sc0111111.bwren.lab |
| {n, m} |  *n*túl*m* hello karakter előfordulását. | Computer=Regex("BW-Win-sc01{3,5}.bwren.Lab") | sávszélesség-Windows-sc0111.bwren.lab<br>sávszélesség-Windows-sc01111.bwren.lab<br>sávszélesség-Windows-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Logikai kifejezések
Válasszon ki több érték.

| Karakter | Leírás | Példa | A minta megfelel |
|:--|:--|:--|:--|
| &#124; | Logikai vagy.  Eredményt adja vissza, ha mindkét kifejezés a megfelelő. | Típusú riasztás AlertSeverity = = RegEx ("figyelmeztetés &#124; "Hiba") | Figyelmeztetés<br>Hiba |
| & | Logikai és művelet.  Eredményt adja vissza, ha mindkét kifejezés a megfelelő | EventData = regex ("(biztonsági.\* &. \*sikeres. \*)") | Biztonsági naplózás sikeres |


## <a name="literals"></a>Literálok
Alakítsa át a speciális karakterek tooliteral karaktereket.  Ez magában foglalja a karakterek, amelyek funkciókat biztosítanak tooregular kifejezések például?-\*^\[\]{}\(\)+\|. &.

| Karakter | Leírás | Példa | A minta megfelel |
|:--|:--|:--|:--|
| \\ | Egy speciális karakter tooa literális alakítja. | Status_CF =\\[hiba\\] @<br>Status_CF hiba =\\-@ | [Hiba] A fájl nem található.<br>Hiba – a fájl nem található. |


## <a name="next-steps"></a>Következő lépések

* Ismerkedjen meg [keresések jelentkezzen](log-analytics-log-searches.md) tooview és elemezhetik a hello Log Analytics-tárházban.
