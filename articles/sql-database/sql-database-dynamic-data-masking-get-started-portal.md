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
# <a name="get-started-with-sql-database-dynamic-data-masking-with-hello-azure-portal"></a><span data-ttu-id="a1b49-103">Az SQL-adatbázis dinamikus adatmaszkolási hello Azure portálon az első lépései</span><span class="sxs-lookup"><span data-stu-id="a1b49-103">Get started with SQL Database dynamic data masking with hello Azure Portal</span></span>

<span data-ttu-id="a1b49-104">Ez a témakör bemutatja, hogyan tooimplement [dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a1b49-104">This topic shows you how tooimplement [dynamic data masking](sql-database-dynamic-data-masking-get-started.md) with hello Azure portal.</span></span> <span data-ttu-id="a1b49-105">Dinamikus adatok maszkolása használatával is megvalósíthatja [Azure SQL Database parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) vagy hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1b49-105">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>


## <a name="set-up-dynamic-data-masking-for-your-database-using-hello-azure-portal"></a><span data-ttu-id="a1b49-106">Hello Azure portál használata az adatbázis dinamikus adatmaszkolási beállítása</span><span class="sxs-lookup"><span data-stu-id="a1b49-106">Set up dynamic data masking for your database using hello Azure Portal</span></span>
1. <span data-ttu-id="a1b49-107">Indítsa el az Azure portálon, a hello [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a1b49-107">Launch hello Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a1b49-108">Keresse meg a kívánt toomask hello bizalmas adatokat tartalmazó adatbázis hello toohello beállítások panel.</span><span class="sxs-lookup"><span data-stu-id="a1b49-108">Navigate toohello settings blade of hello database that includes hello sensitive data you want toomask.</span></span>
3. <span data-ttu-id="a1b49-109">Kattintson a hello **dinamikus Adatmaszkolási** csempe, amely elindítja a hello **dinamikus Adatmaszkolási** konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="a1b49-109">Click hello **Dynamic Data Masking** tile which launches hello **Dynamic Data Masking** configuration blade.</span></span>
   
   * <span data-ttu-id="a1b49-110">Azt is megteheti, hogy is görgessen lefelé toohello **műveletek** szakaszt, és kattintson **dinamikus Adatmaszkolási**.</span><span class="sxs-lookup"><span data-stu-id="a1b49-110">Alternatively, you can scroll down toohello **Operations** section and click **Dynamic Data Masking**.</span></span>
     
     ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. <span data-ttu-id="a1b49-112">A hello **dinamikus Adatmaszkolási** konfigurációs panelen láthatja, hogy néhány adatbázisoszlopok hello javaslatok motor van megjelölve, maszkolási.</span><span class="sxs-lookup"><span data-stu-id="a1b49-112">In hello **Dynamic Data Masking** configuration blade you may see some database columns that hello recommendations engine has flagged for masking.</span></span> <span data-ttu-id="a1b49-113">A rendezés tooaccept hello javaslatok, kattintson az imént **maszk hozzáadása** egy vagy több oszlopot és egy maszk jön létre hello Ez az oszlop alapértelmezett típusa alapján.</span><span class="sxs-lookup"><span data-stu-id="a1b49-113">In order tooaccept hello recommendations, just click **Add Mask** for one or more columns and a mask will be created based on hello default type for this column.</span></span> <span data-ttu-id="a1b49-114">Hello függvény maszkolás hello maszkolási szabályra kattintva, és a mező formátuma tooa más formátumú az Ön által választott maszkolás hello szerkesztésével módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a1b49-114">You can change hello masking function by clicking on hello masking rule and editing hello masking field format tooa different format of your choice.</span></span> <span data-ttu-id="a1b49-115">Lehet, hogy tooclick **mentése** toosave a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a1b49-115">Be sure tooclick **Save** toosave your settings.</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. <span data-ttu-id="a1b49-117">egyetlen oszlop sem az adatbázis hello hello tetején maszkot tooadd **dinamikus Adatmaszkolási** konfigurációs panelen kattintson **maszk hozzáadása** tooopen hello **maszkolás szabály hozzáadása** konfigurációs panel</span><span class="sxs-lookup"><span data-stu-id="a1b49-117">tooadd a mask for any column in your database, at hello top of hello **Dynamic Data Masking** configuration blade click **Add Mask** tooopen hello **Add Masking Rule** configuration blade</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. <span data-ttu-id="a1b49-119">Jelölje be hello **séma**, **tábla** és **oszlop** toodefine hello kapja meg, hogy legyen maszkolva mező.</span><span class="sxs-lookup"><span data-stu-id="a1b49-119">Select hello **Schema**, **Table** and **Column** toodefine hello designated field that will be masked.</span></span>
7. <span data-ttu-id="a1b49-120">Válasszon egy **maszkolás mező formátuma** hello bizalmas adatok maszkolása kategóriák listája.</span><span class="sxs-lookup"><span data-stu-id="a1b49-120">Choose a **Masking Field Format** from hello list of sensitive data masking categories.</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. <span data-ttu-id="a1b49-122">Kattintson a **mentése** hello adatmaszkolási szabály panel tooupdate hello készlete maszkolás dinamikus adatmaszkolási hello házirend szabályai a.</span><span class="sxs-lookup"><span data-stu-id="a1b49-122">Click **Save** in hello data masking rule blade tooupdate hello set of masking rules in hello dynamic data masking policy.</span></span>
9. <span data-ttu-id="a1b49-123">Típus hello SQL-felhasználók vagy AAD identitásokat tartalmaz, amelyek maszkolási ki kell zárni, és hozzáférést fedve toohello bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="a1b49-123">Type hello SQL users or AAD identities that should be excluded from masking, and have access toohello unmasked sensitive data.</span></span> <span data-ttu-id="a1b49-124">Felhasználók pontosvesszővel elválasztott listája legyen.</span><span class="sxs-lookup"><span data-stu-id="a1b49-124">This should be a semicolon-separated list of users.</span></span> <span data-ttu-id="a1b49-125">Ne feledje, hogy rendszergazdai jogokkal rendelkező felhasználók mindig access toohello eredeti látható adatokat.</span><span class="sxs-lookup"><span data-stu-id="a1b49-125">Note that users with administrator privileges always have access toohello original unmasked data.</span></span>
   
    ![Navigációs ablaktábla](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > <span data-ttu-id="a1b49-127">ezért hello a alkalmazásréteg toomake is bizalmas adatok privilegizált alkalmazás felhasználóinak megjelenítése, hello SQL-felhasználó hozzáadása vagy AAD-identitása hello alkalmazást tooquery hello adatbázist használ.</span><span class="sxs-lookup"><span data-stu-id="a1b49-127">toomake it so hello application layer can display sensitive data for application privileged users, add hello SQL user or AAD identity hello application uses tooquery hello database.</span></span> <span data-ttu-id="a1b49-128">Erősen ajánlott, hogy a lista tartalmazza-e megfelelő jogosultsággal rendelkező felhasználók toominimize a hello bizalmas adatok felfedésnek minimális száma.</span><span class="sxs-lookup"><span data-stu-id="a1b49-128">It is highly recommended that this list contain a minimal number of privileged users toominimize exposure of hello sensitive data.</span></span>
   > 
   > 
10. <span data-ttu-id="a1b49-129">Kattintson a **mentése** a hello adatmaszkolási konfigurációs panel toosave hello új vagy frissített maszkolási házirend.</span><span class="sxs-lookup"><span data-stu-id="a1b49-129">Click **Save** in hello data masking configuration blade toosave hello new or updated masking policy.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a1b49-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1b49-130">Next steps</span></span>

* <span data-ttu-id="a1b49-131">Dinamikus adatmaszkolási áttekintését lásd: [dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a1b49-131">For an overview of dynamic data masking, see [dynamic data masking](sql-database-dynamic-data-masking-get-started.md).</span></span>
* <span data-ttu-id="a1b49-132">Dinamikus adatok maszkolása használatával is megvalósíthatja [Azure SQL Database parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) vagy hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1b49-132">You can also implement dynamic data masking using [Azure SQL Database cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) or hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).</span></span>
