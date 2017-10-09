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
# <a name="guest-os-family-1-retirement-notice"></a><span data-ttu-id="32059-103">Vendég operációsrendszer-család 1 használatból való kivonást értesítés</span><span class="sxs-lookup"><span data-stu-id="32059-103">Guest OS Family 1 retirement notice</span></span>
<span data-ttu-id="32059-104">Operációsrendszer-család 1 hello kivonása 2013. június 1 bejelentése volt.</span><span class="sxs-lookup"><span data-stu-id="32059-104">hello retirement of OS Family 1 was first announced on June 1, 2013.</span></span>

<span data-ttu-id="32059-105">**2014. szeptember 2** hello Azure vendég operációs rendszer (a vendég operációs rendszer) termékcsalád 1.x, amely hello Windows Server 2008 operációs rendszeren alapul, hivatalosan lett visszavonva.</span><span class="sxs-lookup"><span data-stu-id="32059-105">**Sept 2, 2014** hello Azure Guest operating system (Guest OS) Family 1.x, which is based on hello Windows Server 2008 operating system, was officially retired.</span></span> <span data-ttu-id="32059-106">Kísérletek toodeploy új szolgáltatások és frissítési meglévő szolgáltatásokat termékcsalád 1 sikertelen lesz, és egy hiba üzenetet fog látni, hogy hello Vendég operációsrendszer-család 1 visszavontuk.</span><span class="sxs-lookup"><span data-stu-id="32059-106">All attempts toodeploy new services or upgrade existing services using Family 1 will fail with an error message informing you that hello Guest OS Family 1 has been retired.</span></span>

<span data-ttu-id="32059-107">**2014. novemberi 3** kibővített támogatása vendég operációs rendszer családja 1 befejeződik, és a teljes kivonják.</span><span class="sxs-lookup"><span data-stu-id="32059-107">**November 3, 2014** Extended support for Guest OS Family 1 ended and it is fully retired.</span></span> <span data-ttu-id="32059-108">Minden szolgáltatás továbbra is a családja 1 csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="32059-108">All services still on Family 1 will be impacted.</span></span> <span data-ttu-id="32059-109">Bármikor előfordulhat, hogy ezek a szolgáltatások leállítása.</span><span class="sxs-lookup"><span data-stu-id="32059-109">We may stop those services at any time.</span></span> <span data-ttu-id="32059-110">Nincs a szolgáltatások toorun folytatódik, ha manuálisan frissíti őket saját kezűleg garancia.</span><span class="sxs-lookup"><span data-stu-id="32059-110">There is no guarantee your services will continue toorun unless you manually upgrade them yourself.</span></span>

