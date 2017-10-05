---
title: "Alkalmazások, engedélyek és jóváhagyás az Azure Active Directoryban | Microsoft Docs"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Így közös identitást biztosíthat az Azure AD-vel integrált Office 365-, Azure- és SaaS-alkalmazásokhoz."
keywords: "az Azure AD bemutatása, alkalmazások, mi az Azure AD Connect, az Active Directory telepítése"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 6f6baf5e1538fb280a899065c64ca5688473c04a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a><span data-ttu-id="15374-105">Alkalmazások, engedélyek és jóváhagyás az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="15374-105">Apps, permissions, and consent in Azure Active Directory</span></span>
<span data-ttu-id="15374-106">Az Azure Active Directoryban hozzáadhat alkalmazásokat a címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="15374-106">Within Azure Active Directory, you can add applications to your directory.</span></span>  <span data-ttu-id="15374-107">Az alkalmazások az alkalmazás típusától függően eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="15374-107">The applications can vary depending on the type of application.</span></span>  <span data-ttu-id="15374-108">Az alkalmazások megtekintéséhez a klasszikus portálon válasszon ki egy címtárat, és válasszon alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="15374-108">To view applications in the classic portal, select a directory and choose applications.</span></span>

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> <span data-ttu-id="15374-109">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="15374-109">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span>

## <a name="types-of-apps"></a><span data-ttu-id="15374-110">Alkalmazástípusok</span><span class="sxs-lookup"><span data-stu-id="15374-110">Types of apps</span></span>

1. <span data-ttu-id="15374-111">**Egybérlős alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="15374-111">**Single-tenant apps**</span></span> </br>
    - <span data-ttu-id="15374-112">**Egybérlős alkalmazások** – Gyakran üzletági (line-of-business, LOB) alkalmazásokként utalnak rájuk.</span><span class="sxs-lookup"><span data-stu-id="15374-112">**Single-tenant apps** - Often referred to as line-of-business (LOB) apps.</span></span> <span data-ttu-id="15374-113">Ilyen alkalmazásról akkor beszélünk, amikor egy szervezet saját alkalmazást fejleszt, és azt szeretné, hogy a felhasználói be tudjanak rá jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="15374-113">This is the case where someone within your organization develops their own app, and would like users in the organization to be able to sign in to the app.</span></span>
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - <span data-ttu-id="15374-114">**Alkalmazásproxy-alkalmazások** – Amikor közzétesz egy helyszíni alkalmazást az Azure AD-alkalmazásproxyval, egy egybérlős alkalmazást regisztrál a bérlőben (az alkalmazásproxy-szolgáltatás mellett).</span><span class="sxs-lookup"><span data-stu-id="15374-114">**App Proxy apps** - When you expose an on-prem application with Azure AD App Proxy, a single-tenant app is registered in your tenant (in addition to the App Proxy service).</span></span> <span data-ttu-id="15374-115">Ez az alkalmazás képviseli a helyszíni alkalmazást minden felhőbeli interakció (például hitelesítés) során.</span><span class="sxs-lookup"><span data-stu-id="15374-115">This app is what represents your on-prem application for all cloud interactions (for example, authentication).</span></span> <span data-ttu-id="15374-116">(Az alkalmazásproxy használatához Basic vagy magasabb szintű Azure AD-licenc szükséges.)</span><span class="sxs-lookup"><span data-stu-id="15374-116">(App Proxy requires Azure AD Basic or higher.)</span></span>


