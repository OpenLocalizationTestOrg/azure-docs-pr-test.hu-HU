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
# <a name="sql-database-threat-detection"></a>SQL adatbázis fenyegetések észlelése

SQL Fenyegetésészlelés szokatlan és potenciálisan káros kísérletek tooaccess vagy biztonsági rés adatbázisok jelző rendellenes tevékenységeket észleli.

## <a name="overview"></a>Áttekintés

A Fenyegetésészlelés SQL biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló egy új réteget biztosít.  Felhasználók adatbázis gyanús tevékenységeket, a potenciális biztonsági réseket, és a SQL injektálási támadások, valamint rendellenes adatbázis hozzáférési minták riasztást kap. SQL Fenyegetésészlelés riasztásokat gyanús tevékenység részleteinek megadása, és hogyan művelet javasolja tooinvestigate és hello fenyegetést. Felhasználók hello gyanús események használatával felfedezheti [SQL Database Auditing](sql-database-auditing.md) toodetermine, ha egy kísérlet tooaccess eredő megsértik, vagy kihasznál hello adatbázis adatait. A Fenyegetésészlelés teszi egyszerű tooaddress potenciális fenyegetések toohello adatbázis hello kell toobe egy biztonsági szakértő nélkül, vagy speciális biztonsági rendszerek figyelése kezelése.

Például SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások. A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások alkalmazás mezőkbe megsértése vagy hello adatbázis adatainak módosítása.

SQL Fenyegetésészlelés integrálja riasztások [az Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), és minden védett SQL-adatbáziskiszolgáló alapján számlázzuk ki hello azonos ár, Azure Security Center szabványos rétegként $15/csomópont/hónappal, ahol minden védett SQL Adatbázis-kiszolgáló egy csomópont számítanak. Szeretnénk felhívni a tootry azt a 60 napig ingyenes. 

## <a name="set-up-threat-detection-for-your-database-in-hello-azure-portal"></a>A fenyegetésészlelés az adatbázis az Azure-portálon hello beállítása
1. Indítsa el a hello Azure portál, [https://portal.azure.com](https://portal.azure.com).
2. Keresse meg a toohello konfigurációs panelen hello toomonitor kívánt SQL-adatbázis található. Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**. 
    ![Navigációs ablaktábla][1]
3. A hello **naplózás és Fenyegetésészlelés** konfigurációs panelen kapcsolja **ON** naplózási megjelenítő hello fenyegetés észlelési beállítások.
  
    ![Navigációs ablaktábla][2]
4. Kapcsolja be **ON** veszélyforrások detektálása.
5. Konfigurálhatja az e-mailek adatbázist érintő rendellenes tevékenységeket észlelése a biztonsági riasztásokat fogadó hello listáját.
6. Kattintson a **mentése** a hello **naplózási & Threat detection** panel toosave hello új vagy frissített naplózás és a fenyegetések észlelési beállítások.
       
    ![Navigációs ablaktábla][3]

## <a name="set-up-threat-detection-using-powershell"></a>Állítsa be a fenyegetésészlelés PowerShell használatával

Tekintse meg a parancsfájl például [konfigurálhatja a naplózás és a fenyegetések észlelésére, a PowerShell használatával](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Fedezze fel az adatbázist érintő rendellenes tevékenységeket gyanús esemény észlelése
1. A adatbázist érintő rendellenes tevékenységeket észlelésekor e-mailben értesítést fog kapni. <br/>
   hello e-mail hello gyanús biztonsági esemény hello rendellenes tevékenységek, az adatbázis neve, a kiszolgáló neve, a alkalmazásnév és a hello esemény időpontja hello jellege beleértve tájékoztatást fogunk adni. Hello e-mailek emellett információt nyújt a lehetséges okok és a javasolt műveletek tooinvestigate, és hello potenciális fenyegetést toohello adatbázis mérséklése.<br/>
     
    ![Navigációs ablaktábla][4]
2. hello e-mail riasztás egy közvetlen hivatkozást toohello SQL Audit napló tartalmazza. Kattintson a hivatkozás elindítja hello Azure portálon, és megnyílik hello SQL auditálási rekordok hello gyanús esemény hello időpontja körül. Kattintson egy naplózási rekord tooview a további részleteket a hello adatbázis gyanús tevékenységeket, így könnyebben toofind hello SQL-utasítások, amelyek végrehajtódtak (ki fért, tevékenységük és mikor) és döntse el, ha hello esemény jogos vagy rosszindulatú volt-e (pl. alkalmazás a biztonsági rés tooSQL injektálási kihasználni, valaki megszegése bizalmas adatokat, stb.).<br/>
   ![Navigációs ablaktábla][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-hello-azure-portal"></a>Fedezze fel az adatbázis hello Azure-portálon a figyelmeztetések

SQL-adatbázis Fenyegetésészlelés integrálja a riasztások [az Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/). Élő SQL biztonság csempe belül hello adatbázis panel az aktív fenyegetések hello Azure portál nyomon követi hello állapotát. 

   ![Navigációs ablaktábla][6]
   
1. A hello SQL biztonsági csempére kattintva hello Azure Security Center riasztások panel elindul, és aktív SQL fenyegetést észlel az hello adatbázis áttekintése. 

  ![Navigációs ablaktábla][7]

2. Egy meghatározott riasztásra kattintva jelenít meg további adatokat, és vizsgálja az ilyen veszélyek kockázatát, és a jövőbeli fenyegetések szervizelés műveletek.

  ![Navigációs ablaktábla][8]


## <a name="next-steps"></a>Következő lépések

* További információ a Fenyegetésészlelés, látogasson el a hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* További információ [Azure SQL Database Auditing](sql-database-auditing.md)
* További információ [Azure Security Centerben](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)
* Az árakkal kapcsolatos további tudnivalókért lásd: hello [lap SQL Database – díjszabás](https://azure.microsoft.com/en-us/pricing/details/sql-database/)  
* Tekintse meg a PowerShell-mintaparancsfájl [konfigurálhatja a naplózás és a fenyegetések észlelésére, a PowerShell használatával](scripts/sql-database-auditing-and-threat-detection-powershell.md)



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


