---
title: "a SAP HANA Azure virtuális gépeken aaaBackup útmutatója |} Microsoft Docs"
description: "Biztonsági mentési SAP Hana az útmutató két fő biztonsági mentési lehetőségek SAP Hana az Azure virtuális gépeken"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: e651091bb5da2698ec8bf80cad405bfce5b192cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Útmutató az Azure-beli virtuális gépeken futó SAP HANA biztonsági mentéséhez

## <a name="getting-started"></a>Első lépések

hello biztonsági mentési útmutató az Azure virtuális gépeken futó SAP Hana csak Azure-specifikus témakörök ismerteti. Általános SAP HANA biztonsági mentési kapcsolódó elemek, dokumentációjában hello SAP HANA (lásd: _biztonsági mentési SAP HANA-dokumentáció_ Ez a cikk későbbi).

hello Ez a cikk célja a két fő biztonsági mentési lehetőségek SAP Hana az Azure virtuális gépeken:

- HANA biztonsági mentési toohello fájlrendszer egy Azure Linux virtuális gépen (lásd: [SAP HANA Azure biztonsági mentési fájl szinten](sap-hana-backup-file-level.md))
- A storage-pillanatfelvételekkel hello az Azure storage blob pillanatkép funkció használatával manuálisan vagy az Azure Backup szolgáltatás alapján HANA biztonsági mentés (lásd: [SAP HANA biztonsági másolat alapján storage-pillanatfelvételekkel](sap-hana-backup-storage-snapshots.md))

SAP HANA API, amely lehetővé teszi, hogy a külső biztonsági mentési eszközök toointegrate közvetlenül az SAP HANA biztonsági kínál. (Ez nem ez az útmutató hello hatókörén belül.) Nincs közvetlen integráció az Azure Backup szolgáltatásban elérhető jobbra a alapján ez az API SAP HANA van.

SAP HANA hivatalosan támogatott Azure virtuális gép típus GS5, egy további korlátozás tooOLAP munkaterhelések az Egypéldányos (lásd: [hitelesített IaaS platformok](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) hello SAP webhelyen). Ez a cikk új ajánlatok SAP Hana Azure elérhető legyen, mint fog frissülni.

Nem érhető el egy SAP HANA hibrid megoldás Azure SAP HANA futtató nem virtualizált fizikai kiszolgálókon. Azonban az SAP HANA Azure biztonsági mentési útmutató hozzá van rendelve egy SAP HANA futtató egy Azure virtuális gép, a SAP HANA-nem fut a tiszta Azure-környezetéhez &quot;nagy példányok.&quot; Lásd: [SAP HANA (nagy példányok) – áttekintés és Azure architektúra](hana-overview-architecture.md) tájékozódhat a biztonsági mentési megoldás a &quot;nagy példányok&quot; storage-pillanatfelvételekkel alapján.

