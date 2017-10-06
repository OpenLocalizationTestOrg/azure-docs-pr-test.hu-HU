---
title: "az Azure AD-katalógusában alkalmazás hozzáadása aaaProblem |} Microsoft Docs"
description: "Hello gyakori problémák személyek arcfelismerési áttekinteni hozzáadása az Azure AD-katalógusában alkalmazások és mit tehet tooresolve őket"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 654f98116176d5590563c0471b92809f8763fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a><span data-ttu-id="7bf26-103">A probléma az Azure AD-katalógusában alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7bf26-103">Problem adding an Azure AD Gallery application</span></span>

<span data-ttu-id="7bf26-104">Ez a cikk segítséget toounderstand hello gyakori problémák személyek arcfelismerési Azure AD-katalógusában alkalmazások és mit tehet hozzáadásakor tooresolve őket.</span><span class="sxs-lookup"><span data-stu-id="7bf26-104">This article help you toounderstand hello common problems people face when adding Azure AD Gallery applications and what you can do tooresolve them.</span></span>

## <a name="i-clicked-hello-add-button-and-my-application-took-a-long-time-tooappear"></a><span data-ttu-id="7bf26-105">"Hozzáadás" gombra, és az alkalmazás által a hosszú idő tooappear hello gombra kattintás után</span><span class="sxs-lookup"><span data-stu-id="7bf26-105">I clicked hello “add” button and my application took a long time tooappear</span></span>

