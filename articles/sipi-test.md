---
title: "Sipi fájl tesztelése |} Microsoft Docs"
description: "Fájl tesztelése ReadyForTest függőségek ellenőrzése"
services: active-directory-b2c
documentationcenter: 
author: Sipi
manager: Sipi
editor: Sipi
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f68
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: Sipi
ms.openlocfilehash: 871d58818dcbaee5f7a5f07c19e2297ec6459a6f
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/28/2017
---
# <a name="sipi-test-file"></a>Sipi fájl tesztelése

Ez a gyors útmutató pár percben összefoglalja egy alkalmazás regisztrálásának folyamatát egy Microsoft Azure Active Directory (Azure AD) B2C-bérlőben. Az útmutató befejeztével az alkalmazás regisztrálva lesz és használatra készen áll az Azure B2C-bérlőben.

## <a name="prerequisites"></a>Előfeltételek

A felhasználói regisztrációt és bejelentkezést elfogadó alkalmazás létrehozásához először regisztrálnia kell az alkalmazást egy Azure Active Directory B2C-bérlővel. Hozzon létre egy saját bérlőt az [Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md) részben ismertetett lépések segítségével.

Az Azure Portal Azure AD B2C paneljén létrehozott alkalmazásokat ugyanarról a helyről kell kezelnie. Ha a B2C-alkalmazásokat a PowerShell vagy egy másik portál használatával szerkeszti, a rendszer nem támogatja többé az alkalmazásokat, és nem fognak működni az Azure AD B2C-vel. További részletek a [hibás alkalmazások](#faulted-apps) szakaszban. 

## <a name="navigate-to-b2c-settings"></a>Navigálás a B2C beállításaihoz

Jelentkezzen be az [Azure Portalra](https://portal.azure.com/) a B2C-bérlő globális rendszergazdájaként. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

Válassza ki a következő lépéseket a regisztrálandó alkalmazás típusától függően:
