---
title: "aaaAzure AD + Active Directory követelményeit, az Azure Remoteappban |} Microsoft Docs"
description: Megtudhatja, hogyan tooset fel az Active Directory toowork az Azure RemoteApp.
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c1c4a7ad6fb96ec4d479fdc231f03d81b58b2b71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a><span data-ttu-id="940d6-103">Az Azure AD + Azure RemoteApp Active Directory követelményei</span><span class="sxs-lookup"><span data-stu-id="940d6-103">Azure AD + Active Directory requirements for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="940d6-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="940d6-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="940d6-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="940d6-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="940d6-106">Az Azure RemoteApp hibrid gyűjtemény vagy egy felhőalapú gyűjtemény, amelyet az AD Connect használatával toofederate toodo hello következőkre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="940d6-106">For your Azure RemoteApp hybrid collection or for a cloud collection that you want toofederate using AD Connect, you need toodo hello following.</span></span>

### <a name="connect-azure-ad-and-active-directory"></a><span data-ttu-id="940d6-107">Az Azure AD Connect és Active Directory</span><span class="sxs-lookup"><span data-stu-id="940d6-107">Connect Azure AD and Active Directory</span></span>
<span data-ttu-id="940d6-108">Ha azt szeretné tooconnect, az Azure AD-bérlő és a helyszíni Active Directory-környezeteket, használja az AD Connect.</span><span class="sxs-lookup"><span data-stu-id="940d6-108">If you want tooconnect your Azure AD tenant and your on-premises Active Directory environments, use AD Connect.</span></span> <span data-ttu-id="940d6-109">Csak eltarthat, [4 kattintással](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello két könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="940d6-109">It will take you only [4 clicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello two directories.</span></span>

<span data-ttu-id="940d6-110">Megjegyzés: - a címtár-szinkronizálás szükséges a hibrid gyűjtemények.</span><span class="sxs-lookup"><span data-stu-id="940d6-110">Note - Directory synchronization is required for hybrid collections.</span></span>

### <a name="make-sure-your-domaincom-match"></a><span data-ttu-id="940d6-111">Győződjön meg arról, hogy a "@domain.com" felel meg</span><span class="sxs-lookup"><span data-stu-id="940d6-111">Make sure your "@domain.com" match</span></span>
<span data-ttu-id="940d6-112">Mielőtt elkezdené, győződjön meg arról, hogy a helyi erdő megfelel hello utótag, az Azure AD-tartomány UPN hello listájában.</span><span class="sxs-lookup"><span data-stu-id="940d6-112">Before you get started, make sure that hello UPN for your on-premises forest matches hello suffix of your Azure AD domain.</span></span> 

<span data-ttu-id="940d6-113">Miután beállította UPN-tartomány utótag hello Azure AD-ben, az Azure RemoteApp-naplózás minden felhasználó fog bejelentkezni "@ felhasználó<hello suffix you set up>."</span><span class="sxs-lookup"><span data-stu-id="940d6-113">After you set up hello UPN domain suffix in Azure AD, all users logging into Azure RemoteApp will log in as "user@<hello suffix you set up>."</span></span> <span data-ttu-id="940d6-114">Győződjön meg arról, hogy a felhasználók is bejelentkezhetnek hello be azonos user@suffix hello helyszíni tartományába.</span><span class="sxs-lookup"><span data-stu-id="940d6-114">Make sure that users can also log in with hello same user@suffix into hello on-premises domain.</span></span> <span data-ttu-id="940d6-115">Bizonyos esetekben állíthat be egy tartománynév-az memóriakapacitásánál felhasználói hello ugyanaz a tartományi utótag meg Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="940d6-115">In certain cases you can set up one domain name in Azure AD while specifying a different domain suffix for hello user on-prem.</span></span> <span data-ttu-id="940d6-116">Ebben az esetben a felhasználók nem fogja tudni tooconnect tooany tartományhoz csatlakoztatott számítógépek vagy Azure RemoteApp erőforrások.</span><span class="sxs-lookup"><span data-stu-id="940d6-116">In this case, your users won't be able tooconnect tooany domain-joined computers or resources through Azure RemoteApp.</span></span>

<span data-ttu-id="940d6-117">Például, ha beállította az UPN-tartomány utótag az aad-ben, mint a contoso.com, de egyes felhasználók a helyi/AD azzal konfigurált toolog @contoso.uk, akkor azoknak a felhasználóknak nem lesz képes toocorrectly jelentkezzen be a hello ARA gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="940d6-117">For example, if you set up your UPN domain suffix in AAD as contoso.com, but some users on premises/AD are configured toolog in with @contoso.uk, then those users will not be able toocorrectly log into hello ARA collection.</span></span> <span data-ttu-id="940d6-118">Legyen egyszerű felhasználónév az aad-ben és az AD felhasználók hello azonos hello bejelentkezési toobe lehetséges"</span><span class="sxs-lookup"><span data-stu-id="940d6-118">Users UPN in AAD and AD must be hello same for hello login toobe possible”</span></span>

### <a name="create-objects-for-azure-remoteapp"></a><span data-ttu-id="940d6-119">Az Azure RemoteApp objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="940d6-119">Create objects for Azure RemoteApp</span></span>
<span data-ttu-id="940d6-120">A helyszíni Active Directory-objektumok a következő toocreate hello is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="940d6-120">You also need toocreate hello following on-premises Active Directory objects:</span></span>

* <span data-ttu-id="940d6-121">A szolgáltatás fiók tooprovide hozzáférés toodomain erőforrásainak RemoteApp programok RDSH végpontok toohello a helyi tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="940d6-121">A service account tooprovide access toodomain resources for RemoteApp programs by joining RDSH end points toohello on-premises domain.</span></span>
* <span data-ttu-id="940d6-122">Egy szervezeti egységet (OU) toocontain RemoteApp gép objektumok.</span><span class="sxs-lookup"><span data-stu-id="940d6-122">An Organizational Unit (OU) toocontain RemoteApp machine objects.</span></span> <span data-ttu-id="940d6-123">Szervezeti egység hello használata javasolt (de nem kötelező) tooisolate hello fiókok és szabályzatok használja, akkor RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="940d6-123">Use of hello OU is recommended (but not required) tooisolate hello accounts and policies you will use with RemoteApp.</span></span>

<span data-ttu-id="940d6-124">Kell mindkét ezeket az objektumokat a RemoteApp-gyűjtemény létrehozásakor ezért lehet, hogy toodo ezeket a lépéseket először.</span><span class="sxs-lookup"><span data-stu-id="940d6-124">You need both of these objects when you create your RemoteApp collection, so be sure toodo these steps first.</span></span>

