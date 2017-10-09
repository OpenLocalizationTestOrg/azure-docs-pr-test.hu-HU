---
title: "Az Azure AD Connect szinkronizálási szolgáltatás: AD Lomtár engedélyezése |} Microsoft Docs"
description: "Ez a témakör azt javasolja, hogy az AD Lomtár szolgáltatás az Azure AD Connect hello használatát."
services: active-directory
keywords: "Az AD Lomtár véletlen törlés, forráshorgony"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2bb4827d677ccecfd8d2861f2a2fcf73b8cc2d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Az Azure AD Connect szinkronizálási szolgáltatás: AD Lomtár engedélyezése
Ajánlott a helyszíni Active könyvtárak, amelyek szinkronizált tooAzure AD hello AD Lomtár funkciót engedélyezni. 

Ha véletlenül törölt egy helyszíni AD-felhasználói objektum és a visszaállítási hello funkcióval, az Azure AD helyreállítja hello megfelelő az Azure AD felhasználói objektum.  Hello AD Lomtár szolgáltatással kapcsolatos további információkért tekintse meg a tooarticle [forgatókönyv áttekintése visszaállítása törölt Active Directory-objektumok](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-hello-ad-recycle-bin"></a>A Lomtár engedélyezése hello AD előnyei
Ez a szolgáltatás segít az Azure AD felhasználói objektumok visszaállítására hello következő tevékenységek végrehajtásával:

* Ha véletlenül törölt egy helyszíni AD-felhasználó objektum hello megfelelő az Azure AD felhasználói objektum törli hello a következő szinkronizálási ciklusban. Alapértelmezés szerint az Azure AD tartja hello törölni az Azure AD felhasználói objektum törölt állapotban 30 napig.

* Ha a helyszíni AD Lomtár szolgáltatás engedélyezve van, a törölt hello visszaállíthatja a helyszíni AD-felhasználói objektum Forráshorgony értékének módosítása nélkül. Ha hello helyreállítani a helyszíni AD-felhasználói objektum szinkronizálása tooAzure AD, az Azure AD visszaállítja hello megfelelő helyreállíthatóan törölt az Azure AD felhasználói objektum. Forráshorgony attribútum kapcsolatos információkért tekintse meg a tooarticle [az Azure AD Connect: tervezési alapelvek](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Ha nem rendelkezik a helyszíni AD Recycle Bin szolgáltatás engedélyezve van, előfordulhat, hogy az AD felhasználói objektum tooreplace hello törölt objektum szükséges toocreate. Ha az Azure AD Connect szinkronizálási szolgáltatást konfigurált toouse a rendszer AD attribútum (például az ObjectGuid) hello Forráshorgony attribútuma, hello újonnan létrehozott AD-felhasználói objektum lesz nem rendelkezik hello azonos Forráshorgony érték hello AD felhasználó-objektum törlése. Ha hello újonnan létrehozott AD felhasználói objektum szinkronizált tooAzure AD, az Azure AD létrehoz egy új Azure AD felhasználói objektum visszaállítása helyreállíthatóan törölt hello Azure AD felhasználói objektum helyett.

> [!NOTE]
> Alapértelmezés szerint az Azure AD tartja törölni az Azure AD felhasználói objektumok helyreállíthatóan törölt állapotban 30 nap elteltével a rendszer véglegesen törli. Azonban a rendszergazdák meggyorsíthatja ilyen objektumok hello törlése. Hello objektumok véglegesen törlődnek, ha azok már nem állíthatók helyre, még akkor is, ha a helyszíni AD Lomtár szolgáltatás engedélyezve van.



## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)

* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
