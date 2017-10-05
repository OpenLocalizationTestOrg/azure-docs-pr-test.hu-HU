---
title: "Oktatóanyag: Azure Active Directoryval integrált IdeaScale |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és IdeaScale között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 88099e942319f16dd721da83e4e69b8fcb836c0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a><span data-ttu-id="1d827-103">Oktatóanyag: Azure Active Directoryval integrált IdeaScale</span><span class="sxs-lookup"><span data-stu-id="1d827-103">Tutorial: Azure Active Directory integration with IdeaScale</span></span>

<span data-ttu-id="1d827-104">Ebben az oktatóanyagban elsajátíthatja IdeaScale integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1d827-104">In this tutorial, you learn how to integrate IdeaScale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1d827-105">IdeaScale integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1d827-105">Integrating IdeaScale with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1d827-106">Megadhatja a IdeaScale hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1d827-106">You can control in Azure AD who has access to IdeaScale</span></span>
- <span data-ttu-id="1d827-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett IdeaScale (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1d827-107">You can enable your users to automatically get signed-on to IdeaScale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1d827-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1d827-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1d827-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1d827-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d827-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d827-110">Prerequisites</span></span>

<span data-ttu-id="1d827-111">Konfigurálása az Azure AD-integrációs IdeaScale, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1d827-111">To configure Azure AD integration with IdeaScale, you need the following items:</span></span>

- <span data-ttu-id="1d827-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1d827-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1d827-113">Egy IdeaScale egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1d827-113">An IdeaScale single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1d827-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1d827-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1d827-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1d827-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1d827-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1d827-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1d827-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d827-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1d827-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1d827-118">Scenario description</span></span>
<span data-ttu-id="1d827-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1d827-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1d827-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1d827-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1d827-121">A gyűjteményből IdeaScale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d827-121">Adding IdeaScale from the gallery</span></span>
2. <span data-ttu-id="1d827-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1d827-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ideascale-from-the-gallery"></a><span data-ttu-id="1d827-123">A gyűjteményből IdeaScale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d827-123">Adding IdeaScale from the gallery</span></span>
<span data-ttu-id="1d827-124">Az Azure AD integrálása a IdeaScale konfigurálásához kell hozzáadnia IdeaScale a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1d827-124">To configure the integration of IdeaScale into Azure AD, you need to add IdeaScale from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1d827-125">**A gyűjteményből IdeaScale hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d827-125">**To add IdeaScale from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1d827-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1d827-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1d827-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1d827-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1d827-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1d827-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1d827-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1d827-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1d827-133">Írja be a keresőmezőbe, **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="1d827-133">In the search box, type **IdeaScale**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. <span data-ttu-id="1d827-135">Az eredmények panelen válassza ki a **IdeaScale**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1d827-135">In the results panel, select **IdeaScale**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1d827-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1d827-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1d827-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján IdeaScale</span><span class="sxs-lookup"><span data-stu-id="1d827-138">In this section, you configure and test Azure AD single sign-on with IdeaScale based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1d827-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó IdeaScale a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1d827-139">For single sign-on to work, Azure AD needs to know what the counterpart user in IdeaScale is to a user in Azure AD.</span></span> <span data-ttu-id="1d827-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a IdeaScale közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1d827-140">In other words, a link relationship between an Azure AD user and the related user in IdeaScale needs to be established.</span></span>

