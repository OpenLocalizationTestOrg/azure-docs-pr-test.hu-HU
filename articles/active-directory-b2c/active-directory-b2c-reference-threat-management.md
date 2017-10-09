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
# <a name="azure-active-directory-b2c-threat-management"></a>Az Azure Active Directory B2C: A fenyegetés kezelése

Fenyegetés felügyeleti a rendszer és a hálózatok elleni támadások elleni védelem megtervezését foglalja magában. Szolgáltatásmegtagadást-tehetik az erőforrások nem érhető el toointended felhasználók. Jelszó támadások átfutási toounauthorized hozzáférés tooresources. Az Azure Active Directory B2C (Azure AD B2C-vel) rendelkezik beépített szolgáltatásokat tartalmaz, amely segítségével az adatok többféle módon ezek a fenyegetések elleni védelméhez.

## <a name="denial-of-service-attacks"></a>Szolgáltatásmegtagadási támadások

Az Azure AD B2C észlelés és javaslat módszerek, például SZIN cookie-kat, és az alapul szolgáló erőforrások-szolgáltatásmegtagadási támadások elleni sebesség és a kapcsolat korlátai tooprotect használja.

## <a name="password-attacks"></a>Jelszó támadások

Az Azure AD B2C is rendelkezik megoldás technikák jelszó támadásokat. Megoldás jelszó találgatásos támadások és a jelszó szótáras tartalmaz. Felhasználó által megadott jelszóban szükséges toobe ésszerűen összetett. Különböző jelek segítségével az Azure AD B2C elemzi a kérelmek hello sértetlenségét. Az Azure AD B2C úgy van kialakítva, toointelligently támadók és botnetek különbséget tenni kívánt felhasználók számára. Az Azure AD B2C biztosít a kifinomult stratégia toolock fiókok alapján megadott hello annak valószínűségét, hogy a támadás hello jelszavak.

További információkért látogasson el a hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
