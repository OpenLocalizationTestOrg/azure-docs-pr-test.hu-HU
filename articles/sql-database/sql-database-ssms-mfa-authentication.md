---
title: aaaMulti-Factor authentication - Azure SQL |} Microsoft Docs
description: "Az SSMS Multi-beleszámítja hitelesítés használjanak SQL-adatbázis és az SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: fbd6e644-0520-439c-8304-2e4fb6d6eb91
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2017
ms.author: rickbyh
ms.openlocfilehash: 57ef63b7c7f2c5cf64c3e1ee194d865ee5b14177
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>Univerzális hitelesítés használata az SQL-adatbázis és az SQL Data Warehouse (többtényezős hitelesítés támogatása SSMS)
Az Azure SQL Database és az Azure SQL Data Warehouse támogatja az SQL Server Management Studio (SSMS) használatával kapcsolatok *Active Directory univerzális hitelesítési*. 
**Töltse le SSMS legújabb hello** – hello ügyfélszámítógépen hello SSMS, legfrissebb verziójának letöltését [töltse le az SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Az összes hello szolgáltatás ebben a témakörben használjon legalább július 2017, 17.2 verzió.  hello legutóbbi kapcsolat párbeszédpanel dolgozunk: ![1mfa-universal-csatlakozás](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "Completes hello felhasználónév mező.")  

## <a name="hello-five-authentication-options"></a>hello öt hitelesítési beállítások  
- Az Active Directory univerzális hitelesítése hello két nem interaktív hitelesítési módszert támogat (`Active Directory - Password` hitelesítési és `Active Directory - Integrated` hitelesítés). Nem interaktív `Active Directory - Password` és `Active Directory - Integrated` hitelesítési módszereket a különböző alkalmazások (ADO.NET JDBC, ODBC, stb.) használható. A két módszerről soha nem eredményez előugró párbeszédpanelen.

- `Active Directory - Universal with MFA`a hitelesítés egy interaktív módszerhez is támogatja az *Azure multi-factor Authentication* (MFA). Az Azure MFA segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Egyszerű hitelesítési beállítások (telefonhívás, szöveges üzenet az intelligens kártyák PIN-kód vagy a mobilalkalmazáson keresztüli értesítések), így felhasználók toochoose hello előnyben részesített módszer számos erős hitelesítés biztosítja. Az Azure ad-val interaktív MFA érvényesítéshez előugró párbeszédpanelen eredményezhet.

A multi-factor Authentication ismertetését lásd: [multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).
A konfigurálás lépéseinek végrehajtásához tekintse meg a [konfigurálása az Azure SQL Database multi-factor authentication szolgáltatást az SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Az Azure AD tartományi nevét vagy a bérlői azonosító paraméter   

Kezdve [SSMS verzió 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), felhasználók hello szolgáltatásba importált egyéb Azure Active könyvtárak vendégfelhasználók, mint a jelenlegi Active Directoryban hello Azure AD tartománynév megadása, vagy bérlői azonosító csatlakozáshoz. Vendégfelhasználók más Azure ADs, Microsoft-fiókok például outlook.com, hotmail.com, live.com vagy más hasonló gmail.com fiókok meghívott felhasználók tartalmazza. Ezt az információt, lehetővé teszi, hogy **Active Directory univerzális MFA hitelesítéssel** tooidentify hello helyes hitelesítő hatóság. Ez a beállítás akkor is szükséges toosupport Microsoft-fiókok (msa-t) például az Outlook.com-os, hotmail.com, live.com vagy nem MSA-fiókok. Minden ezek a felhasználók számára, akik toobe univerzális hitelesítéssel hitelesíteni kell megadnia, az Azure AD-tartomány nevét vagy a bérlői azonosító. Ez a paraméter hello aktuális az Azure AD tartományi neve/bérlői azonosító hello Azure kiszolgáló kapcsolódik jelöli. Például, ha Azure-kiszolgáló Azure AD-tartomány tartozik `contosotest.onmicrosoft.com` ahol felhasználói `joe@contosodev.onmicrosoft.com` importált az Azure AD tartományi felhasználóként van-e tárolva `contosodev.onmicrosoft.com`, ez a felhasználó a tartomány nevét kötelező tooauthenticate hello `contosotest.onmicrosoft.com`. Hello felhasználói hello csatolva az Azure AD tooAzure kiszolgáló a saját felhasználói, és nem MSA-fiókkal, nem tartományi nevét vagy a bérlői azonosító szükség. tooenter hello paraméter (SSMS 17.2 verziójával kezdődően), a hello **tooDatabase csatlakozás** párbeszédpanelen teljes hello párbeszédpanelen kiválasztása **Active Directory - többtényezős hitelesítéssel univerzális** hitelesítési, Kattintson a **beállítások**, teljes hello **felhasználónév** mezőbe, és kattintson a hello **kapcsolat tulajdonságai** fülre. Ellenőrizze a hello **AD tartomány nevét vagy a bérlő azonosítója** mezőbe, majd adja meg, például a tartománynév hello található hitelesítő hatóság (**contosotest.onmicrosoft.com**) vagy hello hello bérlői azonosító GUID-azonosítója  
   ![MFA-bérlő-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-toobusiness-support"></a>Az Azure AD üzleti toobusiness támogatja   
Az Azure Active Directory-felhasználók támogatott az Azure AD B2B forgatókönyvek vendégként (lásd: [Mi az az Azure B2B együttműködés](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) hozott létre az aktuális az Azure AD és a csatlakoztatott manuálisan egy csoport részeként a tooSQL adatbázis és az SQL Data Warehouse csatlakozni tud Transact-SQL használatával hello `CREATE USER` utasításban megadott adatbázisban. Például ha `steve@gmail.com` van a meghívott tooAzure AD `contosotest` (hello Azure Ad-tartomány `contosotest.onmicrosoft.com`), az Azure AD-csoport, például a `usergroup` kell létrehozni az Azure AD hello tartalmazó hello `steve@gmail.com` tag. Ezután ezt a csoportot kell létrehoznia egy adott adatbázis (azaz adatbázis) Azure AD az SQL-rendszergazdák vagy az Azure AD DBO hajtja végre a Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` utasítást. Hello adatbázis-felhasználó létrehozása után majd hello felhasználói `steve@gmail.com` túl bejelentkezhet`MyDatabase` hello SSMS hitelesítési lehetőséggel `Active Directory – Universal with MFA support`. hello felhasználói csoport, alapértelmezés szerint rendelkezik csak az engedéllyel, és minden további adatokhoz való hozzáférést, amelyeket toobe hello meg kell hello szokásos módon. Vegye figyelembe, hogy a felhasználó `steve@gmail.com` a Vendég felhasználó kell hello jelölőnégyzetet és hello AD tartománynév hozzáadása `contosotest.onmicrosoft.com` az hello SSMS **Connection tulajdonság** párbeszédpanel megnyitásához. Hello **AD tartomány neve vagy a bérlői azonosító** beállítás csak a támogatott hello univerzális MFA kapcsolati beállításokkal, ellenkező esetben szürkén jelenik meg.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>SQL-adatbázis és az SQL Data Warehouse univerzális hitelesítési korlátozásai
* SSMS és SqlPackage.exe hello csak eszközök jelenleg engedélyezve van a többtényezős hitelesítés az Active Directory univerzális hitelesítésen keresztül.
* SSMS verzió 17.2, többfelhasználós egyidejű hozzáférés univerzális hitelesítéssel az MFA Használatát támogatja. 17.0 és 17.1, a napló korlátozza az SSMS fiókkal univerzális hitelesítési tooa egyetlen Azure Active Directory-példányok esetében. egy másik Azure AD-fiókot a toolog, az SSMS egy másik példánya kell használnia. (Ez a korlátozás korlátozott tooActive Directory univerzális hitelesítés, az Active Directory jelszavas hitelesítést, Active Directory integrált hitelesítést vagy SQL Server-hitelesítés toodifferent kiszolgálók bejelentkezhet).
* Szolgáltatáshoz az SSMS Object Explorer, a lekérdezés-szerkesztő és a Lekérdezéstár a képi megjelenítéshez tartozó Active Directory univerzális hitelesítést támogatja.
* SSMS verzió 17.2 DacFx varázsló támogatást nyújt exportálási/kivonat/központi telepítése az adatbázist. Univerzális hitelesítéssel hello kezdeti hitelesítési párbeszédpaneléről egy adott felhasználó hitelesítését követően hello DacFx varázsló funkciók hello megegyezik más hitelesítési módszerek mindegyikéhez ugyanúgy.
* hello SSMS tábla Tervező nem támogatja az univerzális hitelesítést.
* Nincsenek további szoftvereket az Active Directory univerzális hitelesítést azzal a különbséggel, hogy az SSMS támogatott verzióját kell használnia.  
* hello Active Directory Authentication Library (ADAL) verzióját az univerzális hitelesítéshez frissített tooits ADAL.dll 3.13.9 érhető el, amely a legfrissebb volt. Lásd: [Active Directory hitelesítési könyvtár 3.14.1](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).


## <a name="next-steps"></a>Következő lépések

- A konfigurálás lépéseinek végrehajtásához tekintse meg a [konfigurálása az Azure SQL Database multi-factor authentication szolgáltatást az SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
- Biztosítani, mások hozzáférhetnek tooyour adatbázis: [SQL adatbázis-hitelesítés és engedélyezés: hozzáférés biztosítása](sql-database-manage-logins.md)  
- Győződjön meg arról, hogy mások hello tűzfalon keresztül csatlakozhassanak: [konfigurálása az Azure SQL Database kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-configure-firewall-settings.md)  
- [Konfigurálhatja és kezelheti az Azure Active Directory-hitelesítés az SQL Database vagy az SQL Data Warehouse](sql-database-aad-authentication-configure.md)  
- [Microsoft SQL Server Adatrétegbeli alkalmazási keretrendszer (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)  
- [Importálja a BACPAC fájl tooa új Azure SQL-adatbázis](../sql-database/sql-database-import.md)  
- [Azure SQL adatbázis tooa BACPAC fájl exportálása](../sql-database/sql-database-export.md)  
- C# felület [IUniversalAuthProvider felület](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
