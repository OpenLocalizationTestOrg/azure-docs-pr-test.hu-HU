---
title: "Alapértelmezett ideiglenes mappa mérete túl kicsi a szerepkör |} Microsoft Docs"
description: "Egy felhőalapú szolgáltatás szerepkör egy korlátozott az ideiglenes mappa lemezterülettel rendelkezik. Ez a cikk megjelöl néhány hogyan kerülheti fut, nincs elég lemezterület."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 577d090a009eb2331b401273257c7cc7c1eea772
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a><span data-ttu-id="fe412-104">Alapértelmezett TEMP mappa mérete túl kicsi a felhőalapú szolgáltatás webes/munkavégző szerepkör</span><span class="sxs-lookup"><span data-stu-id="fe412-104">Default TEMP folder size is too small on a cloud service web/worker role</span></span>
<span data-ttu-id="fe412-105">Az alapértelmezett ideiglenes könyvtár egy felhőalapú szolgáltatás munkavégző vagy webes szerepkör van 100 MB-os, amelyek egy bizonyos ponton teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="fe412-105">The default temporary directory of a cloud service worker or web role has a maximum size of 100 MB, which may become full at some point.</span></span> <span data-ttu-id="fe412-106">Ez a cikk ismerteti, hogyan kevés a hely az ideiglenes könyvtár elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="fe412-106">This article describes how to avoid running out of space for the temporary directory.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a><span data-ttu-id="fe412-107">Miért futtatásához nincs elegendő lemezterület?</span><span class="sxs-lookup"><span data-stu-id="fe412-107">Why do I run out of space?</span></span>
<span data-ttu-id="fe412-108">A szabványos Windows környezeti változók TEMP és TMP elérhetők, hogy az alkalmazás a kód.</span><span class="sxs-lookup"><span data-stu-id="fe412-108">The standard Windows environment variables TEMP and TMP are available to code that is running in your application.</span></span> <span data-ttu-id="fe412-109">A TEMP és TMP, mely legfeljebb 100 MB-os egyetlen könyvtárra mutat.</span><span class="sxs-lookup"><span data-stu-id="fe412-109">Both TEMP and TMP point to a single directory that has a maximum size of 100 MB.</span></span> <span data-ttu-id="fe412-110">Ebben a könyvtárban tárolt adatokat nem őrzi meg a felhőalapú szolgáltatás; életciklus között a szerepkörpéldányok felhőszolgáltatásban lehetőség, ha a könyvtár karbantartása.</span><span class="sxs-lookup"><span data-stu-id="fe412-110">Any data that is stored in this directory is not persisted across the lifecycle of the cloud service; if the role instances in a cloud service are recycled, the directory is cleaned.</span></span>

## <a name="suggestion-to-fix-the-problem"></a><span data-ttu-id="fe412-111">A probléma megoldása érdekében javaslat</span><span class="sxs-lookup"><span data-stu-id="fe412-111">Suggestion to fix the problem</span></span>
<span data-ttu-id="fe412-112">Valósítja meg az alábbi megoldások egyikét:</span><span class="sxs-lookup"><span data-stu-id="fe412-112">Implement one of the following alternatives:</span></span>

* <span data-ttu-id="fe412-113">A helyi tároló egyik erőforrásához konfigurálja, és közvetlenül a TEMP és TMP használata helyett-e érni.</span><span class="sxs-lookup"><span data-stu-id="fe412-113">Configure a local storage resource, and access it directly instead of using TEMP or TMP.</span></span> <span data-ttu-id="fe412-114">A helyi tároló egyik erőforrásához elérje az alkalmazáson belül futó kód, hívja meg a [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="fe412-114">To access a local storage resource from code that is running within your application, call the [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>
* <span data-ttu-id="fe412-115">A helyi tároló egyik erőforrásához konfigurálja, és mutasson a TEMP és TMP könyvtárak, hogy az erőforrás elérési útja a helyi tároló mutasson.</span><span class="sxs-lookup"><span data-stu-id="fe412-115">Configure a local storage resource, and point the TEMP and TMP directories to point to the path of the local storage resource.</span></span> <span data-ttu-id="fe412-116">Ez a módosítás belül kell végrehajtani a [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="fe412-116">This modification should be performed within the [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method.</span></span>

<span data-ttu-id="fe412-117">Az alábbi példakód bemutatja, hogyan módosíthatja a cél könyvtárak TEMP és TMP az OnStart metódus belül:</span><span class="sxs-lookup"><span data-stu-id="fe412-117">The following code example shows how to modify the target directories for TEMP and TMP from within the OnStart method:</span></span>

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="fe412-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe412-118">Next steps</span></span>
<span data-ttu-id="fe412-119">Ismerteti, bloghoz [Azure webes szerepkör ASP.NET ideiglenes mappa méretének növelése](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span><span class="sxs-lookup"><span data-stu-id="fe412-119">Read a blog that describes [How to increase the size of the Azure Web Role ASP.NET Temporary Folder](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span></span>

<span data-ttu-id="fe412-120">További [cikkek hibaelhárítási](/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.</span><span class="sxs-lookup"><span data-stu-id="fe412-120">View more [troubleshooting articles](/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="fe412-121">Felhőalapú szolgáltatás szerepkör kapcsolatos problémák elhárítása az Azure PaaS diagnosztikai adatainak használatával további tudnivalókért tekintse meg [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="fe412-121">To learn how to troubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, view [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
