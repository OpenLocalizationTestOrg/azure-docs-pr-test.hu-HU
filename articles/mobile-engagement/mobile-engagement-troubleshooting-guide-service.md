---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatója - szolgáltatás"
description: "Hibaelhárítás az Azure Mobile Engagement útmutatók"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: cf48db323a873ccef95946f7bb26e8d7473c002f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a><span data-ttu-id="55ef5-103">A szolgáltatásokkal kapcsolatos problémákról hibaelhárítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="55ef5-103">Troubleshooting guide for Service issues</span></span>
<span data-ttu-id="55ef5-104">Az alábbiakban hello lehetséges problémák merülhetnek fel az Azure Mobile Engagement működésével.</span><span class="sxs-lookup"><span data-stu-id="55ef5-104">hello following are possible issues you may encounter with how Azure Mobile Engagement runs.</span></span>

## <a name="service-outages"></a><span data-ttu-id="55ef5-105">Szolgáltatás-kimaradások számát</span><span class="sxs-lookup"><span data-stu-id="55ef5-105">Service Outages</span></span>
### <a name="issue"></a><span data-ttu-id="55ef5-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="55ef5-106">Issue</span></span>
* <span data-ttu-id="55ef5-107">Azure Mobile Engagement szolgáltatáskimaradások által okozott toobe megjelenő problémákat.</span><span class="sxs-lookup"><span data-stu-id="55ef5-107">Issues that appear toobe caused by Azure Mobile Engagement Service Outages.</span></span>

### <a name="causes"></a><span data-ttu-id="55ef5-108">Okok</span><span class="sxs-lookup"><span data-stu-id="55ef5-108">Causes</span></span>
* <span data-ttu-id="55ef5-109">Azure Mobile Engagement szolgáltatáskimaradások által okozott toobe megjelenő problémák számos különböző probléma okozhatja:</span><span class="sxs-lookup"><span data-stu-id="55ef5-109">Issues that appear toobe caused by Azure Mobile Engagement Service Outages can be caused by several different issues:</span></span>
  * <span data-ttu-id="55ef5-110">Az Azure Mobile Engagement rendszeres tooall eredetileg megjelenő elkülönített problémák</span><span class="sxs-lookup"><span data-stu-id="55ef5-110">Isolated issues that originally appear systemic tooall of Azure Mobile Engagement</span></span>
  * <span data-ttu-id="55ef5-111">Ismert problémák okozta a kiszolgáló valamilyen okból kimaradás (mindig nem jeleníti meg a kiszolgáló állapota):</span><span class="sxs-lookup"><span data-stu-id="55ef5-111">Known issues caused by server outages (not always shows in server status):</span></span>
  * <span data-ttu-id="55ef5-112">Ütemezés késések célcsoportkezelést, jelvény a frissítéssel kapcsolatos problémák, hibák statisztika stop gyűjtése, leküldéses nem működik, API leáll, új alkalmazásokat, vagy a felhasználók nem hozható létre, DNS-hibák, és időtúllépést hello felhasználói felület, API vagy alkalmazásokat az eszközön.</span><span class="sxs-lookup"><span data-stu-id="55ef5-112">Scheduling delays, Targeting errors, Badge update issues, Statistics stop collecting, Push stops working, API's stop working, New apps or users can't be created, DNS errors, and Timeout errors in hello UI, API, or Apps on a device.</span></span>
  * <span data-ttu-id="55ef5-113">Függőség kimaradások cloud [Azure szolgáltatás állapota](http://status.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="55ef5-113">Cloud Dependency Outages [Azure Service Status](http://status.azure.com/)</span></span>
  * <span data-ttu-id="55ef5-114">Leküldéses értesítési szolgáltatások (PNS) függőségi leállások esetén</span><span class="sxs-lookup"><span data-stu-id="55ef5-114">Push Notification Services (PNS) Dependency Outages</span></span>
  * <span data-ttu-id="55ef5-115">Alkalmazás-áruház leállások esetén</span><span class="sxs-lookup"><span data-stu-id="55ef5-115">App Store Outages</span></span>

1) <span data-ttu-id="55ef5-116">Ha hello probléma rendszeres tootest toosee olyan funkciókat, a különböző hello tesztelheti</span><span class="sxs-lookup"><span data-stu-id="55ef5-116">tootest toosee if hello problem is systemic you can test hello same function from a different</span></span>

