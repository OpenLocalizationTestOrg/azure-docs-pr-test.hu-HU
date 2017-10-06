---
title: "Oktatóanyag: Azure Active Directoryval integrált PagerDuty |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PagerDuty között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c3cfbedac3bf075e2d8cd833d5de7ca0bc9468b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a><span data-ttu-id="8edb6-103">Oktatóanyag: Azure Active Directoryval integrált PagerDuty</span><span class="sxs-lookup"><span data-stu-id="8edb6-103">Tutorial: Azure Active Directory integration with PagerDuty</span></span>

<span data-ttu-id="8edb6-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PagerDuty az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8edb6-104">In this tutorial, you learn how toointegrate PagerDuty with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8edb6-105">PagerDuty integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8edb6-105">Integrating PagerDuty with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8edb6-106">Megadhatja a hozzáférés tooPagerDuty rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8edb6-106">You can control in Azure AD who has access tooPagerDuty</span></span>
- <span data-ttu-id="8edb6-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPagerDuty (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8edb6-107">You can enable your users tooautomatically get signed-on tooPagerDuty (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8edb6-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8edb6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8edb6-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8edb6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8edb6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8edb6-110">Prerequisites</span></span>

<span data-ttu-id="8edb6-111">az Azure AD integrálása PagerDuty tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8edb6-111">tooconfigure Azure AD integration with PagerDuty, you need hello following items:</span></span>

- <span data-ttu-id="8edb6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8edb6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8edb6-113">Egy PagerDuty egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8edb6-113">A PagerDuty single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8edb6-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8edb6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8edb6-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8edb6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8edb6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8edb6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8edb6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8edb6-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8edb6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8edb6-118">Scenario description</span></span>
<span data-ttu-id="8edb6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8edb6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8edb6-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8edb6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8edb6-121">Hello gyűjteményből PagerDuty hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8edb6-121">Adding PagerDuty from hello gallery</span></span>
2. <span data-ttu-id="8edb6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8edb6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pagerduty-from-hello-gallery"></a><span data-ttu-id="8edb6-123">Hello gyűjteményből PagerDuty hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8edb6-123">Adding PagerDuty from hello gallery</span></span>
<span data-ttu-id="8edb6-124">tooconfigure hello integrációja PagerDuty az Azure AD-be, meg kell tooadd PagerDuty hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8edb6-124">tooconfigure hello integration of PagerDuty into Azure AD, you need tooadd PagerDuty from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8edb6-125">**tooadd PagerDuty hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8edb6-125">**tooadd PagerDuty from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8edb6-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8edb6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="8edb6-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8edb6-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="8edb6-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8edb6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="8edb6-133">Hello keresési mezőbe, írja be a **PagerDuty**, jelölje be **PagerDuty** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8edb6-133">In hello search box, type **PagerDuty**, select  **PagerDuty**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8edb6-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8edb6-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="8edb6-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="8edb6-136">In this section, you configure and test Azure AD single sign-on with PagerDuty based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8edb6-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PagerDuty tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8edb6-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PagerDuty is tooa user in Azure AD.</span></span> <span data-ttu-id="8edb6-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PagerDuty közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8edb6-138">In other words, a link relationship between an Azure AD user and hello related user in PagerDuty needs toobe established.</span></span>

<span data-ttu-id="8edb6-139">PagerDuty, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8edb6-139">In PagerDuty, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8edb6-140">tooconfigure és az Azure AD az egyszeri bejelentkezés PagerDuty-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8edb6-140">tooconfigure and test Azure AD single sign-on with PagerDuty, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8edb6-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8edb6-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8edb6-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8edb6-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8edb6-143">**[PagerDuty tesztfelhasználó létrehozása](#create-a-pagerduty-test-user)**  -toohave egy megfelelője a Britta Simon a PagerDuty, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8edb6-143">**[Create a PagerDuty test user](#create-a-pagerduty-test-user)** - toohave a counterpart of Britta Simon in PagerDuty that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8edb6-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8edb6-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8edb6-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8edb6-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8edb6-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8edb6-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8edb6-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az PagerDuty alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8edb6-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PagerDuty application.</span></span>

<span data-ttu-id="8edb6-148">**az Azure AD tooconfigure egyszeri bejelentkezést a PagerDuty, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8edb6-148">**tooconfigure Azure AD single sign-on with PagerDuty, perform hello following steps:**</span></span>

1. <span data-ttu-id="8edb6-149">Az Azure portál, a hello hello **PagerDuty** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-149">In hello Azure portal, on hello **PagerDuty** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="8edb6-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8edb6-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. <span data-ttu-id="8edb6-153">A hello **PagerDuty tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8edb6-153">On hello **PagerDuty Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk PagerDuty tartomány és az URL-címek](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    <span data-ttu-id="8edb6-155">a.</span><span class="sxs-lookup"><span data-stu-id="8edb6-155">a.</span></span> <span data-ttu-id="8edb6-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="8edb6-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    <span data-ttu-id="8edb6-157">b.</span><span class="sxs-lookup"><span data-stu-id="8edb6-157">b.</span></span> <span data-ttu-id="8edb6-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="8edb6-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8edb6-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8edb6-159">These values are not real.</span></span> <span data-ttu-id="8edb6-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="8edb6-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8edb6-161">Ügyfél [PagerDuty ügyfél-támogatási csoport](https://www.pagerduty.com/support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8edb6-161">Contact [PagerDuty Client support team](https://www.pagerduty.com/support/) tooget these values.</span></span> 

4. <span data-ttu-id="8edb6-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8edb6-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. <span data-ttu-id="8edb6-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8edb6-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8edb6-166">A hello **PagerDuty konfigurációs** kattintson **konfigurálása PagerDuty** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8edb6-166">On hello **PagerDuty Configuration** section, click **Configure PagerDuty** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8edb6-167">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8edb6-167">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![PagerDuty konfiguráció](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. <span data-ttu-id="8edb6-169">Egy másik webes böngészőablakban jelentkezzen be a Pagerduty vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8edb6-169">In a different web browser window, log into your Pagerduty company site as an administrator.</span></span>

8. <span data-ttu-id="8edb6-170">Hello hello felső menüben kattintson a **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-170">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="8edb6-171">![Fiókbeállítások](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="8edb6-171">![Account Settings](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Account Settings")</span></span>

9. <span data-ttu-id="8edb6-172">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-172">Click **Single Sign-on**.</span></span>
   
    <span data-ttu-id="8edb6-173">![Egyszeri bejelentkezés](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="8edb6-173">![Single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "Single sign-on")</span></span>

10. <span data-ttu-id="8edb6-174">A hello **engedélyezése egyszeri bejelentkezés (SSO)** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8edb6-174">On hello **Enable Single Sign-on (SSO)** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="8edb6-175">![Egyszeri bejelentkezés engedélyezése](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "egyszeri bejelentkezés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="8edb6-175">![Enable single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "Enable single sign-on")</span></span>
   
    <span data-ttu-id="8edb6-176">a.</span><span class="sxs-lookup"><span data-stu-id="8edb6-176">a.</span></span> <span data-ttu-id="8edb6-177">A base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello Azure portálról letöltött megnyitása és toohello Beillesztés **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="8edb6-177">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>
  
    <span data-ttu-id="8edb6-178">b.</span><span class="sxs-lookup"><span data-stu-id="8edb6-178">b.</span></span> <span data-ttu-id="8edb6-179">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8edb6-179">In hello **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="8edb6-180">c.</span><span class="sxs-lookup"><span data-stu-id="8edb6-180">c.</span></span> <span data-ttu-id="8edb6-181">A hello **kijelentkezési URL-cím** szövegmezőhöz Beillesztés **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8edb6-181">In hello **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="8edb6-182">d.</span><span class="sxs-lookup"><span data-stu-id="8edb6-182">d.</span></span> <span data-ttu-id="8edb6-183">Válassza ki **kapcsolja be egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-183">Select **Turn on Single Sign-on**.</span></span>
 
    <span data-ttu-id="8edb6-184">e.</span><span class="sxs-lookup"><span data-stu-id="8edb6-184">e.</span></span> <span data-ttu-id="8edb6-185">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-185">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="8edb6-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8edb6-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8edb6-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8edb6-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8edb6-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8edb6-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8edb6-189">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="8edb6-189">Create an Azure AD test user</span></span>

<span data-ttu-id="8edb6-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8edb6-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="8edb6-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8edb6-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8edb6-193">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8edb6-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8edb6-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8edb6-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8edb6-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8edb6-199">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8edb6-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8edb6-201">a.</span><span class="sxs-lookup"><span data-stu-id="8edb6-201">a.</span></span> <span data-ttu-id="8edb6-202">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8edb6-203">b.</span><span class="sxs-lookup"><span data-stu-id="8edb6-203">b.</span></span> <span data-ttu-id="8edb6-204">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8edb6-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8edb6-205">c.</span><span class="sxs-lookup"><span data-stu-id="8edb6-205">c.</span></span> <span data-ttu-id="8edb6-206">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8edb6-207">d.</span><span class="sxs-lookup"><span data-stu-id="8edb6-207">d.</span></span> <span data-ttu-id="8edb6-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8edb6-208">Click **Create**.</span></span>
 
### <a name="create-a-pagerduty-test-user"></a><span data-ttu-id="8edb6-209">PagerDuty tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8edb6-209">Create a PagerDuty test user</span></span>

<span data-ttu-id="8edb6-210">az Azure AD tooenable felhasználók toolog a tooPagerDuty, akkor ki kell építenie PagerDuty be.</span><span class="sxs-lookup"><span data-stu-id="8edb6-210">tooenable Azure AD users toolog in tooPagerDuty, they must be provisioned into PagerDuty.</span></span>  
<span data-ttu-id="8edb6-211">PagerDuty hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8edb6-211">In hello case of PagerDuty, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="8edb6-212">Bármely más Pagerduty felhasználói fiók létrehozása eszközök vagy Pagerduty tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="8edb6-212">You can use any other Pagerduty user account creation tools or APIs provided by Pagerduty tooprovision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="8edb6-213">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8edb6-213">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8edb6-214">Jelentkezzen be tooyour **Pagerduty** bérlő.</span><span class="sxs-lookup"><span data-stu-id="8edb6-214">Log in tooyour **Pagerduty** tenant.</span></span>

2. <span data-ttu-id="8edb6-215">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-215">In hello menu on hello top, click **Users**.</span></span>

3. <span data-ttu-id="8edb6-216">Kattintson a **felhasználók hozzáadása az**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-216">Click **Add Users**.</span></span>
   
    <span data-ttu-id="8edb6-217">![Felhasználók hozzáadása az](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8edb6-217">![Add Users](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "Add Users")</span></span>

4.  <span data-ttu-id="8edb6-218">A hello **hívhat meg a csoport** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8edb6-218">On hello **Invite your team** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="8edb6-219">![A csoport meghívása](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "hívhat meg a csoport")</span><span class="sxs-lookup"><span data-stu-id="8edb6-219">![Invite your team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "Invite your team")</span></span>

    <span data-ttu-id="8edb6-220">a.</span><span class="sxs-lookup"><span data-stu-id="8edb6-220">a.</span></span> <span data-ttu-id="8edb6-221">Típus hello **első és utolsó neve** például a felhasználó **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-221">Type hello **First and Last Name** of user like **Britta Simon**.</span></span> 
   
    <span data-ttu-id="8edb6-222">b.</span><span class="sxs-lookup"><span data-stu-id="8edb6-222">b.</span></span> <span data-ttu-id="8edb6-223">Adja meg **E-mail** felhasználó címét, például  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="8edb6-223">Enter **Email** address of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="8edb6-224">c.</span><span class="sxs-lookup"><span data-stu-id="8edb6-224">c.</span></span> <span data-ttu-id="8edb6-225">Kattintson a **Hozzáadás**, és kattintson a **küldése felkéri**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-225">Click **Add**, and then click **Send Invites**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="8edb6-226">Minden hozzáadott felhasználó kap egy összehíváshoz toocreate egy PagerDuty fiókot.</span><span class="sxs-lookup"><span data-stu-id="8edb6-226">All added users will receive an invite toocreate a PagerDuty account.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="8edb6-227">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="8edb6-227">Assign hello Azure AD test user</span></span>

<span data-ttu-id="8edb6-228">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPagerDuty megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8edb6-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPagerDuty.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="8edb6-230">**tooassign Britta Simon tooPagerDuty, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8edb6-230">**tooassign Britta Simon tooPagerDuty, perform hello following steps:**</span></span>

1. <span data-ttu-id="8edb6-231">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8edb6-233">Hello alkalmazások listában válassza ki a **PagerDuty**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-233">In hello applications list, select **PagerDuty**.</span></span>

    ![hello PagerDuty hivatkozásra hello alkalmazások listája](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. <span data-ttu-id="8edb6-235">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8edb6-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="8edb6-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8edb6-237">Click **Add** button.</span></span> <span data-ttu-id="8edb6-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8edb6-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="8edb6-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8edb6-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8edb6-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8edb6-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8edb6-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8edb6-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8edb6-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8edb6-243">Test single sign-on</span></span>

<span data-ttu-id="8edb6-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8edb6-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8edb6-245">Kattintva hello PagerDuty csempe az Access Panelyou hello kapja meg automatikusan bejelentkezett tooyour PagerDuty alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8edb6-245">When you click hello PagerDuty tile in hello Access Panelyou should get automatically signed-on tooyour PagerDuty application.</span></span>

<span data-ttu-id="8edb6-246">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8edb6-246">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8edb6-247">További források</span><span class="sxs-lookup"><span data-stu-id="8edb6-247">Additional resources</span></span>

* [<span data-ttu-id="8edb6-248">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8edb6-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8edb6-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8edb6-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png

