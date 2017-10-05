---
title: "Az Azure Mobile Engagement felhasználói felület - a Reach"
description: "Megtudhatja, hogyan érheti el az alkalmazás a felhasználók az Azure Mobile Engagement leküldéses értesítéseket"
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
ms.openlocfilehash: ce30456e41ff1a2f4824bcb64246ee115fdd1ef7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a><span data-ttu-id="45de1-103">Hogyan érheti el a felhasználókkal leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="45de1-103">How to reach out to the users of your application with push notifications</span></span>
<span data-ttu-id="45de1-104">Ez a cikk ismerteti a **ELÉRNI** lapján a **a Mobile Engagement** portálon.</span><span class="sxs-lookup"><span data-stu-id="45de1-104">This article describes the **REACH** tab of the **Mobile Engagement** portal.</span></span> <span data-ttu-id="45de1-105">Használja a **a Mobile Engagement** portal felügyeletét és kezelését a mobile apps szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="45de1-105">You use the **Mobile Engagement** portal to monitor and manage your mobile apps.</span></span> <span data-ttu-id="45de1-106">Vegye figyelembe, hogy a portál használatának megkezdéséhez először hozzon létre egy **Azure Mobile Engagement** fiók.</span><span class="sxs-lookup"><span data-stu-id="45de1-106">Note that to start using the portal you first need to create an **Azure Mobile Engagement** account.</span></span> <span data-ttu-id="45de1-107">További információkért lásd: [Azure Mobile Engagement-fiók létrehozása](mobile-engagement-create.md).</span><span class="sxs-lookup"><span data-stu-id="45de1-107">For more information, see [Create an Azure Mobile Engagement account](mobile-engagement-create.md).</span></span>

<span data-ttu-id="45de1-108">A felhasználói felület Reach szakasza a leküldéses kampány felügyeleti eszköz, ahol létrehozása/szerkesztése/aktiválása/Befejezés/figyelő, a következőket teheti, és megtekintheti a statisztikákat a leküldéses értesítési kampányt, és a Reach API (és néhány eleme az alacsony szintű Push API) keresztül is elérhető szolgáltatások .</span><span class="sxs-lookup"><span data-stu-id="45de1-108">The Reach section of the UI is the Push campaign management tool where you can create/edit/activate/finish/monitor and get statistics on Push notification campaigns and features that can also be accessed via the Reach API (and some elements of the low level Push API).</span></span> <span data-ttu-id="45de1-109">Ne feledje, hogy függetlenül attól, hogy az API-k vagy a felhasználói felület, szüksége lesz integrálni Azure Mobile Engagement és a használata előtt az SDK-val az alkalmazásba, az egyes platformokon Reach elérését a kampányok.</span><span class="sxs-lookup"><span data-stu-id="45de1-109">Remember that whether you are using the APIs or the UI, you will need to integrate both Azure Mobile Engagement and Reach into your application for each platform with the SDK before you can use Reach campaigns.</span></span>

> [!NOTE]
> <span data-ttu-id="45de1-110">Sok szakasza a **a Mobile Engagement** portál felhasználói felületének tartalmaz a **megjelenítése SÚGÓ** gombra.</span><span class="sxs-lookup"><span data-stu-id="45de1-110">Many sections of the **Mobile Engagement** portal UI contain the **SHOW HELP** button.</span></span> <span data-ttu-id="45de1-111">Nyomja le az erre a gombra kattintva szakasz környezetfüggő ismertetése.</span><span class="sxs-lookup"><span data-stu-id="45de1-111">Press this button to get more contextual information about a section.</span></span>
> 
> 

## <a name="four-types-of-push-notifications"></a><span data-ttu-id="45de1-112">Leküldéses értesítések négy típusa</span><span class="sxs-lookup"><span data-stu-id="45de1-112">Four types of Push notifications</span></span>
1. <span data-ttu-id="45de1-113">Közlemények - lehetővé teszi, hirdetések küldéséhez felhasználók számára, amelyek átirányítja őket egy másik hely belül az alkalmazás vagy egy weblapot, vagy kívül az app store el őket.</span><span class="sxs-lookup"><span data-stu-id="45de1-113">Announcements - allow you to send advertising messages to users that redirect them to another location inside your app or to send them to a webpage or store outside of your app.</span></span> 
2. <span data-ttu-id="45de1-114">Szavazások - lehetővé teszi információ gyűjtését a végfelhasználók által kérdések kéri a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="45de1-114">Polls - allow you to gather information from end users by asking them questions.</span></span>
3. <span data-ttu-id="45de1-115">Adatleküldések - teszik bináris vagy base64 adatok fájl küldése.</span><span class="sxs-lookup"><span data-stu-id="45de1-115">Data Pushes - allow you to send a binary or base64 data file.</span></span> <span data-ttu-id="45de1-116">Található adatokat küld az alkalmazás aktuális élmény a felhasználók az alkalmazás módosításához.</span><span class="sxs-lookup"><span data-stu-id="45de1-116">The information contained in a data push is sent to your application to modify your users' current experience in your app.</span></span> <span data-ttu-id="45de1-117">Az alkalmazás kell tudni feldolgozni az adatokat található adatokat.</span><span class="sxs-lookup"><span data-stu-id="45de1-117">Your application needs to be able to process the data in a data push.</span></span>

