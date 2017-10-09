---
title: "Azure-portálon: SQL-adatbázis dinamikus adatmaszkolási |} Microsoft Docs"
description: "Hogyan tooget el az SQL-adatbázis dinamikus adatmaszkolási a hello Azure portál"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 5c5f74682a15d42eceb78c3ca0705401e1ac0ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-hello-azure-portal"></a>Az SQL-adatbázis dinamikus adatmaszkolási hello Azure portálon az első lépései

Ez a témakör bemutatja, hogyan tooimplement [dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md) a hello Azure-portálon. Dinamikus adatok maszkolása használatával is megvalósíthatja [Azure SQL Database parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) vagy hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-hello-azure-portal"></a>Hello Azure portál használata az adatbázis dinamikus adatmaszkolási beállítása
1. Indítsa el az Azure portálon, a hello [https://portal.azure.com](https://portal.azure.com).
2. Keresse meg a kívánt toomask hello bizalmas adatokat tartalmazó adatbázis hello toohello beállítások panel.
3. Kattintson a hello **dinamikus Adatmaszkolási** csempe, amely elindítja a hello **dinamikus Adatmaszkolási** konfigurációs panelen.
   
   * Azt is megteheti, hogy is görgessen lefelé toohello **műveletek** szakaszt, és kattintson **dinamikus Adatmaszkolási**.
     
     ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. A hello **dinamikus Adatmaszkolási** konfigurációs panelen láthatja, hogy néhány adatbázisoszlopok hello javaslatok motor van megjelölve, maszkolási. A rendezés tooaccept hello javaslatok, kattintson az imént **maszk hozzáadása** egy vagy több oszlopot és egy maszk jön létre hello Ez az oszlop alapértelmezett típusa alapján. Hello függvény maszkolás hello maszkolási szabályra kattintva, és a mező formátuma tooa más formátumú az Ön által választott maszkolás hello szerkesztésével módosíthatja. Lehet, hogy tooclick **mentése** toosave a beállításokat.
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. egyetlen oszlop sem az adatbázis hello hello tetején maszkot tooadd **dinamikus Adatmaszkolási** konfigurációs panelen kattintson **maszk hozzáadása** tooopen hello **maszkolás szabály hozzáadása** konfigurációs panel
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Jelölje be hello **séma**, **tábla** és **oszlop** toodefine hello kapja meg, hogy legyen maszkolva mező.
7. Válasszon egy **maszkolás mező formátuma** hello bizalmas adatok maszkolása kategóriák listája.
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Kattintson a **mentése** hello adatmaszkolási szabály panel tooupdate hello készlete maszkolás dinamikus adatmaszkolási hello házirend szabályai a.
9. Típus hello SQL-felhasználók vagy AAD identitásokat tartalmaz, amelyek maszkolási ki kell zárni, és hozzáférést fedve toohello bizalmas adatokat. Felhasználók pontosvesszővel elválasztott listája legyen. Ne feledje, hogy rendszergazdai jogokkal rendelkező felhasználók mindig access toohello eredeti látható adatokat.
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > ezért hello a alkalmazásréteg toomake is bizalmas adatok privilegizált alkalmazás felhasználóinak megjelenítése, hello SQL-felhasználó hozzáadása vagy AAD-identitása hello alkalmazást tooquery hello adatbázist használ. Erősen ajánlott, hogy a lista tartalmazza-e megfelelő jogosultsággal rendelkező felhasználók toominimize a hello bizalmas adatok felfedésnek minimális száma.
   > 
   > 
10. Kattintson a **mentése** a hello adatmaszkolási konfigurációs panel toosave hello új vagy frissített maszkolási házirend.


## <a name="next-steps"></a>Következő lépések

* Dinamikus adatmaszkolási áttekintését lásd: [dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md).
* Dinamikus adatok maszkolása használatával is megvalósíthatja [Azure SQL Database parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) vagy hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).
