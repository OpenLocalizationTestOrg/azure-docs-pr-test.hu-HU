---
title: "Mobile Engagement felhasználói felület – aaaAzure szegmensek"
description: "Megtudhatja, hogyan toocreate és kezelheti a szegmenseket, az Azure Mobile Engagementet használó felhasználók tooidentify használati minták"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: bb214c45d05ebfbf243978658a7e331d4a7c6e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-segments-of-users-tooidentify-usage-patterns"></a><span data-ttu-id="4d4e3-103">Hogyan toocreate és kezelheti a szegmenseket felhasználók tooidentify használati minták</span><span class="sxs-lookup"><span data-stu-id="4d4e3-103">How toocreate and manage segments of users tooidentify usage patterns</span></span>
<span data-ttu-id="4d4e3-104">Ez a cikk ismerteti a hello **SZEGMENSEK** hello lapján **a Mobile Engagement** portálon.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-104">This article describes hello **SEGMENTS** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="4d4e3-105">Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span>

<span data-ttu-id="4d4e3-106">felhasználói felület hello hello szegmensek szakasza lehetővé teszi toowork szegmentálja a felhasználók hello másképp viselkednek és elemzés, amelyet az alkalmazás hello kérheti le és hello szegmensek API keresztül is elérheti a.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-106">hello Segments section of hello UI allows you toowork on segmenting your users based on hello different behavior and analytics that you can get from hello application and can also access through hello Segments API.</span></span> <span data-ttu-id="4d4e3-107">Szegmensek először vannak számított 24 órával azt követően hozza létre őket, és azok vannak recomputed 24 óránként hello legújabb analytics adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-107">Segments are first computed 24 hours after they are created, and they are recomputed every 24 hours based on hello latest analytics information.</span></span> <span data-ttu-id="4d4e3-108">Miután egy szegmens kiszámítása, azt jeleníti meg a "Nap tooday előzmények" naponta.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-108">Once a segment is calculated, it displays a "Day tooday history" chart each day.</span></span>

> [!NOTE]
> <span data-ttu-id="4d4e3-109">Hello sok szakasza **a Mobile Engagement** portál felhasználói felületének tartalmazhat hello **megjelenítése SÚGÓ** gombra.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-109">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="4d4e3-110">Nyomja meg a gomb tooget szakasz környezetfüggő tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-110">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="create-segments"></a><span data-ttu-id="4d4e3-111">Szegmensek létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d4e3-111">Create Segments</span></span>
<span data-ttu-id="4d4e3-112">A szegmens hello too60 napokat be egy adott időszakban a too10 feltételek alapján hozhat létre múltbeli hello analytics szakaszából.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-112">You can create a segment based on up too10 criteria on a specific period up too60 days in hello past from hello analytics section.</span></span> <span data-ttu-id="4d4e3-113">Például egy szegmens hello személyeket, akik bizonyos oldalait vagy keresni az alkalmazáson belül hello belül adott tartalomtípusok 10 napja alapján is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-113">For example, you can create a segment based on hello people who have viewed certain pages or searched for specific content within your app within hello last 10 days.</span></span> <span data-ttu-id="4d4e3-114">Ez az információ hello analytics szakaszában érhető el.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-114">This information is available in hello analytics section.</span></span> <span data-ttu-id="4d4e3-115">Igen, toocreate egy szegmens használják, és hozzon létre egy leküldéses értesítési tootarget a felhasználók tooget részhalmazát őket toocome hátsó toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-115">So, you can use it toocreate a segment, and then set up a push notification tootarget this subset of users tooget them toocome back toohello application.</span></span> 

> [!NOTE]
> <span data-ttu-id="4d4e3-116">Miután egy szegmens kiszámított, ezért nem lehet szerkeszteni; azt is csak klónozáshoz (másolt) vagy megsemmisül (törölve).</span><span class="sxs-lookup"><span data-stu-id="4d4e3-116">Once a segment has been calculated, it cannot be edited; it can only be cloned (copied) or destroyed (deleted).</span></span> <span data-ttu-id="4d4e3-117">Egy szegmens belül hello klónozható ugyanahhoz az alkalmazáshoz (az azonos AppID hello), és azt is klónozható más alkalmazásokba (a különböző AppID).</span><span class="sxs-lookup"><span data-stu-id="4d4e3-117">A segment can be cloned within hello same application (with hello same AppID), and it can also be cloned into other applications (with a different AppID).</span></span> 

 ![segments1][35] 

