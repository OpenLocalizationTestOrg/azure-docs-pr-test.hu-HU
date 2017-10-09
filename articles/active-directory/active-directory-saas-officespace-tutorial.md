---
title: "Oktatóanyag: Azure Active Directory-integráció OfficeSpace szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és OfficeSpace szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b53afb648b8a6057c32c782d857e34c06e152c67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a><span data-ttu-id="2bba1-103">Oktatóanyag: Azure Active Directory-integráció OfficeSpace szoftverrel</span><span class="sxs-lookup"><span data-stu-id="2bba1-103">Tutorial: Azure Active Directory integration with OfficeSpace Software</span></span>

<span data-ttu-id="2bba1-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate OfficeSpace szoftver az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2bba1-104">In this tutorial, you learn how toointegrate OfficeSpace Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2bba1-105">OfficeSpace szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2bba1-105">Integrating OfficeSpace Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2bba1-106">Az Azure AD hozzáférési tooOfficeSpace szoftver rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="2bba1-106">You can control in Azure AD who has access tooOfficeSpace Software.</span></span>
- <span data-ttu-id="2bba1-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOfficeSpace szoftver (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="2bba1-107">You can enable your users tooautomatically get signed-on tooOfficeSpace Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="2bba1-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="2bba1-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="2bba1-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2bba1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bba1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2bba1-110">Prerequisites</span></span>

<span data-ttu-id="2bba1-111">tooconfigure OfficeSpace szoftver az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="2bba1-111">tooconfigure Azure AD integration with OfficeSpace Software, you need hello following items:</span></span>

