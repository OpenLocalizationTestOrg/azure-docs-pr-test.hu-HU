---
title: aaaConfigure a multi-factor authentication - Azure SQL |} Microsoft Docs
description: "Az SSMS Multi-beleszámítja hitelesítés használjanak SQL-adatbázis és az SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/17/2017
ms.author: rickbyh
ms.openlocfilehash: acb275965f4199f7d5e1378d5077824a9bbc80e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Az SQL Server Management Studio és az Azure AD többtényezős hitelesítés beállítása

Ez a témakör bemutatja, hogyan toouse Azure Active Directory többtényezős hitelesítés (MFA) az SQL Server Management Studio. Az Azure AD MFA használható SSMS vagy SqlPackage.exe tooAzure SQL-adatbázis és az Azure SQL Data Warehouse kapcsolódáskor.

Többtényezős hitelesítés az Azure SQL Database áttekintését lásd: [univerzális hitelesítés használata az SQL-adatbázis és az SQL Data Warehouse (többtényezős hitelesítés támogatása SSMS)](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Konfigurációs lépések

1. **Egy Azure Active Directory konfigurálása** – további információért lásd: [az Azure AD-címtár felügyelete](https://msdn.microsoft.com/library/azure/hh967611.aspx), [a helyszíni identitások integrálása az Azure Active Directoryval](../active-directory/active-directory-aadconnect.md), [Adja hozzá a saját tartomány neve tooAzure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure mostantól támogatja a Windows Server Active Directory összevonási](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), és [kezelése az Azure AD a Windows PowerShell használatával ](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Többtényezős hitelesítés beállítása** – részletes ismertetését lásd: [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md), [feltételes hozzáférést (többtényezős hitelesítés) az Azure SQL adatbázishoz és Adatraktárhoz](sql-database-conditional-access.md). (Teljes feltételes hozzáférés szükséges egy prémium szintű Azure Active Directory (Azure AD). Korlátozott MFA érhető el a szabványos Azure AD-val.)
3. **Konfigurálja az SQL Database vagy az SQL Data Warehouse az Azure AD-alapú hitelesítés** – részletes ismertetését lásd: [csatlakozás tooSQL adatbázis vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítéssel](sql-database-aad-authentication.md).
4. **Töltse le a szolgáltatáshoz az SSMS** – hello ügyfélszámítógépen, töltse le a legfrissebb SSMS hello [töltse le az SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Az összes hello szolgáltatás ebben a témakörben használjon legalább július 2017, 17.2 verzió.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Csatlakozás univerzális hitelesítési szolgáltatáshoz az SSMS

hello lépések bemutatják, hogyan tooconnect tooSQL adatbázis vagy az SQL Data Warehouse használatával hello SSMS legújabb.

1. Univerzális hitelesítés használatát, a hello tooconnect **tooServer csatlakozás** párbeszédpanelen jelölje ki **Active Directory - MFA-támogatással rendelkező univerzális**. (Ha megjelenik **Active Directory univerzális hitelesítési** hello SSMS legújabb verzióját a rendszer nem.)  
   ![1mfa-universal-csatlakozás][1]  
2. Teljes hello **felhasználónév** hello Azure Active Directory hitelesítő adatokkal, hello formátumban mezőben `user_name@domain.com`.  
   ![1mfa-universal-csatlakozás-felhasználó](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Ha Vendég felhasználóként csatlakozik, akkor a **beállítások**, és a hello **Connection tulajdonság** párbeszédpanelen teljes hello **AD tartomány nevét vagy a bérlő azonosítója** mezőbe. További információkért lásd: [univerzális hitelesítés használata az SQL-adatbázis és az SQL Data Warehouse (többtényezős hitelesítés támogatása SSMS)](sql-database-ssms-mfa-authentication.md).
   ![MFA-bérlő-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. A szokásos módon SQL Database és az SQL Data Warehouse, meg kell nyomnia **beállítások** , és adja meg hello adatbázis hello **beállítások** párbeszédpanel megnyitásához. (Ha hello csatlakozó felhasználó-e a Vendég felhasználói (azaz joe@outlook.com), meg kell hello jelölőnégyzetet, és adja hozzá a hello aktuális AD tartomány neve vagy a bérlői azonosító beállítások részeként. Lásd: [univerzális hitelesítés használata az SQL-adatbázis és az SQL Data Warehouse (többtényezős hitelesítés támogatása SSMS)]()(sql-adatbázis-ssms-mfa-authentication.md. Kattintson a **Connect**.  
5. Ha hello **jelentkezzen be fiókjával tooyour** párbeszédpanel jelenik meg, hello fiókot és az Azure Active Directory-identitás jelszavát. Nem kell jelszót szükség, ha egy felhasználó egy Azure AD-val összevont tartomány része.  
   ![2mfa-bejelentkezés][2]  

   > [!NOTE]
   > Egy olyan fiókkal, többtényezős Hitelesítést nem igénylő univerzális hitelesítéshez csatlakozás ezen a ponton. A felhasználók számára a többtényezős hitelesítés megkövetelése folytassa a lépéseket követve hello:
   >  
   
6. Két többtényezős hitelesítés beállítása párbeszédpanel jelenhet meg. Egyetlen most művelet hello MFA rendszergazda függ, és ezért nem kötelező. Egy engedélyezve van az MFA-tartomány ezt a lépést néha nem előre definiált (például hello tartomány számára szükséges felhasználók toouse az intelligens kártyákat és PIN-kódot).  
   ![3mfa-telepítő][3]  
7. hello második lehetséges párbeszédpanel tooselect hello részleteit a hitelesítési módszer lehetővé teszi egy alkalommal. a rendszergazda által hello lehetséges beállításainak konfigurálása.  
   ![4mfa ellenőrzése 1][4]  
8. hello Azure Active Directory erősítse meg információkat tooyou hello küld. Amikor hello megerősítési kódot, megadhatja azt hello **adja meg a megerősítési kódot** mezőbe, majd kattintson **bejelentkezés**.  
   ![5mfa ellenőrzése 2][5]  

Ha ellenőrzés befejeződött, az SSMS csatlakozik, általában feltéve, hogy érvényes hitelesítő adatokat és a tűzfalon a hozzáférést.

## <a name="next-steps"></a>Következő lépések

* Többtényezős hitelesítés az Azure SQL Database áttekintését lásd: az univerzális hitelesítési [SQL-adatbázis és az SQL Data Warehouse (többtényezős hitelesítés támogatása SSMS)](sql-database-ssms-mfa-authentication.md).
* Biztosítani, mások hozzáférhetnek tooyour adatbázis: [SQL adatbázis-hitelesítés és engedélyezés: hozzáférés biztosítása](sql-database-manage-logins.md)  
Győződjön meg arról, hogy mások hello tűzfalon keresztül csatlakozhassanak: [konfigurálása az Azure SQL Database kiszolgálószintű tűzfal szabály használatával hello Azure-portálon](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

