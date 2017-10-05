---
title: "Beállítása és használata a Machine Learning javaslatok API |} Microsoft Docs"
description: "Az Azure Machine Learning GYIK a beépített Microsoft javaslatok API"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 1ffc3c16-e040-4225-9d72-105129938dfa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 3851589818bb8f4309bf3c65f17b115e0dcd27fa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>A Machine Learning Recommendations API beállítása és használata – GYIK
**Mi az az ajánlások?**

> [!NOTE]
> El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában. A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)
> 
> 

A szervezetek és vállalatok számára, a kereszt-értékesít javaslatok és felfelé-értékesít termékek és az ügyfeleknek szolgáltatásokat használó javaslatok az Azure Machine Learning egy önkiszolgáló javaslatok motor biztosít. A core algoritmus mátrix factorization használó együttműködési szűrés megvalósítása. JAVASLATOK férhetnek hozzá alkalmazásfejlesztők REST API-k használatával. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Mi a teendő a javaslatok?**

JAVASLATOK vesz igénybe, adjon meg egy elem vagy egy elemet, és a vonatkozó javaslatok listáját adja vissza. Például: az ügyfél egy online közvetítő kattint egy termék. Az online közvetítő küld ennek a terméknek javaslatok bemenetként, ismét lekérdezi a termékek listáját, és úgy dönt, amelyhez ezeket a termékeket nem jelenik meg az ügyfél. Előfordulhat, hogy használandó javaslatok optimalizálja az online áruházból vagy akár a vállalaton belüli tájékoztatja értékesítési részleg vagy hívás center.

**Vannak-e használati korlátozások?**

Javaslatok rendelkezik a következő használati korlátozások vonatkoznak:

* Előfizetésenként modellek maximális száma: 10
* A katalógus rendelkező elemek maximális számát: 100 000
* Mindig használati pontok maximális számát ~ 5,000,000. A legrégebbi esetén törlendő újakat feltöltött vagy fogja jelenteni.
* E-mailben (például katalógus adatokat importálhat, használati adatok importálása) elküldött adatok maximális mérete 200 MB
* Tranzakció nincs aktív javaslatok modell build (TP-k) másodpercenkénti száma nem ~ 2 TP-k. Aktív javaslatok modell build legfeljebb 20 TP-k tárolására képes.

## <a name="purchase-and-billing"></a>A vásárlás és számlázási
**Mennyibe javaslatok költség indítási időszakban?**

Javaslatok egy olyan előfizetés-alapú szolgáltatás. Díjszabási havonta tranzakciók mennyisége alapján történik. Ellenőrizheti a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) a Microsoft Azure piactéren díjszabási információkat.

**-E javaslatok rendelkező társított költségekre nyomon követése és tárolja a felhasználói tevékenység a számomra?**

Nem abban a pillanatban.

**Javaslatok rendelkezik egy ingyenes próbaverzióra?**

Nincs szabad nyomokat, amely 10 000 tranzakciók havonta korlátozódik.

**Ha I alapján számlázzuk ajánlások?**

A szolgáltatás fizetős egyetlen előfizetéshez nincs havi díjért. Ha megvásárolja a fizetős verziót választja, azonnal számolnak hónap első használatra. Az ajánlat az előfizetés lapján (és az adót) a társított összeg van szó. Ez a havi díj minden hónapban, mint az eredeti vásárlás naptár napon történik, mindaddig, amíg az előfizetés megszüntetése. 

**Hogyan lehet frissíteni egy magasabb réteg szolgáltatásra?**

Vásároljon, vagy frissítse az előfizetést a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations) oldalon, a Microsoft Azure piactérről.

Amikor frissít egy előfizetést:

* A régi előfizetés hátralévő tranzakciók nem adódnak hozzá az új előfizetés. 
* Teljes ár az új előfizetés fizetnie, annak ellenére, hogy a régi előfizetésben nem használt tranzakciók.

A folyamat egy előfizetés frissítése:

* A Nevigate a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).
* Ha még nem jelentkezett be, jelentkezzen be a piactéren.
* A jobb oldali ablaktáblában az elérhető csomagok vannak felsorolva. Kattintson a gombra, hogy frissíteni szeretné a terv.
* Ha frissíteni szeretné, kattintson a **OK**. Ha nem szeretné frissíteni, kattintson a **Mégse**.

**Fontos** gondosan olvassa el a párbeszédpanelen, mert vannak a számlázási és -felhasználási megvalósítását frissítése előtt.

**Amikor véget ér az előfizetésem ajánlásokhoz?**