## <a name="campaign-details"></a><span data-ttu-id="45de1-118">Kampány adatai</span><span class="sxs-lookup"><span data-stu-id="45de1-118">Campaign Details</span></span>
<span data-ttu-id="45de1-119">Szerkesztése, klónozzon, törlése, és aktiválja a kampányt, amely rendelkezik még nincs aktiválva a nevek fölé vagy kattintva nyissa meg ezeket.</span><span class="sxs-lookup"><span data-stu-id="45de1-119">You can edit, clone, delete, or activate campaigns that have not been activated yet by hovering over their names or you can click to open them.</span></span> <span data-ttu-id="45de1-120">Már aktiválták a nevek fölé kampányok klónozhat, vagy a megnyitásukhoz gombra.</span><span class="sxs-lookup"><span data-stu-id="45de1-120">You can clone campaigns that have already been activated by hovering over their names or you can click to open them.</span></span> <span data-ttu-id="45de1-121">Azonban a kampány nem módosítható, miután aktiválva lett.</span><span class="sxs-lookup"><span data-stu-id="45de1-121">However, you can't change a campaign once it has been activated.</span></span>

![Reach1][18]

## <a name="reach-feedback"></a><span data-ttu-id="45de1-123">Visszajelzés elérni</span><span class="sxs-lookup"><span data-stu-id="45de1-123">Reach Feedback</span></span>
<span data-ttu-id="45de1-124">Kattintson a **statisztika** a Reach-kampány a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="45de1-124">Click on **Statistics** to see the details of a Reach campaign.</span></span> <span data-ttu-id="45de1-125">A **egyszerű** nézet nyújt arról, mi történt egy kampány aktiválása után oszlop oszlopdiagramon formájában vizuális megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="45de1-125">The **Simple** view provides a visual representation in the form of a column bar graph about what happened after a campaign was activated.</span></span> <span data-ttu-id="45de1-126">A **speciális** nézet tartalmazza a leküldéses kampány részletesebb adatait.</span><span class="sxs-lookup"><span data-stu-id="45de1-126">The **Advanced** view provides more granular details about the push campaign.</span></span> <span data-ttu-id="45de1-127">Ezek az adatok nem lesz elérhető, ha egy teszt kampány azaz küld egy vizsgálati eszköz leküldéses.</span><span class="sxs-lookup"><span data-stu-id="45de1-127">These details will not be available if you are sending a test campaign i.e. a push sent to a test device.</span></span> <span data-ttu-id="45de1-128">Itt látható, hogyan kell értelmezni ezeket az adatokat:</span><span class="sxs-lookup"><span data-stu-id="45de1-128">Here is how you should interpret these details:</span></span>

