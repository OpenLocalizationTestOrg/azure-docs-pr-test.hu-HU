---
title: "az Azure SQL Database észlelési - aaaThreat |} Microsoft Docs"
description: "A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli."
services: sql-database
documentationcenter: 
author: rmatchoro
manager: jhubbard
editor: v-romcal
ms.assetid: b50d232a-4225-46ed-91e7-75288f55ee84
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/19/2017
ms.author: ronmat; ronitr
ms.openlocfilehash: 0879d20eff515a4e69358b5a98ceccf57fbd0ea2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-threat-detection"></a><span data-ttu-id="f1077-103">SQL adatbázis fenyegetések észlelése</span><span class="sxs-lookup"><span data-stu-id="f1077-103">SQL Database Threat Detection</span></span>

<span data-ttu-id="f1077-104">SQL Fenyegetésészlelés szokatlan és potenciálisan káros kísérletek tooaccess vagy biztonsági rés adatbázisok jelző rendellenes tevékenységeket észleli.</span><span class="sxs-lookup"><span data-stu-id="f1077-104">SQL Threat Detection detects anomalous activities indicating unusual and potentially harmful attempts tooaccess or exploit databases.</span></span>

## <a name="overview"></a><span data-ttu-id="f1077-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f1077-105">Overview</span></span>

<span data-ttu-id="f1077-106">A Fenyegetésészlelés SQL biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló egy új réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="f1077-106">SQL Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span>  <span data-ttu-id="f1077-107">Felhasználók adatbázis gyanús tevékenységeket, a potenciális biztonsági réseket, és a SQL injektálási támadások, valamint rendellenes adatbázis hozzáférési minták riasztást kap.</span><span class="sxs-lookup"><span data-stu-id="f1077-107">Users will receive an alert upon suspicious database activities, potential vulnerabilities, and SQL injection attacks, as well as anomalous database access patterns.</span></span> <span data-ttu-id="f1077-108">SQL Fenyegetésészlelés riasztásokat gyanús tevékenység részleteinek megadása, és hogyan művelet javasolja tooinvestigate és hello fenyegetést.</span><span class="sxs-lookup"><span data-stu-id="f1077-108">SQL Threat Detection alerts provide details of suspicious activity and recommend action on how tooinvestigate and mitigate hello threat.</span></span> <span data-ttu-id="f1077-109">Felhasználók hello gyanús események használatával felfedezheti [SQL Database Auditing](sql-database-auditing.md) toodetermine, ha egy kísérlet tooaccess eredő megsértik, vagy kihasznál hello adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="f1077-109">Users can explore hello suspicious events using [SQL Database Auditing](sql-database-auditing.md) toodetermine if they result from an attempt tooaccess, breach, or exploit data in hello database.</span></span> <span data-ttu-id="f1077-110">A Fenyegetésészlelés teszi egyszerű tooaddress potenciális fenyegetések toohello adatbázis hello kell toobe egy biztonsági szakértő nélkül, vagy speciális biztonsági rendszerek figyelése kezelése.</span><span class="sxs-lookup"><span data-stu-id="f1077-110">Threat Detection makes it simple tooaddress potential threats toohello database without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="f1077-111">Például SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f1077-111">For example, SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="f1077-112">A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások alkalmazás mezőkbe megsértése vagy hello adatbázis adatainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="f1077-112">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, breaching or modifying data in hello database.</span></span>

<span data-ttu-id="f1077-113">SQL Fenyegetésészlelés integrálja riasztások [az Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), és minden védett SQL-adatbáziskiszolgáló alapján számlázzuk ki hello azonos ár, Azure Security Center szabványos rétegként $15/csomópont/hónappal, ahol minden védett SQL Adatbázis-kiszolgáló egy csomópont számítanak.</span><span class="sxs-lookup"><span data-stu-id="f1077-113">SQL Threat Detection integrates alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), and, each protected SQL Database server will be billed at hello same price as Azure Security Center Standard tier, at $15/node/month, where each protected SQL Database server is counted as one node.</span></span> <span data-ttu-id="f1077-114">Szeretnénk felhívni a tootry azt a 60 napig ingyenes.</span><span class="sxs-lookup"><span data-stu-id="f1077-114">We invite you tootry it out for 60 days for free.</span></span> 

