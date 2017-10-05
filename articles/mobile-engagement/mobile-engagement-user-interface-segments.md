---
title: "Az Azure Mobile Engagement felhasználói felület - szegmensek"
description: "Útmutató: az Azure Mobile Engagement segítségével használati minták azonosításához felhasználók szegmenseinek létrehozása és kezelése"
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
ms.openlocfilehash: 087f4a1fef420abe9669f8dfe2b84c7a847ce263
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a><span data-ttu-id="83abc-103">Hogyan kell a használati minták azonosításához felhasználók szegmenseinek létrehozása és kezelése</span><span class="sxs-lookup"><span data-stu-id="83abc-103">How to create and manage segments of users to identify usage patterns</span></span>
<span data-ttu-id="83abc-104">Ez a cikk ismerteti a **SZEGMENSEK** lapján a **a Mobile Engagement** portálon.</span><span class="sxs-lookup"><span data-stu-id="83abc-104">This article describes the **SEGMENTS** tab of the **Mobile Engagement** portal.</span></span> <span data-ttu-id="83abc-105">Használja a **a Mobile Engagement** portal felügyeletét és kezelését a mobile apps szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="83abc-105">You use the **Mobile Engagement** portal to monitor and manage your mobile apps.</span></span>

<span data-ttu-id="83abc-106">A felhasználói felület szegmensek szakasza lehetővé teszi a felhasználók különböző viselkedését és érheti el az alkalmazást, és a szegmensek API-n keresztül is elérheti analytics alapján szegmentálja a.</span><span class="sxs-lookup"><span data-stu-id="83abc-106">The Segments section of the UI allows you to work on segmenting your users based on the different behavior and analytics that you can get from the application and can also access through the Segments API.</span></span> <span data-ttu-id="83abc-107">Szegmensek először vannak számított 24 órával azt követően hozza létre őket, és azok vannak recomputed 24 óránként legújabb analytics információk alapján.</span><span class="sxs-lookup"><span data-stu-id="83abc-107">Segments are first computed 24 hours after they are created, and they are recomputed every 24 hours based on the latest analytics information.</span></span> <span data-ttu-id="83abc-108">Miután egy szegmens kiszámítása, azt jeleníti meg a "Day nap előzményei" naponta.</span><span class="sxs-lookup"><span data-stu-id="83abc-108">Once a segment is calculated, it displays a "Day to day history" chart each day.</span></span>

> [!NOTE]
> <span data-ttu-id="83abc-109">Sok szakasza a **a Mobile Engagement** portál felhasználói felületének tartalmaz a **megjelenítése SÚGÓ** gombra.</span><span class="sxs-lookup"><span data-stu-id="83abc-109">Many sections of the **Mobile Engagement** portal UI contain the **SHOW HELP** button.</span></span> <span data-ttu-id="83abc-110">Nyomja le az erre a gombra kattintva szakasz környezetfüggő ismertetése.</span><span class="sxs-lookup"><span data-stu-id="83abc-110">Press this button to get more contextual information about a section.</span></span>
> 
> 

## <a name="create-segments"></a><span data-ttu-id="83abc-111">Szegmensek létrehozása</span><span class="sxs-lookup"><span data-stu-id="83abc-111">Create Segments</span></span>
<span data-ttu-id="83abc-112">Létrehozhat egy szegmens legfeljebb 10 feltétel alapján a meghatározott időtartamra fel az elmúlt 60 napra analytics szakaszából.</span><span class="sxs-lookup"><span data-stu-id="83abc-112">You can create a segment based on up to 10 criteria on a specific period up to 60 days in the past from the analytics section.</span></span> <span data-ttu-id="83abc-113">Például egy szegmens a személyeket, akik bizonyos oldalait vagy az utolsó 10 napon belül az adott tartalomhoz az alkalmazáson belül keres alapján is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="83abc-113">For example, you can create a segment based on the people who have viewed certain pages or searched for specific content within your app within the last 10 days.</span></span> <span data-ttu-id="83abc-114">Ez az információ analytics szakaszában érhető el.</span><span class="sxs-lookup"><span data-stu-id="83abc-114">This information is available in the analytics section.</span></span> <span data-ttu-id="83abc-115">Igen segítségével azt hoznia egy szegmenst, és majd állítsa be úgy, hogy a felhasználók részhalmazát célozza leküldéses értesítés azokat a térjen vissza az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="83abc-115">So, you can use it to create a segment, and then set up a push notification to target this subset of users to get them to come back to the application.</span></span> 

