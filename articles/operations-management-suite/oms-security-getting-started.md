---
title: "az Operations Management Suite biztonsági és naplózási megoldás lépései aaaGetting |} Microsoft Docs"
description: "A dokumentum segítségével Operations Management Suite biztonsági és hitelesítési megoldás képességek toomonitor a hibrid felhő használatába, tooget."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: get-started-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 5cb3e5dbb3e60f9702a34c9413ddc1bf2b14b411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Az Operations Management Suite biztonsági és auditálási megoldás használatának első lépései
Ez a dokumentum végigkalauzolja Önt az egyes beállításokon, így segít gyorsan használatba venni az Operations Management Suite (OMS) biztonsági és auditálási megoldás képességeit.

## <a name="what-is-oms"></a>Mi az az OMS?
A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében. További információ az OMS hello a cikk elolvasása [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Az OMS biztonsági és auditálási irányítópultja
hello OMS biztonsági, naplózási megoldást nyújt átfogó képet kaphat a szervezet informatikai biztonságot beépített keresési lekérdezések a jelentős figyelmet igénylő problémák. Hello **biztonsági és naplózási** irányítópult hello kezdőképernyő minden kapcsolódó toosecurity az OMS Szolgáltatáshoz. A számítógépek biztonsági állapotának hello magas szintű betekintést biztosít. Is hello képességét tooview összes események az elmúlt 24 óra, 7 nap hello vagy bármely más egyéni időkereten belül. tooaccess hello **biztonsági és naplózási** irányítópult, kövesse az alábbi lépéseket:

1. A hello **a Microsoft Operations Management Suite** fő irányítópultján kattintson **beállítások** csempe az hello balra.
2. A hello **beállítások** panelen, a **megoldások** kattintson **biztonsági és naplózási** lehetőséget.
3. Hello **biztonsági és naplózási** irányítópult jelenik meg:
   
    ![Az OMS biztonsági és auditálási irányítópultja](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Ha nincs OMS által figyelt eszközök fér hozzá ehhez az irányítópulthoz hello először, hello csempék nem tölti fel hello ügynöktől kapott adatokat. Hello ügynök telepítése után is igénybe vehet néhány alkalommal toopopulate, ezért megjelenő kezdetben előfordulhat, hogy hiányzik néhány adat, azok továbbra is feltölteni toohello felhő.  Ebben az esetben is normál toosee néhány tartalmazó csempék éppen úgy anyagi adatok nélkül. Olvasási [csatlakozás Windows rendszerű számítógépek közvetlenül tooOMS](https://technet.microsoft.com/library/mt484108.aspx) további információt a tooinstall OMS-ügynököt a Windows rendszerben és [csatlakozás Linux számítógépek tooOMS](https://technet.microsoft.com/library/mt622052.aspx) további információt a tooperform ezt a feladatot Linux rendszereken.

> [!NOTE]
> hello ügynök hello aktuális eseményeket, amelyek engedélyezve vannak, például a számítógép nevét, IP-cím és a felhasználói név alapján hello adatokat gyűjt. Nem gyűjt azonban dokumentumokat vagy fájlokat, adatbázisneveket, illetve személyes adatokat.   
> 
> 

A megoldások az ügyfelek fontos kihívásaira választ adó logikai, megjelenítési és adatgyűjtési szabályok gyűjteményei. A Biztonság és auditálás is egy ilyen megoldás, további megoldásokat pedig külön adhat hozzá a rendszerhez. A cikk elolvasása hello [megoldások hozzáadása](https://technet.microsoft.com/library/mt674635.aspx) további információt a tooadd egy új megoldás.

hello OMS biztonsági és naplózási irányítópult négy fő kategóriába vannak rendezve:

* **Biztonsági tartományok**: Ez a terület a képes toofurther lesz felfedezés biztonsági rekordok időbeli, kártevő assessment hozzáférni, felmérési és a hálózati biztonság, a identitás- és hozzáférés adatait, a számítógépek biztonsági események frissítse, és gyorsan hozzáférés tooAzure Security Center irányítópultjának.
* **Jelentős problémák**: Ez a beállítás lehetővé teszi a tooquickly aktív problémák hello számának meghatározása, és ezek a problémák súlyosságát hello.
* **Észlelések (előzetes verzió)**: lehetővé teszi a biztonsági riasztások megjelenítése, hogy elvégezze a erőforrásokon által tooidentify támadási mintákat.
* **A fenyegetés Eszközintelligencia**: lehetővé teszi tooidentify támadási mintákat által kimenő kártékony IP-forgalom, hello rosszindulatú fenyegetés típusa és megjelenítő, ha ezeket az IP-címekről érkező kiszolgálók száma hello megjelenítése. 
* **Általános biztonsági lekérdezések**: ezt a lehetőséget biztosít a leggyakrabban használt biztonsági hello listáját, lekérdezi a használható toomonitor a környezetben. Lekérdezések egyikében kattintáskor hello nyílik **keresési** panel hello eredményekkel ehhez a lekérdezéshez.

> [!NOTE]
> Ha további információkat szeretne arról, hogyan védi meg az OMS az adatait, olvassa el az OMS használatával végzett adatvédelmet ismertető cikket.
> 
> 

## <a name="security-domains"></a>Biztonsági tartományok
Források követésével esetén fontos toobe képes tooquickly hozzáférés hello aktuális állapotát a környezetében. Azonban akkor is fontos toobe képes tootrack hátsó események, a hello múltbeli, hogy mi történik a környezetében bizonyos időpontban időben jobban megértheti tooa okozhat. 

> [!NOTE]
> Az adatmegőrzés toohello OMS tarifacsomagot alapján történik. További információt a Microsoft hello [a Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) árképzést ismertető oldalra.
> 
> 

Incidens válasz és a törvényszéki nyomozások forgatókönyvek közvetlenül előnyösek, hello eredmények hello elérhető **biztonsági rekordok időbeli megoszlása** csempére.

![Biztonsági rekordok időbeli megoszlása](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Erre a csempére kattintva hello **keresési** panel nyílik, egy lekérdezés eredményt megjelenítő **biztonsági események** (típus = SecurityEvents) adatok alapján hello elmúlt hét napban, alább látható módon:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Biztonsági rekordok időbeli megoszlása](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

hello keresési eredmény belül két ablaktábla található osztják: hello bal oldali ablaktáblán lehetővé teszi az információkat a hello található, amelyen ezek az események találhatók, hello számát, amelyet észlelt a rendszer ezeket a számítógépeket és hello típusát hello számítógépek biztonsági események száma tevékenységek. jobb oldali hello biztosít hello teljes eredmények és hello biztonsági események hello számítógép nevét és az esemény tevékenységet időrendi nézetben. Is **további megjelenítése** tooview több ezt az eseményt, például hello eseményadatok, hello eseményazonosító és hello eseményforrás vonatkozó részletek.

> [!NOTE]
> Az OMS keresési lekérdezéseivel kapcsolatos további információkért olvassa el az [OMS keresési referenciáját bemutató cikket](https://technet.microsoft.com/library/mt450427.aspx).
> 
> 

### <a name="antimalware-assessment"></a>Kártevőirtók felmérése
A beállítás engedélyezi, hogy Ön tooquickly számítógépeket azonosító nem megfelelő védelemmel és számítógépekkel a kártevő szoftver által kerülnek veszélybe. Hello felhő feldolgozásra kártevő értékelési állapotát és a figyelt hello kiszolgálókon észlelt fenyegetéssel olvassa el, és majd hello az adatok küldése toohello OMS szolgáltatáshoz. Kiszolgálók észlelt fenyegetésekkel és a kiszolgálók nem megfelelő védelemmel láthatók érhető el a hello kattintás után hello kártevő assessment irányítópult **kártevőirtó Assessment** csempére. 

![kártevők felmérése](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Csakúgy, mint bármilyen más elérhető OMS-irányítópulton élő csempét rajta kattintáskor hello **keresési** panel nyílik meg hello lekérdezés eredménye. Ezt a beállítást, ha a hello kattint a **jelentést nem készítő** lehetőség alatt **védelmi állapot**, hello lekérdezés eredményét, amely tartalmazza a egyetlen bejegyzés, amely tartalmazza a hello számítógép neve és az osztályozás, mint hogy alább látható:

![keresési eredmény](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *dimenziószáma* egy besorolási jogosultságot ad az üdvözlő védelmi állapotának tooreflect hello (a, kikapcsolva, frissítése, stb.) és az észlelt fenyegetéseket. Egy szám segít toomake tervezni, amelynek, amely.
> 
> 

Ha hello számítógépnév gombra kattint, akkor hello időrendi nézetben hello védelemben ehhez a számítógéphez. Ez nagyon fontos forgatókönyvek esetén a felhasználónak kell toounderstand Ha hello kártevőirtó korábban volt telepítve, és bármikor el lett távolítva.   

### <a name="update-assessment"></a>Frissítések felmérése
A beállítás lehetővé teszi, hogy Ön tooquickly meghatározásához hello teljes terheléshez toopotential biztonsági problémákat, és kell-e, vagy hogyan kritikus ezeket a frissítéseket a környezethez. OMS biztonsági és naplózási megoldás csak olyan hello képi megjelenítés ezeket a frissítéseket, az adatok valóságosak hello származik [frissítési megoldások](oms-solution-update-management.md), amely egy másik modul OMS belül. Itt egy példát hello frissíti:

![rendszerfrissítések](./media/oms-security-getting-started/oms-getting-started-fig6-new.png)

> [!NOTE]
> A frissítéskezelési megoldással kapcsolatos további információkért olvassa el [az OMS frissítéskezelési megoldását ismertető cikket](oms-solution-update-management.md).
> 
> 

### <a name="identity-and-access"></a>Identitás és hozzáférés
Hello vezérlő kell lennie a vállalati, az identitás védelme kell lennie a legnagyobb prioritással. Elmúlt hello történtek körül szervezetek kialakítását, és ezeket kialakítását volt egy hello elsődleges defenzív határokat, ma további alkalmazások áthelyezése toohello felhőalapú és több adat hello identitás lesz hello új szegélyhálózati. 

> [!NOTE]
> jelenleg hello adatok csak biztonsági események bejelentkezési adatok (event ID 4624) hello jövőbeli Office365 bejelentkezések alapján történik, és az Azure AD-adatokat is szerepelnek.
> 
> 

Az identitás-tevékenységek figyelése, képes tootake megelőző jellegű lépéseket fogja incidens időt vesz igénybe, hely vagy reaktív műveletek toostop támadás kísérlet előtt. Hello **identitások és hozzáférések** irányítópult biztosít a identitás állapota, beleértve a fiókokat, amelyek ki lettek zárva, próbálkozások során használt hello felhasználói fiók a sikertelen kísérletek toolog hello számát áttekintése fiók megváltozott, vagy alaphelyzetbe állítja a jelszót, és jelenleg bejelentkezett fiókok számát. 

Kattintva hello **identitások és hozzáférések** csempe jelenik meg a következő irányítópult hello:

![identitás és hozzáférés](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

hello információk érhetők el az irányítópulton azonnal segít a tooidentify lehetséges gyanús tevékenységet. Például vannak 338 kísérletek toolog **rendszergazda** és a 100 %-át ezeket a kísérleteket nem sikerült. Ezt a fiók ellen intézett találgatásos támadás okozhatja. Ha rákattint az ezt a fiókot akkor lesz tájékoztatást kaphat, amelyek segítséget nyújthatnak toodetermine hello célerőforrása a potenciális támadási:

![keresési eredmények](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

hello részletes jelentés fontos információkat tartalmaz az esemény, például: hello célszámítógépen, hello típusa (az az eset hálózati bejelentkezés) bejelentkezési, hello tevékenység (az az eset esemény 4625) és egyes kísérlet átfogó ütemtervet. 

### <a name="computers"></a>Számítógépek
Ez a csempe lehet használt tooaccess biztonsági események aktívan rendelkező számítógépeket. A csempén kattintva biztonsági események és események száma hello rendelkező számítógépek hello listája jelenik meg minden olyan számítógépen:

![Számítógépek](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Minden olyan számítógépen kattintson a vizsgálat folytatása, és tekintse át a hello biztonsági eseményeket, amelyek volt megjelölve.

### <a name="threat-intelligence"></a>Fenyegetések felderítése

Hello Fenyegetésfelderítési adataival beállítás OMS biztonsági és a naplózási használatával a rendszergazdák biztonsági fenyegetések ellen hello környezetben, például azonosíthatja, azonosítja, hogy egy adott számítógép egy botnet része. Számítógépek egy botnet csomópontjának válhat, a támadók törvénytelenül telepítésekor kártevők, amelyek a számítógép toohello parancs és a vezérlő titokban csatlakozik. A szolgáltatás emellett az illegális kommunikációs csatornákról, például a darknetekről érkező potenciális fenyegetéseket is azonosítani tudja. Olvasásával Fenyegetésfelderítési adataival kapcsolatos további [figyelési és válaszol toosecurity riasztásokat az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md) cikk.

Bizonyos esetekben észlelhet egy potenciálisan kártékony IP-címet, amelyhez az egyik felügyelt számítógépről fértek hozzá:

![fenyegetésfelderítési leképezés](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Ezt a figyelmeztetést, míg mások belül hello ugyanilyen kategóriájú, OMS biztonsági keresztül, ami által előállított [Microsoft Fenyegetésfelderítési adataival](https://youtu.be/O4WtxgUrDc8). hello Fenyegetésfelderítési adataival adatok van a Microsoft által gyűjtött, valamint származó vezető fenyegetés eszközintelligencia szolgáltatók. Az adatok gyakran frissül, és leírtakon toofast áthelyezése fenyegetéseket. Tooits jellege miatt azt kombinálni kell egyéb információforrásokat biztonsági közben [kivizsgálása](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) egy biztonsági riasztást. 

### <a name="baseline-assessment"></a>Az alapkonfiguráció értékelése

A Microsoft a világszerte elhelyezkedő iparági és kormányzati szervezetekkel közösen olyan Windows-konfigurációt határoz meg, amely nagy biztonságú kiszolgálók üzembe helyezését teszi lehetővé. Ez a konfiguráció beállításkulcsokból, auditálási szabályzatok beállításaiból és biztonsági szabályzatok beállításaiból áll, valamint a Microsoft ajánlott értékeit tartalmazza ezekhez a beállításokhoz. Ez a szabályzathalmaz biztonsági alapkonfigurációként ismert. A beállítással kapcsolatos további információkért olvassa el [Az alapkonfiguráció értékelése az Operations Management Suite biztonsági és auditálási megoldásában](oms-security-baseline.md) című cikket.

### <a name="azure-security-center"></a>Azure Security Center
Ez a csempe tulajdonképpen egy helyi tooaccess Azure Security Center irányítópultjának. Ha további információkat szeretne megtudni erről a megoldásról, olvassa el [Az Azure Security Center használatának első lépései](../security-center/security-center-get-started.md) című cikket.

## <a name="notable-issues"></a>Jelentős problémák
hello fő ezen beállítások csoportja célja tooprovide hello problémákat, amelyek azt a környezetben, azokat a kritikus, figyelmeztetés és tájékoztató kategorizálását által gyors áttekintést. hello aktív probléma típusa csempe ezeknek a problémáknak képi megjelenítés, de nem teszi lehetővé róluk, részletesebben tooexplore toouse hello alsó részén található, amely hello neve hello probléma (név), hogy hány objektumok rendelkeztek (DARABSZÁM) ehhez a csempéhez kell, és mennyire fontos, (SÚLYOSSÁG).

![Jelentős problémák](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Láthatja, hogy ezek a problémák már volt tárgyalt hello különböző területei **biztonsági tartományok** csoport, amely ebben a nézetben hello szándékával megerősíti: megjelenítése hello legfontosabb problémák egyetlen helyéről a környezetben.

## <a name="detections-preview"></a>Észlelések (előnézet)
hello fő ezt a beállítást célja tooallow informatikai tooquickly azonosítani a potenciális fenyegetések tootheir környezet keresztül és a fenyegetést hello súlyossága.

![Fenyegetések felderítése](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Ez a beállítás használata során egy [incidensválasz-vizsgálat](https://blogs.msdn.microsoft.com/azuresecurity/2016/11/30/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) tooperform hello értékelési és hello támadás kapcsolatos további információkhoz.

> [!NOTE]
> További információt a toouse OMS az Incidensmegoldási, ezt a videót: [hogyan tooLeverage hello Azure Security Center & a Microsoft Operations Management Suite egy Incidensmegoldási](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).
> 
> 

## <a name="threat-intelligence"></a>Fenyegetések felderítése
az eszközintelligencia szakasz biztonsági és hitelesítési megoldás hello visualizes hello támadási mintákat többféle módon új fenyegetés hello: hello kártékony IP kimenő forgalmat bonyolító kiszolgálók száma, a kártevő-fenyegetés típusát és helyét megjelenítő hello ezeket az IP-címek érkező. Hello térkép kommunikál, és kattintson az IP-címek hello további információt.

Sárga pushpins hello térképen azt jelzi, hogy kártékony IP-címekről érkező bejövő forgalmat. Nincs ritka kiszolgálókat, amelyek kitett toohello toosee bejövő rosszindulatú internetforgalom, de javasoljuk, hogy tekintse át a kísérletek toomake meg arról, hogy az egyik se sikeres volt. Ezek a jelzők az IIS-naplókon, valamint a WireData- és Windows tűzfal-naplókon alapulnak.  

![Fenyegetések felderítése](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Gyakori biztonsági lekérdezések
hello listáját általános biztonsági lekérdezések érhető el, toorapidly hozzáférés erőforrás információk hasznosak lehetnek, és a környezet igényeinek megfelelően testre szabhatja. E gyakori lekérdezések a következők:

* Minden biztonsági tevékenység
* Biztonsági tevékenységek hello számítógép "computer01.contoso.com" (a név felülírandó a saját gép nevével)
* Biztonsági tevékenységek hello számítógép "computer01.contoso.com" fiók "Rendszergazda" (a név felülírandó a saját gép és fiók nevével)
* Bejelentkezési tevékenységek gépenként
* Fiókok, amelyekből leállították a Microsoft Antimalware-t bármely gépen
* Számítógépek, amelyeken leállították a Microsoft antimalware folyamatot hello
* Gépek, amelyeken futtatták a hash.exe fájlt (a név felülírandó a megfelelő folyamat nevével)
* Az összes elindított folyamat
* Bejelentkezési tevékenységek fiókonként
* Fiókok, amelyekből távolról bejelentkeztek hello számítógép "computer01.contoso.com" (a név felülírandó a saját gép nevével)

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban volt bevezetett tooOMS biztonsági, naplózási megoldást. További információ az OMS biztonsági toolearn tekintse meg a következő cikkek hello:

* [Az Operations Management Suite (OMS) áttekintése](operations-management-suite-overview.md)
* [Figyelés és a válaszoló tooSecurity riasztásait az Operations Management Suite biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
* [Az erőforrások figyelése az Operations Management Suite biztonsági és auditálási megoldásban](oms-security-monitoring-resources.md)