2. <span data-ttu-id="15374-117">**Több-bérlős alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="15374-117">**Multi-tenant apps**</span></span>
    - <span data-ttu-id="15374-118">**Mások által jóváhagyott több-bérlős alkalmazások** – hasonlók, mint „a szervezet által fejlesztett egybérlős alkalmazások”.</span><span class="sxs-lookup"><span data-stu-id="15374-118">**Multi-tenant apps which others can consent to** - similar to “single-tenant apps that your organization develops”.</span></span> <span data-ttu-id="15374-119">A fő különbség (az alkalmazás belső logikáján kívül) az, hogy más bérlők felhasználói is jóváhagyhatják az alkalmazást, és be is jelentkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="15374-119">The main difference (besides the logic in the app itself) is that users from other tenants can also consent to and sign in to the app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - <span data-ttu-id="15374-120">**Mások által fejlesztett több-bérlős alkalmazások, amelyeket a Contoso jóváhagyhat**.</span><span class="sxs-lookup"><span data-stu-id="15374-120">**Multi-tenant apps others develop, which Contoso can consent to**.</span></span> <span data-ttu-id="15374-121">(Röviden „jóváhagyott alkalmazások”.) Ezen alkalmazások a „szervezet által fejlesztett több-bérlős alkalmazások” ellentettjei.</span><span class="sxs-lookup"><span data-stu-id="15374-121">(Or “consented apps”, for short.) This is the flip side of “multi-tenant apps your organization develops”.</span></span> <span data-ttu-id="15374-122">Ha egy másik szervezet fejleszt egy több-bérlős alkalmazást, az Ön üzleti szervezetének felhasználói jóváhagyhatják az alkalmazást, és be is jelentkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="15374-122">When another organization develops a multi-tenant app, users of your organization can consent to the app and sign in to it.</span></span>
    - <span data-ttu-id="15374-123">**Belső Microsoft-alkalmazások** – A Microsoft szolgáltatásait képviselő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="15374-123">**Microsoft first-party apps** - Apps that represent Microsoft services.</span></span> <span data-ttu-id="15374-124">Az alkalmazások használatát a szolgáltatásra való regisztrációval lehet jóváhagyni.</span><span class="sxs-lookup"><span data-stu-id="15374-124">Consent is driven by the fact that you sign up for the service.</span></span> <span data-ttu-id="15374-125">Néha különleges felhasználói felület és logika jellemez bizonyos belső alkalmazásokat, amelyeknek a hatása az alkalmazások hozzáférésére vonatkozó szabályzatokban is érvényesül.</span><span class="sxs-lookup"><span data-stu-id="15374-125">There is sometimes special UX and logic for certain first-party apps that is often used when establishing policies around access to the app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - <span data-ttu-id="15374-126">**Előre integrált alkalmazások** – Az Azure AD alkalmazáskatalógusában elérhető alkalmazások, amelyeket hozzáadhat a címtárhoz, hogy egyszeri bejelentkezést (és esetenként jogosultságkiosztást) biztosíthasson a népszerű SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="15374-126">**Pre-integrated apps** - Apps available in the Azure AD App Gallery, which you can add to your directory to provide single sign-on (and in some cases, provisioning) to popular SaaS apps.</span></span>
    - <span data-ttu-id="15374-127">**Azure AD egyszeri bejelentkezés**: „Valódi” egyszeri bejelentkezés (SSO) olyan alkalmazások számára, amelyek integrálhatók az Azure AD-vel egy támogatott bejelentkezési protokollon, például az SAML 2.0-n vagy az OpenID Connecten keresztül.</span><span class="sxs-lookup"><span data-stu-id="15374-127">**Azure AD single sign-on**: “Real” SSO, for apps that can be integrated with Azure AD, through a supported sign-in protocol such as SAML 2.0 or OpenID Connect.</span></span> <span data-ttu-id="15374-128">A varázsló végigvezeti ennek beállításán.</span><span class="sxs-lookup"><span data-stu-id="15374-128">The wizard walks you through setting it up.</span></span>
    - <span data-ttu-id="15374-129">**Jelszavas egyszeri bejelentkezés**: Az Azure AD biztonságosan tárolja a felhasználó alkalmazásra vonatkozó hitelesítő adatait, amelyeket az Azure AD App Access böngészőbővítmény ad meg a bejelentkezési űrlapon.</span><span class="sxs-lookup"><span data-stu-id="15374-129">**Password single sign-on**: Azure AD securely stores the user’s credentials for the app, and the credentials are “injected” into the sign-in form by the Azure AD App Access browser extension.</span></span> <span data-ttu-id="15374-130">Ez a bejelentkezési módszer „jelszótárolásként” is ismert.</span><span class="sxs-lookup"><span data-stu-id="15374-130">Also known as “password vaulting”.</span></span>

## <a name="permissions"></a><span data-ttu-id="15374-131">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="15374-131">Permissions</span></span>

