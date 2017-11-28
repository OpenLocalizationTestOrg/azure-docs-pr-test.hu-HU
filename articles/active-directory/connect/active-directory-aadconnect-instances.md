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
ms.openlocfilehash: 2a0d8a599cf84cd6530bdbb24951156510d2cf3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a><span data-ttu-id="ecd98-103">Az Azure AD Connect: Különleges szempontok példányok</span><span class="sxs-lookup"><span data-stu-id="ecd98-103">Azure AD Connect: Special considerations for instances</span></span>
<span data-ttu-id="ecd98-104">Az Azure AD Connect leggyakrabban használt hello világszerte példányát az Azure AD és az Office 365.</span><span class="sxs-lookup"><span data-stu-id="ecd98-104">Azure AD Connect is most commonly used with hello world-wide instance of Azure AD and Office 365.</span></span> <span data-ttu-id="ecd98-105">Emellett vannak más esetekben és ezeket az URL-címek és más szempontot különböző követelményekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ecd98-105">But there are also other instances and these have different requirements for URLs and other special considerations.</span></span>

## <a name="microsoft-cloud-germany"></a><span data-ttu-id="ecd98-106">Microsoft Cloud Germany</span><span class="sxs-lookup"><span data-stu-id="ecd98-106">Microsoft Cloud Germany</span></span>
<span data-ttu-id="ecd98-107">Hello [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) egy német adatok adatkezelő által működtetett szuverén felhőalapú.</span><span class="sxs-lookup"><span data-stu-id="ecd98-107">hello [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) is a sovereign cloud operated by a German data trustee.</span></span>

| <span data-ttu-id="ecd98-108">A proxykiszolgáló URL-címek tooopen</span><span class="sxs-lookup"><span data-stu-id="ecd98-108">URLs tooopen in proxy server</span></span> |
| --- |
| <span data-ttu-id="ecd98-109">\*. microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="ecd98-109">\*.microsoftonline.de</span></span> |
| <span data-ttu-id="ecd98-110">\*.windows.net</span><span class="sxs-lookup"><span data-stu-id="ecd98-110">\*.windows.net</span></span> |
| <span data-ttu-id="ecd98-111">+ Tanúsítvány-visszavonási listákat</span><span class="sxs-lookup"><span data-stu-id="ecd98-111">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="ecd98-112">Azure AD-bérlő tooyour bejelentkezéskor hello onmicrosoft.de tartományban olyan fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ecd98-112">When you sign in tooyour Azure AD tenant, you must use an account in hello onmicrosoft.de domain.</span></span>

<span data-ttu-id="ecd98-113">A szolgáltatások jelenleg nem található meg a Microsoft Cloud Németország hello:</span><span class="sxs-lookup"><span data-stu-id="ecd98-113">Features currently not present in hello Microsoft Cloud Germany:</span></span>

* <span data-ttu-id="ecd98-114">**Az Azure AD Connect Health** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ecd98-114">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="ecd98-115">**Az automatikus frissítések** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ecd98-115">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="ecd98-116">**A jelszóvisszaírás** érhető el az előzetes 1.1.570.0 az Azure AD Connect verziójával és után.</span><span class="sxs-lookup"><span data-stu-id="ecd98-116">**Password writeback** is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="ecd98-117">Más Azure AD Premium-szolgáltatások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ecd98-117">Other Azure AD Premium services are not available.</span></span>

## <a name="microsoft-azure-government-cloud"></a><span data-ttu-id="ecd98-118">A Microsoft Azure Government felhő</span><span class="sxs-lookup"><span data-stu-id="ecd98-118">Microsoft Azure Government cloud</span></span>
<span data-ttu-id="ecd98-119">Hello [a Microsoft Azure Government felhő](https://azure.microsoft.com/features/gov/) az Amerikai Egyesült Államok kormánya felhőalapú.</span><span class="sxs-lookup"><span data-stu-id="ecd98-119">hello [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is a cloud for US government.</span></span>

<span data-ttu-id="ecd98-120">A felhő által a DirSync korábbi verziókban támogatott.</span><span class="sxs-lookup"><span data-stu-id="ecd98-120">This cloud has been supported by earlier releases of DirSync.</span></span> <span data-ttu-id="ecd98-121">A build 1.1.180 az Azure AD Connect hello hello felhő következő generációja esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="ecd98-121">From build 1.1.180 of Azure AD Connect, hello next generation of hello cloud is supported.</span></span> <span data-ttu-id="ecd98-122">Ebben a generációban csak USA alapján végpontok használja, és rendelkezik a proxykiszolgáló URL-címek tooopen különböző listáját.</span><span class="sxs-lookup"><span data-stu-id="ecd98-122">This generation is using US-only based endpoints and have a different list of URLs tooopen in your proxy server.</span></span>

| <span data-ttu-id="ecd98-123">A proxykiszolgáló URL-címek tooopen</span><span class="sxs-lookup"><span data-stu-id="ecd98-123">URLs tooopen in proxy server</span></span> |
| --- |
| <span data-ttu-id="ecd98-124">\*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="ecd98-124">\*.microsoftonline.com</span></span> |
| <span data-ttu-id="ecd98-125">\*. microsoftonline.us</span><span class="sxs-lookup"><span data-stu-id="ecd98-125">\*.microsoftonline.us</span></span> |
| <span data-ttu-id="ecd98-126">\*. gov.us.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="ecd98-126">\*.gov.us.microsoftonline.com</span></span> |
| <span data-ttu-id="ecd98-127">+ Tanúsítvány-visszavonási listákat</span><span class="sxs-lookup"><span data-stu-id="ecd98-127">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="ecd98-128">Az Azure AD Connect nem tud tooautomatically észleli, hogy az Azure AD-bérlő hello kormányzati felhőben található.</span><span class="sxs-lookup"><span data-stu-id="ecd98-128">Azure AD Connect is not able tooautomatically detect that your Azure AD tenant is located in hello Government cloud.</span></span> <span data-ttu-id="ecd98-129">Ehelyett az Azure AD Connect telepítése során a következő műveletek tootake hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ecd98-129">Instead you need tootake hello following actions when you install Azure AD Connect.</span></span>

1. <span data-ttu-id="ecd98-130">Hello Azure AD Connect telepítés elindításához.</span><span class="sxs-lookup"><span data-stu-id="ecd98-130">Start hello Azure AD Connect installation.</span></span>
2. <span data-ttu-id="ecd98-131">Hello első oldal, ahol tooaccept hello EULA elvileg akkor jelenik meg, amikor nem folytatható, de hagyja hello telepítővarázslójának futtatása.</span><span class="sxs-lookup"><span data-stu-id="ecd98-131">When you see hello first page where you are supposed tooaccept hello EULA, do not continue but leave hello installation wizard running.</span></span>
3. <span data-ttu-id="ecd98-132">Indítsa el a regedit és hello beállításkulcs módosítása `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello érték `2`.</span><span class="sxs-lookup"><span data-stu-id="ecd98-132">Start regedit and change hello registry key `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello value `2`.</span></span>
4. <span data-ttu-id="ecd98-133">Lépjen vissza toohello az Azure AD Connect telepítővarázsló, fogadja el a végfelhasználói licencszerződés hello és továbbra is.</span><span class="sxs-lookup"><span data-stu-id="ecd98-133">Go back toohello Azure AD Connect installation wizard, accept hello EULA, and continue.</span></span> <span data-ttu-id="ecd98-134">A telepítés során győződjön meg arról, hogy toouse hello **egyéni konfigurációs** elérési útja (és nem Expressz telepítés).</span><span class="sxs-lookup"><span data-stu-id="ecd98-134">During installation, make sure toouse hello **custom configuration** installation path (and not Express installation).</span></span> <span data-ttu-id="ecd98-135">Ezt a szokásos módon hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="ecd98-135">Then continue hello installation as usual.</span></span>

<span data-ttu-id="ecd98-136">A szolgáltatások jelenleg nem található meg a Microsoft Azure Government felhő hello:</span><span class="sxs-lookup"><span data-stu-id="ecd98-136">Features currently not present in hello Microsoft Azure Government cloud:</span></span>

* <span data-ttu-id="ecd98-137">**Az Azure AD Connect Health** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ecd98-137">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="ecd98-138">**Az automatikus frissítések** nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ecd98-138">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="ecd98-139">**A jelszóvisszaírás** érhető el az előzetes 1.1.570.0 az Azure AD Connect verziójával és után.</span><span class="sxs-lookup"><span data-stu-id="ecd98-139">**Password writeback**  is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="ecd98-140">Más Azure AD Premium-szolgáltatások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ecd98-140">Other Azure AD Premium services are not available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecd98-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecd98-141">Next steps</span></span>
<span data-ttu-id="ecd98-142">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="ecd98-142">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
