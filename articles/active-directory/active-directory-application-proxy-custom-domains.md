---
title: "az Azure AD alkalmazásproxy aaaCustom tartományok |} Microsoft Docs"
description: "Egyéni tartományok az Azure AD alkalmazásproxy kezelését hello app hello URL van hello azonos függetlenül, hogy a felhasználók elérhetik azt."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7a433c411976077210a2435c3c087991c7430755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a><span data-ttu-id="216b9-103">Egyéni tartományok az Azure AD alkalmazásproxy használata</span><span class="sxs-lookup"><span data-stu-id="216b9-103">Working with custom domains in Azure AD Application Proxy</span></span>

<span data-ttu-id="216b9-104">Amikor közzétesz egy alkalmazást az Azure Active Directory-proxyn keresztül történő, a felhasználók toogo toowhen azok távolról dolgozik létrehozása egy külső URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="216b9-104">When you publish an application through Azure Active Directory Application Proxy, you create an external URL for your users toogo toowhen they're working remotely.</span></span> <span data-ttu-id="216b9-105">Az URL-cím beolvasása hello alapértelmezett tartomány *yourtenant.msappproxy.net*.</span><span class="sxs-lookup"><span data-stu-id="216b9-105">This URL gets hello default domain *yourtenant.msappproxy.net*.</span></span> <span data-ttu-id="216b9-106">Például ha közzétett alkalmazás nevű költségek és a bérlő neve Contoso, majd hello külső URL-cím https://expenses-contoso.msappproxy.net lenne.</span><span class="sxs-lookup"><span data-stu-id="216b9-106">For example, if you published an app named Expenses and your tenant is named Contoso, then hello external URL would be https://expenses-contoso.msappproxy.net.</span></span> <span data-ttu-id="216b9-107">Ha saját tartománynevét szeretné toouse, az alkalmazás az egyéni tartománynév konfigurálása</span><span class="sxs-lookup"><span data-stu-id="216b9-107">If you want toouse your own domain name, configure a custom domain for your application.</span></span> 

<span data-ttu-id="216b9-108">Azt javasoljuk, hogy állít be egyéni tartományok az alkalmazások, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="216b9-108">We recommend that you set up custom domains for your applications whenever possible.</span></span> <span data-ttu-id="216b9-109">Egyéni tartományok hello előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="216b9-109">Some of hello benefits of custom domains include:</span></span>

- <span data-ttu-id="216b9-110">A felhasználók férhetnek toohello alkalmazás hello URL-CÍMÉRE, hogy működnek-belül vagy kívül a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="216b9-110">Your users can get toohello application with hello same URL, whether they are working inside or outside of your network.</span></span>
- <span data-ttu-id="216b9-111">Ha az alkalmazásokat kell hello azonos belső és külső URL-címeket, mutató hivatkozások egy alkalmazás tooanother hello vállalati hálózaton kívül is toowork továbbra is.</span><span class="sxs-lookup"><span data-stu-id="216b9-111">If all of your applications have hello same internal and external URLs, then links in one application that point tooanother continue toowork even outside hello corporate network.</span></span> 
- <span data-ttu-id="216b9-112">A vállalati arculat szabályozza, és hello URL-címeket szeretne létrehozni.</span><span class="sxs-lookup"><span data-stu-id="216b9-112">You control your branding, and create hello URLs you want.</span></span> 


## <a name="configure-a-custom-domain"></a><span data-ttu-id="216b9-113">Az egyéni tartománynév konfigurálása</span><span class="sxs-lookup"><span data-stu-id="216b9-113">Configure a custom domain</span></span>

### <a name="prerequisites"></a><span data-ttu-id="216b9-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="216b9-114">Prerequisites</span></span>

