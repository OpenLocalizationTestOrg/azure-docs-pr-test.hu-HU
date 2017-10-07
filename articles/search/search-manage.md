---
title: "az Azure Search administration aaaService hello Azure-portálon"
description: "Kezelheti az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, a Microsoft Azure-hello Azure-portál használatával."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/18/2017
ms.author: heidist
ms.openlocfilehash: 9bb33660d93e068e0f35b856cba0a41c92623644
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-administration-for-azure-search-in-hello-azure-portal"></a>Hello Azure-portálon az Azure Search szolgáltatás adminisztrációs
> [!div class="op_single_selector"]
> * [Portál](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Az Azure Search egy teljes körűen felügyelt, felhőalapú keresőszolgáltatás gazdag keresésekhez egyéni alkalmazásokba való létrehozására használt. Ez a cikk foglalkozik hello *felügyeleti szolgáltatás* hello elvégezhető feladatok [Azure-portálon](https://portal.azure.com) már kiépített egy keresési szolgáltatáshoz. *Felügyeleti szolgáltatás* egyszerűsített úgy lett kialakítva, korlátozott toohello a következő feladatokat:

* Kezelése és biztonságos hozzáférést toohello *api-kulcsokat* olvasási vagy írási hozzáférést tooyour szolgáltatáshoz használt.
* Módosítsa a szolgáltatás kapacitás hello lefoglalása a partíciók és replikák módosításával.
* Erőforrás-használat, a szolgáltatási rétegben relatív toomaximum határértékeinek figyeli.

**Nincs a hatókörben** 

*Tartalomkezelési* (vagy felügyeleti indexazonosító) toooperations hivatkozik keresési adatforgalma toounderstand lekérdezés elemzése, például amelyek kapcsolatos kifejezések személyek, keresse meg, és hogyan sikeres keresés eredményei ügyfelek toospecific munkánk felderítése a dokumentumok az indexben. Ezen a területen, lásd: [keresési forgalom Analytics az Azure Search](search-traffic-analytics.md).

*Lekérdezés teljesítmény* nem is ez a cikk hello terjed. További információkért lásd: [használati és a lekérdezés metrikát](search-monitor-usage.md) és [teljesítmény- és optimalizálási](search-performance-optimization.md).

*Frissítés* nincs felügyeleti feladatot. Amikor hello szolgáltatás kiépítve-erőforrásokat foglal le, mert tooa áthelyezése másik réteghez új szolgáltatást igényel. További információkért lásd: [Azure Search szolgáltatás létrehozása](search-create-service-portal.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Rendszergazdai jogosultságok
Kiépítés vagy hello szolgáltatást leszerelése végezhető el az Azure-előfizetéshez rendszergazdai vagy társadminisztrátori.

A szolgáltatáson belül bárki, aki toohello szolgáltatás URL-CÍMEN és egy adminisztrációs api-kulcsot rendelkezik olvasási és írási hozzáférése toohello szolgáltatással. Olvasási és írási hozzáférést biztosít a hello képességét tooadd, törölni vagy módosítani a kiszolgálóobjektumok, beleértve az api-kulcsokat, indexek, indexelők, adatforrások, ütemezéseihez és szerepkör-hozzárendelések keresztül megvalósított módon [RBAC által definiált szerepkörök](#rbac).

Az Azure Search minden felhasználói beavatkozás esik e két beállítás közül: olvasási és írási hozzáférés toohello (rendszergazdai jogosultságokkal) vagy csak olvasási hozzáféréssel toohello szolgáltatást (lekérdezés jogosultságok). További információkért lásd: [hello api-kulcsok kezelése](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>A rendszergazdai hozzáférés RBAC-szerepkörök beállítása
Azure biztosít egy [globális szerepkör-alapú engedélyezési modellt](../active-directory/role-based-access-control-configure.md) hello portál vagy a Resource Manager API-k kezelhetők minden szolgáltatáshoz. Tulajdonos, közreműködő, és ahhoz való olvasóra szerepkörök szolgáltatás-felügyelet az Active Directory-felhasználók, csoportok és biztonsági rendszerbiztonsági tagok hozzárendelt tooeach szerepkör hello szint határozza meg. 

Az Azure Search RBAC engedélyek határozzák meg, a következő felügyeleti feladatok hello:

| Szerepkör | Tevékenység |
| --- | --- |
| Tulajdonos |Hozzon létre, vagy törölje a hello szolgáltatás vagy bármely objektum hello szolgáltatás, így az api-kulcsokat, indexek, indexelők, indexelő adatforrások és az indexelő ütemezések.<p>Szolgáltatás állapotának, beleértve a számát, valamint a tárhely méretét megtekintéséhez.<p>Adja hozzá, vagy törölje a szerepköri tagság (csak egy fiók tulajdonosa felügyelheti szerepköri tagság).<p>Az előfizetés rendszergazdáihoz és a szolgáltatások tulajdonosait tartoznia automatikus hello tulajdonosok szerepkör. |
| Közreműködő |Ugyanazt a hozzáférési szintet tulajdonosaként RBAC szerepkörkezelés csökkentve. Például egy munkatárs megtekintheti és újragenerálása `api-key`, de nem módosíthatja a szerepkörtagságok. |
| Olvasó |Szolgáltatás állapota és a lekérdezési kulcsok megtekintése. A szerepkör tagjai nem módosítható a szolgáltatás konfigurációját, és megtekintheti adminisztrációs kulcsok. |

Szerepkörök nem biztosítanak a hozzáférési jogok toohello szolgáltatásvégpontot. Keresési szolgáltatás műveletek, például a Indexkezelés, index feltöltése és lekérdezések keresési adatokon api-kulcsokat, a szerepkörök nem szabályozza. További információkért lásd: "A felügyeleti és adatok műveletek engedélyezési" a [Mi az szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Naplózás és a rendszer
Az Azure Search nem fed fel naplófájlokat egy adott szolgáltatás hello portal, illetve programozott felületek keresztül. Hello alapszintű rétegben és felett a Microsoft a szolgáltatásszint-szerződések (SLA) / 99,9 %-os rendelkezésre állás érdekében minden Azure Search szolgáltatás figyeli. Ha hello szolgáltatás lassú vagy átviteli sebesség SLA-küszöbértékek alá csökken, támogatási csoportokkal tekintse át, hello napló fájlok elérhető toothem és cím hello probléma.

A szolgáltatás általános információinak tekintetében Itt kaphat információt a következő módokon hello:

* A hello szolgáltatás irányítópultját, értesítések, a tulajdonságok és az állapotüzenetek keresztül hello portálról.
* Használatával [PowerShell](search-manage-powershell.md) vagy hello [felügyeleti REST API](https://docs.microsoft.com/rest/api/searchmanagement/) túl[szolgáltatás tulajdonságait](https://docs.microsoft.com/rest/api/searchmanagement/services), vagy az index Erőforrás kihasználtsága állapotát.
* Keresztül [keresési forgalom analytics](search-traffic-analytics.md), ahogy azt már korábban említettük.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>Api-kulcsok kezelése
Összes kérelem tooa – keresés szolgáltatást kell egy api-kulcs lett létrehozva, kifejezetten a szolgáltatás számára. Az api-kulcsot, akkor hello egyetlen eszköz hozzáférési tooyour keresési szolgáltatási végpont hitelesítéséhez. 

Api-kulcsát: véletlenszerűen generált számok és betűk álló karakterlánc. Keresztül [RBAC engedélyek](#rbac), törölheti vagy hello kulcsok beolvasása a, de nem cserélhető le egy kulcs egy felhasználó által megadott jelszót. 

Kétféle kulcsok vannak használt tooaccess a keresési szolgáltatáshoz:

* A rendszergazda (érvényes hello szolgáltatás bármely olvasási és írási művelet)
* A lekérdezés (a csak olvasási műveletek, például a lekérdezések írásában, az index használható)

Egy adminisztrációs api-kulcsot akkor jön létre, amikor hello szolgáltatás ki van építve. Két felügyeleti kulcsok, mint a kijelölt *elsődleges* és *másodlagos* tookeep őket közvetlenül, de valójában azok felcserélhetők. Minden szolgáltatás van két adminisztrációs kulcsok, hogy lehet vonni egy tooyour szolgáltatás elvesztése nélkül. Helyreállíthatók vagy adminisztrációs kulcsot, de nem adható hozzá toohello teljes rendszergazdai száma. Nincs legfeljebb két adminisztrációs kulcsok érhető el keresési szolgáltatásonként.

Lekérdezési kulcsok keresési közvetlenül hívó ügyfélalkalmazások készültek. Too50 lekérdezési kulcsok hozhat létre. Az alkalmazás kódjában megadhatja a hello keresési URL-címet és egy lekérdezési api-kulcs tooallow csak olvasási hozzáféréssel toohello szolgáltatás. Az alkalmazás kódjában is megadja az alkalmazás által használt hello index. Együtt, hello végpont az api-kulcsot a csak olvasási hozzáféréssel, és a cél index hello hatókör és a hozzáférési szint hello kapcsolat az ügyfélalkalmazás határozza meg.

tooget vagy regenerate api-kulcsokat, nyissa meg hello szolgáltatás irányítópultját. Kattintson a **kulcsok** tooslide hello kulcskezelés lap megnyitásához. A kulcsok létrehozása vagy újragenerálása parancsok is hello oldal hello tetején. Alapértelmezés szerint csak az adminisztrációs kulcsok jönnek létre. Lekérdezés api-kulcsokat manuálisan kell létrehozni.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>A biztonságos api-kulcsok
Kulcs biztonsági hozzáférés-igényléssel hello portál vagy az erőforrás-kezelő felületek (a PowerShell vagy a parancssori felület) korlátozásával biztosítja. Amint az előfizetés rendszergazdáihoz megtekintheti, és minden api-kulcs újragenerálása. Elővigyázatosságból tekintse át a szerepkör-hozzárendelések toounderstand hívóbetűk toohello rendszergazda, aki.

1. Hello szolgáltatás irányítópultján kattintson hello hozzáférés ikon tooslide nyitott hello felhasználók paneljét.
   ![][7]
2. A felhasználók tekintse át a meglévő szerepkör-hozzárendelések. Elvárás előfizetés rendszergazdái már rendelkezik teljes körű hozzáférési toohello szolgáltatás hello tulajdonosi szerepkör keresztül.
3. Kattintson a Tovább, toodrill **előfizetés rendszergazdái** majd bontsa ki a hello szerepkör hozzárendelése lista toosee a keresőszolgáltatása közös rendszergazdai jogosultsággal rendelkező.

Egy másik módja tooview hozzáférési engedélyek tooclick **szerepkörök** hello felhasználók panelen. Ekkor megjelenik az elérhető szerepkörök és a felhasználók vagy csoportok hozzárendelt tooeach szerepkör hello számát.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>A figyelő Erőforrás kihasználtsága
Hello irányítópult az erőforrás figyelési hello szolgáltatás irányítópultját, és ezt úgy szerezheti be hello szolgáltatás lekérdezésével néhány metrikák korlátozott toohello információkat. A hello szolgáltatás irányítópultján hello használati területen is gyorsan meghatározhatja, hogy partíció erőforrás szintek megfelelőek-e az alkalmazás.

Hello Search szolgáltatás API használatával, a dokumentumok és indexek száma kérheti le. Nincsenek társított ezeket a számokat hello IP-címek alapján a szigorú korlátozásokat. További információkért lásd: [Search szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md). 

* [Megtekintheti a statisztikákat Index](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [A dokumentumok száma](https://docs.microsoft.com/rest/api/searchservice/count-documents)

> [!NOTE]
> Gyorsítótárazás viselkedések ideiglenesen overstate korlátozni. Például megosztott hello szolgáltatást használ, megjelenhet egy dokumentum meghaladja hello rögzített 10 000 dokumentumok száma. hello adatról ideiglenes, és a hello következő kényszerítési ellenőrzés észlelni fogja. 
> 
> 

## <a name="disaster-recovery-and-service-outages"></a>Katasztrófa utáni helyreállítás és a szolgáltatás leállások esetén

Bár az adatok azt is maradványérték, Azure Search nem biztosítanak az azonnali hello szolgáltatást Ha kimaradás hello fürt vagy a data center szintjén. Ha egy fürt hello adatközpont meghibásodik, hello műveleti csapata felismeri és toorestore szolgáltatás működik. Állásidő fog tapasztalni szolgáltatás visszaállítás során. Szolgáltatási jóváírások toocompensate / hello szolgáltatás hiányában a kérhet [szolgáltatási szint szerződés (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Folyamatos szükség van a végzetes hibák kívül a Microsoft hello esemény, ha sikerült [további szolgáltatás kiépítése](search-create-service-portal.md) egy másik régióban található, és alkalmazzon egy georeplikáció stratégia tooensure indexek a rendszer az összes szolgáltatásban teljesen redundáns.

Felhasználói [indexelők](search-indexer-overview.md) toopopulate és frissítési indexek kezelni tud a vész-helyreállítási hello kihasználva földrajzi-specifikus indexelők keresztül ugyanazon az adatforráson. Az indexelő futtató különböző régiókban két szolgáltatást sikerült felületindexét hello ugyanaz az adatforrás-tooachieve-georedundancia. Ha az adatforrásokat, amelyek is georedundáns vannak indexelő, vegye figyelembe, hogy Azure keresési indexelő csak hajthat végre növekményes indexelő az elsődleges replikára változott. A feladatátadási esemény lehet, hogy toore-pont hello indexelő toohello új elsődleges replika. 

Ha indexelő nem használja, használja az alkalmazás kódja toopush objektumok és adatok toodifferent keresési szolgáltatások párhuzamosan. További információkért lásd: [teljesítmény- és az Azure Search optimalizálási](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Biztonsági mentés és visszaállítás

Azure Search nincs olyan elsődleges adatok tárolási megoldást, mert a Microsoft nem vállal formális mechanizmusát önkiszolgáló biztonsági mentését és helyreállítását. Az alkalmazás kódjában létrehozására és az index feltöltése használható hello tényleges helyreállítási lehetőség, ha véletlenül törli az index. 

az index, toorebuild ehhez törölje azt (feltéve, hogy létezik-e), hozza létre újra hello indexet hello szolgáltatásban és az elsődleges adattároló az adatok lekérdezése alapján töltse be újra. Másik lehetőségként érhető el túl[ügyfél-támogatási]() toosalvage indexeli a regionális kimaradás esetén.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Felfelé és lefelé skálázás
Minden keresési szolgáltatás kezdődik-e legalább egy másodpéldány és egy partíciót. Ha az egy [réteghez, amelynek biztosít a dedikált erőforrások](search-limits-quotas-capacity.md), hello kattintson **MÉRETEZÉSI** hello szolgáltatás irányítópult tooadjust Erőforrás kihasználtsága csempére.

Bármelyik erőforrás keresztül hozzáadásakor hello szolgáltatás használja őket automatikusan. Semmilyen további műveletet van szükség, de nincs egy kis idő, mielőtt hello hatását hello új erőforrás létrejött. Is igénybe vehet 15 perc vagy több tooprovision további forrásokat.

 ![][10]

### <a name="add-replicas"></a>Adja hozzá a replikák
Lekérdezések / másodperc (QPS) megnövelni, vagy magas rendelkezésre állás elérése érdekében a replikák hozzáadásával történik. Minden egyes replikának egy másolattal rendelkezik index, így tooone hozzáadása egy további replika fordítja le további index elérhető szolgáltatás a lekérdezésekre vonatkozó kérelmek kezelése. Legalább 3 replikák szükségesek a magas rendelkezésre állású (lásd: [kapacitástervezés](search-capacity-planning.md) részletekért).

A keresési szolgáltatást több replikák indexek nagyobb számú betöltheti lekérdezés érkező kérések elosztása. Mivel a lekérdezés kötet, lekérdezés átviteli állapotra vált, toobe gyorsabban amikor hello index elérhető tooservice hello kérelem több példánya. Ha a lekérdezés késést tapasztal, számíthat pozitív hatása a teljesítményre Ha hello további replikák online.

Lekérdezés átviteli következik be, replikák hozzáadásakor, bár ez nem pontosan duplán vagy háromszoros replikák tooyour szolgáltatás való hozzáadása során. Alkalmazások keresése az összes olyan tulajdonos tooexternal a tényezőket, amelyek is hatással vannak a lekérdezések teljesítményét. Összetett lekérdezések és a hálózati késés is két tényező toovariations befolyásolja a lekérdezési válaszidőt.

### <a name="add-partitions"></a>Adja hozzá a partíciók
A legtöbb szolgáltatásalkalmazás nincs további replikák helyett partíciók beépített szüksége. Azokban az esetekben, ahol szükség-e egy nagyobb dokumentumok száma, a partíciók adhat hozzá, ha szabványos szolgáltatáshoz regisztrált. Az alapszintű csomag két további partíció nem ad meg.

Hello Standard csomagra, a partíciók jelentek meg 12 Többszörösök (pontosabban, 1, 2, 3, 4, 6 vagy 12). Ez az összetevő a horizontális. Az index összes partíción 1 vagy 2, 3, 4, 6 vagy 12 partíciók (egy shard partíciónként) egyenlően osztható 12 szilánkok jön létre.

### <a name="remove-replicas"></a>Távolítsa el a replikák
Kötetek a lekérdezési időszak után csökkentése érdekében replikák után keresési lekérdezésekkel rendelkezik normalizált (például követően szünnap értékesítési keresztül).

toodo a, áthelyezés hello replika csúszkát hátsó tooa alacsonyabb értékre. Nincs szükség további lépésekre van. Hello replikaszám lowering elhagyja a virtuális gépek hello adatközpontban. A lekérdezés- és adatfeldolgozást műveletei most a virtuális gépeken kevesebb mint előtt fog futni. hello minimális korlát egy replikát.

### <a name="remove-partitions"></a>Partíciók törlése
Ellentétben eltávolítása replikákat, amely nincs meg a extra erőfeszítést igényel, lehetséges, hogy néhány munkahelyi toodo Ha csökkenteni lehet, mint további tárhelyet használ. Például ha a megoldás három használ, downsizing tooone vagy két partíciót hoz létre hiba esetén hello új tárhely kisebb, mint a szükséges. Ezt a várt, a választott toodelete indexek vagy egy társított index toofree fel helyet a dokumentumokon, illetve tartani hello aktuális konfigurációt.

Nincs jelzi, hogy melyik index szilánkok tárolt adott partíciókra észlelési módszer. Mindegyik partíció körülbelül 25 GB tárhely, nyújt, így a tooreduce tároló tooa méretét elhelyezhetők-hello számú partíciókkal van szüksége lesz. Ha azt szeretné, hogy toorevert tooone partíció, minden 12 szilánkok toofit kell.

a későbbi tervezést toohelp, érdemes lehet toocheck tárolási (használatával [Index statisztika beolvasása](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) mennyi ténylegesen használt toosee. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Gyakorlati tanácsok a méretezés és a központi telepítés
A 30 perces videó ellenőrzi, hogy gyakorlati tanácsok a speciális telepítési forgatókönyvek, beleértve a földrajzilag elosztott munkaterhelések. Azt is láthatja, [teljesítmény- és az Azure Search optimalizálási](search-performance-optimization.md) tartozó súgó adott borítóján hello lapok azonos mutat.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Következő lépések
Miután megismerte a szolgáltatás felügyeleti hello fogalmakat, érdemes lehet [PowerShell](search-manage-powershell.md) tooautomate feladatok.

Javasoljuk továbbá hello megtekintésével [teljesítmény- és optimalizálási cikk](search-performance-optimization.md).

Egy másik javaslat toowatch hello videó hello előző szakaszban leírtaknak. Ebben a szakaszban említett hello technikák mélyebb körét biztosít.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



