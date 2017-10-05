---
title: "Oktatóanyag: Azure Active Directoryval integrált Samanage |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Samanage között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c54dbe407145a29a712acc3c0fb549a38ac26bed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a><span data-ttu-id="b7ace-103">Oktatóanyag: Azure Active Directoryval integrált Samanage</span><span class="sxs-lookup"><span data-stu-id="b7ace-103">Tutorial: Azure Active Directory integration with Samanage</span></span>

<span data-ttu-id="b7ace-104">Ebben az oktatóanyagban elsajátíthatja Samanage integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b7ace-104">In this tutorial, you learn how to integrate Samanage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7ace-105">Samanage integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b7ace-105">Integrating Samanage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b7ace-106">Megadhatja a Samanage hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b7ace-106">You can control in Azure AD who has access to Samanage</span></span>
- <span data-ttu-id="b7ace-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Samanage (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b7ace-107">You can enable your users to automatically get signed-on to Samanage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7ace-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b7ace-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b7ace-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7ace-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7ace-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b7ace-110">Prerequisites</span></span>

<span data-ttu-id="b7ace-111">Konfigurálása az Azure AD-integrációs Samanage, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b7ace-111">To configure Azure AD integration with Samanage, you need the following items:</span></span>

- <span data-ttu-id="b7ace-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b7ace-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7ace-113">Egy Samanage egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b7ace-113">A Samanage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7ace-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b7ace-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7ace-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b7ace-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7ace-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b7ace-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7ace-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7ace-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7ace-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b7ace-118">Scenario description</span></span>
<span data-ttu-id="b7ace-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b7ace-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7ace-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b7ace-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7ace-121">A gyűjteményből Samanage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7ace-121">Adding Samanage from the gallery</span></span>
2. <span data-ttu-id="b7ace-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b7ace-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-samanage-from-the-gallery"></a><span data-ttu-id="b7ace-123">A gyűjteményből Samanage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7ace-123">Adding Samanage from the gallery</span></span>
<span data-ttu-id="b7ace-124">Az Azure AD integrálása a Samanage konfigurálásához kell hozzáadnia Samanage a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b7ace-124">To configure the integration of Samanage into Azure AD, you need to add Samanage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b7ace-125">**A gyűjteményből Samanage hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7ace-125">**To add Samanage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b7ace-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b7ace-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7ace-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b7ace-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b7ace-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="b7ace-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b7ace-133">Írja be a keresőmezőbe, **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-133">In the search box, type **Samanage**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. <span data-ttu-id="b7ace-135">Az eredmények panelen válassza ki a **Samanage**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7ace-135">In the results panel, select **Samanage**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7ace-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b7ace-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7ace-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Samanage.</span><span class="sxs-lookup"><span data-stu-id="b7ace-138">In this section, you configure and test Azure AD single sign-on with Samanage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7ace-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Samanage a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b7ace-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Samanage is to a user in Azure AD.</span></span> <span data-ttu-id="b7ace-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Samanage közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b7ace-140">In other words, a link relationship between an Azure AD user and the related user in Samanage needs to be established.</span></span>

