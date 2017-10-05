---
title: "A probléma nem galéria-alkalmazás hozzáadása |} Microsoft Docs"
description: "A gyakori problémák személyek arcfelismerési áttekinteni egyéni nem galéria-alkalmazások hozzáadása"
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
ms.openlocfilehash: bb3fc7877f4e7cafc3904fc67abd87b897874d8a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problem-adding-a-non-gallery-application"></a><span data-ttu-id="5a4ec-103">A probléma nem galéria-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a4ec-103">Problem adding a non-gallery application</span></span>

<span data-ttu-id="5a4ec-104">Ez a cikk segítenek megérteni a gyakori problémák személyek arcfelismerési hozzáadásakor **egyéni nem galéria alkalmazások** és mit tehet a megoldásukkal együtt.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-104">This article help you to understand the common problems people face when adding **custom non-gallery applications** and what you can do to resolve them.</span></span> 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a><span data-ttu-id="5a4ec-105">A "Hozzáadás" gombra kattintáskor és az alkalmazás hosszú időt vett igénybe jelenik meg</span><span class="sxs-lookup"><span data-stu-id="5a4ec-105">I clicked the “add” button and my application took a long time to appear</span></span>

<span data-ttu-id="5a4ec-106">Bizonyos körülmények percig is eltarthat, 1 – 2 (és egyes esetekben hosszabb) az alkalmazás számára, hogy a címtárban való hozzáadását követően jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-106">Under some circumstances, it can take 1-2 minutes (and sometimes longer) for an application to appear after adding it to your directory.</span></span> <span data-ttu-id="5a4ec-107">Ez nem a normál várt teljesítményét, az alkalmazás hozzáadása folyamatban van a kattintva megtekintheti a **értesítések** (a harang) ikonra a képernyő jobb felső sarkában a [Azure Portal](https://portal.azure.com/) és keresése az egy **folyamatban lévő** vagy **befejezve** feliratú értesítési **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-107">While this is not the normal expected performance, you can see the application addition is in progress by clicking on the **Notifications** icon (the bell) in the upper right of the [Azure Portal](https://portal.azure.com/) and looking for an **In Progress** or **Completed** notification labeled **Create application**.</span></span>

<span data-ttu-id="5a4ec-108">Ha az alkalmazás soha nem adtak hozzá vagy hibát észlel a gombra kattintva a **Hozzáadás** gombra kattint, megjelenik egy **értesítési** a egy **hiba** állapotát.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-108">If your application is never added, or you encounter an error when clicking the **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="5a4ec-109">További információk, vagy egy támogatási engingeer megosztása a hibával kapcsolatos további adatokra van szüksége, ha a hibával kapcsolatos további információkat a lépéseket követve megtekintheti a [a portál értesítései részleteinek megtekintése](#how-to-see-the-details-of-a-portal-notification) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-109">If you want more details about the error to learn more to or share with a support engingeer, you can see more information about the error by following the steps in the [How to see the details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a><span data-ttu-id="5a4ec-110">A "Hozzáadás" gombra kattintáskor és az alkalmazás nem jelenik meg</span><span class="sxs-lookup"><span data-stu-id="5a4ec-110">I clicked the “add” button and my application didn’t appear</span></span>

<span data-ttu-id="5a4ec-111">Egyes esetekben átmeneti hibái miatt hálózati problémák, vagy egy hiba hozzáadása egy kérelem sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-111">Sometimes, due to transient issues, networking problems, or a bug, adding an application fail.</span></span> <span data-ttu-id="5a4ec-112">Beállíthatja, hogy ez akkor fordul elő, amikor kattint a **értesítések** (a harang) ikonra a képernyő jobb felső sarkában az Azure portálon, és a piros (!) ikon mellett látható a **alkalmazás létrehozása** értesítést.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-112">You can tell this happens when you click the **Notifications** icon (the bell) in the upper right of the Azure Portal and you see a red (!) icon next to your **Create application** notification.</span></span> <span data-ttu-id="5a4ec-113">Ez azt jelzi, hogy hiba történt az alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-113">This indicates there was an error when creating the application.</span></span>

<span data-ttu-id="5a4ec-114">Ha hibát észlel a gombra kattintva a **Hozzáadás** gombra kattint, megjelenik egy **értesítési** a egy **hiba** állapotát.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-114">If you encounter an error when clicking the **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="5a4ec-115">További információk, vagy egy támogatási engingeer megosztása a hibával kapcsolatos további adatokra van szüksége, ha a hibával kapcsolatos további információkat a lépéseket követve megtekintheti a [a portál értesítései részleteinek megtekintése](#how-to-see-the-details-of-a-portal-notification) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-115">If you want more details about the error to learn more to or share with a support engingeer, you can see more information about the error by following the steps in the [How to see the details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a><span data-ttu-id="5a4ec-116">Nem tudom, ha felvett, az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="5a4ec-116">I don’t know how to set up my application once I’ve added it</span></span>

<span data-ttu-id="5a4ec-117">Ha segítségre van szüksége egyéni alkalmazások megtanulni a [az Azure AD-alkalmazások dokumentumtár](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) segítséget nyújtanak további információt az egyszeri bejelentkezés az Azure ad-vel és annak működéséről.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-117">If you need help learning about custom applications, the [Azure AD Applications Document Library](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) help you to learn more about single sign-on with Azure AD and how it works.</span></span>

## <a name="how-to-see-the-details-of-a-portal-notification"></a><span data-ttu-id="5a4ec-118">A portál értesítései részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="5a4ec-118">How to see the details of a portal notification</span></span>

<span data-ttu-id="5a4ec-119">A portál értesítései részleteit láthatja az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="5a4ec-119">You can see the details of any portal notification by following the steps below:</span></span>

1.  <span data-ttu-id="5a4ec-120">Kattintson a **értesítések** (a harang) ikonra a képernyő jobb felső sarkában az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="5a4ec-120">click the **Notifications** icon (the bell) in the upper right of the Azure Portal</span></span>

2.  <span data-ttu-id="5a4ec-121">Válassza ki az értesítésekhez egy **hiba** állapota (amelyek a piros (!) mellett).</span><span class="sxs-lookup"><span data-stu-id="5a4ec-121">Select any notification in an **Error** state (those with a red (!) next to them).</span></span>

   >[!NOTE]
   ><span data-ttu-id="5a4ec-122">Nem kattint az értesítések egy **sikeres** vagy **folyamatban lévő** állapotát.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-122">You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
   >
   >

3.  <span data-ttu-id="5a4ec-123">A Megnyitás a **értesítési részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-123">This open the **Notification Details** blade.</span></span>

4.  <span data-ttu-id="5a4ec-124">Ez a témakör a problémával kapcsolatos további részletekért megértéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-124">Use this information yourself to understand more details about the problem.</span></span>

5.  <span data-ttu-id="5a4ec-125">Ha további segítségre van, a ezek az információk megosztása a támogatási szakember vagy a csoport segítség a probléma megoldásában.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-125">If you still need help, you can also share this information with a support engineer or the product group to get help with your problem.</span></span>

6.  <span data-ttu-id="5a4ec-126">Kattintson a **másolás ikon** jobb oldalán a **hiba másolása** szövegmező megosztani a támogatási szolgálathoz vagy a termék csoport mérnöke értesítés részleteinek másolása.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-126">Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer.</span></span>

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a><span data-ttu-id="5a4ec-127">Segítség kérése értesítési részletek küld egy támogatási szakértőhöz</span><span class="sxs-lookup"><span data-stu-id="5a4ec-127">How to get help by sending notification details to a support engineer</span></span>

<span data-ttu-id="5a4ec-128">Nagyon fontos, hogy megosztott **alább felsorolt összes részletes** , ha segítségre van szüksége, így gyorsan segít a támogatási szakértőhöz.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-128">It is very important that you share **all the details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="5a4ec-129">Ehhez az egyszerű **elkészíti a képernyőfelvételt** vagy kattintson a **másolási hibaikon**, míg a talált jobb oldalán a **hiba másolása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-129">You can do this easily by **taking a screenshot,** or by clicking the **Copy error icon**, found to the right of the **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="5a4ec-130">Értesítési részletek alapján</span><span class="sxs-lookup"><span data-stu-id="5a4ec-130">Notification Details Explained</span></span>

<span data-ttu-id="5a4ec-131">Az alábbiakban azt ismerteti, több milyen az értesítés azt jelenti, hogy elemeket, és azok példákat.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-131">The below explains more what each of the notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="5a4ec-132">Alapvető értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="5a4ec-132">Essential Notification Items</span></span>

-   <span data-ttu-id="5a4ec-133">**Cím** – az értesítés informatív címet</span><span class="sxs-lookup"><span data-stu-id="5a4ec-133">**Title** – the descriptive title of the notification</span></span>
   *  <span data-ttu-id="5a4ec-134">Példa – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-134">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="5a4ec-135">**Leírás** – Mi történt a művelet leírása</span><span class="sxs-lookup"><span data-stu-id="5a4ec-135">**Description** – the description of what occurred as a result of the operation</span></span>

   *  <span data-ttu-id="5a4ec-136">Példa – **megadott belső URL-cím már használatban van egy másik alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-136">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="5a4ec-137">**Értesítés azonosítója** – az értesítés egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="5a4ec-137">**Notification Id** – the unique id of the notification</span></span>

   *  <span data-ttu-id="5a4ec-138">Példa – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-138">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="5a4ec-139">**Ügyfélkérelem-azonosító** – a böngésző által végrehajtott egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="5a4ec-139">**Client Request Id** – the specific request id made by your browser</span></span>

   *  <span data-ttu-id="5a4ec-140">Példa – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-140">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="5a4ec-141">**Stamp UTC-idő** – a timestamp, amely során az értesítés történt UTC szerint</span><span class="sxs-lookup"><span data-stu-id="5a4ec-141">**Time Stamp UTC** – the timestamp during which the notification occurred, in UTC</span></span>

   *  <span data-ttu-id="5a4ec-142">Példa – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-142">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="5a4ec-143">**Belső tranzakció azonosítója** – belső azonosítója a Microsoft segítségével, a rendszer keresse meg a hiba</span><span class="sxs-lookup"><span data-stu-id="5a4ec-143">**Internal Transaction Id** – the internal ID we can use to look the error up in our systems</span></span>

   *  <span data-ttu-id="5a4ec-144">Példa – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-144">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="5a4ec-145">**Egyszerű felhasználónév** – a műveletet végző felhasználó</span><span class="sxs-lookup"><span data-stu-id="5a4ec-145">**UPN** – the user who performed the operation</span></span>

   *  <span data-ttu-id="5a4ec-146">– Példa**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-146">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="5a4ec-147">**A bérlői azonosító** – az egyedi azonosító a bérlő által, hogy a művelet a felhasználó tagja volt.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-147">**Tenant Id** – the unique ID of the tenant that the user who performed the operation was a member of</span></span>

   *  <span data-ttu-id="5a4ec-148">Példa – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-148">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="5a4ec-149">**Felhasználói objektum azonosítója** – a művelet a felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="5a4ec-149">**User object Id** – the unique ID of the user who performed the operation</span></span>

 *  <span data-ttu-id="5a4ec-150">Példa – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-150">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="5a4ec-151">Részletes értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="5a4ec-151">Detailed Notification Items</span></span>

-   <span data-ttu-id="5a4ec-152">**Megjelenítendő név** – **(üres is lehet)** a hiba részletes megjelenítendő neve</span><span class="sxs-lookup"><span data-stu-id="5a4ec-152">**Display Name** – **(can be empty)** a more detailed display name for the error</span></span>

  *  <span data-ttu-id="5a4ec-153">Példa – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-153">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="5a4ec-154">**Állapot** – az értesítésre adott állapota</span><span class="sxs-lookup"><span data-stu-id="5a4ec-154">**Status** – the specific status of the notification</span></span>

   *  <span data-ttu-id="5a4ec-155">Példa – **nem sikerült**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-155">Example – **Failed**</span></span>

-   <span data-ttu-id="5a4ec-156">**Objektumazonosító:** – **(üres is lehet)** az objektum azonosítója, amely alapján a művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="5a4ec-156">**Object Id** – **(can be empty)** the object ID against which the operation was performed</span></span>

   *  <span data-ttu-id="5a4ec-157">Példa – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-157">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="5a4ec-158">**Részletek** – a részletes Mi történt a művelet leírása</span><span class="sxs-lookup"><span data-stu-id="5a4ec-158">**Details** – the detailed description of what occurred as a result of the operation</span></span>

   *  <span data-ttu-id="5a4ec-159">Példa – **belső URL-cím "http://bing.com/" érvénytelen, mert már használatban van**</span><span class="sxs-lookup"><span data-stu-id="5a4ec-159">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="5a4ec-160">**Másolja át a hiba** – kattintson a **másolás ikon** jobb oldalán a **hiba másolása** szövegmező megosztani a támogatási szolgálathoz vagy a termék csoport mérnöke értesítés részleteinek másolása</span><span class="sxs-lookup"><span data-stu-id="5a4ec-160">**Copy error** – Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

   *  <span data-ttu-id="5a4ec-161">Példa```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="5a4ec-161">Example ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a4ec-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a4ec-162">Next steps</span></span>
[<span data-ttu-id="5a4ec-163">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="5a4ec-163">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)



