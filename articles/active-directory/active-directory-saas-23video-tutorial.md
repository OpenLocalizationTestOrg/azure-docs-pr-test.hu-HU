---
title: "Oktatóanyag: Azure Active Directoryval integrált 23 videó |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a videó 23 között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 3430e4db3cd1114db62233e6699618071a3646ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a><span data-ttu-id="4a79e-103">Oktatóanyag: Azure Active Directoryval integrált 23 videó</span><span class="sxs-lookup"><span data-stu-id="4a79e-103">Tutorial: Azure Active Directory integration with 23 Video</span></span>

<span data-ttu-id="4a79e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 23 videó az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4a79e-104">In this tutorial, you learn how toointegrate 23 Video with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a79e-105">23 integrálása az Azure AD Video tesz lehetővé a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4a79e-105">Integrating 23 Video with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4a79e-106">Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-ben too23 videó</span><span class="sxs-lookup"><span data-stu-id="4a79e-106">You can control in Azure AD who has access too23 Video</span></span>
- <span data-ttu-id="4a79e-107">Engedélyezheti a felhasználóknak tooautomatically bejelentkezett (egyszeri bejelentkezés) too23 videó az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4a79e-107">You can enable your users tooautomatically get signed-on too23 Video (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4a79e-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4a79e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4a79e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a79e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a79e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4a79e-110">Prerequisites</span></span>

<span data-ttu-id="4a79e-111">tooconfigure 23 videó az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4a79e-111">tooconfigure Azure AD integration with 23 Video, you need hello following items:</span></span>

- <span data-ttu-id="4a79e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4a79e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a79e-113">A 23 videó egyszeri bejelentkezést előfizetés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4a79e-113">A 23 Video single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4a79e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4a79e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4a79e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4a79e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4a79e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4a79e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4a79e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a79e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a79e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4a79e-118">Scenario description</span></span>
<span data-ttu-id="4a79e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4a79e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4a79e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4a79e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a79e-121">Hozzáadása 23 videó hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4a79e-121">Adding 23 Video from hello gallery</span></span>
2. <span data-ttu-id="4a79e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a79e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-23-video-from-hello-gallery"></a><span data-ttu-id="4a79e-123">Hozzáadása 23 videó hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4a79e-123">Adding 23 Video from hello gallery</span></span>
<span data-ttu-id="4a79e-124">tooconfigure hello integrációs 23 videó az Azure AD-be, meg kell tooadd 23 videó hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4a79e-124">tooconfigure hello integration of 23 Video into Azure AD, you need tooadd 23 Video from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4a79e-125">**tooadd 23 videó hello gyűjteményből, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4a79e-125">**tooadd 23 Video from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a79e-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a79e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4a79e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4a79e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4a79e-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4a79e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4a79e-133">Hello keresési mezőbe, írja be a **23 videó**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-133">In hello search box, type **23 Video**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/tutorial_23video_search.png)

5. <span data-ttu-id="4a79e-135">A hello eredmények panelen válassza ki a **23 videó**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4a79e-135">In hello results panel, select **23 Video**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4a79e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a79e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4a79e-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján 23 videó</span><span class="sxs-lookup"><span data-stu-id="4a79e-138">In this section, you configure and test Azure AD single sign-on with 23 Video based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4a79e-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói 23 videó tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4a79e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 23 Video is tooa user in Azure AD.</span></span> <span data-ttu-id="4a79e-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello 23 közötti kapcsolat kapcsolatot videó kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="4a79e-140">In other words, a link relationship between an Azure AD user and hello related user in 23 Video needs toobe established.</span></span>

