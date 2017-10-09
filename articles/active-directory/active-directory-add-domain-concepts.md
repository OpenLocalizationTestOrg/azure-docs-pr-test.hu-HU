---
title: "az Azure Active Directoryban egyéni tartománynevek aaaConceptual áttekintése |} Microsoft Docs"
description: "Azt ismerteti, hello fogalmi keretrendszere, amely az Azure Active Directoryban, beleértve az egyszeri bejelentkezés összevonási egyéni tartománynevek használatával"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fd0c5def-0da2-43af-81bc-76f4cfe86afd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 0a3454ae6b733a8a13a71925df3cc664063f388e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Az Azure Active Directoryban egyéni tartománynevek elméleti áttekintése
A tartomány neve lehet sok címtárerőforrásokkal fontos azonosítója részeként:

* A felhasználói név vagy e-mail cím egy felhasználó számára
* a csoport hello cím
* hello app ID URI az alkalmazáshoz

Az Azure Active Directory (Azure AD) erőforrás tartalmazhatnak toobe tulajdonában hello directory hello erőforrást tartalmaz, amely már ellenőrizve tartománynevet. Csak egy globális rendszergazda Azure AD-ben végrehajthat tartomány feladatokat.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan toomanage a tartománynevek hello Azure AD felügyeleti központban, lásd: [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).

Az Azure AD-tartománynevek nem globálisan egyedi. Egy egyéni tartománynevet egyszerre csak egy Azure AD-bérlő által is használt. Ha ellenőrizte, hogy az Azure AD-címtár egy tartomány nevét, majd más Azure AD-címtár is győződjön meg arról, vagy használja ugyanazt a tartománynevet.

## <a name="initial-and-custom-domain-names"></a>Kezdeti és egyéni tartománynevek
Minden Azure AD-ben tartománynév, vagy egy kezdeti tartománynevet, vagy egy egyéni tartománynevet.

Minden Azure AD egy kezdeti tartománynevet a hello űrlap contoso.onmicrosoft.com tartalmaz. A harmadik szintű tartománynevet, ebben a példában "contoso.onmicrosoft.com," jött létre, amikor hello directory hozta létre, általában hello directory létrehozó Üdvözöljük a rendszergazdákat. hello kezdeti tartománynevet könyvtár nem módosítható vagy törölhető. hello kezdeti tartománynevet, teljesen működőképes, amíg olyan elsősorban az egyéni tartománynév csak bootstrapping mechanizmusként toobe ellenőrzése.

A legtöbb éles környezetben egy könyvtárat legalább egy ellenőrzött és érvényes egyéni tartománnyal rendelkezik, például "contoso.com", és látható tooend felhasználókat, hogy egyéni tartomány. Egy egyéni tartománynév megadása egy tartomány nevét, amely birtokolt, és használják az adott szervezet, például "contoso.com", például a webhely üzemeltetési célból. Ez a tartománynév nem ismerős tooemployees, mert része hello felhasználó nevét, hogy a vállalati hálózat toohello vagy toosend toosign használja, és e-mailek lekérése.

Az Azure AD által használt, hello egyéni tartománynevet hozzáadott tooyour könyvtárnak kell lennie, és ellenőrzése.

## <a name="verified-and-unverified-domain-names"></a>Ellenőrzött és nem ellenőrzött tartomány nevét
hello kezdeti tartománynevet könyvtár implicit módon értékeli az Azure AD által ellenőrzött. A rendszergazda hozzáad egy egyéni tartomány nevét tooan az Azure AD, ha az eredetileg nem ellenőrzött állapotban van. Az Azure AD nem engedélyezi a könyvtár-erőforrások toouse egy nem ellenőrzött tartomány nevét. Ez biztosítja, hogy csak egy címtárban használhat egy adott tartomány nevét, és, hogy a szervezet hello hello nevét használja ténylegesen tulajdonosa a tartománynevet.

