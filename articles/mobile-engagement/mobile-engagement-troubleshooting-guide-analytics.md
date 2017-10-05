---
title: "Az Azure Mobile Engagement hibaelhárítási útmutatója - elemzés"
description: "Az Azure Mobile Engagement Analytics, a figyelés, a szegmentálási és az irányítópult problémák elhárítása"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04a7020a-ad74-4491-be69-0bd574890029
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e30c9ac0a8421ffcf4fc3e2548cfd7ac49701900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a><span data-ttu-id="d69d1-103">Az elemzés, a figyelés, a szegmentálási és az irányítópult problémák hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="d69d1-103">Troubleshooting guide for Analytics, Monitoring, Segmentation, and Dashboard issues</span></span>
<span data-ttu-id="d69d1-104">A következők: lehetséges problémák merülhetnek fel a hogyan Azure Mobile Engagement az alkalmazások, eszközök és felhasználók adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="d69d1-104">The following are possible issues you may encounter with how Azure Mobile Engagement gathers information about your applications, devices, and users.</span></span>

## <a name="missingdelayed-information"></a><span data-ttu-id="d69d1-105">Hiányzó/késleltetett információk</span><span class="sxs-lookup"><span data-stu-id="d69d1-105">Missing/Delayed information</span></span>
### <a name="issue"></a><span data-ttu-id="d69d1-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="d69d1-106">Issue</span></span>
* <span data-ttu-id="d69d1-107">Elemzés, Szegmentálás vagy irányítópulton megjelenő adatokat késleltetett.</span><span class="sxs-lookup"><span data-stu-id="d69d1-107">Information is delayed in appearing in Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="d69d1-108">Hiányzik az figyelés információ.</span><span class="sxs-lookup"><span data-stu-id="d69d1-108">Information is missing from Monitoring.</span></span>
* <span data-ttu-id="d69d1-109">Hiányzik az elemzés, Szegmentálás vagy irányítópult információ.</span><span class="sxs-lookup"><span data-stu-id="d69d1-109">Information is missing from Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="d69d1-110">Szerezze meg a szegmentálási korlátozza.</span><span class="sxs-lookup"><span data-stu-id="d69d1-110">Hitting segmentation limits.</span></span>

### <a name="causes"></a><span data-ttu-id="d69d1-111">Okok</span><span class="sxs-lookup"><span data-stu-id="d69d1-111">Causes</span></span>
* <span data-ttu-id="d69d1-112">Az elemzés API-hoz, a figyelő API, és szegmensek API használatával adatokat van-e a felhasználói felület hiányoznak az API-k segítségével látható.</span><span class="sxs-lookup"><span data-stu-id="d69d1-112">You can use the Analytics API, Monitor API, and Segments API to see if any data you are missing from the UI is visible through the APIs.</span></span>
* <span data-ttu-id="d69d1-113">Ha az Azure Mobile Engagement SDK nem megfelelően integrálva van az alkalmazás ezután sem lesz az elemzés, szegmentálását, figyelés vagy irányítópultok információk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d69d1-113">If the Azure Mobile Engagement SDK is not correctly integrated into your app then you won't be able to see information in the Analytics, Segmentation, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="d69d1-114">Szegmensek nem lehet módosítani a létrehozásuk után, szegmensek csak olyan számítógépekre "Klónozott" (másolt) vagy "megsemmisül" (törölve).</span><span class="sxs-lookup"><span data-stu-id="d69d1-114">Segments can't be changed once they are created, segments can only be "cloned" (copied) or "destroyed" (deleted).</span></span> <span data-ttu-id="d69d1-115">Szegmensek csak tartalmazhat 10 feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d69d1-115">Segments can only contain 10 criteria.</span></span>
* <span data-ttu-id="d69d1-116">A legjobb módszer a figyelésből objektuminformáció egy vizsgálati eszköz beállítása, távolítsa el és/vagy a vizsgálati eszköz telepítse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d69d1-116">The best way to test missing information from monitoring is to setup a test device, uninstall and/or reinstall the app on the test device.</span></span>
* <span data-ttu-id="d69d1-117">Információk 24 óránként frissül elemzés, Szegmentálás vagy irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="d69d1-117">Information is refreshed every 24 hours for Analytics, Segmentation, or Dashboards.</span></span>
* <span data-ttu-id="d69d1-118">Új szegmensek található információk csak akkor jönnek létre, akkor is, ha a szegmens előző információin alapuló követő 24 órában nem jeleníthető meg.</span><span class="sxs-lookup"><span data-stu-id="d69d1-118">Information in new segments may not be displayed until 24 hours after they are created even if the segment is based on previous information.</span></span>
* <span data-ttu-id="d69d1-119">A felhasználói felületen analytics adatok szűrése megjeleníti az összes ilyenek például az alkalmazás verziójától függetlenül (pl. "Lefagy" név szerint szűrve jelennek meg az 1-es és 2-es verziójának az alkalmazásban).</span><span class="sxs-lookup"><span data-stu-id="d69d1-119">Filtering your analytics data in the UI will show all examples of this type regardless of the version of your app (e.g. "Crashes" filtered by name will show from version 1 and version 2 of your app).</span></span>
* <span data-ttu-id="d69d1-120">Az elemzés az adott időszakban, amelynek telefonhoz helytelenül állította be a dátum a felhasználó tudta jelenik meg a megfelelő időszakra vonatkozó a felhasználói eszközbeállítások, a dátumot alapul.</span><span class="sxs-lookup"><span data-stu-id="d69d1-120">The time period for Analytics is based on the date from the users' device settings, so a user whose phone has the date incorrectly set could show up in the wrong time period.</span></span>
* <span data-ttu-id="d69d1-121">Nincs a kiszolgálóoldali adatok naplóz, amikor vissza gombját használja a "teszt" a leküldéses értesítések, az adatokat a valódi leküldéses kampányokra csak naplózza.</span><span class="sxs-lookup"><span data-stu-id="d69d1-121">No server side data is logged when you use the button to "test" pushes, data is only logged for real push campaigns.</span></span>

