---
title: "aaaDefault TEMP mappa mérete túl kicsi a szerepkör |} Microsoft Docs"
description: "Egy felhőalapú szolgáltatás szerepkör egy korlátozott hello TEMP mappa lemezterülettel rendelkezik. Ez a cikk szolgál az egyes javaslatok tooavoid elegendő szabad terület."
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
ms.openlocfilehash: 307dc20f3264e29d122a6616be0028d2ec1282c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a><span data-ttu-id="74bb1-104">Alapértelmezett TEMP mappa mérete túl kicsi a felhőalapú szolgáltatás webes/munkavégző szerepkör</span><span class="sxs-lookup"><span data-stu-id="74bb1-104">Default TEMP folder size is too small on a cloud service web/worker role</span></span>
<span data-ttu-id="74bb1-105">hello alapértelmezett ideiglenes könyvtárat egy felhőalapú szolgáltatás munkavégző vagy webes szerepkör rendelkezik mérete legfeljebb 100 MB, teljes bármikor válhat.</span><span class="sxs-lookup"><span data-stu-id="74bb1-105">hello default temporary directory of a cloud service worker or web role has a maximum size of 100 MB, which may become full at some point.</span></span> <span data-ttu-id="74bb1-106">Ez a cikk ismerteti, hogyan tooavoid elegendő szabad terület hello ideiglenes könyvtár.</span><span class="sxs-lookup"><span data-stu-id="74bb1-106">This article describes how tooavoid running out of space for hello temporary directory.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a><span data-ttu-id="74bb1-107">Miért futtatásához nincs elegendő lemezterület?</span><span class="sxs-lookup"><span data-stu-id="74bb1-107">Why do I run out of space?</span></span>
<span data-ttu-id="74bb1-108">hello szabványos Windows környezeti változók TEMP és TMP érhető el, hogy az alkalmazás fut toocode.</span><span class="sxs-lookup"><span data-stu-id="74bb1-108">hello standard Windows environment variables TEMP and TMP are available toocode that is running in your application.</span></span> <span data-ttu-id="74bb1-109">A TEMP és TMP pont tooa egyetlen könyvtár, mely legfeljebb 100 MB.</span><span class="sxs-lookup"><span data-stu-id="74bb1-109">Both TEMP and TMP point tooa single directory that has a maximum size of 100 MB.</span></span> <span data-ttu-id="74bb1-110">Ebben a könyvtárban tárolt adatokat nem őrzi meg különböző hello életciklus hello felhőszolgáltatás; Ha hello szerepkör-példány egy felhőalapú szolgáltatás újraindul, hello directory karbantartása.</span><span class="sxs-lookup"><span data-stu-id="74bb1-110">Any data that is stored in this directory is not persisted across hello lifecycle of hello cloud service; if hello role instances in a cloud service are recycled, hello directory is cleaned.</span></span>

## <a name="suggestion-toofix-hello-problem"></a><span data-ttu-id="74bb1-111">Javaslat toofix hello probléma</span><span class="sxs-lookup"><span data-stu-id="74bb1-111">Suggestion toofix hello problem</span></span>
<span data-ttu-id="74bb1-112">A következő alternatív hello implementálása:</span><span class="sxs-lookup"><span data-stu-id="74bb1-112">Implement one of hello following alternatives:</span></span>

* <span data-ttu-id="74bb1-113">A helyi tároló egyik erőforrásához konfigurálja, és közvetlenül a TEMP és TMP használata helyett-e érni.</span><span class="sxs-lookup"><span data-stu-id="74bb1-113">Configure a local storage resource, and access it directly instead of using TEMP or TMP.</span></span> <span data-ttu-id="74bb1-114">a helyi tároló egyik erőforrásához az alkalmazásból, hívás hello futtató kódból tooaccess [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="74bb1-114">tooaccess a local storage resource from code that is running within your application, call hello [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>
* <span data-ttu-id="74bb1-115">A helyi tároló egyik erőforrásához konfigurálja, és mutasson a hello TEMP és TMP könyvtárak toopoint toohello elérési útja hello helyi tároló-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="74bb1-115">Configure a local storage resource, and point hello TEMP and TMP directories toopoint toohello path of hello local storage resource.</span></span> <span data-ttu-id="74bb1-116">Ez a módosítás hello belül kell végrehajtani [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="74bb1-116">This modification should be performed within hello [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) method.</span></span>

<span data-ttu-id="74bb1-117">hello következő kódrészlet példa bemutatja, hogyan toomodify hello cél könyvtárak TEMP és TMP a hello OnStart metódus belül:</span><span class="sxs-lookup"><span data-stu-id="74bb1-117">hello following code example shows how toomodify hello target directories for TEMP and TMP from within hello OnStart method:</span></span>

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // hello local resource declaration must have been added toothe
            // service definition file for hello role named WorkerRole1:
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

            // hello rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="74bb1-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74bb1-118">Next steps</span></span>
<span data-ttu-id="74bb1-119">Ismerteti, bloghoz [hogyan tooincrease hello hello Azure webes szerepkör ASP.NET ideiglenes mappa mérete](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span><span class="sxs-lookup"><span data-stu-id="74bb1-119">Read a blog that describes [How tooincrease hello size of hello Azure Web Role ASP.NET Temporary Folder](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).</span></span>

<span data-ttu-id="74bb1-120">További [cikkek hibaelhárítási](/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.</span><span class="sxs-lookup"><span data-stu-id="74bb1-120">View more [troubleshooting articles](/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="74bb1-121">toolearn tootroubleshoot felhőalapú szolgáltatás szerepkör problémák Azure PaaS számítógép diagnosztikai adatok használatával hogyan megtekintése [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="74bb1-121">toolearn how tootroubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, view [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
