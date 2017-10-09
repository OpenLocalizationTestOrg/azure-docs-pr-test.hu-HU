---
title: "Sipi fájl tesztelése |} Microsoft Docs"
description: "Toocheck ReadyForTest fájlfüggőségek tesztelése"
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
ms.openlocfilehash: afd3dc94dfb30926b316256fb06a768a391004f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sipi-test-file"></a>Sipi fájl tesztelése

Ez a gyors útmutató pár percben összefoglalja egy alkalmazás regisztrálásának folyamatát egy Microsoft Azure Active Directory (Azure AD) B2C-bérlőben. Amikor végzett, az alkalmazás regisztrálva van hello Azure B2C-bérlőben.

## <a name="prerequisites"></a>Előfeltételek

a felhasználói regisztrációt és bejelentkezést elfogadó alkalmazás toobuild, először tooregister hello alkalmazás Azure Active Directory B2C-bérlő. Című rész lépéseit hello segítségével könnyebben nyerhet saját bérlőt [az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).

Az Azure AD B2C hello panel az hello Azure-portálon létrehozott alkalmazások felügyelni a hello ugyanazon a helyen. Ha manuálisan szerkeszti a PowerShell vagy egy másik portálon hello B2C alkalmazások, nem támogatott lesz, és nem működik együtt az Azure AD B2C. A részleteket a hello [hibát jelzett az alkalmazások](#faulted-apps) szakasz. 

## <a name="navigate-toob2c-settings"></a>Keresse meg a tooB2C beállítások

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) , hello hello B2C bérlő globális rendszergazdája. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

Válassza ki a következő lépések regisztrál hello alkalmazás típusától függően:
