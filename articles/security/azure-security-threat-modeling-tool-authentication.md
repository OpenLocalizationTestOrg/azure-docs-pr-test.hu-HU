---
title: "-Microsoft fenyegetések modellezése eszköz - aaaAuthentication Azure |} Microsoft Docs"
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
ms.openlocfilehash: 06c1b1aebab25e6fb5b666d24ecd9d86085d656c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-authentication--mitigations"></a>Biztonsági keret: Hitelesítési |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Webalkalmazás**    | <ul><li>[Érdemes lehet egy szabványon alapuló hitelesítési mechanizmus tooauthenticate tooWeb alkalmazás](#standard-authn-web-app)</li><li>[Alkalmazások biztonságos helyen kell kezelni sikertelen hitelesítési forgatókönyvek](#handle-failed-authn)</li><li>[Enable lépés mentése vagy adaptív hitelesítés](#step-up-adaptive-authn)</li><li>[Győződjön meg arról, hogy a felügyeleti felületek megfelelően zárolva miatt](#admin-interface-lockdown)</li><li>[Alkalmazzon biztonságosan elfelejtett jelszó funkciók](#forgot-pword-fxn)</li><li>[Győződjön meg arról, hogy valósíthatók meg jelszó- és házirend](#pword-account-policy)</li><li>[Alkalmazzon vezérlők tooprevent felhasználónév felsorolása](#controls-username-enum)</li></ul> |
| **Adatbázis** | <ul><li>[Ha lehetséges, a csatlakozás tooSQL kiszolgáló Windows-hitelesítés használatára](#win-authn-sql)</li><li>[Ha ez lehetséges Azure Active Directory-hitelesítés használata csatlakozás tooSQL adatbázis](#aad-authn-sql)</li><li>[SQL-hitelesítési mód használata esetén győződjön meg arról, hogy fiókkal és jelszóval házirend SQL-kiszolgálón érvényesítik](#authn-account-pword)</li><li>[SQL-hitelesítés nem tartalmazott adatbázisok használata](#autn-contained-db)</li></ul> |
| **Az Azure Event Hubs** | <ul><li>[Eszköz hitelesítő adatok használatával SaS-tokenje hozott](#authn-sas-tokens)</li></ul> |
| **Az Azure megbízhatósági kapcsolat határán** | <ul><li>[Az Azure-rendszergazdák számára az Azure többtényezős hitelesítés engedélyezése](#multi-factor-azure-admin)</li></ul> |
| **Service Fabric megbízhatósági kapcsolat határán** | <ul><li>[Névtelen hozzáférés tooService Fabric-fürt korlátozása](#anon-access-cluster)</li><li>[Győződjön meg arról, hogy a Service Fabric-csomópont ügyfél tanúsítvány nem azonos a csomópontok tanúsítvány](#fabric-cn-nn)</li><li>[Az AAD tooauthenticate ügyfelek tooservice fabric-fürtök használata](#aad-client-fabric)</li><li>[Győződjön meg arról, hogy a service fabric tanúsítványokat kapnak egy jóváhagyott tanúsítvány hitelesítésszolgáltatói (CA)](#fabric-cert-ca)</li></ul> |
| **Identity Serverben** | <ul><li>[Identitáskezelési kiszolgáló által támogatott szabványon alapuló hitelesítési forgatókönyvek használata](#standard-authn-id)</li><li>[Hello alapértelmezett Identitáskiszolgálók jogkivonat gyorsítótára méretezhető alternatív felülbírálása](#override-token)</li></ul> |
| **Gép megbízhatósági kapcsolat határán** | <ul><li>[Győződjön meg arról, hogy a telepített alkalmazás bináris fájljainak digitális aláírással](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[A WCF tooMSMQ várólisták kapcsolódáskor hitelesítés engedélyezése](#msmq-queues)</li><li>[WCF-ne nincs beállítva üzenet clientCredentialType toonone](#message-none)</li><li>[WCF-ne nincs beállítva átviteli clientCredentialType toonone](#transport-none)</li></ul> |
| **Webes API** | <ul><li>[Győződjön meg arról, hogy szabványon alapuló hitelesítési módszerek használt toosecure webes API-k](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Használja az Azure Active Directory által támogatott szabványos hitelesítési forgatókönyvei](#authn-aad)</li><li>[Hello alapértelmezett ADAL jogkivonat gyorsítótára méretezhető alternatív felülbírálása](#adal-scalable)</li><li>[Győződjön meg arról, hogy TokenReplayCache jogkivonatok ADAL-hitelesítéshez használt tooprevent hello ismétlés](#tokenreplaycache-adal)</li><li>[Használja az ADAL könyvtárakat toomanage token OAuth2 ügyfelek tooAAD kér (vagy a helyszíni AD)](#adal-oauth2)</li></ul> |
| **Az IoT-mező átjáró** | <ul><li>[A mező átjáró toohello csatlakozó eszközök hitelesítéséhez](#authn-devices-field)</li></ul> |
| **Az IoT átjáró** | <ul><li>[Győződjön meg arról, hogy tooCloud átjáró csatlakozó eszközök hitelesítése](#authn-devices-cloud)</li><li>[Eszköz hitelesítő adatok használata](#authn-cred)</li></ul> |
| **Azure Storage** | <ul><li>[Győződjön meg arról, hogy csak hello igényel tárolók és blobok névtelen olvasási hozzáférést kap](#req-containers-anon)</li><li>[Adja meg az Azure storage-ban SAS vagy SAP korlátozott hozzáférésű tooobjects](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Érdemes lehet egy szabványon alapuló hitelesítési mechanizmus tooauthenticate tooWeb alkalmazás

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Részletek | <p>A hitelesítés az hello folyamat, ha egy entitás igazolja magát, általában a hitelesítő adatokat, például egy felhasználónevet és jelszót. Nincsenek több hitelesítési protokollok érhető el lehet tekinteni. Ezek közül az alábbiak:</p><ul><li>Ügyféltanúsítványok</li><li>Windows-alapú</li><li>Űrlap alapú</li><li>Összevonási - AD FS</li><li>Összevonási – az Azure AD</li><li>Összevonási - Identitáskiszolgálók</li></ul><p>Érdemes lehet egy normál hitelesítési mechanizmus tooidentify hello forrás folyamat</p>|

## <a id="handle-failed-authn"></a>Alkalmazások biztonságos helyen kell kezelni sikertelen hitelesítési forgatókönyvek

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Részletek | <p>Alkalmazások, amelyek kifejezetten a felhasználók hitelesítésére kezelni kell a sikertelen hitelesítési forgatókönyvek securely.hello hitelesítési mechanizmust kell:</p><ul><li>Hozzáférés megtagadása tooprivileged erőforrások során a hitelesítés sikertelen</li><li>Egy általános hibaüzenetet jeleníti meg a sikertelen hitelesítés után, és hozzáférés-Megtagadás</li></ul><p>Teszt:</p><ul><li>Sikertelen bejelentkezés után a védett erőforrások védelme</li><li>Általános hibaüzenet jelenik meg a sikertelen hitelesítési és hozzáférés-megtagadási esemény</li><li>Túl sok sikertelen kísérlet után letiltott fiókok</li><ul>|

## <a id="step-up-adaptive-authn"></a>Enable lépés mentése vagy adaptív hitelesítés

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Részletek | <p>Ellenőrizze a hello alkalmazásnak további engedélyezési (például egy fokozzák vagy adaptív hitelesítési, többtényezős hitelesítést, például küldés OTP SMS, e-mail stb. vagy újbóli hitelesítést kér keresztül), hello felhasználói próbálnak futtatni rajta hozzáférést megkapják előtt toosensitive adatokat. Ez a szabály is vonatkozik, hogy a kritikus módosítások tooan fiók vagy a művelet</p><p>Is ez azt jelenti, hogy hitelesítési hello kiigazítása van megvalósítva az ilyen toobe, olyan módon, hogy hello megfelelően érvényesít környezetérzékeny engedélyezési így toonot engedélyezése révén illetéktelen módosítását. példában paraméter illetéktelen módosítását.</p>|

## <a id="admin-interface-lockdown"></a>Győződjön meg arról, hogy a felügyeleti felületek megfelelően zárolva miatt

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Részletek | hello első megoldás toogrant hozzáférést csak egy bizonyos forrás IP-címtartomány toohello felügyeleti kapcsolat. Ez nem volna lehetséges, mint a mindig ajánlott tooenforce többlépéses vagy adaptív hitelesítési bejelentkezés hello rendszergazdai felület |

## <a id="forgot-pword-fxn"></a>Alkalmazzon biztonságosan elfelejtett jelszó funkciók

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Részletek | <p>hello először thing, amely elfelejtette a jelszót és a más helyreállítási elérési út egy hivatkozást, beleértve magát időben korlátozott aktiválási token helyett hello jelszó küldése tooverify. További hitelesítési soft-jogkivonatok (pl. SMS token, natív mobilalkalmazások stb.) alapján szükséges lehet, valamint hello hivatkozás továbbítása előtt. Második, meg kell nem hello felhasználói fiók zárolása miközben folyamatban van egy új jelszót hello folyamatának.</p><p>Tooa szolgáltatásmegtagadási támadások vezethet, ha egy támadó határoz hello felhasználók automatikus támadást toointentionally zárolás. Harmadik amikor hello új jelszó kérelem folyamatban van beállítva, üdvözlőüzenetére jelenítjük meg kell általánosítva rendelés tooprevent felhasználónév felsorolásban. Negyedik mindig engedélyezi a régi jelszó hello használatának és valósítja meg az erős jelszó.</p> |

## <a id="pword-account-policy"></a>Győződjön meg arról, hogy valósíthatók meg jelszó- és házirend

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| Részletek | <p>Szervezeti házirend és gyakorlatok betartása jelszó- és házirend kell végrehajtani.</p><p>toodefend elleni találgatásos és szótár alapú találgatás: erős jelszó megvalósított tooensure, hogy a felhasználók létrehozhatnak erős jelszót hozzon létre (pl. 12 karakterek minimális hosszát, alfanumerikus vagy speciális karaktereket) kell lennie.</p><p>A következő módon hello fiókzárolási házirendek végre:</p><ul><li>**Letölthető zárolási kibővített:** Ez lehet, hogy a felhasználók találgatásos támadásokkal szembeni védelme jó választás. Például amikor hello felhasználó ad háromszor hello alkalmazás helytelen jelszót tudta zárolni hello fiók egy percet a rendelés tooslow le a jelszavát, így kevésbé hatékony hello támadó tooproceed kényszerített találgatásos hello folyamatán. Ha tooimplement merevlemez zárolása kibővített ellenintézkedések – akkor érhetők el ebben a példában a "Dos" véglegesen a fiókok zárolásának által. Másik lehetőségként alkalmazás egy egyszeri Jelszavas (egy alkalommal jelszót) készítése és elküldi a sávon kívüli (mailben, sms stb.) toohello felhasználó. Egy másik módszerrel lehet tooimplement CAPTCHA egy sikertelen bejelentkezési kísérletek számát küszöbérték elérése után.</li><li>**Merevlemez zárolása kibővített:** fiókzárolási az ilyen típusú kell alkalmazni, amikor a felhasználó az alkalmazás támadása észlelése és véglegesen el a fiók zárolását, amíg nem volt idő toodo a törvényszéki választ csoport segítségével teljesítményszámláló számára. Ezen folyamat után döntse el, toogive hello felhasználói vissza a fiók, vagy a vele szemben további jogi műveletek. Az ilyen típusú megközelítés megakadályozza, hogy a hello támadó további túljutó az alkalmazás- és infrastruktúra.</li></ul><p>toodefend támadások alapértelmezett és előre jelezhető, ellenőrizze, hogy kulcsok és a jelszavak cserélhető, amelyek és vannak jön létre vagy cserélni a telepítéshez szükséges idő után.</p><p>Ha hello alkalmazás tooauto-jelszavak előállításához, győződjön meg arról, hogy létrehozott hello jelszavak véletlenszerű és magas entrópiát.</p>|

## <a id="controls-username-enum"></a>Alkalmazzon vezérlők tooprevent felhasználónév felsorolása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Az összes hibaüzenet kell általánosítva rendelés tooprevent felhasználónév felsorolásban. Emellett egyes esetekben nem kerülheti el megakadályozására funkciók, például a regisztrációs oldalon található információt. Itt toouse sebességhatárolt módszerek, például a CAPTCHA tooprevent automatikus támadást egy támadó van szüksége. |

## <a id="win-authn-sql"></a>Ha lehetséges, a csatlakozás tooSQL kiszolgáló Windows-hitelesítés használatára

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | A helyi üzemeltetésű |
| **Attribútumok**              | SQL-verzió - minden |
| **Hivatkozások**              | [SQL Server - hitelesítési módjának kiválasztása](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Lépések** | Windows-hitelesítés Kerberos biztonsági protokollt használ, a jelszó házirend betartatása legutóbb toocomplexity érvényesítéssel erős jelszavak támogatást nyújt a fiók zárolása, és támogatja a jelszó lejárati idejét.|

## <a id="aad-authn-sql"></a>Ha ez lehetséges Azure Active Directory-hitelesítés használata csatlakozás tooSQL adatbázis

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | SQL Azure |
| **Attribútumok**              | SQL-verzió - 12-es verzió |
| **Hivatkozások**              | [Csatlakozás tooSQL adatbázis által használó Azure Active Directory-hitelesítés](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Lépések** | **Minimális verzió:** Azure SQL Database 12-es szükséges tooallow Azure SQL Database toouse AAD-hitelesítés hello Microsoft Directory ellen |

## <a id="authn-account-pword"></a>SQL-hitelesítési mód használata esetén győződjön meg arról, hogy fiókkal és jelszóval házirend SQL-kiszolgálón érvényesítik

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [SQL Server jelszóházirend](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Lépések** | SQL Server-hitelesítést használ, amikor az SQL Server, a Windows felhasználói fiókok nem épülő bejelentkezések jönnek létre. SQL Server használatával létrehozott és az SQL Server tárolt hello felhasználónév és a hello jelszó. SQL Server Windows jelszó házirend mechanizmusok használhatja. Azonos összetettségét, lejárati és házirendeket használja az SQL-kiszolgálón belül használt Windows toopasswords hello is érvényesek lesznek. |

## <a id="autn-contained-db"></a>SQL-hitelesítés nem tartalmazott adatbázisok használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | A helyi üzemeltetésű SQL Azure |
| **Attribútumok**              | SQL - MSSQL2012, SQL verzió - verzió 12-es verzió |
| **Hivatkozások**              | [Ajánlott biztonsági eljárások tartalmazott adatbázisok](http://msdn.microsoft.com/library/ff929055.aspx) |
| **Lépések** | az érvényben levő jelszóházirendnek hello hiányában megnőhet hello annak valószínűségét, hogy gyenge hitelesítő adatokat egy tartalmazott adatbázisban jött létre. Éljen az olyan Windows-hitelesítést. |

## <a id="authn-sas-tokens"></a>Eszköz hitelesítő adatok használatával SaS-tokenje hozott

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure Event Hubs | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Event Hubs hitelesítés és a biztonsági modell – áttekintés](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Lépések** | <p>hello Event Hubs biztonsági modellje a közös hozzáférésű Jogosultságkód (SAS) tokenek és esemény-közzétevők alapul. hello közzétevő neve, amely megkapja a hello token DeviceID hello jelöli. Ez segít az hello megfelelő eszközökkel létrehozott hello jogkivonatok társítani.</p><p>Az összes üzenet a kezdeményező szolgáltatás oldalán kísérletek-címek hamisítását az adattartalom származási észlelési így címkével rendelkeznek. Eszközök hitelesítésekor létrehozni egy eszköz SaS-token hatókörön belüli tooa egyedi publisher száma.</p>|

## <a id="multi-factor-azure-admin"></a>Az Azure-rendszergazdák számára az Azure többtényezős hitelesítés engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az Azure megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Mi az az Azure Multi-Factor Authentication?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Lépések** | <p>Többtényezős hitelesítés (MFA), amely egynél több ellenőrzési módszert igényel, és a kritikus fontosságú második réteget biztonsági toouser bejelentkezéseket és tranzakciókat ad hitelesítési mód. Azzal, hogy bármely két vagy több ellenőrzési módszer a következő hello működik:</p><ul><li>Valami tudja (általában jelszót)</li><li>Valami (megbízható eszközzel rendelkezik, amely nem egyszerű ismétlődik, például a telefon)</li><li>Valami áll (biometria)</li><ul>|

## <a id="anon-access-cluster"></a>Névtelen hozzáférés tooService Fabric-fürt korlátozása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Service Fabric megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Környezet – Azure  |
| **Hivatkozások**              | [A Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Lépések** | <p>Fürtök mindig biztonságos tooprevent nem engedélyezett felhasználók tooyour fürthöz csatlakozzon, különösen akkor, ha a termelési számítási feladatokhoz fut rajta van kell lennie.</p><p>A service fabric-fürt létrehozásakor, győződjön meg arról, hogy hello biztonsági mód beállítása túl "biztonságos", és konfigurálja a szükséges hello X.509 tanúsítvány. Egy "biztonságos" fürt létrehozását lehetővé teszi a névtelen felhasználói tooconnect tooit Ha felügyeleti végpontok toohello teszi elérhetővé nyilvános internethez.</p>|

## <a id="fabric-cn-nn"></a>Győződjön meg arról, hogy a Service Fabric-csomópont ügyfél tanúsítvány nem azonos a csomópontok tanúsítvány

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Service Fabric megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Önálló – Azure, környezet - környezet |
| **Hivatkozások**              | [A Service Fabric-csomópont ügyfél tanúsítvány biztonsági](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [Connect tooa biztonságos fürt ügyféltanúsítvány használatával](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Lépések** | <p>Ügyfél-csomópont tanúsítvány biztonsági létrehozásakor hello fürt hello Azure-portálon keresztül, Resource Manager-sablonok vagy egy önálló JSON-sablon egy rendszergazdai ügyféltanúsítvány és/vagy felhasználói ügyféltanúsítványt megadásával van konfigurálva.</p><p>hello felügyeleti ügyfél és a felhasználói ügyféltanúsítványok adja meg, hogy nem lehet ugyanaz, mint hello elsődleges és másodlagos tanúsítványokat állít be csomópontok biztonsági.</p>|

## <a id="aad-client-fabric"></a>Az AAD tooauthenticate ügyfelek tooservice fabric-fürtök használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Service Fabric megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Környezet – Azure |
| **Hivatkozások**              | [Fürtök biztonsági forgatókönyveinek - biztonsági javaslatok](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Lépések** | Azure-on futó fürtök is biztosíthassa hozzáférés toohello felügyeleti végpontok Azure Active Directory (AAD), leszámítva az ügyféltanúsítványok használata. Az Azure-fürtök esetén ajánlott használni, amikor az AAD biztonsági tooauthenticate ügyfelek és tanúsítványokat-csomópontok biztonsági.|

## <a id="fabric-cert-ca"></a>Győződjön meg arról, hogy a service fabric tanúsítványokat kapnak egy jóváhagyott tanúsítvány hitelesítésszolgáltatói (CA)

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Service Fabric megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Környezet – Azure |
| **Hivatkozások**              | [X.509-tanúsítványokat és a Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Lépések** | <p>Service Fabric X.509 kiszolgálói tanúsítványokat használ a csomópontok és az ügyfelek hitelesítéséhez.</p><p>Néhány fontos dolgot tooconsider szolgáltatás hálók a tanúsítványok használata közben:</p><ul><li>A termelési számítási feladatokhoz rendszert futtató fürtöket használt tanúsítványok legyen a megfelelően konfigurált Windows kiszolgálói tanúsítvány szolgáltatások segítségével létrehozott vagy egy jóváhagyott tanúsítvány hitelesítésszolgáltatói (CA) kapott. hello hitelesítésszolgáltató egy jóváhagyott külső hitelesítésszolgáltató vagy egy megfelelő felügyelt belső nyilvánoskulcs-infrastruktúrát (PKI) lehet.</li><li>Soha ne használja az összes ideiglenes vagy tanúsítványok tesztelése éles üzemben eszközöket, például a MakeCert.exe használatával létrehozott</li><li>Használhat önaláírt tanúsítványt, de csak akkor tegye ezt tesztfürtökön és nem éles környezetben</li></ul>|

## <a id="standard-authn-id"></a>Identitáskezelési kiszolgáló által támogatott szabványon alapuló hitelesítési forgatókönyvek használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Identity Serverben | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [IdentityServer3 - hello nagy vonalakban tekinti](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Lépések** | <p>Az alábbiakban hello identitás-kiszolgáló által támogatott szokásos kapcsolati:</p><ul><li>Webalkalmazások kommunikálni böngészők</li><li>Webalkalmazások kommunikálni a webes API-k (néha a saját, néha egy felhasználó nevében)</li><li>Webböngésző-alapú alkalmazások kommunikálnak a webes API-k</li><li>Webes API-k kommunikálni natív alkalmazások</li><li>Kiszolgáló-alapú alkalmazások kommunikálni a webes API-k</li><li>Webes API-k kommunikálni a webes API-k (néha a saját, néha egy felhasználó nevében)</li></ul>|

## <a id="override-token"></a>Hello alapértelmezett Identitáskiszolgálók jogkivonat gyorsítótára méretezhető alternatív felülbírálása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Identity Serverben | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Identitáskezelési kiszolgáló telepítési - gyorsítótárazás](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Lépések** | <p>IdentityServer rendelkezik egy egyszerű beépített memórián belüli gyorsítótárral. Ez nem jó kis léptékű natív alkalmazásokhoz, akkor nem méretezhető a következő okok miatt hello középső réteg és a háttérkiszolgáló alkalmazások:</p><ul><li>Ezek az alkalmazások által elért sok felhasználó egyszerre. Minden hozzáférési jogkivonatok menteni a hello ugyanarra a tároló elkülönítési problémák hoz létre, és megadja a kihívásokkal léptékű működő: sok felhasználó minden annyi jogkivonatokkal, hello erőforrások hello app fér hozzá, hogy a nevükben is: hatalmas számok és nagyon költséges keresési műveletek</li><li>Ezek az alkalmazások általában telepített Elosztott topológia, ahol több csomópont rendelkeznie kell hozzáféréssel toohello azonos gyorsítótár</li><li>Gyorsítótárazott jogkivonatok folyamat újrahasznosítja azt és deactivations tovább kell működnie</li><li>A fenti okokból implementing web apps, miközben minden hello toooverride hello alapértelmezett identitás kiszolgáló jogkivonat gyorsítótára egy méretezhető alternatív, például az Azure Redis cache-javasolt</li></ul>|

## <a id="binaries-signed"></a>Győződjön meg arról, hogy a telepített alkalmazás bináris fájljainak digitális aláírással

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Gép megbízhatósági kapcsolat határán | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy üzembe helyezett alkalmazás bináris fájljainak digitális aláírással, hogy hello bináris fájlok integritásának hello ellenőrizhető|

## <a id="msmq-queues"></a>A WCF tooMSMQ várólisták kapcsolódáskor hitelesítés engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, NET-keretrendszer 3 |
| **Attribútumok**              | N/A |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Lépések** | Program tooenable hitelesítés nem sikerül, tooMSMQ várólisták kapcsolódáskor, akkor a támadó névtelenül is elküldhetik üzenetek toohello várólistára feldolgozásra. Hitelesítés nem használható tooconnect tooan MSMQ sora toodeliver üzenet tooanother programot, ha egy támadó rosszindulatú névtelen üzenetben sikerült elküldeni.|

### <a name="example"></a>Példa
Hello `<netMsmqBinding/>` hello WCF konfigurációs fájl az alábbi elem utasítja WCF toodisable hitelesítési tooan MSMQ-várólista üzenetkézbesítést kapcsolódáskor.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""None"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```
Mindig minden bejövő vagy kimenő üzenet az MSMQ toorequire Windows-tartomány vagy Tanúsítványalapú hitelesítés konfigurálása

### <a name="example"></a>Példa
Hello `<netMsmqBinding/>` hello WCF konfigurációs fájl az alábbi elem utasítja WCF tooenable tanúsítványhitelesítés tooan MSMQ-várólista kapcsolódáskor. hello ügyfél hitelesítése X.509-tanúsítványokat használ. hello ügyféltanúsítvány hello kiszolgáló tanúsítványtárolójában hello jelen kell lennie.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""Certificate"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```

## <a id="message-none"></a>WCF-ne nincs beállítva üzenet clientCredentialType toonone

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer 3 |
| **Attribútumok**              | Ügyfél-hitelesítő adat típusa: nincs |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | hitelesítési hello hiányában azt jelenti, hogy mindenki van képes tooaccess ezt a szolgáltatást. Egy szolgáltatás, amely nem hitelesíti az ügyfeleket lehetővé teszi a hozzáférést tooall felhasználóknak. Konfigurálja a hello alkalmazás tooauthenticate elleni ügyfél hitelesítő adatait. Ezt megteheti úgy, hogy hello üzenet clientCredentialType tooWindows vagy a tanúsítványt. |

### <a name="example"></a>Példa
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>WCF-ne nincs beállítva átviteli clientCredentialType toonone

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, .NET-keretrendszer 3 |
| **Attribútumok**              | Ügyfél-hitelesítő adat típusa: nincs |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | hitelesítési hello hiányában azt jelenti, hogy mindenki van képes tooaccess ezt a szolgáltatást. Egy szolgáltatás, amely nem hitelesíti az ügyfeleket lehetővé teszi, hogy minden felhasználó tooaccess a működését. Konfigurálja a hello alkalmazás tooauthenticate elleni ügyfél hitelesítő adatait. Ezt megteheti úgy, hogy hello átviteli clientCredentialType tooWindows vagy a tanúsítványt. |

### <a name="example"></a>Példa
```
<transport clientCredentialType=""Certificate""/>
```

## <a id="authn-secure-api"></a>Győződjön meg arról, hogy szabványon alapuló hitelesítési módszerek használt toosecure webes API-k

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Hitelesítési és engedélyezési ASP.NET webes API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [külső hitelesítési szolgáltatások a ASP.NET Web API (C#)](http://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Lépések** | <p>A hitelesítés az hello folyamat, ha egy entitás igazolja magát, általában a hitelesítő adatokat, például egy felhasználónevet és jelszót. Nincsenek több hitelesítési protokollok érhető el lehet tekinteni. Ezek közül az alábbiak:</p><ul><li>Ügyféltanúsítványok</li><li>Windows-alapú</li><li>Űrlap alapú</li><li>Összevonási - AD FS</li><li>Összevonási – az Azure AD</li><li>Összevonási - Identitáskiszolgálók</li></ul><p>Hivatkozások hello references szakaszában adja meg, hogyan lehet egyes hello hitelesítési sémák a kevésbé fontos részletek megvalósított toosecure egy webes API-t.</p>|

## <a id="authn-aad"></a>Használja az Azure Active Directory által támogatott szabványos hitelesítési forgatókönyvei

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure AD | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Hitelesítési forgatókönyvek az Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory-Kódminták](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [Azure Active Directory fejlesztői útmutatója](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Lépések** | <p>Azure Active Directory (Azure AD) egyszerűbbé teszi a fejlesztők hitelesítési azáltal identitás szolgáltatásként, például az OAuth 2.0 és az OpenID Connect szabványos protokollt támogat. Az alábbiakban hello Azure AD által támogatott öt elsődleges alkalmazás-forgatókönyvekre:</p><ul><li>Webalkalmazás-böngésző tooWeb alkalmazás: A felhasználó számára szükséges toosign tooa webes alkalmazás, amelyet az Azure AD</li><li>Egyetlen lap alkalmazás (SPA): A felhasználó számára szükséges toosign alkalmazásban tooa egyetlen lap, amelyet az Azure AD</li><li>Natív alkalmazás tooWeb API: egy natív alkalmazás, amely futtatja a telefon, táblagép vagy PC igények tooauthenticate egy felhasználó tooget erőforrások webes API-k, amelyet az Azure AD</li><li>Webes alkalmazás tooWeb API-t: webalkalmazás kell egy webes API-t az Azure AD által védett tooget erőforrásokat</li><li>Démon vagy kiszolgálói alkalmazás tooWeb API: egy démon alkalmazást vagy egy kiszolgálói alkalmazás webes felhasználói felület nélkül kell tooget erőforrások a webes API-k az Azure AD által védett</li></ul><p>Tekintse meg a toohello hivatkozások hello references szakaszában az alacsony szintű megvalósítás részletei</p>|

## <a id="adal-scalable"></a>Hello alapértelmezett ADAL jogkivonat gyorsítótára méretezhető alternatív felülbírálása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure AD | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Modern hitelesítést és az Azure Active Directory-webalkalmazások számára történő](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [ADAL jogkivonatok gyorsítótárát, Redis használatával](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Lépések** | <p>hello alapértelmezett gyorsítótárhoz adal-t (Active Directory Authentication Library) használ a memóriabeli gyorsítótár, amely statikus üzlet, elérhető folyamat kiterjedő támaszkodik. Ez a natív alkalmazások esetén használható, amíg azt nem méretezhető a következő okok miatt hello középső réteg és a háttérkiszolgáló alkalmazások:</p><ul><li>Ezek az alkalmazások által elért sok felhasználó egyszerre. Minden hozzáférési jogkivonatok menteni a hello ugyanarra a tároló elkülönítési problémák hoz létre, és megadja a kihívásokkal léptékű működő: sok felhasználó minden annyi jogkivonatokkal, hello erőforrások hello app fér hozzá, hogy a nevükben is: hatalmas számok és nagyon költséges keresési műveletek</li><li>Ezek az alkalmazások általában telepített Elosztott topológia, ahol több csomópont rendelkeznie kell hozzáféréssel toohello azonos gyorsítótár</li><li>Gyorsítótárazott jogkivonatok folyamat újrahasznosítja azt és deactivations tovább kell működnie</li></ul><p>A fenti okokból implementing web apps, miközben minden hello toooverride hello alapértelmezett ADAL jogkivonatok gyorsítótárát egy méretezhető alternatív, például az Azure Redis Cache-gyorsítótár ajánlott.</p>|

## <a id="tokenreplaycache-adal"></a>Győződjön meg arról, hogy TokenReplayCache jogkivonatok ADAL-hitelesítéshez használt tooprevent hello ismétlés

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure AD | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Modern hitelesítést és az Azure Active Directory-webalkalmazások számára történő](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Lépések** | <p>hello TokenReplayCache tulajdonság lehetővé teszi a fejlesztők toodefine a hitelesítési karakterláncok ismétlésének gyorsítótár, a tárolóban, használhat a jogkivonatok hello céllal történik, ellenőrizze, hogy nincs token mentése egynél többször is használható.</p><p>Ez a mérték a közös támadások elleni hello szolgáltatási nevű hitelesítési karakterláncok ismétlésének támadás: egy támadó elfogja bejelentkezéskor küldi hello token megpróbálja toosend azt újra toohello app ("" játssza le újra) egy új munkamenet létrehozásához. Például a OIDC kód-grant flow, egy kérelem a felhasználó sikeres hitelesítés után túl "/ signin-oidc" végpont hello függő fél történik, a "id_token", "code" és "állapot" paraméterek.</p><p>függő entitás hello ellenőrzi ezt a kérelmet, és új munkamenetet létesít. Ha egy ellenfél rögzíti ezt a kérelmet, és azt replays, többé a sikeres munkamenet és megszemélyesítés hello felhasználók hozhatnak létre. az OpenID Connect hello nonce hello jelenléte korlátozza, de nem teljes mértékben kiküszöbölhetők hello körülmények között, amelyben hello támadás is kell sikeresen léptetni. tooprotect az alkalmazások, a fejlesztők is ITokenReplayCache megvalósítást, és rendelje hozzá egy példány tooTokenReplayCache.</p>|

### <a name="example"></a>Példa
```C#
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Példa
Íme egy minta hello ITokenReplayCache illesztőfelület megvalósítása. (Adja testreszabása és a projektre vonatkozó gyorsítótárazási keretrendszer megvalósítása)
```C#
public class TokenReplayCache : ITokenReplayCache
{
    private readonly ICacheProvider cache; // Your project-specific cache provider
    public TokenReplayCache(ICacheProvider cache)
    {
        this.cache = cache;
    }
    public bool TryAdd(string securityToken, DateTime expiresOn)
    {
        if (this.cache.Get<string>(securityToken) == null)
        {
            this.cache.Set(securityToken, securityToken);
            return true;
        }
        return false;
    }
    public bool TryFind(string securityToken)
    {
        return this.cache.Get<string>(securityToken) != null;
    }
}
```
megvalósított hello gyorsítótár OIDC beállítások hello "TokenValidationParameters" tulajdonság használatával az alábbiak szerint hivatkozott toobe tartalmaz.
```C#
OpenIdConnectOptions openIdConnectOptions = new OpenIdConnectOptions
{
    AutomaticAuthenticate = true,
    ... // other configuration properties follow..
    TokenValidationParameters = new TokenValidationParameters
    {
        TokenReplayCache = new TokenReplayCache(/*Inject your cache provider*/);
    }
}
```

Vegye figyelembe, hogy ez a konfiguráció a helyi OIDC által védett alkalmazásba bejelentkezési tootest hello hatékonyságát, és hello kérelem túl rögzítése`"/signin-oidc"` a fiddler végpont. Ha hello védelmi nem van beállítva, a kérelem a fiddler visszajátszását állítja be egy új munkamenetcookie-t. Hello kérelmet a rendszer játssza hello TokenReplayCache védelem hozzáadása után, amikor hello alkalmazás fogja kivételt jelez az alábbiak szerint:`SecurityTokenReplayDetectedException: IDX10228: hello securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>Használja az ADAL könyvtárakat toomanage token OAuth2 ügyfelek tooAAD kér (vagy a helyszíni AD)

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure AD | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Lépések** | <p>hello Azure AD authentication Library (ADAL) lehetővé teszi, hogy ügyfél alkalmazás fejlesztők tooeasily felhasználók toocloud hitelesítéséhez vagy a helyszíni Active Directory (AD), és majd a védelmét biztosító API-hívások hozzáférési tokenek beszerzése érdekében.</p><p>ADAL számos lehetőséggel rendelkezik, hogy ellenőrizze a hitelesítési megkönnyíti a fejlesztők számára, például aszinkron támogatását, a konfigurálható jogkivonatok gyorsítótárát, amely tárolja a hozzáférési jogkivonatok és a frissítési jogkivonatokat, automatikus token frissítés, ha az előző lejár, és egy frissítési jogkivonat érhető el, és További.</p><p>Legtöbb hello összetettségi kezelnek, ADAL fejlesztői szűkítheti az üzleti logika tudnak biztosítani az alkalmazásokban, és anélkül, hogy a biztonsági szakértő könnyen erőforrások biztonságossá tételére. Külön szalagtárak .NET, JavaScript (ügyfél és a Node.js), iOS, Android és Java esetében érhetők el.</p>|

## <a id="authn-devices-field"></a>A mező átjáró toohello csatlakozó eszközök hitelesítéséhez

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT-mező átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | Győződjön meg arról, hogy minden eszköz által hitelesített hello mező átjáró őket adatokat fogad és hello átjáró a felsőbb réteggel való kommunikáció biztosítása előtt. Emellett győződjön meg arról, hogy eszközök csatlakoznak-e a egy eszközönként hitelesítőadat-, hogy az egyes eszközökről egyedileg azonosítható.|

## <a id="authn-devices-cloud"></a>Győződjön meg arról, hogy tooCloud átjáró csatlakozó eszközök hitelesítése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános, C#, Node.JS,  |
| **Attribútumok**              | Nincs; átjáró choice - Azure IoT Hub |
| **Hivatkozások**              | Nincs; [.NET Azure IoT hubot](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [egyidejűleg az IoT-központ bevezetés és csomópont JS](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [SAS és tanúsítványok védelmét biztosító IoT](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [Git-tárház](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Lépések** | <ul><li>**Általános:** hitelesítés hello eszköz használata a Transport Layer Security (TLS) vagy az IPSec protokollt. Infrastruktúra támogatnia kell a előmegosztott kulccsal (PSK), amely nem tudja kezelni a teljes aszimmetrikus titkosítási eszközökön. Használja ki az Azure AD Oauth.</li><li>**C#:** alapértelmezés szerint egy DeviceClient példány létrehozásakor hello Create metódussal létrehoz egy IoT-központ hello AMQP protokoll toocommunicate használó DeviceClient példányt. toouse hello HTTPS protokollt, hello felülbírálás hello Create metódus, amely lehetővé teszi toospecify hello protokollt használja. Ha hello HTTPS protokollt használ, hello is fel kell `Microsoft.AspNet.WebApi.Client` NuGet csomag tooyour projekt tooinclude hello `System.Net.Http.Formatting` névtér.</li></ul>|

### <a name="example"></a>Példa
```C#
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Példa
**Node.JS: hitelesítés**
#### <a name="symmetric-key"></a>Szimmetrikus kulcs
* Létrehoz egy IoT-központot az azure-on
* Hello eszközidentitási bejegyzés létrehozása
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* A szimulált eszköz létrehozásához
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
#### <a name="sas-token"></a>SAS-jogkivonat
* Lekérdezi belsőleg generáltak, de szimmetrikus kulcs használata esetén is létrehozhatják és explicit módon is használhatják
* Egy protokoll megadása:`var Http = require('azure-iot-device-http').Http;`
* Hozzon létre egy sas-jogkivonat:
    ```javascript
    resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();
    var deviceName = "<deviceName >";
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    var toSign = resourceUri + '\n' + expires;
    // using crypto
    var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
    const hmac = crypto.createHmac('sha256', decodedPassword);
    hmac.update(toSign);
    var base64signature = hmac.digest('base64');
    var base64UriEncoded = encodeURIComponent(base64signature);
    // construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "%2fdevices%2f"+deviceName+"&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
    ```
* Csatlakozás a sas-jogkivonat használatával: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
#### <a name="certificates"></a>Tanúsítványok
* Létrehozni egy önaláírt aláírt X509 e tanúsítvány eszköz OpenSSL toogenerate például egy .cert és .key fájlok toostore hello tanúsítvány és hello kulcs kulcsattribútumokkal.
* Egy eszköz, amely támogatja a tanúsítványok segítségével biztonságos kapcsolatot kiépítéséhez.
    ```javascript
    var connectionString = '<connectionString>';
    var registry = iothub.Registry.fromConnectionString(connectionString);
    var deviceJSON = {deviceId:"<deviceId>",
    authentication: {
        x509Thumbprint: {
        primaryThumbprint: "<PrimaryThumbprint>",
        secondaryThumbprint: "<SecondaryThumbprint>"
        }
    }}
    var device = deviceJSON;
    registry.create(device, function (err) {});
    ```
* Egy olyan tanúsítványt használ eszköz csatlakoztatása
    ```javascript
    var Protocol = require('azure-iot-device-http').Http;
    var Client = require('azure-iot-device').Client;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>x509=true';
    var client = Client.fromConnectionString(connectionString, Protocol);
    var options = {
        key: fs.readFileSync('./key.pem', 'utf8'),
        cert: fs.readFileSync('./server.crt', 'utf8')
    }; 
    // Calling setOptions with hello x509 certificate and key (and optionally, passphrase) will configure hello client //transport toouse x509 when connecting tooIoT Hub
    client.setOptions(options);
    //call fn tooexecute after hello connection is set up
    client.open(fn);
    ```

## <a id="authn-cred"></a>Eszköz hitelesítő adatok használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró  | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | Átjáró choice - Azure IoT Hub |
| **Hivatkozások**              | [Az Azure IoT-központ biztonsági jogkivonatokat](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Lépések** | Egy eszköz hitelesítő adatok használatával SaS-tokenje használata eszközkulcs vagy ügyféltanúsítvány alapján, helyett az IoT Hub-szintű megosztott elérési házirendeket. Ez megakadályozza, hogy a hitelesítési tokenek az egy eszközre vagy mező átjáró használatának hello egy másik |

## <a id="req-containers-anon"></a>Győződjön meg arról, hogy csak hello igényel tárolók és blobok névtelen olvasási hozzáférést kap

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | StorageType - Blob |
| **Hivatkozások**              | [Kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [megosztott hozzáférési aláírásokkal, 1. rész: Understanding hello SAS-modell](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Lépések** | <p>Alapértelmezés szerint a tároló és szereplő bármely BLOB érhetik csak el hello tárfiók hello tulajdonosa. toogive névtelen felhasználók olvasási engedéllyel a tooa és a benne található blobokat, egy hello tároló engedélyek tooallow nyilvános hozzáférés állíthatja be. Névtelen felhasználók olvashatják a nyilvánosan elérhető tárolóban található blobok hello kérelem hitelesítése nélkül.</p><p>Tárolók adja meg az alábbi beállítások tároló hozzáférés hello:</p><ul><li>Teljes nyilvános olvasási hozzáférés: névtelen kérelem keresztül olvasható tároló és a blob adatait. Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.</li><li>Nyilvános olvasási hozzáférés a blobok csak: Blobadatok található olvasható névtelen kérelem keresztül, de adatai nem érhető el. Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok nem tudja felsorolni.</li><li>Nincs nyilvános olvasási hozzáférés: által hello fiók tulajdonosának csak olvasható tároló és a blob adatait</li></ul><p>Névtelen hozzáférés esetén ajánlott forgatókönyvek, ahol egyes blobok mindig elérhetőknek kell lenniük a névtelen olvasási hozzáférés. Pontosabban megbízhatatlanná vezérlő egyik hozhat létre egy közös hozzáférésű jogosultságkódot, amely lehetővé teszi a különböző jogosultságokkal toodelegate korlátozott hozzáférés és a megadott időtartam alatt. Győződjön meg arról, hogy tárolók és blobok, amelyek vélhetően tartalmazhatnak bizalmas adatokat, nem hozzáférő névtelen véletlenül</p>|

## <a id="limited-access-sas"></a>Adja meg az Azure storage-ban SAS vagy SAP korlátozott hozzáférésű tooobjects

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A |
| **Hivatkozások**              | [A közös hozzáférésű Jogosultságkód, 1. rész: Understanding hello SAS model](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [megosztott hozzáférési aláírásokkal, 2. rész: létrehozása és SAS-kód használatát a Blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [hogyan férhetnek hozzá a toodelegate tooobjects be a fiók használatával Közös hozzáférésű Jogosultságkód és tárolt hozzáférési házirendek](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Lépések** | <p>A közös hozzáférésű jogosultságkód (SAS) használata egy hatékony módját kínálja toogrant korlátozott hozzáférés tooobjects a tárolási fiók tooother ügyfélhez, anélkül, hogy tooexpose tárelérési kulccsal. hello SAS URI, amely magában foglalja a lekérdezési paraméterek minden szükséges hitelesített hello információt tooa tárolási erőforrás elérésére. tárolási erőforrások tooaccess hello SAS, hello ügyfélnek csak kell toopass hello SAS toohello megfelelő konstruktort vagy metódust.</p><p>SAS-kód is használhatja, ha a tárolási fiók tooa ügyfél, amely nem adható meg megbízhatóként a hello fiókkulcs tooprovide hozzáférés tooresources szeretne. A tárfiók kulcsait tartalmazzák mind az elsődleges és másodlagos kulcs, amelyek mindegyikét adjon rendszergazdai hozzáférési tooyour fiók és az összes hello-erőforrásokat. A fiók toohello lehetőségét, amely rosszindulatú vagy gondatlan használatát teszi ki a fiókot kulcsok nyílik meg. Közös hozzáférésű jogosultságkód adjon meg egy biztonságos megoldás, amely lehetővé teszi, hogy más ügyfelekkel tooread, írási és törlési adatok alapján történik, hogy adott toohello engedélyek tárfiókba, és anélkül hello-fiókjához tartozó.</p><p>Ha egy olyan logikai készlete, amelyek hasonló minden alkalommal, amikor paraméterek, a tárolt hozzáférési házirend (SAP) használata segítenek meghatározni. Mert a tárolt házirend származó SAS használatával lehetővé teszi, hogy SAS azonnal, a rendszer hello ajánlott bevált gyakorlat tooalways hello képességét toorevoke használja tárolt hozzáférési házirendeket, ha lehetséges.</p>|
