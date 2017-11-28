---
title: "Vendég operációsrendszer-család 1 használatból való kivonást figyelje meg |} Microsoft Docs"
description: "Ha az Azure vendég operációs rendszer családja 1 használatból való kivonást történt és annak megállapítása, hogy hatással nyújt információkat"
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
ms.openlocfilehash: 3178a09dab1cb972a3460d54dc9908fb95cce68b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="guest-os-family-1-retirement-notice"></a><span data-ttu-id="edbfd-103">Vendég operációsrendszer-család 1 használatból való kivonást értesítés</span><span class="sxs-lookup"><span data-stu-id="edbfd-103">Guest OS Family 1 retirement notice</span></span>
<span data-ttu-id="edbfd-104">Az operációsrendszer-család 1 kivonása 2013. június 1 bejelentése volt.</span><span class="sxs-lookup"><span data-stu-id="edbfd-104">The retirement of OS Family 1 was first announced on June 1, 2013.</span></span>

<span data-ttu-id="edbfd-105">**2014. szeptember 2** az Azure vendég operációs rendszer (a vendég operációs rendszer) termékcsalád 1.x, amely a Windows Server 2008 operációs rendszeren alapul, hivatalosan lett visszavonva.</span><span class="sxs-lookup"><span data-stu-id="edbfd-105">**Sept 2, 2014** The Azure Guest operating system (Guest OS) Family 1.x, which is based on the Windows Server 2008 operating system, was officially retired.</span></span> <span data-ttu-id="edbfd-106">Az új szolgáltatásokat telepíti, vagy frissítse a meglévő szolgáltatások termékcsalád 1 használatával bármilyen kísérlet sikertelen lesz, és hibaüzenetet tájékoztatja arról, hogy a vendég operációs rendszer családja 1 eltávolították.</span><span class="sxs-lookup"><span data-stu-id="edbfd-106">All attempts to deploy new services or upgrade existing services using Family 1 will fail with an error message informing you that the Guest OS Family 1 has been retired.</span></span>

<span data-ttu-id="edbfd-107">**2014. novemberi 3** kibővített támogatása vendég operációs rendszer családja 1 befejeződik, és a teljes kivonják.</span><span class="sxs-lookup"><span data-stu-id="edbfd-107">**November 3, 2014** Extended support for Guest OS Family 1 ended and it is fully retired.</span></span> <span data-ttu-id="edbfd-108">Minden szolgáltatás továbbra is a családja 1 csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="edbfd-108">All services still on Family 1 will be impacted.</span></span> <span data-ttu-id="edbfd-109">Bármikor előfordulhat, hogy ezek a szolgáltatások leállítása.</span><span class="sxs-lookup"><span data-stu-id="edbfd-109">We may stop those services at any time.</span></span> <span data-ttu-id="edbfd-110">Nincs a szolgáltatások tovább fut, ha manuálisan frissíti őket saját kezűleg garancia.</span><span class="sxs-lookup"><span data-stu-id="edbfd-110">There is no guarantee your services will continue to run unless you manually upgrade them yourself.</span></span>

