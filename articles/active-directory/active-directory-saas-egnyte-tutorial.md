---
title: "Oktatóanyag: Azure Active Directoryval integrált Egnyte |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Egnyte között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 62d01333b61e73c83588d2d1701c0c300df4ab1c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a><span data-ttu-id="fe32b-103">Oktatóanyag: Azure Active Directoryval integrált Egnyte</span><span class="sxs-lookup"><span data-stu-id="fe32b-103">Tutorial: Azure Active Directory integration with Egnyte</span></span>

<span data-ttu-id="fe32b-104">Ebben az oktatóanyagban elsajátíthatja Egnyte integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fe32b-104">In this tutorial, you learn how to integrate Egnyte with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fe32b-105">Egnyte integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fe32b-105">Integrating Egnyte with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fe32b-106">Megadhatja a Egnyte hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fe32b-106">You can control in Azure AD who has access to Egnyte</span></span>
- <span data-ttu-id="fe32b-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Egnyte (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="fe32b-107">You can enable your users to automatically get signed-on to Egnyte (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fe32b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fe32b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fe32b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fe32b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe32b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fe32b-110">Prerequisites</span></span>

<span data-ttu-id="fe32b-111">Konfigurálása az Azure AD-integrációs Egnyte, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="fe32b-111">To configure Azure AD integration with Egnyte, you need the following items:</span></span>

- <span data-ttu-id="fe32b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fe32b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fe32b-113">Egy Egnyte egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fe32b-113">An Egnyte single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fe32b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe32b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fe32b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fe32b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fe32b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe32b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fe32b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fe32b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fe32b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fe32b-118">Scenario description</span></span>
<span data-ttu-id="fe32b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fe32b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fe32b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fe32b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fe32b-121">A gyűjteményből Egnyte hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe32b-121">Adding Egnyte from the gallery</span></span>
2. <span data-ttu-id="fe32b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fe32b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-egnyte-from-the-gallery"></a><span data-ttu-id="fe32b-123">A gyűjteményből Egnyte hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fe32b-123">Adding Egnyte from the gallery</span></span>
<span data-ttu-id="fe32b-124">Az Azure AD integrálása a Egnyte konfigurálásához kell hozzáadnia Egnyte a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fe32b-124">To configure the integration of Egnyte into Azure AD, you need to add Egnyte from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fe32b-125">**A gyűjteményből Egnyte hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe32b-125">**To add Egnyte from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fe32b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fe32b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fe32b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="fe32b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="fe32b-133">Írja be a keresőmezőbe, **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-133">In the search box, type **Egnyte**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. <span data-ttu-id="fe32b-135">Az eredmények panelen válassza ki a **Egnyte**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe32b-135">In the results panel, select **Egnyte**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fe32b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fe32b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fe32b-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Egnyte</span><span class="sxs-lookup"><span data-stu-id="fe32b-138">In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fe32b-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Egnyte a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fe32b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Egnyte is to a user in Azure AD.</span></span> <span data-ttu-id="fe32b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Egnyte közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fe32b-140">In other words, a link relationship between an Azure AD user and the related user in Egnyte needs to be established.</span></span>

