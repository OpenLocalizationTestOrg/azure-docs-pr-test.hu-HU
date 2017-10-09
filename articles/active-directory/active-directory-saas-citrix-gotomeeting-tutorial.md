---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Citrix GoToMeeting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 46a5da7504806202a5ec29f73c504e772c61bc2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a><span data-ttu-id="994b5-103">Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="994b5-103">Tutorial: Azure Active Directory integration with Citrix GoToMeeting</span></span>

<span data-ttu-id="994b5-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Citrix GoToMeeting az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="994b5-104">In this tutorial, you learn how toointegrate Citrix GoToMeeting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="994b5-105">Citrix GoToMeeting integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="994b5-105">Integrating Citrix GoToMeeting with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="994b5-106">Megadhatja a hozzáférés tooCitrix GoToMeeting rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="994b5-106">You can control in Azure AD who has access tooCitrix GoToMeeting</span></span>
- <span data-ttu-id="994b5-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCitrix GoToMeeting (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="994b5-107">You can enable your users tooautomatically get signed-on tooCitrix GoToMeeting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="994b5-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="994b5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="994b5-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="994b5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="994b5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="994b5-110">Prerequisites</span></span>

<span data-ttu-id="994b5-111">az Azure AD-integráció a Citrix GoToMeeting tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="994b5-111">tooconfigure Azure AD integration with Citrix GoToMeeting, you need hello following items:</span></span>

- <span data-ttu-id="994b5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="994b5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="994b5-113">A Citrix GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="994b5-113">A Citrix GoToMeeting single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="994b5-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="994b5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="994b5-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="994b5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="994b5-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="994b5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="994b5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="994b5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="994b5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="994b5-118">Scenario description</span></span>
<span data-ttu-id="994b5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="994b5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="994b5-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="994b5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="994b5-121">Citrix GoToMeeting hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="994b5-121">Adding Citrix GoToMeeting from hello gallery</span></span>
2. <span data-ttu-id="994b5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="994b5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-citrix-gotomeeting-from-hello-gallery"></a><span data-ttu-id="994b5-123">Citrix GoToMeeting hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="994b5-123">Adding Citrix GoToMeeting from hello gallery</span></span>
<span data-ttu-id="994b5-124">tooconfigure hello integrációja Citrix GoToMeeting az Azure AD-be, meg kell tooadd Citrix GoToMeeting hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="994b5-124">tooconfigure hello integration of Citrix GoToMeeting into Azure AD, you need tooadd Citrix GoToMeeting from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="994b5-125">**Citrix GoToMeeting hello gyűjteményből tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="994b5-125">**tooadd Citrix GoToMeeting from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="994b5-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="994b5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="994b5-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="994b5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="994b5-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="994b5-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="994b5-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="994b5-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="994b5-133">Hello keresési mezőbe, írja be a **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="994b5-133">In hello search box, type **Citrix GoToMeeting**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. <span data-ttu-id="994b5-135">A hello eredmények panelen válassza ki a **Citrix GoToMeeting**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="994b5-135">In hello results panel, select **Citrix GoToMeeting**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="994b5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="994b5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="994b5-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Citrix GoToMeeting "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="994b5-138">In this section, you configure and test Azure AD single sign-on with Citrix GoToMeeting based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="994b5-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Citrix GoToMeeting tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="994b5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Citrix GoToMeeting is tooa user in Azure AD.</span></span> <span data-ttu-id="994b5-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Citrix GoToMeeting közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="994b5-140">In other words, a link relationship between an Azure AD user and hello related user in Citrix GoToMeeting needs toobe established.</span></span>

<span data-ttu-id="994b5-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="994b5-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Citrix GoToMeeting.</span></span>

