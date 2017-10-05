---
title: "Oktatóanyag: Azure Active Directoryval integrált együttműködési innováció |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az együttműködési innováció között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bba95df3-75a4-4a93-8805-b3a8aa3d4861
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 5706ba9f4e7c92de77a0edc5146aa150de379c9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-collaborative-innovation"></a><span data-ttu-id="d019a-103">Oktatóanyag: Azure Active Directoryval integrált együttműködési innováció</span><span class="sxs-lookup"><span data-stu-id="d019a-103">Tutorial: Azure Active Directory integration with Collaborative Innovation</span></span>

<span data-ttu-id="d019a-104">Ebben az oktatóanyagban elsajátíthatja együttműködési innováció integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d019a-104">In this tutorial, you learn how to integrate Collaborative Innovation with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d019a-105">Együttműködési innováció integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d019a-105">Integrating Collaborative Innovation with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d019a-106">Megadhatja a együttműködési innováció hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d019a-106">You can control in Azure AD who has access to Collaborative Innovation</span></span>
- <span data-ttu-id="d019a-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett való együttműködést innováció (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="d019a-107">You can enable your users to automatically get signed-on to Collaborative Innovation (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d019a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d019a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d019a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d019a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d019a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d019a-110">Prerequisites</span></span>

<span data-ttu-id="d019a-111">Együttműködési innováció konfigurálása az Azure AD-integrációs, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d019a-111">To configure Azure AD integration with Collaborative Innovation, you need the following items:</span></span>

- <span data-ttu-id="d019a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d019a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d019a-113">A együttműködési innováció egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d019a-113">A Collaborative Innovation single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d019a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d019a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d019a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d019a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d019a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d019a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d019a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d019a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d019a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d019a-118">Scenario description</span></span>
<span data-ttu-id="d019a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d019a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d019a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d019a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d019a-121">Együttműködési innováció hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d019a-121">Adding Collaborative Innovation from the gallery</span></span>
2. <span data-ttu-id="d019a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d019a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-collaborative-innovation-from-the-gallery"></a><span data-ttu-id="d019a-123">Együttműködési innováció hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d019a-123">Adding Collaborative Innovation from the gallery</span></span>
<span data-ttu-id="d019a-124">Az Azure AD integrálása a együttműködési innováció konfigurálásához kell hozzáadnia együttműködési innováció a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d019a-124">To configure the integration of Collaborative Innovation into Azure AD, you need to add Collaborative Innovation from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d019a-125">**A gyűjteményből együttműködési innováció hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d019a-125">**To add Collaborative Innovation from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d019a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d019a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d019a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d019a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d019a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d019a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d019a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d019a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d019a-133">Írja be a keresőmezőbe, **együttműködési innováció**.</span><span class="sxs-lookup"><span data-stu-id="d019a-133">In the search box, type **Collaborative Innovation**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_search.png)

5. <span data-ttu-id="d019a-135">Az eredmények panelen válassza ki a **együttműködési innováció**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d019a-135">In the results panel, select **Collaborative Innovation**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d019a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d019a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d019a-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az együttműködési innováció "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="d019a-138">In this section, you configure and test Azure AD single sign-on with Collaborative Innovation based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d019a-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó együttműködési innováció a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d019a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Collaborative Innovation is to a user in Azure AD.</span></span> <span data-ttu-id="d019a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a együttműködési innováció közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d019a-140">In other words, a link relationship between an Azure AD user and the related user in Collaborative Innovation needs to be established.</span></span>