* <span data-ttu-id="55ef5-117">Az Azure Mobile Engagement integrált alkalmazás</span><span class="sxs-lookup"><span data-stu-id="55ef5-117">Azure Mobile Engagement integrated application</span></span>
* <span data-ttu-id="55ef5-118">Vizsgálati eszköz</span><span class="sxs-lookup"><span data-stu-id="55ef5-118">Test device</span></span>
* <span data-ttu-id="55ef5-119">Vizsgálati eszköz operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="55ef5-119">Test device OS version</span></span>
* <span data-ttu-id="55ef5-120">Kampány</span><span class="sxs-lookup"><span data-stu-id="55ef5-120">Campaign</span></span>
* <span data-ttu-id="55ef5-121">Rendszergazdai felhasználói fiókkal</span><span class="sxs-lookup"><span data-stu-id="55ef5-121">Administrator user account</span></span>
* <span data-ttu-id="55ef5-122">Böngésző (IE, Firefox Chrome, stb.)</span><span class="sxs-lookup"><span data-stu-id="55ef5-122">Browser (IE, Firefox, Chrome, etc.)</span></span>
* <span data-ttu-id="55ef5-123">Computer</span><span class="sxs-lookup"><span data-stu-id="55ef5-123">Computer</span></span>

2) <span data-ttu-id="55ef5-124">Ha a probléma hello csak hatással van a hello felhasználói felületén vagy a hello API tootest:</span><span class="sxs-lookup"><span data-stu-id="55ef5-124">tootest if hello problem only affects hello UI or hello API's:</span></span>

* <span data-ttu-id="55ef5-125">Teszt hello azonos mindkét hello Azure Mobile Engagement felhasználói felületén a funkciót, és Azure Mobile Engagement API hello.</span><span class="sxs-lookup"><span data-stu-id="55ef5-125">Test hello same function from both hello Azure Mobile Engagement UI and hello Azure Mobile Engagement API's.</span></span>

3) <span data-ttu-id="55ef5-126">Ha hello probléma van a mobiltelefon hálózati tootest:</span><span class="sxs-lookup"><span data-stu-id="55ef5-126">tootest if hello problem is with your Cellular Phone Network:</span></span>

* <span data-ttu-id="55ef5-127">Teszt során csatlakoztatott toohello interneten keresztül Wi-Fi és a 3G mobiltelefon hálózaton keresztül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="55ef5-127">Test while connected toohello Internet via WIFI and while connected via your 3G cellular phone network.</span></span>
* <span data-ttu-id="55ef5-128">Győződjön meg arról, hogy a tűzfal nem blokkolja hello Azure Mobile Engagement IP-címek és portok.</span><span class="sxs-lookup"><span data-stu-id="55ef5-128">Confirm that your firewall is not blocking any of hello Azure Mobile Engagement IP Addresses or Ports.</span></span>

4) <span data-ttu-id="55ef5-129">Ha az eszköz hello probléma tootest:</span><span class="sxs-lookup"><span data-stu-id="55ef5-129">tootest if hello problem is with your Device:</span></span>

* <span data-ttu-id="55ef5-130">Ha az eszköz képes tooconnect tooAzure a Mobile Engagement egy másik integrált Azure Mobile Engagement-alkalmazás tesztelése.</span><span class="sxs-lookup"><span data-stu-id="55ef5-130">Test if your Device is able tooconnect tooAzure Mobile Engagement with another Azure Mobile Engagement integrated app.</span></span>
* <span data-ttu-id="55ef5-131">Tesztelje, hogy eseményeket, a feladatok és az összeomlások hozhat létre, amely hello Azure Mobile Engagement felhasználói felülete látható a telefonjáról.</span><span class="sxs-lookup"><span data-stu-id="55ef5-131">Test that you can generate Events, Jobs, and Crashes from your phone that can be seen in hello Azure Mobile Engagement UI.</span></span> 
* <span data-ttu-id="55ef5-132">Ha az eszköz azonosító alapján hello Azure Mobile Engagement felhasználói felület tooyour eszközről küldhet leküldéses értesítést tesztelése</span><span class="sxs-lookup"><span data-stu-id="55ef5-132">Test if you can send push notifications from hello Azure Mobile Engagement UI tooyour device based on its Device ID.</span></span> 

5) <span data-ttu-id="55ef5-133">Ha hello probléma van az alkalmazás tootest:</span><span class="sxs-lookup"><span data-stu-id="55ef5-133">tootest if hello problem is with your App:</span></span>

* <span data-ttu-id="55ef5-134">Telepítse, és tesztelje az alkalmazás az emulátor helyett egy fizikai eszközről:</span><span class="sxs-lookup"><span data-stu-id="55ef5-134">Install and test your application from an emulator instead of from a physical device:</span></span>

6) <span data-ttu-id="55ef5-135">Ha hello probléma van az operációs rendszer frissítései tooend felhasználói eszközök, az SDK frissítési tooresolve igénylő tootest:</span><span class="sxs-lookup"><span data-stu-id="55ef5-135">tootest if hello problem is with OS upgrades tooend user Devices, which require an SDK upgrade tooresolve:</span></span>

