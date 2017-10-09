---
title: "aaaSecurity Center tervezéséhez és üzemeltetési útmutató |} Microsoft Docs"
description: "Ez a dokumentum segít tooplan az Azure Security Center és a napi szintű működése kapcsolatos szempontokat elfogadása előtt."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.openlocfilehash: b0a0a6f5fd56fbd46f7736928c99e3bcd0b1e140
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-planning-and-operations-guide"></a>Útmutató az Azure Security Center tervezéséhez és működtetéséhez
Ez az útmutató informatikai (IT) szakemberek, informatikai fejlesztők, adatbiztonsági elemzők és felhő rendszergazdák tervezik toouse az Azure Security Center is.

>[!NOTE] 
>Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához. Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn. a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>

## <a name="planning-guide"></a>Tervezési útmutató
Ez az útmutató azokat a lépéseket és teendőket, amelyeket követve a Security Center használatát a szervezet biztonsági igényeinek és felhőfelügyeleti modelljének alapján toooptimize tartalmazza. a Security Center teljes mértékben tootake, fontos toounderstand különböző egyéni felhasználók számára, vagy a csoportok a szervezete használja hello szolgáltatás toomeet biztonságos fejlesztésre, működésre, ellenőrzésre, irányításra és incidensmegoldásra vonatkozó követelményeket. hello kulcsfontosságú terület tooconsider toouse Security Center tervezése során a következők:

* Biztonsági szerepkörök és hozzáférés-vezérlés
* Biztonsági szabályzatok és javaslatok
* Adatgyűjtés és -tárolás
* A biztonság folyamatos ellenőrzése
* Incidensmegoldás

Hello a következő szakaszban megtudhatja, hogyan tooplan a fenti területekre és alkalmazza a követelmények alapján javasolt lépéseket.

> [!NOTE]
> Olvasási [gyakran ismételt kérdések (GYIK) az Azure Security Center](security-center-faq.md) listáját, amelyek szintén hasznosak lehetnek hello tervezése és tervezési fázis során gyakori kérdésekre.
> 

## <a name="security-roles-and-access-controls"></a>Biztonsági szerepkörök és hozzáférés-vezérlés
Attól függően, hogy hello méretét és a szervezet felépítését sok ember és csoportokat használhat a Security Center tooperform különböző biztonsági feladatok. A következő diagram hello fiktív személyeknek megfelelő szerepkörök és biztonsági feladataikat például közül választhat:

![Szerepkörök](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Biztonsági központ lehetővé teszi, hogy ezen személyek toomeet e különböző feladatokat. Példa:

**Bálint (felhőbeli számítási feladatok felelőse)**

* A felhő munkaterhelését és kapcsolódó erőforrásait kezeli.
* Feladata a védelmi megoldások megvalósítása és kezelése a vállalat biztonsági szabályzatának megfelelően.

**Eszter (adatvédelmi felelős/számítástechnikai felelős)**

* Hello vállalati biztonság minden szempontját felelős
* Toounderstand hello vállalati biztonságot szeretne rendelni a felhő-munkaterhelések között
* Fő támadások és a kockázatok toobe kell

**András (számítástechnikai biztonsági felelős)**

* A vállalati tooensure hello megfelelő védelmet vezessen be olyan biztonsági házirendek beállítása
* Ellenőrzi a szabályzatok betartását.
* Jelentéseket készít a vezetőség és az auditorok számára.

**Judit (biztonsági műveletek felelőse)**

* Figyeli és reagál toosecurity riasztások 24 és Windows 7 rendszerben
* Eszkalálja tooCloud számítási feladatok felelőse vagy informatikai biztonsági elemző

**Sándor (biztonsági elemző)**

* Kivizsgálja a támadásokat.
* Felhőbeli számítási feladatok felelőse tooapply szervizelési használata 

A Security Center által használt [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md), amely biztosítja [beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md) , amely a toousers, csoportok és az Azure rendelhetők. Amikor a felhasználó megnyitja a Security Center, ezek csak részletek tooresources rendelkeznek hozzáféréssel kapcsolatos. Ami azt jelenti, hogy hello felhasználói szerepét hello tulajdonos, közreműködő vagy olvasó toohello előfizetés vagy az erőforrás csoport, amely egy erőforrás tartozik. Továbbá toothese szerepkörök szerepkör is létezik két adott Security Center:

- **Biztonsági olvasó**: toothis szerepkörhöz tartozó felhasználó van képes tooview jogok tooSecurity központ, amely tartalmazza a javaslatok, a riasztások, a házirendhez és a állapotát, de, nem fogja tudni toomake módosítások.
- **Biztonsági rendszergazda**: ugyanaz, mint biztonsági olvasó, de az is frissítheti hello biztonsági házirend, hagyja figyelmen kívül javaslatokra és riasztásokra.

a fent leírt hello Security Center szerepkörök nincs hozzáférési tooother szolgáltatás területéhez Azure például tárolóhely & webes, mobil vagy az eszközök internetes hálózatát.  

> [!NOTE]
> A felhasználó számára legalább egy előfizetés, erőforrás csoport tulajdonosa vagy közreműködője toobe képes toosee az Azure Security Center toobe szükséges. 
> 
> 

Hello személyeknek használ, tekintse meg a hello előző ábrán, következő RBAC szükséges hello:

**Bálint (felhőbeli számítási feladatok felelőse)**

* Erőforráscsoport: tulajdonos/közreműködő

**András (számítástechnikai biztonsági felelős)**

* Előfizetés: tulajdonos/közreműködő vagy biztonsági rendszergazda

**Judit (biztonsági műveletek felelőse)**

* Előfizetés: olvasó vagy biztonsági olvasó tooview riasztások
* Előfizetés: tulajdonos/közreműködő vagy Security Admin szükséges toodismiss riasztások

**Sándor (biztonsági elemző)**

* Előfizetési olvasó tooview riasztásokat
* Előfizetés: tulajdonos/közreműködő a riasztások toodismiss szükséges
* Szükség lehet a hozzáférés toohello munkaterület

Néhány más fontos információk tooconsider:

* A biztonsági szabályzatot kizárólag az előfizetésnél Tulajdonos/Közreműködő vagy Biztonsági rendszergazda szerepkörrel rendelkező személyek módosíthatják
* Az erőforrásra kizárólag az előfizetésnél és az erőforráscsoportnál Tulajdonos és Közreműködő szerepkörrel rendelkező személyek alkalmazhatják a biztonsági javaslatokat

Hozzáférés-vezérlés az RBAC használata a Security Center tervezésével, lehet, hogy toounderstand, aki a szervezet biztonsági központ használatával. Ezenkívül tudnia kell azt, hogy ezek a személyek milyen típusú feladatokat fognak végrehajtani, és ennek megfelelően kell beállítania a szerepköralapú hozzáférés-vezérlést.

> [!NOTE]
> Azt javasoljuk, hogy rendelje hello legkevésbé megengedő szerepkörre felhasználók toocomplete a feladataik ellátásához szükséges. Például felhasználók, akik csak az erőforrások biztonsági állapotát hello tooview információra van szüksége, de nem tesznek lépéseket, például a alkalmazhatnak javaslatokat, és módosíthatják a szabályzatokat rendelhető hello olvasó szerepkört.
> 
> 

## <a name="security-policies-and-recommendations"></a>Biztonsági szabályzatok és javaslatok
A biztonsági házirend vezérlők, javasolt a megadott hello erőforrások hello csoportját határozza meg előfizetés. A biztonsági központban állíthatja be a szabályzatokat tooyour vállalati biztonsági követelmények és az alkalmazások hello típusának vagy hello adatok érzékenységének megfelelően.

Házirendek, amelyek engedélyezve vannak az előfizetési szinten hello automatikusan tooall erőforráscsoport hello előfizetésen belül propagálására, ahogy az ábra a következő hello:

![Biztonsági szabályzatok](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Ha a módosított szabályzatok tooreview van szüksége, használhatja [Azure-beli Auditnaplók](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Az Azure naplói minden esetben rögzítik a szabályzatok változásait.
> 
> 

### <a name="security-recommendations"></a>Biztonsági javaslatok
Biztonsági szabályzatok konfigurálása előtt tekintse át az összes hello [biztonsági javaslatok](security-center-recommendations.md), és határozza meg, hogy ezek a házirendek a különböző előfizetésekhez és erőforráscsoportokhoz megfelelőek. Célszerű is fontos toounderstand mit kell tenni tooaddress [biztonsági javaslatok](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) és hogy a vállalatnál ki felelős megfigyelésének új ajánlást kapott és véve hello szükséges lépéseket.

A Security Center javasolni fogja, hogy adja meg a biztonsági kapcsolattartó adatait az Azure-előfizetéséhez. Ezt az információt a Microsoft toocontact által használható, ha a Microsoft biztonsági válasz Center (MSRC) hello észleli, hogy az az ügyféladatok egy törvénybe ütköző vagy jogosulatlan félnek elérhető-e. Olvasási [adja meg az Azure Security Centerben a biztonsági kapcsolattartási adatait](security-center-provide-security-contact-details.md) további információt a tooenable Ez a javaslat.

## <a name="data-collection-and-storage"></a>Adatgyűjtés és -tárolás
Az Azure Security Center által használt hello Microsoft Monitoring Agent – Ez az Operations Management Suite hello és Naplóelemzés szolgáltatás – toocollect biztonsági adatait, a virtuális gépek által használt ugyanannak az ügynöknek hello. Az ebből az ügynökből gyűjtött adatokat a Log Analytics munkaterület(ek)en tárolja a rendszer.

### <a name="agent"></a>Ügynök

Miután az adatgyűjtés hello biztonsági házirend engedélyezve van, a Microsoft Monitoring Agent hello (a [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) vagy [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) telepítve van minden támogatott Azure virtuális gépeken, majd minden létrehozott újakat.  Ha a virtuális gép már van telepítve, a Microsoft Monitoring Agent hello hello Azure Security Center aktuális hello fogja használni, az ügynök telepítése. hello ügynök folyamat tervezett toobe nem invazív, és nagyon minimális hatása van a virtuális gép teljesítményére.

a Microsoft figyelési ügynök a Windows hello használata TCP 443-as portot igényli. Lásd: hello [hibaelhárítási cikk](security-center-troubleshooting-guide.md) további részleteket.

Ha egy bizonyos ponton toodisable adatok gyűjtését, kikapcsolhatja azt hello biztonsági házirendben. Azonban mivel a Microsoft Monitoring Agent hello felhasználhatja más Azure felügyeleti és -szolgáltatások figyelésének hello ügynök nem lesz eltávolítva automatikusan kikapcsolása esetén adatgyűjtés a Security Center. Hello ügynök manuálisan távolítsa el, ha szükséges.

> [!NOTE]
> olvassa el a hello támogatott virtuális gépek listájának toofind [gyakran ismételt kérdések (GYIK) az Azure Security Center](security-center-faq.md).
> 

### <a name="workspace"></a>Munkaterület

A Microsoft Monitoring Agent (nevében az Azure Security Center) fogja tárolni egy meglévő Naplóelemzési munkaterületek hello során gyűjtött adatok társított Azure-előfizetése vagy egy új munkaterületek figyelembe véve a fiók hello hello VM a földrajzi. 

Hello Azure-portálhoz böngészhet a toosee a Naplóelemzési munkaterület, így azokat az Azure Security Center által létrehozott listáját. Az új munkaterületekhez kapcsolódó erőforráscsoport fog létrejönni. Mindkettő ezt az elnevezési konvenciót követi: 

* Munkaterület: *DefaultWorkspace-[subscription-ID]-[geo]*
* Erőforráscsoport: *DefaultResouceGroup-[geo]*

Az Azure Security Center által létrehozott munkaterületek adatait 30 napig őrzi meg a rendszer. Való kilépés munkaterületek, megőrzés hello munkaterület tarifacsomagjának alapul.

> [!NOTE]
> Microsoft erős kötelezettségvállalások tooprotect hello adatvédelmi és biztonsági adatot ellenőrizze. A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból. Az adatkezeléssel és adatbiztonsággal kapcsolatos további információkért olvassa el az [Azure Security Center adatbiztonság](security-center-data-security.md) című cikket.
> 

## <a name="ongoing-security-monitoring"></a>A biztonság folyamatos ellenőrzése
Kezdeti konfigurációs és a Security Center javaslatait alkalmazása után hello következő lépés a Security Center működési folyamatok tervezi.

hello Azure-portálon kattintson a Security Center tooaccess **Tallózás** és típus **Security Center** hello a **szűrő** mező. hello nézetek, hogy hello felhasználói lekérdezi toothese alkalmazott szűrők alapján történnek, hello az alábbi példa tartalmazza egy olyan környezetben sok problémák toobe címzett:

![irányítópult](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> A Security Center nem akadályozza a megszokott eljárások, azok passzívan ellenőrzi az üzemelő példányokat, és engedélyezte a hello biztonsági házirendek alapján ajánlani.

Amikor először csatlakozott a Security Center toouse a jelenlegi Azure-alapú környezetben, győződjön meg arról, hogy tekintse át a hello teheti összes javaslatot **javaslatok** csempe illetve erőforrásonként (**számítási** **Hálózati**, **adatok & tárterület**, **alkalmazás**).

Amennyiben az összes ajánlásokat hello **megelőzési** szakasznak zölden összes erőforrás foglalkozott kell. Folyamatos figyelés ezen a ponton egyszerűbbé válik óta vesz igénybe az erőforrás biztonsági állapotot és javaslatokat tartalmazó csempék hello változásairól alapuló intézkedések kezdeményezésére.

Hello **észlelési** szakasz gyakrabban, ezek a most aktuálisan vagy korábbi hello történt, és a Security Center vezérlők és a 3. fél rendszerek által észlelt problémákkal kapcsolatos riasztások. hello biztonsági riasztások csempén oszlopdiagramok láthatók naponta, és azok hello fenyegetésészlelési kategóriák (alacsony, közepes, magas) szerinti eloszlását található figyelmeztetések hello számát jeleníti meg. Biztonsági riasztások kapcsolatos további információkért olvassa el a [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Kihasználhatja a Microsoft Power BI toovisualize a Security Center adataiba. Lásd: [Get insights from Azure Security Center data with Power BI](security-center-powerbi.md) (Mélyebb betekintés az Azure Security Center adataiba a Power BI segítségével).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Új vagy módosult erőforrások keresése
Az Azure-környezetek általában dinamikusan változnak: új erőforrások jönnek létre és szűnnek meg, módosulnak a konfigurációk és így tovább. A Security Center segítségével, győződjön meg arról, hogy rendelkezik-e az új erőforrások biztonsági állapotának hello láthatósága.

Amikor új erőforrásokat (virtuális gépeket, SQL-adatbázisok) tooyour Azure-környezethez, a Security Center automatikusan felfedezi és megkezdéséhez toomonitor a biztonsági. Ide tartoznak a PaaS webes és feldolgozói szerepkörei is. Ha engedélyezve van a használatra vonatkozó adatok gyűjtésének a hello [biztonsági házirend](security-center-policies.md), további figyelési képességek automatikusan engedélyezve lesz a virtuális gépek számára.

![Fontos területek](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. A virtuális gépeknél a **Megelőzés** szakaszban kattintson a **Számítás** lehetőségre. Hello javaslatokhoz tartozó problémák adatok vagy az azzal kapcsolatos **áttekintése** fülre, és **figyelési javaslatok** szakasz.
2. Nézet hello **javaslatok** toosee mi, az esetleges biztonsági kockázatok azonosított hello új erőforrás.
3. Nagyon gyakori, hogy amikor új virtuális gépek tooyour környezet, csak hello operációs rendszer van telepítve. hello erőforrás tulajdonosa előfordulhat némi időt toodeploy más alkalmazásokat, amelyek a virtuális gépek által használható.  Ideális esetben érdemes tudni hello munkaterhelés végső céljával. Akkor lesz toobe alkalmazást az alkalmazáskiszolgálóra? Milyen ennek alapján új alkalmazások és szolgáltatások folyamatos toobe, engedélyezheti a megfelelő hello **biztonsági házirend**, vagyis a munkafolyamat harmadik lépése hello.
4. Új erőforrás hozzáadása tooyour Azure-környezetéhez, akkor előfordulhat, hogy új riasztások jelennek meg hello **biztonsági riasztások** csempére. Mindig győződjön meg arról, ha vannak-e új riasztások a csempén, és tegye szerint tooSecurity Center javaslatait.

Érdemes tooregularly hello figyelőállapot meglévő erőforrások tooidentify konfigurációs módosítások hozott létre a biztonsági kockázatokat, mivel a javasolt alapkonfigurációkkal, és a biztonsági riasztások is. Hello Security Center irányítópultjának kezdjék. Ott, hogy három főbb területet tooreview rendszeresen áttekintenie.

![Műveletek](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Hello **megelőzési** szakasz panelen gyorsan elérheti őket tooyour fontos erőforrásokat biztosít. Használja ezt a beállítást toomonitor számítási, hálózati, tárolási & adatokhoz és alkalmazásokhoz.
2. Hello **javaslatok** lehetővé teszi, tooreview Security Center javaslatait. Az ellenőrzés során azt tapasztalhatja, hogy nem állnak naponta, ez nem jelent problémát, mivel a kezdeti beállítás hello a Security Center minden javaslattal foglalkozott. Emiatt nem lehet új információt ebben a szakaszban naponta, és így csak olyankor kell tooaccess, igény szerint.
3. Hello **észlelési** szakasz egy nagyon gyakori, vagy rendkívül ritkán alapján változhatnak. Mindig tekintse meg a biztonsági riasztásokat, és tegye meg a Security Center javaslatai szerinti lépéseket.

## <a name="incident-response"></a>Incidensmegoldás
A Security Center észleli, és riasztást küld, toothreats azok bekövetkezésekor. Szervezetek kell figyeli, hogy új biztonsági riasztásokat, és hajtsa végre a műveletet, mint a további szükséges tooinvestigate vagy hello támadás elhárít. Ha részletes tájékoztatást szeretne kapni a Security Center fenyegetésészlelési funkciójának működéséről, olvassa el az [Azure Security Center detection capabilities](security-center-detection-capabilities.md) (Az Azure Security Center észlelési funkciói) című cikket.

Amíg ez a cikk nem hello leképezési tooassist meg saját Incidensmegoldási terv létrehozása, fogjuk toouse Microsoft Azure biztonsági válasz hello felhő életciklusának hello foundation, az incidensekre adott reakciók fázisból áll. hello szakaszból hello a következő ábrán láthatók:

![Gyanús tevékenység](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Hello nemzeti szabványügyi és technológiai Intézet (NIST) is használhat [számítógép biztonsági incidens kezelése útmutató](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) egy hivatkozási tooassist, akkor országos.
> 

Használhatja a Security Center riasztásait során a következő szakaszok hello:

* **Észlelés**: a gyanús tevékenységek azonosítása egy vagy több erőforrásban. 
* **Értékeléséhez**: hajtsa végre a hello kezdeti assessment tooobtain hello gyanús tevékenységek további információt.
* **Diagnosztizálja**: hello javítási lépéseket tooconduct hello műszaki eljárás tooaddress hello probléma használja.

Minden egyes biztonsági figyelmeztetés információkat tartalmaz, amelyek használható toobetter hello támadás természetét hello megértéséhez, és felajánlja a lehetséges megoldást. Egyes riasztásokban további információkat vagy tooother információforrásokat Azure-ban is megadhatja hivatkozások tooeither. Hello információk további kutatási és toobegin megoldás, és biztonsági a munkaterületen tárolt adatokat is kereshet.

hello alábbi példa bemutatja egy gyanús RDP-tevékenységre figyelmeztető üzenetre:

![Gyanús tevékenység](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Ahogy látja, ezen a panelen hello időre vonatkozó részletes adatainak megjelenítése, hogy hello támadás történt, hello forrás állomásnevére, cél virtuális gép hello és javasolt következő lépésre. Bizonyos körülmények között hello adatforrásra vonatkozó információ hello támadások üres is lehet. Ezzel kapcsolatban további információkat talál a [Missing Source Information in Azure Security Center Alerts](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) (Hiányzó forrásadatok az Azure Security Center riasztásaiban) című cikkben.

A hello [hogyan tooLeverage hello Azure Security Center & a Microsoft Operations Management Suite egy Incidensmegoldási](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) videó láthatja, melyek segíthetnek a Security Center hogyan használható az egyes toounderstand néhány bemutatók Ezek a szakaszok egyikét.

> [!NOTE]
> Olvasási [Leveraging az Azure Security Center Incidensmegoldási](security-center-incident-response.md) toouse Security Center képességek tooassist meg során az Incidensmegoldási folyamat további információt. 
> 
> 

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan tooplan a Security Center bevezetését. További információ a Security Center toolearn hello következő lásd:

* [Kezelése és reagálás toosecurity értesítések az Azure Security Centerben](security-center-managing-and-responding-alerts.md)
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

