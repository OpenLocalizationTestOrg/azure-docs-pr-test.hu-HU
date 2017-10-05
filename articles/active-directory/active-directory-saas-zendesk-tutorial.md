---
title: "Oktatóanyag: Azure Active Directoryval integrált Zendesk |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Zendesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a><span data-ttu-id="b7520-103">Oktatóanyag: Azure Active Directoryval integrált Zendesk</span><span class="sxs-lookup"><span data-stu-id="b7520-103">Tutorial: Azure Active Directory integration with Zendesk</span></span>

<span data-ttu-id="b7520-104">Ebben az oktatóanyagban elsajátíthatja Zendesk integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b7520-104">In this tutorial, you learn how to integrate Zendesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7520-105">Zendesk integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b7520-105">Integrating Zendesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b7520-106">Szabályozhatja, aki hozzáférhet ehhez az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b7520-106">You can control in Azure AD who has access to Zendesk</span></span>
- <span data-ttu-id="b7520-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Zendesk (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b7520-107">You can enable your users to automatically get signed-on to Zendesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7520-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b7520-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b7520-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7520-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7520-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b7520-110">Prerequisites</span></span>

<span data-ttu-id="b7520-111">Ehhez az Azure AD-integrációs konfigurálni, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b7520-111">To configure Azure AD integration with Zendesk, you need the following items:</span></span>

