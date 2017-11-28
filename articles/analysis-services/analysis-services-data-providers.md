---
title: "Kapcsolódás az tooAzure Analysis Services szükséges aaaClient könyvtárak |} Microsoft Docs"
description: "Ismerteti a szükséges ügyfél alkalmazások és eszközök tooconnect Azure Analysis Services klienskódtárak segítségével"
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
ms.openlocfilehash: 74ba5c05ba76c6587c5aed38f200a1ba469aa4f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="client-libraries-for-connecting-tooazure-analysis-services"></a><span data-ttu-id="1cee2-103">Kapcsolódás az tooAzure Analysis Services klienskódtárak segítségével</span><span class="sxs-lookup"><span data-stu-id="1cee2-103">Client libraries for connecting tooAzure Analysis Services</span></span>

<span data-ttu-id="1cee2-104">Ügyféloldali kódtáraknál ügyfél alkalmazások és eszközök tooconnect tooAnalysis szolgáltatások kiszolgálók szükségesek.</span><span class="sxs-lookup"><span data-stu-id="1cee2-104">Client libraries are necessary for client applications and tools tooconnect tooAnalysis Services servers.</span></span> 

<span data-ttu-id="1cee2-105">Analysis Services használata három klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="1cee2-105">Analysis Services utilize three client libraries.</span></span> <span data-ttu-id="1cee2-106">ADOMD.NET és az Analysis Services Management Objects (AMO), olyan felügyelt klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="1cee2-106">ADOMD.NET and Analysis Services Management Objects (AMO), are managed client libraries.</span></span> <span data-ttu-id="1cee2-107">hello Analysis Services OLE DB-szolgáltató (MSOLAP DLL) egy natív ügyfél könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="1cee2-107">hello Analysis Services OLE DB provider (MSOLAP DLL) is a native client library.</span></span> <span data-ttu-id="1cee2-108">Általában három hello telepített ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1cee2-108">Typically, all three are installed at hello same time.</span></span> <span data-ttu-id="1cee2-109">Az Azure Analysis Services hello legújabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="1cee2-109">Azure Analysis Services requires hello latest versions.</span></span> 

<span data-ttu-id="1cee2-110">Microsoft ügyfélalkalmazásokat, például a Power BI Desktop és Excel tárakat három ügyfél telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1cee2-110">Microsoft client applications such as Power BI Desktop and Excel install all three client libraries.</span></span> <span data-ttu-id="1cee2-111">Előfordulhat azonban, attól függően, hogy hello verziója vagy a frissítési gyakoriságának klienskódtárak nem hello Azure Analysis Services által igényelt legújabb verziói.</span><span class="sxs-lookup"><span data-stu-id="1cee2-111">However, depending on hello version or frequency of updates, client libraries may not be hello latest versions required by Azure Analysis Services.</span></span> <span data-ttu-id="1cee2-112">hello Ugyanez vonatkozik toocustom alkalmazások vagy egyéb felületek, például AsCmd, TOM, ADOMD.NET.</span><span class="sxs-lookup"><span data-stu-id="1cee2-112">hello same applies toocustom applications or other interfaces such as AsCmd, TOM, ADOMD.NET.</span></span> <span data-ttu-id="1cee2-113">Ezek az alkalmazások szükséges hello szalagtárak manuális telepítése.</span><span class="sxs-lookup"><span data-stu-id="1cee2-113">These applications require manually installing hello libraries.</span></span> <span data-ttu-id="1cee2-114">hello ügyfél szalagtárak manuális telepítésre terjeszthető csomag SQL Server szolgáltatáscsomagok szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="1cee2-114">hello client libraries for manual installation are included in SQL Server feature packs as distributable packages.</span></span> <span data-ttu-id="1cee2-115">Azonban a ügyfél tárak kapcsolt toohello SQL Server verziója, és nem lehet hello legújabb.</span><span class="sxs-lookup"><span data-stu-id="1cee2-115">However, these client libraries are tied toohello SQL Server version and may not be hello latest.</span></span>  

<span data-ttu-id="1cee2-116">Ügyféloldali kódtáraknál ügyfélkapcsolatok adatok szolgáltatók szükséges tooconnect egy Azure Analysis Services-kiszolgáló tooa adatforrásból eltérnek.</span><span class="sxs-lookup"><span data-stu-id="1cee2-116">Client libraries for client connections are different from data providers required tooconnect from an Azure Analysis Services server tooa data source.</span></span> <span data-ttu-id="1cee2-117">További információ az adatforrás-kapcsolatok esetén toolearn lásd [adatforrás-kapcsolatok](analysis-services-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="1cee2-117">toolearn more about datasource connections, see [Datasource connections](analysis-services-datasource.md).</span></span>

## <a name="download-hello-latest-client-libraries"></a><span data-ttu-id="1cee2-118">Töltse le a legfrissebb klienskódtárak hello</span><span class="sxs-lookup"><span data-stu-id="1cee2-118">Download hello latest client libraries</span></span>  
<span data-ttu-id="1cee2-119">Ha éles környezetben, és teljesen elérhető és támogatott verziója szükséges a következő ügyfél függvénytárainak hello használata.</span><span class="sxs-lookup"><span data-stu-id="1cee2-119">Use hello following client libraries if you are in a production environment and require fully released and supported versions.</span></span>

[<span data-ttu-id="1cee2-120">MSOLAP (amd64)</span><span class="sxs-lookup"><span data-stu-id="1cee2-120">MSOLAP (amd64)</span></span>](https://go.microsoft.com/fwlink/?linkid=829576)</br><span data-ttu-id="1cee2-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span><span class="sxs-lookup"><span data-stu-id="1cee2-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span></span></br><span data-ttu-id="1cee2-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span><span class="sxs-lookup"><span data-stu-id="1cee2-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span></span></br><span data-ttu-id="1cee2-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span><span class="sxs-lookup"><span data-stu-id="1cee2-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="1cee2-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cee2-124">Next steps</span></span>
<span data-ttu-id="1cee2-125">[Csatlakozás az Excel használatával](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="1cee2-125">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
[<span data-ttu-id="1cee2-126">Kapcsolódás PowerBI-jal</span><span class="sxs-lookup"><span data-stu-id="1cee2-126">Connect with Power BI</span></span>](analysis-services-connect-pbi.md)