<span data-ttu-id="4a79e-141">A videó 23 rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4a79e-141">In 23 Video, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4a79e-142">tooconfigure és 23 videó az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4a79e-142">tooconfigure and test Azure AD single sign-on with 23 Video, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4a79e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4a79e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4a79e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a79e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4a79e-145">**[Videó 23 tesztfelhasználó létrehozása](#creating-a-23-video-test-user)**  -toohave egy megfelelője a Britta Simon 23, a felhasználó csatolt toohello az Azure AD ábrázolása videó.</span><span class="sxs-lookup"><span data-stu-id="4a79e-145">**[Creating a 23 Video test user](#creating-a-23-video-test-user)** - toohave a counterpart of Britta Simon in 23 Video that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4a79e-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4a79e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a79e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4a79e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4a79e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a79e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4a79e-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és 23 videó alkalmazásában egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="4a79e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 23 Video application.</span></span>

<span data-ttu-id="4a79e-150">**az Azure AD tooconfigure egyszeri bejelentkezés 23 videó, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a79e-150">**tooconfigure Azure AD single sign-on with 23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a79e-151">Az Azure portál, a hello hello **23 videó** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-151">In hello Azure portal, on hello **23 Video** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4a79e-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4a79e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_samlbase.png)

3. <span data-ttu-id="4a79e-155">A hello **23 videó tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a79e-155">On hello **23 Video Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_url.png)

    <span data-ttu-id="4a79e-157">a.</span><span class="sxs-lookup"><span data-stu-id="4a79e-157">a.</span></span> <span data-ttu-id="4a79e-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.23video.com`</span><span class="sxs-lookup"><span data-stu-id="4a79e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.23video.com`</span></span>

    <span data-ttu-id="4a79e-159">b.</span><span class="sxs-lookup"><span data-stu-id="4a79e-159">b.</span></span> <span data-ttu-id="4a79e-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.23video.com/saml/trust/<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="4a79e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.23video.com/saml/trust/<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4a79e-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4a79e-161">These values are not real.</span></span> <span data-ttu-id="4a79e-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="4a79e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4a79e-163">Ügyfél [23 videó ügyfél-támogatási csoport](mailto:support@23company.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4a79e-163">Contact [23 Video Client support team](mailto:support@23company.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="4a79e-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4a79e-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_certificate.png) 

5. <span data-ttu-id="4a79e-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a79e-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4a79e-168">A hello **23 videó konfiguráció** kattintson **23 videó konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4a79e-168">On hello **23 Video Configuration** section, click **Configure 23 Video** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4a79e-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4a79e-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_configure.png) 

7. <span data-ttu-id="4a79e-171">tooconfigure egyszeri bejelentkezést a **23 videó** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**túl[23 videó támogatási csoport](mailto:support@23company.com).</span><span class="sxs-lookup"><span data-stu-id="4a79e-171">tooconfigure single sign-on on **23 Video** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[23 Video support team](mailto:support@23company.com).</span></span> 


> [!TIP]
> <span data-ttu-id="4a79e-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="4a79e-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4a79e-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="4a79e-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4a79e-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4a79e-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4a79e-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a79e-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="4a79e-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="4a79e-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4a79e-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4a79e-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a79e-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a79e-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4a79e-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4a79e-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a79e-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4a79e-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a79e-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4a79e-187">a.</span><span class="sxs-lookup"><span data-stu-id="4a79e-187">a.</span></span> <span data-ttu-id="4a79e-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4a79e-189">b.</span><span class="sxs-lookup"><span data-stu-id="4a79e-189">b.</span></span> <span data-ttu-id="4a79e-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4a79e-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4a79e-191">c.</span><span class="sxs-lookup"><span data-stu-id="4a79e-191">c.</span></span> <span data-ttu-id="4a79e-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4a79e-193">d.</span><span class="sxs-lookup"><span data-stu-id="4a79e-193">d.</span></span> <span data-ttu-id="4a79e-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4a79e-194">Click **Create**.</span></span>
 
### <a name="creating-a-23-video-test-user"></a><span data-ttu-id="4a79e-195">Videó 23 tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a79e-195">Creating a 23 Video test user</span></span>

<span data-ttu-id="4a79e-196">hello ebben a szakaszban célja toocreate 23 videó Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4a79e-196">hello objective of this section is toocreate a user called Britta Simon in 23 Video.</span></span>

<span data-ttu-id="4a79e-197">**toocreate 23 videó Britta Simon nevű felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a79e-197">**toocreate a user called Britta Simon in 23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a79e-198">Bejelentkezés tooyour 23 videó vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4a79e-198">Sign on tooyour 23 Video company site as administrator.</span></span>

2. <span data-ttu-id="4a79e-199">Nyissa meg túl**beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-199">Go too**Settings**.</span></span>
 
3. <span data-ttu-id="4a79e-200">A **felhasználók** kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-200">In **Users** section, click **Configure**.</span></span>
   
    ![Felhasználó hozzárendelése][400]

4. <span data-ttu-id="4a79e-202">Kattintson a **új felhasználó hozzáadásához**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-202">Click **Add a new user**.</span></span> 
   
    ![Felhasználó hozzárendelése][401]

5. <span data-ttu-id="4a79e-204">A hello **meghívás toojoin ezen a helyen** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4a79e-204">In hello **Invite someone toojoin this site** section, perform hello following steps:</span></span>
   
    ![Felhasználó hozzárendelése][402]

    <span data-ttu-id="4a79e-206">a.</span><span class="sxs-lookup"><span data-stu-id="4a79e-206">a.</span></span> <span data-ttu-id="4a79e-207">A hello **E-mail címek** szövegmező, írja be a Britta Simon e-mail címét az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="4a79e-207">In hello **E-mail addresses** textbox, type Britta Simon's email address in Azure AD.</span></span>  
 
    <span data-ttu-id="4a79e-208">b.</span><span class="sxs-lookup"><span data-stu-id="4a79e-208">b.</span></span> <span data-ttu-id="4a79e-209">Kattintson a **hello felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-209">Click **Add hello user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4a79e-210">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4a79e-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4a79e-211">Ebben a szakaszban engedélyezze Britta Simon megadásával Azure egyszeri bejelentkezés toouse too23 videó eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4a79e-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too23 Video.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4a79e-213">**tooassign Britta Simon too23 videó, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4a79e-213">**tooassign Britta Simon too23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a79e-214">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4a79e-216">Hello alkalmazások listában válassza ki a **23 videó**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-216">In hello applications list, select **23 Video**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_app.png) 

3. <span data-ttu-id="4a79e-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4a79e-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4a79e-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a79e-220">Click **Add** button.</span></span> <span data-ttu-id="4a79e-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a79e-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4a79e-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4a79e-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4a79e-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a79e-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4a79e-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a79e-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4a79e-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4a79e-226">Testing single sign-on</span></span>

<span data-ttu-id="4a79e-227">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="4a79e-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="4a79e-228">Hello 23 videó csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour 23 videó alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="4a79e-228">When you click hello 23 Video tile in hello Access Panel, you should get automatically signed-on tooyour 23 Video application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4a79e-229">További források</span><span class="sxs-lookup"><span data-stu-id="4a79e-229">Additional resources</span></span>

* [<span data-ttu-id="4a79e-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4a79e-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a79e-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4a79e-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png