> [!NOTE]
> <span data-ttu-id="83abc-116">Miután egy szegmens kiszámított, ezért nem lehet szerkeszteni; azt is csak klónozáshoz (másolt) vagy megsemmisül (törölve).</span><span class="sxs-lookup"><span data-stu-id="83abc-116">Once a segment has been calculated, it cannot be edited; it can only be cloned (copied) or destroyed (deleted).</span></span> <span data-ttu-id="83abc-117">A szegmens klónozható a (ugyanazon alkalmazásazonosítóval rendelkező azonos AppID) belül, és azt is klónozható más alkalmazásokba (a különböző AppID).</span><span class="sxs-lookup"><span data-stu-id="83abc-117">A segment can be cloned within the same application (with the same AppID), and it can also be cloned into other applications (with a different AppID).</span></span> 

 ![segments1][35] 

## <a name="examples-segments"></a><span data-ttu-id="83abc-119">Példák szegmensek</span><span class="sxs-lookup"><span data-stu-id="83abc-119">Examples Segments</span></span>
 ![segments2][36]

<span data-ttu-id="83abc-121">Szegmensek lehetővé teszi a szegmenseket, a végfelhasználók számára az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="83abc-121">Segments allow you to segment the end-users of your application.</span></span>
<span data-ttu-id="83abc-122">A felhasználók szegmentálásával egy fontos marketing stratégia.</span><span class="sxs-lookup"><span data-stu-id="83abc-122">Segmenting your users is an important marketing strategy.</span></span> <span data-ttu-id="83abc-123">Az Azure Mobile Engagement lehetővé teszi a régebbi adatok, és hozzon létre egyéni szegmensek.</span><span class="sxs-lookup"><span data-stu-id="83abc-123">Azure Mobile Engagement allows you to get historic data and create custom segments.</span></span> <span data-ttu-id="83abc-124">Az erőteljes eszköz lehetővé teszi az alkalmazás a felhasználók tapasztalatai megismerése.</span><span class="sxs-lookup"><span data-stu-id="83abc-124">This powerful tool enables you to learn about your customers’ experience in your application.</span></span> <span data-ttu-id="83abc-125">Egyszerűen elemezheti a szegmenseket, és használja a szegmenseket leküldéses célként.</span><span class="sxs-lookup"><span data-stu-id="83abc-125">You can easily analyze your segments and use your segments as push targets.</span></span>
<span data-ttu-id="83abc-126">Egy gyakori használati eset az, hogy szeretne-e leküldéses bátorítva használatával minősítheti az alkalmazás az áruházban a végfelhasználók értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="83abc-126">A common use-case is that you want to send a push a notification to encourage your end-users to rate your application in the store.</span></span> <span data-ttu-id="83abc-127">Ahelyett, hogy a végfelhasználók értesítést küld, az alkalmazás minden nap használtak az elmúlt hónapban, és rendelkezésére állt-e a kiváló felhasználói felület csak felhasználók kellene megadnia szegmenssel hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="83abc-127">Rather than sending a notification to all your end-users, you can create a segment that would specify only users that have used your application every day for the last month and have had a great user experience.</span></span> <span data-ttu-id="83abc-128">Kevesebb, jól célzott leküldéses értesítéseket küldeni, a jobb Megtérülési nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="83abc-128">When you send fewer, highly targeted push notifications, you get a better ROI.</span></span>

 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a><span data-ttu-id="83abc-130">Szegmensek hozhat létre a fő Azure Mobile Engagement elemet alapján:</span><span class="sxs-lookup"><span data-stu-id="83abc-130">Segments you can create based on the major Azure Mobile Engagement elements:</span></span>
