---
title: "Oktatóanyag: Azure Active Directoryval integrált PerformanceCentre |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PerformanceCentre között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 19781c0087093a67c70dc90072cf1a119bb2ade0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a><span data-ttu-id="0637d-103">Oktatóanyag: Azure Active Directoryval integrált PerformanceCentre</span><span class="sxs-lookup"><span data-stu-id="0637d-103">Tutorial: Azure Active Directory integration with PerformanceCentre</span></span>

<span data-ttu-id="0637d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PerformanceCentre az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0637d-104">In this tutorial, you learn how toointegrate PerformanceCentre with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0637d-105">PerformanceCentre integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0637d-105">Integrating PerformanceCentre with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0637d-106">Megadhatja a hozzáférés tooPerformanceCentre rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0637d-106">You can control in Azure AD who has access tooPerformanceCentre</span></span>
- <span data-ttu-id="0637d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPerformanceCentre (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0637d-107">You can enable your users tooautomatically get signed-on tooPerformanceCentre (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0637d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0637d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0637d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0637d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0637d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0637d-110">Prerequisites</span></span>

<span data-ttu-id="0637d-111">az Azure AD integrálása PerformanceCentre tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0637d-111">tooconfigure Azure AD integration with PerformanceCentre, you need hello following items:</span></span>

- <span data-ttu-id="0637d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0637d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0637d-113">Egy PerformanceCentre egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="0637d-113">A PerformanceCentre single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0637d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0637d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0637d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0637d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0637d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0637d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0637d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0637d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0637d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0637d-118">Scenario description</span></span>
<span data-ttu-id="0637d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0637d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0637d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0637d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0637d-121">Hello gyűjteményből PerformanceCentre hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0637d-121">Adding PerformanceCentre from hello gallery</span></span>
2. <span data-ttu-id="0637d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0637d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-performancecentre-from-hello-gallery"></a><span data-ttu-id="0637d-123">Hello gyűjteményből PerformanceCentre hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0637d-123">Adding PerformanceCentre from hello gallery</span></span>
<span data-ttu-id="0637d-124">tooconfigure hello integrációja PerformanceCentre az Azure AD-be, meg kell tooadd PerformanceCentre hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="0637d-124">tooconfigure hello integration of PerformanceCentre into Azure AD, you need tooadd PerformanceCentre from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0637d-125">**tooadd PerformanceCentre hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0637d-125">**tooadd PerformanceCentre from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0637d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0637d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0637d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0637d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0637d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0637d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0637d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="0637d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0637d-133">Hello keresési mezőbe, írja be a **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="0637d-133">In hello search box, type **PerformanceCentre**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. <span data-ttu-id="0637d-135">A hello eredmények panelen válassza ki a **PerformanceCentre**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="0637d-135">In hello results panel, select **PerformanceCentre**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0637d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0637d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0637d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PerformanceCentre.</span><span class="sxs-lookup"><span data-stu-id="0637d-138">In this section, you configure and test Azure AD single sign-on with PerformanceCentre based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0637d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PerformanceCentre tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0637d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PerformanceCentre is tooa user in Azure AD.</span></span> <span data-ttu-id="0637d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PerformanceCentre közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="0637d-140">In other words, a link relationship between an Azure AD user and hello related user in PerformanceCentre needs toobe established.</span></span>

<span data-ttu-id="0637d-141">PerformanceCentre, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0637d-141">In PerformanceCentre, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0637d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés PerformanceCentre-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0637d-142">tooconfigure and test Azure AD single sign-on with PerformanceCentre, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0637d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0637d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0637d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0637d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0637d-145">**[PerformanceCentre tesztfelhasználó létrehozása](#creating-a-performancecentre-test-user)**  -toohave egy megfelelője a Britta Simon a PerformanceCentre, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="0637d-145">**[Creating a PerformanceCentre test user](#creating-a-performancecentre-test-user)** - toohave a counterpart of Britta Simon in PerformanceCentre that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0637d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0637d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0637d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0637d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0637d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0637d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0637d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az PerformanceCentre alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0637d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PerformanceCentre application.</span></span>

<span data-ttu-id="0637d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a PerformanceCentre, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0637d-150">**tooconfigure Azure AD single sign-on with PerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="0637d-151">Az Azure portál, a hello hello **PerformanceCentre** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0637d-151">In hello Azure portal, on hello **PerformanceCentre** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0637d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0637d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. <span data-ttu-id="0637d-155">A hello **PerformanceCentre tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0637d-155">On hello **PerformanceCentre Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    <span data-ttu-id="0637d-157">a.</span><span class="sxs-lookup"><span data-stu-id="0637d-157">a.</span></span> <span data-ttu-id="0637d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://companyname.performancecentre.com/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="0637d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://companyname.performancecentre.com/saml/SSO`</span></span>

    <span data-ttu-id="0637d-159">b.</span><span class="sxs-lookup"><span data-stu-id="0637d-159">b.</span></span> <span data-ttu-id="0637d-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://companyname.performancecentre.com`</span><span class="sxs-lookup"><span data-stu-id="0637d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://companyname.performancecentre.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0637d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0637d-161">These values are not real.</span></span> <span data-ttu-id="0637d-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="0637d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="0637d-163">Ügyfél [PerformanceCentre ügyfél-támogatási csoport](https://www.performancecentre.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0637d-163">Contact [PerformanceCentre Client support team](https://www.performancecentre.com/contact-us/) tooget these values.</span></span> 

4. <span data-ttu-id="0637d-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0637d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. <span data-ttu-id="0637d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0637d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0637d-168">A hello **PerformanceCentre konfigurációs** kattintson **konfigurálása PerformanceCentre** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0637d-168">On hello **PerformanceCentre Configuration** section, click **Configure PerformanceCentre** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0637d-169">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0637d-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. <span data-ttu-id="0637d-171">Bejelentkezés tooyour **PerformanceCentre** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0637d-171">Sign-on tooyour **PerformanceCentre** company site as administrator.</span></span>

8. <span data-ttu-id="0637d-172">Hello lapon hello bal oldalán kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="0637d-172">In hello tab on hello left side, click **Configure**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

9. <span data-ttu-id="0637d-174">Hello lapon hello bal oldalán kattintson **vegyes**, és kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0637d-174">In hello tab on hello left side, click **Miscellaneous**, and then click **Single Sign On**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

10. <span data-ttu-id="0637d-176">Mint **protokoll**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="0637d-176">As **Protocol**, select **SAML**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][12]

