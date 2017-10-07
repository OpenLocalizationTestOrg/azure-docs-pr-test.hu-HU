---
title: "Oktatóanyag: Azure Active Directoryval integrált Lesson.ly |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Lesson.ly között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 23b339dcc26471b42aaa7e1baadcb42500d6b7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a><span data-ttu-id="b59c7-103">Oktatóanyag: Azure Active Directoryval integrált Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="b59c7-103">Tutorial: Azure Active Directory integration with Lesson.ly</span></span>

<span data-ttu-id="b59c7-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Lesson.ly az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b59c7-104">In this tutorial, you learn how toointegrate Lesson.ly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b59c7-105">Lesson.ly integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b59c7-105">Integrating Lesson.ly with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b59c7-106">Megadhatja a hozzáférés tooLesson.ly rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b59c7-106">You can control in Azure AD who has access tooLesson.ly</span></span>
- <span data-ttu-id="b59c7-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLesson.ly (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b59c7-107">You can enable your users tooautomatically get signed-on tooLesson.ly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b59c7-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b59c7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b59c7-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b59c7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b59c7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b59c7-110">Prerequisites</span></span>

<span data-ttu-id="b59c7-111">az Azure AD integrálása Lesson.ly tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b59c7-111">tooconfigure Azure AD integration with Lesson.ly, you need hello following items:</span></span>

- <span data-ttu-id="b59c7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b59c7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b59c7-113">Egy Lesson.ly egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b59c7-113">A Lesson.ly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b59c7-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b59c7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b59c7-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b59c7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b59c7-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b59c7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b59c7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b59c7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b59c7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b59c7-118">Scenario description</span></span>
<span data-ttu-id="b59c7-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b59c7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b59c7-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b59c7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b59c7-121">Hello gyűjteményből Lesson.ly hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b59c7-121">Adding Lesson.ly from hello gallery</span></span>
2. <span data-ttu-id="b59c7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b59c7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lessonly-from-hello-gallery"></a><span data-ttu-id="b59c7-123">Hello gyűjteményből Lesson.ly hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b59c7-123">Adding Lesson.ly from hello gallery</span></span>
<span data-ttu-id="b59c7-124">tooconfigure hello integrációja Lesson.ly az Azure AD-be, meg kell tooadd Lesson.ly hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b59c7-124">tooconfigure hello integration of Lesson.ly into Azure AD, you need tooadd Lesson.ly from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b59c7-125">**tooadd Lesson.ly hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b59c7-125">**tooadd Lesson.ly from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b59c7-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b59c7-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b59c7-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b59c7-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b59c7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b59c7-133">Hello keresési mezőbe, írja be a **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-133">In hello search box, type **Lesson.ly**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. <span data-ttu-id="b59c7-135">A hello eredmények panelen válassza ki a **Lesson.ly**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-135">In hello results panel, select **Lesson.ly**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b59c7-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b59c7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b59c7-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="b59c7-138">In this section, you configure and test Azure AD single sign-on with Lesson.ly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b59c7-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Lesson.ly tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b59c7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lesson.ly is tooa user in Azure AD.</span></span> <span data-ttu-id="b59c7-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Lesson.ly közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b59c7-140">In other words, a link relationship between an Azure AD user and hello related user in Lesson.ly needs toobe established.</span></span>

