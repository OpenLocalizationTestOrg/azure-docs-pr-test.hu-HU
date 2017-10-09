---
title: "Azure SQL adatbázishoz és Adatraktárhoz Access - aaaConditional |} Microsoft Doc"
description: "Megtudhatja, hogyan tooconfigure feltételes hozzáférés az Azure SQL adatbázishoz és Adatraktárhoz."
services: sql-database
author: BYHAM
manager: jhubbard
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: rickbyh
ms.openlocfilehash: f49f4708c0f1b3cad1539d630c2efd919f8ece68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Feltételes hozzáférés (MFA) az Azure SQL adatbázishoz és Adatraktárhoz  

SQL Database és az SQL Data Warehouse támogatja a Microsoft feltételes hozzáférést. a következő lépéseket megjelenítése hogyan hello tooconfigure SQL-adatbázis tooenforce feltételes hozzáférési házirendet.  

## <a name="prerequisites"></a>Előfeltételek  
- Konfigurálnia kell az SQL Database vagy az SQL Data Warehouse toosupport Azure Active Directory-hitelesítés. Adott lépéseiért lásd: [konfigurálása és kezelése az Azure Active Directory-hitelesítés az SQL Database vagy az SQL Data Warehouse](sql-database-aad-authentication-configure.md).  
- Ha engedélyezve van a többtényezős hitelesítést, csatlakoznia kell a támogatott eszköz, például a legújabb SSMS hello. További információkért lásd: [konfigurálása az Azure SQL Database multi-factor authentication szolgáltatást az SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Az Azure SQL DB/DW hitelesítésszolgáltató konfigurálása  
1.  Bejelentkezési toohello portálon, válassza ki **Azure Active Directory**, majd válassza ki **feltételes hozzáférés**. További információkért lásd: [Azure Active Directory feltételes hozzáférési technikai útmutató](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference).  
  ![feltételes hozzáférési panel](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2.  A hello **feltételes hozzáférés-házirendek** panelen kattintson a **új házirend**, adjon meg egy nevet, és kattintson a **szabályok konfigurálása**.  
3.  A **hozzárendelések**, jelölje be **felhasználók és csoportok**, ellenőrizze **felhasználók és csoportok kiválasztása**, majd válassza ki a hello felhasználót vagy csoportot a feltételes hozzáférés. Kattintson a **kiválasztása**, és kattintson a **végzett** tooaccept a kijelölés.  
  ![felhasználók és csoportok kiválasztása](./media/sql-database-conditional-access/select-users-and-groups.png)  

4.  Válassza ki **felhőalapú alkalmazásokba**, kattintson a **alkalmazásokról**. Megjelenik az elérhető, a feltételes hozzáférés az összes alkalmazást. Válassza ki **Azure SQL Database**, hello alul kattintson **kiválasztása**, és kattintson a **végzett**.  
  ![Válassza ki az SQL-adatbázis](./media/sql-database-conditional-access/select-sql-database.png)  
  Ha nem talál **Azure SQL Database** szerepel a következő harmadik képernyőfelvétel hello, végezze el a következő lépéseket hello:   
  - Jelentkezzen be egy AAD-rendszergazdai fiók az SSMS használatával tooyour Azure SQL adatbázis vagy Adatraktár-példány.  
  - Végrehajtás `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
  - Jelentkezzen be tooAAD, és ellenőrizze, hogy az Azure SQL Database és az adatraktár szerepel-e hello alkalmazásaiban az aad-ben.  

5.  Válassza ki **hozzáférés-szabályozási**, jelölje be **Grant**, és ellenőrizze a tooapply kívánt hello házirendet. Ez a példa azt válassza **többtényezős hitelesítést**.  
  ![Válassza ki a hozzáférés biztosítása](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Összefoglalás  
hello kijelölt alkalmazás (az Azure SQL Database) tooconnect tooAzure SQL DB/DW Azure AD prémium, így most érvénybe lépteti a kiválasztott hello feltételes hozzáférési házirend, **szükséges többtényezős hitelesítést.**  
Tudnivalók Azure SQL adatbázishoz és Adatraktárhoz többtényezős hitelesítéssel kapcsolatban, forduljon a MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Következő lépések  

Az oktatóanyagok esetén lásd: [az Azure SQL Database biztonságos](sql-database-security-tutorial.md).
