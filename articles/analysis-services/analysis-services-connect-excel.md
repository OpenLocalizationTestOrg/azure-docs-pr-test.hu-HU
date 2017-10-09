---
title: Analysis Services Excel aaaConnect tooAzure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconnect tooan Azure Analysis Services Excel használatával."
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
ms.openlocfilehash: 175b83e7d936716a626aa4b3bf22b5598bb983d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-excel"></a><span data-ttu-id="4005d-103">Csatlakozás az Excellel</span><span class="sxs-lookup"><span data-stu-id="4005d-103">Connect with Excel</span></span>

<span data-ttu-id="4005d-104">Miután elkészítette a kiszolgáló Azure-ban, és telepített egy táblázatos modell tooit, Ön készen tooconnect és adatok megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="4005d-104">Once you've created a server in Azure, and deployed a tabular model tooit, you're ready tooconnect and begin exploring data.</span></span>


## <a name="connect-in-excel"></a><span data-ttu-id="4005d-105">Csatlakozás az Excel programban</span><span class="sxs-lookup"><span data-stu-id="4005d-105">Connect in Excel</span></span>

<span data-ttu-id="4005d-106">Kapcsolódás Excel tooa kiszolgáló támogatja az Excel 2016 adatok beolvasása paranccsal.</span><span class="sxs-lookup"><span data-stu-id="4005d-106">Connecting tooa server in Excel is supported by using Get Data in Excel 2016.</span></span> <span data-ttu-id="4005d-107">Nem támogatott a Kapcsolódás a Power Pivot hello importálása varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="4005d-107">Connecting by using hello Import Table Wizard in Power Pivot is not supported.</span></span> 

<span data-ttu-id="4005d-108">**az Excel 2016 tooconnect**</span><span class="sxs-lookup"><span data-stu-id="4005d-108">**tooconnect in Excel 2016**</span></span>

1. <span data-ttu-id="4005d-109">Az Excel 2016, az hello **adatok** menüszalag, kattintson a **külső adatok beolvasása** > **egyéb forrásokból származó** > **Analysis Services** .</span><span class="sxs-lookup"><span data-stu-id="4005d-109">In Excel 2016, on hello **Data** ribbon, click **Get External Data** > **From Other Sources** > **From Analysis Services**.</span></span>

2. <span data-ttu-id="4005d-110">A hello Adatkapcsolat varázsló, a **kiszolgálónév**, többek között a protokollt és URI hello kiszolgálónevet adja meg.</span><span class="sxs-lookup"><span data-stu-id="4005d-110">In hello Data Connection Wizard, in **Server name**, enter hello server name including protocol and URI.</span></span> <span data-ttu-id="4005d-111">Ezt követően a **bejelentkezési adatok**, jelölje be **a következő felhasználónevet és jelszót használja hello**, majd írja be például a hello szervezeti felhasználónevet, nancy@adventureworks.com, és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="4005d-111">Then, in **Logon credentials**, select **Use hello following User Name and Password**, and then type hello organizational user name, for example nancy@adventureworks.com, and password.</span></span>

    ![Az Excel bejelentkezéstől csatlakozás](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. <span data-ttu-id="4005d-113">A **adatbázis és tábla kijelölése**, hello adatbázis és a modell vagy perspektíva, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="4005d-113">In **Select Database and Table**, select hello database and model or perspective, and then click **Finish**.</span></span>
   
    ![Kapcsolódás Excel válassza modellből](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a><span data-ttu-id="4005d-115">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4005d-115">See also</span></span>
<span data-ttu-id="4005d-116">[Ügyfél-függvénytárak](analysis-services-data-providers.md) </span><span class="sxs-lookup"><span data-stu-id="4005d-116">[Client libraries](analysis-services-data-providers.md) </span></span>  
[<span data-ttu-id="4005d-117">A kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="4005d-117">Manage your server</span></span>](analysis-services-manage.md)     


