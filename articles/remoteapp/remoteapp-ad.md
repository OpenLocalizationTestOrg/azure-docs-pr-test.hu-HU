---
title: "aaaAzure AD + Active Directory követelményeit, az Azure Remoteappban |} Microsoft Docs"
description: Megtudhatja, hogyan tooset fel az Active Directory toowork az Azure RemoteApp.
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c1c4a7ad6fb96ec4d479fdc231f03d81b58b2b71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Az Azure AD + Azure RemoteApp Active Directory követelményei
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp hibrid gyűjtemény vagy egy felhőalapú gyűjtemény, amelyet az AD Connect használatával toofederate toodo hello következőkre lesz szüksége.

### <a name="connect-azure-ad-and-active-directory"></a>Az Azure AD Connect és Active Directory
Ha azt szeretné tooconnect, az Azure AD-bérlő és a helyszíni Active Directory-környezeteket, használja az AD Connect. Csak eltarthat, [4 kattintással](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello két könyvtárak.

Megjegyzés: - a címtár-szinkronizálás szükséges a hibrid gyűjtemények.

### <a name="make-sure-your-domaincom-match"></a>Győződjön meg arról, hogy a "@domain.com" felel meg
Mielőtt elkezdené, győződjön meg arról, hogy a helyi erdő megfelel hello utótag, az Azure AD-tartomány UPN hello listájában. 

Miután beállította UPN-tartomány utótag hello Azure AD-ben, az Azure RemoteApp-naplózás minden felhasználó fog bejelentkezni "@ felhasználó<hello suffix you set up>." Győződjön meg arról, hogy a felhasználók is bejelentkezhetnek hello be azonos user@suffix hello helyszíni tartományába. Bizonyos esetekben állíthat be egy tartománynév-az memóriakapacitásánál felhasználói hello ugyanaz a tartományi utótag meg Azure AD-ben Ebben az esetben a felhasználók nem fogja tudni tooconnect tooany tartományhoz csatlakoztatott számítógépek vagy Azure RemoteApp erőforrások.

Például, ha beállította az UPN-tartomány utótag az aad-ben, mint a contoso.com, de egyes felhasználók a helyi/AD azzal konfigurált toolog @contoso.uk, akkor azoknak a felhasználóknak nem lesz képes toocorrectly jelentkezzen be a hello ARA gyűjtemény. Legyen egyszerű felhasználónév az aad-ben és az AD felhasználók hello azonos hello bejelentkezési toobe lehetséges"

### <a name="create-objects-for-azure-remoteapp"></a>Az Azure RemoteApp objektumok létrehozása
A helyszíni Active Directory-objektumok a következő toocreate hello is szüksége lesz:

* A szolgáltatás fiók tooprovide hozzáférés toodomain erőforrásainak RemoteApp programok RDSH végpontok toohello a helyi tartományhoz.
* Egy szervezeti egységet (OU) toocontain RemoteApp gép objektumok. Szervezeti egység hello használata javasolt (de nem kötelező) tooisolate hello fiókok és szabályzatok használja, akkor RemoteApp.

Kell mindkét ezeket az objektumokat a RemoteApp-gyűjtemény létrehozásakor ezért lehet, hogy toodo ezeket a lépéseket először.

