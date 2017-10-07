---
title: "aaaEnable munkamenet affinitás használatával hello Eclipse Azure eszköztára"
description: "Ismerje meg, hogyan hello Azure eszköztára Eclipse tooenable munkamenet kapcsolat használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 523e728c58bda95e7af4b242e831694eb6d75cb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-session-affinity"></a><span data-ttu-id="2fbbe-103">Munkamenet-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2fbbe-103">Enable Session Affinity</span></span>
<span data-ttu-id="2fbbe-104">Belül hello Azure eszköztára Eclipse engedélyezheti a HTTP-munkamenetben affinitás, munkamenetből vagy annak "kapcsolódó", a szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-104">Within hello Azure Toolkit for Eclipse, you can enable HTTP session affinity, or "sticky sessions", for your roles.</span></span> <span data-ttu-id="2fbbe-105">hello következő kép bemutatja hello **terheléselosztás** tulajdonságai párbeszédpanelen használt tooenable hello munkamenet affinitási szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="2fbbe-105">hello following image shows hello **Load Balancing** properties dialog used tooenable hello session affinity feature:</span></span>

![][ic719492]

## <a name="tooenable-session-affinity-for-your-role"></a><span data-ttu-id="2fbbe-106">tooenable munkamenet-kapcsolatot az adott szerepkörhöz</span><span class="sxs-lookup"><span data-stu-id="2fbbe-106">tooenable session affinity for your role</span></span>
1. <span data-ttu-id="2fbbe-107">Kattintson a jobb gombbal a Eclipse Project Explorer hello szerepkör, kattintson a **Azure**, és kattintson a **terheléselosztás**.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-107">Right-click hello role in Eclipse's Project Explorer, click **Azure**, and then click **Load Balancing**.</span></span>

2. <span data-ttu-id="2fbbe-108">A hello **WorkerRole1 terheléselosztás tulajdonságok** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="2fbbe-108">In hello **Properties for WorkerRole1 Load Balancing** dialog:</span></span>

   <span data-ttu-id="2fbbe-109">a.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-109">a.</span></span> <span data-ttu-id="2fbbe-110">Ellenőrizze **engedélyezése HTTP munkamenet affinitás (a kapcsolódó munkamenetek) ehhez a szerepkörhöz.**</span><span class="sxs-lookup"><span data-stu-id="2fbbe-110">Check **Enable HTTP session affinity (sticky sessions) for this role.**</span></span>

   <span data-ttu-id="2fbbe-111">b.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-111">b.</span></span> <span data-ttu-id="2fbbe-112">A **bemeneti végpont toouse**, válassza ki például egy bemeneti végpont toouse **http (nyilvános: 80-as, saját: 8080)**.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-112">For **Input endpoint toouse**, select an input endpoint toouse, for example, **http (public:80, private:8080)**.</span></span> <span data-ttu-id="2fbbe-113">Az alkalmazás a HTTP-végpontként kell használni ezt a végpontot.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-113">Your application must use this endpoint as its HTTP endpoint.</span></span> <span data-ttu-id="2fbbe-114">Több végpont engedélyezheti az adott szerepkörhöz, ugyanakkor kiválaszthatja csak az egyiket toosupport állandóságát munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-114">You can enable multiple endpoints for your role, but you can select only one of them toosupport sticky sessions.</span></span>

   <span data-ttu-id="2fbbe-115">c.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-115">c.</span></span> <span data-ttu-id="2fbbe-116">Építse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-116">Rebuild your application.</span></span>

<span data-ttu-id="2fbbe-117">Miután engedélyezte őket, ha egynél több szerepkörpéldányt, egy adott ügyfél érkező HTTP-kérelmek továbbra is kezeli hello azonos szerepkörpéldányt.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-117">Once enabled, if you have more than one role instance, HTTP requests coming from a particular client will continue being handled by hello same role instance.</span></span>

