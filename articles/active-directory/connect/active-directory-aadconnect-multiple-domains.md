---
title: "Több tartomány aaaAzure AD Connect"
description: "Ez a dokumentum ismerteti, és az Office 365 és az Azure AD több felső szintű tartomány beállításával."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 91d87875ceacee4e34f132938e4bb0294fb954e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a><span data-ttu-id="5b764-103">Többtartományos támogatás az Azure AD összevonási szolgáltatásához</span><span class="sxs-lookup"><span data-stu-id="5b764-103">Multiple Domain Support for Federating with Azure AD</span></span>
<span data-ttu-id="5b764-104">hello alábbi dokumentáció nyújt útmutatást hogyan toouse több legfelső szintű tartományok és altartományok, ha az Office 365 vagy Azure AD-tartomány összevonását.</span><span class="sxs-lookup"><span data-stu-id="5b764-104">hello following documentation provides guidance on how toouse multiple top-level domains and sub-domains when federating with Office 365 or Azure AD domains.</span></span>

## <a name="multiple-top-level-domain-support"></a><span data-ttu-id="5b764-105">Több felső szintű tartomány támogatása</span><span class="sxs-lookup"><span data-stu-id="5b764-105">Multiple top-level domain support</span></span>
<span data-ttu-id="5b764-106">Összevonását több, az Azure ad-vel a legfelső szintű tartományoknak nincs szükség, amikor egy legfelső szintű tartomány összevonását néhány további konfigurálást igényel.</span><span class="sxs-lookup"><span data-stu-id="5b764-106">Federating multiple, top-level domains with Azure AD requires some additional configuration that is not required when federating with one top-level domain.</span></span>

<span data-ttu-id="5b764-107">Amikor egy tartományt az Azure AD össze van vonva, több tulajdonság beállítva hello tartományhoz az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5b764-107">When a domain is federated with Azure AD, several properties are set on hello domain in Azure.</span></span>  <span data-ttu-id="5b764-108">Egy fontos egyik IssuerUri lesz.</span><span class="sxs-lookup"><span data-stu-id="5b764-108">One important one is IssuerUri.</span></span>  <span data-ttu-id="5b764-109">Ez az URI, amely használja az Azure AD tooidentify által hello token hello tartomány társítva.</span><span class="sxs-lookup"><span data-stu-id="5b764-109">This is a URI that is used by Azure AD tooidentify hello domain that hello token is associated with.</span></span>  <span data-ttu-id="5b764-110">hello URI tooresolve tooanything nem szükséges, de egy érvényes URI-Azonosítót kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5b764-110">hello URI doesn’t need tooresolve tooanything but it must be a valid URI.</span></span>  <span data-ttu-id="5b764-111">Alapértelmezés szerint az Azure AD állítja be a toohello hello összevonási szolgáltatás azonosítóját a helyszíni AD FS konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="5b764-111">By default, Azure AD sets this toohello value of hello federation service identifier in your on-premises AD FS configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="5b764-112">hello összevonási szolgáltatás azonosítója, amely egyedileg azonosítja az összevonási szolgáltatás URI.</span><span class="sxs-lookup"><span data-stu-id="5b764-112">hello federation service identifier is a URI that uniquely identifies a federation service.</span></span>  <span data-ttu-id="5b764-113">hello összevonási szolgáltatás AD FS funkcionáló példányának, mint hello biztonságijogkivonat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5b764-113">hello federation service is an instance of AD FS that functions as hello security token service.</span></span> 
> 
> 