1. <span data-ttu-id="45de1-129">**Leküldött** -azt határozza meg az eszközökre leküldött üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="45de1-129">**Pushed** - This specifies the number of messages pushed to the devices.</span></span> <span data-ttu-id="45de1-130">Ez a szám a leküldéses kampány létrehozásához a megadott célközönség függ.</span><span class="sxs-lookup"><span data-stu-id="45de1-130">This number will depend on the target audience you specified while creating the push campaign.</span></span> <span data-ttu-id="45de1-131">Ha nem adja meg a célközönség, majd a leküldéses fog el lehet küldeni a regisztrált eszközöket.</span><span class="sxs-lookup"><span data-stu-id="45de1-131">If you do not specify any target audience, then this push will be sent out to all the registered devices.</span></span> <span data-ttu-id="45de1-132">Minden más leküldéses szolgáltatások, például a Microsoft nem leküldéses értesítéseket közvetlenül az eszközök számára, de ehelyett azok leküldése az adott platform adott leküldéses értesítéseket kezelő szolgáltatása (PNS - APNS/GCM/WNS), hogy ezek az értesítések által biztosított eszközök.</span><span class="sxs-lookup"><span data-stu-id="45de1-132">Like all other push services, we do not push the notifications directly to the devices but instead push them to the respective platform specific Push Notification Services (PNS - APNS/GCM/WNS) so that they can deliver the notifications to the devices.</span></span> 
2. <span data-ttu-id="45de1-133">**I** -azt határozza meg sikeresen érhetők el a pns-sel, hogy az eszköz és a nyugtázott üzenetek száma, mint a Mobile Engagement SDK által fogadott.</span><span class="sxs-lookup"><span data-stu-id="45de1-133">**Delivered** - This specifies the number of messages which are successfully delivered by the PNS to the device and acknowledged as received by Mobile Engagement SDK.</span></span> 
   
   <span data-ttu-id="45de1-134">*A kézbesített okait száma kevesebb, mint a megnyomott száma:*</span><span class="sxs-lookup"><span data-stu-id="45de1-134">*Reasons for Delivered count being less than Pushed count:*</span></span>
   
   1. <span data-ttu-id="45de1-135">Ha a felhasználó rendelkezik eltávolítja az alkalmazást az eszközről, de a pns-sel kapcsolatos nem ismeri a leküldéses nem küldeni a pns-sel időpontjában az üzenetet a program eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="45de1-135">If the user has uninstalled the app from the device but the PNS doesn't know about it at the time we send the push to the PNS then the message will be dropped.</span></span>
   2. <span data-ttu-id="45de1-136">Ha az eszköz regisztrációját az alkalmazást, de a eszközöktől hosszabb ideig offline állapotban volt, a pns-sel meghiúsul kézbesíteni az üzenetet az eszközt.</span><span class="sxs-lookup"><span data-stu-id="45de1-136">If the device has the app but the devices themselves were offline for long periods of time, then the PNS will fail to deliver the message to the device.</span></span> 
   3. <span data-ttu-id="45de1-137">Ha az üzenet beolvasása juttatni az eszközre, de az alkalmazásban a Mobile Engagement SDK nem ismeri fel az üzenet tartalmát, majd esik az üzenet.</span><span class="sxs-lookup"><span data-stu-id="45de1-137">If the message does get delivered to the device but the Mobile Engagement SDK in the app doesn’t recognize the content of the message, then it drops that message.</span></span> <span data-ttu-id="45de1-138">Ez akkor fordulhat elő, ha az alkalmazásban az értesítés testreszabási hoz létre egy kivételt, amely azt a catch az SDK-t, és dobja el az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="45de1-138">This could happen if the customization of the notification in the app generates an exception which we catch in the SDK and drop the message.</span></span> <span data-ttu-id="45de1-139">Ez akkor is előfordulhat, ha az alkalmazást az eszközön a Mobile Engagement SDK, amely nem érti a platformnak a leküldéses üzenet újabb verziójában a verzióját használja, de ez csak akkor, ha az alkalmazás a t az értesítés elküldése után frissítették. He service platform.</span><span class="sxs-lookup"><span data-stu-id="45de1-139">This could also occur if the app on the device is using a version of the Mobile Engagement SDK which is not able to understand the newer version of the push message sent from the platform but this is only when the app was upgraded after the notification was dispatched from the service platform.</span></span> <span data-ttu-id="45de1-140">A **speciális** lapon megtudhatja, hogy hány üzenetek el lettek dobva.</span><span class="sxs-lookup"><span data-stu-id="45de1-140">The **Advanced** tab will tell how many messages were dropped.</span></span> 
   4. <span data-ttu-id="45de1-141">Az iOS-eszközökön üzenetek néha nem kézbesítve vagy az eszköz az alacsony töltöttségű telepre vonatkozó vagy ha az alkalmazás nem használ jelentős részét, távoli értesítések feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="45de1-141">On iOS devices, messages sometimes do not get delivered if either the device is on low battery or if the app is consuming significant amount of power when processing remote notifications.</span></span> <span data-ttu-id="45de1-142">Ez az iOS-eszközök korlátozása.</span><span class="sxs-lookup"><span data-stu-id="45de1-142">This is a limitation of the iOS devices.</span></span>   
