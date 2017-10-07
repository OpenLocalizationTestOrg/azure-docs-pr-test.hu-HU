---
title: "Oktatóanyag: Azure Active Directoryval integrált ügyféloldali |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az ügyféloldali között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 93add7c1f3cf1fc163acc505f11e34bd696c571c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a><span data-ttu-id="1a017-103">Oktatóanyag: Azure Active Directoryval integrált ügyféloldali</span><span class="sxs-lookup"><span data-stu-id="1a017-103">Tutorial: Azure Active Directory integration with Envoy</span></span>

<span data-ttu-id="1a017-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate az Azure Active Directoryval (Azure AD) követ.</span><span class="sxs-lookup"><span data-stu-id="1a017-104">In this tutorial, you learn how toointegrate Envoy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a017-105">Ügyféloldali integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="1a017-105">Integrating Envoy with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1a017-106">Az Azure AD hozzáférési tooEnvoy rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="1a017-106">You can control in Azure AD who has access tooEnvoy.</span></span>
- <span data-ttu-id="1a017-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEnvoy (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="1a017-107">You can enable your users tooautomatically get signed-on tooEnvoy (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1a017-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="1a017-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="1a017-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a017-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a017-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1a017-110">Prerequisites</span></span>

<span data-ttu-id="1a017-111">tooconfigure ügyféloldali az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="1a017-111">tooconfigure Azure AD integration with Envoy, you need hello following items:</span></span>

- <span data-ttu-id="1a017-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1a017-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a017-113">Az ügyféloldali egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1a017-113">An Envoy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a017-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1a017-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a017-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="1a017-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a017-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1a017-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1a017-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a017-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a017-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1a017-118">Scenario description</span></span>
<span data-ttu-id="1a017-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1a017-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a017-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1a017-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a017-121">Ügyféloldali hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="1a017-121">Adding Envoy from hello gallery</span></span>
2. <span data-ttu-id="1a017-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a017-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-envoy-from-hello-gallery"></a><span data-ttu-id="1a017-123">Ügyféloldali hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="1a017-123">Adding Envoy from hello gallery</span></span>
<span data-ttu-id="1a017-124">tooconfigure hello integrációja ügyféloldali az Azure AD-be, meg kell tooadd ügyféloldali hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="1a017-124">tooconfigure hello integration of Envoy into Azure AD, you need tooadd Envoy from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1a017-125">**tooadd ügyféloldali hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1a017-125">**tooadd Envoy from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a017-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a017-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="1a017-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1a017-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1a017-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a017-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="1a017-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="1a017-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="1a017-133">Hello keresési mezőbe, írja be a **ügyféloldali**, jelölje be **ügyféloldali** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="1a017-133">In hello search box, type **Envoy**, select **Envoy** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Ügyféloldali hello eredmények listájában](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1a017-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a017-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1a017-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján követ.</span><span class="sxs-lookup"><span data-stu-id="1a017-136">In this section, you configure and test Azure AD single sign-on with Envoy based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1a017-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ügyféloldali tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1a017-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Envoy is tooa user in Azure AD.</span></span> <span data-ttu-id="1a017-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ügyféloldali közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="1a017-138">In other words, a link relationship between an Azure AD user and hello related user in Envoy needs toobe established.</span></span>

<span data-ttu-id="1a017-139">Ügyféloldali, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1a017-139">In Envoy, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1a017-140">tooconfigure és az ügyféloldali az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1a017-140">tooconfigure and test Azure AD single sign-on with Envoy, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1a017-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1a017-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1a017-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a017-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a017-143">**[Hozzon létre egy ügyféloldali tesztfelhasználó](#create-an-envoy-test-user)**  -toohave egy megfelelője a Britta Simon a követ, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="1a017-143">**[Create an Envoy test user](#create-an-envoy-test-user)** - toohave a counterpart of Britta Simon in Envoy that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1a017-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1a017-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1a017-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="1a017-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1a017-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a017-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1a017-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ügyféloldali alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="1a017-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Envoy application.</span></span>

<span data-ttu-id="1a017-148">**az Azure AD tooconfigure egyszeri bejelentkezést az ügyféloldali, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1a017-148">**tooconfigure Azure AD single sign-on with Envoy, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a017-149">Az Azure portál, a hello hello **ügyféloldali** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a017-149">In hello Azure portal, on hello **Envoy** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="1a017-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1a017-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_samlbase.png)

