---
title: "a felhasználók fel az Azure AD Join aaaSetting |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogy a rendszergazdák miként állíthatják be az Azure AD Joint a helyszíni címtár- és eszközregisztrációhoz."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: bfc5d415-c918-4d8b-afee-b3f41cc28469
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 60a5aeb11292cb6057ab1065c3ab77e5981d0cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-azure-ad-join-in-your-organization"></a>Az Azure AD Join beállítása a vállalatánál
Az Azure Active Directory Join (Azure AD Join) beállítása előtt tooeither szinkronizálási kell regisztrálnia a helyszíni címtár felhasználói toohello felhő, vagy manuálisan felügyelt fiókokat létrehoznia az Azure AD-ben.

Szinkronizálás a helyszíni felhasználók tooAzure AD ismertetett történő szinkronizálásának részletes útmutatásáért [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

toomanually létrehozása és felhasználók kezelése az Azure ad-ben, olvassa el a túl[felhasználók kezelése az Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Az eszközregisztráció beállítása
1. Jelentkezzen be toohello Azure portálra rendszergazdaként.
2. Hello bal oldali ablaktáblában jelölje ki **Active Directory**.
3. A hello **Directory** lapra, válassza ki azt a címtárat.
4. Jelölje be hello **konfigurálása** fülre.
5. Nyissa meg toohello **eszközök** szakasz.
6. A hello **eszközök** lapján állítsa be a következőket hello:  
   * **MAXIMÁLIS SZÁMÁT az eszközök felhasználónként**: válassza ki, amelyeken az Azure ad-ben a felhasználói eszközök hello maximális számát.  Ha egy felhasználó eléri ezt a kvótát, nem fogják tudni tooadd további eszközöket egy vagy több meglévő eszközt eltávolításáig.
   * **IGÉNYELNEK a TÖBBTÉNYEZŐS hitelesítés tooJOIN eszközök**: akár felhasználók szükség tooprovide beállítása egy második hitelesítési tényező toojoin az eszköz tooAzure AD. További információ az Azure multi-factor Authentication: [Ismerkedés az Azure multi-factor Authentication hello felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).
   * **FELHASZNÁLÓK előfordulhat, hogy az AZURE AD JOIN eszközök**: hello felhasználókat és csoportokat, amelyek toojoin eszközök tooAzure AD kiválasztása.
   * **További RENDSZERGAZDÁK az AZURE AD csatlakoztatott eszközök**: az Azure AD prémium vagy hello nagyvállalati mobilitási csomag (EMS), kiválaszthatja, mely felhasználók kapjanak helyi rendszergazdai jogosultságokkal toohello eszköz. A globális rendszergazdák és eszköztulajdonosok alapértelmezés szerint helyi rendszergazdai jogosultságot kapnak.

<center>![Az eszközregisztráció beállítása](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Beállítása után Azure AD Joint a felhasználók számára, hogy csatlakozhassanak tooAzure AD keresztül a vállalati vagy személyes eszközökön.

Az alábbiakban hello három forgatókönyvvel tooenable a felhasználók tooset mentése az Azure AD Join:

* Felhasználók a vállalati tulajdonban lévő eszköz csatlakozzon közvetlenül tooAzure AD.
* Felhasználók tartományhoz való csatlakozást a vállalati tulajdonú eszköz toohello a helyszíni Active Directory, majd kiterjeszthetik hello eszköz tooAzure AD.
* Felhasználók hozzáadása a munkahelyi vagy iskolai fiókok tooWindows személyes eszköz

## <a name="additional-information"></a>További információ
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

