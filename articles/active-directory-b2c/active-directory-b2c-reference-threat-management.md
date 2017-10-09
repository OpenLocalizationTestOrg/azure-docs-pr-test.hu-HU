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
ms.openlocfilehash: 60bc0cc392b332cc4e9741ddb97dfa58e68ed420
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a><span data-ttu-id="3d785-103">Az Azure Active Directory B2C: A fenyegetés kezelése</span><span class="sxs-lookup"><span data-stu-id="3d785-103">Azure Active Directory B2C: Threat management</span></span>

<span data-ttu-id="3d785-104">Fenyegetés felügyeleti a rendszer és a hálózatok elleni támadások elleni védelem megtervezését foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="3d785-104">Threat management includes planning for protection from attacks against your system and networks.</span></span> <span data-ttu-id="3d785-105">Szolgáltatásmegtagadást-tehetik az erőforrások nem érhető el toointended felhasználók.</span><span class="sxs-lookup"><span data-stu-id="3d785-105">Denial-of-service attacks might make resources unavailable toointended users.</span></span> <span data-ttu-id="3d785-106">Jelszó támadások átfutási toounauthorized hozzáférés tooresources.</span><span class="sxs-lookup"><span data-stu-id="3d785-106">Password attacks lead toounauthorized access tooresources.</span></span> <span data-ttu-id="3d785-107">Az Azure Active Directory B2C (Azure AD B2C-vel) rendelkezik beépített szolgáltatásokat tartalmaz, amely segítségével az adatok többféle módon ezek a fenyegetések elleni védelméhez.</span><span class="sxs-lookup"><span data-stu-id="3d785-107">Azure Active Directory B2C (Azure AD B2C) has built-in features that can help you protect your data against these threats in multiple ways.</span></span>

## <a name="denial-of-service-attacks"></a><span data-ttu-id="3d785-108">Szolgáltatásmegtagadási támadások</span><span class="sxs-lookup"><span data-stu-id="3d785-108">Denial-of-service attacks</span></span>

<span data-ttu-id="3d785-109">Az Azure AD B2C észlelés és javaslat módszerek, például SZIN cookie-kat, és az alapul szolgáló erőforrások-szolgáltatásmegtagadási támadások elleni sebesség és a kapcsolat korlátai tooprotect használja.</span><span class="sxs-lookup"><span data-stu-id="3d785-109">Azure AD B2C uses detection and mitigation techniques like SYN cookies, and rate and connection limits tooprotect underlying resources against denial-of-service attacks.</span></span>

## <a name="password-attacks"></a><span data-ttu-id="3d785-110">Jelszó támadások</span><span class="sxs-lookup"><span data-stu-id="3d785-110">Password attacks</span></span>

<span data-ttu-id="3d785-111">Az Azure AD B2C is rendelkezik megoldás technikák jelszó támadásokat.</span><span class="sxs-lookup"><span data-stu-id="3d785-111">Azure AD B2C also has mitigation techniques in place for password attacks.</span></span> <span data-ttu-id="3d785-112">Megoldás jelszó találgatásos támadások és a jelszó szótáras tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3d785-112">Mitigation includes brute-force password attacks and dictionary password attacks.</span></span> <span data-ttu-id="3d785-113">Felhasználó által megadott jelszóban szükséges toobe ésszerűen összetett.</span><span class="sxs-lookup"><span data-stu-id="3d785-113">Passwords that are set by users are required toobe reasonably complex.</span></span> <span data-ttu-id="3d785-114">Különböző jelek segítségével az Azure AD B2C elemzi a kérelmek hello sértetlenségét.</span><span class="sxs-lookup"><span data-stu-id="3d785-114">By using various signals, Azure AD B2C analyzes hello integrity of requests.</span></span> <span data-ttu-id="3d785-115">Az Azure AD B2C úgy van kialakítva, toointelligently támadók és botnetek különbséget tenni kívánt felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="3d785-115">Azure AD B2C is designed toointelligently differentiate intended users from hackers and botnets.</span></span> <span data-ttu-id="3d785-116">Az Azure AD B2C biztosít a kifinomult stratégia toolock fiókok alapján megadott hello annak valószínűségét, hogy a támadás hello jelszavak.</span><span class="sxs-lookup"><span data-stu-id="3d785-116">Azure AD B2C provides a sophisticated strategy toolock accounts based on hello passwords entered, in hello likelihood of an attack.</span></span>

<span data-ttu-id="3d785-117">További információkért látogasson el a hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span><span class="sxs-lookup"><span data-stu-id="3d785-117">For more information, visit hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).</span></span>
