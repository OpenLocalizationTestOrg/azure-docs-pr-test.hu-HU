---
title: "hello Team adatok tudományos folyamat aaaFeature kiválasztást |} Microsoft Docs"
description: "Szolgáltatás kiválasztása hello célját ismerteti, és hello adatok a fejlesztés folyamatában gépi tanulás szerepkörük példákat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 878541f5-1df8-4368-889a-ced6852aba47
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: 54af93c83e4cc6a3670b3ad62490e0f74082b4ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="feature-selection-in-hello-team-data-science-process-tdsp"></a>Szolgáltatás kiválasztása a hello Team adatok tudományos folyamat (TDSP)
Ez a cikk azt ismerteti, szolgáltatás kiválasztása hello alkalmazásában és példák a szerepét a gépi tanulás hello adatok a fejlesztés folyamata. Ezekben a példákban az Azure Machine Learning Studio állítják. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

hello mérnöki és funkciók kiválasztása része egy hello Team adatok tudományos folyamat (TDSP) című témakörben ismertetett [hello Team adatok tudományos folyamat újdonságai?](data-science-process-overview.md). A szolgáltatás termékgondozó csoportja és a kijelölés hello részét képezik **szolgáltatások fejlesztéséhez** hello TDSP lépését.

* **jellemzőkiemelés**: Ez a folyamat megkísérli toocreate vonatkozó további funkciók hello adatokat, és tooincrease prediktív power toohello tanulási algoritmus a nyers funkciókat meglévő hello.
* **szolgáltatás kiválasztása**: Ez a folyamat az eredeti adatokkal kapcsolatos funkciókkal hello kulcs részét egy kísérlet tooreduce hello dimenzióinak hello képzési probléma, választja ki.

Általában **jellemzőkiemelés** alkalmazott első toogenerate további szolgáltatásokat, valamint hello **kijelölés funkció** lépést elvégezte tooeliminate irreleváns, redundáns vagy magas korrelált szolgáltatások.

## <a name="filtering-features-from-your-data---feature-selection"></a>Az adatok - szolgáltatás kiválasztása a szűrési funkciók
Szolgáltatás kiválasztása rendkívül gyakran alkalmazott prediktív modellezési feladatokhoz, mint az osztályozási vagy regressziós feladatok képzési adathalmaz hello építése folyamat. hello célja tooselect hello szolgáltatások hello eredeti adatkészletből, amelyekkel csökkenthető a dimenziók használatával a legszükségesebb funkciókat toorepresent hello maximális mennyisége variancia hello adatok egy részét. A funkciók részhalmazát, majd, hello csak szolgáltatások toobe tootrain hello modell tartalmazza. Szolgáltatás kiválasztása a két fő célokra szolgál.

* Szolgáltatás kiválasztása először, gyakran nem számít, redundáns kiküszöbölése révén növeli a besorolás pontossága, vagy szolgáltatások szorosan összefügg.
* Második így csökken hello néhány szolgáltatás így hatékonyabb modell képzési folyamat. Ez különösen fontos, amelyek például a támogatási vektoros gépek költséges tootrain tanulókkal számára.

Szolgáltatás kiválasztása a keresési szolgáltatások hello használt adatkészlet tootrain hello modellben tooreduce hello számát, bár nincs általában hivatkozott tooby hello "granularitása csökkentését". A szolgáltatás kiválasztási módszerek őket módosítása nélkül bontsa ki az hello adatokat az eredeti funkciók egy részéhez.  Granularitása csökkentési módszereket tervezni a visszafejtett funkciókra átalakíthatja hello eredeti funkciók és így módosíthatja azokat. Granularitása csökkentési módszerek például egyszerű összetevő elemzés, kanonikus korrelációs elemzés és egyes érték felbontás ellen.

