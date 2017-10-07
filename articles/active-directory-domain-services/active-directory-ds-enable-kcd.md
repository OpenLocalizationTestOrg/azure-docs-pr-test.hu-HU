---
title: "Az Azure Active Directory tartományi szolgáltatások: A kerberos által korlátozott delegálás engedélyezése |} Microsoft Docs"
description: "Kerberos által korlátozott delegálás a felügyelt tartományok Azure Active Directory tartományi szolgáltatások engedélyezése"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: d32547c8018dd1f99c992e95a01d2711d460a261
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Kerberos által korlátozott delegálás (KCD) konfigurálása a felügyelt tartományhoz
Számos alkalmazás tooaccess erőforrások hello hello felhasználó környezetében kell. Az Active Directory támogatja-e a Kerberos-delegálás, amely lehetővé teszi a használati eset nevű. Emellett korlátozhatja delegálás úgy, hogy csak egy adott erőforráshoz elérhető hello hello felhasználó környezetében. Az Azure AD tartományi szolgáltatások felügyelt tartományok különböznek a hagyományos Active Directory-tartományok, mivel biztonságosabban zárolva miatt.

Ez a cikk bemutatja, hogyan tooconfigure kerberos által korlátozott delegálás az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos által korlátozott delegálás (KCD)
Kerberos-delegálás engedélyezi egy fiók tooimpersonate egy másik biztonsági egyszerű (például egy felhasználó) tooaccess erőforrásokat. Fontolja meg egy webalkalmazást, aki hozzáfér a háttér-webes API-k egy felhasználó hello környezetében. Ebben a példában hello (szolgáltatásfiók vagy egy számítógép-vagy számítógépfiók hello környezetében fut) webalkalmazás hello felhasználót megszemélyesítő (webes API) hello erőforráshoz történő hozzáféréskor. Kerberos-delegálás nem biztonságos, mivel nem korlátozza a fiók megszemélyesítése erőforrások hello férhetnek hozzá hello felhasználó környezetében hello hello.

Kerberos által korlátozott delegálás (KCD) szolgáltatások/erőforrások toowhich hello megadott kiszolgáló műveleteket hajthat végre a felhasználó nevében hello hello korlátozza. Hagyományos Kerberos által korlátozott Delegálás szükséges tartományi rendszergazdai jogosultságokkal tooconfigure egy olyan tartományi fiók egy szolgáltatás, és korlátozza a hello fiók tooa egyetlen tartományon.

Hagyományos Kerberos által korlátozott Delegálás is van néhány olyan probléma, társítva. A korábbi operációs rendszerekben, ahol a hello tartományi rendszergazda konfigurálta a hello szolgáltatást hello szolgáltatás-rendszergazda kellett nem használható megoldást tooknow, mely előtér-szolgáltatások meghatalmazott toohello erőforrás-szolgáltatások tulajdonosa volt. És bármely előtér-szolgáltatás, amely átadhatja tooa erőforrás-szolgáltatás potenciális támadási felület. Ha egy kiszolgálót, amely előtér-szolgáltatás tárolt biztonsági szempontból sérült, és a beállított toodelegate tooresource szolgáltatások volt, hello erőforrás-szolgáltatások is utaló jeleket.

> [!NOTE]
> Az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz, a nem tartományi rendszergazdai jogosultságokkal rendelkezik. Ezért **hagyományos Kerberos által korlátozott Delegálás nem konfigurálható egy felügyelt tartomány**. Erőforrás-alapú Kerberos által korlátozott Delegálás használata, ebben a cikkben leírtak szerint. Ez a módszer használata is biztonságosabb.
>
>

## <a name="resource-based-kerberos-constrained-delegation"></a>Erőforrás-alapú kerberos által korlátozott delegálás
A Windows Server 2012 R2 és Windows Server 2012 hello képességét tooconfigure által korlátozott delegálás hello szolgáltatás hatáskörébe tartozik hello tartományi rendszergazda toohello szolgáltatás-rendszergazdát. Ezzel a módszerrel hello háttér-szolgáltatás-rendszergazda engedélyezheti vagy tagadhatja meg előtér-szolgáltatásokat. Ez a modell nevezik **erőforrás-alapú kerberos által korlátozott delegálás**.

Erőforrás-alapú Kerberos által korlátozott Delegálás konfigurálva van a PowerShell használatával. Set-ADComputer hello vagy a Set-ADUser parancsmag, attól függően, hogy hello fiók megszemélyesítése a számítógépfiókkal vagy egy felhasználói fiók/service fiókot használja.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Erőforrás-alapú számítógép fiók Kerberos által korlátozott Delegálás konfigurálása a felügyelt tartományhoz
Tegyük fel, a webes alkalmazás hello számítógépen futó "contoso100-webapp.contoso100.com". Tooaccess hello erőforrás szükséges (a futó webes API "contoso100-api.contoso100.com") a tartományi felhasználók hello környezetében. Itt látható, hogyan állíthat be az erőforrás-alapú ebben a forgatókönyvben a Kerberos által korlátozott Delegálás.

```
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Erőforrás-alapú felhasználói fiókok Kerberos által korlátozott Delegálás konfigurálása a felügyelt tartományhoz
Azt feltételezik, a webes alkalmazás futtatásához használt szolgáltatásfiók "appsvc", és szükséges tooaccess hello erőforrás (a webes API futtatásához használt szolgáltatásfiók - "backendsvc") a tartományi felhasználók hello környezetében. Itt látható, hogyan állíthat be az erőforrás-alapú ebben a forgatókönyvben a Kerberos által korlátozott Delegálás.

```
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [A Kerberos általi korlátozott delegálás áttekintése](https://technet.microsoft.com/library/jj553400.aspx)
