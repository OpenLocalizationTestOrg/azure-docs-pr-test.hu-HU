---
title: "Oktatóanyag: Azure Active Directoryval integrált NetDocuments |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és NetDocuments között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 87c3338d611daa837aa5f079c4b68e0e6fc58455
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a><span data-ttu-id="c9987-103">Oktatóanyag: Azure Active Directoryval integrált NetDocuments</span><span class="sxs-lookup"><span data-stu-id="c9987-103">Tutorial: Azure Active Directory integration with NetDocuments</span></span>

<span data-ttu-id="c9987-104">Ebben az oktatóanyagban elsajátíthatja NetDocuments integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c9987-104">In this tutorial, you learn how to integrate NetDocuments with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9987-105">NetDocuments integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c9987-105">Integrating NetDocuments with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c9987-106">Megadhatja a NetDocuments hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c9987-106">You can control in Azure AD who has access to NetDocuments</span></span>
- <span data-ttu-id="c9987-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett NetDocuments (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c9987-107">You can enable your users to automatically get signed-on to NetDocuments (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9987-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c9987-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c9987-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9987-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9987-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c9987-110">Prerequisites</span></span>

<span data-ttu-id="c9987-111">Konfigurálása az Azure AD-integrációs NetDocuments, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c9987-111">To configure Azure AD integration with NetDocuments, you need the following items:</span></span>

- <span data-ttu-id="c9987-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c9987-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9987-113">Egy NetDocuments egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c9987-113">A NetDocuments single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9987-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c9987-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9987-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c9987-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9987-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9987-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9987-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9987-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9987-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c9987-118">Scenario description</span></span>
<span data-ttu-id="c9987-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c9987-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9987-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c9987-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9987-121">A gyűjteményből NetDocuments hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9987-121">Adding NetDocuments from the gallery</span></span>
2. <span data-ttu-id="c9987-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c9987-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netdocuments-from-the-gallery"></a><span data-ttu-id="c9987-123">A gyűjteményből NetDocuments hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9987-123">Adding NetDocuments from the gallery</span></span>
<span data-ttu-id="c9987-124">Az Azure AD integrálása a NetDocuments konfigurálásához kell hozzáadnia NetDocuments a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c9987-124">To configure the integration of NetDocuments into Azure AD, you need to add NetDocuments from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c9987-125">**A gyűjteményből NetDocuments hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9987-125">**To add NetDocuments from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c9987-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c9987-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c9987-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c9987-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c9987-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9987-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c9987-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c9987-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c9987-133">Írja be a keresőmezőbe, **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="c9987-133">In the search box, type **NetDocuments**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. <span data-ttu-id="c9987-135">Az eredmények panelen válassza ki a **NetDocuments**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c9987-135">In the results panel, select **NetDocuments**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c9987-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c9987-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c9987-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="c9987-138">In this section, you configure and test Azure AD single sign-on with NetDocuments based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9987-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó NetDocuments a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c9987-139">For single sign-on to work, Azure AD needs to know what the counterpart user in NetDocuments is to a user in Azure AD.</span></span> <span data-ttu-id="c9987-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a NetDocuments közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c9987-140">In other words, a link relationship between an Azure AD user and the related user in NetDocuments needs to be established.</span></span>

<span data-ttu-id="c9987-141">NetDocuments, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c9987-141">In NetDocuments, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c9987-142">Az Azure AD egyszeri bejelentkezést a NetDocuments tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c9987-142">To configure and test Azure AD single sign-on with NetDocuments, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c9987-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c9987-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c9987-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c9987-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9987-145">**[NetDocuments tesztfelhasználó létrehozása](#creating-a-netdocuments-test-user)**  - való Britta Simon valami NetDocuments, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c9987-145">**[Creating a NetDocuments test user](#creating-a-netdocuments-test-user)** - to have a counterpart of Britta Simon in NetDocuments that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9987-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9987-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9987-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c9987-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c9987-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9987-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c9987-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az NetDocuments alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c9987-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your NetDocuments application.</span></span>

<span data-ttu-id="c9987-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés NetDocuments, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9987-150">**To configure Azure AD single sign-on with NetDocuments, perform the following steps:**</span></span>

1. <span data-ttu-id="c9987-151">Az Azure portálon a a **NetDocuments** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9987-151">In the Azure portal, on the **NetDocuments** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c9987-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9987-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. <span data-ttu-id="c9987-155">Az a **NetDocuments tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c9987-155">On the **NetDocuments Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    <span data-ttu-id="c9987-157">a.</span><span class="sxs-lookup"><span data-stu-id="c9987-157">a.</span></span> <span data-ttu-id="c9987-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="c9987-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    <span data-ttu-id="c9987-159">b.</span><span class="sxs-lookup"><span data-stu-id="c9987-159">b.</span></span> <span data-ttu-id="c9987-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="c9987-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c9987-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c9987-161">These values are not real.</span></span> <span data-ttu-id="c9987-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="c9987-162">Update these values with the actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="c9987-163">Ügyfél [NetDocuments támogatási csoport](https://support.netdocuments.com/hc/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c9987-163">Contact [NetDocuments support team](https://support.netdocuments.com/hc/) to get these values.</span></span>
 
4. <span data-ttu-id="c9987-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c9987-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. <span data-ttu-id="c9987-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9987-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c9987-168">Egy másik webes böngészőablakban jelentkezzen be a NetDocuments vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c9987-168">In a different web browser window, log into your NetDocuments company site as an administrator.</span></span>

7. <span data-ttu-id="c9987-169">Ugrás a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="c9987-169">Go to **Admin**.</span></span>

8. <span data-ttu-id="c9987-170">Kattintson a **Hozzáadás és eltávolítás felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c9987-170">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="c9987-171">![Tárház](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "tárház")</span><span class="sxs-lookup"><span data-stu-id="c9987-171">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

9. <span data-ttu-id="c9987-172">Kattintson a **speciális hitelesítési beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="c9987-172">Click **Configure advanced authentication options**.</span></span>
    
    <span data-ttu-id="c9987-173">![Speciális hitelesítési beállítások konfigurálása](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "speciális hitelesítési beállítások konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="c9987-173">![Configure advanced authentication options](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "Configure advanced authentication options")</span></span>

10. <span data-ttu-id="c9987-174">Az a **összevont identitási** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c9987-174">On the **Federated Identity** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="c9987-175">![Összevont Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Identitty összevont")</span><span class="sxs-lookup"><span data-stu-id="c9987-175">![Federated Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Federated Identitty")</span></span>
   
    <span data-ttu-id="c9987-176">a.</span><span class="sxs-lookup"><span data-stu-id="c9987-176">a.</span></span> <span data-ttu-id="c9987-177">Mint **összevont identitás kiszolgálótípus**, jelölje be **Active Directory összevonási szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="c9987-177">As **Federated identity server type**, select **Active Directory Federation Services**.</span></span>
   
    <span data-ttu-id="c9987-178">b.</span><span class="sxs-lookup"><span data-stu-id="c9987-178">b.</span></span> <span data-ttu-id="c9987-179">Kattintson a **fájl kiválasztása**, a letöltött metaadatait tartalmazó fájl, amely az Azure-portálról letöltött feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="c9987-179">Click **Choose file**, to upload the downloaded metadata file which you have downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="c9987-180">c.</span><span class="sxs-lookup"><span data-stu-id="c9987-180">c.</span></span> <span data-ttu-id="c9987-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9987-181">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="c9987-182">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c9987-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c9987-183">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c9987-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c9987-184">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9987-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c9987-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9987-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="c9987-186">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c9987-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c9987-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9987-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c9987-189">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c9987-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9987-191">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c9987-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9987-193">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c9987-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9987-195">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c9987-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9987-197">a.</span><span class="sxs-lookup"><span data-stu-id="c9987-197">a.</span></span> <span data-ttu-id="c9987-198">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9987-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9987-199">b.</span><span class="sxs-lookup"><span data-stu-id="c9987-199">b.</span></span> <span data-ttu-id="c9987-200">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9987-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9987-201">c.</span><span class="sxs-lookup"><span data-stu-id="c9987-201">c.</span></span> <span data-ttu-id="c9987-202">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c9987-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c9987-203">d.</span><span class="sxs-lookup"><span data-stu-id="c9987-203">d.</span></span> <span data-ttu-id="c9987-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c9987-204">Click **Create**.</span></span>
 
### <a name="creating-a-netdocuments-test-user"></a><span data-ttu-id="c9987-205">NetDocuments tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9987-205">Creating a NetDocuments test user</span></span>

<span data-ttu-id="c9987-206">Ahhoz, hogy az Azure AD-felhasználók NetDocuments bejelentkezni, akkor ki kell építenie a NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="c9987-206">To enable Azure AD users to log in to NetDocuments, they must be provisioned into NetDocuments.</span></span>  
<span data-ttu-id="c9987-207">NetDocuments, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c9987-207">In the case of NetDocuments, provisioning is a manual task.</span></span>

<span data-ttu-id="c9987-208">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9987-208">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="c9987-209">A Sign a **NetDocuments** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c9987-209">Sing on to your **NetDocuments** company site as administrator.</span></span>

2. <span data-ttu-id="c9987-210">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="c9987-210">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="c9987-211">![Felügyeleti](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="c9987-211">![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")</span></span>

3. <span data-ttu-id="c9987-212">Kattintson a **Hozzáadás és eltávolítás felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c9987-212">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="c9987-213">![Tárház](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "tárház")</span><span class="sxs-lookup"><span data-stu-id="c9987-213">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

4. <span data-ttu-id="c9987-214">Az a **E-mail cím** szövegmező, írjon be egy érvényes Azure Active Directory-fiók rendelkezés szeretne, és kattintson az e-mail címet **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c9987-214">In the **Email Address** textbox, type the email address of a valid Azure Active Directory account you want to provision, and then click **Add User**.</span></span>
   
    <span data-ttu-id="c9987-215">![E-mail cím](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "E-mail cím")</span><span class="sxs-lookup"><span data-stu-id="c9987-215">![Email Address](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "Email Address")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="c9987-216">Az Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmaz egy hivatkozást a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="c9987-216">The Azure Active Directory account holder will get an email that includes a link to confirm the account before it becomes active.</span></span> <span data-ttu-id="c9987-217">Bármely más NetDocuments felhasználói fiók létrehozása eszközök vagy API-k által biztosított NetDocuments kiépítését Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="c9987-217">You can use any other NetDocuments user account creation tools or APIs provided by NetDocuments to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c9987-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c9987-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c9987-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés NetDocuments Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c9987-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to NetDocuments.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c9987-221">**Britta Simon hozzárendelése NetDocuments, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c9987-221">**To assign Britta Simon to NetDocuments, perform the following steps:**</span></span>

1. <span data-ttu-id="c9987-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9987-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c9987-224">Az alkalmazások listában válassza ki a **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="c9987-224">In the applications list, select **NetDocuments**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. <span data-ttu-id="c9987-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c9987-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c9987-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9987-228">Click **Add** button.</span></span> <span data-ttu-id="c9987-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9987-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c9987-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c9987-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c9987-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9987-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9987-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c9987-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c9987-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c9987-234">Testing single sign-on</span></span>

<span data-ttu-id="c9987-235">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c9987-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c9987-236">Ha a hozzáférési panelen NetDocuments csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az NetDocuments alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c9987-236">When you click the NetDocuments tile in the Access Panel, you should get automatically signed-on to your NetDocuments application.</span></span>
<span data-ttu-id="c9987-237">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c9987-237">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9987-238">További források</span><span class="sxs-lookup"><span data-stu-id="c9987-238">Additional resources</span></span>

* [<span data-ttu-id="c9987-239">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c9987-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9987-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c9987-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

