---
title: "aaaAzure Mobile Engagement felhasználói felület – saját fiók"
description: "Megtudhatja, hogyan toomanage az Azure Mobile Engagement segítségével fiók profil és a vizsgálati eszközök"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 22832678-3959-4b8c-9fb2-f2ff5974e5d1
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1d85f0e87c43605f59f6536ae42a7fb6a99ee36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-your-account-profile-and-test-devices"></a><span data-ttu-id="5b3be-103">Hogyan toomanage a fiók profil és a vizsgálati eszközök</span><span class="sxs-lookup"><span data-stu-id="5b3be-103">How toomanage your account profile and test devices</span></span>
<span data-ttu-id="5b3be-104">Ez a cikk ismerteti a hello **Home** hello oldalán **a Mobile Engagement** portálon.</span><span class="sxs-lookup"><span data-stu-id="5b3be-104">This article describes hello **Home** page of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="5b3be-105">Hello használata **a Mobile Engagement** portál toomonitor és a mobilalkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="5b3be-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span> 

<span data-ttu-id="5b3be-106">tooget toohello **fiókomat** fiókja a hello hello lap tetején kattintson.</span><span class="sxs-lookup"><span data-stu-id="5b3be-106">tooget toohello **my account** page, click on your account on hello top of hello page.</span></span>

<span data-ttu-id="5b3be-107">Saját fiók szakasza a felhasználói felület, ahol megtekintheti és a fiókjához, beleértve a profilbeállításokat hello-beállítások módosításához és teszteléséhez eszközazonosítókat hello hello.</span><span class="sxs-lookup"><span data-stu-id="5b3be-107">hello My Account section of hello UI is where you can view and change hello settings associated with your account, including your Profile settings and test Device IDs.</span></span> <span data-ttu-id="5b3be-108">Ezek a beállítások hello Device API-val keresztül is elérhető elemeket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5b3be-108">These settings contain items that can also be accessed via hello Device API.</span></span>

![MyAccount1][7]  

## <a name="profile"></a><span data-ttu-id="5b3be-110">Profil:</span><span class="sxs-lookup"><span data-stu-id="5b3be-110">Profile:</span></span>
<span data-ttu-id="5b3be-111">Megtekintheti vagy módosíthatja az alább látható fiók beállításait.</span><span class="sxs-lookup"><span data-stu-id="5b3be-111">You can view or change any of your account settings shown below.</span></span> <span data-ttu-id="5b3be-112">Is adhat meg egy másik felhasználó engedély toouse az alkalmazást a saját e-mail-cím a hello alapján [Home](mobile-engagement-user-interface-home.md).</span><span class="sxs-lookup"><span data-stu-id="5b3be-112">You can also give another user permission toouse your application based on their e-mail address from hello [Home](mobile-engagement-user-interface-home.md).</span></span>

![MyAccount2][8]  

## <a name="devices"></a><span data-ttu-id="5b3be-114">Eszközök:</span><span class="sxs-lookup"><span data-stu-id="5b3be-114">Devices:</span></span>
<span data-ttu-id="5b3be-115">Megtekintheti, hozzáadhat és eltávolíthat eszköz eseményazonosítók hello vizsgálati eszközök használható tootest tesztelése a **elérni** vagy **leküldéses** kampányok.</span><span class="sxs-lookup"><span data-stu-id="5b3be-115">You can view, add, or remove test Device ID's of hello test devices that you can use tootest your **reach** or **push** campaigns.</span></span> <span data-ttu-id="5b3be-116">Környezetfüggő utasításokat hogyan toofind hello Eszközazonosítót az eszközök az egyes platformokon (iOS, Android, Windows Phone, stb.) Ha kattintson az "Új eszköz" jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5b3be-116">Contextual instructions for how toofind hello Device ID of devices for each platform (iOS, Android, Windows Phone, etc.) are displayed when you click "New Device".</span></span> 

![MyAccount3][9]  

<span data-ttu-id="5b3be-118">toouse leküldéses API vagy a Device API-val tooknow a felhasználók az eszköz egyedi azonosítója (hello deviceid paraméter) van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5b3be-118">toouse Push API or Device API you need tooknow your users' unique device identifier (hello deviceid parameter).</span></span> <span data-ttu-id="5b3be-119">Nincsenek számos módon tooretrieve azt:</span><span class="sxs-lookup"><span data-stu-id="5b3be-119">There are several ways tooretrieve it:</span></span>