<span data-ttu-id="d019a-141">Együttműködési innováció, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d019a-141">In Collaborative Innovation, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d019a-142">Az Azure AD egyszeri bejelentkezést az együttműködési innováció tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d019a-142">To configure and test Azure AD single sign-on with Collaborative Innovation, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d019a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d019a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d019a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d019a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d019a-145">**[Együttműködési innováció tesztfelhasználó létrehozása](#creating-a-collaborative-innovation-test-user)**  - való egy megfelelője a Britta Simon együttműködési innováció, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d019a-145">**[Creating a Collaborative Innovation test user](#creating-a-collaborative-innovation-test-user)** - to have a counterpart of Britta Simon in Collaborative Innovation that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d019a-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d019a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d019a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d019a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d019a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d019a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d019a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az együttműködési innováció alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d019a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Collaborative Innovation application.</span></span>

<span data-ttu-id="d019a-150">**Együttműködési innováció konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d019a-150">**To configure Azure AD single sign-on with Collaborative Innovation, perform the following steps:**</span></span>

1. <span data-ttu-id="d019a-151">Az Azure portálon a a **együttműködési innováció** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d019a-151">In the Azure portal, on the **Collaborative Innovation** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d019a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d019a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_samlbase.png)

3. <span data-ttu-id="d019a-155">Az a **együttműködési innováció tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d019a-155">On the **Collaborative Innovation Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_url.png)

    <span data-ttu-id="d019a-157">a.</span><span class="sxs-lookup"><span data-stu-id="d019a-157">a.</span></span> <span data-ttu-id="d019a-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instancename>.foundry.<companyname>.com/`</span><span class="sxs-lookup"><span data-stu-id="d019a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.foundry.<companyname>.com/`</span></span>

    <span data-ttu-id="d019a-159">b.</span><span class="sxs-lookup"><span data-stu-id="d019a-159">b.</span></span> <span data-ttu-id="d019a-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instancename>.foundry.<companyname>.com`</span><span class="sxs-lookup"><span data-stu-id="d019a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.foundry.<companyname>.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="d019a-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d019a-161">These values are not real.</span></span> <span data-ttu-id="d019a-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d019a-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d019a-163">Ügyfél [együttműködési innováció ügyfél-támogatási csoport](https://www.unilever.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d019a-163">Contact [Collaborative Innovation Client support team](https://www.unilever.com/contact/) to get these values.</span></span>  

4. <span data-ttu-id="d019a-164">Együttműködési innováció alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="d019a-164">Collaborative Innovation application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="d019a-165">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d019a-165">Please configure the following claims for this application.</span></span> <span data-ttu-id="d019a-166">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="d019a-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="d019a-167">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="d019a-167">The following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/attribute.png)
    
5. <span data-ttu-id="d019a-169">Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzet a **felhasználói attribútumok** szakaszban bontsa ki az attribútumok.</span><span class="sxs-lookup"><span data-stu-id="d019a-169">Click **View and edit all other user attributes** checkbox in the **User Attributes** section to expand the attributes.</span></span> <span data-ttu-id="d019a-170">Hajtsa végre a következő lépéseket mindegyik a megjelenített attribútumok-</span><span class="sxs-lookup"><span data-stu-id="d019a-170">Perform the following steps on each of the displayed attributes-</span></span>

    | <span data-ttu-id="d019a-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="d019a-171">Attribute Name</span></span> | <span data-ttu-id="d019a-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="d019a-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="d019a-173">givenName</span><span class="sxs-lookup"><span data-stu-id="d019a-173">givenname</span></span> | <span data-ttu-id="d019a-174">User.givenName</span><span class="sxs-lookup"><span data-stu-id="d019a-174">user.givenname</span></span> |
    | <span data-ttu-id="d019a-175">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="d019a-175">surname</span></span> | <span data-ttu-id="d019a-176">User.surname</span><span class="sxs-lookup"><span data-stu-id="d019a-176">user.surname</span></span> |
    | <span data-ttu-id="d019a-177">e-mail cím</span><span class="sxs-lookup"><span data-stu-id="d019a-177">emailaddress</span></span> | <span data-ttu-id="d019a-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="d019a-178">user.userprincipalname</span></span> |
    | <span data-ttu-id="d019a-179">név</span><span class="sxs-lookup"><span data-stu-id="d019a-179">name</span></span> | <span data-ttu-id="d019a-180">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="d019a-180">user.userprincipalname</span></span> |

    <span data-ttu-id="d019a-181">a.</span><span class="sxs-lookup"><span data-stu-id="d019a-181">a.</span></span> <span data-ttu-id="d019a-182">Az attribútum megnyitásához kattintson a **attribútum szerkesztése** ablak.</span><span class="sxs-lookup"><span data-stu-id="d019a-182">Click the attribute to open the **Edit Attribute** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/url_update.png)

    <span data-ttu-id="d019a-184">b.</span><span class="sxs-lookup"><span data-stu-id="d019a-184">b.</span></span> <span data-ttu-id="d019a-185">Az URL-cím érték törlése a **Namespace**.</span><span class="sxs-lookup"><span data-stu-id="d019a-185">Delete the URL value from the **Namespace**.</span></span>
    
    <span data-ttu-id="d019a-186">c.</span><span class="sxs-lookup"><span data-stu-id="d019a-186">c.</span></span> <span data-ttu-id="d019a-187">Kattintson a **Ok** a beállítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d019a-187">Click **Ok** to save the setting.</span></span>

