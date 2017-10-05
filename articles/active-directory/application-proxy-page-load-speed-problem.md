---
title: "Az alkalmazásproxy alkalmazás betöltése túl sokáig tart |} Microsoft Docs"
description: "Elhárítása lap betöltési teljesítmény az Azure AD-alkalmazásproxy"
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
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a><span data-ttu-id="fcb68-103">Az alkalmazásproxy alkalmazás betöltése túl sokáig tart</span><span class="sxs-lookup"><span data-stu-id="fcb68-103">An Application Proxy application takes too long to load</span></span>

<span data-ttu-id="fcb68-104">Ez a cikk segítenek megérteni, miért érdemes az Azure AD alkalmazásproxy alkalmazás betöltése sok időre lehet szükség, és mit tehet a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="fcb68-104">This article help you to understand why an Azure AD Application Proxy application may take a long time to load, and what you can do to resolve this issue.</span></span>

## <a name="overview"></a><span data-ttu-id="fcb68-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fcb68-105">Overview</span></span>
<span data-ttu-id="fcb68-106">Ha az alkalmazások dolgozik, de a hosszú várakozási ideje fogja látni, lehet néhány kisebb beállításokból állnak a hálózati topológia, érdemes lehet a sebesség növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="fcb68-106">If your applications are working but you see a long latency, there may be some minor tweaks in your network topology that you can consider to improve the speed.</span></span> <span data-ttu-id="fcb68-107">Különböző topológiák értékelése, tekintse meg a [hálózati szempontok dokumentum](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span><span class="sxs-lookup"><span data-stu-id="fcb68-107">For an evaluation of different topologies, see the [network considerations document](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).</span></span>

<span data-ttu-id="fcb68-108">Ha ezeket a szempontokat nem segít, sajnos nem rendelkezik jelenleg tudunk teljesítményhangolás további javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="fcb68-108">If those considerations don’t help, we unfortunately don’t have currently have further recommendations for performance tuning.</span></span> <span data-ttu-id="fcb68-109">Az alkalmazásproxy-szolgáltatás, amely lehet, hogy közelebb vagy további adatközpontok bontja ki, mert megkezdése előfordulhat, hogy közvetlenül a továbbfejlesztett késés megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fcb68-109">As the Application Proxy service expands to more data centers that may be closer to you, you may start to see improved latency directly.</span></span> <span data-ttu-id="fcb68-110">Azure-adatközpont teljes listájának megtekintéséhez lásd a [késés tesztoldalt](http://www.azurespeed.com/Azure/Latency).</span><span class="sxs-lookup"><span data-stu-id="fcb68-110">To see the full list of Azure data centers, you can see the [latency test page](http://www.azurespeed.com/Azure/Latency).</span></span> 

<span data-ttu-id="fcb68-111">Az alkalmazásproxy-szolgáltatás a adatközpontjaiban található az a [összekötő portok vizsgálati eszköz](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span><span class="sxs-lookup"><span data-stu-id="fcb68-111">The data centers with the Application Proxy service can be found with the [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/).</span></span> 

## <a name="feedback-on-application-proxy-data-center-locations"></a><span data-ttu-id="fcb68-112">Alkalmazásproxy data center helyeken visszajelzés</span><span class="sxs-lookup"><span data-stu-id="fcb68-112">Feedback on Application Proxy data center locations</span></span> 
<span data-ttu-id="fcb68-113">Előfordulhat, hogy Azure-adatközpont, amely még nem jelennek meg a alkalmazásproxy, de az Ön a nagy késleltetésű fokozása vezetne.</span><span class="sxs-lookup"><span data-stu-id="fcb68-113">There may be Azure data centers that don’t as yet include Application Proxy but would lead to a great latency improvement for you.</span></span> <span data-ttu-id="fcb68-114">Az Adatközpont-helyen < aadapfeedback@microsoft.com > , tervezze meg, hogy bontsa ki a visszajelzést is használjuk.</span><span class="sxs-lookup"><span data-stu-id="fcb68-114">The data center location at <aadapfeedback@microsoft.com> so we can use your feedback to plan as we expand.</span></span>

<span data-ttu-id="fcb68-115">Jelenleg néhány további képességeket, amelyek a bérlők számára, amelyet jelenleg a hosszú késések fordulnak elő a várakozási fokozásához dolgozik, és ügyeljen arra, hogy a megosztás után rendelkezésre álló dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="fcb68-115">We are working on some additional capabilities that help improve the latency for tenants that currently see long latencies, and be sure to share documentation once available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcb68-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fcb68-116">Next steps</span></span>
[<span data-ttu-id="fcb68-117">A meglévő helyszíni proxykiszolgálókkal működik</span><span class="sxs-lookup"><span data-stu-id="fcb68-117">Work with existing on-premises proxy servers</span></span>](application-proxy-working-with-proxy-servers.md)
