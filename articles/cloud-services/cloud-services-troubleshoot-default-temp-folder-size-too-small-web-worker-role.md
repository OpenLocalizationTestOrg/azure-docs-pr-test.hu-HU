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
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Alapértelmezett TEMP mappa mérete túl kicsi a felhőalapú szolgáltatás webes/munkavégző szerepkör
hello alapértelmezett ideiglenes könyvtárat egy felhőalapú szolgáltatás munkavégző vagy webes szerepkör rendelkezik mérete legfeljebb 100 MB, teljes bármikor válhat. Ez a cikk ismerteti, hogyan tooavoid elegendő szabad terület hello ideiglenes könyvtár.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Miért futtatásához nincs elegendő lemezterület?
hello szabványos Windows környezeti változók TEMP és TMP érhető el, hogy az alkalmazás fut toocode. A TEMP és TMP pont tooa egyetlen könyvtár, mely legfeljebb 100 MB. Ebben a könyvtárban tárolt adatokat nem őrzi meg különböző hello életciklus hello felhőszolgáltatás; Ha hello szerepkör-példány egy felhőalapú szolgáltatás újraindul, hello directory karbantartása.

## <a name="suggestion-toofix-hello-problem"></a>Javaslat toofix hello probléma
A következő alternatív hello implementálása:

* A helyi tároló egyik erőforrásához konfigurálja, és közvetlenül a TEMP és TMP használata helyett-e érni. a helyi tároló egyik erőforrásához az alkalmazásból, hívás hello futtató kódból tooaccess [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metódust.
* A helyi tároló egyik erőforrásához konfigurálja, és mutasson a hello TEMP és TMP könyvtárak toopoint toohello elérési útja hello helyi tároló-erőforrás. Ez a módosítás hello belül kell végrehajtani [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metódust.

hello következő kódrészlet példa bemutatja, hogyan toomodify hello cél könyvtárak TEMP és TMP a hello OnStart metódus belül:

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

## <a name="next-steps"></a>Következő lépések
Ismerteti, bloghoz [hogyan tooincrease hello hello Azure webes szerepkör ASP.NET ideiglenes mappa mérete](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

További [cikkek hibaelhárítási](/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.

toolearn tootroubleshoot felhőalapú szolgáltatás szerepkör problémák Azure PaaS számítógép diagnosztikai adatok használatával hogyan megtekintése [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