Általános információk a támogatott Azure SAP termékek található [SAP Megjegyzés 1928533](https://launchpad.support.sap.com/#/notes/1928533).

hello következő három ábra áttekintést hello SAP HANA natív Azure-képességek jelenleg használt biztonsági mentési lehetőségeket, és három lehetséges jövőbeli biztonsági mentési helyzetet is megjelenítése. hello kapcsolódó cikkekben [SAP HANA Azure biztonsági mentési fájl szinten](sap-hana-backup-file-level.md) és [SAP HANA biztonsági másolat alapján storage-pillanatfelvételekkel](sap-hana-backup-storage-snapshots.md) ezeket a beállításokat, beleértve az SAP mérete és a teljesítménnyel kapcsolatos részletesebb leírása HANA biztonsági mentések, amelyek multi-terabájt méretű.

![Az ábrán látható, két lehetőség közül választhat a hello aktuális VM állapotának mentése](media/sap-hana-backup-guide/image001.png)

Az ábrán látható, hello aktuális virtuális gép állapotát, vagy Azure Backup szolgáltatás vagy Virtuálisgép-lemezek manuális pillanatkép mentésekor hello lehetőségét. Ezt a módszert használja, egy nem &#39; nincs toomanage SAP HANA biztonsági mentéseket. hello challenge hello lemez pillanatkép forgatókönyv fájlrendszer-konzisztenciával, de az alkalmazáskonzisztens lemez állapota. hello konzisztencia témakör hello szakaszban tárgyalt _SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel_ című cikkben. Képességek és korlátozások az Azure Backup szolgáltatással kapcsolatos tooSAP HANA biztonsági mentéseket is ismertetése a cikk későbbi részében.

![Az ábrán látható, hogy egy SAP HANA fogadására beállítások fájlszintű biztonsági belül hello VM](media/sap-hana-backup-guide/image002.png)

Ez a szám egy SAP HANA-biztonsági mentés hello VM belül, és majd tárolásukról HANA biztonságimásolat-fájlok valahol ellenkező esetben a különböző eszközök használatával lehetőségeket mutatja be. HANA biztonsági másolat egy pillanatkép-alapú biztonsági mentési megoldás több időt igényel, azonban mindenképpen sértetlenségét és konzisztencia előnyeit. További részletek találhatók a cikk későbbi részében.

![Ezen az ábrán egy lehetséges jövőbeli SAP HANA biztonsági mentési forgatókönyvet mutat](media/sap-hana-backup-guide/image003.png)

Ezen az ábrán egy lehetséges jövőbeli SAP HANA biztonsági mentési forgatókönyvet mutat. SAP HANA engedélyezett másodlagos replikációból biztonsági másolatok fogadására, ha azt szeretné adja hozzá a biztonsági mentési stratégia további beállításokat. Jelenleg nem lehet megfelelően tooa közlemény hello SAP HANA Wiki:

_&quot;Ennyi az egész lehetséges tootake biztonsági mentések hello másodlagos oldalon?_

_Nem, jelenleg csak adatokat és naplófájl biztonsági mentések hello elsődleges oldalán. Ha automatikus napló biztonsági mentési engedélyezve van, felvásárlási toohello másodlagos oldalon után hello naplóalapú biztonsági mentések automatikusan írandó van.&quot;_

## <a name="sap-resources-for-hana-backup"></a>SAP HANA biztonsági mentés erőforrásait

### <a name="sap-hana-backup-documentation"></a>Biztonsági mentési SAP HANA-dokumentáció

- [Bevezetés tooSAP HANA felügyeleti](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [A biztonsági mentési és helyreállítási stratégia tervezése](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [Biztonsági mentés HANA ABAP DBACOCKPIT ütemezése](http://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Ütemezési adatok biztonsági mentése (SAP HANA Vezérlőpult)](http://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- SAP HANA gyakori kérdések a biztonsági mentés [SAP Megjegyzés 1642148](https://launchpad.support.sap.com/#/notes/1642148)
- Gyakori kérdések az adatbázis és a tárolási pillanatképek SAP HANA [SAP Megjegyzés 2039883](https://launchpad.support.sap.com/#/notes/2039883)
- Nem alkalmas hálózati fájlrendszerek biztonsági másolat és helyreállítás a [SAP Megjegyzés 1820529](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Miért SAP HANA biztonsági mentést?

Az Azure storage kínál rendelkezésre állásának és megbízhatóságának hello kezdő verzióról (lásd: [Azure Storage bemutatása tooMicrosoft](../../../storage/common/storage-introduction.md) további információ az Azure storage).

a minimális hello &quot;biztonsági mentési&quot; toorely hello Azure SLA-kat, ha megtartja hello az SAP HANA adatainak és naplókönyvtárainak fájlok Azure virtuális merevlemezek csatolt toohello SAP HANA-kiszolgálón virtuális gép van. Ez a megközelítés VM hibák, de a nem lehetséges kárt toohello SAP HANA adatok és a naplófájlokat vagy a logikai hibák például törlés, adatok vagy a fájlok véletlenül ismerteti. Biztonsági mentés is szükség, megfelelőségi vagy jogi okokból. Rövid mindig szükség van a SAP HANA biztonsági mentésekhez.

### <a name="how-tooverify-correctness-of-sap-hana-backup"></a>Hogyan tooverify helyességét SAP HANA biztonsági mentés.
Egy teszt visszaállítást egy másik rendszeren futó tárolási pillanatképek használata esetén ajánlott. Ezt a módszert biztosít a módon tooensure, hogy biztonsági helyes-e, és a belső folyamatok biztonsági mentési és visszaállítási munka várt módon. Ez nem egy jelentős a küszöbértéket a helyszínen, sokkal könnyebb tooaccomplish hello felhőben megadásával szükséges erőforrások átmenetileg erre a célra.

Tartsa szem előtt, amely során egy egyszerű visszaállítás és ellenőrizni, hogy HANA működik-e és fut nem elegendő. Ideális esetben egy olyan tábla konzisztencia ellenőrzése toobe meg arról, hogy hello visszaállítani az adatbázis finom kell futtatnia. SAP HANA kínál a konzisztencia-ellenőrzések leírt különböző típusú [SAP Megjegyzés 1977584](https://launchpad.support.sap.com/#/notes/1977584).

Hello tábla konzisztencia-ellenőrzéssel kapcsolatos információkat is hello SAP webhelyen található [tábla és a katalógus konzisztencia-ellenőrzések](http://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

A szokásos fájlszintű biztonsági a teszt visszaállítás nincs szükség. Két SAP HANA-eszközzel, hogy mely biztonsági másolat nem használható visszaállítási toocheck súgó: hdbbackupdiag és hdbbackupcheck. Lásd: [manuális ellenőrzése hogy a helyreállítás az lehetséges](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) ezekkel az eszközökkel kapcsolatos további információk.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Előnyei és hátrányai HANA biztonsági mentés és tárolás pillanatkép

SAP nem &#39; t biztosít preferencia tooeither HANA biztonsági mentés és a tárolási pillanatkép. Felsorolja az előnyei és hátrányai, így egy megállapíthatja, hogy mely toouse hello helyzet és a rendelkezésre álló tár technológia függően (lásd: [tervezési a biztonsági mentési és helyreállítási stratégia](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

On Azure figyelembe hello tényt, hogy az Azure blob pillanatkép funkció nem & #39 hello; t garantálja a fájlrendszer-konzisztenciával (lásd: [blob pillanatképei Using PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). hello a következő szakaszban _SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel_, ez a szolgáltatás kapcsolatos szempontokat ismerteti.

Emellett egy rendelkezik toounderstand hello számlázási gyakorolt hatása, amikor olyan gyakran blob pillanatképek ebben a cikkben leírtak szerint: [ismertetése hogyan pillanatképek keletkeznek költségek](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)– ez nem &#39; t, nyilvánvaló Azure használatával virtuális lemezek.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel

A fájlok rendszer és az alkalmazás konzisztenciájának egy összetett probléma esetén tárolási pillanatképek készítése. hello legegyszerűbb módja tooavoid problémák volna tooshut SAP HANA le, vagy lehet, hogy még a hello egész virtuális gép is. Előfordulhat, hogy a leállás doable egy bemutató vagy prototípus vagy akár egy fejlesztői rendszernek, de nincs lehetőség az éles rendszerre.

A Azure-ban egy rendelkezik vegye figyelembe, hogy az Azure blob pillanatkép funkció nem & #39 hello tookeep; t fájlrendszer-konzisztencia biztosításához. Jól működik azonban hello segítségével SAP HANA pillanatkép-funkció, mindaddig, amíg nincs érintett csak egyetlen virtuális lemez. De egyetlen lemezen, még akkor is a további elemek toobe be van jelölve. [Megjegyzés: 2039883 SAP](https://launchpad.support.sap.com/#/notes/2039883) SAP HANA-biztonsági mentések storage-pillanatfelvételekkel keresztül kapcsolatos fontos információkat tartalmaz. Például, hogy megjelenik-e, hogy a hello XFS fájlrendszer esetén szükséges toorun **xfs\_rögzítése** tooguarantee konzisztencia pillanatkép-tárolási indítása előtt (lásd: [xfs\_freeze(8) - Linux man lap](https://linux.die.net/man/8/xfs_freeze) talál részletes információt **xfs\_rögzítése**).

hello témakör következetesség lesz, abban az esetben, ha egy operációs rendszer által felölelt több lemez/kötet még több kihívást. Például úgy, hogy mdadm vagy LVM használ, és szétosztott. a fenti állapotok SAP Megjegyzés hello:

_&quot;De vegye figyelembe, hogy rendelkezik-e hello tárolórendszer tooguarantee i/o-konzisztencia SAP HANA-adatok kötetenként tárolási pillanatkép létrehozása során, azaz egy SAP HANA-szolgáltatással kapcsolatos adatok kötet pillanatfelvételeinek egy atomi művelet kell lennie.&quot;_

Feltételezve, hogy az átfedés négy Azure virtuális lemezek XFS fájlrendszer, hello alábbi lépések nyújtanak hello HANA adatterület jelölő alkalmazáskonzisztens pillanatkép:

- HANA pillanatkép előkészítése
- Hello fájlrendszer rögzítése (például **xfs\_rögzítése**)
- Minden szükséges blob pillanatkép létrehozása az Azure
- Hello fájlrendszer feloldása
- Hello HANA pillanatkép megerősítése

Javasoljuk, toouse hello fent leírt lépéseket az összes eset toobe hello biztonságos oldalán, függetlenül attól, milyen fájlrendszer. Vagy ha ez egy egyetlen lemez vagy, mdadm vagy LVM több lemezre kiterjedő csíkozást.

Fontos tooconfirm hello HANA pillanatkép. Esedékes toohello &quot;másolási módosításkor,&quot; SAP HANA előfordulhat, hogy nincs szükség további lemezterületet a ez mód pillanatkép-előkészítéséhez. &#39; s is nem lehetséges toostart új biztonsági mentés mindaddig, amíg hello SAP HANA pillanatkép ellenőrzése.

Az Azure Backup szolgáltatás Azure virtuális gép bővítményei tootake kiszolgálásához hello fájlrendszer-konzisztenciával használja. A Virtuálisgép-bővítmények önálló használatra nem érhetők el. Egy még toomanage SAP HANA konzisztencia. Hello kapcsolódó cikkében [SAP HANA Azure Backup a fájlszintű](sap-hana-backup-file-level.md) további információt.

### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA-ütemezési biztonsági mentési stratégia

hello SAP HANA-cikk [tervezési a biztonsági mentési és helyreállítási stratégia](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) állapotok Basicben a terv toodo biztonsági mentések:

- (Naponta) pillanatkép-tárolás
- Teljes biztonsági mentését a fájl vagy bacint formátumban (hetente egyszer)
- Automatikus naplóalapú biztonsági mentések

Szükség esetén egy sikerült nyissa meg teljesen nélkül storage-pillanatfelvételekkel; sikerült cserélhetősége HANA különbözeti biztonsági mentések, például növekményes vagy különbözeti biztonsági másolatok (lásd: [különbözeti biztonsági mentések](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

hello HANA felügyeleti útmutató egy példa listája. Azt javasolja, hogy egy helyreállítási az SAP HANA tooa adott pont, amikor a következő feladatütemezési a biztonsági mentések hello:

1. Teljes biztonságimásolat-készítő
2. Különbözeti biztonsági mentése
3. A növekményes biztonsági mentés 1
4. A növekményes biztonsági mentés 2. régiója
5. Napló-biztonságimásolatokat

Vonatkozó pontos ütemezés toowhen és milyen gyakran történjen egy adott típust, nincs lehetséges toogive általános iránymutatás – ez túl specifikus, és hány adatok változás hello rendszer függ. Az SAP oldalról, általános útmutatásként látható, amely egy alapszintű javaslat toomake egy teljes HANA biztonsági mentés hetente egyszer.
Kapcsolódó napló-biztonságimásolatokat a dokumentációban hello SAP HANA [Naplóalapú biztonsági mentések](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

SAP is azt javasolja, hogy néhány housekeeping hello biztonságimásolat-katalógus tookeep, ezzel az archív végtelenül (lásd: [biztonságimásolat-katalógus és a biztonsági másolatok tárolásának háztartási](http://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>SAP HANA-konfigurációs fájlok

Hello – gyakori kérdések a leírtaknak [SAP Megjegyzés 1642148](https://launchpad.support.sap.com/#/notes/1642148), hello SAP HANA konfigurációs fájljai nem egy szabványos HANA biztonsági másolat része. Nincsenek alapvető toorestore a rendszer. hello HANA konfigurációs sikerült módosítani a manuálisan hello visszaállítás után. Abban az esetben, ha egy szeretné tooget hello azonos egyéni konfiguráció során hello visszaállítási folyamat, a rendszer szükséges tooback hello HANA konfigurációs fájlok külön-külön.

Ha a standard HANA biztonsági mentés készül tooa dedikált HANA biztonságimásolat-fájl rendszer egyik is másolja hello konfigurációs fájlok toohello azonos fájlrendszer biztonsági mentése, és másolja minden együtt toohello végső célhelyet, például a blob storage cool.

### <a name="sap-hana-cockpit"></a>SAP HANA-Vezérlőpult

SAP HANA-Vezérlőpult kínál hello lehetőségét, figyeléséhez és felügyeletéhez SAP HANA böngésző használatával. Azt is lehetővé teszi, hogy a SAP HANA-biztonsági mentések kezelése, és ezért használható alternatív tooSAP HANA Studio és ABAP DBACOCKPIT (lásd: [SAP HANA-Vezérlőpult](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) olvashat).

![Az ábrán látható, hello SAP HANA Vezérlőpult adatbázis felügyeleti képernyő](media/sap-hana-backup-guide/image004.png)

Az ábra azt mutatja be hello SAP HANA Vezérlőpult adatbázis felügyeleti képernyő, és a hello biztonsági mentési csempe a bal oldali hello. Megnéz hello biztonsági mentési csempe bejelentkezési fiókjának megfelelő felhasználói engedélyekkel kell rendelkeznie.

![Biztonsági mentések SAP HANA-Vezérlőpult tudja felügyelni, amíg azok nem folyamatos](media/sap-hana-backup-guide/image005.png)

Biztonsági mentések figyelhető SAP HANA Vezérlőpulton, amíg a folyamatban lévő, és amikor elkészült, az összes hello biztonsági mentési részletek érhetők el.

![Egy példa, egy Azure SLES 12 virtuális gépen a Gnome desktop Firefox használatával](media/sap-hana-backup-guide/image006.png)

hello előző képernyőfelvételeken történtek egy Windows Azure virtuális gépről. Erre látható egy példa egy Azure SLES 12 virtuális gépen a Gnome desktop Firefox segítségével. Hello beállítás toodefine SAP HANA-alapú biztonsági mentési ütemterv SAP HANA Vezérlőpulton jeleníti meg. Egy is láthatja, mivel azt sugallja, dátum/idő előtagjaként hello biztonságimásolat-fájlok. Az SAP HANA Studio eszközben hello alapértelmezett előtag van &quot;COMPLETE\_adatok\_biztonsági MENTÉSI&quot; teljes biztonsági mentés során. Egy egyedi előtag használata javasolt.

### <a name="sap-hana-backup-encryption"></a>SAP HANA a biztonsági mentés titkosítása

SAP HANA adatainak és naplókönyvtárainak titkosításának kínál. Ha az SAP HANA-adatainak és naplókönyvtárainak nincs titkosítva, majd hello biztonsági mentések is nincs titkosítva. Toohello ügyfél toouse mentése harmadik féltől származó megoldás tooencrypt hello SAP HANA biztonsági másolatok egyaránt működik valamilyen formában. Lásd: [adatok és a napló Kötettitkosítást](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) toofind további tudnivalók az SAP HANA-titkosítás.

A Microsoft Azure-ban az ügyfél használhatja az infrastruktúra-szolgáltatási virtuális gép titkosítási szolgáltatás tooencrypt hello. Például egy használhatja dedikált adatok lemezek csatolt toohello virtuális gép, amely használt toostore SAP HANA-biztonsági mentések, akkor ezek a lemezek példányát.

Az Azure Backup szolgáltatás kezelni tud a titkosított virtuális gép/lemez (lásd: [hogyan tooback mentése és visszaállítása titkosított virtuális gépek az Azure Backup](../../../backup/backup-azure-vms-encryption.md)).

Egy másik lehetőség lenne toomaintain hello SAP HANA virtuális gép és a lemezek titkosítás nélkül, és hello SAP HANA biztonságimásolat-fájlok tárolása egy tárfiókot, amelyre engedélyezték a titkosítást (lásd: [Azure Storage szolgáltatás titkosítási inaktív adatok](../../../storage/common/storage-service-encryption.md)) .

## <a name="test-setup"></a>Teszt beállítása

### <a name="test-virtual-machine-on-azure"></a>Teszt virtuális gép az Azure-on

Egy Azure GS5 virtuális gép egy SAP HANA-telepítés a következő biztonsági mentés/visszaállítás tesztek hello lett megadva.

![Az ábrán látható részét hello hello HANA teszt virtuális gép az Azure portál áttekintése](media/sap-hana-backup-guide/image007.png)

Az ábrán látható, hello hello HANA teszt virtuális gép az Azure portál áttekintése részét.

### <a name="test-backup-size"></a>Biztonsági másolat mérete tesztelése

![Ez a szám HANA Studio hello biztonsági mentési konzoljáról készült, és hello HANA index kiszolgáló 229 GB hello biztonságimásolat-fájl méretét jeleníti meg](media/sap-hana-backup-guide/image008.png)

Egy üres táblázatot töltötte az adatok tooget egy teljes biztonsági mentés méretének több mint 200 GB tooderive reális teljesítményadatokat. hello. ábra HANA Studio hello biztonsági mentési konzoljáról készült, és megmutatja a hello HANA index server 229 GB hello biztonságimásolat-fájl méretét. Hello tesztek hello alapértelmezett biztonsági mentési előtag SAP HANA Studio "COMPLETE_DATA_BACKUP" lett megadva. A valós éles rendszerek hasznosabb előtag meg kell határozni. SAP HANA-Vezérlőpult Dátum és idő alapján.

### <a name="test-tool-toocopy-files-directly-tooazure-storage"></a>Vizsgálati eszköz toocopy fájlok közvetlenül tooAzure tároló

tootransfer SAP HANA biztonságimásolat-fájlok közvetlenül tooAzure blob-tároló vagy Azure fájlmegosztásokat, hello blobxfer eszköz lett megadva, mert azt támogatja a célok, és azt egyszerűen integrálhatják automatizálási parancsfájlokat tooits parancssori felület miatt. hello blobxfer eszköz [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Tesztelje a biztonsági mentés méretének becslése

Fontos tooestimate hello biztonsági mentés mérete SAP HANA. Ez a becslés tooimprove teljesítmény segítségével meghatározhat egy biztonságimásolat-fájlok száma hello biztonságimásolat-fájl maximális mérete miatt tooparallelism fájlmásolás közben. (Adatai magyarázatát a cikk későbbi részében.) Egy kell is döntse el, hogy toodo egy teljes biztonsági mentési vagy a különbözeti biztonsági mentés (növekményes vagy különbözeti).

Szerencsére van egy egyszerű hello biztonságimásolat-fájlok hello méretének becslése SQL-utasítást: **válasszon \* m\_biztonsági MENTÉSI\_mérete\_BECSLÉSEKET** (lásd: [ Becsült hello terület szükséges a hello fájlrendszer a biztonsági mentést](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![az SQL-utasítás kimenete hello megegyezik szinte teljesen hello tényleges mérete hello teljes biztonsági mentését a lemezen](media/sap-hana-backup-guide/image009.png)

Az SQL-utasítás kimenete hello hello gazdagépes tesztrendszer esetében megegyezik szinte teljesen hello tényleges mérete hello teljes biztonsági mentését a lemezen.

### <a name="test-hana-backup-file-size"></a>Teszt HANA biztonságimásolat-fájl mérete

![hello HANA Studio biztonsági mentési konzol lehetővé teszi egy toorestrict hello maximális fájlméretét HANA biztonságimásolat-fájlok](media/sap-hana-backup-guide/image010.png)

hello HANA Studio biztonsági mentési konzol lehetővé teszi, hogy egy toorestrict hello maximális HANA biztonsági mentési fájlok méretét. Hello minta környezetben, a szolgáltatás révén lehetséges tooget több kisebb biztonságimásolat-fájlokat egy 230 GB-os biztonsági mentési fájl helyett. Kisebb fájlméretet jelentős hatással van a teljesítményre (hello kapcsolódó cikkében [SAP HANA Azure Backup a fájlszintű](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Összefoglalás

A következő táblák hello megjelenítése és mentése egy SAP HANA-adatbázisból, az Azure virtuális gépeken futó megoldások tooback hello vizsgálati eredmények alapján.

**Készítsen biztonsági másolatot az SAP HANA toohello fájlrendszer, és másolja biztonságimásolat-fájlokat ezek után utolsó biztonsági mentés célhelyének toohello**

|Megoldás                                           |Informatikai szakemberek                                 |Hátrányok                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|A virtuális gépek lemezei HANA biztonsági mentések megtartása                      |Nincsenek további felügyeleti próbálkozások     |Helyi virtuális gép lemezterületet eats           |
|Blobxfer eszköz toocopy biztonságimásolat-fájlok tooblob tároló |Párhuzamossági toocopy több fájlokat, választott toouse ritkán használt adatok blob-tároló | További eszköz karbantartási és az egyéni parancsfájlok | 
|A BLOB másolási Powershell vagy a parancssori felület használatával                    |Nincs szükség esetén további eszköz valósítható meg Azure Powershell vagy a parancssori felület használatával |a manuális eljáráshoz, az ügyfélnek van tootake kiszolgálásához parancsfájlok és a visszaállítási másolt blobok kezelése|
|TooNFS megosztás                                  |Utófeldolgozási biztonsági mentési fájlok és más virtuális gép hello HANA server gyakorolt hatás nélkül|Lassú másolást|
|Blobxfer másolási tooAzure szolgáltatása                |Nem keleti-afrikai fel lemezterületet a helyi virtuális gépek lemezei|Nincs közvetlen támogatási írja HANA biztonsági másolat, fájlméret korlátozása fájlmegosztás jelenleg 5 TB:|
|Az Azure Backup szolgáltatás ügynöke                                 | Előnyben részesített megoldás lenne         | Linux rendszeren jelenleg nem érhető el    |



**Storage-pillanatfelvételekkel alapuló biztonsági mentési SAP HANA**

|Megoldás                                           |Informatikai szakemberek                                 |Hátrányok                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Azure biztonsági mentési szolgáltatás                               | Lehetővé teszi a virtuális gép biztonsági mentése a blob pillanatképek alapján | Használatakor nem fájl szint visszaállítása hello létrehozása új virtuális gép igényel a hello visszaállítási folyamat, majd egy új SAP HANA-licenckulcs hello igények amiből|
|A pillanatképek kézi blob                              | Rugalmasság toocreate és visszaállítási adott virtuális gépek lemezei módosítása nélkül hello virtuális gép egyedi azonosítója|Az összes manuális tevékenység, amelynek hello ügyfél által végzett toobe|

## <a name="next-steps"></a>Következő lépések
* [SAP HANA Azure biztonsági mentési fájl szinten](sap-hana-backup-file-level.md) hello fájl alapú biztonsági mentési beállítás ismerteti.
* [A storage-pillanatfelvételekkel alapján SAP HANA biztonsági mentés](sap-hana-backup-storage-snapshots.md) hello tárolási pillanatkép-alapú biztonsági mentési beállítás ismerteti.
* Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).
