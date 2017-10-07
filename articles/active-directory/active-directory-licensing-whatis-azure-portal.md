---
title: "aaaWhat az Csoportalapú licencelése az Azure Active Directoryban? | Microsoft Docs"
description: "Azure Active Directory biztonságicsoport-alapú licencelési, hogyan működik és ajánlott eljárások leírása"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/29/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 11647de6b76022cd2393751fcafc67ce671aeba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Csoportalapú licencelés alapjai az Azure Active Directoryban

A Microsoft felhőszolgáltatások, például az Office 365, nagyvállalati mobilitási + biztonsági, Dynamics CRM és egyéb hasonló termékek fizetős szükség van a licenceket. A licencek vannak rendelve tooeach felhasználókat, akiknek toothese szolgáltatást. toomanage licencek, a rendszergazdák egyikével hello felügyeleti portálokat (Office vagy Azure) és a PowerShell-parancsmagokat. Azure Active Directory (Azure AD) egy hello alapul szolgáló infrastruktúra, amely támogatja az Identitáskezelés minden Microsoft-szolgáltatásokhoz. Az Azure AD felhasználók hozzárendelés licencállapotukat kapcsolatos információkat tárolja.

Eddig licencek csak rendelhető tesz, a felügyeleti teendők központjaként felügyelet nehéz hello egyéni felhasználói szinten. Például tooadd, vagy távolítsa el felhasználói licencek alapuló szervezeti módosítások szempontjából, például a felhasználók csatlakozni, vagy hagyja hello szervezet vagy egy osztály, a rendszergazda gyakran összetett PowerShell-parancsfájlt kell írnia. Ez a parancsfájl lehetővé teszi egyes hívások toohello felhőalapú szolgáltatás.

tooaddress azokat felkéri, most már az Azure AD magában foglalja a licencelési biztonságicsoport-alapú. Egy vagy több termék licencek tooa csoport rendelhet hozzá. Az Azure AD gondoskodik arról, hogy hello licencek hozzárendelésének tooall hello csoport tagjai. Új tagokat, akik hello csoporttagság hello megfelelő licencek vannak rendelve. Hello csoport kilépő, ezeket a licencek eltávolítását. Ezzel a megoldással hello szükséges a Licenckezelés keresztül PowerShell tooreflect módosítások hello szervezet és a részlegszintű struktúra, felhasználónkénti alapon automatizálásához.

## <a name="features"></a>Szolgáltatások

Az alábbiakban hello fő funkcióinak Csoportalapú Licencelés:

- Licencek hozzárendelhetők legyenek tooany biztonsági csoport az Azure ad-ben. Biztonsági csoportok lehetnek szinkronizálása a helyszíni, az Azure AD Connect használatával. Biztonsági csoportok közvetlenül az Azure ad-ben (más néven csak felhőalapú csoportok), vagy automatikusan keresztül hello Azure AD dinamikus csoport funkciót is létrehozhat.

- A termék licence tooa csoport hozzárendelése esetén hello rendszergazda letilthatja egy vagy több hello termékben service-csomagokról. Általában ez történik, amikor hello szervezet még nem áll készen toostart a termékben szolgáltatást használ. Például hello rendszergazda előfordulhat, hogy rendelje hozzá az Office 365 tooa részleg, de ideiglenesen letilthatja az hello Yammer-szolgáltatás.

- Összes Microsoft-felhőszolgáltatás felhasználói szintű licencelési igénylő támogatottak. Ez magában foglalja az összes Office 365 termékek, nagyvállalati mobilitási + biztonsági és Dynamics CRM-hez.

- Csoportalapú licencelési érhető el jelenleg csak [hello Azure-portálon](https://portal.azure.com). Ha a felhasználók és csoportok kezelése, például a hello Office 365 portál, elsősorban az egyéb felügyeleti portálokat így folytathatja az toodo. De hello Azure portál toomanage licencek csoportok szintjén kell használnia.

- Az Azure AD automatikusan kezeli a csoporttagsági változások licenc a módosításokat. Licenc módosítások általában hatékony a tagság megváltoztatása percen belül.

- Egy felhasználó lehet a megadott licenc házirendek több csoport tagja. A felhasználó is néhány licencek közvetlenül hozzárendelt, kívül minden olyan csoportot. a felhasználói állapot eredő hello hozzárendelt termék és a szolgáltatás licencek.

- Bizonyos esetekben licencek hozzárendelése tooa felhasználói nem végezhető. Például lehet, hogy nincs elegendő elérhető licencek hello bérlői, vagy ütköző szolgáltatásokat lehet, hogy vannak hozzárendelve, hello azonos idő. A rendszergazdák hozzáférés tooinformation azokról a felhasználókról, amely az Azure AD nem tudta teljesen feldolgozni csoport licencek rendelkeznek. Majd érvénybe ezen információk alapján kiigazító intézkedéseket.

- Nyilvános előzetes egy próbaverziós vagy fizetős előfizetést Azure AD alapszintű vagy prémium kiadás hello bérlői toouse Csoportalapú Licenckezelés kell megadni.

## <a name="next-steps"></a>Következő lépések

További információ az egyéb forgatókönyvek Licenckezelés Csoportalapú licenceléssel, toolearn lásd:

* [Ismerkedés az Azure Active Directory-licenc](active-directory-licensing-get-started-azure-portal.md)
* [Az Azure Active Directoryban licencek tooa csoportok átjáróalhálózathoz való hozzárendelése](active-directory-licensing-group-assignment-azure-portal.md)
* [Majd azonosítani és megoldani az Azure Active Directory csoport licenc problémák](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Hogyan toomigrate személy licenccel rendelkező felhasználók toogroup alapú licencelése az Azure Active Directoryban](active-directory-licensing-group-migration-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is](active-directory-licensing-group-advanced.md)
