---
title: "Oktatóanyag: Azure Active Directoryval integrált UserVoice |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a UserVoice között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 9eade8435ae6c6a3821bbbec9ab7c27ed7ad91ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a><span data-ttu-id="8033d-103">Oktatóanyag: Azure Active Directoryval integrált UserVoice</span><span class="sxs-lookup"><span data-stu-id="8033d-103">Tutorial: Azure Active Directory integration with UserVoice</span></span>

<span data-ttu-id="8033d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate UserVoice az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8033d-104">In this tutorial, you learn how toointegrate UserVoice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8033d-105">UserVoice integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-105">Integrating UserVoice with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8033d-106">Az Azure AD hozzáférési tooUserVoice rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="8033d-106">You can control in Azure AD who has access tooUserVoice.</span></span>
- <span data-ttu-id="8033d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooUserVoice (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="8033d-107">You can enable your users tooautomatically get signed-on tooUserVoice (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="8033d-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="8033d-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="8033d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8033d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8033d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8033d-110">Prerequisites</span></span>

<span data-ttu-id="8033d-111">az Azure AD-integráció a UserVoice tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-111">tooconfigure Azure AD integration with UserVoice, you need hello following items:</span></span>

- <span data-ttu-id="8033d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8033d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8033d-113">A UserVoice egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8033d-113">A UserVoice single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8033d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8033d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8033d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8033d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8033d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8033d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8033d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8033d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8033d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8033d-118">Scenario description</span></span>
<span data-ttu-id="8033d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8033d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8033d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8033d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8033d-121">Hello gyűjteményből UserVoice hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8033d-121">Adding UserVoice from hello gallery</span></span>
2. <span data-ttu-id="8033d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8033d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-uservoice-from-hello-gallery"></a><span data-ttu-id="8033d-123">Hello gyűjteményből UserVoice hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8033d-123">Adding UserVoice from hello gallery</span></span>
<span data-ttu-id="8033d-124">tooconfigure hello integrációja UserVoice az Azure AD-be, meg kell tooadd UserVoice hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8033d-124">tooconfigure hello integration of UserVoice into Azure AD, you need tooadd UserVoice from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8033d-125">**tooadd UserVoice hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8033d-125">**tooadd UserVoice from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8033d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8033d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="8033d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8033d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8033d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8033d-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="8033d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8033d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="8033d-133">Hello keresési mezőbe, írja be a **UserVoice**, jelölje be **UserVoice** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8033d-133">In hello search box, type **UserVoice**, select **UserVoice** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8033d-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8033d-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="8033d-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UserVoice.</span><span class="sxs-lookup"><span data-stu-id="8033d-136">In this section, you configure and test Azure AD single sign-on with UserVoice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8033d-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a UserVoice tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8033d-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UserVoice is tooa user in Azure AD.</span></span> <span data-ttu-id="8033d-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a UserVoice közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="8033d-138">In other words, a link relationship between an Azure AD user and hello related user in UserVoice needs toobe established.</span></span>

