---
title: "Alkalmazások, engedélyek és jóváhagyás az Azure Active Directoryban | Microsoft Docs"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazásokhoz közös identitás tooprovide."
keywords: "Bevezetés tooAzure AD, alkalmazások, mi az az Azure AD Connect, az active directory telepítése"
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
ms.openlocfilehash: af0c2669199736fdb41e85876a7e3a7064e80770
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a><span data-ttu-id="9bfa5-105">Alkalmazások, engedélyek és jóváhagyás az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="9bfa5-105">Apps, permissions, and consent in Azure Active Directory</span></span>
<span data-ttu-id="9bfa5-106">Az Azure Active Directoryban hozzáadhat alkalmazásokat tooyour könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-106">Within Azure Active Directory, you can add applications tooyour directory.</span></span>  <span data-ttu-id="9bfa5-107">hello alkalmazások alkalmazás hello típusától függően változhat.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-107">hello applications can vary depending on hello type of application.</span></span>  <span data-ttu-id="9bfa5-108">klasszikus portál hello tooview alkalmazások jelöljön ki egy könyvtárat, és alkalmazások kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-108">tooview applications in hello classic portal, select a directory and choose applications.</span></span>

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> <span data-ttu-id="9bfa5-109">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-109">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span>

## <a name="types-of-apps"></a><span data-ttu-id="9bfa5-110">Alkalmazástípusok</span><span class="sxs-lookup"><span data-stu-id="9bfa5-110">Types of apps</span></span>

1. <span data-ttu-id="9bfa5-111">**Egybérlős alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="9bfa5-111">**Single-tenant apps**</span></span> </br>
    - <span data-ttu-id="9bfa5-112">**Single-bérlői alkalmazások** -nevezik tooas üzletági (LOB) alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-112">**Single-tenant apps** - Often referred tooas line-of-business (LOB) apps.</span></span> <span data-ttu-id="9bfa5-113">Ez az hello esetben, ha valaki a szervezeten belüli házon belül fejlesztett alkalmazásokra saját alkalmazás, kíván tenni a felhasználók a hello szervezet toobe képes toosign toohello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-113">This is hello case where someone within your organization develops their own app, and would like users in hello organization toobe able toosign in toohello app.</span></span>
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - <span data-ttu-id="9bfa5-114">**Alkalmazás Proxy alkalmazások** – Ha az Azure AD alkalmazás Proxy, helyszíni alkalmazás teszi ki a single-bérlői alkalmazások, a bérlő által (hozzáadása toohello App proxyszolgáltatás) regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-114">**App Proxy apps** - When you expose an on-prem application with Azure AD App Proxy, a single-tenant app is registered in your tenant (in addition toohello App Proxy service).</span></span> <span data-ttu-id="9bfa5-115">Ez az alkalmazás képviseli a helyszíni alkalmazást minden felhőbeli interakció (például hitelesítés) során.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-115">This app is what represents your on-prem application for all cloud interactions (for example, authentication).</span></span> <span data-ttu-id="9bfa5-116">(Az alkalmazásproxy használatához Basic vagy magasabb szintű Azure AD-licenc szükséges.)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-116">(App Proxy requires Azure AD Basic or higher.)</span></span>