## <a name="examples-segments"></a><span data-ttu-id="4d4e3-119">Példák szegmensek</span><span class="sxs-lookup"><span data-stu-id="4d4e3-119">Examples Segments</span></span>
 ![segments2][36]

<span data-ttu-id="4d4e3-121">Szegmensek toosegment hello végfelhasználók számára az alkalmazás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-121">Segments allow you toosegment hello end-users of your application.</span></span>
<span data-ttu-id="4d4e3-122">A felhasználók szegmentálásával egy fontos marketing stratégia.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-122">Segmenting your users is an important marketing strategy.</span></span> <span data-ttu-id="4d4e3-123">Az Azure Mobile Engagement lehetővé teszi a tooget régebbi adatok, és hozzon létre egyéni szegmensek.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-123">Azure Mobile Engagement allows you tooget historic data and create custom segments.</span></span> <span data-ttu-id="4d4e3-124">Az erőteljes eszköz lehetővé teszi az alkalmazás a felhasználók tapasztalatairól toolearn.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-124">This powerful tool enables you toolearn about your customers’ experience in your application.</span></span> <span data-ttu-id="4d4e3-125">Egyszerűen elemezheti a szegmenseket, és használja a szegmenseket leküldéses célként.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-125">You can easily analyze your segments and use your segments as push targets.</span></span>
<span data-ttu-id="4d4e3-126">Egy gyakori használati eset, amelyet egy értesítési tooencourage leküldéses toosend a végfelhasználók toorate hello áruházban az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-126">A common use-case is that you want toosend a push a notification tooencourage your end-users toorate your application in hello store.</span></span> <span data-ttu-id="4d4e3-127">Ahelyett, hogy egy értesítési tooall küld a végfelhasználók számára, létrehozhat kellene megadnia csak azok a felhasználók, amelyek az alkalmazás minden nap az előző hónap hello használt, és rendelkezésére állt-e a kiváló felhasználói felülettel szegmenssel.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-127">Rather than sending a notification tooall your end-users, you can create a segment that would specify only users that have used your application every day for hello last month and have had a great user experience.</span></span> <span data-ttu-id="4d4e3-128">Kevesebb, jól célzott leküldéses értesítéseket küldeni, a jobb Megtérülési nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-128">When you send fewer, highly targeted push notifications, you get a better ROI.</span></span>

 ![segments3][37]

### <a name="segments-you-can-create-based-on-hello-major-azure-mobile-engagement-elements"></a><span data-ttu-id="4d4e3-130">Létrehozhat alapuló szegmens hello fő Azure Mobile Engagement elemet:</span><span class="sxs-lookup"><span data-stu-id="4d4e3-130">Segments you can create based on hello major Azure Mobile Engagement elements:</span></span>
* <span data-ttu-id="4d4e3-131">Esemény: hozzon létre egy szegmenst a célokat egy adott esemény hello alkalmazás, amely hetente legfeljebb kétszer került sor.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-131">Event: create a segment that targets one specific event of hello application that happened more than twice a week.</span></span> 
* <span data-ttu-id="4d4e3-132">Munkamenet: hozzon létre egy részének a több mint 5 alkalommal múlt héten hello alkalmazást használó felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-132">Session: create a segment of users that have used hello application more than 5 times last week.</span></span>
* <span data-ttu-id="4d4e3-133">Tevékenység: hozzon létre egy részének a használó felhasználókat egy oldal, vagy a tartalom több vagy kevesebb mint 10 ideje elmúlt hónapban.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-133">Activity: create a segment of users that have used one page or content more or less than 10 time last month.</span></span>
* <span data-ttu-id="4d4e3-134">Feladat: a szegmens, amely naponta legfeljebb kétszer befejeződött egy feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-134">Job: create a segment of users that have completed a job more than twice a day.</span></span>
* <span data-ttu-id="4d4e3-135">Összeomlási: hozzon létre egy szegmens az összes olyan hello felhasználó 10 szintnél mélyebben múlt héten crash volt.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-135">Crash: create a segment of all hello users that have had a crash more than 10 times last week.</span></span> <span data-ttu-id="4d4e3-136">(Ebben a szegmensben egy Bocsánatkérő vagy akár egy ráta sikerült leküldéses!)</span><span class="sxs-lookup"><span data-stu-id="4d4e3-136">(You could push this segment with an apology or even a coupon!)</span></span>
* <span data-ttu-id="4d4e3-137">Hiba: a szegmens, amely több mint 100 alkalommal 3 napnál korábban hibába hello felhasználók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-137">Error: create a segment of all hello users that have had an error more than 100 times last 3 days.</span></span>
* <span data-ttu-id="4d4e3-138">Alkalmazásadat: hozzon létre egy szegmenst, amelynek célja egy egyéni App-Info 25 napja hello során történt.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-138">App Info: create a segment that targets a custom App Info that happened during hello last 25 days.</span></span>
  
  ![segments4][38]