## <a name="set-up-threat-detection-for-your-database-in-hello-azure-portal"></a><span data-ttu-id="f1077-115">A fenyegetésészlelés az adatbázis az Azure-portálon hello beállítása</span><span class="sxs-lookup"><span data-stu-id="f1077-115">Set up threat detection for your database in hello Azure portal</span></span>
1. <span data-ttu-id="f1077-116">Indítsa el a hello Azure portál, [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f1077-116">Launch hello Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f1077-117">Keresse meg a toohello konfigurációs panelen hello toomonitor kívánt SQL-adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="f1077-117">Navigate toohello configuration blade of hello SQL Database you want toomonitor.</span></span> <span data-ttu-id="f1077-118">Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**.</span><span class="sxs-lookup"><span data-stu-id="f1077-118">In hello Settings blade, select **Auditing & Threat Detection**.</span></span> 
    <span data-ttu-id="f1077-119">![Navigációs ablaktábla][1]</span><span class="sxs-lookup"><span data-stu-id="f1077-119">![Navigation pane][1]</span></span>
3. <span data-ttu-id="f1077-120">A hello **naplózás és Fenyegetésészlelés** konfigurációs panelen kapcsolja **ON** naplózási megjelenítő hello fenyegetés észlelési beállítások.</span><span class="sxs-lookup"><span data-stu-id="f1077-120">In hello **Auditing & Threat Detection** configuration blade turn **ON** Auditing, which will display hello threat detection settings.</span></span>
  
    ![Navigációs ablaktábla][2]
4. <span data-ttu-id="f1077-122">Kapcsolja be **ON** veszélyforrások detektálása.</span><span class="sxs-lookup"><span data-stu-id="f1077-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="f1077-123">Konfigurálhatja az e-mailek adatbázist érintő rendellenes tevékenységeket észlelése a biztonsági riasztásokat fogadó hello listáját.</span><span class="sxs-lookup"><span data-stu-id="f1077-123">Configure hello list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>
6. <span data-ttu-id="f1077-124">Kattintson a **mentése** a hello **naplózási & Threat detection** panel toosave hello új vagy frissített naplózás és a fenyegetések észlelési beállítások.</span><span class="sxs-lookup"><span data-stu-id="f1077-124">Click **Save** in hello **Auditing & Threat detection** blade toosave hello new or updated auditing and threat detection settings.</span></span>
       
    ![Navigációs ablaktábla][3]

## <a name="set-up-threat-detection-using-powershell"></a><span data-ttu-id="f1077-126">Állítsa be a fenyegetésészlelés PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f1077-126">Set up threat detection using PowerShell</span></span>

<span data-ttu-id="f1077-127">Tekintse meg a parancsfájl például [konfigurálhatja a naplózás és a fenyegetések észlelésére, a PowerShell használatával](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f1077-127">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="f1077-128">Fedezze fel az adatbázist érintő rendellenes tevékenységeket gyanús esemény észlelése</span><span class="sxs-lookup"><span data-stu-id="f1077-128">Explore anomalous database activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="f1077-129">A adatbázist érintő rendellenes tevékenységeket észlelésekor e-mailben értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="f1077-129">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="f1077-130">hello e-mail hello gyanús biztonsági esemény hello rendellenes tevékenységek, az adatbázis neve, a kiszolgáló neve, a alkalmazásnév és a hello esemény időpontja hello jellege beleértve tájékoztatást fogunk adni.</span><span class="sxs-lookup"><span data-stu-id="f1077-130">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name, application name, and hello event time.</span></span> <span data-ttu-id="f1077-131">Hello e-mailek emellett információt nyújt a lehetséges okok és a javasolt műveletek tooinvestigate, és hello potenciális fenyegetést toohello adatbázis mérséklése.</span><span class="sxs-lookup"><span data-stu-id="f1077-131">In addition, hello email will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span><br/>
     
    ![Navigációs ablaktábla][4]
2. <span data-ttu-id="f1077-133">hello e-mail riasztás egy közvetlen hivatkozást toohello SQL Audit napló tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1077-133">hello email alert includes a direct link toohello SQL Audit log.</span></span> <span data-ttu-id="f1077-134">Kattintson a hivatkozás elindítja hello Azure portálon, és megnyílik hello SQL auditálási rekordok hello gyanús esemény hello időpontja körül.</span><span class="sxs-lookup"><span data-stu-id="f1077-134">Clicking on this link launches hello Azure portal and opens hello SQL Audit records around hello time of hello suspicious event.</span></span> <span data-ttu-id="f1077-135">Kattintson egy naplózási rekord tooview a további részleteket a hello adatbázis gyanús tevékenységeket, így könnyebben toofind hello SQL-utasítások, amelyek végrehajtódtak (ki fért, tevékenységük és mikor) és döntse el, ha hello esemény jogos vagy rosszindulatú volt-e (pl. alkalmazás a biztonsági rés tooSQL injektálási kihasználni, valaki megszegése bizalmas adatokat, stb.).</span><span class="sxs-lookup"><span data-stu-id="f1077-135">Click on an audit record tooview more details on hello suspicious database activities, making it easier toofind hello SQL statements that were executed (who accessed, what they did and when) and determine if hello event was legitimate or malicious (e.g. application vulnerability tooSQL injection was exploited, someone breached sensitive data, etc.).</span></span><br/><span data-ttu-id="f1077-136">
   ![Navigációs ablaktábla][5]</span><span class="sxs-lookup"><span data-stu-id="f1077-136">
![Navigation pane][5]</span></span>


## <a name="explore-threat-detection-alerts-for-your-database-in-hello-azure-portal"></a><span data-ttu-id="f1077-137">Fedezze fel az adatbázis hello Azure-portálon a figyelmeztetések</span><span class="sxs-lookup"><span data-stu-id="f1077-137">Explore threat detection alerts for your database in hello Azure portal</span></span>

<span data-ttu-id="f1077-138">SQL-adatbázis Fenyegetésészlelés integrálja a riasztások [az Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span><span class="sxs-lookup"><span data-stu-id="f1077-138">SQL Database Threat Detection integrates its alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span></span> <span data-ttu-id="f1077-139">Élő SQL biztonság csempe belül hello adatbázis panel az aktív fenyegetések hello Azure portál nyomon követi hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="f1077-139">A live SQL security tile within hello database blade in hello Azure portal tracks hello status of active threats.</span></span> 

   ![Navigációs ablaktábla][6]
   
1. <span data-ttu-id="f1077-141">A hello SQL biztonsági csempére kattintva hello Azure Security Center riasztások panel elindul, és aktív SQL fenyegetést észlel az hello adatbázis áttekintése.</span><span class="sxs-lookup"><span data-stu-id="f1077-141">Clicking on hello SQL security tile launches hello Azure Security Center alerts blade and provides an overview of active SQL threats detected on hello database.</span></span> 

  ![Navigációs ablaktábla][7]

2. <span data-ttu-id="f1077-143">Egy meghatározott riasztásra kattintva jelenít meg további adatokat, és vizsgálja az ilyen veszélyek kockázatát, és a jövőbeli fenyegetések szervizelés műveletek.</span><span class="sxs-lookup"><span data-stu-id="f1077-143">Clicking on a specific alert provides additional details and actions for investigating this threat and remediating future threats.</span></span>

  ![Navigációs ablaktábla][8]


## <a name="next-steps"></a><span data-ttu-id="f1077-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1077-145">Next steps</span></span>

* <span data-ttu-id="f1077-146">További információ a Fenyegetésészlelés, látogasson el a hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span><span class="sxs-lookup"><span data-stu-id="f1077-146">Learn more about Threat Detection, visit hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span></span> 
* <span data-ttu-id="f1077-147">További információ [Azure SQL Database Auditing](sql-database-auditing.md)</span><span class="sxs-lookup"><span data-stu-id="f1077-147">Learn more about [Azure SQL Database Auditing](sql-database-auditing.md)</span></span>
* <span data-ttu-id="f1077-148">További információ [Azure Security Centerben](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span><span class="sxs-lookup"><span data-stu-id="f1077-148">Learn more about [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span></span>
* <span data-ttu-id="f1077-149">Az árakkal kapcsolatos további tudnivalókért lásd: hello [lap SQL Database – díjszabás](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span><span class="sxs-lookup"><span data-stu-id="f1077-149">For more details on pricing, please see hello [SQL Database Pricing page](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span></span>  
* <span data-ttu-id="f1077-150">Tekintse meg a PowerShell-mintaparancsfájl [konfigurálhatja a naplózás és a fenyegetések észlelésére, a PowerShell használatával](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="f1077-150">For a PowerShell script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span></span>



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


