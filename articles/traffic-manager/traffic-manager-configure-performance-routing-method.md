---
title: "Teljesítmény forgalom-útválasztási módszert használja az Azure Traffic Manager konfigurálása |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan Traffic Manager irányíthatja a forgalmat a végpontnak a legkisebb mértékű késleltetést konfigurálása"
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
ms.openlocfilehash: 014aa646459cd64fca7c697419324caa3edaeeea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-performance-traffic-routing-method"></a><span data-ttu-id="223e0-103">A teljesítmény forgalom útválasztási módszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="223e0-103">Configure the performance traffic routing method</span></span>

<span data-ttu-id="223e0-104">A teljesítmény forgalom-útválasztási módszert közvetlen forgalom a végponthoz, a legkisebb mértékű késleltetést az ügyfél hálózati teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="223e0-104">The Performance traffic routing method allows you to direct traffic to the endpoint with the lowest latency from the client's network.</span></span> <span data-ttu-id="223e0-105">A legkisebb mértékű késleltetést az Adatközpont jellemzően a legközelebbi földrajzi távolságot.</span><span class="sxs-lookup"><span data-stu-id="223e0-105">Typically, the datacenter with the lowest latency is the closest in geographic distance.</span></span> <span data-ttu-id="223e0-106">A forgalom-útválasztási módszert nem fiókot használja a hálózati konfigurációban valós idejű módosításokat, vagy nem tölthető be.</span><span class="sxs-lookup"><span data-stu-id="223e0-106">This traffic routing method cannot account for real-time changes in network configuration or load.</span></span>

##  <a name="to-configure-performance-routing-method"></a><span data-ttu-id="223e0-107">Teljesítmény útválasztási módszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="223e0-107">To configure performance routing method</span></span>

1. <span data-ttu-id="223e0-108">Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="223e0-108">From a browser, sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="223e0-109">Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="223e0-109">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="223e0-110">A portál keresősávban, keresse meg a **Traffic Manager-profilok** és kattintson a profil nevére, amelyet vonatkozó útválasztási módszer konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="223e0-110">In the portal’s search bar, search for the **Traffic Manager profiles** and then click the profile name that you want to configure the routing method for.</span></span>
3. <span data-ttu-id="223e0-111">Az a **Traffic Manager-profil** panelen ellenőrizze, hogy a felhőszolgáltatás és a webhelyeket, amelyeket a konfigurációt szeretne megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="223e0-111">In the **Traffic Manager profile** blade, verify that both the cloud services and websites that you want to include in your configuration are present.</span></span>
4. <span data-ttu-id="223e0-112">Az a **beállítások** területen kattintson **konfigurációs**, majd a a **konfigurációs** panelen, befejeződött, az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="223e0-112">In the **Settings** section, click **Configuration**, and in the **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="223e0-113">A **forgalom-útválasztási módszer beállításai**, a **útválasztási módszer** válasszon **teljesítmény**.</span><span class="sxs-lookup"><span data-stu-id="223e0-113">For **traffic routing method settings**, for **Routing method** select **Performance**.</span></span>
    2. <span data-ttu-id="223e0-114">Állítsa be a **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="223e0-114">Set the **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="223e0-115">Válassza ki a megfelelő **protokoll**, és adja meg a **Port** számát.</span><span class="sxs-lookup"><span data-stu-id="223e0-115">Select the appropriate **Protocol**, and specify the **Port** number.</span></span> 
        2. <span data-ttu-id="223e0-116">A **elérési** írja be a perjellel  */* .</span><span class="sxs-lookup"><span data-stu-id="223e0-116">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="223e0-117">Figyelő végpontokat, az elérési útnak és fájlnévnek kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="223e0-117">To monitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="223e0-118">A perjel "/" relatív elérési útja érvényes bejegyzés, és azt jelenti, hogy a fájl a gyökérmappában lévő (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="223e0-118">A forward slash "/" is a valid entry for the relative path and implies that the file is in the root directory (default).</span></span>
        3. <span data-ttu-id="223e0-119">Kattintson a lap tetején **mentése**.</span><span class="sxs-lookup"><span data-stu-id="223e0-119">At the top of the page, click **Save**.</span></span>
5.  <span data-ttu-id="223e0-120">Tesztelje a módosításokat a konfigurációt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="223e0-120">Test the changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="223e0-121">A portál keresősávban, keresse meg a Traffic Manager-profil nevét, majd kattintson az eredményeket a Traffic Manager-profilt, amely a jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="223e0-121">In the portal’s search bar, search for the Traffic Manager profile name and click the Traffic Manager profile in the results that the displayed.</span></span>
    2.  <span data-ttu-id="223e0-122">Az a **Traffic Manager** profil panelen, kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="223e0-122">In the **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="223e0-123">A **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="223e0-123">The **Traffic Manager profile** blade displays the DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="223e0-124">Ez használhatja olyan ügyfelek (például úgy, hogy keresse meg webböngészővel) az beszerzése irányítja át a megfelelő végpont, határozza meg az Útválasztás típusa.</span><span class="sxs-lookup"><span data-stu-id="223e0-124">This can be used by any clients (for example, by navigating to it using a web browser) to get routed to the right endpoint as determined by the routing type.</span></span> <span data-ttu-id="223e0-125">Ebben az esetben az összes rendszer kérést átirányítja a végpont és a legkisebb mértékű az ügyfél hálózatról.</span><span class="sxs-lookup"><span data-stu-id="223e0-125">In this case all requests are routed to the endpoint with the lowest latency from the client's network.</span></span>
6. <span data-ttu-id="223e0-126">A Traffic Manager-profil működik, ha a vállalata tartománynevét mutasson a Traffic Manager tartományneve a mérvadó DNS-kiszolgálón a DNS-rekord szerkesztése</span><span class="sxs-lookup"><span data-stu-id="223e0-126">Once your Traffic Manager profile is working, edit the DNS record on your authoritative DNS server to point your company domain name to the Traffic Manager domain name.</span></span>

![Teljesítmény forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a><span data-ttu-id="223e0-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="223e0-128">Next steps</span></span>

- <span data-ttu-id="223e0-129">További tudnivalók [forgalom-útválasztási módszert súlyozott](traffic-manager-configure-weighted-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="223e0-129">Learn about [weighted traffic routing method](traffic-manager-configure-weighted-routing-method.md).</span></span>
- <span data-ttu-id="223e0-130">További tudnivalók [prioritású virtuális gép útválasztási módszer](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="223e0-130">Learn about [priority routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="223e0-131">További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="223e0-131">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="223e0-132">Megtudhatja, hogyan [Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="223e0-132">Learn how to [test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png