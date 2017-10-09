---
title: "aaaFederating több Azure AD az egyetlen AD FS |} Microsoft Docs"
description: "Ez a dokumentum megtudhatja, hogyan toofederate több Azure AD egy egyetlen Active Directory Összevonási szolgáltatásokkal."
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
ms.openlocfilehash: 442192896b3b13f7bf9388396cd3769e194329d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a><span data-ttu-id="c887d-104">Több Azure AD-példány összevonása egyetlen AD FS-példánnyal</span><span class="sxs-lookup"><span data-stu-id="c887d-104">Federate multiple instances of Azure AD with single instance of AD FS</span></span>

<span data-ttu-id="c887d-105">Egyetlen magas rendelkezésre állású AD FS farm összevonhat több erdőt, ha azok között kétirányú megbízhatósági kapcsolat áll fenn.</span><span class="sxs-lookup"><span data-stu-id="c887d-105">A single high available AD FS farm can federate multiple forests if they have 2-way trust between them.</span></span> <span data-ttu-id="c887d-106">Előfordulhat, hogy több erdők, vagy nem felel meg az toohello azonos Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="c887d-106">These multiple forests may or may not correspond toohello same Azure Active Directory.</span></span> <span data-ttu-id="c887d-107">Ez a cikk útmutatás hogyan tooconfigure összevonási egy egyetlen AD FS üzembe helyezése és egynél több közötti erdőket, hogy szinkronizálás toodifferent az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c887d-107">This article provides instructions on how tooconfigure federation between a single AD FS deployment and more than one forests that sync toodifferent Azure AD.</span></span>

