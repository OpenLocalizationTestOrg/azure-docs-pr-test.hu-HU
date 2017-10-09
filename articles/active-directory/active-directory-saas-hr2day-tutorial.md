---
title: "Oktatóanyag: Azure Active Directoryval integrált által Merces HR2day |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és HR2day által Merces között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 257233767e1fddbaf115878cb0455acf61b2f5f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a><span data-ttu-id="52b46-103">Oktatóanyag: Azure Active Directoryval integrált HR2day Merces által</span><span class="sxs-lookup"><span data-stu-id="52b46-103">Tutorial: Azure Active Directory integration with HR2day by Merces</span></span>

<span data-ttu-id="52b46-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate HR2day által Merces az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="52b46-104">In this tutorial, you learn how toointegrate HR2day by Merces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52b46-105">HR2day által Merces integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="52b46-105">Integrating HR2day by Merces with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="52b46-106">Az Azure AD hozzáférési tooHR2day által Merces rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="52b46-106">You can control in Azure AD who has access tooHR2day by Merces.</span></span>
- <span data-ttu-id="52b46-107">Engedélyezheti a felhasználók tooautomatically beolvasása aláírva a tooHR2day Merces által az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="52b46-107">You can enable your users tooautomatically get signed in tooHR2day by Merces with their Azure AD accounts.</span></span>
- <span data-ttu-id="52b46-108">A fiók egyetlen központi helyen – hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="52b46-108">You can manage your accounts in one central location--hello Azure portal.</span></span>

<span data-ttu-id="52b46-109">Az Azure AD SaaS integrálásáról további információért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52b46-109">For more information about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52b46-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="52b46-110">Prerequisites</span></span>

<span data-ttu-id="52b46-111">tooconfigure HR2day Merces által az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="52b46-111">tooconfigure Azure AD integration with HR2day by Merces, you need hello following items:</span></span>

- <span data-ttu-id="52b46-112">Az Azure AD-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="52b46-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="52b46-113">Egy HR2day által Merces egyszeri bejelentkezés előfizetés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="52b46-113">An HR2day by Merces single sign-on enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="52b46-114">Ebben az oktatóanyagban lépések egy éles környezetben tootest hello használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="52b46-114">We don't recommend using a production environment tootest hello steps in this tutorial.</span></span>

<span data-ttu-id="52b46-115">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="52b46-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="52b46-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="52b46-116">Don't use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="52b46-117">Első egy [ingyenes egy hónapos az Azure AD](https://azure.microsoft.com/pricing/free-trial/) Ha már nincs.</span><span class="sxs-lookup"><span data-stu-id="52b46-117">Get a [one-month free trial of Azure AD](https://azure.microsoft.com/pricing/free-trial/) if you don't already have it.</span></span>  

## <a name="scenario-description"></a><span data-ttu-id="52b46-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="52b46-118">Scenario description</span></span>
<span data-ttu-id="52b46-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="52b46-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52b46-120">Itt vázolt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="52b46-120">hello scenario that's outlined here consists of two main building blocks:</span></span>

1. <span data-ttu-id="52b46-121">Hozzáadása által Merces HR2day hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="52b46-121">Adding HR2day by Merces from hello gallery.</span></span>
2. <span data-ttu-id="52b46-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="52b46-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-hr2day-by-merces-from-hello-gallery"></a><span data-ttu-id="52b46-123">Adja hozzá a HR2day által Merces hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="52b46-123">Add HR2day by Merces from hello gallery</span></span>
<span data-ttu-id="52b46-124">az Azure AD által Merces HR2day tooconfigure hello integrációja HR2day Merces által hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="52b46-124">tooconfigure hello integration of HR2day by Merces into Azure AD, add HR2day by Merces from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="52b46-125">**tooadd HR2day Merces hello gyűjteményből, hajtsa végre a megfelelő hello lépések szerint:**</span><span class="sxs-lookup"><span data-stu-id="52b46-125">**tooadd HR2day by Merces from hello gallery, take hello following steps:**</span></span>

