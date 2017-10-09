---
title: "Oktatóanyag: Azure Active Directory integrációja a TINFOIL SECURITY |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a TINFOIL SECURITY között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1a3fa9880d9e026c2d6d6548188df2269ff69139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a><span data-ttu-id="f0439-103">Oktatóanyag: A TINFOIL SECURITY Azure Active Directory-integráció</span><span class="sxs-lookup"><span data-stu-id="f0439-103">Tutorial: Azure Active Directory integration with TINFOIL SECURITY</span></span>

<span data-ttu-id="f0439-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TINFOIL SECURITY az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f0439-104">In this tutorial, you learn how toointegrate TINFOIL SECURITY with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f0439-105">A TINFOIL SECURITY integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f0439-105">Integrating TINFOIL SECURITY with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f0439-106">Megadhatja a hozzáférés tooTINFOIL biztonsági rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f0439-106">You can control in Azure AD who has access tooTINFOIL SECURITY</span></span>
- <span data-ttu-id="f0439-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTINFOIL biztonsági (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f0439-107">You can enable your users tooautomatically get signed-on tooTINFOIL SECURITY (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f0439-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f0439-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f0439-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f0439-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0439-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f0439-110">Prerequisites</span></span>

<span data-ttu-id="f0439-111">az Azure AD-integráció a TINFOIL SECURITY tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f0439-111">tooconfigure Azure AD integration with TINFOIL SECURITY, you need hello following items:</span></span>

- <span data-ttu-id="f0439-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f0439-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f0439-113">A TINFOIL SECURITY egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f0439-113">A TINFOIL SECURITY single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f0439-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f0439-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f0439-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f0439-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f0439-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f0439-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f0439-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0439-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f0439-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f0439-118">Scenario description</span></span>
<span data-ttu-id="f0439-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f0439-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f0439-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f0439-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f0439-121">Adja hozzá a TINFOIL SECURITY hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f0439-121">Add TINFOIL SECURITY from hello gallery</span></span>
2. <span data-ttu-id="f0439-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0439-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tinfoil-security-from-hello-gallery"></a><span data-ttu-id="f0439-123">Adja hozzá a TINFOIL SECURITY hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f0439-123">Add TINFOIL SECURITY from hello gallery</span></span>
<span data-ttu-id="f0439-124">tooconfigure hello integrációja a TINFOIL SECURITY az Azure AD-be, meg kell tooadd TINFOIL SECURITY hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f0439-124">tooconfigure hello integration of TINFOIL SECURITY into Azure AD, you need tooadd TINFOIL SECURITY from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f0439-125">**tooadd TINFOIL SECURITY hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f0439-125">**tooadd TINFOIL SECURITY from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0439-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f0439-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f0439-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f0439-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f0439-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f0439-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f0439-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f0439-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f0439-133">Hello keresési mezőbe, írja be a **TINFOIL SECURITY**, jelölje be **TINFOIL SECURITY** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f0439-133">In hello search box, type **TINFOIL SECURITY**, select  **TINFOIL SECURITY** from result panel then click **Add** button tooadd hello application.</span></span>

    ![A TINFOIL SECURITY gyűjteményből](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f0439-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0439-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="f0439-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezést a TINFOIL SECURITY "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="f0439-136">In this section, you configure and test Azure AD single sign-on with TINFOIL SECURITY based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f0439-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a TINFOIL SECURITY tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f0439-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TINFOIL SECURITY is tooa user in Azure AD.</span></span> <span data-ttu-id="f0439-138">Ez azt jelenti hello kapcsolódó felhasználó a TINFOIL SECURITY és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f0439-138">In other words, a link relationship between an Azure AD user and hello related user in TINFOIL SECURITY needs toobe established.</span></span>

<span data-ttu-id="f0439-139">A TINFOIL SECURITY, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f0439-139">In TINFOIL SECURITY, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f0439-140">tooconfigure és az Azure AD egyszeri bejelentkezést a TINFOIL SECURITY-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f0439-140">tooconfigure and test Azure AD single sign-on with TINFOIL SECURITY, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f0439-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f0439-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f0439-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0439-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f0439-143">**[A TINFOIL SECURITY tesztfelhasználó létrehozása](#create-a-tinfoil-security-test-user)**  -toohave egy megfelelője a Britta Simon a TINFOIL SECURITY, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f0439-143">**[Create a TINFOIL SECURITY test user](#create-a-tinfoil-security-test-user)** - toohave a counterpart of Britta Simon in TINFOIL SECURITY that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f0439-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f0439-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f0439-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f0439-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f0439-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0439-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f0439-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a TINFOIL SECURITY alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="f0439-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TINFOIL SECURITY application.</span></span>

<span data-ttu-id="f0439-148">**az Azure AD tooconfigure egyszeri bejelentkezést a TINFOIL SECURITY, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f0439-148">**tooconfigure Azure AD single sign-on with TINFOIL SECURITY, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0439-149">Az Azure portál, a hello hello **TINFOIL SECURITY** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f0439-149">In hello Azure portal, on hello **TINFOIL SECURITY** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f0439-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f0439-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. <span data-ttu-id="f0439-153">A hello **TINFOIL SECURITY tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="f0439-153">On hello **TINFOIL SECURITY Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. <span data-ttu-id="f0439-155">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** érték.</span><span class="sxs-lookup"><span data-stu-id="f0439-155">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. <span data-ttu-id="f0439-157">tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="f0439-157">tooadd hello required attribute mappings, perform hello following steps:</span></span>
    
    <span data-ttu-id="f0439-158">![Attribútumok](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="f0439-158">![Attributes](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "Attributes")</span></span>
    
    | <span data-ttu-id="f0439-159">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="f0439-159">Attribute Name</span></span>    |   <span data-ttu-id="f0439-160">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="f0439-160">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="f0439-161">accountid</span><span class="sxs-lookup"><span data-stu-id="f0439-161">accountid</span></span> | <span data-ttu-id="f0439-162">UXXXXXXXXXXXXX</span><span class="sxs-lookup"><span data-stu-id="f0439-162">UXXXXXXXXXXXXX</span></span> |
    
    <span data-ttu-id="f0439-163">a.</span><span class="sxs-lookup"><span data-stu-id="f0439-163">a.</span></span> <span data-ttu-id="f0439-164">Kattintson a **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="f0439-164">Click **add user attribute**.</span></span>
    
    <span data-ttu-id="f0439-165">![Hozzáadás attribútum](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="f0439-165">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "Attributes")</span></span>
    
    <span data-ttu-id="f0439-166">![Hozzáadás attribútum](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="f0439-166">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "Attributes")</span></span>
    
    <span data-ttu-id="f0439-167">b.</span><span class="sxs-lookup"><span data-stu-id="f0439-167">b.</span></span> <span data-ttu-id="f0439-168">A hello **attribútumnév** szövegmezőhöz típus **accountid**.</span><span class="sxs-lookup"><span data-stu-id="f0439-168">In hello **Attribute Name** textbox, type **accountid**.</span></span>
    
    <span data-ttu-id="f0439-169">c.</span><span class="sxs-lookup"><span data-stu-id="f0439-169">c.</span></span> <span data-ttu-id="f0439-170">A hello **attribútumérték** szövegmezőhöz Beillesztés hello fiók azonosító érték, amely később a hello oktatóanyag fog kapni.</span><span class="sxs-lookup"><span data-stu-id="f0439-170">In hello **Attribute Value** textbox, paste hello account ID value which you will get later on hello tutorial.</span></span>
    
    <span data-ttu-id="f0439-171">d.</span><span class="sxs-lookup"><span data-stu-id="f0439-171">d.</span></span> <span data-ttu-id="f0439-172">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f0439-172">Click **Ok**.</span></span>    

6. <span data-ttu-id="f0439-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f0439-173">Click **Save** button.</span></span>

    ![Mentés gombja](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="f0439-175">A hello **TINFOIL SECURITY Configuration** kattintson **konfigurálása a TINFOIL SECURITY** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f0439-175">On hello **TINFOIL SECURITY Configuration** section, click **Configure TINFOIL SECURITY** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f0439-176">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f0439-176">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![A TINFOIL SECURITY Configuration](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. <span data-ttu-id="f0439-178">Egy másik webes böngészőablakban jelentkezzen be a TINFOIL SECURITY vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f0439-178">In a different web browser window, log into your TINFOIL SECURITY company site as an administrator.</span></span>

9. <span data-ttu-id="f0439-179">Hello hello felső eszköztárán kattintson **saját fiók**.</span><span class="sxs-lookup"><span data-stu-id="f0439-179">In hello toolbar on hello top, click **My Account**.</span></span>
   
    <span data-ttu-id="f0439-180">![Irányítópult](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "irányítópult")</span><span class="sxs-lookup"><span data-stu-id="f0439-180">![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Dashboard")</span></span>

10. <span data-ttu-id="f0439-181">Kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="f0439-181">Click **Security**.</span></span>
   
    <span data-ttu-id="f0439-182">![Biztonsági](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="f0439-182">![Security](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "Security")</span></span>

11. <span data-ttu-id="f0439-183">A hello **egyszeri bejelentkezés** konfiguráció lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f0439-183">On hello **Single Sign-On** configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="f0439-184">![Egyszeri bejelentkezés](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="f0439-184">![Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="f0439-185">a.</span><span class="sxs-lookup"><span data-stu-id="f0439-185">a.</span></span> <span data-ttu-id="f0439-186">Válassza ki **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="f0439-186">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="f0439-187">b.</span><span class="sxs-lookup"><span data-stu-id="f0439-187">b.</span></span> <span data-ttu-id="f0439-188">Kattintson a **kézi konfigurálás**.</span><span class="sxs-lookup"><span data-stu-id="f0439-188">Click **Manual Configuration**.</span></span>
   
    <span data-ttu-id="f0439-189">c.</span><span class="sxs-lookup"><span data-stu-id="f0439-189">c.</span></span> <span data-ttu-id="f0439-190">A **SAML Post URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** amely másolta Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f0439-190">In **SAML Post URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal</span></span>
   
    <span data-ttu-id="f0439-191">d.</span><span class="sxs-lookup"><span data-stu-id="f0439-191">d.</span></span> <span data-ttu-id="f0439-192">A **SAML-alapú tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello értékének **ujjlenyomat** , amely a másolt **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f0439-192">In **SAML Certificate Fingerprint** textbox, paste hello value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span>
  
    <span data-ttu-id="f0439-193">e.</span><span class="sxs-lookup"><span data-stu-id="f0439-193">e.</span></span> <span data-ttu-id="f0439-194">Másolás **a Fiókazonosító** értékét, és illessze be a hello érték **attribútumérték** szövegmező alatt **attribútum hozzáadása** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0439-194">Copy **Your Account ID** value and paste hello value in **Attribute Value** textbox under **Add Attribute** section in Azure portal.</span></span>
   
    <span data-ttu-id="f0439-195">f.</span><span class="sxs-lookup"><span data-stu-id="f0439-195">f.</span></span> <span data-ttu-id="f0439-196">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f0439-196">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="f0439-197">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f0439-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f0439-198">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f0439-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f0439-199">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f0439-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f0439-200">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f0439-200">Create an Azure AD test user</span></span>
<span data-ttu-id="f0439-201">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f0439-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f0439-203">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f0439-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0439-204">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f0439-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f0439-206">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f0439-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![<span data-ttu-id="f0439-207">Felhasználók és csoportok -> minden felhasználó</span><span class="sxs-lookup"><span data-stu-id="f0439-207">Users and groups -> All users</span></span> ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f0439-208">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f0439-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Felhasználó](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f0439-210">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f0439-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f0439-212">a.</span><span class="sxs-lookup"><span data-stu-id="f0439-212">a.</span></span> <span data-ttu-id="f0439-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f0439-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f0439-214">b.</span><span class="sxs-lookup"><span data-stu-id="f0439-214">b.</span></span> <span data-ttu-id="f0439-215">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f0439-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f0439-216">c.</span><span class="sxs-lookup"><span data-stu-id="f0439-216">c.</span></span> <span data-ttu-id="f0439-217">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f0439-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f0439-218">d.</span><span class="sxs-lookup"><span data-stu-id="f0439-218">d.</span></span> <span data-ttu-id="f0439-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f0439-219">Click **Create**.</span></span>
 
### <a name="create-a-tinfoil-security-test-user"></a><span data-ttu-id="f0439-220">A TINFOIL SECURITY tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0439-220">Create a TINFOIL SECURITY test user</span></span>

<span data-ttu-id="f0439-221">A sorrend tooenable az Azure AD felhasználók toolog a TINFOIL SECURITY azok ki kell építenie a TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="f0439-221">In order tooenable Azure AD users toolog into TINFOIL SECURITY, they must be provisioned into TINFOIL SECURITY.</span></span> <span data-ttu-id="f0439-222">A TINFOIL SECURITY hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f0439-222">In hello case of TINFOIL SECURITY, provisioning is a manual task.</span></span>

<span data-ttu-id="f0439-223">**tooget üzembe helyezve, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f0439-223">**tooget a user provisioned, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0439-224">Ha hello felhasználói a vállalati fiók része, akkor túl[hello TINFOIL SECURITY támogatási csoportjához](https://www.tinfoilsecurity.com/contact) tooget hello felhasználói fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f0439-224">If hello user is a part of an Enterprise account, you need too[contact hello TINFOIL SECURITY support team](https://www.tinfoilsecurity.com/contact) tooget hello user account created.</span></span>

2. <span data-ttu-id="f0439-225">Ha hello felhasználói rendszeres TINFOIL SECURITY Szolgáltatottszoftver-felhasználó, hello felhasználó egy közreműködő tooany hello felhasználói helyeket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="f0439-225">If hello user is a regular TINFOIL SECURITY SaaS user, then hello user can add a collaborator tooany of hello user’s sites.</span></span> <span data-ttu-id="f0439-226">Az eseményindítók a folyamat toosend egy meghívó toohello megadott e-mail toocreate TINFOIL SECURITY új felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="f0439-226">This triggers a process toosend an invitation toohello specified email toocreate a new TINFOIL SECURITY user account.</span></span>

> [!NOTE]
> <span data-ttu-id="f0439-227">Bármely más TINFOIL SECURITY felhasználói fiók létrehozása eszközök, vagy a TINFOIL SECURITY tooprovision által nyújtott API-k az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="f0439-227">You can use any other TINFOIL SECURITY user account creation tools or APIs provided by TINFOIL SECURITY tooprovision Azure AD user accounts.</span></span>
> 
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f0439-228">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="f0439-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f0439-229">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTINFOIL biztonsági megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f0439-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTINFOIL SECURITY.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f0439-231">**tooassign Britta Simon tooTINFOIL biztonsági, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f0439-231">**tooassign Britta Simon tooTINFOIL SECURITY, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0439-232">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f0439-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f0439-234">Hello alkalmazások listában válassza ki a **TINFOIL SECURITY**.</span><span class="sxs-lookup"><span data-stu-id="f0439-234">In hello applications list, select **TINFOIL SECURITY**.</span></span>

    ![a TINFOIL SECURITY kiválasztása](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. <span data-ttu-id="f0439-236">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f0439-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f0439-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f0439-238">Click **Add** button.</span></span> <span data-ttu-id="f0439-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f0439-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f0439-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f0439-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f0439-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f0439-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f0439-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f0439-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f0439-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f0439-244">Test single sign-on</span></span>

<span data-ttu-id="f0439-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f0439-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f0439-246">Hello TINFOIL SECURITY csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour TINFOIL SECURITY alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="f0439-246">When you click hello TINFOIL SECURITY tile in hello Access Panel, you should get automatically signed-on tooyour TINFOIL SECURITY application.</span></span> <span data-ttu-id="f0439-247">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0439-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0439-248">További források</span><span class="sxs-lookup"><span data-stu-id="f0439-248">Additional resources</span></span>

* [<span data-ttu-id="f0439-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f0439-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f0439-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f0439-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

