---
title: Azure Application Insights az Analytics aaaTroubleshoot |} Microsoft Docs
description: "Problémák az Application Insights analytics? Itt érdemes kezdenie. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: bwren
ms.openlocfilehash: e3be2fbc0237440d3b8a736484434a9f276296c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a><span data-ttu-id="a74f4-104">Elemzés hibaelhárítása az Application Insights szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="a74f4-104">Troubleshoot Analytics in Application Insights</span></span>
<span data-ttu-id="a74f4-105">Problémák [Application Insights Analytics](app-insights-analytics.md)?</span><span class="sxs-lookup"><span data-stu-id="a74f4-105">Problems with [Application Insights Analytics](app-insights-analytics.md)?</span></span> <span data-ttu-id="a74f4-106">Itt érdemes kezdenie.</span><span class="sxs-lookup"><span data-stu-id="a74f4-106">Start here.</span></span> <span data-ttu-id="a74f4-107">Analytics egy hello hatékony keresési eszköz az Azure Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="a74f4-107">Analytics is hello powerful search tool of Azure Application Insights.</span></span>

## <a name="limits"></a><span data-ttu-id="a74f4-108">Korlátok</span><span class="sxs-lookup"><span data-stu-id="a74f4-108">Limits</span></span>
* <span data-ttu-id="a74f4-109">Jelenleg a lekérdezési eredmények korlátozott toojust: hetente az elmúlt adatok keresztül.</span><span class="sxs-lookup"><span data-stu-id="a74f4-109">At present, query results are limited toojust over a week of past data.</span></span>
* <span data-ttu-id="a74f4-110">A teszteljük böngészők: Chrome, a peremhálózati és az Internet Explorer legújabb verziója.</span><span class="sxs-lookup"><span data-stu-id="a74f4-110">Browsers we test on: latest editions of Chrome, Edge, and Internet Explorer.</span></span>

## <a name="known-incompatible-browser-extensions"></a><span data-ttu-id="a74f4-111">Ismert kompatibilis böngészőbővítmények</span><span class="sxs-lookup"><span data-stu-id="a74f4-111">Known incompatible browser extensions</span></span>
* <span data-ttu-id="a74f4-112">Ghostery</span><span class="sxs-lookup"><span data-stu-id="a74f4-112">Ghostery</span></span>

<span data-ttu-id="a74f4-113">Hello bővítmény letiltása, vagy használjon egy másik böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a74f4-113">Disable hello extension or use a different browser.</span></span>

## <span data-ttu-id="a74f4-114"><a name="e-a"></a>"Nem várt hiba"</span><span class="sxs-lookup"><span data-stu-id="a74f4-114"><a name="e-a"></a> "Unexpected error"</span></span>
![Váratlan hiba képernyő](./media/app-insights-analytics-troubleshooting/010.png)

<span data-ttu-id="a74f4-116">Belső hiba történt a portál futásidőben – nem kezelt kivétel.</span><span class="sxs-lookup"><span data-stu-id="a74f4-116">Internal error occurred during portal runtime – unhandled exception.</span></span>

* <span data-ttu-id="a74f4-117">Hello a gyorsítótár törlése.</span><span class="sxs-lookup"><span data-stu-id="a74f4-117">Clean hello browser's cache.</span></span> 

## <span data-ttu-id="a74f4-118"><a name="e-b"></a>403... Kérjük, próbálkozzon tooreload</span><span class="sxs-lookup"><span data-stu-id="a74f4-118"><a name="e-b"></a>403 ... please try tooreload</span></span>
![403... Kérjük, próbálkozzon tooreload](./media/app-insights-analytics-troubleshooting/020.png)

<span data-ttu-id="a74f4-120">Egy hitelesítési kapcsolatos hiba történt (hitelesítés vagy során a hozzáférési jogkivonatok létrehozásához).</span><span class="sxs-lookup"><span data-stu-id="a74f4-120">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="a74f4-121">Előfordulhat, hogy hello portal semmilyen módon nem túl helyreállítása böngésző beállításainak módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="a74f4-121">hello portal may have no way too recover without changing browser settings.</span></span>

