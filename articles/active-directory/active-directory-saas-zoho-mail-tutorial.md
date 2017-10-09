---
title: "Oktatóanyag: Azure Active Directoryval integrált Zoho |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Zoho között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 5d1c196d3b97babab196f509ea68e7d61523a7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a><span data-ttu-id="cf90d-103">Oktatóanyag: Azure Active Directoryval integrált Zoho</span><span class="sxs-lookup"><span data-stu-id="cf90d-103">Tutorial: Azure Active Directory integration with Zoho</span></span>

<span data-ttu-id="cf90d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zoho az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cf90d-104">In this tutorial, you learn how toointegrate Zoho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cf90d-105">Zoho integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-105">Integrating Zoho with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cf90d-106">Az Azure AD hozzáférési tooZoho rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="cf90d-106">You can control in Azure AD who has access tooZoho.</span></span>
- <span data-ttu-id="cf90d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZoho (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="cf90d-107">You can enable your users tooautomatically get signed-on tooZoho (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="cf90d-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="cf90d-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="cf90d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cf90d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf90d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cf90d-110">Prerequisites</span></span>

<span data-ttu-id="cf90d-111">az Azure AD integrálása Zoho tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-111">tooconfigure Azure AD integration with Zoho, you need hello following items:</span></span>