Többek között szolgáltatás kijelölés módszerek olyan felügyelt környezetben egy széles körben alkalmazott kategória neve "szűrő alapú szolgáltatás kiválasztása". Kiértékelésével hello korrelációs közötti minden egyes szolgáltatást és hello target attribútummal, ezek a módszerek a statisztikai mérték tooassign pontszám tooeach szolgáltatása alkalmazni. hello szolgáltatások majd rangsora hello pontszám, esetleg a használt toohelp beállított hello küszöbértéket tartására, vagy egy adott funkcióhoz kiküszöbölése szerint. Hello statisztikai intézkedések ezen módszerek használt például személy korrelációs, a kölcsönös adatokat és az hello Chi squared tesztelése.

Az Azure Machine Learning Studióban nincsenek szolgáltatás kiválasztása a megadott modulokat. Hello a következő ábrán az látható, ezek a modulok tartalmaznia kell [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] és [Fisher lineáris Discriminant elemzés] [ fisher-linear-discriminant-analysis].

![Szolgáltatás kiválasztása – példa](./media/machine-learning-data-science-select-features/feature-Selection.png)

Fontolja meg, például a hello hello használata [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modul. Egyszerűség hello célra toouse hello szöveg adatbányászati példában a fent vázolt továbbra is. Azt feltételezik, hogy azt szeretnénk, 256 funkciókat után regressziós modell keresztül hello jönnek létre toobuild [Szolgáltatáskivonatolás] [ feature-hashing] modul, és adott hello válasz változó "Oszlop1" hello, és egy könyv jelképez Tekintse át a minősítések 1 too5 kezdve. "A szolgáltatás pontozási metódus" toobe "Pearson korrelációs" beállításával hello "Céloszlop" toobe "Oszlop1" és "Kívánt szolgáltatások száma" too50 hello. Ezután hello modul [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] együtt hello target attribútuma "Oszlop1" 50 szolgáltatásokat tartalmazó adatkészletet eredményez. hello alábbi ábra azt mutatja be hello folyamata a kísérlet, és hello csak leírt bemeneti paraméterek.

![Szolgáltatás kiválasztása – példa](./media/machine-learning-data-science-select-features/feature-Selection1.png)

hello alábbi ábrán látható hello eredményül kapott adatkészletek. Egyes szolgáltatások program pontozza a mennyiségeket alapján a hello Pearson korrelációs maga között, és target attribútuma "Oszlop1" hello. a felső pontszámok hello funkcióinak használatát tartanak.

![Szolgáltatás kiválasztása – példa](./media/machine-learning-data-science-select-features/feature-Selection2.png)

hello megfelelő pontszámok kijelölt hello szolgáltatást hello a következő ábrán láthatók.

![Szolgáltatás kiválasztása – példa](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Ez alkalmazásával [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modul, 50 kívüli 256 szolgáltatások van kiválasztva, mert azok rendelkezik hello hello célváltozó "Oszlop1", a legtöbb korrelált funkcióinak alapján hello pontozási "Pearson korrelációs" metódust.

## <a name="conclusion"></a>Összegzés
Szolgáltatás termékgondozó csoportja és a szolgáltatás kiválasztása: a két gyakran fejthetők vissza, és kijelölt szolgáltatások folyamat, amely tooextract hello kulcs információhoz hello betanítása hello hello hatékonyságának növelése. Akkor is tovább fejlesztheti a modellek tooclassify hello bemeneti adatokat hello power pontosan és toopredict eredményeit megkeresheti az Önt érdeklő több abroncsnyomásmérők erős. A szolgáltatás termékgondozó csoportja és a kijelölés kombinálhatja is toomake hello tanulási több számításilag tractable. Szerint továbbfejlesztésének kezeli, és majd a szolgáltatások hello számának csökkentése szükséges toocalibrate vagy tanítási modell. Matematikailag beszéd hello szolgáltatások kijelölt tootrain hello modell olyan minimális hello adatok hello minták ismertetik, és majd sikeresen az eredmények előrejelzése független változók.

Ne feledje, hogy nem mindig feltétlenül tooperform szolgáltatás műszaki osztály vagy a szolgáltatás kiválasztása. E rá szükség, vagy nem attól függ, hogy azt rendelkezik vagy gyűjtése, válassza ki, hogy hello algoritmus és cél hello kísérlet hello hello adatokat.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