1. <span data-ttu-id="52b46-126">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, jelölje ki a hello **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="52b46-126">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="52b46-128">Nyissa meg túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="52b46-128">Go too**Enterprise applications**.</span></span> <span data-ttu-id="52b46-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="52b46-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="52b46-131">tooadd egy új alkalmazást, jelölje be hello **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="52b46-131">tooadd a new application, select hello **New application** button on hello top of hello dialog box.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="52b46-133">Hello keresési mezőbe, írja be a **által Merces HR2day**.</span><span class="sxs-lookup"><span data-stu-id="52b46-133">In hello search box, type **HR2day by Merces**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. <span data-ttu-id="52b46-135">Hello eredmények panelen, jelölje ki a **HR2day Merces által**, majd válassza ki a hello **hozzáadása** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="52b46-135">In hello results panel, select **HR2day by Merces**, and then select hello **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="52b46-137">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="52b46-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="52b46-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a HR2day "Britta Simon." nevű tesztfelhasználó alapján Merces által</span><span class="sxs-lookup"><span data-stu-id="52b46-138">In this section, you configure and test Azure AD single sign-on with HR2day by Merces based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="52b46-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow, aki hello megfelelőjére felhasználó által Merces HR2day tooa felhasználói Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="52b46-139">For single sign-on toowork, Azure AD needs tooknow who hello counterpart user in HR2day by Merces is tooa user in Azure AD.</span></span> <span data-ttu-id="52b46-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello HR2day Merces által a közötti kapcsolat tooestablish van szüksége.</span><span class="sxs-lookup"><span data-stu-id="52b46-140">In other words, you need tooestablish a link between an Azure AD user and hello related user in HR2day by Merces.</span></span>

<span data-ttu-id="52b46-141">HR2day Merces által, rendelje hozzá hello **felhasználónév** az Azure AD túl **felhasználónév** tooestablish hello kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="52b46-141">In HR2day by Merces, assign hello **user name** in Azure AD too **Username** tooestablish hello relationship.</span></span>

