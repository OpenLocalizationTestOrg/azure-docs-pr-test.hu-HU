---
title: "aaaConfigure SaaS-alkalmazásokhoz az Azure Active Directory B2B együttműködés |} Microsoft Docs"
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
ms.openlocfilehash: c3f22f81567c04ac23ef2316c09de718ecb15d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a><span data-ttu-id="a9c93-103">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a9c93-103">Configure SaaS apps for B2B collaboration</span></span>

<span data-ttu-id="a9c93-104">Az Azure Active Directory (Azure AD) B2B együttműködés működik együtt a legtöbb alkalmazást, amelyekbe beépül az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9c93-104">Azure Active Directory (Azure AD) B2B collaboration works with most apps that integrate with Azure AD.</span></span> <span data-ttu-id="a9c93-105">Ez a szakasz azt végezze el az Azure AD B2B néhány népszerű SaaS-alkalmazások a konfigurálására vonatkozó útmutatásokat.</span><span class="sxs-lookup"><span data-stu-id="a9c93-105">In this section, we walk through instructions for configuring some popular SaaS apps for use with Azure AD B2B.</span></span>

<span data-ttu-id="a9c93-106">Mielőtt az alkalmazás-specifikus utasításokkal tekinti meg, az alábbiakban néhány szabályok megoldás:</span><span class="sxs-lookup"><span data-stu-id="a9c93-106">Before you look at app-specific instructions, here are some rules of thumb:</span></span>

* <span data-ttu-id="a9c93-107">A legtöbb hello alkalmazások, a felhasználó a telepítőnek toohappen manuálisan.</span><span class="sxs-lookup"><span data-stu-id="a9c93-107">For most of hello apps, user setup needs toohappen manually.</span></span> <span data-ttu-id="a9c93-108">Ez azt jelenti, hogy felhasználók manuálisan kell létrehozni, valamint hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a9c93-108">That is, users must be created manually in hello app as well.</span></span>

* <span data-ttu-id="a9c93-109">Automatikus telepítés, például a Dropbox, támogató alkalmazások esetében külön meghívókat hello alkalmazásokból jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="a9c93-109">For apps that support automatic setup, such as Dropbox, separate invitations are created from hello apps.</span></span> <span data-ttu-id="a9c93-110">Felhasználók kell lennie, hogy tooaccept minden meghívó.</span><span class="sxs-lookup"><span data-stu-id="a9c93-110">Users must be sure tooaccept each invitation.</span></span>

* <span data-ttu-id="a9c93-111">Hello felhasználói attribútumok toomitigate esetleges problémáinak összekeveredett felhasználói profil lemezre (UPD) a vendégfelhasználók, mindig beállította **felhasználói azonosító** túl**user.mail**.</span><span class="sxs-lookup"><span data-stu-id="a9c93-111">In hello user attributes, toomitigate any issues with mangled user profile disk (UPD) in guest users, always set **User Identifier** too**user.mail**.</span></span>


## <a name="dropbox-business"></a><span data-ttu-id="a9c93-112">Dropbox üzleti</span><span class="sxs-lookup"><span data-stu-id="a9c93-112">Dropbox Business</span></span>

<span data-ttu-id="a9c93-113">tooenable felhasználók toosign a szervezeti fiókjával be, manuálisan kell konfigurálni az Azure AD Dropbox üzleti toouse Security Assertion Markup Language (SAML) identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="a9c93-113">tooenable users toosign in using their organization account, you must manually configure Dropbox Business toouse Azure AD as a Security Assertion Markup Language (SAML) identity provider.</span></span> <span data-ttu-id="a9c93-114">Ha Dropbox üzleti nem konfigurált toodo tehát nem kérni és egyébként teszik lehetővé a felhasználók toosign az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="a9c93-114">If Dropbox Business has not been configured toodo so, it cannot prompt or otherwise allow users toosign in using Azure AD.</span></span>