- <span data-ttu-id="b7520-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b7520-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7520-113">A Zendesk egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b7520-113">A Zendesk single sign-on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="b7520-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b7520-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="b7520-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b7520-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7520-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b7520-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7520-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7520-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="b7520-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b7520-118">Scenario description</span></span>
<span data-ttu-id="b7520-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b7520-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7520-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b7520-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7520-121">Zendesk hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b7520-121">Adding Zendesk from the gallery</span></span>
2. <span data-ttu-id="b7520-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b7520-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zendesk-from-the-gallery"></a><span data-ttu-id="b7520-123">Zendesk hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b7520-123">Adding Zendesk from the gallery</span></span>
<span data-ttu-id="b7520-124">Az Azure AD integrálása a Zendesk konfigurálásához kell hozzáadnia ehhez a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b7520-124">To configure the integration of Zendesk into Azure AD, you need to add Zendesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b7520-125">**Adja hozzá ehhez a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7520-125">**To add Zendesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b7520-126">Az a  **[Azure Portal](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b7520-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7520-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b7520-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b7520-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7520-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b7520-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b7520-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b7520-133">Írja be a keresőmezőbe, **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="b7520-133">In the search box, type **Zendesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. <span data-ttu-id="b7520-135">Az eredmények panelen válassza ki a **Zendesk**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7520-135">In the results panel, select **Zendesk**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7520-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b7520-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7520-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zendesk.</span><span class="sxs-lookup"><span data-stu-id="b7520-138">In this section, you configure and test Azure AD single sign-on with Zendesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7520-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Zendesk tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b7520-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zendesk is to a user in Azure AD.</span></span> <span data-ttu-id="b7520-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Zendesk közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b7520-140">In other words, a link relationship between an Azure AD user and the related user in Zendesk needs to be established.</span></span>

<span data-ttu-id="b7520-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Zendesk.</span><span class="sxs-lookup"><span data-stu-id="b7520-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zendesk.</span></span>

<span data-ttu-id="b7520-142">Az Azure AD egyszeri bejelentkezést a Zendesk tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b7520-142">To configure and test Azure AD single sign-on with Zendesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b7520-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b7520-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b7520-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b7520-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7520-145">**[Zendesk tesztfelhasználó létrehozása](#creating-a-zendesk-test-user)**  - való Britta Simon valami Zendesk, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b7520-145">**[Creating a Zendesk test user](#creating-a-zendesk-test-user)** - to have a counterpart of Britta Simon in Zendesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7520-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b7520-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7520-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b7520-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7520-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7520-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7520-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Zendesk-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b7520-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zendesk application.</span></span>

<span data-ttu-id="b7520-150">**A Zendesk konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7520-150">**To configure Azure AD single sign-on with Zendesk, perform the following steps:**</span></span>

1. <span data-ttu-id="b7520-151">Az Azure portálon a a **Zendesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b7520-151">In the Azure portal, on the **Zendesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b7520-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b7520-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. <span data-ttu-id="b7520-155">Az a **Zendesk-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b7520-155">On the **Zendesk Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    <span data-ttu-id="b7520-157">a.</span><span class="sxs-lookup"><span data-stu-id="b7520-157">a.</span></span> <span data-ttu-id="b7520-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="b7520-158">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<subdomain>.zendesk.com`</span></span>

    <span data-ttu-id="b7520-159">b.</span><span class="sxs-lookup"><span data-stu-id="b7520-159">b.</span></span> <span data-ttu-id="b7520-160">Az a **azonosító** szövegmező, írja be az értéket a következő minta használatával:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="b7520-160">In the **Identifier** textbox, type the value using the following pattern: `https://<subdomain>.zendesk.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b7520-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b7520-161">These values are not real.</span></span> <span data-ttu-id="b7520-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosító URL-t.</span><span class="sxs-lookup"><span data-stu-id="b7520-162">Update these values with the actual Sign-on URL and Identifier URL.</span></span> <span data-ttu-id="b7520-163">Ügyfél [Zendesk támogatási csoport](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b7520-163">Contact [Zendesk support team](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) to get these values.</span></span> 

4. <span data-ttu-id="b7520-164">Ehhez a SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="b7520-164">Zendesk expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="b7520-165">Nem kötelező SAML attribútum, de opcionálisan hozzáadhat egy attribútumot **felhasználói attribútumok** következő szakasz az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b7520-165">There are no mandatory SAML attributes but optionally you can add an attribute from **User Attributes** section by following the below steps:</span></span> 

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    <span data-ttu-id="b7520-167">a.</span><span class="sxs-lookup"><span data-stu-id="b7520-167">a.</span></span> <span data-ttu-id="b7520-168">Kattintson a **megtekintése és szerkesztése az összes többi attribútumával** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b7520-168">Click the **View and edit all the other attributes** check box.</span></span>
     
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    <span data-ttu-id="b7520-170">b.</span><span class="sxs-lookup"><span data-stu-id="b7520-170">b.</span></span> <span data-ttu-id="b7520-171">Kattintson a **attribútum hozzáadása** megnyitásához **Hozzáadás attribútum** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7520-171">Click the **Add Attribute** to open **Add attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="b7520-173">c.</span><span class="sxs-lookup"><span data-stu-id="b7520-173">c.</span></span> <span data-ttu-id="b7520-174">Az a **neve** szövegmező, írja be az attribútumnak a nevét (például **emailaddress**).</span><span class="sxs-lookup"><span data-stu-id="b7520-174">In the **Name** textbox, type the attribute name (for example **emailaddress**).</span></span>
    
    <span data-ttu-id="b7520-175">d.</span><span class="sxs-lookup"><span data-stu-id="b7520-175">d.</span></span> <span data-ttu-id="b7520-176">Az a **érték** listára, válassza ki az attribútum értéke (mint **user.mail**).</span><span class="sxs-lookup"><span data-stu-id="b7520-176">From the **Value** list, select the attribute value (as **user.mail**).</span></span>
    
    <span data-ttu-id="b7520-177">e.</span><span class="sxs-lookup"><span data-stu-id="b7520-177">e.</span></span> <span data-ttu-id="b7520-178">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="b7520-178">Click **Ok**</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="b7520-179">A bővítményattribútumokat használatával ad hozzá az attribútumokat, amelyek nem az alapértelmezés szerint az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b7520-179">You use extension attributes to add attributes that are not in Azure AD by default.</span></span> <span data-ttu-id="b7520-180">Kattintson a [állítható be SAML felhasználói attribútumok](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) teljes listáját a beolvasandó SAML attribútumok, amelyek **Zendesk** fogad el.</span><span class="sxs-lookup"><span data-stu-id="b7520-180">Click [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) to get the complete list of SAML attributes that **Zendesk** accepts.</span></span> 

5. <span data-ttu-id="b7520-181">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="b7520-181">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. <span data-ttu-id="b7520-183">Az a **Zendesk konfigurációs** területén kattintson **konfigurálása Zendesk** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b7520-183">On the **Zendesk Configuration** section, click **Configure Zendesk** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b7520-184">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b7520-184">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. <span data-ttu-id="b7520-186">Egy másik webes böngészőablakban jelentkezzen be a Zendesk vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b7520-186">In a different web browser window, log into your Zendesk company site as an administrator.</span></span>

8. <span data-ttu-id="b7520-187">Kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="b7520-187">Click **Admin**.</span></span>

9. <span data-ttu-id="b7520-188">A bal oldali navigációs ablaktáblán kattintson **beállítások**, és kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="b7520-188">In the left navigation pane, click **Settings**, and then click **Security**.</span></span>

10. <span data-ttu-id="b7520-189">Az a **biztonsági** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b7520-189">On the **Security** page, perform the following steps:</span></span> 
   
     <span data-ttu-id="b7520-190">![Biztonsági](./media/active-directory-saas-zendesk-tutorial/ic773089.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="b7520-190">![Security](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")</span></span>

    <span data-ttu-id="b7520-191">![Egyszeri bejelentkezés](./media/active-directory-saas-zendesk-tutorial/ic773090.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="b7520-191">![Single sign-on](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single sign-on")</span></span>

     <span data-ttu-id="b7520-192">a.</span><span class="sxs-lookup"><span data-stu-id="b7520-192">a.</span></span> <span data-ttu-id="b7520-193">Kattintson a **Admin & ügynökök** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7520-193">Click the **Admin & Agents** tab.</span></span>

     <span data-ttu-id="b7520-194">b.</span><span class="sxs-lookup"><span data-stu-id="b7520-194">b.</span></span> <span data-ttu-id="b7520-195">Válassza ki **egyszeri bejelentkezés (SSO) és a SAML**, majd válassza ki **SAML**.</span><span class="sxs-lookup"><span data-stu-id="b7520-195">Select **Single sign-on (SSO) and SAML**, and then select **SAML**.</span></span>

     <span data-ttu-id="b7520-196">c.</span><span class="sxs-lookup"><span data-stu-id="b7520-196">c.</span></span> <span data-ttu-id="b7520-197">A **SAML SSO URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b7520-197">In **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

     <span data-ttu-id="b7520-198">d.</span><span class="sxs-lookup"><span data-stu-id="b7520-198">d.</span></span> <span data-ttu-id="b7520-199">A **távoli kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b7520-199">In **Remote Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
        
     <span data-ttu-id="b7520-200">e.</span><span class="sxs-lookup"><span data-stu-id="b7520-200">e.</span></span> <span data-ttu-id="b7520-201">A **tanúsítvány-ujjlenyomat** szövegmező, illessze be a **ujjlenyomat** érték tanúsítvány, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b7520-201">In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="b7520-202">f.</span><span class="sxs-lookup"><span data-stu-id="b7520-202">f.</span></span> <span data-ttu-id="b7520-203">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7520-203">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7520-204">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7520-204">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7520-205">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b7520-205">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b7520-207">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7520-207">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b7520-208">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b7520-208">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7520-210">Megjelenítendő felhasználót tartalmazó listát Ugrás **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b7520-210">To display the list of users go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7520-212">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7520-212">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7520-214">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b7520-214">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7520-216">a.</span><span class="sxs-lookup"><span data-stu-id="b7520-216">a.</span></span> <span data-ttu-id="b7520-217">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7520-217">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7520-218">b.</span><span class="sxs-lookup"><span data-stu-id="b7520-218">b.</span></span> <span data-ttu-id="b7520-219">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7520-219">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7520-220">c.</span><span class="sxs-lookup"><span data-stu-id="b7520-220">c.</span></span> <span data-ttu-id="b7520-221">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b7520-221">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b7520-222">d.</span><span class="sxs-lookup"><span data-stu-id="b7520-222">d.</span></span> <span data-ttu-id="b7520-223">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b7520-223">Click **Create**.</span></span> 

### <a name="creating-a-zendesk-test-user"></a><span data-ttu-id="b7520-224">Zendesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7520-224">Creating a Zendesk test user</span></span>

<span data-ttu-id="b7520-225">Ahhoz, hogy be tudjon jelentkezni az Azure AD-felhasználók **Zendesk**, akkor ki kell építenie a **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="b7520-225">To enable Azure AD users to log into **Zendesk**, they must be provisioned into **Zendesk**.</span></span>  
<span data-ttu-id="b7520-226">Attól függően, hogy az alkalmazások szerepkörrel a normális működés:</span><span class="sxs-lookup"><span data-stu-id="b7520-226">Depending on the role assigned in the apps, it's the expected behavior:</span></span>

 1. <span data-ttu-id="b7520-227">**Végfelhasználói** fiókok bejelentkezéskor automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="b7520-227">**End-user** accounts are automatically provisioned when signing in.</span></span>
 2. <span data-ttu-id="b7520-228">**Ügynök** és **Admin** fiókokat kell manuálisan építhető **Zendesk** bejelentkezés előtt.</span><span class="sxs-lookup"><span data-stu-id="b7520-228">**Agent** and **Admin** accounts need to be manually provisioned in **Zendesk** before signing in.</span></span>
 
<span data-ttu-id="b7520-229">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7520-229">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="b7520-230">Jelentkezzen be a **Zendesk** bérlő.</span><span class="sxs-lookup"><span data-stu-id="b7520-230">Log in to your **Zendesk** tenant.</span></span>

2. <span data-ttu-id="b7520-231">Válassza ki a **Ügyféllista** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7520-231">Select the **Customer List** tab.</span></span>

3. <span data-ttu-id="b7520-232">Válassza ki a **felhasználói** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b7520-232">Select the **User** tab, and click **Add**.</span></span>
   
    <span data-ttu-id="b7520-233">![Felhasználó hozzáadása](./media/active-directory-saas-zendesk-tutorial/ic773632.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b7520-233">![Add user](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Add user")</span></span>
4. <span data-ttu-id="b7520-234">Írja be az e-mail cím egy meglévő Azure AD-fiókot rendelkezés szeretne, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b7520-234">Type the email address of an existing Azure AD account you want to provision, and then click **Save**.</span></span>
   
    <span data-ttu-id="b7520-235">![Új felhasználó](./media/active-directory-saas-zendesk-tutorial/ic773633.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="b7520-235">![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")</span></span>

> [!NOTE]
> <span data-ttu-id="b7520-236">Bármely más Zendesk felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz Zendesk által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="b7520-236">You can use any other Zendesk user account creation tools or APIs provided by Zendesk to provision AAD user accounts.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b7520-237">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b7520-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b7520-238">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Zendesk Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b7520-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zendesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b7520-240">**Zendesk Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b7520-240">**To assign Britta Simon to Zendesk, perform the following steps:**</span></span>

1. <span data-ttu-id="b7520-241">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b7520-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b7520-243">Az alkalmazások listában válassza ki a **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="b7520-243">In the applications list, select **Zendesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. <span data-ttu-id="b7520-245">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b7520-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b7520-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7520-247">Click **Add** button.</span></span> <span data-ttu-id="b7520-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7520-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b7520-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b7520-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b7520-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7520-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7520-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b7520-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7520-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b7520-253">Testing single sign-on</span></span>

<span data-ttu-id="b7520-254">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b7520-254">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b7520-255">Ha a hozzáférési panelen Zendesk csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Zendesk-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="b7520-255">When you click the Zendesk tile in the Access Panel, you should get automatically signed-on to your Zendesk application.</span></span>
<span data-ttu-id="b7520-256">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b7520-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7520-257">További források</span><span class="sxs-lookup"><span data-stu-id="b7520-257">Additional resources</span></span>

* [<span data-ttu-id="b7520-258">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b7520-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7520-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b7520-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
