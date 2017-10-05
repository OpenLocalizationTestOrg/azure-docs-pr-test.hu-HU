---
title: "Azure Analysis Services való kapcsolódáshoz szükséges ügyfél-könyvtárak |} Microsoft Docs"
description: "Ismerteti a szükséges ügyfél-alkalmazások és az eszközök csatlakozni az Azure Analysis Services ügyfél könyvtárak"
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
ms.openlocfilehash: a96e7fe671dc051da35168fa7b49ecba53b4c4a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a><span data-ttu-id="cf209-103">Ügyfél-könyvtárakban csatlakozás Azure Analysis Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="cf209-103">Client libraries for connecting to Azure Analysis Services</span></span>

<span data-ttu-id="cf209-104">Ügyfél könyvtárak olyan ügyfél-alkalmazások és eszközök Analysis Services-kiszolgálókhoz való kapcsolódáshoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf209-104">Client libraries are necessary for client applications and tools to connect to Analysis Services servers.</span></span> 

<span data-ttu-id="cf209-105">Analysis Services használata három klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="cf209-105">Analysis Services utilize three client libraries.</span></span> <span data-ttu-id="cf209-106">ADOMD.NET és az Analysis Services Management Objects (AMO), olyan felügyelt klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="cf209-106">ADOMD.NET and Analysis Services Management Objects (AMO), are managed client libraries.</span></span> <span data-ttu-id="cf209-107">Az Analysis Services OLE DB-szolgáltató (MSOLAP DLL) egy natív ügyfél könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="cf209-107">The Analysis Services OLE DB provider (MSOLAP DLL) is a native client library.</span></span> <span data-ttu-id="cf209-108">Általában három telepített egy időben.</span><span class="sxs-lookup"><span data-stu-id="cf209-108">Typically, all three are installed at the same time.</span></span> <span data-ttu-id="cf209-109">Az Azure Analysis Services a legújabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf209-109">Azure Analysis Services requires the latest versions.</span></span> 

<span data-ttu-id="cf209-110">Microsoft ügyfélalkalmazásokat, például a Power BI Desktop és Excel tárakat három ügyfél telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf209-110">Microsoft client applications such as Power BI Desktop and Excel install all three client libraries.</span></span> <span data-ttu-id="cf209-111">Előfordulhat azonban, attól függően, hogy a verzió vagy frissítési gyakoriságának klienskódtárak nem a legújabb verziók Azure Analysis Services által igényelt.</span><span class="sxs-lookup"><span data-stu-id="cf209-111">However, depending on the version or frequency of updates, client libraries may not be the latest versions required by Azure Analysis Services.</span></span> <span data-ttu-id="cf209-112">Ugyanez vonatkozik az egyéni alkalmazásokra vagy olyan egyéb felületekre, mint például az AsCmd, a TOM vagy az ADOMD.NET.</span><span class="sxs-lookup"><span data-stu-id="cf209-112">The same applies to custom applications or other interfaces such as AsCmd, TOM, ADOMD.NET.</span></span> <span data-ttu-id="cf209-113">Ezek az alkalmazások szükség a szalagtárak manuális telepítése.</span><span class="sxs-lookup"><span data-stu-id="cf209-113">These applications require manually installing the libraries.</span></span> <span data-ttu-id="cf209-114">Az ügyfél szalagtárak manuális telepítésre terjeszthető csomag SQL Server szolgáltatáscsomagok szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="cf209-114">The client libraries for manual installation are included in SQL Server feature packs as distributable packages.</span></span> <span data-ttu-id="cf209-115">Azonban az ügyfél tárakban vannak társítva, az SQL Server-verzióra, és nem lehet a legújabb.</span><span class="sxs-lookup"><span data-stu-id="cf209-115">However, these client libraries are tied to the SQL Server version and may not be the latest.</span></span>  

<span data-ttu-id="cf209-116">Ügyféloldali kódtáraknál ügyfélkapcsolatok Azure Analysis Services-kiszolgálóról egy adatforrás eléréséhez szükséges adatszolgáltatók eltérnek.</span><span class="sxs-lookup"><span data-stu-id="cf209-116">Client libraries for client connections are different from data providers required to connect from an Azure Analysis Services server to a data source.</span></span> <span data-ttu-id="cf209-117">Adatforrás-kapcsolatok kapcsolatos további információkért lásd: [adatforrás-kapcsolatok](analysis-services-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="cf209-117">To learn more about datasource connections, see [Datasource connections](analysis-services-datasource.md).</span></span>

## <a name="download-the-latest-client-libraries"></a><span data-ttu-id="cf209-118">Töltse le a legfrissebb klienskódtárak segítségével</span><span class="sxs-lookup"><span data-stu-id="cf209-118">Download the latest client libraries</span></span>  
<span data-ttu-id="cf209-119">Használja a következő klienskódtárak segítségével, ha éles környezetben, és teljesen elérhető és támogatott verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf209-119">Use the following client libraries if you are in a production environment and require fully released and supported versions.</span></span>

[<span data-ttu-id="cf209-120">MSOLAP (amd64)</span><span class="sxs-lookup"><span data-stu-id="cf209-120">MSOLAP (amd64)</span></span>](https://go.microsoft.com/fwlink/?linkid=829576)</br><span data-ttu-id="cf209-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span><span class="sxs-lookup"><span data-stu-id="cf209-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span></span></br><span data-ttu-id="cf209-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span><span class="sxs-lookup"><span data-stu-id="cf209-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span></span></br><span data-ttu-id="cf209-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span><span class="sxs-lookup"><span data-stu-id="cf209-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="cf209-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf209-124">Next steps</span></span>
<span data-ttu-id="cf209-125">[Csatlakozás az Excel használatával](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="cf209-125">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
[<span data-ttu-id="cf209-126">Kapcsolódás PowerBI-jal</span><span class="sxs-lookup"><span data-stu-id="cf209-126">Connect with Power BI</span></span>](analysis-services-connect-pbi.md)
