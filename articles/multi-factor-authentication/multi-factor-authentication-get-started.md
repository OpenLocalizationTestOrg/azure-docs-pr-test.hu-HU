---
title: "az Azure MFA felhőben vagy a kiszolgáló közötti aaaChoose |} Microsoft Docs"
description: "Válassza ki a hello a multi-factor authentication biztonsági megoldás, amely az Ön számára legmegfelelőbb azzal, hogy, milyen am I toosecure majd hol vannak a felhasználók található.  Ezután válassza a felhő, az MFA-kiszolgáló vagy az AD FS lehetőséget."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: bd9639e5f744f586d9143c6e90b105ed645eecb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="choose-hello-azure-multi-factor-authentication-solution-for-you"></a>Válassza ki a hello Azure multi-factor Authentication megoldást
Mivel több verziója Azure multi-factor Authentication (MFA), azt a néhány kérdések toofigure verziószámának ki van hello megfelelő egy toouse válaszolnia kell.  A kérdések a következők:

* [Mi I kísérlet toosecure](#what-am-i-trying-to-secure)
* [Hol található a hello felhasználók](#where-are-the-users-located)
* [Mely szolgáltatásokra van szükségem?](#what-featured-do-i-need)

hello alábbi szakaszok nyújtanak útmutatást ezek a válaszok mindegyikére meghatározásának.

## <a name="what-am-i-trying-toosecure"></a>Mi I kísérlet toosecure?
toodetermine hello megfelelő kétlépéses ellenőrzés megoldás, először azt válaszolnia kell, miközben a rendszer toosecure egy második hitelesítési eljárást is, melyek hello kérdését.  Egy alkalmazást az Azure-ban?  Vagy egy távelérésű rendszert?  Mi keressük meghatározásával toosecure, hogy meg tudja válaszolni, hello kérdés, amikor a multi-factor Authentication toobe engedélyezni kell.  

| Mik azok a közben toosecure | Többtényezős hitelesítés hello felhőben | MFA-kiszolgáló |
| --- |:---:|:---:|
| Belső Microsoft-alkalmazások |● |● |
| SaaS-alkalmazásokat az hello alkalmazásgyűjtemény |● |  |
| Az Azure AD-alkalmazásproxyn keresztül közzétett webalkalmazások |● |  |
| Nem az Azure AD-alkalmazásproxyn keresztül közzétett IIS-alkalmazások | |● |
| Távelérés, például VPN vagy RDG | ● | ● |

## <a name="where-are-hello-users-located"></a>Hol található a hello felhasználók
A következő megnézi a felhasználóinkat, hol találhatók a toodetermine hello megfelelő megoldás toouse, segítségével a hello felhő, vagy a helyszíni használatával hello MFA kiszolgáló.

| Felhasználó helye | Többtényezős hitelesítés hello felhőben | MFA-kiszolgáló |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD és helyszíni AD összevonással az AD FS-sel |● |● |
| DirSync, Azure AD Sync és Azure AD Connect szolgáltatást használó Azure AD és helyszíni AD – jelszó-szinkronizálás nélkül |● |● |
| DirSync, Azure AD Sync és Azure AD Connect szolgáltatást használó Azure AD és helyszíni AD – jelszó-szinkronizálással |● | |
| Helyszíni Active Directory | |● |

## <a name="what-features-do-i-need"></a>Mely szolgáltatásokra van szükségem?
hello alábbi táblázat összehasonlítja hello elérhető funkcióját, és a többtényezős hitelesítés hello felhőben és a multi-factor Authentication kiszolgáló hello.

| Szolgáltatás | Többtényezős hitelesítés hello felhőben | MFA-kiszolgáló |
| --- |:---:|:---:|
| Mobilalkalmazásos értesítés második tényezőként | ● | ● |
| Mobilalkalmazásos ellenőrzőkód második tényezőként | ● | ● |
| Telefonhívás második tényezőként | ● | ● |
| Egyirányú SMS második tényezőként | ● | ● |
| Kétirányú SMS második tényezőként | | ● |
| Hardvertokenek második tényezőként | | ● |
| Alkalmazásjelszavak az MFA-t nem támogató Office 365-ügyfelekhez | ● | |
| A hitelesítési módszerek rendszergazdai szabályozása | ● | ● |
| PIN-mód | | ● |
| Csalási riasztás |● | ● |
| MFA-jelentések |● | ● |
| Egyszeri mellőzés | | ● |
| Egyéni üdvözlések a telefonhívásokhoz | ● | ● |
| Testreszabható hívóazonosító a telefonhívásokhoz | ● | ● |
| Megbízható IP-címek | ● | ● |
| MFA megjegyzése megbízható eszközökön | ● | |
| Feltételes hozzáférés | ● | ● |
| Gyorsítótár |  | ● |

## <a name="next-steps"></a>Következő lépések

Most, hogy azt észlelte toouse felhőalapú többtényezős hitelesítést vagy hello MFA kiszolgáló a helyszíni, hogy azt is elkezdheti beállításával és Azure multi-factor Authentication használatával. **Válassza ki a forgatókönyv hello ikonjára**

<center>




[![Felhőalapú](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Kiszolgáló](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</center>