<span data-ttu-id="32059-111">Ha további kérdésekre, keresse fel a hello [Cloud Services fórumban](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) vagy [kérje az Azure támogatási](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="32059-111">If you have additional questions, visit hello [Cloud Services Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) or [contact Azure support](https://azure.microsoft.com/support/options/).</span></span>

## <a name="are-you-affected"></a><span data-ttu-id="32059-112">Érintettek?</span><span class="sxs-lookup"><span data-stu-id="32059-112">Are you affected?</span></span>
<span data-ttu-id="32059-113">A Cloud Services érintett közül legalább egy hello alábbi esetekben alkalmazza:</span><span class="sxs-lookup"><span data-stu-id="32059-113">Your Cloud Services are affected if any one of hello following applies:</span></span>

1. <span data-ttu-id="32059-114">Az érték "osFamily ="1"explicit módon megadott hello ServiceConfiguration.cscfg fájlban a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="32059-114">You have a value of "osFamily = "1" explicitly specified in hello ServiceConfiguration.cscfg file for your Cloud Service.</span></span>
2. <span data-ttu-id="32059-115">Nincs a felhőszolgáltatás hello ServiceConfiguration.cscfg fájlban explicit módon megadott osFamily értékét.</span><span class="sxs-lookup"><span data-stu-id="32059-115">You do not have a value for osFamily explicitly specified in hello ServiceConfiguration.cscfg file for your Cloud Service.</span></span> <span data-ttu-id="32059-116">Jelenleg hello rendszer hello alapértelmezett értéket használja az "1" Ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="32059-116">Currently, hello system uses hello default value of "1" in this case.</span></span>
3. <span data-ttu-id="32059-117">hello Azure-portálon, a "Windows Server 2008" sorolja fel a Vendég operációsrendszer-családba tartozó értéket.</span><span class="sxs-lookup"><span data-stu-id="32059-117">hello Azure portal lists your Guest Operating System family value as "Windows Server 2008".</span></span>

<span data-ttu-id="32059-118">toofind, amelyek a felhőalapú szolgáltatások futnak melyik operációsrendszer-család, futtathatja a következő Azure PowerShell-parancsfájl hello kell, ha [beállítása az Azure PowerShell](/powershell/azureps-cmdlets-docs) első.</span><span class="sxs-lookup"><span data-stu-id="32059-118">toofind which of your cloud services are running which OS Family, you can run hello following script in Azure PowerShell, though you must [set up Azure PowerShell](/powershell/azureps-cmdlets-docs) first.</span></span> <span data-ttu-id="32059-119">Hello parancsfájl további információkért lásd: [Azure vendég operációs rendszer termékcsalád 1 vége az élettartam: 2014. június](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="32059-119">For more information on hello script, see [Azure Guest OS Family 1 End of Life: June 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span></span>

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

<span data-ttu-id="32059-120">A felhőszolgáltatások által érintett operációsrendszer-család 1 használatból való kivonást Ha hello osFamily oszlop a hello parancsfájl kimenetében üres, vagy "1" tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="32059-120">Your cloud services will be impacted by OS Family 1 retirement if hello osFamily column in hello script output is empty or contains a "1".</span></span>

## <a name="recommendations-if-you-are-affected"></a><span data-ttu-id="32059-121">Ha érinti javaslatok</span><span class="sxs-lookup"><span data-stu-id="32059-121">Recommendations if you are affected</span></span>
<span data-ttu-id="32059-122">Azt javasoljuk, hogy telepít át, a felhőalapú szolgáltatás szerepkörök tooone hello támogatott vendég operációs rendszer címcsaládokból:</span><span class="sxs-lookup"><span data-stu-id="32059-122">We recommend you migrate your Cloud Service roles tooone of hello supported Guest OS Families:</span></span>

<span data-ttu-id="32059-123">**Vendég operációs rendszer termékcsalád 4.x** – Windows Server 2012 R2 *(ajánlott)*</span><span class="sxs-lookup"><span data-stu-id="32059-123">**Guest OS family 4.x** - Windows Server 2012 R2 *(recommended)*</span></span>

1. <span data-ttu-id="32059-124">Győződjön meg arról, hogy az alkalmazás SDK 2.1-es vagy újabb használ a .NET-keretrendszer 4.0-s, 4.5-ös és 4.5.1-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="32059-124">Ensure that your application is using SDK 2.1 or later with .NET framework 4.0, 4.5 or 4.5.1.</span></span>
2. <span data-ttu-id="32059-125">Állítsa be a hello osFamily attribútum túl "4" a hello ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="32059-125">Set hello osFamily attribute too“4” in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="32059-126">**Vendég operációs rendszer termékcsalád 3.x** – Windows Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="32059-126">**Guest OS family 3.x** - Windows Server 2012</span></span>

1. <span data-ttu-id="32059-127">Győződjön meg arról, hogy az alkalmazás SDK 1,8 vagy újabb használ a .NET-keretrendszer 4.0 vagy 4.5.</span><span class="sxs-lookup"><span data-stu-id="32059-127">Ensure that your application is using SDK 1.8 or later with .NET framework 4.0 or 4.5.</span></span>
2. <span data-ttu-id="32059-128">Állítsa be a hello osFamily attribútum túl "3" a hello ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="32059-128">Set hello osFamily attribute too“3” in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="32059-129">**Vendég operációs rendszer termékcsalád 2.x** – Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="32059-129">**Guest OS family 2.x** - Windows Server 2008 R2</span></span>

1. <span data-ttu-id="32059-130">Győződjön meg arról, hogy az alkalmazás SDK 1.3 használ, és a fenti .NET-keretrendszer 3.5-ös vagy 4.0-s verziójával.</span><span class="sxs-lookup"><span data-stu-id="32059-130">Ensure that your application is using SDK 1.3 and above with .NET framework 3.5 or 4.0.</span></span>
2. <span data-ttu-id="32059-131">Állítsa be a hello osFamily attribútum túl "2" a hello ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="32059-131">Set hello osFamily attribute too"2" in hello ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a><span data-ttu-id="32059-132">Kiterjesztett terméktámogatás a Vendég operációsrendszer-család 1 rendszerhez 2014. november 3.</span><span class="sxs-lookup"><span data-stu-id="32059-132">Extended Support for Guest OS Family 1 ended Nov 3, 2014</span></span>
<span data-ttu-id="32059-133">A Vendég operációsrendszer-család 1 felhőszolgáltatások támogatása megszűnt.</span><span class="sxs-lookup"><span data-stu-id="32059-133">Cloud services on Guest OS family 1 are no longer supported.</span></span> <span data-ttu-id="32059-134">Áttelepítés kikapcsolása termékcsalád 1, amint lehetséges tooavoid szolgáltatás megszűnésének.</span><span class="sxs-lookup"><span data-stu-id="32059-134">Migrate off family 1 as soon as possible tooavoid service disruption.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="32059-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32059-135">Next steps</span></span>
<span data-ttu-id="32059-136">Felülvizsgálati hello legújabb [feloldja a vendég operációs rendszer](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="32059-136">Review hello latest [Guest OS releases](cloud-services-guestos-update-matrix.md).</span></span>
