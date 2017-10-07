---
title: "-Microsoft fenyegetések modellezése eszköz - aaaCryptography Azure |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz hello felfedett fenyegetések megoldást"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: cab981bf116a0e76bbf44fe0f0a1a3650e4ab0f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-cryptography--mitigations"></a>Biztonsági keret: Titkosítás |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Webalkalmazás** | <ul><li>[Csak a jóváhagyott szimmetrikus blokk Rejtjelek és a kulcshosszok](#cipher-length)</li><li>[Használja a jóváhagyott blokk titkosítási módok és szimmetrikus titkosítási inicializálási vektor ismert](#vector-ciphers)</li><li>[Jóváhagyott aszimmetrikus algoritmusok, kulcshossz és kitöltési használata](#padding)</li><li>[Jóváhagyott véletlenszerű szám generátorokat használata](#numgen)</li><li>[Ne használjon szimmetrikus adatfolyam rejtjel.](#stream-ciphers)</li><li>[Jóváhagyott MAC/HMAC/kulccsal kivonatoló algoritmusok használata](#mac-hash)</li><li>[Csak a jóváhagyott titkosítási kivonatot funkciók](#hash-functions)</li></ul> |
| **Adatbázis** | <ul><li>[Erős titkosítási algoritmusok tooencrypt adatokat hello adatbázis használata](#strong-db)</li><li>[SSIS-csomagok legyen titkosítva, és digitálisan aláírt](#ssis-signed)</li><li>[Digitális aláírás toocritical adatbázis securables hozzáadása](#securables-db)</li><li>[SQL server EKM tooprotect titkosítási kulcsok használata](#ekm-keys)</li><li>[Használják AlwaysEncrypted funkciót, ha a titkosítási kulcsok nem lehet feltárt tooDatabase motor](#keys-engine)</li></ul> |
| **IoT-eszközök** | <ul><li>[Kriptográfiai kulcsokat tárolja biztonságos helyen az IoT-eszközök](#keys-iot)</li></ul> | 
| **Az IoT átjáró** | <ul><li>[Hitelesítési tooIoT Hub hosszúnak egy véletlenszerű szimmetrikus kulcs létrehozása](#random-hub)</li></ul> | 
| **Dynamics CRM mobil ügyfél** | <ul><li>[Győződjön meg arról, egy eszköz-kezelési házirenddel van, amely csak a PIN-kód használatát igényli, és lehetővé teszi a távoli törlése](#pin-remote)</li></ul> | 
| **Dynamics CRM Outlook ügyfélben** | <ul><li>[Győződjön meg arról egy eszköz-kezelési házirenddel, amely csak a PIN-kód/jelszó vagy automatikus zárolás igényel, és titkosítja az összes adatot (pl. Bitlocker)](#bitlocker)</li></ul> | 
| **Identity Serverben** | <ul><li>[Győződjön meg arról, hogy aláíró kulcsok vannak tanúsítványváltást Identitáskiszolgálók használata esetén](#rolled-server)</li><li>[Győződjön meg arról, hogy erős titkosítással ügyfél-azonosító, ügyfélkulcs használja az Identity Serverben](#client-server)</li></ul> | 

## <a id="cipher-length"></a>Csak a jóváhagyott szimmetrikus blokk Rejtjelek és a kulcshosszok

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Termékek csak blokk szimmetrikus titkosítási és társított kulcshosszokat hello titkosítási Advisor a szervezet által explicit módon jóváhagyott kell használnia. A Microsoftnál engedélyezett szimmetrikus algoritmusok blokk Rejtjelek következő hello tartalmazza:</p><ul><li>Új kód AES-128, AES-192 és AES-256-ot is elfogadható</li><li>A meglévő kód visszamenőleges kompatibilitás érdekében három-kulcs 3DES elfogadható</li><li>A termékek szimmetrikus blokk Rejtjelek használatával:<ul><li>Advanced Encryption Standard (AES) szükség az új kódot</li><li>Három-kulcs triple Data Encryption Standard (3DES) megengedett a meglévő kódot a visszamenőleges kompatibilitás érdekében</li><li>Minden más blokk Rejtjelek, többek között a RC2, DES, 2 kulcs 3DES, DESX és bonító, csak akkor használható, a régi adatok visszafejtése, és le kell cserélni, ha a titkosításhoz használt</li></ul></li><li>Szimmetrikus blokkot a titkosítási algoritmusok a minimális kulcshosszúság 128 bit szükség. egyetlen új kódot ajánlott blokk titkosítási algoritmus az AES hello (az AES-128, AES-192 és AES-256 elfogadhatók összes)</li><li>Három-kulcs 3DES jelenleg elfogadható, ha már használatban a meglévő kód; átmenet tooAES ajánlott. DES, DESX, RC2 és bonító nem tekinti biztonságos. Ezek az algoritmusok csak akkor használható, a visszamenőleges kompatibilitás érdekében hello szakét meglévő adatainak visszafejtése, és adatokat kell újból titkosítja a javasolt blokktitkosításon</li></ul><p>Vegye figyelembe, hogy minden szimmetrikus blokk Rejtjelek kell használni egy jóváhagyott titkosítási mód, amely egy megfelelő inicializálási vektor (IV) használatát igényli. Egy megfelelő IV általában egy véletlenszerű számot, és soha nem állandó érték</p><p>(mint megakadályozását toowriting új adatokat) meglévő adatainak olvasása hello örökölt vagy egyéb nem jóváhagyott titkosítási algoritmusokat és kulcshosszokat kisebb használatát a szervezet titkosítási Board áttekintése után engedélyezhető. Azonban meg kell a fájl a kivételt a követelménnyel szemben. Emellett a vállalati telepítésekben termékek gondolja át figyelmeztetés rendszergazdák használt tooread adatok esetén a gyenge titkosítás. Ilyen figyelmeztetések magyarázó és végrehajthatóként kell lennie. Bizonyos esetekben lehet megfelelő toohave csoportházirend vezérlő hello használata gyenge titkosítás</p><p>.NET-algoritmusok használata felügyelt titkosítási agilitást engedélyezett (preferencia szerinti sorrendben)</p><ul><li>AesCng (FIPS-kompatibilis)</li><li>AuthenticatedAesCng (FIPS-kompatibilis)</li><li>AESCryptoServiceProvider (FIPS-kompatibilis)</li><li>(Nem érvényes a FIPS-kompatibilis) AESManaged</li></ul><p>Vegye figyelembe, hogy ezek az algoritmusok egyike adható meg keresztül hello `SymmetricAlgorithm.Create` vagy `CryptoConfig.CreateFromName` módszerek anélkül, hogy a módosítások toohello machine.config fájlban. Azt is, vegye figyelembe, hogy az előzetes too.NET .NET 3.5-ös verziójában AES neve `RijndaelManaged`, és `AesCng` és `AuthenticatedAesCng` vannak > Codeplex webhelyen keresztül elérhető és az operációs rendszer alapjául szolgáló hello CNG igényel</p>

## <a id="vector-ciphers"></a>Használja a jóváhagyott blokk titkosítási módok és szimmetrikus titkosítási inicializálási vektor ismert

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Minden szimmetrikus blokk Rejtjelek egy jóváhagyott szimmetrikus titkosítási mód együtt kell használni. csak a jóváhagyott hello módok a következők: CBC és CTS. Különösen el kell kerülni a hello elektronikus kód könyv (ECB) módú működés; ECB használatát a szervezet titkosítási Board felülvizsgálatot igényel. Minden használati OFB, a CFB, a Parancsra, a CCM, és a GCM vagy más titkosítási mód a szervezet titkosítási Board kell vizsgálnia. Újbóli felhasználása hello azonos inicializálási vektor (IV) rendelkező blokk Rejtjelek "streaming Rejtjelek módok" Parancsra, például a derüljön ki a titkosított adatok toobe okozhat. Minden szimmetrikus blokk Rejtjelek is használható kell egy megfelelő inicializálási vektor (IV). Egy megfelelő IV egy kriptográfiai erős, véletlenszerű számot, és soha nem egy állandó értékkel. |

## <a id="padding"></a>Jóváhagyott aszimmetrikus algoritmusok, kulcshossz és kitöltési használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>tiltott titkosítási algoritmusok hello használata jelentős kockázat tooproduct biztonsági vezet be, és el kell kerülni. Termékek kell használnia csak titkosítási algoritmusokat és társított kulcshosszokat és kitöltési explicit módon a szervezet titkosítási Bizottság által jóváhagyott.</p><ul><li>**RSA -** titkosítás, a kulcscsere és az aláírási használható. RSA-titkosítás csak hello OAEP típusú vagy RSA-KEM kitöltési módot kell használnia. A meglévő kódot PKCS #1 1.5-ös verzióját kitöltési mód csak a kompatibilitás használhat. Használjon null szövegtávolság explicit módon le van tiltva. Kulcsok > = 2048 bit szükséges új kódot. A meglévő kódot támogathatja a kulcsok < 2048 bit, csak az visszamenőleges kompatibilitás a szervezet titkosítási testület áttekintése után. Kulcsok < 1024 bit a régi adatok visszafejtése/ellenőrzése csak akkor használható, és kicserélt if használandó titkosítási és aláírási műveletekhez</li><li>**ECDSA -** csak aláírás használható. Az ECDSA > =, 256 bites kulcsok szükséges új kódot. ECDSA alapú aláírások hello három NIST jóváhagyott görbék egyikét kell használnia (P-256, 384 vagy P521). Alaposan elemzett görbék csak a szervezete titkosítási Board áttekintése után is használható.</li><li>**ECDH -** csak a kulcscseréhez használt használható. A ECDH > 256 bites kulcsok = új kód szükséges. Kulcscsere ECDH alapú hello három NIST jóváhagyott görbék egyikét kell használnia (P-256, 384 vagy P521). Alaposan elemzett görbék csak a szervezete titkosítási Board áttekintése után is használható.</li><li>**DSA -** áttekintésre és jóváhagyásra a szervezet titkosítási tábláról után elfogadható lehet. A biztonsági tanácsadó tooschedule Forduljon a szervezet titkosítási Board áttekintése. Ha Ön miként használja a DSA jóvá van hagyva, ne feledje, hogy fog kulcsok használata tooprohibit kisebb, mint 2048 bit hosszúságú. CNG támogatja a Windows 8-től 2048 bites és nagyobb kulcshosszokat.</li><li>**Diffie-Hellman -** munkamenet kulcskezelés csak használható. Kulcshossz > = 2048 bit szükséges új kódot. A meglévő kódot támogathatja a kulcshossza < 2048 bit, csak az visszamenőleges kompatibilitási a szervezet titkosítási testület áttekintése után. Kulcsok < 1024 bit nem használható.</li><ul>

## <a id="numgen"></a>Jóváhagyott véletlenszerű szám generátorokat használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Termékek jóváhagyott véletlenszerű szám generátorokat kell használnia. Pseudorandom funkciókat, például a hello C futásidejű függvény VÉL, hello .NET-keretrendszer osztály System.Random vagy rendszer funkciókat, például a GetTickCount kell, ezért soha nem használhatók ilyen kódban. Hello kettős elliptikus görbéjű véletlenszerű szám generátor (DUAL_EC_DRBG) algoritmus használata nem engedélyezett</p><ul><li>**CNG -** BCryptGenRandom (használata ajánlott, ha bármely IRQL [Ez azt jelenti, hogy PASSIVE_LEVEL] 0-nál nagyobb lehet, hogy futtassa hello hívó hello BCRYPT_USE_SYSTEM_PREFERRED_RNG jelző)</li><li>**CAPI -** cryptGenRandom</li><li>**A Win32/64-** (új megvalósítások használja BCryptGenRandom vagy CryptGenRandom) RtlGenRandom * rand_s * SystemPrng (a kernel mód)</li><li>**. A NET -** RNGCryptoServiceProvider vagy RNGCng</li><li>**A Windows Store Apps -** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom vagy. GenerateRandomNumber</li><li>**Apple OS X (10.7+)/iOS(2.0+) -** int SecRandomCopyBytes (SecRandomRef véletlenszerű, size_t száma, uint8_t *bájt)</li><li>**Apple OS X (< 10.7)-** használatát / fejlesztői/véletlenszerű tooretrieve véletlenszerű számok</li><li>**Java(including Google Android Java Code) -** java.security.SecureRandom osztály. Fontos: az Android 4.3 (ún Bean), a fejlesztők kell Android ajánlott megoldás hello kövesse, és frissítse az alkalmazások tooexplicitly hello PRNG /dev/urandom vagy /dev/random entrópia rendelkező inicializálása</li></ul>|

## <a id="stream-ciphers"></a>Ne használjon szimmetrikus adatfolyam rejtjel.

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Szimmetrikus adatfolyam Rejtjelek, például az RC4, nem kell használni. Szimmetrikus adatfolyam Rejtjelek helyett termékek használjon a blokktitkosításon, kifejezetten AES legalább 128 bites kulcsot használnak. |

## <a id="mac-hash"></a>Jóváhagyott MAC/HMAC/kulccsal kivonatoló algoritmusok használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Termékek csak a jóváhagyott üzenethitelesítő kódot (MAC) vagy a kivonat-alapú üzenethitelesítési kód (HMAC) algoritmusok kell használnia.</p><p>Üzenethitelesítő kódot (MAC) egy információs csatolt tooa üzenet, amely lehetővé teszi, hogy a címzett tooverify mindkét hello hitelességét hello feladó és egy titkos kulccsal üdvözlőüzenetére hello sértetlenségét. hello használata vagy a kivonat-alapú MAC ([HMAC](http://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)) vagy [titkosítási-blokkalapú MAC](http://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) akkor megengedett, ha minden alapul szolgáló kivonatoló vagy szimmetrikus titkosítási algoritmussal vannak is használatra jóváhagyott; jelenleg ez tartalmazza a HMAC-SHA2 funkciók (HMAC-SHA-256, HMAC-SHA384 és HMAC-SHA512) hello és hello CMAC/OMAC1 és OMAC2 blokkolja a Mac számítógépek (alapulhat AES) titkosító-alapú.</p><p>Lehet, hogy a HMAC-SHA1 használata megengedett platform kompatibilitás, de meg lesz szükséges toofile egy kivétel toothis eljárást, majd a szervezet titkosítási felülvizsgálati változni. Csonkolási üzenetkivonatokat tooless 128 bit a nem engedélyezett. Ügyfél módszerek toohash kulcs és az adatok nincs jóváhagyva, és a szervezet titkosítási Board tekintse át a korábbi toouse kell alávetni.</p>|

## <a id="hash-functions"></a>Csak a jóváhagyott titkosítási kivonatot funkciók

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Termékek hello SHA-2 család kivonatoló algoritmus (SHA-256, SHA384 és az SHA512) kell használnia. Egy rövidebb kivonatot van szüksége, például a 128 bites kimeneti hosszánál rendelés toofit olyan adatszerkezet kialakítása során hello rövidebb MD5 kivonatoló szem előtt, a termékekért felelős csoportokkal hello SHA2 kivonatok (általában az SHA256 titkosítást) egyik lehetséges, hogy csonkolja. Vegye figyelembe, hogy SHA384 SHA512 csonkolt verzióját. A biztonsági célra tooless 128 bit a kriptográfiai kivonatokat csonkolása nem engedélyezett. Új kód nem használhatnak hello MD2, MD4, MD5, SHA-0, SHA-1 vagy RIPEMD kivonatoló algoritmus. Kivonatütköztetés számításilag megvalósítható az ezek az algoritmusok, ami gyakorlatilag feltörésüket.</p><p>A felügyelt titkosítási agilitást .NET kivonatoló algoritmusainak engedélyezett (preferencia szerinti sorrendben):</p><ul><li>SHA512Cng (FIPS-kompatibilis)</li><li>SHA384Cng (FIPS-kompatibilis)</li><li>SHA256Cng (FIPS-kompatibilis)</li><li>SHA512Managed (nem érvényes a FIPS-kompatibilis) (használja az SHA512 algoritmust neveként hívások tooHashAlgorithm.Create vagy CryptoConfig.CreateFromName)</li><li>SHA384Managed (nem érvényes a FIPS-kompatibilis) (használja az SHA384 a hívások tooHashAlgorithm.Create vagy CryptoConfig.CreateFromName algoritmus neve)</li><li>SHA256Managed (nem érvényes a FIPS-kompatibilis) (használja az SHA-256 algoritmus neve hívások tooHashAlgorithm.Create vagy CryptoConfig.CreateFromName)</li><li>SHA512CryptoServiceProvider (FIPS-kompatibilis)</li><li>SHA256CryptoServiceProvider (FIPS-kompatibilis)</li><li>SHA384CryptoServiceProvider (FIPS-kompatibilis)</li></ul>| 

## <a id="strong-db"></a>Erős titkosítási algoritmusok tooencrypt adatokat hello adatbázis használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Egy titkosítási algoritmus kiválasztása](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Lépések** | Titkosítási algoritmusok határozza meg, amelyek jogosulatlan felhasználók számára nem lehet könnyen visszavonni az adatátalakításokat. SQL Server lehetővé teszi, hogy a rendszergazdák és fejlesztők toochoose közül több algoritmusokat, beleértve az DES, háromszoros DES, TRIPLE_DES_3KEY, RC2, RC4, 128 bites RC4, DESX, 128 bites AES, 192 bites AES és 256 bites AES |

## <a id="ssis-signed"></a>SSIS-csomagok legyen titkosítva, és digitálisan aláírt

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Azonosítsa a hello csomagok forrás digitális aláírással megőrzött](https://msdn.microsoft.com/library/ms141174.aspx), [fenyegetés és biztonsági megoldás (integrációs szolgáltatások)](https://msdn.microsoft.com/library/bb522559.aspx) |
| **Lépések** | hello csomag forrása egyedi hello vagy a szervezet által létrehozott hello csomag. Ismeretlen vagy nem megbízható forrásból származó csomag futtató kockázatos lehet. tooprevent nem engedélyezett, illetéktelen módosításának SSIS-csomagok, digitális aláírások kell használni. Továbbá tooensure hello bizalmas hello csomagok tárolási/átvitel során, SSIS-csomagok, hogy titkosított toobe |

## <a id="securables-db"></a>Digitális aláírás toocritical adatbázis securables hozzáadása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [ALÁÍRÁS (Transact-SQL) hozzáadása](https://msdn.microsoft.com/library/ms181700) |
| **Lépések** | Esetekben, amikor biztonságos kritikus adatbázis hello sértetlenségének ellenőrzése toobe digitális aláírások kell használni. Például egy tárolt eljárás, függvény, assembly vagy eseményindító adatbázis securables digitális aláírás. Az alábbiakban látható egy példa, ha ez akkor lehet hasznos: ossza meg velünk tegyük fel például, egy független Szoftverszállító (független szoftvergyártók) nyújtott támogatás tooa kézbesíteni szoftver tooone több, a ügyfelek. Támogatásának biztosítása előtt hello ISV azt szeretné, hogy biztonságos hello szoftverfrissítési adatbázis nem sérült vagy módosították vagy véletlenül vagy rosszindulatú kísérlet tooensure. Biztonságos hello digitálisan aláírt, hello ISV ellenőrizheti a digitális aláírás és a sértetlenségének ellenőrzése.| 

## <a id="ekm-keys"></a>SQL server EKM tooprotect titkosítási kulcsok használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [SQL Server a bővíthető kulcskezelési (EKM)](https://msdn.microsoft.com/library/bb895340), [a bővíthető kulcskezelési használata az Azure Key Vault (SQL Server)](https://msdn.microsoft.com/library/dn198405) |
| **Lépések** | SQL Server bővíthető kulcskezelési lehetővé teszi, hogy a hello hello adatbázis fájlok toobe egy be eszköz, például intelligens kártyával, USB-eszköz vagy EKM/HSM modul tárolt védő titkosítási kulcsokat. Ennek a szolgáltatásnak köszönhetően az adatok védelmét a adatbázis-rendszergazdák (kivéve a hello SysAdmin (rendszergazda) csoport tagjai). Adatok titkosíthatók, hogy csak hello adatbázis felhasználó rendelkezik hozzáférési tooon hello külső EKM/HSM modul titkosítási kulcsok használatával. |

## <a id="keys-engine"></a>Használják AlwaysEncrypted funkciót, ha a titkosítási kulcsok nem lehet feltárt tooDatabase motor

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Az SQL Azure, a helyi üzemeltetésű |
| **Attribútumok**              | SQL-verzió - 12-es verzió, MsSQL2016 |
| **Hivatkozások**              | [Mindig titkosítja (adatbázismotor)](https://msdn.microsoft.com/library/mt163865) |
| **Lépések** | Mindig titkosított egy szolgáltatás, tooprotect érzékeny adatok, például hitelkártyaszámok vagy a nemzeti azonosító szám (pl. Egyesült államokbeli társadalombiztosítási számot), az Azure SQL Database vagy az SQL Server adatbázis tárolja. Titkosított mindig lehetővé teszi az ügyfelek tooencrypt ügyfélalkalmazások található bizalmas adatok, és soha ne adja meg hello titkosítási kulcsok toohello adatbázis-kezelő (SQL-adatbázis vagy SQL Server). Ennek eredményeképpen Always Encrypted funkciója saját hello adatok (és megtekinthesse) elkülönítése és azokra, akik hello adatok kezelése (de nem hozzáféréssel kell rendelkeznie) |

## <a id="keys-iot"></a>Kriptográfiai kulcsokat tárolja biztonságos helyen az IoT-eszközök

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | IoT-eszközök | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Az eszköz operációs rendszere - Windows IoT Core Eszközkapcsolatok - Azure IoT eszközoldali SDK-k |
| **Hivatkozások**              | [A Windows IoT Core TPM](https://developer.microsoft.com/windows/iot/docs/tpm), [Windows IoT alapvető TPM beállítása](https://developer.microsoft.com/windows/iot/win10/setuptpm), [Azure IoT eszköz SDK TPM](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Lépések** | Symmetric vagy a tanúsítvány titkos kulcsokat biztonságosan hardveres tárolási például TPM vagy intelligens kártyás processzorok védett. Windows 10 IoT Core hello felhasználói TPM támogatja, és nincsenek használható több kompatibilis TPM: https://developer.microsoft.com/windows/iot/win10/tpm. Az ajánlott toouse, a belső vezérlőprogram vagy különálló TPM. A szoftver TPM csak fejlesztési és tesztelési célra használható. Miután érhető el TPM, és azt hello kulcsok törlődnek, hello token generáló kódot hello rögzített kódolási benne bizalmas információk nélkül lehet írni. | 

### <a name="example"></a>Példa
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on hello TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Amint is látható, hello eszköz elsődleges kulcsa megtalálható nem hello kódot. Ehelyett tárolódik hello TPM: adatszalagot a 0. TPM eszköz hoz létre egy rövid élettartamú SAS-tokent majd használt tooconnect toohello IoT-központot. 

## <a id="random-hub"></a>Hitelesítési tooIoT Hub hosszúnak egy véletlenszerű szimmetrikus kulcs létrehozása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Átjáró choice - Azure IoT Hub |
| **Hivatkozások**              | N/A  |
| **Lépések** | Az IoT-központ Identitásjegyzékhez eszköz tartalmaz, és közben az eszköz kiépítése, automatikusan létrehoz egy véletlenszerű szimmetrikus kulcsot. Ez a szolgáltatás hello Azure IoT Hub identitás toogenerate hello beállításkulcs-hitelesítéshez használt toouse ajánlott. Az IoT-központ is lehetővé teszi egy kulcs toobe hello eszköz létrehozásakor megadott. Ha a kulcs előállítási kívül IoT Hub eszköz kiépítése során, ajánlott toocreate véletlenszerű szimmetrikus kulcs, vagy legalább 256 bit. |

## <a id="pin-remote"></a>Győződjön meg arról, egy eszköz-kezelési házirenddel van, amely csak a PIN-kód használatát igényli, és lehetővé teszi a távoli törlése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM mobil ügyfél | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, egy eszköz-kezelési házirenddel van, amely csak a PIN-kód használatát igényli, és lehetővé teszi a távoli törlése |

## <a id="bitlocker"></a>Győződjön meg arról egy eszköz-kezelési házirenddel, amely csak a PIN-kód/jelszó vagy automatikus zárolás igényel, és titkosítja az összes adatot (pl. Bitlocker)

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM Outlook ügyfélben | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról egy eszköz-kezelési házirenddel, amely csak a PIN-kód/jelszó vagy automatikus zárolás igényel, és titkosítja az összes adatot (pl. Bitlocker) |

## <a id="rolled-server"></a>Győződjön meg arról, hogy aláíró kulcsok vannak tanúsítványváltást Identitáskiszolgálók használata esetén

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Identity Serverben | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Identitáskezelési kiszolgáló - kulcsokat, aláírás és titkosítás](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Lépések** | Győződjön meg arról, hogy aláíró kulcsok vannak tanúsítványváltást Identitáskiszolgálók használatakor. hello hivatkozás hello references szakaszában ismerteti, hogyan ez kell megtervezni anélkül, hogy ez a kimaradások tooapplications függő identitás-kiszolgálón. |

## <a id="client-server"></a>Győződjön meg arról, hogy erős titkosítással ügyfél-azonosító, ügyfélkulcs használja az Identity Serverben

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Identity Serverben | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Győződjön meg arról, hogy erős titkosítással ügyfél-azonosító, titkos ügyfélkulcs az Identity Serverben használt. egy ügyfél-azonosító és a titkos kulcs létrehozása közben a következő irányelveket hello kell használni:</p><ul><li>Hozzon létre egy véletlenszerű GUID Azonosítót hello ügyfél-azonosító szerint</li><li>Hozzon létre egy kriptográfiai véletlenszerű 256 bites kulcsot hello titkos kulcsként</li></ul>|
