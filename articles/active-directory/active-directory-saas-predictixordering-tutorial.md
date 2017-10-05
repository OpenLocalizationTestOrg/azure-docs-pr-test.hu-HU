---
title: "Oktatóanyag: Azure Active Directoryval integrált Predictix rendezés |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a rendezés Predictix között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2fe2f976-e97f-4368-9695-3e1624409e8b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 8536a741f9b114ac6787c7aefb4c76ec6c4ed83e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a><span data-ttu-id="96c3f-103">Oktatóanyag: Azure Active Directoryval integrált Predictix rendezése</span><span class="sxs-lookup"><span data-stu-id="96c3f-103">Tutorial: Azure Active Directory integration with Predictix Ordering</span></span>

<span data-ttu-id="96c3f-104">Ebben az oktatóanyagban elsajátíthatja Predictix rendelési integrálása Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="96c3f-104">In this tutorial, you learn how to integrate Predictix Ordering with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="96c3f-105">Predictix rendelési integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="96c3f-105">Integrating Predictix Ordering with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="96c3f-106">Az Azure AD, aki hozzáfér Predictix rendelési szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="96c3f-106">You can control in Azure AD who has access to Predictix Ordering.</span></span>
- <span data-ttu-id="96c3f-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Predictix rendezés (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="96c3f-107">You can enable your users to automatically get signed-on to Predictix Ordering (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="96c3f-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="96c3f-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="96c3f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="96c3f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96c3f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="96c3f-110">Prerequisites</span></span>

<span data-ttu-id="96c3f-111">Konfigurálása az Azure AD-integrációs Predictix rendezés, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="96c3f-111">To configure Azure AD integration with Predictix Ordering, you need the following items:</span></span>

- <span data-ttu-id="96c3f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="96c3f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="96c3f-113">Egy Predictix rendelési egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="96c3f-113">A Predictix Ordering single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="96c3f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="96c3f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="96c3f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="96c3f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="96c3f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="96c3f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="96c3f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="96c3f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="96c3f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="96c3f-118">Scenario description</span></span>
<span data-ttu-id="96c3f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="96c3f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="96c3f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="96c3f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="96c3f-121">Predictix rendelési hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="96c3f-121">Adding Predictix Ordering from the gallery</span></span>
2. <span data-ttu-id="96c3f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="96c3f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-ordering-from-the-gallery"></a><span data-ttu-id="96c3f-123">Predictix rendelési hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="96c3f-123">Adding Predictix Ordering from the gallery</span></span>
<span data-ttu-id="96c3f-124">Az Azure AD integrálása a Predictix rendelési konfigurálásához kell hozzáadnia Predictix rendezést a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="96c3f-124">To configure the integration of Predictix Ordering into Azure AD, you need to add Predictix Ordering from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="96c3f-125">**Adja hozzá a Predictix rendezést a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="96c3f-125">**To add Predictix Ordering from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="96c3f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="96c3f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="96c3f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="96c3f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="96c3f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="96c3f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="96c3f-133">Írja be a keresőmezőbe, **Predictix rendelési**, jelölje be **Predictix rendelési** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="96c3f-133">In the search box, type **Predictix Ordering**, select **Predictix Ordering** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában rendelési Predictix](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="96c3f-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96c3f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="96c3f-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Predictix rendelési "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="96c3f-136">In this section, you configure and test Azure AD single sign-on with Predictix Ordering based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="96c3f-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Predictix sorrendben a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="96c3f-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Predictix Ordering is to a user in Azure AD.</span></span> <span data-ttu-id="96c3f-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Predictix sorrendben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="96c3f-138">In other words, a link relationship between an Azure AD user and the related user in Predictix Ordering needs to be established.</span></span>

<span data-ttu-id="96c3f-139">Rendezés Predictix, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="96c3f-139">In Predictix Ordering, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="96c3f-140">Az Azure AD egyszeri bejelentkezést a Predictix rendelési tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="96c3f-140">To configure and test Azure AD single sign-on with Predictix Ordering, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="96c3f-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="96c3f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="96c3f-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="96c3f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="96c3f-143">**[Predictix rendelési tesztfelhasználó létrehozása](#create-a-predictix-ordering-test-user)**  - való egy megfelelője a Britta Simon Predictix rendezés, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="96c3f-143">**[Create a Predictix Ordering test user](#create-a-predictix-ordering-test-user)** - to have a counterpart of Britta Simon in Predictix Ordering that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="96c3f-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="96c3f-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="96c3f-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="96c3f-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="96c3f-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96c3f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="96c3f-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Predictix rendelési alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="96c3f-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Predictix Ordering application.</span></span>

<span data-ttu-id="96c3f-148">**Predictix rendelési konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="96c3f-148">**To configure Azure AD single sign-on with Predictix Ordering, perform the following steps:**</span></span>

1. <span data-ttu-id="96c3f-149">Az Azure portálon a a **Predictix rendelési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-149">In the Azure portal, on the **Predictix Ordering** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="96c3f-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="96c3f-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_samlbase.png)

3. <span data-ttu-id="96c3f-153">Az a **Predictix rendelési tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="96c3f-153">On the **Predictix Ordering Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat Predictix rendelési tartomány és az URL-címek](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_url.png)

    <span data-ttu-id="96c3f-155">a.</span><span class="sxs-lookup"><span data-stu-id="96c3f-155">a.</span></span> <span data-ttu-id="96c3f-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname-pricing>.ordering.predictix.com/sso/request`</span><span class="sxs-lookup"><span data-stu-id="96c3f-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname-pricing>.ordering.predictix.com/sso/request`</span></span>

    <span data-ttu-id="96c3f-157">b.</span><span class="sxs-lookup"><span data-stu-id="96c3f-157">b.</span></span> <span data-ttu-id="96c3f-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="96c3f-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span> 
    | |
    |--|
    | `https://<companyname-pricing>.dev.ordering.predictix.com` |
    | `https://<companyname-pricing>.ordering.predictix.com` |

    > [!NOTE] 
    > <span data-ttu-id="96c3f-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="96c3f-159">These values are not real.</span></span> <span data-ttu-id="96c3f-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="96c3f-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="96c3f-161">Ügyfél [Predictix rendelési ügyfél-támogatási csoport](https://www.predix.io/support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="96c3f-161">Contact [Predictix Ordering Client support team](https://www.predix.io/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="96c3f-162">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="96c3f-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_certificate.png) 

