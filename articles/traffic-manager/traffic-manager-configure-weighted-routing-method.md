---
title: "aaaConfigure súlyozott ciklikus időszeletelési forgalom-használata az Azure Traffic Manager útválasztási módszer |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooload egyenleg módszerrel ciklikus multiplexelés a Traffic Manager forgalom"
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
ms.openlocfilehash: 7e2866ead0b2b653845435dd420a763c5e175f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-weighted-traffic-routing-method-in-traffic-manager"></a><span data-ttu-id="cbd56-103">Súlyozott hello forgalom-útválasztási módszert a Traffic Manager konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cbd56-103">Configure hello weighted traffic routing method in Traffic Manager</span></span>

<span data-ttu-id="cbd56-104">A közös forgalom útválasztási módszer minta tooprovide korábbiakkal megegyező végpontokat, amelyek közé tartoznak a felhőszolgáltatások és webhelyek, és küldjön forgalmat tooeach ciklikus multiplexelés csoportja.</span><span class="sxs-lookup"><span data-stu-id="cbd56-104">A common traffic routing method pattern is tooprovide a set of identical endpoints, which include cloud services and websites, and send traffic tooeach in a round-robin fashion.</span></span> <span data-ttu-id="cbd56-105">a lépéseket követve hello szerkezeti hogyan tooconfigure ez típusú forgalom-útválasztási módszer.</span><span class="sxs-lookup"><span data-stu-id="cbd56-105">hello following steps outline how tooconfigure this type of traffic routing method.</span></span>

> [!NOTE]
> <span data-ttu-id="cbd56-106">Azure-webhelyek már meg ciklikus multiplexelés terheléselosztási funkciója (más néven régiónként) adatközponton belüli webhelyekhez.</span><span class="sxs-lookup"><span data-stu-id="cbd56-106">Azure Websites already provide round-robin load balancing functionality for websites within a datacenter (also known as a region).</span></span> <span data-ttu-id="cbd56-107">A TRAFFIC Manager lehetővé teszi toospecify ciklikus időszeletelési forgalom-útválasztási módszert webhelyekhez különböző adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="cbd56-107">Traffic Manager allows you toospecify round-robin traffic routing method for websites in different datacenters.</span></span>

## <a name="tooconfigure-hello-weighted-traffic-routing-method"></a><span data-ttu-id="cbd56-108">tooconfigure súlyozott hello forgalom-útválasztási módszert</span><span class="sxs-lookup"><span data-stu-id="cbd56-108">tooconfigure hello weighted traffic routing method</span></span>