<span data-ttu-id="52b46-142">tooconfigure és az Azure AD az egyszeri bejelentkezés által Merces HR2day-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="52b46-142">tooconfigure and test Azure AD single sign-on with HR2day by Merces, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="52b46-143">[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on): a funkció engedélyezése a felhasználók toouse.</span><span class="sxs-lookup"><span data-stu-id="52b46-143">[Configure Azure AD single sign-on](#configuring-azure-ad-single-sign-on): Enable your users toouse this feature.</span></span>
2. <span data-ttu-id="52b46-144">[Hozzon létre egy Azure AD-teszt felhasználó](#creating-an-azure-ad-test-user): tesztelése az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52b46-144">[Create an Azure AD test user](#creating-an-azure-ad-test-user): Test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52b46-145">[Hozzon létre egy HR2day Merces teszt felhasználó](#creating-an-hr2day-by-merces-test-user): hozzon létre egy megfelelője a Britta Simon HR2day által Merces, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="52b46-145">[Create an HR2day by Merces test user](#creating-an-hr2day-by-merces-test-user): Create a counterpart of Britta Simon in HR2day by Merces that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="52b46-146">[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user): az Azure AD az egyszeri bejelentkezés engedélyezése Britta Simon toouse.</span><span class="sxs-lookup"><span data-stu-id="52b46-146">[Assign hello Azure AD test user](#assigning-the-azure-ad-test-user): Enable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52b46-147">[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on): Győződjön meg arról, hogy működik-e hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="52b46-147">[Test single sign-on](#testing-single-sign-on): Verify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="52b46-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="52b46-148">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="52b46-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez a HR2day Merces alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="52b46-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HR2day by Merces application.</span></span>

<span data-ttu-id="52b46-150">**az Azure AD tooconfigure egyszeri bejelentkezés HR2day Merces, amelyet a érvénybe hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="52b46-150">**tooconfigure Azure AD single sign-on with HR2day by Merces, take hello following steps:**</span></span>

1. <span data-ttu-id="52b46-151">Az Azure portál, a hello hello **által Merces HR2day** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="52b46-151">In hello Azure portal, on hello **HR2day by Merces** application integration page, select **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="52b46-153">tooenable egyszeri bejelentkezést, a hello **egyszeri bejelentkezés** párbeszédpanelen jelölje ki **mód** , **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="52b46-153">tooenable single sign-on, in hello **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. <span data-ttu-id="52b46-155">A hello **HR2day Merces tartomány és az URL-címek** csoportjában tenni a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="52b46-155">In hello **HR2day by Merces Domain and URLs** section, take hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    <span data-ttu-id="52b46-157">a.</span><span class="sxs-lookup"><span data-stu-id="52b46-157">a.</span></span> <span data-ttu-id="52b46-158">A hello **bejelentkezési URL-cím** mezőbe írja be egy URL-cím a következő mintát hello segítségével: `https://<tenantname>.force.com/<instancename>`.</span><span class="sxs-lookup"><span data-stu-id="52b46-158">In hello **Sign-on URL** box, type a URL by using hello following pattern: `https://<tenantname>.force.com/<instancename>`.</span></span>

    <span data-ttu-id="52b46-159">b.</span><span class="sxs-lookup"><span data-stu-id="52b46-159">b.</span></span> <span data-ttu-id="52b46-160">A hello **azonosító** mezőbe írja be egy URL-cím a következő mintát hello segítségével: `https://hr2day.force.com/<companyname>`.</span><span class="sxs-lookup"><span data-stu-id="52b46-160">In hello **Identifier** box, type a URL by using hello following pattern: `https://hr2day.force.com/<companyname>`.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="52b46-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="52b46-161">These values are not real.</span></span> <span data-ttu-id="52b46-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="52b46-162">Update these values with hello actual sign-on URL and identifier.</span></span> <span data-ttu-id="52b46-163">Kapcsolattartási hello [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="52b46-163">Contact hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl) tooget these values.</span></span> 
 


4. <span data-ttu-id="52b46-164">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **Certificate(Base64)**, majd mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="52b46-164">On hello **SAML Signing Certificate** section, select **Certificate(Base64)**, and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. <span data-ttu-id="52b46-166">Ez a szakasz ismerteti, hogyan tooenable felhasználók tooauthenticate tooHR2day Merces által az Azure AD-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="52b46-166">This section describes how tooenable users tooauthenticate tooHR2day by Merces with their account in Azure AD.</span></span> <span data-ttu-id="52b46-167">Ennek segítségével tehetik összevonási hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="52b46-167">They do this by using federation that's based on hello SAML protocol.</span></span>

    <span data-ttu-id="52b46-168">A HR2day Merces alkalmazás által meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour SAML-jogkivonat hello SAML helyességi feltételek vár.</span><span class="sxs-lookup"><span data-stu-id="52b46-168">Your HR2day by Merces application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token.</span></span> <span data-ttu-id="52b46-169">a következő képernyőkép hello ez példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="52b46-169">hello following screenshot shows an example of this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    <span data-ttu-id="52b46-171">Hello SAML-előfeltétel konfigurálása előtt vegye fel a kapcsolatot hello [HR2day Merces ügyfél-támogatási csapat](mailto:servicedesk@merces.nl) hello egyedi azonosító attribútum értékének hello kérik a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="52b46-171">Before you can configure hello SAML assertion, you must contact hello [HR2day by Merces Client support team](mailto:servicedesk@merces.nl) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="52b46-172">Az érték toocomplete hello lépéseket hello a következő szakaszban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="52b46-172">You need this value toocomplete hello steps in hello next section.</span></span> 

6. <span data-ttu-id="52b46-173">A hello **egyszeri bejelentkezés** párbeszédpanel hello **felhasználói attribútumok** területen hello SAML-jogkivonat attribútum konfigurálja, a kép a következő hello látható módon.</span><span class="sxs-lookup"><span data-stu-id="52b46-173">In hello **Single sign-on** dialog box, in hello **User Attributes** section, configure hello SAML token attribute as shown in hello following image.</span></span> <span data-ttu-id="52b46-174">Majd hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="52b46-174">Then take hello following steps.</span></span>
    
      | <span data-ttu-id="52b46-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="52b46-175">Attribute name</span></span>    |   <span data-ttu-id="52b46-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="52b46-176">Attribute value</span></span> |  
    | ------------------- | -------------------- |    
    | <span data-ttu-id="52b46-177">ATTR_LOGINCLAIM</span><span class="sxs-lookup"><span data-stu-id="52b46-177">ATTR_LOGINCLAIM</span></span> | <span data-ttu-id="52b46-178">csatlakoztatás ([mail] "102938475Z", "@"</span><span class="sxs-lookup"><span data-stu-id="52b46-178">join([mail],"102938475Z","@"</span></span> |
    
      <span data-ttu-id="52b46-179">a.</span><span class="sxs-lookup"><span data-stu-id="52b46-179">a.</span></span> <span data-ttu-id="52b46-180">tooopen hello **attribútum hozzáadása** párbeszédablakban válassza **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="52b46-180">tooopen hello **Add Attribute** dialog, select **Add attribute**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="52b46-183">b.</span><span class="sxs-lookup"><span data-stu-id="52b46-183">b.</span></span> <span data-ttu-id="52b46-184">A hello **neve** mezőbe írja be **ATTR_LOGINCLAIM**.</span><span class="sxs-lookup"><span data-stu-id="52b46-184">In hello **Name** box, type **ATTR_LOGINCLAIM**.</span></span>

    <span data-ttu-id="52b46-185">c.</span><span class="sxs-lookup"><span data-stu-id="52b46-185">c.</span></span> <span data-ttu-id="52b46-186">A hello **érték** listáról válassza ki **Join()**.</span><span class="sxs-lookup"><span data-stu-id="52b46-186">From hello **Value** list, select **Join()**.</span></span>

    <span data-ttu-id="52b46-187">d.</span><span class="sxs-lookup"><span data-stu-id="52b46-187">d.</span></span> <span data-ttu-id="52b46-188">A hello **karakterlánc1** listáról válassza ki **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="52b46-188">From hello **String1** list, select **user.mail**.</span></span>

    <span data-ttu-id="52b46-189">e.</span><span class="sxs-lookup"><span data-stu-id="52b46-189">e.</span></span> <span data-ttu-id="52b46-190">A **karakterlánc2**, írja be a HR2day csapata által biztosított hello egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="52b46-190">For **String2**, type hello unique identifier that's provided by your HR2day team.</span></span>

    <span data-ttu-id="52b46-191">f.</span><span class="sxs-lookup"><span data-stu-id="52b46-191">f.</span></span> <span data-ttu-id="52b46-192">A hello **elválasztó** mezőbe írja be  **@** .</span><span class="sxs-lookup"><span data-stu-id="52b46-192">In hello **Separator** box, type **@**.</span></span>
    
    <span data-ttu-id="52b46-193">g.</span><span class="sxs-lookup"><span data-stu-id="52b46-193">g.</span></span> <span data-ttu-id="52b46-194">Válassza ki **Ok**.</span><span class="sxs-lookup"><span data-stu-id="52b46-194">Select **Ok**.</span></span>

7. <span data-ttu-id="52b46-195">Jelölje be hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="52b46-195">Select hello **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="52b46-197">A hello **Merces konfigurációja HR2day** szakaszban jelölje be **konfigurálása HR2day által Merces** tooopen hello **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="52b46-197">In hello **HR2day by Merces Configuration** section, select **Configure HR2day by Merces** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="52b46-198">Másolás hello **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="52b46-198">Copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference** section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. <span data-ttu-id="52b46-200">az alkalmazás, ügyfél hello az SSO tooconfigure [Merces ügyfél-támogatási csapat HR2day](mailTo:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="52b46-200">tooconfigure SSO  for your application, contact hello [HR2day by Merces client support team](mailTo:servicedesk@merces.nl).</span></span> <span data-ttu-id="52b46-201">Csatlakoztassa a letöltött hello **Certificate(Base64)** tooyour e-mail fájlt.</span><span class="sxs-lookup"><span data-stu-id="52b46-201">Attach hello downloaded **Certificate(Base64)** file tooyour email.</span></span> <span data-ttu-id="52b46-202">Hello adni **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** , hogy az egyszeri bejelentkezés integráció konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="52b46-202">Also provide hello **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** so that they can be configured for SSO integration.</span></span>

    > [!NOTE]
    ><span data-ttu-id="52b46-203">Említse meg, amelyet ez az integráció beállítása hello mintával Entitásazonosító toobe hello toohello Merces team **https://hr2day.force.com/INSTANCENAME**.</span><span class="sxs-lookup"><span data-stu-id="52b46-203">Mention toohello Merces team that this integration needs hello Entity ID toobe set with hello pattern **https://hr2day.force.com/INSTANCENAME**.</span></span>

    > [!TIP]
    ><span data-ttu-id="52b46-204">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="52b46-204">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="52b46-205">Miután hozzáadta az alkalmazás a hello **Active Directory** > **vállalati alkalmazások** szakaszban, jelölje be hello **egyszeri bejelentkezés** lapon. Majd hozzáférés hello beágyazott keresztül hello dokumentáció **konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="52b46-205">After you add this app from hello **Active Directory** > **Enterprise Applications** section, select hello **Single Sign-On** tab. Then access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="52b46-206">További szolgáltatásáról hello embedded dokumentációjából hello [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="52b46-206">You can read more about hello embedded documentation feature in hello [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="52b46-207">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="52b46-207">Create an Azure AD test user</span></span>
<span data-ttu-id="52b46-208">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="52b46-208">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="52b46-210">**az Azure AD, a következő lépéseket hajtsa végre a megfelelő hello tesztfelhasználó toocreate:**</span><span class="sxs-lookup"><span data-stu-id="52b46-210">**toocreate a test user in Azure AD, take hello following steps:**</span></span>

1. <span data-ttu-id="52b46-211">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, jelölje ki a hello **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="52b46-211">In hello **Azure portal**, on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="52b46-213">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, majd válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="52b46-213">toodisplay hello list of users, go too**Users and groups**, and then select **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="52b46-215">tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="52b46-215">tooopen hello **User** dialog box, select **Add** on hello top of hello dialog box.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="52b46-217">A hello **felhasználói** párbeszédpanelen hajtsa végre a megfelelő hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="52b46-217">In hello **User** dialog box, take hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="52b46-219">a.</span><span class="sxs-lookup"><span data-stu-id="52b46-219">a.</span></span> <span data-ttu-id="52b46-220">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52b46-220">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52b46-221">b.</span><span class="sxs-lookup"><span data-stu-id="52b46-221">b.</span></span> <span data-ttu-id="52b46-222">A hello **felhasználónév** mezőbe, a típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="52b46-222">In hello **User name** box, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="52b46-223">c.</span><span class="sxs-lookup"><span data-stu-id="52b46-223">c.</span></span> <span data-ttu-id="52b46-224">Válassza ki **megjelenítése jelszó**, majd írja le hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="52b46-224">Select **Show Password**, and then write down hello password.</span></span>

    <span data-ttu-id="52b46-225">d.</span><span class="sxs-lookup"><span data-stu-id="52b46-225">d.</span></span> <span data-ttu-id="52b46-226">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="52b46-226">Select **Create**.</span></span>
 
### <a name="create-an-hr2day-by-merces-test-user"></a><span data-ttu-id="52b46-227">Hozzon létre egy HR2day Merces teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="52b46-227">Create an HR2day by Merces test user</span></span>

<span data-ttu-id="52b46-228">hello ebben a szakaszban célja toocreate Britta Simon a HR2day Merces által meghívott felhasználó.</span><span class="sxs-lookup"><span data-stu-id="52b46-228">hello objective of this section is toocreate a user called Britta Simon in HR2day by Merces.</span></span> <span data-ttu-id="52b46-229">hello használata tooadd hello felhasználók hello HR2day fiók [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="52b46-229">tooadd hello users in hello HR2day account, work with hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span> 

> [!NOTE]
> <span data-ttu-id="52b46-230">Ha egy felhasználó toocreate manuálisan kell, lépjen kapcsolatba a hello [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="52b46-230">If you need toocreate a user manually, contact hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="52b46-231">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="52b46-231">Assign hello Azure AD test user</span></span>

<span data-ttu-id="52b46-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooHR2day által Merces megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="52b46-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooHR2day by Merces.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="52b46-234">**tooassign Britta Simon tooHR2day Merces, hajtsa végre a megfelelő hello lépések szerint:**</span><span class="sxs-lookup"><span data-stu-id="52b46-234">**tooassign Britta Simon tooHR2day by Merces, take hello following steps:**</span></span>

1. <span data-ttu-id="52b46-235">A hello Azure portál, nyissa meg hello alkalmazások megtekintéséhez nyissa toohello könyvtár nézetben, és keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="52b46-235">In hello Azure portal, open hello applications view, go toohello directory view, and then go too**Enterprise applications**.</span></span> <span data-ttu-id="52b46-236">Válassza ki, **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="52b46-236">Next, select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="52b46-238">Hello alkalmazások listában válassza ki a **által Merces HR2day**.</span><span class="sxs-lookup"><span data-stu-id="52b46-238">In hello applications list, select **HR2day by Merces**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. <span data-ttu-id="52b46-240">Hello hello bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="52b46-240">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="52b46-242">Jelölje be hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="52b46-242">Select hello **Add** button.</span></span> <span data-ttu-id="52b46-243">Ezt követően a hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="52b46-243">Then, in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="52b46-245">A hello **felhasználók és csoportok** párbeszédpanel hello **felhasználók** listáról válassza ki **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="52b46-245">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="52b46-246">Kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="52b46-246">Click hello **Select** button.</span></span>

7. <span data-ttu-id="52b46-247">A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="52b46-247">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="52b46-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="52b46-248">Test single sign-on</span></span>

<span data-ttu-id="52b46-249">hello ebben a szakaszban célja tootest az Azure AD egyszeri bejelentkezés beállításai használatával hello hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="52b46-249">hello objective of this section is tootest your Azure AD single sign-on configuration by using hello Access Panel.</span></span>  

<span data-ttu-id="52b46-250">Ha hello HR2day jelöl ki a hozzáférési Panel hello Merces csempe, automatikusan lekérni jelentkezett be tooyour HR2day Merces alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="52b46-250">When you select hello HR2day by Merces tile in hello Access Panel, you automatically get signed in  tooyour HR2day by Merces application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52b46-251">További források</span><span class="sxs-lookup"><span data-stu-id="52b46-251">Additional resources</span></span>

* [<span data-ttu-id="52b46-252">Kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="52b46-252">List of tutorials about how tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52b46-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="52b46-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

