---
title: "aaaAzure Mobile Engagement hibaelhárítási útmutatója - elemzés"
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
ms.openlocfilehash: 69c6ff8f5c8540f8ba8b85b9ffec55acc59329fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a><span data-ttu-id="9751b-103">Az elemzés, a figyelés, a szegmentálási és az irányítópult problémák hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="9751b-103">Troubleshooting guide for Analytics, Monitoring, Segmentation, and Dashboard issues</span></span>
<span data-ttu-id="9751b-104">hello az alábbiakban lehetséges problémák merülhetnek fel a hogyan Azure Mobile Engagement az alkalmazások, eszközök és felhasználók adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="9751b-104">hello following are possible issues you may encounter with how Azure Mobile Engagement gathers information about your applications, devices, and users.</span></span>

## <a name="missingdelayed-information"></a><span data-ttu-id="9751b-105">Hiányzó/késleltetett információk</span><span class="sxs-lookup"><span data-stu-id="9751b-105">Missing/Delayed information</span></span>
### <a name="issue"></a><span data-ttu-id="9751b-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="9751b-106">Issue</span></span>
* <span data-ttu-id="9751b-107">Elemzés, Szegmentálás vagy irányítópulton megjelenő adatokat késleltetett.</span><span class="sxs-lookup"><span data-stu-id="9751b-107">Information is delayed in appearing in Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="9751b-108">Hiányzik az figyelés információ.</span><span class="sxs-lookup"><span data-stu-id="9751b-108">Information is missing from Monitoring.</span></span>
* <span data-ttu-id="9751b-109">Hiányzik az elemzés, Szegmentálás vagy irányítópult információ.</span><span class="sxs-lookup"><span data-stu-id="9751b-109">Information is missing from Analytics, Segmentation, or Dashboard.</span></span>
* <span data-ttu-id="9751b-110">Szerezze meg a szegmentálási korlátozza.</span><span class="sxs-lookup"><span data-stu-id="9751b-110">Hitting segmentation limits.</span></span>

### <a name="causes"></a><span data-ttu-id="9751b-111">Okok</span><span class="sxs-lookup"><span data-stu-id="9751b-111">Causes</span></span>
* <span data-ttu-id="9751b-112">Hello Analytics API figyelő API-t is használhatja, és a szegmensek API toosee, ha bármely adatok hiányoznak a felhasználói felület hello látható hello API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="9751b-112">You can use hello Analytics API, Monitor API, and Segments API toosee if any data you are missing from hello UI is visible through hello APIs.</span></span>
* <span data-ttu-id="9751b-113">Ha hello Azure Mobile Engagement SDK nem megfelelően integrálva van az alkalmazás ezután nem fog hello elemzés, szegmentálását, figyelés vagy irányítópultok képes toosee adatokat.</span><span class="sxs-lookup"><span data-stu-id="9751b-113">If hello Azure Mobile Engagement SDK is not correctly integrated into your app then you won't be able toosee information in hello Analytics, Segmentation, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="9751b-114">Szegmensek nem lehet módosítani a létrehozásuk után, szegmensek csak olyan számítógépekre "Klónozott" (másolt) vagy "megsemmisül" (törölve).</span><span class="sxs-lookup"><span data-stu-id="9751b-114">Segments can't be changed once they are created, segments can only be "cloned" (copied) or "destroyed" (deleted).</span></span> <span data-ttu-id="9751b-115">Szegmensek csak tartalmazhat 10 feltételeket.</span><span class="sxs-lookup"><span data-stu-id="9751b-115">Segments can only contain 10 criteria.</span></span>
* <span data-ttu-id="9751b-116">hello hiányoznak egyes adatok megfigyelése legjobb módja tootest toosetup egy vizsgálati eszköz, távolítsa el és/vagy telepítse újra az hello alkalmazást hello vizsgálati eszköz.</span><span class="sxs-lookup"><span data-stu-id="9751b-116">hello best way tootest missing information from monitoring is toosetup a test device, uninstall and/or reinstall hello app on hello test device.</span></span>
* <span data-ttu-id="9751b-117">Információk 24 óránként frissül elemzés, Szegmentálás vagy irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="9751b-117">Information is refreshed every 24 hours for Analytics, Segmentation, or Dashboards.</span></span>
* <span data-ttu-id="9751b-118">Új szegmensek található információk csak akkor jönnek létre, akkor is, ha az előző információin alapuló hello szegmens követő 24 órában nem jeleníthető meg.</span><span class="sxs-lookup"><span data-stu-id="9751b-118">Information in new segments may not be displayed until 24 hours after they are created even if hello segment is based on previous information.</span></span>
* <span data-ttu-id="9751b-119">A felhasználói felület hello analytics adatok szűrése megjeleníti az összes ilyenek például az alkalmazás hello verziótól függetlenül (pl. "Lefagy" név szerint szűrve jelennek meg az 1-es és 2-es verziójának az alkalmazásban).</span><span class="sxs-lookup"><span data-stu-id="9751b-119">Filtering your analytics data in hello UI will show all examples of this type regardless of hello version of your app (e.g. "Crashes" filtered by name will show from version 1 and version 2 of your app).</span></span>
* <span data-ttu-id="9751b-120">hello Analytics időszakát alapul hello felhasználói eszközbeállítások, hello dátum, a felhasználó, akinek telefonhoz hello dátum helytelenül állította be a sikerült megjelennek hello megfelelő időszakra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="9751b-120">hello time period for Analytics is based on hello date from hello users' device settings, so a user whose phone has hello date incorrectly set could show up in hello wrong time period.</span></span>
* <span data-ttu-id="9751b-121">Nem a rendszer adatokat naplózza hello gomb használatakor kiszolgálóoldali túl "teszt" leküldéses értesítések, adatokat csak naplózza a valódi leküldéses kampányokra.</span><span class="sxs-lookup"><span data-stu-id="9751b-121">No server side data is logged when you use hello button too"test" pushes, data is only logged for real push campaigns.</span></span>

