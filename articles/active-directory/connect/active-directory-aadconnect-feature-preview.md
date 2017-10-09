---
title: "Az Azure AD Connect: Preview szolgáltatásai |} Microsoft Docs"
description: "Ez a témakör ismerteti a további részletek funkciók még csak előzetes verziójúak, az Azure AD Connectben."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bcfc710861b19d8f86f094ced0d1c691e0911f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="more-details-about-features-in-preview"></a>További információt az előzetes funkciók
Ez a témakör ismerteti, hogyan toouse jelenleg előzetes verzióban érhetők szolgáltatásokat.

## <a name="group-writeback"></a>Group writeback (Csoportvisszaíró)
a csoportok visszaírásához a választható funkciók a hello beállítás lehetővé teszi toowriteback **Office 365-csoportok** telepített Exchange tooa erdő. Ez az egy csoportot, amely mindig hello felhőben értékűre. Ha a helyszíni Exchange-hez, majd írhat vissza ezen csoportok tooon helyi, a felhasználók egy helyszíni Exchange postaládával küldhet és fogadhat e-mailek ezekből a csoportokból.

További információ az Office 365-csoportokat, és hogyan toouse őket található [Itt](http://aka.ms/O365g).

Az Office 365-csoportok egy jelenik meg a helyszíni terjesztési csoport Active Directory tartományi Szolgáltatásokban. A helyszíni Exchange server Ez az új csoporttípus Exchange 2013-as összesítő frissítés 8 (2015. márciusi jelent) vagy az Exchange 2016 toorecognize kell lennie.

**Hello előzetes megjegyzések**

* hello cím könyv attribútum jelenleg nincs feltöltve a hello előzetes kiadásában. Ez az attribútum nélkül hello csoport nincs a globális Címlista hello látható. Ennek legegyszerűbb módja toopopulate hello toouse hello Exchange PowerShell-parancsmag erre az attribútumra `update-recipient`.
* Hello Exchange-séma csak erdőt a csoportok érvényes cél. Ha nincs Exchange észlelte, majd csoportvisszaírásról nincs lehetséges tooenable.
* Csak egyerdős Exchange-szervezet telepítések jelenleg támogatottak. Ha több mint egy szervezet helyszíni Exchange-, akkor szüksége egy helyszíni GALSync megoldás ezen csoportok tooappear a más erdőkben.
* hello csoport visszaírási szolgáltatása nem kezeli a biztonsági csoportok vagy terjesztési csoportok.

> [!NOTE]
> Egy előfizetés tooAzure AD Premium szükség a csoportok visszaírásához.
> 
>

## <a name="user-writeback"></a>Felhasználók visszaírásához.
> [!IMPORTANT]
> hello felhasználói visszaírási előzetes funkció el lett távolítva a hello 2015. augusztus frissítés tooAzure AD Connect. Ha engedélyezte azt, majd tiltsa le ezt a szolgáltatást.
>
>

## <a name="next-steps"></a>Következő lépések
Továbbra is a [az Azure AD Connect egyéni telepítési](active-directory-aadconnect-get-started-custom.md).

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