<span data-ttu-id="5b764-114">View IssuerUri hello PowerShell-paranccsal is `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span><span class="sxs-lookup"><span data-stu-id="5b764-114">You can vew IssuerUri by using hello PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="5b764-116">A probléma merül fel, ha azt szeretné, hogy tooadd több felső szintű tartomány.</span><span class="sxs-lookup"><span data-stu-id="5b764-116">A problem arises when we want tooadd more than one top-level domain.</span></span>  <span data-ttu-id="5b764-117">Például tegyük fel, a telepítő összevonási az Azure AD között van, és a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="5b764-117">For example, let's say you have setup federation between Azure AD and your on-premises environment.</span></span>  <span data-ttu-id="5b764-118">A dokumentum használok bmcontoso.com.  Most már hozzáadtam egy második felső szintű tartomány bmfabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="5b764-118">For this document I am using bmcontoso.com.  Now I have added a second, top-level domain, bmfabrikam.com.</span></span>

![Tartományok](./media/active-directory-multiple-domains/domains.png)

<span data-ttu-id="5b764-120">Ha azt a bmfabrikam.com tartomány toobe összevont tooconvert, azt hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="5b764-120">When we attempt tooconvert our bmfabrikam.com domain toobe federated, we receive an error.</span></span>  <span data-ttu-id="5b764-121">hello ennek oka, Azure AD is rendelkezik, amely nem engedélyezi a hello IssuerUri tulajdonság toohave hello értékeként azonos értéket egynél több tartomány korlátozás.</span><span class="sxs-lookup"><span data-stu-id="5b764-121">hello reason for this is, Azure AD has a constraint that does not allow hello IssuerUri property toohave hello same value for more than one domain.</span></span>  

![Összevonási hiba](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a><span data-ttu-id="5b764-123">SupportMultipleDomain paraméter</span><span class="sxs-lookup"><span data-stu-id="5b764-123">SupportMultipleDomain Parameter</span></span>
<span data-ttu-id="5b764-124">tooworkaround, igazolnia kell, hogy egy másik IssuerUri, amely hello segítségével végezhető tooadd `-SupportMultipleDomain` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5b764-124">tooworkaround this, we need tooadd a different IssuerUri which can be done by using hello `-SupportMultipleDomain` parameter.</span></span>  <span data-ttu-id="5b764-125">A paraméter a következő parancsmagok hello használata:</span><span class="sxs-lookup"><span data-stu-id="5b764-125">This parameter is used with hello following cmdlets:</span></span>

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

<span data-ttu-id="5b764-126">Ez a paraméter lehetővé teszi az Azure AD hello IssuerUri konfigurálása, hogy hello tartomány nevét hello alapul.</span><span class="sxs-lookup"><span data-stu-id="5b764-126">This parameter makes Azure AD configure hello IssuerUri so that it is based on hello name of hello domain.</span></span>  <span data-ttu-id="5b764-127">Ez lesz egyedi könyvtárak között az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5b764-127">This will be unique across directories in Azure AD.</span></span>  <span data-ttu-id="5b764-128">Hello paraméter használata lehetővé teszi, hogy hello PowerShell-parancs toocomplete sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="5b764-128">Using hello parameter allows hello PowerShell command toocomplete successfully.</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/convert.png)

<span data-ttu-id="5b764-130">Az új bmfabrikam.com tartomány hello-beállítások megnézi hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="5b764-130">Looking at hello settings of our new bmfabrikam.com domain you can see hello following:</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/settings.png)

<span data-ttu-id="5b764-132">Vegye figyelembe, hogy `-SupportMultipleDomain` hello nem változtatja meg más végpontok, amelyek továbbra is konfigurált toopoint tooour összevonási szolgáltatás adfs.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="5b764-132">Note that `-SupportMultipleDomain` does not change hello other endpoints which are still configured toopoint tooour federation service on adfs.bmcontoso.com.</span></span>

<span data-ttu-id="5b764-133">Egy másik művelet, amely `-SupportMultipleDomain` does, hogy azt biztosítja, hogy hello AD FS rendszer tartalmazza-e az Azure AD kiállított jogkivonatokat a hello kibocsátó értéke.</span><span class="sxs-lookup"><span data-stu-id="5b764-133">Another thing that `-SupportMultipleDomain` does is that it ensures that hello AD FS system includes hello proper Issuer value in tokens issued for Azure AD.</span></span> <span data-ttu-id="5b764-134">Hello felhasználók egyszerű Felhasználónevük hello tartomány része, és ez hello IssuerUri, azaz https://{upn utótag hello tartomány beállítását tesz} / adfs/services/megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="5b764-134">It does this by taking hello domain portion of hello users UPN and setting this as hello domain in hello IssuerUri, i.e. https://{upn suffix}/adfs/services/trust.</span></span> 

<span data-ttu-id="5b764-135">Így során hitelesítési tooAzure AD vagy az Office 365, hello IssuerUri hello felhasználói jogkivonat eleme használt toolocate hello tartományhoz az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5b764-135">Thus during authentication tooAzure AD or Office 365, hello IssuerUri element in hello user’s token is used toolocate hello domain in Azure AD.</span></span>  <span data-ttu-id="5b764-136">Ha nem található egyező hello hitelesítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5b764-136">If a match cannot be found hello authentication will fail.</span></span> 

<span data-ttu-id="5b764-137">Például, ha a felhasználói UPN van bsimon@bmcontoso.com, toohttp://bmcontoso.com/adfs/services/trust hello token AD FS problémák hello IssuerUri eleme lesz beállítva.</span><span class="sxs-lookup"><span data-stu-id="5b764-137">For example, if a user’s UPN is bsimon@bmcontoso.com, hello IssuerUri element in hello token AD FS issues will be set toohttp://bmcontoso.com/adfs/services/trust.</span></span> <span data-ttu-id="5b764-138">Ez fog egyezni a hello Azure Active Directory beállítása, és a hitelesítés sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="5b764-138">This will match hello Azure AD configuration, and authentication will succeed.</span></span>

<span data-ttu-id="5b764-139">hello az alábbiakban olvashat a logikát megvalósító hello egyéni jogcímszabályt:</span><span class="sxs-lookup"><span data-stu-id="5b764-139">hello following is hello customized claim rule that implements this logic:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> <span data-ttu-id="5b764-140">A sorrend toouse hello - SupportMultipleDomain kapcsoló új tooadd tett kísérlet során, vagy alakítsa át a már hozzáadott tartomány, toohave kell beállítani az összevont megbízhatósági kapcsolathoz toosupport azokat eredetileg.</span><span class="sxs-lookup"><span data-stu-id="5b764-140">In order toouse hello -SupportMultipleDomain switch when attempting tooadd new or convert already added domains, you need toohave setup your federated trust toosupport them originally.</span></span>  
> 
> 

## <a name="how-tooupdate-hello-trust-between-ad-fs-and-azure-ad"></a><span data-ttu-id="5b764-141">Hogyan tooupdate hello megbízhatóság az AD FS és az Azure AD között</span><span class="sxs-lookup"><span data-stu-id="5b764-141">How tooupdate hello trust between AD FS and Azure AD</span></span>
<span data-ttu-id="5b764-142">Ha nem állított be hello összevont megbízhatóság az AD FS és az Azure AD példányával között, szükség lehet a toore-megbízhatóság létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5b764-142">If you did not setup hello federated trust between AD FS and your instance of Azure AD, you may need toore-create this trust.</span></span>  <span data-ttu-id="5b764-143">Ennek az az oka eredetileg beállítása nélkül hello `-SupportMultipleDomain` paraméter, hello IssuerUri beállítása hello alapértelmezett értékkel.</span><span class="sxs-lookup"><span data-stu-id="5b764-143">This is because, when it is originally setup without hello `-SupportMultipleDomain` parameter, hello IssuerUri is set with hello default value.</span></span>  <span data-ttu-id="5b764-144">A hello az alábbi képernyőfelvételen látható hello IssuerUri toohttps://adfs.bmcontoso.com/adfs/services/trust van beállítva.</span><span class="sxs-lookup"><span data-stu-id="5b764-144">In hello screenshot below you can see hello IssuerUri is set toohttps://adfs.bmcontoso.com/adfs/services/trust.</span></span>

<span data-ttu-id="5b764-145">Így mostantól, ha azt egy új tartomány sikeresen hozzáadta hello Azure AD portálon, majd próbálja meg a tooconvert használatával `Convert-MsolDomaintoFederated -DomainName <your domain>`, azt lekérése a következő hiba hello.</span><span class="sxs-lookup"><span data-stu-id="5b764-145">So now, if we have successfully added an new domain in hello Azure AD portal and then attempt tooconvert it using `Convert-MsolDomaintoFederated -DomainName <your domain>`, we get hello following error.</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/trust1.png)

<span data-ttu-id="5b764-147">Ha megpróbál tooadd hello `-SupportMultipleDomain` kapcsoló azt fog kapni a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="5b764-147">If you try tooadd hello `-SupportMultipleDomain` switch we will receive hello following error:</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/trust2.png)

<span data-ttu-id="5b764-149">Egyszerűen próbált toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` hello az eredeti tartomány is jelent hibát.</span><span class="sxs-lookup"><span data-stu-id="5b764-149">Simply trying toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` on hello original domain will also result in an error.</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/trust3.png)

<span data-ttu-id="5b764-151">Hello lépésekkel tooadd további felső szintű tartomány alatt.</span><span class="sxs-lookup"><span data-stu-id="5b764-151">Use hello steps below tooadd an additional top-level domain.</span></span>  <span data-ttu-id="5b764-152">Ha már hozzáadott egy tartományhoz, és nem használta hello `-SupportMultipleDomain` eltávolítása és frissítése az eredeti tartomány hello lépései paraméter kezdődik.</span><span class="sxs-lookup"><span data-stu-id="5b764-152">If you have already added a domain and did not use hello `-SupportMultipleDomain` parameter start with hello steps for removing and updating your original domain.</span></span>  <span data-ttu-id="5b764-153">Ha nem adja hozzá a legfelső szintű tartomány még indítható hello lépései a tartomány PowerShell az Azure AD Connect használatával való hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="5b764-153">If you have not added a top-level domain yet you can start with hello steps for adding a domain using PowerShell of Azure AD Connect.</span></span>

<span data-ttu-id="5b764-154">Használja a következő lépéseket tooremove hello Microsoft Online megbízhatósági hello és az eredeti tartomány.</span><span class="sxs-lookup"><span data-stu-id="5b764-154">Use hello following steps tooremove hello Microsoft Online trust and update your original domain.</span></span>

1. <span data-ttu-id="5b764-155">Az AD FS összevonási kiszolgálón nyissa meg **AD FS kezelő.**</span><span class="sxs-lookup"><span data-stu-id="5b764-155">On your AD FS federation server open **AD FS Management.**</span></span> 
2. <span data-ttu-id="5b764-156">Bontsa ki a hello bal oldali **megbízhatósági kapcsolatok** és **függő entitás Megbízhatóságai**</span><span class="sxs-lookup"><span data-stu-id="5b764-156">On hello left, expand **Trust Relationships** and **Relying Party Trusts**</span></span>
3. <span data-ttu-id="5b764-157">Jobb oldali hello törölje hello **Microsoft Office 365 Identitásplatformmal** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="5b764-157">On hello right, delete hello **Microsoft Office 365 Identity Platform** entry.</span></span>
   <span data-ttu-id="5b764-158">![Távolítsa el a Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span><span class="sxs-lookup"><span data-stu-id="5b764-158">![Remove Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span></span>
4. <span data-ttu-id="5b764-159">A gépen, amelyen van [Active Directory modul Windows Powershellhez készült Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) telepítve van-e futtassa a következő hello: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="5b764-159">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run hello following: `$cred=Get-Credential`.</span></span>  
5. <span data-ttu-id="5b764-160">Adja meg a összevonja hello Azure AD-tartomány hello felhasználónevet és jelszót egy globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="5b764-160">Enter hello username and password of a global administrator for hello Azure AD domain you are federating with</span></span>
6. <span data-ttu-id="5b764-161">Adja meg a PowerShellben`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="5b764-161">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
7. <span data-ttu-id="5b764-162">Adja meg a PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span><span class="sxs-lookup"><span data-stu-id="5b764-162">In PowerShell enter `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span></span>  <span data-ttu-id="5b764-163">Ez a hello eredeti tartomány.</span><span class="sxs-lookup"><span data-stu-id="5b764-163">This is for hello original domain.</span></span>  <span data-ttu-id="5b764-164">Tartományok lenne a fenti hello használatával:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span><span class="sxs-lookup"><span data-stu-id="5b764-164">So using hello above domains it would be:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span></span>

<span data-ttu-id="5b764-165">A következő lépéseket tooadd hello új legfelső szintű tartomány PowerShell-lel hello használata</span><span class="sxs-lookup"><span data-stu-id="5b764-165">Use hello following steps tooadd hello new top-level domain using PowerShell</span></span>

1. <span data-ttu-id="5b764-166">A gépen, amelyen van [Active Directory modul Windows Powershellhez készült Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) telepítve van-e futtassa a következő hello: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="5b764-166">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run hello following: `$cred=Get-Credential`.</span></span>  
2. <span data-ttu-id="5b764-167">Adja meg a összevonja hello Azure AD-tartomány hello felhasználónevet és jelszót egy globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="5b764-167">Enter hello username and password of a global administrator for hello Azure AD domain you are federating with</span></span>
3. <span data-ttu-id="5b764-168">Adja meg a PowerShellben`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="5b764-168">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
4. <span data-ttu-id="5b764-169">Adja meg a PowerShellben`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span><span class="sxs-lookup"><span data-stu-id="5b764-169">In PowerShell enter `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span></span>

