---
title: "Hitelesítés az Azure SQL Data Warehouse |} Microsoft Docs"
description: "Az Azure Active Directory (AAD) és az SQL Server hitelesítés az Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
tags: 
ms.assetid: fefaaa75-2d0c-4e5d-aadb-410342d1ad73
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.custom: security
ms.date: 03/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 92f48027051bc4aff4d6b8d66fdd6de81bba3657
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/21/2017
---
# <a name="authentication-to-azure-sql-data-warehouse"></a>Authentication to Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Biztonság – áttekintés](sql-data-warehouse-overview-manage-security.md)
> * [Hitelesítés](sql-data-warehouse-authentication.md)
> * [Titkosítás (portál)](sql-data-warehouse-encryption-tde.md)
> * [Titkosítás (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Csatlakozás az SQL Data Warehouse, át kell a biztonsági hitelesítő adatok hitelesítési célokra. A kapcsolat létrehozása után egyes kapcsolat beállításai a lekérdezés munkamenet létrehozásának részeként.  

A biztonsággal és a data warehouse kapcsolatainak engedélyezésével további információkért lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL-hitelesítés
Csatlakozás az SQL Data Warehouse, meg kell adnia a következőket:

* Teljes kiszolgálónév
* Adja meg az SQL-hitelesítés
* Felhasználónév
* Jelszó
* Alapértelmezett adatbázis (nem kötelező)

Alapértelmezés szerint a kapcsolat csatlakozik a *fő* és nem a felhasználói adatbázis. Szeretne csatlakozni a felhasználói adatbázis, ehhez két lehetőség közül választhat:

* Adja meg az alapértelmezett adatbázis, ha a kiszolgáló regisztrálása az SQL Server Object Explorer, az SSDT, SSMS, vagy az alkalmazás kapcsolódási karakterláncban. Például adja meg a InitialCatalog értékét az ODBC-kapcsolat.
* Jelölje ki a felhasználói adatbázis SSDT-ben a munkamenet létrehozása előtt.

> [!NOTE]
> A Transact-SQL-utasítás **használata adatbázis;** az adatbázist a kapcsolat módosítása nem támogatott. Kapcsolódás az SQL Data Warehouse és az SSDT együttes útmutatásért tekintse meg a [lekérdezése a Visual Studio] [ Query with Visual Studio] cikk.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Az Azure Active Directory (AAD) hitelesítés
[Az Azure Active Directory] [ What is Azure Active Directory] hitelesítési egy olyan mechanizmus kapcsolódni a Microsoft Azure SQL Data Warehouse identitások az Azure Active Directory (Azure AD) segítségével. Azure Active Directory-hitelesítéssel adatbázis-felhasználók identitását, és más Microsoft-szolgáltatásokban egyetlen központi helyen központilag kezelheti. Központi azonosítófelügyeleti egy helyen az SQL Data Warehouse felhasználók kezeléséhez biztosít, és egyszerűbbé teszi a jogosultság kezelése. 

### <a name="benefits"></a>Előnyök
Az Azure Active Directory előnyöket nyújtja:

* Köszönhetően a SQL Server-hitelesítést.
* Segítséget nyújt a felhasználói identitások elterjedése leállítása adatbázis-kiszolgáló között.
* Lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen
* Adatbázis-engedélyek a külső (AAD) csoportok kezelése.
* Jelszavak tárolását megszünteti az integrált Windows-hitelesítés és egyéb Azure Active Directory által támogatott hitelesítési engedélyezésével.
* Használja az adatbázis-felhasználók számára az adatbázis szintjén személyazonosság tartalmazott.
* Kapcsolódás az SQL Data Warehouse-alkalmazások jogkivonat-alapú hitelesítést is támogatja.
* Az SQL Server Management Studio támogatja a többtényezős hitelesítés az Active Directory univerzális hitelesítésen keresztül. A multi-factor Authentication ismertetését lásd: [SSMS támogatása az Azure AD MFA az SQL-adatbázis és az SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Az Azure Active Directory még viszonylag új, és bizonyos korlátozások vonatkoznak. Győződjön meg arról, hogy Azure Active Directory remekül beválik, ha a környezetben, lásd: [Azure AD-funkciókat és korlátozások][Azure AD features and limitations], kifejezetten a további szempontok.
> 
> 

### <a name="configuration-steps"></a>Konfigurációs lépések
Kövesse az alábbi lépéseket az Azure Active Directory-hitelesítés konfigurálása.

1. Létrehozása és feltöltése az Azure Active Directoryban
2. Választható lehetőség: Hozzárendelése vagy az active directory jelenleg az Azure-előfizetéshez társított módosítása
3. Azure Active Directory-rendszergazda az Azure SQL Data Warehouse létrehozása.
4. Állítsa be az ügyfélszámítógépen
5. Tartalmazott adatbázis-felhasználók létrehozása az Azure AD-identitások leképezve az adatbázisban
6. Csatlakozás az adatraktár identitások az Azure AD segítségével

Azure Active Directory-felhasználók jelenleg nem láthatók az SSDT Object Explorerben. Megoldás a felhasználók megtekintése [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-the-details"></a>A részletei
* Konfigurálhatja és használhatja az Azure Active Directory hitelesítési lépésekre csaknem azonosak, az Azure SQL Database és Azure SQL Data warehouse-bA. Kövesse a témakör részletes lépéseit [csatlakozás az SQL Database vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítéssel](../sql-database/sql-database-aad-authentication.md).
* Egyéni adatbázis-szerepkörök létrehozása és felhasználók hozzáadása a szerepkörökhöz. A szerepkörök részletes szükséges engedélyeket adja meg. További információkért lásd: [Ismerkedés az adatbázis-motor engedélyek](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>További lépések
Indítsa el a Visual Studio és más alkalmazások az adatraktár lekérdezésére, lásd: [lekérdezése a Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