<span data-ttu-id="7bf26-106">Bizonyos körülmények percig is eltarthat, 1 – 2 (és egyes esetekben hosszabb) számára az alkalmazás tooappear tooyour directory adásakor.</span><span class="sxs-lookup"><span data-stu-id="7bf26-106">Under some circumstances, it can take 1-2 minutes (and sometimes longer) for an application tooappear after adding it tooyour directory.</span></span> <span data-ttu-id="7bf26-107">Ez nem hello normál várt teljesítményét, hello alkalmazás hozzáadása folyamatban van a hello kattintva megtekintheti **értesítések** hello jobb felső sarkában hello (hello harang) ikonra [Azure Portal](https://portal.azure.com/), és keres egy **folyamatban lévő** vagy **befejezve** feliratú értesítési **alkalmazás létrehozása.**</span><span class="sxs-lookup"><span data-stu-id="7bf26-107">While this is not hello normal expected performance, you can see hello application addition is in progress by clicking on hello **Notifications** icon (hello bell) in hello upper right of hello [Azure Portal](https://portal.azure.com/) and looking for an **In Progress** or **Completed** notification labeled **Create application.**</span></span>

<span data-ttu-id="7bf26-108">Ha az alkalmazás soha nem adtak hozzá vagy hibát észlel a gombra kattintva hello **hozzáadása** gombra kattint, megjelenik egy **értesítési** a egy **hiba** állapota.</span><span class="sxs-lookup"><span data-stu-id="7bf26-108">If your application is never added, or you encounter an error when clicking hello **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="7bf26-109">Ha azt szeretné, hogy további információkat a támogatási engingeer megosztása több tooor hello hiba toolearn, hello hibával kapcsolatos további tudnivalókért hello hello utasításait követve megtekintheti [hogyan toosee hello adatait a portál értesítései](#how-to-see-the-details-of-a-portal-notification) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7bf26-109">If you want more details about hello error toolearn more tooor share with a support engingeer, you can see more information about hello error by following hello steps in hello [How toosee hello details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-clicked-hello-add-button-and-my-application-didnt-appear"></a><span data-ttu-id="7bf26-110">I hello "Hozzáadás" gombra kattintott, és az alkalmazás nem jelenik meg</span><span class="sxs-lookup"><span data-stu-id="7bf26-110">I clicked hello “add” button and my application didn’t appear</span></span>

<span data-ttu-id="7bf26-111">Egyes esetekben tootransient problémák miatt hálózati problémák, vagy egy hiba hozzáadása egy kérelem sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="7bf26-111">Sometimes, due tootransient issues, networking problems, or a bug, adding an application fail.</span></span> <span data-ttu-id="7bf26-112">Beállíthatja, hogy ez akkor fordul elő, amikor hello kattint **értesítések** (hello harang) ikonra az hello jobb felső sarkában hello Azure portálon, és tekintse meg a piros (!) ikon következő tooyour **alkalmazás létrehozása** értesítés.</span><span class="sxs-lookup"><span data-stu-id="7bf26-112">You can tell this happens when you click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal and you see a red (!) icon next tooyour **Create application** notification.</span></span> <span data-ttu-id="7bf26-113">Ez azt jelzi, hogy hibás hello alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7bf26-113">This indicates there was an error when creating hello application.</span></span>

<span data-ttu-id="7bf26-114">Ha hibát észlel a gombra kattintva hello **Hozzáadás** gombra kattint, megjelenik egy **értesítési** a egy **hiba** állapota.</span><span class="sxs-lookup"><span data-stu-id="7bf26-114">If you encounter an error when clicking hello **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="7bf26-115">Ha azt szeretné, hogy további információkat a támogatási engingeer megosztása több tooor hello hiba toolearn, hello hibával kapcsolatos további tudnivalókért hello hello utasításait követve megtekintheti [hogyan toosee hello adatait a portál értesítései](#how-to-see-the-details-of-a-portal-notification) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7bf26-115">If you want more details about hello error toolearn more tooor share with a support engingeer, you can see more information about hello error by following hello steps in hello [How toosee hello details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

 ## <a name="i-dont-know-how-tooset-up-my-application-once-ive-added-it"></a><span data-ttu-id="7bf26-116">Nem tudom, hogy hogyan tooset be egyszer az alkalmazásban felvett azt</span><span class="sxs-lookup"><span data-stu-id="7bf26-116">I don’t know how tooset up my application once I’ve added it</span></span>

<span data-ttu-id="7bf26-117">Ha alkalmazások megtanulni segítségre van szüksége, hello [oktatóanyagok lista ismerteti tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) cikk egy remek toostart.</span><span class="sxs-lookup"><span data-stu-id="7bf26-117">If you need help learning about applications, hello [List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) article is a good place toostart.</span></span>

<span data-ttu-id="7bf26-118">A hozzáadása toothis hello [az Azure AD-alkalmazások dokumentumtár](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) toolearn kapcsolatos további információkért az egyszeri bejelentkezés az Azure ad-vel és annak működéséről.</span><span class="sxs-lookup"><span data-stu-id="7bf26-118">In addition toothis, hello [Azure AD Applications Document Library](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) help you toolearn more about single sign-on with Azure AD and how it works.</span></span>

## <a name="how-toosee-hello-details-of-a-portal-notification"></a><span data-ttu-id="7bf26-119">Hogyan toosee hello adatait a portál értesítései</span><span class="sxs-lookup"><span data-stu-id="7bf26-119">How toosee hello details of a portal notification</span></span>

<span data-ttu-id="7bf26-120">A portál értesítései hello részleteit láthatja hello alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="7bf26-120">You can see hello details of any portal notification by following hello steps below:</span></span>

1.  <span data-ttu-id="7bf26-121">Kattintson a hello **értesítések** hello hello Azure portál jobb felső sarkában (hello harang) ikonra</span><span class="sxs-lookup"><span data-stu-id="7bf26-121">click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal</span></span>

2.  <span data-ttu-id="7bf26-122">Válassza ki az értesítésekhez egy **hiba** (piros (!) következő toothem rendelkezők) állapotát.</span><span class="sxs-lookup"><span data-stu-id="7bf26-122">Select any notification in an **Error** state (those with a red (!) next toothem).</span></span>

    >[!NOTE]
    ><span data-ttu-id="7bf26-123">Nem kattint az értesítések egy **sikeres** vagy **folyamatban lévő** állapotát.</span><span class="sxs-lookup"><span data-stu-id="7bf26-123">You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
    >
    >

3.  <span data-ttu-id="7bf26-124">A nyitott hello **értesítési részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="7bf26-124">This open hello **Notification Details** blade.</span></span>

4.  <span data-ttu-id="7bf26-125">Ezt az információt használja saját kezűleg toounderstand több hello probléma vonatkozó részletek.</span><span class="sxs-lookup"><span data-stu-id="7bf26-125">Use this information yourself toounderstand more details about hello problem.</span></span>

5.  <span data-ttu-id="7bf26-126">Ha további segítségre van, is megoszthatja ezeket az információkat a támogatási szakértőhöz vagy hello csoport tooget Terméksúgó a probléma megoldásában.</span><span class="sxs-lookup"><span data-stu-id="7bf26-126">If you still need help, you can also share this information with a support engineer or hello product group tooget help with your problem.</span></span>

6.  <span data-ttu-id="7bf26-127">Hello kattintson **másolása** **ikon** toohello sarkában hello **hiba másolása** szövegmező toocopy összes hello értesítési részletek tooshare a támogatási szolgálathoz vagy a termék csoport visszafejtés</span><span class="sxs-lookup"><span data-stu-id="7bf26-127">Click hello **copy** **icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a><span data-ttu-id="7bf26-128">Hogyan segíthet az tooget értesítési részletek tooa támogatási szakértőhöz küldése</span><span class="sxs-lookup"><span data-stu-id="7bf26-128">How tooget help by sending notification details tooa support engineer</span></span>

<span data-ttu-id="7bf26-129">Nagyon fontos, hogy megosztott **összes alább felsorolt részletek hello** , ha segítségre van szüksége, így gyorsan segít a támogatási szakértőhöz.</span><span class="sxs-lookup"><span data-stu-id="7bf26-129">It is very important that you share **all hello details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="7bf26-130">Ehhez egyszerűen az **elkészíti a képernyőfelvételt** vagy hello kattintva **másolási hibaikon**, hello toohello jobbra található **hiba másolása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7bf26-130">You can do this easily by **taking a screenshot,** or by clicking hello **Copy error icon**, found toohello right of hello **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="7bf26-131">Értesítési részletek alapján</span><span class="sxs-lookup"><span data-stu-id="7bf26-131">Notification Details Explained</span></span>

<span data-ttu-id="7bf26-132">az alábbi hello több milyen egyes hello értesítési elemek jelent, és azok által biztosított példák ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7bf26-132">hello below explains more what each of hello notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="7bf26-133">Alapvető értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="7bf26-133">Essential Notification Items</span></span>

-   <span data-ttu-id="7bf26-134">**Cím** – hello informatív címet hello értesítés</span><span class="sxs-lookup"><span data-stu-id="7bf26-134">**Title** – hello descriptive title of hello notification</span></span>

  * <span data-ttu-id="7bf26-135">Példa – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="7bf26-135">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="7bf26-136">**Leírás** – hello Mi történt hello művelet leírása</span><span class="sxs-lookup"><span data-stu-id="7bf26-136">**Description** – hello description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="7bf26-137">Példa – **megadott belső URL-cím már használatban van egy másik alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="7bf26-137">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="7bf26-138">**Értesítés azonosítója** – hello értesítési hello egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="7bf26-138">**Notification Id** – hello unique id of hello notification</span></span>

    -   <span data-ttu-id="7bf26-139">Példa – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="7bf26-139">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="7bf26-140">**Ügyfélkérelem-azonosító** – a böngésző által végrehajtott hello adott kérelem azonosítója</span><span class="sxs-lookup"><span data-stu-id="7bf26-140">**Client Request Id** – hello specific request id made by your browser</span></span>

    -   <span data-ttu-id="7bf26-141">Példa – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="7bf26-141">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="7bf26-142">**Stamp UTC-idő** – hello időbélyeg időintervalluma hello értesítési történt UTC szerint</span><span class="sxs-lookup"><span data-stu-id="7bf26-142">**Time Stamp UTC** – hello timestamp during which hello notification occurred, in UTC</span></span>

    -   <span data-ttu-id="7bf26-143">Példa – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="7bf26-143">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="7bf26-144">**Belső tranzakció azonosítója** – hello használhatjuk toolook hello hiba a rendszer belső azonosítója</span><span class="sxs-lookup"><span data-stu-id="7bf26-144">**Internal Transaction Id** – hello internal ID we can use toolook hello error up in our systems</span></span>

    -   <span data-ttu-id="7bf26-145">Példa – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="7bf26-145">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="7bf26-146">**Egyszerű felhasználónév** – hello műveletet végre hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="7bf26-146">**UPN** – hello user who performed hello operation</span></span>

    -   <span data-ttu-id="7bf26-147">– Példa**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="7bf26-147">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="7bf26-148">**A bérlői azonosító** – hello egyedi Azonosítóját hello olyan bérlő számára, felhasználói hello hello műveletet végrehajtó tagja volt.</span><span class="sxs-lookup"><span data-stu-id="7bf26-148">**Tenant Id** – hello unique ID of hello tenant that hello user who performed hello operation was a member of</span></span>

    -   <span data-ttu-id="7bf26-149">Példa – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="7bf26-149">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="7bf26-150">**Felhasználói objektum azonosítója** – hello hello műveletet végre hello felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="7bf26-150">**User object Id** – hello unique ID of hello user who performed hello operation</span></span>

    -   <span data-ttu-id="7bf26-151">Példa – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="7bf26-151">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="7bf26-152">Részletes értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="7bf26-152">Detailed Notification Items</span></span>

-   <span data-ttu-id="7bf26-153">**Megjelenítendő név** – **(üres is lehet)** hello hiba részletes megjelenítendő neve</span><span class="sxs-lookup"><span data-stu-id="7bf26-153">**Display Name** – **(can be empty)** a more detailed display name for hello error</span></span>

    -   <span data-ttu-id="7bf26-154">Példa – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="7bf26-154">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="7bf26-155">**Állapot** – hello hello értesítési adott állapota</span><span class="sxs-lookup"><span data-stu-id="7bf26-155">**Status** – hello specific status of hello notification</span></span>

    -   <span data-ttu-id="7bf26-156">Példa – **nem sikerült**</span><span class="sxs-lookup"><span data-stu-id="7bf26-156">Example – **Failed**</span></span>

-   <span data-ttu-id="7bf26-157">**Objektumazonosító:** – **(üres is lehet)** hello Objektumazonosító alapján mely hello művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7bf26-157">**Object Id** – **(can be empty)** hello object ID against which hello operation was performed</span></span>

    -   <span data-ttu-id="7bf26-158">Példa – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="7bf26-158">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="7bf26-159">**Részletek** – hello részletes Mi történt hello művelet leírása</span><span class="sxs-lookup"><span data-stu-id="7bf26-159">**Details** – hello detailed description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="7bf26-160">Példa – **belső URL-cím "http://bing.com/" érvénytelen, mert már használatban van**</span><span class="sxs-lookup"><span data-stu-id="7bf26-160">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="7bf26-161">**Másolja át a hiba** – hello kattintson **másolás ikon** sarkában hello toohello **hiba másolása** szövegmező toocopy összes hello értesítési részletek tooshare a támogatási szolgálathoz vagy a termék csoport visszafejtés</span><span class="sxs-lookup"><span data-stu-id="7bf26-161">**Copy error** – Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

    -   <span data-ttu-id="7bf26-162">Példa```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="7bf26-162">Example ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="7bf26-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7bf26-163">Next steps</span></span>
[<span data-ttu-id="7bf26-164">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="7bf26-164">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
