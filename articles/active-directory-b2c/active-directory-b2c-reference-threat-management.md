---
title: "Az Azure Active Directory B2C: A felügyeleti fenyegetés |} Microsoft Docs"
description: "További információk a észlelés és javaslat technikák-szolgáltatásmegtagadási támadások és az Azure Active Directory B2C jelszó támadásokat."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 9472cb01eb713e297053727b1a314293574bb657
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a><span data-ttu-id="23694-103">Az Azure Active Directory B2C: A fenyegetés kezelése</span><span class="sxs-lookup"><span data-stu-id="23694-103">Azure Active Directory B2C: Threat management</span></span>

<span data-ttu-id="23694-104">Fenyegetés felügyeleti a rendszer és a hálózatok elleni támadások elleni védelem megtervezését foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="23694-104">Threat management includes planning for protection from attacks against your system and networks.</span></span> <span data-ttu-id="23694-105">Szolgáltatásmegtagadást-tehetik az erőforrások nem érhető el a kívánt felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="23694-105">Denial-of-service attacks might make resources unavailable to intended users.</span></span> <span data-ttu-id="23694-106">Jelszó támadások erőforrások jogosulatlan elérésére vezethet.</span><span class="sxs-lookup"><span data-stu-id="23694-106">Password attacks lead to unauthorized access to resources.</span></span> <span data-ttu-id="23694-107">Az Azure Active Directory B2C (Azure AD B2C-vel) rendelkezik beépített szolgáltatásokat tartalmaz, amely segítségével az adatok többféle módon ezek a fenyegetések elleni védelméhez.</span><span class="sxs-lookup"><span data-stu-id="23694-107">Azure Active Directory B2C (Azure AD B2C) has built-in features that can help you protect your data against these threats in multiple ways.</span></span>

## <a name="denial-of-service-attacks"></a><span data-ttu-id="23694-108">Szolgáltatásmegtagadási támadások</span><span class="sxs-lookup"><span data-stu-id="23694-108">Denial-of-service attacks</span></span>

<span data-ttu-id="23694-109">Az Azure AD B2C által használt észlelés és javaslat módszerek, például SZIN cookie-k vagy sebesség és a kapcsolat korlátai az alapul szolgáló erőforrások-szolgáltatásmegtagadási támadások elleni védelméhez.</span><span class="sxs-lookup"><span data-stu-id="23694-109">Azure AD B2C uses detection and mitigation techniques like SYN cookies, and rate and connection limits to protect underlying resources against denial-of-service attacks.</span></span>

## <a name="password-attacks"></a><span data-ttu-id="23694-110">Jelszó támadások</span><span class="sxs-lookup"><span data-stu-id="23694-110">Password attacks</span></span>

<span data-ttu-id="23694-111">Az Azure AD B2C is rendelkezik megoldás technikák jelszó támadásokat.</span><span class="sxs-lookup"><span data-stu-id="23694-111">Azure AD B2C also has mitigation techniques in place for password attacks.</span></span> <span data-ttu-id="23694-112">Megoldás jelszó találgatásos támadások és a jelszó szótáras tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="23694-112">Mitigation includes brute-force password attacks and dictionary password attacks.</span></span> <span data-ttu-id="23694-113">Ésszerűen összetettnek lennie, hogy a felhasználók által beállított van szükség.</span><span class="sxs-lookup"><span data-stu-id="23694-113">Passwords that are set by users are required to be reasonably complex.</span></span> <span data-ttu-id="23694-114">Az Azure AD B2C különböző jelek segítségével elemzi a kérelmek integritását.</span><span class="sxs-lookup"><span data-stu-id="23694-114">By using various signals, Azure AD B2C analyzes the integrity of requests.</span></span> <span data-ttu-id="23694-115">Az Azure AD B2C intelligens módon megkülönböztetni azokat a támadók, és a botnetek kívánt felhasználók számára készült.</span><span class="sxs-lookup"><span data-stu-id="23694-115">Azure AD B2C is designed to intelligently differentiate intended users from hackers and botnets.</span></span> <span data-ttu-id="23694-116">Az Azure AD B2C biztosít a támadások valószínűségét a beírt jelszavak alapján fiókok zárolására kifinomult stratégiát.</span><span class="sxs-lookup"><span data-stu-id="23694-116">Azure AD B2C provides a sophisticated strategy to lock accounts based on the passwords entered, in the likelihood of an attack.</span></span>

<span data-ttu-id="23694-117">További tudnivalókért keresse fel a [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span><span class="sxs-lookup"><span data-stu-id="23694-117">For more information, visit the [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span></span>