<span data-ttu-id="15374-132">Egy alkalmazás regisztrálásakor az alkalmazásregisztrációt végrehajtó felhasználó (vagyis a fejlesztő) határozza meg azon engedélyeket és erőforrásokat, amelyeket az alkalmazásnak el kell érnie.</span><span class="sxs-lookup"><span data-stu-id="15374-132">When an app is registered, the user performing the app registration (that is, the developer) defines which permissions the app needs access to, and which resources.</span></span> <span data-ttu-id="15374-133">(Maguk az erőforrások más alkalmazásokként vannak meghatározva.) Egy levélolvasó alkalmazás fejlesztője megadhatja például, hogy az alkalmazásnak a „Postaládák elérése bejelentkezett felhasználóként” engedélyre van szüksége az „Office 365 Exchange Online” erőforrásban:</span><span class="sxs-lookup"><span data-stu-id="15374-133">(The resources are, themselves, defined as other apps.) For example, someone building a mail reader app, would state that their app requires the “Access mailboxes as the signed-in user” permission in the “Office 365 Exchange Online” resource:</span></span>
    
![](media/active-directory-apps-permissions-consent/apps6.png)

<span data-ttu-id="15374-134">Ahhoz, hogy egy alkalmazás (az ügyfél) adott engedélyt igényelhessen egy másik alkalmazástól (az erőforrástól), az erőforrás-alkalmazás fejlesztője határozza meg a meglévő engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="15374-134">In order for one app (the client) to request a certain permission from another app (the resource), the developer of the resource app defines the permissions that exist.</span></span> <span data-ttu-id="15374-135">A példánkban a Microsoft, az „Office 365 Exchange Online” erőforrás-alkalmazás tulajdonosa meghatározta a „Postaládák elérése bejelentkezett felhasználóként” nevű engedélyt.</span><span class="sxs-lookup"><span data-stu-id="15374-135">In our example, Microsoft, the owner of the “Office 365 Exchange Online” resource app, have defined a permission named “Access mailboxes as the signed-in user”.</span></span>

<span data-ttu-id="15374-136">Az engedélyek meghatározásakor az alkalmazás fejlesztőjének meg kell adnia, hogy az engedélyt jóvá lehet-e hagyni, vagy pedig rendszergazdai jóváhagyásra van-e szükség.</span><span class="sxs-lookup"><span data-stu-id="15374-136">When defining permissions, the app developer must define if the permission can be consented to, or if it requires admin consent.</span></span> <span data-ttu-id="15374-137">A fejlesztők így lehetővé tehetik a felhasználók számára alacsony szintű engedélyeket kérő alkalmazásaik jóváhagyását, magasabb szintű engedélyek esetén pedig rendszergazdai jóváhagyást követelhetnek meg.</span><span class="sxs-lookup"><span data-stu-id="15374-137">This allows developers to allow users to consent on their own to apps requesting only low-sensitivity permissions, but require admins to consent to more sensitive permissions.</span></span> <span data-ttu-id="15374-138">Az „Azure Active Directory” erőforrás-alkalmazás például úgy lett meghatározva, hogy a felhasználók maguk is jóváhagyhatják az alkalmazásokat, ha csak korlátozott írásvédett engedélyeket kérnek.</span><span class="sxs-lookup"><span data-stu-id="15374-138">For example, the “Azure Active Directory” resource app, has been defined, so users can consent to apps, requesting limited read-only permissions.</span></span>  <span data-ttu-id="15374-139">A teljes olvasási és minden írási engedélyhez azonban rendszergazdai jóváhagyás szükséges.</span><span class="sxs-lookup"><span data-stu-id="15374-139">However, admin consent is required for full read permissions, and all write permissions.</span></span>

