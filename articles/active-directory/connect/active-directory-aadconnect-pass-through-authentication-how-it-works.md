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
ms.openlocfilehash: d34ccd40082edbe036d963ad548bff648119bdd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a><span data-ttu-id="0dd99-105">Az Azure Active Directory átmenő hitelesítést: Műszaki részletes bemutatója</span><span class="sxs-lookup"><span data-stu-id="0dd99-105">Azure Active Directory Pass-through Authentication: Technical deep dive</span></span>

>[!IMPORTANT]
><span data-ttu-id="0dd99-106">Az Azure AD áteresztő hitelesítés jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="0dd99-106">Azure AD Pass-through Authentication is currently in preview.</span></span> 

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a><span data-ttu-id="0dd99-107">Hogyan működik az Azure Active Directory átmenő hitelesítést?</span><span class="sxs-lookup"><span data-stu-id="0dd99-107">How does Azure Active Directory Pass-through Authentication work?</span></span>

<span data-ttu-id="0dd99-108">Amikor egy felhasználó próbál bejelentkezni az Azure Active Directory (Azure AD) által védett alkalmazáshoz, és az áteresztő hitelesítés engedélyezve van a bérlő, a következő lépések következnek:</span><span class="sxs-lookup"><span data-stu-id="0dd99-108">When a user attempts to sign into an application secured by Azure Active Directory (Azure AD), and if Pass-through Authentication is enabled on the tenant, the following steps occur:</span></span>