<span data-ttu-id="5b764-170">A következő lépéseket tooadd hello új legfelső szintű tartomány Azure AD Connect használatával hello használata.</span><span class="sxs-lookup"><span data-stu-id="5b764-170">Use hello following steps tooadd hello new top-level domain using Azure AD Connect.</span></span>

1. <span data-ttu-id="5b764-171">Indítsa el az Azure AD Connect hello asztalon vagy start menüben</span><span class="sxs-lookup"><span data-stu-id="5b764-171">Launch Azure AD Connect from hello desktop or start menu</span></span>
2. <span data-ttu-id="5b764-172">"Egy további Azure AD-tartomány hozzáadása" Válasszon ![adnia egy további Azure AD-tartomány](./media/active-directory-multiple-domains/add1.png)</span><span class="sxs-lookup"><span data-stu-id="5b764-172">Choose “Add an additional Azure AD Domain” ![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add1.png)</span></span>
3. <span data-ttu-id="5b764-173">Adja meg az Azure AD és az Active Directory hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5b764-173">Enter your Azure AD and Active Directory credentials</span></span>
4. <span data-ttu-id="5b764-174">Válassza ki a hello második tartományt összevonásra tooconfigure kívánja.</span><span class="sxs-lookup"><span data-stu-id="5b764-174">Select hello second domain you wish tooconfigure for federation.</span></span>
   <span data-ttu-id="5b764-175">![Felvesz egy további Azure AD-tartomány](./media/active-directory-multiple-domains/add2.png)</span><span class="sxs-lookup"><span data-stu-id="5b764-175">![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add2.png)</span></span>
