---
title: "Az Azure Active Directory B2B együttműködés SaaS-alkalmazások konfigurálása |} Microsoft Docs"
description: "Azure Active Directory B2B együttműködés kód és a PowerShell-példák"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 149a493f7b369415f0a2726dd6a576f0195c13d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a><span data-ttu-id="f8a23-103">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8a23-103">Configure SaaS apps for B2B collaboration</span></span>

<span data-ttu-id="f8a23-104">Az Azure Active Directory (Azure AD) B2B együttműködés működik együtt a legtöbb alkalmazást, amelyekbe beépül az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8a23-104">Azure Active Directory (Azure AD) B2B collaboration works with most apps that integrate with Azure AD.</span></span> <span data-ttu-id="f8a23-105">Ez a szakasz azt végezze el az Azure AD B2B néhány népszerű SaaS-alkalmazások a konfigurálására vonatkozó útmutatásokat.</span><span class="sxs-lookup"><span data-stu-id="f8a23-105">In this section, we walk through instructions for configuring some popular SaaS apps for use with Azure AD B2B.</span></span>

<span data-ttu-id="f8a23-106">Mielőtt az alkalmazás-specifikus utasításokkal tekinti meg, az alábbiakban néhány szabályok megoldás:</span><span class="sxs-lookup"><span data-stu-id="f8a23-106">Before you look at app-specific instructions, here are some rules of thumb:</span></span>

* <span data-ttu-id="f8a23-107">Az alkalmazások a legtöbb, a felhasználó beállítása fordulhat elő, manuálisan kell.</span><span class="sxs-lookup"><span data-stu-id="f8a23-107">For most of the apps, user setup needs to happen manually.</span></span> <span data-ttu-id="f8a23-108">Ez azt jelenti, hogy felhasználók manuálisan kell létrehozni az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f8a23-108">That is, users must be created manually in the app as well.</span></span>

* <span data-ttu-id="f8a23-109">Automatikus telepítés, például a Dropbox, támogató alkalmazások esetében külön meghívókat készített alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f8a23-109">For apps that support automatic setup, such as Dropbox, separate invitations are created from the apps.</span></span> <span data-ttu-id="f8a23-110">Lehet, hogy a felhasználók meg arról, hogy minden egyes meghívó elfogadásához.</span><span class="sxs-lookup"><span data-stu-id="f8a23-110">Users must be sure to accept each invitation.</span></span>

* <span data-ttu-id="f8a23-111">A felhasználói attribútumok a problémák merülnek fel a összekeveredett felhasználói profil lemezre (UPD) a vendégfelhasználók mérséklése érdekében mindig beállította **felhasználói azonosító** való **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="f8a23-111">In the user attributes, to mitigate any issues with mangled user profile disk (UPD) in guest users, always set **User Identifier** to **user.mail**.</span></span>


## <a name="dropbox-business"></a><span data-ttu-id="f8a23-112">Dropbox üzleti</span><span class="sxs-lookup"><span data-stu-id="f8a23-112">Dropbox Business</span></span>

<span data-ttu-id="f8a23-113">Ahhoz, hogy a felhasználók jelentkezhetnek be a szervezeti fiókjával, manuálisan kell konfigurálni a Dropbox üzleti az Azure AD használatára a Security Assertion Markup Language (SAML) identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="f8a23-113">To enable users to sign in using their organization account, you must manually configure Dropbox Business to use Azure AD as a Security Assertion Markup Language (SAML) identity provider.</span></span> <span data-ttu-id="f8a23-114">Ha Dropbox üzleti nem erre van konfigurálva, nem kérése és egyéb engedélyezése a felhasználók számára, hogy jelentkezzen be az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8a23-114">If Dropbox Business has not been configured to do so, it cannot prompt or otherwise allow users to sign in using Azure AD.</span></span>

