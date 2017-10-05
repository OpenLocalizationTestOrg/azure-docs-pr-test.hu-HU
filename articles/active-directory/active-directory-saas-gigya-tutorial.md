---
title: "Oktatóanyag: Azure Active Directoryval integrált Gigya |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Gigya között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: b65a33989a045a3e0b57fda522a9bc3b0770c7f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a><span data-ttu-id="75113-103">Oktatóanyag: Azure Active Directoryval integrált Gigya</span><span class="sxs-lookup"><span data-stu-id="75113-103">Tutorial: Azure Active Directory integration with Gigya</span></span>

<span data-ttu-id="75113-104">Ebben az oktatóanyagban elsajátíthatja Gigya integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="75113-104">In this tutorial, you learn how to integrate Gigya with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75113-105">Gigya integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="75113-105">Integrating Gigya with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="75113-106">Megadhatja a Gigya hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="75113-106">You can control in Azure AD who has access to Gigya</span></span>
- <span data-ttu-id="75113-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Gigya (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="75113-107">You can enable your users to automatically get signed-on to Gigya (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75113-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="75113-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="75113-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75113-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75113-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="75113-110">Prerequisites</span></span>

<span data-ttu-id="75113-111">Konfigurálása az Azure AD-integrációs Gigya, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="75113-111">To configure Azure AD integration with Gigya, you need the following items:</span></span>

- <span data-ttu-id="75113-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="75113-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75113-113">Egy Gigya egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="75113-113">A Gigya single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75113-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="75113-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75113-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="75113-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75113-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="75113-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75113-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75113-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75113-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="75113-118">Scenario description</span></span>
<span data-ttu-id="75113-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="75113-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75113-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="75113-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75113-121">A gyűjteményből Gigya hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75113-121">Adding Gigya from the gallery</span></span>
2. <span data-ttu-id="75113-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="75113-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gigya-from-the-gallery"></a><span data-ttu-id="75113-123">A gyűjteményből Gigya hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75113-123">Adding Gigya from the gallery</span></span>
<span data-ttu-id="75113-124">Az Azure AD integrálása a Gigya konfigurálásához kell hozzáadnia Gigya a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="75113-124">To configure the integration of Gigya into Azure AD, you need to add Gigya from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="75113-125">**A gyűjteményből Gigya hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="75113-125">**To add Gigya from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="75113-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="75113-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75113-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="75113-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="75113-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="75113-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="75113-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="75113-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="75113-133">Írja be a keresőmezőbe, **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="75113-133">In the search box, type **Gigya**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_search.png)

5. <span data-ttu-id="75113-135">Az eredmények panelen válassza ki a **Gigya**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="75113-135">In the results panel, select **Gigya**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="75113-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="75113-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="75113-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Gigya.</span><span class="sxs-lookup"><span data-stu-id="75113-138">In this section, you configure and test Azure AD single sign-on with Gigya based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="75113-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Gigya a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="75113-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Gigya is to a user in Azure AD.</span></span> <span data-ttu-id="75113-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Gigya közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="75113-140">In other words, a link relationship between an Azure AD user and the related user in Gigya needs to be established.</span></span>

<span data-ttu-id="75113-141">Gigya, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="75113-141">In Gigya, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="75113-142">Az Azure AD egyszeri bejelentkezést a Gigya tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="75113-142">To configure and test Azure AD single sign-on with Gigya, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="75113-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="75113-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="75113-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="75113-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75113-145">**[Gigya tesztfelhasználó létrehozása](#creating-a-gigya-test-user)**  - való Britta Simon valami Gigya, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="75113-145">**[Creating a Gigya test user](#creating-a-gigya-test-user)** - to have a counterpart of Britta Simon in Gigya that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="75113-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="75113-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75113-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="75113-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="75113-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="75113-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="75113-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Gigya alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="75113-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Gigya application.</span></span>

<span data-ttu-id="75113-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Gigya, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="75113-150">**To configure Azure AD single sign-on with Gigya, perform the following steps:**</span></span>

1. <span data-ttu-id="75113-151">Az Azure portálon a a **Gigya** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="75113-151">In the Azure portal, on the **Gigya** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="75113-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="75113-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_samlbase.png)

