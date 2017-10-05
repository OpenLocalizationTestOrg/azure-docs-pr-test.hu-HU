---
title: "Az Azure AD Connect: Szolgáltatáspéldány szinkronizálása |} Microsoft Docs"
description: "Ezen a lapon dokumentumokat az Azure AD-példányban szempontjai."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a><span data-ttu-id="da95d-103">Az Azure AD Connect: Különleges szempontok példányok</span><span class="sxs-lookup"><span data-stu-id="da95d-103">Azure AD Connect: Special considerations for instances</span></span>
<span data-ttu-id="da95d-104">Az Azure AD Connect leggyakrabban használt világszerte példányát az Azure AD és az Office 365.</span><span class="sxs-lookup"><span data-stu-id="da95d-104">Azure AD Connect is most commonly used with the world-wide instance of Azure AD and Office 365.</span></span> <span data-ttu-id="da95d-105">Emellett vannak más esetekben és ezeket az URL-címek és más szempontot különböző követelményekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="da95d-105">But there are also other instances and these have different requirements for URLs and other special considerations.</span></span>

## <a name="microsoft-cloud-germany"></a><span data-ttu-id="da95d-106">Microsoft Cloud Germany</span><span class="sxs-lookup"><span data-stu-id="da95d-106">Microsoft Cloud Germany</span></span>
<span data-ttu-id="da95d-107">A [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) egy német adatok adatkezelő által működtetett szuverén felhőalapú.</span><span class="sxs-lookup"><span data-stu-id="da95d-107">The [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) is a sovereign cloud operated by a German data trustee.</span></span>

| <span data-ttu-id="da95d-108">Nyissa meg a proxykiszolgáló URL-címek</span><span class="sxs-lookup"><span data-stu-id="da95d-108">URLs to open in proxy server</span></span> |
| --- |
| <span data-ttu-id="da95d-109">\*. microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="da95d-109">\*.microsoftonline.de</span></span> |
| <span data-ttu-id="da95d-110">\*.windows.net</span><span class="sxs-lookup"><span data-stu-id="da95d-110">\*.windows.net</span></span> |
| <span data-ttu-id="da95d-111">+ Tanúsítvány-visszavonási listákat</span><span class="sxs-lookup"><span data-stu-id="da95d-111">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="da95d-112">Amikor bejelentkezik az Azure AD-bérlő, a onmicrosoft.de tartományban olyan fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="da95d-112">When you sign in to your Azure AD tenant, you must use an account in the onmicrosoft.de domain.</span></span>

<span data-ttu-id="da95d-113">Jelenleg nem szerepel a Microsoft Cloud németországi funkciói:</span><span class="sxs-lookup"><span data-stu-id="da95d-113">Features currently not present in the Microsoft Cloud Germany:</span></span>

* <span data-ttu-id="da95d-114">**Az Azure AD Connect Health** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="da95d-114">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="da95d-115">**Az automatikus frissítések** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="da95d-115">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="da95d-116">**A jelszóvisszaírás** érhető el az előzetes 1.1.570.0 az Azure AD Connect verziójával és után.</span><span class="sxs-lookup"><span data-stu-id="da95d-116">**Password writeback** is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="da95d-117">Más Azure AD Premium-szolgáltatások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="da95d-117">Other Azure AD Premium services are not available.</span></span>

