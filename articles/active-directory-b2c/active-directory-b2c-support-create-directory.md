---
title: "Az Azure Active Directory B2C: Létrehozása bérlők hibaelhárítása |} Microsoft Docs"
description: "Problémák és megoldások létrehozásához az Azure Active Directory vagy az Azure Active Directory B2C-bérlő."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 4bb7ec6ec67d3368a0e36c098f290f582510714a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Hibaelhárítás az Azure Active Directory vagy az Azure Active Directory B2C-bérlő létrehozása 

## <a name="create-an-azure-ad-tenant"></a>Az Azure AD-bérlő létrehozása
Ha nem hozható létre az Azure Active Directory (Azure AD) bérlő hello első kísérlet alkalmával, próbálkozzon újra. Ha hello a probléma továbbra is fennáll, forduljon az Azure támogatási szolgálatához.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C bérlő létrehozása
Ha hibát tapasztal amikor Ön [hozzon létre egy Azure Active Directory B2C (Azure AD B2C) bérlő](active-directory-b2c-get-started.md), próbálja meg az alábbi beállítások hello:

* Ha hello Azure AD B2C bérlő nem jelenik meg a bérlők listáját, próbálja meg újra toocreate hello bérlői.
* Hello Azure AD B2C bérlő jelenik meg a bérlők listáját, és hello a következő hibaüzenet jelenik meg, ha hello bérlői törölje és hozza létre újra:

    "Nem hajtható végre hello B2C-bérlő"contosob2c"hello létrehozását. Látogasson el a [hivatkozás](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) további útmutatást. "
* Nincs ismert problémák Ha töröl egy meglévő Azure AD B2C-bérlőben és hozza létre újból hello segítségével ugyanazon a néven. Amikor létrehoz egy új Azure AD B2C-bérlő, a másik tartománynevet kell használnia.
* Ha ezek a megoldások nem működnek, lépjen kapcsolatba az Azure támogatási szolgálatához. További információkért lásd: [fájl támogatási kérelmek az Azure AD B2C](active-directory-b2c-support.md).

