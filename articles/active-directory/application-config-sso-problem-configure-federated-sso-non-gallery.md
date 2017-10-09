---
title: "összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása aaaProblem |} Microsoft Docs"
description: "Néhány hello gyakori problémák jelentkezhetnek, ha összevont egyszeri bejelentkezés tooyour egyéni SAML-alkalmazások konfigurálása, amely nem szerepel az Azure AD Application Gallery hello cím"
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
ms.openlocfilehash: 8c80f0001de01cbc9930ef0441cd804082ee8578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="926fe-103">A probléma összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="926fe-103">Problem configuring federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="926fe-104">Ha probléma merül fel, ha az alkalmazások konfigurálásáról.</span><span class="sxs-lookup"><span data-stu-id="926fe-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="926fe-105">Ellenőrizze, hogy minden hello lépésekkel hello cikkben [konfigurálása egyszeri bejelentkezéshez tooapplications, amelyek hello Azure Active Directory alkalmazáskatalógusában nem találhatók.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span><span class="sxs-lookup"><span data-stu-id="926fe-105">Verify you have followed all hello steps in hello article [Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)</span></span>

## <a name="cant-add-another-instance-of-hello-application"></a><span data-ttu-id="926fe-106">Hello alkalmazás egy másik példánya nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="926fe-106">Can’t add another instance of hello application</span></span>

<span data-ttu-id="926fe-107">tooadd egy második alkalmazáspéldányt, kell toobe képes:</span><span class="sxs-lookup"><span data-stu-id="926fe-107">tooadd a second instance of an application, you need toobe able to:</span></span>

-   <span data-ttu-id="926fe-108">Konfigurálja a második példány hello egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="926fe-108">Configure a unique identifier for hello second instance.</span></span> <span data-ttu-id="926fe-109">Nem fog tudni tooconfigure hello hello első példánynál használt ugyanezzel az azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="926fe-109">You won’t be able tooconfigure hello same identifier used for hello first instance.</span></span>

-   <span data-ttu-id="926fe-110">Egy másik tanúsítványt, mint egy hello elsősorban a hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="926fe-110">Configure a different certificate than hello one used for hello first instance.</span></span>

<span data-ttu-id="926fe-111">Ha hello alkalmazás nem támogatja a fenti hello bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="926fe-111">If hello application doesn’t support any of hello above.</span></span> <span data-ttu-id="926fe-112">Majd nem fogja tudni tooconfigure egy második példányt.</span><span class="sxs-lookup"><span data-stu-id="926fe-112">Then, you won’t be able tooconfigure a second instance.</span></span>

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a><span data-ttu-id="926fe-113">Ahol a hello entityid (felhasználói azonosító) beállítást formátum beállítása</span><span class="sxs-lookup"><span data-stu-id="926fe-113">Where do I set hello EntityID (User Identifier) format</span></span>

<span data-ttu-id="926fe-114">Nem lehet, hogy az Azure AD küld toohello alkalmazás hello válaszul felhasználói hitelesítés után képes tooselect hello entityid beállítást (felhasználói azonosító) formátumban.</span><span class="sxs-lookup"><span data-stu-id="926fe-114">You won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="926fe-115">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="926fe-115">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="926fe-116">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) hello szakaszban NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="926fe-116">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy,</span></span>

## <a name="where-do-i-get-hello-application-metadata-or-certificate-from-azure-ad"></a><span data-ttu-id="926fe-117">Hol hello alkalmazás metaadatainak vagy a tanúsítvány hozzá az Azure AD</span><span class="sxs-lookup"><span data-stu-id="926fe-117">Where do I get hello application metadata or certificate from Azure AD</span></span>

<span data-ttu-id="926fe-118">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="926fe-118">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="926fe-119">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="926fe-119">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="926fe-120">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="926fe-120">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="926fe-121">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="926fe-121">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="926fe-122">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="926fe-122">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="926fe-123">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="926fe-123">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="926fe-124">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="926fe-124">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="926fe-125">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="926fe-125">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="926fe-126">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="926fe-126">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="926fe-127">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="926fe-127">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="926fe-128">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="926fe-128">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="926fe-129">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="926fe-129">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="926fe-130">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="926fe-130">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a><span data-ttu-id="926fe-131">Nem tudjuk, hogyan toocustomize SAML jogcímek küldött tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="926fe-131">Don't know how toocustomize SAML claims sent tooan application</span></span>

<span data-ttu-id="926fe-132">toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="926fe-132">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="926fe-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="926fe-133">Next steps</span></span>
[<span data-ttu-id="926fe-134">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="926fe-134">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
