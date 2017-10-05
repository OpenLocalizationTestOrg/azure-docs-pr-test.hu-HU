---
title: "Az Azure-eszközkészlet használata az Eclipse munkamenet-kapcsolat engedélyezése"
description: "Megtudhatja, hogyan engedélyezze a munkamenet-kapcsolatot az Azure-eszközkészlet használata az eclipse-ben."
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
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a><span data-ttu-id="706b6-103">Munkamenet-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="706b6-103">Enable Session Affinity</span></span>
<span data-ttu-id="706b6-104">Belül az Eclipse Azure eszközkészlet engedélyezheti a HTTP-munkamenetben affinitás, munkamenetből vagy annak "kapcsolódó", a szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="706b6-104">Within the Azure Toolkit for Eclipse, you can enable HTTP session affinity, or "sticky sessions", for your roles.</span></span> <span data-ttu-id="706b6-105">Az alábbi képen látható a **terheléselosztás** tulajdonságainak párbeszédpanelét jeleníti meg a munkamenet affinitási szolgáltatás lehetővé teszi, hogy:</span><span class="sxs-lookup"><span data-stu-id="706b6-105">The following image shows the **Load Balancing** properties dialog used to enable the session affinity feature:</span></span>

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a><span data-ttu-id="706b6-106">Munkamenet-kapcsolatot az adott szerepkörhöz engedélyezése</span><span class="sxs-lookup"><span data-stu-id="706b6-106">To enable session affinity for your role</span></span>
1. <span data-ttu-id="706b6-107">Kattintson a jobb gombbal a szerepkör által Eclipse Project Explorer parancsra **Azure**, és kattintson a **terheléselosztás**.</span><span class="sxs-lookup"><span data-stu-id="706b6-107">Right-click the role in Eclipse's Project Explorer, click **Azure**, and then click **Load Balancing**.</span></span>

2. <span data-ttu-id="706b6-108">Az a **WorkerRole1 terheléselosztás tulajdonságok** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="706b6-108">In the **Properties for WorkerRole1 Load Balancing** dialog:</span></span>

   <span data-ttu-id="706b6-109">a.</span><span class="sxs-lookup"><span data-stu-id="706b6-109">a.</span></span> <span data-ttu-id="706b6-110">Ellenőrizze **engedélyezése HTTP munkamenet affinitás (a kapcsolódó munkamenetek) ehhez a szerepkörhöz.**</span><span class="sxs-lookup"><span data-stu-id="706b6-110">Check **Enable HTTP session affinity (sticky sessions) for this role.**</span></span>

   <span data-ttu-id="706b6-111">b.</span><span class="sxs-lookup"><span data-stu-id="706b6-111">b.</span></span> <span data-ttu-id="706b6-112">A **bemeneti végpont használandó**, válassza ki például használatához bemeneti végpontja **http (nyilvános: 80-as, saját: 8080)**.</span><span class="sxs-lookup"><span data-stu-id="706b6-112">For **Input endpoint to use**, select an input endpoint to use, for example, **http (public:80, private:8080)**.</span></span> <span data-ttu-id="706b6-113">Az alkalmazás a HTTP-végpontként kell használni ezt a végpontot.</span><span class="sxs-lookup"><span data-stu-id="706b6-113">Your application must use this endpoint as its HTTP endpoint.</span></span> <span data-ttu-id="706b6-114">Több végpont engedélyezheti az adott szerepkörhöz, ugyanakkor kiválaszthatja, csak az egyiket a kapcsolódó munkamenetek támogatásához.</span><span class="sxs-lookup"><span data-stu-id="706b6-114">You can enable multiple endpoints for your role, but you can select only one of them to support sticky sessions.</span></span>

   <span data-ttu-id="706b6-115">c.</span><span class="sxs-lookup"><span data-stu-id="706b6-115">c.</span></span> <span data-ttu-id="706b6-116">Építse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="706b6-116">Rebuild your application.</span></span>

<span data-ttu-id="706b6-117">Miután engedélyezte őket, ha egynél több szerepkörpéldányt, egy adott ügyfél érkező HTTP-kérelmek továbbra is megegyező szerepkörpéldányon kezeli.</span><span class="sxs-lookup"><span data-stu-id="706b6-117">Once enabled, if you have more than one role instance, HTTP requests coming from a particular client will continue being handled by the same role instance.</span></span>

