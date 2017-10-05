---
title: "Oktatóanyag: Azure Active Directoryval integrált YouEarnedIt |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és YouEarnedIt között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: c29d218dbca581f102caf8070fa40894e7006e71
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a><span data-ttu-id="84a2b-103">Oktatóanyag: Azure Active Directoryval integrált YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="84a2b-103">Tutorial: Azure Active Directory integration with YouEarnedIt</span></span>

<span data-ttu-id="84a2b-104">Ebben az oktatóanyagban elsajátíthatja YouEarnedIt integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="84a2b-104">In this tutorial, you learn how to integrate YouEarnedIt with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84a2b-105">YouEarnedIt integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="84a2b-105">Integrating YouEarnedIt with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="84a2b-106">Az Azure AD, aki hozzáfér YouEarnedIt szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="84a2b-106">You can control in Azure AD who has access to YouEarnedIt.</span></span>
- <span data-ttu-id="84a2b-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett YouEarnedIt (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="84a2b-107">You can enable your users to automatically get signed-on to YouEarnedIt (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="84a2b-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="84a2b-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="84a2b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84a2b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84a2b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84a2b-110">Prerequisites</span></span>

<span data-ttu-id="84a2b-111">Konfigurálása az Azure AD-integrációs YouEarnedIt, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="84a2b-111">To configure Azure AD integration with YouEarnedIt, you need the following items:</span></span>

