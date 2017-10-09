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
# <a name="manage-personal-data-in-microsoft-azure"></a><span data-ttu-id="59a19-103">A Microsoft Azure-ban személyes adatok kezelése</span><span class="sxs-lookup"><span data-stu-id="59a19-103">Manage personal data in Microsoft Azure</span></span>

<span data-ttu-id="59a19-104">Ez a cikk toocorrect, frissítése, törlése, és a személyes adatok az Azure Active Directory és az Azure SQL Database exportálása nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="59a19-104">This article provides guidance on how toocorrect, update, delete, and export personal data in Azure Active Directory and Azure SQL Database.</span></span>

## <a name="scenario"></a><span data-ttu-id="59a19-105">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="59a19-105">Scenario</span></span>

<span data-ttu-id="59a19-106">Dublin-alapú vállalati biztosít egy helyen csúcskategóriás cél esküvő Írországban és mind a helyi és a nemzetközi felhasználói alapja a hello világ számára.</span><span class="sxs-lookup"><span data-stu-id="59a19-106">A Dublin-based company provides one-stop shopping for high end destination weddings in Ireland and around hello world for both a local and international customer base.</span></span> <span data-ttu-id="59a19-107">Irodák, az ügyfelek, az alkalmazottak és szállítók hello world toofully szolgáltatás hello helyszínek kínálnak körül található rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="59a19-107">They have offices, customers, employees, and vendors located around hello world toofully service hello venues they offer.</span></span>

<span data-ttu-id="59a19-108">Sok egyéb elemek közötti hello vállalati nyomon követi az étele allergiák és étkezési beállításokat tartalmazó RSVPs.</span><span class="sxs-lookup"><span data-stu-id="59a19-108">Among many other items, hello company keeps track of RSVPs that include food allergies and dietary preferences.</span></span> <span data-ttu-id="59a19-109">Lakodalmát vendégek horseback haladás, barangolás, lakókocsik fel stb., mint például a különböző tevékenységekhez regisztrálhatja és még egymással a központi weblapon toohello eseményt vezető hello hónap során.</span><span class="sxs-lookup"><span data-stu-id="59a19-109">Wedding guests can register for various activities such as horseback riding, surfing, boat rides, etc., and even interact with one another on a central web page during hello months leading up toohello event.</span></span> <span data-ttu-id="59a19-110">hello vállalati személyes adatokat gyűjti össze az alkalmazottak, szállítók, ügyfelek és lakodalmát vendégek.</span><span class="sxs-lookup"><span data-stu-id="59a19-110">hello company collects personal information from employees, vendors, customers, and wedding guests.</span></span> <span data-ttu-id="59a19-111">Hello üzleti hello vállalati nemzetközi jellege miatt hello meg kell felelnie, több szinten szabályozás.</span><span class="sxs-lookup"><span data-stu-id="59a19-111">Because of hello international nature of hello business hello company must comply with multiple levels of regulation.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="59a19-112">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="59a19-112">Problem statement</span></span>

- <span data-ttu-id="59a19-113">Adatok rendszergazdák képesek toocorrect pontatlan személyes információkat és frissítési hiányos vagy változó személyes adatokat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="59a19-113">Data admins must be able toocorrect inaccurate personal information and update incomplete or changing personal information.</span></span>

- <span data-ttu-id="59a19-114">Adatok rendszergazdák kell tudni toodelete személyes információkat hello kérésre adatok tárgy kell lennie.</span><span class="sxs-lookup"><span data-stu-id="59a19-114">Data admins need must be able toodelete personal information upon hello request of a data subject.</span></span>

- <span data-ttu-id="59a19-115">Adatok rendszergazdák tooexport adatokra van szükségük, és a tooa érintett saját kérésre közös, strukturált formátumban adja meg.</span><span class="sxs-lookup"><span data-stu-id="59a19-115">Data admins need tooexport data and provide it tooa data subject in a common, structured format upon his or her request.</span></span>