## <a name="cant-locate-items-in-ui"></a><span data-ttu-id="9751b-122">Nem található elemek a felhasználói felületen</span><span class="sxs-lookup"><span data-stu-id="9751b-122">Can't locate items in UI</span></span>
### <a name="issue"></a><span data-ttu-id="9751b-123">Probléma</span><span class="sxs-lookup"><span data-stu-id="9751b-123">Issue</span></span>
* <span data-ttu-id="9751b-124">Nem hozható létre az egyes beépített alapuló szegmens, vagy egyéni alkalmazásadatok címke feltételek.</span><span class="sxs-lookup"><span data-stu-id="9751b-124">Can't create segments based on certain built in or custom app info tag criteria.</span></span>
* <span data-ttu-id="9751b-125">Nem található egyes beépített, vagy egyéni alkalmazásadatok elemzés, figyelés vagy irányítópultok szempontok címkét.</span><span class="sxs-lookup"><span data-stu-id="9751b-125">Can't find certain built in or custom app info tag criteria in Analytics, Monitoring, or Dashboards.</span></span>
* <span data-ttu-id="9751b-126">Elemzés, figyelés, Szegmentálás vagy irányítópultok hello adatokat nem tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="9751b-126">Can't interpret hello data in Analytics, Monitoring, Segmentation, or Dashboards.</span></span>

### <a name="causes"></a><span data-ttu-id="9751b-127">Okok</span><span class="sxs-lookup"><span data-stu-id="9751b-127">Causes</span></span>
* <span data-ttu-id="9751b-128">Néhány beépített elemek és alkalmazásadatok címkék csak leküldéses feltételként használható, de nem lehet hozzáadni tooa szegmens vagy elemzés, figyelés vagy irányítópulton látható.</span><span class="sxs-lookup"><span data-stu-id="9751b-128">Some built in items and app info tags are only available as push criteria but may not be added tooa segment or visible from Analytics, Monitoring, or Dashboard.</span></span> 
* <span data-ttu-id="9751b-129">A beépített elemek és alkalmazásadatok nem lehet címkéket hozzáadni tooa szegmens, szüksége lesz minden olyan funkciókat, mint egy szegmens alapján kampány tooperform hello feltételeket célzó toosetup listája.</span><span class="sxs-lookup"><span data-stu-id="9751b-129">For built in items and app info tags that can't be added tooa segment, you will need toosetup list of targeting criteria in each campaign tooperform hello same function as targeting based on a segment.</span></span>
* <span data-ttu-id="9751b-130">Hello helyi menük hello elemzés, figyelés, Szegmentálás és irányítópultok fejezeteiben hello Azure Mobile Engagement felhasználói felületén, további segítségért tekintse meg, és hogyan tooinformation.</span><span class="sxs-lookup"><span data-stu-id="9751b-130">See hello context menus in hello Analytics, Monitoring, Segmentation, and Dashboards sections of hello Azure Mobile Engagement UI for more help and how tooinformation.</span></span>

## <a name="crash-troubleshooting"></a><span data-ttu-id="9751b-131">Összeomlás-hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9751b-131">Crash troubleshooting</span></span>
### <a name="issue"></a><span data-ttu-id="9751b-132">Probléma</span><span class="sxs-lookup"><span data-stu-id="9751b-132">Issue</span></span>
* <span data-ttu-id="9751b-133">Alkalmazás összeomlik elemzés, figyelés és az irányítópult jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9751b-133">Application Crashes appearing in Analytics, Monitoring, or Dashboard.</span></span>

### <a name="causes"></a><span data-ttu-id="9751b-134">Okok</span><span class="sxs-lookup"><span data-stu-id="9751b-134">Causes</span></span>
* <span data-ttu-id="9751b-135">tootroubleshoot alkalmazás összeomlik elemzés, figyelés vagy irányítópulton látható győződjön meg arról, hogy toocheck hello kibocsátási megjegyzések az ismert problémák hello SDK korábbi verzióival.</span><span class="sxs-lookup"><span data-stu-id="9751b-135">tootroubleshoot Application Crashes seen in Analytics, Monitoring, or Dashboard make sure toocheck hello release notes for known issues with previous versions of hello SDK.</span></span>
* <span data-ttu-id="9751b-136">toofurther hibáinak elhárítása az alkalmazás összeomlik végezzen el egy eseményt a vizsgálati eszköz a telepített alkalmazásokkal rendelkező, és keresse meg az Eszközazonosítót hello Azure Mobile Engagement felhasználói felületén hello "figyelője – események" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="9751b-136">toofurther troubleshoot application crashes perform an event from a test device with your application installed and look up your device ID in hello “Monitor – Events” section of hello Azure Mobile Engagement UI.</span></span> <span data-ttu-id="9751b-137">Ezután hajtsa végre az alkalmazás toocrash okozó hello esemény, és további tudnivalókat a hello hello Azure Mobile Engagement felhasználói felület "Figyelője – összeomlás" szakasza.</span><span class="sxs-lookup"><span data-stu-id="9751b-137">Then perform hello event that is causing your application toocrash and look up additional information in hello “Monitor – Crash” section of hello Azure Mobile Engagement UI.</span></span> 