* <span data-ttu-id="55ef5-136">Hello az operációs rendszer különböző verziói különböző eszközökön az alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="55ef5-136">Test your application on different devices with different versions of hello OS.</span></span>
* <span data-ttu-id="55ef5-137">Ellenőrizze, hogy hello hello SDK legújabb verzióját használja-e.</span><span class="sxs-lookup"><span data-stu-id="55ef5-137">Confirm that you are using hello most recent version of hello SDK.</span></span>

## <a name="connectivity-and-incorrect-information-issues"></a><span data-ttu-id="55ef5-138">Kapcsolat és a megfelelő információkat problémák</span><span class="sxs-lookup"><span data-stu-id="55ef5-138">Connectivity and Incorrect Information Issues</span></span>
### <a name="issue"></a><span data-ttu-id="55ef5-139">Probléma</span><span class="sxs-lookup"><span data-stu-id="55ef5-139">Issue</span></span>
* <span data-ttu-id="55ef5-140">Bejelentkezés problémák hello Azure Mobile Engagement felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="55ef5-140">Problems logging into hello Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="55ef5-141">Hello Azure Mobile Engagement API kapcsolati hibák.</span><span class="sxs-lookup"><span data-stu-id="55ef5-141">Connection errors with hello Azure Mobile Engagement API's.</span></span>
* <span data-ttu-id="55ef5-142">App-Info címkék feltöltésével keresztül hello Device API-val kapcsolatos problémák.</span><span class="sxs-lookup"><span data-stu-id="55ef5-142">Problems uploading App Info Tags via hello Device API.</span></span>
* <span data-ttu-id="55ef5-143">A problémák naplók és exportált adatok letöltése az Azure Mobile engagement szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="55ef5-143">Problems downloading logs or exported data from Azure Mobile Engagement.</span></span>
* <span data-ttu-id="55ef5-144">Helytelen információt hello Azure Mobile Engagement felhasználói felülete látható.</span><span class="sxs-lookup"><span data-stu-id="55ef5-144">Incorrect information shown in hello Azure Mobile Engagement UI.</span></span>
* <span data-ttu-id="55ef5-145">Azure Mobile Engagement naplók látható tájékoztatás helytelen.</span><span class="sxs-lookup"><span data-stu-id="55ef5-145">Incorrect information shown in Azure Mobile Engagement logs.</span></span>

### <a name="causes"></a><span data-ttu-id="55ef5-146">Okok</span><span class="sxs-lookup"><span data-stu-id="55ef5-146">Causes</span></span>
* <span data-ttu-id="55ef5-147">Győződjön meg arról, a felhasználói fiók rendelkezik a megfelelő engedélyek tooperform hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="55ef5-147">Confirm your user account has sufficient permissions tooperform hello task.</span></span>
* <span data-ttu-id="55ef5-148">Győződjön meg arról, hogy hello probléma nem elszigetelt tooone számítógép vagy a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="55ef5-148">Confirm that hello problem is not isolated tooone computer or your local network.</span></span>
* <span data-ttu-id="55ef5-149">Győződjön meg róla, hogy adott hello Azure Mobile Engagement-szolgáltatás nem jelentett üzemkimaradások.</span><span class="sxs-lookup"><span data-stu-id="55ef5-149">Confirm that that hello Azure Mobile Engagement service has no reported outages.</span></span>
* <span data-ttu-id="55ef5-150">Győződjön meg arról, hogy az App-Info címke fájlok hajtsa végre az összes szabályt:</span><span class="sxs-lookup"><span data-stu-id="55ef5-150">Confirm that your App Info Tag files follow all of these rules:</span></span>
  * <span data-ttu-id="55ef5-151">Használjon csak hello UTF8 karakterkészlet (hello ANSI karakterkészlet nem támogatott).</span><span class="sxs-lookup"><span data-stu-id="55ef5-151">Use only hello UTF8 character set (hello ANSI character set is not supported).</span></span>
  * <span data-ttu-id="55ef5-152">Használjon vesszőt "," hello elválasztó karakterként (a vesszőt nyithatja meg a szolgáltatási kérelem toorequest toochange hello .csv elválasztó karaktert pontosvesszővel tooanother karakter "," ";").</span><span class="sxs-lookup"><span data-stu-id="55ef5-152">Use a comma "," as hello separator character (you can open a service request toorequest toochange hello .csv separator character from a comma "," tooanother character such as a semi-colon ";").</span></span>
  * <span data-ttu-id="55ef5-153">Minden kisbetű használható logikai értékek a "true" és "false".</span><span class="sxs-lookup"><span data-stu-id="55ef5-153">Use all lower case for Boolean values "true" and "false".</span></span>
  * <span data-ttu-id="55ef5-154">Maximális fájlméret hello 35MB-nál kisebb fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="55ef5-154">Use a file that is smaller than hello maximum file size of 35MB.</span></span>

