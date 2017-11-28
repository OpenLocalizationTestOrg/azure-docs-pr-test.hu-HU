---
title: "aaaConfigure teljesítmény forgalom-útválasztási módszerrel Azure Traffic Manager |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooconfigure tooroute Traffic Manager forgalom-toohello végpont legkisebb mértékű késleltetést"
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
ms.openlocfilehash: d0ccd4c9de411844c6f36068859265244f4aa52b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-performance-traffic-routing-method"></a><span data-ttu-id="15f4b-103">Hello teljesítmény forgalom útválasztási módszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="15f4b-103">Configure hello performance traffic routing method</span></span>

<span data-ttu-id="15f4b-104">hello teljesítmény forgalom-útválasztási módszert lehetővé teszi a toodirect forgalom toohello végpont a legkisebb mértékű késleltetést hello hello ügyfél hálózatról.</span><span class="sxs-lookup"><span data-stu-id="15f4b-104">hello Performance traffic routing method allows you toodirect traffic toohello endpoint with hello lowest latency from hello client's network.</span></span> <span data-ttu-id="15f4b-105">A legkisebb mértékű késleltetést hello hello datacenter általában földrajzi távolság legközelebbi hello.</span><span class="sxs-lookup"><span data-stu-id="15f4b-105">Typically, hello datacenter with hello lowest latency is hello closest in geographic distance.</span></span> <span data-ttu-id="15f4b-106">A forgalom-útválasztási módszert nem fiókot használja a hálózati konfigurációban valós idejű módosításokat, vagy nem tölthető be.</span><span class="sxs-lookup"><span data-stu-id="15f4b-106">This traffic routing method cannot account for real-time changes in network configuration or load.</span></span>

##  <a name="tooconfigure-performance-routing-method"></a><span data-ttu-id="15f4b-107">tooconfigure teljesítmény útválasztási módszer</span><span class="sxs-lookup"><span data-stu-id="15f4b-107">tooconfigure performance routing method</span></span>

1. <span data-ttu-id="15f4b-108">Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="15f4b-108">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="15f4b-109">Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="15f4b-109">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="15f4b-110">A hello portal keresősávban, keresse meg a hello **Traffic Manager-profilok** majd tooconfigure hello útválasztási módszer a használni kívánt hello-profil neve.</span><span class="sxs-lookup"><span data-stu-id="15f4b-110">In hello portal’s search bar, search for hello **Traffic Manager profiles** and then click hello profile name that you want tooconfigure hello routing method for.</span></span>
3. <span data-ttu-id="15f4b-111">A hello **Traffic Manager-profil** panelen ellenőrizze, hogy mindkét hello felhőszolgáltatások és a webhelyeket, tooinclude konfigurációs találhatók.</span><span class="sxs-lookup"><span data-stu-id="15f4b-111">In hello **Traffic Manager profile** blade, verify that both hello cloud services and websites that you want tooinclude in your configuration are present.</span></span>
4. <span data-ttu-id="15f4b-112">A hello **beállítások** területen kattintson **konfigurációs**, és a hello **konfigurációs** panelen, befejeződött, az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="15f4b-112">In hello **Settings** section, click **Configuration**, and in hello **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="15f4b-113">A **forgalom-útválasztási módszer beállításai**, a **útválasztási módszer** válasszon **teljesítmény**.</span><span class="sxs-lookup"><span data-stu-id="15f4b-113">For **traffic routing method settings**, for **Routing method** select **Performance**.</span></span>
    2. <span data-ttu-id="15f4b-114">Set hello **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="15f4b-114">Set hello **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="15f4b-115">Jelölje be hello megfelelő **protokoll**, és adja meg a hello **Port** számát.</span><span class="sxs-lookup"><span data-stu-id="15f4b-115">Select hello appropriate **Protocol**, and specify hello **Port** number.</span></span> 
        2. <span data-ttu-id="15f4b-116">A **elérési** írja be a perjellel  */* .</span><span class="sxs-lookup"><span data-stu-id="15f4b-116">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="15f4b-117">toomonitor végpontok, adjon meg egy elérési utat és fájlnevet.</span><span class="sxs-lookup"><span data-stu-id="15f4b-117">toomonitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="15f4b-118">A perjel "/" hello relatív elérési út érvényes bejegyzés, és azt jelenti, hogy hello fájl hello gyökérkönyvtárában (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="15f4b-118">A forward slash "/" is a valid entry for hello relative path and implies that hello file is in hello root directory (default).</span></span>
        3. <span data-ttu-id="15f4b-119">Hello hello oldal tetején, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="15f4b-119">At hello top of hello page, click **Save**.</span></span>
5.  <span data-ttu-id="15f4b-120">Tesztelje hello módosításokat a konfigurációt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="15f4b-120">Test hello changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="15f4b-121">Hello portal keresősávban keressen a hello Traffic Manager-profil nevét, majd kattintson a hello Traffic Manager-profil hello eredmények jelenik meg, hogy hello.</span><span class="sxs-lookup"><span data-stu-id="15f4b-121">In hello portal’s search bar, search for hello Traffic Manager profile name and click hello Traffic Manager profile in hello results that hello displayed.</span></span>
    2.  <span data-ttu-id="15f4b-122">A hello **Traffic Manager** profil panelen, kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="15f4b-122">In hello **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="15f4b-123">Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="15f4b-123">hello **Traffic Manager profile** blade displays hello DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="15f4b-124">Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható.</span><span class="sxs-lookup"><span data-stu-id="15f4b-124">This can be used by any clients (for example, by navigating tooit using a web browser) tooget routed toohello right endpoint as determined by hello routing type.</span></span> <span data-ttu-id="15f4b-125">Ebben az esetben az összes kérelem hello legkisebb mértékű késleltetést hello ügyfél hálózatról irányított toohello végpont.</span><span class="sxs-lookup"><span data-stu-id="15f4b-125">In this case all requests are routed toohello endpoint with hello lowest latency from hello client's network.</span></span>
6. <span data-ttu-id="15f4b-126">A Traffic Manager-profil működik, ha szerkesztése hello DNS-rekordot a mérvadó DNS-kiszolgáló toopoint meg a vállalati tartomány neve toohello Traffic Manager szolgáltatásbeli tartománynevére.</span><span class="sxs-lookup"><span data-stu-id="15f4b-126">Once your Traffic Manager profile is working, edit hello DNS record on your authoritative DNS server toopoint your company domain name toohello Traffic Manager domain name.</span></span>

![Teljesítmény forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a><span data-ttu-id="15f4b-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15f4b-128">Next steps</span></span>

- <span data-ttu-id="15f4b-129">További tudnivalók [forgalom-útválasztási módszert súlyozott](traffic-manager-configure-weighted-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="15f4b-129">Learn about [weighted traffic routing method](traffic-manager-configure-weighted-routing-method.md).</span></span>
- <span data-ttu-id="15f4b-130">További tudnivalók [prioritású virtuális gép útválasztási módszer](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="15f4b-130">Learn about [priority routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="15f4b-131">További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="15f4b-131">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="15f4b-132">Ismerje meg, hogyan túl[Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="15f4b-132">Learn how too[test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png