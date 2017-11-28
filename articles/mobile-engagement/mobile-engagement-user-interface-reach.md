---
title: "Mobile Engagement felhasználói felület - a Reach aaaAzure"
description: "Megtudhatja, hogyan tooreach toohello felhasználók az alkalmazás az Azure Mobile Engagement leküldéses értesítéseket"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 40d5162ddeccec82c2c9f5b0d72b4cb10c9ddc38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreach-out-toohello-users-of-your-application-with-push-notifications"></a><span data-ttu-id="72b97-103">Hogyan tooreach toohello felhasználók az alkalmazás a leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="72b97-103">How tooreach out toohello users of your application with push notifications</span></span>
<span data-ttu-id="72b97-104">Ez a cikk ismerteti a hello **ELÉRNI** hello lapján **a Mobile Engagement** portálon.</span><span class="sxs-lookup"><span data-stu-id="72b97-104">This article describes hello **REACH** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="72b97-105">Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="72b97-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span> <span data-ttu-id="72b97-106">Vegye figyelembe, hogy először toocreate hello portálon toostart egy **Azure Mobile Engagement** fiók.</span><span class="sxs-lookup"><span data-stu-id="72b97-106">Note that toostart using hello portal you first need toocreate an **Azure Mobile Engagement** account.</span></span> <span data-ttu-id="72b97-107">További információkért lásd: [Azure Mobile Engagement-fiók létrehozása](mobile-engagement-create.md).</span><span class="sxs-lookup"><span data-stu-id="72b97-107">For more information, see [Create an Azure Mobile Engagement account](mobile-engagement-create.md).</span></span>

<span data-ttu-id="72b97-108">hello éri el a felhasználói felület hello szakasza hello leküldéses kampány felügyeleti eszköz itt lehetséges létrehozása/szerkesztése/aktiválása/Befejezés/figyelő, és megtekintheti a statisztikákat a leküldéses értesítési kampányt és hello Reach API (és néhány eleme hello alacsony keresztül is elérhető szolgáltatások szint leküldéses API).</span><span class="sxs-lookup"><span data-stu-id="72b97-108">hello Reach section of hello UI is hello Push campaign management tool where you can create/edit/activate/finish/monitor and get statistics on Push notification campaigns and features that can also be accessed via hello Reach API (and some elements of hello low level Push API).</span></span> <span data-ttu-id="72b97-109">Ne feledje, függetlenül attól, hogy hello API-k vagy hello felhasználói felület, akkor kell toointegrate Azure Mobile Engagement és a Reach az alkalmazásba, az egyes platformokon hello SDK használata előtt a Reach-kampányok.</span><span class="sxs-lookup"><span data-stu-id="72b97-109">Remember that whether you are using hello APIs or hello UI, you will need toointegrate both Azure Mobile Engagement and Reach into your application for each platform with hello SDK before you can use Reach campaigns.</span></span>

> [!NOTE]
> <span data-ttu-id="72b97-110">Hello sok szakasza **a Mobile Engagement** portál felhasználói felületének tartalmazhat hello **megjelenítése SÚGÓ** gombra.</span><span class="sxs-lookup"><span data-stu-id="72b97-110">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="72b97-111">Nyomja meg a gomb tooget szakasz környezetfüggő tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="72b97-111">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="four-types-of-push-notifications"></a><span data-ttu-id="72b97-112">Leküldéses értesítések négy típusa</span><span class="sxs-lookup"><span data-stu-id="72b97-112">Four types of Push notifications</span></span>
1. <span data-ttu-id="72b97-113">Közlemények - toosend hirdetési üzenetek toousers, amely átirányítja őket az alkalmazás vagy a toosend belül tooanother hely lehetővé teszi őket tooa weblap, vagy az alkalmazás kívül tárolja.</span><span class="sxs-lookup"><span data-stu-id="72b97-113">Announcements - allow you toosend advertising messages toousers that redirect them tooanother location inside your app or toosend them tooa webpage or store outside of your app.</span></span> 
2. <span data-ttu-id="72b97-114">Szavazások - lehetővé teszi a végfelhasználók toogather adatait által kérdések kéri a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="72b97-114">Polls - allow you toogather information from end users by asking them questions.</span></span>
3. <span data-ttu-id="72b97-115">Adatleküldések - lehetővé teszi egy bináris vagy base64 adatfájlt toosend.</span><span class="sxs-lookup"><span data-stu-id="72b97-115">Data Pushes - allow you toosend a binary or base64 data file.</span></span> <span data-ttu-id="72b97-116">adatoknak lévő hello információt az alkalmazásban a felhasználók aktuális élmény tooyour alkalmazás toomodify küldött.</span><span class="sxs-lookup"><span data-stu-id="72b97-116">hello information contained in a data push is sent tooyour application toomodify your users' current experience in your app.</span></span> <span data-ttu-id="72b97-117">Az alkalmazás kell toobe képes tooprocess hello adatok található adatokat.</span><span class="sxs-lookup"><span data-stu-id="72b97-117">Your application needs toobe able tooprocess hello data in a data push.</span></span>