<span data-ttu-id="fe32b-141">Egnyte, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fe32b-141">In Egnyte, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fe32b-142">Az Azure AD egyszeri bejelentkezést a Egnyte tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="fe32b-142">To configure and test Azure AD single sign-on with Egnyte, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fe32b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fe32b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fe32b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fe32b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fe32b-145">**[Egy Egnyte tesztfelhasználó létrehozása](#creating-an-egnyte-test-user)**  - való Britta Simon valami Egnyte, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fe32b-145">**[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - to have a counterpart of Britta Simon in Egnyte that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fe32b-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe32b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fe32b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fe32b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fe32b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe32b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fe32b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Egnyte alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fe32b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Egnyte application.</span></span>

<span data-ttu-id="fe32b-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Egnyte, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe32b-150">**To configure Azure AD single sign-on with Egnyte, perform the following steps:**</span></span>

1. <span data-ttu-id="fe32b-151">Az Azure portálon a a **Egnyte** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-151">In the Azure portal, on the **Egnyte** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="fe32b-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe32b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. <span data-ttu-id="fe32b-155">Az a **Egnyte tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe32b-155">On the **Egnyte Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    <span data-ttu-id="fe32b-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.egnyte.com`</span><span class="sxs-lookup"><span data-stu-id="fe32b-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.egnyte.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fe32b-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="fe32b-158">This value is not real.</span></span> <span data-ttu-id="fe32b-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="fe32b-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="fe32b-160">Ügyfél [Egnyte ügyfél-támogatási csoport](https://www.egnyte.com/corp/contact_egnyte.html) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="fe32b-160">Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) to get this value.</span></span> 
 
4. <span data-ttu-id="fe32b-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fe32b-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. <span data-ttu-id="fe32b-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fe32b-165">A a **Egnyte konfigurációs** kattintson **konfigurálása Egnyte** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fe32b-165">On the **Egnyte Configuration** section, click **Configure Egnyte** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fe32b-166">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fe32b-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. <span data-ttu-id="fe32b-168">Egy másik webes böngészőablakban jelentkezzen be a Egnyte vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fe32b-168">In a different web browser window, log in to your Egnyte company site as an administrator.</span></span>

8. <span data-ttu-id="fe32b-169">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-169">Click **Settings**.</span></span>
   
   <span data-ttu-id="fe32b-170">![Beállítások](./media/active-directory-saas-egnyte-tutorial/ic787819.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="fe32b-170">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787819.png "Settings")</span></span>

9. <span data-ttu-id="fe32b-171">A menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-171">In the menu, click **Settings**.</span></span>

   <span data-ttu-id="fe32b-172">![Beállítások](./media/active-directory-saas-egnyte-tutorial/ic787820.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="fe32b-172">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787820.png "Settings")</span></span>

10. <span data-ttu-id="fe32b-173">Kattintson a **konfigurációs** fülre, majd **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-173">Click the **Configuration** tab, and then click **Security**.</span></span>

    <span data-ttu-id="fe32b-174">![Biztonsági](./media/active-directory-saas-egnyte-tutorial/ic787821.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="fe32b-174">![Security](./media/active-directory-saas-egnyte-tutorial/ic787821.png "Security")</span></span>

11. <span data-ttu-id="fe32b-175">Az a **egyszeri bejelentkezés hitelesítési** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe32b-175">In the **Single Sign-On Authentication** section, perform the following steps:</span></span>

    <span data-ttu-id="fe32b-176">![Egyszeri bejelentkezés hitelesítési](./media/active-directory-saas-egnyte-tutorial/ic787822.png "egyszeri bejelentkezés a hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="fe32b-176">![Single Sign On Authentication](./media/active-directory-saas-egnyte-tutorial/ic787822.png "Single Sign On Authentication")</span></span>   
    
    <span data-ttu-id="fe32b-177">a.</span><span class="sxs-lookup"><span data-stu-id="fe32b-177">a.</span></span> <span data-ttu-id="fe32b-178">Mint **egyszeri bejelentkezés hitelesítési**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-178">As **Single sign-on authentication**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="fe32b-179">b.</span><span class="sxs-lookup"><span data-stu-id="fe32b-179">b.</span></span> <span data-ttu-id="fe32b-180">Mint **identitásszolgáltató**, jelölje be **AzureAD**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-180">As **Identity provider**, select **AzureAD**.</span></span>
   
    <span data-ttu-id="fe32b-181">c.</span><span class="sxs-lookup"><span data-stu-id="fe32b-181">c.</span></span> <span data-ttu-id="fe32b-182">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** másolja az Azure-portálon a **Identity provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe32b-182">Paste **SAML Single Sign-On Service URL** copied from Azure portal into the **Identity provider login URL** textbox.</span></span>
   
    <span data-ttu-id="fe32b-183">d.</span><span class="sxs-lookup"><span data-stu-id="fe32b-183">d.</span></span> <span data-ttu-id="fe32b-184">Beillesztés **SAML Entitásazonosító** , amely az Azure portálról másolta a **Identity provider Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe32b-184">Paste **SAML Entity ID** which you have copied from Azure portal into the **Identity provider entity ID** textbox.</span></span>
      
    <span data-ttu-id="fe32b-185">e.</span><span class="sxs-lookup"><span data-stu-id="fe32b-185">e.</span></span> <span data-ttu-id="fe32b-186">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról letöltött, a tartalmának másolása a vágólapra és illessze be azt a **szolgáltató identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe32b-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Identity provider certificate** textbox.</span></span>
   
    <span data-ttu-id="fe32b-187">f.</span><span class="sxs-lookup"><span data-stu-id="fe32b-187">f.</span></span> <span data-ttu-id="fe32b-188">Mint **alapértelmezett felhasználói hozzárendelésének**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-188">As **Default user mapping**, select **Email address**.</span></span>
   
    <span data-ttu-id="fe32b-189">g.</span><span class="sxs-lookup"><span data-stu-id="fe32b-189">g.</span></span> <span data-ttu-id="fe32b-190">Mint **tartományspecifikus kibocsátó érték**, jelölje be **le van tiltva**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-190">As **Use domain-specific issuer value**, select **disabled**.</span></span>
   
    <span data-ttu-id="fe32b-191">h.</span><span class="sxs-lookup"><span data-stu-id="fe32b-191">h.</span></span> <span data-ttu-id="fe32b-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-192">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="fe32b-193">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fe32b-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fe32b-194">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fe32b-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fe32b-195">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fe32b-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fe32b-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe32b-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="fe32b-197">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fe32b-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="fe32b-199">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe32b-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fe32b-200">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fe32b-202">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fe32b-204">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="fe32b-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fe32b-206">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fe32b-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fe32b-208">a.</span><span class="sxs-lookup"><span data-stu-id="fe32b-208">a.</span></span> <span data-ttu-id="fe32b-209">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fe32b-210">b.</span><span class="sxs-lookup"><span data-stu-id="fe32b-210">b.</span></span> <span data-ttu-id="fe32b-211">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fe32b-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fe32b-212">c.</span><span class="sxs-lookup"><span data-stu-id="fe32b-212">c.</span></span> <span data-ttu-id="fe32b-213">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fe32b-214">d.</span><span class="sxs-lookup"><span data-stu-id="fe32b-214">d.</span></span> <span data-ttu-id="fe32b-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-215">Click **Create**.</span></span>
 
### <a name="creating-an-egnyte-test-user"></a><span data-ttu-id="fe32b-216">Egy Egnyte tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe32b-216">Creating an Egnyte test user</span></span>

<span data-ttu-id="fe32b-217">Ahhoz, hogy az Azure AD-felhasználók Egnyte bejelentkezni, akkor ki kell építenie a Egnyte.</span><span class="sxs-lookup"><span data-stu-id="fe32b-217">To enable Azure AD users to log in to Egnyte, they must be provisioned into Egnyte.</span></span> <span data-ttu-id="fe32b-218">Egnyte, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fe32b-218">In the case of Egnyte, provisioning is a manual task.</span></span>

<span data-ttu-id="fe32b-219">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe32b-219">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="fe32b-220">Jelentkezzen be a **Egnyte** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fe32b-220">Log in to your **Egnyte** company site as administrator.</span></span>

2. <span data-ttu-id="fe32b-221">Ugrás a **beállítások \> felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-221">Go to **Settings \> Users & Groups**.</span></span>

3. <span data-ttu-id="fe32b-222">Kattintson a **új felhasználó hozzáadása**, majd válassza ki a hozzáadni kívánt felhasználó típusát.</span><span class="sxs-lookup"><span data-stu-id="fe32b-222">Click **Add New User**, and then select the type of user you want to add.</span></span>
   
   <span data-ttu-id="fe32b-223">![Felhasználók](./media/active-directory-saas-egnyte-tutorial/ic787824.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="fe32b-223">![Users](./media/active-directory-saas-egnyte-tutorial/ic787824.png "Users")</span></span>

4. <span data-ttu-id="fe32b-224">Az a **új általános jogú felhasználó** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe32b-224">In the **New Standard User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="fe32b-225">![Új általános jogú felhasználó](./media/active-directory-saas-egnyte-tutorial/ic787825.png "új általános jogú felhasználó")</span><span class="sxs-lookup"><span data-stu-id="fe32b-225">![New Standard User](./media/active-directory-saas-egnyte-tutorial/ic787825.png "New Standard User")</span></span>   

   <span data-ttu-id="fe32b-226">a.</span><span class="sxs-lookup"><span data-stu-id="fe32b-226">a.</span></span> <span data-ttu-id="fe32b-227">Típus a **E-mail**, **felhasználónév**, és egyéb részleteket a egy érvényes Azure Active Directory-fióknevet, amelyet kiépítését.</span><span class="sxs-lookup"><span data-stu-id="fe32b-227">Type the **Email**, **Username**, and other details of a valid Azure Active Directory account you want to provision.</span></span>
   
   <span data-ttu-id="fe32b-228">b.</span><span class="sxs-lookup"><span data-stu-id="fe32b-228">b.</span></span> <span data-ttu-id="fe32b-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-229">Click **Save**.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="fe32b-230">Az Azure Active Directory fióktulajdonos e-mailben értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="fe32b-230">The Azure Active Directory account holder will receive a notification email.</span></span>
    >

>[!NOTE]
><span data-ttu-id="fe32b-231">Bármely más Egnyte felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Egnyte által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="fe32b-231">You can use any other Egnyte user account creation tools or APIs provided by Egnyte to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fe32b-232">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fe32b-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fe32b-233">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Egnyte Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="fe32b-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Egnyte.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fe32b-235">**Britta Simon hozzárendelése Egnyte, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe32b-235">**To assign Britta Simon to Egnyte, perform the following steps:**</span></span>

1. <span data-ttu-id="fe32b-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fe32b-238">Az alkalmazások listában válassza ki a **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-238">In the applications list, select **Egnyte**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. <span data-ttu-id="fe32b-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fe32b-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fe32b-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe32b-242">Click **Add** button.</span></span> <span data-ttu-id="fe32b-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe32b-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fe32b-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fe32b-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fe32b-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe32b-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fe32b-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe32b-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fe32b-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fe32b-248">Testing single sign-on</span></span>

<span data-ttu-id="fe32b-249">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fe32b-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fe32b-250">Ha a hozzáférési panelen Egnyte csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Egnyte alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="fe32b-250">When you click the Egnyte tile in the Access Panel, you should get automatically signed-on to your Egnyte application.</span></span>
<span data-ttu-id="fe32b-251">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fe32b-251">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fe32b-252">További források</span><span class="sxs-lookup"><span data-stu-id="fe32b-252">Additional resources</span></span>

* [<span data-ttu-id="fe32b-253">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fe32b-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fe32b-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fe32b-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

