---
title: "Office 365 függő entitás megbízhatóságához aaaChange aláírás-kivonatoló algoritmus |} Microsoft Docs"
description: "Ezen a lapon útmutatást nyújt a SHA-képzési algoritmus összevonási megbízhatósági kapcsolat, és az Office 365 módosítása"
keywords: "SHA1, SHA256, O365, összevonási, aadconnect, az AD FS, az ad fs, a módosítás sha összevonási megbízhatósági kapcsolat, a megbízható függő entitás"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: samueld
editor: 
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: anandy
ms.openlocfilehash: 3333d1384aff8bdf6b3bcc894f8c633fd9ccc3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a><span data-ttu-id="34d24-104">Aláírás-kivonatoló algoritmus az Office 365 megbízható függő entitás módosítása</span><span class="sxs-lookup"><span data-stu-id="34d24-104">Change signature hash algorithm for Office 365 relying party trust</span></span>
## <a name="overview"></a><span data-ttu-id="34d24-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="34d24-105">Overview</span></span>
<span data-ttu-id="34d24-106">Active Directory összevonási szolgáltatások (AD FS) a jogkivonatok tooMicrosoft Azure Active Directory tooensure, hogy azok ne lehessen illetéktelenül jelentkezik.</span><span class="sxs-lookup"><span data-stu-id="34d24-106">Active Directory Federation Services (AD FS) signs its tokens tooMicrosoft Azure Active Directory tooensure that they cannot be tampered with.</span></span> <span data-ttu-id="34d24-107">Az aláírás SHA1 vagy SHA-256-alapú lehet.</span><span class="sxs-lookup"><span data-stu-id="34d24-107">This signature can be based on SHA1 or SHA256.</span></span> <span data-ttu-id="34d24-108">Az Azure Active Directory mostantól támogatja az SHA-256 algoritmussal aláírt jogkivonatokat, és azt javasoljuk, hello a hello legmagasabb szintű biztonság meg jogkivonat-aláíró algoritmus a tooSHA256 beállítása.</span><span class="sxs-lookup"><span data-stu-id="34d24-108">Azure Active Directory now supports tokens signed with an SHA256 algorithm, and we recommend setting hello token-signing algorithm tooSHA256 for hello highest level of security.</span></span> <span data-ttu-id="34d24-109">Ez a cikk szükséges tooset hello jogkivonat-aláíró algoritmus toohello biztonságosabb SHA-256 szintet hello lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="34d24-109">This article describes hello steps needed tooset hello token-signing algorithm toohello more secure SHA256 level.</span></span>

>[!NOTE]
><span data-ttu-id="34d24-110">A Microsoft azt javasolja, hogy SHA-256 használatát hello algoritmusként biztonságosabb, mint az SHA1, de SHA1 még mindig egy támogatott beállítás a jogkivonatok aláírásához.</span><span class="sxs-lookup"><span data-stu-id="34d24-110">Microsoft recommends usage of SHA256 as hello algorithm for signing tokens as it is more secure than SHA1 but SHA1 still remains a supported option.</span></span>

## <a name="change-hello-token-signing-algorithm"></a><span data-ttu-id="34d24-111">Hello jogkivonat-aláíró algoritmus módosítása</span><span class="sxs-lookup"><span data-stu-id="34d24-111">Change hello token-signing algorithm</span></span>
<span data-ttu-id="34d24-112">Hello aláírási algoritmus valamelyik az alábbi két folyamatok hello beállítása után az AD FS aláírja hello jogkivonatok az Office 365 megbízható függő entitás SHA-256.</span><span class="sxs-lookup"><span data-stu-id="34d24-112">After you have set hello signature algorithm with one of hello two processes below, AD FS signs hello tokens for Office 365 relying party trust with SHA256.</span></span> <span data-ttu-id="34d24-113">Nem kell toomake a további konfigurációs változásokat, és ez a változás nincs hatással a képes tooaccess Office 365 vagy más Azure AD-alkalmazások rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="34d24-113">You don't need toomake any extra configuration changes, and this change has no impact on your ability tooaccess Office 365 or other Azure AD applications.</span></span>

### <a name="ad-fs-management-console"></a><span data-ttu-id="34d24-114">AD FS felügyeleti konzol</span><span class="sxs-lookup"><span data-stu-id="34d24-114">AD FS management console</span></span>
1. <span data-ttu-id="34d24-115">Nyissa meg a hello AD FS-kezelőkonzolon hello elsődleges AD FS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="34d24-115">Open hello AD FS management console on hello primary AD FS server.</span></span>
2. <span data-ttu-id="34d24-116">Hello AD FS csomópontot, és kattintson a **függő entitás Megbízhatóságai**.</span><span class="sxs-lookup"><span data-stu-id="34d24-116">Expand hello AD FS node and click **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="34d24-117">Kattintson a jobb gombbal az Office 365 vagy az Azure függő entitás megbízhatóságához, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="34d24-117">Right-click your Office 365/Azure relying party trust and select **Properties**.</span></span>
4. <span data-ttu-id="34d24-118">Jelölje be hello **speciális** lapra, és jelölje be hello biztonságos kivonatoló algoritmus SHA-256.</span><span class="sxs-lookup"><span data-stu-id="34d24-118">Select hello **Advanced** tab and select hello secure hash algorithm SHA256.</span></span>
5. <span data-ttu-id="34d24-119">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="34d24-119">Click **OK**.</span></span>

![SHA-256 aláírási algoritmus--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a><span data-ttu-id="34d24-121">AD FS PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="34d24-121">AD FS PowerShell cmdlets</span></span>
1. <span data-ttu-id="34d24-122">Egyetlen AD FS-kiszolgálón nyissa meg a Powershellt a rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="34d24-122">On any AD FS server, open PowerShell under administrator privileges.</span></span>
2. <span data-ttu-id="34d24-123">Set hello biztonságos kivonatoló algoritmus hello segítségével **Set-AdfsRelyingPartyTrust** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="34d24-123">Set hello secure hash algorithm by using hello **Set-AdfsRelyingPartyTrust** cmdlet.</span></span>
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a><span data-ttu-id="34d24-124">Is</span><span class="sxs-lookup"><span data-stu-id="34d24-124">Also read</span></span>
* [<span data-ttu-id="34d24-125">Az Azure AD Connect Office 365-megbízhatóság javítása</span><span class="sxs-lookup"><span data-stu-id="34d24-125">Repair Office 365 trust with Azure AD Connect</span></span>](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