Az előfizetés véget ér, amikor vethető el. Ha szeretné, hogy az előfizetések megszakítja, tekintse meg az alábbi utasításokat.

**Hogyan javaslatok előfizetés megszüntetése?**

Az előfizetés megszüntetéséhez használja az alábbi lépéseket. Ha a jelenlegi előfizetés a fizetős verziót választja, az előfizetés továbbra is érvényes az aktuális elszámolási időszak végéig. Ha a visszavonás azonnal érvénybe a van szüksége, lépjen velünk kapcsolatba [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Megjegyzés:** visszatérítés nem kap, ha megszakítja a számlázási időszak vagy a nem használt tranzakciók vége előtt számlázási időszakban.

* Keresse meg a [lap kínálnak](https://datamarket.azure.com/dataset/amla/recommendations).
* Ha még nem jelentkezett be, jelentkezzen be a piactéren.
* Kattintson a **Mégse** jobb oldalán a DataSet adatkészlet nevét és állapotát. Ez az előfizetés az aktuális elszámolási időszak végéig is használhatja, vagy a tranzakció eléri a korlátot (amelyik következik be előbb).

Ha azt szeretné, azonnal, egy új előfizetést vásárolhat előfizetést, a fájl a jegy [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

## <a name="getting-started-with-recommendations"></a>Ismerkedés a javaslatok
**Az ajánlások a számomra?** 

A Machine Learning javaslatok a szervezetek és vállalatok számára, a kereszt-értékesít ajánlásokat, a fel-értékesít termékek és a szolgáltatások az ügyfeleknek használó van. Ha egy ügyfél-webhely, az értékesítési szakemberek, a belső értékesítés, vagy egy ügyfélszolgálatával, és, ha csak néhány dozen termékek vagy szolgáltatások katalógus, a lényeg előfordulhat, hogy előnyt az ajánlások. 

A javaslatok kísérletezés célja, hogy meglehetősen egyszerűek legyenek. A jelenlegi API-alapú verziójával alapszintű programozási szakértelmet igényel. Ha segítségre van szüksége, forduljon a webhely fejlesztett szállító. Ha egy belső IT-részlegtől vagy egy belső fejlesztő, össze kell tudni lehetősége javaslatok beszerzése. 

**Mik azok a javaslatok beállításával kapcsolatos előfeltételek**

Javaslatok megköveteli, hogy Ön naplózása a felhasználó lehetőségek a katalógus vonatkozik. Ha nincs ilyen naplót, és egy ügyfél elérhető webhelyek rendelkezik, a javaslatok gyűjthet felhasználói tevékenység meg. 

Javaslatok is szükséges a termékek vagy szolgáltatások egy katalógust. Ha nincs a katalógusban, javaslatok használhatja a tényleges felhasználói használati adatok és átalakítást egy katalógust. Egy implicit katalógus nem fog tartalmazni, amelyek nem jelentették a felhasználói tranzakció részeként elemeket.

**Hogyan állíthatom be javaslatok először?**

Után [előfizető](https://datamarket.azure.com/dataset/amla/recommendations) ajánlásokat, az API dokumentációjának a használjon a [Azure Machine Learning ajánlásokkal – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md) állíthatja be a szolgáltatást.

**Hol található API-JÁNAK dokumentációja?** 

Az API-dokumentáció [Azure Machine Learning javaslatok – első lépések útmutatójának](machine-learning-recommendation-api-quick-start-guide.md).

**Milyen lehetőségek állnak rendelkezésre a katalógus és használati adatok feltöltése a javaslatok?**

A katalógus és használati adatok feltöltése a két lehetőség közül választhat: exportálja az adatokat a CRM-rendszerből vagy más naplókat, és töltse fel a javaslatok, vagy címkék hozzáadása a webhely, amelyet a felhasználói tevékenységek fogja követni. Ha az utóbbi módszert használja, az adattárolás az Azure-ban.

## <a name="maintenance-and-support"></a>Karbantartási és támogatás
**Hogyan nagy a adatkészlet lehet?**

Minden egyes, legfeljebb 100 000 katalóguselemek tartalmaz, és akár 2048 MB-ra a használati adatok.
Emellett egy előfizetés legfeljebb 10 adatkészletek (modellek) tartalmazhat.

**Hol találhatok ajánlások a technikai támogatási szolgálathoz?**

Technikai támogatás érhető el a [Microsoft Azure támogatási](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) hely.

**Hol találnak a használati feltételeket?**

[Microsoft Azure Machine Learning szolgáltatás javaslatok API feltételeit](https://datamarket.azure.com/dataset/amla/recommendations#terms).

