---
title: a Power BI Analysis Services aaaConnect tooAzure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconnect tooan Azure Analysis Services-kiszolgáló a Power BI használatával."
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
ms.openlocfilehash: f6c4cdec6edb92900ad2e552e23a4d9172ba9b84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-power-bi"></a><span data-ttu-id="1119d-103">Csatlakozás a Power BI</span><span class="sxs-lookup"><span data-stu-id="1119d-103">Connect with Power BI</span></span>

<span data-ttu-id="1119d-104">Miután elkészítette a kiszolgáló Azure-ban, és egy táblázatos modell tooit telepített, a szervezet felhasználói készen tooconnect és adatok.</span><span class="sxs-lookup"><span data-stu-id="1119d-104">Once you've created a server in Azure, and deployed a tabular model tooit, users in your organization are ready tooconnect and begin exploring data.</span></span> 

> [!TIP]
> <span data-ttu-id="1119d-105">Lehet, hogy toouse hello legújabb verziójának [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="1119d-105">Be sure toouse hello latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span></span>
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a><span data-ttu-id="1119d-106">Csatlakozás a Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="1119d-106">Connect in Power BI Desktop</span></span>

1. <span data-ttu-id="1119d-107">A Power BI Desktopban, kattintson a **adatok beolvasása** > **Azure** > **Azure Analysis Services-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="1119d-107">In Power BI Desktop, click **Get Data** > **Azure** > **Azure Analysis Services database**.</span></span>

2. <span data-ttu-id="1119d-108">A **Server**, adjon meg hello kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="1119d-108">In **Server**, enter hello server name.</span></span> 
    
    <span data-ttu-id="1119d-109">Lehet, hogy tooinclude hello teljes URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1119d-109">Be sure tooinclude hello full URL.</span></span> <span data-ttu-id="1119d-110">Például asazure://westcentralus.asazure.windows.net/advworks.</span><span class="sxs-lookup"><span data-stu-id="1119d-110">For example, asazure://westcentralus.asazure.windows.net/advworks.</span></span>

3. <span data-ttu-id="1119d-111">A **adatbázis**, ha tudja hello hello táblázatos modellű adatbázisnál vagy azt szeretné, hogy a, illessze be ide tooconnect perspektíva nevét.</span><span class="sxs-lookup"><span data-stu-id="1119d-111">In **Database**, if you know hello name of hello tabular model database or perspective you want tooconnect to, paste it here.</span></span> <span data-ttu-id="1119d-112">Ellenkező esetben hagyja üresen a mezőt, és jelöljön ki egy adatbázist vagy perspektíva később.</span><span class="sxs-lookup"><span data-stu-id="1119d-112">Otherwise, you can leave this field blank and select a database or perspective later.</span></span>

4. <span data-ttu-id="1119d-113">Hagyja hello alapértelmezett **élő csatlakozás** lehetőséget, majd nyomja le az **Connect**.</span><span class="sxs-lookup"><span data-stu-id="1119d-113">Leave hello default **Connect live** option, then press **Connect**.</span></span> 

5. <span data-ttu-id="1119d-114">Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1119d-114">If prompted, enter your login credentials.</span></span> 

6. <span data-ttu-id="1119d-115">A **Navigator**, hello kiszolgálót, majd válassza ki a hello modell vagy perspektíva tooconnect számára, majd kattintson a kívánt **Connect**.</span><span class="sxs-lookup"><span data-stu-id="1119d-115">In **Navigator**, expand hello server, then select hello model or perspective you want tooconnect to, then click **Connect**.</span></span> <span data-ttu-id="1119d-116">Kattintson a modell vagy perspektíva tooshow, ha a nézet összes hello objektum.</span><span class="sxs-lookup"><span data-stu-id="1119d-116">Click  a model or perspective tooshow all hello objects for that view.</span></span>

    <span data-ttu-id="1119d-117">a jelentések nézetének üres jelentést a Power BI Desktop hello modell megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1119d-117">hello model opens in Power BI Desktop with a blank report in Report view.</span></span> <span data-ttu-id="1119d-118">hello mezők listája nem rejtett modell objektumait.</span><span class="sxs-lookup"><span data-stu-id="1119d-118">hello Fields list displays all non-hidden model objects.</span></span> <span data-ttu-id="1119d-119">A kapcsolat állapota hello jobb alsó sarkában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1119d-119">Connection status is displayed in hello lower-right corner.</span></span>

## <a name="connect-in-power-bi-service"></a><span data-ttu-id="1119d-120">Csatlakozás a Power bi-ban (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="1119d-120">Connect in Power BI (service)</span></span>

1. <span data-ttu-id="1119d-121">Hozzon létre egy Power BI Desktop fájlt, amely rendelkezik egy élő kapcsolattal tooyour modell a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="1119d-121">Create a Power BI Desktop file that has a live connection tooyour model on your server.</span></span>
2. <span data-ttu-id="1119d-122">A [Power BI](https://powerbi.microsoft.com), kattintson a **adatok beolvasása** > **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="1119d-122">In [Power BI](https://powerbi.microsoft.com), click **Get Data** > **Files**.</span></span> <span data-ttu-id="1119d-123">Keresse meg és jelölje ki a fájlt.</span><span class="sxs-lookup"><span data-stu-id="1119d-123">Locate and select your file.</span></span>



## <a name="see-also"></a><span data-ttu-id="1119d-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1119d-124">See also</span></span>
<span data-ttu-id="1119d-125">[Csatlakozás tooAzure Analysis Services](analysis-services-connect.md) </span><span class="sxs-lookup"><span data-stu-id="1119d-125">[Connect tooAzure Analysis Services](analysis-services-connect.md) </span></span>  
[<span data-ttu-id="1119d-126">Ügyfél-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="1119d-126">Client libraries</span></span>](analysis-services-data-providers.md)