3. <span data-ttu-id="1a017-153">A hello **ügyféloldali tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1a017-153">On hello **Envoy Domain and URLs** section, perform hello following steps:</span></span>

    ![Ügyféloldali tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_url.png)

    <span data-ttu-id="1a017-155">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.Envoy.com`</span><span class="sxs-lookup"><span data-stu-id="1a017-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Envoy.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="1a017-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="1a017-156">This value is not real.</span></span> <span data-ttu-id="1a017-157">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1a017-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="1a017-158">Ügyfél [ügyféloldali ügyfél-támogatási csoport](https://envoy.com/contact/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="1a017-158">Contact [Envoy Client support team](https://envoy.com/contact/) tooget this value.</span></span>

4. <span data-ttu-id="1a017-159">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékének...</span><span class="sxs-lookup"><span data-stu-id="1a017-159">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate..</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_certificate.png) 

5. <span data-ttu-id="1a017-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a017-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-envoy-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1a017-163">A hello **ügyféloldali konfigurációs** kattintson **konfigurálása ügyféloldali** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1a017-163">On hello **Envoy Configuration** section, click **Configure Envoy** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1a017-164">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1a017-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Ügyféloldali konfiguráció](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_configure.png)

7. <span data-ttu-id="1a017-166">Egy másik webes böngészőablakban jelentkezzen be a ügyféloldali vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1a017-166">In a different web browser window, log into your Envoy company site as an administrator.</span></span>

8. <span data-ttu-id="1a017-167">Hello hello felső eszköztárán kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1a017-167">In hello toolbar on hello top, click **Settings**.</span></span>

    <span data-ttu-id="1a017-168">![Ügyféloldali](./media/active-directory-saas-envoy-tutorial/ic776782.png "ügyféloldali")</span><span class="sxs-lookup"><span data-stu-id="1a017-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span></span>

9. <span data-ttu-id="1a017-169">Kattintson a **vállalati**.</span><span class="sxs-lookup"><span data-stu-id="1a017-169">Click **Company**.</span></span>

    <span data-ttu-id="1a017-170">![Vállalati](./media/active-directory-saas-envoy-tutorial/ic776783.png "vállalati")</span><span class="sxs-lookup"><span data-stu-id="1a017-170">![Company](./media/active-directory-saas-envoy-tutorial/ic776783.png "Company")</span></span>

10. <span data-ttu-id="1a017-171">Kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="1a017-171">Click **SAML**.</span></span>

    <span data-ttu-id="1a017-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="1a017-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span></span>

11. <span data-ttu-id="1a017-173">A hello **SAML-alapú hitelesítés** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1a017-173">In hello **SAML Authentication** configuration section, perform hello following steps:</span></span>

    <span data-ttu-id="1a017-174">![SAML-alapú hitelesítés](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="1a017-174">![SAML authentication](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML authentication")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="1a017-175">hello hello HQ Helyazonosító értéke hello alkalmazás által létrehozott automatikus.</span><span class="sxs-lookup"><span data-stu-id="1a017-175">hello value for hello HQ location ID is auto generated by hello application.</span></span>
    
    <span data-ttu-id="1a017-176">a.</span><span class="sxs-lookup"><span data-stu-id="1a017-176">a.</span></span> <span data-ttu-id="1a017-177">A **ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="1a017-177">In **Fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="1a017-178">b.</span><span class="sxs-lookup"><span data-stu-id="1a017-178">b.</span></span> <span data-ttu-id="1a017-179">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely a másolt alkotnak hello Azure portál be hello **IDENTITY PROVIDER HTTP SAML-alapú URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1a017-179">Paste **SAML Single Sign-On Service URL** value, which you have copied form hello Azure portal into hello **IDENTITY PROVIDER HTTP SAML URL** textbox.</span></span>
    
    <span data-ttu-id="1a017-180">c.</span><span class="sxs-lookup"><span data-stu-id="1a017-180">c.</span></span> <span data-ttu-id="1a017-181">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="1a017-181">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="1a017-182">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="1a017-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1a017-183">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="1a017-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1a017-184">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1a017-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1a017-185">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="1a017-185">Create an Azure AD test user</span></span>

<span data-ttu-id="1a017-186">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="1a017-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="1a017-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1a017-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a017-189">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a017-189">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-envoy-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1a017-191">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1a017-191">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-envoy-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1a017-193">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1a017-193">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-envoy-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1a017-195">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1a017-195">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-envoy-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1a017-197">a.</span><span class="sxs-lookup"><span data-stu-id="1a017-197">a.</span></span> <span data-ttu-id="1a017-198">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a017-198">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a017-199">b.</span><span class="sxs-lookup"><span data-stu-id="1a017-199">b.</span></span> <span data-ttu-id="1a017-200">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="1a017-200">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="1a017-201">c.</span><span class="sxs-lookup"><span data-stu-id="1a017-201">c.</span></span> <span data-ttu-id="1a017-202">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="1a017-202">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="1a017-203">d.</span><span class="sxs-lookup"><span data-stu-id="1a017-203">d.</span></span> <span data-ttu-id="1a017-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a017-204">Click **Create**.</span></span>
 
### <a name="create-an-envoy-test-user"></a><span data-ttu-id="1a017-205">Az ügyféloldali tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a017-205">Create an Envoy test user</span></span>

<span data-ttu-id="1a017-206">Nincs a akkor tooconfigure felhasználók átadásához tooEnvoy művelet elem.</span><span class="sxs-lookup"><span data-stu-id="1a017-206">There is no action item for you tooconfigure user provisioning tooEnvoy.</span></span> <span data-ttu-id="1a017-207">Ha egy hozzárendelt felhasználó megpróbál toolog ügyféloldali hello hozzáférési panelen történő, az ügyféloldali ellenőrzi, hogy létezik-e hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1a017-207">When an assigned user tries toolog into Envoy using hello access panel, Envoy checks whether hello user exists.</span></span> <span data-ttu-id="1a017-208">Nincs még nincs felhasználói fiók érhető el, ha a ügyféloldali automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1a017-208">If there is no user account available yet, it is automatically created by Envoy.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="1a017-209">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="1a017-209">Assign hello Azure AD test user</span></span>

<span data-ttu-id="1a017-210">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEnvoy megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1a017-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEnvoy.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="1a017-212">**tooassign Britta Simon tooEnvoy, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="1a017-212">**tooassign Britta Simon tooEnvoy, perform hello following steps:**</span></span>

1. <span data-ttu-id="1a017-213">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a017-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1a017-215">Hello alkalmazások listában válassza ki a **ügyféloldali**.</span><span class="sxs-lookup"><span data-stu-id="1a017-215">In hello applications list, select **Envoy**.</span></span>

    ![hello ügyféloldali hivatkozásra hello alkalmazások listája](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_app.png)  

3. <span data-ttu-id="1a017-217">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1a017-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="1a017-219">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a017-219">Click **Add** button.</span></span> <span data-ttu-id="1a017-220">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a017-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="1a017-222">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1a017-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1a017-223">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a017-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a017-224">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a017-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1a017-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1a017-225">Test single sign-on</span></span>

<span data-ttu-id="1a017-226">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="1a017-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1a017-227">Ha a hozzáférési Panel hello hello ügyféloldali csempe gombra kattint, automatikusan bejelentkezett tooyour ügyféloldali alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="1a017-227">When you click hello Envoy tile in hello Access Panel, you should get automatically signed-on tooyour Envoy application.</span></span>
<span data-ttu-id="1a017-228">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1a017-228">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1a017-229">További források</span><span class="sxs-lookup"><span data-stu-id="1a017-229">Additional resources</span></span>

* [<span data-ttu-id="1a017-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1a017-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a017-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1a017-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_203.png