11. <span data-ttu-id="0637d-178">Nyissa meg a letöltött metaadatok fájlt a Jegyzettömb, hello tartalom másolása, illessze be hello **Identity Provider metaadatok** szövegmező, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0637d-178">Open your downloaded metadata file in notepad, copy hello content, paste it into hello **Identity Provider Metadata** textbox, and then click **Save**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][13]

12. <span data-ttu-id="0637d-180">Győződjön meg arról, hogy hello értékeinek hello **entitás alap URL-cím** és **entitás azonosító URL-cím** helyes-e.</span><span class="sxs-lookup"><span data-stu-id="0637d-180">Verify that hello values for hello **Entity Base URL** and **Entity ID URL** are correct.</span></span>
    
     ![Az Azure AD-egyszeri bejelentkezés][14]

> [!TIP]
> <span data-ttu-id="0637d-182">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="0637d-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0637d-183">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="0637d-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0637d-184">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0637d-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0637d-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0637d-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="0637d-186">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="0637d-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0637d-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0637d-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0637d-189">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0637d-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0637d-191">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0637d-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0637d-193">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0637d-193">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0637d-195">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0637d-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0637d-197">a.</span><span class="sxs-lookup"><span data-stu-id="0637d-197">a.</span></span> <span data-ttu-id="0637d-198">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0637d-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0637d-199">b.</span><span class="sxs-lookup"><span data-stu-id="0637d-199">b.</span></span> <span data-ttu-id="0637d-200">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0637d-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0637d-201">c.</span><span class="sxs-lookup"><span data-stu-id="0637d-201">c.</span></span> <span data-ttu-id="0637d-202">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0637d-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0637d-203">d.</span><span class="sxs-lookup"><span data-stu-id="0637d-203">d.</span></span> <span data-ttu-id="0637d-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0637d-204">Click **Create**.</span></span>
 