- <span data-ttu-id="84a2b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="84a2b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84a2b-113">Egy YouEarnedIt egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="84a2b-113">A YouEarnedIt single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84a2b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="84a2b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84a2b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="84a2b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84a2b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="84a2b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84a2b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84a2b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84a2b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="84a2b-118">Scenario description</span></span>
<span data-ttu-id="84a2b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="84a2b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84a2b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="84a2b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84a2b-121">A gyűjteményből YouEarnedIt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84a2b-121">Adding YouEarnedIt from the gallery</span></span>
2. <span data-ttu-id="84a2b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="84a2b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-youearnedit-from-the-gallery"></a><span data-ttu-id="84a2b-123">A gyűjteményből YouEarnedIt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84a2b-123">Adding YouEarnedIt from the gallery</span></span>
<span data-ttu-id="84a2b-124">Az Azure AD integrálása a YouEarnedIt konfigurálásához kell hozzáadnia YouEarnedIt a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="84a2b-124">To configure the integration of YouEarnedIt into Azure AD, you need to add YouEarnedIt from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="84a2b-125">**A gyűjteményből YouEarnedIt hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84a2b-125">**To add YouEarnedIt from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="84a2b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="84a2b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="84a2b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="84a2b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="84a2b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="84a2b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="84a2b-133">Írja be a keresőmezőbe, **YouEarnedt**, jelölje be **YouEarnedt** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84a2b-133">In the search box, type **YouEarnedt**, select  **YouEarnedt**  from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában YouEarnedIt](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="84a2b-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84a2b-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="84a2b-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="84a2b-136">In this section, you configure and test Azure AD single sign-on with YouEarnedIt based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="84a2b-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó YouEarnedIt a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="84a2b-137">For single sign-on to work, Azure AD needs to know what the counterpart user in YouEarnedIt is to a user in Azure AD.</span></span> <span data-ttu-id="84a2b-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a YouEarnedIt közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="84a2b-138">In other words, a link relationship between an Azure AD user and the related user in YouEarnedIt needs to be established.</span></span>

<span data-ttu-id="84a2b-139">YouEarnedIt, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="84a2b-139">In YouEarnedIt, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="84a2b-140">Az Azure AD egyszeri bejelentkezést a YouEarnedIt tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="84a2b-140">To configure and test Azure AD single sign-on with YouEarnedIt, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="84a2b-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="84a2b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="84a2b-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="84a2b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84a2b-143">**[YouEarnedIt tesztfelhasználó létrehozása](#create-a-youearnedit-test-user)**  - való Britta Simon valami YouEarnedIt, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="84a2b-143">**[Create a YouEarnedIt test user](#create-a-youearnedit-test-user)** - to have a counterpart of Britta Simon in YouEarnedIt that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="84a2b-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84a2b-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84a2b-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="84a2b-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="84a2b-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84a2b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="84a2b-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az YouEarnedIt alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84a2b-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your YouEarnedIt application.</span></span>

<span data-ttu-id="84a2b-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés YouEarnedIt, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84a2b-148">**To configure Azure AD single sign-on with YouEarnedIt, perform the following steps:**</span></span>

1. <span data-ttu-id="84a2b-149">Az Azure portálon a a **YouEarnedIt** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-149">In the Azure portal, on the **YouEarnedIt** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="84a2b-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84a2b-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. <span data-ttu-id="84a2b-153">Az a **YouEarnedIt tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="84a2b-153">On the **YouEarnedIt Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk YouEarnedIt tartomány és az URL-címek](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_url.png)

    <span data-ttu-id="84a2b-155">a.</span><span class="sxs-lookup"><span data-stu-id="84a2b-155">a.</span></span> <span data-ttu-id="84a2b-156">Az a **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="84a2b-156">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    | <span data-ttu-id="84a2b-157">Környezet</span><span class="sxs-lookup"><span data-stu-id="84a2b-157">Environment</span></span>  | <span data-ttu-id="84a2b-158">Minta</span><span class="sxs-lookup"><span data-stu-id="84a2b-158">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="84a2b-159">Éles</span><span class="sxs-lookup"><span data-stu-id="84a2b-159">Production</span></span> | `https://<company name>.youearnedit.com/users/sign_in` |
    | <span data-ttu-id="84a2b-160">A védőfal</span><span class="sxs-lookup"><span data-stu-id="84a2b-160">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    <span data-ttu-id="84a2b-161">b.</span><span class="sxs-lookup"><span data-stu-id="84a2b-161">b.</span></span> <span data-ttu-id="84a2b-162">Az a **azonosító** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="84a2b-162">In the **Identifier** textbox, type a URL using the following patterns:</span></span>
    | <span data-ttu-id="84a2b-163">Környezet</span><span class="sxs-lookup"><span data-stu-id="84a2b-163">Environment</span></span>  | <span data-ttu-id="84a2b-164">Minta</span><span class="sxs-lookup"><span data-stu-id="84a2b-164">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="84a2b-165">Éles</span><span class="sxs-lookup"><span data-stu-id="84a2b-165">Production</span></span> | `https://<company name>.youearnedit.com` |
    | <span data-ttu-id="84a2b-166">A védőfal</span><span class="sxs-lookup"><span data-stu-id="84a2b-166">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > <span data-ttu-id="84a2b-167">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="84a2b-167">These values are not real.</span></span> <span data-ttu-id="84a2b-168">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="84a2b-168">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="84a2b-169">Ügyfél [YouEarnedIt ügyfél-támogatási csoport](https://youearnedit.freshdesk.com/support/tickets/new) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="84a2b-169">Contact [YouEarnedIt Client support team](https://youearnedit.freshdesk.com/support/tickets/new) to get these values.</span></span> 
 
4. <span data-ttu-id="84a2b-170">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="84a2b-170">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. <span data-ttu-id="84a2b-172">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="84a2b-172">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-youearnedit-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="84a2b-174">A a **YouEarnedIt konfigurációs** kattintson **konfigurálása YouEarnedIt** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="84a2b-174">On the **YouEarnedIt Configuration** section, click **Configure YouEarnedIt** to open **Configure sign-on** window.</span></span> <span data-ttu-id="84a2b-175">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="84a2b-175">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![YouEarnedIt konfiguráció](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. <span data-ttu-id="84a2b-177">Egyszeri bejelentkezés konfigurálása **YouEarnedIt** oldalon kell küldeniük a letöltött **Certificate(Base64)** és **SAML-alapú egyszeri bejelentkezési URL-címe** való [YouEarnedIt támogatási csoport](https://youearnedit.freshdesk.com/support/tickets/new).</span><span class="sxs-lookup"><span data-stu-id="84a2b-177">To configure single sign-on on **YouEarnedIt** side, you need to send the downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** to [YouEarnedIt support team](https://youearnedit.freshdesk.com/support/tickets/new).</span></span> <span data-ttu-id="84a2b-178">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="84a2b-178">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="84a2b-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="84a2b-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="84a2b-180">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="84a2b-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="84a2b-181">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84a2b-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="84a2b-182">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="84a2b-182">Create an Azure AD test user</span></span>

<span data-ttu-id="84a2b-183">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="84a2b-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="84a2b-185">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84a2b-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="84a2b-186">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="84a2b-186">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="84a2b-188">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-188">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="84a2b-190">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="84a2b-190">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="84a2b-192">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84a2b-192">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="84a2b-194">a.</span><span class="sxs-lookup"><span data-stu-id="84a2b-194">a.</span></span> <span data-ttu-id="84a2b-195">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-195">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84a2b-196">b.</span><span class="sxs-lookup"><span data-stu-id="84a2b-196">b.</span></span> <span data-ttu-id="84a2b-197">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84a2b-197">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="84a2b-198">c.</span><span class="sxs-lookup"><span data-stu-id="84a2b-198">c.</span></span> <span data-ttu-id="84a2b-199">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="84a2b-199">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="84a2b-200">d.</span><span class="sxs-lookup"><span data-stu-id="84a2b-200">d.</span></span> <span data-ttu-id="84a2b-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84a2b-201">Click **Create**.</span></span>
 
### <a name="create-a-youearnedit-test-user"></a><span data-ttu-id="84a2b-202">YouEarnedIt tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a2b-202">Create a YouEarnedIt test user</span></span>

<span data-ttu-id="84a2b-203">Ebben a szakaszban egy YouEarnedIt Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="84a2b-203">In this section, you create a user called Britta Simon in YouEarnedIt.</span></span> <span data-ttu-id="84a2b-204">A felhasználók hozzáadása a YouEarnedIt platform YouEarnedIt támogatási csapattal működik.</span><span class="sxs-lookup"><span data-stu-id="84a2b-204">Please work with YouEarnedIt support team to add the users in the YouEarnedIt platform.</span></span>

>[!NOTE]
><span data-ttu-id="84a2b-205">YouEarnedIt az identitásszolgáltató fogja tartalmazni az e-mail cím vagy felhasználónév a NameID attribútumot várt.</span><span class="sxs-lookup"><span data-stu-id="84a2b-205">YouEarnedIt expect the Identity Provider to supply an EmailAddress  or UserName in the NameID attribute.</span></span> <span data-ttu-id="84a2b-206">Ha egy megfelelő felhasználónév vagy e-mail cím nem található az adatbázisban, vagy nem felel meg pontosan a hitelesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="84a2b-206">Authentication will fail if a corresponding UserName or EmailAddress is not found within the database or does not match exactly.</span></span> <span data-ttu-id="84a2b-207">Ehhez szükséges, hogy a fiókok a YouEarnedIt rendszert, mielőtt az SSO-integráció (általában vagy API-t vagy a fürt megosztott kötetei szolgáltatás importálási keresztül) importálhatók.</span><span class="sxs-lookup"><span data-stu-id="84a2b-207">This will require that accounts be imported into the YouEarnedIt system before the SSO integration (Typically either via API or CSV import).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="84a2b-208">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="84a2b-208">Assign the Azure AD test user</span></span>

<span data-ttu-id="84a2b-209">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés YouEarnedIt Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="84a2b-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to YouEarnedIt.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="84a2b-211">**Britta Simon hozzárendelése YouEarnedIt, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="84a2b-211">**To assign Britta Simon to YouEarnedIt, perform the following steps:**</span></span>

1. <span data-ttu-id="84a2b-212">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="84a2b-214">Az alkalmazások listában válassza ki a **YouEarnedIt**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-214">In the applications list, select **YouEarnedIt**.</span></span>

    ![Az alkalmazások listáját a YouEarnedIt hivatkozás](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. <span data-ttu-id="84a2b-216">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="84a2b-216">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="84a2b-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="84a2b-218">Click **Add** button.</span></span> <span data-ttu-id="84a2b-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84a2b-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="84a2b-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="84a2b-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="84a2b-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84a2b-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84a2b-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="84a2b-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="84a2b-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="84a2b-224">Test single sign-on</span></span>

<span data-ttu-id="84a2b-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="84a2b-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="84a2b-226">Ha a hozzáférési panelen YouEarnedIt csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az YouEarnedIt alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="84a2b-226">When you click the YouEarnedIt tile in the Access Panel, you should get automatically signed-on to your YouEarnedIt application.</span></span>
<span data-ttu-id="84a2b-227">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84a2b-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="84a2b-228">További források</span><span class="sxs-lookup"><span data-stu-id="84a2b-228">Additional resources</span></span>

* [<span data-ttu-id="84a2b-229">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="84a2b-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84a2b-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="84a2b-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png