<span data-ttu-id="4d4e3-140">toouse szegmens optimálisan, még kell meg egy testreszabott hello SDK integrációja "Alkalmazásadatok" címkék címkézési tervvel az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-140">toouse Segment in an optimal way, you must have done a customized integration of hello SDK in your application with a tagging plan of "App Info" tags.</span></span>
<span data-ttu-id="4d4e3-141">Folytassa a kezdőlap toohello hello illesztőfelület, ki hello alkalmazást, majd kattintson a hello "Szegmensek" szakasz.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-141">Then, go toohello home page of hello interface, select hello application you want and click on hello "Segments" section.</span></span>

1. <span data-ttu-id="4d4e3-142">Válassza ki a hello "Szegmensek" szakaszt.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-142">Select hello "Segments" section.</span></span>
2. <span data-ttu-id="4d4e3-143">Kattintson az "Új szegmens" hello toocreate új szegmens gombra kattint.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-143">Click on hello "New segment" button toocreate a new segment.</span></span>

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a><span data-ttu-id="4d4e3-144">Valós élettartama. példa: Hozzon létre egy egyszerű szegmens "Munkamenet" adatok alapján</span><span class="sxs-lookup"><span data-stu-id="4d4e3-144">Real Life Example: Create a simple segment based on "Session" information</span></span>
<span data-ttu-id="4d4e3-145">Hozzon létre egy szegmens az összes hello végfelhasználók számára, amely használták az alkalmazást legalább 50 alkalommal hello múlt héten.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-145">Create a segment of all hello end-users that have used your app at least 50 times in hello last week.</span></span> <span data-ttu-id="4d4e3-146">Ott csak az alkalmazás munkamenetenként legalább 30 másodperces töltötték hello végfelhasználók található.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-146">From there, find only hello end-users that have spent at least 30 seconds in your app per session.</span></span> <span data-ttu-id="4d4e3-147">Ez az alkalmazás egy pozitív élményt rendelkező összes hello végfelhasználók jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-147">This will show all hello end-users who have a positive experience in your app.</span></span> <span data-ttu-id="4d4e3-148">Ezután létre hello szegmens használt toopush egy értesítési toothese végfelhasználók tooask lehet őket toorate az alkalmazást a hello tárolja.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-148">Then, hello segment created could be used toopush a notification toothese end-users tooask them toorate your app in hello store.</span></span>

 ![segments5][39]

1. <span data-ttu-id="4d4e3-150">Nevezze el a szegmensek a rendelés toofind legyen gyorsan hello "Szegmens" lista.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-150">Give your segment a name in order toofind it quickly in hello "Segment" list.</span></span>
2. <span data-ttu-id="4d4e3-151">Kattintson a hello "Létrehozás" gombra.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-151">Click on hello "Create" button.</span></span>
   
   ![segments6][40]

<span data-ttu-id="4d4e3-153">Jelölje ki a munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-153">Select Session.</span></span>

 ![segments7][41]

1. <span data-ttu-id="4d4e3-155">Válassza az "Elmúlt hét" hello időszak.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-155">Select hello period of "Last week".</span></span>
2. <span data-ttu-id="4d4e3-156">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-156">Click next.</span></span>
   
   ![segments8][42]
