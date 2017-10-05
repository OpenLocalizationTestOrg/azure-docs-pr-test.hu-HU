---
title: "Kapcsolódás Azure Analysis Services a Power BI |} Microsoft Docs"
description: "Tudnivalók a Power BI használatával egy Azure Analysis Services-kiszolgálóhoz csatlakoznak."
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
ms.openlocfilehash: 3a2af043feddb4a1d6d63f50e838c8a39035449f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-power-bi"></a><span data-ttu-id="a5675-103">Csatlakozás a Power BI</span><span class="sxs-lookup"><span data-stu-id="a5675-103">Connect with Power BI</span></span>

<span data-ttu-id="a5675-104">Miután elkészítette a kiszolgáló Azure-ban, és telepített egy táblázatos modell, a szervezet felhasználói csatlakozás és újra kell kezdenie a adatok készen áll.</span><span class="sxs-lookup"><span data-stu-id="a5675-104">Once you've created a server in Azure, and deployed a tabular model to it, users in your organization are ready to connect and begin exploring data.</span></span> 

> [!TIP]
> <span data-ttu-id="a5675-105">Ügyeljen arra, hogy a legújabb verzióját használja [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span><span class="sxs-lookup"><span data-stu-id="a5675-105">Be sure to use the latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).</span></span>
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a><span data-ttu-id="a5675-106">Csatlakozás a Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="a5675-106">Connect in Power BI Desktop</span></span>

1. <span data-ttu-id="a5675-107">A Power BI Desktopban, kattintson a **adatok beolvasása** > **Azure** > **Azure Analysis Services-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="a5675-107">In Power BI Desktop, click **Get Data** > **Azure** > **Azure Analysis Services database**.</span></span>

2. <span data-ttu-id="a5675-108">A **Server**, adja meg a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="a5675-108">In **Server**, enter the server name.</span></span> 
    
    <span data-ttu-id="a5675-109">Győződjön meg arról, hogy a teljes URL-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a5675-109">Be sure to include the full URL.</span></span> <span data-ttu-id="a5675-110">Például asazure://westcentralus.asazure.windows.net/advworks.</span><span class="sxs-lookup"><span data-stu-id="a5675-110">For example, asazure://westcentralus.asazure.windows.net/advworks.</span></span>

3. <span data-ttu-id="a5675-111">A **adatbázis**, ha ismeri a nevét, táblázatos modellű adatbázisnál vagy perspektíva, amelyhez csatlakozni kíván, illessze be ide.</span><span class="sxs-lookup"><span data-stu-id="a5675-111">In **Database**, if you know the name of the tabular model database or perspective you want to connect to, paste it here.</span></span> <span data-ttu-id="a5675-112">Ellenkező esetben hagyja üresen a mezőt, és jelöljön ki egy adatbázist vagy perspektíva később.</span><span class="sxs-lookup"><span data-stu-id="a5675-112">Otherwise, you can leave this field blank and select a database or perspective later.</span></span>

4. <span data-ttu-id="a5675-113">Hagyja meg az alapértelmezett **élő csatlakozás** lehetőséget, majd nyomja le az **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a5675-113">Leave the default **Connect live** option, then press **Connect**.</span></span> 

5. <span data-ttu-id="a5675-114">Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a5675-114">If prompted, enter your login credentials.</span></span> 

6. <span data-ttu-id="a5675-115">A **Navigator**, bontsa ki a kiszolgálót, majd válassza ki a modell vagy perspektíva csatlakozni kíván, majd kattintson a kívánt **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a5675-115">In **Navigator**, expand the server, then select the model or perspective you want to connect to, then click **Connect**.</span></span> <span data-ttu-id="a5675-116">Kattintson a modell vagy perspektíva, ha a nézet az összes objektum megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a5675-116">Click  a model or perspective to show all the objects for that view.</span></span>

    <span data-ttu-id="a5675-117">A modell a Power BI Desktop nyílik meg egy üres jelentést jelentéskészítő nézetben.</span><span class="sxs-lookup"><span data-stu-id="a5675-117">The model opens in Power BI Desktop with a blank report in Report view.</span></span> <span data-ttu-id="a5675-118">A mezők listája nem rejtett modell objektumait.</span><span class="sxs-lookup"><span data-stu-id="a5675-118">The Fields list displays all non-hidden model objects.</span></span> <span data-ttu-id="a5675-119">A kapcsolat állapota a jobb alsó sarkában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a5675-119">Connection status is displayed in the lower-right corner.</span></span>

## <a name="connect-in-power-bi-service"></a><span data-ttu-id="a5675-120">Csatlakozás a Power bi-ban (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="a5675-120">Connect in Power BI (service)</span></span>

1. <span data-ttu-id="a5675-121">Hozzon létre egy Power BI Desktop-fájl, amely a modellhez élő kapcsolattal rendelkezik a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a5675-121">Create a Power BI Desktop file that has a live connection to your model on your server.</span></span>
2. <span data-ttu-id="a5675-122">A [Power BI](https://powerbi.microsoft.com), kattintson a **adatok beolvasása** > **fájlok**.</span><span class="sxs-lookup"><span data-stu-id="a5675-122">In [Power BI](https://powerbi.microsoft.com), click **Get Data** > **Files**.</span></span> <span data-ttu-id="a5675-123">Keresse meg és jelölje ki a fájlt.</span><span class="sxs-lookup"><span data-stu-id="a5675-123">Locate and select your file.</span></span>



## <a name="see-also"></a><span data-ttu-id="a5675-124">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a5675-124">See also</span></span>
<span data-ttu-id="a5675-125">[Kapcsolódás Azure Analysis Services](analysis-services-connect.md) </span><span class="sxs-lookup"><span data-stu-id="a5675-125">[Connect to Azure Analysis Services](analysis-services-connect.md) </span></span>  
[<span data-ttu-id="a5675-126">Ügyfél-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="a5675-126">Client libraries</span></span>](analysis-services-data-providers.md)

