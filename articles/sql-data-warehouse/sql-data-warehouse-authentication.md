---
title: az SQL Data Warehouse aaaAuthentication tooAzure |} Microsoft Docs
description: "Az Azure Active Directory (AAD) és az SQL Server hitelesítési tooAzure SQL Data Warehouse."
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
ms.openlocfilehash: 66673ce1d4761243755254c8f64de1078a0ea82e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-tooazure-sql-data-warehouse"></a>Az SQL Data Warehouse hitelesítési tooAzure
> [!div class="op_single_selector"]
> * [Biztonság – áttekintés](sql-data-warehouse-overview-manage-security.md)
> * [Hitelesítés](sql-data-warehouse-authentication.md)
> * [Titkosítás (portál)](sql-data-warehouse-encryption-tde.md)
> * [Titkosítás (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

tooconnect tooSQL Data warehouse-ba, át kell adnia a hitelesítő adatokat hitelesítési célokra. A kapcsolat létrehozása után egyes kapcsolat beállításai a lekérdezés munkamenet létrehozásának részeként.  

További információ a biztonsági és hogyan tooenable kapcsolatok tooyour adatraktár, lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL-hitelesítés
tooconnect tooSQL Data warehouse-ba, meg kell adnia a következő információk hello:

* Teljes kiszolgálónév
* Adja meg az SQL-hitelesítés
* Felhasználónév
* Jelszó
* Alapértelmezett adatbázis (nem kötelező)

Alapértelmezés szerint kapcsolódás toohello *fő* és nem a felhasználói adatbázis. tooconnect tooyour felhasználói adatbázishoz, választhat toodo két dolog egyikét:

* Adjon meg hello alapértelmezett adatbázis, amikor a kiszolgáló regisztrálása a hello SQL Server Object Explorer SSDT, SSMS, vagy az alkalmazás kapcsolódási karakterláncban. Például a hello InitialCatalog paraméter az ODBC-kapcsolat.
* Jelölje ki a felhasználói adatbázis hello SSDT-ben a munkamenet létrehozása előtt.

> [!NOTE]
> a Transact-SQL hello **használata adatbázis;** hello adatbázis kapcsolat módosítása nem támogatott. Csatlakozás az adatraktár tooSQL és az SSDT együttes útmutatásért tekintse meg a toohello [lekérdezése a Visual Studio] [ Query with Visual Studio] cikk.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Az Azure Active Directory (AAD) hitelesítés
[Az Azure Active Directory] [ What is Azure Active Directory] hitelesítési egy olyan mechanizmus, amely tooMicrosoft Azure SQL Data Warehouse identitások az Azure Active Directory (Azure AD) segítségével. Azure Active Directory-hitelesítéssel központilag kezelheti hello identitások az adatbázis-felhasználók és más Microsoft-szolgáltatásokban egyetlen központi helyen. Központi azonosítófelügyeleti biztosít egy helyen toomanage SQL Data Warehouse felhasználók számára, és egyszerűbbé teszi a jogosultság kezelése. 

### <a name="benefits"></a>Előnyök
Az Azure Active Directory előnyöket nyújtja:

* Egy alternatív tooSQL kiszolgáló hitelesítést nyújt.
* Segít hello elterjedése tartozó felhasználói azonosítók leállítása adatbázis-kiszolgáló között.
* Lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen
* Adatbázis-engedélyek a külső (AAD) csoportok kezelése.
* Jelszavak tárolását megszünteti az integrált Windows-hitelesítés és egyéb Azure Active Directory által támogatott hitelesítési engedélyezésével.
* Felhasználási tartalmazott adatbázis felhasználók tooauthenticate identitások hello adatbázis szintjén.
* Csatlakozás az adatraktár tooSQL alkalmazások jogkivonat-alapú hitelesítést is támogatja.
* Az SQL Server Management Studio támogatja a többtényezős hitelesítés az Active Directory univerzális hitelesítésen keresztül. A multi-factor Authentication ismertetését lásd: [SSMS támogatása az Azure AD MFA az SQL-adatbázis és az SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Az Azure Active Directory még viszonylag új, és bizonyos korlátozások vonatkoznak. tooensure, hogy az Azure Active Directory remekül beválik, ha a környezetben, lásd: [Azure AD-funkciókat és korlátozások][Azure AD features and limitations], kifejezetten hello további szempontokat.
> 
> 

### <a name="configuration-steps"></a>Konfigurációs lépések
Kövesse a lépéseket tooconfigure Azure Active Directory-hitelesítés.

1. Létrehozása és feltöltése az Azure Active Directoryban
2. Választható lehetőség: Hozzárendelése vagy hello active directory jelenleg az Azure-előfizetéshez társított módosítása
3. Azure Active Directory-rendszergazda az Azure SQL Data Warehouse létrehozása.
4. Állítsa be az ügyfélszámítógépen
5. Felhasználók létrehozása a tartalmazott adatbázis az adatbázis leképezve tooAzure AD identitások
6. Tooyour adatraktár csatlakoztatása az Azure AD-azonosítók használatával

Azure Active Directory-felhasználók jelenleg nem láthatók az SSDT Object Explorerben. A probléma megoldásához hello felhasználók megtekintése [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-hello-details"></a>Hello részletei
* hello lépéseket tooconfigure és -felhasználási Azure Active Directory-hitelesítés esetén Azure SQL Database és az Azure SQL Data Warehouse csaknem azonosak. Hajtsa végre a hello hello témakörben ismertetett lépések részletes [csatlakozás tooSQL adatbázis vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítéssel](../sql-database/sql-database-aad-authentication.md).
* Hozzon létre egyéni adatbázis-szerepkörök, és a felhasználók toohello szerepkörök hozzáadásához. A részletes szükséges engedélyeket adja meg toohello szerepkörök. További információkért lásd: [Ismerkedés az adatbázis-motor engedélyek](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Következő lépések
a Visual Studio és más alkalmazásokkal, az adatraktár lekérdezésére toostart lásd [lekérdezése a Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