## <a name="campaign-details"></a><span data-ttu-id="72b97-118">Kampány adatai</span><span class="sxs-lookup"><span data-stu-id="72b97-118">Campaign Details</span></span>
<span data-ttu-id="72b97-119">Szerkesztése, klónozzon, törlése, és aktiválja a kampányt, amely rendelkezik még nincs aktiválva a nevek fölé vagy tooopen kattintva őket.</span><span class="sxs-lookup"><span data-stu-id="72b97-119">You can edit, clone, delete, or activate campaigns that have not been activated yet by hovering over their names or you can click tooopen them.</span></span> <span data-ttu-id="72b97-120">Már aktiválták a nevek fölé kampányok klónozhat vagy tooopen kattintva őket.</span><span class="sxs-lookup"><span data-stu-id="72b97-120">You can clone campaigns that have already been activated by hovering over their names or you can click tooopen them.</span></span> <span data-ttu-id="72b97-121">Azonban a kampány nem módosítható, miután aktiválva lett.</span><span class="sxs-lookup"><span data-stu-id="72b97-121">However, you can't change a campaign once it has been activated.</span></span>

![Reach1][18]

## <a name="reach-feedback"></a><span data-ttu-id="72b97-123">Visszajelzés elérni</span><span class="sxs-lookup"><span data-stu-id="72b97-123">Reach Feedback</span></span>
<span data-ttu-id="72b97-124">Kattintson a **statisztika** toosee hello részletei egy Reach-kampány.</span><span class="sxs-lookup"><span data-stu-id="72b97-124">Click on **Statistics** toosee hello details of a Reach campaign.</span></span> <span data-ttu-id="72b97-125">Hello **egyszerű** nézet visual reprezentációja arról, mi történt egy kampány aktiválása után oszlop oszlopdiagramon hello formájában.</span><span class="sxs-lookup"><span data-stu-id="72b97-125">hello **Simple** view provides a visual representation in hello form of a column bar graph about what happened after a campaign was activated.</span></span> <span data-ttu-id="72b97-126">Hello **speciális** nézet tartalmazza hello leküldéses kampány részletesebb adatait.</span><span class="sxs-lookup"><span data-stu-id="72b97-126">hello **Advanced** view provides more granular details about hello push campaign.</span></span> <span data-ttu-id="72b97-127">Ezek az adatok nem lesz elérhető, ha a teszt kampány azaz leküldéses elküldött tooa vizsgálati eszköz.</span><span class="sxs-lookup"><span data-stu-id="72b97-127">These details will not be available if you are sending a test campaign i.e. a push sent tooa test device.</span></span> <span data-ttu-id="72b97-128">Itt látható, hogyan kell értelmezni ezeket az adatokat:</span><span class="sxs-lookup"><span data-stu-id="72b97-128">Here is how you should interpret these details:</span></span>

