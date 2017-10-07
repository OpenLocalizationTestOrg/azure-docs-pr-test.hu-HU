---
title: "aaaAzure adatbázis biztonsági áttekintése |} Microsoft Docs"
description: "Ez a cikk áttekintést hello Azure-adatbázis biztonsági funkciók."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: TomSh
ms.openlocfilehash: 13f14b99d15800e85e9906a9d167eb0adf2135de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-overview"></a>Azure-adatbázis biztonsági áttekintése

Biztonsági elsődleges szempont kezeléséhez, adatbázisok, és az Azure SQL Database prioritást mindig lett. Az Azure SQL Database tűzfalszabályok és a kapcsolat titkosítási kapcsolat biztonsági szolgáltatásait támogatja. Támogatja a felhasználónévvel és jelszóval hitelesítési és az Azure Active Directory-hitelesítéssel, amely Azure Active Directory által kezelt identitások használ. Engedélyezési szerepköralapú hozzáférés-vezérlés használja.

Az Azure SQL Database valós idejű titkosítási és visszafejtési adatbázist, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi elvégzésével anélkül, hogy a módosítások toohello alkalmazás támogatja a titkosítást.

A Microsoft további módszereket kínál tooencrypt vállalati adatokat:

-   Cellaszintű titkosítási tooencrypt adott oszlopok vagy az adatok még akkor is, cellák különböző titkosítási kulcsokkal.
-   Ha egy hardveres biztonsági modul vagy a titkosítási kulcs hierarchia központi felügyeleti van szüksége, fontolja meg az Azure Key Vault SQL Server egy Azure virtuális gép.
-   (Jelenleg az előzetes verzió) titkosított mindig lehetővé teszi a titkosítás átlátható tooapplications, és lehetővé teszi az ügyfelek tooencrypt ügyfélalkalmazások található bizalmas adatok megosztása hello titkosítási kulcsokat az SQL Database nélkül.