## <a name="company-goals"></a><span data-ttu-id="59a19-116">Vállalati célok</span><span class="sxs-lookup"><span data-stu-id="59a19-116">Company goals</span></span>

- <span data-ttu-id="59a19-117">Ügyfél hibás vagy hiányos, lakodalmát Vendég, alkalmazott és szállítói személyes adatokat kell javítani vagy frissítése az Azure Active Directory és az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="59a19-117">Inaccurate or incomplete customer, wedding guest, employee, and vendor personal information must be corrected or updated in Azure Active Directory and Azure SQL Database.</span></span>

- <span data-ttu-id="59a19-118">Személyes adatokat törölni kell az Azure Active Directory és az Azure SQL Database egy érintett hello kérésére.</span><span class="sxs-lookup"><span data-stu-id="59a19-118">Personal information must be deleted in Azure Active Directory and Azure SQL Database upon hello request of a data subject.</span></span>

- <span data-ttu-id="59a19-119">Személyes adatok egy érintett hello kérésére közös, strukturált formátumban kell exportálni.</span><span class="sxs-lookup"><span data-stu-id="59a19-119">Personal data must be exported in a common, structured format upon hello request of a data subject.</span></span>

## <a name="solutions"></a><span data-ttu-id="59a19-120">Megoldások</span><span class="sxs-lookup"><span data-stu-id="59a19-120">Solutions</span></span>

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a><span data-ttu-id="59a19-121">Az Azure Active Directory: kijavításához vagy javítsa ki hibás vagy hiányos személyes adatokat, és a törlés vagy a személyes adatok és felhasználói profilok törlése</span><span class="sxs-lookup"><span data-stu-id="59a19-121">Azure Active Directory: rectify/correct inaccurate or incomplete personal data and erase/delete personal data/user profiles</span></span>