![Több-bérlős összevonás egyetlen AD FS-szel](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> <span data-ttu-id="c887d-109">Ebben az esetben az eszközvisszaírás és automatikus eszköz-csatlakoztatás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c887d-109">Device writeback and automatic device join are not supported in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="c887d-110">Az Azure AD Connect nem lehet ebben a forgatókönyvben használt tooconfigure összevonási, mint az Azure AD Connect összevonási tartományok konfigurálható egy egyetlen Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c887d-110">Azure AD Connect cannot be used tooconfigure federation in this scenario as Azure AD Connect can configure federation for domains in a single Azure AD.</span></span>

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a><span data-ttu-id="c887d-111">Az AD FS több Azure AD-címtárral való összevonásának lépései</span><span class="sxs-lookup"><span data-stu-id="c887d-111">Steps for federating AD FS with multiple Azure AD</span></span>

<span data-ttu-id="c887d-112">Fontolja meg egy Azure Active Directory contoso.onmicrosoft.com a contoso.com tartomány már össze van vonva az AD FS hello helyszíni contoso.com a helyszíni Active Directory-környezetbe telepített.</span><span class="sxs-lookup"><span data-stu-id="c887d-112">Consider a domain contoso.com in Azure Active Directory contoso.onmicrosoft.com is already federated with hello AD FS on-premises installed in contoso.com on-premises Active Directory environment.</span></span> <span data-ttu-id="c887d-113">A fabrikam.com a fabrikam.onmicrosoft.com Azure Active Directory-címtár egy tartománya.</span><span class="sxs-lookup"><span data-stu-id="c887d-113">Fabrikam.com is a domain in fabrikam.onmicrosoft.com Azure Active Directory.</span></span>

##<a name="step-1-establish-a-two-way-trust"></a><span data-ttu-id="c887d-114">1. lépés: A kétirányú megbízhatósági kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c887d-114">Step 1: Establish a two-way trust</span></span>
 
<span data-ttu-id="c887d-115">Az AD FS a contoso.com toobe képes tooauthenticate felhasználókat, fabrikam.com a kétirányú megbízhatósági kapcsolattal a contoso.com és fabrikam.com között van szükség. Hajtsa végre az ezen hello iránymutatás [cikk](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello kétirányú megbízhatósági kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="c887d-115">For AD FS in contoso.com toobe able tooauthenticate users in fabrikam.com, a two-way trust is needed between contoso.com and fabrikam.com. Follow hello guideline in this [article](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello two-way trust.</span></span>
 
##<a name="step-2-modify-contosocom-federation-settings"></a><span data-ttu-id="c887d-116">2. lépés: A contoso.com összevonási beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="c887d-116">Step 2: Modify contoso.com federation settings</span></span> 
 
<span data-ttu-id="c887d-117">hello alapértelmezett egy összevont egyetlen tartományt tooAD FS beállítása nem "http://ADFSServiceFQDN/adfs/services/trust", például "http://fs.contoso.com/adfs/services/trust".</span><span class="sxs-lookup"><span data-stu-id="c887d-117">hello default issuer set for a single domain federated tooAD FS is "http://ADFSServiceFQDN/adfs/services/trust", for example, “http://fs.contoso.com/adfs/services/trust”.</span></span> <span data-ttu-id="c887d-118">Az Azure Active Directory összevont tartományonként egyedi kiállítót igényel.</span><span class="sxs-lookup"><span data-stu-id="c887d-118">Azure Active Directory requires unique issuer for each federated domain.</span></span> <span data-ttu-id="c887d-119">Hello ugyanazt az AD FS toofederate két tartomány lesz, mivel a hello kibocsátó érték módosítható úgy, hogy minden egyes tartományhoz, az AD FS az Azure Active Directoryval federates egyedi toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c887d-119">Since hello same AD FS is going toofederate two domains, hello issuer value needs toobe modified so that it is unique for each domain AD FS federates with Azure Active Directory.</span></span> 
 
<span data-ttu-id="c887d-120">Hello AD FS-kiszolgálón nyissa meg az Azure AD PowerShell segítségével, és hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c887d-120">On hello AD FS server, open Azure AD PowerShell and perform hello following steps:</span></span>
 
<span data-ttu-id="c887d-121">Csatlakozás az Azure Active Directory - tartománynév contoso.com contoso.com frissítés-MsolFederatedDomain hello tartomány contoso.com Connect-MsolService frissítés hello összevonási beállításokat tartalmazó toohello – SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="c887d-121">Connect toohello Azure Active Directory that contains hello domain contoso.com Connect-MsolService Update hello federation settings for contoso.com Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain</span></span>
 
<span data-ttu-id="c887d-122">Tartomány-összevonási beállítás hello kibocsátó változnak túl "http://contoso.com/adfs/services/trust" és egy kiállítási jogcím-szabályt a rendszer az Azure AD hello függő entitás megbízhatóságának tooissue hello megfelelő issuerId érték alapján hello egyszerű Felhasználónévi utótagot adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="c887d-122">Issuer in hello domain federation setting will be changed too"http://contoso.com/adfs/services/trust" and an issuance claim rule will be added for hello Azure AD Relying Party Trust tooissue hello correct issuerId value based on hello UPN suffix.</span></span>
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a><span data-ttu-id="c887d-123">3. lépés: A fabrikam.com összevonása az AD FS-szel</span><span class="sxs-lookup"><span data-stu-id="c887d-123">Step 3: Federate fabrikam.com with AD FS</span></span>
 
<span data-ttu-id="c887d-124">Az Azure AD powershell munkamenet hajtsa végre a lépéseket követve hello: tooAzure hello tartomány fabrikam.com tartalmazó Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="c887d-124">In Azure AD powershell session perform hello following steps: Connect tooAzure Active Directory that contains hello domain fabrikam.com</span></span>

    Connect-MsolService
<span data-ttu-id="c887d-125">Átalakítás hello, fabrikam.com felügyelt tartomány toofederated:</span><span class="sxs-lookup"><span data-stu-id="c887d-125">Convert hello fabrikam.com managed domain toofederated:</span></span>

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
<span data-ttu-id="c887d-126">hello fent művelet fog összevonni hello tartomány fabrikam.com hello ugyanazt az AD FS a.</span><span class="sxs-lookup"><span data-stu-id="c887d-126">hello above operation will federate hello domain fabrikam.com with hello same AD FS.</span></span> <span data-ttu-id="c887d-127">Mindkét tartomány Get-MsolDomainFederationSettings használatával ellenőrizheti hello tartomány beállításait.</span><span class="sxs-lookup"><span data-stu-id="c887d-127">You can verify hello domain settings by using Get-MsolDomainFederationSettings for both domains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c887d-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c887d-128">Next steps</span></span>
[<span data-ttu-id="c887d-129">Az Active Directory csatlakoztatása az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="c887d-129">Connect Active Directory with Azure Active Directory</span></span>](active-directory-aadconnect.md)