5. <span data-ttu-id="5b764-176">Kattintson a telepítés</span><span class="sxs-lookup"><span data-stu-id="5b764-176">Click Install</span></span>

### <a name="verify-hello-new-top-level-domain"></a><span data-ttu-id="5b764-177">Új legfelső szintű tartomány hello ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5b764-177">Verify hello new top-level domain</span></span>
<span data-ttu-id="5b764-178">Hello PowerShell-paranccsal `Get-MsolDomainFederationSettings -DomainName <your domain>`megtekintheti hello IssuerUri frissítése.</span><span class="sxs-lookup"><span data-stu-id="5b764-178">By using hello PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`you can view hello updated IssuerUri.</span></span>  <span data-ttu-id="5b764-179">hello az alábbi képernyőfelvétel hello összevonási beállítások lettek frissítve az az eredeti tartomány http://bmcontoso.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="5b764-179">hello screenshot below shows hello federation settings were updated on our original domain http://bmcontoso.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="5b764-181">Az új tartományon IssuerUri hello toohttps://bmfabrikam.com/adfs/services/trust van beállítva, és</span><span class="sxs-lookup"><span data-stu-id="5b764-181">And hello IssuerUri on our new domain has been set toohttps://bmfabrikam.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a><span data-ttu-id="5b764-183">Altartományok támogatása</span><span class="sxs-lookup"><span data-stu-id="5b764-183">Support for Sub-domains</span></span>
<span data-ttu-id="5b764-184">Amennyiben altartomány hozzáadásához miatt hello módon az Azure AD tartományok, azt öröklik a szülő hello hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="5b764-184">When you add a sub-domain, because of hello way Azure AD handled domains, it will inherit hello settings of hello parent.</span></span>  <span data-ttu-id="5b764-185">Ez azt jelenti, hogy hello IssuerUri toomatch hello szülők kell.</span><span class="sxs-lookup"><span data-stu-id="5b764-185">This means that hello IssuerUri needs toomatch hello parents.</span></span>

<span data-ttu-id="5b764-186">Így lehetővé teszi, hogy tegyük fel például, hogy rendelkezem bmcontoso.com, és adja meg az corp.bmcontoso.com.  Ez azt jelenti, hogy hello IssuerUri, az a felhasználó a corp.bmcontoso.com kell toobe **http://bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="5b764-186">So lets say for example that I have bmcontoso.com and then add corp.bmcontoso.com.  This means that hello IssuerUri for a user from corp.bmcontoso.com will need toobe **http://bmcontoso.com/adfs/services/trust.**</span></span>  <span data-ttu-id="5b764-187">Hello szabványos szabály megvalósított felett az Azure AD, azonban hoz létre a jogkivonatot kibocsátó, az **http://corp.bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="5b764-187">However hello standard rule implemented above for Azure AD, will generate a token with an issuer as **http://corp.bmcontoso.com/adfs/services/trust.**</span></span> <span data-ttu-id="5b764-188">ami nem fog egyezni a hello tartományi szükséges érték, és a hitelesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5b764-188">which will not match hello domain's required value and authentication will fail.</span></span>

### <a name="how-tooenable-support-for-sub-domains"></a><span data-ttu-id="5b764-189">Hogyan támogatják a tooenable az altartományok</span><span class="sxs-lookup"><span data-stu-id="5b764-189">How tooenable support for sub-domains</span></span>
<span data-ttu-id="5b764-190">Az AD FS megbízható függő entitás a Microsoft Online rendelés toowork a hello körül toobe frissítve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5b764-190">In order toowork around this hello AD FS relying party trust for Microsoft Online needs toobe updated.</span></span>  <span data-ttu-id="5b764-191">toodo, konfigurálnia kell egyéni jogcímszabályt, így azt szalagok ki bármely altartományok hello felhasználói UPN-utótagot a hello egyéni kibocsátó érték kiszámításakor.</span><span class="sxs-lookup"><span data-stu-id="5b764-191">toodo this, you must configure a custom claim rule so that it strips off any sub-domains from hello user’s UPN suffix when constructing hello custom Issuer value.</span></span> 

<span data-ttu-id="5b764-192">a következő jogcím hello fog tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5b764-192">hello following claim will do this:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
<span data-ttu-id="5b764-193">hello hello reguláris kifejezés utolsó számának beállítása hello hány szülő tartományt a legfelső szintű tartomány van.</span><span class="sxs-lookup"><span data-stu-id="5b764-193">hello last number in hello regular expression set hello how many parent domains there is in your root domain.</span></span> <span data-ttu-id="5b764-194">Itt az i bmcontoso.com rendelkezik, ezért két szülő tartomány szükség.</span><span class="sxs-lookup"><span data-stu-id="5b764-194">Here i have bmcontoso.com so two parent domains are necessary.</span></span> <span data-ttu-id="5b764-195">Ha három szülő tartományok volt toobe tartott (vagyis: corp.bmcontoso.com), majd hello szám kellett volna lennie három.</span><span class="sxs-lookup"><span data-stu-id="5b764-195">If three parent domains were toobe kept (i.e.: corp.bmcontoso.com), then hello number would have been three.</span></span> <span data-ttu-id="5b764-196">Széles Eventualy fel kell tüntetni, hello egyezés mindig lesz toomatch hello tartományok maximálisan engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="5b764-196">Eventualy a range can be indicated, hello match will always be made toomatch hello maximum of domains.</span></span> <span data-ttu-id="5b764-197">"a(z) {2,3}" fog egyezni a tartományok toothree (pl.: bmfabrikam.com és corp.bmcontoso.com).</span><span class="sxs-lookup"><span data-stu-id="5b764-197">"{2,3}" will match two toothree domains (i.e.: bmfabrikam.com and corp.bmcontoso.com).</span></span>

<span data-ttu-id="5b764-198">A következő lépéseket tooadd hello egyéni jogcím toosupport altartományok használja.</span><span class="sxs-lookup"><span data-stu-id="5b764-198">Use hello following steps tooadd a custom claim toosupport sub-domains.</span></span>

1. <span data-ttu-id="5b764-199">Nyissa meg az AD FS-Szolgáltatáskezelés</span><span class="sxs-lookup"><span data-stu-id="5b764-199">Open AD FS Management</span></span>
2. <span data-ttu-id="5b764-200">Hello Microsoft Online RP megbízhatósági kattintson jobb gombbal, majd válassza ki a jogcímszabályok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="5b764-200">Right click hello Microsoft Online RP trust and choose Edit Claim rules</span></span>
3. <span data-ttu-id="5b764-201">Válassza ki a hello harmadik jogcímszabály, és cserélje le ![jogcímszabályok szerkesztése](./media/active-directory-multiple-domains/sub1.png)</span><span class="sxs-lookup"><span data-stu-id="5b764-201">Select hello third claim rule, and replace ![Edit claim](./media/active-directory-multiple-domains/sub1.png)</span></span>
4. <span data-ttu-id="5b764-202">Cserélje le a jelenlegi jogcím hello:</span><span class="sxs-lookup"><span data-stu-id="5b764-202">Replace hello current claim:</span></span>
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Cserélje ki a jogcímet](./media/active-directory-multiple-domains/sub2.png)

5. <span data-ttu-id="5b764-204">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="5b764-204">Click Ok.</span></span>  <span data-ttu-id="5b764-205">Az Alkalmaz gombra.</span><span class="sxs-lookup"><span data-stu-id="5b764-205">Click Apply.</span></span>  <span data-ttu-id="5b764-206">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="5b764-206">Click Ok.</span></span>  <span data-ttu-id="5b764-207">Zárja be az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="5b764-207">Close AD FS Management.</span></span>

