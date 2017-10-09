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
# <a name="authentication-tooazure-sql-data-warehouse"></a><span data-ttu-id="7ba94-103">Az SQL Data Warehouse hitelesítési tooAzure</span><span class="sxs-lookup"><span data-stu-id="7ba94-103">Authentication tooAzure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7ba94-104">Biztonság – áttekintés</span><span class="sxs-lookup"><span data-stu-id="7ba94-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="7ba94-105">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7ba94-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="7ba94-106">Titkosítás (portál)</span><span class="sxs-lookup"><span data-stu-id="7ba94-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="7ba94-107">Titkosítás (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="7ba94-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="7ba94-108">tooconnect tooSQL Data warehouse-ba, át kell adnia a hitelesítő adatokat hitelesítési célokra.</span><span class="sxs-lookup"><span data-stu-id="7ba94-108">tooconnect tooSQL Data Warehouse, you must pass in security credentials for authentication purposes.</span></span> <span data-ttu-id="7ba94-109">A kapcsolat létrehozása után egyes kapcsolat beállításai a lekérdezés munkamenet létrehozásának részeként.</span><span class="sxs-lookup"><span data-stu-id="7ba94-109">Upon establishing a connection, certain connection settings are configured as part of establishing your query session.</span></span>  

<span data-ttu-id="7ba94-110">További információ a biztonsági és hogyan tooenable kapcsolatok tooyour adatraktár, lásd: [az SQL Data Warehouse adatbázis védelme][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="7ba94-110">For more information on security and how tooenable connections tooyour data warehouse, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>

## <a name="sql-authentication"></a><span data-ttu-id="7ba94-111">SQL-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7ba94-111">SQL authentication</span></span>
<span data-ttu-id="7ba94-112">tooconnect tooSQL Data warehouse-ba, meg kell adnia a következő információk hello:</span><span class="sxs-lookup"><span data-stu-id="7ba94-112">tooconnect tooSQL Data Warehouse, you must provide hello following information:</span></span>

* <span data-ttu-id="7ba94-113">Teljes kiszolgálónév</span><span class="sxs-lookup"><span data-stu-id="7ba94-113">Fully qualified servername</span></span>
* <span data-ttu-id="7ba94-114">Adja meg az SQL-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7ba94-114">Specify SQL authentication</span></span>
* <span data-ttu-id="7ba94-115">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="7ba94-115">Username</span></span>
* <span data-ttu-id="7ba94-116">Jelszó</span><span class="sxs-lookup"><span data-stu-id="7ba94-116">Password</span></span>
* <span data-ttu-id="7ba94-117">Alapértelmezett adatbázis (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="7ba94-117">Default database (optional)</span></span>

<span data-ttu-id="7ba94-118">Alapértelmezés szerint kapcsolódás toohello *fő* és nem a felhasználói adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7ba94-118">By default your connection connects toohello *master* database and not your user database.</span></span> <span data-ttu-id="7ba94-119">tooconnect tooyour felhasználói adatbázishoz, választhat toodo két dolog egyikét:</span><span class="sxs-lookup"><span data-stu-id="7ba94-119">tooconnect tooyour user database, you can choose toodo one of two things:</span></span>

* <span data-ttu-id="7ba94-120">Adjon meg hello alapértelmezett adatbázis, amikor a kiszolgáló regisztrálása a hello SQL Server Object Explorer SSDT, SSMS, vagy az alkalmazás kapcsolódási karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="7ba94-120">Specify hello default database when registering your server with hello SQL Server Object Explorer in SSDT, SSMS, or in your application connection string.</span></span> <span data-ttu-id="7ba94-121">Például a hello InitialCatalog paraméter az ODBC-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="7ba94-121">For example, include hello InitialCatalog parameter for an ODBC connection.</span></span>
* <span data-ttu-id="7ba94-122">Jelölje ki a felhasználói adatbázis hello SSDT-ben a munkamenet létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="7ba94-122">Highlight hello user database before creating a session in SSDT.</span></span>

> [!NOTE]
> <span data-ttu-id="7ba94-123">a Transact-SQL hello **használata adatbázis;** hello adatbázis kapcsolat módosítása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7ba94-123">hello Transact-SQL statement **USE MyDatabase;** is not supported for changing hello database for a connection.</span></span> <span data-ttu-id="7ba94-124">Csatlakozás az adatraktár tooSQL és az SSDT együttes útmutatásért tekintse meg a toohello [lekérdezése a Visual Studio] [ Query with Visual Studio] cikk.</span><span class="sxs-lookup"><span data-stu-id="7ba94-124">For guidance connecting tooSQL Data Warehouse with SSDT, refer toohello [Query with Visual Studio][Query with Visual Studio] article.</span></span>
> 
> 

## <a name="azure-active-directory-aad-authentication"></a><span data-ttu-id="7ba94-125">Az Azure Active Directory (AAD) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7ba94-125">Azure Active Directory (AAD) authentication</span></span>
<span data-ttu-id="7ba94-126">[Az Azure Active Directory] [ What is Azure Active Directory] hitelesítési egy olyan mechanizmus, amely tooMicrosoft Azure SQL Data Warehouse identitások az Azure Active Directory (Azure AD) segítségével.</span><span class="sxs-lookup"><span data-stu-id="7ba94-126">[Azure Active Directory][What is Azure Active Directory] authentication is a mechanism of connecting tooMicrosoft Azure SQL Data Warehouse by using identities in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="7ba94-127">Azure Active Directory-hitelesítéssel központilag kezelheti hello identitások az adatbázis-felhasználók és más Microsoft-szolgáltatásokban egyetlen központi helyen.</span><span class="sxs-lookup"><span data-stu-id="7ba94-127">With Azure Active Directory authentication, you can centrally manage hello identities of database users and other Microsoft services in one central location.</span></span> <span data-ttu-id="7ba94-128">Központi azonosítófelügyeleti biztosít egy helyen toomanage SQL Data Warehouse felhasználók számára, és egyszerűbbé teszi a jogosultság kezelése.</span><span class="sxs-lookup"><span data-stu-id="7ba94-128">Central ID management provides a single place toomanage SQL Data Warehouse users and simplifies permission management.</span></span> 

### <a name="benefits"></a><span data-ttu-id="7ba94-129">Előnyök</span><span class="sxs-lookup"><span data-stu-id="7ba94-129">Benefits</span></span>
<span data-ttu-id="7ba94-130">Az Azure Active Directory előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="7ba94-130">Azure Active Directory benefits include:</span></span>

* <span data-ttu-id="7ba94-131">Egy alternatív tooSQL kiszolgáló hitelesítést nyújt.</span><span class="sxs-lookup"><span data-stu-id="7ba94-131">Provides an alternative tooSQL Server authentication.</span></span>
* <span data-ttu-id="7ba94-132">Segít hello elterjedése tartozó felhasználói azonosítók leállítása adatbázis-kiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="7ba94-132">Helps stop hello proliferation of user identities across database servers.</span></span>
* <span data-ttu-id="7ba94-133">Lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen</span><span class="sxs-lookup"><span data-stu-id="7ba94-133">Allows password rotation in a single place</span></span>
* <span data-ttu-id="7ba94-134">Adatbázis-engedélyek a külső (AAD) csoportok kezelése.</span><span class="sxs-lookup"><span data-stu-id="7ba94-134">Manage database permissions using external (AAD) groups.</span></span>
* <span data-ttu-id="7ba94-135">Jelszavak tárolását megszünteti az integrált Windows-hitelesítés és egyéb Azure Active Directory által támogatott hitelesítési engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="7ba94-135">Eliminates storing passwords by enabling integrated Windows authentication and other forms of authentication supported by Azure Active Directory.</span></span>
* <span data-ttu-id="7ba94-136">Felhasználási tartalmazott adatbázis felhasználók tooauthenticate identitások hello adatbázis szintjén.</span><span class="sxs-lookup"><span data-stu-id="7ba94-136">Uses contained database users tooauthenticate identities at hello database level.</span></span>
* <span data-ttu-id="7ba94-137">Csatlakozás az adatraktár tooSQL alkalmazások jogkivonat-alapú hitelesítést is támogatja.</span><span class="sxs-lookup"><span data-stu-id="7ba94-137">Supports token-based authentication for applications connecting tooSQL Data Warehouse.</span></span>
* <span data-ttu-id="7ba94-138">Az SQL Server Management Studio támogatja a többtényezős hitelesítés az Active Directory univerzális hitelesítésen keresztül.</span><span class="sxs-lookup"><span data-stu-id="7ba94-138">Supports Multi-Factor authentication through Active Directory Universal Authentication for SQL Server Management Studio.</span></span> <span data-ttu-id="7ba94-139">A multi-factor Authentication ismertetését lásd: [SSMS támogatása az Azure AD MFA az SQL-adatbázis és az SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7ba94-139">For a description of Multi-Factor Authentication, see [SSMS support for Azure AD MFA with SQL Database and SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7ba94-140">Az Azure Active Directory még viszonylag új, és bizonyos korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="7ba94-140">Azure Active Directory is still relatively new and has some limitations.</span></span> <span data-ttu-id="7ba94-141">tooensure, hogy az Azure Active Directory remekül beválik, ha a környezetben, lásd: [Azure AD-funkciókat és korlátozások][Azure AD features and limitations], kifejezetten hello további szempontokat.</span><span class="sxs-lookup"><span data-stu-id="7ba94-141">tooensure that Azure Active Directory is a good fit for your environment, see [Azure AD features and limitations][Azure AD features and limitations], specifically hello Additional considerations.</span></span>
> 
> 

### <a name="configuration-steps"></a><span data-ttu-id="7ba94-142">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="7ba94-142">Configuration steps</span></span>
<span data-ttu-id="7ba94-143">Kövesse a lépéseket tooconfigure Azure Active Directory-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="7ba94-143">Follow these steps tooconfigure Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="7ba94-144">Létrehozása és feltöltése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="7ba94-144">Create and populate an Azure Active Directory</span></span>
2. <span data-ttu-id="7ba94-145">Választható lehetőség: Hozzárendelése vagy hello active directory jelenleg az Azure-előfizetéshez társított módosítása</span><span class="sxs-lookup"><span data-stu-id="7ba94-145">Optional: Associate or change hello active directory that is currently associated with your Azure Subscription</span></span>
3. <span data-ttu-id="7ba94-146">Azure Active Directory-rendszergazda az Azure SQL Data Warehouse létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7ba94-146">Create an Azure Active Directory administrator for Azure SQL Data Warehouse.</span></span>
4. <span data-ttu-id="7ba94-147">Állítsa be az ügyfélszámítógépen</span><span class="sxs-lookup"><span data-stu-id="7ba94-147">Configure your client computers</span></span>
5. <span data-ttu-id="7ba94-148">Felhasználók létrehozása a tartalmazott adatbázis az adatbázis leképezve tooAzure AD identitások</span><span class="sxs-lookup"><span data-stu-id="7ba94-148">Create contained database users in your database mapped tooAzure AD identities</span></span>
6. <span data-ttu-id="7ba94-149">Tooyour adatraktár csatlakoztatása az Azure AD-azonosítók használatával</span><span class="sxs-lookup"><span data-stu-id="7ba94-149">Connect tooyour data warehouse by using Azure AD identities</span></span>

<span data-ttu-id="7ba94-150">Azure Active Directory-felhasználók jelenleg nem láthatók az SSDT Object Explorerben.</span><span class="sxs-lookup"><span data-stu-id="7ba94-150">Currently Azure Active Directory users are not shown in SSDT Object Explorer.</span></span> <span data-ttu-id="7ba94-151">A probléma megoldásához hello felhasználók megtekintése [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ba94-151">As a workaround, view hello users in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span></span>

### <a name="find-hello-details"></a><span data-ttu-id="7ba94-152">Hello részletei</span><span class="sxs-lookup"><span data-stu-id="7ba94-152">Find hello details</span></span>
* <span data-ttu-id="7ba94-153">hello lépéseket tooconfigure és -felhasználási Azure Active Directory-hitelesítés esetén Azure SQL Database és az Azure SQL Data Warehouse csaknem azonosak.</span><span class="sxs-lookup"><span data-stu-id="7ba94-153">hello steps tooconfigure and use Azure Active Directory authentication are nearly identical for Azure SQL Database and Azure SQL Data Warehouse.</span></span> <span data-ttu-id="7ba94-154">Hajtsa végre a hello hello témakörben ismertetett lépések részletes [csatlakozás tooSQL adatbázis vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítéssel](../sql-database/sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7ba94-154">Follow hello detailed steps in hello topic [Connecting tooSQL Database or SQL Data Warehouse By Using Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md).</span></span>
* <span data-ttu-id="7ba94-155">Hozzon létre egyéni adatbázis-szerepkörök, és a felhasználók toohello szerepkörök hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="7ba94-155">Create custom database roles and add users toohello roles.</span></span> <span data-ttu-id="7ba94-156">A részletes szükséges engedélyeket adja meg toohello szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="7ba94-156">Then grant granular permissions toohello roles.</span></span> <span data-ttu-id="7ba94-157">További információkért lásd: [Ismerkedés az adatbázis-motor engedélyek](https://msdn.microsoft.com/library/mt667986.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ba94-157">For more information, see [Getting Started with Database Engine Permissions](https://msdn.microsoft.com/library/mt667986.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ba94-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ba94-158">Next steps</span></span>
<span data-ttu-id="7ba94-159">a Visual Studio és más alkalmazásokkal, az adatraktár lekérdezésére toostart lásd [lekérdezése a Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="7ba94-159">toostart querying your data warehouse with Visual Studio and other applications, see [Query with Visual Studio][Query with Visual Studio].</span></span>

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
