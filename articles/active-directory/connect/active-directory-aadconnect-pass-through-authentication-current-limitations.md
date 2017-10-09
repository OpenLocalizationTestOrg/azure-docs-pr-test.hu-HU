---
title: "Az Azure AD Connect: Áteresztő hitelesítés – aktuális korlátozásai |} Microsoft Docs"
description: "Ez a cikk ismerteti a jelenlegi korlátozások hello Azure Active Directory (Azure AD) áteresztő hitelesítés."
services: active-directory
keywords: "Az Azure AD Connect áteresztő hitelesítés, telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 2933745d071aae205c44659e6ea92697f390effb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a><span data-ttu-id="d227e-104">Az Azure Active Directory átmenő hitelesítést: Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="d227e-104">Azure Active Directory Pass-through Authentication: Current limitations</span></span>

>[!IMPORTANT]
><span data-ttu-id="d227e-105">Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="d227e-105">Azure AD Pass-through Authentication is currently in preview.</span></span> <span data-ttu-id="d227e-106">Egy szabad szolgáltatást, és nem kell minden Azure AD toouse fizetős verziója azt.</span><span class="sxs-lookup"><span data-stu-id="d227e-106">It is a free feature, and you don't need any paid editions of Azure AD toouse it.</span></span> <span data-ttu-id="d227e-107">Áteresztő hitelesítés csak érhető el az Azure AD, és nem a hello világszerte példány [Microsoft Cloud Németország](http://www.microsoft.de/cloud-deutschland) és [Microsoft Azure Government felhő](https://azure.microsoft.com/features/gov/).</span><span class="sxs-lookup"><span data-stu-id="d227e-107">Pass-through Authentication is only available in hello world-wide instance of Azure AD, and not on [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) and [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="d227e-108">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="d227e-108">Supported scenarios</span></span>

<span data-ttu-id="d227e-109">a következő forgatókönyvek hello előzetes teljes mértékben támogatottak:</span><span class="sxs-lookup"><span data-stu-id="d227e-109">hello following scenarios are fully supported during preview:</span></span>

- <span data-ttu-id="d227e-110">Felhasználói bejelentkezések minden böngésző alapú webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d227e-110">User sign-ins into all web browser-based applications.</span></span>
- <span data-ttu-id="d227e-111">Felhasználói bejelentkezések be, amely támogatja az Office 365-ügyfélalkalmazások [modern hitelesítést](https://aka.ms/modernauthga).</span><span class="sxs-lookup"><span data-stu-id="d227e-111">User sign-ins into Office 365 client applications that support [modern authentication](https://aka.ms/modernauthga).</span></span>
- <span data-ttu-id="d227e-112">Az Azure AD Join Windows 10 rendszerű eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="d227e-112">Azure AD Join for Windows 10 devices.</span></span>
- <span data-ttu-id="d227e-113">Exchange ActiveSync-támogatását.</span><span class="sxs-lookup"><span data-stu-id="d227e-113">Exchange ActiveSync support.</span></span>

## <a name="unsupported-scenarios"></a><span data-ttu-id="d227e-114">Nem támogatott forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d227e-114">Unsupported scenarios</span></span>

<span data-ttu-id="d227e-115">hello következő forgatókönyvek a következők _nem_ előzetes támogatott:</span><span class="sxs-lookup"><span data-stu-id="d227e-115">hello following scenarios are _not_ supported during preview:</span></span>

- <span data-ttu-id="d227e-116">Felhasználói bejelentkezések alkalmazásokba örökölt Office ügyfél (az Office 2013 vagy korábbi).</span><span class="sxs-lookup"><span data-stu-id="d227e-116">User sign-ins into legacy Office client applications (Office 2013 or earlier).</span></span> <span data-ttu-id="d227e-117">Ha lehetséges szervezetek a következők javasolt tooswitch toomodern hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d227e-117">Organizations are encouraged tooswitch toomodern authentication, if possible.</span></span> <span data-ttu-id="d227e-118">A modern hitelesítés lehetővé teszi az áteresztő hitelesítés támogatásához azonban is segítséget nyújt a felhasználó mindig lássa fiókok [feltételes hozzáférés](../active-directory-conditional-access.md) funkciók, például a többtényezős hitelesítés (MFA).</span><span class="sxs-lookup"><span data-stu-id="d227e-118">Modern authentication allows for Pass-through Authentication support but also helps you secure your user accounts using [conditional access](../active-directory-conditional-access.md) features such as Multi-Factor Authentication (MFA).</span></span>
- <span data-ttu-id="d227e-119">Felhasználói bejelentkezések Skype az üzleti ügyfélalkalmazások, beleértve a Skype vállalati 2016 esetében.</span><span class="sxs-lookup"><span data-stu-id="d227e-119">User sign-ins into Skype for Business client applications, including Skype for Business 2016.</span></span>
- <span data-ttu-id="d227e-120">Felhasználói bejelentkezések a PowerShell 1.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="d227e-120">User sign-ins into PowerShell v1.0.</span></span> <span data-ttu-id="d227e-121">Javasoljuk, hogy inkább PowerShell 2.0-s verzió.</span><span class="sxs-lookup"><span data-stu-id="d227e-121">It is recommended that you use PowerShell v2.0 instead.</span></span>

>[!IMPORTANT]
><span data-ttu-id="d227e-122">Nem támogatott forgatókönyvek megoldás engedélyezése Jelszókivonat-szinkronizálást a hello [választható szolgáltatások](active-directory-aadconnect-get-started-custom.md#optional-features) hello Azure AD Connect varázsló lapja.</span><span class="sxs-lookup"><span data-stu-id="d227e-122">As a workaround for unsupported scenarios, enable Password Hash Synchronization on hello [Optional features](active-directory-aadconnect-get-started-custom.md#optional-features) page in hello Azure AD Connect wizard.</span></span> <span data-ttu-id="d227e-123">Jelszókivonat-szinkronizálást úgy működik, mint a fenti forgatókönyvek hello fallback _csak_ (és _nem_ általános tartalék hitelesítésként tooPass keresztül).</span><span class="sxs-lookup"><span data-stu-id="d227e-123">Password Hash Synchronization acts as a fallback for hello preceding scenarios _only_ (and _not_ as a generic fallback tooPass-through Authentication).</span></span> <span data-ttu-id="d227e-124">Ha ezek a forgatókönyvek nem szükséges, kapcsolja ki a Jelszókivonat-szinkronizálást.</span><span class="sxs-lookup"><span data-stu-id="d227e-124">If you don't need these scenarios, turn off Password Hash Synchronization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d227e-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d227e-125">Next steps</span></span>
- <span data-ttu-id="d227e-126">[**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d227e-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="d227e-127">[**Műszaki mélyreható** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) – Ez a funkció működésének megismerése.</span><span class="sxs-lookup"><span data-stu-id="d227e-127">[**Technical Deep Dive**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="d227e-128">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -kérdések toofrequently válaszol.</span><span class="sxs-lookup"><span data-stu-id="d227e-128">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="d227e-129">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.</span><span class="sxs-lookup"><span data-stu-id="d227e-129">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="d227e-130">[**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.</span><span class="sxs-lookup"><span data-stu-id="d227e-130">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="d227e-131">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="d227e-131">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
