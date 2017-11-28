---
title: "Az Azure AD Connect több tartományban"
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
ms.openlocfilehash: 8e3f496c2868cc3430e0efd47805aec2205168aa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a><span data-ttu-id="996b2-103">Többtartományos támogatás az Azure AD összevonási szolgáltatásához</span><span class="sxs-lookup"><span data-stu-id="996b2-103">Multiple Domain Support for Federating with Azure AD</span></span>
<span data-ttu-id="996b2-104">Az alábbi dokumentáció nyújt útmutatást több felső szintű tartományok és altartományok használata, ha az Office 365 vagy az Azure AD-tartomány összevonását.</span><span class="sxs-lookup"><span data-stu-id="996b2-104">The following documentation provides guidance on how to use multiple top-level domains and sub-domains when federating with Office 365 or Azure AD domains.</span></span>

## <a name="multiple-top-level-domain-support"></a><span data-ttu-id="996b2-105">Több felső szintű tartomány támogatása</span><span class="sxs-lookup"><span data-stu-id="996b2-105">Multiple top-level domain support</span></span>
<span data-ttu-id="996b2-106">Összevonását több, az Azure ad-vel a legfelső szintű tartományoknak nincs szükség, amikor egy legfelső szintű tartomány összevonását néhány további konfigurálást igényel.</span><span class="sxs-lookup"><span data-stu-id="996b2-106">Federating multiple, top-level domains with Azure AD requires some additional configuration that is not required when federating with one top-level domain.</span></span>

<span data-ttu-id="996b2-107">Amikor egy tartományt az Azure AD össze van vonva, több tulajdonság a tartományban az Azure-ban beállítva.</span><span class="sxs-lookup"><span data-stu-id="996b2-107">When a domain is federated with Azure AD, several properties are set on the domain in Azure.</span></span>  <span data-ttu-id="996b2-108">Egy fontos egyik IssuerUri lesz.</span><span class="sxs-lookup"><span data-stu-id="996b2-108">One important one is IssuerUri.</span></span>  <span data-ttu-id="996b2-109">Ez az URI, amely az Azure ad azonosítására szolgál a tartományhoz, amely a token társítva van.</span><span class="sxs-lookup"><span data-stu-id="996b2-109">This is a URI that is used by Azure AD to identify the domain that the token is associated with.</span></span>  <span data-ttu-id="996b2-110">Az URI nem kell tartoznia semmi, de kell lennie egy érvényes URI.</span><span class="sxs-lookup"><span data-stu-id="996b2-110">The URI doesn’t need to resolve to anything but it must be a valid URI.</span></span>  <span data-ttu-id="996b2-111">Alapértelmezés szerint az Azure AD beállítja az összevonási szolgáltatás azonosítóját értékének a helyszíni AD FS konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="996b2-111">By default, Azure AD sets this to the value of the federation service identifier in your on-premises AD FS configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="996b2-112">Az összevonási szolgáltatás azonosítóját, amely egyedileg azonosítja az összevonási szolgáltatás URI.</span><span class="sxs-lookup"><span data-stu-id="996b2-112">The federation service identifier is a URI that uniquely identifies a federation service.</span></span>  <span data-ttu-id="996b2-113">Az összevonási szolgáltatás AD FS funkcionáló példányának, mint a biztonsági jogkivonatokkal kapcsolatos szolgáltatástól.</span><span class="sxs-lookup"><span data-stu-id="996b2-113">The federation service is an instance of AD FS that functions as the security token service.</span></span> 
> 
> 

