---
title: "Több Azure AD összevonása egyetlen AD FS-szel | Microsoft Docs"
description: "Ebből a dokumentumból megtudhatja, hogyan vonhat össze több Azure AD-t egyetlen AD FS-szel."
keywords: federate, ADFS, AD FS, multiple tenants, single AD FS, one ADFS, multi-tenant federation, multi-forest adfs, aad connect, federation, cross-tenant federation
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.openlocfilehash: 436bf5905d2b203dc4cceea97f4fb90593df7111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a><span data-ttu-id="1fa07-104">Több Azure AD-példány összevonása egyetlen AD FS-példánnyal</span><span class="sxs-lookup"><span data-stu-id="1fa07-104">Federate multiple instances of Azure AD with single instance of AD FS</span></span>

<span data-ttu-id="1fa07-105">Egyetlen magas rendelkezésre állású AD FS farm összevonhat több erdőt, ha azok között kétirányú megbízhatósági kapcsolat áll fenn.</span><span class="sxs-lookup"><span data-stu-id="1fa07-105">A single high available AD FS farm can federate multiple forests if they have 2-way trust between them.</span></span> <span data-ttu-id="1fa07-106">Ezek az erdők megfelelhetnek ugyanannak az Azure Active Directory-címtárnak, vagy sem.</span><span class="sxs-lookup"><span data-stu-id="1fa07-106">These multiple forests may or may not correspond to the same Azure Active Directory.</span></span> <span data-ttu-id="1fa07-107">Ez a cikk útmutatást nyújt az összevonás konfigurálásához egyetlen AD FS környezet és egynél több, más Azure AD-címtárra szinkronizáló erdő között.</span><span class="sxs-lookup"><span data-stu-id="1fa07-107">This article provides instructions on how to configure federation between a single AD FS deployment and more than one forests that sync to different Azure AD.</span></span>