<span data-ttu-id="2fbbe-118">hello Eclipse eszközkészlet lehetővé teszi, hogy ez egy speciális, a szerepkörpéldányok mindegyik alkalmazás alkalmazáskérelmek útválasztása (ARR) nevű IIS modul telepítésével.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-118">hello Eclipse Toolkit enables this by installing a special IIS module called Application Request Routing (ARR) into each of your role instances.</span></span> <span data-ttu-id="2fbbe-119">Az ARR reroutes HTTP kérelmeket toohello megfelelő szerepkörpéldányt.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-119">ARR reroutes HTTP requests toohello appropriate role instance.</span></span> <span data-ttu-id="2fbbe-120">hello eszközkészlet automatikusan ismét beállítja a kiválasztott hello végpont, úgy, hogy hello bejövő HTTP-forgalom első irányított toohello ARR szoftver.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-120">hello toolkit automatically reconfigures hello selected endpoint so that hello incoming HTTP traffic is first routed toohello ARR software.</span></span> <span data-ttu-id="2fbbe-121">hello eszközkészlet is létrehoz egy új belső végpont, amely a Java-kiszolgáló számára konfigurált toolisten.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-121">hello toolkit also creates a new internal endpoint that your Java server is configured toolisten to.</span></span> <span data-ttu-id="2fbbe-122">Ez az ARR tooreroute hello HTTP forgalom toohello megfelelő szerepkör példánya által használt hello végpontnak.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-122">That is hello endpoint used by ARR tooreroute hello HTTP traffic toohello appropriate role instance.</span></span> <span data-ttu-id="2fbbe-123">Ezzel a módszerrel a többpéldányos környezetben minden szerepkörpéldányt, egy fordított proxy összes hello más esetekben a kapcsolódó munkamenetek engedélyezése szolgálja ki.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-123">This way, each role instance in your multi-instance deployment serves as a reverse proxy for all hello other instances, enabling sticky sessions.</span></span>

## <a name="notes-about-session-affinity"></a><span data-ttu-id="2fbbe-124">Munkamenet-kapcsolatra vonatkozó megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2fbbe-124">Notes about session affinity</span></span>
* <span data-ttu-id="2fbbe-125">Munkamenet-kapcsolat nem működik a hello compute emulator.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-125">Session affinity does not work in hello compute emulator.</span></span> <span data-ttu-id="2fbbe-126">hello-beállítások hello compute emulator a felépítési folyamat zavarása nélkül lehet alkalmazni, vagy az emulátor végrehajtási számítási, de hello szolgáltatást hello compute emulator belül nem működik.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-126">hello settings can be applied in hello compute emulator without interfering with your build process or compute emulator execution, but hello feature itself does not function within hello compute emulator.</span></span>

* <span data-ttu-id="2fbbe-127">Munkamenet-affinitás lemezterület az Azure, a telepítés során, további szoftvereket a rendszer letölti és telepíti a szerepkörpéldányok hello Azure-felhőbe a szolgáltatás indításakor hello mennyisége növekedését eredményezi.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-127">Enabling session affinity will result in an increase in hello amount of disk space taken up by your deployment in Azure, as additional software will be downloaded and installed into your role instances when your service is started in hello Azure cloud.</span></span>

* <span data-ttu-id="2fbbe-128">hello idő tooinitialize szerepkörönként hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-128">hello time tooinitialize each role will take longer.</span></span>

* <span data-ttu-id="2fbbe-129">Belső végpont, mint a forgalom rerouter, fent említett toofunction lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="2fbbe-129">An internal endpoint, toofunction as a traffic rerouter as mentioned above, will be added.</span></span>


## <a name="see-also"></a><span data-ttu-id="2fbbe-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2fbbe-130">See Also</span></span>
<span data-ttu-id="2fbbe-131">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2fbbe-131">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="2fbbe-132">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2fbbe-132">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="2fbbe-133">[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2fbbe-133">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="2fbbe-134">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="2fbbe-134">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How tooMaintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