<span data-ttu-id="8033d-139">UserVoice, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8033d-139">In UserVoice, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8033d-140">tooconfigure és az Azure AD az egyszeri bejelentkezés UserVoice-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8033d-140">tooconfigure and test Azure AD single sign-on with UserVoice, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8033d-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8033d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8033d-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8033d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8033d-143">**[UserVoice tesztfelhasználó létrehozása](#create-a-uservoice-test-user)**  -toohave egy megfelelője a Britta Simon a UserVoice, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8033d-143">**[Create a UserVoice test user](#create-a-uservoice-test-user)** - toohave a counterpart of Britta Simon in UserVoice that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8033d-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8033d-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8033d-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8033d-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8033d-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8033d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8033d-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a UserVoice-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8033d-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UserVoice application.</span></span>

<span data-ttu-id="8033d-148">**az Azure AD tooconfigure egyszeri bejelentkezést a UserVoice, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8033d-148">**tooconfigure Azure AD single sign-on with UserVoice, perform hello following steps:**</span></span>

1. <span data-ttu-id="8033d-149">Az Azure portál, a hello hello **UserVoice** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8033d-149">In hello Azure portal, on hello **UserVoice** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="8033d-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8033d-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. <span data-ttu-id="8033d-153">A hello **UserVoice tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-153">On hello **UserVoice Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információt UserVoice tartomány és az URL-címek](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    <span data-ttu-id="8033d-155">a.</span><span class="sxs-lookup"><span data-stu-id="8033d-155">a.</span></span> <span data-ttu-id="8033d-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="8033d-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    <span data-ttu-id="8033d-157">b.</span><span class="sxs-lookup"><span data-stu-id="8033d-157">b.</span></span> <span data-ttu-id="8033d-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="8033d-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8033d-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8033d-159">These values are not real.</span></span> <span data-ttu-id="8033d-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="8033d-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8033d-161">Ügyfél [UserVoice ügyfél-támogatási csoport](https://www.uservoice.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8033d-161">Contact [UserVoice Client support team](https://www.uservoice.com/) tooget these values.</span></span>

4. <span data-ttu-id="8033d-162">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="8033d-162">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. <span data-ttu-id="8033d-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8033d-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8033d-166">A hello **UserVoice konfigurációs** kattintson **konfigurálása UserVoice** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8033d-166">On hello **UserVoice Configuration** section, click **Configure UserVoice** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8033d-167">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8033d-167">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![UserVoice-konfiguráció](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. <span data-ttu-id="8033d-169">Egy másik webes böngészőablakban jelentkezzen tooyour UserVoice vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8033d-169">In a different web browser window, log in tooyour UserVoice company site as an administrator.</span></span>

8. <span data-ttu-id="8033d-170">Hello hello felső eszköztárán kattintson **beállítások**, majd válassza ki **webes portál** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="8033d-170">In hello toolbar on hello top, click **Settings**, and then select **Web portal** from hello menu.</span></span>
   
    <span data-ttu-id="8033d-171">![Az alkalmazás ügyféloldali beállítások szakaszban](./media/active-directory-saas-uservoice-tutorial/ic777519.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="8033d-171">![Settings Section On App Side](./media/active-directory-saas-uservoice-tutorial/ic777519.png "Settings")</span></span>

9. <span data-ttu-id="8033d-172">A hello **webes portál** lap hello **felhasználóhitelesítés** területén kattintson **szerkesztése** tooopen hello **felhasználói hitelesítés szerkesztése** párbeszédpanel lap.</span><span class="sxs-lookup"><span data-stu-id="8033d-172">On hello **Web portal** tab, in hello **User authentication** section, click **Edit** tooopen hello **Edit User Authentication** dialog page.</span></span>
   
    <span data-ttu-id="8033d-173">![Webes portál lapon](./media/active-directory-saas-uservoice-tutorial/ic777520.png "webes portál")</span><span class="sxs-lookup"><span data-stu-id="8033d-173">![Web portal Tab](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web portal")</span></span>

10. <span data-ttu-id="8033d-174">A hello **felhasználói hitelesítés szerkesztése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-174">On hello **Edit User Authentication** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="8033d-175">![Felhasználói hitelesítés szerkesztése](./media/active-directory-saas-uservoice-tutorial/ic777521.png "felhasználói hitelesítés szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="8033d-175">![Edit user authentication](./media/active-directory-saas-uservoice-tutorial/ic777521.png "Edit user authentication")</span></span>
   
    <span data-ttu-id="8033d-176">a.</span><span class="sxs-lookup"><span data-stu-id="8033d-176">a.</span></span> <span data-ttu-id="8033d-177">Kattintson a **egyszeri bejelentkezés (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="8033d-177">Click **Single Sign-On (SSO)**.</span></span>
 
    <span data-ttu-id="8033d-178">b.</span><span class="sxs-lookup"><span data-stu-id="8033d-178">b.</span></span> <span data-ttu-id="8033d-179">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **SSO távoli bejelentkezés** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8033d-179">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **SSO Remote Sign-In** textbox.</span></span>

    <span data-ttu-id="8033d-180">c.</span><span class="sxs-lookup"><span data-stu-id="8033d-180">c.</span></span> <span data-ttu-id="8033d-181">Beillesztés hello **Sign-Out URL-cím** értéket, amely akkor másolta, az Azure-portálon hello hello **SSO távoli Sign-Out szövegmező**.</span><span class="sxs-lookup"><span data-stu-id="8033d-181">Paste hello **Sign-Out URL** value, which you have copied from hello Azure portal into hello **SSO Remote Sign-Out textbox**.</span></span>
 
    <span data-ttu-id="8033d-182">d.</span><span class="sxs-lookup"><span data-stu-id="8033d-182">d.</span></span> <span data-ttu-id="8033d-183">Beillesztés hello **ujjlenyomat** értéket, amely az Azure portálról másolta a **aktuális SHA1 tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8033d-183">Paste hello **Thumbprint** value , which you have copied from Azure portal  into the **Current certificate SHA1 fingerprint** textbox.</span></span>
    
    <span data-ttu-id="8033d-184">e.</span><span class="sxs-lookup"><span data-stu-id="8033d-184">e.</span></span> <span data-ttu-id="8033d-185">Kattintson a **hitelesítési beállításainak mentése**.</span><span class="sxs-lookup"><span data-stu-id="8033d-185">Click **Save authentication settings**.</span></span>

> [!TIP]
> <span data-ttu-id="8033d-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8033d-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8033d-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8033d-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8033d-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8033d-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8033d-189">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="8033d-189">Create an Azure AD test user</span></span>

<span data-ttu-id="8033d-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8033d-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="8033d-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8033d-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8033d-193">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="8033d-193">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="8033d-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8033d-195">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="8033d-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="8033d-197">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="8033d-199">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-199">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    <span data-ttu-id="8033d-201">a.</span><span class="sxs-lookup"><span data-stu-id="8033d-201">a.</span></span> <span data-ttu-id="8033d-202">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8033d-202">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8033d-203">b.</span><span class="sxs-lookup"><span data-stu-id="8033d-203">b.</span></span> <span data-ttu-id="8033d-204">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="8033d-204">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="8033d-205">c.</span><span class="sxs-lookup"><span data-stu-id="8033d-205">c.</span></span> <span data-ttu-id="8033d-206">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="8033d-206">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="8033d-207">d.</span><span class="sxs-lookup"><span data-stu-id="8033d-207">d.</span></span> <span data-ttu-id="8033d-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8033d-208">Click **Create**.</span></span>
 
### <a name="create-a-uservoice-test-user"></a><span data-ttu-id="8033d-209">UserVoice tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8033d-209">Create a UserVoice test user</span></span>

<span data-ttu-id="8033d-210">az Azure AD tooenable felhasználók toolog a tooUserVoice, akkor ki kell építenie a UserVoice.</span><span class="sxs-lookup"><span data-stu-id="8033d-210">tooenable Azure AD users toolog in tooUserVoice, they must be provisioned into UserVoice.</span></span> <span data-ttu-id="8033d-211">UserVoice hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8033d-211">In hello case of UserVoice, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="8033d-212">tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-212">tooprovision a user account, perform hello following steps:</span></span>
1. <span data-ttu-id="8033d-213">Jelentkezzen be tooyour **UserVoice** bérlő.</span><span class="sxs-lookup"><span data-stu-id="8033d-213">Log in tooyour **UserVoice** tenant.</span></span>

2. <span data-ttu-id="8033d-214">Nyissa meg túl**beállítások**.</span><span class="sxs-lookup"><span data-stu-id="8033d-214">Go too**Settings**.</span></span>
   
    <span data-ttu-id="8033d-215">![Beállítások](./media/active-directory-saas-uservoice-tutorial/ic777811.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="8033d-215">![Settings](./media/active-directory-saas-uservoice-tutorial/ic777811.png "Settings")</span></span>

3. <span data-ttu-id="8033d-216">Kattintson a **általános**.</span><span class="sxs-lookup"><span data-stu-id="8033d-216">Click **General**.</span></span>

4. <span data-ttu-id="8033d-217">Kattintson a **az ügynökök és az engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="8033d-217">Click **Agents and permissions**.</span></span>
   
    <span data-ttu-id="8033d-218">![Az ügynökök és az engedélyek](./media/active-directory-saas-uservoice-tutorial/ic777812.png "az ügynökök és engedélyek")</span><span class="sxs-lookup"><span data-stu-id="8033d-218">![Agents and permissions](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents and permissions")</span></span>

5. <span data-ttu-id="8033d-219">Kattintson a **rendszergazdák hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8033d-219">Click **Add admins**.</span></span>
   
    <span data-ttu-id="8033d-220">![Adja hozzá a rendszergazdák](./media/active-directory-saas-uservoice-tutorial/ic777813.png "rendszergazdák hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8033d-220">![Add admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "Add admins")</span></span>

6. <span data-ttu-id="8033d-221">A hello **rendszergazdák meghívása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8033d-221">On hello **Invite admins** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="8033d-222">![Rendszergazdák meghívása](./media/active-directory-saas-uservoice-tutorial/ic777814.png "rendszergazdák meghívása")</span><span class="sxs-lookup"><span data-stu-id="8033d-222">![Invite admins](./media/active-directory-saas-uservoice-tutorial/ic777814.png "Invite admins")</span></span>
   
    <span data-ttu-id="8033d-223">a.</span><span class="sxs-lookup"><span data-stu-id="8033d-223">a.</span></span> <span data-ttu-id="8033d-224">Hello e-mailek szövegmezőben, írja be a hello e-mail címét hello kívánt fiókra tooprovision, majd kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8033d-224">In hello Emails textbox, type hello email address of hello account you want tooprovision, and then click **Add**.</span></span>
   
    <span data-ttu-id="8033d-225">b.</span><span class="sxs-lookup"><span data-stu-id="8033d-225">b.</span></span> <span data-ttu-id="8033d-226">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="8033d-226">Click **Invite**.</span></span>

> [!NOTE]
> <span data-ttu-id="8033d-227">Bármely más UserVoice felhasználói fiók létrehozása eszközök vagy UserVoice tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="8033d-227">You can use any other UserVoice user account creation tools or APIs provided by UserVoice tooprovision AAD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="8033d-228">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="8033d-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="8033d-229">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooUserVoice megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8033d-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUserVoice.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="8033d-231">**tooassign Britta Simon tooUserVoice, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8033d-231">**tooassign Britta Simon tooUserVoice, perform hello following steps:**</span></span>

1. <span data-ttu-id="8033d-232">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8033d-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8033d-234">Hello alkalmazások listában válassza ki a **UserVoice**.</span><span class="sxs-lookup"><span data-stu-id="8033d-234">In hello applications list, select **UserVoice**.</span></span>

    ![hello UserVoice hivatkozásra hello alkalmazások listája](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. <span data-ttu-id="8033d-236">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8033d-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="8033d-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8033d-238">Click **Add** button.</span></span> <span data-ttu-id="8033d-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8033d-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="8033d-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8033d-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8033d-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8033d-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8033d-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8033d-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8033d-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8033d-244">Test single sign-on</span></span>

<span data-ttu-id="8033d-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8033d-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8033d-246">Ha a hozzáférési Panel hello hello UserVoice csempe gombra kattint, automatikusan bejelentkezett tooyour UserVoice alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="8033d-246">When you click hello UserVoice tile in hello Access Panel, you should get automatically signed-on tooyour UserVoice application.</span></span>
<span data-ttu-id="8033d-247">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8033d-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8033d-248">További források</span><span class="sxs-lookup"><span data-stu-id="8033d-248">Additional resources</span></span>

* [<span data-ttu-id="8033d-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8033d-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8033d-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8033d-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