3. <span data-ttu-id="45de1-143">**Megjelenített** -azt határozza meg, amelyek sikeresen jelennek meg az értesítési központba leküldéses/out-az-app rendszerértesítőként vagy belül a mobilalkalmazás az alkalmazásbeli értesítések formájában az eszközön az alkalmazás felhasználói üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="45de1-143">**Displayed** - This specifies the number of messages which are successfully shown to the app user on the device in the form of a system push/out-of-app notification in the notification center or an in-app notification within the mobile app.</span></span>  <span data-ttu-id="45de1-144">A **speciális** lapon megtudhatja, hány volt rendszer értesítéseket és alkalmazáson belüli értesítések.</span><span class="sxs-lookup"><span data-stu-id="45de1-144">The **Advanced** tab will tell you how many were system notifications and how many were in-app notifications.</span></span> 
   
   <span data-ttu-id="45de1-145">*A megjelenő okait száma kevesebb, mint a kézbesített száma (megjelenítendő várakozási)*</span><span class="sxs-lookup"><span data-stu-id="45de1-145">*Reasons for Displayed count being less than Delivered count (waiting to be displayed)*</span></span>
   
   1. <span data-ttu-id="45de1-146">Az értesítési kampány záródátumot volt, majd esetén előfordulhat, hogy az értesítési lett kézbesítve, de a idő kapott megnyitásához, és megjeleníti azt az alkalmazás felhasználó számára, amikor azt már elévült, a rendszer soha nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="45de1-146">If the notification campaign had an end date on it then it is possible that the notification was delivered but when the time came to open and display it to the app user, it was already expired so it was never displayed.</span></span>   
   2. <span data-ttu-id="45de1-147">Esetén az értesítés az alkalmazáson belüli értesítések a notification csak akkor látható, ha az alkalmazás felhasználó megnyitja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45de1-147">If the notification is an in-app notification then the notification is only displayed when the app user opens the app.</span></span> <span data-ttu-id="45de1-148">Azokban az esetekben, ahol az alkalmazás felhasználói nem nyitotta meg az alkalmazást az SDK jelentést, hogy az értesítés-i, de még nem jelenik meg az alkalmazás már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="45de1-148">In cases where the app user hasn't opened the app, the SDK will report that the notification was delivered but not yet displayed until the app is opened.</span></span> 
   3. <span data-ttu-id="45de1-149">Ha az értesítést az alkalmazáson belüli értesítést, és akkor is megjelenik az értesítés, szállított, de még nem kézbesíteni, amikor a felhasználó egy adott tevékenység képernyőn megjelenítendő konfigurált megnyitja az alkalmazás az adott képernyőn.</span><span class="sxs-lookup"><span data-stu-id="45de1-149">If the notification is an in-app notification and configured to be shown on a specific activity/screen then also the notification will be reported as delivered but not yet delivered until the user opens the app on a specific screen.</span></span> 
