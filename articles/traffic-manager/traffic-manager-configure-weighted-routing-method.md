---
title: "Súlyozott ciklikus időszeletelési forgalom-útválasztási módszert használó Azure Traffic Manager konfigurálása |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan módszerrel ciklikus multiplexelés a Traffic Manager forgalom"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 7aa4c9120d44ff1b3e59a57090ea04e3f8021fc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a><span data-ttu-id="3b1ad-103">A súlyozott forgalom-útválasztási módszert a Traffic Manager konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b1ad-103">Configure the weighted traffic routing method in Traffic Manager</span></span>

<span data-ttu-id="3b1ad-104">Egy közös forgalom útválasztási módszer mintát, hogy olyan korábbiakkal megegyező végpontokat, köztük a felhőszolgáltatásokat és webhelyeket, és mindegyik ciklikus multiplexelés forgalmat küldeni.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-104">A common traffic routing method pattern is to provide a set of identical endpoints, which include cloud services and websites, and send traffic to each in a round-robin fashion.</span></span> <span data-ttu-id="3b1ad-105">Forgalom-útválasztási módszert az ilyen típusú konfigurálásának lépései.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-105">The following steps outline how to configure this type of traffic routing method.</span></span>

> [!NOTE]
> <span data-ttu-id="3b1ad-106">Azure-webhelyek már meg ciklikus multiplexelés terheléselosztási funkciója (más néven régiónként) adatközponton belüli webhelyekhez.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-106">Azure Websites already provide round-robin load balancing functionality for websites within a datacenter (also known as a region).</span></span> <span data-ttu-id="3b1ad-107">A TRAFFIC Manager lehetővé teszi különböző adatközpontokban ciklikus időszeletelési forgalom-útválasztási módszert webhelyekhez megadását.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-107">Traffic Manager allows you to specify round-robin traffic routing method for websites in different datacenters.</span></span>

## <a name="to-configure-the-weighted-traffic-routing-method"></a><span data-ttu-id="3b1ad-108">A súlyozott forgalom-útválasztási módszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3b1ad-108">To configure the weighted traffic routing method</span></span>

1. <span data-ttu-id="3b1ad-109">Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-109">From a browser, sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="3b1ad-110">Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-110">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="3b1ad-111">A portál keresősávban, keresse meg a **Traffic Manager-profilok** és kattintson a profil nevére, amelyet vonatkozó útválasztási módszer konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-111">In the portal’s search bar, search for the **Traffic Manager profiles** and then click the profile name that you want to configure the routing method for.</span></span>
3. <span data-ttu-id="3b1ad-112">Az a **Traffic Manager-profil** panelen ellenőrizze, hogy a felhőszolgáltatás és a webhelyeket, amelyeket a konfigurációt szeretne megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-112">In the **Traffic Manager profile** blade, verify that both the cloud services and websites that you want to include in your configuration are present.</span></span>
4. <span data-ttu-id="3b1ad-113">Az a **beállítások** területen kattintson **konfigurációs**, majd a a **konfigurációs** panelen, befejeződött, az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="3b1ad-113">In the **Settings** section, click **Configuration**, and in the **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="3b1ad-114">A **forgalom-útválasztási módszer beállításai**, győződjön meg arról, hogy a forgalom-útválasztási módszert **Weighted**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-114">For **traffic routing method settings**, verify that the traffic routing method is **Weighted**.</span></span> <span data-ttu-id="3b1ad-115">Ha nem, kattintson a **Weighted** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-115">If it is not, click **Weighted** from the dropdown list.</span></span>
    2. <span data-ttu-id="3b1ad-116">Állítsa be a **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3b1ad-116">Set the **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="3b1ad-117">Válassza ki a megfelelő **protokoll**, és adja meg a **Port** számát.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-117">Select the appropriate **Protocol**, and specify the **Port** number.</span></span> 
        2. <span data-ttu-id="3b1ad-118">A **elérési** írja be a perjellel  */* .</span><span class="sxs-lookup"><span data-stu-id="3b1ad-118">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="3b1ad-119">Figyelő végpontokat, az elérési útnak és fájlnévnek kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-119">To monitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="3b1ad-120">A perjel "/" relatív elérési útja érvényes bejegyzés, és azt jelenti, hogy a fájl a gyökérmappában lévő (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-120">A forward slash "/" is a valid entry for the relative path and implies that the file is in the root directory (default).</span></span>
        3. <span data-ttu-id="3b1ad-121">Kattintson a lap tetején **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-121">At the top of the page, click **Save**.</span></span>
5. <span data-ttu-id="3b1ad-122">Tesztelje a módosításokat a konfigurációt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3b1ad-122">Test the changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="3b1ad-123">A portál keresősávban, keresse meg a Traffic Manager-profil nevét, majd kattintson az eredményeket a Traffic Manager-profilt, amely a jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-123">In the portal’s search bar, search for the Traffic Manager profile name and click the Traffic Manager profile in the results that the displayed.</span></span>
    2.  <span data-ttu-id="3b1ad-124">Az a **Traffic Manager** profil panelen, kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-124">In the **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="3b1ad-125">A **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-125">The **Traffic Manager profile** blade displays the DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="3b1ad-126">Ez használhatja olyan ügyfelek (például úgy, hogy keresse meg webböngészővel) az beszerzése irányítja át a megfelelő végpont, határozza meg az Útválasztás típusa.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-126">This can be used by any clients (for example,by navigating to it using a web browser) to get routed to the right endpoint as determined by the routing type.</span></span> <span data-ttu-id="3b1ad-127">Ebben az esetben az összes rendszer kérést átirányítja a ciklikus multiplexelés végpontok.</span><span class="sxs-lookup"><span data-stu-id="3b1ad-127">In this case all requests are routed each endpoint in a round-robin fashion.</span></span>
6. <span data-ttu-id="3b1ad-128">A Traffic Manager-profil működik, ha a vállalata tartománynevét mutasson a Traffic Manager tartományneve a mérvadó DNS-kiszolgálón a DNS-rekord szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3b1ad-128">Once your Traffic Manager profile is working, edit the DNS record on your authoritative DNS server to point your company domain name to the Traffic Manager domain name.</span></span>

![Súlyozott forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a><span data-ttu-id="3b1ad-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b1ad-130">Next steps</span></span>

- <span data-ttu-id="3b1ad-131">További tudnivalók [prioritású virtuális gép forgalom-útválasztási módszer](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-131">Learn about [priority traffic routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="3b1ad-132">További tudnivalók [teljesítmény forgalom-útválasztási módszer](traffic-manager-configure-performance-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-132">Learn about [performance traffic routing method](traffic-manager-configure-performance-routing-method.md).</span></span>
- <span data-ttu-id="3b1ad-133">További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-133">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="3b1ad-134">Megtudhatja, hogyan [Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="3b1ad-134">Learn how to [test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
