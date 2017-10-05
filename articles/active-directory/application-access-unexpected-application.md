---
title: "Az alkalmazások listáját a váratlan alkalmazás |} Microsoft Docs"
description: "Tekintse meg a bérlő összes alkalmazást, és megérteni, hogyan alkalmazások jelennek meg az összes alkalmazások listáját a vállalati alkalmazások"
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
ms.openlocfilehash: 0f8136cb066fa6e3e4a8dd06e34b9f866e3916f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-application-in-my-applications-list"></a><span data-ttu-id="414c0-103">Az alkalmazások listáját a váratlan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="414c0-103">Unexpected application in my applications list</span></span>

<span data-ttu-id="414c0-104">Ez a cikk segítenek megérteni, hogyan az alkalmazások megjelennek a **összes alkalmazás** listában az **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="414c0-104">This article help you to understand how applications appear in your **All Applications** list under **Enterprise Applications**.</span></span> 

## <a name="how-to-see-all-applications-in-your-tenant"></a><span data-ttu-id="414c0-105">A bérlő szereplő összes alkalmazás megtekintése</span><span class="sxs-lookup"><span data-stu-id="414c0-105">How to see all applications in your tenant</span></span>

<span data-ttu-id="414c0-106">A bérlő összes alkalmazás megtekintéséhez kell használnia a **szűrő** vezérlő megjelenítése **összes alkalmazás** alatt a **összes alkalmazás** listája.</span><span class="sxs-lookup"><span data-stu-id="414c0-106">To see all the applications in your tenant, you need to use the **Filter** control to show **All Applications** under the **All Applications** list.</span></span> <span data-ttu-id="414c0-107">Ehhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="414c0-107">To do this, follow the steps below:</span></span>

1.  <span data-ttu-id="414c0-108">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="414c0-108">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="414c0-109">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="414c0-109">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="414c0-110">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="414c0-110">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="414c0-111">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="414c0-111">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="414c0-112">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="414c0-112">click **All Applications** to view a list of all your applications.</span></span>

6.  <span data-ttu-id="414c0-113">Kattintson a használata a **szűrő** vezérlő tetején a **összes alkalmazások listáját**.</span><span class="sxs-lookup"><span data-stu-id="414c0-113">click the use the **Filter** control at the top of the **All Applications List**.</span></span>

7.  <span data-ttu-id="414c0-114">A a **szűrő** panelen állítsa a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="414c0-114">On the **Filter** blade, set the **Show** option to **All Applications.**</span></span>

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a><span data-ttu-id="414c0-115">Miért nem egy adott alkalmazás jelenik meg az összes alkalmazások listáját?</span><span class="sxs-lookup"><span data-stu-id="414c0-115">Why does a specific application appear in my all applications list?</span></span>

<span data-ttu-id="414c0-116">Ha szűrt **összes alkalmazás**, a **összes alkalmazás** **lista** minden egyszerű szolgáltatásnév objektum szerepel a bérlő.</span><span class="sxs-lookup"><span data-stu-id="414c0-116">When filtered to **All Applications**, the **All Applications** **List** shows every Service Principal object in your tenant.</span></span> <span data-ttu-id="414c0-117">Egyszerű objektumok ebben a listában, a különböző módon jelenhetnek meg:</span><span class="sxs-lookup"><span data-stu-id="414c0-117">Service Principal objects can appear in this list in a various ways:</span></span>

1.  <span data-ttu-id="414c0-118">Amikor bármely alkalmazás az alkalmazás gyűjteményből, beleértve:</span><span class="sxs-lookup"><span data-stu-id="414c0-118">When you add any application from the application gallery, including:</span></span>

   1. <span data-ttu-id="414c0-119">**Azure AD-gyűjtemény alkalmazások** –, amelyet az egyszeri bejelentkezés az Azure ad-vel előre integrált alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="414c0-119">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

   2. <span data-ttu-id="414c0-120">**Alkalmazás Proxy alkalmazások** – szeretne biztosítani a biztonságos egyszeri bejelentkezést a külsőleg a helyszíni környezetben futó alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="414c0-120">**Application Proxy Applications** – An application running in your on-premises environment that you want to provide secure single-sign on to externally.</span></span>

   3. <span data-ttu-id="414c0-121">**Egyéni által fejlesztett alkalmazások** – az alkalmazás, amely a szervezet által platformon az Azure AD alkalmazás fejlesztési kialakításához, de, amely nem még létezik.</span><span class="sxs-lookup"><span data-stu-id="414c0-121">**Custom-developed Applications** – An application that your organization wishes to develop on the Azure AD Application Development Platform, but that may not exist yet.</span></span>

   4. <span data-ttu-id="414c0-122">**Nem-gyűjtemény alkalmazások** – kapcsolja a saját alkalmazásai!</span><span class="sxs-lookup"><span data-stu-id="414c0-122">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="414c0-123">Bármely kívánt webes hivatkozást, vagy egy felhasználónevet és jelszót a mezőben, amely alkalmazás SAML vagy az OpenID Connect protokollt támogatja, vagy az egyszeri bejelentkezés az Azure ad-vel integrálni kívánt SCIM támogatja.</span><span class="sxs-lookup"><span data-stu-id="414c0-123">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish to integrate for single sign-on with Azure AD.</span></span>

