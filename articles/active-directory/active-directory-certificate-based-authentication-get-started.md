---
title: "aaaAzure Active Directory-alapú hitelesítés – első lépések |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure tanúsítvány alapú hitelesítést a környezetben"
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 3c73bdf56018c0716085c923a61e9560dbe4004c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a><span data-ttu-id="f6330-103">Ismerkedés az Azure Active Directory-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="f6330-103">Get started with certificate-based authentication in Azure Active Directory</span></span>

<span data-ttu-id="f6330-104">Tanúsítvány alapú hitelesítés lehetővé teszi az Exchange online-fiókját az kapcsolódáskor egy Windows-, Android vagy iOS eszközön ügyféltanúsítvánnyal az Azure Active Directory hitelesített toobe:</span><span class="sxs-lookup"><span data-stu-id="f6330-104">Certificate-based authentication enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

- <span data-ttu-id="f6330-105">Office-mobilalkalmazások például a Microsoft Outlook és a Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="f6330-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   

- <span data-ttu-id="f6330-106">Exchange ActiveSync (EAS) ügyfeleket</span><span class="sxs-lookup"><span data-stu-id="f6330-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="f6330-107">Ez a funkció beállítása nem hello kell tooenter egy felhasználónév és jelszó kombináció bizonyos mail és a Microsoft Office-alkalmazásokat a mobileszközön.</span><span class="sxs-lookup"><span data-stu-id="f6330-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="f6330-108">Ez a témakör:</span><span class="sxs-lookup"><span data-stu-id="f6330-108">This topic:</span></span>

- <span data-ttu-id="f6330-109">Hello való tooconfigure lépések és egyszeri Tanúsítványalapú hitelesítés a felhasználók számára a bérlő az Office 365 Enterprise, vállalati, oktatási és az Egyesült Államok kormánya tervek biztosít.</span><span class="sxs-lookup"><span data-stu-id="f6330-109">Provides you with hello steps tooconfigure and utilize certificate-based authentication for users of tenants in Office 365 Enterprise, Business, Education, and US Government plans.</span></span> <span data-ttu-id="f6330-110">Ez a funkció érhető el az előzetes verzió az Office 365 Kína, a US Government védelmet és a US Government Szövetségi csomagokban.</span><span class="sxs-lookup"><span data-stu-id="f6330-110">This feature is available in preview in Office 365 China, US Government Defense, and US Government Federal plans.</span></span> 