Az Azure SQL Database Auditing lehetővé teszi a vállalatok toorecord események tooan naplózási bejelentkezési Azure Storage. SQL Database Auditing együttműködik a Microsoft Power BI toofacilitate részletes jelentések és elemzések.

 SQL Azure-adatbázisok szorosan biztonságos toosatisfy legtöbb szabályozó vagy biztonsági követelményeket, például a HIPAA, ISO 27001/27002 és PCI DSS szintjét 1, többek között lehet. A biztonsági megfelelőség tanúsítványok aktuális listáját érhető el: hello [Microsoft Azure Trust Center webhely](http://azure.microsoft.com/support/trust-center/services/).

Ez a cikk végigvezeti a strukturált, a Tabular és a relációs adatok biztonságossá tétele a Microsoft Azure SQL-adatbázisok hello alapjait. A cikk ismerteti az adatok védelméhez, a hozzáférés szabályozásához és a proaktív figyeléshez szükséges erőforrások az első lépéseit is.

Azure-adatbázis biztonsági áttekintő cikkben a következő területeken hello koncentrál:

-   Adatok védelme
-   Hozzáférés-vezérlés
-   Proaktív figyelés
-   Központi kezelés
-   Azure Piactér

## <a name="protect-data"></a>Adatok védelme

SQL-adatbázis titkosítja az adatokat, adja meg a titkosítási adatok mozgásérzékelési használatával [Transport Layer Security](https://support.microsoft.com/kb/3135244), az adatokat a rest-használatával [átlátható adattitkosítási](http://go.microsoft.com/fwlink/?LinkId=526242), és az adatok használata használatával [ Mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx).

Ez a szakasz a döntésről bővebben:

-   Mozgásérzékelő – a titkosítás
-   Titkosítás inaktív állapotban
-   Titkosítás használata (ügyfél)

A más módon tooencrypt adatait, vegye figyelembe:

-   [Cellaszintű titkosítását alkalmazza](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt adott oszlopok vagy még akkor is, cellák az adatok különböző titkosítási kulcsokat.
-   Ha hardveres biztonsági modulra vagy a titkosítási kulcshierarchia központi kezelésére van szüksége, fontolja meg az [Azure Key Vault és az SQL Server együttes használatát egy Azure virtuális gépen](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).

### <a name="encryption-in-motion"></a>Mozgásérzékelő – a titkosítás

Összes ügyfél/kiszolgáló alkalmazás gyakori probléma esetén adatvédelmi hello szükségességét, mert az adatok áthelyezése nyilvános és titkos hálózatokon keresztül. Ha nem titkosított a hálózaton keresztül helyezze át adatokat, nincs hello esélye, hogy azt rögzíthetik és a jogosulatlan felhasználók ellopták. Adatbázis-szolgáltatásokat a meghatározásakor kell toomake meg arról, hogy az adatok titkosítását, valamint hello adatbázis ügyfél és kiszolgáló közötti adatbázis-kiszolgálók kommunikálnak egymással, és a középső rétegbeli alkalmazásokkal.

Az egyik probléma egy hálózat adminisztrálásakor az alkalmazások között nem megbízható hálózaton keresztül küldött adatok. Használhat [a TLS/SSL](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol) tooauthenticate kiszolgálók és ügyfelek majd a használatával tooencrypt üzeneteket hello hitelesített felek között.

Hello hitelesítési folyamatban egy TLS/SSL ügyfél küld egy üzenet tooa TLS/SSL-kiszolgálónak, és hello kiszolgáló hello kiszolgálóval kell magát tooauthenticate hello információkkal válaszol. hello ügyfél és kiszolgáló emellett végrehajtja a munkamenetkulcsok cseréjét, és a hitelesítési párbeszéd vége hello. Hitelesítés végbemegy, hello kiszolgáló és hello hello hitelesítési folyamat során létesített szimmetrikus titkosítási kulcsok használatával hello ügyfél megkezdődhet az SSL-lel védett kommunikáció.

Minden kapcsolatok tooAzure SQL-adatbázis titkosításának megkövetelése (SSL/TLS) minden alkalommal "az átvitel során" tooand hello adatbázisból közben. SQL Azure használja a TLS/SSL tooauthenticate kiszolgálók és ügyfelek és tooencrypt üzenetek hello hitelesített felek közötti használja. Kapcsolat-karakterláncban az alkalmazást akkor adja meg a paraméterek tooencrypt hello kapcsolat, és nem tootrust hello kiszolgálótanúsítvány (ebben az esetben, ha a kapcsolati karakterlánc hello klasszikus Azure portálon kívül), ellenkező esetben a kapcsolat lesz hello hello kiszolgáló nem hello személyazonosságát, és ki vannak téve túl "man-a-személyek" támadásainak lesz. Hello ADO.NET illesztőprogram, például a kapcsolódási karakterlánc paraméterek: Encrypt = True és TrustServerCertificate = False.

### <a name="encryption-at-rest"></a>Titkosítás inaktív állapotban
Több óvintézkedéseket toohelp biztonságos hello adatbázisban, például a biztonságos rendszerek tervezése, bizalmas eszközök titkosítása és felépítése körül hello adatbázis-kiszolgálók egy tűzfal is igénybe vehet. Azonban forgatókönyv esetében, ahol hello fizikai adathordozó (például meghajtókat vagy a biztonsági mentési szalagot) ellopják, a rosszindulatú fél csak visszaállíthatja vagy hello adatbázis csatolása és Tallózás hello adatokat.

Egy megoldás tooencrypt hello bizalmas adatok hello adatbázisban, és megakadályozza a hello kulcsokat is használt tooencrypt hello adatok tanúsítvánnyal. Ez megakadályozza, hogy bárki hello kulcsok nélkül hello adatokkal, de ilyen védelemre tervezett kell lennie.

Ez a probléma, az SQL Server és az Azure SQL támogatja toosolve [átlátszó Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/securityrecryption/transparent-data-encryption-tde). TDE titkosítja az SQL Server és az Azure SQL Database az adatfájlok, inaktív adatok titkosítása néven ismert.

Az Azure SQL Database átlátható adattitkosítás segít valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok és a tranzakciós naplófájlok nyugalmi anélkül, hogy a módosítások végrehajtásával a kártékony tevékenység hello fenyegetés elleni védelem toohello alkalmazás.  

TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja. Az SQL-adatbázis adatbázis-titkosítási kulcs hello védi egy beépített kiszolgálói tanúsítványt. hello beépített kiszolgálói tanúsítvány egyedi SQL-adatbázis kiszolgálónként.

Ha egy adatbázis részt GeoDR-kapcsolatban, védi egy másik kulcsot minden kiszolgálón. Ha két adatbázisok csatlakoztatott toohello ugyanarra a kiszolgálóra, ossza hello beépített ugyanazt a tanúsítványt. Microsoft legalább 90 naponta automatikusan elforgatja ezeket a tanúsítványokat. TDE általános ismertetését lásd: [átlátszó Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde).

### <a name="encryption-in-use-client"></a>Titkosítás használata (ügyfél)

A legtöbb, adatszivárgáshoz tartalmaz, amely hello ellopása kritikus fontosságú adatok, például hitelkártyaszámok vagy személyes azonosításra alkalmas adatokat. Adatbázisok kincsem troves a bizalmas adatok is lehetnek. Tartalmazhat olyan ügyfelek személyes adatokat, a versenyképesség fenntartása érdekében bizalmas információkat és a szellemi tulajdont. Elveszett vagy ellopott adatok, különösen a felhasználói adatok, a márka kárt, versenyképes hátránya és súlyos bírságok eredményezhet – peres eljárások is.

![Always Encrypted](./media/azure-databse-security-overview/azure-database-fig1.png)

[Mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx) egy szolgáltatás, tooprotect bizalmas adatokat, hitelkártyaszámok vagy a nemzeti azonosító szám (például Egyesült államokbeli társadalombiztosítási számot), például az Azure SQL Database vagy az SQL Server adatbázis tárolja. Titkosított mindig lehetővé teszi az ügyfelek tooencrypt ügyfélalkalmazások található bizalmas adatok, és soha ne adja meg hello titkosítási kulcsok toohello adatbázis-kezelő (SQL-adatbázis vagy SQL Server).

Mindig titkosított biztosít egy elkülönítése is megvalósuljon azok, akik saját hello adatok (és megtekinthesse) és azokra, akik hello adatok kezelése (de nem hozzáféréssel kell rendelkeznie). Biztosítja a helyi adatbázis-rendszergazdák, a felhő adatbázis operátorok, vagy más kiemelt jogosultságú, illetéktelen felhasználókkal szemben, de nem tudja elérni a hello titkosított adatok,

Mindig titkosítja emellett lehetővé teszi a titkosítás átlátható tooapplications. Egy mindig titkosítja-engedélyezve van telepítve illesztőprogram hello ügyfélszámítógépen, hogy azt is automatikusan titkosításához és visszafejtéséhez hello ügyfélalkalmazás lévő bizalmas adatokat. hello illesztőprogram hello adatok toohello adatbázismotor előtt titkosítja az érzékeny oszlopok hello adatokat, és automatikusan a úgy, hogy hello szemantikáját toohello alkalmazás megőrződnek újraírja lekérdezések. Hasonlóképpen a hello illesztőprogram transzparens módon visszafejti a titkosított oszlopokat, a lekérdezés eredményei között szereplő tárolt adatok.

## <a name="access-control"></a>Hozzáférés-vezérlés
tooprovide biztonsági, SQL-adatbázis szabályozza a hozzáférést igénylő felhasználók tooprove hitelesítési mechanizmusok korlátozása kapcsolat IP-címe, tűzfal-szabályokat a személyazonosságukat, és korlátozza a felhasználók toospecific műveletek és az adatok engedélyezési mechanizmusokat.

### <a name="database-access"></a>Adatbázis-elérés

Az adatvédelem első tooyour adatok hozzáférés szabályozása. hello datacenter adatait tartalmazó fizikai hozzáférés kezeli, amíg egy tűzfal toomanage biztonsági hello hálózati rétegben konfigurálhatja. Akkor is hozzáférést bejelentkezések a hitelesítés konfigurálása és engedélyek a kiszolgáló és az adatbázis-szerepkörök meghatározása.

Ez a szakasz a döntésről bővebben:

-   Tűzfal és tűzfalszabályok
-   Authentication
-   Engedélyezés

#### <a name="firewall-and-firewall-rules"></a>Tűzfal és tűzfalszabályok

A Microsoft Azure SQL Database egy relációs adatbázis-szolgáltatást nyújt az Azure és egyéb internetalapú alkalmazások számára. az adatok védelméhez toohelp, tűzfalak tooyour adatbázis-kiszolgáló minden hozzáférés tiltása, amíg megadhatja, hogy mely számítógépek rendelkeznek engedéllyel. hello tűzfal engedélyezi a hozzáférést toodatabases származó IP-cím az egyes kérelmek hello alapján. További információkért lásd: [Az Azure SQL Database-tűzfalszabályok áttekintése](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

Hello [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) szolgáltatás 1433-as TCP-porton keresztül csak érhető el. a számítógép egy SQL-adatbázis tooaccess győződjön meg arról, hogy az ügyfél számítógép tűzfal engedélyezi-e az 1433-as TCP-port kimenő TCP-kommunikáció. Ha más alkalmazásoknak nincs szüksége rá, akkor blokkolja az 1433-as TCP-port bejövő kapcsolatait.

#### <a name="authentication"></a>Authentication

SQL-adatbázis hitelesítési toohow, akkor a személyazonosság toohello adatbázishoz kapcsolódáskor hivatkozik. Az SQL Database két hitelesítési típust támogat:

-   **SQL-hitelesítés:** logikai SQL-példány létrehozása után hello SQL adatbázis előfizető fiók neve, létrejön egy egyszeri bejelentkezési fiók. Ez a fiók csatlakozik használatával [SQL Server-hitelesítés](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview) (felhasználónév és jelszó). Ezt a fiókot, a rendszergazda és az összes felhasználói adatbázisokon hello logikai server-példányon van csatolva toothat példány. hello előfizető fiók hello engedélyei nem korlátozhatók. Ilyen fiókból csak egy létezhet.
-   **Az Azure Active Directory-hitelesítés:** [az Azure Active Directory hitelesítési](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) tooMicrosoft Azure SQL Database és az SQL Data Warehouse identitásokkal az Azure Active Directoryban (Csatlakozás egy mechanizmus Az Azure AD). Ez lehetővé teszi, hogy toocentrally identitáskezelést adatbázis-felhasználók.

![Authentication](./media/azure-databse-security-overview/azure-database-fig2.png)

 Azure Active Directory-hitelesítés előnyei a következők:
  - Egy alternatív tooSQL kiszolgálóhitelesítés biztosít.
  - Is segít hello elterjedése tartozó felhasználói azonosítók adatbázis-kiszolgáló között, és lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen.
  - Adatbázis-engedélyek külső (az Azure Active Directory) csoportok használatával kezelheti.
  - Integrált Windows-hitelesítés és egyéb Azure Active Directory által támogatott hitelesítési engedélyezésével azt megnövelésével kiküszöbölheti jelszavak tárolását.

#### <a name="authorization"></a>Engedélyezés
[Engedélyezési](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins) toowhat hivatkozik egy felhasználó egy Azure SQL Database belül végrehajtható, és ez által a felhasználói fiók adatbázis [szerepkörtagságok](https://msdn.microsoft.com/library/ms189121) és [objektumszintű engedélyeket](https://msdn.microsoft.com/library/ms191291.aspx). Engedélyezési az hello folyamat meghatározása során a biztonságos erőforrások rendszerbiztonsági tag férhetnek hozzá, és milyen műveletek az adott erőforrás használata engedélyezett.

### <a name="application-access"></a>Alkalmazás-hozzáférés

Ez a szakasz a döntésről bővebben:

-   Dinamikus adatmaszkolás
-   Sorszintű biztonság

#### <a name="dynamic-data-masking"></a>Dinamikus adatmaszkolás
A képviselőjével egy ügyfélszolgálatával, előfordulhat, hogy azonosíthatja a hívók társadalombiztosítási szám vagy hitelkártyaszám több számjegyet, de ilyen elemek nem kell teljesen adatok kitett toohello munkatársának.

Egy maszkolási szabályra meghatározása, hogy minden maszkok, de hello utolsó négy számjegyét társadalombiztosítási szám vagy hitelkártyaszám hello eredménykészletben bármely lekérdezés.

![Dinamikus adatmaszkolás](./media/azure-databse-security-overview/azure-database-fig3.png)

Másik példaként a megfelelő adatok maszk lehet meghatározott tooprotect személyes azonosításra alkalmas adatokat (PII) adatok, úgy, hogy a fejlesztő lekérdezheti a termelési környezetben hibaelhárítási célból megfelelőségi szabályzat megsértése nélkül.

[SQL adatbázis dinamikus Adatmaszkolási](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) bizalmas adatok veszélyeztetettségének korlátozása maszkolás azt toonon jogosultságú felhasználók által. Dinamikus adatmaszkolási Azure SQL Database 12-es verziójának hello támogatott.

[Dinamikus adatmaszkolási](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) segítségével megakadályozható az illetéktelen hozzáféréstől toosensitive adatokat azáltal, hogy engedélyezi toodesignate mennyi hello bizalmas adatok tooreveal hello alkalmazás réteg gyakorolt minimális hatás mellett. Egy csoportházirend-alapú biztonsági funkció, hello bizalmas adatok hello eredménykészletben lekérdezés elrejti a kijelölt adatbázis mezők keresztül közben hello adatbázis hello adatai nem változik.


> [!Note]
> Dinamikus adatmaszkolási hello Azure adatbázis admin, kiszolgálói rendszergazda vagy biztonsági tisztviselő szerepköröket konfigurálhatja.

#### <a name="row-level-security"></a>Sorszintű biztonság
Több-bérlős adatbázisok egy másik közös biztonsági követelménye [sorszintű biztonság](https://msdn.microsoft.com/library/dn765131.aspx). A szolgáltatás lehetővé teszi toocontrol hozzáférés toorows egy adatbázis tábláinak hello jellemzők hello felhasználó (pl. csoport tagsági vagy a végrehajtási környezet) lekérdezése alapján.

![A biztonsági szint sor](./media/azure-databse-security-overview/azure-database-fig4.png)

hello hozzáférés korlátozási logika hello adatbázis rétegben található, hanem egy másik alkalmazás réteg adataiból hello funkcióval. hello adatbázisrendszer hello hozzáférési korlátozásokat alkalmazza, minden alkalommal, amikor az adott adatok próbál meg elérni a réteg alapján. Így a biztonsági rendszer megbízhatóbb és robusztus csökkenti a biztonsági rendszer hello támadási felületét.

Sorszintű biztonság predikátum alapú hozzáférés-vezérlés vezet be. A rugalmas, a központi, a predikátum alapú próbaverzióra által elvégezhető szempont metaadatok vagy bármely más feltételek hello rendszergazdája határozza meg a megfelelő jellemzői. hello predikátum egy feltétel toodetermine lesz-e hello felhasználó rendelkezik-e hello megfelelő hozzáférési toohello adatokat a felhasználói attribútumok alapján. Címke-alapú hozzáférés-vezérlés predikátum-alapú hozzáférés-vezérlés használatával valósítható meg.

## <a name="proactive-monitoring"></a>Proaktív figyelés
SQL-adatbázis titkosítja az adatokat, adja meg a **naplózás** és **veszélyforrások detektálása** képességeit.

### <a name="auditing"></a>Naplózás
SQL Database Auditing növeli a képes toogain betekintést események és hello adatbázisban, beleértve a frissítéseket és a lekérdezések hello adatok alapján elvégzett módosításokat.

[Az Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) követi az adatbázisban, majd beírja őket az Azure Storage-fiókban tooan audit napló. A naplózás segíthet a jogszabályi megfelelőség fenntartásában és az adatbázison végzett tevékenység megértésében, valamint az esetleg üzleti veszélyeket vagy biztonsági problémákat jelző rendellenességek feltárásában. Naplózás lehetővé teszi, hogy és betartása toocompliance szabványok elősegíti, de nem biztosítja a megfelelőség.

SQL Database Auditing teszi lehetővé:

-   **Tartsa meg** kijelölt események egy naplóban. Adatbázis-műveletek toobe naplózva kategóriáinak adhat meg.
-   **A jelentés** adatbázis-tevékenység. Használhat előre konfigurált jelentések és egy irányítópult tooget használatának gyors megkezdése tevékenységet és az események naplózásához.
-   **Elemezze** jelentéseket. Gyanús események, a szokatlan tevékenység és a trendeket találja.

A naplózás két módszer áll rendelkezésre:

-   **Blobnaplózási funkció** -írja tooAzure Blob Storage a naplókat. Ez az újabb naplózási módszer, amely nagyobb teljesítményt nyújt, támogatja a nagyobb részletességgel objektumszintű naplózást, és költséghatékonyabb megoldás.
-   **Tábla naplózás** -írja tooAzure Table Storage a naplókat.

### <a name="threat-detection"></a>Fenyegetések észlelése
[Az Azure SQL Database fenyegetésészlelés](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) gyanús tevékenységek észlelése esetén, amelyek esetleges biztonsági fenyegetéseket jelezhetnek jelzik. A fenyegetésészlelés lehetővé teszi toorespond toosuspicious események hello adatbázisban, például az SQL-utasítások, azok bekövetkezésekor. Riasztások biztosít, és engedélyezi, hogy hello Azure SQL Database Auditing tooexplore hello gyanús események.

![A fenyegetésészlelés](./media/azure-databse-security-overview/azure-database-fig5.jpg)

Például SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások. A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások alkalmazás mezőkbe megsértése vagy hello adatbázis adatainak módosítása.

Biztonsági tisztviselő vagy más kijelölt rendszergazdák kérheti le az azonnali értesítések adatbázis gyanús tevékenységekről azok bekövetkezésekor. Minden értesítés részletesen hello gyanús tevékenységekre, illetve hogyan toofurther vizsgálja meg, és hello fenyegetések mérséklésére javasolja.        

## <a name="centralized-security-management"></a>Központi kezelés

[Az Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) megakadályozása, észlelésében és kezelésében toothreats segít. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

[A Security Center](https://docs.microsoft.com/azure/security-center/security-center-sql-database) megakadályozhatja a adatokat az SQL-adatbázis azáltal, hogy a kiszolgálók és adatbázisok hello biztonságának betekintést nyújt segítséget. A Security Center a következőket teheti:

-   SQL adatbázis-titkosítás és naplózási házirendek meghatározása.
-   Az előfizetések közötti hello biztonsági SQL adatbázis-erőforrások figyelése
-   Gyorsan problémák azonosítása és javítása biztonsági.
-   Riasztások integrálni [Azure SQL Database fenyegetésészlelés](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
-   Biztonsági központ szerepkörön alapuló hozzáférés támogatja.

## <a name="azure-marketplace"></a>Azure Piactér

hello Azure piactér egy online alkalmazásokhoz és szolgáltatásokhoz piactérre, amely lehetővé teszi, hogy újonnan induló vállalkozások és független keresve toooffer hello világ tooAzure megoldások ügyfeleiknek.
hello Azure piactér hatékonyan ötvözi a Microsoft Azure partner ökoszisztéma be egy egyetlen, egységes platform toobetter kiszolgálásához, az ügyfelek és partnerek. Kattintson a [Itt](https://azuremarketplace.microsoft.com/marketplace/apps?search=Database%20Security&page=1) tooglance adatbázis biztonsági termékeinek az Azure piactéren elérhető.

## <a name="next-steps"></a>Következő lépések

- További információ [az Azure SQL Database biztonságos](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
- További információ [az Azure Security Center és az Azure SQL Database szolgáltatás](https://docs.microsoft.com/azure/security-center/security-center-sql-database).
- toolearn fenyegetésészlelés, kapcsolatos további információkért lásd: [SQL adatbázis Fenyegetésészlelés](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- több, lásd: toolearn [javítása SQL-adatbázis teljesítményének](https://docs.microsoft.com/azure/sql-database/sql-database-performance-tutorial). 