<span data-ttu-id="996b2-114">A PowerShell-paranccsal is View IssuerUri `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span><span class="sxs-lookup"><span data-stu-id="996b2-114">You can vew IssuerUri by using the PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="996b2-116">A probléma merül fel, ha azt szeretné, egynél több felső szintű tartomány hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="996b2-116">A problem arises when we want to add more than one top-level domain.</span></span>  <span data-ttu-id="996b2-117">Például tegyük fel, a telepítő összevonási az Azure AD között van, és a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="996b2-117">For example, let's say you have setup federation between Azure AD and your on-premises environment.</span></span>  <span data-ttu-id="996b2-118">A dokumentum használok bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="996b2-118">For this document I am using bmcontoso.com.</span></span>  <span data-ttu-id="996b2-119">Most már hozzáadtam egy második felső szintű tartomány bmfabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="996b2-119">Now I have added a second, top-level domain, bmfabrikam.com.</span></span>

![Tartományok](./media/active-directory-multiple-domains/domains.png)

<span data-ttu-id="996b2-121">Próbálja meg konvertálni a bmfabrikam.com tartomány átalakításakor, ha azt hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="996b2-121">When we attempt to convert our bmfabrikam.com domain to be federated, we receive an error.</span></span>  <span data-ttu-id="996b2-122">Ennek oka az, Azure AD is rendelkezik, amely nem teszi lehetővé a IssuerUri tulajdonság ugyanazt az értéket egynél több tartomány korlátozás.</span><span class="sxs-lookup"><span data-stu-id="996b2-122">The reason for this is, Azure AD has a constraint that does not allow the IssuerUri property to have the same value for more than one domain.</span></span>  

![Összevonási hiba](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a><span data-ttu-id="996b2-124">SupportMultipleDomain paraméter</span><span class="sxs-lookup"><span data-stu-id="996b2-124">SupportMultipleDomain Parameter</span></span>
<span data-ttu-id="996b2-125">A megoldás, azt kell adnia egy másik IssuerUri, amelyek segítségével teheti a `-SupportMultipleDomain` paraméter.</span><span class="sxs-lookup"><span data-stu-id="996b2-125">To workaround this, we need to add a different IssuerUri which can be done by using the `-SupportMultipleDomain` parameter.</span></span>  <span data-ttu-id="996b2-126">Ezt a paramétert a következő parancsmagokat használja:</span><span class="sxs-lookup"><span data-stu-id="996b2-126">This parameter is used with the following cmdlets:</span></span>

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

<span data-ttu-id="996b2-127">Ez a paraméter lehetővé teszi az Azure AD a IssuerUri konfigurálja, hogy annak a tartománynak a nevét alapul.</span><span class="sxs-lookup"><span data-stu-id="996b2-127">This parameter makes Azure AD configure the IssuerUri so that it is based on the name of the domain.</span></span>  <span data-ttu-id="996b2-128">Ez lesz egyedi könyvtárak között az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="996b2-128">This will be unique across directories in Azure AD.</span></span>  <span data-ttu-id="996b2-129">A paraméter használata lehetővé teszi, hogy a PowerShell-parancs végrehajtása sikeres legyen.</span><span class="sxs-lookup"><span data-stu-id="996b2-129">Using the parameter allows the PowerShell command to complete successfully.</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/convert.png)

<span data-ttu-id="996b2-131">A beállításokat az új bmfabrikam.com tartomány megnézzük látható a következő:</span><span class="sxs-lookup"><span data-stu-id="996b2-131">Looking at the settings of our new bmfabrikam.com domain you can see the following:</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/settings.png)

<span data-ttu-id="996b2-133">Vegye figyelembe, hogy `-SupportMultipleDomain` nem változtatja meg a többi végpont, amely az összevonási szolgáltatás adfs.bmcontoso.com mutasson konfigurálása még folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="996b2-133">Note that `-SupportMultipleDomain` does not change the other endpoints which are still configured to point to our federation service on adfs.bmcontoso.com.</span></span>

<span data-ttu-id="996b2-134">Egy másik művelet, amely `-SupportMultipleDomain` does, biztosítja, hogy az AD FS rendszer tartalmazza a megfelelő kibocsátó érték az Azure AD kiállított jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="996b2-134">Another thing that `-SupportMultipleDomain` does is that it ensures that the AD FS system includes the proper Issuer value in tokens issued for Azure AD.</span></span> <span data-ttu-id="996b2-135">A felhasználók egyszerű Felhasználónevük tartomány része, és ezt a tartományt a IssuerUri, azaz https://{upn utótag beállítását tesz} / adfs/services/megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="996b2-135">It does this by taking the domain portion of the users UPN and setting this as the domain in the IssuerUri, i.e. https://{upn suffix}/adfs/services/trust.</span></span> 

<span data-ttu-id="996b2-136">Így az Azure AD-hitelesítés során vagy az Office 365, a felhasználói jogkivonat IssuerUri elemének segítségével keresse meg a tartományt az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="996b2-136">Thus during authentication to Azure AD or Office 365, the IssuerUri element in the user’s token is used to locate the domain in Azure AD.</span></span>  <span data-ttu-id="996b2-137">Ha nem található egyezés a hitelesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="996b2-137">If a match cannot be found the authentication will fail.</span></span> 

<span data-ttu-id="996b2-138">Például, ha a felhasználói UPN van bsimon@bmcontoso.com, http://bmcontoso.com/adfs/services/trust úgy lesz beállítva, a jogkivonatot AD FS problémák IssuerUri elemében.</span><span class="sxs-lookup"><span data-stu-id="996b2-138">For example, if a user’s UPN is bsimon@bmcontoso.com, the IssuerUri element in the token AD FS issues will be set to http://bmcontoso.com/adfs/services/trust.</span></span> <span data-ttu-id="996b2-139">Ez fog egyezni az Azure Active Directory beállítása, és a hitelesítés sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="996b2-139">This will match the Azure AD configuration, and authentication will succeed.</span></span>

<span data-ttu-id="996b2-140">A következő értéke a testreszabott jogcímszabály, amely megvalósítja ezt a programot:</span><span class="sxs-lookup"><span data-stu-id="996b2-140">The following is the customized claim rule that implements this logic:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> <span data-ttu-id="996b2-141">A - SupportMultipleDomain kapcsoló használata, amikor új hozzáadása vagy konvertálása már hozzáadott tartomány, van szüksége a telepítő az összevont megbízhatósági kapcsolathoz azokat eredetileg támogatásához.</span><span class="sxs-lookup"><span data-stu-id="996b2-141">In order to use the -SupportMultipleDomain switch when attempting to add new or convert already added domains, you need to have setup your federated trust to support them originally.</span></span>  
> 
> 

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a><span data-ttu-id="996b2-142">Az AD FS és az Azure AD közötti megbízhatósági kapcsolat frissítése</span><span class="sxs-lookup"><span data-stu-id="996b2-142">How to update the trust between AD FS and Azure AD</span></span>
<span data-ttu-id="996b2-143">Ha nem állított be az összevont megbízhatósági kapcsolathoz AD FS és az Azure AD példányával között, szükség lehet újból létrehozni a bizalmi kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="996b2-143">If you did not setup the federated trust between AD FS and your instance of Azure AD, you may need to re-create this trust.</span></span>  <span data-ttu-id="996b2-144">Ennek az az oka eredetileg beállítása nélkül a `-SupportMultipleDomain` paraméter, a IssuerUri értéke az alapértelmezett értékkel.</span><span class="sxs-lookup"><span data-stu-id="996b2-144">This is because, when it is originally setup without the `-SupportMultipleDomain` parameter, the IssuerUri is set with the default value.</span></span>  <span data-ttu-id="996b2-145">Az alábbi képernyőképen látható a IssuerUri https://adfs.bmcontoso.com/adfs/services/trust értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="996b2-145">In the screenshot below you can see the IssuerUri is set to https://adfs.bmcontoso.com/adfs/services/trust.</span></span>

<span data-ttu-id="996b2-146">Így mostantól, ha azt egy új tartomány sikeresen hozzáadta az Azure AD portálon, majd próbálja meg konvertálni használatával `Convert-MsolDomaintoFederated -DomainName <your domain>`, azt a következő hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="996b2-146">So now, if we have successfully added an new domain in the Azure AD portal and then attempt to convert it using `Convert-MsolDomaintoFederated -DomainName <your domain>`, we get the following error.</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/trust1.png)

<span data-ttu-id="996b2-148">Ha megpróbálja felvenni a `-SupportMultipleDomain` azt hibaüzenet jelenik meg a következő kapcsolóhoz:</span><span class="sxs-lookup"><span data-stu-id="996b2-148">If you try to add the `-SupportMultipleDomain` switch we will receive the following error:</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/trust2.png)

<span data-ttu-id="996b2-150">Egyszerűen a futtatni kívánt `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` az eredeti tartományon is jelent hibát.</span><span class="sxs-lookup"><span data-stu-id="996b2-150">Simply trying to run `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` on the original domain will also result in an error.</span></span>

![Összevonási hiba](./media/active-directory-multiple-domains/trust3.png)

<span data-ttu-id="996b2-152">Az alábbi lépések segítségével további felső szintű tartomány hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="996b2-152">Use the steps below to add an additional top-level domain.</span></span>  <span data-ttu-id="996b2-153">Ha már hozzáadott egy tartományhoz, és nem használja a `-SupportMultipleDomain` eltávolítása és frissítése az eredeti tartomány lépései paraméter kezdődik.</span><span class="sxs-lookup"><span data-stu-id="996b2-153">If you have already added a domain and did not use the `-SupportMultipleDomain` parameter start with the steps for removing and updating your original domain.</span></span>  <span data-ttu-id="996b2-154">Ha nem adja hozzá a legfelső szintű tartomány még Kezdésként használhatja az PowerShell Azure AD Connect használatával tartomány lépései.</span><span class="sxs-lookup"><span data-stu-id="996b2-154">If you have not added a top-level domain yet you can start with the steps for adding a domain using PowerShell of Azure AD Connect.</span></span>

<span data-ttu-id="996b2-155">Az alábbi lépések segítségével a Microsoft Online bizalmi kapcsolatot, és az eredeti tartomány frissítése.</span><span class="sxs-lookup"><span data-stu-id="996b2-155">Use the following steps to remove the Microsoft Online trust and update your original domain.</span></span>

1. <span data-ttu-id="996b2-156">Az AD FS összevonási kiszolgálón nyissa meg **AD FS kezelő.**</span><span class="sxs-lookup"><span data-stu-id="996b2-156">On your AD FS federation server open **AD FS Management.**</span></span> 
2. <span data-ttu-id="996b2-157">Bontsa ki a bal oldali **megbízhatósági kapcsolatok** és **függő entitás Megbízhatóságai**</span><span class="sxs-lookup"><span data-stu-id="996b2-157">On the left, expand **Trust Relationships** and **Relying Party Trusts**</span></span>
3. <span data-ttu-id="996b2-158">A jobb oldalon törölje a **Microsoft Office 365 Identitásplatformmal** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="996b2-158">On the right, delete the **Microsoft Office 365 Identity Platform** entry.</span></span>
   <span data-ttu-id="996b2-159">![Távolítsa el a Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span><span class="sxs-lookup"><span data-stu-id="996b2-159">![Remove Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span></span>
4. <span data-ttu-id="996b2-160">A gépen, amelyen van [Active Directory modul Windows Powershellhez készült Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) telepítve van-e futtassa a következő: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="996b2-160">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run the following: `$cred=Get-Credential`.</span></span>  
5. <span data-ttu-id="996b2-161">Adja meg a felhasználónevet és jelszót egy globális rendszergazda összevonja az Azure AD-tartomány</span><span class="sxs-lookup"><span data-stu-id="996b2-161">Enter the username and password of a global administrator for the Azure AD domain you are federating with</span></span>
6. <span data-ttu-id="996b2-162">Adja meg a PowerShellben`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="996b2-162">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
7. <span data-ttu-id="996b2-163">Adja meg a PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span><span class="sxs-lookup"><span data-stu-id="996b2-163">In PowerShell enter `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span></span>  <span data-ttu-id="996b2-164">Ez az eredeti tartomány esetén.</span><span class="sxs-lookup"><span data-stu-id="996b2-164">This is for the original domain.</span></span>  <span data-ttu-id="996b2-165">A fenti tartományok lenne használatával:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span><span class="sxs-lookup"><span data-stu-id="996b2-165">So using the above domains it would be:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span></span>

<span data-ttu-id="996b2-166">Az alábbi lépések segítségével a PowerShell használatával új legfelső szintű tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="996b2-166">Use the following steps to add the new top-level domain using PowerShell</span></span>

1. <span data-ttu-id="996b2-167">A gépen, amelyen van [Active Directory modul Windows Powershellhez készült Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) telepítve van-e futtassa a következő: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="996b2-167">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run the following: `$cred=Get-Credential`.</span></span>  
2. <span data-ttu-id="996b2-168">Adja meg a felhasználónevet és jelszót egy globális rendszergazda összevonja az Azure AD-tartomány</span><span class="sxs-lookup"><span data-stu-id="996b2-168">Enter the username and password of a global administrator for the Azure AD domain you are federating with</span></span>
3. <span data-ttu-id="996b2-169">Adja meg a PowerShellben`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="996b2-169">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
4. <span data-ttu-id="996b2-170">Adja meg a PowerShellben`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span><span class="sxs-lookup"><span data-stu-id="996b2-170">In PowerShell enter `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span></span>