![Több-bérlős összevonás egyetlen AD FS-szel](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> <span data-ttu-id="1fa07-109">Ebben az esetben az eszközvisszaírás és automatikus eszköz-csatlakoztatás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="1fa07-109">Device writeback and automatic device join are not supported in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="1fa07-110">Az Azure AD Connect esetünkben nem használható, mivel az Azure AD Connect az egyetlen Azure AD-címtárban lévő tartományok esetén használható az összevonás konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="1fa07-110">Azure AD Connect cannot be used to configure federation in this scenario as Azure AD Connect can configure federation for domains in a single Azure AD.</span></span>

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a><span data-ttu-id="1fa07-111">Az AD FS több Azure AD-címtárral való összevonásának lépései</span><span class="sxs-lookup"><span data-stu-id="1fa07-111">Steps for federating AD FS with multiple Azure AD</span></span>

<span data-ttu-id="1fa07-112">Vegye figyelembe, hogy a contoso.com tartomány a contoso.onmicrosoft.com Azure Active Directory-címtárban már össze van vonva a contoso.com helyszíni Active Directory-környezetbe telepített helyszíni AD FS-szel.</span><span class="sxs-lookup"><span data-stu-id="1fa07-112">Consider a domain contoso.com in Azure Active Directory contoso.onmicrosoft.com is already federated with the AD FS on-premises installed in contoso.com on-premises Active Directory environment.</span></span> <span data-ttu-id="1fa07-113">A fabrikam.com a fabrikam.onmicrosoft.com Azure Active Directory-címtár egy tartománya.</span><span class="sxs-lookup"><span data-stu-id="1fa07-113">Fabrikam.com is a domain in fabrikam.onmicrosoft.com Azure Active Directory.</span></span>

##<a name="step-1-establish-a-two-way-trust"></a><span data-ttu-id="1fa07-114">1. lépés: A kétirányú megbízhatósági kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="1fa07-114">Step 1: Establish a two-way trust</span></span>
 
<span data-ttu-id="1fa07-115">Ahhoz, hogy a contoso.com-beli AD FS hitelesíthesse a fabrikam.com-beli felhasználókat, kétirányú megbízhatósági kapcsolatra van szükség a contoso.com és fabrikam.com között.</span><span class="sxs-lookup"><span data-stu-id="1fa07-115">For AD FS in contoso.com to be able to authenticate users in fabrikam.com, a two-way trust is needed between contoso.com and fabrikam.com.</span></span> <span data-ttu-id="1fa07-116">A k kétirányú megbízhatósági kapcsolat létrehozásához kövesse a [cikk](https://technet.microsoft.com/library/cc816590.aspx) iránymutatását.</span><span class="sxs-lookup"><span data-stu-id="1fa07-116">Follow the guideline in this [article](https://technet.microsoft.com/library/cc816590.aspx) to create the two-way trust.</span></span>
 
##<a name="step-2-modify-contosocom-federation-settings"></a><span data-ttu-id="1fa07-117">2. lépés: A contoso.com összevonási beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="1fa07-117">Step 2: Modify contoso.com federation settings</span></span> 
 
<span data-ttu-id="1fa07-118">Az AD FS-szel összevont egyetlen tartományhoz beállított alapértelmezett kiállító: „http://ADFSSzolgáltatásFQDN/adfs/services/trust”, például „http://fs.contoso.com/adfs/services/trust”.</span><span class="sxs-lookup"><span data-stu-id="1fa07-118">The default issuer set for a single domain federated to AD FS is "http://ADFSServiceFQDN/adfs/services/trust", for example, “http://fs.contoso.com/adfs/services/trust”.</span></span> <span data-ttu-id="1fa07-119">Az Azure Active Directory összevont tartományonként egyedi kiállítót igényel.</span><span class="sxs-lookup"><span data-stu-id="1fa07-119">Azure Active Directory requires unique issuer for each federated domain.</span></span> <span data-ttu-id="1fa07-120">Mivel ugyanaz az AD FS fog két tartományt összevonni, a kiállító értékét módosítani kell, hogy minden egyes tartományhoz egyedi legyen, amelyet az AD FS az Azure Active Directoryval összevon.</span><span class="sxs-lookup"><span data-stu-id="1fa07-120">Since the same AD FS is going to federate two domains, the issuer value needs to be modified so that it is unique for each domain AD FS federates with Azure Active Directory.</span></span> 
 
<span data-ttu-id="1fa07-121">Az AD FS-kiszolgálón nyissa meg az Azure AD PowerShellt, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1fa07-121">On the AD FS server, open Azure AD PowerShell and perform the following steps:</span></span>
 
<span data-ttu-id="1fa07-122">Csatlakozzon a contoso.com tartományt tartalmazó Azure Active Directory-címtárhoz Connect-MsolService frissítse a contoso.com összevonási beállításait Update-MsolFederatedDomain -tartománynév.contoso.com –SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="1fa07-122">Connect to the Azure Active Directory that contains the domain contoso.com Connect-MsolService Update the federation settings for contoso.com Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain</span></span>
 
<span data-ttu-id="1fa07-123">A tartomány-összevonási beállításbeli kiállító a „http://contoso.com/adfs/services/trust” értékre változik, és egy kiállítási jogcímszabály hozzáadására kerül sor az Azure AD függő entitás megbízhatóságához a megfelelő issuerId érték utótagja alapján.</span><span class="sxs-lookup"><span data-stu-id="1fa07-123">Issuer in the domain federation setting will be changed to "http://contoso.com/adfs/services/trust" and an issuance claim rule will be added for the Azure AD Relying Party Trust to issue the correct issuerId value based on the UPN suffix.</span></span>
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a><span data-ttu-id="1fa07-124">3. lépés: A fabrikam.com összevonása az AD FS-szel</span><span class="sxs-lookup"><span data-stu-id="1fa07-124">Step 3: Federate fabrikam.com with AD FS</span></span>
 
<span data-ttu-id="1fa07-125">Az Azure AD PowerShell-munkamenetben hajtsa végre a következő lépéseket: Csatlakozzon a fabrikam.com tartományt tartalmazó Azure Active Directory-címtárhoz</span><span class="sxs-lookup"><span data-stu-id="1fa07-125">In Azure AD powershell session perform the following steps: Connect to Azure Active Directory that contains the domain fabrikam.com</span></span>

    Connect-MsolService
<span data-ttu-id="1fa07-126">Konvertálja a fabrikam.com felügyelt tartományt összevontra:</span><span class="sxs-lookup"><span data-stu-id="1fa07-126">Convert the fabrikam.com managed domain to federated:</span></span>

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
<span data-ttu-id="1fa07-127">A fenti művelet összevonja a fabrikam.com tartományt ugyanazon AD FS-szel.</span><span class="sxs-lookup"><span data-stu-id="1fa07-127">The above operation will federate the domain fabrikam.com with the same AD FS.</span></span> <span data-ttu-id="1fa07-128">Mindkét tartományban a Get-MsolDomainFederationSettings használatával ellenőrizheti a tartomány beállításait.</span><span class="sxs-lookup"><span data-stu-id="1fa07-128">You can verify the domain settings by using Get-MsolDomainFederationSettings for both domains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fa07-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fa07-129">Next steps</span></span>
[<span data-ttu-id="1fa07-130">Az Active Directory csatlakoztatása az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="1fa07-130">Connect Active Directory with Azure Active Directory</span></span>](active-directory-aadconnect.md)
