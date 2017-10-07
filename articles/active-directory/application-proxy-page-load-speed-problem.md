---
title: "Alkalmazásproxy alkalmazás aaaAn túl hosszú tooload fogad |} Microsoft Docs"
description: "Elhárítása lap betöltési teljesítmény hello Azure AD alkalmazásproxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c7a51f96840966a1d88933fa4e30f39479d8a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-application-proxy-application-takes-too-long-tooload"></a><span data-ttu-id="87db8-103">Az alkalmazásproxy alkalmazás túl hosszú tooload vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="87db8-103">An Application Proxy application takes too long tooload</span></span>

<span data-ttu-id="87db8-104">Ez a cikk segítséget nyújtanak az Azure AD alkalmazásproxy-alkalmazás miért eltarthat egy hosszú ideig tooload, és milyen toounderstand teheti tooresolve probléma.</span><span class="sxs-lookup"><span data-stu-id="87db8-104">This article help you toounderstand why an Azure AD Application Proxy application may take a long time tooload, and what you can do tooresolve this issue.</span></span>

## <a name="overview"></a><span data-ttu-id="87db8-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="87db8-105">Overview</span></span>
<span data-ttu-id="87db8-106">Ha az alkalmazások dolgozik, de a hosszú várakozási ideje fogja látni, lehet néhány kisebb beállításokból állnak a hálózati topológia, érdemes lehet a tooimprove hello sebessége.</span><span class="sxs-lookup"><span data-stu-id="87db8-106">If your applications are working but you see a long latency, there may be some minor tweaks in your network topology that you can consider tooimprove hello speed.</span></span> <span data-ttu-id="87db8-107">Különböző topológiák értékelése, lásd: hello [hálózati szempontok dokumentum](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span><span class="sxs-lookup"><span data-stu-id="87db8-107">For an evaluation of different topologies, see hello [network considerations document](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span></span>

<span data-ttu-id="87db8-108">Ha ezeket a szempontokat nem segít, sajnos nem rendelkezik jelenleg tudunk teljesítményhangolás további javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="87db8-108">If those considerations don’t help, we unfortunately don’t have currently have further recommendations for performance tuning.</span></span> <span data-ttu-id="87db8-109">Hello alkalmazásproxy-szolgáltatás, amely lehet, hogy szorosabb tooyou toomore adatközpontok bővíti, mert közvetlenül megkezdheti továbbfejlesztett toosee késés.</span><span class="sxs-lookup"><span data-stu-id="87db8-109">As hello Application Proxy service expands toomore data centers that may be closer tooyou, you may start toosee improved latency directly.</span></span> <span data-ttu-id="87db8-110">toosee hello a teljes listáját az Azure data adatközpontok úgy is, hogy hello [késés tesztoldalt](http://www.azurespeed.com/Azure/Latency).</span><span class="sxs-lookup"><span data-stu-id="87db8-110">toosee hello full list of Azure data centers, you can see hello [latency test page](http://www.azurespeed.com/Azure/Latency).</span></span> 

<span data-ttu-id="87db8-111">hello adatközpontjaiban hello alkalmazásproxy-szolgáltatás található a hello [összekötő portok vizsgálati eszköz](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span><span class="sxs-lookup"><span data-stu-id="87db8-111">hello data centers with hello Application Proxy service can be found with hello [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span></span> 

## <a name="feedback-on-application-proxy-data-center-locations"></a><span data-ttu-id="87db8-112">Alkalmazásproxy data center helyeken visszajelzés</span><span class="sxs-lookup"><span data-stu-id="87db8-112">Feedback on Application Proxy data center locations</span></span> 
<span data-ttu-id="87db8-113">Előfordulhat, hogy Azure-adatközpont, amelyek még nem jelennek meg a alkalmazásproxy, de átmásolásának tooa nagy késleltetésű fokozása meg.</span><span class="sxs-lookup"><span data-stu-id="87db8-113">There may be Azure data centers that don’t as yet include Application Proxy but would lead tooa great latency improvement for you.</span></span> <span data-ttu-id="87db8-114">hello adatközpont a(z) < aadapfeedback@microsoft.com > tehát használhatjuk a visszajelzés tooplan, mivel azt a csomópontot.</span><span class="sxs-lookup"><span data-stu-id="87db8-114">hello data center location at <aadapfeedback@microsoft.com> so we can use your feedback tooplan as we expand.</span></span>

<span data-ttu-id="87db8-115">Jelenleg néhány további képességeket, amelyek a bérlők számára, amelyet jelenleg a hosszú késések hello késés fokozásához dolgozik, és meg arról, hogy tooshare dokumentáció után érhető el.</span><span class="sxs-lookup"><span data-stu-id="87db8-115">We are working on some additional capabilities that help improve hello latency for tenants that currently see long latencies, and be sure tooshare documentation once available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87db8-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87db8-116">Next steps</span></span>
[<span data-ttu-id="87db8-117">A meglévő helyszíni proxykiszolgálókkal működik</span><span class="sxs-lookup"><span data-stu-id="87db8-117">Work with existing on-premises proxy servers</span></span>](application-proxy-working-with-proxy-servers.md)
