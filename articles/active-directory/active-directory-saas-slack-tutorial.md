---
title: "Oktatóanyag: Azure Active Directoryval integrált Slackhez |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Slackhez között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 7f0151401af4dc63d2f714d4b4f66380c4b51e0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a><span data-ttu-id="b2b0d-103">Oktatóanyag: Azure Active Directoryval integrált Slackhez</span><span class="sxs-lookup"><span data-stu-id="b2b0d-103">Tutorial: Azure Active Directory integration with Slack</span></span>

<span data-ttu-id="b2b0d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Slack-e az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b2b0d-104">In this tutorial, you learn how toointegrate Slack with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b2b0d-105">Slackhez integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-105">Integrating Slack with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b2b0d-106">Megadhatja a hozzáférés tooSlack rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b2b0d-106">You can control in Azure AD who has access tooSlack</span></span>
- <span data-ttu-id="b2b0d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSlack (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b2b0d-107">You can enable your users tooautomatically get signed-on tooSlack (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b2b0d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b2b0d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b2b0d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b2b0d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2b0d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b2b0d-110">Prerequisites</span></span>

<span data-ttu-id="b2b0d-111">tooconfigure az Azure AD-integráció a Slackhez, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-111">tooconfigure Azure AD integration with Slack, you need hello following items:</span></span>

- <span data-ttu-id="b2b0d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b2b0d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b2b0d-113">A Slack egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b2b0d-113">A Slack single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b2b0d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b2b0d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b2b0d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b2b0d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b2b0d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b2b0d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b2b0d-118">Scenario description</span></span>
<span data-ttu-id="b2b0d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b2b0d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b2b0d-121">Hozzáadás a Slackhez hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b2b0d-121">Adding Slack from hello gallery</span></span>
2. <span data-ttu-id="b2b0d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b2b0d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-slack-from-hello-gallery"></a><span data-ttu-id="b2b0d-123">Hozzáadás a Slackhez hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b2b0d-123">Adding Slack from hello gallery</span></span>
<span data-ttu-id="b2b0d-124">tooconfigure hello integrálása az Azure AD-be Slackhez, meg kell tooadd Slackhez hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-124">tooconfigure hello integration of Slack into Azure AD, you need tooadd Slack from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b2b0d-125">**tooadd Slackhez hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b2b0d-125">**tooadd Slack from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2b0d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b2b0d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b2b0d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b2b0d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b2b0d-133">Hello keresési mezőbe, írja be a **Slackhez**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-133">In hello search box, type **Slack**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. <span data-ttu-id="b2b0d-135">A hello eredmények panelen válassza ki a **Slackhez**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-135">In hello results panel, select **Slack**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b2b0d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b2b0d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b2b0d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Slackhez.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-138">In this section, you configure and test Azure AD single sign-on with Slack based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b2b0d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Slackhez tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Slack is tooa user in Azure AD.</span></span> <span data-ttu-id="b2b0d-140">Ez azt jelenti hello kapcsolódó felhasználó a Slackhez és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-140">In other words, a link relationship between an Azure AD user and hello related user in Slack needs toobe established.</span></span>

<span data-ttu-id="b2b0d-141">A Slackhez, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-141">In Slack, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b2b0d-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Slackhez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-142">tooconfigure and test Azure AD single sign-on with Slack, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b2b0d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b2b0d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b2b0d-145">**[Slack tesztfelhasználó létrehozása](#creating-a-slack-test-user)**  -toohave egy megfelelője a Britta Simon a Slackhez, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-145">**[Creating a Slack test user](#creating-a-slack-test-user)** - toohave a counterpart of Britta Simon in Slack that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b2b0d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b2b0d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b2b0d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2b0d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b2b0d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az közzététele a Slack-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Slack application.</span></span>

<span data-ttu-id="b2b0d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Slackhez, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b2b0d-150">**tooconfigure Azure AD single sign-on with Slack, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2b0d-151">Az Azure portál, a hello hello **Slackhez** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-151">In hello Azure portal, on hello **Slack** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b2b0d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. <span data-ttu-id="b2b0d-155">A hello **Slackhez tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-155">On hello **Slack Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    <span data-ttu-id="b2b0d-157">a.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-157">a.</span></span> <span data-ttu-id="b2b0d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.slack.com`</span><span class="sxs-lookup"><span data-stu-id="b2b0d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.slack.com`</span></span>

    <span data-ttu-id="b2b0d-159">b.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-159">b.</span></span> <span data-ttu-id="b2b0d-160">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://slack.com`</span><span class="sxs-lookup"><span data-stu-id="b2b0d-160">In hello **Identifier** textbox, type hello URL: `https://slack.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b2b0d-161">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-161">hello value is not real.</span></span> <span data-ttu-id="b2b0d-162">Hogy tooupdate hello értékének hello tényleges bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-162">You have tooupdate hello value with hello actual Sign On URL.</span></span> <span data-ttu-id="b2b0d-163">Ügyfél [Slack támogatási csoport](https://slack.com/help/contact) tooget hello érték</span><span class="sxs-lookup"><span data-stu-id="b2b0d-163">Contact [Slack support team](https://slack.com/help/contact) tooget hello value</span></span>
     
4. <span data-ttu-id="b2b0d-164">Alkalmazás közzététele a Slack hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-164">Slack application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="b2b0d-165">Az alkalmazás jogcímek a következő hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="b2b0d-166">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="b2b0d-167">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-167">hello following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. <span data-ttu-id="b2b0d-169">A hello **felhasználói attribútumok** hello szakaszt **egyszeri bejelentkezés** párbeszédablakban válassza **user.mail** , **felhasználói azonosító** és minden egyes sorára látható hello az alábbi táblázatban szereplő hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-169">In hello **User Attributes** section on hello **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in hello table below, perform hello following steps:</span></span>
    
    | <span data-ttu-id="b2b0d-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b2b0d-170">Attribute Name</span></span> | <span data-ttu-id="b2b0d-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b2b0d-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="b2b0d-172">Utónév</span><span class="sxs-lookup"><span data-stu-id="b2b0d-172">first_name</span></span> | <span data-ttu-id="b2b0d-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="b2b0d-173">user.givenname</span></span> |
    | <span data-ttu-id="b2b0d-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="b2b0d-174">last_name</span></span> | <span data-ttu-id="b2b0d-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="b2b0d-175">user.surname</span></span> |
    | <span data-ttu-id="b2b0d-176">User.Email</span><span class="sxs-lookup"><span data-stu-id="b2b0d-176">User.Email</span></span> | <span data-ttu-id="b2b0d-177">User.mail</span><span class="sxs-lookup"><span data-stu-id="b2b0d-177">user.mail</span></span> |  
    | <span data-ttu-id="b2b0d-178">User.Username</span><span class="sxs-lookup"><span data-stu-id="b2b0d-178">User.Username</span></span> | <span data-ttu-id="b2b0d-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="b2b0d-179">user.userprincipalname</span></span> |

    <span data-ttu-id="b2b0d-180">a.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-180">a.</span></span> <span data-ttu-id="b2b0d-181">Kattintson a **attribútum** tooopen **attribútum szerkesztése** párbeszédpanel mezőbe, majd hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-181">Click on **Attribute** tooopen **Edit Attribute** dialog box and perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    <span data-ttu-id="b2b0d-183">a.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-183">a.</span></span> <span data-ttu-id="b2b0d-184">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="b2b0d-185">b.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-185">b.</span></span> <span data-ttu-id="b2b0d-186">A hello **érték** listájában, az adott sorhoz feltüntetett válassza hello attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-186">From hello **Value** list, select hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b2b0d-187">c.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-187">c.</span></span> <span data-ttu-id="b2b0d-188">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="b2b0d-188">Click **OK**</span></span>

6. <span data-ttu-id="b2b0d-189">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-189">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. <span data-ttu-id="b2b0d-191">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-191">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b2b0d-193">A hello **Slackhez konfigurációs** kattintson **konfigurálása Slackhez** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-193">On hello **Slack Configuration** section, click **Configure Slack** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b2b0d-194">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b2b0d-194">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  <span data-ttu-id="b2b0d-196">Egy másik webes böngészőablakban jelentkezzen tooyour Slack vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-196">In a different web browser window, log in tooyour Slack company site as an administrator.</span></span>

10.  <span data-ttu-id="b2b0d-197">Keresse meg a túl**Microsoft Azure AD** folytassa túl**Team beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-197">Navigate too**Microsoft Azure AD** then go too**Team Settings**.</span></span>

     ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  <span data-ttu-id="b2b0d-199">A hello **Team beállításainak** területen kattintson a hello **hitelesítési** fülre, majd **beállításainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-199">In hello **Team Settings** section, click hello **Authentication** tab, and then click **Change Settings**.</span></span>

     ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. <span data-ttu-id="b2b0d-201">A hello **SAML-alapú hitelesítési beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-201">On hello **SAML Authentication Settings** dialog, perform hello following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    <span data-ttu-id="b2b0d-203">a.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-203">a.</span></span>  <span data-ttu-id="b2b0d-204">A hello **SAML 2.0 végpontot (HTTP)** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-204">In hello **SAML 2.0 Endpoint (HTTP)** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b2b0d-205">b.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-205">b.</span></span>  <span data-ttu-id="b2b0d-206">A hello **Identity Provider kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-206">In hello **Identity Provider Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b2b0d-207">c.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-207">c.</span></span>  <span data-ttu-id="b2b0d-208">Nyissa meg a letöltött fájlt a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **nyilvános tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-208">Open your downloaded certificate file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Public Certificate** textbox.</span></span>

    <span data-ttu-id="b2b0d-209">d.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-209">d.</span></span> <span data-ttu-id="b2b0d-210">A fenti három beállítások hello beállítása a Slack-csoport nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-210">Configure hello above three settings as appropriate for your Slack team.</span></span> <span data-ttu-id="b2b0d-211">Hello beállításaival kapcsolatos további információkért kérjük található hello **Slackhez tartozó SSO konfigurációs útmutató** itt.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-211">For more information about hello settings, please find hello **Slack's SSO configuration guide** here.</span></span> `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    <span data-ttu-id="b2b0d-212">e.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-212">e.</span></span>  <span data-ttu-id="b2b0d-213">Kattintson a **konfigurációjának mentéséhez**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-213">Click **Save Configuration**.</span></span>
     
    <!-- Deselect **Allow users toochange their email address**.

    e.  Select **Allow users toochoose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> <span data-ttu-id="b2b0d-214">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b2b0d-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b2b0d-215">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b2b0d-216">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b2b0d-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b2b0d-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2b0d-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="b2b0d-218">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b2b0d-220">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b2b0d-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2b0d-221">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b2b0d-223">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b2b0d-225">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b2b0d-227">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b2b0d-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b2b0d-229">a.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-229">a.</span></span> <span data-ttu-id="b2b0d-230">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b2b0d-231">b.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-231">b.</span></span> <span data-ttu-id="b2b0d-232">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b2b0d-233">c.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-233">c.</span></span> <span data-ttu-id="b2b0d-234">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b2b0d-235">d.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-235">d.</span></span> <span data-ttu-id="b2b0d-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-236">Click **Create**.</span></span>
 
### <a name="creating-a-slack-test-user"></a><span data-ttu-id="b2b0d-237">Slack tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b2b0d-237">Creating a Slack test user</span></span>

<span data-ttu-id="b2b0d-238">hello ebben a szakaszban célja toocreate a Slackhez Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-238">hello objective of this section is toocreate a user called Britta Simon in Slack.</span></span> <span data-ttu-id="b2b0d-239">Slackhez támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-239">Slack supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b2b0d-240">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-240">There is no action item for you in this section.</span></span> <span data-ttu-id="b2b0d-241">Új felhasználó jön létre egy kísérlet tooaccess Slackhez során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-241">A new user is created during an attempt tooaccess Slack if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="b2b0d-242">A felhasználó toocreate manuálisan kell, ha szüksége van-e tooContact [Slack támogatási csoport](https://slack.com/help/contact).</span><span class="sxs-lookup"><span data-stu-id="b2b0d-242">If you need toocreate a user manually, you need tooContact [Slack support team](https://slack.com/help/contact).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b2b0d-243">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b2b0d-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b2b0d-244">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSlack megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSlack.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b2b0d-246">**tooassign Britta Simon tooSlack, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b2b0d-246">**tooassign Britta Simon tooSlack, perform hello following steps:**</span></span>

1. <span data-ttu-id="b2b0d-247">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b2b0d-249">Hello alkalmazások listában válassza ki a **Slackhez**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-249">In hello applications list, select **Slack**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. <span data-ttu-id="b2b0d-251">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b2b0d-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-253">Click **Add** button.</span></span> <span data-ttu-id="b2b0d-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b2b0d-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b2b0d-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b2b0d-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b2b0d-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b2b0d-259">Testing single sign-on</span></span>

<span data-ttu-id="b2b0d-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b2b0d-261">Hello Slack csempére kattintva a hozzáférési Panel hello, automatikusan bejelentkezett tooyour Slack alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="b2b0d-261">When you click hello Slack tile in hello Access Panel, you should get automatically signed-on tooyour Slack application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2b0d-262">További források</span><span class="sxs-lookup"><span data-stu-id="b2b0d-262">Additional resources</span></span>

* [<span data-ttu-id="b2b0d-263">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b2b0d-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b2b0d-264">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b2b0d-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

