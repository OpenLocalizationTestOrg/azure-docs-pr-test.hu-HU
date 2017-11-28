---
title: "összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása aaaProblem |} Microsoft Docs"
description: "Cím néhány hello gyakori problémák jelentkezhetnek, ha konfigurálása összevont egyszeri bejelentkezés alkalmazásokhoz az Azure AD Application Gallery hello szereplő SAML használatával"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2ae1e511bd49b19159e1ab83cf79a7db5403b9a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="b4a05-103">A probléma összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4a05-103">Problem configuring federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="b4a05-104">Ha probléma merül fel, ha az alkalmazások konfigurálásáról.</span><span class="sxs-lookup"><span data-stu-id="b4a05-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="b4a05-105">Ellenőrizze, hogy minden hello lépéseket követte hello oktatóprogram hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b4a05-105">Verify you have followed all hello steps in hello tutorial for hello application.</span></span> <span data-ttu-id="b4a05-106">Hello alkalmazást, a beágyazott dokumentációja hogyan tooconfigure hello alkalmazás van.</span><span class="sxs-lookup"><span data-stu-id="b4a05-106">In hello application’s configuration, you have inline documentation on how tooconfigure hello application.</span></span> <span data-ttu-id="b4a05-107">Emellett érheti el hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="b4a05-107">Also, you can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

## <a name="cant-add-another-instance-of-hello-application"></a><span data-ttu-id="b4a05-108">Hello alkalmazás egy másik példánya nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="b4a05-108">Can’t add another instance of hello application</span></span>

<span data-ttu-id="b4a05-109">tooadd egy második alkalmazáspéldányt, kell toobe képes:</span><span class="sxs-lookup"><span data-stu-id="b4a05-109">tooadd a second instance of an application, you need toobe able to:</span></span>

-   <span data-ttu-id="b4a05-110">Konfigurálja a második példány hello egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b4a05-110">Configure a unique identifier for hello second instance.</span></span> <span data-ttu-id="b4a05-111">Nem fog tudni tooconfigure hello hello első példánynál használt ugyanezzel az azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="b4a05-111">You won’t be able tooconfigure hello same identifier used for hello first instance.</span></span>

-   <span data-ttu-id="b4a05-112">Egy másik tanúsítványt, mint egy hello elsősorban a hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b4a05-112">Configure a different certificate than hello one used for hello first instance.</span></span>

<span data-ttu-id="b4a05-113">Ha hello alkalmazás nem támogatja a fenti hello bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="b4a05-113">If hello application doesn’t support any of hello above.</span></span> <span data-ttu-id="b4a05-114">Majd nem fogja tudni tooconfigure egy második példányt.</span><span class="sxs-lookup"><span data-stu-id="b4a05-114">Then, you won’t be able tooconfigure a second instance.</span></span>

## <a name="cant-add-hello-identifier-or-hello-reply-url"></a><span data-ttu-id="b4a05-115">Nem adható hozzá hello azonosító, vagy nem hello válasz URL-címe</span><span class="sxs-lookup"><span data-stu-id="b4a05-115">Can’t add hello Identifier or hello Reply URL</span></span>

<span data-ttu-id="b4a05-116">Ha nem tudja tooconfigure hello azonosítója vagy hello válasz URL-CÍMEN, erősítse meg a hello azonosítója, és válasz URL-CÍMEN megegyeznek hello minták hello alkalmazás előre konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="b4a05-116">If you’re not able tooconfigure hello Identifier or hello Reply URL, confirm hello Identifier and Reply URL values match hello patterns pre-configured for hello application.</span></span>

<span data-ttu-id="b4a05-117">tooknow hello minták hello alkalmazás előre konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="b4a05-117">tooknow hello patterns pre-configured for hello application:</span></span>

1.  <span data-ttu-id="b4a05-118">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.** Nyissa meg toostep 7.</span><span class="sxs-lookup"><span data-stu-id="b4a05-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.** Go toostep 7.</span></span> <span data-ttu-id="b4a05-119">Ha már hello alkalmazás konfigurációs panel az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="b4a05-119">If you are already in hello application configuration blade on Azure AD.</span></span>

2.  <span data-ttu-id="b4a05-120">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="b4a05-120">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b4a05-121">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b4a05-121">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b4a05-122">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b4a05-122">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b4a05-123">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="b4a05-123">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="b4a05-124">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b4a05-124">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="b4a05-125">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b4a05-125">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="b4a05-126">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b4a05-126">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b4a05-127">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b4a05-127">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="b4a05-128">Nyissa meg toohello **azonosítója** vagy **válasz URL-CÍMEN** szövegmező alatt hello **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="b4a05-128">Go toohello **Identifier** or **Reply URL** textbox, under hello **Domain and URLs section.**</span></span>

