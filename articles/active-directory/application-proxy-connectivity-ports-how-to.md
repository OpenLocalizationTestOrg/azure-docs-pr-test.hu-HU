---
title: "aaaHow tooopen hello tűzfalportok az alkalmazásproxy-alkalmazáshoz szükséges |} Microsoft Docs"
description: "Megtudhatja, milyen portokat tooopen a hello Azure AD alkalmazásproxy toowork megfelelően"
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
ms.openlocfilehash: cdc7badb7c15591689a3bfd6bb26da182b00fb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-hello-firewall-ports-required-for-an-application-proxy-application"></a><span data-ttu-id="b5c38-103">Hogyan tooopen hello az alkalmazásproxy-alkalmazáshoz szükséges tűzfal portok</span><span class="sxs-lookup"><span data-stu-id="b5c38-103">How tooopen hello firewall ports required for an Application Proxy application</span></span>

<span data-ttu-id="b5c38-104">hello szükséges portok és hello funkció az egyes portok teljes listáját toosee hello Előfeltételek című hello [alkalmazásproxy dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span><span class="sxs-lookup"><span data-stu-id="b5c38-104">toosee a full list of hello required ports and hello function of each port, see hello prerequisites section of hello [Application Proxy documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span></span> <span data-ttu-id="b5c38-105">Vegye figyelembe, hogy a alkalmazásproxy csak használja-e a kimenő portokat.</span><span class="sxs-lookup"><span data-stu-id="b5c38-105">note that Application Proxy only uses outbound ports.</span></span>

<span data-ttu-id="b5c38-106">Azt is ellenőrizheti, hogy rendelkezik-e minden hello szükséges portok nyitva megnyitása hello által [összekötő portok vizsgálati eszköz](https://aadap-portcheck.connectorporttest.msappproxy.net/) a helyszíni hálózatról.</span><span class="sxs-lookup"><span data-stu-id="b5c38-106">You can also check whether you have all hello required ports open by opening hello [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) from your on-prem network.</span></span> <span data-ttu-id="b5c38-107">További zöld jelölők azt jelenti, hogy a nagyobb rugalmasság.</span><span class="sxs-lookup"><span data-stu-id="b5c38-107">More green checkmarks means greater resiliency.</span></span> 

## <a name="app-proxy-regions"></a><span data-ttu-id="b5c38-108">Alkalmazás Proxy régiók</span><span class="sxs-lookup"><span data-stu-id="b5c38-108">App Proxy regions</span></span>

<span data-ttu-id="b5c38-109">Úgy tudja, melyik e régiók toolet kell toobe zöld, jelenleg is dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="b5c38-109">We are working on a way toolet you know which of these regions needs toobe green for you.</span></span> <span data-ttu-id="b5c38-110">Most győződjön meg arról, hogy az összes.</span><span class="sxs-lookup"><span data-stu-id="b5c38-110">For now, make sure they all are.</span></span> <span data-ttu-id="b5c38-111">USA középső RÉGIÓJA is függetlenül melyik régióban jogkörrel rendelkezik arra szükség.</span><span class="sxs-lookup"><span data-stu-id="b5c38-111">Central US is also required regardless of which region you are in.</span></span>

<span data-ttu-id="b5c38-112">toomake meg arról, hogy hello eszközt ad meg hello jobb eredményeket, ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="b5c38-112">toomake sure hello tool gives you hello right results, be sure to:</span></span>

-   <span data-ttu-id="b5c38-113">Nyissa meg a böngésző hello eszköz hello kiszolgálóra, amelyre telepítve van a hello összekötő.</span><span class="sxs-lookup"><span data-stu-id="b5c38-113">Open hello tool on a browser from hello server where you have installed hello Connector.</span></span>

-   <span data-ttu-id="b5c38-114">Győződjön meg arról, hogy bármely proxyk és tűzfalak megfelelő tooyour összekötő is vannak alkalmazva toothis lap.</span><span class="sxs-lookup"><span data-stu-id="b5c38-114">Ensure that any proxies or firewalls applicable tooyour Connector are also applied toothis page.</span></span> <span data-ttu-id="b5c38-115">Ezt megteheti az Internet Explorer túl címen**beállítások**  - &gt; **Internetbeállítások**  - &gt; **kapcsolatok**  - &gt; **Lan-beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b5c38-115">This can be done in Internet Explorer by going too**Settings** -&gt; **Internet Options** -&gt; **Connections** -&gt; **Lan Settings**.</span></span> <span data-ttu-id="b5c38-116">Ezen a lapon megjelenik a "Hálózati használata az egy Proxy kiszolgáló esetében a" hello mező.</span><span class="sxs-lookup"><span data-stu-id="b5c38-116">On this page, you see hello field “Use a Proxy Server for your LAN”.</span></span> <span data-ttu-id="b5c38-117">Jelölje be a, és hogy hello proxycím hello "Address" mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b5c38-117">Select this box, and put hello proxy address into hello “Address” field.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5c38-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5c38-118">Next steps</span></span>
[<span data-ttu-id="b5c38-119">Az Azure AD-alkalmazásproxy összekötők ismertetése</span><span class="sxs-lookup"><span data-stu-id="b5c38-119">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
