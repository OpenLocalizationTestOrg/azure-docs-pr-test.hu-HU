---
title: "Oktatóanyag: Azure Active Directoryval integrált cukor CRM |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és cukor CRM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: c27aef24e859522b8001ecb747906abdca14d87a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a><span data-ttu-id="8f637-103">Oktatóanyag: Azure Active Directoryval integrált cukor CRM</span><span class="sxs-lookup"><span data-stu-id="8f637-103">Tutorial: Azure Active Directory integration with Sugar CRM</span></span>

<span data-ttu-id="8f637-104">Ebben az oktatóanyagban elsajátíthatja cukor CRM integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8f637-104">In this tutorial, you learn how to integrate Sugar CRM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8f637-105">Cukor CRM integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8f637-105">Integrating Sugar CRM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8f637-106">Szabályozhatja az Azure AD, aki hozzáfér a cukor CRM-hez</span><span class="sxs-lookup"><span data-stu-id="8f637-106">You can control in Azure AD who has access to Sugar CRM</span></span>
- <span data-ttu-id="8f637-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett a cukor CRM-hez (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="8f637-107">You can enable your users to automatically get signed-on to Sugar CRM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8f637-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8f637-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8f637-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8f637-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f637-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f637-110">Prerequisites</span></span>

<span data-ttu-id="8f637-111">Az Azure AD-integráció konfigurálása a cukor CRM, a következő elemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8f637-111">To configure Azure AD integration with Sugar CRM, you need the following items:</span></span>

- <span data-ttu-id="8f637-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8f637-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8f637-113">Egy cukor CRM egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8f637-113">A Sugar CRM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8f637-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8f637-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8f637-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8f637-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8f637-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8f637-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8f637-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f637-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8f637-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8f637-118">Scenario description</span></span>
<span data-ttu-id="8f637-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8f637-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8f637-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8f637-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8f637-121">A gyűjteményből cukor CRM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f637-121">Adding Sugar CRM from the gallery</span></span>
2. <span data-ttu-id="8f637-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8f637-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sugar-crm-from-the-gallery"></a><span data-ttu-id="8f637-123">A gyűjteményből cukor CRM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f637-123">Adding Sugar CRM from the gallery</span></span>
<span data-ttu-id="8f637-124">Az Azure AD integrálása a cukor CRM konfigurálásához kell hozzáadnia cukor CRM a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="8f637-124">To configure the integration of Sugar CRM into Azure AD, you need to add Sugar CRM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8f637-125">**A gyűjteményből cukor CRM hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8f637-125">**To add Sugar CRM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8f637-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f637-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8f637-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8f637-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8f637-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8f637-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8f637-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8f637-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8f637-133">Írja be a keresőmezőbe, **cukor CRM**.</span><span class="sxs-lookup"><span data-stu-id="8f637-133">In the search box, type **Sugar CRM**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. <span data-ttu-id="8f637-135">Az eredmények panelen válassza ki a **cukor CRM**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8f637-135">In the results panel, select **Sugar CRM**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8f637-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8f637-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8f637-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés cukor CRM "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="8f637-138">In this section, you configure and test Azure AD single sign-on with Sugar CRM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8f637-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó cukor CRM a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8f637-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sugar CRM is to a user in Azure AD.</span></span> <span data-ttu-id="8f637-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a cukor CRM közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8f637-140">In other words, a link relationship between an Azure AD user and the related user in Sugar CRM needs to be established.</span></span>

