---
title: "Azure-portálon: SQL Database georeplikációja |} Microsoft Docs"
description: "Georeplikáció konfigurálása az Azure SQL Database hello Azure-portálon és kezdeményezhet feladatátvevő"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/06/2016
ms.author: carlrab
ms.openlocfilehash: 09cbbdb040f36c42593e3be87ce6db2238f36656
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-hello-azure-portal-and-initiate-failover"></a>Aktív georeplikáció konfigurálása az Azure SQL Database hello Azure-portálon és kezdeményezhet feladatátvételi

Ez a cikk bemutatja, hogyan tooconfigure aktív georeplikáció SQL-adatbázis a hello [Azure-portálon](http://portal.azure.com) és tooinitiate feladatátvételi.

hello Azure-portálon tooinitiate feladatátvétel lásd [kezdeményezze a tervezett vagy nem tervezett feladatátvétel az Azure SQL Database hello Azure-portálon](sql-database-geo-replication-portal.md).

tooconfigure aktív georeplikáció hello Azure-portál használatával, a következő erőforrás hello kell:

* Azure SQL-adatbázis: hello elsődleges adatbázis, amelyet az tooreplicate tooa különböző földrajzi régióban.

> [!Note]
Aktív georeplikáció hello adatbázisok közé kell esnie ugyanahhoz az előfizetéshez.

## <a name="add-a-secondary-database"></a>A másodlagos adatbázis hozzáadása
hello lépések hozzon létre egy új másodlagos adatbázis egy georeplikáció együttműködve.  

egy másodlagos adatbázis tooadd hello előfizetés tulajdonosa vagy a társtulajdonos kell lennie.

hello másodlagos adatbázis hello azonos nevet hello elsődleges adatbázisként van, és alapértelmezés szerint rendelkezik, hello azonos. hello másodlagos adatbázis egy adatbázis vagy a rugalmas készletekben található adatbázis lehet. További információkért lásd: [szolgáltatásszintek](sql-database-service-tiers.md).
Miután másodlagos hello létrehozták, és a rendezés, az adatok kezdete hello elsődleges adatbázis toohello új másodlagos adatbázis replikálásához.

> [!NOTE]
> Ha hello partner adatbázis már létezik (például miatt leállítja egy előző georeplikáció kapcsolat) hello parancs sikertelen lesz.
> 

1. A hello [Azure-portálon](http://portal.azure.com), keresse meg, hogy szeretné-e az georeplikáció szolgáltatáshoz tooset toohello adatbázis.
2. Hello SQL-adatbázis lapján válassza ki a **georeplikáció**, majd válassza ki a hello régió toocreate hello másodlagos adatbázist. Kiválaszthatja, hogy minden régióban hello régió hello elsődleges adatbázist üzemeltető, de azt javasoljuk, hogy hello [párosított régió](../best-practices-availability-paired-regions.md).
   
    ![Aktív georeplikáció konfigurálása](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Válassza ki, vagy konfigurálja a kiszolgáló hello és hello másodlagos adatbázis tarifacsomagjának.
   
    ![Másodlagos konfigurálása](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Ha szükséges egy másodlagos adatbázis-tooan rugalmas készletet is hozzáadhat. toocreate hello másodlagos adatbázis egy tárolókészletben, kattintson a **rugalmas készlet** , és válasszon ki egy készletet hello cél kiszolgálón. Egy készlet hello célkiszolgálón már léteznie kell. Ez a munkafolyamat nem hoz létre a készletet.
5. Kattintson a **létrehozása** tooadd hello másodlagos.
6. hello másodlagos adatbázis jön létre, és a folyamat összehangolása hello kezdődik.
   
    ![Másodlagos konfigurálása](./media/sql-database-geo-replication-portal/seeding0.png)
7. Hello összehangolása a folyamat befejeződése után a hello másodlagos adatbázis állapotát jeleníti meg.
   
    ![Teljes összehangolása](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Kezdeményezzen feladatátvételt

hello másodlagos adatbázis elsődleges kapcsolt toobecome hello lehet.  

1. A hello [Azure-portálon](http://portal.azure.com), keresse meg az elsődleges adatbázis toohello hello georeplikáció együttműködve.
2. Hello SQL-adatbázis paneljén válassza **összes beállítás** > **georeplikáció**.
3. A hello **másodlagos** lista, jelölje be hello adatbázis toobecome hello új elsődleges, és kattintson a **feladatátvételi**.
   
    ![feladatátvétel](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Kattintson a **Igen** toobegin hello feladatátvételi.

hello parancs azonnal kapcsolók hello másodlagos adatbázis hello elsődleges szerepkörhöz. 

Egy rövid időszak során, ami két adatbázis nem érhető el (a 0 too25 másodperc hello sorrendben) közben hello szerepkörök bekapcsolt állapotban van. Ha a hello elsődleges adatbázis több másodlagos adatbázist, hello parancs automatikusan Átkonfigurálás hello más másodlagos tooconnect toohello új elsődleges. hello teljes műveletet kell végrehajtani kisebb, mint egy perc toocomplete normál körülmények között. 

> [!NOTE]
> Ez a parancs a gyors helyreállításának hello adatbázis egy esetleges leállás esetén szolgál. Elindítja a feladatátvétel (kényszerített feladatátvételi) adatok szinkronizálás nélkül.  Ha elsődleges hello online állapotban, és a tranzakciók véglegesítése, amikor hello parancs adatvesztést fordulhat elő. 
> 
> 

## <a name="remove-secondary-database"></a>Távolítsa el a másodlagos adatbázis
Ez a művelet végleg megszakítja hello replikációs toohello másodlagos adatbázis, és módosításokat hello hello másodlagos tooa rendszeres olvasási és írási adatbázis szerepe. Ha hello kapcsolat toohello másodlagos adatbázis nem működő, hello parancs sikeresen befejeződik, de hello másodlagos biztosítja, nem lett írható-olvasható, amíg a kapcsolat helyreállítását követő rögzítésénél.  

1. A hello [Azure-portálon](http://portal.azure.com), keresse meg az elsődleges adatbázis toohello hello georeplikáció együttműködve.
2. Hello SQL-adatbázis lapján válassza ki a **georeplikáció**.
3. A hello **másodlagos** lista, jelölje be hello adatbázis tooremove hello georeplikáció partneri kapcsolat áll fenn a kívánt.
4. Kattintson a **állítsa le a replikációt**.
   
    ![Távolítsa el a másodlagos](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Megnyílik egy ablak. Kattintson a **Igen** tooremove hello adatbázis hello georeplikáció partneri kapcsolat áll fenn. (Állítani tooa olvasási és írási adatbázis nem része semmilyen replikációs.)

## <a name="next-steps"></a>Következő lépések
* További információ az aktív georeplikáció, toolearn lásd [aktív georeplikáció](sql-database-geo-replication-overview.md).
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).