10. <span data-ttu-id="b4a05-129">Háromféleképpen tooknow hello támogatott minták hello alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="b4a05-129">There are three ways tooknow hello supported patterns for hello application:</span></span>

   * <span data-ttu-id="b4a05-130">Hello szövegmezőjének helyőrzőként látja hello támogatott példány szükséges *példa:* <https://contoso.com>.</span><span class="sxs-lookup"><span data-stu-id="b4a05-130">In hello textbox, you see hello supported pattern(s) as a placeholder *Example:* <https://contoso.com>.</span></span>

   * <span data-ttu-id="b4a05-131">hello nem módosíthatók, ha egy piros felkiáltójel látja hello szövegmezőjének tooenter hello értéke meg.</span><span class="sxs-lookup"><span data-stu-id="b4a05-131">if hello pattern is not supported, you see a red exclamation mark when you try tooenter hello value in hello textbox.</span></span> <span data-ttu-id="b4a05-132">Ha az egérmutatóval rámutat hello piros felkiáltójel, lásd: hello támogatott kombinációját.</span><span class="sxs-lookup"><span data-stu-id="b4a05-132">If you hover your mouse over hello red exclamation mark, you see hello supported patterns.</span></span>

   * <span data-ttu-id="b4a05-133">Hello alkalmazás hello oktatóanyagban is kaphat a hello támogatott mintájával kapcsolatos információkért.</span><span class="sxs-lookup"><span data-stu-id="b4a05-133">In hello tutorial for hello application, you can also get information about hello supported patterns.</span></span> <span data-ttu-id="b4a05-134">A hello **az Azure AD konfigurálása egyszeri bejelentkezéshez** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4a05-134">Under hello **Configure Azure AD single sign-on** section.</span></span> <span data-ttu-id="b4a05-135">Nyissa meg toohello lépés konfigurált hello értékek alapján hello **tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4a05-135">Go toohello step for configured hello values under hello **Domain and URLs** section.</span></span>

<span data-ttu-id="b4a05-136">Ha hello értékek nem egyeznek meg hello mintákat előre konfigurált Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="b4a05-136">If hello values don’t match with hello patterns pre-configured on Azure AD.</span></span> <span data-ttu-id="b4a05-137">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b4a05-137">You can:</span></span>

-   <span data-ttu-id="b4a05-138">Hello alkalmazás szállító tooget értékek, amelyek megfelelnek az Azure ad-val előre konfigurált hello minta használata</span><span class="sxs-lookup"><span data-stu-id="b4a05-138">Work with hello application vendor tooget values that match hello pattern pre-configured on Azure AD</span></span>

-   <span data-ttu-id="b4a05-139">Másik lehetőségként felveheti a kapcsolatot az Azure AD-csapatának a < aadapprequest@microsoft.com > vagy hagyja üresen a Megjegyzés hello oktatóanyag toorequest hello frissítés hello alkalmazás hello támogatott minták</span><span class="sxs-lookup"><span data-stu-id="b4a05-139">Or, you can contact Azure AD team at <aadapprequest@microsoft.com> or leave a comment in hello tutorial toorequest hello update of hello supported patterns for hello application</span></span>

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a><span data-ttu-id="b4a05-140">Ahol a hello entityid (felhasználói azonosító) beállítást formátum beállítása</span><span class="sxs-lookup"><span data-stu-id="b4a05-140">Where do I set hello EntityID (User Identifier) format</span></span>

<span data-ttu-id="b4a05-141">Nem lehet, hogy az Azure AD küld toohello alkalmazás hello válaszul felhasználói hitelesítés után képes tooselect hello entityid beállítást (felhasználói azonosító) formátumban.</span><span class="sxs-lookup"><span data-stu-id="b4a05-141">You won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="b4a05-142">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="b4a05-142">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="b4a05-143">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) hello szakaszban NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="b4a05-143">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy,</span></span>

## <a name="cant-find-hello-azure-ad-metadata-toocomplete-hello-configuration-with-hello-application"></a><span data-ttu-id="b4a05-144">Nem található a hello Azure Active Directory metaadatok toocomplete hello beállítása hello alkalmazással</span><span class="sxs-lookup"><span data-stu-id="b4a05-144">Can’t find hello Azure AD metadata toocomplete hello configuration with hello application</span></span>

<span data-ttu-id="b4a05-145">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b4a05-145">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="b4a05-146">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b4a05-146">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b4a05-147">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="b4a05-147">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b4a05-148">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b4a05-148">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b4a05-149">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b4a05-149">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b4a05-150">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="b4a05-150">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="b4a05-151">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b4a05-151">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="b4a05-152">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b4a05-152">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="b4a05-153">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b4a05-153">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b4a05-154">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="b4a05-154">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="b4a05-155">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b4a05-155">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="b4a05-156">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="b4a05-156">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="b4a05-157">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="b4a05-157">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a><span data-ttu-id="b4a05-158">Nem tudjuk, hogyan toocustomize SAML jogcímek küldött tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b4a05-158">Don't know how toocustomize SAML claims sent tooan application</span></span>

<span data-ttu-id="b4a05-159">toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="b4a05-159">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4a05-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4a05-160">Next steps</span></span>
[<span data-ttu-id="b4a05-161">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="b4a05-161">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