<span data-ttu-id="8f637-141">A cukor CRM, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8f637-141">In Sugar CRM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8f637-142">Az Azure AD egyszeri bejelentkezést a cukor CRM tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8f637-142">To configure and test Azure AD single sign-on with Sugar CRM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8f637-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8f637-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8f637-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8f637-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8f637-145">**[Cukor CRM tesztfelhasználó létrehozása](#creating-a-sugar-crm-test-user)**  - való egy megfelelője a Britta Simon cukor CRM-hez, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8f637-145">**[Creating a Sugar CRM test user](#creating-a-sugar-crm-test-user)** - to have a counterpart of Britta Simon in Sugar CRM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8f637-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8f637-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8f637-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8f637-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8f637-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f637-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8f637-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az cukor CRM alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8f637-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sugar CRM application.</span></span>

<span data-ttu-id="8f637-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés cukor CRM, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8f637-150">**To configure Azure AD single sign-on with Sugar CRM, perform the following steps:**</span></span>

1. <span data-ttu-id="8f637-151">Az Azure portálon a a **cukor CRM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8f637-151">In the Azure portal, on the **Sugar CRM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8f637-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8f637-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. <span data-ttu-id="8f637-155">Az a **cukor CRM tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8f637-155">On the **Sugar CRM Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    <span data-ttu-id="8f637-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="8f637-157">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > <span data-ttu-id="8f637-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="8f637-158">The value is not real.</span></span> <span data-ttu-id="8f637-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="8f637-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="8f637-160">Ügyfél [cukor CRM ügyfél-támogatási csoport](https://support.sugarcrm.com/) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="8f637-160">Contact [Sugar CRM Client support team](https://support.sugarcrm.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="8f637-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8f637-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. <span data-ttu-id="8f637-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f637-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8f637-165">A a **cukor CRM konfigurációs** kattintson **konfigurálása cukor CRM** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8f637-165">On the **Sugar CRM Configuration** section, click **Configure Sugar CRM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8f637-166">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8f637-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. <span data-ttu-id="8f637-168">Egy másik webes böngészőablakban jelentkezzen be a cukor CRM vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8f637-168">In a different web browser window, log in to your Sugar CRM company site as an administrator.</span></span>

8. <span data-ttu-id="8f637-169">Ugrás a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="8f637-169">Go to **Admin**.</span></span>
   
    <span data-ttu-id="8f637-170">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="8f637-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

9. <span data-ttu-id="8f637-171">Az a **felügyeleti** kattintson **jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="8f637-171">In the **Administration** section, click **Password Management**.</span></span>
   
    <span data-ttu-id="8f637-172">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="8f637-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span></span>

10. <span data-ttu-id="8f637-173">Válassza ki **SAML-alapú hitelesítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="8f637-173">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="8f637-174">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="8f637-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span></span>

11. <span data-ttu-id="8f637-175">Az a **SAML-alapú hitelesítés** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8f637-175">In the **SAML Authentication** section, perform the following steps:</span></span>
   
    <span data-ttu-id="8f637-176">![SAML-alapú hitelesítés](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="8f637-176">![SAML Authentication](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML Authentication")</span></span>  
 
    <span data-ttu-id="8f637-177">a.</span><span class="sxs-lookup"><span data-stu-id="8f637-177">a.</span></span> <span data-ttu-id="8f637-178">A a **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8f637-178">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="8f637-179">b.</span><span class="sxs-lookup"><span data-stu-id="8f637-179">b.</span></span> <span data-ttu-id="8f637-180">A a **SLO URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8f637-180">In the **SLO URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="8f637-181">c.</span><span class="sxs-lookup"><span data-stu-id="8f637-181">c.</span></span> <span data-ttu-id="8f637-182">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be a teljes tanúsítványt **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8f637-182">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="8f637-183">d.</span><span class="sxs-lookup"><span data-stu-id="8f637-183">d.</span></span> <span data-ttu-id="8f637-184">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f637-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="8f637-185">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8f637-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8f637-186">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8f637-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8f637-187">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8f637-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8f637-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f637-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="8f637-189">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8f637-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8f637-191">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8f637-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8f637-192">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f637-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8f637-194">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8f637-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8f637-196">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8f637-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8f637-198">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8f637-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8f637-200">a.</span><span class="sxs-lookup"><span data-stu-id="8f637-200">a.</span></span> <span data-ttu-id="8f637-201">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f637-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8f637-202">b.</span><span class="sxs-lookup"><span data-stu-id="8f637-202">b.</span></span> <span data-ttu-id="8f637-203">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8f637-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8f637-204">c.</span><span class="sxs-lookup"><span data-stu-id="8f637-204">c.</span></span> <span data-ttu-id="8f637-205">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8f637-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8f637-206">d.</span><span class="sxs-lookup"><span data-stu-id="8f637-206">d.</span></span> <span data-ttu-id="8f637-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f637-207">Click **Create**.</span></span>
 
### <a name="creating-a-sugar-crm-test-user"></a><span data-ttu-id="8f637-208">Cukor CRM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f637-208">Creating a Sugar CRM test user</span></span>

<span data-ttu-id="8f637-209">Ahhoz, hogy az Azure AD-felhasználók cukor CRM bejelentkezni, akkor ki kell építenie a cukor CRM-hez.</span><span class="sxs-lookup"><span data-stu-id="8f637-209">In order to enable Azure AD users to log in to Sugar CRM, they must be provisioned to Sugar CRM.</span></span>

<span data-ttu-id="8f637-210">Cukor CRM, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8f637-210">In the case of Sugar CRM, provisioning is a manual task.</span></span>

<span data-ttu-id="8f637-211">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8f637-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="8f637-212">Jelentkezzen be a **cukor CRM** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8f637-212">Log in to your **Sugar CRM** company site as administrator.</span></span>

2. <span data-ttu-id="8f637-213">Ugrás a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="8f637-213">Go to **Admin**.</span></span>
   
    <span data-ttu-id="8f637-214">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="8f637-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

3. <span data-ttu-id="8f637-215">Az a **felügyeleti** kattintson **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="8f637-215">In the **Administration** section, click **User Management**.</span></span>
   
    <span data-ttu-id="8f637-216">![Felügyeleti](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="8f637-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span></span>

4. <span data-ttu-id="8f637-217">Ugrás a **felhasználók \> új felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8f637-217">Go to **Users \> Create New User**.</span></span>
   
    <span data-ttu-id="8f637-218">![Új felhasználó létrehozása](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "új felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8f637-218">![Create New User](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Create New User")</span></span>

5. <span data-ttu-id="8f637-219">Az a **felhasználói profil** lapra, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8f637-219">On the **User Profile** tab, perform the following steps:</span></span>
   
    <span data-ttu-id="8f637-220">![Új felhasználó](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="8f637-220">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "New User")</span></span>

    <span data-ttu-id="8f637-221">a.</span><span class="sxs-lookup"><span data-stu-id="8f637-221">a.</span></span> <span data-ttu-id="8f637-222">Típus a **felhasználónév**, **Vezetéknév**, és **e-mail cím** a kapcsolódó szövegmezők be egy érvényes Azure Active Directory felhasználó.</span><span class="sxs-lookup"><span data-stu-id="8f637-222">Type the **user name**, **last name**, and **email address** of a valid Azure Active Directory user into the related textboxes.</span></span>
  
6. <span data-ttu-id="8f637-223">Mint **állapot**, jelölje be **aktív**.</span><span class="sxs-lookup"><span data-stu-id="8f637-223">As **Status**, select **Active**.</span></span>

7. <span data-ttu-id="8f637-224">A jelszó lapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8f637-224">On the Password tab, perform the following steps:</span></span>
   
    <span data-ttu-id="8f637-225">![Új felhasználó](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="8f637-225">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "New User")</span></span>

    <span data-ttu-id="8f637-226">a.</span><span class="sxs-lookup"><span data-stu-id="8f637-226">a.</span></span> <span data-ttu-id="8f637-227">A kapcsolódó szövegmezőbe írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="8f637-227">Type the password into the related textbox.</span></span>

    <span data-ttu-id="8f637-228">b.</span><span class="sxs-lookup"><span data-stu-id="8f637-228">b.</span></span> <span data-ttu-id="8f637-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f637-229">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="8f637-230">Bármely más cukor CRM felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz cukor CRM által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="8f637-230">You can use any other Sugar CRM user account creation tools or APIs provided by Sugar CRM to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8f637-231">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8f637-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8f637-232">Ebben a szakaszban Britta Simon használandó által biztosított hozzáférés cukor CRM, Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8f637-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sugar CRM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8f637-234">**Britta Simon hozzárendelése cukor CRM, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8f637-234">**To assign Britta Simon to Sugar CRM, perform the following steps:**</span></span>

1. <span data-ttu-id="8f637-235">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8f637-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8f637-237">Az alkalmazások listában válassza ki a **cukor CRM**.</span><span class="sxs-lookup"><span data-stu-id="8f637-237">In the applications list, select **Sugar CRM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. <span data-ttu-id="8f637-239">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8f637-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8f637-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f637-241">Click **Add** button.</span></span> <span data-ttu-id="8f637-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f637-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8f637-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8f637-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8f637-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f637-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8f637-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f637-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8f637-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8f637-247">Testing single sign-on</span></span>

<span data-ttu-id="8f637-248">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="8f637-248">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8f637-249">Ha a hozzáférési panelen cukor CRM csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az cukor CRM-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8f637-249">When you click the Sugar CRM tile in the Access Panel, you should get automatically signed-on to your Sugar CRM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8f637-250">További források</span><span class="sxs-lookup"><span data-stu-id="8f637-250">Additional resources</span></span>

* [<span data-ttu-id="8f637-251">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8f637-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f637-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8f637-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

