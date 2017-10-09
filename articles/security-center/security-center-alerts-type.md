---
title: "aaaSecurity riasztások típus az Azure Security Centerben |} Microsoft Docs"
description: "A cikk ismerteti az Azure Security Centerben elérhető biztonsági riasztások különböző típusú hello."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.openlocfilehash: ee69cb9035c35f5bc2ed51f9b9d6f29486b4caf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Az Azure Security Center biztonsági riasztásainak megismerése
Ez a cikk segít toounderstand hello különböző típusú biztonsági riasztások és a kapcsolódó elemzések, amelyek elérhetők az Azure Security Centerben. További információt a hogyan toomanage riasztások és incidensek, a következő webhelyet: [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Speciális észlelések mentése tooset a Security Center szabványos frissítési tooAzure. A 60 napos próbaverzió ingyenes. tooupgrade, jelölje be **Tarifacsomagot** a hello [biztonsági házirend](security-center-policies.md). toolearn több, tekintse meg a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/security-center/).
>

## <a name="what-type-of-alerts-are-available"></a>Milyen típusú riasztások állnak rendelkezésre?
Az Azure Security Center által használt különféle [az észlelési képességek](security-center-detection-capabilities.md) tooalert ügyfelek toopotential támadások célzó a környezetben. Ezek a riasztások hello értékes információkat tartalmaznak, mi indított hello riasztást, hello erőforrások vonatkozik, és hello hello támadás forrására. riasztás hello kacsolódjanak hello típusú használt analytics toodetect hello fenyegetés függ. Az incidensek további környezeti információkat is tartalmazhatnak, amelyek hasznosak lehetnek a fenyegetések vizsgálata során.  Ez a cikk a következő riasztástípusok hello kapcsolatos információkat tartalmazza:

* Virtuális gép működésének elemzése (VMBA)
* Hálózatelemzés
* Erőforrás-elemzés
* Környezeti információk

## <a name="virtual-machine-behavioral-analysis"></a>Virtuális gép működésének elemzése
Az Azure Security Center a virtuális gép esemény naplóinak elemzésén alapul a viselkedéselemzés sérült tooidentify erőforrásokat használhatja. Ilyenek például a folyamat-létrehozási események és a bejelentkezési események. Emellett nincs korrelációban állnak más jelek toocheck bizonyíték a széles körű kampány támogatásához.

> [!NOTE]
> Ha részletes tájékoztatást szeretne kapni a Security Center észlelési funkcióinak működéséről, tekintse meg [az Azure Security Center észlelési funkcióit ismertető](security-center-detection-capabilities.md) cikket.
>

### <a name="crash-analysis"></a>Összeomlás-elemzés
Összeomlási memóriakép memória elemzésre egy használt módszer toodetect kifinomult kártevő szoftver, amely képes tooevade hagyományos biztonsági megoldásokat. Kártevő szoftver különféle próbálja tooreduce hello esélyét víruskereső szoftverek által észlelt alatt, soha nem írásával toodisk vagy toodisk írt szoftverösszetevőket titkosításával. Így hello kártevő nehéz toodetect hagyományos kártevőirtó módszer használatával. Azonban az ilyen típusú kártevő észlelhető a memória az elemzést követően mert kártevők kell hagyja a nyomkövetések rendelés toofunction memóriája.

Ha szoftver összeomlik, összeomlási memóriaképet egy részéhez memória hello összeomlási hello helyreállításkor rögzíti. hello összeomlási kártevő szoftver, általános alkalmazás vagy rendszerrel kapcsolatos problémák is okozhatja. Hello összeomlási memóriakép hello memóriája elemzésével, Security Center képesek észlelni technikák tooexploit biztonsági rések használt szoftver, bizalmas adatok eléréséhez, és a sérült biztonságú gépekről rejtett megőrzéséhez. Mindez minimális teljesítmény hatás toohosts a, hello elemzés vissza a Security Center end hello hajtja végre.

a következő mezők hello esetekben toohello összeomlási memóriakép riasztás például a cikk későbbi részében megjelenő:

* Memóriakép-fájl: Hello memóriaképet neve.
* FOLYAMATNÉV: Folyamat összeomló hello neve.
* PROCESSVERSION: Folyamat összeomló hello verzióját.

