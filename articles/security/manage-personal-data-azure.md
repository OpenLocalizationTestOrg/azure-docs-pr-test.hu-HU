---
title: "aaaManage személyes adatokat a Microsoft Azure |} Microsoft Docs"
description: "útmutatás toocorrect, frissítése, törlése, és a személyes adatok az Azure Active Directory és az Azure SQL Database exportálása"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 032f70d32377cb9395cb2c35c27dad05001537c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a>A Microsoft Azure-ban személyes adatok kezelése

Ez a cikk toocorrect, frissítése, törlése, és a személyes adatok az Azure Active Directory és az Azure SQL Database exportálása nyújt útmutatást.

## <a name="scenario"></a>Forgatókönyv

Dublin-alapú vállalati biztosít egy helyen csúcskategóriás cél esküvő Írországban és mind a helyi és a nemzetközi felhasználói alapja a hello világ számára. Irodák, az ügyfelek, az alkalmazottak és szállítók hello world toofully szolgáltatás hello helyszínek kínálnak körül található rendelkeznek.

Sok egyéb elemek közötti hello vállalati nyomon követi az étele allergiák és étkezési beállításokat tartalmazó RSVPs. Lakodalmát vendégek horseback haladás, barangolás, lakókocsik fel stb., mint például a különböző tevékenységekhez regisztrálhatja és még egymással a központi weblapon toohello eseményt vezető hello hónap során. hello vállalati személyes adatokat gyűjti össze az alkalmazottak, szállítók, ügyfelek és lakodalmát vendégek. Hello üzleti hello vállalati nemzetközi jellege miatt hello meg kell felelnie, több szinten szabályozás.

## <a name="problem-statement"></a>Probléma leírása

- Adatok rendszergazdák képesek toocorrect pontatlan személyes információkat és frissítési hiányos vagy változó személyes adatokat kell lennie.

- Adatok rendszergazdák kell tudni toodelete személyes információkat hello kérésre adatok tárgy kell lennie.

- Adatok rendszergazdák tooexport adatokra van szükségük, és a tooa érintett saját kérésre közös, strukturált formátumban adja meg.

## <a name="company-goals"></a>Vállalati célok

- Ügyfél hibás vagy hiányos, lakodalmát Vendég, alkalmazott és szállítói személyes adatokat kell javítani vagy frissítése az Azure Active Directory és az Azure SQL Database.

- Személyes adatokat törölni kell az Azure Active Directory és az Azure SQL Database egy érintett hello kérésére.

- Személyes adatok egy érintett hello kérésére közös, strukturált formátumban kell exportálni.

## <a name="solutions"></a>Megoldások

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Az Azure Active Directory: kijavításához vagy javítsa ki hibás vagy hiányos személyes adatokat, és a törlés vagy a személyes adatok és felhasználói profilok törlése

