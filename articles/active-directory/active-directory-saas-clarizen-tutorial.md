---
title: "Oktatóanyag: Azure Active Directoryval integrált Clarizen |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Clarizen között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 574c6877bddac8be7d6d541bfabbdc10f6be3101
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a><span data-ttu-id="75ce7-103">Oktatóanyag: Azure Active Directoryval integrált Clarizen</span><span class="sxs-lookup"><span data-stu-id="75ce7-103">Tutorial: Azure Active Directory integration with Clarizen</span></span>

<span data-ttu-id="75ce7-104">Ebben az oktatóanyagban Azure Active Directory (Azure AD) integrálása elsajátíthatja a Clarizen.</span><span class="sxs-lookup"><span data-stu-id="75ce7-104">In this tutorial, you learn how to integrate Azure Active Directory (Azure AD) with Clarizen.</span></span> <span data-ttu-id="75ce7-105">Ez az integráció lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="75ce7-105">This integration gives you the following benefits:</span></span>

- <span data-ttu-id="75ce7-106">Azt is szabályozhatja, az Azure AD, aki hozzáfér Clarizen.</span><span class="sxs-lookup"><span data-stu-id="75ce7-106">You can control, in Azure AD, who has access to Clarizen.</span></span>
- <span data-ttu-id="75ce7-107">Engedélyezheti a felhasználóknak, hogy automatikusan jelentkeznie Clarizen (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="75ce7-107">You can enable your users to be automatically signed in to Clarizen (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="75ce7-108">A fiók egyetlen központi helyen, az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="75ce7-108">You can manage your accounts in one central location, the Azure portal.</span></span>

<span data-ttu-id="75ce7-109">Ebben az oktatóanyagban a forgatókönyv két fő feladatokat foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="75ce7-109">The scenario in this tutorial consists of two main tasks:</span></span>

1. <span data-ttu-id="75ce7-110">Adja hozzá a Clarizen a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="75ce7-110">Add Clarizen from the gallery.</span></span>
2. <span data-ttu-id="75ce7-111">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="75ce7-111">Configure and test Azure AD single sign-on.</span></span>

<span data-ttu-id="75ce7-112">Ha szoftver további adatait, és az Azure AD egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75ce7-112">If you want more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75ce7-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="75ce7-113">Prerequisites</span></span>
<span data-ttu-id="75ce7-114">Konfigurálása az Azure AD-integrációs Clarizen, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="75ce7-114">To configure Azure AD integration with Clarizen, you need the following items:</span></span>

- <span data-ttu-id="75ce7-115">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="75ce7-115">An Azure AD subscription</span></span>
- <span data-ttu-id="75ce7-116">Az egyszeri bejelentkezés engedélyezett Clarizen előfizetés</span><span class="sxs-lookup"><span data-stu-id="75ce7-116">A Clarizen subscription that's enabled for single sign-on</span></span>

<span data-ttu-id="75ce7-117">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="75ce7-117">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="75ce7-118">Az Azure AD az egyszeri bejelentkezés tesztelése egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="75ce7-118">Test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75ce7-119">Az éles környezetben ne használjon, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="75ce7-119">Don't use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="75ce7-120">Ha még nem rendelkezik az Azure AD-tesztelési környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75ce7-120">If you don't have an Azure AD test environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="add-clarizen-from-the-gallery"></a><span data-ttu-id="75ce7-121">Adja hozzá a Clarizen a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="75ce7-121">Add Clarizen from the gallery</span></span>
<span data-ttu-id="75ce7-122">Az Azure AD integrálása a Clarizen megadásához vegye fel Clarizen a gyűjteményből a felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="75ce7-122">To configure the integration of Clarizen into Azure AD, add Clarizen from the gallery to your list of managed SaaS apps.</span></span>

1. <span data-ttu-id="75ce7-123">Az a [Azure-portálon](https://portal.azure.com), a bal oldali ablaktáblán kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-123">In the [Azure portal](https://portal.azure.com), in the left pane, click the **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory ikonra][1]

2. <span data-ttu-id="75ce7-125">Kattintson a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-125">Click **Enterprise applications**.</span></span> <span data-ttu-id="75ce7-126">Kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-126">Then click **All applications**.</span></span>

    ![Kattintson a "Vállalati alkalmazások" és "Összes alkalmazás"][2]

3. <span data-ttu-id="75ce7-128">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="75ce7-128">Click the **Add** button at the top of the dialog box.</span></span>

    ![A "Hozzáadás" gombra][3]

4. <span data-ttu-id="75ce7-130">Írja be a keresőmezőbe, **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-130">In the search box, type **Clarizen**.</span></span>

    ![A keresőmezőbe írja be a "Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. <span data-ttu-id="75ce7-132">Az eredmények ablaktáblájában válassza **Clarizen**, és kattintson a **Hozzáadás** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="75ce7-132">In the results pane, select **Clarizen**, and then click **Add** to add the application.</span></span>

    ![Az eredménypanelen Clarizen kiválasztása](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="75ce7-134">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="75ce7-134">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="75ce7-135">A következő szakaszokban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezést a tesztfelhasználó számára Britta Simon alapján Clarizen.</span><span class="sxs-lookup"><span data-stu-id="75ce7-135">In the following sections, you configure and test Azure AD single sign-on with Clarizen based on the test user Britta Simon.</span></span>

<span data-ttu-id="75ce7-136">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Clarizen a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="75ce7-136">For single sign-on to work, Azure AD needs to know what the counterpart user in Clarizen is to a user in Azure AD.</span></span> <span data-ttu-id="75ce7-137">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Clarizen közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="75ce7-137">In other words, a link relationship between an Azure AD user and the related user in Clarizen needs to be established.</span></span> <span data-ttu-id="75ce7-138">Kapcsolatot hoz létre a kapcsolat kapcsolat értékének hozzárendelésével **felhasználónév** értékeként Azure AD-ben **felhasználónév** a Clarizen.</span><span class="sxs-lookup"><span data-stu-id="75ce7-138">You establish this link relationship by assigning the value of **user name** in Azure AD as the value of **Username** in Clarizen.</span></span>

<span data-ttu-id="75ce7-139">Az Azure AD egyszeri bejelentkezést a Clarizen tesztelése és konfigurálása, végezze el a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="75ce7-139">To configure and test Azure AD single sign-on with Clarizen, complete the following building blocks:</span></span>

1. <span data-ttu-id="75ce7-140">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  ahhoz, hogy a felhasználók számára a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="75ce7-140">**[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** to enable your users to use this feature.</span></span>
2. <span data-ttu-id="75ce7-141">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="75ce7-141">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75ce7-142">**[Clarizen tesztfelhasználó létrehozása](#create-a-clarizen-test-user)**  való Britta Simon valami Clarizen, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="75ce7-142">**[Create a Clarizen test user](#create-a-clarizen-test-user)** to have a counterpart of Britta Simon in Clarizen that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="75ce7-143">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="75ce7-143">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75ce7-144">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  ellenőrzése, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="75ce7-144">**[Test single sign-on](#test-single-sign-on)** to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="75ce7-145">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="75ce7-145">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="75ce7-146">Az Azure AD az egyszeri bejelentkezés az Azure portálon engedélyezése és konfigurálása egyszeri bejelentkezéshez az Clarizen alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="75ce7-146">Enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Clarizen application.</span></span>

1. <span data-ttu-id="75ce7-147">Az Azure portálon a a **Clarizen** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-147">In the Azure portal, on the **Clarizen** application integration page, click **Single sign-on**.</span></span>

    ![Kattintson az "Egyszeri bejelentkezéshez"][4]

2. <span data-ttu-id="75ce7-149">Az a **egyszeri bejelentkezés** párbeszédpanelen a **mód**, jelölje be **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="75ce7-149">In the **Single sign-on** dialog box, for **Mode**, select **SAML-based Sign-on** to enable single sign-on.</span></span>

    !["SAML-alapú bejelentkezés" kiválasztása](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. <span data-ttu-id="75ce7-151">Az a **Clarizen tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="75ce7-151">In the **Clarizen Domain and URLs** section, perform the following steps:</span></span>

    ![Jelölőnégyzetéből azonosítója és a válasz URL-címe](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    <span data-ttu-id="75ce7-153">a.</span><span class="sxs-lookup"><span data-stu-id="75ce7-153">a.</span></span> <span data-ttu-id="75ce7-154">Az a **azonosító** mezőbe írja be az értéket, mint: **Clarizen**</span><span class="sxs-lookup"><span data-stu-id="75ce7-154">In the **Identifier** box, type the value as: **Clarizen**</span></span>

    <span data-ttu-id="75ce7-155">b.</span><span class="sxs-lookup"><span data-stu-id="75ce7-155">b.</span></span> <span data-ttu-id="75ce7-156">Az a **válasz URL-CÍMEN** mezőbe írja be egy URL-cím használatával a következő mintát: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span><span class="sxs-lookup"><span data-stu-id="75ce7-156">In the **Reply URL** box, type a URL by using the following pattern: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span></span>

    > [!NOTE]
    > <span data-ttu-id="75ce7-157">Ezek a megadandó nem valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="75ce7-157">These are not the real values.</span></span> <span data-ttu-id="75ce7-158">Ki kell a tényleges azonosítót és a válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="75ce7-158">You have to use the actual identifier and reply URL.</span></span> <span data-ttu-id="75ce7-159">Itt javasoljuk, hogy az azonosító, a karakterlánc egyedi érték használatát.</span><span class="sxs-lookup"><span data-stu-id="75ce7-159">Here we suggest that you use the unique value of a string as the identifier.</span></span> <span data-ttu-id="75ce7-160">Ahhoz, hogy a tényleges értékek, lépjen kapcsolatba a [Clarizen támogatási csoport](https://success.clarizen.com/hc/en-us/requests/new).</span><span class="sxs-lookup"><span data-stu-id="75ce7-160">To get the actual values, contact the [Clarizen support team](https://success.clarizen.com/hc/en-us/requests/new).</span></span>

4. <span data-ttu-id="75ce7-161">Az a **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-161">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Kattintson a "Hozható létre új tanúsítvány"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. <span data-ttu-id="75ce7-163">Az a **új tanúsítvány létrehozása** párbeszédpanel mezőben, kattintson a naptár ikonra, és válassza ki a lejárati dátummal.</span><span class="sxs-lookup"><span data-stu-id="75ce7-163">In the **Create New Certificate** dialog box, click the calendar icon and select an expiry date.</span></span> <span data-ttu-id="75ce7-164">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-164">Then click **Save**.</span></span>

    ![Válassza ki, majd lejárati dátummal mentése](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="75ce7-166">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-166">In the **SAML Signing Certificate** section, select **Make new certificate active**, and then click **Save**.</span></span>

    ![Jelölje be a jelölőnégyzetet, hogy az új tanúsítvány aktív](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. <span data-ttu-id="75ce7-168">Az a **helyettesítő tanúsítvány** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-168">In the **Rollover certificate** dialog box, click **OK**.</span></span>

    ![Annak ellenőrzéséhez, hogy szeretné-e a tanúsítvány aktiválásához az "OK" gombra kattintva](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="75ce7-170">Az a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="75ce7-170">In the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Kattintson a "Tanúsítvány (Base64)" a letöltés megkezdéséhez](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. <span data-ttu-id="75ce7-172">Az a **Clarizen konfigurációs** területén kattintson **konfigurálása Clarizen** megnyitásához a **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="75ce7-172">In the **Clarizen Configuration** section, click **Configure Clarizen** to open the **Configure sign-on** window.</span></span>

    ![Kattintson a "Clarizen konfigurálása"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    !["Bejelentkezés konfigurálása" ablak, beleértve a fájlok és az URL-címek](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. <span data-ttu-id="75ce7-175">Egy másik webes böngészőablakban jelentkezzen be a Clarizen vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="75ce7-175">In a different web browser window, sign in to your Clarizen company site as an administrator.</span></span>

11. <span data-ttu-id="75ce7-176">Kattintson a felhasználónevére, majd **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-176">Click your username, and then click **Settings**.</span></span>

    <span data-ttu-id="75ce7-177">![Kattintson a "Beállítások" a felhasználónév](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="75ce7-177">![Clicking "Settings" under your username](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")</span></span>

12. <span data-ttu-id="75ce7-178">Kattintson a **globális beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="75ce7-178">Click the **Global Settings** tab.</span></span> <span data-ttu-id="75ce7-179">Ezt követően a **összevont hitelesítési**, kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-179">Then, next to **Federated Authentication**, click **edit**.</span></span>

    <span data-ttu-id="75ce7-180">!["Globális beállítások" lapon](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globális beállítások")</span><span class="sxs-lookup"><span data-stu-id="75ce7-180">!["Global Settings" tab](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")</span></span>

13. <span data-ttu-id="75ce7-181">Az a **összevont hitelesítési** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="75ce7-181">In the **Federated Authentication** dialog box, perform the following steps:</span></span>

    <span data-ttu-id="75ce7-182">![A párbeszédpanel "Összevont hitelesítési"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "összevont hitelesítési")</span><span class="sxs-lookup"><span data-stu-id="75ce7-182">!["Federated Authentication" dialog box](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")</span></span>

    <span data-ttu-id="75ce7-183">a.</span><span class="sxs-lookup"><span data-stu-id="75ce7-183">a.</span></span> <span data-ttu-id="75ce7-184">Válassza ki **engedélyezése összevont hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-184">Select **Enable Federated Authentication**.</span></span>

    <span data-ttu-id="75ce7-185">b.</span><span class="sxs-lookup"><span data-stu-id="75ce7-185">b.</span></span> <span data-ttu-id="75ce7-186">Kattintson a **feltöltése** a letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="75ce7-186">Click **Upload** to upload your downloaded certificate.</span></span>

    <span data-ttu-id="75ce7-187">c.</span><span class="sxs-lookup"><span data-stu-id="75ce7-187">c.</span></span> <span data-ttu-id="75ce7-188">Az a **bejelentkezési URL** mezőbe írja be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** a Azure AD alkalmazás-konfigurációs ablakból.</span><span class="sxs-lookup"><span data-stu-id="75ce7-188">In the **Sign-in URL** box, enter the value of **SAML Single Sign-On Service URL** from the Azure AD application configuration window.</span></span>

    <span data-ttu-id="75ce7-189">d.</span><span class="sxs-lookup"><span data-stu-id="75ce7-189">d.</span></span> <span data-ttu-id="75ce7-190">Az a **Sign-out URL-cím** mezőbe írja be az értékét **Sign-Out URL-cím** a Azure AD alkalmazás-konfigurációs ablakból.</span><span class="sxs-lookup"><span data-stu-id="75ce7-190">In the **Sign-out URL** box, enter the value of **Sign-Out URL** from the Azure AD application configuration window.</span></span>

    <span data-ttu-id="75ce7-191">e.</span><span class="sxs-lookup"><span data-stu-id="75ce7-191">e.</span></span> <span data-ttu-id="75ce7-192">Válassza ki **használja a FELADÁS egy vagy több**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-192">Select **Use POST**.</span></span>

    <span data-ttu-id="75ce7-193">f.</span><span class="sxs-lookup"><span data-stu-id="75ce7-193">f.</span></span> <span data-ttu-id="75ce7-194">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-194">Click **Save**.</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="75ce7-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="75ce7-195">Create an Azure AD test user</span></span>
<span data-ttu-id="75ce7-196">Az Azure-portálon Britta Simon nevű tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="75ce7-196">In the Azure portal, create a test user called Britta Simon.</span></span>

![Név és e-mail címét az Azure AD-teszt felhasználó][100]

1. <span data-ttu-id="75ce7-198">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-198">In the Azure portal, in the left pane, click the **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory ikonra](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="75ce7-200">Kattintson a **felhasználók és csoportok**, és kattintson a **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="75ce7-200">Click **Users and groups**, and then click **All users** to display the list of users.</span></span>

    ![Kattintson a "Felhasználók és csoportok" és "Minden felhasználó"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="75ce7-202">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="75ce7-202">At the top of the dialog box, click **Add** to open the **User** dialog box.</span></span>

    ![A "Hozzáadás" gombra](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="75ce7-204">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="75ce7-204">In the **User** dialog box, perform the following steps:</span></span>

    !["User" párbeszédpanel a nevét, e-mail címét és jelszavát kitöltve](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    <span data-ttu-id="75ce7-206">a.</span><span class="sxs-lookup"><span data-stu-id="75ce7-206">a.</span></span> <span data-ttu-id="75ce7-207">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-207">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75ce7-208">b.</span><span class="sxs-lookup"><span data-stu-id="75ce7-208">b.</span></span> <span data-ttu-id="75ce7-209">Az a **felhasználónév** mezőbe írja be a Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="75ce7-209">In the **User name** box, type the email address of the Britta Simon account.</span></span>

    <span data-ttu-id="75ce7-210">c.</span><span class="sxs-lookup"><span data-stu-id="75ce7-210">c.</span></span> <span data-ttu-id="75ce7-211">Válassza ki **megjelenítése jelszó** írja le a értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-211">Select **Show Password** and write down the value of **Password**.</span></span>

    <span data-ttu-id="75ce7-212">d.</span><span class="sxs-lookup"><span data-stu-id="75ce7-212">d.</span></span> <span data-ttu-id="75ce7-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-213">Click **Create**.</span></span>

### <a name="create-a-clarizen-test-user"></a><span data-ttu-id="75ce7-214">Clarizen tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="75ce7-214">Create a Clarizen test user</span></span>
<span data-ttu-id="75ce7-215">Ahhoz, hogy az Azure AD-felhasználók Clarizen bejelentkezni, el kell juttatnia a felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="75ce7-215">To enable Azure AD users to sign in to Clarizen, you must provision user accounts.</span></span> <span data-ttu-id="75ce7-216">Clarizen, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="75ce7-216">In the case of Clarizen, provisioning is a manual task.</span></span>

1. <span data-ttu-id="75ce7-217">Jelentkezzen be rendszergazdaként a Clarizen vállalati webhelyre.</span><span class="sxs-lookup"><span data-stu-id="75ce7-217">Sign in to your Clarizen company site as an administrator.</span></span>

2. <span data-ttu-id="75ce7-218">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-218">Click **People**.</span></span>

    <span data-ttu-id="75ce7-219">![Kattintson a "Személyek"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="75ce7-219">![Clicking "People"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="75ce7-220">Kattintson a **felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-220">Click **Invite User**.</span></span>

    <span data-ttu-id="75ce7-221">!["A felhasználó meghívása" gomb](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "felhasználók meghívása")</span><span class="sxs-lookup"><span data-stu-id="75ce7-221">!["Invite User" button](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="75ce7-222">Az a **meghívása személyek** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="75ce7-222">In the **Invite People** dialog box, perform the following steps:</span></span>

    <span data-ttu-id="75ce7-223">!["Felkérése" párbeszédpanelen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "személyek meghívása")</span><span class="sxs-lookup"><span data-stu-id="75ce7-223">!["Invite People" dialog box](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="75ce7-224">a.</span><span class="sxs-lookup"><span data-stu-id="75ce7-224">a.</span></span> <span data-ttu-id="75ce7-225">Az a **E-mail** mezőbe írja be a Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="75ce7-225">In the **Email** box, type the email address of the Britta Simon account.</span></span>

    <span data-ttu-id="75ce7-226">b.</span><span class="sxs-lookup"><span data-stu-id="75ce7-226">b.</span></span> <span data-ttu-id="75ce7-227">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-227">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="75ce7-228">Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="75ce7-228">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="75ce7-229">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="75ce7-229">Assign the Azure AD test user</span></span>
<span data-ttu-id="75ce7-230">Britta Simon által biztosított a hozzáférés Clarizen használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="75ce7-230">Enable Britta Simon to use Azure single sign-on by granting her access to Clarizen.</span></span>

![Hozzárendelt tesztfelhasználó számára][200]

1. <span data-ttu-id="75ce7-232">Az Azure portál, nyissa meg az alkalmazások megtekintéséhez keresse meg a könyvtár nézetben kattintson **vállalati alkalmazások**, és kattintson a **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-232">In the Azure portal, open the applications view, browse to the directory view, click **Enterprise applications**, and then click **All applications**.</span></span>

    ![Kattintson a "Vállalati alkalmazások" és "Összes alkalmazás"][201]

2. <span data-ttu-id="75ce7-234">Az alkalmazások listában válassza ki a **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-234">In the applications list, select **Clarizen**.</span></span>

    ![Clarizen listából történő kiválasztásakor](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. <span data-ttu-id="75ce7-236">Kattintson a bal oldali ablaktáblában **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-236">In the left pane, click **Users and groups**.</span></span>

    ![Kattintson a "Felhasználók és csoportok"][202]

4. <span data-ttu-id="75ce7-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-238">Click the **Add** button.</span></span> <span data-ttu-id="75ce7-239">Ezt követően a a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="75ce7-239">Then, in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![A "Hozzáadás" gombra, és a "Hozzáadás hozzárendeléshez" párbeszédpanel][203]

5. <span data-ttu-id="75ce7-241">Az a **felhasználók és csoportok** párbeszédpanelen jelölje ki **Britta Simon** a felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="75ce7-241">In the **Users and groups** dialog box, select **Britta Simon** in the list of users.</span></span>

6. <span data-ttu-id="75ce7-242">Az a **felhasználók és csoportok** párbeszédpanel, kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-242">In the **Users and groups** dialog box, click the **Select** button.</span></span>

7. <span data-ttu-id="75ce7-243">Az a **hozzáadása hozzárendelés** párbeszédpanel, kattintson a **hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="75ce7-243">In the **Add Assignment** dialog box, click the **Assign** button.</span></span>

### <a name="test-single-sign-on"></a><span data-ttu-id="75ce7-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="75ce7-244">Test single sign-on</span></span>
<span data-ttu-id="75ce7-245">Az Azure AD egyszeri bejelentkezés beállításai tesztelése a hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="75ce7-245">Test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="75ce7-246">Ha a hozzáférési panelen Clarizen csempére kattint, be kell automatikusan jelentkeznie az Clarizen alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="75ce7-246">When you click the Clarizen tile in the Access Panel, you should be automatically signed in to your Clarizen application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75ce7-247">További források</span><span class="sxs-lookup"><span data-stu-id="75ce7-247">Additional resources</span></span>

* [<span data-ttu-id="75ce7-248">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="75ce7-248">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75ce7-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="75ce7-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
