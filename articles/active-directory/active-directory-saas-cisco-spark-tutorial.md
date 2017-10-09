---
title: "Oktatóanyag: Azure Active Directoryval integrált Cisco Spark |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Cisco Spark között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 386c4fd816095e1c61de01dd1dee1bbd00311a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a><span data-ttu-id="c6fc1-103">Oktatóanyag: Azure Active Directoryval integrált Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="c6fc1-103">Tutorial: Azure Active Directory integration with Cisco Spark</span></span>

<span data-ttu-id="c6fc1-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Cisco Spark az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c6fc1-104">In this tutorial, you learn how toointegrate Cisco Spark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c6fc1-105">Cisco Spark integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-105">Integrating Cisco Spark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c6fc1-106">Megadhatja a hozzáférés tooCisco Spark rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c6fc1-106">You can control in Azure AD who has access tooCisco Spark</span></span>
- <span data-ttu-id="c6fc1-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCisco (egyszeri bejelentkezés) Spark az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c6fc1-107">You can enable your users tooautomatically get signed-on tooCisco Spark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c6fc1-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c6fc1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c6fc1-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c6fc1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6fc1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c6fc1-110">Prerequisites</span></span>

<span data-ttu-id="c6fc1-111">Cisco Spark az Azure AD integrálása tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-111">tooconfigure Azure AD integration with Cisco Spark, you need hello following items:</span></span>

- <span data-ttu-id="c6fc1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c6fc1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c6fc1-113">A Cisco Spark egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c6fc1-113">A Cisco Spark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c6fc1-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c6fc1-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c6fc1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c6fc1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c6fc1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c6fc1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c6fc1-118">Scenario description</span></span>
<span data-ttu-id="c6fc1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c6fc1-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c6fc1-121">Cisco Spark hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c6fc1-121">Adding Cisco Spark from hello gallery</span></span>
2. <span data-ttu-id="c6fc1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c6fc1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cisco-spark-from-hello-gallery"></a><span data-ttu-id="c6fc1-123">Cisco Spark hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c6fc1-123">Adding Cisco Spark from hello gallery</span></span>
<span data-ttu-id="c6fc1-124">tooconfigure hello integrációja Cisco Spark az Azure AD-be, meg kell tooadd Cisco Spark hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-124">tooconfigure hello integration of Cisco Spark into Azure AD, you need tooadd Cisco Spark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c6fc1-125">**tooadd Cisco Spark hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c6fc1-125">**tooadd Cisco Spark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6fc1-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c6fc1-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c6fc1-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c6fc1-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c6fc1-133">Hello keresési mezőbe, írja be a **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-133">In hello search box, type **Cisco Spark**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. <span data-ttu-id="c6fc1-135">A hello eredmények panelen válassza ki a **Cisco Spark**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-135">In hello results panel, select **Cisco Spark**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c6fc1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c6fc1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c6fc1-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Cisco Spark "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="c6fc1-138">In this section, you configure and test Azure AD single sign-on with Cisco Spark based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c6fc1-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Cisco Spark tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cisco Spark is tooa user in Azure AD.</span></span> <span data-ttu-id="c6fc1-140">Ez azt jelenti hello kapcsolódó felhasználói Cisco Spark és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-140">In other words, a link relationship between an Azure AD user and hello related user in Cisco Spark needs toobe established.</span></span>

