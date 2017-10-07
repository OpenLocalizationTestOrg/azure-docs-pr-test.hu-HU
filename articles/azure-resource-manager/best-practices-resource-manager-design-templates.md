---
title: "aaaDesign Azure sablonok összetett megoldások |} Microsoft Docs"
description: "Azt mutatja be ajánlott eljárások az Azure Resource Manager sablonok összetett forgatókönyvek tervezése"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: aa45e9a46d79a6336b696cff8fd37f65fa449f11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Az Azure Resource Manager-sablonok összetett megoldások telepítésekor tervezési minták
Egy rugalmas megközelítéssel Azure Resource Manager-sablonok alapján telepítheti a komplex topológiákba gyorsan és következetesen legyenek. A központi telepítéseket könnyen ajánlatok fejlődnek core vagy halegyedeket esetek vagy az ügyfelek tooaccommodate változatait, módosíthatja.

Ez a témakör egy nagyobb tanulmány részét képezi. Töltse le a teljes papír tooread hello [globális osztály Azure Resource Manager sablonok szempontok és eljárások bizonyítása](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

Sablonok együttesen alapjául szolgáló Azure Resource Manager hello alkalmazkodás és a JavaScript Object Notation (JSON) olvashatóság hello hello előnyeit. Sablonok használatával:

* Topológiákat és a munkaterhelések következetesen kell telepíteni.
* Erőforráscsoportok használata az alkalmazásban az erőforrások kezelése.
* Szerepköralapú hozzáférés-vezérlést (RBAC) toogrant megfelelő hozzáférési toousers, a csoportok és a szolgáltatások alkalmazni.
* Címkézési társítások toostreamline feladatokat, mint a számlázási kumulatív használja.

Ez a cikk részletesen fogyasztás forgatókönyvek, architektúra és a Tervező munkamenetek és az Azure Ügyféltanácsadói csapatának (AzureCAT) ügyfelekkel valós sablon megvalósítások során feltárt megvalósítási minták. Messze academic, ezek a módszerek bizonyítottan 12 hello felső Linux-alapú OSS technológiák, például sablonokat hello fejlesztésének figyelmeztet eljárásokat: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, alapszintű DataStax vállalati technológiával Apache Cassandra, Elasticsearch, Jenkins, MongoDB, PostgreSQL, Redis és Nagios. 

Ez a cikk a bevált gyakorlat toohelp meg globális osztály Azure Resource Manager-sablonok tervezővel megosztja.  

A munka az ügyfelek, a vállalatok számára, a rendszer kiegészítők (SI) s és a megosztott fürtkötetek több Resource Manager sablon felhasználásának követelménye azonosította azt. hello következő szakaszokban gyakori forgatókönyvek és minták magas szintű áttekintését különböző vevő típusokhoz.

## <a name="enterprises-and-system-integrators"></a>A vállalatok és rendszerintegrátorok
Nagy méretű szervezeteknek belül általában látható Resource Manager-sablonok két fogyasztóinak: belső szoftver a fejlesztési csapat és a vállalati informatikai. Azt, hogy hello SIs hello forgatókönyvei toohello forgatókönyvek hozzárendelését a vállalatok, talált Igen hello ugyanazok a feltételek vonatkoznak.

### <a name="internal-software-development-teams"></a>Belső szoftver a fejlesztési csapat
A csapat házon belül fejlesztett alkalmazásokra szoftver toosupport az üzleti, ha a sablonok tooquickly telepítése technológiákat biztosítja a megoldások üzleti-specifikus könnyű megoldást biztosítson. Is használhatja a sablonok toorapidly létrehozása képzési környezetek, amelyek lehetővé teszik a csapat tagjai toogain szükséges képességek.

Használhatja a sablont,- vagy kiterjesztése vagy tooaccommodate állítható össze az igényeinek. A sablonokon belüli címkézést, megadhatja számlázási összefoglaló különböző nézetekkel például team projekt, egyéni vagy oktatási.

Vállalatok gyakran érdemes szoftverfejlesztői csapatának toocreate egységes telepítési a megoldás sablonját. hello sablon megkötések könnyíti meg úgy, hogy a környezetben lévő egyes elemek rögzítettek, és nem bírálható felül. Például egy bank előfordulhat, hogy a sablon tooinclude RBAC, programozók nem vizsgálja felül a banki megoldás toosend adatok tooa személyes tárfiók.

### <a name="corporate-it"></a>Vállalati informatikai
Vállalati informatikai szervezetek általában a sablonokkal kézbesítéséhez felhő kapacitása és a felhőben üzemeltetett képességeket.

#### <a name="cloud-capacity"></a>Felhő kapacitása
Közös megoldást, hogy a vállalati informatikai csoportok tooprovide felhő kapacitása csoportjai "pólóra méretek", amelyek szabványos ajánlat például kis, közepes méretű és nagy jelenti. hello méretű pólóra kérésajánlatok ugyanakkor biztosítható a egy szint, amely megkönnyíti a lehetséges toouse sablonok szabványosítás kombinálhatja a különböző típusú és mennyiségben. hello sablonok kapacitás biztosításához, érvényesíti a vállalati házirendeket és címkézési tooprovide jóváírási tooconsuming szervezetek konzisztens módon.

Például szükség lehet tooprovide fejlesztési, tesztelési vagy belül mely hello szoftver a fejlesztési csapat megoldásuk telepítheti termelési környezetben. hello környezet rendelkezik egy előre definiált hálózati topológia és a szoftver a fejlesztési csapat hello elemek nem módosíthatók, például a szabályok hozzáférés toohello nyilvános internet és a csomag végzett vizsgálat végrehajtásához. Hello környezet különböző hozzáférési jogokkal rendelkező ilyen környezetben a szervezet-specifikus szerepköröket is rendelkezhetnek.

#### <a name="cloud-hosted-capabilities"></a>Felhőben üzemeltetett képességek
Sablonok toosupport felhőben üzemeltetett funkcióinak használata, beleértve az egyes szoftvercsomagok vagy összetett ajánlatok felkínált toointernal üzletágak. Példa egy összetett ajánlat lenne elemzés,--szolgáltatás – elemzés, adatmegjelenítési és más technológiák – előre meghatározott hálózati topológia optimalizált, csatlakoztatott konfigurációban kézbesíteni.

Hello biztonsági és a szerepkör szempontok szerint hello felhő kapacitása kínál, amelyek azok van építve a felhőben üzemeltetett képességek van hatással. Ezek a képességek eredeti formájukban, vagy egy felügyelt szolgáltatásként érhető el. Az utóbbi hello korlátozott hozzáférés-szerepkörök szükségesek tooenable hozzáférés hello környezetbe felügyelet céljából.

## <a name="cloud-service-vendors"></a>Cloud service szállítók
A CSV-k toomany van szó, után felismertük több megközelítés közül választhat az ügyfelekkel és a hozzá tartozó követelmény toodeploy szolgáltatásokat.

### <a name="csv-hosted-offering"></a>Fürt megosztott kötetei szolgáltatás által üzemeltetett ajánlat
Az ajánlat működteti a saját Azure-előfizetéssel, ha két üzemeltetési szempontok megegyeznek: minden ügyfél esetében a különböző központi telepítési üzembe, vagy a méretezési egység, amelyek az összes ügyfél számára használja a megosztott infrastruktúra telepítése.

* **Különböző központi telepítések minden ügyfél esetében.** Egy ügyfél különböző telepítés a különböző ismert konfigurációk rögzített topológiák igényel. Előfordulhat, hogy különböző virtuális gépek (VM) méretét, a változó számú csomópontok és a társított tárolási különböző mennyiségű történő központi telepítését. A központi telepítések címkézés összesítő számlázási minden ügyfél szolgál. Az RBAC engedélyezett tooallow ügyfelek hozzáférési tooaspects a felhőalapú környezet lehet.
* **Méretezési egységek megosztott több-bérlős környezetekben.** A sablon jelenthet a méretezési egység, több-bérlős környezetekben. Ebben az esetben az ugyanazon az infrastruktúrán hello használt toosupport az összes ügyfél számára. hello központi telepítések határoz meg, hogy a kapacitás hello üzemeltetett kínál, például a felhasználók száma és a tranzakciók száma a szintű erőforrások csoportja. A méretezési egységeket vannak növelhető vagy csökkenthető, igény szerint van szüksége.

### <a name="csv-offering-injected-into-customer-subscription"></a>Ügyfél-előfizetést be a nézetmodellbe, CSV ajánlat
Előfordulhat, hogy a kívánt toodeploy a szoftverek végfelhasználók által birtokolt előfizetések. Is sablonok toodeploy különböző központi telepítéseket használ, az ügyfél Azure fiókjába.

A központi telepítéseket az RBAC használatát, így frissítése és hello felhasználói fiókon belül hello telepítések kezelése.

### <a name="azure-marketplace"></a>Azure Piactér
tooadvertise és eladásra ajánlatok létrehozásában a piactéren, például az Azure piactéren keresztül, a sablonok toodeliver különböző típusait ügyfél Azure fiókjában fejleszthet. A különböző központi telepítéseket általában jelentheti egy Póló mérete (kis, közepes, nagy), a termék/célközönség típusa (közösségi, fejlesztők, vállalati) vagy a szolgáltatás típusa (alapszintű, a magas rendelkezésre állású).  Néhány esetben ezek a típusok lehetővé teszik toospecify bizonyos attribútumai hello központi telepítést végez, például Virtuálisgép-típussá vagy lemezek számát.

## <a name="oss-projects"></a>OSS-projektek
Nyílt forráskódú projekteket belül a Resource Manager-sablonok egy megoldást gyorsan használatával bevált gyakorlatok engedélyezése egy közösségi toodeploy. Sablonok tárolhatja a GitHub-tárházban, így hello közösségi felülvizsgálhatja őket az adott idő alatt. Felhasználók a saját Azure-előfizetések sablonok telepítése.

hello alábbi szakaszok ismertetik a megoldás tervezése előtt kell tooconsider hello dolgot.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Kívül és belül a virtuális gépek azonosítása
A sablon tervezéséhez, hasznos toolook: hello követelmények tekintetében kívül és belül hello virtuális gépek (VM):

* Külső azt jelenti, hogy a virtuális gépek és más erőforrások, például a hello hálózati topológia, címkézéssel, hivatkozások toohello Tanúsítványos/titkos kulcsok és a szerepköralapú hozzáférés-vezérlés a telepítés hello. Ezeket az erőforrásokat a sablon a részét képezik.
* Belső: hello telepített szoftvereket, és a teljes szükséges konfigurációs állapot. Más módszerek, például Virtuálisgép-bővítmények vagy parancsfájlok, részben vagy egészben szolgálnak. Ezek a mechanizmusok azonosítását és hello sablon hajtja végre, de nem található.

Tevékenységek kellene tennie "belső hello kezdő" közös például-  

* Telepítése vagy eltávolítása a kiszolgálói szerepkörök és szolgáltatások
* Telepítse és konfigurálja a szoftverfrissítési hello csomópont vagy a fürt szintjén
* Egy webkiszolgálón webhelyek központi telepítése
* Adatbázis-sémák telepítése
* Beállításjegyzék vagy egyéb típusú konfigurációs beállítások kezelése
* Fájlok és könyvtárak kezelése
* Indítás, Leállítás, és a folyamatok és szolgáltatások kezelése
* Helyi csoportok és felhasználói fiókok kezelése
* Telepítheti és kezelheti a csomagokat (.msi, .exe, yum, stb.)
* Környezeti változók kezelése
* Natív parancsfájlt (a Windows PowerShell, bash, stb.)

### <a name="desired-state-configuration-dsc"></a>Kívánt állapot konfigurációs (szolgáltatása DSC)
Szeretné végezni a virtuális gépek telepítési túl hello belső állapotával kapcsolatos, a toomake meg arról, hogy a központi telepítés nem "sodródni" hello konfigurációról, hogy definiálva, és be van jelölve a verziókövetési rendszerrel. Ez a megközelítés biztosítja a fejlesztők számára, vagy műveleti személyzet nem ellenőrizze alkalmi módosítások tooan környezet nem vetted, tesztelni, vagy tárolja, amely a verziókövetési rendszerrel. A vezérlő fontos, mert hello manuális módosítások nem szerepelnek a verziókövetési rendszerrel. Is nem hello normál telepítés része, és hatással lesz a jövőbeni automatizált regisztrációt hello szoftver.

A belső munkatársra túl kívánt állapot konfigurációs az is fontos biztonsági szempontból. A támadók rendszeresen próbált toocompromise, és szoftver rendszerek kihasználására. Ha sikeres, közös tooinstall fájlok, és egy fertőzött rendszer egyéb hello állapotváltozáshoz. Kívánt állapot konfigurációs, segítségével szükséges hello és tényleges állapot közötti eltérések azonosítására és egy ismert beállításainak visszaállítása.

Nincsenek hello legnépszerűbb mechanizmusok DSC - a PowerShell DSC, Chef vagy Puppet erőforrás-bővítményt. Ezek a bővítmények mindegyikének hello a virtuális gép kezdeti állapotában telepítheti és is használt toomake meg arról, hogy hello szükséges állapot megmarad.

## <a name="common-template-scopes"></a>Közös sablon hatókörök
Az a tapasztalat világossá három fő megoldás sablonok hatókörök is láttuk. – A kapacitás, a funkció és a végpont megoldás – ezek három hatókör hello a következő szakaszok ismertetik.

### <a name="capacity-scope"></a>A kapacitás hatókör
A kapacitás hatókör nyújt több erőforrást, amely előkonfigurált toobe szabályozások és szabályzatok betartása szabványos topológiában. hello leggyakoribb például egy vállalati informatikai vagy SI forgatókönyvben szabványos fejlesztői környezetben telepít.

### <a name="capability-scope"></a>Képesség hatókör
A funkció hatókör központi telepítését és konfigurálását a topológia egy adott technológia összpontosít. Gyakori forgatókönyvek, például az SQL Server, Cassandra, Hadoop technológiák többek között.

### <a name="end-to-end-solution-scope"></a>Végpontok közötti megoldás hatókör
A végpont megoldás hatókörök megcélzott túl egyetlen funkció, és helyette arra irányul, hogy egy több képességek áll end tooend megoldást hoz létre.  

A megoldás hatókörű sablon hatóköréhez ügyfélrendszer egy vagy több képesség hatókörbe tartozó sablonokat Megoldásfüggő erőforrásokat, a logikai és a kívánt állapot készletként. Megoldás hatókörű sablon példa: egy záró tooend csővezeték megoldás sablon. hello sablon előfordulhat, hogy keverje Megoldásfüggő topológia és állapotának például Kafka, a Storm és a Hadoop több megoldás funkció hatókörbe tartozó sablonokat.

## <a name="choosing-free-form-vs-known-configurations"></a>Szabad formátumú és ismert konfigurációk kiválasztása
Először érdemes egy sablont kell rugalmasan fogyasztók hello lehető legnagyobb mértékben, de sok szempontjai hatással hello lehetőségét toouse szabad formátumú konfigurációk és beállításainak. Ez a szakasz azonosítja a hello kulcs felhasználói követelmények és műszaki olyan szempontot, amelyek a jelen dokumentum megosztott hello megközelítés alakú.

### <a name="free-form-configurations"></a>Szabad formátumú konfigurációk
Hello felületen szabad formátumú konfigurációk hangkártya ideális. Ezek lehetővé teszik egy Virtuálisgép-típussá tooselect és adjon meg egy tetszőleges számú csomópontot és csatlakoztatott lemezekkel azokat a csomópontokat, a – és a paraméterek tooa sablonként ehhez. Ezt a módszert azonban bizonyos forgatókönyvek nem ideális el.

A [virtuális gépek méretei](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), hello különböző Virtuálisgép-típusokon és elérhető méretek azonosítja, és egyes tartós hello száma lemezek, (2, 4, 8, 16 vagy 32), amely csatolható. Minden csatlakoztatott lemezre 500 IOPS biztosít, és ezeknek a lemezeknek többszörösei lehet közösen egy szorzóval IOPS adott számú lehet. Például 16 lemez lehet készletezett tooprovide 8000 iops-érték. Készletezését hello operációs rendszerben, a Microsoft Windows tárolóhelyek vagy a redundáns tömbjének (RAID) olcsó lemezek Linux konfigurációval történik.

Egy szabad formátumú konfiguráció lehetővé teszi hello kijelölés Virtuálisgép-példányok több, különböző virtuális gép meg kell adnia, és az ezeken a példányokon hello Virtuálisgép-típussá, különböző lemez méretének és egy vagy több parancsfájl tooconfigure hello virtuális gép tartalmát.

Esetében gyakori, hogy a központi telepítés lehetnek többféle típusú csomópontok, például a fő és adatcsomópontokat, úgy erre a rugalmasságra gyakran esetén minden csomópont-típus van megadva.

A kezdési toodeploy fürtök bármely jelentősége, az összetett forgatókönyvekben toowork megkezdése. Ha egy Hadoop-fürt volt telepítése, például 8 főcsomópont és 200 adatcsomópontokat és készletezett 4 csatolt minden egyes főcsomópont és készletezett 16 csatlakoztatott lemezek adatok csomópontonként kellene lennie 208 virtuális gépek és 3,232 lemezek toomanage.

A tárfiók lesz szabályozás kérelmek fenti az azonosított 20 000 tranzakciók/másodperc korlátozásához, inkább tekintse meg a tárolási fiók particionálás és toodetermine hello megfelelő mennyiségű tárhelyet fiókok tooaccommodate ebben a topológiában számítások használatát. Hello szabadkézi módszert használja, a dinamikus számítások által támogatott kombinációk hello számos megadott van szükség toodetermine hello megfelelő particionálást. hello Azure Resource Manager sablon nyelvi jelenleg matematikai funkcióval rendelkezik, ezért el kell végeznie ezeket a számításokat a kódban, hello megfelelő adatokkal olyan egyedi, kódolt sablon létrehozása.

A vállalati informatikai és SI forgatókönyvek esetében valaki kell hello sablonok karbantartása és telepített hello topológiákat támogatja a egy vagy több szervezet számára. Ez a további terhelés – különböző konfigurációt, és minden ügyfél esetében sablonok – nem kívánatos.

Az ügyfél Azure-előfizetést ezen sablonok toodeploy környezetekben használható, de a vállalati informatikai csapata hozza létre és CSV-k általában telepíteni őket a saját előfizetések használata a jóváírási függvény toobill az ügyfeleknek. Ezekben az esetekben hello célja toodeploy kapacitás több ügyfélhez előfizetések és a tárolás során is garantálják központi telepítések sűrűn a hello előfizetések toominimize előfizetés sprawl az tölti fel egy készlete közötti – Ez azt jelenti, hogy további előfizetések toomanage. Valóban dinamikus telepítési méret az ilyen típusú sűrűség elérése igényel a gondos tervezéssel és a további fejlesztési állványok munka hello szervezet nevében.

Emellett egy API-híváson keresztül előfizetések nem hozható létre, de ezt manuálisan kell megtennie hello portálon keresztül. Előfizetések hello száma növekszik, a létrejövő előfizetés sprawl emberi beavatkozást igényelnek – nem lehet automatizálni. A túl nagy variancia hello méretű telepítések, kellene lennie toopre-provision előfizetések száma manuálisan tooensure előfizetések érhetők el.

E tényezők, figyelembe véve a valóban szabad formátumú konfigurációs kevesebb mint, először blush tetszetős.

### <a name="known-configurations--hello-t-shirt-sizing-approach"></a>Ismert konfigurációk – hello pólóra méretezési módszer
Ahelyett, hogy a sablont, amely a teljes és számos változata kínál, mint a tapasztalatunk egy közös mintát: tooprovide hello képességét tooselect ismert konfigurációk – érvényben, szabványos pólóra például védőfal kis, közepes és nagy méretű. Más pólóra méretű például a termék ajánlatokat, community edition vagy enterprise edition.  Más esetekben lehet munkaterhelés-specifikus konfigurációk egy technológia – például a térkép csökkentése vagy a nem sql.

Sok vállalati informatikai szervezetek, OSS szállítók és SIs elérhetővé tegye az ajánlatok ma ily módon a helyszíni virtualizált környezetekben (vállalatok) vagy szoftver-,--szolgáltatás (SaaS) offerings (CSV-k és OSVs).

Ezt a módszert biztosít az ügyfelek különböző méretű előre konfigurált megfelelő, ismert konfigurációk. Ismert konfigurációk nélkül végfelhasználók kell meghatározni a fürt méretezése a saját, számításba a platform erőforrás-korlátozások és tegye a matematikai tooidentify hello eredő particionálás storage-fiókok és egyéb erőforrások (miatt toocluster méret- és erőforrás megkötések). Ismert konfigurációk engedélyezése az ügyfelek tooeasily válassza hello megfelelő pólóra a mérete – Ez azt jelenti, hogy egy adott központi telepítés. Továbbá toomaking hello ügyfél hatékonyabb működését, ismert konfigurációk kis számú könnyebb toosupport és sűrűség magasabb szintű fájlmegosztásba nyújt segítséget.

Egy ismert konfigurációs módszer pólóra méretek összpontosított is eltérő számú méretet egy csomópontja. Például egy kis pólóra mérete 3 – 10 csomópontok között lehet.  hello Póló mérete volna tervezett tooaccommodate too10 csomópontja fel kell, és adja meg a hello fogyasztói hello képességét toomake szabad formátumú beállításokat toohello azonosított maximális méretét.  

Egy pólóra munkaterhelés típusától függően lehet, hogy további szabad formátumú jellegű is telepíthető, de rendelkeznek munkaterhelések különböző csomópont méretére és konfigurációs hello szoftver hello csomóponton csomópontok száma hello tekintetében.

Pólóra méretet termékajánlatokat, például a közösségi vagy Enterprise, előfordulhat, hogy különböző erőforrástípusok és telepíthető, csomópontok maximális száma kötött toolicensing szempontok vagy a szolgáltatás rendelkezésre állásával általában hello különböző ajánlatok között.

A sablonokkal hello JSON-alapú egyedi Variant adattípusban is lehetővé teszi az ügyfelek. A kiugró meghatározásakor hello megfelelő tervezési és fejlesztői támogatási és költségszámítás szempontjai is használhatja.

Felismertük hello sablon fogyasztás forgatókönyvet, és ez a dokumentum hello elején meghatározott követelmények alapján, a sablon felbontás ellen egy mintát.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Kapacitás és a megoldás funkció hatókörű sablonok
Felbontás ellen nyújt, amely támogatja a újbóli, kiterjeszthetőség, tesztelése és tooling eszköz moduláris megközelítést tootemplate fejlesztői. Ez a szakasz részletes biztosít a felbontás ellen megközelítés hogyan lehet kapacitást és -készítés hatókörű alkalmazott tootemplates.

Ezt a módszert használja egy fő sablont paraméterértékek kap egy sablon végfelhasználói, majd a sablonok és a parancsfájlok tooseveral típusú után csatolja az alább látható módon. Paraméterek, a statikus változókat és a létrehozott változók értékei használt tooprovide mindkét kapcsolódó hello sablonok.

![Sablonparaméterek](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Paraméterek át lettek adva, tooa fő sablont, majd toolinked sablonok**

hello a következő szakaszok fókusz hello típusú sablonok és a parancsfájlok, amelyek ugyanazt a sablont a van kiválasztott. hello szakaszok az állapotadatokat hello sablonok között továbbításához megközelítések jelenthet. Minden sablon és a hello parancsfájl típusok hello kép együtt példák ismerteti. Tekintse meg a környezetfüggő például "üzembe azt: a minta megvalósítása" a jelen dokumentum későbbi.

### <a name="template-metadata"></a>Sablon metaadatait
Sablon metaadatait (hello metadata.json fájl) kulcs/érték párok, amelyek ismertetik a JSON-ban, amely az emberek és szoftver-rendszerek által olvasható sablont tartalmaz.

![Sablon metaadatait](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Sablon metaadatait hello metadata.json fájl leírása**

Szoftver ügynökök hello metadata.json fájl lekérdezhetik és hello információkat és a hivatkozás toohello sablont egy weblap vagy directory közzé. Elemek például *itemDisplayName*, *leírás*, *összefoglaló*, *githubUsername*, és *dateUpdated*.

Egy példa fájl egészében alább láthatók.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. hello database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Fő sablon
hello fő sablon paraméterek kap a felhasználó, adott információk toopopulate összetett objektumot változókat használ, és végrehajtja a kapcsolódó hello sablonok.

![Fő sablon](./media/best-practices-resource-manager-design-templates/main-template.png)

**hello fő sablon paraméterek kap a felhasználó**

Egy megadott paraméter olyan ismert konfigurációs is hello pólóra size paraméternek kis, közepes vagy nagy a szabványos értékek miatt. A gyakorlatban ez a paraméter több módon használhatja. További információkért lásd: "Ismert konfigurációs erőforrások sablon" a dokumentum későbbi szakaszában.

Bizonyos erőforrások telepítése egy felhasználó paraméter által megadott hello ismert konfigurációs függetlenül. Ezeket az erőforrásokat egyetlen megosztott erőforrás-sablon használatával telepített, és így hello megosztott erőforrás sablon első futtatásakor sablonok által megosztott.

Bizonyos erőforrások nem kötelezően telepített hello ismert konfigurációs függetlenül.

### <a name="shared-resources-template"></a>Megosztott erőforrások sablon
Ez a sablon minden ismert konfigurációk által közösen használt erőforrások nyújt. Hello virtuális hálózati, a rendelkezésre állási készletek, valamint a telepített hello ismert konfigurációs sablon függetlenül is szükségesek, más erőforrásokat tartalmazza.

![Sablonerőforrás](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Megosztott erőforrások sablon**

Erőforrás nevét, például hello virtuális hálózat nevét, a hello fő sablon alapján. Adja meg azokat, hogy a sablon belül változóként, vagy a hello felhasználó, a szervezete által megkövetelt paraméterként fogadásukra.

### <a name="optional-resources-template"></a>Nem kötelező erőforrások sablon
hello választható erőforrások sablonban erőforrások programozott módon telepített alapján hello paraméter vagy változó értékét.

![Nem kötelező erőforrások](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**Nem kötelező erőforrások sablon**

Például használhatja egy választható erőforrások sablon tooconfigure egy jumpbox, amely lehetővé teszi a közvetett access tooa telepített környezet hello a nyilvános internethez. A paraméter vagy változó tooidentify szeretné használni, hogy hello jumpbox engedélyezni kell és hello *concat* toobuild hello cél neve hello sablon működnek, mint *jumpbox_enabled.json*. Sablon linking hello eredményül kapott változó tooinstall hello jumpbox szeretné használni.

Hello választható erőforrások sablon több helyen is kapcsolhat:

* Ha alkalmazható tooevery telepítési paraméterekkel hivatkozás létrehozása hello megosztott erőforrások sablonból.
* Ha alkalmazható tooselect ismert konfigurációk – például csak telepíteni, a nagyméretű telepítések – paraméterekkel vagy változó adatvezérelt hivatkozás létrehozása hello ismert konfigurációs sablonból.

Nem kötelező-e egy adott erőforráshoz nem vezethetik hello sablon fogyasztói hanem hello sablon szolgáltató. Például szükség lehet toosatisfy egy adott termék követelmény vagy a termék bővítmény (common a CSV-k) vagy a tooenforce házirendek (gyakori, hogy a SIs és a nagyvállalati informatikai csoportok). Ezekben az esetekben változó tooidentify is használhatja, hogy hello erőforrás kell telepíteni.

### <a name="known-configuration-resources-template"></a>Ismert konfigurációs erőforrások sablon
Hello fő sablonban a paraméter lehet kitett tooallow hello sablon fogyasztói toospecify egy ismert szükségeskonfiguráció toodeploy. Gyakran az ismert konfigurációs rögzített konfigurációs méretek védőfal, kis, közepes és nagy számú használja a pólóra mérete megközelítést.

![Ismert konfigurációs erőforrások](./media/best-practices-resource-manager-design-templates/known-config.png)

**Ismert konfigurációs erőforrások sablon**

hello pólóra mérete módszer általában arra használják, de hello paraméterek minden olyan ismert konfigurációk jelenthet. Például egy vállalati alkalmazás, például a fejlesztői, tesztelési és termék környezeteket készlete is megadhat. Vagy használhat egy felhőalapú szolgáltatás toorepresent különböző a méretezési egység, termékverziók vagy termék konfigurációkat, például a közösségi, Developer és Enterprise.

Hello megosztott erőforrás sablon, a változók átadott toohello ismert konfigurációk sablon alkalmazásból:

* A felhasználó – Ez azt jelenti, hogy hello paraméterek küldött toohello fő sablont.
* Egy szervezet – Ez azt jelenti, hogy a változók hello fő sablonban, amelyek belső követelmények vagy házirendek hello.

### <a name="member-resources-template"></a>Tag erőforrások sablon
Egy ismert konfigurációs belül egy vagy több tag csomóponttípusok gyakran érhetők el. Például Hadoop lehetősége főcsomópont és adatcsomópontokat. Ha MongoDB telepíti, akkor adatcsomópontokat és egy soron. Ha alapszintű DataStax telepít, akkor adatok csomópontok és a virtuális gép telepítve OpsCenter és.

![Tagok erőforrások](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Tag erőforrások sablon**

Különböző típusú csomópontok mérete eltér a virtuális gépek, a csatlakoztatott lemezek, a parancsfájlok tooinstall számát, és hello csomópontok, a virtuális gépek hello portbeállításokat, -példányok száma és egyéb részletek beállítása. Így az egyes csomóponttípusok lekérdezi a saját a szoftver belül hello virtuális gép konfigurálása, és tag erőforrás sablon, amely tartalmazza a hello telepítése és konfigurálása az infrastruktúra, valamint parancsfájlok toodeploy végrehajtása adatokat.

Virtuális gépek általában parancsfájlok kétféle használt, széles körben újrafelhasználható és egyéni parancsfájlok.

### <a name="widely-reusable-scripts"></a>Széles körben újrafelhasználható parancsfájlok
Széles körben újrafelhasználható parancsfájlok segítségével többféle típusú sablonok. Hello jobb példák a széles körben újrafelhasználható parancsfájlok egyikét állítja be a Linux toopool lemez RAID, és nagyobb számú IOPS szerezhet. Függetlenül a virtuális gép hello telepítendő hello szoftver ezt a parancsfájlt biztosít a bevált gyakorlatok újbóli gyakori forgatókönyvek.

![Újrafelhasználható parancsfájlok](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Tag erőforrásokat sablonok meghívhatja széles körben újrafelhasználható parancsfájlok**

### <a name="custom-scripts"></a>Egyéni parancsfájlok
Sablonok általában egy vagy több olyan parancsfájlok, amelyek telepítése és konfigurálása a virtuális gépeken szoftver hívja. A közös minta látható a nagy topológiákhoz, ahol egy vagy több tag típusa több példánya van telepítve. Minden virtuális gép ezzel párhuzamosan futtatható kezdeményezett egy telepítési parancsfájlt a telepítési parancsprogram, minden virtuális gép (vagy egy megadott tag típusú virtuális gépeinek) üzembe helyezése után is nevezett követ.

![Egyéni parancsfájlok](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Tag erőforrásokat sablonok meghívhatja a parancsfájlokat egy adott célra, például a Virtuálisgép-konfiguráció**

## <a name="capability-scoped-solution-template-example---redis"></a>Képesség hatókörű megoldás sablon példa - Redis
tooshow megvalósítását működhetnek, hogyan vizsgáljuk meg a sablont, amely elősegíti a hello a telepítését és konfigurálását a Redis szabványos pólóra méretben felépítése gyakorlati példa.  

Hello központi telepítését a megosztott erőforrásokat (virtuális hálózatot, tárfiókot, rendelkezésre állási készletek) és egy opcionális erőforrás (jumpbox) vannak. Több ismert konfigurációk-ként pólóra méretek (kis, közepes, nagy), de az egyetlen csomópont írja be. Van még két célú-specifikus parancsfájlok (telepítési, konfigurációs).

### <a name="creating-hello-template-files"></a>Hello sablon fájlok létrehozása
Azuredeploy.json fő sablont hozna létre.

Megosztott resources.json nevű megosztott erőforrások sablon létrehozása

Egy nem kötelező erőforrás sablon tooenable hello telepítését egy jumpbox jumpbox_enabled.json nevű létrehozása

A redis csak egyetlen csomópont típusa, használ, így a csomópont-resources.json nevű egyetlen tag erőforrás sablont hoz létre.

A redis gyorsítótárral tooinstall szeretné, hogy minden egyes csomóponton, és ezután hello fürt beállítása.  Parancsfájlok tooaccommodate hello telepítése és beállítása, a redis-fürt-install.sh és a redis-fürt-setup.sh.

### <a name="linking-hello-templates"></a>Hivatkozási hello sablonok
Hello fő sablon ki toohello megosztott erőforrások sablont, amely létrehozza a virtuális hálózati hello sablon linking használva hivatkozik.

Egy jumpbox kell telepíteni, hogy logika hello fő sablon tooenable fogyasztói hello sablon toospecify belül jelenik meg. Egy *engedélyezett* hello értéke *EnableJumpbox* paraméter azt jelzi, hogy hello ügyfél toodeploy egy jumpbox szeretne. Ha ezt az értéket, hello sablon összefűzi *_enabled* hello jumpbox funkció utótag tooa alap sablon nevét.

hello fő sablont alkalmazza hello *nagy* paraméter értékének pólóra méretét, és ezután használja az utótag tooa alap sablon nevét egy olyan sablon hivatkozás ki ezt az értéket túl*technology_on_os_large.json*.

Ezen az ábrán hello topológia hasonló lesz.

![Redis sablon](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Egy Redis-sablon sablon struktúra**

### <a name="configuring-state"></a>Állapot beállítása
Hello fürt hello csomópontján tooconfiguring hello állapot két lépésből áll, mindkettő célú adott parancsfájlok által képviselt.  "redis-fürt-install.sh" telepíti Redis és "redis-fürt-setup.sh" hello fürt beállítása a.

### <a name="supporting-different-size-deployments"></a>A különböző méretű központi telepítések támogatása
Változók, belül hello pólóra mérete sablon minden típusú csomópontok hello számát adja meg a hello toodeploy megadott mérete (*nagy*). Majd központilag telepíti ezt a számot használó erőforrás hurkok, a csomópont nevét a numerikus sorozatszáma hozzáfűzésével egyedi nevek tooresources biztosító Virtuálisgép-példányokat *copyIndex()*. Az hajtja végre ezeket a lépéseket mindkét gyakran és meleg zóna virtuális gépekhez, a hello pólóra sablon

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>Felbontás ellen és végpont megoldás hatókörű sablonok
A végpont megoldás hatókörű megoldássablon arra irányul, hogy egy végpont megoldást hoz létre.  Erre akkor általában több képesség hatókörbe tartozó sablonokat további erőforrások, a logikai és a állapot áll.

Kiemelve a hello alábbi rendszerképek közül, mint hello ugyanannak a modellnek hatókörű funkció sablonok használt ki van bővítve a végpont megoldás hatókörű sablonok.

A megosztott erőforrások sablon és a választható erőforrásokat sablonok szolgál, hello hello kapacitás, és képes hasonlóan azonos funkciót sablon megközelítések hatókörű, de hello end tooend megoldás hatóköre.

Záró tooend megoldás hatókörű sablonok is általában rendelkezhetnek pólóra méretek, hello konfigurációs erőforrások ismert sablon tükrözi hello megoldás egy adott ismert konfiguráció szükséges.

hello ismert konfigurációs erőforrások sablon hivatkozások tooone vagy további funkció hatókörű megoldás sablonok, amelyek megfelelő toohello end tooend megoldás, valamint a hello tag erőforrás sablonok hello end tooend megoldás a szükséges.

Hello pólóra hello megoldás lehet, hogy hello egyedi képesség hatókörű sablon eltér, belül hello konfigurációs erőforrások sablon ismert változók értékei használt tooprovide hello megfelelő hatókörű alárendelt funkció megoldás sablonok toodeploy hello megfelelő pólóra mérete.

![Végpontok közötti](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**hello modell használt kapacitás vagy hatókörű funkció megoldás sablonok könnyen bővíthető end tooend megoldás sablon hatókörök**

## <a name="preparing-templates-for-hello-marketplace"></a>Sablonok előkészítése hello piactér
könnyen megelőző megközelítés hello forgatókönyvek, ahol vállalatok számára, a SIs és a megosztott fürtkötetek kívánt tooeither maguk hello sablonok telepítése, vagy engedélyezze a saját maguk a felhasználók toodeploy kezelésére képes.

Egy másik kívánt forgatókönyv telepít, egy sablon hello piactéren keresztül.  A felbontás ellen megközelítés a hello piactér is, néhány apró eltéréstől működik.

Ahogy korábban említettük, a sablonok használt toooffer különböző központi telepítési típusok a hello piactér eladásra lehet. Különböző központi telepítési típusok pólóra méretek (kis, közepes, nagy), a termék/célközönség típusa (közösségi, fejlesztők, vállalati) vagy a szolgáltatás típusa (alapszintű, a magas rendelkezésre állású) lehet.

Mint a záró tooend meglévő, alább látható módon hello megoldás vagy hatókörön belüli sablonok könnyen lehet funkció be a toolist hello különböző ismert konfigurációk hello piactéren.

hello paraméterek toohello fő sablon először módosítva lett tooremove hello tshirtSize nevű bemeneti paraméter.

Miközben hello különböző központi telepítési típusok toohello konfigurációs erőforrások sablon ismert leképezése, hello közös erőforrások is szükséges, és a konfigurációs található hello megosztott erőforrások sablont és potenciálisan azokat az opcionális erőforrás-sablonokban.

Ha azt szeretné, toopublish a sablon toohello piactér, kapcsolatot hoz létre a fő sablont, amely kiváltja a hello korábban elérhető bejövő paraméter tshirtSize tooa változó ágyazott hello sablon különböző példányait.

![Piactér](./media/best-practices-resource-manager-design-templates/marketplace.png)

**A megoldás testreszabásához hatókörű hello piactér sablonját**

## <a name="next-steps"></a>Következő lépések
* toolearn megosztása állapot-sablonok, lásd: [állapotát az Azure Resource Manager sablonokban megosztása](best-practices-resource-manager-state.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

