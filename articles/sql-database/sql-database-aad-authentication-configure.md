---
title: "aaaConfigure Azure Active Directory-hitelesítés - SQL |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooSQL adatbázis és az SQL Data Warehouse Azure Active Directory-hitelesítés használatával."
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
ms.date: 07/10/2017
ms.author: rickbyh
ms.openlocfilehash: d6222da0b840f96d4bcfbc02964dc7c54d5ea1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql-database-or-sql-data-warehouse"></a>Konfigurálhatja és kezelheti az Azure Active Directory-hitelesítés az SQL Database vagy az SQL Data Warehouse

Ez a cikk bemutatja, hogyan toocreate feltöltése az Azure AD és az Azure AD az Azure SQL Database és az SQL Data Warehouse majd használni. Megtudhatja, [Azure Active Directory-hitelesítéssel](sql-database-aad-authentication.md).

>  [!NOTE]  
>  Csatlakozás tooSQL-kiszolgáló egy Azure virtuális gépen nem támogatott az Azure Active Directory-fiókkal. A tartomány Active Directory-fiókot használni.

## <a name="create-and-populate-an-azure-ad"></a>Létrehozása és feltöltése az Azure AD
Hozzon létre egy Azure AD és a felhasználókat és csoportokat. Az Azure AD hello kezdeti tartomány az Azure AD által kezelt tartomány lehet. Az Azure AD egy a helyszíni Active Directory tartományi szolgáltatások, amelyek össze van vonva az Azure AD hello is lehet.