<span data-ttu-id="996b2-171">A következő lépésekkel adja hozzá az új legfelső szintű tartomány, az Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="996b2-171">Use the following steps to add the new top-level domain using Azure AD Connect.</span></span>

1. <span data-ttu-id="996b2-172">Indítsa el az Azure AD Connect az asztalon vagy start menüben</span><span class="sxs-lookup"><span data-stu-id="996b2-172">Launch Azure AD Connect from the desktop or start menu</span></span>
2. <span data-ttu-id="996b2-173">"Egy további Azure AD-tartomány hozzáadása" Válasszon ![adnia egy további Azure AD-tartomány](./media/active-directory-multiple-domains/add1.png)</span><span class="sxs-lookup"><span data-stu-id="996b2-173">Choose “Add an additional Azure AD Domain” ![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add1.png)</span></span>
3. <span data-ttu-id="996b2-174">Adja meg az Azure AD és az Active Directory hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="996b2-174">Enter your Azure AD and Active Directory credentials</span></span>
4. <span data-ttu-id="996b2-175">Válassza ki a második tartomány szeretne beállítani az összevonáshoz.</span><span class="sxs-lookup"><span data-stu-id="996b2-175">Select the second domain you wish to configure for federation.</span></span>
   <span data-ttu-id="996b2-176">![Felvesz egy további Azure AD-tartomány](./media/active-directory-multiple-domains/add2.png)</span><span class="sxs-lookup"><span data-stu-id="996b2-176">![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add2.png)</span></span>