* <span data-ttu-id="83abc-131">Esemény: hozzon létre egy szegmenst, hogy célokat egy adott esemény az alkalmazás, amely hetente legfeljebb kétszer került sor.</span><span class="sxs-lookup"><span data-stu-id="83abc-131">Event: create a segment that targets one specific event of the application that happened more than twice a week.</span></span> 
* <span data-ttu-id="83abc-132">Munkamenet: hozzon létre egy részének a az alkalmazás több mint 5 alkalommal múlt héten használó felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="83abc-132">Session: create a segment of users that have used the application more than 5 times last week.</span></span>
* <span data-ttu-id="83abc-133">Tevékenység: hozzon létre egy részének a használó felhasználókat egy oldal, vagy a tartalom több vagy kevesebb mint 10 ideje elmúlt hónapban.</span><span class="sxs-lookup"><span data-stu-id="83abc-133">Activity: create a segment of users that have used one page or content more or less than 10 time last month.</span></span>
* <span data-ttu-id="83abc-134">Feladat: a szegmens, amely naponta legfeljebb kétszer befejeződött egy feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="83abc-134">Job: create a segment of users that have completed a job more than twice a day.</span></span>
* <span data-ttu-id="83abc-135">Összeomlási: hozzon létre egy szegmens az összes olyan felhasználó, amely 10 szintnél mélyebben múlt héten crash volt.</span><span class="sxs-lookup"><span data-stu-id="83abc-135">Crash: create a segment of all the users that have had a crash more than 10 times last week.</span></span> <span data-ttu-id="83abc-136">(Ebben a szegmensben egy Bocsánatkérő vagy akár egy ráta sikerült leküldéses!)</span><span class="sxs-lookup"><span data-stu-id="83abc-136">(You could push this segment with an apology or even a coupon!)</span></span>
* <span data-ttu-id="83abc-137">Hiba: a szegmens az összes olyan felhasználó, amelyek több mint 100 alkalommal 3 napnál korábban hibába hozható létre.</span><span class="sxs-lookup"><span data-stu-id="83abc-137">Error: create a segment of all the users that have had an error more than 100 times last 3 days.</span></span>
* <span data-ttu-id="83abc-138">Alkalmazásadat: hozzon létre egy szegmenst, amelynek célja egy egyéni App-Info, amelyek az elmúlt 25 napban történtek.</span><span class="sxs-lookup"><span data-stu-id="83abc-138">App Info: create a segment that targets a custom App Info that happened during the last 25 days.</span></span>
  
  ![segments4][38]

<span data-ttu-id="83abc-140">Szegmens optimálisan használatához még kell meg egy testreszabott integrációt az SDK az alkalmazás "Alkalmazásadatok" címkék címkézési tervvel.</span><span class="sxs-lookup"><span data-stu-id="83abc-140">To use Segment in an optimal way, you must have done a customized integration of the SDK in your application with a tagging plan of "App Info" tags.</span></span>
<span data-ttu-id="83abc-141">Ezt követően folytassa a felület kezdőlapján, válassza ki az alkalmazást, majd kattintson a "Szegmensek" szakasz.</span><span class="sxs-lookup"><span data-stu-id="83abc-141">Then, go to the home page of the interface, select the application you want and click on the "Segments" section.</span></span>

1. <span data-ttu-id="83abc-142">Válassza ki a "Szegmensek" szakaszt.</span><span class="sxs-lookup"><span data-stu-id="83abc-142">Select the "Segments" section.</span></span>
2. <span data-ttu-id="83abc-143">Kattintson az "új szegmens" gombra kattintva hozzon létre egy új szegmenst.</span><span class="sxs-lookup"><span data-stu-id="83abc-143">Click on the "New segment" button to create a new segment.</span></span>

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a><span data-ttu-id="83abc-144">Valós élettartama. példa: Hozzon létre egy egyszerű szegmens "Munkamenet" adatok alapján</span><span class="sxs-lookup"><span data-stu-id="83abc-144">Real Life Example: Create a simple segment based on "Session" information</span></span>
<span data-ttu-id="83abc-145">Hozzon létre egy szegmens az összes a végfelhasználók számára használták az alkalmazást legalább 50 alkalommal az elmúlt hét.</span><span class="sxs-lookup"><span data-stu-id="83abc-145">Create a segment of all the end-users that have used your app at least 50 times in the last week.</span></span> <span data-ttu-id="83abc-146">Ott csak a végfelhasználók számára, amelyekkel töltötték legalább 30 másodperces munkamenetenként az alkalmazásban található.</span><span class="sxs-lookup"><span data-stu-id="83abc-146">From there, find only the end-users that have spent at least 30 seconds in your app per session.</span></span> <span data-ttu-id="83abc-147">Ez az alkalmazás egy pozitív élményt rendelkező összes végfelhasználók jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="83abc-147">This will show all the end-users who have a positive experience in your app.</span></span> <span data-ttu-id="83abc-148">Ezt követően létre szegmens is leküldéses értesítést a végfelhasználóknak, amiben megkéri, hogy értékeljék az alkalmazást a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="83abc-148">Then, the segment created could be used to push a notification to these end-users to ask them to rate your app in the store.</span></span>

 ![segments5][39]

1. <span data-ttu-id="83abc-150">Nevezze el a szegmensek ahhoz, hogy gyorsan megtalálhatja a "Szegmens" listában.</span><span class="sxs-lookup"><span data-stu-id="83abc-150">Give your segment a name in order to find it quickly in the "Segment" list.</span></span>
2. <span data-ttu-id="83abc-151">Kattintson a "Létrehozás" gombra.</span><span class="sxs-lookup"><span data-stu-id="83abc-151">Click on the "Create" button.</span></span>
   
   ![segments6][40]

<span data-ttu-id="83abc-153">Jelölje ki a munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="83abc-153">Select Session.</span></span>

 ![segments7][41]