2.  <span data-ttu-id="414c0-124">Ha regisztrálni szeretne, vagy egy 3 történő bejelentkezés<sup>távoli asztali</sup> gyártótól származó alkalmazás Azure Active Directory integrált része.</span><span class="sxs-lookup"><span data-stu-id="414c0-124">When signing up for, or signing in to, a 3<sup>rd</sup> party application integrated with Azure Active Directory.</span></span> <span data-ttu-id="414c0-125">Egy példa erre, [Smartsheet](https://app.smartsheet.com/b/home) vagy [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span><span class="sxs-lookup"><span data-stu-id="414c0-125">One example of this is [Smartsheet](https://app.smartsheet.com/b/home) or [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).</span></span>

3.  <span data-ttu-id="414c0-126">Ha regisztrálni szeretne, vagy licencet ad hozzá egy felhasználóhoz vagy csoporthoz első fél alkalmazás, például [Microsoft Office 365](http://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="414c0-126">When signing up for, or adding a license to a user or a group to a first party application, like [Microsoft Office 365](http://products.office.com/).</span></span>

4.  <span data-ttu-id="414c0-127">Amikor felvesz egy új alkalmazás regisztrációja hozzon létre egy egyéni által fejlesztett alkalmazás használja a [alkalmazás beállításjegyzék](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span><span class="sxs-lookup"><span data-stu-id="414c0-127">When you add a new application registration by creating a custom-developed application using the [Application Registry](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).</span></span>

5.  <span data-ttu-id="414c0-128">Amikor felvesz egy új alkalmazás regisztrációja hozzon létre egy egyéni által fejlesztett alkalmazás használja a [V2.0 alkalmazásregisztrációs portálra](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span><span class="sxs-lookup"><span data-stu-id="414c0-128">When you add a new application registration by creating a custom-developed application using the [V2.0 Application Registration Portal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).</span></span>

6.  <span data-ttu-id="414c0-129">Ha egy alkalmazás kidolgozása Visual Studio használatával [ASP.net hitelesítési módszerek](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) vagy [kapcsolódó szolgáltatások](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span><span class="sxs-lookup"><span data-stu-id="414c0-129">When you add an application you’re developing using Visual Studio’s [ASP.net Authentication Methods](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) or [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).</span></span>

7.  <span data-ttu-id="414c0-130">Amikor hoz létre a szolgáltatás elsődleges objektumot használ a [Azure AD PowerShell modult](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="414c0-130">When you create a service principal object using the [Azure AD PowerShell Module](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

8.  <span data-ttu-id="414c0-131">Ha Ön [járul hozzá az alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) adatok használata a bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="414c0-131">When you [consent to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) as an administrator to use data in your tenant.</span></span>

9.  <span data-ttu-id="414c0-132">Ha egy [felhasználói járul hozzá az alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) adatok használata az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="414c0-132">When a [user consents to an application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) to use data in your tenant.</span></span>

10. <span data-ttu-id="414c0-133">Ha engedélyezi az egyes szolgáltatások tárolt adatok az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="414c0-133">When you enable certain services that store data in your tenant.</span></span> <span data-ttu-id="414c0-134">Egy példa erre, jelszó alaphelyzetbe állítása, amely szerint a szolgáltatás egyszerű tárolni a jelszó-visszaállítási házirend van modellezve biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="414c0-134">One example of this is Password Reset, which is modeled as a service principal to store your password reset policy securely.</span></span>

<span data-ttu-id="414c0-135">További részleteket a hogyan alkalmazások hozzáadódnak a könyvtárhoz, olvassa el [útmutató és érvek alkalmazások felvétele az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span><span class="sxs-lookup"><span data-stu-id="414c0-135">To get more details on how apps are added to your directory, read [How and why applications are added to Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).</span></span>

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a><span data-ttu-id="414c0-136">Egy alkalmazás egy adott felhasználó vagy csoport-hozzárendelés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="414c0-136">I want to remove a specific user’s or group’s assignment to an application</span></span>

<span data-ttu-id="414c0-137">Egy felhasználó vagy csoport-hozzárendelés alkalmazáshoz való eltávolításához kövesse a lépéseket a [egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) cikk.</span><span class="sxs-lookup"><span data-stu-id="414c0-137">To remove a user or group assignment to an application, follow the steps listed in the [Remove a user or group assignment from an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) article.</span></span>

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a><span data-ttu-id="414c0-138">Szeretném tiltani minden hozzáférést egy alkalmazáshoz minden felhasználó</span><span class="sxs-lookup"><span data-stu-id="414c0-138">I want to disable all access to an application for every user</span></span>

<span data-ttu-id="414c0-139">Minden felhasználói bejelentkezések alkalmazáshoz való letiltásához kövesse a lépéseket a [tiltsa le a felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) cikk.</span><span class="sxs-lookup"><span data-stu-id="414c0-139">To disable all user sign-ins to an application, follow the steps listed in the [Disable user sign-ins for an enterprise app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) article.</span></span>

## <a name="i-want-to-delete-an-application-entirely"></a><span data-ttu-id="414c0-140">Az alkalmazás teljesen törlése</span><span class="sxs-lookup"><span data-stu-id="414c0-140">I want to delete an application entirely</span></span>

<span data-ttu-id="414c0-141">A **alkalmazás törlése**, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="414c0-141">To **delete an application**, follow the instructions below:</span></span>

1.  <span data-ttu-id="414c0-142">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="414c0-142">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="414c0-143">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="414c0-143">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="414c0-144">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="414c0-144">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="414c0-145">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="414c0-145">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="414c0-146">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="414c0-146">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="414c0-147">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="414c0-147">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="414c0-148">Válassza ki a törölni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="414c0-148">Select the application you want to delete.</span></span>

7.  <span data-ttu-id="414c0-149">Ha az alkalmazás betölt, kattintson **törlése** ikonra a felső alkalmazás **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="414c0-149">Once the application loads, click **Delete** icon from the top application’s **Overview** blade.</span></span>

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a><span data-ttu-id="414c0-150">Szeretném tiltani az összes jövőbeni felhasználói hozzájárulás műveletek bármely alkalmazás</span><span class="sxs-lookup"><span data-stu-id="414c0-150">I want to disable all future user consent operations to any application</span></span>

<span data-ttu-id="414c0-151">Felhasználói hozzájárulás letiltása, az a teljes címtár megakadályozhatja a végfelhasználók számára hozzájárul ahhoz, hogy minden alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="414c0-151">Disabling user consent for your entire directory prevent end users from consenting to any application.</span></span> <span data-ttu-id="414c0-152">A rendszergazdák továbbra is a felhasználó behalves is hozzájárulás.</span><span class="sxs-lookup"><span data-stu-id="414c0-152">Administrators can still consent on user’s behalves.</span></span> <span data-ttu-id="414c0-153">Tudjon meg többet a kérelem jóváhagyását, és ezért előfordulhat, hogy, vagy előfordulhat, hogy nem szeretne ehhez olvassa el a [ismertetése felhasználói és rendszergazdai hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span><span class="sxs-lookup"><span data-stu-id="414c0-153">To learn more about application consent, and why you may or may not wish to do this, read [Understanding user and admin consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).</span></span>

<span data-ttu-id="414c0-154">A **tiltsa le a teljes címtár minden jövőbeni felhasználói hozzájárulás műveletei**, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="414c0-154">To **disable all future user consent operations in your entire directory**, follow the instructions below:</span></span>

1.  <span data-ttu-id="414c0-155">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="414c0-155">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="414c0-156">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="414c0-156">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="414c0-157">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="414c0-157">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="414c0-158">Kattintson a **felhasználók és csoportok** a navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="414c0-158">click **Users and groups** in the navigation menu.</span></span>

5.  <span data-ttu-id="414c0-159">Kattintson a **felhasználói beállítások**.</span><span class="sxs-lookup"><span data-stu-id="414c0-159">click **User settings**.</span></span>

6.  <span data-ttu-id="414c0-160">Tiltsa le az összes jövőbeni felhasználói hozzájárulás műveletek úgy, hogy a **felhasználók is engedélyezi, hogy az alkalmazások hozzáférjenek az adataikhoz** kapcsolót **nem** , és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="414c0-160">Disable all future user consent operations by setting the **Users can allow apps to access their data** toggle to **No** and click the **Save** button.</span></span>

## <a name="next-steps"></a><span data-ttu-id="414c0-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="414c0-161">Next steps</span></span>
[<span data-ttu-id="414c0-162">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="414c0-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
