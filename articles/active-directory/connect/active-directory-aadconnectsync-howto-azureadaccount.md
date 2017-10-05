---
title: "Azure AD Connect szinkronizálása: az Azure AD-szolgáltatásfiók kezelése |} Microsoft Docs"
description: "Ez a témakör az Azure AD-szolgáltatásfiók visszaállítása dokumentumokat."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, az Azure AD Connect szinkronizálási szolgáltatás Connector-szolgáltatásfióknak a jelszó alaphelyzetbe állítása"
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
ms.openlocfilehash: 8e9e8192ee4fcb636b5be91d2616acbc9120c8c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a><span data-ttu-id="a1095-104">Azure AD Connect szinkronizálása: kezelése az Azure AD-szolgáltatásfiók</span><span class="sxs-lookup"><span data-stu-id="a1095-104">Azure AD Connect sync: How to manage the Azure AD service account</span></span>
<span data-ttu-id="a1095-105">Az Azure AD-összekötő által használt szolgáltatásfiók kellene lennie ingyenes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a1095-105">The service account used by the Azure AD Connector is supposed to be service free.</span></span> <span data-ttu-id="a1095-106">Ha a hitelesítő adatok alaphelyzetbe kell, akkor ez a témakör értéke meg.</span><span class="sxs-lookup"><span data-stu-id="a1095-106">If you need to reset its credentials, then this topic is for you.</span></span> <span data-ttu-id="a1095-107">Például ha egy globális rendszergazda által hibát állítsa vissza a PowerShell használatával a szolgáltatásfiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="a1095-107">For example, if a Global Administrator has by mistake reset the password on the service account using PowerShell.</span></span>

## <a name="reset-the-credentials"></a><span data-ttu-id="a1095-108">A hitelesítő adatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="a1095-108">Reset the credentials</span></span>
<span data-ttu-id="a1095-109">Ha a fiók az Azure AD Connectoron definiálva az Azure AD hitelesítési problémák miatt nem tud kapcsolatba lépni, a jelszó állítható vissza.</span><span class="sxs-lookup"><span data-stu-id="a1095-109">If the service account defined on the Azure AD Connector cannot contact Azure AD due to authentication problems, the password can be reset.</span></span>

1. <span data-ttu-id="a1095-110">Jelentkezzen be az Azure AD Connect szinkronizálási kiszolgálót, majd indítsa el a Powershellt.</span><span class="sxs-lookup"><span data-stu-id="a1095-110">Sign in to the Azure AD Connect sync server and start PowerShell.</span></span>
2. <span data-ttu-id="a1095-111">Futtassa az `Add-ADSyncAADServiceAccount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a1095-111">Run `Add-ADSyncAADServiceAccount`.</span></span>  
   <span data-ttu-id="a1095-112">![PowerShell-parancsmag addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span><span class="sxs-lookup"><span data-stu-id="a1095-112">![PowerShell cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)</span></span>
3. <span data-ttu-id="a1095-113">Adja meg Azure AD globális rendszergazda hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a1095-113">Provide Azure AD Global admin credentials.</span></span>

<span data-ttu-id="a1095-114">Ez a parancsmag állítja vissza a szolgáltatásfiók jelszavát, és frissítse az Azure ad-ben és a szinkronizálási motor.</span><span class="sxs-lookup"><span data-stu-id="a1095-114">This cmdlet resets the password for the service account and update it both in Azure AD and in the sync engine.</span></span>

## <a name="known-issues-these-steps-can-solve"></a><span data-ttu-id="a1095-115">A lépések segítségével megoldható ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a1095-115">Known issues these steps can solve</span></span>
<span data-ttu-id="a1095-116">Ez a szakasz a hitelesítő adatok alaphelyzetbe állítása a következőn az Azure AD-szolgáltatásfiók által kijavított ügyfelei által jelentett hiba.</span><span class="sxs-lookup"><span data-stu-id="a1095-116">This section is a list of errors reported by customers that were fixed by a credentials reset on the Azure AD service account.</span></span>

- - -
<span data-ttu-id="a1095-117">Esemény 6900</span><span class="sxs-lookup"><span data-stu-id="a1095-117">Event 6900</span></span>  
<span data-ttu-id="a1095-118">A kiszolgáló váratlan hibába ütközött a jelszó-módosítási értesítés feldolgozása során:</span><span class="sxs-lookup"><span data-stu-id="a1095-118">The server encountered an unexpected error while processing a password change notification:</span></span>  
<span data-ttu-id="a1095-119">AADSTS70002: Hiba a érvényesítéséhez hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a1095-119">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="a1095-120">AADSTS50054: Régi jelszó-hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="a1095-120">AADSTS50054: Old password is used for authentication.</span></span>

- - -
<span data-ttu-id="a1095-121">Esemény 659</span><span class="sxs-lookup"><span data-stu-id="a1095-121">Event 659</span></span>  
<span data-ttu-id="a1095-122">Hiba történt a jelszó szinkronizálása konfigurálása beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a1095-122">Error while retrieving password policy sync configuration.</span></span> <span data-ttu-id="a1095-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span><span class="sxs-lookup"><span data-stu-id="a1095-123">Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:</span></span>  
<span data-ttu-id="a1095-124">AADSTS70002: Hiba a érvényesítéséhez hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a1095-124">AADSTS70002: Error validating credentials.</span></span> <span data-ttu-id="a1095-125">AADSTS50054: Régi jelszó-hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="a1095-125">AADSTS50054: Old password is used for authentication.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1095-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1095-126">Next steps</span></span>
<span data-ttu-id="a1095-127">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="a1095-127">**Overview topics**</span></span>

* [<span data-ttu-id="a1095-128">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="a1095-128">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="a1095-129">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a1095-129">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

