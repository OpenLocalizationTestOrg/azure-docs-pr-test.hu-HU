---
title: "Azure AD Connect szinkronizálása: hogyan toomanage hello Azure AD szolgáltatás fiókja |} Microsoft Docs"
description: "Ez a témakör dokumentumok hogyan toorestore hello Azure AD szolgáltatás fiókja."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, hogyan tooreset hello jelszavát hello Azure AD Connect szinkronizálási szolgáltatás összekötő szolgáltatás fiókja"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e563518eae173de42a1d40bb5a76e63f29f9da42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomanage-hello-azure-ad-service-account"></a><span data-ttu-id="019a6-104">Azure AD Connect szinkronizálása: hogyan toomanage hello Azure AD szolgáltatás fiókja</span><span class="sxs-lookup"><span data-stu-id="019a6-104">Azure AD Connect sync: How toomanage hello Azure AD service account</span></span>
<span data-ttu-id="019a6-105">hello Azure AD-összekötő által használt szolgáltatásfiók hello toobe szolgáltatás szabad kellene.</span><span class="sxs-lookup"><span data-stu-id="019a6-105">hello service account used by hello Azure AD Connector is supposed toobe service free.</span></span> <span data-ttu-id="019a6-106">Ha tooreset a hitelesítő adatok szükségesek, akkor ez a témakör értéke meg.</span><span class="sxs-lookup"><span data-stu-id="019a6-106">If you need tooreset its credentials, then this topic is for you.</span></span> <span data-ttu-id="019a6-107">Ha például egy globális rendszergazda által hibát hello jelszó alaphelyzetbe állítása a PowerShell használatával hello szolgáltatásfiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="019a6-107">For example, if a Global Administrator has by mistake reset hello password on hello service account using PowerShell.</span></span>

## <a name="reset-hello-credentials"></a><span data-ttu-id="019a6-108">Hello hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="019a6-108">Reset hello credentials</span></span>
<span data-ttu-id="019a6-109">Ha hello szolgáltatásfiók definiált hello Azure AD-összekötő nem tud kapcsolatba lépni az Azure AD tooauthentication problémák miatt, hello vissza tudja állítani.</span><span class="sxs-lookup"><span data-stu-id="019a6-109">If hello service account defined on hello Azure AD Connector cannot contact Azure AD due tooauthentication problems, hello password can be reset.</span></span>

1. <span data-ttu-id="019a6-110">Jelentkezzen be toohello az Azure AD Connect szinkronizálási kiszolgálót, és indítsa el a Powershellt.</span><span class="sxs-lookup"><span data-stu-id="019a6-110">Sign in toohello Azure AD Connect sync server and start PowerShell.</span></span>
2. <span data-ttu-id="019a6-111">Futtassa az `Add-ADSyncAADServiceAccount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="019a6-111">Run `Add-ADSyncAADServiceAccount`.</span></span>  
   <span data-ttu-id="019a6-112">![PowerShell-parancsmag addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span><span class="sxs-lookup"><span data-stu-id="019a6-112">![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span></span>
3. <span data-ttu-id="019a6-113">Adja meg Azure AD globális rendszergazda hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="019a6-113">Provide Azure AD Global admin credentials.</span></span>

<span data-ttu-id="019a6-114">Ez a parancsmag hello szolgáltatásfiók hello jelszavának alaphelyzetbe állítása és az Azure ad-ben és a szinkronizálási motor hello is frissítse.</span><span class="sxs-lookup"><span data-stu-id="019a6-114">This cmdlet resets hello password for hello service account and update it both in Azure AD and in hello sync engine.</span></span>

## <a name="known-issues-these-steps-can-solve"></a><span data-ttu-id="019a6-115">A lépések segítségével megoldható ismert problémák</span><span class="sxs-lookup"><span data-stu-id="019a6-115">Known issues these steps can solve</span></span>
<span data-ttu-id="019a6-116">Ez a szakasz a hitelesítő adatok alaphelyzetbe állítása a következőn hello Azure AD-szolgáltatásfiók által kijavított ügyfelei által jelentett hiba.</span><span class="sxs-lookup"><span data-stu-id="019a6-116">This section is a list of errors reported by customers that were fixed by a credentials reset on hello Azure AD service account.</span></span>

- - -
<span data-ttu-id="019a6-117">Esemény 6900</span><span class="sxs-lookup"><span data-stu-id="019a6-117">Event 6900</span></span>  
<span data-ttu-id="019a6-118">hello kiszolgáló váratlan hibába ütközött a jelszó-módosítási értesítés feldolgozása során:</span><span class="sxs-lookup"><span data-stu-id="019a6-118">hello server encountered an unexpected error while processing a password change notification:</span></span>  
<span data-ttu-id="019a6-119">AADSTS70002: Hiba a érvényesítéséhez hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="019a6-119">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="019a6-120">AADSTS50054: Régi jelszó-hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="019a6-120">AADSTS50054: Old password is used for authentication.</span></span>

- - -
<span data-ttu-id="019a6-121">Esemény 659</span><span class="sxs-lookup"><span data-stu-id="019a6-121">Event 659</span></span>  
<span data-ttu-id="019a6-122">Hiba történt a jelszó szinkronizálása konfigurálása beolvasása.</span><span class="sxs-lookup"><span data-stu-id="019a6-122">Error while retrieving password policy sync configuration.</span></span> <span data-ttu-id="019a6-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span><span class="sxs-lookup"><span data-stu-id="019a6-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span></span>  
<span data-ttu-id="019a6-124">AADSTS70002: Hiba a érvényesítéséhez hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="019a6-124">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="019a6-125">AADSTS50054: Régi jelszó-hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="019a6-125">AADSTS50054: Old password is used for authentication.</span></span>

## <a name="next-steps"></a><span data-ttu-id="019a6-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="019a6-126">Next steps</span></span>
<span data-ttu-id="019a6-127">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="019a6-127">**Overview topics**</span></span>

* [<span data-ttu-id="019a6-128">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="019a6-128">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="019a6-129">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="019a6-129">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