<span data-ttu-id="59a19-122">[Az Azure Active Directory](https://azure.microsoft.com/services/active-directory/) a Microsoft felhőalapú, több-bérlős directory és az identity management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="59a19-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) is Microsoft’s cloud-based, multi-tenant directory and identity management service.</span></span>
<span data-ttu-id="59a19-123">Javítsa ki, frissíteni vagy törölni a felhasználói profilok felhasználói és az alkalmazottak és a felhasználói munkahelyi adatokat, amelyek tartalmazzák a személyes adatok, például a felhasználó nevét, munkahelyi cím, cím vagy telefonszám, a [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) hello segítségével környezet [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="59a19-123">You can correct, update, or delete customer and employee user profiles and user work information that contain personal data, such as a user’s name, work title, address, or phone number, in your [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) environment by using hello [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="59a19-124">Hello könyvtár egy globális rendszergazdai fiókkal kell bejelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="59a19-124">You must sign in with an account that’s a global admin for hello directory.</span></span>

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a><span data-ttu-id="59a19-125">Hogyan javítsa vagy frissítse a felhasználói profil és a munkahelyi Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="59a19-125">How do I correct or update user profile and work information in Azure Active Directory?</span></span>

1. <span data-ttu-id="59a19-126">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="59a19-126">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>

2. <span data-ttu-id="59a19-127">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="59a19-127">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

    ![Media/image1.png](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="59a19-129">A hello **felhasználók és csoportok** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="59a19-129">On hello **Users and groups** blade, select **Users**.</span></span>

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="59a19-131">A hello **felhasználók és csoportok - felhasználók** panelen hello listából válasszon ki egy felhasználót, és ezt követően a kiválasztott felhasználó hello hello panelen válassza **profil** tooview hello felhasználói profil adatait, hogy a javított toobe kell vagy frissített.</span><span class="sxs-lookup"><span data-stu-id="59a19-131">On hello **Users and groups - Users** blade, select a user from hello list, and then, on hello blade for hello selected user, select **Profile** tooview hello user profile information that needs toobe corrected or updated.</span></span>

    ![Media/image3.png](media/manage-personal-data-azure/image005.png)

5. <span data-ttu-id="59a19-133">Javítsa ki vagy hello frissítése, és ezt követően hello parancssávon válassza **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="59a19-133">Correct or update hello information, and then, in hello command bar, select **Save.**</span></span>

6.  <span data-ttu-id="59a19-134">A kiválasztott felhasználó hello hello panelen válassza ki a **munkahelyi adatai** tooview felhasználói munkahelyi adatokat, amelyek toobe javítani, vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="59a19-134">On hello blade for hello selected user, select **Work Info** tooview user work information that needs toobe corrected or updated.</span></span>

    ![Media/image4.png](media/manage-personal-data-azure/image007.png)

7. <span data-ttu-id="59a19-136">Javítsa ki vagy hello felhasználói a munkahelyi adatok frissítése, és ezt követően hello parancssávon válassza **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="59a19-136">Correct or update hello user work information, and then, in hello command bar, select **Save.**</span></span>

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a><span data-ttu-id="59a19-137">Az Azure Active Directory felhasználói profil törlése</span><span class="sxs-lookup"><span data-stu-id="59a19-137">How do I delete a user profile in Azure Active Directory?</span></span>

1. <span data-ttu-id="59a19-138">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="59a19-138">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>

2. <span data-ttu-id="59a19-139">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="59a19-139">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

    ![](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="59a19-140">A hello **felhasználók és csoportok** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="59a19-140">On hello **Users and groups** blade, select **Users**.</span></span>

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="59a19-142">A hello **felhasználók és csoportok - felhasználók** panelen válassza ki a megfelelő felhasználói hello listából.</span><span class="sxs-lookup"><span data-stu-id="59a19-142">On hello **Users and groups - Users** blade, select a user from hello list.</span></span>

    ![Media/image3.png](media/manage-personal-data-azure/image007.png)

5. <span data-ttu-id="59a19-144">A kiválasztott felhasználó hello hello panelen válassza ki a **áttekintése**, majd a hello parancssávon válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="59a19-144">On hello blade for hello selected user, select **Overview**, and then in hello command bar, select **Delete**.</span></span>

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a><span data-ttu-id="59a19-145">Az SQL Database: kijavításához/helyesbítse hibás vagy hiányos személyes adatokat; a törlés vagy törlése a személyes adatok; személyes adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="59a19-145">SQL Database: rectify/correct inaccurate or incomplete personal data; erase/delete personal data; export personal data</span></span> 

<span data-ttu-id="59a19-146">[Az Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) egy felhő-adatbázis, amely a fejlesztőket létrehozása és alkalmazások karbantartása.</span><span class="sxs-lookup"><span data-stu-id="59a19-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) is a cloud database that helps developers build and maintain applications.</span></span>

<span data-ttu-id="59a19-147">A személyes adatok frissíthető [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) szabványos SQL-lekérdezések, és azt is törölheti.</span><span class="sxs-lookup"><span data-stu-id="59a19-147">Personal data can be updated in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) using standard SQL queries, and it can also be deleted.</span></span> <span data-ttu-id="59a19-148">Személyes adatokat emellett exportálhatja különböző módszerek, például hello Azure SQL Server importálása és exportálása varázslóban, és többféle formátumúak, beleértve a BACPAC fájl SQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="59a19-148">Additionally, personal data can be exported from SQL Database using a variety of methods, including hello Azure SQL Server import and export wizard, and in a variety of formats, including a BACPAC file.</span></span>

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a><span data-ttu-id="59a19-149">Hogyan javítsa ki, frissítés, vagy törölheti a személyes adatokat az SQL-adatbázis?</span><span class="sxs-lookup"><span data-stu-id="59a19-149">How do I correct, update, or erase personal data in SQL Database?</span></span>

<span data-ttu-id="59a19-150">toolearn hogyan toocorrect vagy az update SQL-adatbázis, a személyes adatok Microsoft hello [frissítés (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [szöveg frissítése](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [közös Táblakifejezés frissíteni](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), vagy [ Szöveg írása frissítése](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="59a19-150">toolearn how toocorrect or update personal data in SQL Database, visit hello [Update (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [Update Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [Update with Common Table Expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), or [Update Write Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) documentation.</span></span>

<span data-ttu-id="59a19-151">toolearn hogyan keresse fel a személyes adatok toodelete az SQL-adatbázis, a hello [törlése (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="59a19-151">toolearn how toodelete personal data in SQL Database, visit hello [Delete (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) documentation.</span></span>

#### <a name="how-do-i-export-personal-data-tooa-bacpac-file-in-sql-database"></a><span data-ttu-id="59a19-152">Személyes adatok tooa BACPAC fájlt az SQL-adatbázis exportálása?</span><span class="sxs-lookup"><span data-stu-id="59a19-152">How do I export personal data tooa BACPAC file in SQL Database?</span></span>

<span data-ttu-id="59a19-153">Egy BACPAC fájlt hello SQL-adatbázis adatait és a metaadatok tartalmaz, és egy zip-fájl BACPAC kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="59a19-153">A BACPAC file includes hello SQL Database data and metadata and is a zip file with a BACPAC extension.</span></span> <span data-ttu-id="59a19-154">Ezt megteheti hello segítségével [Azure-portálon](https://portal.azure.com/), hello SQLPackage parancssori segédprogram, az SQL Server Management Studio (SSMS) vagy a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59a19-154">This can be done using hello [Azure portal](https://portal.azure.com/), hello SQLPackage command-line utility, SQL Server Management Studio (SSMS), or PowerShell.</span></span>

<span data-ttu-id="59a19-155">toolearn hogyan tooexport tooa BACPAC adatfájlt, látogasson el hello [tooa Azure SQL adatbázis BACPAC fájl exportálása](https://docs.microsoft.com/azure/sql-database/sql-database-export) lapon, amely az egyes módszerek fent felsorolt részletes utasításokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="59a19-155">toolearn how tooexport data tooa BACPAC file, visit hello [Export an Azure SQL database tooa BACPAC file](https://docs.microsoft.com/azure/sql-database/sql-database-export) page, which includes detailed instructions for each method listed above.</span></span>

<span data-ttu-id="59a19-156">Személyes adatok exportálása az SQL Database adatbázishoz az SQL Server importálhat hello és exportálása varázsló</span><span class="sxs-lookup"><span data-stu-id="59a19-156">How do I export personal data from SQL Database with hello SQL Server Import and Export Wizard?</span></span>

<span data-ttu-id="59a19-157">A varázsló segítségével másolja át a forrás tooa cél adatokat.</span><span class="sxs-lookup"><span data-stu-id="59a19-157">This wizard helps you copy data from a source tooa destination.</span></span> <span data-ttu-id="59a19-158">Egy bevezető toohello varázsló, beleértve a hogyan tooget azt, engedélyek adatokat, és hogyan tooget súgó hello eszközzel, látogasson el hello [importálása és az adatok exportálása az SQL Server importálhat hello és a Tanúsítványexportáló varázslóban](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) weblap.</span><span class="sxs-lookup"><span data-stu-id="59a19-158">For an introduction toohello wizard, including how tooget it, permissions information, and how tooget help with hello tool, visit hello [Import and Export Data with hello SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) web page.</span></span>

<span data-ttu-id="59a19-159">Hello varázsló lépéseinek áttekintése, a Microsoft hello [hello SQL Server importálása és exportálása varázsló lépései](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) weblap.</span><span class="sxs-lookup"><span data-stu-id="59a19-159">For an overview of steps for hello wizard, visit hello [Steps in hello SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) web page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59a19-160">További lépések:</span><span class="sxs-lookup"><span data-stu-id="59a19-160">Next Steps:</span></span>

[<span data-ttu-id="59a19-161">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="59a19-161">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[<span data-ttu-id="59a19-162">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="59a19-162">Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