1. <span data-ttu-id="a9c93-115">tooadd hello Dropbox üzleti alkalmazást az Azure AD, válassza ki **vállalati alkalmazások** hello bal oldali ablaktáblán, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a9c93-115">tooadd hello Dropbox Business app into Azure AD, select **Enterprise applications** in hello left pane, and then click **Add**.</span></span>

  ![hello vállalati alkalmazások lapon hello "Hozzáadás" gombra](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. <span data-ttu-id="a9c93-117">A hello **alkalmazás hozzáadása** ablak, írja be **dropbox** hello a keresési mezőbe, és válassza ki **Dropbox vállalati** hello eredménylistában.</span><span class="sxs-lookup"><span data-stu-id="a9c93-117">In hello **Add an application** window, enter **dropbox** in hello search box, and then select **Dropbox for Business** in hello results list.</span></span>

  ![Keressen a "dropbox" hello egy alkalmazás-weblap hozzáadása](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. <span data-ttu-id="a9c93-119">A hello **egyszeri bejelentkezés** lapon jelölje be **egyszeri bejelentkezés** a hello bal oldali ablaktáblán, és írja be **user.mail** a hello **felhasználói azonosító** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a9c93-119">On hello **Single sign-on** page, select **Single sign-on** in hello left pane, and then enter **user.mail** in hello **User Identifier** box.</span></span> <span data-ttu-id="a9c93-120">(Érték szerint UPN alapértelmezés szerint.)</span><span class="sxs-lookup"><span data-stu-id="a9c93-120">(It's set as UPN by default.)</span></span>

  ![Egyszeri bejelentkezés hello alkalmazás konfigurálása](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. <span data-ttu-id="a9c93-122">toodownload hello tanúsítvány toouse Dropbox-konfigurációhoz, válassza ki **konfigurálása DropBox**, majd válassza ki **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím** hello listában.</span><span class="sxs-lookup"><span data-stu-id="a9c93-122">toodownload hello certificate toouse for Dropbox configuration, select **Configure DropBox**, and then select **SAML Single Sign On Service URL** in hello list.</span></span>

  ![Dropbox konfigurációs hello tanúsítvány letöltése](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. <span data-ttu-id="a9c93-124">Jelentkezzen be a hello tooDropbox bejelentkezés URL-címet a hello **egyszeri bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="a9c93-124">Sign in tooDropbox with hello sign-on URL from hello **Single sign-on** page.</span></span>

  ![hello Dropbox-bejelentkezés lap](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. <span data-ttu-id="a9c93-126">A hello menüben válassza **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="a9c93-126">On hello menu, select **Admin Console**.</span></span>

  ![hello "Felügyeleti konzol" hivatkozásra kattintva hello Dropbox menü](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. <span data-ttu-id="a9c93-128">A hello **hitelesítési** párbeszédpanelen jelölje ki **további**, hello-tanúsítvány feltöltése, a hello **jelentkezzen be az URL-cím** SAML-alapú egyszeri bejelentkezést hello URL-címet adja meg.</span><span class="sxs-lookup"><span data-stu-id="a9c93-128">In hello **Authentication** dialog box, select **More**, upload hello certificate and then, in hello **Sign in URL** box, enter hello SAML single sign-on URL.</span></span>

  ![hello hello "Több" hivatkozásra összecsukott párbeszédpanel](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![a hello "Bejelentkezési URL-a" Hello kibontva párbeszédpanel](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. <span data-ttu-id="a9c93-131">tooconfigure automatikus felhasználó beállítása a hello Azure-portálon válassza **kiépítési** hello bal oldali ablaktáblában jelöljön ki **automatikus** a hello **kiépítési üzemmódban** mezőbe, majd válassza ki **Engedélyezik**.</span><span class="sxs-lookup"><span data-stu-id="a9c93-131">tooconfigure automatic user setup in hello Azure portal, select **Provisioning** in hello left pane, select **Automatic** in hello **Provisioning Mode** box, and then select **Authorize**.</span></span>

  ![Automatikus felhasználók átadására a hello Azure-portálon](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

<span data-ttu-id="a9c93-133">Vendég vagy tag felhasználók hello Dropbox alkalmazásban van beállítva, akkor kapnak egy külön meghívó az dropbox-bA.</span><span class="sxs-lookup"><span data-stu-id="a9c93-133">After guest or member users have been set up in hello Dropbox app, they receive a separate invitation from Dropbox.</span></span> <span data-ttu-id="a9c93-134">toouse Dropbox egyszeri bejelentkezést, a meghívott személyeknek el kell fogadnia hello felkérést egy hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="a9c93-134">toouse Dropbox single sign-on, invitees must accept hello invitation by clicking a link in it.</span></span>

## <a name="box"></a><span data-ttu-id="a9c93-135">Box</span><span class="sxs-lookup"><span data-stu-id="a9c93-135">Box</span></span>
<span data-ttu-id="a9c93-136">Összevonási alapuló hello SAML protokoll segítségével lehetővé teheti a felhasználók tooauthenticate mezőben vendég felhasználók az Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="a9c93-136">You can enable users tooauthenticate Box guest users with their Azure AD account by using federation that's based on hello SAML protocol.</span></span> <span data-ttu-id="a9c93-137">Ezzel az eljárással metaadatok tooBox.com feltöltése.</span><span class="sxs-lookup"><span data-stu-id="a9c93-137">In this procedure, you upload metadata tooBox.com.</span></span>

1. <span data-ttu-id="a9c93-138">Hello vállalati alkalmazások hello Box alkalmazásához adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="a9c93-138">Add hello Box app from hello enterprise apps.</span></span>

2. <span data-ttu-id="a9c93-139">Egyszeri bejelentkezés konfigurálása sorrend hello:</span><span class="sxs-lookup"><span data-stu-id="a9c93-139">Configure single sign-on in hello following order:</span></span>

  ![Mezőbe az egyszeri bejelentkezés konfigurálása](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 <span data-ttu-id="a9c93-141">a.</span><span class="sxs-lookup"><span data-stu-id="a9c93-141">a.</span></span> <span data-ttu-id="a9c93-142">A hello **bejelentkezési URL-cím** győződjön meg arról, hogy hello bejelentkezési URL-cím értékre van megfelelően a Box hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a9c93-142">In hello **Sign on URL** box, ensure that hello sign-on URL is set appropriately for Box in hello Azure portal.</span></span> <span data-ttu-id="a9c93-143">Az URL-cím a Box.com bérlő hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a9c93-143">This URL is hello URL of your Box.com tenant.</span></span> <span data-ttu-id="a9c93-144">Akkor érdemes követnie hello elnevezési *https://.box.com*.</span><span class="sxs-lookup"><span data-stu-id="a9c93-144">It should follow hello naming convention *https://.box.com*.</span></span>  
 <span data-ttu-id="a9c93-145">Hello **azonosító** toothis nem felel meg az alkalmazás, de azt is megjelenik a kötelező mező.</span><span class="sxs-lookup"><span data-stu-id="a9c93-145">hello **Identifier** does not apply toothis app, but it still appears as a mandatory field.</span></span>

 <span data-ttu-id="a9c93-146">b.</span><span class="sxs-lookup"><span data-stu-id="a9c93-146">b.</span></span> <span data-ttu-id="a9c93-147">A hello **felhasználói azonosító** adja meg a **user.mail** (az egyszeri bejelentkezés a Vendég-fiókok).</span><span class="sxs-lookup"><span data-stu-id="a9c93-147">In hello **User identifier** box, enter **user.mail** (for SSO for guest accounts).</span></span>

 <span data-ttu-id="a9c93-148">c.</span><span class="sxs-lookup"><span data-stu-id="a9c93-148">c.</span></span> <span data-ttu-id="a9c93-149">A **SAML-aláíró tanúsítványa**, kattintson a **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="a9c93-149">Under **SAML Signing Certificate**, click **Create new certificate**.</span></span>

 <span data-ttu-id="a9c93-150">d.</span><span class="sxs-lookup"><span data-stu-id="a9c93-150">d.</span></span> <span data-ttu-id="a9c93-151">a Box.com bérlői toouse az Azure AD konfigurálása identitás-szolgáltatóként, toobegin hello metaadatait tartalmazó fájl letöltése, és mentse azt tooyour helyi meghajtó.</span><span class="sxs-lookup"><span data-stu-id="a9c93-151">toobegin configuring your Box.com tenant toouse Azure AD as an identity provider, download hello metadata file and then save it tooyour local drive.</span></span>

 <span data-ttu-id="a9c93-152">e.</span><span class="sxs-lookup"><span data-stu-id="a9c93-152">e.</span></span> <span data-ttu-id="a9c93-153">Továbbítsa hello metaadatok fájl toohello mezőben támogatási csoport, amely egyszeri bejelentkezést az Ön konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="a9c93-153">Forward hello metadata file toohello Box support team, which configures single sign-on for you.</span></span>

3. <span data-ttu-id="a9c93-154">Az Azure AD automatikus felhasználóbeállítás hello bal oldali ablaktáblában válassza a **kiépítési**, majd válassza ki **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="a9c93-154">For Azure AD automatic user setup, in hello left pane, select **Provisioning**, and then select **Authorize**.</span></span>

  ![Az Azure AD tooconnect tooBox engedélyezése](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

<span data-ttu-id="a9c93-156">Például a Dropbox a meghívott személyeknek mezőben a meghívott személyeknek kell beváltani a meghívó hello mezőben alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="a9c93-156">Like Dropbox invitees, Box invitees must redeem their invitation from hello Box app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9c93-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9c93-157">Next steps</span></span>

<span data-ttu-id="a9c93-158">Tekintse meg a következő cikkek az Azure AD B2B együttműködés hello:</span><span class="sxs-lookup"><span data-stu-id="a9c93-158">See hello following articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="a9c93-159">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="a9c93-159">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="a9c93-160">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="a9c93-160">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="a9c93-161">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9c93-161">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="a9c93-162">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="a9c93-162">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="a9c93-163">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="a9c93-163">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="a9c93-164">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="a9c93-164">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="a9c93-165">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="a9c93-165">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="a9c93-166">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="a9c93-166">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="a9c93-167">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="a9c93-167">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="a9c93-168">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="a9c93-168">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