<span data-ttu-id="b7ace-141">Samanage, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="b7ace-141">In Samanage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b7ace-142">Az Azure AD egyszeri bejelentkezést a Samanage tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b7ace-142">To configure and test Azure AD single sign-on with Samanage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b7ace-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b7ace-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b7ace-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b7ace-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7ace-145">**[Samanage tesztfelhasználó létrehozása](#creating-a-samanage-test-user)**  - való Britta Simon valami Samanage, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b7ace-145">**[Creating a Samanage test user](#creating-a-samanage-test-user)** - to have a counterpart of Britta Simon in Samanage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7ace-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b7ace-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7ace-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b7ace-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7ace-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7ace-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7ace-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Samanage alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b7ace-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Samanage application.</span></span>

<span data-ttu-id="b7ace-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Samanage, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7ace-150">**To configure Azure AD single sign-on with Samanage, perform the following steps:**</span></span>

1. <span data-ttu-id="b7ace-151">Az Azure portálon a a **Samanage** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-151">In the Azure portal, on the **Samanage** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b7ace-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b7ace-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. <span data-ttu-id="b7ace-155">Az a **Samanage tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b7ace-155">On the **Samanage Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    <span data-ttu-id="b7ace-157">a.</span><span class="sxs-lookup"><span data-stu-id="b7ace-157">a.</span></span> <span data-ttu-id="b7ace-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<Company Name>.samanage.com/saml_login/<Company Name>`</span><span class="sxs-lookup"><span data-stu-id="b7ace-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<Company Name>.samanage.com/saml_login/<Company Name>`</span></span>

    <span data-ttu-id="b7ace-159">b.</span><span class="sxs-lookup"><span data-stu-id="b7ace-159">b.</span></span> <span data-ttu-id="b7ace-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<Company Name>.samanage.com`</span><span class="sxs-lookup"><span data-stu-id="b7ace-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<Company Name>.samanage.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b7ace-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b7ace-161">These values are not real.</span></span> <span data-ttu-id="b7ace-162">Frissítheti ezeket az értékeket és a tényleges bejelentkezési URL-cím és -azonosítót, amelynek az ismertetése, az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="b7ace-162">Update these values with the actual Sign-on URL and Identifier, which is explained later in the tutorial.</span></span> <span data-ttu-id="b7ace-163">További részletekért forduljon [Samanage ügyfél-támogatási csoport](https://www.samanage.com/support).</span><span class="sxs-lookup"><span data-stu-id="b7ace-163">For more details contact [Samanage Client support team](https://www.samanage.com/support).</span></span>    
 
4. <span data-ttu-id="b7ace-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b7ace-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. <span data-ttu-id="b7ace-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7ace-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b7ace-168">A a **Samanage konfigurációs** kattintson **konfigurálása Samanage** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b7ace-168">On the **Samanage Configuration** section, click **Configure Samanage** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b7ace-169">Másolás a **Sign-Out URL-címet, és a SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b7ace-169">Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. <span data-ttu-id="b7ace-171">Egy másik webes böngészőablakban jelentkezzen be a Samanage vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b7ace-171">In a different web browser window, log into your Samanage company site as an administrator.</span></span>

8. <span data-ttu-id="b7ace-172">Kattintson a **irányítópult** válassza **telepítő** bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="b7ace-172">Click **Dashboard** and select **Setup** in left navigation pane.</span></span>
   
    <span data-ttu-id="b7ace-173">![Irányítópult](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "irányítópult")</span><span class="sxs-lookup"><span data-stu-id="b7ace-173">![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")</span></span>

9. <span data-ttu-id="b7ace-174">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-174">Click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="b7ace-175">![Egyszeri bejelentkezés](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="b7ace-175">![Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")</span></span>

10. <span data-ttu-id="b7ace-176">Navigáljon a **SAML használatával bejelentkezési** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b7ace-176">Navigate to **Login using SAML** section, perform the following steps:</span></span>
   
    <span data-ttu-id="b7ace-177">![Bejelentkezés SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "bejelentkezési SAML használatával")</span><span class="sxs-lookup"><span data-stu-id="b7ace-177">![Login using SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "Login using SAML")</span></span>
 
    <span data-ttu-id="b7ace-178">a.</span><span class="sxs-lookup"><span data-stu-id="b7ace-178">a.</span></span> <span data-ttu-id="b7ace-179">Kattintson a **engedélyezése egyszeri bejelentkezéshez a SAML**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-179">Click **Enable Single Sign-On with SAML**.</span></span>  
 
    <span data-ttu-id="b7ace-180">b.</span><span class="sxs-lookup"><span data-stu-id="b7ace-180">b.</span></span> <span data-ttu-id="b7ace-181">Az a **identitási szolgáltató URL-cím** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b7ace-181">In the **Identity Provider URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>    
 
    <span data-ttu-id="b7ace-182">c.</span><span class="sxs-lookup"><span data-stu-id="b7ace-182">c.</span></span> <span data-ttu-id="b7ace-183">Erősítse meg a **bejelentkezési URL-cím** megegyezik a **URL-cím bejelentkezési** a **Samanage tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b7ace-183">Confirm the **Login URL** matches the **Sign On URL** of **Samanage Domain and URLs** section in Azure portal.</span></span>
 
    <span data-ttu-id="b7ace-184">d.</span><span class="sxs-lookup"><span data-stu-id="b7ace-184">d.</span></span> <span data-ttu-id="b7ace-185">Az a **kijelentkezési URL-cím** szövegmezőhöz értékének megadása **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b7ace-185">In the **Logout URL** textbox, enter the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="b7ace-186">e.</span><span class="sxs-lookup"><span data-stu-id="b7ace-186">e.</span></span> <span data-ttu-id="b7ace-187">Az a **SAML kibocsátó** szövegmező, írja be az alkalmazásazonosító URI a az identitásszolgáltató beállítása.</span><span class="sxs-lookup"><span data-stu-id="b7ace-187">In the **SAML Issuer** textbox, type the app id URI set in your identity provider.</span></span>
 
    <span data-ttu-id="b7ace-188">f.</span><span class="sxs-lookup"><span data-stu-id="b7ace-188">f.</span></span> <span data-ttu-id="b7ace-189">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben az Azure portálról letöltött, annak tartalmának másolása a vágólapra és illessze be azt a **illessze be az identitásszolgáltató x.509 tanúsítvány az alábbi** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b7ace-189">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **Paste your Identity Provider x.509 Certificate below** textbox.</span></span>
 
    <span data-ttu-id="b7ace-190">g.</span><span class="sxs-lookup"><span data-stu-id="b7ace-190">g.</span></span> <span data-ttu-id="b7ace-191">Kattintson a **felhasználók létrehozása, ha azok nem léteznek Samanage**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-191">Click **Create users if they do not exist in Samanage**.</span></span>
 
    <span data-ttu-id="b7ace-192">h.</span><span class="sxs-lookup"><span data-stu-id="b7ace-192">h.</span></span> <span data-ttu-id="b7ace-193">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-193">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="b7ace-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="b7ace-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b7ace-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="b7ace-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b7ace-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7ace-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7ace-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7ace-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7ace-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b7ace-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b7ace-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7ace-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b7ace-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b7ace-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7ace-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7ace-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b7ace-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7ace-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b7ace-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7ace-209">a.</span><span class="sxs-lookup"><span data-stu-id="b7ace-209">a.</span></span> <span data-ttu-id="b7ace-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7ace-211">b.</span><span class="sxs-lookup"><span data-stu-id="b7ace-211">b.</span></span> <span data-ttu-id="b7ace-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7ace-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7ace-213">c.</span><span class="sxs-lookup"><span data-stu-id="b7ace-213">c.</span></span> <span data-ttu-id="b7ace-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b7ace-215">d.</span><span class="sxs-lookup"><span data-stu-id="b7ace-215">d.</span></span> <span data-ttu-id="b7ace-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7ace-216">Click **Create**.</span></span>
 
### <a name="creating-a-samanage-test-user"></a><span data-ttu-id="b7ace-217">Samanage tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7ace-217">Creating a Samanage test user</span></span>

<span data-ttu-id="b7ace-218">Ahhoz, hogy az Azure AD-felhasználók Samanage bejelentkezni, akkor ki kell építenie a Samanage.</span><span class="sxs-lookup"><span data-stu-id="b7ace-218">To enable Azure AD users to log in to Samanage, they must be provisioned into Samanage.</span></span>  
<span data-ttu-id="b7ace-219">Samanage, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b7ace-219">In the case of Samanage, provisioning is a manual task.</span></span>

<span data-ttu-id="b7ace-220">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7ace-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="b7ace-221">Jelentkezzen be a Samanage vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b7ace-221">Log into your Samanage company site as an administrator.</span></span>

2. <span data-ttu-id="b7ace-222">Kattintson a **irányítópult** válassza **telepítő** a bal oldali navigációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="b7ace-222">Click **Dashboard** and select **Setup** in left navigation pan.</span></span>
   
    <span data-ttu-id="b7ace-223">![A telepítő](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="b7ace-223">![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")</span></span>

3. <span data-ttu-id="b7ace-224">Kattintson a **felhasználók** lap</span><span class="sxs-lookup"><span data-stu-id="b7ace-224">Click the **Users** tab</span></span>
   
    <span data-ttu-id="b7ace-225">![Felhasználók](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="b7ace-225">![Users](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "Users")</span></span>

4. <span data-ttu-id="b7ace-226">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-226">Click **New User**.</span></span>
   
    <span data-ttu-id="b7ace-227">![Új felhasználó](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="b7ace-227">![New User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "New User")</span></span>

5. <span data-ttu-id="b7ace-228">Típus a **neve** és a **E-mail cím** egy Azure Active Directory-fiók ellátásához, majd kattintson a kívánt **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-228">Type the **Name** and the **Email Address** of an Azure Active Directory account you want to provision and click **Create user**.</span></span>
   
    <span data-ttu-id="b7ace-229">![Hozzon létre felhasználói](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="b7ace-229">![Create User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "Create User")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="b7ace-230">Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="b7ace-230">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span> <span data-ttu-id="b7ace-231">Bármely más Samanage felhasználói fiók létrehozása eszközök vagy API-k által biztosított Samanage kiépítését Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="b7ace-231">You can use any other Samanage user account creation tools or APIs provided by Samanage to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b7ace-232">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b7ace-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b7ace-233">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Samanage Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b7ace-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Samanage.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b7ace-235">**Britta Simon hozzárendelése Samanage, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7ace-235">**To assign Britta Simon to Samanage, perform the following steps:**</span></span>

1. <span data-ttu-id="b7ace-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b7ace-238">Az alkalmazások listában válassza ki a **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-238">In the applications list, select **Samanage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. <span data-ttu-id="b7ace-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b7ace-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b7ace-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7ace-242">Click **Add** button.</span></span> <span data-ttu-id="b7ace-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7ace-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b7ace-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b7ace-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b7ace-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7ace-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7ace-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7ace-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7ace-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b7ace-248">Testing single sign-on</span></span>

<span data-ttu-id="b7ace-249">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b7ace-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b7ace-250">Ha a hozzáférési panelen Samanage csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Samanage alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="b7ace-250">When you click the Samanage tile in the Access Panel, you should get automatically signed-on to your Samanage application.</span></span>
<span data-ttu-id="b7ace-251">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b7ace-251">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7ace-252">További források</span><span class="sxs-lookup"><span data-stu-id="b7ace-252">Additional resources</span></span>

* [<span data-ttu-id="b7ace-253">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b7ace-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7ace-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b7ace-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