[Az Azure Active Directory](https://azure.microsoft.com/services/active-directory/) a Microsoft felhőalapú, több-bérlős directory és az identity management szolgáltatás.
Javítsa ki, frissíteni vagy törölni a felhasználói profilok felhasználói és az alkalmazottak és a felhasználói munkahelyi adatokat, amelyek tartalmazzák a személyes adatok, például a felhasználó nevét, munkahelyi cím, cím vagy telefonszám, a [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) hello segítségével környezet [Azure-portálon](https://portal.azure.com/).

Hello könyvtár egy globális rendszergazdai fiókkal kell bejelentkeznie.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Hogyan javítsa vagy frissítse a felhasználói profil és a munkahelyi Azure Active Directoryban?

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.

2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

    ![Media/image1.png](media/manage-personal-data-azure/image001.png)

3. A hello **felhasználók és csoportok** panelen válassza **felhasználók**.

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. A hello **felhasználók és csoportok - felhasználók** panelen hello listából válasszon ki egy felhasználót, és ezt követően a kiválasztott felhasználó hello hello panelen válassza **profil** tooview hello felhasználói profil adatait, hogy a javított toobe kell vagy frissített.

    ![Media/image3.png](media/manage-personal-data-azure/image005.png)

5. Javítsa ki vagy hello frissítése, és ezt követően hello parancssávon válassza **mentéséhez.**

6.  A kiválasztott felhasználó hello hello panelen válassza ki a **munkahelyi adatai** tooview felhasználói munkahelyi adatokat, amelyek toobe javítani, vagy frissíteni.

    ![Media/image4.png](media/manage-personal-data-azure/image007.png)

7. Javítsa ki vagy hello felhasználói a munkahelyi adatok frissítése, és ezt követően hello parancssávon válassza **mentéséhez.**

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Az Azure Active Directory felhasználói profil törlése

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.

2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

    ![](media/manage-personal-data-azure/image001.png)

3. A hello **felhasználók és csoportok** panelen válassza **felhasználók**.

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. A hello **felhasználók és csoportok - felhasználók** panelen válassza ki a megfelelő felhasználói hello listából.

    ![Media/image3.png](media/manage-personal-data-azure/image007.png)

5. A kiválasztott felhasználó hello hello panelen válassza ki a **áttekintése**, majd a hello parancssávon válassza **törlése**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>Az SQL Database: kijavításához/helyesbítse hibás vagy hiányos személyes adatokat; a törlés vagy törlése a személyes adatok; személyes adatok exportálása 

[Az Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) egy felhő-adatbázis, amely a fejlesztőket létrehozása és alkalmazások karbantartása.

A személyes adatok frissíthető [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) szabványos SQL-lekérdezések, és azt is törölheti. Személyes adatokat emellett exportálhatja különböző módszerek, például hello Azure SQL Server importálása és exportálása varázslóban, és többféle formátumúak, beleértve a BACPAC fájl SQL-adatbázisból.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Hogyan javítsa ki, frissítés, vagy törölheti a személyes adatokat az SQL-adatbázis?

toolearn hogyan toocorrect vagy az update SQL-adatbázis, a személyes adatok Microsoft hello [frissítés (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [szöveg frissítése](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [közös Táblakifejezés frissíteni](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), vagy [ Szöveg írása frissítése](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentációját.

toolearn hogyan keresse fel a személyes adatok toodelete az SQL-adatbázis, a hello [törlése (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentációját.

#### <a name="how-do-i-export-personal-data-tooa-bacpac-file-in-sql-database"></a>Személyes adatok tooa BACPAC fájlt az SQL-adatbázis exportálása?

Egy BACPAC fájlt hello SQL-adatbázis adatait és a metaadatok tartalmaz, és egy zip-fájl BACPAC kiterjesztéssel. Ezt megteheti hello segítségével [Azure-portálon](https://portal.azure.com/), hello SQLPackage parancssori segédprogram, az SQL Server Management Studio (SSMS) vagy a PowerShell.

toolearn hogyan tooexport tooa BACPAC adatfájlt, látogasson el hello [tooa Azure SQL adatbázis BACPAC fájl exportálása](https://docs.microsoft.com/azure/sql-database/sql-database-export) lapon, amely az egyes módszerek fent felsorolt részletes utasításokat tartalmaz.

Személyes adatok exportálása az SQL Database adatbázishoz az SQL Server importálhat hello és exportálása varázsló

A varázsló segítségével másolja át a forrás tooa cél adatokat. Egy bevezető toohello varázsló, beleértve a hogyan tooget azt, engedélyek adatokat, és hogyan tooget súgó hello eszközzel, látogasson el hello [importálása és az adatok exportálása az SQL Server importálhat hello és a Tanúsítványexportáló varázslóban](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) weblap.

Hello varázsló lépéseinek áttekintése, a Microsoft hello [hello SQL Server importálása és exportálása varázsló lépései](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) weblap.

## <a name="next-steps"></a>További lépések:

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