- <span data-ttu-id="cf90d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cf90d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cf90d-113">Egy Zoho egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="cf90d-113">A Zoho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cf90d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="cf90d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cf90d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="cf90d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cf90d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf90d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cf90d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cf90d-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cf90d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cf90d-118">Scenario description</span></span>
<span data-ttu-id="cf90d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cf90d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cf90d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cf90d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cf90d-121">Hello gyűjteményből Zoho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cf90d-121">Adding Zoho from hello gallery</span></span>
2. <span data-ttu-id="cf90d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cf90d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoho-from-hello-gallery"></a><span data-ttu-id="cf90d-123">Hello gyűjteményből Zoho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cf90d-123">Adding Zoho from hello gallery</span></span>
<span data-ttu-id="cf90d-124">tooconfigure hello integrációja Zoho az Azure AD-be, meg kell tooadd Zoho hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="cf90d-124">tooconfigure hello integration of Zoho into Azure AD, you need tooadd Zoho from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cf90d-125">**tooadd Zoho hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cf90d-125">**tooadd Zoho from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf90d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="cf90d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cf90d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="cf90d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="cf90d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="cf90d-133">Hello keresési mezőbe, írja be a **Zoho**, jelölje be **Zoho** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-133">In hello search box, type **Zoho**, select **Zoho** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Zoho](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="cf90d-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cf90d-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="cf90d-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zoho.</span><span class="sxs-lookup"><span data-stu-id="cf90d-136">In this section, you configure and test Azure AD single sign-on with Zoho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cf90d-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Zoho tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="cf90d-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zoho is tooa user in Azure AD.</span></span> <span data-ttu-id="cf90d-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Zoho közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="cf90d-138">In other words, a link relationship between an Azure AD user and hello related user in Zoho needs toobe established.</span></span>

<span data-ttu-id="cf90d-139">Zoho, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="cf90d-139">In Zoho, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cf90d-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Zoho-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="cf90d-140">tooconfigure and test Azure AD single sign-on with Zoho, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cf90d-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cf90d-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cf90d-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf90d-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cf90d-143">**[Zoho tesztfelhasználó létrehozása](#create-a-zoho-test-user)**  -toohave egy megfelelője a Britta Simon a Zoho, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="cf90d-143">**[Create a Zoho test user](#create-a-zoho-test-user)** - toohave a counterpart of Britta Simon in Zoho that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cf90d-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cf90d-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cf90d-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="cf90d-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="cf90d-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cf90d-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="cf90d-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Zoho alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cf90d-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zoho application.</span></span>

<span data-ttu-id="cf90d-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Zoho, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cf90d-148">**tooconfigure Azure AD single sign-on with Zoho, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf90d-149">Az Azure portál, a hello hello **Zoho** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-149">In hello Azure portal, on hello **Zoho** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="cf90d-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cf90d-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. <span data-ttu-id="cf90d-153">A hello **Zoho tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-153">On hello **Zoho Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Zoho tartomány és az URL-címek](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    <span data-ttu-id="cf90d-155">a.</span><span class="sxs-lookup"><span data-stu-id="cf90d-155">a.</span></span> <span data-ttu-id="cf90d-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.zohomail.com`</span><span class="sxs-lookup"><span data-stu-id="cf90d-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.zohomail.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cf90d-157">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="cf90d-157">This value is not real.</span></span> <span data-ttu-id="cf90d-158">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="cf90d-158">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="cf90d-159">Ügyfél [Zoho ügyfél-támogatási csoport](https://www.zoho.com/mail/contact.html) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="cf90d-159">Contact [Zoho Client support team](https://www.zoho.com/mail/contact.html) tooget this value.</span></span> 
 
4. <span data-ttu-id="cf90d-160">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cf90d-160">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. <span data-ttu-id="cf90d-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cf90d-164">A hello **Zoho konfigurációs** kattintson **konfigurálása Zoho** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="cf90d-164">On hello **Zoho Configuration** section, click **Configure Zoho** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cf90d-165">Másolás hello **Sign-Out URL-cím, jelszó URL-cím módosítása és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="cf90d-165">Copy hello **Sign-Out URL, Change Password URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Zoho konfiguráció](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. <span data-ttu-id="cf90d-167">Egy másik webes böngészőablakban jelentkezzen be a Zoho Mail vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cf90d-167">In a different web browser window, log into your Zoho Mail company site as an administrator.</span></span>

8. <span data-ttu-id="cf90d-168">Nyissa meg toohello **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-168">Go toohello **Control panel**.</span></span>
   
    <span data-ttu-id="cf90d-169">![Vezérlőpult](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "vezérlőpultot")</span><span class="sxs-lookup"><span data-stu-id="cf90d-169">![Control Panel](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Control Panel")</span></span>

9. <span data-ttu-id="cf90d-170">Kattintson a hello **SAML-alapú hitelesítés** fülre.</span><span class="sxs-lookup"><span data-stu-id="cf90d-170">Click hello **SAML Authentication** tab.</span></span>
   
    <span data-ttu-id="cf90d-171">![SAML-alapú hitelesítés](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="cf90d-171">![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML Authentication")</span></span>

10. <span data-ttu-id="cf90d-172">A hello **SAML-alapú hitelesítés részletei** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-172">In hello **SAML Authentication Details** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="cf90d-173">![SAML-alapú hitelesítés részletei](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML-alapú hitelesítés részletei")</span><span class="sxs-lookup"><span data-stu-id="cf90d-173">![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML Authentication Details")</span></span>
   
    <span data-ttu-id="cf90d-174">a.</span><span class="sxs-lookup"><span data-stu-id="cf90d-174">a.</span></span> <span data-ttu-id="cf90d-175">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cf90d-175">In hello **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="cf90d-176">b.</span><span class="sxs-lookup"><span data-stu-id="cf90d-176">b.</span></span> <span data-ttu-id="cf90d-177">A hello **kijelentkezési URL-cím** szövegmezőhöz Beillesztés **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cf90d-177">In hello **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="cf90d-178">c.</span><span class="sxs-lookup"><span data-stu-id="cf90d-178">c.</span></span> <span data-ttu-id="cf90d-179">A hello **jelszó URL-cím módosítása** szövegmezőhöz Beillesztés **jelszó URL-cím módosítása** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="cf90d-179">In hello **Change Password URL** textbox, paste **Change Password URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="cf90d-180">d.</span><span class="sxs-lookup"><span data-stu-id="cf90d-180">d.</span></span> <span data-ttu-id="cf90d-181">A base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello Azure portálról letöltött megnyitása és toohello Beillesztés **PublicKey** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="cf90d-181">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **PublicKey** textbox.</span></span>
   
    <span data-ttu-id="cf90d-182">e.</span><span class="sxs-lookup"><span data-stu-id="cf90d-182">e.</span></span> <span data-ttu-id="cf90d-183">Mint **algoritmus**, jelölje be **RSA**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-183">As **Algorithm**, select **RSA**.</span></span>
   
    <span data-ttu-id="cf90d-184">f.</span><span class="sxs-lookup"><span data-stu-id="cf90d-184">f.</span></span> <span data-ttu-id="cf90d-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-185">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="cf90d-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="cf90d-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cf90d-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="cf90d-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cf90d-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cf90d-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="cf90d-189">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="cf90d-189">Create an Azure AD test user</span></span>

<span data-ttu-id="cf90d-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="cf90d-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="cf90d-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="cf90d-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf90d-193">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-193">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="cf90d-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-195">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="cf90d-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="cf90d-197">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="cf90d-199">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-199">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    <span data-ttu-id="cf90d-201">a.</span><span class="sxs-lookup"><span data-stu-id="cf90d-201">a.</span></span> <span data-ttu-id="cf90d-202">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-202">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cf90d-203">b.</span><span class="sxs-lookup"><span data-stu-id="cf90d-203">b.</span></span> <span data-ttu-id="cf90d-204">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="cf90d-204">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="cf90d-205">c.</span><span class="sxs-lookup"><span data-stu-id="cf90d-205">c.</span></span> <span data-ttu-id="cf90d-206">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="cf90d-206">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="cf90d-207">d.</span><span class="sxs-lookup"><span data-stu-id="cf90d-207">d.</span></span> <span data-ttu-id="cf90d-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-208">Click **Create**.</span></span>
 
### <a name="create-a-zoho-test-user"></a><span data-ttu-id="cf90d-209">Zoho tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf90d-209">Create a Zoho test user</span></span>

<span data-ttu-id="cf90d-210">A sorrend tooenable az Azure AD felhasználók toolog Zoho Mail be azok kell üzembe Zoho Mail be.</span><span class="sxs-lookup"><span data-stu-id="cf90d-210">In order tooenable Azure AD users toolog into Zoho Mail, they must be provisioned into Zoho Mail.</span></span> <span data-ttu-id="cf90d-211">Zoho Mail hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="cf90d-211">In hello case of Zoho Mail, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="cf90d-212">Bármely más Zoho Mail felhasználói fiók létrehozása eszközök vagy Zoho Mail tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="cf90d-212">You can use any other Zoho Mail user account creation tools or APIs provided by Zoho Mail tooprovision AAD user accounts.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="cf90d-213">tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-213">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="cf90d-214">Jelentkezzen be tooyour **Zoho Mail** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cf90d-214">Log in tooyour **Zoho Mail** company site as an administrator.</span></span>

2. <span data-ttu-id="cf90d-215">Nyissa meg túl**Vezérlőpult \> Mail & Docs**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-215">Go too**Control Panel \> Mail & Docs**.</span></span>

3. <span data-ttu-id="cf90d-216">Nyissa meg túl**felhasználó adatait \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-216">Go too**User Details \> Add User**.</span></span>
   
    <span data-ttu-id="cf90d-217">![Felhasználó hozzáadása](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cf90d-217">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "Add User")</span></span>

4. <span data-ttu-id="cf90d-218">A hello **felhasználók hozzáadása az** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cf90d-218">On hello **Add users** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="cf90d-219">![Felhasználó hozzáadása](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="cf90d-219">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "Add User")</span></span>
   
    <span data-ttu-id="cf90d-220">a.</span><span class="sxs-lookup"><span data-stu-id="cf90d-220">a.</span></span> <span data-ttu-id="cf90d-221">A hello **Utónév** szövegmező, például a felhasználó hello első nevét **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-221">In hello **First Name** textbox, type hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="cf90d-222">b.</span><span class="sxs-lookup"><span data-stu-id="cf90d-222">b.</span></span> <span data-ttu-id="cf90d-223">A hello **Vezetéknév** szövegmező, például a felhasználó típusa hello Vezetéknév **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-223">In hello **Last Name** textbox, type hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="cf90d-224">c.</span><span class="sxs-lookup"><span data-stu-id="cf90d-224">c.</span></span> <span data-ttu-id="cf90d-225">A hello **E-mail azonosító** szövegmező, például a felhasználó hello e-mail azonosítója  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cf90d-225">In hello **Email ID** textbox, type hello email id of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="cf90d-226">d.</span><span class="sxs-lookup"><span data-stu-id="cf90d-226">d.</span></span> <span data-ttu-id="cf90d-227">A hello **jelszó** szövegmező, adja meg a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cf90d-227">In hello **Password** textbox, enter password of user.</span></span>
   
    <span data-ttu-id="cf90d-228">e.</span><span class="sxs-lookup"><span data-stu-id="cf90d-228">e.</span></span> <span data-ttu-id="cf90d-229">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-229">Click **OK**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="cf90d-230">hello Azure Active Directory fióktulajdonos kap e-mailben található egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="cf90d-230">hello Azure Active Directory account holder will receive an email with a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="cf90d-231">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="cf90d-231">Assign hello Azure AD test user</span></span>

<span data-ttu-id="cf90d-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZoho megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="cf90d-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZoho.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="cf90d-234">**tooassign Britta Simon tooZoho, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="cf90d-234">**tooassign Britta Simon tooZoho, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf90d-235">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cf90d-237">Hello alkalmazások listában válassza ki a **Zoho**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-237">In hello applications list, select **Zoho**.</span></span>

    ![hello Zoho hivatkozásra hello alkalmazások listája](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. <span data-ttu-id="cf90d-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cf90d-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="cf90d-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cf90d-241">Click **Add** button.</span></span> <span data-ttu-id="cf90d-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf90d-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="cf90d-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cf90d-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cf90d-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf90d-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cf90d-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cf90d-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="cf90d-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cf90d-247">Test single sign-on</span></span>

<span data-ttu-id="cf90d-248">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="cf90d-248">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cf90d-249">Ha a hozzáférési Panel hello hello Zoho csempe gombra kattint, automatikusan bejelentkezett tooyour Zoho alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="cf90d-249">When you click hello Zoho tile in hello Access Panel, you should get automatically signed-on tooyour Zoho application.</span></span>
<span data-ttu-id="cf90d-250">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cf90d-250">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cf90d-251">További források</span><span class="sxs-lookup"><span data-stu-id="cf90d-251">Additional resources</span></span>

* [<span data-ttu-id="cf90d-252">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="cf90d-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cf90d-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cf90d-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

