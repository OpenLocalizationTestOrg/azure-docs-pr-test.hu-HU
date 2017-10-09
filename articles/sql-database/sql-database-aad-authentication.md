---
title: "aaaAzure Active Directory hitelesítési - Azure SQL (áttekintés) |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Active Directory-hitelesítés és az SQL-adatbázis és az SQL Data Warehouse az"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/11/2017
ms.author: rickbyh
ms.openlocfilehash: 7a63a162653b65294e11d3fa5bf39c320e742854
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-or-sql-data-warehouse"></a>Az SQL Database vagy az SQL Data Warehouse hitelesítéshez használandó Azure Active Directory-hitelesítés
Az Azure Active Directory-hitelesítés egy olyan mechanizmus, amely tooMicrosoft Azure SQL Database és [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) identitások az Azure Active Directory (Azure AD) segítségével. Az Azure AD-alapú hitelesítés az adatbázis-felhasználók hello identitások és más Microsoft-szolgáltatásokban egyetlen központi helyen központilag kezelheti. Központi azonosítófelügyeleti itt egyetlen toomanage adatbázis-felhasználók és egyszerűbbé teszi a jogosultság kezelése. Előnyöket hello alábbiakat foglalja magába:

* Egy alternatív tooSQL kiszolgálóhitelesítés biztosít.
* Segít hello elterjedése tartozó felhasználói azonosítók leállítása adatbázis-kiszolgáló között.
* Lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen
* Az ügyfelek adatbázis-engedélyek külső (AAD) csoportok használatával kezelheti.
* Integrált Windows-hitelesítés és egyéb Azure Active Directory által támogatott hitelesítési engedélyezésével azt megnövelésével kiküszöbölheti jelszavak tárolását.
* Az Azure AD authentication használja a tartalmazott adatbázis felhasználók tooauthenticate identitások hello adatbázis szintjén.
* Az Azure AD Kapcsolódás adatbázis tooSQL alkalmazások jogkivonat-alapú hitelesítést támogatja.
* Az Azure AD authentication az ADFS (tartomány-összevonás) vagy a natív felhasználói/jelszó-hitelesítés támogat egy helyi Azure Active Directory tartományi szinkronizálás nélkül.  
* Az Azure AD-példányokhoz SQL Server Management Studio eszközt, amely az Active Directory univerzális hitelesítés használatára, amely magában foglalja a többtényezős hitelesítés (MFA).  Többtényezős hitelesítés tartalmazza, könnyen ellenőrzési lehetőségek széles erős hitelesítés – telefonhívás, szöveges üzenetet, intelligens kártyák PIN-kód és az értesítést a mobilalkalmazásban. További információkért lásd: [SSMS támogatása az Azure AD MFA az SQL-adatbázis és az SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).  

>  [!NOTE]  
>  Csatlakozás tooSQL-kiszolgáló egy Azure virtuális gépen nem támogatott az Azure Active Directory-fiókkal. A tartomány Active Directory-fiókot használni.  

hello konfigurációs lépések közé tartozik a következő eljárások tooconfigure hello és Azure Active Directory-hitelesítés használatára.