2. <span data-ttu-id="9bfa5-117">**Több-bérlős alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="9bfa5-117">**Multi-tenant apps**</span></span>
    - <span data-ttu-id="9bfa5-118">**Több-bérlős alkalmazásokhoz, amelyek mások is** - hasonló túl "single-bérlői alkalmazások, a szervezet házon belül fejlesztett alkalmazásokra".</span><span class="sxs-lookup"><span data-stu-id="9bfa5-118">**Multi-tenant apps which others can consent to** - similar too“single-tenant apps that your organization develops”.</span></span> <span data-ttu-id="9bfa5-119">hello fő (mellett hello alkalmazás maga a logikai hello) különbség, hogy a felhasználók a többi bérlőtől is is hozzájárulás tooand bejelentkezési toohello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-119">hello main difference (besides hello logic in hello app itself) is that users from other tenants can also consent tooand sign in toohello app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - <span data-ttu-id="9bfa5-120">**Mások által fejlesztett több-bérlős alkalmazások, amelyeket a Contoso jóváhagyhat**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-120">**Multi-tenant apps others develop, which Contoso can consent to**.</span></span> <span data-ttu-id="9bfa5-121">(Röviden „jóváhagyott alkalmazások”.) Ez a "több-bérlős alkalmazások a szervezet házon belül fejlesztett alkalmazásokra" hello tükrözés oldalán.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-121">(Or “consented apps”, for short.) This is hello flip side of “multi-tenant apps your organization develops”.</span></span> <span data-ttu-id="9bfa5-122">Ha egy másik szervezet egy több-bérlős alkalmazást fejleszt, a szervezet felhasználói hozzájárulás toohello alkalmazás, és tooit bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-122">When another organization develops a multi-tenant app, users of your organization can consent toohello app and sign in tooit.</span></span>
    - <span data-ttu-id="9bfa5-123">**Belső Microsoft-alkalmazások** – A Microsoft szolgáltatásait képviselő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-123">**Microsoft first-party apps** - Apps that represent Microsoft services.</span></span> <span data-ttu-id="9bfa5-124">Hello tényt, hogy regisztrál hello szolgáltatás célja a hozzájárulásukat adják.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-124">Consent is driven by hello fact that you sign up for hello service.</span></span> <span data-ttu-id="9bfa5-125">Nincs néha különleges UX és egyes belső alkalmazások logikát, gyakran használt szabályzatok hozzáférés toohello alkalmazások körül kialakítása során.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-125">There is sometimes special UX and logic for certain first-party apps that is often used when establishing policies around access toohello app.</span></span></br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - <span data-ttu-id="9bfa5-126">**Előzetesen beépített alkalmazások** -hello Azure AD-Alkalmazásgyűjtemény, amely is hozzáadhat az elérhető alkalmazások tooyour directory tooprovide egyszeri bejelentkezés (és egyes esetekben kiépítése) toopopular SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-126">**Pre-integrated apps** - Apps available in hello Azure AD App Gallery, which you can add tooyour directory tooprovide single sign-on (and in some cases, provisioning) toopopular SaaS apps.</span></span>
    - <span data-ttu-id="9bfa5-127">**Azure AD egyszeri bejelentkezés**: „Valódi” egyszeri bejelentkezés (SSO) olyan alkalmazások számára, amelyek integrálhatók az Azure AD-vel egy támogatott bejelentkezési protokollon, például az SAML 2.0-n vagy az OpenID Connecten keresztül.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-127">**Azure AD single sign-on**: “Real” SSO, for apps that can be integrated with Azure AD, through a supported sign-in protocol such as SAML 2.0 or OpenID Connect.</span></span> <span data-ttu-id="9bfa5-128">hello varázsló bemutatja, hogyan állítsa be azt.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-128">hello wizard walks you through setting it up.</span></span>
    - <span data-ttu-id="9bfa5-129">**Jelszó az egyszeri bejelentkezés**: az Azure AD biztonságosan tárolja a hello app hello felhasználói hitelesítő adatokat, és hello hitelesítő adatok vannak "be a nézetmodellbe" hello bejelentkezési űrlap hello Azure AD alkalmazás-hozzáférés bővítmény által.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-129">**Password single sign-on**: Azure AD securely stores hello user’s credentials for hello app, and hello credentials are “injected” into hello sign-in form by hello Azure AD App Access browser extension.</span></span> <span data-ttu-id="9bfa5-130">Ez a bejelentkezési módszer „jelszótárolásként” is ismert.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-130">Also known as “password vaulting”.</span></span>

## <a name="permissions"></a><span data-ttu-id="9bfa5-131">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="9bfa5-131">Permissions</span></span>

<span data-ttu-id="9bfa5-132">Az alkalmazás regisztrálásakor hello felhasználói hello app regisztrációs (Ez azt jelenti, hogy hello fejlesztői) végrehajtása határozza meg, melyik engedélyek hello alkalmazásnak kell elérnie, és mely erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-132">When an app is registered, hello user performing hello app registration (that is, hello developer) defines which permissions hello app needs access to, and which resources.</span></span> <span data-ttu-id="9bfa5-133">(hello erőforrásokat, maguk is, más alkalmazások meghatározva.) Például valaki a mail olvasó alkalmazás elkészítése volna állapotban, hogy az alkalmazás engedélyre van szüksége, hello "Postaládák hello bejelentkezett felhasználó nevében" hello "Office 365 Exchange online-hoz" erőforrás a:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-133">(hello resources are, themselves, defined as other apps.) For example, someone building a mail reader app, would state that their app requires hello “Access mailboxes as hello signed-in user” permission in hello “Office 365 Exchange Online” resource:</span></span>
    
![](media/active-directory-apps-permissions-consent/apps6.png)

