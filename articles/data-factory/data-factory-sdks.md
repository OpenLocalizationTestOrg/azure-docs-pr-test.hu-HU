---
title: "Azure Data Factory – fejlesztői referencia"
description: "Különböző módjait létrehozása, felügyelete és kezelése az Azure adat-előállítók"
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
ms.openlocfilehash: e0f6c078b60df6bb7381d066bebb3e400b3b97ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-data-factory-developer-reference"></a><span data-ttu-id="09fe7-103">Azure Data Factory – fejlesztői referencia</span><span class="sxs-lookup"><span data-stu-id="09fe7-103">Azure Data Factory Developer Reference</span></span>
<span data-ttu-id="09fe7-104">Hozzon létre, figyelése és kezelése az Azure-portál, Azure PowerShell, .NET Class Library, vagy REST API használatával előállítók.</span><span class="sxs-lookup"><span data-stu-id="09fe7-104">You can create, monitor, and manage the factories using either Azure portal, Azure PowerShell, .NET Class Library, or REST API.</span></span>

| <span data-ttu-id="09fe7-105">Módszer</span><span class="sxs-lookup"><span data-stu-id="09fe7-105">Method</span></span> | <span data-ttu-id="09fe7-106">Resource Location (Erőforrás helye)</span><span class="sxs-lookup"><span data-stu-id="09fe7-106">Resource Location</span></span> | <span data-ttu-id="09fe7-107">Fejlesztői hivatkozások</span><span class="sxs-lookup"><span data-stu-id="09fe7-107">Developer References</span></span> |
| --- | --- | --- |
| <span data-ttu-id="09fe7-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="09fe7-108">Azure portal</span></span> |[<span data-ttu-id="09fe7-109">https://Portal.Azure.com/</span><span class="sxs-lookup"><span data-stu-id="09fe7-109">https://portal.azure.com/</span></span>](https://portal.azure.com) |[<span data-ttu-id="09fe7-110">Ismerkedés az Azure Data Factory (Azure-portál)</span><span class="sxs-lookup"><span data-stu-id="09fe7-110">Get started with Azure Data Factory (Azure portal)</span></span>](data-factory-build-your-first-pipeline-using-editor.md) |
| <span data-ttu-id="09fe7-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="09fe7-111">Azure PowerShell</span></span> |<span data-ttu-id="09fe7-112">Töltse le a legújabb [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="09fe7-112">Download the latest [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span></span> |[<span data-ttu-id="09fe7-113">Parancsmag-referencia</span><span class="sxs-lookup"><span data-stu-id="09fe7-113">Cmdlet reference</span></span>](https://msdn.microsoft.com/library/dn820234.aspx) |
| <span data-ttu-id="09fe7-114">.NET osztálytár</span><span class="sxs-lookup"><span data-stu-id="09fe7-114">.NET Class Library</span></span> |<span data-ttu-id="09fe7-115">Az Azure Data Factory .NET SDK lehetővé teszi, hogy hozzon létre, a figyelheti, és a kezelése az Azure adat-előállítók és a Data Factory használatával a .NET tevékenység kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="09fe7-115">The Azure Data Factory .NET SDK enables you to create, monitor, and manage Azure data factories and extend Data Factory using a .NET activity.</span></span> <span data-ttu-id="09fe7-116">Lásd: [egyéni tevékenységeket használni egy Azure Data Factory-folyamathoz](data-factory-use-custom-activities.md) és [létrehozása, figyelheti és kezelheti a Data Factory .NET SDK használatával Azure adat-előállítók](data-factory-create-data-factories-programmatically.md) cikkek segítséget nyújtanak a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="09fe7-116">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) and [Create, monitor, and manage Azure data factories using Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) articles to help you get started.</span></span><br/><br/><span data-ttu-id="09fe7-117"><b>A legújabb Nuget letöltése</b></span><span class="sxs-lookup"><span data-stu-id="09fe7-117"><b>Downloading the latest Nuget</b></span></span><br/><span data-ttu-id="09fe7-118">Letöltheti a legfrissebb Azure Data Factory felügyeleti Library Nuget-csomagot a: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span><span class="sxs-lookup"><span data-stu-id="09fe7-118">You can download the latest Azure Data Factory Management Library Nuget package from: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span></span><br/><br/><span data-ttu-id="09fe7-119">**A Visual Studio Csomagkezelő konzol használata**</span><span class="sxs-lookup"><span data-stu-id="09fe7-119">**Using Package Manager Console in Visual Studio**</span></span><br/><span data-ttu-id="09fe7-120">A következő parancsot futtathatja a Visual Studio Package Manager Console a legújabb Azure Data Factory felügyeleti függvénytár</span><span class="sxs-lookup"><span data-stu-id="09fe7-120">You can run the following command in Visual Studio’s Package Manager Console to get the latest Azure Data Factory Management Library</span></span><br/><br/><span data-ttu-id="09fe7-121">Install-Package Microsoft.Azure.Management.DataFactories</span><span class="sxs-lookup"><span data-stu-id="09fe7-121">Install-Package Microsoft.Azure.Management.DataFactories</span></span> |[<span data-ttu-id="09fe7-122">.NET SDK-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="09fe7-122">.NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt415893.aspx) |
| <span data-ttu-id="09fe7-123">REST API</span><span class="sxs-lookup"><span data-stu-id="09fe7-123">REST API</span></span> |<span data-ttu-id="09fe7-124">A Data Factory REST API létrehozása, felügyelete és kezelése az Azure adat-előállítók használhatja.</span><span class="sxs-lookup"><span data-stu-id="09fe7-124">You can use the Data Factory REST API to create, monitor, and manage Azure data factories.</span></span> |[<span data-ttu-id="09fe7-125">REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="09fe7-125">REST API Reference</span></span>](https://msdn.microsoft.com/library/dn906738.aspx) |

