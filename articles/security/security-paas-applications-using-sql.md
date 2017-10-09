---
title: "aaaSecuring PaaS adatbázisokat az Azure-ban |} Microsoft Docs"
description: " Tudnivalók Azure SQL Database és az SQL Data Warehouse biztonsági gyakorlati tanácsok a PaaS webes és mobilalkalmazásokhoz biztonságossá tételéhez. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.openlocfilehash: a7f9a2846b59dcb05e82f2ad88b41c5213295603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-databases-in-azure"></a>Az Azure-ban PaaS-adatbázisok védelme

Ez a cikk arról lesz szó gyűjteménye [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) és [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme. Az alábbi gyakorlati tanácsok az Azure-ral tapasztalatunk származik, és ügyfelek hello lép, például saját maga.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Az Azure SQL Database és SQL-adatraktár
[Az Azure SQL Database](../sql-database/sql-database-technical-overview.md) és [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) adja meg az Internet-alapú alkalmazások relációs adatbázis-szolgáltatás. Nézzük szolgáltatások, amelyek segítenek megvédeni az alkalmazások és adatok Azure SQL Database és az SQL Data Warehouse PaaS központi telepítés használata esetén:

- Az Azure Active Directory authentication (helyett az SQL Server-hitelesítés)
- Az Azure SQL-tűzfal
- Az átlátható adattitkosítás (TDE)

## <a name="best-practices"></a>Ajánlott eljárások

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Egy központi identitás-tárházat használja a hitelesítéshez és engedélyezéshez

Az Azure SQL-adatbázisok lehet két típusú hitelesítés egyik konfigurált toouse:

- **SQL-hitelesítés** felhasználónevet és jelszót használja. Hello logikai kiszolgáló létrehozása után az adatbázis, a "kiszolgáló-rendszergazda" Bejelentkezés a felhasználónevet és jelszót adott meg. Ezeket a hitelesítő adatokat használva hitelesítheti, ezen a kiszolgálón tooany adatbázis hello adatbázis tulajdonosa.

- **Az Azure Active Directory-hitelesítéssel** használja az Azure Active Directory által kezelt identitások és a felügyelt és integrált tartományok használata támogatott. toouse Azure Active Directory-hitelesítéssel, létre kell hoznia egy másik kiszolgáló rendszergazdája nevű hello "Azure AD admin," tooadminister az Azure Active Directory-felhasználók és csoportok engedélyezett. Ez a rendszergazda a normál kiszolgálói rendszergazdák által elvégezhető összes műveletet is végrehajthatja.

[Az Azure Active Directory authentication](../active-directory/develop/active-directory-authentication-scenarios.md) egy mechanizmus, amely tooAzure SQL-adatbázis és az SQL Data Warehouse identitások az Azure Active Directory (AD) segítségével. Az Azure AD egy alternatív tooSQL kiszolgálóhitelesítés biztosít, így hello elterjedése tartozó felhasználói azonosítók leállíthatja, adatbázis-kiszolgáló között. Az Azure AD hitelesítési lehetővé teszi, hogy Ön toocentrally adatbázis-felhasználók és más Microsoft-szolgáltatásokban egyetlen központi helyen hello identitáskezelést. Központi azonosítófelügyeleti itt egyetlen toomanage adatbázis-felhasználók és egyszerűbbé teszi a jogosultság kezelése.  

SQL-hitelesítés helyett az Azure AD-alapú hitelesítés használatának előnyei a következők:

- Lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen.
- Kezeli az adatbázis-engedélyek használata külső Azure Active Directory csoportokat.
- Jelszavak tárolását megszünteti az integrált Windows-hitelesítés és más Azure AD által támogatott hitelesítési engedélyezésével.
- Felhasználási tartalmazott adatbázis felhasználók tooauthenticate identitások hello adatbázis szintjén.
- Kapcsolódás adatbázis tooSQL alkalmazások jogkivonat-alapú hitelesítést is támogatja.
- AD FS (tartomány-összevonás) vagy a natív felhasználói/jelszó-hitelesítés támogat egy helyi az Azure AD tartományi szinkronizálás nélkül.
- Active Directory univerzális hitelesítést használó, beleértve az SQL Server Management Studio kapcsolatokat támogatja [multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md). Többtényezős hitelesítés tartalmazza, könnyen ellenőrzési lehetőségek széles erős hitelesítés – telefonhívás, szöveges üzenetet, intelligens kártyák PIN-kód és az értesítést a mobilalkalmazásban. További információkért lásd: [SSMS támogatása az Azure AD MFA az SQL-adatbázis és az SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

További információ az Azure AD-alapú hitelesítés, toolearn lásd:

- [Csatlakozás tooSQL adatbázis vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítés](../sql-database/sql-database-aad-authentication.md)
- [Az SQL Data Warehouse hitelesítési tooAzure](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Azure SQL DB használatával az Azure AD hitelesítési jogkivonat-alapú hitelesítés támogatása](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> tooensure, hogy az Azure Active Directory remekül beválik, ha a környezetben, lásd: [Azure AD-funkciókat és korlátozások](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), kifejezetten hello további szempontokat.
>
>

### <a name="restrict-access-based-on-ip-address"></a>IP-címen alapuló hozzáférés korlátozása
Létrehozhat olyan tűzfalszabály, amely elfogadható IP-címek tartományát adja meg. Ezek a szabályok hello server és adatbázis szintek is telepíthető. Azt javasoljuk, adatbázis-szintű tűzfalszabályok amikor csak lehetséges tooenhance biztonsági és toomake az adatbázis több hordozható. Kiszolgálószintű tűzfal-szabályok használata leginkább a rendszergazdák számára, és ha sok adatbázis, amely rendelkezik hello ugyanazt a hozzáférési követelményeket, de nincs szüksége toospend idő, minden adatbázisnak külön-külön konfigurálása.

SQL-adatbázis alapértelmezett forrás IP-címkorlátozások engedélyezi a hozzáférést a következő bármely Azure-(beleértve más előfizetések és a bérlők). Korlátozhatja a tooonly lehetővé teszik az IP-címek tooaccess hello példányt. Erős hitelesítés még akkor is, az SQL-tűzfal és az IP-címkorlátozások, továbbra is szükséges. Az ebben a cikkben végrehajtott hello ajánlott megtekintéséhez.

További információ az Azure SQL tűzfal és az IP-korlátozásokat toolearn lásd:

- [Az Azure SQL Database hozzáférés-vezérlés](../sql-database/sql-database-control-access.md)
- [Az Azure SQL Database tűzfalszabályok - konfigurálása – áttekintés](../sql-database/sql-database-firewall-configure.md)
- [Hello Azure portál használata az Azure SQL Database kiszolgálószintű tűzfal szabály konfigurálása](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>A tárolt adatok titkosítása
[Transzparens Data Encryption (TDE)](https://msdn.microsoft.com/library/azure/bb934049) alapértelmezés szerint engedélyezve van. TDE transzparens módon titkosítja az SQL Server, az Azure SQL Database és Azure SQL Data Warehouse adatainak és naplókönyvtárainak fájlokat. TDE közvetlen hozzáférést toohello fájlok vagy a biztonsági mentés a biztonsági sérülésekkel szembeni védelmet nyújt. Ez lehetővé teszi inaktív adatok tooencrypt meglévő alkalmazások módosítása nélkül. TDE mindig maradjon engedélyezett; Ez azonban nem állítja le egy támadó, hello normál elérési útvonalat. TDE hello képességét toocomply sok törvényi, a szabályzat és a különböző iparágak létrehozott iránymutatásokat tartalmaz.

Az Azure SQL kulcs kapcsolatos probléma TDE kezeli. A TDE, a helyszínen különös gondot kell fordítani tooensure helyreállíthatósága, amikor adatbázisok áthelyezése. Kifinomultabb forgatókönyvekben hello kulcsok explicit módon felügyelhető az Azure Key Vault a bővíthető kulcskezelési (lásd: [TDE engedélyezése az SQL Server használatával EKM](/security/encryption/enable-tde-on-sql-server-using-ekm)). Ezenkívül lehetővé teszi a kapcsolja a saját kulcs használatának (BYOK) Azure kulcs tárolók BYOK funkció segítségével.

Az Azure SQL az oszlopok titkosítását biztosítja [mindig titkosítja](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Ez lehetővé teszi, hogy csak engedélyezett alkalmazások toosensitive oszlopok. Az ilyen típusú titkosítás használata korlátozza a titkosított oszlopok tooequality-alapú értékek az SQL-lekérdezések.

Alkalmazás a blokkszintű titkosítás szelektív adatok is használható. Adatok közös joghatóság alá aggályokat néha kivédhető, hogy a megfelelő országot hello kulccsal adatok titkosításával. Ez megakadályozza, hogy még véletlen adatátvitel okozza a problémát, mert lehetetlen toodecrypt hello adatok nélkül hello kulcs, feltéve, hogy egy erős algoritmust használja (például az AES 256).

További óvintézkedéseket toohelp biztonságos hello adatbázis például a biztonságos rendszerek tervezése, bizalmas eszközök titkosítása és felépítése körül hello adatbázis-kiszolgálók egy tűzfal is használhatja.

## <a name="next-steps"></a>Következő lépések
Ez a cikk rendszerben jelent meg tooa gyűjtemény SQL-adatbázis és az SQL Data Warehouse ajánlott biztonsági eljárások az PaaS webes és mobilalkalmazások védelme. További információ a PaaS üzemelő példányok védelme toolearn lásd:

- [PaaS-környezetek védelme](security-paas-deployments.md)
- [A PaaS webes és mobilalkalmazásokhoz, az Azure App Services biztonságossá tétele](security-paas-applications-using-app-services.md)
