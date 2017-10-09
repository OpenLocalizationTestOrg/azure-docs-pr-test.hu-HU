---
title: "aaaAzure Data Factory fejlesztői leírás"
description: "Ismerje meg különböző módokon toocreate kapcsolatos figyelése és kezelése az Azure adat-előállítók"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: dc752fa1-a6c3-4753-904e-9f32d0a940b7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: spelluru
redirect_url: https://azure.microsoft.com/services/data-factory/
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 5384f2209ff0f788527542ddd400c1d163d587c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory-developer-reference"></a><span data-ttu-id="84e99-103">Azure Data Factory – fejlesztői referencia</span><span class="sxs-lookup"><span data-stu-id="84e99-103">Azure Data Factory Developer Reference</span></span>
<span data-ttu-id="84e99-104">Hozzon létre, figyelése és kezelése az Azure-portál, Azure PowerShell, .NET Class Library, vagy REST API használatával hello előállítók.</span><span class="sxs-lookup"><span data-stu-id="84e99-104">You can create, monitor, and manage hello factories using either Azure portal, Azure PowerShell, .NET Class Library, or REST API.</span></span>

| <span data-ttu-id="84e99-105">Módszer</span><span class="sxs-lookup"><span data-stu-id="84e99-105">Method</span></span> | <span data-ttu-id="84e99-106">Resource Location (Erőforrás helye)</span><span class="sxs-lookup"><span data-stu-id="84e99-106">Resource Location</span></span> | <span data-ttu-id="84e99-107">Fejlesztői hivatkozások</span><span class="sxs-lookup"><span data-stu-id="84e99-107">Developer References</span></span> |
| --- | --- | --- |
| <span data-ttu-id="84e99-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84e99-108">Azure portal</span></span> |[<span data-ttu-id="84e99-109">https://Portal.Azure.com/</span><span class="sxs-lookup"><span data-stu-id="84e99-109">https://portal.azure.com/</span></span>](https://portal.azure.com) |[<span data-ttu-id="84e99-110">Ismerkedés az Azure Data Factory (Azure-portál)</span><span class="sxs-lookup"><span data-stu-id="84e99-110">Get started with Azure Data Factory (Azure portal)</span></span>](data-factory-build-your-first-pipeline-using-editor.md) |
| <span data-ttu-id="84e99-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="84e99-111">Azure PowerShell</span></span> |<span data-ttu-id="84e99-112">Töltse le a hello legújabb [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="84e99-112">Download hello latest [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span></span> |[<span data-ttu-id="84e99-113">Parancsmag-referencia</span><span class="sxs-lookup"><span data-stu-id="84e99-113">Cmdlet reference</span></span>](https://msdn.microsoft.com/library/dn820234.aspx) |
| <span data-ttu-id="84e99-114">.NET osztálytár</span><span class="sxs-lookup"><span data-stu-id="84e99-114">.NET Class Library</span></span> |<span data-ttu-id="84e99-115">hello Azure Data Factory .NET SDK lehetővé teszi, hogy Ön toocreate figyelése, és kezelése az Azure adat-előállítók és Data Factory használatával a .NET tevékenység kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="84e99-115">hello Azure Data Factory .NET SDK enables you toocreate, monitor, and manage Azure data factories and extend Data Factory using a .NET activity.</span></span> <span data-ttu-id="84e99-116">Lásd: [egyéni tevékenységeket használni egy Azure Data Factory-folyamathoz](data-factory-use-custom-activities.md) és [létrehozása, figyelheti és kezelheti a Data Factory .NET SDK használatával Azure adat-előállítók](data-factory-create-data-factories-programmatically.md) cikkek toohelp használatának első lépéseit.</span><span class="sxs-lookup"><span data-stu-id="84e99-116">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) and [Create, monitor, and manage Azure data factories using Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) articles toohelp you get started.</span></span><br/><br/><span data-ttu-id="84e99-117"><b>Letölti a legújabb Nuget hello</b></span><span class="sxs-lookup"><span data-stu-id="84e99-117"><b>Downloading hello latest Nuget</b></span></span><br/><span data-ttu-id="84e99-118">Letöltheti a hello legújabb Azure Data Factory felügyeleti Library Nuget-csomagot: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span><span class="sxs-lookup"><span data-stu-id="84e99-118">You can download hello latest Azure Data Factory Management Library Nuget package from: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span></span><br/><br/><span data-ttu-id="84e99-119">**A Visual Studio Csomagkezelő konzol használata**</span><span class="sxs-lookup"><span data-stu-id="84e99-119">**Using Package Manager Console in Visual Studio**</span></span><br/><span data-ttu-id="84e99-120">Futtathatja a következő hello parancs a Visual Studio Csomagkezelő konzol tooget hello legújabb Azure Data Factory kezelési könyvtár</span><span class="sxs-lookup"><span data-stu-id="84e99-120">You can run hello following command in Visual Studio’s Package Manager Console tooget hello latest Azure Data Factory Management Library</span></span><br/><br/><span data-ttu-id="84e99-121">Install-Package Microsoft.Azure.Management.DataFactories</span><span class="sxs-lookup"><span data-stu-id="84e99-121">Install-Package Microsoft.Azure.Management.DataFactories</span></span> |[<span data-ttu-id="84e99-122">.NET SDK-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="84e99-122">.NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt415893.aspx) |
| <span data-ttu-id="84e99-123">REST API</span><span class="sxs-lookup"><span data-stu-id="84e99-123">REST API</span></span> |<span data-ttu-id="84e99-124">Használhatja a hello Data Factory REST API toocreate, figyelheti és kezelheti az Azure adat-előállítók.</span><span class="sxs-lookup"><span data-stu-id="84e99-124">You can use hello Data Factory REST API toocreate, monitor, and manage Azure data factories.</span></span> |[<span data-ttu-id="84e99-125">REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="84e99-125">REST API Reference</span></span>](https://msdn.microsoft.com/library/dn906738.aspx) |

