---
title: "az Azure-ban aaaEnhance távfelügyelet biztonsági |} Microsoft Docs"
description: "Ez a cikk a Microsoft Azure-környezetek, például a Cloud Services, a Virtual Machines szolgáltatás és az egyéni alkalmazások távfelügyeletével kapcsolatos funkciók biztonságának fokozása érdekében végrehajtandó lépéseket ismerteti."
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 2431feba-3364-4a63-8e66-858926061dd3
ms.service: security
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 9262cfb98bfe51d15fbad8f18997c4573668d9ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-management-in-azure"></a>Biztonságkezelés az Azure-ban
Az Azure-előfizetők több eszközről kezelhetik felhőkörnyezeteiket, például felügyeleti munkaállomásokról, fejlesztői PC-kről, és olyan jogosult végfelhasználói eszközökről is, amelyek feladatspecifikus engedélyekkel rendelkeznek. Bizonyos esetekben felügyeleti funkciók hello például webalapú konzolok használatával végzik [Azure-portálon](https://azure.microsoft.com/features/azure-portal/). Más esetekben lehet közvetlen kapcsolatokon keresztül tooAzure a helyszíni rendszerekben a virtuális magánhálózatok (VPN), a Terminálszolgáltatások, ügyfél-alkalmazásprotokollokon, vagy (szoftveresen) hello Azure Service Management API (SMAPI) keresztül. Továbbá az ügyfél-végpontok lehetnek vagy tartományhoz csatlakoztatottak, vagy pedig elkülönítettek és felügyelet nélküliek, mint például a táblagépek vagy az okostelefonok.

Bár több hozzáférési és kezelési képesség a lehetőségek széles skáláját adja meg, a variancia jelentős kockázat tooa felhő üzembe helyezése adhat hozzá. Nehéz toomanage lehet követni, és rendszergazdai műveletek naplózhatók. A variancia is vezethet be a felhőszolgáltatások kezelésére használt ügyfélvégpontokhoz hozzáférés tooclient végpontok biztonsági fenyegetésekkel. Az általános vagy személyes munkaállomások fejlesztésre és infrastruktúra-kezelésre való használata olyan kiszámíthatatlan fenyegetési vektoroknak enged utat, mint például a webböngészés (pl. alapesetben megbízható weboldalak megfertőződése, ún. watering hole attack) vagy az e-mail (pl. pszichológiai manipuláció és adathalászat).

![][1]

potenciális támadások elleni hello ilyen típusú környezet növekedése, mivel kihívást tooconstruct biztonsági házirendeket és mechanizmusokat tooappropriately hozzáférés tooAzure felületekhez (mint például a SMAPI-t) kezelése a rendkívül változatos végpontokból.

### <a name="remote-management-threats"></a>Távfelügyelettel kapcsolatos fenyegetések
A támadók gyakran kísérlik meg toogain emelt szintű hozzáférés megszerzésével a fiók hitelesítő adatait (például próbálgatásos jelszavas támadáson, adathalászat és keresztül credential harvesting), vagy felhasználókat trükk segítségével ártalmas kódnak (például a webhelyek káros eltérítésén letölti a meghajtó által vagy káros e-mail-mellékletek). Távfelügyelt felhőkörnyezetben tooan járhat fiók nőtt kockázata miatt tooanywhere, bármikor hozzáférést.

Fő rendszergazdai fiókok szigorú felügyelete alacsonyabb szintű felhasználói fiókok a biztonsági stratégia használt tooexploit gyenge lehet. A megfelelő biztonsági képzés hiánya szintén vezethet toobreaches véletlen vagy fiókadatok keresztül.

Ha egy felhasználói munkaállomás felügyeleti feladatok elvégzésére is használatos, sok különböző ponton sérülhet a rendszer biztonsága. Hogy egy felhasználó hello webböngészés, 3. féltől származó és nyílt forrású eszközök használata, vagy nevű trójai program tartalmazó ártalmas dokumentum megnyitása a fájl megnyitása.

Általában a célzott támadásokkal szemben, amelyek adatszivárgáshoz eredményül nyomon követni, spear phishing (e-mail) asztali számítógépek, toobrowser biztonsági réseket és beépülő modulok (például Flash, PDF vagy Java). Ezek a gépek rendelkezhetnek rendszergazdai szintű vagy szolgáltatásszintű engedélyekkel tooaccess kiszolgálók live, vagy hálózati eszközök használatakor fejlesztésre vagy más eszközök felügyeleti műveleteihez.

### <a name="operational-security-fundamentals"></a>A működési biztonság alapjai
A biztonságosabb kezelés és a műveletek minimalizálhatja egy ügyfél támadási felületét hello lehetséges belépési pontok számának csökkentésével. Ez olyan rendszerbiztonsági alapelvekkel oldható meg, mint a „feladatkörök szétválasztása” és a „környezetek elkülönítése”.

Különítse el a bizalmas funkciókat egymástól toodecrease hello valószínűsége, hogy egy szinten hibát egy másik tooa szabálysértésnek minősülő vezet. Példák:

* Felügyeleti feladatok ne végezzen olyan tevékenységet, amely tooa sérült biztonság esetén (például egy rendszergazda e-mailben az infrastruktúra-kiszolgálók átterjedhet kártevő) vezethet.
* A munkaállomás magas a bizalmas műveletek elvégzéséhez használható nem lehet hello ugyanarra a rendszerre böngészési hello Internet például magas kockázatú célokat szolgál.

Hello rendszer támadási felület csökkentése a szükségtelen szoftverek eltávolításával. Példa:

* Általános jellegű rendszergazdai, támogatási vagy fejlesztői munkaállomáson elvégzéséhez nem szükséges egy levelezőprogram vagy más irodai alkalmazás telepítését hello eszköz fő felhasználási toomanage felhőszolgáltatások esetén.

Rendszergazdai hozzáférés tooinfrastructure összetevőjének kell lennie rendelkező ügyfélrendszereket alávetni toohello legszigorúbb lehetséges házirend tooreduce biztonsági kockázatokat. Példák:

* Biztonsági házirendek tartalmazhatnak olyan csoportházirend-beállításokat, nyissa meg az Internet-hozzáférés megtagadása hello eszköz és a korlátozó tűzfal-konfiguráció használatát.
* Használjon IPsec alapú VPN-eket, ha közvetlen hozzáférésre van szükség.
* Állítson be különálló felügyeleti és fejlesztői Active Directory-tartományokat.
* Különítse el és szűrje a felügyeleti munkaállomások hálózati forgalmát.
* Használjon kártevőirtó szoftvert.
* A multi-factor authentication tooreduce hello kockázatát lopott fiókadatokkal megvalósításához.

A hozzáférési erőforrások konszolidálása és a felügyelet nélküli végpontok kiiktatása egyszerűsíti a felügyeleti feladatokat.

### <a name="providing-security-for-azure-remote-management"></a>Az Azure távfelügyelet biztonsági megoldásai
Azure mechanizmusok tooaid rendszergazdákkal Azure felhőszolgáltatások és virtuális gépek biztonságot nyújt. Ezen mechanizmusok az alábbiak:

* Hitelesítés és [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).
* Figyelés és naplózás.
* Tanúsítványok és titkosított kommunikáció.
* Webes felügyeleti portál.
* Hálózaticsomag-szűrés.

Az ügyféloldali biztonsági konfiguráció és egy felügyeleti átjáró adatközpontbeli akkor a lehetséges toorestrict és a figyelő rendszergazdai hozzáférés toocloud alkalmazásokhoz és adatokhoz.

> [!NOTE]
> Egyes, ebben a cikkben olvasható javaslatok megnövekedett adat-, hálózati vagy számítási erőforrás-igénybevétellel járhatnak, és megnövelhetik a licencek vagy előfizetések költségeit.
>
>

## <a name="hardened-workstation-for-management"></a>Megerősített munkaállomás a felügyelethez
hello a munkaállomások megerősítésének célja az összes tooeliminate, de hello nélkülözhetetlen, kritikus funkciókon szükséges toooperate, így a lehető legkisebb hello potenciális támadási felületet. A rendszer megerősítéséhez tartozik a telepített szolgáltatások és alkalmazások, az alkalmazásfuttatás korlátozása, mire van szükség, a hálózati hozzáférés tooonly korlátozása hello számának minimalizálása, és mindig a hello rendszer mentése toodate tartása. Továbbá a megerősített munkaállomás felügyeletre való használata elkülöníti a felügyeleti eszközöket és tevékenységeket a többi végfelhasználói feladattól.

Egy helyszíni vállalati környezeten belül hello támadási felületét dedikált felügyeleti hálózatok, a kártya hozzáférés kiszolgálótermek és a munkaállomások, hello hálózat védett területein a fizikai infrastruktúra korlátozhatja. Felhőalapú vagy hibrid informatikai modellek esetében bonyolultabb felügyeleti szolgáltatások biztonságának lehet a fizikai hozzáférés tooIT erőforrások hello hiánya miatt. A védelmi megoldások megvalósítása precíz szoftverkonfigurációt, biztonságközpontú folyamatokat és átfogó házirendeket követel meg.

Használja a legalacsonyabb jogosultsági lehető legkevesebb szoftver telepítésével egy zárolt munkaállomást a felhőfelügyelet – és az alkalmazásfejlesztéshez – hello távoli felügyeleti és fejlesztői környezetek szabványosításával csökkentheti a biztonsági események hello kockázatát. A megerősített munkaállomás-konfiguráció segíthet használt toomanage kritikus felhőalapú erőforrások közé tartozik kihasználói biztonsági réseket és a kártevő szoftver által használt fiókok hello esetleges illetéktelen használat megelőzése. Pontosabban, használhat [Windows AppLocker](http://technet.microsoft.com/library/dd759117.aspx) és a Hyper-V technológia toocontrol és elkülönítheti az ügyfélrendszer viselkedését, és elhárítani a fenyegetéseket, beleértve az e-mailek és a webes böngészéssel.

A megerősített munkaállomásokon hello rendszergazda fut egy általános jogú felhasználói fiók (amely megakadályozza a rendszergazdai szintű kódvégrehajtást), és társított alkalmazásokat engedélyezési lista szabályozza. a megerősített munkaállomás alapszintű hello elemei a következők:

* Aktív vizsgálat és javítás. Kártevőirtó szoftver központi telepítése, rendszeres biztonsági rés összetevőjére és összes munkaállomásnak. frissítését a legújabb biztonsági frissítéseket hello időben.
* Korlátozott funkciók. Távolítsa el az összes olyan alkalmazást, amelyre nincs szükség, és tiltsa le a szükségtelen (rendszerindításkor elinduló) szolgáltatásokat.
* A hálózat megerősítése. A Windows tűzfal szabályai tooallow csak érvényes IP-címeket, portokat és URL-címek kapcsolódó tooAzure felügyeleti használja. Győződjön meg arról, hogy a bejövő távoli kapcsolatok toohello munkaállomás is le vannak tiltva.
* Végrehajtási korlátozások. Csak előre megadott végrehajtható fájlokat, a szükséges felügyeleti toorun meghatározott engedélyezése (említett tooas "alapértelmezésként Elutasítás"). Alapértelmezés szerint felhasználók nem kap engedélyt, a program, kivéve, ha az explicit módon definiálva van hello toorun engedélyezési lista.
* Legkisebb jogosultság. A felügyeleti munkaállomások felhasználóinak nem kell rendszergazdai jogosultsága a helyi számítógépen hello magát. Ezzel a módszerrel tudják módosítani hello rendszer konfigurációját vagy hello rendszerfájlok, szándékos vagy véletlen.

Használatával kényszerítheti az összes ezt [csoportházirend-objektumok](https://www.microsoft.com/download/details.aspx?id=2612) (GPO-k) Active Directory tartományi szolgáltatások (AD DS), és alkalmazza őket a (helyi) felügyeleti tartomány tooall felügyeleti fiókokon keresztül.

### <a name="managing-services-applications-and-data"></a>A szolgáltatások, alkalmazások és adatok felügyelete
Azure felhőszolgáltatások konfigurálása keresztül hello Windows PowerShell parancssori felületen, vagy olyan egyedi alkalmazás használatával, amely kihasználja ezeket a RESTful-felületeket hello Azure-portálon vagy SMAPI-t, keresztül történik. Ilyen mechanizmusokat használó szolgáltatások például az alábbiak: Azure Active Directory (Azure AD), Azure Storage, Azure Websites, Azure Virtual Network.

Virtuális gépek által telepített alkalmazások adja meg a saját ügyféleszközöket és igény szerint például hello Microsoft Management Console (MMC), vállalati felügyeleti konzolok (például a Microsoft System Center vagy a Windows Intune) vagy egy másik felügyeleti felületek alkalmazás – Microsoft SQL Server Management Studio, például. Ezek az eszközök általában vállalati környezetben vagy az ügyfélhálózaton találhatóak meg. Függhetnek meghatározott hálózati protokolloktól (ilyen például a Remote Desktop Protocol (RDP)), amelyek közvetlen, állapotalapú kapcsolatokat igényelnek. Néhány is rendelkezhet, webes felületek, amelyet nem szabad nyilvánosan közzétett vagy hello interneten keresztül érhető el.

Használatával korlátozhatja a hozzáférést tooinfrastructure és a platformszolgáltatások kezeléséhez az Azure-ban [a multi-factor authentication](../multi-factor-authentication/multi-factor-authentication.md), [X.509 felügyeleti tanúsítványok](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/), és a tűzfalszabályokat. hello Azure-portál és a SMAPI-t igényelnek a Transport Layer Security (TLS). Azonban szolgáltatások és alkalmazások központi telepítése az Azure használatba tootake védelmi intézkedéseket, amelyek adott alkalmazástól függően. Ezek a mechanizmusok gyakran egyszerűbben engedélyezhetőek egy szabványosított, megerősített munkaállomás-konfigurációval.

### <a name="management-gateway"></a>Felügyeleti átjáró
az összes felügyeleti toocentralize eléréséhez és leegyszerűsíthesse a figyelést, és a naplózás, telepíthet egy dedikált [távoli asztali átjáró](https://technet.microsoft.com/library/dd560672) (RD átjáró) kiszolgálót a helyszíni hálózati, csatlakoztatott tooyour Azure környezetben.

A távoli asztali átjáró egy házirendalapú RDP-proxyszolgáltatás, amely képes kikényszeríteni a biztonsági követelményeket. Az RD-átjáró Windows Server-hálózatvédelemmel (NAP) együtt alkalmazva segít biztosítani, hogy csak az Active Directory tartományi szolgáltatások (AD DS) csoportházirend-objektumai (GPO-k) által meghatározott biztonságiállapot-feltételeknek megfelelő ügyfelek csatlakozhatnak. Továbbá:

* Kiépítés egy [Azure felügyeleti tanúsítványhoz](http://msdn.microsoft.com/library/azure/gg551722.aspx) a távoli asztali átjáró hello úgy, hogy hello csak állomás engedélyezett tooaccess hello Azure-portálon.
* Csatlakozzon a távoli asztali átjáró toohello hello azonos [felügyeleti tartomány](http://technet.microsoft.com/library/bb727085.aspx) , a rendszergazdai munkaállomások hello. Ez azért szükséges, az a pont-pont IPsec VPN vagy ExpressRoute, amely rendelkezik egy egyirányú bizalmi kapcsolat tooAzure AD egy tartományon belül, vagy ha a helyszíni közötti összevonja a hitelesítő adatokat az AD DS-példány és az Azure AD.
* Konfigurálja a [ügyfélkapcsolat-engedélyezési házirendet](http://technet.microsoft.com/library/cc753324.aspx) toolet hello távoli asztali átjáró győződjön meg arról, hogy hello ügyfélgép neve érvényes (a tartományhoz csatlakozó) és engedélyezett tooaccess hello Azure-portálon.
* Az IPsec használatának [Azure VPN](https://azure.microsoft.com/documentation/services/vpn-gateway/) toofurther védje a felügyeleti adatforgalmat a lehallgatástól és, vagy fontolja meg az elkülönített Internet-kapcsolaton keresztül [Azure ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).
* Állítson be többtényezős hitelesítést ([Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) használatával) vagy intelligens kártyás hitelesítést az RD-átjárón keresztül bejelentkező rendszergazdák számára.
* Állítson be forrás [IP-címkorlátozások](http://azure.microsoft.com/blog/2013/08/27/confirming-dynamic-ip-address-restrictions-in-windows-azure-web-sites/) vagy [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md) megengedett felügyeleti végpontok Azure toominimize hello száma.

## <a name="security-guidelines"></a>Biztonsági irányelvek
Hello felhő használatára van bármilyen, helyszíni munkaállomás esetében alkalmazott gyakorlathoz. toohello hasonló. általában segíti a rendszergazdai munkaállomások toosecure – például kis méretben build és a korlátozó engedélyek. Felhőfelügyelet bizonyos egyedi aspektusai több akin tooremote vagy vállalati sávon kívüli felügyelet. Ezek közé tartozik a hello használata és a hitelesítő adatokat, a biztonságos távoli hozzáférést, és a fenyegetések észlelése és a válasz naplózását.

### <a name="authentication"></a>Authentication
Használhat Azure bejelentkezési korlátozásokat tooconstrain forrás IP-címek felügyeleti eszközökhöz hozzáférni és hozzáférési kérelmek naplózása. toohelp Azure felügyeleti ügyfeleket (munkaállomásokat és/vagy alkalmazások), mind a SMAPI-t (az ügyfelek által fejlesztett eszközök például a Windows PowerShell-parancsmagok) keresztül, és a hello Azure portál toorequire ügyféloldali felügyeleti tanúsítványokat toobe azonosítása telepített, továbbá tooSSL tanúsítványok. Javasolt a többtényezős hitelesítés bevezetése a rendszergazdai hozzáférés esetében is.

Egyes Azure-ra telepített alkalmazások vagy szolgáltatások saját hitelesítési mechanizmusokkal rendelkezhetnek mind a végfelhasználói, mind a rendszergazdai hozzáféréshez, míg mások az Azure AD előnyeit használják ki. Attól függően, hogy összevonja a hitelesítő adatokat az Active Directory összevonási szolgáltatások (AD FS), a címtár-szinkronizálás használatával, vagy kizárólag a hello a felhasználói fiókokat a felhő, használatával [Microsoft Identity Manager](https://technet.microsoft.com/library/mt218776.aspx) (részben Prémium szintű Azure AD) segítségével felügyelheti a identitás életciklusának hello erőforrások között.

### <a name="connectivity"></a>Kapcsolatok
Több mechanizmus vannak elérhető toohelp biztonságos ügyfél kapcsolatok tooyour egy Azure virtuális hálózatot. Ezen mechanizmusok közül kettő [telephelyek közötti VPN](https://channel9.msdn.com/series/Azure-Site-to-Site-VPN) (S2S) és [pont – hely típusú VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) (P2S) engedélyezése hello iparági szabványnak megfelelő IPsec (S2S) vagy hello [Secure Socket Tunneling Protocol](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx)(SSTP) (P2S) használatát titkosítás és Alagútkezelés. Ha Azure toopublic elérhető Azure-szolgáltatások felügyeleti hello Azure-portál például kapcsolódik, az Azure-nak Hypertext Transfer Protocol Secure (HTTPS).

Önálló megerősített munkaállomás, amelyek nem kapcsolódnak tooAzure egy távoli asztali átjárón keresztül kell hello SSTP alapú pont-pont VPN toocreate hello kezdeti kapcsolat toohello Azure Virtual Network használjon, és majd létre az RDP-kapcsolat tooindividual virtuális gépekhez hello VPN-alagúton.

### <a name="management-auditing-vs-policy-enforcement"></a>Felügyeleti naplózás vagy házirend kikényszerítése
Nincs általában két megközelítés használatos toosecure eljárások: naplózás és a házirendek kikényszerítése. A két módszer együttes használata átfogó ellenőrzést tesz lehetővé, de nem minden helyzetben lehetséges a kivitelezése. Emellett a mindkét megközelítés különböző szintű kockázat, a költségek és a társított biztonsági különösen az egyéni felhasználók számára és a rendszer architektúrák fektetett bizalom toohello szint személyekbe erőfeszítések rendelkezik.

A megfigyelés és a naplózás biztosítják követésének és értelmezésének felügyeleti tevékenységek alapul, de nem mindig lehet megvalósítható tooaudit összes művelet befejezéséhez miatt keletkező toohello adatmennyiség részletei. Az ajánlott eljárás szerint azonban hello hello felügyeleti házirendek hatékonyságának naplózása.

A szigorú hozzáférés-vezérlést magában foglaló házirend-kikényszerítés olyan szoftveres mechanizmusokat léptet életbe, amelyek képesek felügyelni a rendszergazdák tevékenységét, és segít biztosítani, hogy azok minden lehetséges védelmi intézkedést betartsanak. A naplózás bizonyítékkal végrehajtás, továbbá tooa rekordjának ki volt, mi az, hogy hol és mikor. Naplózás is lehetővé teszi, hogyan hajtsa végre a rendszergazdák a házirendeket és összehasonlítását azzal tooaudit információt, és a tevékenységek bizonyítékait

## <a name="client-configuration"></a>Ügyfél-konfiguráció
Háromféle elsődleges megerősített munkaállomás-konfigurációt ajánlunk. hello legnagyobb differentiators közöttük minden beállítások között egy hasonló biztonsági profil megőrzésével költség, a használhatóság és a kisegítő lehetőségek. a következő táblázat hello a hello előnyökről és kockázatokról tooeach egy rövid elemzést tartalmaz. (Vegye figyelembe, hogy a "Vállalati PC" tooa szokásos asztali PC-konfigurációt, amely akkor telepíthető hivatkozik az összes tartományi felhasználó számára, szerepkörtől függetlenül.)

| Konfiguráció | Előnyök | Hátrányok |
| --- | --- | --- |
| Önálló, megerősített munkaállomás |Szigorúan ellenőrzött munkaállomás |A dedikált asztali gépek magasabb költsége |
| - | Az alkalmazások biztonsági réseinek kihasználásával kapcsolatos kisebb kockázat |Nagyobb energiabefektetést igénylő felügyelet |
| - | A feladatok egyértelmű elkülönítése | - |
| Vállalati PC mint virtuális gép |Alacsonyabb hardverköltségek | - |
| - | Szerepkör és alkalmazások elkülönítése | - |
| A BitLocker meghajtótitkosítás Windows toogo |Kompatibilitás a legtöbb PC-vel |Eszközkövetés |
| - | Költséghatékonyság és hordozhatóság | - |
| - | Elkülönített felügyeleti környezet |- |

Fontos, hogy hello megerősített munkaállomás hello gazdagépet, amely hello Vendég, és semmi ne legyen közötti hello nyújt operációs rendszer és hello hardver. A következő hello "tiszta forrás alapelv" (más néven "biztonságos eredet") azt jelenti, hogy hello üzemeltető kell lennie hello legtöbb megerősítve. Ellenkező esetben hello megerősített munkaállomás (vendég) tulajdonos tooattacks, amelyen üzemeltetett hello rendszeren.

Még jobban elkülönítheti a felügyeleti funkciók dedikált rendszerkép használatával minden egyes megerősített munkaállomáshoz, amelyek csak hello eszközök és az engedélyek szükséges kezeléséhez válassza ki az Azure és a felhőalapú alkalmazásokhoz, az adott helyi AD DS csoportházirend-objektumok hello szükséges feladatok.

A helyszíni infrastruktúrával (például nincs hozzáférési tooa helyi Active Directory tartományi szolgáltatások csoportházirend-objektumok példányt, mert az összes kiszolgáló hello felhőben), nem rendelkező informatikai környezetek esetében olyan szolgáltatások, mint [a Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx) egyszerűbbé teheti az telepíteni és fenntartani munkaállomás-konfigurációk.

### <a name="stand-alone-hardened-workstation-for-management"></a>Önálló megerősített munkaállomás a felügyelethez
Önálló megerősített munkaállomás használatával a rendszergazdák rendelkeznek egy PC-vel vagy laptoppal, amelyet felügyeleti feladatok ellátására használnak, valamint egy másik, különálló PC-vel vagy laptoppal az egyéb feladatokhoz. Egy dedikált munkaállomás toomanaging az Azure-szolgáltatások nem kell más alkalmazások telepítésére. Továbbá a [platformmegbízhatósági modulokat](https://technet.microsoft.com/library/cc766159) (TPM) vagy hasonló hardverszintű kriptográfiás technológiát támogató munkaállomások használata segít az eszközök hitelesítésében és bizonyos típusú támadások megelőzésében. TPM is képes támogatni hello rendszermeghajtó teljes kötet védelmét használatával [a BitLocker meghajtótitkosítás](https://technet.microsoft.com/library/cc732774.aspx).

Hello önálló megerősített munkaállomás-forgatókönyvben (lásd alább), a Windows tűzfal (vagy egy nem Microsoft által készített ügyféltűzfal) helyi példányának hello konfigurált tooblock bejövő kapcsolatok, mint például az RDP. hello rendszergazdák jelentkezhetnek be toohello megerősített munkaállomás és indítson el egy RDP-munkamenetet, amely a VPN-en egy Azure virtuális hálózathoz csatlakozzon, de nem jelentkezhetnek be tooa vállalati PC és -felhasználási RDP tooconnect toohello létrehozása után tooAzure megerősítve munkaállomás saját magát.

![][2]

### <a name="corporate-pc-as-virtual-machine"></a>Vállalati PC mint virtuális gép
Azokban az esetekben, ahol egy önálló megerősített munkaállomás akadályozó vagy kényelmetlen, hello költséges megerősített munkaállomás üzemeltethet egy virtuális gép tooperform nem rendszergazdai feladatokat.

![][3]

tooavoid ugyanannak a munkaállomásnak rendszerfelügyeletre és más napi Munkafeladatok ellátására használatából eredő lehetséges biztonsági kockázatokat, telepíthet egy Windows Hyper-V virtuális gép toohello megerősítve a munkaállomáson. A virtuális gép használható vállalati PC hello. hello vállalati PC-környezet így elkülönül hello állomás, ami csökkenti annak támadási felületét, és a bizalmas felügyeleti feladatoktól gazdagéptől hello felhasználó napi tevékenységeit (például az e-mailhez) eltávolítása.

hello vállalati PC virtuális gépe védett térben fut, és lehetővé teszi a felhasználó. hello gazdagép "tiszta forrás" marad, és érvényesíti a szigorú hálózati házirendek hello gyökér operációs rendszerben (például blokkoló RDP-hozzáférést hello virtuális gépről).

### <a name="windows-toogo"></a>Windows tooGo
Egy másik alternatív toorequiring önálló megerősített munkaállomás toouse egy [Windows tooGo](https://technet.microsoft.com/library/hh831833.aspx) meghajtó, amely támogatja az olyan ügyféloldali USB-rendszerindítási képességet. Windows tooGo lehetővé teszi, hogy a felhasználók tooboot a kompatibilis PC tooan elkülönített rendszerképpel indítsák, titkosított USB flash meghajtóról. További eszközöket biztosít a távfelügyeleti végpontok ellenőrzésére mert hello rendszerkép teljes mértékben felügyelhető egy vállalati informatikai csoport, szigorú biztonsági házirendekkel, egy minimális operációsrendszer-verzióval és TPM-támogatással.

Az alábbi ábrán hello hello hordozható rendszerkép egy tartományhoz csatlakoztatott rendszer, amely csak előre beállított tooconnect tooAzure, többtényezős hitelesítést igényel, és blokkol minden nem felügyeleti forgalmat. Ha egy felhasználó rendszerindításnál hello azonos PC toohello általános jellegű vállalati rendszerképpel és a távoli asztali átjáró elérése az Azure felügyeleti eszközökhöz záma, hello munkamenet le van tiltva. Windows tooGo válik hello gyökérszintű operációs rendszer, és nincs további rétegek szükséges (gazda operációs rendszer, hipervizor, virtuális gép), amely lehet, hogy jobban ki vannak téve toooutside támadásokat.

![][4]

Fontos, hogy az USB flash meghajtók elvesznek könnyebben egy átlagos asztali PC mint toonote. A BitLocker tooencrypt hello teljes kötet, és erős jelszóval, is teszi kevésbé valószínű, hogy egy támadó használhatja hello rendszerképet rosszindulatú célokra. Emellett ha hello USB flash meghajtó elveszik, visszavonásával és [egy új felügyeleti tanúsítvány kiállítását](https://technet.microsoft.com/library/hh831574.aspx) valamint gyors jelszó alaphelyzetbe állítása csökkenteni lehet a kitettséget. A felügyeleti naplók legyen elhelyezve Azure,-ban nem hello ügyfélen, tovább csökkentve az esetleges adatvesztés.

## <a name="best-practices"></a>Ajánlott eljárások
Vegye figyelembe a következő irányelveket, amikor az alkalmazások és adatok az Azure-ban kezelt hello.

### <a name="dos-and-donts"></a>Ajánlott és nem ajánlott műveletek
Nem érdemes feltételezni, hogy egy munkaállomás zárolva lett, mert más gyakori biztonsági követelményeket nem kell toobe teljesül. hello kockázatokkal értéke magasabb a megemelt hozzáférési szintek, amelyek rendszergazdai fiókok általában rendelkeznek miatt. Kockázatokra való példákat és az elkerülésükre használatos biztonsági eljárásokat hello az alábbi táblázatban láthatók.

| Nem ajánlott | Ajánlott |
| --- | --- |
| Ne küldjön e-mailben rendszergazdai hozzáféréshez használatos hitelesítő adatokat vagy más titkos adatokat (pl. SSL vagy felügyeleti tanúsítványokat). |Őrizze meg az adatok bizalmas mivoltát a fióknevek és jelszavak szóbeli közvetítésével (de ne tárolja őket hangpostán), végezze távolról az ügyfél-/kiszolgáló-tanúsítványok telepítését (titkosított munkameneten), védett hálózati megosztásról végezzen letöltéseket, illetve az adatokat személyesen, cserélhető adathordozókon tegye közzé. |
| - | Kezelje proaktívan a felügyeleti tanúsítvány-életciklusait. |
| Ne tároljon titkosítatlan vagy nem kivonatolt fiókjelszavakat alkalmazástárolókban (mint például táblázatokban, SharePoint-webhelyeken vagy fájlmegosztásokon). |Létre biztonságfelügyeleti alapelveket és rendszer-megerősítési házirendeket, és alkalmazza őket tooyour fejlesztési környezet. |
| - | Használjon [Enhanced Mitigation Experience Toolkit 5.5](https://technet.microsoft.com/security/jj653751) tanúsítvány rögzítését szabályok tooensure megfelelő hozzáféréssel tooAzure SSL/TLS-helyekhez. |
| Ne ossza meg ugyanazt a fiókot vagy jelszót több rendszergazda között, és ne használja ugyanazt a jelszót több felhasználói fiókhoz vagy szolgáltatáshoz, különösképpen közösségi oldalakon és más nem felügyeleti tevékenységekhez. |Hozzon létre dedikált Microsoft-fiók toomanage az Azure-előfizetéshez – egy fiókot, amelyet nem használ személyes e-mailt. |
| Ne küldjön el e-mail-mellékletként konfigurációs fájlokat. |A konfigurációs fájlokat és profilokat megbízható helyről telepítse (például titkosított USB flash meghajtóról), nem pedig olyan biztonsági szempontból támadásoknak kitett mechanizmust igénybe véve, mint például az e-mail. |
| Ne használjon gyenge vagy egyszerű bejelentkezési jelszót. |Tartassa be az erős jelszavak használatára vonatkozó házirendeket, és alkalmazzon lejárati ciklusokat (első használatkori megváltoztatással), konzolhasználati időkorlátokat, valamint automatikus fiókzárolást. Használjon ügyféljelszó-kezelő rendszert, többtényezős hitelesítéssel a jelszótárolóhoz való hozzáféréshez. |
| Nem teszi közzé a felügyeleti portokat toohello Internet. |Azure portok és IP-címek toorestrict felügyeleti elérést zárolása. További információkért lásd: hello [Azure hálózati biztonság](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) találhatók meg. |
| - | Használjon tűzfalat, VPN-t és NAP-ot minden felügyeleti kapcsolat esetében. |

## <a name="azure-operations"></a>Az Azure üzemeltetése
A Microsoftnál az Azure üzemeltetésének keretein belül az üzemeltetési mérnökök és a támogatási személyzet az Azure éles rendszereihez [virtuális géppel ellátott megerősített munkaállomás PC-ken](#stand-alone-hardened-workstation-for-management) keresztül férnek hozzá. A virtuális gépen keresztül lehetséges a vállalati hálózathoz és alkalmazásokhoz (például e-mail, intranet stb.) való hozzáférés. Minden felügyeleti munkaállomás rendelkezik TPM-mel, hello gazda-rendszerindítási meghajtó Bitlockerrel van titkosítva, és azok illesztett tooa speciális szervezeti egységhez (OU) a Microsoft elsődleges vállalati tartományában.

A rendszer-megerősítést a csoportházirendekkel kényszerítjük ki, központosított szoftverfrissítés mellett. Vizsgálati és elemzési eseménynaplókat (mint a biztonsági és az AppLocker) felügyeleti munkaállomásokról gyűjtött és tooa központi helyre menti.

Emellett a dedikált jump-Box a Microsoft hálózatán kéttényezős hitelesítést igénylő használt tooconnect tooAzure éles hálózati környezetben is.

## <a name="azure-security-checklist"></a>Azure biztonsági ellenőrzőlista
Minimalizálja a feladatokat, amelyek a rendszergazdák hajthatják végre a megerősített munkaállomásokon hello számát csökkentheti hello támadási felületet a fejlesztői és felügyeleti környezetben. A következő technológiák toohelp használata hello a megerősített munkaállomás védelme:

* Az IE megerősítése. hello Internet Explorer böngésző (illetve bármelyik böngésző számára, hogy függetlenül attól, hogy) fontos belépési pont lehet az ártalmas kód tooits a külső kiszolgálókkal való HOSSZÚTÁVÚ kapcsolatok miatt. Tekintse át az ügyfélházirendjeit, és kényszerítse a védett üzemmódban futást. Tiltsa le a bővítményeket és a letöltéseket, és használjon [Microsoft SmartScreen](https://technet.microsoft.com/library/jj618329.aspx) szűrést. Győződjön meg róla, hogy a biztonsági figyelmeztetések mindig megjelennek. Használja ki az internetzónák előnyeit, és hozzon létre egy listát a megbízható webhelyekről, amelyekhez megerősítést állított be. Tiltson le minden más webhelyet és böngészőn belüli kódot, mint például az ActiveX és a Java használatát.
* Általános jogokkal rendelkező felhasználó. Futó egy szabványos felhasználóként való üzemeltetés több előnnyel jár, amelyek legnagyobb hello, hogy rendszergazdai hitelesítő adatokat ellophassák nehezebb a. Továbbá a normál felhasználói fiók nem rendelkezik emelt szintű jogosultságokkal hello gyökér operációs rendszeren, és sok beállítási lehetőség és API-k zárolt alapértelmezés szerint.
* AppLocker. Használhat [AppLocker](http://technet.microsoft.com/library/ee619725.aspx) toorestrict hello programokat és parancsfájlokat, felhasználók által futtatható. Az AppLocker futtatható naplózási és kényszerítési módban. Az AppLocker alapértelmezés szerint rendelkezik egy olyan engedélyezési szabály, amely lehetővé teszi, hogy a felhasználók, akik egy rendszergazda token toorun hello ügyfélen összes kódot. Ez a szabály létezik tooprevent rendszergazdák kizárják ki magukat, és csak tooelevated jogkivonatok vonatkozik. Lásd még: Kódintegritás a Windows Server [központi biztonságának](http://technet.microsoft.com/library/dd348705.aspx) részeként.
* Kódaláírás. Az összes, rendszergazdák által használt eszköz és programkód kódaláírással való ellátása jól kezelhető mechanizmust biztosít alkalmazászárolási házirendek megvalósítására. Kivonatok arányosan toohello kód gyors változásait, és Fájlelérési utak nem biztosít a magas biztonsági szintjét. Ötvözze az AppLocker-szabályok PowerShell [végrehajtási házirend](http://technet.microsoft.com/library/ee176961.aspx) , amely csak lehetővé teszi, hogy a megadott aláírt programkódok és parancsfájlok toobe [végre](http://technet.microsoft.com/library/hh849812.aspx).
* Csoportházirend. Hozzon létre globális felügyeleti házirendet, amely a kezeléshez (és letiltják a hozzáférést a többi) használt alkalmazott tooany tartomány munkaállomásra, és ezen munkaállomásokon hitelesített toouser fiókok.
* Megnövelt biztonságú kiépítés. A megerősített munkaállomások kiindulási rendszerképének védelme toohelp illetéktelen hozzáférés elleni védelem érdekében. Használja a titkosítás és az elkülönítési toostore lemezképek, a virtuális gépek és a parancsfájlok hasonló biztonsági intézkedéseket, és korlátozza a hozzáférést (esetleg használja egy naplózható ellenőrzés – a és kijelentkezési folyamat).
* Javítások. Egy egységes buildet karbantartása (vagy készítsen különböző rendszerképeket fejlesztési, műveletek és más felügyeleti feladatokra), vizsgálata változtatások és kártevők megtalálása érdekében, tartsa be toodate hello build, és csak gépek aktiválása, amikor szükség van.
* Titkosítás. Győződjön meg arról, hogy felügyeleti munkaállomások rendelkezik-e a TPM toomore biztonságosan [titkosított fájlrendszer](https://technet.microsoft.com/library/cc700811.aspx) (EFS) és a BitLocker. Ha Windows tooGo használ, csak titkosított USB kulcsok használata Bitlockerrel együtt.
* Irányítás. Az AD DS csoportházirend-objektumok toocontrol minden hello rendszergazda Windows felületek, például a fájlmegosztás használja. Terjessze ki a naplózási és megfigyelési folyamatokat a felügyeleti munkaállomásokra. Kövessen nyomon minden rendszergazdai és fejlesztői hozzáférést és tevékenységet.

## <a name="summary"></a>Összefoglalás
A megerősített munkaállomás-konfiguráció Azure-felhőszolgáltatások, virtuális gépek és szolgáltatások felügyeletére való használata segíthet számos olyan kockázatok és fenyegetések elkerülésében, amelyek a kritikus informatikai infrastruktúrák távfelügyeletével járhatnak. Az Azure és a Windows adja meg, hogy toohelp is alkalmazhat mechanizmusok védeni, és szabályozhatja a kommunikáció, hitelesítés és az ügyfél működését.

## <a name="next-steps"></a>Következő lépések
hello következő erőforrások állnak rendelkezésre tooprovide Azure további általános információt és a kapcsolódó Microsoft-szolgáltatások, a hivatkozott dokumentum továbbá toospecific elemeket:

* [Emelt szintű hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx) – hello technikai részleteit leíró lekérése tervezéséről és kiépítéséről az Azure felügyeleti biztonságos felügyeleti munkaállomás
* [A Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Security/AzureSecurity) - információ a védelem hello Azure platform olyan képességeit Azure-hálót és hello munkaterhelések, hogy futtathatók azokon Azure
* [Microsoft Security Response Center](http://www.microsoft.com/security/msrc/default.aspx) – Ha a Microsoft biztonsági réseiről, beleértve az Azure-problémák jelenthetők vagy e-mailben túl[secure@microsoft.com](mailto:secure@microsoft.com)
* [Az Azure biztonsági Blog](http://blogs.msdn.com/b/azuresecurity/) – figyelemmel kísérheti a legújabb az Azure biztonsági hello toodate

<!--Image references-->
[1]: ./media/azure-security-management/typical-management-network-topology.png
[2]: ./media/azure-security-management/stand-alone-hardened-workstation-topology.png
[3]: ./media/azure-security-management/hardened-workstation-enabled-with-hyper-v.png
[4]: ./media/azure-security-management/hardened-workstation-using-windows-to-go-on-a-usb-flash-drive.png