* <span data-ttu-id="a74f4-122">Győződjön meg arról [külső cookie-k engedélyezettek](#cookies) hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a74f4-122">Verify [third party cookies are enabled](#cookies) in hello browser.</span></span> 

## <span data-ttu-id="a74f4-123"><a name="authentication"></a>403... Ellenőrizze a biztonsági zóna</span><span class="sxs-lookup"><span data-stu-id="a74f4-123"><a name="authentication"></a>403 ... verify security zone</span></span>
![403.. .verify biztonsági zóna](./media/app-insights-analytics-troubleshooting/030.png)

<span data-ttu-id="a74f4-125">Egy hitelesítési kapcsolatos hiba történt (hitelesítés vagy során a hozzáférési jogkivonatok létrehozásához).</span><span class="sxs-lookup"><span data-stu-id="a74f4-125">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="a74f4-126">Előfordulhat, hogy hello portal semmilyen módon nem túl helyreállítása böngésző beállításainak módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="a74f4-126">hello portal may have no way too recover without changing browser settings.</span></span>

1. <span data-ttu-id="a74f4-127">Győződjön meg arról [külső cookie-k engedélyezettek](#cookies) hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a74f4-127">Verify [third party cookies are enabled](#cookies) in hello browser.</span></span> 
2. <span data-ttu-id="a74f4-128">Használta a kedvenc, könyvjelző vagy mentett hivatkozást tooopen hello Analytics portál?</span><span class="sxs-lookup"><span data-stu-id="a74f4-128">Did you use a favorite, bookmark or saved link tooopen hello Analytics portal?</span></span> <span data-ttu-id="a74f4-129">Jelentkezett be, mint amennyi hello hivatkozás mentésekor használt más hitelesítő adatokkal van?</span><span class="sxs-lookup"><span data-stu-id="a74f4-129">Are you signed in with different credentials than you used when you saved hello link?</span></span>
3. <span data-ttu-id="a74f4-130">Próbálja meg egy-az-private vagy incognito böngésző ablakban (után minden ilyen windows bezárása) használatával.</span><span class="sxs-lookup"><span data-stu-id="a74f4-130">Try using an in-private/incognito browser window (after closing all such windows).</span></span> <span data-ttu-id="a74f4-131">A hitelesítő adatokat kell tooprovide.</span><span class="sxs-lookup"><span data-stu-id="a74f4-131">You'll have tooprovide your credentials.</span></span> 
4. <span data-ttu-id="a74f4-132">Nyisson meg egy másik (normál) böngészőablakot, és nyissa meg túl[Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a74f4-132">Open another (ordinary) browser window and go too[Azure](https://portal.azure.com).</span></span> <span data-ttu-id="a74f4-133">Jelentkezzen ki. Ezután nyissa meg a hivatkozást, és hello megfelelő hitelesítő adatokkal jelentkezhetnek be.</span><span class="sxs-lookup"><span data-stu-id="a74f4-133">Sign out. Then open your link and sign in with hello correct credentials.</span></span>
5. <span data-ttu-id="a74f4-134">Széle és az Internet Explorer is is felhasználónál Ez a hiba fog történni megbízható helyek zónájába beállítások nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a74f4-134">Edge and Internet Explorer users can also get this error when trusted zone settings are not supported.</span></span>
   
    <span data-ttu-id="a74f4-135">Győződjön meg arról is [Analytics portál](https://analytics.applicationinsights.io) és [Azure Active Directory portálon](https://portal.azure.com) a hello ugyanazt biztonsági zóna:</span><span class="sxs-lookup"><span data-stu-id="a74f4-135">Verify both [Analytics portal](https://analytics.applicationinsights.io) and [Azure Active Directory portal](https://portal.azure.com) are in hello same security zone:</span></span>
   
   * <span data-ttu-id="a74f4-136">Az Internet Explorerben nyissa meg a **Internetbeállítások**, **biztonsági**, **megbízható helyek**, **helyek**:</span><span class="sxs-lookup"><span data-stu-id="a74f4-136">In Internet Explorer, open **Internet Options**, **Security**, **Trusted sites**, **Sites**:</span></span>
     
     ![Internetbeállítások párbeszédpanel, ha egy helyet tooTrusted helyek](./media/app-insights-analytics-troubleshooting/033.png)
     
     <span data-ttu-id="a74f4-138">Hello webhelyek listája, a következő URL-címek hello érhetők el, ha győződjön meg arról, hogy hello mások szerepelnek is:</span><span class="sxs-lookup"><span data-stu-id="a74f4-138">In hello Websites list, if any of hello following URLs are included, make sure that hello others are included also:</span></span>
     
     <span data-ttu-id="a74f4-139">https://Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="a74f4-139">https://analytics.applicationinsights.io</span></span><br/>
     <span data-ttu-id="a74f4-140">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="a74f4-140">https://login.microsoftonline.com</span></span><br/>
     <span data-ttu-id="a74f4-141">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="a74f4-141">https://login.windows.net</span></span>

## <span data-ttu-id="a74f4-142"><a name="e-d"></a>404 ... Nem található az erőforrás</span><span class="sxs-lookup"><span data-stu-id="a74f4-142"><a name="e-d"></a>404 ... Resource not found</span></span>
![404... erőforrás nem található](./media/app-insights-analytics-troubleshooting/040.png)

<span data-ttu-id="a74f4-144">Alkalmazás-erőforrás törölve lett az Application Insights, és már nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="a74f4-144">Application resource was deleted from Application Insights and isn’t available anymore.</span></span> <span data-ttu-id="a74f4-145">Ez akkor fordulhat elő, ha mentett hello URL-cím toohello Analytics lap.</span><span class="sxs-lookup"><span data-stu-id="a74f4-145">This can happen if you saved hello URL toohello Analytics page.</span></span>

## <span data-ttu-id="a74f4-146"><a name="e-e"></a>403 ... Nincs engedély</span><span class="sxs-lookup"><span data-stu-id="a74f4-146"><a name="e-e"></a>403 ... No authorization</span></span>
![403... nem engedélyezett](./media/app-insights-analytics-troubleshooting/050.png)

<span data-ttu-id="a74f4-148">Nincs engedélye tooopen az alkalmazás Analytics.</span><span class="sxs-lookup"><span data-stu-id="a74f4-148">You don't have permission tooopen this application in Analytics.</span></span>

* <span data-ttu-id="a74f4-149">Kapott hello hivatkozás a valaki más?</span><span class="sxs-lookup"><span data-stu-id="a74f4-149">Did you get hello link from someone else?</span></span> <span data-ttu-id="a74f4-150">Kérje meg őket arról, hogy Ön hello toomake [olvasók vagy az erőforráscsoport közreműködők](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="a74f4-150">Ask them toomake sure you are in hello [readers or contributors for this resource group](app-insights-resources-roles-access-control.md).</span></span>
* <span data-ttu-id="a74f4-151">Menti hello kapcsolat más hitelesítő adatok használatával?</span><span class="sxs-lookup"><span data-stu-id="a74f4-151">Did you save hello link using different credentials?</span></span> <span data-ttu-id="a74f4-152">Nyissa meg hello [Azure-portálon](https://portal.azure.com), és jelentkezzen ki, majd próbálja meg a hivatkozás újra, adja meg hello megfelelő hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a74f4-152">Open hello [Azure portal](https://portal.azure.com), sign out, and then try this link again, providing hello correct credentials.</span></span>

## <span data-ttu-id="a74f4-153"><a name="html-storage"></a>403 ... HTML5-tárolóra</span><span class="sxs-lookup"><span data-stu-id="a74f4-153"><a name="html-storage"></a>403 ... HTML5 Storage</span></span>
<span data-ttu-id="a74f4-154">A portál HTML5 localStorage és sessionStorage használja.</span><span class="sxs-lookup"><span data-stu-id="a74f4-154">Our portal uses HTML5 localStorage and sessionStorage.</span></span>

* <span data-ttu-id="a74f4-155">Chrome: Beállítások, adatvédelmi, tartalombeállításait.</span><span class="sxs-lookup"><span data-stu-id="a74f4-155">Chrome: Settings, privacy, content settings.</span></span>
* <span data-ttu-id="a74f4-156">Az Internet Explorer: Internetbeállítások, Speciális lap biztonsági, DOM tárolási engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a74f4-156">Internet Explorer: Internet Options, Advanced tab, Security, Enable DOM Storage</span></span>

![403... Próbálja tooenable HTML5-tároló](./media/app-insights-analytics-troubleshooting/060.png)

## <span data-ttu-id="a74f4-158"><a name="e-g"></a>404 ... Nem található előfizetés</span><span class="sxs-lookup"><span data-stu-id="a74f4-158"><a name="e-g"></a>404 ... Subscription not found</span></span>
![404 ... Nem található előfizetés](./media/app-insights-analytics-troubleshooting/070.png)

<span data-ttu-id="a74f4-160">hello URL-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="a74f4-160">hello URL is invalid.</span></span> 

* <span data-ttu-id="a74f4-161">Nyissa meg az alkalmazás-erőforrást hello [Application Insights portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a74f4-161">Open hello app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="a74f4-162">Ezután használja az hello Analytics gombra.</span><span class="sxs-lookup"><span data-stu-id="a74f4-162">Then use hello Analytics button.</span></span>

## <span data-ttu-id="a74f4-163"><a name="e-h"></a>404... a lap nem található</span><span class="sxs-lookup"><span data-stu-id="a74f4-163"><a name="e-h"></a>404 ... page doesn't exist</span></span>
![404 ... A lap nem található.](./media/app-insights-analytics-troubleshooting/080.png)

<span data-ttu-id="a74f4-165">hello URL-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="a74f4-165">hello URL is invalid.</span></span>

* <span data-ttu-id="a74f4-166">Nyissa meg az alkalmazás-erőforrást hello [Application Insights portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a74f4-166">Open hello app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="a74f4-167">Ezután használja az hello Analytics gombra.</span><span class="sxs-lookup"><span data-stu-id="a74f4-167">Then use hello Analytics button.</span></span>

## <span data-ttu-id="a74f4-168"><a name="cookies"></a>Cookie-k engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a74f4-168"><a name="cookies"></a>Enable third-party cookies</span></span>
  <span data-ttu-id="a74f4-169">Lásd: [hogyan toodisable harmadik fél cookie-k](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), de figyelje meg, igazolnia kell túl**engedélyezése** őket.</span><span class="sxs-lookup"><span data-stu-id="a74f4-169">See [how toodisable third party cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), but notice we need too**enable** them.</span></span>


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