3. <span data-ttu-id="4d4e3-158">SELECT hello között hello lista fontos operátor: =; ≥, ≤.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-158">Select hello Operator that is relevant among hello list: =; ≥, ≤.</span></span>
4. <span data-ttu-id="4d4e3-159">Adja meg a hello kívánt száma.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-159">Enter hello Count you want.</span></span>
5. <span data-ttu-id="4d4e3-160">Válassza ki a kívánt előfordulási hello.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-160">Select hello Occurrence you want.</span></span> 
6. <span data-ttu-id="4d4e3-161">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-161">Click next.</span></span>
   <span data-ttu-id="4d4e3-162">Ebben a példában van beállítva, így az adott over hello múlt héten egyezés felhasználók, amelyek legalább 50 munkamenetek történtek.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-162">This example is set so that over hello last week, match users that have made at least 50 sessions.</span></span>
   
   ![segments9][43]

<span data-ttu-id="4d4e3-164">Hello munkamenet szegmentálását kiválaszthatja feltételként munkamenetenként hello hossza.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-164">For hello Session segmentation, you can choose hello length per session as a criteria.</span></span>

1. <span data-ttu-id="4d4e3-165">Válassza ki a hello operátor hello listából.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-165">Select hello Operator from hello list.</span></span>
2. <span data-ttu-id="4d4e3-166">Adja meg a hello hossza munkamenetenként.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-166">Provide hello Length per session.</span></span>
3. <span data-ttu-id="4d4e3-167">Kattintson a Next (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-167">Click Next.</span></span>
   <span data-ttu-id="4d4e3-168">Ebben a példában a felirat látható, hogy folyamatosan hello munkamenetek, amelyek rendelkeznek lett szegmentált hello előfordulási szakasz, jelölje ki a csak hello felhasználókat, akik rendelkeznek a munkamenetenként több mint 30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-168">In this example, it says that over all hello sessions that have been segmented on hello occurrence section, select only hello users that have spent more than 30 seconds per session.</span></span>
   
   ![segments10][44]

<span data-ttu-id="4d4e3-170">Nevet a feltételnek rendelés tooretrieve legyen hello végezze el a tölcsér, és kattintson a Befejezés gombra.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-170">Name your criterion in order tooretrieve it in hello complete funnel, and click Finish.</span></span>

 ![segments11][45]

<span data-ttu-id="4d4e3-172">Miután végzett a beállítás mentése a feltételnek, a hello szegmens tölcsér jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-172">When you have finished setting up your criterion, it will appear in hello segment funnel.</span></span>
<span data-ttu-id="4d4e3-173">A szegmens analytics adatokon alapul, mert a szegmensek naponta egyszer kiszámítása a történik.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-173">Because a segment is based on analytics data, segments are computed once per day.</span></span>
<span data-ttu-id="4d4e3-174">Ebben a példában 47,7 hello teljes végfelhasználók % hello feltételnek megfelel.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-174">In this example, 47,7% of hello total end-users matched hello criterion.</span></span> <span data-ttu-id="4d4e3-175">Hello felhasználók rendelkezésére állt-e a megfelelő környezet kell őket, és fog kell egy magasabb értékelése, ha értesítést leküldéses őket valószínűleg tooprovide kéri a felhasználót toorate hello alkalmazás hello áruházban.</span><span class="sxs-lookup"><span data-stu-id="4d4e3-175">They should be hello users who have had a good experience and will be likely tooprovide a higher rating if you push them a notification asking them toorate hello app in hello store.</span></span>

## <a name="see-also"></a><span data-ttu-id="4d4e3-176">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4d4e3-176">See also</span></span>
* <span data-ttu-id="4d4e3-177">[Alapfogalmak][Link 6]</span><span class="sxs-lookup"><span data-stu-id="4d4e3-177">[Concepts][Link 6]</span></span>
* <span data-ttu-id="4d4e3-178">[Hibaelhárítási útmutató szolgáltatás][Link 24]</span><span class="sxs-lookup"><span data-stu-id="4d4e3-178">[Troubleshooting Guide Service][Link 24]</span></span>

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

