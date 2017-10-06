---
title: "Biztonságos LDAP (LDAPS) az Azure AD tartományi szolgáltatásokban aaaConfigure |} Microsoft Docs"
description: "Biztonságos LDAP (LDAPS) konfigurálása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 99e44d917b115d7f7a67ff0a5e22c5c9165287e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="b999e-103">Biztonságos LDAP (LDAPS) használatos az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz tartozó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b999e-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="b999e-104">Ez a cikk bemutatja, hogyan engedélyezheti biztonságos Lightweight Directory Access Protocol (LDAPS) vonatkozóan az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="b999e-104">This article shows how you can enable Secure Lightweight Directory Access Protocol (LDAPS) for your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="b999e-105">Biztonságos LDAP más néven az "Lightweight Directory Access Protocol (LDAP) Secure Sockets Layer (SSL) rétegen keresztül / Transport Layer Security (TLS)".</span><span class="sxs-lookup"><span data-stu-id="b999e-105">Secure LDAP is also known as 'Lightweight Directory Access Protocol (LDAP) over Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b999e-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b999e-106">Before you begin</span></span>
<span data-ttu-id="b999e-107">a cikkben szereplő tooperform hello feladatok lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b999e-107">tooperform hello tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="b999e-108">Egy érvényes **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="b999e-108">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="b999e-109">Egy **Azure AD-címtár** -vagy egy helyszíni címtár vagy egy csak felhőalapú directory szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="b999e-109">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="b999e-110">**Azure AD tartományi szolgáltatások** hello Azure Active directory engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="b999e-110">**Azure AD Domain Services** must be enabled for hello Azure AD directory.</span></span> <span data-ttu-id="b999e-111">Ha még nem tette meg, kövesse a hello ismertetett feladatok hello [első lépések útmutató](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b999e-111">If you haven't done so, follow all hello tasks outlined in hello [Getting Started guide](active-directory-ds-getting-started.md).</span></span>
4. <span data-ttu-id="b999e-112">A **tanúsítvány használt toobe tooenable biztonságos LDAP**.</span><span class="sxs-lookup"><span data-stu-id="b999e-112">A **certificate toobe used tooenable secure LDAP**.</span></span>

   * <span data-ttu-id="b999e-113">**Ajánlott** -megbízható, nyilvános hitelesítésszolgáltatótól származó tanúsítvány beszerzése.</span><span class="sxs-lookup"><span data-stu-id="b999e-113">**Recommended** - Obtain a certificate from a trusted public certification authority.</span></span> <span data-ttu-id="b999e-114">Ez a beállítás értéke nagyobb biztonságot nyújt.</span><span class="sxs-lookup"><span data-stu-id="b999e-114">This configuration option is more secure.</span></span>
   * <span data-ttu-id="b999e-115">Alternatív megoldásként is választhatja, hogy túl[hozzon létre egy önaláírt tanúsítványt](#task-1---obtain-a-certificate-for-secure-ldap) a cikk későbbi részében látható módon.</span><span class="sxs-lookup"><span data-stu-id="b999e-115">Alternately, you may also choose too[create a self-signed certificate](#task-1---obtain-a-certificate-for-secure-ldap) as shown later in this article.</span></span>

<br>

### <a name="requirements-for-hello-secure-ldap-certificate"></a><span data-ttu-id="b999e-116">Hello biztonságos LDAP tanúsítványra vonatkozó követelményekről</span><span class="sxs-lookup"><span data-stu-id="b999e-116">Requirements for hello secure LDAP certificate</span></span>
<span data-ttu-id="b999e-117">Biztonságos LDAP engedélyezése előtt a következő irányelveket, hello egy érvényes tanúsítványt beszerezni.</span><span class="sxs-lookup"><span data-stu-id="b999e-117">Acquire a valid certificate per hello following guidelines, before you enable secure LDAP.</span></span> <span data-ttu-id="b999e-118">Ha tooenable hibákat észlel LDAP biztonságos a felügyelt tartományok az érvénytelen vagy nem megfelelő tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b999e-118">You encounter failures if you try tooenable secure LDAP for your managed domain with an invalid/incorrect certificate.</span></span>

1. <span data-ttu-id="b999e-119">**A megbízható kiállítók** -toohello biztonságos LDAP segítségével felügyelt tartományhoz csatlakozó számítógépek megbízható hatóság hello tanúsítványt kell kiállítani.</span><span class="sxs-lookup"><span data-stu-id="b999e-119">**Trusted issuer** - hello certificate must be issued by an authority trusted by computers connecting toohello managed domain using secure LDAP.</span></span> <span data-ttu-id="b999e-120">A szolgáltató egy nyilvános hitelesítésszolgáltatót megbízhatónak ezeket a számítógépeket is lehet.</span><span class="sxs-lookup"><span data-stu-id="b999e-120">This authority may be a public certification authority trusted by these computers.</span></span>
2. <span data-ttu-id="b999e-121">**Élettartam** -hello tanúsítvány illeszkedniük kell legalább a következő 3-6 hónapban hello.</span><span class="sxs-lookup"><span data-stu-id="b999e-121">**Lifetime** - hello certificate must be valid for at least hello next 3-6 months.</span></span> <span data-ttu-id="b999e-122">Biztonságos LDAP hozzáférés tooyour által kezelt tartomány megszakad, amikor hello tanúsítványa lejár.</span><span class="sxs-lookup"><span data-stu-id="b999e-122">Secure LDAP access tooyour managed domain is disrupted when hello certificate expires.</span></span>
3. <span data-ttu-id="b999e-123">**Tulajdonos neve** -hello tanúsítvány tulajdonosának neve hello kell lennie a felügyelt tartományok helyettesítő karakter.</span><span class="sxs-lookup"><span data-stu-id="b999e-123">**Subject name** - hello subject name on hello certificate must be a wildcard for your managed domain.</span></span> <span data-ttu-id="b999e-124">Például, ha a tartomány neve "contoso100.com", hello tanúsítvány tulajdonosának neve lehet "*. contoso100.com".</span><span class="sxs-lookup"><span data-stu-id="b999e-124">For instance, if your domain is named 'contoso100.com', hello certificate's subject name must be '*.contoso100.com'.</span></span> <span data-ttu-id="b999e-125">Állítsa be a hello DNS-nevét (tulajdonos alternatív neve) toothis helyettesítő nevét.</span><span class="sxs-lookup"><span data-stu-id="b999e-125">Set hello DNS name (subject alternate name) toothis wildcard name.</span></span>
4. <span data-ttu-id="b999e-126">**Kulcshasználat** - hello tanúsítványt kell beállítani a következő hello használ - digitális aláírásokra és kulcstitkosítás.</span><span class="sxs-lookup"><span data-stu-id="b999e-126">**Key usage** - hello certificate must be configured for hello following uses - Digital signatures and key encipherment.</span></span>
5. <span data-ttu-id="b999e-127">**Tanúsítvány célja** -hello tanúsítványnak kell lennie az SSL-kiszolgáló hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="b999e-127">**Certificate purpose** - hello certificate must be valid for SSL server authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="b999e-128">**Vállalati hitelesítésszolgáltatók:** Azure AD tartományi szolgáltatások nem támogatja a szervezete vállalati hitelesítésszolgáltató által kiállított biztonságos LDAP-tanúsítványok használatával.</span><span class="sxs-lookup"><span data-stu-id="b999e-128">**Enterprise Certification Authorities:** Azure AD Domain Services does not support using secure LDAP certificates issued by your organization's enterprise certification authority.</span></span> <span data-ttu-id="b999e-129">Ez a korlátozás az oka, hogy hello szolgáltatást nem bízik meg a vállalati hitelesítésszolgáltató egy legfelső szintű hitelesítésszolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="b999e-129">This restriction is because hello service does not trust your enterprise CA as a root certification authority.</span></span> 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a><span data-ttu-id="b999e-130">1. feladat – biztonságos LDAP tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="b999e-130">Task 1 - obtain a certificate for secure LDAP</span></span>
<span data-ttu-id="b999e-131">hello első feladat magában foglalja a biztonságos LDAP hozzáférés toohello által kezelt tartomány a tanúsítványok beszerzéséről.</span><span class="sxs-lookup"><span data-stu-id="b999e-131">hello first task involves obtaining a certificate used for secure LDAP access toohello managed domain.</span></span> <span data-ttu-id="b999e-132">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="b999e-132">You have two options:</span></span>

* <span data-ttu-id="b999e-133">Szerezzen be egy tanúsítványt egy hitelesítésszolgáltatótól.</span><span class="sxs-lookup"><span data-stu-id="b999e-133">Obtain a certificate from a certification authority.</span></span> <span data-ttu-id="b999e-134">hello hatóság lehet egy nyilvános hitelesítésszolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="b999e-134">hello authority may be a public certification authority.</span></span>
* <span data-ttu-id="b999e-135">Hozzon létre egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b999e-135">Create a self-signed certificate.</span></span>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a><span data-ttu-id="b999e-136">Lehetőség (ajánlott) - biztonságos LDAP tanúsítvány beszerzése hitelesítésszolgáltatótól</span><span class="sxs-lookup"><span data-stu-id="b999e-136">Option A (Recommended) - Obtain a secure LDAP certificate from a certification authority</span></span>
<span data-ttu-id="b999e-137">Ha a szervezet beszerzi a tanúsítványokat nyilvános hitelesítésszolgáltatótól származó, tanúsítványra van szükség tooobtain hello biztonságos LDAP, hogy nyilvános hitelesítésszolgáltatótól.</span><span class="sxs-lookup"><span data-stu-id="b999e-137">If your organization obtains its certificates from a public certification authority, you need tooobtain hello secure LDAP certificate from that public certification authority.</span></span>

<span data-ttu-id="b999e-138">A tanúsítvány igénylésekor győződjön meg arról, hogy megfelelnek-e leírt követelményeinek hello [hello biztonságos LDAP tanúsítványra vonatkozó követelményekről](#requirements-for-the-secure-ldap-certificate).</span><span class="sxs-lookup"><span data-stu-id="b999e-138">When requesting a certificate, ensure that you satisfy all hello requirements outlined in [Requirements for hello secure LDAP certificate](#requirements-for-the-secure-ldap-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="b999e-139">Ügyfélszámítógépek tooconnect toohello felügyelt tartományra biztonságos LDAP igénylő hello biztonságos LDAP tanúsítvány kiállítója hello megbízhatónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b999e-139">Client computers that need tooconnect toohello managed domain using secure LDAP must trust hello issuer of hello secure LDAP certificate.</span></span>
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a><span data-ttu-id="b999e-140">B lehetőség – biztonságos LDAP önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="b999e-140">Option B - Create a self-signed certificate for secure LDAP</span></span>
<span data-ttu-id="b999e-141">Ha nem tervezi toouse tanúsítványt nyilvános hitelesítésszolgáltatótól származó, választhatja, hogy toocreate önaláírt tanúsítvány biztonságos LDAP.</span><span class="sxs-lookup"><span data-stu-id="b999e-141">If you do not expect toouse a certificate from a public certification authority, you may choose toocreate a self-signed certificate for secure LDAP.</span></span>

<span data-ttu-id="b999e-142">**Hozzon létre egy önaláírt tanúsítványt PowerShell használatával**</span><span class="sxs-lookup"><span data-stu-id="b999e-142">**Create a self-signed certificate using PowerShell**</span></span>

<span data-ttu-id="b999e-143">A Windows számítógépen nyisson meg egy új PowerShell-ablakot, **rendszergazda** és hello típus a következő parancsokat, toocreate egy új önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b999e-143">On your Windows computer, open a new PowerShell window as **Administrator** and type hello following commands, toocreate a new self-signed certificate.</span></span>

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

<span data-ttu-id="b999e-144">Cserélje le a hello minta megelőző, "*. contoso100.com" hello DNS-tartománynévvel a felügyelt tartomány. For example, ha létrehozott egy "contoso100.onmicrosoft.com" nevű felügyelt tartomány, cserélje le a(z)*. contoso100.com "hello parancsfájl a fent a" *. contoso100.onmicrosoft.com ").</span><span class="sxs-lookup"><span data-stu-id="b999e-144">In hello preceding sample, replace '*.contoso100.com' with hello DNS domain name of your managed domain. For example, if you created a managed domain called 'contoso100.onmicrosoft.com', replace '*.contoso100.com' in hello above script with '*.contoso100.onmicrosoft.com').</span></span>

![Azure AD címtár kiválasztása](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

<span data-ttu-id="b999e-146">az újonnan létrehozott hello önaláírt tanúsítvány kerül hello helyi számítógép tanúsítványtárolójába.</span><span class="sxs-lookup"><span data-stu-id="b999e-146">hello newly created self-signed certificate is placed in hello local machine's certificate store.</span></span>


## <a name="next-step"></a><span data-ttu-id="b999e-147">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="b999e-147">Next step</span></span>
[<span data-ttu-id="b999e-148">2. feladat – hello biztonságos LDAP tanúsítvány tooa exportálása. PFX-fájlból</span><span class="sxs-lookup"><span data-stu-id="b999e-148">Task 2 - export hello secure LDAP certificate tooa .PFX file</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
