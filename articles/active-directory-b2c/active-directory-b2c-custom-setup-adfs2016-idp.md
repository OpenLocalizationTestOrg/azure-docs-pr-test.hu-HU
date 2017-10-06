---
title: "Az Azure Active Directory B2C: Adja hozzá az AD FS-t egyéni házirendekkel SAML-Identitásszolgáltatóként"
description: "Hogyan-tooarticle a SAML protokoll és az egyéni házirendek használata az AD FS 2016 beállítása"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 30fb7700e7834e3d91fab1fc1b169b761584b204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a><span data-ttu-id="1d9b1-103">Az Azure Active Directory B2C: Adja hozzá az AD FS-t egyéni házirendekkel SAML-Identitásszolgáltatóként</span><span class="sxs-lookup"><span data-stu-id="1d9b1-103">Azure Active Directory B2C: Add ADFS as a SAML identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="1d9b1-104">Ez a cikk bemutatja, hogyan tooenable bejelentkezhet a felhasználók hello használata az AD FS-fiókból [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1d9b1-104">This article shows you how tooenable sign-in for users from ADFS account through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d9b1-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d9b1-105">Prerequisites</span></span>

<span data-ttu-id="1d9b1-106">Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="1d9b1-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-107">These steps include:</span></span>

1.  <span data-ttu-id="1d9b1-108">Az AD FS megbízható függő entitás megbízhatóságának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-108">Creating an ADFS Relying Party Trust.</span></span>
2.  <span data-ttu-id="1d9b1-109">Hello az AD FS megbízható függő entitás megbízhatóságának tanúsítvány tooAzure AD B2C hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-109">Adding hello ADFS Relying Party Trust certificate tooAzure AD B2C.</span></span>
3.  <span data-ttu-id="1d9b1-110">Jogcím-szolgáltató tooa házirend hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-110">Adding claims provider tooa policy.</span></span>
4.  <span data-ttu-id="1d9b1-111">Regisztrálásakor hello az AD FS fiók jogcím-szolgáltató tooa felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-111">Registering hello ADFS account claims provider tooa user journey.</span></span>
5.  <span data-ttu-id="1d9b1-112">Hello házirend tooan az Azure AD B2C feltöltése a bérlői és tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-112">Uploading hello policy tooan Azure AD B2C tenant and test it.</span></span>

## <a name="toocreate-a-claims-aware-relying-party-trust"></a><span data-ttu-id="1d9b1-113">a jogcímeket figyelembe vevő Függőentitás-megbízhatóság toocreate</span><span class="sxs-lookup"><span data-stu-id="1d9b1-113">toocreate a claims-aware Relying Party Trust</span></span>

<span data-ttu-id="1d9b1-114">az AD FS toouse identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, az AD FS megbízható függő entitás megbízhatóságának toocreate kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-114">toouse ADFS as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate an ADFS Relying Party Trust and supply it with hello right parameters.</span></span>

<span data-ttu-id="1d9b1-115">egy új függő entitás hello AD FS felügyelet beépülő moduljában a megbízható, és hello-beállítások manuális konfigurálásával tooadd összevonási kiszolgálók és az eljárást követő hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-115">tooadd a new relying party trust by using hello AD FS Management snap-in and manually configure hello settings, perform hello following procedure on a federation server.</span></span>

<span data-ttu-id="1d9b1-116">Tagság a **rendszergazdák**, vagy ezzel egyenértékű hello helyi számítógépen hello minimálisan szükséges toocomplete ezt az eljárást.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-116">Membership in **Administrators**, or equivalent, on hello local computer is hello minimum required toocomplete this procedure.</span></span> <span data-ttu-id="1d9b1-117">Olvashat hello megfelelő fiókok és csoporttagságok [alapértelmezett helyi és tartományi csoportok](http://go.microsoft.com/fwlink/?LinkId=83477)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-117">Review details about using hello appropriate accounts and group memberships at [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)</span></span>

1.  <span data-ttu-id="1d9b1-118">A Kiszolgálókezelőben kattintson **eszközök**, majd válassza ki **az AD FS felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-118">In Server Manager, click **Tools**, and then select **ADFS Management**.</span></span>

2.  <span data-ttu-id="1d9b1-119">Kattintson a **függő entitás megbízhatóságának hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-119">Click on **Add Relying Party Trust**.</span></span>
    <span data-ttu-id="1d9b1-120">![Függő entitás megbízhatóságának hozzáadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-120">![Add Relying Party Trust](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)</span></span>

3.  <span data-ttu-id="1d9b1-121">A hello **üdvözlő** lapon, válassza ki **kompatibilis jogcím** kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-121">On hello **Welcome** page, choose **Claims aware** and click **Start**.</span></span>
    <span data-ttu-id="1d9b1-122">![A hello kezdőlapján válassza a jogcímek használatát](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-122">![On hello Welcome page, choose Claims aware](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)</span></span>
4.  <span data-ttu-id="1d9b1-123">A hello **adatforrás kiválasztása** kattintson **hello függő entitással kapcsolatos adatok manuális megadása**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-123">On hello **Select Data Source** page, click **Enter data about hello relying party manually**, and then click **Next**.</span></span>
    <span data-ttu-id="1d9b1-124">![Hello függő entitással kapcsolatos adatok megadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-124">![Enter data about hello relying party](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)</span></span>

5.  <span data-ttu-id="1d9b1-125">A hello **megjelenített név meghatározása** írja be egy nevet a **megjelenített név**a **megjegyzések** írja be a függő entitás megbízhatóságának leírását, és kattintson **tovább** .</span><span class="sxs-lookup"><span data-stu-id="1d9b1-125">On hello **Specify Display Name** page, type a name in **Display name**, under **Notes** type a description for this relying party trust, and then click **Next**.</span></span>
    <span data-ttu-id="1d9b1-126">![Adja meg a megjelenítendő nevek és megjegyzések](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-126">![Specify Display Name and notes](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)</span></span>
6.  <span data-ttu-id="1d9b1-127">Választható.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-127">Optional.</span></span> <span data-ttu-id="1d9b1-128">Ha van egy nem kötelező jogkivonat titkosítási tanúsítványt, majd hello **tanúsítvány konfigurálása** kattintson **Tallózás** toolocate a tanúsítványfájlt, és kattintson **következő** .</span><span class="sxs-lookup"><span data-stu-id="1d9b1-128">If you have an optional token encryption certificate, then on hello **Configure Certificate** page, click **Browse** toolocate your certificate file, and then click **Next**.</span></span>
    <span data-ttu-id="1d9b1-129">![Tanúsítvány konfigurálása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-129">![Configure Certificate](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)</span></span>
7.  <span data-ttu-id="1d9b1-130">A hello **URL konfigurálása** lapra, jelölje be hello **hello SAML 2.0 WebSSO-protokoll támogatásának engedélyezése a** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-130">On hello **Configure URL** page, select hello **Enable support for hello SAML 2.0 WebSSO protocol** check box.</span></span> <span data-ttu-id="1d9b1-131">A **függő entitás SAML 2.0 SSO szolgáltatás URL-címe**, írja be a hello Security Assertion Markup Language (SAML) szolgáltatás végponti URL-Címének függő entitás megbízhatóságához, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-131">Under **Relying party SAML 2.0 SSO service URL**, type hello Security Assertion Markup Language (SAML) service endpoint URL for this relying party trust, and then click **Next**.</span></span>  <span data-ttu-id="1d9b1-132">A hello **függő entitás SAML 2.0 SSO szolgáltatás URL-címe**, illessze be a hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-132">For hello **Relying party SAML 2.0 SSO service URL**, paste hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`.</span></span> <span data-ttu-id="1d9b1-133">Cserélje le a bérlő neve (például contosob2c.onmicrosoft.com) {tenant} és {házirend} hello cserélje le a bővítményeket házirend nevét (például B2C_1A_TrustFrameworkExtensions).</span><span class="sxs-lookup"><span data-stu-id="1d9b1-133">Replace {tenant} with your tenant's name (for example, contosob2c.onmicrosoft.com), and replace hello {policy} with your extensions policy name (for example, B2C_1A_TrustFrameworkExtensions).</span></span>
    > [!IMPORTANT]
    ><span data-ttu-id="1d9b1-134">hello házirendnév lehet hello egy öröklő signup_or_signin házirend, ebben az esetben: `B2C_1A_TrustFrameworkExtensions`.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-134">hello policy name  is hello one that signup_or_signin policy inherits from, in this case it is: `B2C_1A_TrustFrameworkExtensions`.</span></span>
    ><span data-ttu-id="1d9b1-135">Például hello URL-cím lehet: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-135">For example hello URL could be:   https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.</span></span>

    ![Függő entitás SAML 2.0 SSO URL-címe](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. <span data-ttu-id="1d9b1-137">A hello **azonosítók konfigurálása** adja meg azokat a hello kattintson az előző lépésben hello URL-CÍMÉRE **hozzáadása** tooadd őket toohello listára, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-137">On hello **Configure Identifiers** page, specify hello same URL as hello previous step, click **Add** tooadd them toohello list, and then click **Next**.</span></span>
    <span data-ttu-id="1d9b1-138">![Függő entitások megbízhatósági azonosítóinak](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-138">![Relying party trust identifiers](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)</span></span>
9.  <span data-ttu-id="1d9b1-139">A hello **hozzáférés-vezérlési szabályzat kiválasztása** jelöljön ki egy házirendet, majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-139">On hello **Choose Access Control Policy** select a policy and click **Next**.</span></span>
    <span data-ttu-id="1d9b1-140">![Válassza ki a hozzáférés-vezérlési házirend](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-140">![Choose Access Control Policy](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)</span></span>
10.  <span data-ttu-id="1d9b1-141">A hello **készen áll a megbízhatóság tooAdd** lapon ellenőrizze a hello beállításokat, és kattintson a **következő** toosave a függő entitás megbízhatóságának adatait.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-141">On hello **Ready tooAdd Trust** page, review hello settings, and then click **Next** toosave your relying party trust information.</span></span>
    <span data-ttu-id="1d9b1-142">![Mentse a függő entitás megbízhatósági adatok](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-142">![Save your relying party trust information](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)</span></span>
11.  <span data-ttu-id="1d9b1-143">A hello **Befejezés** kattintson **Bezárás**, ez a művelet automatikusan megjeleníti a hello **Jogcímszabályok szerkesztése** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-143">On hello **Finish** page, click **Close**, this action automatically displays hello **Edit Claim Rules** dialog box.</span></span>
    <span data-ttu-id="1d9b1-144">![Jogcímszabályok szerkesztése](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-144">![Edit Claim Rules](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)</span></span>
12. <span data-ttu-id="1d9b1-145">Kattintson a **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-145">Click **Add Rule**.</span></span>  
      <span data-ttu-id="1d9b1-146">![Új szabály hozzáadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-146">![Add new rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)</span></span>
13.  <span data-ttu-id="1d9b1-147">A **Jogcímszabály-sablon**, jelölje be **LDAP attribútumok küldése jogcímekként**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-147">In **Claim rule template**, select **Send LDAP attributes as claims**.</span></span>
    <span data-ttu-id="1d9b1-148">![Válassza ki az LDAP attribútumok küldése jogcímszabályként sablon](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-148">![Select Send LDAP attributes as claims template rule](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)</span></span>
14.  <span data-ttu-id="1d9b1-149">Adjon meg **Jogcímszabály nevének**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-149">Provide **Claim rule name**.</span></span> <span data-ttu-id="1d9b1-150">A hello **attribútumtár** válasszon **válassza ki az Active Directory** adja hozzá a következő jogcímeket hello, majd kattintson az **Befejezés** és **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-150">For hello **Attribute store** select **Select Active Directory** Add hello following claims, then click **Finish** and **OK**.</span></span>
    <span data-ttu-id="1d9b1-151">![A szabály tulajdonságainak beállítása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-151">![Set rule properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)</span></span>
15.  <span data-ttu-id="1d9b1-152">A Kiszolgálókezelő ablakában válassza **függő entitás Megbízhatóságai** majd jelöljön ki hello létrehozott függő entitás megbízhatóságához, majd **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-152">In Server Manager, select **Relying Party Trusts** then select hello relying party trust you created and click **Properties**.</span></span>
    <span data-ttu-id="1d9b1-153">![Függő entitás tulajdonságok szerkesztése](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-153">![Relying party edit properties](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)</span></span>
16.  <span data-ttu-id="1d9b1-154">Egy függő entitás megbízhatósági (B2C bemutató) tulajdonságai ablakban kattintson hello **aláírás** fülre, és kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-154">One hello relying party trust (B2C Demo) properties window click **Signature** tab and click **Add**.</span></span>  
    <span data-ttu-id="1d9b1-155">![Set-aláírás](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-155">![Set signature](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)</span></span>
17.  <span data-ttu-id="1d9b1-156">Adja hozzá az aláírási tanúsítvány (.cert fájlt, titkos kulcs nélkül).</span><span class="sxs-lookup"><span data-stu-id="1d9b1-156">Add your signature certificate (.cert file, without private key).</span></span>  
    ![Az aláírási tanúsítvány hozzáadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  <span data-ttu-id="1d9b1-158">A hello függő entitás megbízhatósági (B2C bemutató) tulajdonságai ablakban kattintson a **speciális** lapon, és módosítsa a hello **biztonságos kivonatoló algoritmus** túl**SHA-1**, kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-158">On hello relying party trust (B2C Demo) properties window click **Advanced** tab and change hello **Secure hash algorithm** too**SHA-1**, Click **Ok**.</span></span>  
    <span data-ttu-id="1d9b1-159">![Állítsa be a biztonságos kivonatoló algoritmus tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-159">![Set secure hash algorithm tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)</span></span>

## <a name="add-hello-adfs-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="1d9b1-160">Hello az AD FS fiók alkalmazás fő tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d9b1-160">Add hello ADFS account application key tooAzure AD B2C</span></span>
<span data-ttu-id="1d9b1-161">Összevonás az AD FS-fiókok egy ügyfélkulcsot az AD FS fiók tootrust az Azure AD B2C hello alkalmazás nevében szükséges.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-161">Federation with ADFS accounts requires a client secret for ADFS account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="1d9b1-162">Tanúsítványra van szükség toostore az AD FS az Azure AD B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-162">You need toostore your ADFS certificate in your Azure AD B2C tenant.</span></span> 

1.  <span data-ttu-id="1d9b1-163">Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="1d9b1-163">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="1d9b1-164">Válassza ki **házirend kulcsok** tooview hello kulcsok, amely az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-164">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="1d9b1-165">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-165">Click **+Add**.</span></span>
4.  <span data-ttu-id="1d9b1-166">A **beállítások**, használjon **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-166">For **Options**, use **Upload**.</span></span>
5.  <span data-ttu-id="1d9b1-167">A **neve**, használjon `ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-167">For **Name**, use `ADFSSamlCert`.</span></span>  
    <span data-ttu-id="1d9b1-168">hello előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-168">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="1d9b1-169">Hello fájl feltöltése, a ** válassza ki a .pfx-fájl titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-169">In hello File upload,** select your certificate .pfx file with private key.</span></span> <span data-ttu-id="1d9b1-170">Megjegyzés: ezt a tanúsítványt (hello titkos kulcsával) hello azonos azt, amelyik hello az AD FS függő entitásnak történő kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-170">Note: this certificate (with hello private key) should be hello same one that issued and used for hello ADFS relying party.</span></span>
<span data-ttu-id="1d9b1-171">![Töltse fel a házirend-kulcs](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span><span class="sxs-lookup"><span data-stu-id="1d9b1-171">![Upload policy key](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)</span></span>
7.  <span data-ttu-id="1d9b1-172">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="1d9b1-172">Click **Create**</span></span>
8.  <span data-ttu-id="1d9b1-173">Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_ADFSSamlCert`.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-173">Confirm that you've created hello key `B2C_1A_ADFSSamlCert`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="1d9b1-174">A jogcím-szolgáltató hozzáadása a bővítmény házirend</span><span class="sxs-lookup"><span data-stu-id="1d9b1-174">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="1d9b1-175">Ha azt szeretné, felhasználók toosign AD FS-fiókkal, toodefine az AD FS-fiók szükséges, a Jogcímszolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-175">If you want users toosign in by using ADFS account, you need toodefine ADFS account as a claims provider.</span></span> <span data-ttu-id="1d9b1-176">Más szóval kell toospecify olyan végponttal, amely az Azure AD B2C-vel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-176">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="1d9b1-177">hello végpontja biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-177">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="1d9b1-178">Az AD FS meghatározni, a Jogcímszolgáltatók hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-178">Define ADFS as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1. <span data-ttu-id="1d9b1-179">A munkakönyvtár hello bővítmény házirendfájl (TrustFrameworkExtensions.xml) megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-179">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="1d9b1-180">Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-180">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2. <span data-ttu-id="1d9b1-181">Hello található `<ClaimsProviders>` szakasz</span><span class="sxs-lookup"><span data-stu-id="1d9b1-181">Find hello `<ClaimsProviders>` section</span></span>
3. <span data-ttu-id="1d9b1-182">Adja hozzá a következő XML-részletet a hello hello `ClaimsProviders` elem és a név felülírandó `identityProvider` a DNS-beli (tetszőleges érték, amely jelzi a tartomány), és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-182">Add hello following XML snippet under hello `ClaimsProviders` element and replace `identityProvider` with your DNS (Arbitrary value that indicates your domain), and save hello file.</span></span> 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-adfs-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="1d9b1-183">Hello az AD FS fiók jogcímeket szolgáltató tooSign regisztrálásához fel, vagy jelentkezzen be a felhasználó út</span><span class="sxs-lookup"><span data-stu-id="1d9b1-183">Register hello ADFS account claims provider tooSign up or Sign in user journey</span></span>
<span data-ttu-id="1d9b1-184">Ezen a ponton hello identitásszolgáltató be van állítva.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-184">At this point, hello identity provider has been set up.</span></span>  <span data-ttu-id="1d9b1-185">Azonban nincs sem hello sign-Close-Up/sign-in képernyők áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-185">However, it is not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="1d9b1-186">Most tooadd hello az AD FS fiók identitását szolgáltató tooyour felhasználói `SignUpOrSignIn` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-186">Now you need tooadd hello ADFS account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="1d9b1-187">toomake akkor érhető el, egy meglévő sablon felhasználói út duplikált létrehozhatunk.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-187">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="1d9b1-188">Ezt követően azt módosítja azt, hello az AD FS identitásszolgáltató tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-188">Then, we modify it so it includes hello ADFS identity provider:</span></span>
    >[!NOTE]
    >If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  <span data-ttu-id="1d9b1-189">Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="1d9b1-189">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="1d9b1-190">Hello található `<UserJourneys>` elemet, és másolja hello teljes tartalma `<UserJourneys>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-190">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="1d9b1-191">Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-191">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="1d9b1-192">Ha hello elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-192">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="1d9b1-193">Illessze be a teljes tartalma hello `<UserJournesy>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-193">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="1d9b1-194">Megjelenítési hello gomb</span><span class="sxs-lookup"><span data-stu-id="1d9b1-194">Display hello button</span></span>
<span data-ttu-id="1d9b1-195">Hello `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások hello listáját és sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-195">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="1d9b1-196">`<ClaimsProviderSelection>`az elem hasonló tooan identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-196">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="1d9b1-197">Ha ad hozzá egy `<ClaimsProviderSelection>` elem az AD FS-fiók, egy új gomb megjelenik, amikor egy felhasználó fájljai hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-197">If you add a `<ClaimsProviderSelection>` element for  ADFS account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="1d9b1-198">tooadd ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-198">tooadd this element:</span></span>

1.  <span data-ttu-id="1d9b1-199">Hello található `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a másolt hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-199">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="1d9b1-200">Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="1d9b1-200">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="1d9b1-201">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-201">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="1d9b1-202">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="1d9b1-202">Link hello button tooan action</span></span>

<span data-ttu-id="1d9b1-203">Most, hogy a gomb helyen, toolink kell azt tooan művelet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-203">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="1d9b1-204">hello művelet, ebben az esetben, akkor az AD FS fiók tooreceive az Azure AD B2C toocommunicate jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-204">hello action, in this case, is for Azure AD B2C toocommunicate with ADFS account tooreceive a token.</span></span> <span data-ttu-id="1d9b1-205">Hivatkozás hello tooan megnyomásakor létrehozhatja, ha az AD FS fiók jogcímeket szolgáltató műszaki hello-profil:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-205">Link hello button tooan action by linking hello technical profile for your ADFS account claims provider:</span></span>

1.  <span data-ttu-id="1d9b1-206">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-206">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="1d9b1-207">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-207">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * <span data-ttu-id="1d9b1-208">Győződjön meg arról hello `Id` hello azonos érték, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-208">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section.</span></span>
> * <span data-ttu-id="1d9b1-209">Győződjön meg arról `TechnicalProfileReferenceId` toohello műszaki profil korábbi (a Contoso-egy SAML2) létrehozott van beállítva.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-209">Ensure `TechnicalProfileReferenceId` is set toohello technical profile you created earlier (Contoso-SAML2).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="1d9b1-210">Hello házirend tooyour bérlői feltöltése</span><span class="sxs-lookup"><span data-stu-id="1d9b1-210">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="1d9b1-211">A hello [Azure-portálon](https://portal.azure.com), hello átváltani [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg hello **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-211">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="1d9b1-212">Válassza ki **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-212">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="1d9b1-213">Nyissa meg hello **házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-213">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="1d9b1-214">Válassza ki **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-214">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="1d9b1-215">Ellenőrizze **hello házirend felülírja, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-215">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="1d9b1-216">**Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello</span><span class="sxs-lookup"><span data-stu-id="1d9b1-216">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="1d9b1-217">Tesztelje hello egyéni házirend segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="1d9b1-217">Test hello custom policy by using Run Now</span></span>
1.  <span data-ttu-id="1d9b1-218">Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-218">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="1d9b1-219">Nyissa meg **B2C_1A_signup_signin**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-219">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="1d9b1-220">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-220">Select **Run now**.</span></span>
3.  <span data-ttu-id="1d9b1-221">Az AD FS fiókkal tud toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-221">You should be able toosign in using ADFS account.</span></span>

## <a name="optional-register-hello-adfs-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="1d9b1-222">[Választható] Hello az AD FS fiók jogcímeket szolgáltató tooProfile szerkesztheti a felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="1d9b1-222">[Optional] Register hello ADFS account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="1d9b1-223">Érdemes lehet tooadd hello az AD FS-fiók identitásszolgáltató is tooyour felhasználói `ProfileEdit` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-223">You may want tooadd hello ADFS account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="1d9b1-224">toomake teheti, hogy ismételje meg a hello utolsó két lépést:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-224">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="1d9b1-225">Megjelenítési hello gomb</span><span class="sxs-lookup"><span data-stu-id="1d9b1-225">Display hello button</span></span>
1.  <span data-ttu-id="1d9b1-226">Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml) házirend.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-226">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="1d9b1-227">Hello található `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a másolt hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-227">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="1d9b1-228">Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="1d9b1-228">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="1d9b1-229">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-229">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="1d9b1-230">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="1d9b1-230">Link hello button tooan action</span></span>
1.  <span data-ttu-id="1d9b1-231">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-231">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="1d9b1-232">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="1d9b1-232">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="1d9b1-233">Egyéni házirend hello a profil-módosítása tesztelése Futtatás most használatával</span><span class="sxs-lookup"><span data-stu-id="1d9b1-233">Test hello custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="1d9b1-234">Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-234">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="1d9b1-235">Nyissa meg **B2C_1A_ProfileEdit**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-235">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="1d9b1-236">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-236">Select **Run now**.</span></span>
3.  <span data-ttu-id="1d9b1-237">Az AD FS fiókkal tud toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-237">You should be able toosign in using ADFS account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="1d9b1-238">Hello teljes házirend fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="1d9b1-238">Download hello complete policy files</span></span>
<span data-ttu-id="1d9b1-239">Nem kötelező: Javasoljuk az egyéni házirendek első lépések útmutató hello befejezése után a saját egyéni házirend fájlok használata forgatókönyv létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1d9b1-239">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through.</span></span> [<span data-ttu-id="1d9b1-240">Házirend mintafájlok csak referenciaként</span><span class="sxs-lookup"><span data-stu-id="1d9b1-240">Policy sample files for reference only</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