<span data-ttu-id="15374-140">Mivel a natív ügyfelek nincsenek hitelesítve, a natív ügyfélalkalmazásként meghatározott alkalmazások csak delegált engedélyeket kérhetnek.</span><span class="sxs-lookup"><span data-stu-id="15374-140">Because native clients are not authenticated, an app defined as a native client app can only request delegated permissions.</span></span> <span data-ttu-id="15374-141">Ez azt jelenti, hogy a tokenek beszerzését egy tényleges felhasználónak kell intéznie.</span><span class="sxs-lookup"><span data-stu-id="15374-141">This means that there must always be an actual user involved when obtaining a token.</span></span> <span data-ttu-id="15374-142">A webes alkalmazásoknak és webes API-knak (bizalmas ügyfeleknek) mindig hitelesíteniük kell az Azure AD-vel, amikor hozzáférési tokent igényelnek.</span><span class="sxs-lookup"><span data-stu-id="15374-142">Web apps and web APIs (confidential clients), must always authenticate with Azure AD when getting an access token.</span></span> <span data-ttu-id="15374-143">Ezek az ügyfelek csak az alkalmazásra vonatkozó engedélyeket is kérhetnek.</span><span class="sxs-lookup"><span data-stu-id="15374-143">Meaning they also have the possibility of requesting app-only permissions.</span></span> <span data-ttu-id="15374-144">Ilyen eset például, amikor egy háttérszolgáltatásnak hitelesítenie kell magát egy másik háttérszolgáltatás előtt.</span><span class="sxs-lookup"><span data-stu-id="15374-144">For example, if one back-end service needs to authenticate to another back-end service.</span></span> <span data-ttu-id="15374-145">A csak az alkalmazásra vonatkozó engedélyeket kérő alkalmazások mindig rendszergazdai jóváhagyást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="15374-145">Applications requesting app-only permissions always require administrator consent.</span></span>

<span data-ttu-id="15374-146">Összefoglalva:</span><span class="sxs-lookup"><span data-stu-id="15374-146">Summarizing:</span></span>



- <span data-ttu-id="15374-147">Az alkalmazás (ügyfél) határozza meg azon engedélyeket, amelyekre szüksége van a többi alkalmazáshoz (erőforráshoz).</span><span class="sxs-lookup"><span data-stu-id="15374-147">An app (client) states the permissions it needs for other apps (resources).</span></span>
- <span data-ttu-id="15374-148">Az alkalmazás (erőforrás) határozza meg, hogy milyen engedélyek tehetők közzé a többi alkalmazás (ügyfél) számára.</span><span class="sxs-lookup"><span data-stu-id="15374-148">An app (resource) states what permissions are exposed to other apps (clients).</span></span>
- <span data-ttu-id="15374-149">Az engedély lehet csak az alkalmazásra vonatkozó vagy delegált engedély is.</span><span class="sxs-lookup"><span data-stu-id="15374-149">A permission can be an app-only permission, or a delegated permission.</span></span>
- <span data-ttu-id="15374-150">A delegált engedély megjelölhető „felhasználói jóváhagyást engedélyező” vagy „rendszergazdai jóváhagyást igénylő” engedélyként.</span><span class="sxs-lookup"><span data-stu-id="15374-150">A delegated permission can be marked as “allows user consent”, or “requires admin consent”.</span></span>
- <span data-ttu-id="15374-151">Egy alkalmazás működhet ügyfélként (ha kijelenti, hogy engedélyekre van szüksége egy erőforráshoz), erőforrásként (ha megnevezi az általa közzétett engedélyeket) vagy mindkettőként.</span><span class="sxs-lookup"><span data-stu-id="15374-151">An app can behave as a client (by declaring that it needs permissions to a resource), as a resource (by declaring which permissions it exposes), or as both.</span></span>

## <a name="controls"></a><span data-ttu-id="15374-152">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="15374-152">Controls</span></span>

<span data-ttu-id="15374-153">Alább az ilyen működési módokhoz elérhető különböző rendszergazdai vezérlők listája látható.</span><span class="sxs-lookup"><span data-stu-id="15374-153">The following is a list of the different admin controls available for all this behavior.</span></span> <span data-ttu-id="15374-154">A rendszergazdai vezérlők a klasszikus portálon a címtár konfigurálási szakaszánál érhetők el.</span><span class="sxs-lookup"><span data-stu-id="15374-154">The admin controls can be accessed in the classic portal from configure under the directory.</span></span>

![](media/active-directory-apps-permissions-consent/apps7.png)

<span data-ttu-id="15374-155">Az Azure Portalon a vezérlőket a **Felhasználói beállítások** **Kezelés** menüpontjában találja meg.</span><span class="sxs-lookup"><span data-stu-id="15374-155">In the Azure portal, under **manage**, **user settings**.</span></span>

![](media/active-directory-apps-permissions-consent/apps11.png)



- <span data-ttu-id="15374-156">Szabályozhatja, hogy a felhasználók jóváhagyhatnak-e alkalmazásokat:</span><span class="sxs-lookup"><span data-stu-id="15374-156">You can control whether users can consent to apps:</span></span>