<span data-ttu-id="706b6-118">Az Eclipse eszközkészlet lehetővé teszi, hogy ez egy speciális, a szerepkörpéldányok mindegyik alkalmazás alkalmazáskérelmek útválasztása (ARR) nevű IIS modul telepítésével.</span><span class="sxs-lookup"><span data-stu-id="706b6-118">The Eclipse Toolkit enables this by installing a special IIS module called Application Request Routing (ARR) into each of your role instances.</span></span> <span data-ttu-id="706b6-119">Az ARR reroutes HTTP-kéréseket a megfelelő szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="706b6-119">ARR reroutes HTTP requests to the appropriate role instance.</span></span> <span data-ttu-id="706b6-120">Az eszközkészlet automatikusan újrakonfigurálja a kijelölt végponttanúsítvány, úgy, hogy a bejövő HTTP-forgalom először csatlakoznak az ARR szoftver.</span><span class="sxs-lookup"><span data-stu-id="706b6-120">The toolkit automatically reconfigures the selected endpoint so that the incoming HTTP traffic is first routed to the ARR software.</span></span> <span data-ttu-id="706b6-121">Az eszközkészlet is létrehoz egy új belső végpontot, amely a Java kiszolgáló figyelésére van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="706b6-121">The toolkit also creates a new internal endpoint that your Java server is configured to listen to.</span></span> <span data-ttu-id="706b6-122">Ez a végpont a megfelelő szerepkör-példányhoz a HTTP-forgalom átirányítása ARR segítségével.</span><span class="sxs-lookup"><span data-stu-id="706b6-122">That is the endpoint used by ARR to reroute the HTTP traffic to the appropriate role instance.</span></span> <span data-ttu-id="706b6-123">Ezzel a módszerrel a többpéldányos környezetben minden szerepkörpéldányt fordított proxykiszolgálójaként a többi példányát, a kapcsolódó munkamenetek engedélyezése funkcionál.</span><span class="sxs-lookup"><span data-stu-id="706b6-123">This way, each role instance in your multi-instance deployment serves as a reverse proxy for all the other instances, enabling sticky sessions.</span></span>

## <a name="notes-about-session-affinity"></a><span data-ttu-id="706b6-124">Munkamenet-kapcsolatra vonatkozó megjegyzések</span><span class="sxs-lookup"><span data-stu-id="706b6-124">Notes about session affinity</span></span>
* <span data-ttu-id="706b6-125">Munkamenet-kapcsolat nem működik a compute emulator.</span><span class="sxs-lookup"><span data-stu-id="706b6-125">Session affinity does not work in the compute emulator.</span></span> <span data-ttu-id="706b6-126">A beállítások a felépítési folyamat zavarása nélkül lehet alkalmazni a compute emulator vagy compute emulator végrehajtási, de maga a szolgáltatás nem működik a compute emulator belül.</span><span class="sxs-lookup"><span data-stu-id="706b6-126">The settings can be applied in the compute emulator without interfering with your build process or compute emulator execution, but the feature itself does not function within the compute emulator.</span></span>

* <span data-ttu-id="706b6-127">Munkamenet-affinitás lemezterület az Azure, a telepítés során, további szoftvereket a rendszer letölti és telepíti a szerepkörpéldányok a szolgáltatás indításakor Azure felhőben mennyiségének olyan növekedése eredményez.</span><span class="sxs-lookup"><span data-stu-id="706b6-127">Enabling session affinity will result in an increase in the amount of disk space taken up by your deployment in Azure, as additional software will be downloaded and installed into your role instances when your service is started in the Azure cloud.</span></span>

* <span data-ttu-id="706b6-128">Minden egyes szerepkör inicializálása idő hosszabb ideig tart.</span><span class="sxs-lookup"><span data-stu-id="706b6-128">The time to initialize each role will take longer.</span></span>

* <span data-ttu-id="706b6-129">A forgalom rerouter fog működni, fent említett belső végpont lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="706b6-129">An internal endpoint, to function as a traffic rerouter as mentioned above, will be added.</span></span>


## <a name="see-also"></a><span data-ttu-id="706b6-130">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="706b6-130">See Also</span></span>
<span data-ttu-id="706b6-131">[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="706b6-131">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="706b6-132">[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="706b6-132">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="706b6-133">[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="706b6-133">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="706b6-134">Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="706b6-134">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