<span data-ttu-id="9bfa5-134">Ahhoz, hogy egy alkalmazás (hello ügyfél) toorequest (hello erőforrás) egy másik alkalmazás egy bizonyos engedélyt hello fejlesztői hello erőforrás alkalmazás létező hello engedélyek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-134">In order for one app (hello client) toorequest a certain permission from another app (hello resource), hello developer of hello resource app defines hello permissions that exist.</span></span> <span data-ttu-id="9bfa5-135">A példánkban a Microsoft hello tulajdonos hello "Office 365 Exchange online-hoz" erőforrás alkalmazás definiált "Postaládák hello bejelentkezett felhasználó nevében" nevű engedély.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-135">In our example, Microsoft, hello owner of hello “Office 365 Exchange Online” resource app, have defined a permission named “Access mailboxes as hello signed-in user”.</span></span>

<span data-ttu-id="9bfa5-136">Engedélyek meghatározásakor hello alkalmazás fejlesztőjének definiálnia kell, ha hello engedéllyel is kell átadni kívánt hozzájárult e, vagy ha a rendszergazda jóváhagyását igényli.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-136">When defining permissions, hello app developer must define if hello permission can be consented to, or if it requires admin consent.</span></span> <span data-ttu-id="9bfa5-137">Ez lehetővé teszi a fejlesztők tooallow felhasználók tooconsent a saját tooapps csak a bizalmas alacsony engedéllyel a kért, de a rendszergazdák tooconsent toomore bizalmas engedély szükséges.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-137">This allows developers tooallow users tooconsent on their own tooapps requesting only low-sensitivity permissions, but require admins tooconsent toomore sensitive permissions.</span></span> <span data-ttu-id="9bfa5-138">Például az erőforrás alkalmazás "Azure Active Directory" hello, definiálva van, így a felhasználó be tudja hozzájárulás egy tooapps, korlátozott csak olvasási engedéllyel a kért.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-138">For example, hello “Azure Active Directory” resource app, has been defined, so users can consent tooapps, requesting limited read-only permissions.</span></span>  <span data-ttu-id="9bfa5-139">A teljes olvasási és minden írási engedélyhez azonban rendszergazdai jóváhagyás szükséges.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-139">However, admin consent is required for full read permissions, and all write permissions.</span></span>

<span data-ttu-id="9bfa5-140">Mivel a natív ügyfelek nincsenek hitelesítve, a natív ügyfélalkalmazásként meghatározott alkalmazások csak delegált engedélyeket kérhetnek.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-140">Because native clients are not authenticated, an app defined as a native client app can only request delegated permissions.</span></span> <span data-ttu-id="9bfa5-141">Ez azt jelenti, hogy a tokenek beszerzését egy tényleges felhasználónak kell intéznie.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-141">This means that there must always be an actual user involved when obtaining a token.</span></span> <span data-ttu-id="9bfa5-142">A webes alkalmazásoknak és webes API-knak (bizalmas ügyfeleknek) mindig hitelesíteniük kell az Azure AD-vel, amikor hozzáférési tokent igényelnek.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-142">Web apps and web APIs (confidential clients), must always authenticate with Azure AD when getting an access token.</span></span> <span data-ttu-id="9bfa5-143">Tehát ezenkívül hello lehetőségét csak alkalmazás engedéllyel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-143">Meaning they also have hello possibility of requesting app-only permissions.</span></span> <span data-ttu-id="9bfa5-144">Ha például egy háttér-szolgáltatás tooauthenticate tooanother háttérszolgáltatásnak szüksége van.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-144">For example, if one back-end service needs tooauthenticate tooanother back-end service.</span></span> <span data-ttu-id="9bfa5-145">A csak az alkalmazásra vonatkozó engedélyeket kérő alkalmazások mindig rendszergazdai jóváhagyást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-145">Applications requesting app-only permissions always require administrator consent.</span></span>

<span data-ttu-id="9bfa5-146">Összefoglalva:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-146">Summarizing:</span></span>