1. Létrehozása és feltöltése az Azure AD.
2. Választható lehetőség: Hozzárendelése vagy hello active directory jelenleg az Azure-előfizetéshez társított módosítása.
3. Hozzon létre egy Azure Active Directory-rendszergazda az Azure SQL server vagy [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. Állítsa be az ügyfélszámítógépen.
5. Felhasználók létrehozása a tartalmazott adatbázis az adatbázis leképezve tooAzure AD identitásokat.
6. Csatlakozás tooyour adatbázis az Azure AD-identitások segítségével.

> [!NOTE]
> toolearn hogyan toocreate és feltöltése az Azure AD és az Azure AD majd konfigurálása az Azure SQL Database és az SQL Data Warehouse, lásd: [konfigurálása az Azure SQL Database az Azure AD](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Bizalmi kapcsolat architektúrája
a következő magas szintű diagramját hello hello megoldásarchitektúra Azure AD-alapú hitelesítés használata az Azure SQL Database foglalja össze. hello koncepciók alkalmazása tooSQL Data warehouse-bA. az Azure AD saját felhasználói jelszót, csak hello felhő része, és az Azure AD vagy az Azure SQL Database toosupport tekinthető. toosupport külső hitelesítés (vagy/jelszó a Windows rendszerbeli hitelesítő adatokat), az AD FS blokk hello kommunikációra szükség. hello nyilak kommunikációs útvonallal jelzi.

![az aad-hitelesítés diagramja][1]

hello következő diagram azt jelzi, hello összevonási megbízhatósági és üzemeltetési kapcsolat, amelyben engedélyezte az ügyfél tooconnect tooa adatbázis által jogkivonat elküldése. hello token hitelesítése az Azure AD, és hello adatbázis megbízhatónak. Ügyfél 1 natív felhasználóival az Azure Active Directory vagy egy Azure AD az összevont felhasználók jelenthet. Ügyfél 2 jelöli egy lehetséges megoldás, beleértve az importált felhasználók; Ebben a példában egy összevont Azure Active Directory és az AD FS az Azure Active Directoryval szinkronizált származik. Fontos, az Azure AD-alapú hitelesítés tooa adatbázist elérő toounderstand kell lennie, adott előfizetés üzemeltető hello társított toohello az Azure AD. hello ugyanahhoz az előfizetéshez kell használt toocreate hello SQL Server üzemeltetési hello Azure SQL Database vagy az SQL Data Warehouse.

![előfizetés kapcsolat][2]

## <a name="administrator-structure"></a>Rendszergazda-struktúra
Az Azure AD-hitelesítést használ, amikor nincsenek rendszergazdai fiókok két hello SQL adatbázis-kiszolgáló; hello eredeti SQL Server-rendszergazdai és hello Azure AD-rendszergazda. hello koncepciók alkalmazása tooSQL Data warehouse-bA. Csak az Azure AD-fiókot alapján hello rendszergazda hozhat létre hello első Azure AD tartalmazott adatbázis-felhasználó a felhasználói adatbázisban. hello Azure AD rendszergazdai bejelentkezés egy Azure AD-felhasználó vagy egy Azure AD-csoport lehet. Ha hello rendszergazda csoportfiók, azt csoporttagot, lehetővé téve az SQL Server-példány hello több Azure AD-rendszergazdák által használható. Fiók használatával, mint a rendszergazda lehetővé teszik, hogy javítja a kezelhetőségi toocentrally hozzáadása, és távolítsa el a csoport tagjai az Azure AD hello felhasználók vagy az SQL-adatbázis engedélyek módosítása nélkül. Csak egy Azure AD-rendszergazda (egy felhasználó vagy csoport) bármikor konfigurálhatja.

![rendszergazda-struktúra][3]

## <a name="permissions"></a>Engedélyek
új felhasználók toocreate, rendelkeznie kell hello `ALTER ANY USER` hello adatbázis engedélyt. Hello `ALTER ANY USER` engedélyt kaphatnak tooany adatbázis-felhasználót. Hello `ALTER ANY USER` engedély is birtokában hello server-rendszergazdai fiók, és az adatbázis felhasználóinak hello `CONTROL ON DATABASE` vagy `ALTER ON DATABASE` engedéllyel az adatbázishoz tartozó, és hello tagjai `db_owner` adatbázis-szerepkör.

toocreate Azure SQL Database vagy az SQL Data Warehouse tartalmazott adatbázis-felhasználó, csatlakoznia kell egy Azure AD identity toohello adatbázist. toocreate hello első tartalmazott adatbázis-felhasználó, az Azure AD a rendszergazda (hello adatbázis tulajdonosa hello van) használatával kell csatlakoztatni toohello adatbázis. Ezt mutatják be a 4. és 5 az alábbi lépéseket. Minden Azure AD-alapú hitelesítés csak akkor lehetséges, ha hello Azure AD admin létrejött az Azure SQL Database vagy az SQL Data Warehouse-kiszolgálóhoz. Ha hello Azure Active Directory-rendszergazda hello kiszolgálóról el lett távolítva, korábban létrehozott SQL Server meglévő Azure Active Directory-felhasználók már nem kapcsolódhatnak toohello adatbázis az Azure Active Directory hitelesítő adataival.

## <a name="azure-ad-features-and-limitations"></a>Az Azure AD-funkciókat és korlátozások
a következő Azure AD tagjai hello Azure SQL server vagy az SQL Data Warehouse kiépítése:

* Natív tagok: tagja hozott létre az Azure AD hello által kezelt tartomány vagy egy felhasználói tartományban. További információkért lásd: [adja hozzá a saját tartomány neve tooAzure AD](../active-directory/active-directory-add-domain.md).
* Összevont tartományhoz: hozott létre az Azure AD egy összevont tartomány tagja. További információkért lásd: [Microsoft Azure mostantól támogatja a Windows Server Active Directory összevonási](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
* Importált tagjai más Azure AD-ből natív vagy összevont tartománytagok.
* Active Directory csoport biztonsági csoportok jönnek létre.

Microsoft-fiókok (például outlook.com, hotmail.com, live.com) vagy más vendégfiókok (például gmail.com, yahoo.com) nem támogatottak. Ha bejelentkezhet túl[https://login.live.com](https://login.live.com) hello fiókot és jelszót, majd a Microsoft-fiókot használ, amely használata nem támogatott az Azure AD-alapú hitelesítés az Azure SQL Database vagy az Azure SQL Data Warehouse.

## <a name="connecting-using-azure-ad-identities"></a>Csatlakozás az Azure AD-azonosítók használatával

Az Azure Active Directory hitelesítési módszerek az Azure AD identitásokkal csatlakozó tooa adatbázis a következő hello támogatja:

* Integrált Windows-hitelesítés használatával
* Az Azure AD egyszerű felhasználónév és jelszó használatával
* Alkalmazás-tokent használó hitelesítés használatával

### <a name="additional-considerations"></a>Néhány fontos megjegyzés

* tooenhance kezelhetőségi, ajánlott az egy dedikált az Azure AD kiépítése csoport rendszergazdaként.   
* Csak egy Azure AD-rendszergazda (egy felhasználó vagy csoport) bármikor konfigurálhatja az Azure SQL server vagy az Azure SQL Data warehouse-bA.   
* Csak az SQL Server az Azure AD rendszergazdája kezdetben kapcsolódhatnak toohello Azure SQL server vagy az Azure SQL Data Warehouse egy Azure Active Directory-fiókkal. hello Active Directory rendszergazda úgy is konfigurálhatja az Azure AD további adatbázis-felhasználók.   
* Javasoljuk, hello kapcsolat időkorlátja too30 másodpercben.   
* SQL Server 2016 Management Studio és az SQL Server Data Tools a Visual Studio 2015 (2016 vagy újabb verzió 14.0.60311.1April) támogatja az Azure Active Directory-hitelesítés. (Az azure AD-alapú hitelesítés támogatja hello **.NET Framework Data Provider – SqlServer**; legalább .NET-keretrendszer 4.6 verzióra). Ezért hello ezek az eszközök legújabb verzióra, és adatrétegbeli alkalmazások (DAC és .bacpac) az Azure AD-alapú hitelesítés használható.   
* [ODBC verziója 13.1](https://www.microsoft.com/download/details.aspx?id=53339) azonban támogatja az Azure Active Directory hitelesítési `bcp.exe` nem tud csatlakozni az Azure Active Directory-hitelesítés használatával, mert azt egy régebbi ODBC-szolgáltatóját használja.   
* `sqlcmd`támogatja az Azure Active Directory hitelesítési-tól kezdődően elérhető hello 13.1 verzió [letöltőközpontból](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server Data Tools a Visual Studio 2015 felhasználó számára legalább hello 2016. április verziója hello Data Tools (verzió: 14.0.60311.1). Az Azure Active Directory-felhasználók jelenleg nem láthatók az SSDT Object Explorerben. A probléma megoldásához hello felhasználók megtekintése [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
* [Microsoft JDBC illesztőprogram 6.0 az SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) támogatja az Azure AD hitelesítési. Lásd még [hello kapcsolati tulajdonságok beállítása](https://msdn.microsoft.com/library/ms378988.aspx).   
* A PolyBase nem tudják hitelesíteni magukat az Azure AD-hitelesítés használatával.   
* Az Azure AD-alapú hitelesítés van támogatja az SQL Database hello Azure-portálon **adatbázis importálása** és **adatbázis exportálása** paneleken. Importálás és exportálás az Azure AD-alapú hitelesítés használata is támogatott hello PowerShell-parancsot a.   
* Az Azure AD-alapú hitelesítés esetén támogatott SQL-adatbázis és az SQL Data Warehouse parancssori felület használatával. További információkért lásd: [konfigurálása és kezelése az Azure Active Directory-hitelesítés az SQL Database vagy az SQL Data Warehouse](sql-database-aad-authentication-configure.md) és [SQL Server - az sql server](https://docs.microsoft.com/en-us/cli/azure/sql/server).

## <a name="next-steps"></a>Következő lépések
- toolearn hogyan toocreate és feltöltése az Azure AD és az Azure AD majd konfigurálása az Azure SQL Database vagy az Azure SQL Data Warehouse, lásd: [konfigurálása és kezelése az Azure Active Directory-hitelesítés az SQL Database vagy az SQL Data Warehouse](sql-database-aad-authentication-configure.md).
- Az SQL Database hozzáféréseinek és felügyeletének áttekintéséről az [SQL Database-hozzáférés és -felügyelet](sql-database-control-access.md) részben olvashat.
- Az SQL Database bejelentkezéseinek, felhasználóinak és adatbázis-szerepköreinek áttekintését a [Bejelentkezések, felhasználók és adatbázis-szerepkörök](sql-database-manage-logins.md) részben találja.
- További információ az adatbázis résztvevőivel kapcsolatban: [Résztvevők](https://msdn.microsoft.com/library/ms181127.aspx).
- További információ az adatbázis-szerepkörökkel kapcsolatban: [Adatbázis-szerepkörök](https://msdn.microsoft.com/library/ms189121.aspx).
- További információ az SQL Database tűzfalszabályaival kapcsolatban: [SQL Database tűzfalszabályok](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

