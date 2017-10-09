---
title: "aaaAuditing és naplózás - Microsoft fenyegetések modellezése eszköz - Azure |} Microsoft Docs"
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
ms.openlocfilehash: f28988833eba251b6ceb8bbd47cde37e871af609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Biztonsági keret: A vizsgálati és naplózási |} Megoldást 
| A termék vagy szolgáltatás | Cikk |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[A megoldásban bizalmas entitások azonosítására és változásának naplózása megvalósítása](#sensitive-entities)</li></ul> |
| **Webalkalmazás** | <ul><li>[Győződjön meg arról, hogy a vizsgálati és naplózási megvalósul hello alkalmazás](#auditing)</li><li>[Ellenőrizze, hogy naplóváltás és elválasztási helyen](#log-rotation)</li><li>[Győződjön meg arról, hogy az alkalmazás hello nem naplózza a bizalmas felhasználói adatok](#log-sensitive-data)</li><li>[Győződjön meg arról, hogy naplózási és naplófájlok rendelkezik-e a korlátozott hozzáférés](#log-restricted-access)</li><li>[Győződjön meg arról, hogy a felhasználó felügyeleti események bejelentkezve](#user-management)</li><li>[Győződjön meg arról, hogy rendelkezik-e hello rendszer beépített védelmet való visszaélés elleni](#inbuilt-defenses)</li><li>[Az Azure App Service web Apps diagnosztikai naplózás engedélyezése](#diagnostics-logging)</li></ul> |
| **Adatbázis** | <ul><li>[Győződjön meg arról, hogy bejelentkezés naplózása engedélyezve van az SQL Server](#identify-sensitive-entities)</li><li>[Az Azure SQL fenyegetésészlelés engedélyezéséhez](#threat-detection)</li></ul> |
| **Azure Storage** | <ul><li>[Azure Storage Analytics tooaudit hozzáférés az Azure Storage használata](#analytics)</li></ul> |
| **WCF** | <ul><li>[Elegendő naplózási megvalósítása](#sufficient-logging)</li><li>[Valósítja meg a megfelelő naplózási hiba kezelése](#audit-failure-handling)</li></ul> |
| **Webes API** | <ul><li>[Győződjön meg arról, hogy a vizsgálati és naplózási megvalósul webes API](#logging-web-api)</li></ul> |
| **Az IoT-mező átjáró** | <ul><li>[Győződjön meg arról, hogy megfelelő vizsgálati és naplózási megvalósul mező átjáró](#logging-field-gateway)</li></ul> |
| **Az IoT átjáró** | <ul><li>[Győződjön meg arról, hogy megfelelő vizsgálati és naplózási megvalósul átjáró](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>A megoldásban bizalmas entitások azonosítására és változásának naplózása megvalósítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Dynamics CRM | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | A bizalmas adatokat tartalmazó megoldásban entitások azonosítása, és ezek entitásokat és mezőket a naplózás módosítása megvalósítása |

## <a id="auditing"></a>Győződjön meg arról, hogy a vizsgálati és naplózási megvalósul hello alkalmazás

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | A vizsgálati és naplózási összes engedélyezése. Naplók kell rögzítheti a felhasználói környezet. Az összes fontos események azonosítása, és az események. A központi naplózás megvalósítása |

## <a id="log-rotation"></a>Ellenőrizze, hogy naplóváltás és elválasztási helyen

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | <p>Naplóváltás egy automatikus folyamat, amelyben dátummal naplófájlok archivált rendszerfelügyelet használatban. Kiszolgálók, amelyek nagy alkalmazások gyakran irányuló minden kérés naplózására: hello arc nagyméretű naplói, naplóváltás egy módon toolimit hello teljes hello naplók mérete, miközben továbbra is lehetővé teszi a legutóbbi események elemzését. </p><p>Napló elválasztási alapvetően azt jelenti, hogy a naplófájlokat, ha az operációs rendszer vagy alkalmazás futó rendelés tooavert a szolgáltatásmegtagadást egy másik partíció már, vagy a teljesítményét, az alkalmazás visszaminősítése hello toostore</p>|

## <a id="log-sensitive-data"></a>Győződjön meg arról, hogy az alkalmazás hello nem naplózza a bizalmas felhasználói adatok

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | <p>Ellenőrizze, hogy nem jelentkezik, hogy a felhasználó elküld tooyour hely bizalmas információt. Ellenőrizze, hogy szándékos naplózását, valamint a tervezési szempontokat által okozott hatásai. Bizalmas adatok közé tartoznak például:</p><ul><li>Felhasználói hitelesítő adatok</li><li>Társadalombiztosítási szám vagy egyéb fontos adatokat</li><li>Hitelkártyaszámok vagy egyéb pénzügyi információ</li><li>Állapottal kapcsolatos adatok</li><li>A titkos kulcsok vagy egyéb adatokhoz lehet használt toodecrypt titkosított adatai</li><li>Rendszer vagy alkalmazás információ használható toomore hatékonyan támadás hello alkalmazás</li></ul>|

## <a id="log-restricted-access"></a>Győződjön meg arról, hogy naplózási és naplófájlok rendelkezik-e a korlátozott hozzáférés

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | <p>Ellenőrizze, hogy tooensure hozzáférési jogok toolog fájlok megfelelően vannak beállítva. Alkalmazás fiókok csak írási hozzáféréssel és operátorok kell, és a technikai támogatási csoporthoz olvasási hozzáféréssel kell rendelkeznie, igény szerint.</p><p>Rendszergazdák számítanak azok a hello csak fiókok, amelyek teljes hozzáféréssel kell rendelkeznie. Ellenőrizze a Windows hozzáférés-vezérlési listája naplózási azok megfelelően korlátozott tooensure fájlok:</p><ul><li>Alkalmazás fiókok csak írási hozzáféréssel kell rendelkeznie.</li><li>Operátorok és a technikai támogatási csoporthoz kell csak olvasási hozzáféréssel, igény szerint</li><li>A rendszergazdák hello csak fiókoknak teljes hozzáféréssel kell rendelkeznie</li></ul>|

## <a id="user-management"></a>Győződjön meg arról, hogy a felhasználó felügyeleti események bejelentkezve

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | <p>Győződjön meg arról, hogy hello alkalmazás figyeli a felhasználó felügyeleti események például a sikeres és sikertelen a felhasználói bejelentkezéseket, a jelszó alaphelyzetbe állítása, jelszó-változtatásának, a fiók zárolása, a felhasználói regisztráció. Ez segít a toodetect, és reagálni toopotentially gyanús viselkedését. Azt is lehetővé teszi, hogy toogather műveletek; például ki fér hozzá hello alkalmazás tootrack</p>|

## <a id="inbuilt-defenses"></a>Győződjön meg arról, hogy rendelkezik-e hello rendszer beépített védelmet való visszaélés elleni

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések**                   | <p>Vezérlők helyen kell lennie, amely throw biztonsági kivétel alkalmazás helytelen használat esetén. Például ha bemenet-ellenőrzéshez van beállítva, és a támadó megpróbál tooinject kártékony kódot, amely nem egyezik meg a hello regex, biztonsági kivételt is jelezni, amely lehet a rendszer visszaélés lehetőségét előzetes</p><p>Ajánlott például toohave biztonsági kivételek naplózza, és az a következő problémák hello végrehajtott műveleteket:</p><ul><li>Bemenet-ellenőrzést</li><li>CSRF megsértése</li><li>Találgatásos (felhasználónként és erőforrás-kérelmek számát a felső határ)</li><li>Fájl feltöltése megsértése</li><ul>|

## <a id="diagnostics-logging"></a>Az Azure App Service web Apps diagnosztikai naplózás engedélyezése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Web Application | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | EnvironmentType – Azure |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Azure biztosít a hibakeresés egy App Service beépített diagnosztika tooassist webalkalmazás. Is tooAPI alkalmazásokra és mobilalkalmazásokra egyaránt érvényes. App Service web apps naplóinformációk hello webkiszolgáló és a webes alkalmazás hello diagnosztikai funkciók biztosítanak.</p><p>Ezek logikailag oszthatók web server diagnostics és az application diagnostics</p>|

## <a id="identify-sensitive-entities"></a>Győződjön meg arról, hogy bejelentkezés naplózása engedélyezve van az SQL Server

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Bejelentkezési naplózásának konfigurálása](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Lépések** | <p>Adatbázis bejelentkezés naplózása engedélyezve toodetect/megerősítése jelszó-előállító támadások kell lennie. Fontos toocapture sikertelen bejelentkezési kísérletek. Törvényszéki vizsgálatokat közben mindkét sikeres és sikertelen bejelentkezési kísérletek rögzítése biztosít további előnye</p>|

## <a id="threat-detection"></a>Az Azure SQL fenyegetésészlelés engedélyezéséhez

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Adatbázis | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | SQL Azure |
| **Attribútumok**              | SQL-verzió - 12-es verzió |
| **Hivatkozások**              | [Az SQL-adatbázis Fenyegetésészlelés első lépései](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Lépések** |<p>A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli. Biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló új réteget biztosít.</p><p>A felhasználók hello használata az Azure SQL Database Auditing toodetermine Ha egy kísérlet tooaccess eredő megsértik a gyanús események tallózása vagy hello adatbázisban kihasználására.</p><p>A Fenyegetésészlelés teszi egyszerű tooaddress potenciális fenyegetések toohello adatbázis hello kell toobe egy biztonsági szakértő nélkül, vagy kezelheti a speciális biztonsági rendszerek figyelése</p>|

## <a id="analytics"></a>Azure Storage Analytics tooaudit hozzáférés az Azure Storage használata

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Azure Storage | 
| **SDL fázis**               | Környezet |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A |
| **Hivatkozások**              | [Tárolási analitika toomonitor engedélyezési típus használatával](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Lépések** | <p>Minden tárfiók egy Azure Storage Analytics tooperform naplózás engedélyezése és metrikákat adatainak tárolásához. hello storage analytics naplók tárolási elérésekor valaki által használt hitelesítési módszert például fontos információkat tartalmaznak.</p><p>Ez valóban akkor hasznos, ha szorosan hozzáférés toostorage esetlegesen korán lehet. Például a Blob Storage tárolóban után az összes hello tárolók tooprivate is valósítja meg az SAS-szolgáltatás hello használata az alkalmazások teljes. Ezután ellenőrizheti a hello naplók rendszeresen toosee Ha blobok hello tárfiókok kulcsait, ami azt jelezheti a biztonság megsértése, érhető el, vagy ha hello blobok olyan nyilvános, de nem lehetnek.</p>|

## <a id="sufficient-logging"></a>Elegendő naplózási megvalósítása

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | <p>egy biztonsági incidens után egy megfelelő napló hello hiánya Törvényszéki erőfeszítéseket is akadályozhatják. A Windows Communication Foundation (WCF) hello képességét toolog sikeres vagy sikertelen hitelesítési kísérlet kínál.</p><p>Naplózza a sikertelen hitelesítési kísérleteket esetleges találgatásos támadások rendszergazdák figyelmeztetést küld. Hasonlóképpen sikeres hitelesítési események naplózása biztosít hasznos napló amikor jogos fiók biztonsága sérül. A WCF szolgáltatás biztonsági eseményekre vonatkozó naplózási funkciója engedélyezése |

### <a name="example"></a>Példa
hello következő egy példa konfiguráció, a naplózás engedélyezve
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a id="audit-failure-handling"></a>Valósítja meg a megfelelő naplózási hiba kezelése

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | WCF | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | .NET-keretrendszer |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Lépések** | <p>Fejlett megoldás beállítása nem toogenerate toowrite tooan napló hiba esetén egy kivételt. Ha konfigurálva van a WCF toothrow kivételt nem toowrite tooan napló, a hello program nem kapnak értesítést a hello hiba és a kritikus fontosságú biztonsági események naplózását esetleg nem kerül sor.</p>|

### <a name="example"></a>Példa
Hello `<behavior/>` hello WCF konfigurációs fájl az alábbi elem arra utasítja a WCF toonot hello alkalmazás értesítése, ha a WCF toowrite tooan napló sikertelen lesz.
````
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
````
Ha nem toowrite tooan napló WCF toonotify hello program konfigurálása hello program hely tooalert hello szervezet elkérése nem tartják egy alternatív értesítési sémát kell rendelkezniük. 

## <a id="logging-web-api"></a>Győződjön meg arról, hogy a vizsgálati és naplózási megvalósul webes API

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Webes API | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | A vizsgálati és naplózási a webes API-k engedélyezése. Naplók kell rögzítheti a felhasználói környezet. Az összes fontos események azonosítása, és az események. A központi naplózás megvalósítása |

## <a id="logging-field-gateway"></a>Győződjön meg arról, hogy megfelelő vizsgálati és naplózási megvalósul mező átjáró

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT-mező átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | N/A  |
| **Lépések** | <p>Tooa mező átjáró több eszköz csatlakoztatásakor győződjön meg kapcsolódási kísérletek és hitelesítési állapot (sikeres vagy sikertelen) az egyes eszközök a naplóba, és kezelése a hello mező átjáró.</p><p>Emellett azokban az esetekben, ahol mező átjáró hello karbantartása az IoT-központ hitelesítő adatait, egyes eszközöket, győződjön meg arról, hogy naplózása történik meg, amikor a rendszer beolvassa a hitelesítő adatokat. A folyamat tooperiodically feltöltés hello naplók tooAzure IoT-központ tárolási a hosszú távú megőrzési fejlesztéséhez.</p> |

## <a id="logging-cloud-gateway"></a>Győződjön meg arról, hogy megfelelő vizsgálati és naplózási megvalósul átjáró

| Cím                   | Részletek      |
| ----------------------- | ------------ |
| **Összetevő**               | Az IoT átjáró | 
| **SDL fázis**               | Felépítés |  
| **Alkalmazandó technológiák** | Általános |
| **Attribútumok**              | N/A  |
| **Hivatkozások**              | [Bevezetés tooIoT Hub műveletek figyelése](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Lépések** | <p>Terv a összegyűjtése és tárolása naplózási keresztül gyűjtött adatokat az IoT Hub műveletek figyelését. A következő figyelési kategóriák hello engedélyezése:</p><ul><li>Eszköz identitása műveletek</li><li>Eszköz-felhő kommunikáció</li><li>Felhő eszközre kommunikáció</li><li>Kapcsolatok</li><li>Fájlfeltöltések</li></ul>|