További információkért lásd: [a helyszíni identitások integrálása az Azure Active Directoryval](../active-directory/active-directory-aadconnect.md), [adja hozzá a saját tartomány neve tooAzure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure mostantól támogatja az összevonási Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [az Azure AD-címtár felügyelete](https://msdn.microsoft.com/library/azure/hh967611.aspx), [kezelése Windows PowerShell használatával az Azure AD](/powershell/azure/overview?view=azureadps-2.0), és [hibrid identitás Szükséges portok és protokollok](../active-directory/active-directory-aadconnect-ports.md).

## <a name="optional-associate-or-change-hello-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>Választható lehetőség: Hozzárendelése vagy hello active directory jelenleg az Azure-előfizetéshez társított módosítása
tooassociate hello Azure Active Directory a szervezeténél, az adatbázis ellenőrizze hello directory megbízható könyvtárat hello Azure-előfizetés üzemeltetési hello adatbázis számára. További információkért lásd: [How Azure subscriptions are associated with Azure AD?](https://msdn.microsoft.com/library/azure/dn629581.aspx) (Hogyan kapcsolódnak az Azure-előfizetések az Azure AD-hoz?).

**További információ:** minden Azure-előfizetéshez az Azure AD-példányban megbízhatósági kapcsolatban áll. Ez azt jelenti, hogy megbízik, hogy directory tooauthenticate felhasználók, szolgáltatások és eszközök. Több előfizetés is megbízhat hello ugyanabban a könyvtárban, de egy előfizetéshez csak egy címtárban bízhat. Láthatja, melyik címtárban bízik meg az előfizetése hello **beállítások** lapot [https://manage.windowsazure.com/](https://manage.windowsazure.com/). A bizalmi kapcsolat, amely egy előfizetés tartozik egy könyvtárat eltérően előfizetéssel rendelkező összes többi erőforrása (webhelyek, adatbázisok és így tovább), Azure előfizetés, mint amelyek hello kapcsolat van. Ha egy előfizetés lejár, majd elérni toothose más hello előfizetéshez társított erőforrásokat is leáll. De hello címtár az Azure-ban marad, és más előfizetéseket társíthat a könyvtárhoz, és továbbra is toomanage hello directory-felhasználók. Erőforrások kapcsolatos további információkért lásd: [az az Azure-erőforrások hozzáférésének megismerése](https://msdn.microsoft.com/library/azure/dn584083.aspx).

hello következő eljárások bemutatják, hogyan toochange hello kapcsolódó egy adott előfizetéshez tartozó.
1. Csatlakozás tooyour [klasszikus Azure portál](https://manage.windowsazure.com/) Azure-előfizetési rendszergazda segítségével.
2. Hello bal szalagcím, válassza ki a **beállítások**.
3. Az előfizetések hello beállítások képernyő jelenik meg. Ha hello szükségeskonfiguráció-előfizetés nem jelenik meg, kattintson a **előfizetések** hello felső legördülő listán hello **szűrő által DIRECTORY** mezőbe, és válassza ki, amely tartalmazza az előfizetések hello könyvtárat, majd kattintson **ALKALMAZ**.
   
    ![Válassza ki az előfizetést][4]
4. A hello **beállítások** területen kattintson az előfizetéshez, majd **könyvtár szerkesztése** hello lap hello alján.
   
    ![ad-beállítások-portálon][5]
5. A hello **könyvtár szerkesztése** mezőben, válassza ki a hello Azure Active Directoryban, amely az SQL Server vagy az SQL Data Warehouse van társítva, majd kattintson a Tovább nyílra hello.
   
    ![Edit directory jelölje ki][6]
6. A hello **megerősítése** directory leképezési párbeszédpanelen ellenőrizze, hogy "**összes társrendszergazdák törlődni fog.**"
   
    ![Edit directory megerősítése][7]
7. Kattintson a hello ellenőrzés tooreload hello portálon.

   > [!NOTE]
   > Ha hello könyvtár, tooall közös rendszergazdák, az Azure Active Directory-felhasználók és csoportok, módosíthatja és könyvtár biztonsági erőforrás-felhasználókat törlődnek, és már nem rendelkezik hozzáféréssel toothis előfizetés vagy az erőforrások. Csak, szolgáltatás-rendszergazdaként, konfigurálja a hozzáférést a rendszerbiztonsági tagoknak hello új könyvtár alapján. Ez a változás jelentős mennyiségű időt toopropagate tooall erőforrásokat is igénybe vehet. Hello directory módosítása, is módosítások SQL-adatbázis és az SQL Data Warehouse hello Azure AD-rendszergazda és a meglévő Azure Active Directory-felhasználók adatbázis-hozzáférés letiltása. az Azure AD Üdvözöljük a rendszergazdákat kell lennie (alább leírtak) alaphelyzetbe állítása és az új Azure AD-felhasználók kell létrehozni.
   >  

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Az Azure AD-rendszergazda Azure SQL-kiszolgáló létrehozása
Minden Azure SQL-kiszolgáló (amely egy SQL Database vagy az SQL Data Warehouse) egy önálló kiszolgáló-rendszergazdai fiók hello teljes Azure SQL server rendszergazdájának hello kezdődik. A második SQL kiszolgáló rendszergazdája kell létrehozni, amely egy Azure AD-fiókot. Tartalmazott adatbázis-felhasználó rendszerbiztonsági hello master adatbázisban jön létre. A rendszergazdáknak, hello server-rendszergazdai fiók tagjai hello **db_owner** minden felhasználói szerepkör adatbázisából, és adjon meg minden egyes felhasználói adatbázis hello **dbo** felhasználó. Hello kiszolgáló rendszergazdai fiókokkal kapcsolatos további információkért lásd: [kezelése adatbázisok és bejelentkezések az Azure SQL Database](sql-database-manage-logins.md).

Ha az Azure Active Directoryval a georeplikációt, hello Azure Active Directory-rendszergazda hello elsődleges és másodlagos kiszolgálók hello kell konfigurálni. Ha a kiszolgáló nem rendelkezik egy Azure Active Directory-rendszergazda, majd Azure Active Directory bejelentkezési és a felhasználók "Nem lehet csatlakozni" tooserver hibaüzenetet kap.

> [!NOTE]
> Az Azure AD-fiókot (beleértve a hello Azure SQL server-rendszergazdai fiók), nem alapuló felhasználók nem hozható létre az Azure AD-alapú felhasználók, mert nem rendelkeznek engedéllyel toovalidate hello Azure AD az adatbázis-felhasználók javasolt.
> 

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server"></a>Az Azure SQL server Azure Active Directory-rendszergazda kiépítése

a következő két eljárás hello mutatja be az Azure SQL server hello Azure-portálon, és a PowerShell használatával Azure Active Directory-rendszergazda tooprovision.

### <a name="azure-portal"></a>Azure Portal
1. A hello [Azure-portálon](https://portal.azure.com/), a hello jobb felső sarkában, ha a kapcsolat toodrop vonatkozó esetleges aktív könyvtárainak listáját. Válassza ki a hello javítsa ki az Active Directory az Azure AD hello alapértelmezettként. E lépés hivatkozások hello előfizetés hozzárendelését a Active Directory Azure SQL-kiszolgálóval, annak biztosítása, hogy hello ugyanahhoz az előfizetéshez szolgál, mind az Azure AD és az SQL Server. (hello Azure SQL-kiszolgálót is szolgáltató vagy az Azure SQL Database vagy az Azure SQL Data Warehouse.)   
    ![Válasszon ad][8]   
    
2. Hello balra szalagcím válassza ki a **SQL Server-kiszolgálók**, jelölje be a **SQL server**, majd a hello **SQL Server** panelen kattintson **Active Directory-rendszergazda**.   
3. A hello **Active Directory-rendszergazda** panelen kattintson a **-rendszergazda beállítása**.   
    ![az active directory kiválasztása](./media/sql-database-aad-authentication/select-active-directory.png)  
    
4. A hello **adja hozzá a rendszergazda** panelen keresse meg a felhasználó, jelölje be hello felhasználó vagy csoport toobe rendszergazdaként, és kattintson a **válasszon**. (hello Active Directory felügyeleti panelt jeleníti meg minden tagok és Active Directory-csoport. Felhasználók vagy csoportok használhatók, nem állítható be, mert nem támogatják az Azure AD rendszergazdai szerepkörrel. (A hello támogatott rendszergazdák hello tartalmazó lista **az Azure AD-funkciókat és korlátozások** szakasza [Azure Active Directory hitelesítés használata a hitelesítés és az SQL Database vagy az SQL Data Warehouse](sql-database-aad-authentication.md).) Szerepköralapú hozzáférés-vezérlést (RBAC) csak toohello portál vonatkozik, és nincs propagált tooSQL kiszolgáló.   
    ![Válassza ki a rendszergazda](./media/sql-database-aad-authentication/select-admin.png)  
    
5. Hello hello tetején **Active Directory-rendszergazda** panelen kattintson a **mentése**.   
    ![Mentse admin](./media/sql-database-aad-authentication/save-admin.png)   

hello folyamatát hello rendszergazda eltarthat néhány percig. Új rendszergazda hello hello jelenik meg, majd **Active Directory-rendszergazda** mezőbe.

   > [!NOTE]
   > Az Azure AD Üdvözöljük a rendszergazdákat beállításakor hello új felügyeleti neve (felhasználó vagy csoport) nem már megtalálható hello virtuális master adatbázis SQL Server hitelesítési felhasználóként. Ha van ilyen, a hello Azure AD rendszergazdai telepítés sikertelen lesz; visszaállítása a létrehozása és, amely jelzi, hogy az ilyen rendszergazda (név) már létezik. Mivel ilyen egy SQL Server-hitelesítés felhasználói része nem hello Azure AD, bármely elérhető tooconnect toohello kiszolgáló használata az Azure AD-alapú hitelesítés sikertelen lesz.
   > 


toolater hello hello tetején, hogy egy rendszergazda eltávolítása **Active Directory-rendszergazda** panelen kattintson a **távolítsa el a felügyeleti**, és kattintson a **mentése**.

### <a name="powershell"></a>PowerShell
toorun PowerShell-parancsmagok, meg kell toohave Azure PowerShell telepítése és futtatása. Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

az Azure AD rendszergazdai tooprovision hello Azure PowerShell-parancsok a következő hajtható végre:

* Add-AzureRmAccount
* SELECT-AzureRmSubscription

Parancsmagok tooprovision használt, és az Azure AD admin kezelése:

| Parancsmag neve | Leírás |
| --- | --- |
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/set-azurermsqlserveractivedirectoryadministrator) |Azure Active Directory-rendszergazda Azure SQL server vagy az Azure SQL Data Warehouse kiépítése. (Kell lennie a jelenlegi előfizetés hello.) |
| [Remove-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/remove-azurermsqlserveractivedirectoryadministrator) |Eltávolítja az Azure Active Directory-rendszergazda Azure SQL server vagy az Azure SQL Data warehouse-bA. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator) |A jelenleg konfigurált hello Azure SQL server vagy az Azure SQL Data Warehouse Azure Active Directory-rendszergazda információt ad vissza. |

Használhatja a PowerShell-parancs get-help toosee több adatai ezen parancsok mindegyike például ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

parancsfájl kiosztja az Azure AD felügyeleti csoport neve a következő hello **DBA_Group** (objektumazonosító: `40b79501-b343-44ed-9ce7-da4c8cc7353f`) a hello **demo_server** nevű erőforráscsoportban server **csoport – 23** :

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

Hello **DisplayName** bemeneti paraméter fogad el, vagy az Azure AD hello megjelenített név, vagy hello egyszerű felhasználónév. Például ``DisplayName="John Smith"`` és ``DisplayName="johns@contoso.com"``. Csak az Azure AD-csoportok hello Azure AD megjelenített név támogatott.

> [!NOTE]
> hello Azure PowerShell-paranccsal ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` nem akadályozza meg a felhasználók nem támogatott az Azure AD-rendszergazdái kiosztásakor. Egy nem támogatott felhasználói telepíthető, de nem lehet csatlakozni a tooa adatbázis. 
> 

hello alábbi példában választható hello **ObjectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> az Azure AD hello **ObjectID** szükség, amikor hello **DisplayName** eleme nem egyedi. tooretrieve hello **ObjectID** és **DisplayName** értékek, hello Active Directory szakaszban a klasszikus Azure portálon, és egy felhasználó vagy csoport hello tulajdonságainak megtekintése.
> 

a következő példa hello Azure SQL-kiszolgáló aktuális az Azure AD Üdvözöljük a rendszergazdákat információt ad vissza:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

a következő példa hello Azure AD-rendszergazdaként eltávolítja:

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

Egy Azure Active Directory rendszergazdai hello REST API-k használatával is kiépíthetők. További információkért lásd: [az Azure SQL-adatbázisok Azure SQL-adatbázisok műveletek műveletek és a Service Management REST API-referencia](https://msdn.microsoft.com/library/azure/dn505719.aspx)

### <a name="cli"></a>parancssori felület  
Az Azure AD-rendszergazda által a következő parancssori felület parancsait hívó hello is kiépíthetők:
| Parancs | Leírás |
| --- | --- |
|[az sql server ad-rendszergazda létrehozása](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#create) |Azure Active Directory-rendszergazda Azure SQL server vagy az Azure SQL Data Warehouse kiépítése. (Kell lennie a jelenlegi előfizetés hello.) |
|[az sql server ad-rendszergazda törlése](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#delete) |Eltávolítja az Azure Active Directory-rendszergazda Azure SQL server vagy az Azure SQL Data warehouse-bA. |
|[az sql server ad-rendszergazda listája](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#list) |A jelenleg konfigurált hello Azure SQL server vagy az Azure SQL Data Warehouse Azure Active Directory-rendszergazda információt ad vissza. |
|[az sql server ad-rendszergazda frissítése](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#update) |Hello Active Directory-rendszergazda az Azure SQL server vagy az Azure SQL Data Warehouse frissíti. |

Parancssori felület parancsait kapcsolatos további információkért lásd: [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  


## <a name="configure-your-client-computers"></a>Állítsa be az ügyfélszámítógépen
Az összes ügyfélszámítógépre, amelyről alkalmazás vagy felhasználó csatlakozás tooAzure SQL Database vagy az Azure SQL Data Warehouse az Azure AD identitásokkal, telepítenie kell a következő szoftver hello:

* .NET-keretrendszer 4.6-os vagy újabb a [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* Az Azure Active Directory hitelesítési tár SQL Serverhez (**ADALSQL. DLL**) több nyelven is elérhető (x86 és amd64) hello letöltőközpontján [Microsoft Active Directory Authentication Library Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

E követelményeknek szerint:

* Telepítése vagy [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) vagy [SQL Server Data Tools a Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) megfelel-e a .NET-keretrendszer 4.6 követelmény hello.
* SSMS hello x86 verzióját telepíti **ADALSQL. DLL**.
* Az SSDT hello AMD64-es verzióját telepíti **ADALSQL. DLL**.
* hello a legújabb Visual Studio [Visual Studio letöltések](https://www.visualstudio.com/downloads/download-visual-studio-vs) megfelel-e a .NET-keretrendszer 4.6 hello követelménynek, de nem telepíti az hello szükséges AMD64-es verziójának **ADALSQL. DLL**.

## <a name="create-contained-database-users-in-your-database-mapped-tooazure-ad-identities"></a>Felhasználók létrehozása a tartalmazott adatbázis az adatbázis leképezve tooAzure AD identitások

Az Azure Active Directory-hitelesítés szükséges adatbázis felhasználók toobe tartalmazott adatbázis-felhasználók létre. Az Azure AD identity alapján tartalmazott adatbázis-felhasználó az adatbázis-felhasználó, amely nem rendelkezik olyan bejelentkezési hello master adatbázisban, és melyik maps tooan identitása az Azure AD-címtár hello adatbázis társított hello. az Azure AD identity hello vagy az egyéni felhasználói fiókot, vagy a csoport lehet. További információ a tartalmazott adatbázis-felhasználók: [tartalmazott adatbázis-felhasználók – hogy a saját adatbázis hordozható](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Adatbázis-felhasználók (kivétellel hello rendszergazdák) portal nem hozható létre. Az RBAC-szerepkörök nem kiszolgáló, az SQL Database vagy az SQL Data Warehouse propagált tooSQL. Az Azure RBAC-szerepkörök Azure-erőforrások kezelésére használt, és toodatabase engedélyek alkalmazásának kihagyása. Például hello **SQL Server közreműködői** szerepkör nem biztosít hozzáférést tooconnect toohello SQL Database vagy az SQL Data Warehouse. közvetlenül a Transact-SQL-utasítások segítségével hello adatbázis hello hozzáférési engedélyt kell adni.
>

az Azure AD-alapú tartalmazott adatbázis (nem hello kiszolgálói rendszergazda felhasználók hello adatbázis birtokló), toohello adatbázis kapcsolódás az Azure AD identitással rendelkező felhasználóként toocreate legalább hello **bármely felhasználó ALTER** engedéllyel. Kövesse a következő Transact-SQL-szintaxis hello:

```
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* hello egyszerű felhasználónevéhez az Azure AD felhasználói vagy hello megjelenített névbe egy Azure AD-csoport lehet.

**Példák:** toocreate az Azure AD közti tartalmazott adatbázis-felhasználó összevont vagy kezelt tartományi felhasználói:
```
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

toocreate tartalmazott adatbázis-felhasználó az Azure AD közti vagy összevont tartományi csoport, adja meg egy biztonsági csoport hello megjelenített neve:
```
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

egy alkalmazás, amely összeköti az Azure AD token használatával képviselő tartalmazott adatbázis-felhasználó toocreate:

```
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

>  [!TIP]
>  A felhasználó közvetlenül az Azure Active Directory eltérő hello Azure Active Directory, az Azure-előfizetéshez társított nem készíteni. Azonban más aktív könyvtárakat a hello az importált felhasználók tagjai társított Active Directory (külső felhasználók úgynevezett) felveheti tooan Active Directory-csoport hello bérlői Active Directory. Hozzon létre egy tartalmazott adatbázis-felhasználót, hogy AD-csoport, hello hello felhasználók külső Active Directory kaphatnak a hozzáférési tooSQL adatbázis.   

Tartalmazott létrehozásával kapcsolatos további információért adatbázis-felhasználók Azure Active Directory identitások alapján című [felhasználó létrehozása (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Az Azure SQL server eltávolításakor hello Azure Active Directory rendszergazdája megakadályozza, hogy az Azure AD hitelesítési munkamenetből toohello kiszolgálóhoz csatlakozó. Ha szükséges, nem használható az Azure Active Directory-felhasználók manuálisan SQL adatbázis-rendszergazda általi törölhetők.   

>  [!NOTE]
>  Ha megjelenik egy **kapcsolat időkorlátja lejárt**, szükség lehet a tooset hello `TransparentNetworkIPResolution` hello kapcsolati karakterlánc toofalse paramétere. További információkért lásd: [kapcsolat időtúllépési problémát .NET-keretrendszer 4.6.1 - TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/2016/05/07/connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).   

   
Amikor létrehoz egy adatbázis-felhasználó, a felhasználó kap-e a hello **CONNECT** engedély és csatlakozni tud toothat adatbázis hello tagjaként **nyilvános** szerepkör. Csak a rendelkezésre álló toohello felhasználói bármely engedélyek toohello engedélyek kezdetben hello **nyilvános** szerepkör, vagy azokat az engedélyeket megadta tooany Windows-csoportok tagjai legyenek. Miután kiépíteni az Azure AD-alapú tartalmazott adatbázis-felhasználó, hello felhasználói további engedélyek megadásához hello ugyanaz, mint adjon engedélye tooany felhasználó más típusú. Általában engedélyeket toodatabase szerepköröket, és adja hozzá a felhasználók tooroles. További információkért lásd: [Database Engine engedély alapjai](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). További információ a speciális SQL adatbázis-szerepkörök: [kezelése adatbázisok és bejelentkezések az Azure SQL Database](sql-database-manage-logins.md).
A kezelés tartományába importált összevont tartományi felhasználó hello felügyelt tartomány identitása kell használnia.

> [!NOTE]
> Az Azure Active Directory-felhasználók jelöli az adatbázis-metaadatok hello típusú E (EXTERNAL_USER) és a csoportok X (EXTERNAL_GROUPS) típus. További információkért lásd: [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx). 
>

## <a name="connect-toohello-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>Csatlakozás toohello felhasználói adatbázis vagy adatraktár SSMS vagy az SSDT használatával  
tooconfirm hello Azure AD rendszergazdai helyesen van beállítva, a csatlakozás toohello **fő** adatbázis hello Azure AD rendszergazdai fiók használatával.
tooprovision egy Azure AD-alapú tartalmazott adatbázis (nem hello kiszolgálói rendszergazda felhasználók hello adatbázis birtokló), csatlakozás toohello adatbázis egy Azure AD identitás, amely rendelkezik hozzáférési toohello adatbázis.

> [!IMPORTANT]
> Az Azure Active Directory-hitelesítés támogatásának érhető [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) a Visual Studio 2015-öt. 2016 augusztusától kiadás az SSMS hello is támogatja az Active Directory univerzális hitelesítést, amely lehetővé teszi a rendszergazdák toorequire multi-factor Authentication használatával telefonhívással, szöveges üzenetet, az intelligens kártyák PIN-kódot, vagy a mobilalkalmazáson keresztüli értesítések.
 
## <a name="using-an-azure-ad-identity-tooconnect-using-ssms-or-ssdt"></a>Az Azure AD identity tooconnect SSMS vagy az SSDT használatával  

hello a következő eljárások bemutatják, hogyan tooconnect tooa SQL-adatbázis egy SQL Server Management Studio vagy SQL Server adatbázis-eszközt használ az Azure AD-identitás.

### <a name="active-directory-integrated-authentication"></a>Active Directory integrált hitelesítést

Ezt a módszert használja, ha tooWindows összevont tartományhoz az Azure Active Directory hitelesítő adatokkal van bejelentkezve.

1. Indítsa el a Management Studio vagy az eszközök és hello **tooServer csatlakozás** (vagy **tooDatabase motor csatlakozás**) párbeszédpanelen a hello **hitelesítési** Jelöljeki**Active Directory - integrált**. Nem kell jelszót van szükség, vagy megadható, mert a meglévő hitelesítő adatok számára jelenik meg a hello kapcsolat.   

    ![AD integrált hitelesítés kiválasztása][11]
2. Hello kattintson **beállítások** gombra, és a hello **kapcsolat tulajdonságai** lap hello **toodatabase csatlakozás** mezőbe, írja be a hello nevet hello felhasználói adatbázis tooconnect kívánt a. (hello **AD tartomány neve vagy a bérlői azonosító**"beállítás csak a támogatott **univerzális MFA kapcsolattal rendelkező** beállítások, ellenkező esetben azt szürkén jelenik meg.)  

    ![Válassza ki a hello adatbázis neve][13]

## <a name="active-directory-password-authentication"></a>Active Directory jelszavas hitelesítést

Ezt a módszert használja, egy Azure AD egyszerű felhasználónév használata az Azure AD hello összekapcsolása felügyelt tartomány. Is használhatja az összevont fiók hozzáférési toohello tartomány, például amikor távolról használata nélkül.

Ezt a módszert használja, ha tooWindows egy nem összevont Azure-ral tartományi hitelesítő adatokkal van bejelentkezve, vagy az Azure AD használatával használata esetén az Azure AD kezdeti hello alapján, vagy hello ügyfél tartomány.

1. Indítsa el a Management Studio vagy az eszközök és hello **tooServer csatlakozás** (vagy **tooDatabase motor csatlakozás**) párbeszédpanelen a hello **hitelesítési** Jelöljeki**Active Directory - jelszó**.
2. A hello **felhasználónév** hello formátumban írja be az Azure Active Directory felhasználónév  **username@domain.com** . Ez az hello Azure Active Directory-fiók vagy hello Azure Active Directory összevonni az olyan tartományi fiók.
3. A hello **jelszó** mezőben adja meg a felhasználói jelszó hello Azure Active Directory-fiókkal vagy tartományi fiók összevont.

    ![Válassza ki az AD-jelszó-hitelesítés][12]
4. Hello kattintson **beállítások** gombra, és a hello **kapcsolat tulajdonságai** lap hello **toodatabase csatlakozás** mezőbe, írja be a hello nevet hello felhasználói adatbázis tooconnect kívánt a. (Lásd az előző beállítás hello hello ábra.)

## <a name="using-an-azure-ad-identity-tooconnect-from-a-client-application"></a>Az Azure AD identity tooconnect a ügyfélalkalmazás használata

hello a következő eljárások bemutatják, hogyan tooconnect tooa SQL adatbázis-e a ügyfélalkalmazás Azure AD identitással.

###  <a name="active-directory-integrated-authentication"></a>Active Directory integrált hitelesítést

toouse integrált Windows-hitelesítés, a tartomány Active Directory Azure Active Directoryval összevont kell lennie. Csatlakozás toohello adatbázis az ügyfélalkalmazást (vagy egy szolgáltatás) a felhasználó tartományi hitelesítő adatok a tartományhoz csatlakoztatott számítógépen kell futnia.

hitelesítés és az Azure AD identity tooconnect tooa adatbázis használatával integrált, tooActive Directory integrált hello hitelesítési kulcsszót hello adatbázis-kapcsolati karakterláncot kell beállítani. hello alábbi C# kódminta használ ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

kapcsolati karakterlánc kulcsszó hello ``Integrated Security=True`` tooAzure SQL-adatbázis csatlakoztatása nem támogatott. Ha egy ODBC-kapcsolat, először tooremove szóközt kell, állítsa be a hitelesítési too'ActiveDirectoryIntegrated ".

### <a name="active-directory-password-authentication"></a>Active Directory jelszavas hitelesítést

hitelesítés és az Azure AD identity tooconnect tooa adatbázis használatával integrált, hello hitelesítési kulcsszó tooActive könyvtárhoz tartozó jelszóval kell beállítani. hello kapcsolati karakterláncnak tartalmaznia kell a felhasználói azonosító vagy UID és a jelszó/PWD kulcsszavak és az értékeket. hello alábbi C# kódminta használ ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

További tudnivalók az Azure AD hitelesítési módszerek használatával hello bemutató mintakódok elérhető [GitHub bemutató az Azure AD hitelesítési](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Az Azure AD-jogkivonat
Ez a hitelesítési módszer középső rétegbeli szolgáltatások tooconnect tooAzure SQL Database vagy az Azure SQL Data Warehouse lehetővé teszi a jogkivonat beszerzése az Azure Active Directory (AAD). Ez lehetővé teszi a kifinomult forgatókönyvek például a tanúsítvány alapú hitelesítés. Négy alapvető lépéseket toouse az Azure AD jogkivonat hitelesítési kell elvégeznie:

1. Az alkalmazás regisztrálása az Azure Active Directoryban, és hello ügyfél-azonosító lekérése a kódot. 
2. Hozzon létre egy adatbázis felhasználói képviselő hello alkalmazást. (A korábbi 6. lépés befejeződött.)
3. Hozzon létre egy tanúsítványt a hello ügyfél számítógép futtatása hello alkalmazás.
4. Hello tanúsítvány hozzáadása az alkalmazáshoz kulcsként.

A minta kapcsolati karakterlánc:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

További információkért lásd: [SQL Server biztonsági Blog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="sqlcmd"></a>sqlcmd

hello utasításokat követve, csatlakozni az Sqlcmd használatával, amely elérhető a hello 13.1 verzióját használja [letöltőközpontból](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Következő lépések
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
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png