<span data-ttu-id="994b5-142">tooconfigure és a Citrix GoToMeeting az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="994b5-142">tooconfigure and test Azure AD single sign-on with Citrix GoToMeeting, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="994b5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="994b5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="994b5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="994b5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="994b5-145">**[Citrix GoToMeeting tesztfelhasználó létrehozása](#creating-a-citrix-gotomeeting-test-user)**  -toohave egy megfelelője a Britta Simon a Citrix GoToMeeting, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="994b5-145">**[Creating a Citrix GoToMeeting test user](#creating-a-citrix-gotomeeting-test-user)** - toohave a counterpart of Britta Simon in Citrix GoToMeeting that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="994b5-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="994b5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="994b5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="994b5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="994b5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="994b5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="994b5-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Citrix GoToMeeting alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="994b5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Citrix GoToMeeting application.</span></span>

<span data-ttu-id="994b5-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Citrix GoToMeeting, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="994b5-150">**tooconfigure Azure AD single sign-on with Citrix GoToMeeting, perform hello following steps:**</span></span>

1. <span data-ttu-id="994b5-151">Az Azure portál, a hello hello **Citrix GoToMeeting** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="994b5-151">In hello Azure portal, on hello **Citrix GoToMeeting** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="994b5-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="994b5-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. <span data-ttu-id="994b5-155">A hello **Citrix GoToMeeting tartomány és az URL-címek** szakaszban, nincs szükség tooperform olyan lépéseket.</span><span class="sxs-lookup"><span data-stu-id="994b5-155">On hello **Citrix GoToMeeting Domain and URLs** section, no need tooperform any steps.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. <span data-ttu-id="994b5-157">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="994b5-157">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. <span data-ttu-id="994b5-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="994b5-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. <span data-ttu-id="994b5-161">A Citrix GoToMeeting SAML-alapú konfigurációs szakasz hello kattintson a Citrix GoToMeeting SAML konfigurálása tooopen konfigurálása bejelentkezés ablak.</span><span class="sxs-lookup"><span data-stu-id="994b5-161">On hello Citrix GoToMeeting SAML Configuration section, click Configure Citrix GoToMeeting SAML tooopen Configure sign-on window.</span></span> <span data-ttu-id="994b5-162">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="994b5-162">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

6. <span data-ttu-id="994b5-163">Egy másik böngészőablakban, jelentkezzen be tooyour [Citrix szervezet Center](https://account.citrixonline.com/organization/administration/).</span><span class="sxs-lookup"><span data-stu-id="994b5-163">In a different browser window, log in tooyour [Citrix Organization Center](https://account.citrixonline.com/organization/administration/).</span></span>

7. <span data-ttu-id="994b5-164">Kattintson a hello **identitásszolgáltató** lapot, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="994b5-164">Click hello **Identity Provider** tab, and then perform hello following steps:</span></span>  
   
    <span data-ttu-id="994b5-165">![SAML-alapú telepítő](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-alapú telepítő")</span><span class="sxs-lookup"><span data-stu-id="994b5-165">![SAML setup](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML setup")</span></span>
   
    <span data-ttu-id="994b5-166">a.</span><span class="sxs-lookup"><span data-stu-id="994b5-166">a.</span></span> <span data-ttu-id="994b5-167">Válassza ki **manuális**</span><span class="sxs-lookup"><span data-stu-id="994b5-167">Select **Manual**</span></span>

    <span data-ttu-id="994b5-168">b.</span><span class="sxs-lookup"><span data-stu-id="994b5-168">b.</span></span> <span data-ttu-id="994b5-169">Az Azure portál, a hello hello **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **bejelentkezési oldal URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="994b5-169">In hello Azure portal, on hello **Configure single sign-on at Citrix GoToMeeting** dialog page, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Sign-in page URL** textbox.</span></span> 

    <span data-ttu-id="994b5-170">c.</span><span class="sxs-lookup"><span data-stu-id="994b5-170">c.</span></span> <span data-ttu-id="994b5-171">Hello hello az Azure-portálon a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás hello **Sign-Out URL-cím** értékét, és illessze be hello **kijelentkezési URL-címe**szövegmező.</span><span class="sxs-lookup"><span data-stu-id="994b5-171">In hello Azure portal, on hello **Configure single sign-on at Citrix GoToMeeting** dialog page, copy hello **Sign-Out URL** value, and then paste it into hello **Sign-out page URL** textbox.</span></span>

    <span data-ttu-id="994b5-172">d.</span><span class="sxs-lookup"><span data-stu-id="994b5-172">d.</span></span> <span data-ttu-id="994b5-173">Hello hello az Azure-portálon a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás hello **SAML Entitásazonosító** értékét, és illessze be hello **Identity Provider entitás azonosítója**  szövegmező.</span><span class="sxs-lookup"><span data-stu-id="994b5-173">In hello Azure portal, on hello **Configure single sign-on at Citrix GoToMeeting** dialog page, copy hello **SAML Entity ID** value, and then paste it into hello **Identity Provider Entity ID** textbox.</span></span>

    <span data-ttu-id="994b5-174">e.</span><span class="sxs-lookup"><span data-stu-id="994b5-174">e.</span></span> <span data-ttu-id="994b5-175">tooupload a letöltött tanúsítvány kattintson **tanúsítvány feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="994b5-175">tooupload your downloaded certificate, click **Upload Certificate**.</span></span>

    <span data-ttu-id="994b5-176">f.</span><span class="sxs-lookup"><span data-stu-id="994b5-176">f.</span></span> <span data-ttu-id="994b5-177">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="994b5-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="994b5-178">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="994b5-178">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="994b5-179">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="994b5-179">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="994b5-180">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="994b5-180">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="994b5-181">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="994b5-181">Creating an Azure AD test user</span></span>
<span data-ttu-id="994b5-182">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="994b5-182">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="994b5-184">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="994b5-184">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="994b5-185">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="994b5-185">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="994b5-187">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="994b5-187">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="994b5-189">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="994b5-189">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="994b5-191">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="994b5-191">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="994b5-193">a.</span><span class="sxs-lookup"><span data-stu-id="994b5-193">a.</span></span> <span data-ttu-id="994b5-194">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="994b5-194">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="994b5-195">b.</span><span class="sxs-lookup"><span data-stu-id="994b5-195">b.</span></span> <span data-ttu-id="994b5-196">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="994b5-196">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="994b5-197">c.</span><span class="sxs-lookup"><span data-stu-id="994b5-197">c.</span></span> <span data-ttu-id="994b5-198">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="994b5-198">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="994b5-199">d.</span><span class="sxs-lookup"><span data-stu-id="994b5-199">d.</span></span> <span data-ttu-id="994b5-200">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="994b5-200">Click **Create**.</span></span>
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a><span data-ttu-id="994b5-201">Citrix GoToMeeting tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="994b5-201">Creating a Citrix GoToMeeting test user</span></span>

<span data-ttu-id="994b5-202">Ebben a szakaszban egy felhasználó Britta Simon nevű Citrix GoToMeeting jön létre.</span><span class="sxs-lookup"><span data-stu-id="994b5-202">In this section, a user called Britta Simon is created in Citrix GoToMeeting.</span></span> <span data-ttu-id="994b5-203">Citrix GoToMeeting támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="994b5-203">Citrix GoToMeeting supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="994b5-204">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="994b5-204">There is no action item for you in this section.</span></span> <span data-ttu-id="994b5-205">Ha a felhasználó nem létezik a Citrix GoToMeeting, egy új jön létre, Citrix GoToMeeting tooaccess tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="994b5-205">If a user doesn't already exist in Citrix GoToMeeting, a new one is created when you attempt tooaccess Citrix GoToMeeting.</span></span>

>[!Note]
><span data-ttu-id="994b5-206">Ha a felhasználó manuálisan, forduljon a toocreate kell [Citrix GoToMeeting támogatási csoport](https://care.citrixonline.com/gotomeeting)</span><span class="sxs-lookup"><span data-stu-id="994b5-206">If you need toocreate a user manually, Contact [Citrix GoToMeeting support team](https://care.citrixonline.com/gotomeeting)</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="994b5-207">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="994b5-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="994b5-208">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCitrix GoToMeeting megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="994b5-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCitrix GoToMeeting.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="994b5-210">**tooassign Britta Simon tooCitrix GoToMeeting, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="994b5-210">**tooassign Britta Simon tooCitrix GoToMeeting, perform hello following steps:**</span></span>

1. <span data-ttu-id="994b5-211">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="994b5-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="994b5-213">Hello alkalmazások listában válassza ki a **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="994b5-213">In hello applications list, select **Citrix GoToMeeting**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. <span data-ttu-id="994b5-215">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="994b5-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="994b5-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="994b5-217">Click **Add** button.</span></span> <span data-ttu-id="994b5-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="994b5-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="994b5-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="994b5-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="994b5-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="994b5-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="994b5-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="994b5-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="994b5-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="994b5-223">Testing single sign-on</span></span>

<span data-ttu-id="994b5-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="994b5-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="994b5-225">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="994b5-225">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="994b5-226">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="994b5-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="994b5-227">További források</span><span class="sxs-lookup"><span data-stu-id="994b5-227">Additional resources</span></span>

* [<span data-ttu-id="994b5-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="994b5-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="994b5-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="994b5-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="994b5-230">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="994b5-230">Configure User Provisioning</span></span>](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

