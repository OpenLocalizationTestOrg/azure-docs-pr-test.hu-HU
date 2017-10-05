---
title: "Azure-portálon: SQL-adatbázis dinamikus adatmaszkolási |} Microsoft Docs"
description: "SQL-adatbázis dinamikus adatmaszkolási az Azure portálon az első lépések"
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
ms.openlocfilehash: 15184e14d4e1e23b56126bbf9f972c1619dcba80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a><span data-ttu-id="f34f8-103">Ismerkedés az SQL-adatbázis dinamikus adatmaszkolási az Azure portállal</span><span class="sxs-lookup"><span data-stu-id="f34f8-103">Get started with SQL Database dynamic data masking with the Azure Portal</span></span>

<span data-ttu-id="f34f8-104">Ez a témakör bemutatja, hogyan megvalósításához [dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f34f8-104">This topic shows you how to implement [dynamic data masking](sql-database-dynamic-data-masking-get-started.md) with the Azure portal.</span></span> <span data-ttu-id="f34f8-105">Dinamikus adatok maszkolása használatával is megvalósíthatja [Azure SQL Database parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) vagy a [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="f34f8-105">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or the [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a><span data-ttu-id="f34f8-106">Az Azure portál használata az adatbázis dinamikus adatmaszkolási beállítása</span><span class="sxs-lookup"><span data-stu-id="f34f8-106">Set up dynamic data masking for your database using the Azure Portal</span></span>
1. <span data-ttu-id="f34f8-107">Indítsa el az Azure portálon, a [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f34f8-107">Launch the Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f34f8-108">Nyissa meg a beállítások panelről a maszkolandó bizalmas adatokat tartalmazó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f34f8-108">Navigate to the settings blade of the database that includes the sensitive data you want to mask.</span></span>
3. <span data-ttu-id="f34f8-109">Kattintson a **dinamikus Adatmaszkolási** csempe, amely elindítja a **dinamikus Adatmaszkolási** konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="f34f8-109">Click the **Dynamic Data Masking** tile which launches the **Dynamic Data Masking** configuration blade.</span></span>
   
   * <span data-ttu-id="f34f8-110">Azt is megteheti, hogy is görgessen le a **műveletek** szakaszt, és kattintson **dinamikus Adatmaszkolási**.</span><span class="sxs-lookup"><span data-stu-id="f34f8-110">Alternatively, you can scroll down to the **Operations** section and click **Dynamic Data Masking**.</span></span>
     
     ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. <span data-ttu-id="f34f8-112">Az a **dinamikus Adatmaszkolási** konfigurációs panelen láthatja maszkolási van megjelölve, a javaslatok motor adatbázis oszlopok.</span><span class="sxs-lookup"><span data-stu-id="f34f8-112">In the **Dynamic Data Masking** configuration blade you may see some database columns that the recommendations engine has flagged for masking.</span></span> <span data-ttu-id="f34f8-113">Ahhoz, hogy fogadja el a javaslatok, egyszerűen kattintson **maszk hozzáadása** egy vagy több oszlopot és egy maszk jön létre az oszlop alapértelmezett típusa alapján.</span><span class="sxs-lookup"><span data-stu-id="f34f8-113">In order to accept the recommendations, just click **Add Mask** for one or more columns and a mask will be created based on the default type for this column.</span></span> <span data-ttu-id="f34f8-114">A maszkolási függvényt a maszkolási szabályra kattintva, és a maszkolás szerkesztésével módosíthatja az Ön által választott más formátumba mező formátuma.</span><span class="sxs-lookup"><span data-stu-id="f34f8-114">You can change the masking function by clicking on the masking rule and editing the masking field format to a different format of your choice.</span></span> <span data-ttu-id="f34f8-115">Ügyeljen arra, hogy kattintson **mentése** a beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="f34f8-115">Be sure to click **Save** to save your settings.</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. <span data-ttu-id="f34f8-117">Egy maszk bármelyik oszlop tetején, az adatbázis hozzáadása a **dinamikus Adatmaszkolási** konfigurációs panelen kattintson **maszk hozzáadása** megnyitásához a **maszkolás szabály hozzáadása** konfigurációs panel</span><span class="sxs-lookup"><span data-stu-id="f34f8-117">To add a mask for any column in your database, at the top of the **Dynamic Data Masking** configuration blade click **Add Mask** to open the **Add Masking Rule** configuration blade</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. <span data-ttu-id="f34f8-119">Válassza ki a **séma**, **tábla** és **oszlop** legyen maszkolva a kijelölt mező meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="f34f8-119">Select the **Schema**, **Table** and **Column** to define the designated field that will be masked.</span></span>
7. <span data-ttu-id="f34f8-120">Válasszon egy **maszkolás mező formátuma** bizalmas adatok maszkolása kategóriák közül.</span><span class="sxs-lookup"><span data-stu-id="f34f8-120">Choose a **Masking Field Format** from the list of sensitive data masking categories.</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. <span data-ttu-id="f34f8-122">Kattintson a **mentése** a a adatmaszkolási szabály panelt, és a dinamikus adatmaszkolási házirend szabályai maszkolás frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f34f8-122">Click **Save** in the data masking rule blade to update the set of masking rules in the dynamic data masking policy.</span></span>
9. <span data-ttu-id="f34f8-123">Írja be az SQL-felhasználók vagy AAD identitások, a maszkolásból ki kell zárni, és a maszkolás bizalmas adatokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="f34f8-123">Type the SQL users or AAD identities that should be excluded from masking, and have access to the unmasked sensitive data.</span></span> <span data-ttu-id="f34f8-124">Felhasználók pontosvesszővel elválasztott listája legyen.</span><span class="sxs-lookup"><span data-stu-id="f34f8-124">This should be a semicolon-separated list of users.</span></span> <span data-ttu-id="f34f8-125">Ne feledje, hogy rendszergazdai jogokkal rendelkező felhasználók mindig az eredeti látható adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="f34f8-125">Note that users with administrator privileges always have access to the original unmasked data.</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > <span data-ttu-id="f34f8-127">Ahhoz, az alkalmazási rétegre megjeleníthető kiemelt alkalmazások felhasználók bizalmas adatait, adja hozzá az SQL-felhasználó vagy az adatbázis lekérdezésének használja az alkalmazás AAD-identitása.</span><span class="sxs-lookup"><span data-stu-id="f34f8-127">To make it so the application layer can display sensitive data for application privileged users, add the SQL user or AAD identity the application uses to query the database.</span></span> <span data-ttu-id="f34f8-128">Erősen ajánlott, hogy a lista tartalmazza-e megfelelő jogosultsággal rendelkező felhasználók a bizalmas adatok felfedésnek minimalizálása érdekében a lehető legkevesebb.</span><span class="sxs-lookup"><span data-stu-id="f34f8-128">It is highly recommended that this list contain a minimal number of privileged users to minimize exposure of the sensitive data.</span></span>
   > 
   > 
10. <span data-ttu-id="f34f8-129">Kattintson a **mentése** az adatok maszkolása konfigurációs panelt, és mentse az új vagy frissített maszkolás házirend.</span><span class="sxs-lookup"><span data-stu-id="f34f8-129">Click **Save** in the data masking configuration blade to save the new or updated masking policy.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f34f8-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f34f8-130">Next steps</span></span>

* <span data-ttu-id="f34f8-131">Dinamikus adatmaszkolási áttekintését lásd: [dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f34f8-131">For an overview of dynamic data masking, see [dynamic data masking](sql-database-dynamic-data-masking-get-started.md).</span></span>
* <span data-ttu-id="f34f8-132">Dinamikus adatok maszkolása használatával is megvalósíthatja [Azure SQL Database parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) vagy a [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="f34f8-132">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or the [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>