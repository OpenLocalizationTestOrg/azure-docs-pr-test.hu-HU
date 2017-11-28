---
title: "aaaListing az alkalmazás hello Azure Active Directory alkalmazáskatalógusában"
description: "Hogyan toolist egy alkalmazás, amely támogatja az egyszeri bejelentkezést a hello Azure Active Directory-gyűjtemény |} A Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 09ccd3b4645a180059b9a9d502e39f1b8933c988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="listing-your-application-in-hello-azure-active-directory-application-gallery"></a><span data-ttu-id="920de-103">Az alkalmazás hello Azure Active Directory alkalmazáskatalógusában listázása</span><span class="sxs-lookup"><span data-stu-id="920de-103">Listing your application in hello Azure Active Directory application gallery</span></span>
<span data-ttu-id="920de-104">egy alkalmazás, amely támogatja az egyszeri bejelentkezés az Azure Active Directoryban a hello toolist [az Azure AD-gyűjtemény](https://azure.microsoft.com/marketplace/active-directory/all/), először meg kell tooimplement a következő integrációs módok hello hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="920de-104">toolist an application that supports single sign-on with Azure Active Directory in hello [Azure AD gallery](https://azure.microsoft.com/marketplace/active-directory/all/), hello application first needs tooimplement one of hello following integration modes:</span></span>

* <span data-ttu-id="920de-105">**OpenID Connect** - közvetlen integráció az Azure AD OpenID Connect használata a hitelesítéshez és az Azure AD hozzájárulási API konfiguráció hello.</span><span class="sxs-lookup"><span data-stu-id="920de-105">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and hello Azure AD consent API for configuration.</span></span> <span data-ttu-id="920de-106">Ha az alkalmazás nem támogatja az SAML-alapú integrációs csak indításakor, majd ez érték hello ajánlott módja.</span><span class="sxs-lookup"><span data-stu-id="920de-106">If you are just starting an integration and your application does not support SAML, then this is hello recommend mode.</span></span>
* <span data-ttu-id="920de-107">**SAML** – az alkalmazás már rendelkezik hello képességét tooconfigure harmadik fél Identitásszolgáltatók hello SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="920de-107">**SAML** – Your application already has hello ability tooconfigure third-party identity providers using hello SAML protocol.</span></span>

<span data-ttu-id="920de-108">Listaelem követelmények az egyes módokban alatt van.</span><span class="sxs-lookup"><span data-stu-id="920de-108">Listing requirements for each mode are below.</span></span>

## <a name="openid-connect-integration"></a><span data-ttu-id="920de-109">OpenID Connect integráció</span><span class="sxs-lookup"><span data-stu-id="920de-109">OpenID Connect Integration</span></span>
<span data-ttu-id="920de-110">toointegrate az alkalmazás az Azure AD, a következő hello [fejlesztői útmutatás](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="920de-110">toointegrate your application with Azure AD, following hello [developer instructions](active-directory-authentication-scenarios.md).</span></span> <span data-ttu-id="920de-111">Ezután fejezze be az alábbi kérdésekre hello és küldése toowaadpartners@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="920de-111">Then complete hello questions below and send toowaadpartners@microsoft.com.</span></span>

* <span data-ttu-id="920de-112">Azokat a hitelesítő adatokat egy tesztelési bérlőn vagy a fiókot, amely az Azure AD-csoport tootest hello integrációs hello használhatja az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="920de-112">Provide credentials for a test tenant or account with your application that can be used by hello Azure AD team tootest hello integration.</span></span>  
* <span data-ttu-id="920de-113">Hogyan hello Azure AD-csoport is bejelentkezhet, és csatlakozzon az Azure AD tooyour alkalmazást hello egy példányát vonatkozó utasítások [az Azure AD hozzájárulási keretrendszer](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span><span class="sxs-lookup"><span data-stu-id="920de-113">Provide instructions on how hello Azure AD team can sign in and connect an instance of Azure AD tooyour application using hello [Azure AD consent framework](active-directory-integrating-applications.md#overview-of-the-consent-framework).</span></span> 
* <span data-ttu-id="920de-114">Adja meg a szükséges hello Azure AD a további utasításokat csapat tootest egyszeri bejelentkezést az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="920de-114">Provide any further instructions required for hello Azure AD team tootest single sign-on with your application.</span></span> 
* <span data-ttu-id="920de-115">Az alábbi hello adatainak megadása:</span><span class="sxs-lookup"><span data-stu-id="920de-115">Provide hello info below:</span></span>

> <span data-ttu-id="920de-116">Vállalat neve:</span><span class="sxs-lookup"><span data-stu-id="920de-116">Company Name:</span></span>
> 
> <span data-ttu-id="920de-117">Vállalati webhely:</span><span class="sxs-lookup"><span data-stu-id="920de-117">Company Website:</span></span>
> 
> <span data-ttu-id="920de-118">Alkalmazás neve:</span><span class="sxs-lookup"><span data-stu-id="920de-118">Application Name:</span></span>
> 
> <span data-ttu-id="920de-119">Az alkalmazás leírása (200 karakter lehet):</span><span class="sxs-lookup"><span data-stu-id="920de-119">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="920de-120">Alkalmazás webhely (tájékoztató):</span><span class="sxs-lookup"><span data-stu-id="920de-120">Application Website (informational):</span></span>
> 
> <span data-ttu-id="920de-121">Alkalmazás technikai támogatási webhely vagy kapcsolattartási adatokat:</span><span class="sxs-lookup"><span data-stu-id="920de-121">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="920de-122">Alkalmazásazonosító hello alkalmazás látható módon: https://portal.azure.com hello alkalmazás részletei:</span><span class="sxs-lookup"><span data-stu-id="920de-122">Application  ID of hello application, as shown in hello application details at https://portal.azure.com:</span></span>
> 
> <span data-ttu-id="920de-123">Alkalmazás Sign-Up URL-Címének toosign a felhasználók hová és/vagy beszerzési hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="920de-123">Application Sign-Up URL where customers go toosign up for and /or purchase hello application:</span></span>
> 
> <span data-ttu-id="920de-124">Válassza ki a felsorolt (a kategóriák lásd az Azure Active Directory Marketplace hello) alkalmazás toobe toothree kategóriáit fel:</span><span class="sxs-lookup"><span data-stu-id="920de-124">Choose up toothree categories for your application toobe listed under (for available categories see hello Azure Active Directory Marketplace):</span></span>
> 
> <span data-ttu-id="920de-125">Csatolja az alkalmazás kis ikon (PNG-fájlt, 45px 45px, teli háttérszíne szerint):</span><span class="sxs-lookup"><span data-stu-id="920de-125">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="920de-126">Csatolja az alkalmazás nagy méretben (PNG-fájlt, 215px 215px, teli háttérszíne szerint):</span><span class="sxs-lookup"><span data-stu-id="920de-126">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="920de-127">Csatolja az alkalmazás embléma (PNG-fájlt, 122px, háttérszínt által 150px):</span><span class="sxs-lookup"><span data-stu-id="920de-127">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

## <a name="saml-integration"></a><span data-ttu-id="920de-128">SAML-integráció</span><span class="sxs-lookup"><span data-stu-id="920de-128">SAML Integration</span></span>
<span data-ttu-id="920de-129">Bármely alkalmazás, amely támogatja az SAML 2.0 integrálható közvetlenül az Azure AD-bérlő segítségével [ezen utasításokat tooadd egyéni alkalmazás](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="920de-129">Any app that supports SAML 2.0 can be integrated directly with an Azure AD tenant using [these instructions tooadd a custom application](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="920de-130">Miután ellenőrizte, hogy működik-e az alkalmazások integrálása az Azure ad szolgáltatással, küldjön hello túl a következő információk<mailto:waadpartners@microsoft.com>.</span><span class="sxs-lookup"><span data-stu-id="920de-130">Once you have tested that your application integration works with Azure AD, send hello following information too<mailto:waadpartners@microsoft.com>.</span></span>

* <span data-ttu-id="920de-131">Azokat a hitelesítő adatokat egy tesztelési bérlőn vagy a fiókot, amely az Azure AD-csoport tootest hello integrációs hello használhatja az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="920de-131">Provide credentials for a test tenant or account with your application that can be used by hello Azure AD team tootest hello integration.</span></span>  
* <span data-ttu-id="920de-132">Adja meg a hello SAML bejelentkezési URL-címe, kiállítójának URL-címe (entitás azonosítója), és az alkalmazás értékei válasz URL-CÍMEN (helyességi feltétel fogyasztói szolgáltatás) leírtak [Itt](../active-directory-saas-custom-apps.md).</span><span class="sxs-lookup"><span data-stu-id="920de-132">Provide hello SAML Sign-On URL, Issuer URL (entity ID), and Reply URL (assertion consumer service) values for your application, as described [here](../active-directory-saas-custom-apps.md).</span></span> <span data-ttu-id="920de-133">Ha általában megadja ezeket az értékeket a SAML-metaadatfájl részeként, majd küldje, amely is.</span><span class="sxs-lookup"><span data-stu-id="920de-133">If you typically provide these values as part of a SAML metadata file, then please send that as well.</span></span>
* <span data-ttu-id="920de-134">Adjon meg egy rövid leírást, hogy hogyan tooconfigure az Azure AD az alkalmazás SAML 2.0 használatával az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="920de-134">Provide a brief description of how tooconfigure Azure AD as an identity provider in your application using SAML 2.0.</span></span> <span data-ttu-id="920de-135">Ha az alkalmazás konfigurálása az Azure AD felügyeleti önkiszolgáló portál keresztül identitás-szolgáltatóként támogatja, majd győződjön meg arról fent megadott hello hitelesítő adatok is tartalmazzák hello képességét tooset a mentést.</span><span class="sxs-lookup"><span data-stu-id="920de-135">If your application supports configuring Azure AD as an identity provider through a self-service administrative portal, then please ensure hello credentials provided above include hello ability tooset this up.</span></span>
* <span data-ttu-id="920de-136">Az alábbi hello adatainak megadása:</span><span class="sxs-lookup"><span data-stu-id="920de-136">Provide hello info below:</span></span>

> <span data-ttu-id="920de-137">Vállalat neve:</span><span class="sxs-lookup"><span data-stu-id="920de-137">Company Name:</span></span>
> 
> <span data-ttu-id="920de-138">Vállalati webhely:</span><span class="sxs-lookup"><span data-stu-id="920de-138">Company Website:</span></span>
> 
> <span data-ttu-id="920de-139">Alkalmazás neve:</span><span class="sxs-lookup"><span data-stu-id="920de-139">Application Name:</span></span>
> 
> <span data-ttu-id="920de-140">Az alkalmazás leírása (200 karakter lehet):</span><span class="sxs-lookup"><span data-stu-id="920de-140">Application Description (200 character limit):</span></span>
> 
> <span data-ttu-id="920de-141">Alkalmazás webhely (tájékoztató):</span><span class="sxs-lookup"><span data-stu-id="920de-141">Application Website (informational):</span></span>
> 
> <span data-ttu-id="920de-142">Alkalmazás technikai támogatási webhely vagy kapcsolattartási adatokat:</span><span class="sxs-lookup"><span data-stu-id="920de-142">Application Technical Support Website or Contact Info:</span></span>
> 
> <span data-ttu-id="920de-143">Alkalmazás Sign-Up URL-Címének toosign a felhasználók hová és/vagy beszerzési hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="920de-143">Application Sign-Up URL where customers go toosign up for and /or purchase hello application:</span></span>
> 
> <span data-ttu-id="920de-144">Válassza ki a felsorolt alkalmazások toobe toothree kategóriáit be (További kategóriák: hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span><span class="sxs-lookup"><span data-stu-id="920de-144">Choose up toothree categories for your application toobe listed under (for available categories see hello [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):</span></span>
> 
> <span data-ttu-id="920de-145">Csatolja az alkalmazás kis ikon (PNG-fájlt, 45px 45px, teli háttérszíne szerint):</span><span class="sxs-lookup"><span data-stu-id="920de-145">Attach Application Small Icon (PNG file, 45px by 45px, solid background color):</span></span>
> 
> <span data-ttu-id="920de-146">Csatolja az alkalmazás nagy méretben (PNG-fájlt, 215px 215px, teli háttérszíne szerint):</span><span class="sxs-lookup"><span data-stu-id="920de-146">Attach Application Large Icon (PNG file, 215px by 215px, solid background color):</span></span>
> 
> <span data-ttu-id="920de-147">Csatolja az alkalmazás embléma (PNG-fájlt, 122px, háttérszínt által 150px):</span><span class="sxs-lookup"><span data-stu-id="920de-147">Attach Application Logo (PNG file, 150px by 122px, transparent background color):</span></span>
> 
> 