1. <span data-ttu-id="cbd56-109">Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cbd56-109">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="cbd56-110">Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="cbd56-110">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="cbd56-111">A hello portal keresősávban, keresse meg a hello **Traffic Manager-profilok** majd tooconfigure hello útválasztási módszer a használni kívánt hello-profil neve.</span><span class="sxs-lookup"><span data-stu-id="cbd56-111">In hello portal’s search bar, search for hello **Traffic Manager profiles** and then click hello profile name that you want tooconfigure hello routing method for.</span></span>
3. <span data-ttu-id="cbd56-112">A hello **Traffic Manager-profil** panelen ellenőrizze, hogy mindkét hello felhőszolgáltatások és a webhelyeket, tooinclude konfigurációs találhatók.</span><span class="sxs-lookup"><span data-stu-id="cbd56-112">In hello **Traffic Manager profile** blade, verify that both hello cloud services and websites that you want tooinclude in your configuration are present.</span></span>
4. <span data-ttu-id="cbd56-113">A hello **beállítások** területen kattintson **konfigurációs**, és a hello **konfigurációs** panelen, befejeződött, az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="cbd56-113">In hello **Settings** section, click **Configuration**, and in hello **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="cbd56-114">A **forgalom-útválasztási módszer beállításai**, győződjön meg arról, hogy hello forgalom-útválasztási módszert **Weighted**.</span><span class="sxs-lookup"><span data-stu-id="cbd56-114">For **traffic routing method settings**, verify that hello traffic routing method is **Weighted**.</span></span> <span data-ttu-id="cbd56-115">Ha nem, kattintson a **Weighted** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="cbd56-115">If it is not, click **Weighted** from hello dropdown list.</span></span>
    2. <span data-ttu-id="cbd56-116">Set hello **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbd56-116">Set hello **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="cbd56-117">Jelölje be hello megfelelő **protokoll**, és adja meg a hello **Port** számát.</span><span class="sxs-lookup"><span data-stu-id="cbd56-117">Select hello appropriate **Protocol**, and specify hello **Port** number.</span></span> 
        2. <span data-ttu-id="cbd56-118">A **elérési** írja be a perjellel  */* .</span><span class="sxs-lookup"><span data-stu-id="cbd56-118">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="cbd56-119">toomonitor végpontok, adjon meg egy elérési utat és fájlnevet.</span><span class="sxs-lookup"><span data-stu-id="cbd56-119">toomonitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="cbd56-120">A perjel "/" hello relatív elérési út érvényes bejegyzés, és azt jelenti, hogy hello fájl hello gyökérkönyvtárában (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="cbd56-120">A forward slash "/" is a valid entry for hello relative path and implies that hello file is in hello root directory (default).</span></span>
        3. <span data-ttu-id="cbd56-121">Hello hello oldal tetején, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="cbd56-121">At hello top of hello page, click **Save**.</span></span>
5. <span data-ttu-id="cbd56-122">Tesztelje hello módosításokat a konfigurációt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbd56-122">Test hello changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="cbd56-123">Hello portal keresősávban keressen a hello Traffic Manager-profil nevét, majd kattintson a hello Traffic Manager-profil hello eredmények jelenik meg, hogy hello.</span><span class="sxs-lookup"><span data-stu-id="cbd56-123">In hello portal’s search bar, search for hello Traffic Manager profile name and click hello Traffic Manager profile in hello results that hello displayed.</span></span>
    2.  <span data-ttu-id="cbd56-124">A hello **Traffic Manager** profil panelen, kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="cbd56-124">In hello **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="cbd56-125">Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="cbd56-125">hello **Traffic Manager profile** blade displays hello DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="cbd56-126">Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható.</span><span class="sxs-lookup"><span data-stu-id="cbd56-126">This can be used by any clients (for example,by navigating tooit using a web browser) tooget routed toohello right endpoint as determined by hello routing type.</span></span> <span data-ttu-id="cbd56-127">Ebben az esetben az összes rendszer kérést átirányítja a ciklikus multiplexelés végpontok.</span><span class="sxs-lookup"><span data-stu-id="cbd56-127">In this case all requests are routed each endpoint in a round-robin fashion.</span></span>
6. <span data-ttu-id="cbd56-128">A Traffic Manager-profil működik, ha szerkesztése hello DNS-rekordot a mérvadó DNS-kiszolgáló toopoint meg a vállalati tartomány neve toohello Traffic Manager szolgáltatásbeli tartománynevére.</span><span class="sxs-lookup"><span data-stu-id="cbd56-128">Once your Traffic Manager profile is working, edit hello DNS record on your authoritative DNS server toopoint your company domain name toohello Traffic Manager domain name.</span></span>

![Súlyozott forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a><span data-ttu-id="cbd56-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbd56-130">Next steps</span></span>

- <span data-ttu-id="cbd56-131">További tudnivalók [prioritású virtuális gép forgalom-útválasztási módszer](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="cbd56-131">Learn about [priority traffic routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="cbd56-132">További tudnivalók [teljesítmény forgalom-útválasztási módszer](traffic-manager-configure-performance-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="cbd56-132">Learn about [performance traffic routing method](traffic-manager-configure-performance-routing-method.md).</span></span>
- <span data-ttu-id="cbd56-133">További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="cbd56-133">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="cbd56-134">Ismerje meg, hogyan túl[Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="cbd56-134">Learn how too[test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