### <a name="shellcode-discovered"></a>Héjkód észlelhető
Shellcode kártevő kihasználva szoftver után futtatott hello payload. Ez a riasztás azt jelzi, hogy az összeomlási memóriakép elemzése olyan végrehajtható kódot talált, amely a kártékony kódokra jellemző működés jeleit mutatja. Bár előfordulhat, hogy nem rosszindulatú szoftverhez tartozik az adott működés, ez nem jellemző a szokásos szoftverfejlesztési gyakorlatban.

hello Shellcode riasztás például a következő kiegészítő mező hello biztosítja:

* CÍM: hello helye hello shellcode a memóriában.

Példa az ilyen típusú riasztásra:

![Héjkód riasztása](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Moduleltérítés észlelhető
A Windows dinamikus kötésű kódtárakon (dll) tooallow szoftver tooutilize közös Windows rendszer funkcióit használja. DLL térít megváltozásakor következik be kártevő hello DLL betöltése rendelés tooload rosszindulatú vonatkozó Payload van jelen a memóriába, amelyben tetszőleges kódot hajtható végre. Ez a riasztás azt jelzi, hogy hello összeomlási memóriakép elemzés észlelt egy hasonlóan elnevezett modul, amely két különböző elérési utakról be van töltve. Betöltött hello elérési utak közül egy Windows rendszer bináris hely származik.

Jogos szoftverfejlesztők alkalmanként hello DLL betöltési sorrendjének például leírására, kiterjesztése hello Windows operációs rendszer vagy egy Windows-alkalmazás kiterjesztése nem rosszindulatú okokból módosítani. toohelp megkülönböztetni a rosszindulatú és potenciálisan jóindulatú módosítások toohello DLL betöltési sorrendjét, az Azure Security Center ellenőrzi, hogy a betöltött modul tooa gyanús profil megfelel-e. az ellenőrzés eredményének hello hello "Aláírás" mező hello riasztás jelzi, és hello súlyossági hello riasztást, a riasztás leírása és a riasztási javítási lépéseket is megjelenik. tooresearch hogy hello modul jogos vagy rosszindulatú, elemezheti hello modul térít hello lemez példányán. Például ellenőrizze a hello fájl digitális aláírását, vagy egy víruskereső vizsgálat futtatása.

Továbbá toohello közös mezők hello korábbi "Shellcode felderített" szakaszban leírt, a riasztás biztosít a következő mezők hello:

* ALÁÍRÁS: Azt jelzi, ha modul térít hello tooa gyanús viselkedését profil megfelel-e.
* HIJACKEDMODULE: hello hello neve azokat támadás érte Windows rendszer modul.
* HIJACKEDMODULEPATH: hello elérési útja hello azokat támadás érte Windows rendszer modul.
* HIJACKINGMODULEPATH: hello elérési útja hello eltérítés modul.

Példa az ilyen típusú riasztásra:

![Moduleltérítési riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Álcázásos Windows-modul észlelhető
Kártevő szoftver használhatják a Windows rendszer bináris fájljait (például SVCHOST köznapi neve. EXE) vagy modulokat (például NTDLL. DLL-fájl) túl*keverése* és hello kártevő szoftvert a rendszergazdák hello jellege átalakítani. Ez a riasztás azt jelzi, hogy hello összeomlási memóriakép elemzés azt észleli, hogy hello memóriaképet, amely a Windows rendszer modulneveket, de az nem felel meg más feltételek, amelyek kifejezetten a Windows-modulok lehetővé tevő modulokat tartalmaz. A Lemezmásolás hello színleg modul elemzésekor hello előfordulhat, hogy további információval szolgálnak hello jogos vagy rosszindulatú jellegű e modul. Az elemzés a következőket tartalmazhatja:

* Győződjön meg arról, hogy a szóban forgó hello fájl jogos szoftvertelepítési csomag részeként van kiadva.
* Ellenőrizze a hello fájl digitális aláírását.
* Víruskereső futtatása a hello fájl.

Továbbá toohello közös mezők hello "Shellcode felderített" szakaszában bemutatott, a riasztás biztosít további mezőket a következő hello:

* Részletek: Ismerteti, hogy hello modul metaadatai érvényes, és hogy hello modul lett betöltve, a rendszer elérési útról.
* : Hello név hello színleg Windows modul.
* Elérési út: hello elérési toohello színleg Windows modul.

Ez a riasztás is bontja ki, és megjeleníti az egyes mezők hello modul PE fejlécéből, például a "ELLENŐRZŐÖSSZEG" és "IDŐBÉLYEG." Ezeket a mezőket csak akkor jelennek meg, ha hello mezők szerepelnek a hello modul. Lásd: hello [Microsoft PE és a COFF meghatározása](https://msdn.microsoft.com/windows/hardware/gg463119.aspx) meg ezeket a mezőket.

Példa az ilyen típusú riasztásra:

![Álcázásos Windows-modul miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Módosított bináris rendszerfájl észlelhető
Kártevő szoftver core rendszer bináris rendelés toocovertly hozzáférési adatok módosíthatók vagy rejtett megőrizni a sérült biztonságú rendszeren. Ez a riasztás azt jelzi, hogy, hogy hello összeomlási memóriakép elemzés észlelte, hogy az alapvető Windows operációs rendszer bináris fájlokat a memóriában vagy a lemez módosult-e.

A megbízható szoftverfejlesztők esetenként nem ártó szándékkal módosítják a memóriában lévő rendszermodulokat, hanem például elkerülő megoldásokhoz vagy az alkalmazások kompatibilitásához. toohelp megkülönböztetni a rosszindulatú és potenciálisan jogos modulok, az Azure Security Center ellenőrzi, hogy hello módosított modul tooa gyanús profil megfelel-e. az ellenőrzés eredményének hello hello riasztást, a riasztás leírása és a riasztási javítási lépéseket hello súlyosságát jelzi.

Továbbá toohello közös mezők hello "Shellcode felderített" szakaszában bemutatott, a riasztás biztosít további mezőket a következő hello:

* Modulnév: Bináris módosító hello neve.
* MODULEVERSION: Hello verziója bináris módosítani.

Példa az ilyen típusú riasztásra:

![Bináris rendszerfájl miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Gyanús folyamat lett végrehajtva
A Security Center azonosítja egy gyanús folyamat, amely hello cél virtuális gépen fut, és ezután riasztást. hello észlelési hello adott névvel nem keres, de nem keres hello végrehajtható fájl paraméter. Ezért még akkor is, ha hello támadó átnevezi hello végrehajtható, a Security Center is észlelni tudja hello gyanús folyamat.

Példa az ilyen típusú riasztásra:

![Gyanús folyamat miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domain-accounts-queried"></a>Több tartományi fiók lett lekérdezve
A Security Center képes észlelni, több kísérletek tooquery Active Directory tartományi fiókokat, amelyek hálózati felderítés során a támadók általában történik. A támadók használja ki az ezzel a technikával tooquery hello tartomány tooidentify hello felhasználók, hello tartományi rendszergazdai fiókok azonosítása, tartományvezérlők és hello lehetséges tartományi megbízhatósági kapcsolattal más tartományokból is azonosíthatja hello számítógépek azonosításához.

Példa az ilyen típusú riasztásra:

![Több tartományi fiók miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>A helyi rendszergazdák csoport tagjait enumerálták

A Security Center tootrigger figyelmeztetés állapotra vált, ha hello biztonsági esemény 4798, a Windows Server 2016 és a Windows 10, van-e indítani. Ez a helyi rendszergazdák csoportok tagjainak enumerálásakor történik meg, amit általában a hálózat felderítése során hajtanak végre a támadók. A támadók a technika tooquery hello identitást rendszergazdai jogosultságokkal rendelkező felhasználók használhatják fel.

Példa az ilyen típusú riasztásra:

![Helyi rendszergazda](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Nagy- és kisbetűs karakterek rendellenes kombinációja

A Security Center riasztást indít, amikor azt észleli, hogy a kis- és kisbetűket hello parancssorból kombinációját hello használatát. Néhány támadó használhat ez a módszer toohide a kis-és nagybetűket, vagy a kivonat-alapú számítógép szabály.

Példa az ilyen típusú riasztásra:

![Rendellenes kombináció](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Gyanús Kerberos-aranyjegyes támadás

A sérült biztonságú [krbtgt](https://technet.microsoft.com/library/dn745899.aspx) kulcs egy támadó toocreate Kerberos "Arany jegyek," hello támadó tooimpersonate így kívánják bármely felhasználó használhatja. A Security Center tootrigger figyelmeztetés állapotra vált, amikor azt észleli, hogy a tevékenységet.

> [!NOTE] 
> A „Kerberos-aranyjeggyel” kapcsolatos további információkért olvassa el a [Windows 10-es hitelesítőadatok lopásának megelőzésére vonatkozó útmutatót](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx).

Példa az ilyen típusú riasztásra:

![Aranyjegy](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Gyanús fiók létrehozása

A Security Center egy riasztást aktivál a rendszergazdai jogosultságokkal rendelkező meglévő beépített fiókokhoz nagyon hasonló fiókok létrehozása esetén. Ez a módszer egy engedélyezetlen fiók alatt első fellépése emberi ellenőrzésével tooavoid támadók toocreate használhatja.
 
Példa az ilyen típusú riasztásra:

![Gyanús fiók](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Gyanús tűzfalszabály létrehozása

A támadók toocircumvent állomás biztonsági megpróbálja hozzon létre egyéni tűzfal szabályok tooallow rosszindulatú alkalmazások toocommunicate parancs és a vezérlés vagy toolaunch támadások hello a hálózaton keresztül hello keresztül sérült állomás. A Security Center riasztást aktivál, ha azt észleli, hogy egy gyanús helyen lévő végrehajtható fájl egy új tűzfalszabályt hoz létre.
 
Példa az ilyen típusú riasztásra:

![Tűzfalszabály](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>HTA és PowerShell gyanús kombinációja

A Security Center egy riasztást aktivál, ha azt észleli, hogy egy Microsoft HTML-alkalmazásgazda (HTA) PowerShell-parancsokat indít. Ez a támadók toolaunch rosszindulatú PowerShell-parancsfájlok által használt módszer.
 
Példa az ilyen típusú riasztásra:

![HTA és PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>Hálózatelemzés
A Security Center hálózati fenyegetettség-észlelése úgy működik, hogy automatikusan összegyűjti a biztonsági információkat az Azure IPFIX (IP-folyamatadatok exportálása) forgalmából. Ezt az információt, gyakran adatok az adatokat több forrásból tooidentify fenyegetések megvizsgálja.

### <a name="suspicious-outgoing-traffic-detected"></a>Gyanús kimenő forgalom észlelhető
Hálózati eszközök felderítése és a sok hello csatolást más típusú rendszerekhez hasonlóan történik. A támadók általában a portkereséssel kezdik. Hello következő példában egy virtuális gép forgalmát gyanús Secure Shell (SSH) rendelkezik. Ebben az esetben SSH találgatásos vagy portkereséses támadás lehet folyamatban egy külső erőforrás ellen.

![Gyanús kimenő forgalom riasztása](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Ez a riasztás biztosítja használható, de a használt tooinitiate tooidentify hello erőforrás a támadás információkat. Ez a riasztás tooidentify hello sérült a gép, a hello észlelés ideje, valamint a hello protokoll és a használt port információkat is biztosít. Ezen a panelen is biztosít, amelyek javítási lépéseket listája használt toomitigate probléma lehet.

### <a name="network-communication-with-a-malicious-machine"></a>Kártékony géppel folytatott hálózati kommunikáció
Az Azure Security Center a Microsoft fenyegetésfelderítő hírcsatornáinak használatával észlelni tudja a kártékony IP-címmel kommunikáló feltört gépeket. Sok esetben hello rosszindulatú címe parancs és a vezérlő center. Ebben az esetben a Security Center észlelte, hogy hello kommunikációs póni betöltő kártevő szoftverek segítségével végezhető el (más néven [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![hálózati kommunikáció miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Ez a riasztás ad információt, amely lehetővé teszi, de a használt tooinitiate tooidentify hello erőforrás a támadás, hello megtámadott erőforrások, hello áldozata IP, hello támadó IP és hello észlelés ideje.

> [!NOTE]
> A valódi IP-címek adatvédelmi okból el lettek távolítva erről a képernyőfelvételről.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Lehetséges kimenő szolgáltatásmegtagadási támadás észlelése
Rendellenes egy virtuális gép származó hálózati forgalom okozhat a Security Center tootrigger egy potenciális szolgáltatásmegtagadást-típusú támadások.

Példa az ilyen típusú riasztásra:

![Kimenő szolgáltatásmegtagadás](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Erőforrás-elemzés
A Security Center erőforrás-elemzés a platformszolgáltatási szolgáltatásként, például hello hello integrációja összpontosít platform [Azure SQL Database fenyegetésészlelés](../sql-database/sql-database-threat-detection.md) szolgáltatás. Ezek a területek hello elemzés eredményeinek alapján, a Security Center erőforrásokra vonatkozó riasztás váltja ki.

### <a name="potential-sql-injection"></a>Potenciális SQL-injektálás
SQL-injektálás ahol kártékony kódot később átadott tooan példány az SQL Server elemzési és végrehajtási karakterláncok bekerülnek a támadás. Az SQL-utasításokat létrehozó összes eljárást meg kell vizsgálni az injektálási biztonsági rések felderítéséhez, mivel az SQL Server végrehajtja az összes olyan lekérdezést, amely szintaktikailag érvényes. SQL Fenyegetésészlelés gépi tanulás, viselkedéssel összefüggő elemzésekkel és anomáliadetektálási észlelési toodetermine gyanús eseményeket, előfordulhat, hogy tart az Azure SQL Database adatbázisok helyet használ. Példa:

* Egy korábbi alkalmazott megpróbált hozzáférni az adatbázishoz
* SQL-injektálási támadások
* Szokatlan tooa éles adatbázist a felhasználó otthoni

![Potenciális SQL-injektálás miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

a riasztás hello információkat csak használt tooidentify hello megtámadott erőforrások, hello észlelés ideje és hello támadás hello állapotát. Is tartalmaz egy hivatkozást toofurther vizsgálati lépéseket.

### <a name="vulnerability-toosql-injection"></a>A biztonsági rés tooSQL injektálási
Ez a riasztás akkor aktiválódik, ha a rendszer alkalmazáshibát észlelt egy adatbázisban. Ez a riasztás azt jelezheti egy esetleges biztonsági rés tooSQL injektálási támadások.

![Potenciális SQL-injektálás miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Ismeretlen helyről történt szokatlan hozzáférés
Ez a riasztás akkor lesz kiváltva, ha egy ismeretlen IP-címről érkező hozzáférési esemény észlelhető hello kiszolgálón, amely korábban nem látott hello az utolsó időszak.

![Szokatlan hozzáférés miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Környezeti információk
A vizsgálat során elemzők kell extra környezetben tooreach egy verdict kapcsolatos hello fenyegetés hello jellege és hogyan toomitigate azt.  Például egy hálózati anomáliadetektálási észlelt, de ismertetése nélkül más fájlvédelem hello hálózaton vagy a legutóbb toohello céloz erőforrás, akkor minden rögzített toounderstand milyen műveletek tootake mellett. tooaid vele, egy biztonsági incidens összetevők, a kapcsolódó események és tartalmazhat hello vizsgálatot végző információkat. hello elérhetősége függően eltérőek lesznek a veszéllyel fenyegetett típusú hello hello a környezet konfigurációjának, és nem lesz elérhető az összes biztonsági eseményekre.

További információ áll rendelkezésre, ha látható hello biztonsági incidens hello riasztások listája alatt történik. Ez többek között az alábbi információkat tartalmazhatja:

- Naplótörlési események
- PNP-eszköz csatlakoztatása ismeretlen eszközről
- Beavatkozást nem igénylő riasztások 

![Szokatlan hozzáférés miatti riasztás](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>Lásd még:
Ebben a cikkben megismerte a Security Center biztonsági riasztások különböző típusú hello. További információ a Security Center toolearn hello következő lásd:

* [Biztonsági incidensek kezelése az Azure Security Centerben](security-center-incident.md)
* [Az Azure Security Center észlelési képességei](security-center-detection-capabilities.md)
* [Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md)
* [Azure Security Center: GYIK](security-center-faq.md): gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/): Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
