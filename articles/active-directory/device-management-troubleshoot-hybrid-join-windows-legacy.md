---
title: "Azure Active Directory aaaTroubleshooting hibrid csatlakoztatott régebbi eszközök |} Microsoft Docs"
description: "Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott régebbi eszközök."
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
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: edd56b89579fac6b427732902284ad9c568b87b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Hibaelhárítás az Azure Active Directory hibrid csatlakoztatott régebbi eszközök 

Ez a témakör a következő eszközök alkalmazható csak toohello: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 és Windows Server 2016: [hibaelhárítás hibrid Azure Active Directoryhoz csatlakoztatott Windows 10 és Windows Server 2016-os eszközök](device-management-troubleshoot-hybrid-join-windows-current.md).

Ez a témakör azt feltételezi, hogy [konfigurált hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello a következő esetekben:

- Eszközalapú feltételes hozzáférés

- [Vállalati központi beállítások](active-directory-windows-enterprise-state-roaming-overview.md)

- [Vállalati Windows Hello](active-directory-azureadjoin-passport-deployment.md) 





Ez a témakör útmutatást hogyan tooresolve lehetőségeket kínál a problémák hibaelhárítása.  

**Tudnivalók:** 

- hello maximális száma felhasználónként eszközközpontú. Például ha *jdoe* és *jharnett* bejelentkezési tooa eszköz, a különböző regisztrációs (DeviceID) hoz létre minden egyes azokat hello **felhasználói** információ lapon.  

- kezdeti regisztrációs hello / join az eszközök az beállított tooperform egy kísérlet bejelentkezési vagy a zárolás / zárolásának feloldásához. A Feladatütemező által indított 5 perces késleltetés lehet. 

- Egy hello operációs rendszer vagy a manuális újratelepítés unregister és regisztrálja újra lehet, hogy hozzon létre egy új regisztrációs Azure ad-val és hello felhasználó adatai lap több bejegyzést eredményez hello Azure-portálon. 


## <a name="step-1-retrieve-hello-registration-status"></a>1. lépés: Hello regisztrációs állapotának lekérése 

**tooverify hello regisztrációs állapotát:**  

1. Nyissa meg hello parancssort rendszergazdaként 

2. Típusa`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

A parancs egy párbeszédpanelt, amely biztosít hello illesztési állapotával kapcsolatos további adatokat jeleníti meg.

![A munkahelyi csatlakoztatás Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-hybrid-azure-ad-join-status"></a>2. lépés: Hello hibrid az Azure AD join állapotának kiértékelésére. 

Ha hello hibrid az Azure AD join nem volt sikeres, hello párbeszédpanel biztosít hello probléma történt részleteit.

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
  
**egy sikertelen a hibrid az Azure AD join hello leggyakoribb okai a következők:** 

- A számítógép be kapcsolva nem hello vállalati belső hálózaton vagy egy virtuális Magánhálózati kapcsolat tooan nélkül a helyszíni AD-tartományvezérlő.

- Tooyour számítógép helyi fiókkal van bejelentkezve. 

- Szolgáltatás konfigurációs problémák: 

  - hello összevonási kiszolgáló lett konfigurált toosupport **WIAORMULTIAUTHN**. 

  - Nincs tooyour ellenőrzött tartomány nevét az Azure AD oldalra mutat, ahol hello a számítógép tartozik hello AD-erdő szolgáltatáskapcsolódási pont objektum.

  - A felhasználó elérte eszközök hello korlátot. 

## <a name="next-steps"></a>Következő lépések

A kérdésekhez lásd: hello [eszköz felügyeleti kapcsolatos gyakori kérdések](device-management-faq.md)  