<span data-ttu-id="15374-157">A klasszikus portálon válassza a **Users may give applications permissions to access their data**
![](media/active-directory-apps-permissions-consent/apps8.png) (A felhasználók engedélyt adhatnak az alkalmazásoknak az adataik elérésére) elemet.</span><span class="sxs-lookup"><span data-stu-id="15374-157">In the classic portal, select **Users may give applications permissions to access their data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span></span>

<span data-ttu-id="15374-158">Az Azure Portalon válassza **A felhasználók engedélyezhetik alkalmazások számára az adataikhoz való hozzáférést** elemet.</span><span class="sxs-lookup"><span data-stu-id="15374-158">In the Azure portal, select **users can allow apps to access their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps12.png)



- <span data-ttu-id="15374-159">Szabályozhatja, hogy a felhasználók regisztrálhatják-e saját egybérlős LOB-alkalmazásaikat. A klasszikus portálon válassza a **Users may add integrated applications**
![](media/active-directory-apps-permissions-consent/apps9.png) (A felhasználók hozzáadhatnak integrált alkalmazásokat) elemet.</span><span class="sxs-lookup"><span data-stu-id="15374-159">You can control whether users can register their own single-tenant LOB apps: In the classic portal select **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span></span>

<span data-ttu-id="15374-160">Az Azure Portalon válassza **A felhasználók engedélyezhetik alkalmazások számára az adataikhoz való hozzáférést** elemet.</span><span class="sxs-lookup"><span data-stu-id="15374-160">In the Azure portal, select **users can allow apps to access their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
><span data-ttu-id="15374-161">A regisztrálható alkalmazások köre még akkor is korlátozva van, ha engedélyezi a felhasználók számára az egybérlős LOB-alkalmazások regisztrálását.</span><span class="sxs-lookup"><span data-stu-id="15374-161">Even if you do allow users to register single-tenant LOB apps, there are limits to what can be registered.</span></span>  
><span data-ttu-id="15374-162">A korlátozások vonatkoznak például olyan fejlesztőkre, akik nem címtár-rendszergazdák.</span><span class="sxs-lookup"><span data-stu-id="15374-162">For example, developers who are not directory admins.</span></span>
>
>- <span data-ttu-id="15374-163">A felhasználók nem alakíthatnak át egy egybérlős alkalmazást több-bérlőssé.</span><span class="sxs-lookup"><span data-stu-id="15374-163">Users cannot make a single-tenant app a multi-tenant app.</span></span>
>- <span data-ttu-id="15374-164">Egybérlős LOB-alkalmazások regisztrálásakor a felhasználók nem kérhetnek csak az alkalmazásra vonatkozó engedélyeket más alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="15374-164">When registering single-tenant LOB apps, users cannot request app-only permissions to other apps.</span></span>
>- <span data-ttu-id="15374-165">Egybérlős LOB-alkalmazások regisztrálásakor a felhasználók nem kérhetnek delegált engedélyeket más alkalmazásokhoz, ha az engedélyekhez rendszergazdai jóváhagyásra van szükség.</span><span class="sxs-lookup"><span data-stu-id="15374-165">When registering single-tenant LOB apps, users cannot request delegated permissions to other apps if those permissions require admin consent.</span></span>
>- <span data-ttu-id="15374-166">A felhasználók nem módosíthatnak olyan alkalmazásokat, amelyeknek nem a tulajdonosaik.</span><span class="sxs-lookup"><span data-stu-id="15374-166">Users cannot make changes to apps that they are not owners of.</span></span>



- <span data-ttu-id="15374-167">Szabályozhatja, hogy a felhasználók hozzáadhatnak-e előre integrált, jelszavas egyszeri bejelentkezést (más néven „jelszótárolást”) használó alkalmazásokat. ![](media/active-directory-apps-permissions-consent/apps10.png)</span><span class="sxs-lookup"><span data-stu-id="15374-167">You can control whether users can themselves add pre-integrated apps that use password SSO (aka “password vaulting”) ![](media/active-directory-apps-permissions-consent/apps10.png)</span></span>