5. <span data-ttu-id="996b2-177">Kattintson a telepítés</span><span class="sxs-lookup"><span data-stu-id="996b2-177">Click Install</span></span>

### <a name="verify-the-new-top-level-domain"></a><span data-ttu-id="996b2-178">Ellenőrizze az új legfelső szintű tartomány</span><span class="sxs-lookup"><span data-stu-id="996b2-178">Verify the new top-level domain</span></span>
<span data-ttu-id="996b2-179">A PowerShell-paranccsal `Get-MsolDomainFederationSettings -DomainName <your domain>`tekintheti meg a frissített IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="996b2-179">By using the PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`you can view the updated IssuerUri.</span></span>  <span data-ttu-id="996b2-180">Az alábbi képernyőfelvételen látható az összevonási beállítások lettek frissítve az az eredeti tartomány http://bmcontoso.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="996b2-180">The screenshot below shows the federation settings were updated on our original domain http://bmcontoso.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="996b2-182">És az új tartományon IssuerUri https://bmfabrikam.com/adfs/services/trust értékre lett állítva</span><span class="sxs-lookup"><span data-stu-id="996b2-182">And the IssuerUri on our new domain has been set to https://bmfabrikam.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a><span data-ttu-id="996b2-184">Altartományok támogatása</span><span class="sxs-lookup"><span data-stu-id="996b2-184">Support for Sub-domains</span></span>
<span data-ttu-id="996b2-185">Altartomány, a kezelt módon az Azure AD-tartományok miatt hozzáadásakor azt öröklik a szülő beállításait.</span><span class="sxs-lookup"><span data-stu-id="996b2-185">When you add a sub-domain, because of the way Azure AD handled domains, it will inherit the settings of the parent.</span></span>  <span data-ttu-id="996b2-186">Ez azt jelenti, hogy a IssuerUri a szülők egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="996b2-186">This means that the IssuerUri needs to match the parents.</span></span>

<span data-ttu-id="996b2-187">Így lehetővé teszi, hogy tegyük fel például, hogy rendelkezem bmcontoso.com, és adja meg az corp.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="996b2-187">So lets say for example that I have bmcontoso.com and then add corp.bmcontoso.com.</span></span>  <span data-ttu-id="996b2-188">Ez azt jelenti, hogy egy felhasználó a corp.bmcontoso.com IssuerUri kell lennie **http://bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="996b2-188">This means that the IssuerUri for a user from corp.bmcontoso.com will need to be **http://bmcontoso.com/adfs/services/trust.**</span></span>  <span data-ttu-id="996b2-189">Azonban a standard szabály az Azure AD fent megvalósított hoz létre a jogkivonatot kibocsátó, az **http://corp.bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="996b2-189">However the standard rule implemented above for Azure AD, will generate a token with an issuer as **http://corp.bmcontoso.com/adfs/services/trust.**</span></span> <span data-ttu-id="996b2-190">amely nem azonos a tartomány szükséges érték, és a hitelesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="996b2-190">which will not match the domain's required value and authentication will fail.</span></span>

### <a name="how-to-enable-support-for-sub-domains"></a><span data-ttu-id="996b2-191">Altartományok támogatásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="996b2-191">How To enable support for sub-domains</span></span>
<span data-ttu-id="996b2-192">Ahhoz, hogy elkerüléséhez az AD FS függőentitás-megbízhatóságot Microsoft Online frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="996b2-192">In order to work around this the AD FS relying party trust for Microsoft Online needs to be updated.</span></span>  <span data-ttu-id="996b2-193">Ehhez konfigurálnia kell egyéni jogcímszabályt, hogy azt törli a felhasználói UPN-utótagot a bármely altartományok ki az egyéni kibocsátó érték kiszámításakor.</span><span class="sxs-lookup"><span data-stu-id="996b2-193">To do this, you must configure a custom claim rule so that it strips off any sub-domains from the user’s UPN suffix when constructing the custom Issuer value.</span></span> 

<span data-ttu-id="996b2-194">A következő jogcím fog tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="996b2-194">The following claim will do this:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
<span data-ttu-id="996b2-195">A reguláris kifejezés utolsó számát a hány szülő tartományt, a legfelső szintű tartomány van beállítva.</span><span class="sxs-lookup"><span data-stu-id="996b2-195">The last number in the regular expression set the how many parent domains there is in your root domain.</span></span> <span data-ttu-id="996b2-196">Itt az i bmcontoso.com rendelkezik, ezért két szülő tartomány szükség.</span><span class="sxs-lookup"><span data-stu-id="996b2-196">Here i have bmcontoso.com so two parent domains are necessary.</span></span> <span data-ttu-id="996b2-197">Ha három szülő tartományok tartandó volt-e (pl.: corp.bmcontoso.com), majd a szám kellett volna lennie három.</span><span class="sxs-lookup"><span data-stu-id="996b2-197">If three parent domains were to be kept (i.e.: corp.bmcontoso.com), then the number would have been three.</span></span> <span data-ttu-id="996b2-198">Eventualy széles jelezni lehet, az egyeztetés mindig lesz a tartományok maximálisan megengedett kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="996b2-198">Eventualy a range can be indicated, the match will always be made to match the maximum of domains.</span></span> <span data-ttu-id="996b2-199">"a(z) {2,3}" fog egyezni a két-három tartományok (pl.: bmfabrikam.com és corp.bmcontoso.com).</span><span class="sxs-lookup"><span data-stu-id="996b2-199">"{2,3}" will match two to three domains (i.e.: bmfabrikam.com and corp.bmcontoso.com).</span></span>

<span data-ttu-id="996b2-200">A következő lépésekkel adja hozzá az altartományok támogatásához egyéni jogcímleírásokat.</span><span class="sxs-lookup"><span data-stu-id="996b2-200">Use the following steps to add a custom claim to support sub-domains.</span></span>

1. <span data-ttu-id="996b2-201">Nyissa meg az AD FS-Szolgáltatáskezelés</span><span class="sxs-lookup"><span data-stu-id="996b2-201">Open AD FS Management</span></span>
2. <span data-ttu-id="996b2-202">Kattintson jobb gombbal a Microsoft Online függő Entitás megbízhatóságát, majd válassza ki a jogcímszabályok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="996b2-202">Right click the Microsoft Online RP trust and choose Edit Claim rules</span></span>
3. <span data-ttu-id="996b2-203">Válassza ki a harmadik jogcímszabály, és cserélje le ![jogcímszabályok szerkesztése](./media/active-directory-multiple-domains/sub1.png)</span><span class="sxs-lookup"><span data-stu-id="996b2-203">Select the third claim rule, and replace ![Edit claim](./media/active-directory-multiple-domains/sub1.png)</span></span>
4. <span data-ttu-id="996b2-204">Cserélje le a jelenlegi jogcímek:</span><span class="sxs-lookup"><span data-stu-id="996b2-204">Replace the current claim:</span></span>
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Cserélje ki a jogcímet](./media/active-directory-multiple-domains/sub2.png)

5. <span data-ttu-id="996b2-206">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="996b2-206">Click Ok.</span></span>  <span data-ttu-id="996b2-207">Az Alkalmaz gombra.</span><span class="sxs-lookup"><span data-stu-id="996b2-207">Click Apply.</span></span>  <span data-ttu-id="996b2-208">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="996b2-208">Click Ok.</span></span>  <span data-ttu-id="996b2-209">Zárja be az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="996b2-209">Close AD FS Management.</span></span>

