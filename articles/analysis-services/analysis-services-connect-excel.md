---
title: "Kapcsolódás Azure Analysis Services Excel |} Microsoft Docs"
description: "Útmutató az Azure Analysis Services-kiszolgálóhoz kapcsolódni az Excel használatával."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: d51b6980120f1cf9bc8d053d463a73ac600b915f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-excel"></a><span data-ttu-id="c603e-103">Csatlakozás az Excellel</span><span class="sxs-lookup"><span data-stu-id="c603e-103">Connect with Excel</span></span>

<span data-ttu-id="c603e-104">Miután elkészítette a kiszolgáló Azure-ban, és telepített egy táblázatos modell, készen áll csatlakozni, és az adatok megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="c603e-104">Once you've created a server in Azure, and deployed a tabular model to it, you're ready to connect and begin exploring data.</span></span>


## <a name="connect-in-excel"></a><span data-ttu-id="c603e-105">Csatlakozás az Excel programban</span><span class="sxs-lookup"><span data-stu-id="c603e-105">Connect in Excel</span></span>

<span data-ttu-id="c603e-106">Adatok beolvasása használata az Excel 2016 az Excel programban a kiszolgálóhoz való kapcsolódás esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="c603e-106">Connecting to a server in Excel is supported by using Get Data in Excel 2016.</span></span> <span data-ttu-id="c603e-107">Nem támogatott a Kapcsolódás a Power Pivot tábla importálása varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="c603e-107">Connecting by using the Import Table Wizard in Power Pivot is not supported.</span></span> 

<span data-ttu-id="c603e-108">**Az Excel 2016 kapcsolódni**</span><span class="sxs-lookup"><span data-stu-id="c603e-108">**To connect in Excel 2016**</span></span>

1. <span data-ttu-id="c603e-109">Az Excel 2016-ot, a **adatok** menüszalag, kattintson a **külső adatok beolvasása** > **egyéb forrásokból származó** > **Analysis Services** .</span><span class="sxs-lookup"><span data-stu-id="c603e-109">In Excel 2016, on the **Data** ribbon, click **Get External Data** > **From Other Sources** > **From Analysis Services**.</span></span>

2. <span data-ttu-id="c603e-110">Az Adatkapcsolat varázsló a **kiszolgálónév**, adja meg a kiszolgáló nevét, többek között a protokollt és URI-t.</span><span class="sxs-lookup"><span data-stu-id="c603e-110">In the Data Connection Wizard, in **Server name**, enter the server name including protocol and URI.</span></span> <span data-ttu-id="c603e-111">Ezt követően a **bejelentkezési adatok**, jelölje be **használja a következő felhasználónév és jelszó**, és írja be a szervezeti felhasználók nevét, például nancy@adventureworks.com, és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c603e-111">Then, in **Logon credentials**, select **Use the following User Name and Password**, and then type the organizational user name, for example nancy@adventureworks.com, and password.</span></span>

    ![Az Excel bejelentkezéstől csatlakozás](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. <span data-ttu-id="c603e-113">A **adatbázis és tábla kijelölése**, az adatbázis és a modell vagy perspektíva, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="c603e-113">In **Select Database and Table**, select the database and model or perspective, and then click **Finish**.</span></span>
   
    ![Kapcsolódás Excel válassza modellből](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a><span data-ttu-id="c603e-115">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c603e-115">See also</span></span>
<span data-ttu-id="c603e-116">[Ügyfél-függvénytárak](analysis-services-data-providers.md) </span><span class="sxs-lookup"><span data-stu-id="c603e-116">[Client libraries](analysis-services-data-providers.md) </span></span>  
[<span data-ttu-id="c603e-117">A kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="c603e-117">Manage your server</span></span>](analysis-services-manage.md)     