<span data-ttu-id="edbfd-111">Ha további kérdésekre, keresse fel a [Cloud Services fórumban](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) vagy [kérje az Azure támogatási](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="edbfd-111">If you have additional questions, visit the [Cloud Services Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) or [contact Azure support](https://azure.microsoft.com/support/options/).</span></span>

## <a name="are-you-affected"></a><span data-ttu-id="edbfd-112">Érintettek?</span><span class="sxs-lookup"><span data-stu-id="edbfd-112">Are you affected?</span></span>
<span data-ttu-id="edbfd-113">A Cloud Services érintett közül legalább az alábbi esetekben alkalmazza:</span><span class="sxs-lookup"><span data-stu-id="edbfd-113">Your Cloud Services are affected if any one of the following applies:</span></span>

1. <span data-ttu-id="edbfd-114">Az érték "osFamily ="1"explicit módon megadott a ServiceConfiguration.cscfg fájlban a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="edbfd-114">You have a value of "osFamily = "1" explicitly specified in the ServiceConfiguration.cscfg file for your Cloud Service.</span></span>
2. <span data-ttu-id="edbfd-115">Nincs a felhőszolgáltatás explicit módon a ServiceConfiguration.cscfg fájlban megadott osFamily értékét.</span><span class="sxs-lookup"><span data-stu-id="edbfd-115">You do not have a value for osFamily explicitly specified in the ServiceConfiguration.cscfg file for your Cloud Service.</span></span> <span data-ttu-id="edbfd-116">Jelenleg a rendszer használja az alapértelmezett érték "1" Ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="edbfd-116">Currently, the system uses the default value of "1" in this case.</span></span>
3. <span data-ttu-id="edbfd-117">Az Azure-portálon, a "Windows Server 2008" sorolja fel a Vendég operációsrendszer-családba tartozó értéket.</span><span class="sxs-lookup"><span data-stu-id="edbfd-117">The Azure portal lists your Guest Operating System family value as "Windows Server 2008".</span></span>

<span data-ttu-id="edbfd-118">Kereséséhez, amelyek a felhőalapú szolgáltatások futnak melyik operációsrendszer-család, futtathatja a következő parancsfájl az Azure PowerShell, bár kell [beállítása az Azure PowerShell](/powershell/azureps-cmdlets-docs) első.</span><span class="sxs-lookup"><span data-stu-id="edbfd-118">To find which of your cloud services are running which OS Family, you can run the following script in Azure PowerShell, though you must [set up Azure PowerShell](/powershell/azureps-cmdlets-docs) first.</span></span> <span data-ttu-id="edbfd-119">A parancsfájl további információkért lásd: [Azure vendég operációs rendszer termékcsalád 1 vége az élettartam: 2014. június](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="edbfd-119">For more information on the script, see [Azure Guest OS Family 1 End of Life: June 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).</span></span>

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

<span data-ttu-id="edbfd-120">A felhőszolgáltatások által érintett operációsrendszer-család 1 kivonása, ha az osFamily a parancsfájl kimenetében üres vagy tartalmaz egy "1".</span><span class="sxs-lookup"><span data-stu-id="edbfd-120">Your cloud services will be impacted by OS Family 1 retirement if the osFamily column in the script output is empty or contains a "1".</span></span>

## <a name="recommendations-if-you-are-affected"></a><span data-ttu-id="edbfd-121">Ha érinti javaslatok</span><span class="sxs-lookup"><span data-stu-id="edbfd-121">Recommendations if you are affected</span></span>
<span data-ttu-id="edbfd-122">Azt javasoljuk, hogy a Felhőszolgáltatási szerepkörök áttelepítéséhez a támogatott vendég operációsrendszer-családok egyikét:</span><span class="sxs-lookup"><span data-stu-id="edbfd-122">We recommend you migrate your Cloud Service roles to one of the supported Guest OS Families:</span></span>

<span data-ttu-id="edbfd-123">**Vendég operációs rendszer termékcsalád 4.x** – Windows Server 2012 R2 *(ajánlott)*</span><span class="sxs-lookup"><span data-stu-id="edbfd-123">**Guest OS family 4.x** - Windows Server 2012 R2 *(recommended)*</span></span>

1. <span data-ttu-id="edbfd-124">Győződjön meg arról, hogy az alkalmazás SDK 2.1-es vagy újabb használ a .NET-keretrendszer 4.0-s, 4.5-ös és 4.5.1-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="edbfd-124">Ensure that your application is using SDK 2.1 or later with .NET framework 4.0, 4.5 or 4.5.1.</span></span>
2. <span data-ttu-id="edbfd-125">Az osFamily attribútum "4" értékre a ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="edbfd-125">Set the osFamily attribute to “4” in the ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="edbfd-126">**Vendég operációs rendszer termékcsalád 3.x** – Windows Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="edbfd-126">**Guest OS family 3.x** - Windows Server 2012</span></span>

1. <span data-ttu-id="edbfd-127">Győződjön meg arról, hogy az alkalmazás SDK 1,8 vagy újabb használ a .NET-keretrendszer 4.0 vagy 4.5.</span><span class="sxs-lookup"><span data-stu-id="edbfd-127">Ensure that your application is using SDK 1.8 or later with .NET framework 4.0 or 4.5.</span></span>
2. <span data-ttu-id="edbfd-128">Az osFamily attribútum "3" értékre a ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="edbfd-128">Set the osFamily attribute to “3” in the ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

<span data-ttu-id="edbfd-129">**Vendég operációs rendszer termékcsalád 2.x** – Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="edbfd-129">**Guest OS family 2.x** - Windows Server 2008 R2</span></span>

1. <span data-ttu-id="edbfd-130">Győződjön meg arról, hogy az alkalmazás SDK 1.3 használ, és a fenti .NET-keretrendszer 3.5-ös vagy 4.0-s verziójával.</span><span class="sxs-lookup"><span data-stu-id="edbfd-130">Ensure that your application is using SDK 1.3 and above with .NET framework 3.5 or 4.0.</span></span>
2. <span data-ttu-id="edbfd-131">Az osFamily attribútum "2" értékre a ServiceConfiguration.cscfg fájlban, és telepítse újra a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="edbfd-131">Set the osFamily attribute to "2" in the ServiceConfiguration.cscfg file, and redeploy your cloud service.</span></span>

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a><span data-ttu-id="edbfd-132">Kiterjesztett terméktámogatás a Vendég operációsrendszer-család 1 rendszerhez 2014. november 3.</span><span class="sxs-lookup"><span data-stu-id="edbfd-132">Extended Support for Guest OS Family 1 ended Nov 3, 2014</span></span>
<span data-ttu-id="edbfd-133">A Vendég operációsrendszer-család 1 felhőszolgáltatások támogatása megszűnt.</span><span class="sxs-lookup"><span data-stu-id="edbfd-133">Cloud services on Guest OS family 1 are no longer supported.</span></span> <span data-ttu-id="edbfd-134">Áttelepítés kikapcsolása termékcsalád 1 szolgáltatás megszűnésének megelőzése érdekében a lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="edbfd-134">Migrate off family 1 as soon as possible to avoid service disruption.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="edbfd-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edbfd-135">Next steps</span></span>
<span data-ttu-id="edbfd-136">Tekintse át a legutóbbi [feloldja a vendég operációs rendszer](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="edbfd-136">Review the latest [Guest OS releases](cloud-services-guestos-update-matrix.md).</span></span>