6. <span data-ttu-id="d019a-188">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d019a-188">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_certificate.png) 

7. <span data-ttu-id="d019a-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d019a-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="d019a-192">Egyszeri bejelentkezés konfigurálása **együttműködési innováció** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [együttműködési innováció támogatási csoport](https://www.unilever.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="d019a-192">To configure single sign-on on **Collaborative Innovation** side, you need to send the downloaded **Metadata XML** to [Collaborative Innovation support team](https://www.unilever.com/contact/).</span></span> <span data-ttu-id="d019a-193">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d019a-193">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d019a-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d019a-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d019a-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d019a-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d019a-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d019a-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d019a-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d019a-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="d019a-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d019a-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d019a-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d019a-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d019a-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d019a-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d019a-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d019a-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d019a-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d019a-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d019a-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d019a-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-collaborativeinnovation-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d019a-209">a.</span><span class="sxs-lookup"><span data-stu-id="d019a-209">a.</span></span> <span data-ttu-id="d019a-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d019a-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d019a-211">b.</span><span class="sxs-lookup"><span data-stu-id="d019a-211">b.</span></span> <span data-ttu-id="d019a-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d019a-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d019a-213">c.</span><span class="sxs-lookup"><span data-stu-id="d019a-213">c.</span></span> <span data-ttu-id="d019a-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d019a-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d019a-215">d.</span><span class="sxs-lookup"><span data-stu-id="d019a-215">d.</span></span> <span data-ttu-id="d019a-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d019a-216">Click **Create**.</span></span>
 
### <a name="creating-a-collaborative-innovation-test-user"></a><span data-ttu-id="d019a-217">Együttműködési innováció tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d019a-217">Creating a Collaborative Innovation test user</span></span>

<span data-ttu-id="d019a-218">Ahhoz, hogy az Azure AD-felhasználók együttműködési innováció bejelentkezni, akkor ki kell építenie az együttműködést innováció.</span><span class="sxs-lookup"><span data-stu-id="d019a-218">To enable Azure AD users to log in to Collaborative Innovation, they must be provisioned into Collaborative Innovation.</span></span>  

<span data-ttu-id="d019a-219">Ez az alkalmazás esetén automatikus, ha az alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="d019a-219">In case of this application provisioning is automatic as the application supports just in time user provisioning.</span></span> <span data-ttu-id="d019a-220">Így nincs szükség az itt bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d019a-220">So there is no need to perform any steps here.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d019a-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d019a-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d019a-222">Ebben a szakaszban Britta Simon használandó Azure egyszeri bejelentkezés által biztosított együttműködési innováció hozzáférés engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d019a-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Collaborative Innovation.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d019a-224">**Együttműködési innováció Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d019a-224">**To assign Britta Simon to Collaborative Innovation, perform the following steps:**</span></span>

1. <span data-ttu-id="d019a-225">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d019a-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d019a-227">Az alkalmazások listában válassza ki a **együttműködési innováció**.</span><span class="sxs-lookup"><span data-stu-id="d019a-227">In the applications list, select **Collaborative Innovation**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_collaborativeinnovation_app.png) 

3. <span data-ttu-id="d019a-229">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d019a-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d019a-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d019a-231">Click **Add** button.</span></span> <span data-ttu-id="d019a-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d019a-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d019a-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d019a-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d019a-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d019a-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d019a-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d019a-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d019a-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d019a-237">Testing single sign-on</span></span>

<span data-ttu-id="d019a-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d019a-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d019a-239">Ha a hozzáférési Panel együttműködési innováció mozaik gombra kattint, szerezheti be együttműködési innováció alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="d019a-239">When you click the Collaborative Innovation tile in the Access Panel, you should get login page of Collaborative Innovation application.</span></span>
<span data-ttu-id="d019a-240">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d019a-240">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d019a-241">További források</span><span class="sxs-lookup"><span data-stu-id="d019a-241">Additional resources</span></span>

* [<span data-ttu-id="d019a-242">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d019a-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d019a-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d019a-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-collaborativeinnovation-tutorial/tutorial_general_203.png