## <a name="cant-locate-items-in-ui"></a><span data-ttu-id="d69d1-122">Nem található elemek a felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="d69d1-122">Can't locate items in UI</span></span>
### <a name="issue"></a><span data-ttu-id="d69d1-123">Probléma</span><span class="sxs-lookup"><span data-stu-id="d69d1-123">Issue</span></span>
* <span data-ttu-id="d69d1-124">Nem hozható létre az egyes beépített alapuló szegmens, vagy egyéni alkalmazásadatok címke feltételek.</span><span class="sxs-lookup"><span data-stu-id="d69d1-124">Can't create segments based on certain built in or custom app info tag criteria.</span></span>
* <span data-ttu-id="d69d1-125">Nem található egyes beépített, vagy egyéni alkalmazásadatok elemzés, figyelés vagy irányítópultok szempontok címkét.</span><span class="sxs-lookup"><span data-stu-id="d69d1-125">Can't find certain built in or custom app info tag criteria in Analytics, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="d69d1-126">Az elemzés, figyelés, Szegmentálás vagy irányítópultok adatokat nem tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="d69d1-126">Can't interpret the data in Analytics, Monitoring, Segmentation, or Dashboards.</span></span>

### <a name="causes"></a><span data-ttu-id="d69d1-127">Okok</span><span class="sxs-lookup"><span data-stu-id="d69d1-127">Causes</span></span>
* <span data-ttu-id="d69d1-128">Néhány elemet a beépített és az app-info címkék csak leküldéses feltételként rendelkezésre állnak, de nem lehetnek felvett egy szegmens vagy látható elemzés, figyelés vagy irányítópult.</span><span class="sxs-lookup"><span data-stu-id="d69d1-128">Some built in items and app info tags are only available as push criteria but may not be added to a segment or visible from Analytics, Monitoring, or Dashboard.</span></span> 
* <span data-ttu-id="d69d1-129">Beépített elemek és alkalmazás info címkék, amely egy szegmens nem adható hozzá akkor beállítási listája minden egyes megadott célcsoport-kezelési szegmens alapján célzó megegyező művelet végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="d69d1-129">For built in items and app info tags that can't be added to a segment, you will need to setup list of targeting criteria in each campaign to perform the same function as targeting based on a segment.</span></span>
* <span data-ttu-id="d69d1-130">Tekintse meg a helyi menük az elemzés, figyelés, Szegmentálás és irányítópultok szakaszok Azure Mobile Engagement a felhasználói felület további segítséget és információt.</span><span class="sxs-lookup"><span data-stu-id="d69d1-130">See the context menus in the Analytics, Monitoring, Segmentation, and Dashboards sections of the Azure Mobile Engagement UI for more help and how to information.</span></span>

## <a name="crash-troubleshooting"></a><span data-ttu-id="d69d1-131">Összeomlás-hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="d69d1-131">Crash troubleshooting</span></span>
### <a name="issue"></a><span data-ttu-id="d69d1-132">Probléma</span><span class="sxs-lookup"><span data-stu-id="d69d1-132">Issue</span></span>
* <span data-ttu-id="d69d1-133">Alkalmazás összeomlik elemzés, figyelés és az irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d69d1-133">Application Crashes appearing in Analytics, Monitoring, or Dashboard.</span></span>

### <a name="causes"></a><span data-ttu-id="d69d1-134">Okok</span><span class="sxs-lookup"><span data-stu-id="d69d1-134">Causes</span></span>
* <span data-ttu-id="d69d1-135">Elhárítása alkalmazás összeomlik elemzés, figyelés vagy irányítópulton látható ellenőrizze, hogy az SDK korábbi verziói szolgáltatással kapcsolatos ismert problémák a kibocsátási megjegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="d69d1-135">To troubleshoot Application Crashes seen in Analytics, Monitoring, or Dashboard make sure to check the release notes for known issues with previous versions of the SDK.</span></span>
* <span data-ttu-id="d69d1-136">További hibaelhárításhoz alkalmazás-összeomlásokat hajtsa végre az esemény egy vizsgálati eszköz a telepített alkalmazásokkal rendelkező, keresse meg a az Eszközazonosítót az Azure Mobile Engagement felhasználói felület "Figyelője – események" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="d69d1-136">To further troubleshoot application crashes perform an event from a test device with your application installed and look up your device ID in the “Monitor – Events” section of the Azure Mobile Engagement UI.</span></span> <span data-ttu-id="d69d1-137">Végezze el az eseményt, amely okozza az alkalmazás összeomlását, és keresse meg az Azure Mobile Engagement felhasználói felület "Figyelője – összeomlási" részében tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="d69d1-137">Then perform the event that is causing your application to crash and look up additional information in the “Monitor – Crash” section of the Azure Mobile Engagement UI.</span></span> 