5. <span data-ttu-id="96c3f-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="96c3f-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-predictixordering-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="96c3f-166">A a **Predictix rendelési konfigurációs** kattintson **Predictix rendelési konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="96c3f-166">On the **Predictix Ordering Configuration** section, click **Configure Predictix Ordering** to open **Configure sign-on** window.</span></span> <span data-ttu-id="96c3f-167">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="96c3f-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Predictix rendelési konfiguráció](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_configure.png) 

7. <span data-ttu-id="96c3f-169">Egyszeri bejelentkezés konfigurálása **Predictix rendelési** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [terméktámogatási csapathoz rendelési Predictix](https://www.predix.io/support/).</span><span class="sxs-lookup"><span data-stu-id="96c3f-169">To configure single sign-on on **Predictix Ordering** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Predictix Ordering support team](https://www.predix.io/support/).</span></span> <span data-ttu-id="96c3f-170">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="96c3f-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="96c3f-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="96c3f-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="96c3f-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="96c3f-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="96c3f-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="96c3f-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="96c3f-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="96c3f-174">Create an Azure AD test user</span></span>
<span data-ttu-id="96c3f-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="96c3f-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="96c3f-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="96c3f-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="96c3f-178">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="96c3f-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="96c3f-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="96c3f-182">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="96c3f-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="96c3f-184">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="96c3f-184">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_04.png)

   <span data-ttu-id="96c3f-186">a.</span><span class="sxs-lookup"><span data-stu-id="96c3f-186">a.</span></span> <span data-ttu-id="96c3f-187">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-187">In the **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="96c3f-188">b.</span><span class="sxs-lookup"><span data-stu-id="96c3f-188">b.</span></span> <span data-ttu-id="96c3f-189">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96c3f-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

   <span data-ttu-id="96c3f-190">c.</span><span class="sxs-lookup"><span data-stu-id="96c3f-190">c.</span></span> <span data-ttu-id="96c3f-191">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="96c3f-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

   <span data-ttu-id="96c3f-192">d.</span><span class="sxs-lookup"><span data-stu-id="96c3f-192">d.</span></span> <span data-ttu-id="96c3f-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="96c3f-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-ordering-test-user"></a><span data-ttu-id="96c3f-194">Predictix rendelési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="96c3f-194">Create a Predictix Ordering test user</span></span>

<span data-ttu-id="96c3f-195">Ebben a szakaszban Britta Simon meghívta Predictix rendelési felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="96c3f-195">In this section, you create a user called Britta Simon in Predictix Ordering.</span></span> <span data-ttu-id="96c3f-196">Együttműködve [terméktámogatási csapathoz rendelési Predictix](https://www.predix.io/support/) a felhasználók hozzáadása a Predictix rendelési platform.</span><span class="sxs-lookup"><span data-stu-id="96c3f-196">Work with [Predictix Ordering support team](https://www.predix.io/support/) to add the users in the Predictix Ordering platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="96c3f-197">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="96c3f-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="96c3f-198">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által biztosított hozzáférés Predictix rendezés használatára.</span><span class="sxs-lookup"><span data-stu-id="96c3f-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Predictix Ordering.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="96c3f-200">**Britta Simon hozzárendelése Predictix rendezés, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="96c3f-200">**To assign Britta Simon to Predictix Ordering, perform the following steps:**</span></span>

1. <span data-ttu-id="96c3f-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="96c3f-203">Az alkalmazások listában válassza ki a **Predictix rendelési**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-203">In the applications list, select **Predictix Ordering**.</span></span>

    ![Az alkalmazások listáját a Predictix rendelési hivatkozás](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_app.png)  

3. <span data-ttu-id="96c3f-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="96c3f-205">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="96c3f-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="96c3f-207">Click **Add** button.</span></span> <span data-ttu-id="96c3f-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96c3f-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="96c3f-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="96c3f-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="96c3f-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96c3f-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="96c3f-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="96c3f-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="96c3f-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="96c3f-213">Test single sign-on</span></span>

<span data-ttu-id="96c3f-214">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="96c3f-214">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="96c3f-215">Ha a hozzáférési Panel Predictix rendelési mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett Predictix rendelési Alkalmazásmódosítások.</span><span class="sxs-lookup"><span data-stu-id="96c3f-215">When you click the Predictix Ordering tile in the Access Panel, you should get automatically signed-on to your Predictix Ordering application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="96c3f-216">További források</span><span class="sxs-lookup"><span data-stu-id="96c3f-216">Additional resources</span></span>

* [<span data-ttu-id="96c3f-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="96c3f-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="96c3f-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="96c3f-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_203.png