1. <span data-ttu-id="f8a23-115">Az Azure AD-be a Dropbox üzleti alkalmazás hozzáadásához válassza **vállalati alkalmazások** a bal oldali ablaktáblán, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f8a23-115">To add the Dropbox Business app into Azure AD, select **Enterprise applications** in the left pane, and then click **Add**.</span></span>

  ![A "Hozzáadás" gombra a vállalati alkalmazások lap](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. <span data-ttu-id="f8a23-117">Az a **alkalmazás hozzáadása** ablak, írja be **dropbox** a keresési mezőbe, és válassza a **vállalati Dropbox** az eredménylistában.</span><span class="sxs-lookup"><span data-stu-id="f8a23-117">In the **Add an application** window, enter **dropbox** in the search box, and then select **Dropbox for Business** in the results list.</span></span>

  ![Egy alkalmazás-lap hozzáadása "dropbox" keresése](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. <span data-ttu-id="f8a23-119">A a **egyszeri bejelentkezés** lapon jelölje be **egyszeri bejelentkezés** a bal oldali ablaktáblán, majd adja meg **user.mail** a a **felhasználói azonosító** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f8a23-119">On the **Single sign-on** page, select **Single sign-on** in the left pane, and then enter **user.mail** in the **User Identifier** box.</span></span> <span data-ttu-id="f8a23-120">(Érték szerint UPN alapértelmezés szerint.)</span><span class="sxs-lookup"><span data-stu-id="f8a23-120">(It's set as UPN by default.)</span></span>

  ![Egyszeri bejelentkezés az alkalmazás konfigurálása](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. <span data-ttu-id="f8a23-122">A Dropbox-konfigurációhoz használni kívánt tanúsítványt letöltéséhez, jelölje be az **konfigurálása DropBox**, majd válassza ki **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím** a listában.</span><span class="sxs-lookup"><span data-stu-id="f8a23-122">To download the certificate to use for Dropbox configuration, select **Configure DropBox**, and then select **SAML Single Sign On Service URL** in the list.</span></span>

  ![A Dropbox-konfiguráció tanúsítvány letöltése](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. <span data-ttu-id="f8a23-124">Jelentkezzen be a bejelentkezési URL-címet a Dropbox a **egyszeri bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="f8a23-124">Sign in to Dropbox with the sign-on URL from the **Single sign-on** page.</span></span>

  ![A Dropbox bejelentkezési oldal](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. <span data-ttu-id="f8a23-126">Válassza a menü **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="f8a23-126">On the menu, select **Admin Console**.</span></span>

  ![A "Felügyeleti konzol" hivatkozásra kattintva a Dropbox menü](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. <span data-ttu-id="f8a23-128">A a **hitelesítési** párbeszédpanelen jelölje ki **további**, a tanúsítvány feltöltése, a a **jelentkezzen be az URL-cím** mezőbe írja be a SAML egyetlen bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="f8a23-128">In the **Authentication** dialog box, select **More**, upload the certificate and then, in the **Sign in URL** box, enter the SAML single sign-on URL.</span></span>

  ![A "Több" hivatkozásra a összecsukott párbeszédpanel](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![A "bejelentkezés URL-címe" a hitelesítés kibontott párbeszédpanelen](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. <span data-ttu-id="f8a23-131">Automatikus felhasználó beállítása az Azure portálon konfigurálásához jelölje ki **kiépítési** a bal oldali panelen válassza ki a **automatikus** a a **kiépítési üzemmódban** mezőbe, majd válassza ki a  **Engedélyezi**.</span><span class="sxs-lookup"><span data-stu-id="f8a23-131">To configure automatic user setup in the Azure portal, select **Provisioning** in the left pane, select **Automatic** in the **Provisioning Mode** box, and then select **Authorize**.</span></span>

  ![Automatikus felhasználók átadására az Azure-portálon](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

<span data-ttu-id="f8a23-133">Vendég vagy tag felhasználók a Dropbox alkalmazás van beállítva, akkor kapnak egy külön meghívó az dropbox-bA.</span><span class="sxs-lookup"><span data-stu-id="f8a23-133">After guest or member users have been set up in the Dropbox app, they receive a separate invitation from Dropbox.</span></span> <span data-ttu-id="f8a23-134">Dropbox egyszeri bejelentkezést használ, a meghívott személyeknek el kell fogadnia a meghívó a hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="f8a23-134">To use Dropbox single sign-on, invitees must accept the invitation by clicking a link in it.</span></span>

## <a name="box"></a><span data-ttu-id="f8a23-135">Box</span><span class="sxs-lookup"><span data-stu-id="f8a23-135">Box</span></span>
<span data-ttu-id="f8a23-136">Engedélyezheti a felhasználók számára a vendégfelhasználók mezőben az Azure AD-fiókkal hitelesítést összevonási alapuló az SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="f8a23-136">You can enable users to authenticate Box guest users with their Azure AD account by using federation that's based on the SAML protocol.</span></span> <span data-ttu-id="f8a23-137">Ezzel az eljárással feltöltött Box.com metaadatok.</span><span class="sxs-lookup"><span data-stu-id="f8a23-137">In this procedure, you upload metadata to Box.com.</span></span>

1. <span data-ttu-id="f8a23-138">A Box alkalmazásához adja hozzá a vállalati alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="f8a23-138">Add the Box app from the enterprise apps.</span></span>

2. <span data-ttu-id="f8a23-139">Egyszeri bejelentkezés konfigurálása a következő sorrendben:</span><span class="sxs-lookup"><span data-stu-id="f8a23-139">Configure single sign-on in the following order:</span></span>

  ![Mezőbe az egyszeri bejelentkezés konfigurálása](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 <span data-ttu-id="f8a23-141">a.</span><span class="sxs-lookup"><span data-stu-id="f8a23-141">a.</span></span> <span data-ttu-id="f8a23-142">Az a **bejelentkezési URL-cím** győződjön meg arról, hogy a bejelentkezési URL-cím beállításai megfelelően be az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f8a23-142">In the **Sign on URL** box, ensure that the sign-on URL is set appropriately for Box in the Azure portal.</span></span> <span data-ttu-id="f8a23-143">Az URL-cím a Box.com bérlői URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f8a23-143">This URL is the URL of your Box.com tenant.</span></span> <span data-ttu-id="f8a23-144">Azt az elnevezési konvenciót kell követnie *https://.box.com*.</span><span class="sxs-lookup"><span data-stu-id="f8a23-144">It should follow the naming convention *https://.box.com*.</span></span>  
 <span data-ttu-id="f8a23-145">A **azonosító** nem felel meg az alkalmazáshoz, de is megjelenik a kötelező mező.</span><span class="sxs-lookup"><span data-stu-id="f8a23-145">The **Identifier** does not apply to this app, but it still appears as a mandatory field.</span></span>

 <span data-ttu-id="f8a23-146">b.</span><span class="sxs-lookup"><span data-stu-id="f8a23-146">b.</span></span> <span data-ttu-id="f8a23-147">Az a **felhasználói azonosító** adja meg a **user.mail** (az egyszeri bejelentkezés a Vendég-fiókok).</span><span class="sxs-lookup"><span data-stu-id="f8a23-147">In the **User identifier** box, enter **user.mail** (for SSO for guest accounts).</span></span>

 <span data-ttu-id="f8a23-148">c.</span><span class="sxs-lookup"><span data-stu-id="f8a23-148">c.</span></span> <span data-ttu-id="f8a23-149">A **SAML-aláíró tanúsítványa**, kattintson a **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="f8a23-149">Under **SAML Signing Certificate**, click **Create new certificate**.</span></span>

 <span data-ttu-id="f8a23-150">d.</span><span class="sxs-lookup"><span data-stu-id="f8a23-150">d.</span></span> <span data-ttu-id="f8a23-151">A Box.com bérlő identitás-szolgáltatóként használhatja az Azure Active Directory konfigurálása a kezdéshez töltse le a metaadatokat, és mentse a helyi meghajtóról.</span><span class="sxs-lookup"><span data-stu-id="f8a23-151">To begin configuring your Box.com tenant to use Azure AD as an identity provider, download the metadata file and then save it to your local drive.</span></span>

 <span data-ttu-id="f8a23-152">e.</span><span class="sxs-lookup"><span data-stu-id="f8a23-152">e.</span></span> <span data-ttu-id="f8a23-153">A mezőben a metaadatfájl előre támogatja a csoport, amely egyszeri bejelentkezést az Ön konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="f8a23-153">Forward the metadata file to the Box support team, which configures single sign-on for you.</span></span>

3. <span data-ttu-id="f8a23-154">Az Azure AD automatikus felhasználó beállítása, a bal oldali panelen válassza a **kiépítési**, majd válassza ki **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="f8a23-154">For Azure AD automatic user setup, in the left pane, select **Provisioning**, and then select **Authorize**.</span></span>

  ![Engedélyezi az Azure AD-be való kapcsolódáshoz](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

<span data-ttu-id="f8a23-156">Például a Dropbox a meghívott személyeknek a mezőben a meghívott személyeknek a meghívó a Box alkalmazásához a kell beváltani.</span><span class="sxs-lookup"><span data-stu-id="f8a23-156">Like Dropbox invitees, Box invitees must redeem their invitation from the Box app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8a23-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8a23-157">Next steps</span></span>

<span data-ttu-id="f8a23-158">Az Azure AD B2B együttműködés, tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="f8a23-158">See the following articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="f8a23-159">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="f8a23-159">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="f8a23-160">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f8a23-160">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="f8a23-161">Egy szerepkör B2B együttműködés felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f8a23-161">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="f8a23-162">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="f8a23-162">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="f8a23-163">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="f8a23-163">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="f8a23-164">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="f8a23-164">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="f8a23-165">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="f8a23-165">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="f8a23-166">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="f8a23-166">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="f8a23-167">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="f8a23-167">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="f8a23-168">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="f8a23-168">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