<span data-ttu-id="c6fc1-141">Cisco Spark, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-141">In Cisco Spark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c6fc1-142">tooconfigure és a Cisco Spark az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-142">tooconfigure and test Azure AD single sign-on with Cisco Spark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c6fc1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c6fc1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c6fc1-145">**[Cisco Spark tesztfelhasználó létrehozása](#creating-a-cisco-spark-test-user)**  -toohave egy megfelelője a Britta Simon Cisco Spark, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-145">**[Creating a Cisco Spark test user](#creating-a-cisco-spark-test-user)** - toohave a counterpart of Britta Simon in Cisco Spark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c6fc1-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c6fc1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c6fc1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6fc1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c6fc1-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Cisco Spark alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cisco Spark application.</span></span>

<span data-ttu-id="c6fc1-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Cisco Spark, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c6fc1-150">**tooconfigure Azure AD single sign-on with Cisco Spark, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6fc1-151">Az Azure portál, a hello hello **Cisco Spark** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-151">In hello Azure portal, on hello **Cisco Spark** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c6fc1-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. <span data-ttu-id="c6fc1-155">A hello **Cisco Spark tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-155">On hello **Cisco Spark Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    <span data-ttu-id="c6fc1-157">a.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-157">a.</span></span> <span data-ttu-id="c6fc1-158">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://web.ciscospark.com/#/signin`</span><span class="sxs-lookup"><span data-stu-id="c6fc1-158">In hello **Sign-on URL** textbox, type a URL as: `https://web.ciscospark.com/#/signin`</span></span>

    <span data-ttu-id="c6fc1-159">b.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-159">b.</span></span> <span data-ttu-id="c6fc1-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://idbroker.webex.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="c6fc1-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://idbroker.webex.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c6fc1-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-161">This value is not real.</span></span> <span data-ttu-id="c6fc1-162">Frissítse ezt az értéket hello tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-162">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="c6fc1-163">Ügyfél [Cisco Spark ügyfél-támogatási csoport](https://support.ciscospark.com/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-163">Contact [Cisco Spark Client support team](https://support.ciscospark.com/) tooget this value.</span></span> 
 
4. <span data-ttu-id="c6fc1-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. <span data-ttu-id="c6fc1-166">Cisco Spark alkalmazás hello SAML helyességi feltételek toocontain adott attribútumok vár.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-166">Cisco Spark application expects hello SAML assertions toocontain specific attributes.</span></span> <span data-ttu-id="c6fc1-167">A következő attribútumok ehhez az alkalmazáshoz hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-167">Configure hello following attributes  for this application.</span></span> <span data-ttu-id="c6fc1-168">Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-168">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="c6fc1-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-169">hello following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. <span data-ttu-id="c6fc1-171">A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="c6fc1-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c6fc1-172">Attribute Name</span></span>  | <span data-ttu-id="c6fc1-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c6fc1-173">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    |   <span data-ttu-id="c6fc1-174">egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="c6fc1-174">uid</span></span>    | <span data-ttu-id="c6fc1-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="c6fc1-175">user.userprincipalname</span></span> |   

    <span data-ttu-id="c6fc1-176">a.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-176">a.</span></span> <span data-ttu-id="c6fc1-177">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="c6fc1-180">b.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-180">b.</span></span> <span data-ttu-id="c6fc1-181">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="c6fc1-182">c.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-182">c.</span></span> <span data-ttu-id="c6fc1-183">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c6fc1-184">d.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-184">d.</span></span> <span data-ttu-id="c6fc1-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-185">Click **Ok**.</span></span>

7. <span data-ttu-id="c6fc1-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c6fc1-188">Jelentkezzen be a túl[Cisco Cloud Collaboration felügyeleti](https://admin.ciscospark.com/) a teljes körű rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-188">Sign in too[Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

9. <span data-ttu-id="c6fc1-189">Válassza ki **beállítások** a hello **hitelesítési** kattintson **módosítás**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-189">Select **Settings** and under hello **Authentication** section, click **Modify**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. <span data-ttu-id="c6fc1-191">Válassza ki **a 3. fél identitásszolgáltató integrálását. (Speciális)**  és toohello nyissa meg a következő képernyő.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-191">Select **Integrate a 3rd-party identity provider. (Advanced)** and go toohello next screen.</span></span>

11. <span data-ttu-id="c6fc1-192">A hello **Idp metaadatok importálása** lapon, vagy húzza dobja el az Azure AD hello metaadatfájl hello lapra vagy használ hello fájl böngésző beállítás toolocate és feltöltése az Azure AD hello metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-192">On hello **Import Idp Metadata** page, either drag and drop hello Azure AD metadata file onto hello page or use hello file browser option toolocate and upload hello Azure AD metadata file.</span></span> <span data-ttu-id="c6fc1-193">Ezt követően válassza **szükséges metaadatokat (biztonságosabb) a hitelesítésszolgáltató által aláírt tanúsítvány** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-193">Then, select **Require certificate signed by a certificate authority in Metadata (more secure)** and click **Next**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. <span data-ttu-id="c6fc1-195">Válassza ki **SSO-kapcsolat tesztelése**, és egy új böngészőlapon nyílik meg, amikor hitelesítéséhez az Azure ad-vel történő bejelentkezéssel.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-195">Select **Test SSO Connection**, and when a new browser tab opens, authenticate with Azure AD by signing in.</span></span>

13. <span data-ttu-id="c6fc1-196">Térjen vissza a toohello **Cisco Cloud Collaboration felügyeleti** böngészőlapon. Ha hello teszt sikeres volt, válassza ki a **Ez a teszt sikeres volt. Engedélyezi az egyszeri bejelentkezés beállítást** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-196">Return toohello **Cisco Cloud Collaboration Management** browser tab. If hello test was successful, select **This test was successful. Enable Single Sign-On option** and click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="c6fc1-197">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c6fc1-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c6fc1-198">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c6fc1-199">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c6fc1-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c6fc1-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6fc1-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="c6fc1-201">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c6fc1-203">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c6fc1-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6fc1-204">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c6fc1-206">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c6fc1-208">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c6fc1-210">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c6fc1-212">a.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-212">a.</span></span> <span data-ttu-id="c6fc1-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c6fc1-214">b.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-214">b.</span></span> <span data-ttu-id="c6fc1-215">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c6fc1-216">c.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-216">c.</span></span> <span data-ttu-id="c6fc1-217">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c6fc1-218">d.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-218">d.</span></span> <span data-ttu-id="c6fc1-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-219">Click **Create**.</span></span>
 
### <a name="creating-a-cisco-spark-test-user"></a><span data-ttu-id="c6fc1-220">Cisco Spark tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c6fc1-220">Creating a Cisco Spark test user</span></span>

<span data-ttu-id="c6fc1-221">Ebben a szakaszban egy Cisco Spark Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-221">In this section, you create a user called Britta Simon in Cisco Spark.</span></span> <span data-ttu-id="c6fc1-222">Ebben a szakaszban egy Cisco Spark Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-222">In this section, you create a user called Britta Simon in Cisco Spark.</span></span>

1. <span data-ttu-id="c6fc1-223">Nyissa meg toohello [Cisco Cloud Collaboration felügyeleti](https://admin.ciscospark.com/) a teljes körű rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-223">Go toohello [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

2. <span data-ttu-id="c6fc1-224">Kattintson a **felhasználók** , majd **felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-224">Click **Users** and then **Manage Users**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. <span data-ttu-id="c6fc1-226">A hello **kezelése felhasználó** ablakban válassza ki **manuális hozzáadása vagy módosítása a felhasználók** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-226">In hello **Manage User** window, select **Manually add or modify users** and click **Next**.</span></span>

4. <span data-ttu-id="c6fc1-227">Válassza ki **nevek és E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-227">Select **Names and Email address**.</span></span> <span data-ttu-id="c6fc1-228">Ezután adja meg hello szövegmező az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c6fc1-228">Then, fill out hello textbox as follows:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    <span data-ttu-id="c6fc1-230">a.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-230">a.</span></span> <span data-ttu-id="c6fc1-231">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-231">In hello **First Name** textbox, type **Britta**.</span></span> 
    
    <span data-ttu-id="c6fc1-232">b.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-232">b.</span></span> <span data-ttu-id="c6fc1-233">A hello **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-233">In hello **Last Name** textbox, type **Simon**.</span></span>
    
    <span data-ttu-id="c6fc1-234">c.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-234">c.</span></span> <span data-ttu-id="c6fc1-235">A hello **E-mail cím** szövegmezőhöz típus  **britta.simon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="c6fc1-235">In hello **Email address** textbox, type **britta.simon@contoso.com**.</span></span>

5. <span data-ttu-id="c6fc1-236">Kattintson a hello, valamint tooadd Britta Simon aláírásához.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-236">Click hello plus sign tooadd Britta Simon.</span></span> <span data-ttu-id="c6fc1-237">Ezután kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-237">Then, click **Next**.</span></span>

6. <span data-ttu-id="c6fc1-238">A hello **szolgáltatások hozzáadása a felhasználók** ablak, kattintson a **mentése** , majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-238">In hello **Add Services for Users** window, click **Save** and then **Finish**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c6fc1-239">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c6fc1-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c6fc1-240">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCisco Spark megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCisco Spark.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c6fc1-242">**tooassign Britta Simon tooCisco Spark, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c6fc1-242">**tooassign Britta Simon tooCisco Spark, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6fc1-243">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c6fc1-245">Hello alkalmazások listában válassza ki a **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-245">In hello applications list, select **Cisco Spark**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. <span data-ttu-id="c6fc1-247">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c6fc1-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-249">Click **Add** button.</span></span> <span data-ttu-id="c6fc1-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c6fc1-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c6fc1-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c6fc1-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c6fc1-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c6fc1-255">Testing single sign-on</span></span>

<span data-ttu-id="c6fc1-256">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-256">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="c6fc1-257">Ha a hozzáférési Panel hello hello Cisco Spark csempe gombra kattint, automatikusan bejelentkezett tooyour Cisco Spark alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c6fc1-257">When you click hello Cisco Spark tile in hello Access Panel, you should get automatically signed-on tooyour Cisco Spark application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6fc1-258">További források</span><span class="sxs-lookup"><span data-stu-id="c6fc1-258">Additional resources</span></span>

* [<span data-ttu-id="c6fc1-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c6fc1-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c6fc1-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c6fc1-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

