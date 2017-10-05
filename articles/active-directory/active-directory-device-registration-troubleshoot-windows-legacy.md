---
title: "A régebbi Windows-ügyfelek automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit |} Microsoft Docs"
description: "A régebbi Windows-ügyfelek automatikus regisztráció az Azure AD-tartomány hibaelhárítási csatlakoztatott számítógépeit."
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
ms.openlocfilehash: a7c8ef4c59c53c21258f0c61963d8f994a3946ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a>Automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépeit az Azure AD Windows régebbi ügyfelek 

Ez a témakör a tulajdonság csak a következő ügyfelek vonatkozik: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 és Windows Server 2016: [automatikus regisztráció tartomány hibaelhárítási csatlakoztatott számítógépeit az Azure AD – a Windows 10 és Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).

Ez a témakör feltételezi, hogy konfigurálta a tartományhoz csatlakoztatott eszközök automatikus regisztráció megfelelően ismertetett [konfigurálása a Windows-tartományhoz csatlakoztatott eszközök automatikus regisztrálása az Azure Active Directoryval](active-directory-device-registration-get-started.md).
 
Ez a témakör nyújt hibaelhárítási útmutatót a lehetséges problémák megoldásához.  
A sikeres eredményekkel ügyeljen a következőkre: 

- Az ügyfelek az Azure AD-regisztráció van felhasználó/eszköz. Példa: jdoe és jharnett jelentkezzen be az eszközt, ha ezeket a felhasználókat, a felhasználó adatai lap egy különálló regisztrációs (DeviceID) van-e létre.  

- Ezek az ügyfelek a beépített nyilvántartási bejelentkezéskor vagy zárolt vagy feloldott próbálja van konfigurálva, és lehet, hogy ez akkor váltódik ki, a Feladatütemező feladat 5 perces késleltetés. 

- Egy telepítse újra az operációs rendszer vagy egy manuális regisztrációját és regisztrálja újra az lehet, hogy hozzon létre egy új regisztrációs Azure ad-val, és hatására a több bejegyzést a felhasználó adatai lap az Azure-portálon. 


## <a name="step-1-retrieve-the-registration-status"></a>1. lépés: A regisztráció állapotának lekérése 

**A regisztráció állapotának ellenőrzése:**  

1. Nyissa meg a parancssort rendszergazdaként 

2. Típusa`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

A parancs egy párbeszédpanelt, amely lehetővé teszi az illesztési állapotával kapcsolatos további adatokat jeleníti meg.

![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a>2. lépés: A regisztráció állapotának kiértékelésére. 

Ha a csatlakozás sikertelen volt, a párbeszédpanel biztosít információkhoz juthat a problémáról történt.

**A leggyakoribb problémák vannak:**

- Egy helytelenül konfigurált AD FS vagy az Azure AD

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Nincs bejelentkezve tartományi felhasználóként

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- A kvóta elérve

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- A szolgáltatás nem válaszol. 

    ![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Az állapot információt az eseménynaplóban a is talál **alkalmazások és szolgáltatások Log\Microsoft-munkahelyi csatlakoztatás**.
  
**Sikertelen regisztráció leggyakoribb okai a következők:** 

- A számítógép nincs a szervezet belső hálózaton vagy egy helyszíni kapcsolat nélkül VPN AD-tartományvezérlő.

- A számítógép helyi fiókkal jelentkezett be. 

- Szolgáltatás konfigurációs problémák: 

  - Az összevonási kiszolgáló konfigurációja támogatja **WIAORMULTIAUTHN**. 

  - Nincs mutat, a ellenőrzött tartomány nevét az Azure ad-ben az Active Directory-erdőben, ahol a számítógép tartozik szolgáltatáskapcsolódási pont objektum.

  - A felhasználó elérte a határértéket, az eszközök. Lásd: Ismerkedés az Azure Active Directory Eszközregisztráció.

## <a name="next-steps"></a>Következő lépések

További információkért lásd: a [automatikus eszközregisztráció – gyakori kérdések](active-directory-device-registration-faq.md) 
