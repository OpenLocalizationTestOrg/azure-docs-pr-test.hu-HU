---
title: "aaaGuest operációsrendszer-család 1 használatból való kivonást figyelje meg |} Microsoft Docs"
description: "Információt nyújt hello Azure vendég operációs rendszer családja 1 használatból való kivonást történt, mikor és hogyan toodetermine Ha érinti"
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: fa8b904c6560dbbe4982c301f818c7a5cbc4eacb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guest-os-family-1-retirement-notice"></a>Vendég operációsrendszer-család 1 használatból való kivonást értesítés
Operációsrendszer-család 1 hello kivonása 2013. június 1 bejelentése volt.

**2014. szeptember 2** hello Azure vendég operációs rendszer (a vendég operációs rendszer) termékcsalád 1.x, amely hello Windows Server 2008 operációs rendszeren alapul, hivatalosan lett visszavonva. Kísérletek toodeploy új szolgáltatások és frissítési meglévő szolgáltatásokat termékcsalád 1 sikertelen lesz, és egy hiba üzenetet fog látni, hogy hello Vendég operációsrendszer-család 1 visszavontuk.

**2014. novemberi 3** kibővített támogatása vendég operációs rendszer családja 1 befejeződik, és a teljes kivonják. Minden szolgáltatás továbbra is a családja 1 csökkenhet. Bármikor előfordulhat, hogy ezek a szolgáltatások leállítása. Nincs a szolgáltatások toorun folytatódik, ha manuálisan frissíti őket saját kezűleg garancia.

Ha további kérdésekre, keresse fel a hello [Cloud Services fórumban](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) vagy [kérje az Azure támogatási](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Érintettek?
A Cloud Services érintett közül legalább egy hello alábbi esetekben alkalmazza:

1. Az érték "osFamily ="1"explicit módon megadott hello ServiceConfiguration.cscfg fájlban a felhőalapú szolgáltatáshoz.
2. Nincs a felhőszolgáltatás hello ServiceConfiguration.cscfg fájlban explicit módon megadott osFamily értékét. Jelenleg hello rendszer hello alapértelmezett értéket használja az "1" Ebben az esetben.
3. hello Azure-portálon, a "Windows Server 2008" sorolja fel a Vendég operációsrendszer-családba tartozó értéket.

toofind, amelyek a felhőalapú szolgáltatások futnak melyik operációsrendszer-család, futtathatja a következő Azure PowerShell-parancsfájl hello kell, ha [beállítása az Azure PowerShell](/powershell/azureps-cmdlets-docs) első. Hello parancsfájl további információkért lásd: [Azure vendég operációs rendszer termékcsalád 1 vége az élettartam: 2014. június](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

A felhőszolgáltatások által érintett operációsrendszer-család 1 használatból való kivonást Ha hello osFamily oszlop a hello parancsfájl kimenetében üres, vagy "1" tartalmaz.

## <a name="recommendations-if-you-are-affected"></a>Ha érinti javaslatok
Azt javasoljuk, hogy telepít át, a felhőalapú szolgáltatás szerepkörök tooone hello támogatott vendég operációs rendszer címcsaládokból:

**Vendég operációs rendszer termékcsalád 4.x** – Windows Server 2012 R2 *(ajánlott)*

1. Győződjön meg arról, hogy az alkalmazás SDK 2.1-es vagy újabb használ a .NET-keretrendszer 4.0-s, 4.5-ös és 4.5.1-es verzióját.
2. Állítsa be a hello osFamily attribútum túl "4" a hello ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.

**Vendég operációs rendszer termékcsalád 3.x** – Windows Server 2012-ben

1. Győződjön meg arról, hogy az alkalmazás SDK 1,8 vagy újabb használ a .NET-keretrendszer 4.0 vagy 4.5.
2. Állítsa be a hello osFamily attribútum túl "3" a hello ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.

**Vendég operációs rendszer termékcsalád 2.x** – Windows Server 2008 R2

1. Győződjön meg arról, hogy az alkalmazás SDK 1.3 használ, és a fenti .NET-keretrendszer 3.5-ös vagy 4.0-s verziójával.
2. Állítsa be a hello osFamily attribútum túl "2" a hello ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Kiterjesztett terméktámogatás a Vendég operációsrendszer-család 1 rendszerhez 2014. november 3.
A Vendég operációsrendszer-család 1 felhőszolgáltatások támogatása megszűnt. Áttelepítés kikapcsolása termékcsalád 1, amint lehetséges tooavoid szolgáltatás megszűnésének.  

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati hello legújabb [feloldja a vendég operációs rendszer](cloud-services-guestos-update-matrix.md).
