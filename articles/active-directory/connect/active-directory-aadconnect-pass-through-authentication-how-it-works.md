---
title: "Az Azure AD Connect: Áteresztő hitelesítés – hogyan működik? | Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Active Directory áteresztő hitelesítés működéséről."
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
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: ffcebee572a9ba2840e81250651dea45599d65d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a><span data-ttu-id="c26f4-105">Az Azure Active Directory átmenő hitelesítést: Műszaki részletes bemutatója</span><span class="sxs-lookup"><span data-stu-id="c26f4-105">Azure Active Directory Pass-through Authentication: Technical deep dive</span></span>

>[!IMPORTANT]
><span data-ttu-id="c26f4-106">Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="c26f4-106">Azure AD Pass-through Authentication is currently in preview.</span></span> 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a><span data-ttu-id="c26f4-107">Hogyan működik az Azure Active Directory átmenő hitelesítést?</span><span class="sxs-lookup"><span data-stu-id="c26f4-107">How does Azure Active Directory Pass-through Authentication work?</span></span>

<span data-ttu-id="c26f4-108">Következő lépések fordulhat elő, amikor a felhasználó toosign megpróbál az Azure Active Directory (Azure AD) által védett alkalmazáshoz, és ha átmenő hitelesítés engedélyezve van a hello bérlői hello:</span><span class="sxs-lookup"><span data-stu-id="c26f4-108">When a user attempts toosign into an application secured by Azure Active Directory (Azure AD), and if Pass-through Authentication is enabled on hello tenant, hello following steps occur:</span></span>

1. <span data-ttu-id="c26f4-109">hello felhasználó megpróbál tooaccess egy alkalmazás (például az Outlook Web App - hello https://outlook.office365.com/owa/).</span><span class="sxs-lookup"><span data-stu-id="c26f4-109">hello user tries tooaccess an application (for example, hello Outlook Web App - https://outlook.office365.com/owa/).</span></span>
2. <span data-ttu-id="c26f4-110">Hello felhasználó már nem jelentkezett be, ha a hello felhasználó átirányított toohello az Azure AD bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="c26f4-110">If hello user is not already signed in, hello user is redirected toohello Azure AD sign-in page.</span></span>
3. <span data-ttu-id="c26f4-111">hello felhasználó felhasználónevét és jelszavát köt hello Azure AD bejelentkezési oldalára, és hello "Bejelentkezés" gombra kattint.</span><span class="sxs-lookup"><span data-stu-id="c26f4-111">hello user enters their username and password into hello Azure AD sign-in page and clicks hello "Sign in" button.</span></span>
4. <span data-ttu-id="c26f4-112">Az Azure Active Directory, a hello bejelentkezési kérelem fogadása hello felhasználónevét és jelszavát (nyilvános kulccsal titkosított) helyez egy üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="c26f4-112">Azure AD, on receiving hello sign-in request, places hello username and password (encrypted  using a public key) on a queue.</span></span>
5. <span data-ttu-id="c26f4-113">A helyszíni áteresztő hitelesítés ügynök lehetővé teszi egy kimenő hívás toohello várólistát, és beolvassa a hello felhasználónévvel és jelszóval titkosított.</span><span class="sxs-lookup"><span data-stu-id="c26f4-113">An on-premises Pass-through Authentication agent makes an outbound call toohello queue and retrieves hello username and encrypted password.</span></span>
6. <span data-ttu-id="c26f4-114">hello ügynök visszafejti hello jelszót a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="c26f4-114">hello agent decrypts hello password using its private key.</span></span>
7. <span data-ttu-id="c26f4-115">hello ügynök szerint hitelesíti hello felhasználónév és jelszó használatával normál Windows API-k (egy hasonló mechanizmus toowhat szolgál az Active Directory összevonási szolgáltatások) Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="c26f4-115">hello agent then validates hello username and password against Active Directory using standard Windows APIs (a similar mechanism toowhat is used by Active Directory Federation Services).</span></span> <span data-ttu-id="c26f4-116">hello felhasználónév lehet vagy hello helyszíni alapértelmezett felhasználónév (általában `userPrincipalName`) vagy az Azure AD Connect konfigurált egy másik attribútum (úgynevezett `Alternate ID`).</span><span class="sxs-lookup"><span data-stu-id="c26f4-116">hello username can be either hello on-premises default username (usually `userPrincipalName`) or another attribute configured in Azure AD Connect (known as `Alternate ID`).</span></span>
8. <span data-ttu-id="c26f4-117">hello a helyszíni Active Directory tartományvezérlőn (DC), majd hello kérelem kiértékeli, és értéket ad vissza megfelelő választ hello (sikeres, sikertelen, a jelszó lejárt, vagy felhasználói zárolása) toohello ügynök.</span><span class="sxs-lookup"><span data-stu-id="c26f4-117">hello on-premises Active Directory Domain Controller (DC) then evaluates hello request and returns hello appropriate response (success, failure, password expired or user locked out) toohello agent.</span></span>
9. <span data-ttu-id="c26f4-118">hello ügynök, pedig ez választ AD vissza tooAzure adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c26f4-118">hello agent, in turn, returns this response back tooAzure AD.</span></span>
10. <span data-ttu-id="c26f4-119">Az Azure AD hello választ ad meg, és szükség szerint toohello felhasználói válaszol – például azt vagy hello felhasználó jelentkezik be azonnal, vagy kérelmeket a multi-factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="c26f4-119">Azure AD evaluates hello response and responds toohello user as appropriate - for example, it either signs hello user in immediately or requests for Multi-Factor Authentication (MFA).</span></span>
11. <span data-ttu-id="c26f4-120">Hello felhasználói bejelentkezés sikeres, hello felhasználói akkor tudja tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c26f4-120">If hello user sign-in is successful, hello user is able tooaccess hello application.</span></span>

<span data-ttu-id="c26f4-121">hello alábbi ábrán látható összes hello összetevők és hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="c26f4-121">hello following diagram illustrates all hello components and hello steps involved.</span></span>

![Átmenő hitelesítés](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a><span data-ttu-id="c26f4-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c26f4-123">Next steps</span></span>
- <span data-ttu-id="c26f4-124">[**Aktuális korlátozások** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – Ez a funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="c26f4-124">[**Current limitations**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - This feature is currently in preview.</span></span> <span data-ttu-id="c26f4-125">Ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.</span><span class="sxs-lookup"><span data-stu-id="c26f4-125">Learn which scenarios are supported and which ones are not.</span></span>
- <span data-ttu-id="c26f4-126">[**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="c26f4-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="c26f4-127">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -kérdések toofrequently válaszol.</span><span class="sxs-lookup"><span data-stu-id="c26f4-127">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="c26f4-128">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.</span><span class="sxs-lookup"><span data-stu-id="c26f4-128">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="c26f4-129">[**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.</span><span class="sxs-lookup"><span data-stu-id="c26f4-129">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="c26f4-130">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="c26f4-130">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