1. <span data-ttu-id="0dd99-109">A felhasználó megpróbál hozzáférni az alkalmazáshoz (például az Outlook Web App - https://outlook.office365.com/owa/).</span><span class="sxs-lookup"><span data-stu-id="0dd99-109">The user tries to access an application (for example, the Outlook Web App - https://outlook.office365.com/owa/).</span></span>
2. <span data-ttu-id="0dd99-110">Ha a felhasználó nem jelentkezett, a felhasználót a rendszer átirányítja az Azure AD bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="0dd99-110">If the user is not already signed in, the user is redirected to the Azure AD sign-in page.</span></span>
3. <span data-ttu-id="0dd99-111">A felhasználó felhasználónevét és jelszavát köt az Azure AD bejelentkezési oldal, és a "Bejelentkezés" gombra kattint.</span><span class="sxs-lookup"><span data-stu-id="0dd99-111">The user enters their username and password into the Azure AD sign-in page and clicks the "Sign in" button.</span></span>
4. <span data-ttu-id="0dd99-112">Az Azure Active Directory, a bejelentkezési kérelem fogadása helyez el a felhasználónevet és jelszót (a nyilvános kulccsal titkosított) várólista.</span><span class="sxs-lookup"><span data-stu-id="0dd99-112">Azure AD, on receiving the sign-in request, places the username and password (encrypted  using a public key) on a queue.</span></span>
5. <span data-ttu-id="0dd99-113">Egy helyszíni áteresztő hitelesítési ügynök a várólistára kimenő hívást, és lekéri a felhasználónév és a titkosított jelszót.</span><span class="sxs-lookup"><span data-stu-id="0dd99-113">An on-premises Pass-through Authentication agent makes an outbound call to the queue and retrieves the username and encrypted password.</span></span>
6. <span data-ttu-id="0dd99-114">Az ügynök visszafejti a jelszót a titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="0dd99-114">The agent decrypts the password using its private key.</span></span>
7. <span data-ttu-id="0dd99-115">Az ügynök szerint hitelesíti a felhasználónevet és jelszót normál Windows API-k (hasonló eljárást, amely Active Directory összevonási szolgáltatások által használttól) Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="0dd99-115">The agent then validates the username and password against Active Directory using standard Windows APIs (a similar mechanism to what is used by Active Directory Federation Services).</span></span> <span data-ttu-id="0dd99-116">A felhasználónév vagy a helyszíni alapértelmezett felhasználónév lehet (általában `userPrincipalName`) vagy az Azure AD Connect konfigurált egy másik attribútum (úgynevezett `Alternate ID`).</span><span class="sxs-lookup"><span data-stu-id="0dd99-116">The username can be either the on-premises default username (usually `userPrincipalName`) or another attribute configured in Azure AD Connect (known as `Alternate ID`).</span></span>
8. <span data-ttu-id="0dd99-117">A helyszíni Active Directory tartományvezérlőn (DC) ezután kiértékeli a kérelmet, és a megfelelő választ ad vissza (sikeres, sikertelen, a jelszó lejárt, vagy felhasználói zárolása) ügynökkel.</span><span class="sxs-lookup"><span data-stu-id="0dd99-117">The on-premises Active Directory Domain Controller (DC) then evaluates the request and returns the appropriate response (success, failure, password expired or user locked out) to the agent.</span></span>
9. <span data-ttu-id="0dd99-118">Az ügynök visszaadó ezt a választ az Azure AD vissza.</span><span class="sxs-lookup"><span data-stu-id="0dd99-118">The agent, in turn, returns this response back to Azure AD.</span></span>
10. <span data-ttu-id="0dd99-119">Az Azure AD a válasz kiértékeli, és válaszol-e a felhasználó megfelelő – például azt vagy a felhasználó jelentkezik be azonnal, vagy kérelmeket a multi-factor Authentication (MFA).</span><span class="sxs-lookup"><span data-stu-id="0dd99-119">Azure AD evaluates the response and responds to the user as appropriate - for example, it either signs the user in immediately or requests for Multi-Factor Authentication (MFA).</span></span>
11. <span data-ttu-id="0dd99-120">Ha a felhasználói bejelentkezés sikeres, a felhasználó nem tud hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="0dd99-120">If the user sign-in is successful, the user is able to access the application.</span></span>

<span data-ttu-id="0dd99-121">A következő ábra összetevőit és a lépéseit mutatja be.</span><span class="sxs-lookup"><span data-stu-id="0dd99-121">The following diagram illustrates all the components and the steps involved.</span></span>

![Áteresztő hitelesítés](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a><span data-ttu-id="0dd99-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0dd99-123">Next steps</span></span>
- <span data-ttu-id="0dd99-124">[**Aktuális korlátozások** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – Ez a funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="0dd99-124">[**Current limitations**](active-directory-aadconnect-pass-through-authentication-current-limitations.md) - This feature is currently in preview.</span></span> <span data-ttu-id="0dd99-125">Ismerje meg, melyik forgatókönyvek is támogatottak, és melyek nem.</span><span class="sxs-lookup"><span data-stu-id="0dd99-125">Learn which scenarios are supported and which ones are not.</span></span>
- <span data-ttu-id="0dd99-126">[**Gyors üzembe helyezési** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - létrehozásához, és fut az Azure AD áteresztő hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="0dd99-126">[**Quick Start**](active-directory-aadconnect-pass-through-authentication-quick-start.md) - Get up and running Azure AD Pass-through Authentication.</span></span>
- <span data-ttu-id="0dd99-127">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-pass-through-authentication-faq.md) -gyakran feltett kérdésekre adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="0dd99-127">[**Frequently Asked Questions**](active-directory-aadconnect-pass-through-authentication-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="0dd99-128">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -Útmutató: a szolgáltatással kapcsolatos gyakori problémák megoldása.</span><span class="sxs-lookup"><span data-stu-id="0dd99-128">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="0dd99-129">[**Az Azure AD zökkenőmentes SSO** ](active-directory-aadconnect-sso.md) -további információ a kiegészítő funkció.</span><span class="sxs-lookup"><span data-stu-id="0dd99-129">[**Azure AD Seamless SSO**](active-directory-aadconnect-sso.md) - Learn more about this complementary feature.</span></span>
- <span data-ttu-id="0dd99-130">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="0dd99-130">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