- <span data-ttu-id="f6330-111">Feltételezi, hogy már rendelkezik egy [nyilvános kulcsokra épülő infrastruktúra (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) és [AD FS](connect/active-directory-aadconnectfed-whatis.md) konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f6330-111">Assumes that you already have a [public key infrastructure (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) and [AD FS](connect/active-directory-aadconnectfed-whatis.md) configured.</span></span>    


## <a name="requirements"></a><span data-ttu-id="f6330-112">Követelmények</span><span class="sxs-lookup"><span data-stu-id="f6330-112">Requirements</span></span>

<span data-ttu-id="f6330-113">tooconfigure tanúsítvány alapú hitelesítést hello alábbiaknak igaznak kell lenniük:</span><span class="sxs-lookup"><span data-stu-id="f6330-113">tooconfigure certificate-based authentication, hello following must be true:</span></span>  

- <span data-ttu-id="f6330-114">Tanúsítvány alapú hitelesítést (CBA) csak külső környezeteket böngészőalkalmazásokban vagy natív modern hitelesítést (ADAL) használó ügyfelek esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="f6330-114">Certificate-based authentication (CBA) is only supported for Federated environments for browser applications or native clients using modern authentication (ADAL).</span></span> <span data-ttu-id="f6330-115">hello egyetlen kivétel ez alól az Exchange Active Sync (EAS) a EXO, mind a összevont, és a felügyelt fiók használható.</span><span class="sxs-lookup"><span data-stu-id="f6330-115">hello one exception is Exchange Active Sync (EAS) for EXO which can be used for both, federated and managed accounts.</span></span> 

- <span data-ttu-id="f6330-116">hello legfelső szintű hitelesítésszolgáltatót és köztes hitelesítésszolgáltatót kell állítani az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="f6330-116">hello root certificate authority and any intermediate certificate authorities must be configured in Azure Active Directory.</span></span>  

- <span data-ttu-id="f6330-117">Minden egyes hitelesítésszolgáltató visszavont tanúsítványok listájának (CRL), amely az Internet felé néző URL-cím használatával lehet hivatkozni kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f6330-117">Each certificate authority must have a certificate revocation list (CRL) that can be referenced via an Internet facing URL.</span></span>  

- <span data-ttu-id="f6330-118">Rendelkeznie kell legalább egy hitelesítésszolgáltatót az Azure Active Directoryban konfigurált.</span><span class="sxs-lookup"><span data-stu-id="f6330-118">You must have at least one certificate authority configured in Azure Active Directory.</span></span> <span data-ttu-id="f6330-119">Hello található kapcsolódó lépések [hello tanúsítványszolgáltatók konfigurálása](#step-2-configure-the-certificate-authorities) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f6330-119">You can find related steps in hello [Configure hello certificate authorities](#step-2-configure-the-certificate-authorities) section.</span></span>  

- <span data-ttu-id="f6330-120">Exchange ActiveSync-ügyfelek hello ügyféltanúsítvány hello felhasználói irányítható e-mail cím az Exchange online vagy hello egyszerű felhasználónév vagy hello hello tulajdonos alternatív neve mező értékének RFC822 nevét kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f6330-120">For Exchange ActiveSync clients, hello client certificate must have hello user’s routable email address in Exchange online in either hello Principal Name or hello RFC822 Name value of hello Subject Alternative Name field.</span></span> <span data-ttu-id="f6330-121">Az Azure Active Directory leképezi a hello RFC822 érték toohello proxycím attribútum hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="f6330-121">Azure Active Directory maps hello RFC822 value toohello Proxy Address attribute in hello directory.</span></span>  

- <span data-ttu-id="f6330-122">Az ügyfél hozzáférési tooat legalább egy hitelesítésszolgáltató által kiállított tanúsítványt kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="f6330-122">Your client device must have access tooat least one certificate authority that issues client certificates.</span></span>  

- <span data-ttu-id="f6330-123">Ügyféltanúsítványt az ügyfél-hitelesítést kell kiállították tooyour ügyfél.</span><span class="sxs-lookup"><span data-stu-id="f6330-123">A client certificate for client authentication must have been issued tooyour client.</span></span>  




## <a name="step-1-select-your-device-platform"></a><span data-ttu-id="f6330-124">1. lépés: Válassza ki az eszközplatformhoz</span><span class="sxs-lookup"><span data-stu-id="f6330-124">Step 1: Select your device platform</span></span>

<span data-ttu-id="f6330-125">Első lépésként hello eszközplatform, az Ön számára legfontosabb kell tooreview hello következő:</span><span class="sxs-lookup"><span data-stu-id="f6330-125">As a first step, for hello device platform you care about, you need tooreview hello following:</span></span>

- <span data-ttu-id="f6330-126">hello Office-mobilalkalmazás támogatja</span><span class="sxs-lookup"><span data-stu-id="f6330-126">hello Office mobile applications support</span></span> 
- <span data-ttu-id="f6330-127">hello adott megvalósítás követelményei</span><span class="sxs-lookup"><span data-stu-id="f6330-127">hello specific implementation requirements</span></span>  

<span data-ttu-id="f6330-128">hello kapcsolatos információkat a következő eszközplatformokat hello létezik:</span><span class="sxs-lookup"><span data-stu-id="f6330-128">hello related information exists for hello following device platforms:</span></span>

- [<span data-ttu-id="f6330-129">Android</span><span class="sxs-lookup"><span data-stu-id="f6330-129">Android</span></span>](active-directory-certificate-based-authentication-android.md)
- [<span data-ttu-id="f6330-130">iOS</span><span class="sxs-lookup"><span data-stu-id="f6330-130">iOS</span></span>](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-hello-certificate-authorities"></a><span data-ttu-id="f6330-131">2. lépés: Hello tanúsítványszolgáltatók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6330-131">Step 2: Configure hello certificate authorities</span></span> 

<span data-ttu-id="f6330-132">tooconfigure a hitelesítésszolgáltatók minden tanúsítvány-szolgáltató az Azure Active Directoryban, töltse fel a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f6330-132">tooconfigure your certificate authorities in Azure Active Directory, for each certificate authority, upload hello following:</span></span> 

* <span data-ttu-id="f6330-133">a hello tanúsítvány nyilvános részét hello *.cer* formátumban</span><span class="sxs-lookup"><span data-stu-id="f6330-133">hello public portion of hello certificate, in *.cer* format</span></span> 
* <span data-ttu-id="f6330-134">hello internetre irányuló URL-címeket, ahol hello visszavont tanúsítványok listájának (CRL) találhatók</span><span class="sxs-lookup"><span data-stu-id="f6330-134">hello Internet facing URLs where hello Certificate Revocation Lists (CRLs) reside</span></span>

<span data-ttu-id="f6330-135">hello sémát a hitelesítésszolgáltatótól a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="f6330-135">hello schema for a certificate authority looks as follows:</span></span> 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 

<span data-ttu-id="f6330-136">Hello konfigurációs hello használhatja [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0):</span><span class="sxs-lookup"><span data-stu-id="f6330-136">For hello configuration, you can use hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span></span>  

1. <span data-ttu-id="f6330-137">Indítsa el a Windows PowerShell rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="f6330-137">Start Windows PowerShell with administrator privileges.</span></span> 
2. <span data-ttu-id="f6330-138">Hello Azure AD-modul telepítése.</span><span class="sxs-lookup"><span data-stu-id="f6330-138">Install hello Azure AD module.</span></span> <span data-ttu-id="f6330-139">Tooinstall verziót kell [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="f6330-139">You need tooinstall Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) or higher.</span></span>  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

<span data-ttu-id="f6330-140">Első lépésként konfigurációs tooestablish kapcsolatot és a bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="f6330-140">As a first configuration step, you need tooestablish a connection with your tenant.</span></span> <span data-ttu-id="f6330-141">Amint létezik egy kapcsolat tooyour bérlői, áttekintheti, adja hozzá, törlése és módosítsa a könyvtárat a definiált hello megbízható hitelesítésszolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="f6330-141">As soon as a connection tooyour tenant exists, you can review, add, delete and modify hello trusted certificate authorities that are defined in your directory.</span></span> 

### <a name="connect"></a><span data-ttu-id="f6330-142">Kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="f6330-142">Connect</span></span>

<span data-ttu-id="f6330-143">a bérlő használata hello kapcsolatot tooestablish [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="f6330-143">tooestablish a connection with your tenant, use hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span></span>

    Connect-AzureAD 


### <a name="retrieve"></a><span data-ttu-id="f6330-144">Beolvasása</span><span class="sxs-lookup"><span data-stu-id="f6330-144">Retrieve</span></span> 

<span data-ttu-id="f6330-145">tooretrieve hello megbízható hitelesítésszolgáltatók a könyvtárban, meghatározott hello használata [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f6330-145">tooretrieve hello trusted certificate authorities that are defined in your directory, use hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span></span> 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a><span data-ttu-id="f6330-146">Hozzáadás</span><span class="sxs-lookup"><span data-stu-id="f6330-146">Add</span></span>

<span data-ttu-id="f6330-147">megbízható hitelesítésszolgáltató toocreate hello használata [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmag és a set hello **Vlelérésihelye** attribútum tooa megfelelő érték:</span><span class="sxs-lookup"><span data-stu-id="f6330-147">toocreate a trusted certificate authority, use hello [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet and set hello **crlDistributionPoint** attribute tooa correct value:</span></span> 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF hello CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a><span data-ttu-id="f6330-148">Eltávolítás</span><span class="sxs-lookup"><span data-stu-id="f6330-148">Remove</span></span>

<span data-ttu-id="f6330-149">megbízható hitelesítésszolgáltató tooremove hello használata [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="f6330-149">tooremove a trusted certificate authority, use hello [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a><span data-ttu-id="f6330-150">Modfiy</span><span class="sxs-lookup"><span data-stu-id="f6330-150">Modfiy</span></span>

<span data-ttu-id="f6330-151">megbízható hitelesítésszolgáltató toomodify hello használata [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="f6330-151">toomodify a trusted certificate authority, use hello [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a><span data-ttu-id="f6330-152">3. lépés: A visszavont tanúsítványok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6330-152">Step 3: Configure revocation</span></span>

<span data-ttu-id="f6330-153">egy ügyféltanúsítványt, az Azure Active Directory toorevoke beolvassa hello tanúsítványt a visszavont tanúsítványok listáját (CRL) a hello URL hitelesítésszolgáltató adatai részeként feltöltött és gyorsítótárba helyezi azt.</span><span class="sxs-lookup"><span data-stu-id="f6330-153">toorevoke a client certificate, Azure Active Directory fetches hello certificate revocation list (CRL) from hello URLs uploaded as part of certificate authority information and caches it.</span></span> <span data-ttu-id="f6330-154">hello utolsó közzétételi időbélyeg (**dátumig** tulajdonság) a tanúsítvány-visszavonási listát használt hello tooensure hello CRL Elérési útja még érvényes.</span><span class="sxs-lookup"><span data-stu-id="f6330-154">hello last publish timestamp (**Effective Date** property) in hello CRL is used tooensure hello CRL is still valid.</span></span> <span data-ttu-id="f6330-155">tanúsítvány-visszavonási listát hello rendszeresen hivatkozott toorevoke hozzáférés toocertificates hello lista egy részét képező.</span><span class="sxs-lookup"><span data-stu-id="f6330-155">hello CRL is periodically referenced toorevoke access toocertificates that are a part of hello list.</span></span>

<span data-ttu-id="f6330-156">Ha több azonnali visszavonás szükséges (például, ha a felhasználó elveszíti a eszköz), érvénytelenítve lenne hello engedélyezési jogkivonat hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f6330-156">If a more instant revocation is required (for example, if a user loses a device), hello authorization token of hello user can be invalidated.</span></span> <span data-ttu-id="f6330-157">hello engedélyezési jogkivonat, beállíthatja hello tooinvalidate **StsRefreshTokenValidFrom** mezőt az adott felhasználó Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f6330-157">tooinvalidate hello authorization token, set hello **StsRefreshTokenValidFrom** field for this particular user using Windows PowerShell.</span></span> <span data-ttu-id="f6330-158">Frissítenie kell a hello **StsRefreshTokenValidFrom** mezőt, minden felhasználóhoz toorevoke hozzáférést szeretne.</span><span class="sxs-lookup"><span data-stu-id="f6330-158">You must update hello **StsRefreshTokenValidFrom** field for each user you want toorevoke access for.</span></span>

<span data-ttu-id="f6330-159">hello visszavonási továbbra is fennáll, tooensure, be kell hello **dátumig** hello CRL tooa dátum után értéke hello **StsRefreshTokenValidFrom** , és ellenőrizze, hogy a szóban forgó hello tanúsítvány hello CRL-t.</span><span class="sxs-lookup"><span data-stu-id="f6330-159">tooensure that hello revocation persists, you must set hello **Effective Date** of hello CRL tooa date after hello value set by **StsRefreshTokenValidFrom** and ensure hello certificate in question is in hello CRL.</span></span>

<span data-ttu-id="f6330-160">a következő lépéseket vázlat hello folyamat frissítésére és hello engedélyezési jogkivonat érvénytelenítése által hello beállítása hello **StsRefreshTokenValidFrom** mező.</span><span class="sxs-lookup"><span data-stu-id="f6330-160">hello following steps outline hello process for updating and invalidating hello authorization token by setting hello **StsRefreshTokenValidFrom** field.</span></span> 

<span data-ttu-id="f6330-161">**tooconfigure visszavonási:**</span><span class="sxs-lookup"><span data-stu-id="f6330-161">**tooconfigure revocation:**</span></span> 

1. <span data-ttu-id="f6330-162">Csatlakozás a rendszergazdai hitelesítő adatok toohello MSOL szolgáltatással:</span><span class="sxs-lookup"><span data-stu-id="f6330-162">Connect with admin credentials toohello MSOL service:</span></span> 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. <span data-ttu-id="f6330-163">Lekéri az hello aktuális StsRefreshTokensValidFrom értéket a felhasználóhoz:</span><span class="sxs-lookup"><span data-stu-id="f6330-163">Retrieve hello current StsRefreshTokensValidFrom value for a user:</span></span> 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. <span data-ttu-id="f6330-164">Adja meg egy új StsRefreshTokensValidFrom értéket hello felhasználói egyenlő toohello aktuális időbélyeg:</span><span class="sxs-lookup"><span data-stu-id="f6330-164">Configure a new StsRefreshTokensValidFrom value for hello user equal toohello current timestamp:</span></span> 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

<span data-ttu-id="f6330-165">beállíthatja hello dátuma jövőbeli hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6330-165">hello date you set must be in hello future.</span></span> <span data-ttu-id="f6330-166">Ha hello dátuma jövőbeli hello van, hello **StsRefreshTokensValidFrom** tulajdonsága nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="f6330-166">If hello date is not in hello future, hello **StsRefreshTokensValidFrom** property is not set.</span></span> <span data-ttu-id="f6330-167">Ha hello dátuma jövőbeli, hello **StsRefreshTokensValidFrom** értéke toohello aktuális idő (nem hello dátum szerint Set-MsolUser paranccsal).</span><span class="sxs-lookup"><span data-stu-id="f6330-167">If hello date is in hello future, **StsRefreshTokensValidFrom** is set toohello current time (not hello date indicated by Set-MsolUser command).</span></span> 


## <a name="step-4-test-your-configuration"></a><span data-ttu-id="f6330-168">4. lépés: A konfiguráció tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6330-168">Step 4: Test your configuration</span></span>

### <a name="testing-your-certificate"></a><span data-ttu-id="f6330-169">A tanúsítvány tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6330-169">Testing your certificate</span></span>

<span data-ttu-id="f6330-170">Első konfiguráció teszteléséhez, mint-t érdemes kipróbálni a toosign túl[Outlook Web Access](https://outlook.office365.com) vagy [SharePoint Online](https://microsoft.sharepoint.com) használatával a **eszközökön böngészőben**.</span><span class="sxs-lookup"><span data-stu-id="f6330-170">As a first configuration test, you should try toosign in too[Outlook Web Access](https://outlook.office365.com) or [SharePoint Online](https://microsoft.sharepoint.com) using your **on-device browser**.</span></span>

<span data-ttu-id="f6330-171">Ha a bejelentkezés sikeres, akkor tudja, hogy:</span><span class="sxs-lookup"><span data-stu-id="f6330-171">If your sign-in is successful, then you know that:</span></span>

- <span data-ttu-id="f6330-172">hello felhasználói tanúsítvány már kiépített tooyour vizsgálati eszköz</span><span class="sxs-lookup"><span data-stu-id="f6330-172">hello user certificate has been provisioned tooyour test device</span></span>
- <span data-ttu-id="f6330-173">Az AD FS megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f6330-173">AD FS is configured correctly</span></span>  


### <a name="testing-office-mobile-applications"></a><span data-ttu-id="f6330-174">Office-mobilalkalmazások tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6330-174">Testing Office mobile applications</span></span>

<span data-ttu-id="f6330-175">**tootest Tanúsítványalapú hitelesítés az Office-mobilalkalmazás:**</span><span class="sxs-lookup"><span data-stu-id="f6330-175">**tootest certificate-based authentication on your mobile Office application:**</span></span> 

1. <span data-ttu-id="f6330-176">A vizsgálati eszköz telepítse az Office-mobilalkalmazás (például a OneDrive).</span><span class="sxs-lookup"><span data-stu-id="f6330-176">On your test device, install an Office mobile application (e.g., OneDrive).</span></span>
3. <span data-ttu-id="f6330-177">Indítsa el a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f6330-177">Launch hello application.</span></span> 
4. <span data-ttu-id="f6330-178">Adja meg a felhasználónevét, majd válassza ki a kívánt toouse hello felhasználói tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="f6330-178">Enter your user name, and then select hello user certificate you want toouse.</span></span> 

<span data-ttu-id="f6330-179">Akkor érdemes lehet sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="f6330-179">You should be successfully signed in.</span></span> 

### <a name="testing-exchange-activesync-client-applications"></a><span data-ttu-id="f6330-180">Exchange ActiveSync ügyfél alkalmazások tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6330-180">Testing Exchange ActiveSync client applications</span></span>

<span data-ttu-id="f6330-181">Exchange ActiveSync (EAS) tooaccess tanúsítvány alapú hitelesítést, az EAS-profil hello ügyfél tanúsítványát tartalmazó keresztül elérhető toohello alkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6330-181">tooaccess Exchange ActiveSync (EAS) via certificate-based authentication, an EAS profile containing hello client certificate must be available toohello application.</span></span> 

<span data-ttu-id="f6330-182">EAS-profil hello hello a következő információkat kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="f6330-182">hello EAS profile must contain hello following information:</span></span>

- <span data-ttu-id="f6330-183">hello felhasználói tanúsítvány toobe hitelesítéshez használt</span><span class="sxs-lookup"><span data-stu-id="f6330-183">hello user certificate toobe used for authentication</span></span> 

- <span data-ttu-id="f6330-184">hello EAS végpont (például outlook.office365.com)</span><span class="sxs-lookup"><span data-stu-id="f6330-184">hello EAS endpoint (for example, outlook.office365.com)</span></span>

<span data-ttu-id="f6330-185">Az EAS-profil konfigurálhatók és elhelyezett hello eszközön keresztül hello mobileszköz-kezelés (MDM) például az Intune-ban, vagy úgy manuálisan hello tanúsítvány hello EAS-profil hello eszközön-felhasználását.</span><span class="sxs-lookup"><span data-stu-id="f6330-185">An EAS profile can be configured and placed on hello device through hello utilization of Mobile device management (MDM) such as Intune or by manually placing hello certificate in hello EAS profile on hello device.</span></span>  

### <a name="testing-eas-client-applications-on-android"></a><span data-ttu-id="f6330-186">Az Android EAS-ügyfél alkalmazások tesztelése</span><span class="sxs-lookup"><span data-stu-id="f6330-186">Testing EAS client applications on Android</span></span>

<span data-ttu-id="f6330-187">**tanúsítványhitelesítés tootest:**</span><span class="sxs-lookup"><span data-stu-id="f6330-187">**tootest certificate authentication:**</span></span>  

1. <span data-ttu-id="f6330-188">Az EAS-profil konfigurálása, amely eleget tesz a fenti követelmények hello hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f6330-188">Configure an EAS profile in hello application that satisfies hello requirements above.</span></span>  
2. <span data-ttu-id="f6330-189">Nyissa meg a hello alkalmazást, és győződjön meg arról, hogy mail szinkronizál.</span><span class="sxs-lookup"><span data-stu-id="f6330-189">Open hello application, and verify that mail is synchronizing.</span></span> 