<span data-ttu-id="1d827-141">IdeaScale, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1d827-141">In IdeaScale, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1d827-142">Az Azure AD egyszeri bejelentkezést a IdeaScale tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1d827-142">To configure and test Azure AD single sign-on with IdeaScale, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1d827-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1d827-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1d827-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1d827-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1d827-145">**[Egy IdeaScale tesztfelhasználó létrehozása](#creating-an-ideascale-test-user)**  - való Britta Simon valami IdeaScale, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d827-145">**[Creating an IdeaScale test user](#creating-an-ideascale-test-user)** - to have a counterpart of Britta Simon in IdeaScale that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1d827-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1d827-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1d827-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1d827-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1d827-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d827-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1d827-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az IdeaScale alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1d827-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your IdeaScale application.</span></span>

<span data-ttu-id="1d827-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés IdeaScale, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d827-150">**To configure Azure AD single sign-on with IdeaScale, perform the following steps:**</span></span>

1. <span data-ttu-id="1d827-151">Az Azure portálon a a **IdeaScale** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1d827-151">In the Azure portal, on the **IdeaScale** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1d827-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1d827-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. <span data-ttu-id="1d827-155">Az a **IdeaScale tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1d827-155">On the **IdeaScale Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    <span data-ttu-id="1d827-157">a.</span><span class="sxs-lookup"><span data-stu-id="1d827-157">a.</span></span> <span data-ttu-id="1d827-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.ideascale.com`</span><span class="sxs-lookup"><span data-stu-id="1d827-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.ideascale.com`</span></span>

    <span data-ttu-id="1d827-159">b.</span><span class="sxs-lookup"><span data-stu-id="1d827-159">b.</span></span> <span data-ttu-id="1d827-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="1d827-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > <span data-ttu-id="1d827-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="1d827-161">These values are not real.</span></span> <span data-ttu-id="1d827-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1d827-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1d827-163">Ügyfél [IdeaScale ügyfél-támogatási csoport](http://support.ideascale.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1d827-163">Contact [IdeaScale Client support team](http://support.ideascale.com/) to get these values.</span></span> 
 
4. <span data-ttu-id="1d827-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1d827-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. <span data-ttu-id="1d827-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d827-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1d827-168">A a **IdeaScale konfigurációs** kattintson **konfigurálása IdeaScale** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1d827-168">On the **IdeaScale Configuration** section, click **Configure IdeaScale** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1d827-169">Másolás a **Sign-Out URL-címet, és a SAML Entitásazonosító** a a **rövid összefoglaló szakasz**.</span><span class="sxs-lookup"><span data-stu-id="1d827-169">Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. <span data-ttu-id="1d827-171">Egy másik webes böngészőablakban jelentkezzen be a IdeaScale vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1d827-171">In a different web browser window, log in to your IdeaScale company site as an administrator.</span></span>

8. <span data-ttu-id="1d827-172">Ugrás a **közösségi beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1d827-172">Go to **Community Settings**.</span></span>
   
    <span data-ttu-id="1d827-173">![Közösségi beállítások](./media/active-directory-saas-ideascale-tutorial/ic790847.png "közösségi beállításai")</span><span class="sxs-lookup"><span data-stu-id="1d827-173">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

9. <span data-ttu-id="1d827-174">Ugrás a **biztonsági \> az egyszeri bejelentkezés beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1d827-174">Go to **Security \> Single Signon Settings**.</span></span>
   
    <span data-ttu-id="1d827-175">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-ideascale-tutorial/ic790848.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="1d827-175">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790848.png "Single Signon Settings")</span></span>

10. <span data-ttu-id="1d827-176">Mint **egyszeri-bejelentkezés típusa**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="1d827-176">As **Single-Signon Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="1d827-177">![Az egyszeri bejelentkezés típusa](./media/active-directory-saas-ideascale-tutorial/ic790849.png "az egyszeri bejelentkezés típusa")</span><span class="sxs-lookup"><span data-stu-id="1d827-177">![Single Signon Type](./media/active-directory-saas-ideascale-tutorial/ic790849.png "Single Signon Type")</span></span>

11. <span data-ttu-id="1d827-178">Az a **egyszeri bejelentkezés beállítások** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1d827-178">On the **Single Signon Settings** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="1d827-179">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-ideascale-tutorial/ic790850.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="1d827-179">![Single Signon Settings](./media/active-directory-saas-ideascale-tutorial/ic790850.png "Single Signon Settings")</span></span>
   
    <span data-ttu-id="1d827-180">a.</span><span class="sxs-lookup"><span data-stu-id="1d827-180">a.</span></span> <span data-ttu-id="1d827-181">A **SAML IdP Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1d827-181">In **SAML IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1d827-182">b.</span><span class="sxs-lookup"><span data-stu-id="1d827-182">b.</span></span> <span data-ttu-id="1d827-183">Másolja a letöltött metaadatfájl Azure-portálon, és illessze be azt a **SAML IdP metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1d827-183">Copy the content of your downloaded metadata file from Azure portal, and paste it into the **SAML IdP Metadata** textbox.</span></span>

    <span data-ttu-id="1d827-184">c.</span><span class="sxs-lookup"><span data-stu-id="1d827-184">c.</span></span> <span data-ttu-id="1d827-185">A **kijelentkezési sikeres URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1d827-185">In **Logout Success URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1d827-186">d.</span><span class="sxs-lookup"><span data-stu-id="1d827-186">d.</span></span> <span data-ttu-id="1d827-187">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="1d827-187">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="1d827-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1d827-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1d827-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1d827-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1d827-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1d827-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1d827-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d827-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="1d827-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1d827-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1d827-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d827-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1d827-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1d827-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1d827-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1d827-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1d827-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1d827-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1d827-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1d827-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1d827-203">a.</span><span class="sxs-lookup"><span data-stu-id="1d827-203">a.</span></span> <span data-ttu-id="1d827-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1d827-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1d827-205">b.</span><span class="sxs-lookup"><span data-stu-id="1d827-205">b.</span></span> <span data-ttu-id="1d827-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1d827-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1d827-207">c.</span><span class="sxs-lookup"><span data-stu-id="1d827-207">c.</span></span> <span data-ttu-id="1d827-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1d827-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1d827-209">d.</span><span class="sxs-lookup"><span data-stu-id="1d827-209">d.</span></span> <span data-ttu-id="1d827-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d827-210">Click **Create**.</span></span>
 
### <a name="creating-an-ideascale-test-user"></a><span data-ttu-id="1d827-211">Egy IdeaScale tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d827-211">Creating an IdeaScale test user</span></span>

<span data-ttu-id="1d827-212">Ahhoz, hogy az Azure AD-felhasználók IdeaScale bejelentkezni, akkor ki kell építenie a IdeaScale.</span><span class="sxs-lookup"><span data-stu-id="1d827-212">To enable Azure AD users to log into IdeaScale, they must be provisioned in to IdeaScale.</span></span> <span data-ttu-id="1d827-213">IdeaScale, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1d827-213">In the case of IdeaScale, provisioning is a manual task.</span></span>

<span data-ttu-id="1d827-214">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d827-214">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="1d827-215">Jelentkezzen be a **IdeaScale** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1d827-215">Log in to your **IdeaScale** company site as administrator.</span></span>

2. <span data-ttu-id="1d827-216">Ugrás a **közösségi beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1d827-216">Go to **Community Settings**.</span></span>
   
    <span data-ttu-id="1d827-217">![Közösségi beállítások](./media/active-directory-saas-ideascale-tutorial/ic790847.png "közösségi beállításai")</span><span class="sxs-lookup"><span data-stu-id="1d827-217">![Community Settings](./media/active-directory-saas-ideascale-tutorial/ic790847.png "Community Settings")</span></span>

3. <span data-ttu-id="1d827-218">Ugrás a **alapbeállítások \> tag felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="1d827-218">Go to **Basic Settings \> Member Management**.</span></span>

4. <span data-ttu-id="1d827-219">Kattintson a **felvenni abban az esetben**.</span><span class="sxs-lookup"><span data-stu-id="1d827-219">Click **Add Member**.</span></span>
   
    <span data-ttu-id="1d827-220">![Tag felügyeleti](./media/active-directory-saas-ideascale-tutorial/ic790852.png "tag felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="1d827-220">![Member Management](./media/active-directory-saas-ideascale-tutorial/ic790852.png "Member Management")</span></span>

5. <span data-ttu-id="1d827-221">Az új tag hozzáadása a szakaszban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1d827-221">In the Add New Member section, perform the following steps:</span></span>
   
    <span data-ttu-id="1d827-222">![Adja hozzá az új tag](./media/active-directory-saas-ideascale-tutorial/ic790853.png "új tag hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="1d827-222">![Add New Member](./media/active-directory-saas-ideascale-tutorial/ic790853.png "Add New Member")</span></span>
   
    <span data-ttu-id="1d827-223">a.</span><span class="sxs-lookup"><span data-stu-id="1d827-223">a.</span></span> <span data-ttu-id="1d827-224">Az a **E-mail címek** szövegmező, írja be egy érvényes AAD-fiókba, azt szeretné, hogy építse ki e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="1d827-224">In the **Email Addresses** textbox, type the email address of a valid AAD account you want to provision.</span></span>
   
    <span data-ttu-id="1d827-225">b.</span><span class="sxs-lookup"><span data-stu-id="1d827-225">b.</span></span> <span data-ttu-id="1d827-226">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="1d827-226">Click **Save Changes**.</span></span> 
   
    >[!NOTE]
    ><span data-ttu-id="1d827-227">Az Azure Active Directory fióktulajdonos beolvassa e-mailben található egy hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="1d827-227">The Azure Active Directory account holder gets an email with a link to confirm the account before it becomes active.</span></span>
      
>[!NOTE]
><span data-ttu-id="1d827-228">Bármely más IdeaScale felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz IdeaScale által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="1d827-228">You can use any other IdeaScale user account creation tools or APIs provided by IdeaScale to provision AAD user accounts.</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1d827-229">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1d827-229">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1d827-230">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés IdeaScale Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1d827-230">In this section, you enable Britta Simon to use Azure single sign-on by granting access to IdeaScale.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1d827-232">**Britta Simon hozzárendelése IdeaScale, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d827-232">**To assign Britta Simon to IdeaScale, perform the following steps:**</span></span>

1. <span data-ttu-id="1d827-233">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1d827-233">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1d827-235">Az alkalmazások listában válassza ki a **IdeaScale**.</span><span class="sxs-lookup"><span data-stu-id="1d827-235">In the applications list, select **IdeaScale**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. <span data-ttu-id="1d827-237">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1d827-237">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1d827-239">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d827-239">Click **Add** button.</span></span> <span data-ttu-id="1d827-240">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d827-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1d827-242">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1d827-242">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1d827-243">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d827-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1d827-244">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d827-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1d827-245">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1d827-245">Testing single sign-on</span></span>


<span data-ttu-id="1d827-246">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="1d827-246">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1d827-247">Ha a hozzáférési panelen IdeaScale csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az IdeaScale alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="1d827-247">When you click the IdeaScale tile in the Access Panel, you should get automatically signed-on to your IdeaScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1d827-248">További források</span><span class="sxs-lookup"><span data-stu-id="1d827-248">Additional resources</span></span>

* [<span data-ttu-id="1d827-249">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1d827-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1d827-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1d827-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

