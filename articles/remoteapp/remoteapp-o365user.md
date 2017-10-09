---
title: "aaaHow toouse Azure RemoteApp az Office 365 felhasználói fiókjainak |} Microsoft Docs"
description: "Ismerje meg, hogy az Office 365 felhasználói fiókkal rendelkező Azure RemoteApp toouse"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: aba70fd3-60b0-4b44-9321-1ab18760ccd5
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d2dbed2a6838adf9bb0f7508eb7dcecb0a74a62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-remoteapp-with-office-365-user-accounts"></a>Hogyan toouse Azure RemoteApp az Office 365 felhasználói fiókok
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Ha az Office 365-előfizetéssel rendelkezik egy Azure Active Directoryban, amely tárolja a felhasználói neveket és jelszavakat tooaccess Office 365-szolgáltatásokhoz használt. Például amikor a felhasználók Office 365 ProPlus aktiválása hitelesítéshez az Azure AD toocheck licencek ellen. A legtöbb ügyfél volna például toouse hello ugyanazt az Azure RemoteApp könyvtárába.

Azure RemoteApp telepítése valószínűleg használ egy másik Azure AD társított Azure-előfizetéssel. A rendezés toouse az Office 365-könyvtár, szüksége lesz a toomove hello Azure-előfizetés abba a könyvtárba.

Hogyan információk toodeploy Office 365 ügyfélalkalmazások, lásd: [hogyan toouse az Office 365-előfizetéshez az Azure RemoteApp](remoteapp-officesubscription.md).

## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>1. fázis: Az Office 365 Azure Active Directory ingyenes előfizetés regisztrálása
A klasszikus Azure portálon hello használatakor lépésekkel hello a [regisztrálása az Azure Active Directory ingyenes előfizetés](https://technet.microsoft.com/library/dn832618.aspx) tooget rendszergazdai hozzáférés tooyour az Azure AD hello Azure felügyeleti portálján keresztül. Hello így a folyamat legyen képes toolog hello Azure-portálon való és lásd: a könyvtár létezik – ezen a ponton nem láthatók sokkal mivel hello használ az Azure RemoteApp teljes Azure-előfizetés használatban van egy másik címtárhoz.

Hello nevét és hello rendszergazdai fiókjának ebben a lépésben létrehozott jelszó megjegyzése – azok a 2. szakasza szükség lesz.

Ha hello Azure-portált használja, tekintse meg [hogyan tooregister és aktiválása az Office 365-portál használatával, ingyenes Azure Active Directory](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-hello-azure-ad-associated-with-your-azure-subscription"></a>2. fázis: Módosítás hello Azure AD az Azure-előfizetéshez társítva.
Fogjuk toochange az Azure-előfizetéshez az aktuális könyvtárból azt az 1. fázis együttműködik hello Office 365 könyvtárba.

Útmutatás alapján hello ismertetett [módosítás hello Azure Active Directory-bérlőt az Azure Remoteappban](remoteapp-changetenant.md). Fordítson különös figyelmet toohello a következő lépéseket:

* #1. lépés: Ha telepítette az Azure RemoteApp (ARA) ebben az előfizetésben, győződjön meg arról, előtt távolítsa el az Azure AD felhasználói fiókokhoz bármely ARA gyűjtemények először, semmi mást. Azt is megteheti érdemes lehet a meglévő gyűjtemények törlése.
* #2. lépés: Ez egy fontos lépés. Toouse Microsoft-fiókkal van szüksége (pl. @outlook.com), a szolgáltatás-rendszergazdáknak hello előfizetésben; ennek az az oka nem tudunk hello csatolva az Azure AD toohello előfizetés – meglévő, ha azt választja, az összes felhasználói fiókot a Microsoft nem fogja tudni toomove azt tooa másik Azure AD.
* #4. lépés: Egy létező könyvtár hozzáadásakor hello rendszer kérni fogja toosign be hello rendszergazdai fiókkal könyvtárhoz. Győződjön meg arról, hogy toouse hello rendszergazdai fiók az 1. szakasza.
* #5. lépés: Hello hello előfizetés tooyour Office 365-könyvtár-jének szülőkönyvtárában módosítása. hello végeredménynek kell, hogy a beállítások -> az előfizetés sorolja fel az Office 365 hello directory előfizetések. 
  ![Hello hello előfizetés-jének szülőkönyvtárában módosítása](./media/remoteapp-o365user/settings.png)

Ezen a ponton az Azure RemoteApp előfizetés társul az Office 365 az Azure AD; hello meglévő Office 365 felhasználói fiókjainak használhatja az Azure RemoteApp!