<span data-ttu-id="b59c7-141">Lesson.ly, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b59c7-141">In Lesson.ly, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b59c7-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Lesson.ly-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b59c7-142">tooconfigure and test Azure AD single sign-on with Lesson.ly, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b59c7-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b59c7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b59c7-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b59c7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b59c7-145">**[Lesson.ly tesztfelhasználó létrehozása](#creating-a-lessonly-test-user)**  -toohave egy megfelelője a Britta Simon a Lesson.ly, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b59c7-145">**[Creating a Lesson.ly test user](#creating-a-lessonly-test-user)** - toohave a counterpart of Britta Simon in Lesson.ly that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b59c7-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b59c7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b59c7-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b59c7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b59c7-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b59c7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b59c7-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Lesson.ly alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b59c7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lesson.ly application.</span></span>

<span data-ttu-id="b59c7-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Lesson.ly, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b59c7-150">**tooconfigure Azure AD single sign-on with Lesson.ly, perform hello following steps:**</span></span>

1. <span data-ttu-id="b59c7-151">Az Azure portál, a hello hello **Lesson.ly** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-151">In hello Azure portal, on hello **Lesson.ly** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b59c7-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b59c7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. <span data-ttu-id="b59c7-155">A hello **Lesson.ly tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b59c7-155">On hello **Lesson.ly Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    <span data-ttu-id="b59c7-157">a.</span><span class="sxs-lookup"><span data-stu-id="b59c7-157">a.</span></span> <span data-ttu-id="b59c7-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="b59c7-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    ><span data-ttu-id="b59c7-159">Amikor egy általános hivatkozó neve, amely **#companyname** lecserélve a tényleges nevének toobe kell.</span><span class="sxs-lookup"><span data-stu-id="b59c7-159">When referencing a generic name that **companyname** needs toobe replaced by an actual name.</span></span>
    
    <span data-ttu-id="b59c7-160">b.</span><span class="sxs-lookup"><span data-stu-id="b59c7-160">b.</span></span> <span data-ttu-id="b59c7-161">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="b59c7-161">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > <span data-ttu-id="b59c7-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b59c7-162">These values are not real.</span></span> <span data-ttu-id="b59c7-163">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="b59c7-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b59c7-164">Ügyfél [Lesson.ly ügyfél-támogatási csoport](mailto:dev@lessonly.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b59c7-164">Contact [Lesson.ly Client support team](mailto:dev@lessonly.com) tooget these values.</span></span> 

4. <span data-ttu-id="b59c7-165">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b59c7-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. <span data-ttu-id="b59c7-167">hello Lesson.ly alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **SAML-jogkivonat attribútumok** configuration.hello alábbi képernyőfelvételen a példáját mutatja be. Ez.</span><span class="sxs-lookup"><span data-stu-id="b59c7-167">hello Lesson.ly application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.hello following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. <span data-ttu-id="b59c7-169">A hello **felhasználói attribútumok** hello szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a lemezkép megelőző hello látható módon, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b59c7-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>

    | <span data-ttu-id="b59c7-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b59c7-170">Attribute Name</span></span>   | <span data-ttu-id="b59c7-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b59c7-171">Attribute Value</span></span> |
    | ---------------  | ----------------|
    | <span data-ttu-id="b59c7-172">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="b59c7-172">urn:oid:2.5.4.42</span></span> |<span data-ttu-id="b59c7-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="b59c7-173">user.givenname</span></span> |
    | <span data-ttu-id="b59c7-174">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="b59c7-174">urn:oid:2.5.4.4</span></span>  |<span data-ttu-id="b59c7-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="b59c7-175">user.surname</span></span> |
    | <span data-ttu-id="b59c7-176">urn: oid:0.9.2342.19200300.1001.3</span><span class="sxs-lookup"><span data-stu-id="b59c7-176">urn:oid:0.9.2342.19200300.1001.3</span></span> |<span data-ttu-id="b59c7-177">User.mail</span><span class="sxs-lookup"><span data-stu-id="b59c7-177">user.mail</span></span> |

    <span data-ttu-id="b59c7-178">a.</span><span class="sxs-lookup"><span data-stu-id="b59c7-178">a.</span></span> <span data-ttu-id="b59c7-179">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c7-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="b59c7-182">b.</span><span class="sxs-lookup"><span data-stu-id="b59c7-182">b.</span></span> <span data-ttu-id="b59c7-183">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="b59c7-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="b59c7-184">c.</span><span class="sxs-lookup"><span data-stu-id="b59c7-184">c.</span></span> <span data-ttu-id="b59c7-185">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="b59c7-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b59c7-186">d.</span><span class="sxs-lookup"><span data-stu-id="b59c7-186">d.</span></span> <span data-ttu-id="b59c7-187">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-187">Click **Ok**.</span></span>     

7. <span data-ttu-id="b59c7-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-188">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b59c7-190">A hello **Lesson.ly konfigurációs** kattintson **konfigurálása Lesson.ly** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b59c7-190">On hello **Lesson.ly Configuration** section, click **Configure Lesson.ly** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b59c7-191">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b59c7-191">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. <span data-ttu-id="b59c7-193">tooconfigure egyszeri bejelentkezést a **Lesson.ly** oldalon kell letöltött toosend hello **Certificate(Base64)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[Lesson.ly támogatási csoport](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="b59c7-193">tooconfigure single sign-on on **Lesson.ly** side, you need toosend hello downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

> [!TIP]
> <span data-ttu-id="b59c7-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b59c7-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b59c7-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b59c7-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b59c7-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b59c7-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b59c7-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b59c7-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="b59c7-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b59c7-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b59c7-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b59c7-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b59c7-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b59c7-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b59c7-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c7-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b59c7-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b59c7-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b59c7-209">a.</span><span class="sxs-lookup"><span data-stu-id="b59c7-209">a.</span></span> <span data-ttu-id="b59c7-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b59c7-211">b.</span><span class="sxs-lookup"><span data-stu-id="b59c7-211">b.</span></span> <span data-ttu-id="b59c7-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b59c7-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b59c7-213">c.</span><span class="sxs-lookup"><span data-stu-id="b59c7-213">c.</span></span> <span data-ttu-id="b59c7-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b59c7-215">d.</span><span class="sxs-lookup"><span data-stu-id="b59c7-215">d.</span></span> <span data-ttu-id="b59c7-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-216">Click **Create**.</span></span>
 
### <a name="creating-a-lessonly-test-user"></a><span data-ttu-id="b59c7-217">Lesson.ly tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b59c7-217">Creating a Lesson.ly test user</span></span>

<span data-ttu-id="b59c7-218">hello ebben a szakaszban célja toocreate Lesson.ly Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b59c7-218">hello objective of this section is toocreate a user called Britta Simon in Lesson.ly.</span></span> <span data-ttu-id="b59c7-219">Lesson.LY támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="b59c7-219">Lesson.ly supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b59c7-220">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="b59c7-220">There is no action item for you in this section.</span></span> <span data-ttu-id="b59c7-221">Új felhasználó létrejön egy kísérlet tooaccess Lesson.ly során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="b59c7-221">A new user will be created during an attempt tooaccess Lesson.ly if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="b59c7-222">Ha egy felhasználó toocreate manuálisan kell, toocontact hello kell [Lesson.ly támogatási csoport](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="b59c7-222">If you need toocreate an user manually, you need toocontact hello [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b59c7-223">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b59c7-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b59c7-224">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLesson.ly megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b59c7-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLesson.ly.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b59c7-226">**tooassign Britta Simon tooLesson.ly, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b59c7-226">**tooassign Britta Simon tooLesson.ly, perform hello following steps:**</span></span>

1. <span data-ttu-id="b59c7-227">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b59c7-229">Hello alkalmazások listában válassza ki a **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-229">In hello applications list, select **Lesson.ly**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. <span data-ttu-id="b59c7-231">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b59c7-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b59c7-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b59c7-233">Click **Add** button.</span></span> <span data-ttu-id="b59c7-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c7-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b59c7-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b59c7-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b59c7-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c7-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b59c7-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b59c7-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b59c7-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b59c7-239">Testing single sign-on</span></span>

<span data-ttu-id="b59c7-240">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="b59c7-240">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b59c7-241">Ha a hozzáférési Panel hello hello Lesson.ly csempe gombra kattint, automatikusan bejelentkezett tooyour Lesson.ly alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b59c7-241">When you click hello Lesson.ly tile in hello Access Panel, you should get automatically signed-on tooyour Lesson.ly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b59c7-242">További források</span><span class="sxs-lookup"><span data-stu-id="b59c7-242">Additional resources</span></span>

* [<span data-ttu-id="b59c7-243">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b59c7-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b59c7-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b59c7-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png