### <a name="creating-a-performancecentre-test-user"></a><span data-ttu-id="0637d-205">PerformanceCentre tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0637d-205">Creating a PerformanceCentre test user</span></span>

<span data-ttu-id="0637d-206">hello ebben a szakaszban célja toocreate PerformanceCentre Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="0637d-206">hello objective of this section is toocreate a user called Britta Simon in PerformanceCentre.</span></span>

<span data-ttu-id="0637d-207">**toocreate Britta Simon meghívta PerformanceCentre, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0637d-207">**toocreate a user called Britta Simon in PerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="0637d-208">Bejelentkezés tooyour PerformanceCentre vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0637d-208">Sign on tooyour PerformanceCentre company site as administrator.</span></span>

2. <span data-ttu-id="0637d-209">Hello hello bal oldali menüben kattintson a **Interrelate**, és kattintson a **létrehozása résztvevő**.</span><span class="sxs-lookup"><span data-stu-id="0637d-209">In hello menu on hello left, click **Interrelate**, and then click **Create Participant**.</span></span>
   
    ![Felhasználó létrehozása][400]

3. <span data-ttu-id="0637d-211">A hello **Interrelate - résztvevő létrehozása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0637d-211">On hello **Interrelate - Create Participant** dialog, perform hello following steps:</span></span>
   
    ![Felhasználó létrehozása][401]
    
    <span data-ttu-id="0637d-213">a.</span><span class="sxs-lookup"><span data-stu-id="0637d-213">a.</span></span> <span data-ttu-id="0637d-214">Írja be szükséges hello attribútumok Britta Simon a kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="0637d-214">Type hello required attributes for Britta Simon into related textboxes.</span></span>
    
    >[!IMPORTANT]
    ><span data-ttu-id="0637d-215">A PerformanceCentre attribútumnak kell lennie Britta tartozó felhasználónév hello megegyeznek a felhasználónév hello Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0637d-215">Britta's User Name attribute in PerformanceCentre must be hello same as hello User Name in Azure AD.</span></span>
    
    <span data-ttu-id="0637d-216">b.</span><span class="sxs-lookup"><span data-stu-id="0637d-216">b.</span></span> <span data-ttu-id="0637d-217">Válassza ki **ügyfél rendszergazda** , **válassza ki a szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="0637d-217">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="0637d-218">c.</span><span class="sxs-lookup"><span data-stu-id="0637d-218">c.</span></span> <span data-ttu-id="0637d-219">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0637d-219">Click **Save**.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0637d-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0637d-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0637d-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPerformanceCentre megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="0637d-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPerformanceCentre.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0637d-223">**tooassign Britta Simon tooPerformanceCentre, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="0637d-223">**tooassign Britta Simon tooPerformanceCentre, perform hello following steps:**</span></span>

1. <span data-ttu-id="0637d-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0637d-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0637d-226">Hello alkalmazások listában válassza ki a **PerformanceCentre**.</span><span class="sxs-lookup"><span data-stu-id="0637d-226">In hello applications list, select **PerformanceCentre**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. <span data-ttu-id="0637d-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0637d-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0637d-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0637d-230">Click **Add** button.</span></span> <span data-ttu-id="0637d-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0637d-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0637d-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0637d-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0637d-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0637d-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0637d-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0637d-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0637d-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0637d-236">Testing single sign-on</span></span>

<span data-ttu-id="0637d-237">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="0637d-237">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="0637d-238">Ha a hozzáférési Panel hello hello PerformanceCentre csempe gombra kattint, automatikusan bejelentkezett tooyour PerformanceCentre alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="0637d-238">When you click hello PerformanceCentre tile in hello Access Panel, you should get automatically signed-on tooyour PerformanceCentre application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0637d-239">További források</span><span class="sxs-lookup"><span data-stu-id="0637d-239">Additional resources</span></span>

* [<span data-ttu-id="0637d-240">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0637d-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0637d-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0637d-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