## <a name="microsoft-azure-government-cloud"></a><span data-ttu-id="da95d-118">A Microsoft Azure Government felhő</span><span class="sxs-lookup"><span data-stu-id="da95d-118">Microsoft Azure Government cloud</span></span>
<span data-ttu-id="da95d-119">A [a Microsoft Azure Government felhő](https://azure.microsoft.com/features/gov/) az Amerikai Egyesült Államok kormánya felhőalapú.</span><span class="sxs-lookup"><span data-stu-id="da95d-119">The [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is a cloud for US government.</span></span>

<span data-ttu-id="da95d-120">A felhő által a DirSync korábbi verziókban támogatott.</span><span class="sxs-lookup"><span data-stu-id="da95d-120">This cloud has been supported by earlier releases of DirSync.</span></span> <span data-ttu-id="da95d-121">A build 1.1.180 az Azure AD Connect a felhő következő generációja esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="da95d-121">From build 1.1.180 of Azure AD Connect, the next generation of the cloud is supported.</span></span> <span data-ttu-id="da95d-122">Ebben a generációban csak USA alapján végpontok használja, és nyissa meg a proxykiszolgáló URL-címek különböző listája.</span><span class="sxs-lookup"><span data-stu-id="da95d-122">This generation is using US-only based endpoints and have a different list of URLs to open in your proxy server.</span></span>

| <span data-ttu-id="da95d-123">Nyissa meg a proxykiszolgáló URL-címek</span><span class="sxs-lookup"><span data-stu-id="da95d-123">URLs to open in proxy server</span></span> |
| --- |
| <span data-ttu-id="da95d-124">\*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="da95d-124">\*.microsoftonline.com</span></span> |
| <span data-ttu-id="da95d-125">\*. microsoftonline.us</span><span class="sxs-lookup"><span data-stu-id="da95d-125">\*.microsoftonline.us</span></span> |
| <span data-ttu-id="da95d-126">\*. gov.us.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="da95d-126">\*.gov.us.microsoftonline.com</span></span> |
| <span data-ttu-id="da95d-127">+ Tanúsítvány-visszavonási listákat</span><span class="sxs-lookup"><span data-stu-id="da95d-127">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="da95d-128">Az Azure AD Connect nincs automatikusan észleli, hogy az Azure AD-bérlő a kormányzati felhőben található.</span><span class="sxs-lookup"><span data-stu-id="da95d-128">Azure AD Connect is not able to automatically detect that your Azure AD tenant is located in the Government cloud.</span></span> <span data-ttu-id="da95d-129">Ehelyett szükség az Azure AD Connect telepítése során, hajtsa végre a következő műveleteket.</span><span class="sxs-lookup"><span data-stu-id="da95d-129">Instead you need to take the following actions when you install Azure AD Connect.</span></span>

1. <span data-ttu-id="da95d-130">Az Azure AD Connect telepítés elindításához.</span><span class="sxs-lookup"><span data-stu-id="da95d-130">Start the Azure AD Connect installation.</span></span>
2. <span data-ttu-id="da95d-131">Amikor megjelenik az első lap, ahol vannak, a végfelhasználói licencszerződés elfogadásához kellene, nem folytatható, de hagyja meg a telepítési varázsló futtatása.</span><span class="sxs-lookup"><span data-stu-id="da95d-131">When you see the first page where you are supposed to accept the EULA, do not continue but leave the installation wizard running.</span></span>
3. <span data-ttu-id="da95d-132">Indítsa el a regedit és a beállításkulcs módosítása `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` értékre `2`.</span><span class="sxs-lookup"><span data-stu-id="da95d-132">Start regedit and change the registry key `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` to the value `2`.</span></span>
4. <span data-ttu-id="da95d-133">Lépjen vissza az Azure AD Connect telepítővarázsló fogadnia a végfelhasználói LICENCSZERZŐDÉST, és folytassa.</span><span class="sxs-lookup"><span data-stu-id="da95d-133">Go back to the Azure AD Connect installation wizard, accept the EULA, and continue.</span></span> <span data-ttu-id="da95d-134">A telepítés során ügyeljen arra, hogy használja a **egyéni konfigurációs** elérési útja (és nem Expressz telepítés).</span><span class="sxs-lookup"><span data-stu-id="da95d-134">During installation, make sure to use the **custom configuration** installation path (and not Express installation).</span></span> <span data-ttu-id="da95d-135">Ezután a megszokott módon folytathatja a telepítést.</span><span class="sxs-lookup"><span data-stu-id="da95d-135">Then continue the installation as usual.</span></span>

<span data-ttu-id="da95d-136">Jelenleg nem szerepel a Microsoft Azure Government felhőalapú szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="da95d-136">Features currently not present in the Microsoft Azure Government cloud:</span></span>

* <span data-ttu-id="da95d-137">**Az Azure AD Connect Health** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="da95d-137">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="da95d-138">**Az automatikus frissítések** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="da95d-138">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="da95d-139">**A jelszóvisszaírás** érhető el az előzetes 1.1.570.0 az Azure AD Connect verziójával és után.</span><span class="sxs-lookup"><span data-stu-id="da95d-139">**Password writeback**  is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="da95d-140">Más Azure AD Premium-szolgáltatások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="da95d-140">Other Azure AD Premium services are not available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da95d-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da95d-141">Next steps</span></span>
<span data-ttu-id="da95d-142">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="da95d-142">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
