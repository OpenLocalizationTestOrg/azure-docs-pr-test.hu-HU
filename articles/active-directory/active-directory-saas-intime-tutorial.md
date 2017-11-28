---
title: "Oktatóanyag: Azure Active Directoryval integrált InTime |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és InTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4e2c6e1-ae5d-4d2c-8ffc-1b24534d376a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jeedes
ms.openlocfilehash: 4bb22c92ad7f6963be6ca15073f7f01da99ba2bb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intime"></a><span data-ttu-id="a7feb-103">Oktatóanyag: Azure Active Directoryval integrált InTime</span><span class="sxs-lookup"><span data-stu-id="a7feb-103">Tutorial: Azure Active Directory integration with InTime</span></span>

<span data-ttu-id="a7feb-104">Ebben az oktatóanyagban elsajátíthatja InTime integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a7feb-104">In this tutorial, you learn how to integrate InTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a7feb-105">InTime integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a7feb-105">Integrating InTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a7feb-106">Az Azure AD, aki hozzáfér InTime szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="a7feb-106">You can control in Azure AD who has access to InTime.</span></span>
- <span data-ttu-id="a7feb-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett InTime (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="a7feb-107">You can enable your users to automatically get signed-on to InTime (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a7feb-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="a7feb-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="a7feb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a7feb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7feb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a7feb-110">Prerequisites</span></span>

<span data-ttu-id="a7feb-111">Konfigurálása az Azure AD-integrációs InTime, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a7feb-111">To configure Azure AD integration with InTime, you need the following items:</span></span>