1. <span data-ttu-id="72b97-129">**Leküldött** -azt határozza meg, hogy hello toohello eszközökre leküldött üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="72b97-129">**Pushed** - This specifies hello number of messages pushed toohello devices.</span></span> <span data-ttu-id="72b97-130">Ez a szám hello leküldéses kampány létrehozásához a megadott hello célközönség függ.</span><span class="sxs-lookup"><span data-stu-id="72b97-130">This number will depend on hello target audience you specified while creating hello push campaign.</span></span> <span data-ttu-id="72b97-131">Ha nem adja meg a célközönség, majd a leküldéses elküldi tooall hello regisztrált eszközökre.</span><span class="sxs-lookup"><span data-stu-id="72b97-131">If you do not specify any target audience, then this push will be sent out tooall hello registered devices.</span></span> <span data-ttu-id="72b97-132">Minden más leküldéses szolgáltatások, például a Microsoft nem leküldéses hello értesítések közvetlenül toohello eszközök, de ehelyett küldje le őket toohello adott platform adott leküldéses értesítéseket kezelő szolgáltatása (PNS - APNS/GCM/WNS), hogy azok által biztosított hello értesítések toohello eszközök.</span><span class="sxs-lookup"><span data-stu-id="72b97-132">Like all other push services, we do not push hello notifications directly toohello devices but instead push them toohello respective platform specific Push Notification Services (PNS - APNS/GCM/WNS) so that they can deliver hello notifications toohello devices.</span></span> 
2. <span data-ttu-id="72b97-133">**I** -azt határozza meg hello sikeresen kézbesítette hello PNS toohello eszköz és a nyugtázott üzenetek száma, mint a Mobile Engagement SDK által fogadott.</span><span class="sxs-lookup"><span data-stu-id="72b97-133">**Delivered** - This specifies hello number of messages which are successfully delivered by hello PNS toohello device and acknowledged as received by Mobile Engagement SDK.</span></span> 
   
   <span data-ttu-id="72b97-134">*A kézbesített okait száma kevesebb, mint a megnyomott száma:*</span><span class="sxs-lookup"><span data-stu-id="72b97-134">*Reasons for Delivered count being less than Pushed count:*</span></span>
   
   1. <span data-ttu-id="72b97-135">Ha hello felhasználó eltávolította hello app hello eszközről, de hello PNS nem ismeri az hello leküldéses toohello PNS küldünk hello időpontban üdvözlőüzenetére a program eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="72b97-135">If hello user has uninstalled hello app from hello device but hello PNS doesn't know about it at hello time we send hello push toohello PNS then hello message will be dropped.</span></span>
   2. <span data-ttu-id="72b97-136">Ha hello eszközzel hello alkalmazás van, de a hello eszközöktől hosszabb ideig offline állapotban volt, majd hello PNS nem toodeliver hello üzenet toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="72b97-136">If hello device has hello app but hello devices themselves were offline for long periods of time, then hello PNS will fail toodeliver hello message toohello device.</span></span> 
   3. <span data-ttu-id="72b97-137">Ha üdvözlőüzenetére toohello eszköz kézbesítve, de hello Mobile Engagement SDK hello alkalmazásban nem ismeri fel a üdvözlőüzenetére hello tartalmát, majd esik az üzenet.</span><span class="sxs-lookup"><span data-stu-id="72b97-137">If hello message does get delivered toohello device but hello Mobile Engagement SDK in hello app doesn’t recognize hello content of hello message, then it drops that message.</span></span> <span data-ttu-id="72b97-138">Ez akkor fordulhat elő, ha hello testreszabási hello értesítés hello App SDK és az vetett hello üdvözlőüzenetére kivételt, amely azt a catch hoz létre.</span><span class="sxs-lookup"><span data-stu-id="72b97-138">This could happen if hello customization of hello notification in hello app generates an exception which we catch in hello SDK and drop hello message.</span></span> <span data-ttu-id="72b97-139">Ez akkor is előfordulhat, ha hello alkalmazást hello eszközön hello platform által küldött hello Mobile Engagement SDK, amely nem tud toounderstand hello újabb verziójával leküldéses üdvözlőüzenetére verzióját használja, de ez csak akkor, ha hello app frissítették hello értesítés után hello service platformról elküldése.</span><span class="sxs-lookup"><span data-stu-id="72b97-139">This could also occur if hello app on hello device is using a version of hello Mobile Engagement SDK which is not able toounderstand hello newer version of hello push message sent from hello platform but this is only when hello app was upgraded after hello notification was dispatched from hello service platform.</span></span> <span data-ttu-id="72b97-140">Hello **speciális** lapon megtudhatja, hogy hány üzenetek el lettek dobva.</span><span class="sxs-lookup"><span data-stu-id="72b97-140">hello **Advanced** tab will tell how many messages were dropped.</span></span> 
   4. <span data-ttu-id="72b97-141">Az iOS-eszközökön üzenetek néha nem kézbesítve az alacsony töltöttségű telepre vonatkozó egyik hello eszköz esetén, vagy ha hello app van fel power jelentős mennyiségű, távoli értesítések feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="72b97-141">On iOS devices, messages sometimes do not get delivered if either hello device is on low battery or if hello app is consuming significant amount of power when processing remote notifications.</span></span> <span data-ttu-id="72b97-142">Ez az iOS-eszközök hello korlátozása.</span><span class="sxs-lookup"><span data-stu-id="72b97-142">This is a limitation of hello iOS devices.</span></span>   
