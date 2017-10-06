---
title: "Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása |} Microsoft Docs"
description: "Azt ismerteti, hogyan működik az Azure AD Connect szinkronizálási szolgáltatás, és hogyan toocustomize."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: markvi
ms.openlocfilehash: 97e4bd9904b077f2628e5f8dcbaa6f1a76168a12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása
hello Azure Active Directory Connect szinkronizálási szolgáltatások (Azure AD Connect szinkronizálási) Azure AD Connect fő összetevője. Az összes hello művelet kapcsolódó toosynchronize azonosító adatok között a helyszíni környezetben és az Azure AD gondoskodik. Azure AD Connect szinkronizálása hello követő DirSync, az Azure AD Sync és a Forefront Identity Manager rendelkező hello Azure Active Directory Connector konfigurálva.

Ez a témakör az hello az otthoni **az Azure AD Connect szinkronizálási szolgáltatás** (más néven **szinkronizálási motor**) és a listák tooall más témakörök kapcsolódó tooit. Hivatkozások tooAzure AD Connect, lásd: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

helyszíni hello két összetevőből áll: hello szinkronizálási szolgáltatás **az Azure AD Connect szinkronizálási szolgáltatás** összetevő és hello szolgáltatás oldalán az Azure AD nevű **az Azure AD Connect szinkronizálási szolgáltatás**. hello szolgáltatás esetében gyakori DirSync, az Azure AD Sync és az Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Az Azure AD Connect szinkronizálási kapcsolatos témakörök
| Témakör | Azt ismerteti, és ha tooread |
| --- | --- |
| **Az Azure AD Connect-szinkronizálás – alapok** | |
| [Hello architektúra ismertetése](active-directory-aadconnectsync-understanding-architecture.md) |Azok az Ön számára új toohello szinkronizálási motor és hello architektúra és hello kifejezések toolearn szeretné. |
| [Technikai kulcsfogalmak](active-directory-aadconnectsync-technical-concepts.md) |A rövid verzió hello architektúra témakör és röviden ismerteti hello használt kifejezések. |
| [Azure AD Connect-topológiák](active-directory-aadconnect-topologies.md) |Hello különböző topológiák és forgatókönyvek hello szinkronizálási motor által támogatott ismerteti. |
| **Egyéni konfiguráció** | |
| [Futó hello telepítővarázsló újra](active-directory-aadconnectsync-installation-wizard.md) |Ismerteti a beállítások hello Azure AD Connect telepítővarázsló újra futtatásakor elérhető rendelkezik. |
| [Deklaratív kiépítés ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Hello konfigurációs modell neve deklaratív kiépítés ismerteti. |
| [Deklaratív kiépítés kifejezéseinek ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Deklaratív kiépítés használt hello kifejezés nyelv szintaxisának hello ismerteti. |
| [Understanding hello alapértelmezett konfigurációja](active-directory-aadconnectsync-understanding-default-configuration.md) |Hello out-of-box szabályok és hello alapértelmezett konfiguráció ismertetése Is ismerteti, hogyan hello szabályok működik együtt a hello out-of-box forgatókönyvek toowork. |
| [Felhasználók és névjegyek](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Az előző témakörben hello továbbra is fennáll, és bemutatja a felhasználók és névjegyek hello konfigurálása működését együtt, különösen Többerdős környezetben. |
| [Hogyan toomake egy módosítás toohello alapértelmezett konfiguráció](active-directory-aadconnectsync-change-the-configuration.md) |Bemutatja, hogyan hogyan toomake egy közös konfigurációs módosítás tooattribute zajlik. |
| [Hello alapértelmezett konfiguráció módosításának ajánlott eljárásai](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Támogatás korlátai, és adja meg a megfelelő módosításokat toohello out-of-box konfigurációs. |
| [A szűrés konfigurálása](active-directory-aadconnectsync-configure-filtering.md) |Ismerteti, hogyan toolimit objektumok folyamatban van, amely szinkronizálva tooAzure AD és a részletes különböző lehetőségek hello tooconfigure ezeket a beállításokat. |
| **Szolgáltatások és forgatókönyvek** | |
| [Véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Ismerteti, hello *véletlen törlések megakadályozása* szolgáltatás, és hogyan tooconfigure azt. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |Hello beépített ütemezési, amely importálása, szinkronizálását, és az adatok exportálása ismerteti. |
| [Jelszó-szinkronizálás megvalósítása](active-directory-aadconnectsync-implement-password-synchronization.md) |Bemutatja a jelszó-szinkronizálás működését, hogyan tooimplement, és hogyan toooperate és hibáinak elhárításában. |
| [Eszközök visszaírásához.](active-directory-aadconnect-feature-device-writeback.md) |Az Azure AD Connectben eszközvisszaíró működését ismerteti. |
| [Címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md) |Ismerteti, hogyan tooextend hello Azure AD-séma a saját egyéni attribútumokkal. |
| **A szinkronizálási szolgáltatás** | |
| [Az Azure AD Connect szinkronizálási szolgáltatások](active-directory-aadconnectsyncservice-features.md) |Hello szinkronizálási szolgáltatás és az hogyan toochange szinkronizálják az Azure AD-beállításait ismerteti. |
| [Ismétlődő attribútum rugalmasság](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Ismerteti, hogyan tooenable és **userPrincipalName** és **proxyAddresses** ismétlődő attribútum-érték rugalmasság. |
| **Műveletek és a felhasználói felület** | |
| [Szinkronizálási szolgáltatáskezelő](active-directory-aadconnectsync-service-manager-ui.md) |Ismerteti a Synchronization Service Manager felhasználói felületén, beleértve a hello [műveletek](active-directory-aadconnectsync-service-manager-ui-operations.md), [összekötők](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverzumtervező](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), és [Metaverzum-keresés](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) lapokon. |
| [Operatív feladatok és szempontok](active-directory-aadconnectsync-operations.md) |Fontos információk, például a vész-helyreállítási ismerteti. |
| **kézikönyv...** | |
| [Alaphelyzetbe állítja a hello Azure AD-fiók](active-directory-aadconnectsync-howto-azureadaccount.md) |Hogyan tooreset hello hello szolgáltatás fiókjának hitelesítő adatait használja az Azure AD Connect szinkronizálási tooAzure AD tooconnect. |
| **További információk és segédanyagok** | |
| [Portok](active-directory-aadconnect-ports.md) |Listák, amelyek portjai meg kell tooopen hello szinkronizálási motor és a helyszíni címtárak és az Azure AD között. |
| [Attribútumok szinkronizálva tooAzure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) |Felsorolja az összes attribútumok szinkronizálása között a helyszíni AD és az Azure AD. |
| [Függvényreferencia](active-directory-aadconnectsync-functions-reference.md) |Felsorolja az összes elérhető deklaratív kiépítés funkciók. |

## <a name="additional-resources"></a>További források
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