1. <span data-ttu-id="5b3be-120">A háttérrendszerből szolgáltatással hello "Get" hello Device API-val tooget hello teljes listájának eszközazonosítók.</span><span class="sxs-lookup"><span data-stu-id="5b3be-120">From your backend, you can use hello "Get" feature of hello Device API tooget hello full list of device identifiers.</span></span>
2. <span data-ttu-id="5b3be-121">Az alkalmazásból hello SDK tooget használhatja azt.</span><span class="sxs-lookup"><span data-stu-id="5b3be-121">From your app, you can use hello SDK tooget it.</span></span> <span data-ttu-id="5b3be-122">(Az Android, hívja meg az ügynök osztály hello hello getDeviceID() függvényt, és IOS rendszerű eszközökön, olvassa el a hello deviceid tulajdonsága hello ügynök osztály.)</span><span class="sxs-lookup"><span data-stu-id="5b3be-122">(On Android, call hello getDeviceID() function of hello Agent class, and on iOS, read hello deviceid property of hello Agent class.)</span></span>
3. <span data-ttu-id="5b3be-123">A Reach-közlemények Ha hello műveleti URL-cím hello közlemény társított tartalmaz hello {deviceid} mintát, azt automatikusan helyébe hello műveletet kiváltó hello eszköz hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5b3be-123">From a Reach announcement, if hello action URL associated with hello announcement contains hello {deviceid} pattern, it will be automatically replaced by hello identifier of hello device triggering hello action.</span></span>
   <span data-ttu-id="5b3be-124">http://<example>.com/registeruser? deviceid = {deviceid} & otherparam = myparamdata cseréli: http://<example>.com/registeruser? deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata</span><span class="sxs-lookup"><span data-stu-id="5b3be-124">http://<example>.com/registeruser?deviceid={deviceid}&otherparam=myparamdata will be replaced by: http://<example>.com/registeruser?deviceid=XXXXXXXXXXXXXXXX&otherparam=myparamdata</span></span> 
4. <span data-ttu-id="5b3be-125">A Reach-webalkalmazás közlemények Ha hello hello bejelentés HTML-kódot tartalmaz hello {deviceid} mintát, azt automatikusan helyébe hello webes hirdetmény megjelenítő hello eszköz hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5b3be-125">From a Reach web announcement, if hello HTML code of hello announcement contains hello {deviceid} pattern, it will be automatically replaced by hello identifier of hello device displaying hello web announcement.</span></span>
   <span data-ttu-id="5b3be-126">Ez az eszközazonosító: {deviceid} cseréli: Ez az eszközazonosító: XXXXXXXXXXXXXXXX</span><span class="sxs-lookup"><span data-stu-id="5b3be-126">Here is my device identifier: {deviceid} will be replaced by: Here is my device identifier: XXXXXXXXXXXXXXXX</span></span>
5. <span data-ttu-id="5b3be-127">Indítsa el az alkalmazást az eszközön, és végezze el az alkalmazást, amely címkézett esemény.</span><span class="sxs-lookup"><span data-stu-id="5b3be-127">Open your application on your device and perform an Event in your app which has been tagged.</span></span>
   <span data-ttu-id="5b3be-128">A "Részletek – az alkalmazás - figyelő - események - UI" megkeresi a hello esemény hello listában hajtotta végre.</span><span class="sxs-lookup"><span data-stu-id="5b3be-128">From "UI - your app - Monitor - Events - Details", find hello Event you performed in hello list.</span></span>
   <span data-ttu-id="5b3be-129">Kattintson a figyelő hello toothis eseményre.</span><span class="sxs-lookup"><span data-stu-id="5b3be-129">Click toothis event in hello Monitor.</span></span>
   <span data-ttu-id="5b3be-130">Keresse meg az Eszközazonosítót hello eszközök végre ezt az eseményt hello listájában.</span><span class="sxs-lookup"><span data-stu-id="5b3be-130">You should find your Device ID in hello list of hello devices that have performed this event.</span></span>
   <span data-ttu-id="5b3be-131">Ezután másolja le az eszköz Azonosítót, és regisztrálja a hello "UI - fiókom - eszközök – új eszköz - kiválasztása eszközplatformhoz".</span><span class="sxs-lookup"><span data-stu-id="5b3be-131">Then, you can copy this Device ID and register it in hello "UI - My Account - Devices - New Device - Select your device platform".</span></span>
   ><span data-ttu-id="5b3be-132">(Vegye figyelembe, hogy ha idfa-JÁT le van tiltva az iOS, hello Eszközazonosító idővel megváltozhat hello Ha távolítsa el, majd telepítse újra az alkalmazást.)</span><span class="sxs-lookup"><span data-stu-id="5b3be-132">(Be aware that when IDFA is disabled for iOS, hello Device ID may change over hello time if you uninstall and reinstall your app.)</span></span>

## <a name="troubleshooting-guide"></a><span data-ttu-id="5b3be-133">Hibaelhárítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="5b3be-133">Troubleshooting Guide</span></span>
* <span data-ttu-id="5b3be-134">[Hibaelhárítási útmutató - szolgáltatás][Link 24]</span><span class="sxs-lookup"><span data-stu-id="5b3be-134">[Troubleshooting Guide - Service][Link 24]</span></span>

## <a name="see-also"></a><span data-ttu-id="5b3be-135">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5b3be-135">See also</span></span>
* <span data-ttu-id="5b3be-136">[Felhasználói felületének dokumentációja – kezdőlap][Link 13]</span><span class="sxs-lookup"><span data-stu-id="5b3be-136">[UI Documentation - Home][Link 13]</span></span>

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