- <span data-ttu-id="a7feb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a7feb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a7feb-113">Egy InTime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a7feb-113">A InTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a7feb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a7feb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a7feb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a7feb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a7feb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7feb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a7feb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a7feb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a7feb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a7feb-118">Scenario description</span></span>
<span data-ttu-id="a7feb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a7feb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a7feb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a7feb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a7feb-121">A gyűjteményből InTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a7feb-121">Adding InTime from the gallery</span></span>
2. <span data-ttu-id="a7feb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a7feb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intime-from-the-gallery"></a><span data-ttu-id="a7feb-123">A gyűjteményből InTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a7feb-123">Adding InTime from the gallery</span></span>
<span data-ttu-id="a7feb-124">Az Azure AD integrálása a InTime konfigurálásához kell hozzáadnia InTime a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a7feb-124">To configure the integration of InTime into Azure AD, you need to add InTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a7feb-125">**A gyűjteményből InTime hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7feb-125">**To add InTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a7feb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a7feb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="a7feb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a7feb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="a7feb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a7feb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="a7feb-133">Írja be a keresőmezőbe, **InTime**, jelölje be **InTime** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a7feb-133">In the search box, type **InTime**, select **InTime** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában InTime](./media/active-directory-saas-intime-tutorial/tutorial_intime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a7feb-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a7feb-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a7feb-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján InTime.</span><span class="sxs-lookup"><span data-stu-id="a7feb-136">In this section, you configure and test Azure AD single sign-on with InTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a7feb-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó InTime a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a7feb-137">For single sign-on to work, Azure AD needs to know what the counterpart user in InTime is to a user in Azure AD.</span></span> <span data-ttu-id="a7feb-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a InTime közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a7feb-138">In other words, a link relationship between an Azure AD user and the related user in InTime needs to be established.</span></span>

<span data-ttu-id="a7feb-139">InTime, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a7feb-139">In InTime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a7feb-140">Az Azure AD egyszeri bejelentkezést a InTime tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a7feb-140">To configure and test Azure AD single sign-on with InTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a7feb-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a7feb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a7feb-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a7feb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a7feb-143">**[InTime tesztfelhasználó létrehozása](#create-a-intime-test-user)**  - való Britta Simon valami InTime, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a7feb-143">**[Create a InTime test user](#create-a-intime-test-user)** - to have a counterpart of Britta Simon in InTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a7feb-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a7feb-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a7feb-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a7feb-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a7feb-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a7feb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a7feb-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az InTime alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a7feb-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your InTime application.</span></span>

<span data-ttu-id="a7feb-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés InTime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7feb-148">**To configure Azure AD single sign-on with InTime, perform the following steps:**</span></span>

1. <span data-ttu-id="a7feb-149">Az Azure portálon a a **InTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-149">In the Azure portal, on the **InTime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="a7feb-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a7feb-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-intime-tutorial/tutorial_intime_samlbase.png)

3. <span data-ttu-id="a7feb-153">Az a **InTime tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a7feb-153">On the **InTime Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat inTime tartomány- és URL-címek](./media/active-directory-saas-intime-tutorial/tutorial_intime_url.png)

    <span data-ttu-id="a7feb-155">a.</span><span class="sxs-lookup"><span data-stu-id="a7feb-155">a.</span></span> <span data-ttu-id="a7feb-156">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://intime6.intimesoft.com/mytime/login/login.xhtml`</span><span class="sxs-lookup"><span data-stu-id="a7feb-156">In the **Sign-on URL** textbox, type the URL: `https://intime6.intimesoft.com/mytime/login/login.xhtml`</span></span>

    <span data-ttu-id="a7feb-157">b.</span><span class="sxs-lookup"><span data-stu-id="a7feb-157">b.</span></span> <span data-ttu-id="a7feb-158">Az a **azonosító** szövegmező, írja be az URL-cím:`https://auth.intimesoft.com/auth/realms/master`</span><span class="sxs-lookup"><span data-stu-id="a7feb-158">In the **Identifier** textbox, type the URL: `https://auth.intimesoft.com/auth/realms/master`</span></span>

4. <span data-ttu-id="a7feb-159">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a7feb-159">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-intime-tutorial/tutorial_intime_certificate.png) 

5. <span data-ttu-id="a7feb-161">A InTime alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="a7feb-161">Your InTime application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="a7feb-162">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="a7feb-162">The following screenshot shows an example for this.</span></span> <span data-ttu-id="a7feb-163">Az alapértelmezett érték **felhasználói azonosító** van **user.userprincipalname** de InTime vár a képezhető le a felhasználó e-mail címmel.</span><span class="sxs-lookup"><span data-stu-id="a7feb-163">The default value of **User Identifier** is **user.userprincipalname** but InTime expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="a7feb-164">Az adott használhatja **user.mail** attribútumot a listából, vagy használja a megfelelő attribútum értéket a szervezet konfiguráció alapján</span><span class="sxs-lookup"><span data-stu-id="a7feb-164">For that you can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration</span></span> 

    ![Attribútum konfigurálása](./media/active-directory-saas-intime-tutorial/tutorial_intime_attribute.png)

6. <span data-ttu-id="a7feb-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7feb-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-intime-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a7feb-168">Az a **InTime konfigurációs** kattintson **InTime konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a7feb-168">On the **InTime Configuration** section, click **Configure InTime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a7feb-169">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a7feb-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![InTime konfiguráció](./media/active-directory-saas-intime-tutorial/tutorial_intime_configure.png) 

8. <span data-ttu-id="a7feb-171">Egyszeri bejelentkezés konfigurálása **InTime** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja**, **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** való [ InTime támogatási csoport](mailto:hdollard@intimesoft.com).</span><span class="sxs-lookup"><span data-stu-id="a7feb-171">To configure single sign-on on **InTime** side, you need to send the downloaded **Metadata XML**, **Sign-Out URL, and SAML Single Sign-On Service URL** to [InTime support team](mailto:hdollard@intimesoft.com).</span></span> <span data-ttu-id="a7feb-172">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="a7feb-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a7feb-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a7feb-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a7feb-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a7feb-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a7feb-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a7feb-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a7feb-176">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="a7feb-176">Create an Azure AD test user</span></span>

<span data-ttu-id="a7feb-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a7feb-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="a7feb-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7feb-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a7feb-180">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7feb-180">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-intime-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a7feb-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-182">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-intime-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a7feb-184">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a7feb-184">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-intime-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a7feb-186">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a7feb-186">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-intime-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a7feb-188">a.</span><span class="sxs-lookup"><span data-stu-id="a7feb-188">a.</span></span> <span data-ttu-id="a7feb-189">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-189">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a7feb-190">b.</span><span class="sxs-lookup"><span data-stu-id="a7feb-190">b.</span></span> <span data-ttu-id="a7feb-191">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7feb-191">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="a7feb-192">c.</span><span class="sxs-lookup"><span data-stu-id="a7feb-192">c.</span></span> <span data-ttu-id="a7feb-193">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a7feb-193">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="a7feb-194">d.</span><span class="sxs-lookup"><span data-stu-id="a7feb-194">d.</span></span> <span data-ttu-id="a7feb-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a7feb-195">Click **Create**.</span></span>
 
### <a name="create-a-intime-test-user"></a><span data-ttu-id="a7feb-196">InTime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7feb-196">Create a InTime test user</span></span>

<span data-ttu-id="a7feb-197">Ebben a szakaszban egy InTime Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a7feb-197">In this section, you create a user called Britta Simon in InTime.</span></span> <span data-ttu-id="a7feb-198">Együttműködve [InTime támogatási csoport](mailto:hdollard@intimesoft.com) a felhasználók hozzáadása a InTime platform.</span><span class="sxs-lookup"><span data-stu-id="a7feb-198">Work with [InTime support team](mailto:hdollard@intimesoft.com) to add the users in the InTime platform.</span></span> <span data-ttu-id="a7feb-199">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="a7feb-199">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a7feb-200">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="a7feb-200">Assign the Azure AD test user</span></span>

<span data-ttu-id="a7feb-201">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés InTime Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a7feb-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to InTime.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="a7feb-203">**Britta Simon hozzárendelése InTime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7feb-203">**To assign Britta Simon to InTime, perform the following steps:**</span></span>

1. <span data-ttu-id="a7feb-204">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a7feb-206">Az alkalmazások listában válassza ki a **InTime**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-206">In the applications list, select **InTime**.</span></span>

    ![Az alkalmazások listáját a InTime hivatkozásra](./media/active-directory-saas-intime-tutorial/tutorial_intime_app.png)  

3. <span data-ttu-id="a7feb-208">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a7feb-208">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="a7feb-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7feb-210">Click **Add** button.</span></span> <span data-ttu-id="a7feb-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7feb-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="a7feb-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a7feb-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a7feb-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7feb-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a7feb-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7feb-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a7feb-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a7feb-216">Test single sign-on</span></span>

<span data-ttu-id="a7feb-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a7feb-217">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a7feb-218">Ha InTime csempére kattint a hozzáférési panelen, a bejelentkezési oldalt a InTime alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="a7feb-218">When you click the InTime tile in the Access Panel, you should get the login page of your InTime application.</span></span> <span data-ttu-id="a7feb-219">Kattintson a **bejelentkezési** gombra, majd IdPs sorozata gombok listája megjelenik.</span><span class="sxs-lookup"><span data-stu-id="a7feb-219">Click the **Login** button, then a series of IdPs will be displayed on a list of buttons.</span></span> <span data-ttu-id="a7feb-220">Kattintson a **IDP neve** által megadott [InTime támogatási csoport](mailto:hdollard@intimesoft.com) használatát a bejelentkezéshez az InTime alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="a7feb-220">click **IDP name** given by [InTime support team](mailto:hdollard@intimesoft.com) to login into your InTime application.</span></span> <span data-ttu-id="a7feb-221">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a7feb-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a7feb-222">További források</span><span class="sxs-lookup"><span data-stu-id="a7feb-222">Additional resources</span></span>

* [<span data-ttu-id="a7feb-223">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a7feb-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a7feb-224">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a7feb-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intime-tutorial/tutorial_general_203.png

