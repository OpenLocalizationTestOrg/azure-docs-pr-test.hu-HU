---
title: "Office 365 függő entitás megbízhatóságához módosítás aláírás-kivonatoló algoritmus |} Microsoft Docs"
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
ms.openlocfilehash: c581b1468630a9f28204592c936360b72f42f0d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a><span data-ttu-id="7bc6f-104">Aláírás-kivonatoló algoritmus az Office 365 megbízható függő entitás módosítása</span><span class="sxs-lookup"><span data-stu-id="7bc6f-104">Change signature hash algorithm for Office 365 relying party trust</span></span>
## <a name="overview"></a><span data-ttu-id="7bc6f-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7bc6f-105">Overview</span></span>
<span data-ttu-id="7bc6f-106">Active Directory összevonási szolgáltatások (AD FS) a Microsoft Azure Active Directoryban annak érdekében, hogy azok ne lehessen illetéktelenül jogkivonatai jelentkezik.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-106">Active Directory Federation Services (AD FS) signs its tokens to Microsoft Azure Active Directory to ensure that they cannot be tampered with.</span></span> <span data-ttu-id="7bc6f-107">Az aláírás SHA1 vagy SHA-256-alapú lehet.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-107">This signature can be based on SHA1 or SHA256.</span></span> <span data-ttu-id="7bc6f-108">Az Azure Active Directory mostantól támogatja az SHA-256 algoritmussal aláírt jogkivonatokat, és javasolt SHA256 a legmagasabb szintű biztonság a jogkivonat-aláíró algoritmus beállítást.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-108">Azure Active Directory now supports tokens signed with an SHA256 algorithm, and we recommend setting the token-signing algorithm to SHA256 for the highest level of security.</span></span> <span data-ttu-id="7bc6f-109">Ez a cikk ismerteti a jogkivonat-aláíró algoritmus szint a biztonságosabb SHA-256 beállításához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-109">This article describes the steps needed to set the token-signing algorithm to the more secure SHA256 level.</span></span>

>[!NOTE]
><span data-ttu-id="7bc6f-110">A Microsoft azt javasolja, hogy SHA-256 használatát a algoritmusként biztonságosabb, mint az SHA1, de SHA1 még mindig egy támogatott beállítás a jogkivonatok aláírásához.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-110">Microsoft recommends usage of SHA256 as the algorithm for signing tokens as it is more secure than SHA1 but SHA1 still remains a supported option.</span></span>

## <a name="change-the-token-signing-algorithm"></a><span data-ttu-id="7bc6f-111">A jogkivonat-aláíró algoritmus módosítása</span><span class="sxs-lookup"><span data-stu-id="7bc6f-111">Change the token-signing algorithm</span></span>
<span data-ttu-id="7bc6f-112">Miután beállította az aláírási algoritmus az alábbi két folyamatok egyike, az AD FS aláírja a jogkivonatok az Office 365 megbízható függő entitás SHA-256.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-112">After you have set the signature algorithm with one of the two processes below, AD FS signs the tokens for Office 365 relying party trust with SHA256.</span></span> <span data-ttu-id="7bc6f-113">Nem kell további konfigurációs módosításokat, és ez a változás nincs hatással van a elérését Office 365- vagy más Azure AD-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-113">You don't need to make any extra configuration changes, and this change has no impact on your ability to access Office 365 or other Azure AD applications.</span></span>

### <a name="ad-fs-management-console"></a><span data-ttu-id="7bc6f-114">AD FS felügyeleti konzol</span><span class="sxs-lookup"><span data-stu-id="7bc6f-114">AD FS management console</span></span>
1. <span data-ttu-id="7bc6f-115">Nyissa meg az elsődleges AD FS-kiszolgáló az AD FS felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-115">Open the AD FS management console on the primary AD FS server.</span></span>
2. <span data-ttu-id="7bc6f-116">Bontsa ki az AD FS csomópontot, és kattintson a **függő entitás Megbízhatóságai**.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-116">Expand the AD FS node and click **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="7bc6f-117">Kattintson a jobb gombbal az Office 365 vagy az Azure függő entitás megbízhatóságához, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-117">Right-click your Office 365/Azure relying party trust and select **Properties**.</span></span>
4. <span data-ttu-id="7bc6f-118">Válassza ki a **speciális** fülre, és válassza ki a biztonságos kivonatoló algoritmus SHA-256.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-118">Select the **Advanced** tab and select the secure hash algorithm SHA256.</span></span>
5. <span data-ttu-id="7bc6f-119">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-119">Click **OK**.</span></span>

![SHA-256 aláírási algoritmus--MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a><span data-ttu-id="7bc6f-121">AD FS PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="7bc6f-121">AD FS PowerShell cmdlets</span></span>
1. <span data-ttu-id="7bc6f-122">Egyetlen AD FS-kiszolgálón nyissa meg a Powershellt a rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-122">On any AD FS server, open PowerShell under administrator privileges.</span></span>
2. <span data-ttu-id="7bc6f-123">Állítsa be a biztonságos kivonatoló algoritmus használatával a **Set-AdfsRelyingPartyTrust** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7bc6f-123">Set the secure hash algorithm by using the **Set-AdfsRelyingPartyTrust** cmdlet.</span></span>
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a><span data-ttu-id="7bc6f-124">Is</span><span class="sxs-lookup"><span data-stu-id="7bc6f-124">Also read</span></span>
* [<span data-ttu-id="7bc6f-125">Az Azure AD Connect Office 365-megbízhatóság javítása</span><span class="sxs-lookup"><span data-stu-id="7bc6f-125">Repair Office 365 trust with Azure AD Connect</span></span>](connect/active-directory-aadconnect-federation-management.md#repairthetrust)