<span data-ttu-id="216b9-115">Az egyéni tartománynév konfigurálása előtt győződjön meg arról, hogy rendelkezik-e hello előkészített követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="216b9-115">Before you configure a custom domain, make sure that you have hello following requirements prepared:</span></span> 
- <span data-ttu-id="216b9-116">A [ellenőrzött tartomány hozzáadása az Active Directory tooAzure](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="216b9-116">A [verified domain added tooAzure Active Directory](active-directory-domains-add-azure-portal.md).</span></span>
- <span data-ttu-id="216b9-117">Egyéni tanúsítvány hello tartomány, a PFX-fájl hello formában.</span><span class="sxs-lookup"><span data-stu-id="216b9-117">A custom certificate for hello domain, in hello form of a PFX file.</span></span> 
- <span data-ttu-id="216b9-118">A helyszíni alkalmazások [proxyn keresztül történő közzétett](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="216b9-118">An on-premises app [published through Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

### <a name="configure-your-custom-domain"></a><span data-ttu-id="216b9-119">Az egyéni tartomány konfigurálása</span><span class="sxs-lookup"><span data-stu-id="216b9-119">Configure your custom domain</span></span>

<span data-ttu-id="216b9-120">Ha készen áll a három követelményekről, kövesse a lépéseket tooset be az egyéni tartomány:</span><span class="sxs-lookup"><span data-stu-id="216b9-120">When you have those three requirements ready, follow these steps tooset up your custom domain:</span></span>

1. <span data-ttu-id="216b9-121">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="216b9-121">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="216b9-122">Keresse meg a túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** hello alkalmazást válasszon, és azt szeretné, hogy toomanage.</span><span class="sxs-lookup"><span data-stu-id="216b9-122">Navigate too**Azure Active Directory** > **Enterprise applications** > **All applications** and choose hello app you want toomanage.</span></span>
3. <span data-ttu-id="216b9-123">Válassza ki **alkalmazásproxy**.</span><span class="sxs-lookup"><span data-stu-id="216b9-123">Select **Application Proxy**.</span></span> 
4. <span data-ttu-id="216b9-124">Hello külső URL-cím mezőben az egyéni tartomány hello legördülő lista tooselect használja.</span><span class="sxs-lookup"><span data-stu-id="216b9-124">In hello External URL field, use hello dropdown list tooselect your custom domain.</span></span> <span data-ttu-id="216b9-125">Ha nem látja a tartomány hello listában, majd azt még nem nincs ellenőrizve még.</span><span class="sxs-lookup"><span data-stu-id="216b9-125">If you don't see your domain in hello list, then it hasn't been verified yet.</span></span> 
5. <span data-ttu-id="216b9-126">Válassza ki **mentése**</span><span class="sxs-lookup"><span data-stu-id="216b9-126">Select **Save**</span></span>
5. <span data-ttu-id="216b9-127">Hello **tanúsítvány** letiltott mező aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="216b9-127">hello **Certificate** field that was disabled becomes enabled.</span></span> <span data-ttu-id="216b9-128">Válassza ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="216b9-128">Select this field.</span></span> 

   ![Kattintson a tooupload tanúsítvány](./media/active-directory-application-proxy-custom-domains/certificate.png)

   <span data-ttu-id="216b9-130">Ha már feltöltött tanúsítványt a tartomány, a hello tanúsítványmezőt hello tanúsítvány adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="216b9-130">If you already uploaded a certificate for this domain, hello Certificate field displays hello certificate information.</span></span> 

6. <span data-ttu-id="216b9-131">Hello PFX-tanúsítvány feltöltése, és írja be a hello jelszót hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="216b9-131">Upload hello PFX certificate and enter hello password for hello certificate.</span></span> 
7. <span data-ttu-id="216b9-132">Válassza ki **mentése** toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="216b9-132">Select **Save** toosave your changes.</span></span> 
8. <span data-ttu-id="216b9-133">Adja hozzá a [DNS-rekord](../dns/dns-operations-recordsets-portal.md) , hogy az átirányítások hello URL-cím toohello új külső msappproxy.net tartomány.</span><span class="sxs-lookup"><span data-stu-id="216b9-133">Add a [DNS record](../dns/dns-operations-recordsets-portal.md) that redirects hello new external URL toohello msappproxy.net domain.</span></span> 

>[!TIP] 
><span data-ttu-id="216b9-134">Csak akkor kell egy tanúsítványt tooupload egyéni tartományonként.</span><span class="sxs-lookup"><span data-stu-id="216b9-134">You only need tooupload one certificate per custom domain.</span></span> <span data-ttu-id="216b9-135">Tanúsítvány feltöltése után kiválaszthatja hello egyéni tartomány, amikor egy új alkalmazás közzététele, és nincs további konfigurációs toodo hello DNS-rekord kivételével.</span><span class="sxs-lookup"><span data-stu-id="216b9-135">Once you upload a certificate, you can choose hello custom domain when you publish a new app and not have toodo additional configuration except for hello DNS record.</span></span> 

## <a name="manage-certificates"></a><span data-ttu-id="216b9-136">Tanúsítványok kezelése</span><span class="sxs-lookup"><span data-stu-id="216b9-136">Manage certificates</span></span>

### <a name="certificate-format"></a><span data-ttu-id="216b9-137">A tanúsítvány formátuma</span><span class="sxs-lookup"><span data-stu-id="216b9-137">Certificate format</span></span>
<span data-ttu-id="216b9-138">Nincs a tanúsítvány-aláírás módszerek hello korlátozva.</span><span class="sxs-lookup"><span data-stu-id="216b9-138">There is no restriction on hello certificate signature methods.</span></span> <span data-ttu-id="216b9-139">Elliptikus görbe alapú titkosítást (ECC), a tulajdonos alternatív nevére (SAN) és a közös tanúsítvány egyéb használatát támogatja.</span><span class="sxs-lookup"><span data-stu-id="216b9-139">Elliptic Curve Cryptography (ECC), Subject Alternative Name (SAN), and other common certificate types are all supported.</span></span> 

<span data-ttu-id="216b9-140">Helyettesítő tanúsítvány használható, amíg hello helyettesítő hello szükséges külső URL-címe megegyezik.</span><span class="sxs-lookup"><span data-stu-id="216b9-140">You can use a wildcard certificate as long as hello wildcard matches hello desired external URL.</span></span> 

<span data-ttu-id="216b9-141">Önaláírt tanúsítványok is használható.</span><span class="sxs-lookup"><span data-stu-id="216b9-141">You can use self-signed certificates, as well.</span></span> <span data-ttu-id="216b9-142">Ha a titkos hitelesítésszolgáltatót használ, hello CDP (tanúsítvány-visszavonási pont terjesztési pont) hello tanúsítvány nyilvános kell lennie.</span><span class="sxs-lookup"><span data-stu-id="216b9-142">If you’re using a private certificate authority, hello CDP (certificate revocation point distribution point) for hello certificate should be public.</span></span>

### <a name="changing-hello-domain"></a><span data-ttu-id="216b9-143">Hello tartomány módosítása</span><span class="sxs-lookup"><span data-stu-id="216b9-143">Changing hello domain</span></span>
<span data-ttu-id="216b9-144">Hello külső URL-cím legördülő lista az alkalmazás összes ellenőrzött tartományok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="216b9-144">All verified domains appear in hello External URL dropdown list for your application.</span></span> <span data-ttu-id="216b9-145">toochange hello tartomány, csak olyan frissítés, amely hello alkalmazás mezőben.</span><span class="sxs-lookup"><span data-stu-id="216b9-145">toochange hello domain, just update that field for hello application.</span></span> <span data-ttu-id="216b9-146">Ha hello listán, nem kívánt hello tartomány [ellenőrzött tartományt adja hozzá azt](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="216b9-146">If hello domain you want isn't in hello list, [add it as a verified domain](active-directory-domains-add-azure-portal.md).</span></span> <span data-ttu-id="216b9-147">Ha még nem rendelkezik társított tanúsítvány egy tartományba, kövesse a lépéseket 5-7 tooadd hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="216b9-147">If you select a domain that doesn't have an associated certificate yet, follow steps 5-7 tooadd hello certificate.</span></span> <span data-ttu-id="216b9-148">Ezt követően ne hello DNS rekord tooredirect hello új külső URL-címről.</span><span class="sxs-lookup"><span data-stu-id="216b9-148">Then, make sure you update hello DNS record tooredirect from hello new external URL.</span></span> 

### <a name="certificate-management"></a><span data-ttu-id="216b9-149">Tanúsítványkezelés</span><span class="sxs-lookup"><span data-stu-id="216b9-149">Certificate management</span></span>
<span data-ttu-id="216b9-150">Azonos tanúsítvány egyszerre több alkalmazás, kivéve, ha hello alkalmazások megosztott egy külső gazdagép hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="216b9-150">You can use hello same certificate for multiple applications unless hello applications share an external host.</span></span> 

<span data-ttu-id="216b9-151">Megjelenik egy figyelmeztetés, ha a tanúsítvány lejár, felszólító tooupload hello portálon keresztül másik tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="216b9-151">You get a warning when a certificate expires, telling you tooupload another certificate through hello portal.</span></span> <span data-ttu-id="216b9-152">Ha hello tanúsítvány visszavonása esetén előfordulhat, hogy jelennek meg a felhasználók biztonsági figyelmeztetés hello alkalmazáshoz való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="216b9-152">If hello certificate is revoked, your users may see a security warning when accessing hello application.</span></span> <span data-ttu-id="216b9-153">A tanúsítványok visszavonási ellenőrzést nem végezzük.</span><span class="sxs-lookup"><span data-stu-id="216b9-153">We don’t perform revocation checks for certificates.</span></span>  <span data-ttu-id="216b9-154">tooupdate hello tanúsítvány egy adott alkalmazáshoz, keresse meg a toohello alkalmazás, és utasításai 5-7 egyéni tartományok konfigurálása a közzétett alkalmazások tooupload egy új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="216b9-154">tooupdate hello certificate for a given application, navigate toohello application and follow steps 5-7 for configuring custom domains on published applications tooupload a new certificate.</span></span> <span data-ttu-id="216b9-155">Ha hello régi tanúsítvány más alkalmazás nem használatban van, az automatikusan törlődik.</span><span class="sxs-lookup"><span data-stu-id="216b9-155">If hello old certificate is not being used by other applications, it is deleted automatically.</span></span> 

<span data-ttu-id="216b9-156">Minden Tanúsítványkezelő jelenleg egyes lapjain keresztül így toomanage tanúsítványok hello vonatkozó kérelmek hello környezetében kell.</span><span class="sxs-lookup"><span data-stu-id="216b9-156">Currently all certificate management is through individual application pages so you need toomanage certificates in hello context of hello relevant applications.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="216b9-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="216b9-157">Next steps</span></span>
* <span data-ttu-id="216b9-158">[Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md) tooyour közzétett alkalmazást az Azure AD-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="216b9-158">[Enable single sign-on](active-directory-application-proxy-sso-using-kcd.md) tooyour published apps with Azure AD authentication.</span></span>
* <span data-ttu-id="216b9-159">[A feltételes hozzáférés engedélyezése](active-directory-application-proxy-conditional-access.md) tooyour közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="216b9-159">[Enable conditional access](active-directory-application-proxy-conditional-access.md) tooyour published apps.</span></span>
* [<span data-ttu-id="216b9-160">Az egyéni tartomány nevét tooAzure AD hozzá</span><span class="sxs-lookup"><span data-stu-id="216b9-160">Add your custom domain name tooAzure AD</span></span>](active-directory-domains-add-azure-portal.md)