- <span data-ttu-id="15374-168">Szabályozhatja, hogy az alkalmazások milyen feltételek esetén férhetők hozzá (azaz feltételes hozzáférést is megadhat).</span><span class="sxs-lookup"><span data-stu-id="15374-168">You can control under which conditions applications can be accessed (that is, conditional access).</span></span> <span data-ttu-id="15374-169">Ügyeljen arra, hogy ez az ügyfélalkalmazásra és az erőforrás-alkalmazásra egyaránt vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="15374-169">Be aware this applies both to the client app and to the resource app.</span></span> <span data-ttu-id="15374-170">Tegyük fel például, hogy olyan feltételes hozzáférési szabályzatot állít be, amely alapján az „Office 365 Exchange Online” alkalmazás csak a szabályzatnak megfelelő gépekről érhető el.</span><span class="sxs-lookup"><span data-stu-id="15374-170">So, say you set a conditional access policy that says that the “Office 365 Exchange Online” app can only be accessed from machines, which are compliant.</span></span>  <span data-ttu-id="15374-171">A szabályzat akkor is érvényesül, ha egy felhasználó olyan ügyfélalkalmazást próbál használni, amely engedélyeket kért az Exchange Online-hoz.</span><span class="sxs-lookup"><span data-stu-id="15374-171">This policy will also kick in when a user attempts to use a client app which requests permissions to Exchange Online.</span></span>



- <span data-ttu-id="15374-172">Láthatja, hogy mely alkalmazásokat hagytak jóvá, és mely alkalmazásokat használják éppen.</span><span class="sxs-lookup"><span data-stu-id="15374-172">You have visibility into which apps have been consented to and which ones are being used.</span></span>

1.  <span data-ttu-id="15374-173">Amikor egy felhasználó jóváhagy egy alkalmazást, a rendszer létrehoz egy ServicePrincipal objektumot a bérlőben.</span><span class="sxs-lookup"><span data-stu-id="15374-173">When a user consents to an app, a ServicePrincipal object is created in the tenant.</span></span> <span data-ttu-id="15374-174">A ServicePrincipal létrehozásának ténye megjelenik a naplózási jelentésben.</span><span class="sxs-lookup"><span data-stu-id="15374-174">ServicePrincipal creation is included in the audit report.</span></span>
2.  <span data-ttu-id="15374-175">A felhasználói bejelentkezési tevékenységről szóló jelentések tájékoztatják arról, hogy a felhasználó mely alkalmazásba jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="15374-175">User sign-in activity reports tell you which app the user is signing in to.</span></span> 

## <a name="example"></a><span data-ttu-id="15374-176">Példa</span><span class="sxs-lookup"><span data-stu-id="15374-176">Example</span></span>

<span data-ttu-id="15374-177">Példaként a „FabrikamMail for Office 365” alkalmazást használjuk. Ön észleli, hogy a bérlő felhasználói bejelentkeznek az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="15374-177">As an example, let’s take the “FabrikamMail for Office 365” app, which you’ve noticed users in your tenant are signing in to.</span></span> <span data-ttu-id="15374-178">A „FabrikamMail” egy levélolvasó alkalmazás Android rendszerhez, amelyet a „Fabrikam, Inc.” tett közzé.</span><span class="sxs-lookup"><span data-stu-id="15374-178">“FabrikamMail” is a mail reader app for Android, published by “Fabrikam, Inc.”.</span></span> <span data-ttu-id="15374-179">Az alkalmazás így a „mások által fejlesztett több-bérlős alkalmazások, amelyeket a Contoso jóváhagyhat” típusba tartozik.</span><span class="sxs-lookup"><span data-stu-id="15374-179">This falls into the “multi-tenant apps other develop, which Contoso can consent to”.</span></span>

<span data-ttu-id="15374-180">Ha a felhasználók számára engedélyezett a jóváhagyás, a rendszer a jóváhagyásukat kéri az első bejelentkezéskor: ![](media/active-directory-apps-permissions-consent/apps14.png)</span><span class="sxs-lookup"><span data-stu-id="15374-180">If users are allowed to consent, they get a consent prompt the first time they sign in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span></span>

<span data-ttu-id="15374-181">Az „Access your mailboxes” (Postaládák hozzáférése) az „Office 365 Exchange Online” (vagyis az Exchange) által közzétett „Postaládák elérése bejelentkezett felhasználóként” engedély felhasználóknak megjelenő jóváhagyási karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="15374-181">“Access your mailboxes” is the user-facing consent string for the “Access mailboxes as the signed-in user” permission exposed by “Office 365 Exchange Online” (that is, Exchange).</span></span>