3. <span data-ttu-id="72b97-143">**Megjelenített** -azt határozza meg, hogy sikeresen látható toohello alkalmazás felhasználói hello értesítési központ leküldéses/out-az-app rendszerértesítőként vagy egy alkalmazásbeli értesítés belül hello hello űrlapján hello eszközön mobil üzenetek hello száma alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="72b97-143">**Displayed** - This specifies hello number of messages which are successfully shown toohello app user on hello device in hello form of a system push/out-of-app notification in hello notification center or an in-app notification within hello mobile app.</span></span>  <span data-ttu-id="72b97-144">Hello **speciális** lapon megtudhatja, hány volt rendszer értesítéseket és alkalmazáson belüli értesítések.</span><span class="sxs-lookup"><span data-stu-id="72b97-144">hello **Advanced** tab will tell you how many were system notifications and how many were in-app notifications.</span></span> 
   
   <span data-ttu-id="72b97-145">*A megjelenő okait száma kevesebb, mint a kézbesített száma (várakozási toobe jelenik meg)*</span><span class="sxs-lookup"><span data-stu-id="72b97-145">*Reasons for Displayed count being less than Delivered count (waiting toobe displayed)*</span></span>
   
   1. <span data-ttu-id="72b97-146">Hello értesítési kampány befejező dátumát volna meg, akkor lehetséges, hogy hello értesítés lett kézbesítve, de amikor hello idő léptek tooopen, és megjeleníti azt toohello alkalmazás felhasználói, ha azt már lejárt, a rendszer soha nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="72b97-146">If hello notification campaign had an end date on it then it is possible that hello notification was delivered but when hello time came tooopen and display it toohello app user, it was already expired so it was never displayed.</span></span>   
   2. <span data-ttu-id="72b97-147">Ha hello értesítést az alkalmazáson belüli értesítést majd hello értesítési csak jelenik meg hello alkalmazás felhasználói hello alkalmazás megnyitása.</span><span class="sxs-lookup"><span data-stu-id="72b97-147">If hello notification is an in-app notification then hello notification is only displayed when hello app user opens hello app.</span></span> <span data-ttu-id="72b97-148">Azokban az esetekben, ahol hello alkalmazás felhasználói nem nyitotta meg hello app hello SDK jelentést, hogy hello értesítési kézbesíteni, de még nem jelenik meg hello alkalmazás már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="72b97-148">In cases where hello app user hasn't opened hello app, hello SDK will report that hello notification was delivered but not yet displayed until hello app is opened.</span></span> 
   3. <span data-ttu-id="72b97-149">Ha hello értesítést az alkalmazáson belüli értesítést, és akkor is hello értesítési lesz jelentve szállított egy adott tevékenység/képernyőn megjelenő toobe konfigurálva, de amíg kézbesíteni még nem hello felhasználó megnyit egy adott képernyő hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="72b97-149">If hello notification is an in-app notification and configured toobe shown on a specific activity/screen then also hello notification will be reported as delivered but not yet delivered until hello user opens hello app on a specific screen.</span></span> 