1. <span data-ttu-id="83abc-155">Válassza ki az "Elmúlt hét" időszak.</span><span class="sxs-lookup"><span data-stu-id="83abc-155">Select the period of "Last week".</span></span>
2. <span data-ttu-id="83abc-156">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="83abc-156">Click next.</span></span>
   
   ![segments8][42]
3. <span data-ttu-id="83abc-158">Válassza ki azt az operátort, fontos listájában: =; ≥, ≤.</span><span class="sxs-lookup"><span data-stu-id="83abc-158">Select the Operator that is relevant among the list: =; ≥, ≤.</span></span>
4. <span data-ttu-id="83abc-159">Adja meg a kívánt szám.</span><span class="sxs-lookup"><span data-stu-id="83abc-159">Enter the Count you want.</span></span>
5. <span data-ttu-id="83abc-160">Válassza ki a kívánt előfordulása.</span><span class="sxs-lookup"><span data-stu-id="83abc-160">Select the Occurrence you want.</span></span> 
6. <span data-ttu-id="83abc-161">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="83abc-161">Click next.</span></span>
   <span data-ttu-id="83abc-162">Ebben a példában van beállítva, így a múlt héten keresztül megfelelő felhasználók, amelyek legalább 50 munkamenetek történtek.</span><span class="sxs-lookup"><span data-stu-id="83abc-162">This example is set so that over the last week, match users that have made at least 50 sessions.</span></span>
   
   ![segments9][43]

<span data-ttu-id="83abc-164">A munkamenet Szegmentálás esetén dönthet úgy, a munkamenet feltételként hossza.</span><span class="sxs-lookup"><span data-stu-id="83abc-164">For the Session segmentation, you can choose the length per session as a criteria.</span></span>

1. <span data-ttu-id="83abc-165">Válassza ki a listából az operátor.</span><span class="sxs-lookup"><span data-stu-id="83abc-165">Select the Operator from the list.</span></span>
2. <span data-ttu-id="83abc-166">Adja meg a munkamenet hossza.</span><span class="sxs-lookup"><span data-stu-id="83abc-166">Provide the Length per session.</span></span>
3. <span data-ttu-id="83abc-167">Kattintson a Next (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="83abc-167">Click Next.</span></span>
   <span data-ttu-id="83abc-168">Ebben a példában a felirat látható, hogy a munkamenet, amely rendelkezik a előfordulási szakasz lett szegmentált, jelölje be a felhasználókra, akik rendelkeznek a munkamenetenként több mint 30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="83abc-168">In this example, it says that over all the sessions that have been segmented on the occurrence section, select only the users that have spent more than 30 seconds per session.</span></span>
   
   ![segments10][44]

<span data-ttu-id="83abc-170">A feltétel neve a teljes tölcsér a webkiszolgálótól, és kattintson a Befejezés gombra.</span><span class="sxs-lookup"><span data-stu-id="83abc-170">Name your criterion in order to retrieve it in the complete funnel, and click Finish.</span></span>

 ![segments11][45]

<span data-ttu-id="83abc-172">Miután végzett a beállítás mentése a feltételnek, a szegmens tölcsérhez jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="83abc-172">When you have finished setting up your criterion, it will appear in the segment funnel.</span></span>
<span data-ttu-id="83abc-173">A szegmens analytics adatokon alapul, mert a szegmensek naponta egyszer kiszámítása a történik.</span><span class="sxs-lookup"><span data-stu-id="83abc-173">Because a segment is based on analytics data, segments are computed once per day.</span></span>
<span data-ttu-id="83abc-174">Ebben a példában 47,7 a teljes végfelhasználók % megfelel a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="83abc-174">In this example, 47,7% of the total end-users matched the criterion.</span></span> <span data-ttu-id="83abc-175">A felhasználók rendelkezésére állt-e a megfelelő környezet és várhatóan magasabb minősítést adja meg, ha a értékelje az áruházban az alkalmazás kéri a felhasználót értesítést leküldéses őket kell őket.</span><span class="sxs-lookup"><span data-stu-id="83abc-175">They should be the users who have had a good experience and will be likely to provide a higher rating if you push them a notification asking them to rate the app in the store.</span></span>

## <a name="see-also"></a><span data-ttu-id="83abc-176">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="83abc-176">See also</span></span>
* <span data-ttu-id="83abc-177">[Alapfogalmak][Link 6]</span><span class="sxs-lookup"><span data-stu-id="83abc-177">[Concepts][Link 6]</span></span>
* <span data-ttu-id="83abc-178">[Hibaelhárítási útmutató szolgáltatás][Link 24]</span><span class="sxs-lookup"><span data-stu-id="83abc-178">[Troubleshooting Guide Service][Link 24]</span></span>

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

