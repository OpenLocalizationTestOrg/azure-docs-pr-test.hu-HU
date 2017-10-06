---
title: "aaaAzure AD Connect szinkronizálási szolgáltatás árnyékmásolat attribútumok |} Microsoft Docs"
description: "Ismerteti az árnyékmásolat attribútumok működése az Azure AD Connect szinkronizálási szolgáltatást."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1b8665e7488c6078b655f8a3e35519145bacd898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Az Azure AD Connect szinkronizálási szolgáltatás árnyékmásolat attribútumok
A legtöbb attribútumokat hello azonos módon az Azure AD mivel ezek a helyszíni Active Directoryban. De néhány attribútum rendelkezik néhány különleges kezelést és az Azure AD hello attribútumérték eltérhet az Azure AD Connect szinkronizálása.

## <a name="introducing-shadow-attributes"></a>Árnyékmásolat attribútumok bemutatása
Egyes attribútumok két felelősséget rendelkezik az Azure ad-ben. Hello helyszíni érték és a számított érték tárolja. A további attribútumok árnyékmásolat attribútumok nevezzük. Ez a viselkedés megtapasztalhatja hello két leggyakoribb attribútumok **userPrincipalName** és **proxyAddress**. Attribútumértékek hello változás történik, ha ezek az attribútumok nem ellenőrzött tartományok jelölő értékek vannak. De hello szinkronizálási motor a csatlakozás hello értéket olvasó hello árnyékmásolat attribútumban, a szempontjából, hello attribútum visszaigazolását Azure ad.

Nem található hello árnyékmásolat attribútumok hello Azure portál vagy a PowerShell használatával. De ismertetése hello koncepció segít meg tootroubleshoot bizonyos esetekben ha hello attribútum különböző érték is a helyszíni és hello felhőben.

toobetter hello viselkedésének megértése, nézze meg ebben a példában a Fabrikam:  
![Tartományok](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
A helyszíni Active Directoryban több egyszerű Felhasználónévi utótagot rendelkeznek, de azok csak ellenőrzését egy.

### <a name="userprincipalname"></a>UserPrincipalName
A felhasználó rendelkezik-e a következő attribútum értékei nem ellenőrzött tartomány hello:

| Attribútum | Érték |
| --- | --- |
| a helyszíni userPrincipalName | lee.sperry@fabrikam.com |
| Az Azure AD-shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Az Azure AD userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

hello userPrincipalName attribútum értéke hello megjelenik, amikor a PowerShell használatával.

Mivel a hello valós helyszíni attribútumérték rendszer az Azure AD, ha ellenőrizte, fabrikam.com tartomány hello, az Azure AD hello userPrincipalName attribútum hello shadowUserPrincipalName hello értéket frissíti. Nincs toosynchronize az Azure AD Connect ezen értékek toobe frissítése a módosításokat.

### <a name="proxyaddresses"></a>proxyAddresses
hello azonos folyamata csak beleértve az ellenőrzött tartományok is következik be, a proxyAddresses, de néhány további logikát. az ellenőrzött tartományok hello ellenőrzése csak a postaláda felhasználóknak zavartalan. Egy levelezési felhasználó vagy az ügyfél felel meg a felhasználó Exchange-szervezet egy másik, és proxyAddresses toothese objektumok minden olyan értéket adhat hozzá.

Postaláda-felhasználónak, a helyszíni vagy az Exchange Online esetén csak az ellenőrzött tartományok értékek jelennek meg. Az nézhet ki:

| Attribútum | Érték |
| --- | --- |
| a helyszíni proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

Ebben az esetben  **smtp:abbie.spencer@fabrikam.com**  el lett távolítva, mert az adott tartomány nincs ellenőrizve. De adott Exchange  **SIP:abbie.spencer@fabrikamonline.com** . Fabrikam nem használt Lync/Skype a helyszínen, de az Azure AD és az Exchange Online előkészítése.

Ez a módszer a proxyAddresses hivatkozott tooas **ProxyCalc**. ProxyCalc minden változás, a felhasználó meghívása során:

- hello felhasználói azonosítót, amely tartalmazza az Exchange Online, még akkor is, ha hello felhasználó nem rendelkezik licenccel az Exchange service-csomag. Például ha hello felhasználó hozzá van rendelve hello Office E3 SKU, de csak SharePoint online-hoz lett rendelve. Ez igaz, akkor is, ha a postaláda továbbra is a helyszíni.
- hello attribútum msExchRecipientTypeDetails értéke.
- A módosítás tooproxyAddresses vagy a userPrincipalName elvégezte.

ProxyCalc is igénybe vehet néhány alkalommal tooprocess módosítását a felhasználó, ezért nem szinkron hello Azure AD Connect exportálási folyamat során.

> [!NOTE]
> hello ProxyCalc logika van néhány további eszközöket nem ebben a témakörben leírt speciális forgatókönyvek esetén. Ez a témakör kerül meg toounderstand hello viselkedését, és nem az összes belső logikai dokumentum.

### <a name="quarantined-attribute-values"></a>Karanténba helyezett attribútumértékek
Árnyékmásolat attribútumok is használják, amikor ismétlődő attribútum-érték. További információkért lásd: [ismétlődő attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Lásd még:
* [Az Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