4. <span data-ttu-id="72b97-150">**Felhasználói kapcsolati** -azt határozza meg, hogy mely hello alkalmazás felhasználói rendelkezik kezelni és hello üzeneteket, amelyek műveletet kiváltó, illetve amelyekből kiléptek üzenetek hello száma.</span><span class="sxs-lookup"><span data-stu-id="72b97-150">**User Interactions** - This specifies hello number of messages which hello app user has interacted with and will include hello messages which are either actioned or exited.</span></span> 
   
   * <span data-ttu-id="72b97-151">*hello alkalmazás felhasználói a következő módokon reagálhat értesítést hello a következő módszerek valamelyikével:*</span><span class="sxs-lookup"><span data-stu-id="72b97-151">*hello app user can action a notification in either of hello following ways:*</span></span>
     
     1. <span data-ttu-id="72b97-152">Ha hello értesítési rendszer/out-az-app értesítést vagy csak értesítést küld egy alkalmazásbeli értesítés akkor hello app felhasználó hello értesítési kattint.</span><span class="sxs-lookup"><span data-stu-id="72b97-152">If hello notification is a system/out-of-app notification or an in-app notification sent as notification-only then hello app user clicks on hello notification.</span></span>
     2. <span data-ttu-id="72b97-153">Ha hello értesítést az alkalmazáson belüli értesítést a szöveges vagy webes-nézet vagy szavazások majd hello alkalmazás felhasználói kattint hello hello értesítési művelet gombjára.</span><span class="sxs-lookup"><span data-stu-id="72b97-153">If hello notification is an in-app notification with a text or web-view or polls then hello app user clicks on hello Action button in hello notification.</span></span>
     3. <span data-ttu-id="72b97-154">Ha hello értesítés az alkalmazáson belüli értesítések, a webes nézet majd hello app felhasználó kattint hello webes nézetben [Android csak] egy URL-címen</span><span class="sxs-lookup"><span data-stu-id="72b97-154">If hello notification is an in-app notification with a web-view then hello app user clicks on a URL in hello web view [Android Only]</span></span>
   * <span data-ttu-id="72b97-155">*hello alkalmazás felhasználói a következő módokon léphet a következő módokon hello valamelyikével értesítést:*</span><span class="sxs-lookup"><span data-stu-id="72b97-155">*hello app user can exit a notification in either of hello following ways:*</span></span>
     
     1. <span data-ttu-id="72b97-156">Közvetlenül kattintva hello hello értesítési Bezárás gombjára.</span><span class="sxs-lookup"><span data-stu-id="72b97-156">Clicking hello close button on hello notification directly.</span></span> 
     2. <span data-ttu-id="72b97-157">Lehúzni számítógépnél, vagy törölje a hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="72b97-157">Swiping away or deleting hello notification.</span></span> 
     3. <span data-ttu-id="72b97-158">Alkalmazáson belüli értesítések szöveges vagy webes tartalom és szavazások esetén egy kétlépéses folyamat során általában megjelenített toohello alkalmazás felhasználói.</span><span class="sxs-lookup"><span data-stu-id="72b97-158">In-app notifications with text/web content and polls are typically displayed toohello app user in a two-step process.</span></span> <span data-ttu-id="72b97-159">Akkor jelenik meg értesítés először, és azok kattintson rá, ha azok hello későbbi a lekérdezési/szöveges/webes tartalom megtekintését.</span><span class="sxs-lookup"><span data-stu-id="72b97-159">They see a notification first and when they click on it, they see hello subsequent text/web/poll content.</span></span> <span data-ttu-id="72b97-160">hello alkalmazás felhasználói a következő módokon léphet egy értesítést az alábbi lépéseket, és hello részletek hello speciális nézetben ez rögzíti.</span><span class="sxs-lookup"><span data-stu-id="72b97-160">hello app user can exit a notification in either of these steps and hello details in hello Advanced view captures this.</span></span> 
5. <span data-ttu-id="72b97-161">**Műveletet kiváltó** -azt határozza meg, hogy hello volt hello app felhasználó explicit módon műveletet kiváltó üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="72b97-161">**Actioned** - This specifies hello number of messages which were explicitly actioned by hello app user.</span></span> <span data-ttu-id="72b97-162">Ez az hello a legérdekesebb számát, ez alapján alkalmazást hány felhasználó által üdvözlőüzenetére hello értesítés leküldött volt interested.</span><span class="sxs-lookup"><span data-stu-id="72b97-162">This is hello most interesting number as this tells how many app users were interested by hello message you pushed out in hello notification.</span></span> 

> [!NOTE]
> <span data-ttu-id="72b97-163">Az iOS és Windows platformra, ha hello felhasználó hello alkalmazást nyitott és hello kampány nem "Bármikor" kampány akkor lehetséges, hogy mindkét kívüli alkalmazás és az alkalmazáson belüli értesítések megjelenítése hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="72b97-163">On iOS & Windows platforms, if hello user has hello app open and hello campaign was an "AnyTime" campaign then it is possible that both out of app and in-app notifications are displayed at hello same time.</span></span> <span data-ttu-id="72b97-164">Emiatt a magasabb, mint a kézbesített hello megjelenített számát.</span><span class="sxs-lookup"><span data-stu-id="72b97-164">This may cause a Displayed count higher than hello Delivered.</span></span> <span data-ttu-id="72b97-165">Ha hello felhasználói együttműködő, vagy a műveletek hello értesítés, még akkor is, majd hello felhasználói interakció/Actioned száma nagyobb, mint a kézbesített lehet.</span><span class="sxs-lookup"><span data-stu-id="72b97-165">If hello user interacts or actions hello notification, then even hello User Interactions/Actioned count could be greater than Delivered.</span></span> 
> 
> 

![Reach2][19]

## <a name="see-also"></a><span data-ttu-id="72b97-167">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="72b97-167">See also</span></span>
* <span data-ttu-id="72b97-168">[Alapfogalmak][Link 6]</span><span class="sxs-lookup"><span data-stu-id="72b97-168">[Concepts][Link 6]</span></span>
* <span data-ttu-id="72b97-169">[Hibaelhárítási útmutató szolgáltatás][Link 24]</span><span class="sxs-lookup"><span data-stu-id="72b97-169">[Troubleshooting Guide Service][Link 24]</span></span>

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

