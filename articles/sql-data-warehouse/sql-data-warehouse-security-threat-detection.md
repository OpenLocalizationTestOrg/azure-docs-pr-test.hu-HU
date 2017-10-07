---
title: "az SQL Data Warehouse Fenyegetésészlelés lépései aaaGet"
description: "Tooget indításának fenyegetések észlelése"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: dec0b734849e7f52434e099db0b38fbf0bf6ad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-threat-detection"></a><span data-ttu-id="b0856-103">A fenyegetésészlelés az első lépései</span><span class="sxs-lookup"><span data-stu-id="b0856-103">Get started with threat detection</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b0856-104">Naplózás</span><span class="sxs-lookup"><span data-stu-id="b0856-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="b0856-105">Fenyegetések észlelése</span><span class="sxs-lookup"><span data-stu-id="b0856-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="b0856-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b0856-106">Overview</span></span>
<span data-ttu-id="b0856-107">A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli.</span><span class="sxs-lookup"><span data-stu-id="b0856-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="b0856-108">A Fenyegetésészlelés preview, és az SQL Data Warehouse támogatja.</span><span class="sxs-lookup"><span data-stu-id="b0856-108">Threat Detection is in preview and is supported for SQL Data Warehouse.</span></span>

<span data-ttu-id="b0856-109">A Fenyegetésészlelés biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló új réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="b0856-109">Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="b0856-110">Felhasználók hello gyanús események használatával felfedezheti [Azure SQL Data Warehouse-naplózás](sql-data-warehouse-auditing-overview.md) toodetermine, ha egy kísérlet tooaccess eredő megsértik vagy hello adatraktár adatai kihasználására.</span><span class="sxs-lookup"><span data-stu-id="b0856-110">Users can explore hello suspicious events using [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) toodetermine if they result from an attempt tooaccess, breach or exploit data in hello data warehouse.</span></span>
<span data-ttu-id="b0856-111">A Fenyegetésészlelés teszi a potenciális fenyegetések egyszerű tooaddress toohello adatok nélkül hello kell toobe adatraktár egy biztonsági szakértő, vagy kezelje a speciális biztonsági rendszerek figyelése.</span><span class="sxs-lookup"><span data-stu-id="b0856-111">Threat Detection makes it simple tooaddress potential threats toohello data warehouse without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="b0856-112">A Fenyegetésészlelés például bizonyos adatbázist érintő rendellenes tevékenységeket utaló esetleges SQL injektálási kísérletek begyűjtésére észleli.</span><span class="sxs-lookup"><span data-stu-id="b0856-112">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="b0856-113">SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b0856-113">SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="b0856-114">A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások mezőkbe alkalmazás bejegyzést, megsértése vagy hello adatbázis adatainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="b0856-114">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, for breaching or modifying data in hello database.</span></span>

## <a name="set-up-threat-detection-for-your-database"></a><span data-ttu-id="b0856-115">A fenyegetésészlelés az adatbázis beállítása</span><span class="sxs-lookup"><span data-stu-id="b0856-115">Set up threat detection for your database</span></span>
1. <span data-ttu-id="b0856-116">Indítsa el az Azure portálon, a hello [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b0856-116">Launch hello Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b0856-117">Keresse meg a toohello konfigurációs panelen található az SQL Data Warehouse toomonitor kívánt hello.</span><span class="sxs-lookup"><span data-stu-id="b0856-117">Navigate toohello configuration blade of hello SQL Data Warehouse you want toomonitor.</span></span> <span data-ttu-id="b0856-118">Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**.</span><span class="sxs-lookup"><span data-stu-id="b0856-118">In hello Settings blade, select **Auditing & Threat Detection**.</span></span>
   
    ![Navigációs ablaktábla][1]
3. <span data-ttu-id="b0856-120">A hello **naplózás és Fenyegetésészlelés** konfigurációs panelen kapcsolja **ON** naplózás, amely megjeleníti hello fenyegetés észlelési beállítások.</span><span class="sxs-lookup"><span data-stu-id="b0856-120">In hello **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display hello Threat detection settings.</span></span>
   
    ![Navigációs ablaktábla][2]
4. <span data-ttu-id="b0856-122">Kapcsolja be **ON** veszélyforrások detektálása.</span><span class="sxs-lookup"><span data-stu-id="b0856-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="b0856-123">Konfigurálhatja a hello listáját, amelyek megkapják a biztonsági riasztások adatraktár rendellenes tevékenységek észlelésekor e-maileket.</span><span class="sxs-lookup"><span data-stu-id="b0856-123">Configure hello list of emails that will receive security alerts upon detection of anomalous data warehouse activities.</span></span>
6. <span data-ttu-id="b0856-124">Kattintson a **mentése** a hello **naplózási & Threat detection** konfigurációs panel toosave hello új vagy frissített naplózás és a fenyegetések szabályzat.</span><span class="sxs-lookup"><span data-stu-id="b0856-124">Click **Save** in hello **Auditing & Threat detection** configuration blade toosave hello new or updated auditing and threat detection policy.</span></span>
   
    ![Navigációs ablaktábla][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="b0856-126">Megismerkedhet a rendellenes data warehouse tevékenységek egy gyanús esemény észlelése</span><span class="sxs-lookup"><span data-stu-id="b0856-126">Explore anomalous data warehouse activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="b0856-127">A adatbázist érintő rendellenes tevékenységeket észlelésekor e-mailben értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="b0856-127">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="b0856-128">hello e-mail hello gyanús biztonsági esemény hello rendellenes tevékenységek, az adatbázis neve, a kiszolgáló nevét és hello esemény időpontja hello jellege beleértve tájékoztatást fogunk adni.</span><span class="sxs-lookup"><span data-stu-id="b0856-128">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name and hello event time.</span></span> <span data-ttu-id="b0856-129">Emellett az információt nyújt a lehetséges okok és ajánlott műveletek tooinvestigate és hello potenciális fenyegetést toohello adatbázis mérsékelni.</span><span class="sxs-lookup"><span data-stu-id="b0856-129">In addition, it will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span><br/>
   
    ![Navigációs ablaktábla][4]
2. <span data-ttu-id="b0856-131">Hello e-mailben, kattintson a hello **Azure SQL-naplózás napló** hivatkozás, amely indítsa el a klasszikus Azure portál hello és hello vonatkozó naplózási bejegyzések hello gyanús esemény hello időpontja körül megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b0856-131">In hello email, click on hello **Azure SQL Auditing Log** link, which will launch hello Azure Classic Portal and show hello relevant Auditing records around hello time of hello suspicious event.</span></span>
   
    ![Navigációs ablaktábla][5]
3. <span data-ttu-id="b0856-133">Kattintson a hello naplózási bejegyzések tooview további részleteket a hello adatbázis gyanús tevékenységek, például az SQL-utasítás hiba okát, és az ügyfél IP.</span><span class="sxs-lookup"><span data-stu-id="b0856-133">Click on hello audit records tooview more details on hello suspicious database activities such as SQL statement, failure reason and client IP.</span></span>
   
    ![Navigációs ablaktábla][6]
4. <span data-ttu-id="b0856-135">Hello rekordok naplózása paneljén kattintson **Megnyitás az Excelben** előre konfigurált tooopen excel-sablon tooimport és futtatási mélyebb elemzésének hello audit napló hello gyanús esemény hello időpontja körül.</span><span class="sxs-lookup"><span data-stu-id="b0856-135">In hello Auditing Records blade, click  **Open in Excel** tooopen a pre-configured excel template tooimport and run deeper analysis of hello audit log around hello time of hello suspicious event.</span></span><br/><span data-ttu-id="b0856-136">
   **Megjegyzés:** az Excel 2010 vagy újabb, a Power Query és hello **gyors összevonás** beállításra akkor szükség</span><span class="sxs-lookup"><span data-stu-id="b0856-136">
**Note:** In Excel 2010 or later, Power Query and hello **Fast Combine** setting is required</span></span>
   
    ![Navigációs ablaktábla][7]
5. <span data-ttu-id="b0856-138">tooconfigure hello **gyors összevonás** beállítás - hello **POWER QUERY** menüszalag lapon jelölje be **beállítások** toodisplay hello beállítások párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="b0856-138">tooconfigure hello **Fast Combine** setting - In hello **POWER QUERY** ribbon tab, select **Options** toodisplay hello Options dialog.</span></span> <span data-ttu-id="b0856-139">Jelölje ki hello adatvédelmi szakaszt, és válassza ki a hello második lehetőség - "Hello adatvédelmi szintek figyelmen kívül, és a teljesítmény lehetséges javítása":</span><span class="sxs-lookup"><span data-stu-id="b0856-139">Select hello Privacy section and choose hello second option - 'Ignore hello Privacy Levels and potentially improve performance':</span></span>
   
    ![Navigációs ablaktábla][8]
6. <span data-ttu-id="b0856-141">tooload SQL naplókat, győződjön meg arról, hogy hello paraméterek hello beállítások lapon helyesen van beállítva és majd hello "Data" menüszalagon válassza hello "Az összes frissítés" gombra.</span><span class="sxs-lookup"><span data-stu-id="b0856-141">tooload SQL audit logs, ensure that hello parameters in hello settings tab are set correctly and then select hello 'Data' ribbon and click hello 'Refresh All' button.</span></span>
   
    ![Navigációs ablaktábla][9]
7. <span data-ttu-id="b0856-143">hello eredményei jelennek meg hello **SQL naplók** lap, amely mélyebb elemzése toorun hello rendellenes tevékenységet észlelt, és csökkenteni hello hatását hello biztonsági esemény az alkalmazás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="b0856-143">hello results appear in hello **SQL Audit Logs** sheet which enables you toorun deeper analysis of hello anomalous activities that were detected, and mitigate hello impact of hello security event in your application.</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
