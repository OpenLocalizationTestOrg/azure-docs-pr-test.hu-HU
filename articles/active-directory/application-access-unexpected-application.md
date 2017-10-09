---
title: "az alkalmazások listáját a aaaUnexpected alkalmazás |} Microsoft Docs"
description: "Hogyan toosee a bérlő lévő összes alkalmazásnál és megérteni, hogyan alkalmazások jelennek meg az összes alkalmazások listáját a vállalati alkalmazások"
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
ms.openlocfilehash: 2f974046b655bc36a05f002c56511a8a988cd01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-application-in-my-applications-list"></a><span data-ttu-id="5afb2-103">Az alkalmazások listáját a váratlan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5afb2-103">Unexpected application in my applications list</span></span>

<span data-ttu-id="5afb2-104">Ez a cikk segítséget toounderstand hogyan az alkalmazások megjelennek a **összes alkalmazás** listában az **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5afb2-104">This article help you toounderstand how applications appear in your **All Applications** list under **Enterprise Applications**.</span></span> 

## <a name="how-toosee-all-applications-in-your-tenant"></a><span data-ttu-id="5afb2-105">Hogyan toosee összes alkalmazás-bérlőben</span><span class="sxs-lookup"><span data-stu-id="5afb2-105">How toosee all applications in your tenant</span></span>

<span data-ttu-id="5afb2-106">toosee összes hello alkalmazások az Ön bérlőjében, toouse hello kell **szűrő** tooshow szabályozása **összes alkalmazás** alatt hello **összes alkalmazás** listája.</span><span class="sxs-lookup"><span data-stu-id="5afb2-106">toosee all hello applications in your tenant, you need toouse hello **Filter** control tooshow **All Applications** under hello **All Applications** list.</span></span> <span data-ttu-id="5afb2-107">toodo ezt, hajtsa végre hello az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5afb2-107">toodo this, follow hello steps below:</span></span>

1.  <span data-ttu-id="5afb2-108">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="5afb2-108">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="5afb2-109">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="5afb2-109">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5afb2-110">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5afb2-110">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5afb2-111">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="5afb2-111">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5afb2-112">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="5afb2-112">click **All Applications** tooview a list of all your applications.</span></span>

6.  <span data-ttu-id="5afb2-113">Kattintson a hello használata hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját**.</span><span class="sxs-lookup"><span data-stu-id="5afb2-113">click hello use hello **Filter** control at hello top of hello **All Applications List**.</span></span>

7.  <span data-ttu-id="5afb2-114">A hello **szűrő** panelen, a set hello **megjelenítése** beállítás túl**összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="5afb2-114">On hello **Filter** blade, set hello **Show** option too**All Applications.**</span></span>

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a><span data-ttu-id="5afb2-115">Miért nem egy adott alkalmazás jelenik meg az összes alkalmazások listáját?</span><span class="sxs-lookup"><span data-stu-id="5afb2-115">Why does a specific application appear in my all applications list?</span></span>

<span data-ttu-id="5afb2-116">Túl szűrve**összes alkalmazás**, hello **összes alkalmazás** **lista** minden egyszerű szolgáltatásnév objektum szerepel a bérlő.</span><span class="sxs-lookup"><span data-stu-id="5afb2-116">When filtered too**All Applications**, hello **All Applications** **List** shows every Service Principal object in your tenant.</span></span> <span data-ttu-id="5afb2-117">Egyszerű objektumok ebben a listában, a különböző módon jelenhetnek meg:</span><span class="sxs-lookup"><span data-stu-id="5afb2-117">Service Principal objects can appear in this list in a various ways:</span></span>

1.  <span data-ttu-id="5afb2-118">Amikor bármely alkalmazás hello alkalmazás gyűjteményből, beleértve:</span><span class="sxs-lookup"><span data-stu-id="5afb2-118">When you add any application from hello application gallery, including:</span></span>

   1. <span data-ttu-id="5afb2-119">**Azure AD-gyűjtemény alkalmazások** –, amelyet az egyszeri bejelentkezés az Azure ad-vel előre integrált alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5afb2-119">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

   2. <span data-ttu-id="5afb2-120">**Alkalmazás Proxy alkalmazások** – a helyszíni környezetben futó tooprovide biztonságos egyszeri bejelentkezést tooexternally kívánt alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="5afb2-120">**Application Proxy Applications** – An application running in your on-premises environment that you want tooprovide secure single-sign on tooexternally.</span></span>

   3. <span data-ttu-id="5afb2-121">**Egyéni által fejlesztett alkalmazások** – olyan alkalmazás, amely a szervezet által toodevelop a hello Azure AD alkalmazás-fejlesztő Platform, de, amely nem még létezik.</span><span class="sxs-lookup"><span data-stu-id="5afb2-121">**Custom-developed Applications** – An application that your organization wishes toodevelop on hello Azure AD Application Development Platform, but that may not exist yet.</span></span>

   4. <span data-ttu-id="5afb2-122">**Nem-gyűjtemény alkalmazások** – kapcsolja a saját alkalmazásai!</span><span class="sxs-lookup"><span data-stu-id="5afb2-122">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="5afb2-123">Bármely kívánt webes hivatkozást, vagy egy felhasználónevet és jelszót a mezőben, amely alkalmazás SAML vagy az OpenID Connect protokollt támogatja, vagy az egyszeri bejelentkezés az Azure ad-val toointegrate verzióváltáshoz SCIM támogatja.</span><span class="sxs-lookup"><span data-stu-id="5afb2-123">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish toointegrate for single sign-on with Azure AD.</span></span>