3. <span data-ttu-id="75113-155">Az a **Gigya tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="75113-155">On the **Gigya Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_url.png)

    <span data-ttu-id="75113-157">a.</span><span class="sxs-lookup"><span data-stu-id="75113-157">a.</span></span> <span data-ttu-id="75113-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`http://<companyname>.gigya.com`</span><span class="sxs-lookup"><span data-stu-id="75113-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<companyname>.gigya.com`</span></span>

    <span data-ttu-id="75113-159">b.</span><span class="sxs-lookup"><span data-stu-id="75113-159">b.</span></span> <span data-ttu-id="75113-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://fidm.gigya.com/saml/v2.0/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="75113-160">In the **Identifier** textbox, type a URL using the following pattern: `https://fidm.gigya.com/saml/v2.0/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="75113-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="75113-161">These values are not real.</span></span> <span data-ttu-id="75113-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="75113-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="75113-163">Ügyfél [Gigya ügyfél-támogatási csoport](https://www.gigya.com/support-policy/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="75113-163">Contact [Gigya Client support team](https://www.gigya.com/support-policy/) to get these values.</span></span> 
 
4. <span data-ttu-id="75113-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="75113-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_certificate.png) 

5. <span data-ttu-id="75113-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="75113-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="75113-168">A a **Gigya konfigurációs** kattintson **konfigurálása Gigya** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="75113-168">On the **Gigya Configuration** section, click **Configure Gigya** to open **Configure sign-on** window.</span></span> <span data-ttu-id="75113-169">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="75113-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_configure.png) 

7. <span data-ttu-id="75113-171">Egy másik webes böngészőablakban jelentkezzen be a Gigya vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="75113-171">In a different web browser window, log into your Gigya company site as an administrator.</span></span>