Az Azure AD egy tartománynevet tulajdonjogát keresi, egy adott bejegyzés hello tartomány nevét (DNS) szolgáltatás zónafájlját hello tartománynév ellenőrzi. tooverify tulajdonjogát, a tartomány nevét, egy rendszergazda lekérdezi hello DNS-bejegyzést az Azure AD, hogy az Azure AD keresi, és hozzáadja az adott bejegyzés toohello DNS-zónafájlját hello tartomány nevét. Ehhez a tartományhoz hello regisztrációs hello DNS-zónafájlját tartja fenn. hello lépéseket tooverify hello szóló cikkben jelennek meg a tartomány [hozzáadása egy egyéni tartomány tooyour az Azure Active directory](active-directory-add-domain.md).

A DNS-bejegyzés toohello zónafájlját hello tartománynév hozzáadása más tartományi szolgáltatások, például az e-mailben vagy webtároláshoz nincs hatással.

## <a name="federated-and-managed-domain-names"></a>Összevont és a kezelt tartománynevek
Az Azure AD egy egyéni tartománynevet is lehet konfigurált toogive felhasználók egy összevont bejelentkezési élményt nyújt a helyszíni Active Directory és az Azure AD között. Tartománynév beállítása az összevonáshoz szükség van a frissítések tooprivileged erőforrások az Azure AD és a Windows Server Active Directory tooyour is. Egy összevont tartományt el kell végezni az Azure AD Connect konfigurálása, vagy a PowerShell használatával. Egyéni tartomány összevonását a klasszikus Azure portálon hello nem kezdeményezhető. [Tekintse meg a videót toolearn kapcsolatos felhasználói bejelentkezés az Azure AD Connect AD FS konfigurálása](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Tartományok, amelyek nem összevont felügyelt tartományok is nevezik. kezdeti tartomány hello Azure AD-címtár egy felügyelt tartomány implicit módon történik.

## <a name="primary-domain-names"></a>Elsődleges tartománynevek
hello könyvtár elsődleges tartománynév megadása, amely az előre kijelölt hello alapértelmezés szerinti érték a hello "" tartományrésze hello felhasználónevet, amikor a rendszergazda létrehoz egy új felhasználót a hello hello tartománynév [Azure-portálon](https://portal.azure.com/), vagy egy másik portálon például hello Office 365 felügyeleti portál vagy a Microsoft Intune-portál hello. A könyvtár csak egy elsődleges tartománynév lehet. Egy rendszergazda módosíthatja hello elsődleges tartomány neve toobe bármely ellenőrzött egyéni tartomány, amely nem össze van vonva, és toohello kezdeti tartomány.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Az Azure AD-tartománynevek és más Microsoft Online Services
Az Azure AD egy másik Microsoft Online szolgáltatás például az Exchange Online, SharePoint online-hoz és a Intune használata előtt ellenőrizni kell a tartomány nevét. Ezek a szolgáltatások használatához általában szükség van egy rendszergazda tooadd egy vagy több DNS-bejegyzéseit, amelyek adott toohello szolgáltatás.

Azure-webalkalmazás használ a saját tartomány mechanizmus tooverify tulajdonjogát. Egy tartomány ellenőrizni kell az Azure ad-vel használja akkor is, ha korábban ellenőrzését használható Azure-webalkalmazás egy előfizetésben található, amely támaszkodik, hogy az Azure AD által. Azure-webalkalmazás egy tartomány nevét, amely egy másik könyvtárba, amely biztosítja a hello webalkalmazás hello könyvtárból ellenőrzése után használhatja.

## <a name="managing-domain-names"></a>Tartománynevek kezelése
Tartományi felügyeleti feladatok hello a klasszikus Azure portálon, és a PowerShell hajthatók végre. Számos feladatot hello Azure AD Graph API segítségével kell végrehajtani.

* [Hozzáadásával és az egyéni tartománynév ellenőrzése](active-directory-add-domain.md)
* [A klasszikus Azure portálon hello tartományok kezelése](active-directory-add-manage-domain-names.md)
* [Az Azure AD PowerShell toomanage tartománynevek használatával](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Az Azure AD hello Azure AD Graph API toomanage tartománynevek használatával](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