2.  <span data-ttu-id="5afb2-124">Ha regisztrálni szeretne, vagy egy 3 történő bejelentkezés<sup>távoli asztali</sup> gyártótól származó alkalmazás Azure Active Directory integrált része.</span><span class="sxs-lookup"><span data-stu-id="5afb2-124">When signing up for, or signing in to, a 3<sup>rd</sup> party application integrated with Azure Active Directory.</span></span> <span data-ttu-id="5afb2-125">Egy példa erre, [Smartsheet](https://app.smartsheet.com/b/home) vagy [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span><span class="sxs-lookup"><span data-stu-id="5afb2-125">One example of this is [Smartsheet](https://app.smartsheet.com/b/home) or [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span></span>

3.  <span data-ttu-id="5afb2-126">Ha regisztrálni szeretne, vagy a licenc tooa felhasználó vagy csoport tooa első gyártótól származó alkalmazás hozzáadása, például [Microsoft Office 365](http://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="5afb2-126">When signing up for, or adding a license tooa user or a group tooa first party application, like [Microsoft Office 365](http://products.office.com/).</span></span>

4.  <span data-ttu-id="5afb2-127">Amikor felvesz egy új alkalmazás regisztrációja hozzon létre egy egyéni által fejlesztett alkalmazás használatával hello [alkalmazás beállításjegyzék](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span><span class="sxs-lookup"><span data-stu-id="5afb2-127">When you add a new application registration by creating a custom-developed application using hello [Application Registry](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span></span>

5.  <span data-ttu-id="5afb2-128">Amikor felvesz egy új alkalmazás regisztrációja hozzon létre egy egyéni által fejlesztett alkalmazás használatával hello [V2.0 alkalmazásregisztrációs portálra](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span><span class="sxs-lookup"><span data-stu-id="5afb2-128">When you add a new application registration by creating a custom-developed application using hello [V2.0 Application Registration Portal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span></span>

6.  <span data-ttu-id="5afb2-129">Ha egy alkalmazás kidolgozása Visual Studio használatával [ASP.net hitelesítési módszerek](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) vagy [kapcsolódó szolgáltatások](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span><span class="sxs-lookup"><span data-stu-id="5afb2-129">When you add an application you’re developing using Visual Studio’s [ASP.net Authentication Methods](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) or [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span></span>

7.  <span data-ttu-id="5afb2-130">Amikor egy szolgáltatás egyszerű objektumot hello segítségével hoz létre [Azure AD PowerShell modult](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="5afb2-130">When you create a service principal object using hello [Azure AD PowerShell Module](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

8.  <span data-ttu-id="5afb2-131">Ha Ön [tooan alkalmazás hozzájárulás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) egy a bérlői rendszergazda toouse adatként.</span><span class="sxs-lookup"><span data-stu-id="5afb2-131">When you [consent tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) as an administrator toouse data in your tenant.</span></span>

9.  <span data-ttu-id="5afb2-132">Ha egy [felhasználói beleegyezik tooan alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse adatok az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="5afb2-132">When a [user consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data in your tenant.</span></span>

10. <span data-ttu-id="5afb2-133">Ha engedélyezi az egyes szolgáltatások tárolt adatok az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="5afb2-133">When you enable certain services that store data in your tenant.</span></span> <span data-ttu-id="5afb2-134">Egy példa erre, a jelszó alaphelyzetbe állítása, amely van modellezve a szolgáltatás egyszerű toostore, a jelszó-visszaállítási házirend biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="5afb2-134">One example of this is Password Reset, which is modeled as a service principal toostore your password reset policy securely.</span></span>

<span data-ttu-id="5afb2-135">tooget hogyan az alkalmazások kerülnek tooyour directory, a további részletekért olvassa el [útmutató és érvek alkalmazások felvétele tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span><span class="sxs-lookup"><span data-stu-id="5afb2-135">tooget more details on how apps are added tooyour directory, read [How and why applications are added tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span></span>

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a><span data-ttu-id="5afb2-136">Egy adott felhasználó vagy csoport hozzárendelése tooan alkalmazás kívánt tooremove</span><span class="sxs-lookup"><span data-stu-id="5afb2-136">I want tooremove a specific user’s or group’s assignment tooan application</span></span>

<span data-ttu-id="5afb2-137">egy felhasználó vagy csoport hozzárendelése tooan alkalmazást, tooremove lépésekkel hello hello felsorolt [egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) cikk.</span><span class="sxs-lookup"><span data-stu-id="5afb2-137">tooremove a user or group assignment tooan application, follow hello steps listed in hello [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

## <a name="i-want-toodisable-all-access-tooan-application-for-every-user"></a><span data-ttu-id="5afb2-138">Minden felhasználó az összes access tooan alkalmazás kívánt toodisable</span><span class="sxs-lookup"><span data-stu-id="5afb2-138">I want toodisable all access tooan application for every user</span></span>

<span data-ttu-id="5afb2-139">toodisable minden felhasználói bejelentkezések tooan alkalmazást, kövesse hello lépéseket hello [tiltsa le a felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) cikk.</span><span class="sxs-lookup"><span data-stu-id="5afb2-139">toodisable all user sign-ins tooan application, follow hello steps listed in hello [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-toodelete-an-application-entirely"></a><span data-ttu-id="5afb2-140">Egy alkalmazás toodelete teljesen kívánt</span><span class="sxs-lookup"><span data-stu-id="5afb2-140">I want toodelete an application entirely</span></span>

<span data-ttu-id="5afb2-141">túl**alkalmazás törlése**, kövesse az alábbi hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="5afb2-141">too**delete an application**, follow hello instructions below:</span></span>

1.  <span data-ttu-id="5afb2-142">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="5afb2-142">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="5afb2-143">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="5afb2-143">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5afb2-144">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5afb2-144">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5afb2-145">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="5afb2-145">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5afb2-146">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="5afb2-146">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="5afb2-147">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="5afb2-147">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="5afb2-148">Válassza ki a kívánt toodelete hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5afb2-148">Select hello application you want toodelete.</span></span>

7.  <span data-ttu-id="5afb2-149">Amikor hello alkalmazás betölt, kattintson a **törlése** hello felső alkalmazás ikonja **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="5afb2-149">Once hello application loads, click **Delete** icon from hello top application’s **Overview** blade.</span></span>

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a><span data-ttu-id="5afb2-150">Az összes jövőbeni felhasználói hozzájárulás műveletek tooany alkalmazás kívánt toodisable</span><span class="sxs-lookup"><span data-stu-id="5afb2-150">I want toodisable all future user consent operations tooany application</span></span>

<span data-ttu-id="5afb2-151">Felhasználói hozzájárulás letiltása, az a teljes címtár megakadályozhatja a végfelhasználók számára a küldőnek tooany alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="5afb2-151">Disabling user consent for your entire directory prevent end users from consenting tooany application.</span></span> <span data-ttu-id="5afb2-152">A rendszergazdák továbbra is a felhasználó behalves is hozzájárulás.</span><span class="sxs-lookup"><span data-stu-id="5afb2-152">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="5afb2-153">További információ az alkalmazás toolearn hozzájárulás, és miért lehet, vagy előfordulhat, hogy nem kívánja toodo, olvassa el a következőt [ismertetése felhasználói és rendszergazdai hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="5afb2-153">toolearn more about application consent, and why you may or may not wish toodo this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="5afb2-154">túl**tiltsa le a teljes címtár minden jövőbeni felhasználói hozzájárulás műveletei**, kövesse az alábbi hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="5afb2-154">too**disable all future user consent operations in your entire directory**, follow hello instructions below:</span></span>

1.  <span data-ttu-id="5afb2-155">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="5afb2-155">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5afb2-156">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="5afb2-156">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5afb2-157">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="5afb2-157">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5afb2-158">Kattintson a **felhasználók és csoportok** hello navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="5afb2-158">click **Users and groups** in hello navigation menu.</span></span>

5.  <span data-ttu-id="5afb2-159">Kattintson a **felhasználói beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5afb2-159">click **User settings**.</span></span>

6.  <span data-ttu-id="5afb2-160">Tiltsa le az összes jövőbeni felhasználói hozzájárulás műveletek hello beállítása az **felhasználók engedélyezhetik alkalmazások tooaccess adataikat** túl váltása**nem** hello kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5afb2-160">Disable all future user consent operations by setting hello **Users can allow apps tooaccess their data** toggle too**No** and click hello **Save** button.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5afb2-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5afb2-161">Next steps</span></span>
[<span data-ttu-id="5afb2-162">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="5afb2-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