- <span data-ttu-id="2bba1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2bba1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2bba1-113">Egy OfficeSpace szoftver egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2bba1-113">A OfficeSpace Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2bba1-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2bba1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2bba1-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2bba1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2bba1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2bba1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2bba1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2bba1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2bba1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2bba1-118">Scenario description</span></span>
<span data-ttu-id="2bba1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2bba1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2bba1-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2bba1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2bba1-121">Hello gyűjteményből OfficeSpace szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2bba1-121">Adding OfficeSpace Software from hello gallery</span></span>
2. <span data-ttu-id="2bba1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2bba1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-officespace-software-from-hello-gallery"></a><span data-ttu-id="2bba1-123">Hello gyűjteményből OfficeSpace szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2bba1-123">Adding OfficeSpace Software from hello gallery</span></span>
<span data-ttu-id="2bba1-124">tooconfigure hello integrációs OfficeSpace szoftver az Azure AD-be, meg kell tooadd OfficeSpace szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2bba1-124">tooconfigure hello integration of OfficeSpace Software into Azure AD, you need tooadd OfficeSpace Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2bba1-125">**tooadd OfficeSpace szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2bba1-125">**tooadd OfficeSpace Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bba1-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2bba1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="2bba1-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2bba1-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="2bba1-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2bba1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="2bba1-133">Hello keresési mezőbe, írja be a **OfficeSpace szoftver**, jelölje be **OfficeSpace szoftver** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2bba1-133">In hello search box, type **OfficeSpace Software**, select **OfficeSpace Software** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello OfficeSpace szoftver eredmények listájában](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2bba1-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2bba1-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="2bba1-136">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés OfficeSpace szoftverrel "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="2bba1-136">In this section, you configure and test Azure AD single sign-on with OfficeSpace Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2bba1-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói OfficeSpace szoftverfrissítési tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2bba1-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in OfficeSpace Software is tooa user in Azure AD.</span></span> <span data-ttu-id="2bba1-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello OfficeSpace szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2bba1-138">In other words, a link relationship between an Azure AD user and hello related user in OfficeSpace Software needs toobe established.</span></span>

<span data-ttu-id="2bba1-139">OfficeSpace szoftver, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2bba1-139">In OfficeSpace Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2bba1-140">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése OfficeSpace szoftverrel, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2bba1-140">tooconfigure and test Azure AD single sign-on with OfficeSpace Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2bba1-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2bba1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2bba1-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2bba1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2bba1-143">**[OfficeSpace szoftver tesztfelhasználó létrehozása](#create-a-officespace-software-test-user)**  -toohave egy megfelelője a Britta Simon OfficeSpace szoftver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2bba1-143">**[Create a OfficeSpace Software test user](#create-a-officespace-software-test-user)** - toohave a counterpart of Britta Simon in OfficeSpace Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2bba1-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2bba1-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2bba1-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2bba1-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="2bba1-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2bba1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="2bba1-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a OfficeSpace alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2bba1-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your OfficeSpace Software application.</span></span>

<span data-ttu-id="2bba1-148">**az Azure AD tooconfigure egyszeri bejelentkezés OfficeSpace szoftvert, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2bba1-148">**tooconfigure Azure AD single sign-on with OfficeSpace Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bba1-149">Az Azure portál, a hello hello **OfficeSpace szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-149">In hello Azure portal, on hello **OfficeSpace Software** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="2bba1-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2bba1-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. <span data-ttu-id="2bba1-153">A hello **OfficeSpace szoftver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2bba1-153">On hello **OfficeSpace Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk OfficeSpace szoftver tartomány és az URL-címek](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    <span data-ttu-id="2bba1-155">a.</span><span class="sxs-lookup"><span data-stu-id="2bba1-155">a.</span></span> <span data-ttu-id="2bba1-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.officespacesoftware.com/users/sign_in/saml`</span><span class="sxs-lookup"><span data-stu-id="2bba1-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.officespacesoftware.com/users/sign_in/saml`</span></span>

    <span data-ttu-id="2bba1-157">b.</span><span class="sxs-lookup"><span data-stu-id="2bba1-157">b.</span></span> <span data-ttu-id="2bba1-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`<company name>.officespacesoftware.com`</span><span class="sxs-lookup"><span data-stu-id="2bba1-158">In hello **Identifier** textbox, type a URL using hello following pattern: `<company name>.officespacesoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2bba1-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2bba1-159">These values are not real.</span></span> <span data-ttu-id="2bba1-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="2bba1-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2bba1-161">Ügyfél [OfficeSpace szoftver ügyfél-támogatási csoport](mailto:support@officespacesoftware.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2bba1-161">Contact [OfficeSpace Software Client support team](mailto:support@officespacesoftware.com) tooget these values.</span></span> 

4. <span data-ttu-id="2bba1-162">OfficeSpace alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="2bba1-162">OfficeSpace Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2bba1-163">Állítsa be az alkalmazás jogcímek a következő hello.</span><span class="sxs-lookup"><span data-stu-id="2bba1-163">Please configure hello following claims for this application.</span></span> <span data-ttu-id="2bba1-164">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="2bba1-164">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="2bba1-165">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="2bba1-165">hello following screenshot shows an example for this.</span></span>
    
    ![Attribútum konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. <span data-ttu-id="2bba1-167">A hello **felhasználói attribútumok** hello szakaszt **egyszeri bejelentkezés** párbeszédablakban válassza **user.mail** , **felhasználói azonosító** és minden egyes sorára látható hello az alábbi táblázatban szereplő hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="2bba1-167">In hello **User Attributes** section on hello **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in hello table below, perform hello following steps:</span></span>
    
    | <span data-ttu-id="2bba1-168">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="2bba1-168">Attribute Name</span></span> | <span data-ttu-id="2bba1-169">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="2bba1-169">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="2bba1-170">E-mailek</span><span class="sxs-lookup"><span data-stu-id="2bba1-170">email</span></span> | <span data-ttu-id="2bba1-171">User.mail</span><span class="sxs-lookup"><span data-stu-id="2bba1-171">user.mail</span></span> |
    | <span data-ttu-id="2bba1-172">név</span><span class="sxs-lookup"><span data-stu-id="2bba1-172">name</span></span> | <span data-ttu-id="2bba1-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="2bba1-173">user.displayname</span></span> |
    | <span data-ttu-id="2bba1-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="2bba1-174">first_name</span></span> | <span data-ttu-id="2bba1-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2bba1-175">user.givenname</span></span> |
    | <span data-ttu-id="2bba1-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="2bba1-176">last_name</span></span> | <span data-ttu-id="2bba1-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="2bba1-177">user.surname</span></span> |

    <span data-ttu-id="2bba1-178">a.</span><span class="sxs-lookup"><span data-stu-id="2bba1-178">a.</span></span> <span data-ttu-id="2bba1-179">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2bba1-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![<span data-ttu-id="2bba1-180">Konfigurálása hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2bba1-180">Configure Add</span></span> ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Attribútum konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="2bba1-182">b.</span><span class="sxs-lookup"><span data-stu-id="2bba1-182">b.</span></span> <span data-ttu-id="2bba1-183">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="2bba1-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="2bba1-184">c.</span><span class="sxs-lookup"><span data-stu-id="2bba1-184">c.</span></span> <span data-ttu-id="2bba1-185">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="2bba1-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2bba1-186">d.</span><span class="sxs-lookup"><span data-stu-id="2bba1-186">d.</span></span> <span data-ttu-id="2bba1-187">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="2bba1-187">Click **Ok**</span></span>
 
6. <span data-ttu-id="2bba1-188">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** hello tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="2bba1-188">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of hello certificate.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. <span data-ttu-id="2bba1-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2bba1-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2bba1-192">A hello **OfficeSpace szoftverkonfigurációt** kattintson **OfficeSpace szoftver konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2bba1-192">On hello **OfficeSpace Software Configuration** section, click **Configure OfficeSpace Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2bba1-193">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2bba1-193">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![OfficeSpace szoftverkonfigurációjának összeállítása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. <span data-ttu-id="2bba1-195">Egy másik webes böngészőablakban bejelentkezni a OfficeSpace szoftver Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="2bba1-195">In a different web browser window, log into your OfficeSpace Software tenant as an administrator.</span></span>

10. <span data-ttu-id="2bba1-196">Nyissa meg túl**beállítások** kattintson **összekötők**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-196">Go too**Settings** and click **Connectors**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. <span data-ttu-id="2bba1-198">Kattintson a **SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-198">Click **SAML Authentication**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. <span data-ttu-id="2bba1-200">A hello **SAML-alapú hitelesítés** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2bba1-200">In hello **SAML Authentication** section, perform hello following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    <span data-ttu-id="2bba1-202">a.</span><span class="sxs-lookup"><span data-stu-id="2bba1-202">a.</span></span> <span data-ttu-id="2bba1-203">A hello **kijelentkezési szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="2bba1-203">In hello **Logout provider url** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2bba1-204">b.</span><span class="sxs-lookup"><span data-stu-id="2bba1-204">b.</span></span> <span data-ttu-id="2bba1-205">A hello **ügyfél idp célként megadott url** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="2bba1-205">In hello **Client idp target url** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2bba1-206">c.</span><span class="sxs-lookup"><span data-stu-id="2bba1-206">c.</span></span> <span data-ttu-id="2bba1-207">Beillesztés hello **ujjlenyomat** érték, amely az Azure portálról történő hello másolta **ügyfél IDP tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2bba1-207">Paste hello **Thumbprint** value which you have copied from Azure portal, into hello **Client IDP certificate fingerprint** textbox.</span></span> 

    <span data-ttu-id="2bba1-208">d.</span><span class="sxs-lookup"><span data-stu-id="2bba1-208">d.</span></span> <span data-ttu-id="2bba1-209">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-209">Click **Save Settings**.</span></span>


> [!TIP]
> <span data-ttu-id="2bba1-210">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2bba1-210">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2bba1-211">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2bba1-211">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2bba1-212">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2bba1-212">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2bba1-213">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="2bba1-213">Create an Azure AD test user</span></span>

<span data-ttu-id="2bba1-214">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2bba1-214">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="2bba1-216">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2bba1-216">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bba1-217">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="2bba1-217">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="2bba1-219">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-219">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="2bba1-221">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2bba1-221">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="2bba1-223">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2bba1-223">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="2bba1-225">a.</span><span class="sxs-lookup"><span data-stu-id="2bba1-225">a.</span></span> <span data-ttu-id="2bba1-226">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-226">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2bba1-227">b.</span><span class="sxs-lookup"><span data-stu-id="2bba1-227">b.</span></span> <span data-ttu-id="2bba1-228">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="2bba1-228">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="2bba1-229">c.</span><span class="sxs-lookup"><span data-stu-id="2bba1-229">c.</span></span> <span data-ttu-id="2bba1-230">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2bba1-230">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="2bba1-231">d.</span><span class="sxs-lookup"><span data-stu-id="2bba1-231">d.</span></span> <span data-ttu-id="2bba1-232">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2bba1-232">Click **Create**.</span></span>
 
### <a name="create-a-officespace-software-test-user"></a><span data-ttu-id="2bba1-233">OfficeSpace szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bba1-233">Create a OfficeSpace Software test user</span></span>

<span data-ttu-id="2bba1-234">hello ebben a szakaszban célja toocreate OfficeSpace szoftver Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="2bba1-234">hello objective of this section is toocreate a user called Britta Simon in OfficeSpace Software.</span></span> <span data-ttu-id="2bba1-235">OfficeSpace szoftver, amellyel just-in-time átadása, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="2bba1-235">OfficeSpace Software supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="2bba1-236">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="2bba1-236">There is no action item for you in this section.</span></span> <span data-ttu-id="2bba1-237">Új felhasználó létrejön egy kísérlet tooaccess OfficeSpace szoftver során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="2bba1-237">A new user will be created during an attempt tooaccess OfficeSpace Software if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="2bba1-238">Ha egy felhasználó toocreate manuálisan kell, tooContact kell [OfficeSpace szoftver támogatási csoport](mailto:support@officespacesoftware.com).</span><span class="sxs-lookup"><span data-stu-id="2bba1-238">If you need toocreate an user manually, you need tooContact [OfficeSpace Software support team](mailto:support@officespacesoftware.com).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="2bba1-239">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="2bba1-239">Assign hello Azure AD test user</span></span>

<span data-ttu-id="2bba1-240">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOfficeSpace szoftver megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2bba1-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOfficeSpace Software.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="2bba1-242">**tooassign Britta Simon tooOfficeSpace szoftver, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2bba1-242">**tooassign Britta Simon tooOfficeSpace Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bba1-243">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2bba1-245">Hello alkalmazások listában válassza ki a **OfficeSpace szoftver**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-245">In hello applications list, select **OfficeSpace Software**.</span></span>

    ![hello OfficeSpace szoftverhivatkozás hello alkalmazások listájában](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. <span data-ttu-id="2bba1-247">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2bba1-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="2bba1-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2bba1-249">Click **Add** button.</span></span> <span data-ttu-id="2bba1-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2bba1-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="2bba1-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2bba1-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2bba1-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2bba1-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2bba1-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2bba1-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="2bba1-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2bba1-255">Test single sign-on</span></span>

<span data-ttu-id="2bba1-256">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2bba1-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2bba1-257">Hello OfficeSpace szoftver hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour OfficeSpace alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2bba1-257">When you click hello OfficeSpace Software tile in hello Access Panel, you should get automatically signed-on tooyour OfficeSpace Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bba1-258">További források</span><span class="sxs-lookup"><span data-stu-id="2bba1-258">Additional resources</span></span>

* [<span data-ttu-id="2bba1-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2bba1-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2bba1-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2bba1-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