- <span data-ttu-id="9bfa5-147">Egy alkalmazás (ügyfél) szerint hello engedélyek más alkalmazások (erőforrások) szükséges.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-147">An app (client) states hello permissions it needs for other apps (resources).</span></span>
- <span data-ttu-id="9bfa5-148">(Erőforrás) alkalmazás-állapotok az engedélyeit olyan kitett tooother alkalmazások (ügyfelek).</span><span class="sxs-lookup"><span data-stu-id="9bfa5-148">An app (resource) states what permissions are exposed tooother apps (clients).</span></span>
- <span data-ttu-id="9bfa5-149">Az engedély lehet csak az alkalmazásra vonatkozó vagy delegált engedély is.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-149">A permission can be an app-only permission, or a delegated permission.</span></span>
- <span data-ttu-id="9bfa5-150">A delegált engedély megjelölhető „felhasználói jóváhagyást engedélyező” vagy „rendszergazdai jóváhagyást igénylő” engedélyként.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-150">A delegated permission can be marked as “allows user consent”, or “requires admin consent”.</span></span>
- <span data-ttu-id="9bfa5-151">Egy alkalmazás viselkedhet ügyfélként (azáltal, hogy kell-e engedélyekkel tooa erőforrás), egy erőforrást (is deklarálni kell jogosultságokat az érheti el), vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-151">An app can behave as a client (by declaring that it needs permissions tooa resource), as a resource (by declaring which permissions it exposes), or as both.</span></span>

## <a name="controls"></a><span data-ttu-id="9bfa5-152">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="9bfa5-152">Controls</span></span>

<span data-ttu-id="9bfa5-153">hello hello különböző felügyeleti vezérlő érhető el ez a viselkedés az listája látható.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-153">hello following is a list of hello different admin controls available for all this behavior.</span></span> <span data-ttu-id="9bfa5-154">Üdvözöljük a rendszergazdákat vezérlők is elérhetők a klasszikus portálon hello konfigurálása hello könyvtára alatt tárolja.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-154">hello admin controls can be accessed in hello classic portal from configure under hello directory.</span></span>

![](media/active-directory-apps-permissions-consent/apps7.png)

<span data-ttu-id="9bfa5-155">A hello Azure portál, a **kezelése**, **felhasználói beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-155">In hello Azure portal, under **manage**, **user settings**.</span></span>

![](media/active-directory-apps-permissions-consent/apps11.png)



- <span data-ttu-id="9bfa5-156">Szabályozhatja, hogy a felhasználók is hozzájárulás tooapps:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-156">You can control whether users can consent tooapps:</span></span>

<span data-ttu-id="9bfa5-157">Válassza hello klasszikus portál **felhasználók adhat alkalmazások engedélyek tooaccess adataikat.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-157">In hello classic portal, select **Users may give applications permissions tooaccess their data.**
![](media/active-directory-apps-permissions-consent/apps8.png)</span></span>

<span data-ttu-id="9bfa5-158">Hello Azure-portálon, válassza ki **felhasználók engedélyezhetik alkalmazások tooaccess adataikat**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-158">In hello Azure portal, select **users can allow apps tooaccess their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps12.png)



- <span data-ttu-id="9bfa5-159">Szabályozhatja, hogy a felhasználók regisztrálhatják saját single-bérlő LOB-alkalmazások: A klasszikus portál select hello **felhasználók hozzáadhatnak integrált alkalmazások.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-159">You can control whether users can register their own single-tenant LOB apps: In hello classic portal select **Users may add integrated applications.**
![](media/active-directory-apps-permissions-consent/apps9.png)</span></span>

<span data-ttu-id="9bfa5-160">Hello Azure-portálon, válassza ki **felhasználók engedélyezhetik alkalmazások tooaccess adataikat**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-160">In hello Azure portal, select **users can allow apps tooaccess their data**.</span></span>
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
><span data-ttu-id="9bfa5-161">Akkor is, ha engedélyezi a felhasználók tooregister single-bérlő LOB-alkalmazások, nincsenek toowhat regisztrálható korlátozások.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-161">Even if you do allow users tooregister single-tenant LOB apps, there are limits toowhat can be registered.</span></span>  
><span data-ttu-id="9bfa5-162">A korlátozások vonatkoznak például olyan fejlesztőkre, akik nem címtár-rendszergazdák.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-162">For example, developers who are not directory admins.</span></span>
>
>- <span data-ttu-id="9bfa5-163">A felhasználók nem alakíthatnak át egy egybérlős alkalmazást több-bérlőssé.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-163">Users cannot make a single-tenant app a multi-tenant app.</span></span>
>- <span data-ttu-id="9bfa5-164">Amikor regisztrál egy bérlői LOB-alkalmazások, felhasználók csak alkalmazás engedélyek tooother alkalmazások nem igényelhetnek.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-164">When registering single-tenant LOB apps, users cannot request app-only permissions tooother apps.</span></span>
>- <span data-ttu-id="9bfa5-165">Amikor regisztrál egy bérlői LOB-alkalmazások, a felhasználók nem kérik delegált jogosultságokkal sikeresen telepítették tooother alkalmazások, ha ezeket az engedélyeket a rendszergazda jóváhagyását van szükség.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-165">When registering single-tenant LOB apps, users cannot request delegated permissions tooother apps if those permissions require admin consent.</span></span>
>- <span data-ttu-id="9bfa5-166">Felhasználók, amelyek még nincsenek tulajdonosainak módosítások tooapps nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-166">Users cannot make changes tooapps that they are not owners of.</span></span>



- <span data-ttu-id="9bfa5-167">Szabályozhatja, hogy a felhasználók hozzáadhatnak-e előre integrált, jelszavas egyszeri bejelentkezést (más néven „jelszótárolást”) használó alkalmazásokat. ![](media/active-directory-apps-permissions-consent/apps10.png)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-167">You can control whether users can themselves add pre-integrated apps that use password SSO (aka “password vaulting”) ![](media/active-directory-apps-permissions-consent/apps10.png)</span></span>



- <span data-ttu-id="9bfa5-168">Szabályozhatja, hogy az alkalmazások milyen feltételek esetén férhetők hozzá (azaz feltételes hozzáférést is megadhat).</span><span class="sxs-lookup"><span data-stu-id="9bfa5-168">You can control under which conditions applications can be accessed (that is, conditional access).</span></span> <span data-ttu-id="9bfa5-169">Ügyeljen arra, hogy ez toohello ügyfélalkalmazás és toohello erőforrás alkalmazás is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-169">Be aware this applies both toohello client app and toohello resource app.</span></span> <span data-ttu-id="9bfa5-170">Igen fel hogy feltételes hozzáférési szabályzatot, amely szerint a "Office 365 Exchange online-hoz" hello alkalmazást csak a számítógépek, amelyek megfelelő érhető el.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-170">So, say you set a conditional access policy that says that hello “Office 365 Exchange Online” app can only be accessed from machines, which are compliant.</span></span>  <span data-ttu-id="9bfa5-171">Ez a házirend is fog indítsa, amikor egy felhasználó megpróbál egy Online engedélyek tooExchange kérő ügyfélalkalmazás toouse.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-171">This policy will also kick in when a user attempts toouse a client app which requests permissions tooExchange Online.</span></span>



- <span data-ttu-id="9bfa5-172">Lehetősége van, amelybe alkalmazások hozzájárult, melyeket használt tooand volt látható.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-172">You have visibility into which apps have been consented tooand which ones are being used.</span></span>

1.  <span data-ttu-id="9bfa5-173">Amikor a felhasználó hozzájárul tooan alkalmazás, egy szolgáltatásnév objektum hello bérlő jön létre.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-173">When a user consents tooan app, a ServicePrincipal object is created in hello tenant.</span></span> <span data-ttu-id="9bfa5-174">Szolgáltatásnév létrehozása hello ellenőrzési jelentés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-174">ServicePrincipal creation is included in hello audit report.</span></span>
2.  <span data-ttu-id="9bfa5-175">Felhasználói bejelentkezési tevékenység jelentések meg, melyik alkalmazás hello felhasználó próbál bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-175">User sign-in activity reports tell you which app hello user is signing in to.</span></span> 

## <a name="example"></a><span data-ttu-id="9bfa5-176">Példa</span><span class="sxs-lookup"><span data-stu-id="9bfa5-176">Example</span></span>

<span data-ttu-id="9bfa5-177">Tegyük fel megtudhatja, hogy észrevette a bérlő felhasználói bejelentkezés hello "Office 365-höz FabrikamMail" alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-177">As an example, let’s take hello “FabrikamMail for Office 365” app, which you’ve noticed users in your tenant are signing in to.</span></span> <span data-ttu-id="9bfa5-178">A „FabrikamMail” egy levélolvasó alkalmazás Android rendszerhez, amelyet a „Fabrikam, Inc.” tett közzé.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-178">“FabrikamMail” is a mail reader app for Android, published by “Fabrikam, Inc.”.</span></span> <span data-ttu-id="9bfa5-179">Ez beleesik hello "több-bérlős alkalmazásokhoz, amelyek Contoso is más develop".</span><span class="sxs-lookup"><span data-stu-id="9bfa5-179">This falls into hello “multi-tenant apps other develop, which Contoso can consent to”.</span></span>

<span data-ttu-id="9bfa5-180">Ha tooconsent a felhasználók számára engedélyezett, akkor töltse le hozzájárulás kérése hello első bejelentkezéskor:![](media/active-directory-apps-permissions-consent/apps14.png)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-180">If users are allowed tooconsent, they get a consent prompt hello first time they sign in: ![](media/active-directory-apps-permissions-consent/apps14.png)</span></span>

<span data-ttu-id="9bfa5-181">Hozzáférés-postaládáit"érték hello felhasználók számára is elérhető hozzájárulási karakterlánc hello"Postaládák hello bejelentkezett felhasználó nevében"engedélyt"Office 365 Exchange online-hoz"(Ez azt jelenti, hogy az Exchange) tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-181">“Access your mailboxes” is hello user-facing consent string for hello “Access mailboxes as hello signed-in user” permission exposed by “Office 365 Exchange Online” (that is, Exchange).</span></span>

<span data-ttu-id="9bfa5-182">Az Exchange (hello erőforrás), amelyhez hozzá lett adva a regisztráció során az Office 365 hello szolgáltatásnév objektum szervezetifiók hello engedélyek tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-182">You can see hello permissions by looking up hello ServicePrincipal object for Exchange (hello resource), which was added when you signed up for Office 365.</span></span> <span data-ttu-id="9bfa5-183">Az eltolásokat tekintheti hello szolgáltatásnév objektum hello app "példányának" az Ön bérlőjében, különböző beállítások és konfigurációk rögzítése használt.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-183">You can think of hello ServicePrincipal object of an “instance” of hello app in your tenant, which is used for recording different options and configurations.</span></span>  <span data-ttu-id="9bfa5-184">Erre úgy tekinthet hello segítségével `Get-AzureADServicePrincipal` a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-184">You can see this by using hello `Get-AzureADServicePrincipal` in PowerShell.</span></span>

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
                                  AdminConsentDescription : Allows hello app toohave hello same access toomailboxes as hello signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as hello signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows hello app full access tooyour mailboxes on your behalf.
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

<span data-ttu-id="9bfa5-185">Amikor hello felhasználó kattint az "Elfogadás" hozzájárulási indítható.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-185">Consent is initiated when hello user clicks “Accept”.</span></span> <span data-ttu-id="9bfa5-186">Először egy szolgáltatásnév objektum "Office 365-höz FabrikamMail" hello bérlő jön létre.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-186">First, a ServicePrincipal object for “FabrikamMail for Office 365” is created in hello tenant.</span></span> <span data-ttu-id="9bfa5-187">hello szolgáltatásnév a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-187">hello ServicePrincipal looks something like this:</span></span>

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

<span data-ttu-id="9bfa5-188">Küldőnek tooan app kapcsolatot hoz létre Oauth2PermissionGrant hello következő között:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-188">Consenting tooan app creates an Oauth2PermissionGrant link between hello following:</span></span>
  
- <span data-ttu-id="9bfa5-189">hello felhasználói objektum</span><span class="sxs-lookup"><span data-stu-id="9bfa5-189">hello user object</span></span>
- <span data-ttu-id="9bfa5-190">hello ügyfélalkalmazások ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-190">hello client apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="9bfa5-191">hello erőforrás alkalmazások ServicePrincipalName (SPN)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-191">hello resource apps ServicePrincipalName (SPN)</span></span>
- <span data-ttu-id="9bfa5-192">engedélyek hello erőforrás alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-192">permissions in hello resource app.</span></span>  

<span data-ttu-id="9bfa5-193">FabrikamMail hello esetben azt a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-193">In hello case of FabrikamMail, it looks something like this:</span></span>

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

<span data-ttu-id="9bfa5-194">(**ClientId** FabrikamMail a szolgáltatás egyszerű objektum azonosítója (hello egy újonnan létrehozott kapott), **PrincipalId** hello felhasználói objektum azonosítója (hello felhasználó átadni kívánt hozzájárult e), **ResourceId**Exchange a szolgáltatás egyszerű Objektumazonosító, a hatókör lett átadni kívánt hozzájárult e Exchange hello engedélyre).</span><span class="sxs-lookup"><span data-stu-id="9bfa5-194">(**ClientId** is FabrikamMail’s service principal object ID (hello one that just got created), **PrincipalId** is hello user object ID (of hello user who consented), **ResourceId** is Exchange’s service principal object ID, Scope is hello permission in Exchange that was consented to).</span></span>

<span data-ttu-id="9bfa5-195">Ha a felhasználók nem írhatják tooconsent, látnak, amely szerint az engedélyt képernyő szükség.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-195">If users are not allowed tooconsent, they will see a screen that says that permission is required.</span></span>

