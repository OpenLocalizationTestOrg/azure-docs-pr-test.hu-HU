---
title: "aaaTroubleshooting hello automatikus regisztráció az Azure AD-tartomány csatlakoztatott számítógépek Windows régebbi ügyfelekhez |} Microsoft Docs"
description: "Windows-kezelés régebbi ügyfelek hello automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 84fe666576f13de09d1eaa5692517d45a4dbeebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad-for-windows-down-level-clients"></a>Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD Windows régebbi ügyfelek 

Ez a témakör a következő ügyfelek alkalmazható csak toohello: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 és Windows Server 2016: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).

Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-device-registration-get-started.md).
 
Ez a témakör útmutatást hogyan tooresolve lehetőségeket kínál a problémák hibaelhárítása.  
Néhány dolgot toonote a sikeres eredményekkel: 

- Az ügyfelek az Azure AD-regisztráció van felhasználó/eszköz. Példa: Ha jdoe és jharnett bejelentkezéskor toothis eszköz, a különböző regisztrációs (DeviceID) hoz létre minden egyes ezen felhasználók hello felhasználó adatai lap.  

- Ezen ügyfelek hello kezdő verzióról regisztrációs bejelentkezéskor vagy zárolt vagy feloldott konfigurált tootry, és lehet, hogy ez akkor váltódik ki, a Feladatütemező feladat 5 perces késleltetés. 

- Egy telepítse újra a hello operációs rendszer vagy egy manuális regisztrációját és regisztrálja újra az lehet, hogy hozzon létre egy új regisztrációs Azure ad-val, és hatására a több bejegyzés hello felhasználó adatai lap hello Azure-portálon. 


## <a name="step-1-retrieve-hello-registration-status"></a>1. lépés: Hello regisztrációs állapotának lekérése 

**tooverify hello regisztrációs állapotát:**  

1. Nyissa meg hello parancssort rendszergazdaként 

2. Típusa`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

A parancs egy párbeszédpanelt, amely biztosít hello illesztési állapotával kapcsolatos további adatokat jeleníti meg.

![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-registration-status"></a>2. lépés: Hello regisztrációs állapotának kiértékelésére. 

Ha hello csatlakoztatás sikertelen volt, hello párbeszédpanel biztosít hello probléma történt részleteit.

**hello kapcsolatos leggyakoribb hibák a következők:**

- Egy helytelenül konfigurált AD FS vagy az Azure AD

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Nincs bejelentkezve tartományi felhasználóként

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- A kvóta elérve

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- hello szolgáltatás nem válaszol 

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Hello állapotadatokat is található eseménynaplóban hello **alkalmazások és szolgáltatások Log\Microsoft-munkahelyi csatlakoztatás**.
  
**Sikertelen regisztráció a hello leggyakoribb okok a következők:** 

- A számítógép be kapcsolva nem hello vállalati belső hálózaton vagy egy virtuális Magánhálózati kapcsolat tooan nélkül a helyszíni AD-tartományvezérlő.

- Tooyour számítógép helyi fiókkal van bejelentkezve. 

- Szolgáltatás konfigurációs problémák: 

  - hello összevonási kiszolgáló lett konfigurált toosupport **WIAORMULTIAUTHN**. 

  - Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum.

  - A felhasználó elérte eszközök hello korlátot. Lásd: Ismerkedés az Azure Active Directory Eszközregisztráció.

## <a name="next-steps"></a>Következő lépések

További információkért lásd: hello [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md) 
