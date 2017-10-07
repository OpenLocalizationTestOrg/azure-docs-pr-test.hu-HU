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
# <a name="get-started-with-threat-detection"></a>A fenyegetésészlelés az első lépései
> [!div class="op_single_selector"]
> * [Naplózás](sql-data-warehouse-auditing-overview.md)
> * [Fenyegetések észlelése](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Áttekintés
A Fenyegetésészlelés az adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli. A Fenyegetésészlelés preview, és az SQL Data Warehouse támogatja.

A Fenyegetésészlelés biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló új réteget biztosít. Felhasználók hello gyanús események használatával felfedezheti [Azure SQL Data Warehouse-naplózás](sql-data-warehouse-auditing-overview.md) toodetermine, ha egy kísérlet tooaccess eredő megsértik vagy hello adatraktár adatai kihasználására.
A Fenyegetésészlelés teszi a potenciális fenyegetések egyszerű tooaddress toohello adatok nélkül hello kell toobe adatraktár egy biztonsági szakértő, vagy kezelje a speciális biztonsági rendszerek figyelése.

A Fenyegetésészlelés például bizonyos adatbázist érintő rendellenes tevékenységeket utaló esetleges SQL injektálási kísérletek begyűjtésére észleli. SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások. A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások mezőkbe alkalmazás bejegyzést, megsértése vagy hello adatbázis adatainak módosítása.

## <a name="set-up-threat-detection-for-your-database"></a>A fenyegetésészlelés az adatbázis beállítása
1. Indítsa el az Azure portálon, a hello [https://portal.azure.com](https://portal.azure.com).
2. Keresse meg a toohello konfigurációs panelen található az SQL Data Warehouse toomonitor kívánt hello. Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**.
   
    ![Navigációs ablaktábla][1]
3. A hello **naplózás és Fenyegetésészlelés** konfigurációs panelen kapcsolja **ON** naplózás, amely megjeleníti hello fenyegetés észlelési beállítások.
   
    ![Navigációs ablaktábla][2]
4. Kapcsolja be **ON** veszélyforrások detektálása.
5. Konfigurálhatja a hello listáját, amelyek megkapják a biztonsági riasztások adatraktár rendellenes tevékenységek észlelésekor e-maileket.
6. Kattintson a **mentése** a hello **naplózási & Threat detection** konfigurációs panel toosave hello új vagy frissített naplózás és a fenyegetések szabályzat.
   
    ![Navigációs ablaktábla][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Megismerkedhet a rendellenes data warehouse tevékenységek egy gyanús esemény észlelése
1. A adatbázist érintő rendellenes tevékenységeket észlelésekor e-mailben értesítést fog kapni. <br/>
   hello e-mail hello gyanús biztonsági esemény hello rendellenes tevékenységek, az adatbázis neve, a kiszolgáló nevét és hello esemény időpontja hello jellege beleértve tájékoztatást fogunk adni. Emellett az információt nyújt a lehetséges okok és ajánlott műveletek tooinvestigate és hello potenciális fenyegetést toohello adatbázis mérsékelni.<br/>
   
    ![Navigációs ablaktábla][4]
2. Hello e-mailben, kattintson a hello **Azure SQL-naplózás napló** hivatkozás, amely indítsa el a klasszikus Azure portál hello és hello vonatkozó naplózási bejegyzések hello gyanús esemény hello időpontja körül megjelenítése.
   
    ![Navigációs ablaktábla][5]
3. Kattintson a hello naplózási bejegyzések tooview további részleteket a hello adatbázis gyanús tevékenységek, például az SQL-utasítás hiba okát, és az ügyfél IP.
   
    ![Navigációs ablaktábla][6]
4. Hello rekordok naplózása paneljén kattintson **Megnyitás az Excelben** előre konfigurált tooopen excel-sablon tooimport és futtatási mélyebb elemzésének hello audit napló hello gyanús esemény hello időpontja körül.<br/>
   **Megjegyzés:** az Excel 2010 vagy újabb, a Power Query és hello **gyors összevonás** beállításra akkor szükség
   
    ![Navigációs ablaktábla][7]
5. tooconfigure hello **gyors összevonás** beállítás - hello **POWER QUERY** menüszalag lapon jelölje be **beállítások** toodisplay hello beállítások párbeszédpanelen. Jelölje ki hello adatvédelmi szakaszt, és válassza ki a hello második lehetőség - "Hello adatvédelmi szintek figyelmen kívül, és a teljesítmény lehetséges javítása":
   
    ![Navigációs ablaktábla][8]
6. tooload SQL naplókat, győződjön meg arról, hogy hello paraméterek hello beállítások lapon helyesen van beállítva és majd hello "Data" menüszalagon válassza hello "Az összes frissítés" gombra.
   
    ![Navigációs ablaktábla][9]
7. hello eredményei jelennek meg hello **SQL naplók** lap, amely mélyebb elemzése toorun hello rendellenes tevékenységet észlelt, és csökkenteni hello hatását hello biztonsági esemény az alkalmazás lehetővé teszi.

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