<span data-ttu-id="15374-182">Az engedélyeket az Exchange (az erőforrás) ServicePrincipal objektumának ellenőrzésével tekintheti meg. Az objektum hozzáadására az Office 365-re való regisztráláskor került sor.</span><span class="sxs-lookup"><span data-stu-id="15374-182">You can see the permissions by looking up the ServicePrincipal object for Exchange (the resource), which was added when you signed up for Office 365.</span></span> <span data-ttu-id="15374-183">A ServicePrincipal objektumot tekintheti az alkalmazás különböző beállítások és konfigurációk rögzítésére szolgáló egyik „példányának” a bérlőben.</span><span class="sxs-lookup"><span data-stu-id="15374-183">You can think of the ServicePrincipal object of an “instance” of the app in your tenant, which is used for recording different options and configurations.</span></span>  <span data-ttu-id="15374-184">Az objektumot a `Get-AzureADServicePrincipal` paranccsal tekintheti meg a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="15374-184">You can see this by using the `Get-AzureADServicePrincipal` in PowerShell.</span></span>

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

<span data-ttu-id="15374-185">Ha a felhasználó az „Accept” (Elfogadás) gombra kattint, a rendszer kezdeményezi a jóváhagyást.</span><span class="sxs-lookup"><span data-stu-id="15374-185">Consent is initiated when the user clicks “Accept”.</span></span> <span data-ttu-id="15374-186">Először is létrehozza a „FabrikamMail for Office 365” ServicePrincipal objektumát a bérlőben.</span><span class="sxs-lookup"><span data-stu-id="15374-186">First, a ServicePrincipal object for “FabrikamMail for Office 365” is created in the tenant.</span></span> <span data-ttu-id="15374-187">A ServicePrincipal nagyjából így néz ki:</span><span class="sxs-lookup"><span data-stu-id="15374-187">The ServicePrincipal looks something like this:</span></span>

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

<span data-ttu-id="15374-188">Az alkalmazás jóváhagyása egy Oauth2PermissionGrant hivatkozást hoz létre az alábbiak között:</span><span class="sxs-lookup"><span data-stu-id="15374-188">Consenting to an app creates an Oauth2PermissionGrant link between the following:</span></span>
  
- <span data-ttu-id="15374-189">a felhasználói objektum;</span><span class="sxs-lookup"><span data-stu-id="15374-189">the user object</span></span>
- <span data-ttu-id="15374-190">az ügyfélalkalmazás ServicePrincipalName (SPN) eleme;</span><span class="sxs-lookup"><span data-stu-id="15374-190">the client apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="15374-191">az erőforrás-alkalmazás ServicePrincipalName (SPN) eleme;</span><span class="sxs-lookup"><span data-stu-id="15374-191">the resource apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="15374-192">az erőforrás-alkalmazásban található engedélyek.</span><span class="sxs-lookup"><span data-stu-id="15374-192">permissions in the resource app.</span></span>  

<span data-ttu-id="15374-193">A FabrikamMail esetében ez nagyjából a következők szerint néz ki:</span><span class="sxs-lookup"><span data-stu-id="15374-193">In the case of FabrikamMail, it looks something like this:</span></span>

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

<span data-ttu-id="15374-194">(A **ClientId** a FabrikamMail ServicePrincipal objektumának azonosítója (amelyet a rendszer éppen létrehozott), a **PrincipalId** a felhasználói objektum azonosítója (a jóváhagyást adó felhasználóé), a **ResourceId** az Exchange ServicePrincipal objektumának azonosítója, a Scope pedig az Exchange azon engedélye, amelyet a felhasználó jóváhagy.)</span><span class="sxs-lookup"><span data-stu-id="15374-194">(**ClientId** is FabrikamMail’s service principal object ID (the one that just got created), **PrincipalId** is the user object ID (of the user who consented), **ResourceId** is Exchange’s service principal object ID, Scope is the permission in Exchange that was consented to).</span></span>

<span data-ttu-id="15374-195">Ha a felhasználók nem adhatnak maguktól jóváhagyást, egy olyan képernyő jelenik meg, amely közli, hogy engedélyre van szükség.</span><span class="sxs-lookup"><span data-stu-id="15374-195">If users are not allowed to consent, they will see a screen that says that permission is required.</span></span>