4. <span data-ttu-id="45de1-150">**Felhasználói kapcsolati** -azt határozza meg, amely az alkalmazás felhasználói kezelni rendelkezik üzenetek száma és az üzeneteket, amelyek műveletet kiváltó, illetve amelyekből kiléptek.</span><span class="sxs-lookup"><span data-stu-id="45de1-150">**User Interactions** - This specifies the number of messages which the app user has interacted with and will include the messages which are either actioned or exited.</span></span> 
   
   * <span data-ttu-id="45de1-151">*Az alkalmazás felhasználói a következő módokon reagálhat egy értesítést az alábbi módon:*</span><span class="sxs-lookup"><span data-stu-id="45de1-151">*The app user can action a notification in either of the following ways:*</span></span>
     
     1. <span data-ttu-id="45de1-152">Ha az értesítési rendszer/out-az-app értesítést vagy egy csak értesítést küldött, majd az alkalmazás felhasználó kattint az értesítés az alkalmazáson belüli értesítés.</span><span class="sxs-lookup"><span data-stu-id="45de1-152">If the notification is a system/out-of-app notification or an in-app notification sent as notification-only then the app user clicks on the notification.</span></span>
     2. <span data-ttu-id="45de1-153">Ha az értesítést az alkalmazáson belüli értesítést a szöveges vagy webes-nézetet, vagy majd lekérdezi az alkalmazás felhasználó kattint az akciógombra kattinthat, az értesítés.</span><span class="sxs-lookup"><span data-stu-id="45de1-153">If the notification is an in-app notification with a text or web-view or polls then the app user clicks on the Action button in the notification.</span></span>
     3. <span data-ttu-id="45de1-154">Ha az értesítést az alkalmazáson belüli értesítést a web-nézetet majd az alkalmazás felhasználó a hivatkozásra kattint a webes nézet [Android csak] egy URL-cím</span><span class="sxs-lookup"><span data-stu-id="45de1-154">If the notification is an in-app notification with a web-view then the app user clicks on a URL in the web view [Android Only]</span></span>
   * <span data-ttu-id="45de1-155">*Az alkalmazás felhasználói a következő módokon léphet egy értesítést az alábbi módon:*</span><span class="sxs-lookup"><span data-stu-id="45de1-155">*The app user can exit a notification in either of the following ways:*</span></span>
     
     1. <span data-ttu-id="45de1-156">Közvetlenül az értesítés Bezárás gombjára kattintva.</span><span class="sxs-lookup"><span data-stu-id="45de1-156">Clicking the close button on the notification directly.</span></span> 
     2. <span data-ttu-id="45de1-157">Lehúzni számítógépnél, vagy törölje az értesítést.</span><span class="sxs-lookup"><span data-stu-id="45de1-157">Swiping away or deleting the notification.</span></span> 
     3. <span data-ttu-id="45de1-158">Alkalmazáson belüli értesítések szöveges vagy webes tartalom és szavazások esetén általában jelennek meg az alkalmazás felhasználói egy kétlépéses folyamat.</span><span class="sxs-lookup"><span data-stu-id="45de1-158">In-app notifications with text/web content and polls are typically displayed to the app user in a two-step process.</span></span> <span data-ttu-id="45de1-159">Akkor jelenik meg értesítés először, és ha azok kattintson rá, akkor jelenik meg a következő szöveges/webes/lekérdezési tartalmat.</span><span class="sxs-lookup"><span data-stu-id="45de1-159">They see a notification first and when they click on it, they see the subsequent text/web/poll content.</span></span> <span data-ttu-id="45de1-160">Az alkalmazás felhasználói a következő módokon léphet egy értesítést az alábbi lépéseket, és a Speciális nézetben részleteit ez rögzíti.</span><span class="sxs-lookup"><span data-stu-id="45de1-160">The app user can exit a notification in either of these steps and the details in the Advanced view captures this.</span></span> 
5. <span data-ttu-id="45de1-161">**Műveletet kiváltó** -azt határozza meg, amely az alkalmazás felhasználó explicit módon műveletet kiváltó üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="45de1-161">**Actioned** - This specifies the number of messages which were explicitly actioned by the app user.</span></span> <span data-ttu-id="45de1-162">Ez az a a legérdekesebb, ez alapján hány alkalmazás felhasználók által az értesítési leküldött üzenet volt interested.</span><span class="sxs-lookup"><span data-stu-id="45de1-162">This is the most interesting number as this tells how many app users were interested by the message you pushed out in the notification.</span></span> 

> [!NOTE]
> <span data-ttu-id="45de1-163">IOS & Windows nyissa meg a platformok, ha a felhasználó rendelkezik-e az alkalmazás és a kampány lett egy "Bármikor" kampány, akkor lehetséges, hogy mindkét kívül és alkalmazáson belüli értesítések jelennek meg egyszerre.</span><span class="sxs-lookup"><span data-stu-id="45de1-163">On iOS & Windows platforms, if the user has the app open and the campaign was an "AnyTime" campaign then it is possible that both out of app and in-app notifications are displayed at the same time.</span></span> <span data-ttu-id="45de1-164">Emiatt a magasabb, mint a kézbesített megjelenített számát.</span><span class="sxs-lookup"><span data-stu-id="45de1-164">This may cause a Displayed count higher than the Delivered.</span></span> <span data-ttu-id="45de1-165">Ha a felhasználó kommunikál esetleg műveletek az értesítés, akkor még a felhasználó kapcsolati/Actioned száma nagyobb, mint a kézbesített.</span><span class="sxs-lookup"><span data-stu-id="45de1-165">If the user interacts or actions the notification, then even the User Interactions/Actioned count could be greater than Delivered.</span></span> 
> 
> 

![Reach2][19]

## <a name="see-also"></a><span data-ttu-id="45de1-167">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="45de1-167">See also</span></span>
* <span data-ttu-id="45de1-168">[Alapfogalmak][Link 6]</span><span class="sxs-lookup"><span data-stu-id="45de1-168">[Concepts][Link 6]</span></span>
* <span data-ttu-id="45de1-169">[Hibaelhárítási útmutató szolgáltatás][Link 24]</span><span class="sxs-lookup"><span data-stu-id="45de1-169">[Troubleshooting Guide Service][Link 24]</span></span>

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