8. <span data-ttu-id="75113-172">Lépjen **beállítások \> SAML bejelentkezési**, majd kattintson a **Hozzáadás** gomb.</span><span class="sxs-lookup"><span data-stu-id="75113-172">Go to **Settings \> SAML Login**, and then click the **Add** button.</span></span>
   
    <span data-ttu-id="75113-173">![SAML bejelentkezési](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML-bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="75113-173">![SAML Login](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML Login")</span></span>

9. <span data-ttu-id="75113-174">Az a **SAML bejelentkezési** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="75113-174">In the **SAML Login** section, perform the following steps:</span></span>
   
    <span data-ttu-id="75113-175">![SAML-alapú konfigurációs](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="75113-175">![SAML Configuration](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML Configuration")</span></span>
   
    <span data-ttu-id="75113-176">a.</span><span class="sxs-lookup"><span data-stu-id="75113-176">a.</span></span> <span data-ttu-id="75113-177">Az a **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="75113-177">In the **Name** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="75113-178">b.</span><span class="sxs-lookup"><span data-stu-id="75113-178">b.</span></span> <span data-ttu-id="75113-179">A **kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="75113-179">In **Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure Portal.</span></span> 
   
    <span data-ttu-id="75113-180">c.</span><span class="sxs-lookup"><span data-stu-id="75113-180">c.</span></span> <span data-ttu-id="75113-181">A **egyszeri bejelentkezési URL-címe** szövegmezőhöz illessze be az értékét **egyszeri bejelentkezési URL-címe** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="75113-181">In **Single Sign-On Service URL** textbox, paste the value of **Single Sign-On Service URL** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="75113-182">d.</span><span class="sxs-lookup"><span data-stu-id="75113-182">d.</span></span> <span data-ttu-id="75113-183">A **név Azonosítóformátumának** szövegmezőhöz illessze be az értékét **azonosító formátuma** , amely az Azure portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="75113-183">In **Name ID Format** textbox, paste the value of **Name Identifier Format** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="75113-184">e.</span><span class="sxs-lookup"><span data-stu-id="75113-184">e.</span></span> <span data-ttu-id="75113-185">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról letöltött, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="75113-185">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="75113-186">f.</span><span class="sxs-lookup"><span data-stu-id="75113-186">f.</span></span> <span data-ttu-id="75113-187">Kattintson a **beállítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="75113-187">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="75113-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="75113-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="75113-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="75113-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="75113-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75113-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="75113-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="75113-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="75113-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="75113-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="75113-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="75113-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="75113-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="75113-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="75113-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="75113-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75113-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="75113-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75113-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="75113-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gigya-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75113-203">a.</span><span class="sxs-lookup"><span data-stu-id="75113-203">a.</span></span> <span data-ttu-id="75113-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75113-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75113-205">b.</span><span class="sxs-lookup"><span data-stu-id="75113-205">b.</span></span> <span data-ttu-id="75113-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75113-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75113-207">c.</span><span class="sxs-lookup"><span data-stu-id="75113-207">c.</span></span> <span data-ttu-id="75113-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="75113-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="75113-209">d.</span><span class="sxs-lookup"><span data-stu-id="75113-209">d.</span></span> <span data-ttu-id="75113-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="75113-210">Click **Create**.</span></span>
 
### <a name="creating-a-gigya-test-user"></a><span data-ttu-id="75113-211">Gigya tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="75113-211">Creating a Gigya test user</span></span>

<span data-ttu-id="75113-212">Ahhoz, hogy az Azure AD-felhasználók Gigya bejelentkezni, akkor ki kell építenie Gigya be.</span><span class="sxs-lookup"><span data-stu-id="75113-212">In order to enable Azure AD users to log into Gigya, they must be provisioned into Gigya.</span></span>  
<span data-ttu-id="75113-213">Gigya, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="75113-213">In the case of Gigya, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="75113-214">A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="75113-214">To provision a user accounts, perform the following steps:</span></span>

1. <span data-ttu-id="75113-215">Jelentkezzen be a **Gigya** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="75113-215">Log in to your **Gigya** company site as an administrator.</span></span>

2. <span data-ttu-id="75113-216">Nyissa meg a **Admin \> felhasználók kezelése**, és kattintson a **meghívása felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="75113-216">Go to **Admin \> Manage Users**, and then click **Invite Users**.</span></span>
   
    <span data-ttu-id="75113-217">![Felhasználók kezelése](./media/active-directory-saas-gigya-tutorial/ic789535.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="75113-217">![Manage Users](./media/active-directory-saas-gigya-tutorial/ic789535.png "Manage Users")</span></span>

3. <span data-ttu-id="75113-218">A felhasználók meghívása párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="75113-218">On the Invite Users dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="75113-219">![Meghívott felhasználóknak](./media/active-directory-saas-gigya-tutorial/ic789536.png "meghívott felhasználóknak")</span><span class="sxs-lookup"><span data-stu-id="75113-219">![Invite Users](./media/active-directory-saas-gigya-tutorial/ic789536.png "Invite Users")</span></span>
   
    <span data-ttu-id="75113-220">a.</span><span class="sxs-lookup"><span data-stu-id="75113-220">a.</span></span> <span data-ttu-id="75113-221">Az a **E-mail** szövegmező, írja be egy érvényes Azure Active Directory-fiókot rendelkezés kívánt e-mail aliasát.</span><span class="sxs-lookup"><span data-stu-id="75113-221">In the **Email** textbox, type the email alias of a valid Azure Active Directory account you want to provision.</span></span>
    
    <span data-ttu-id="75113-222">b.</span><span class="sxs-lookup"><span data-stu-id="75113-222">b.</span></span> <span data-ttu-id="75113-223">Kattintson a **felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="75113-223">Click **Invite User**.</span></span>
      
    > [!NOTE]
    > <span data-ttu-id="75113-224">Az Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmaz egy hivatkozást a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="75113-224">The Azure Active Directory account holder will receive an email that includes a link to confirm the account before it becomes active.</span></span>
    > 
    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="75113-225">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="75113-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="75113-226">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Gigya Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="75113-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Gigya.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="75113-228">**Britta Simon hozzárendelése Gigya, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="75113-228">**To assign Britta Simon to Gigya, perform the following steps:**</span></span>

1. <span data-ttu-id="75113-229">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="75113-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="75113-231">Az alkalmazások listában válassza ki a **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="75113-231">In the applications list, select **Gigya**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_app.png) 

3. <span data-ttu-id="75113-233">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="75113-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="75113-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="75113-235">Click **Add** button.</span></span> <span data-ttu-id="75113-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75113-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="75113-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="75113-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="75113-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75113-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75113-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="75113-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="75113-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="75113-241">Testing single sign-on</span></span>

<span data-ttu-id="75113-242">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="75113-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="75113-243">Ha a hozzáférési panelen Gigya csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Gigya alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="75113-243">When you click the Gigya tile in the Access Panel, you should get automatically signed-on to your Gigya application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75113-244">További források</span><span class="sxs-lookup"><span data-stu-id="75113-244">Additional resources</span></span>

* [<span data-ttu-id="75113-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="75113-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75113-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="75113-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_203.png

