---
title: "aaaExtending felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási |} Microsoft Docs"
description: "Hogyan a Windows 10-eszközökre az Azure Active Directoryban regisztrálva az Azure AD Join tooget nyújthatnak részletes áttekintést nyújt."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 0cd4942f-7d54-474e-bd12-8e6764b0d42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: db9ae9caeb3951d1fdd1d2477827012fd10ace60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extending-cloud-capabilities-toowindows-10-devices-through-azure-active-directory-join"></a>Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése
## <a name="what-is-azure-active-directory-join"></a>Az Azure Active Directory-csatlakozás bemutatása
Az Azure Active Directory Join (Azure AD Join), amely a vállalat által birtokolt eszköz regisztrálása az Azure Active Directory tooenable központi felügyeleti hello eszköz hello funkciót. Lehetővé teszi, hogy a felhasználók számára, például az alkalmazottak és a diákok tooconnect toohello vállalati felhőalapú Azure Active Directoryn keresztül. Ez lehetővé teszi egyszerűsített Windows központi telepítése és hozzáférési tooorganizational alkalmazásokhoz és erőforrásokhoz bárhonnan, bármilyen Windows-eszköz, mind a vállalat által birtokolt és személyes tulajdonú (eszközök használata BYOD).

Az Azure AD Join vállalatok, amelyek az első vagy felhő-csak felhőalapú – általában kis és közepes méretű vállalatok számára a helyi Windows Server Active Directory infrastruktúrával nem rendelkező számára készült. Hogy az említett, az Azure AD Join is, valamint azt, hogy használják az eszközökön, amelyek nem alkalmas, ez egy hagyományos tartományhoz való csatlakozást (mobil eszközök, például), vagy a felhasználók, akik elsősorban tooaccess Office 365 vagy más SaaS-alkalmazásokhoz az Azure ad-vel integrált nagy méretű szervezeteknek.

Bár a hello hagyományos tartományhoz való csatlakozást továbbra is kínál hello helyszíni legjobb élmény képes a tartományhoz csatlakozó eszközök, a Azure AD Join megfelelő-e az eszközöket, amelyek nem tartományhoz való csatlakozást. Az Azure AD Join alkalmas is hello felhőben felhasználók kezelése. Ilyeneket ahelyett, hogy a mobileszköz-kezelési képességei használatával hagyományos tartományi felügyeleti eszközök például a csoportházirend és a System Center Configuration Manager (SCCM) használatával.

![Vállalati eszközök és személyes eszközöket a helyszíni Active Directory és az Azure AD – áttekintés](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)

## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Miért kell vállalkozások bevezessék az Azure AD Join?
* **Vállalatok számára, amelyek nagy mértékben a felhő hello**: került, vagy pedig csökkentheti a helyszíni kezdjen, és szeretné, hogy további hello felhőben toooperate tooa modell áthelyezni, ha az Azure AD Join sikerült előnyei. Lehet, hogy manuálisan, illetve a helyszíni Active Directory szinkronizálása az Azure AD-fiókok hozott létre. Mindkét esetben van fiókja Azure AD-ben, és használhatja azt toosign tooWindows 10. A felhasználók kapcsolódhatnak a számítógépek tooAzure AD vagy hello out-of-box élmény (OOBE) vagy keresztül hello-beállítások menüjében. Után szeretne csatlakozni, a felhasználó élvezi az egyszeri bejelentkezés (SSO) toocloud erőforrások eléréséhez Office 365-höz hasonló böngészőjük vagy egy Office-alkalmazásban.
* **Oktatási intézmények**: hozzáadunk kapcsolatos gyakran hello forgatókönyvek egyike, oktatási intézmények rendelkezik-e a kétféle felhasználói: faculty és a diákok. Faculty tagok hello szervezet hosszabb távú tagjai minősülnek, így a helyszíni fiókokat hoz létre nekik kívánatos. De a diákok hello szervezet shorter-term tagja, és így kezelheti az Azure ad-ben. Ez azt jelenti, hogy directory méretezési továbbíthatja a helyszínen tárolt helyett toohello felhő. Azt is jelenti, hogy a diákok a saját Azure AD-fiókok tooWindows bejelentkezhet és elérheti tooOffice 365 erőforrásokat, a böngészőjük vagy egy Office-alkalmazásban.
* **Kiskereskedelmi vállalatok**: hallottuk az ügyfelektől kapcsolatban egy másik helyzet a desire toomanage határozza munkavállalók könnyebben.  Ebben az esetben hosszabb távú, a teljes munkaidejű alkalmazottak vannak általában csak hozhatók létre fiókok a helyszíni fiókok, tartományhoz csatlakoztatott számítógépeken. De határozza munkavállalók tagjai shorter-term hello szervezeti, ezért ezt a kívánatos toomanage ezeket ahol licenceket könnyebben átvihetők. A felhasználói fiókokat hozhat létre az Office 365 licencek hello felhőben lehetővé teszi, hogy a hello felhasználók tooget hello előnyei tooWindows és Office-alkalmazások az Azure AD-fiókkal bejelentkezni. Eközben a licenccel rendelkező nagyobb rugalmasságot karbantartása, hogy kilép a.
* **Egyéb üzleti szervezetektől**: annak ellenére, hogy a felhasználók a helyszíni Active Directoryban karbantartása, továbbra is hasznosak rendelkező felhasználók Azure AD-csatlakoztatott lehet. Ennek oka az Azure AD egy egyszerűsített csatlakozó élmény, hatékony kezelése, automatikus mobileszköz-kezelési beléptetés és egyszeri bejelentkezési képesség kínál az Azure AD és a helyszíni erőforrások.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Az Azure AD Join nyújtja a milyen lehetőségek?
Az Azure AD Join hello következő beolvasása:

* **Vállalat által birtokolt eszközök az önálló kiépítése**: Windows 10-felhasználók beállíthatják egy új, légmentes fóliacsomagolásúak eszköz hello out-of-box élmény informatikai használata nélkül.
* **Modern kivitelűek támogatása**: Azure AD Join működik-e az eszközök, amelyek nem rendelkeznek hello hagyományos tartományhoz csatlakoztatás képességeket.  
* **Támogatja a meglévő munkahelyi és iskolai fiókok**: felhasználók már nem kell toocreate és karbantartása a személyes Microsoft-fiók tooget hello a vállalat által kiállított eszközökön, a legjobb élményt, mint a Windows 8. Akkor használja a meglévő munkahelyi fiókok az Azure AD. A legtöbb szervezet számára, ez alapvetően azt jelenti, hogy a felhasználók állítsa be, és jelentkezzen be a hello tooWindows azonos tooaccess Office 365 használata hitelesítő adatokat.
* **Automatikus mobileszköz-kezelési beléptetés**: az eszközök automatikus regisztrálása a mobileszköz-kezelés csatlakozáskor tooAzure AD. Ez a folyamat működik együtt a Microsoft Intune és a partner mobileszköz-kezelési megoldások. Kezelés az Intune-nal történik, amikor a rendszergazdák figyelő/kezelheti az Azure AD-csatlakoztatott eszközök mellett a tartományhoz csatlakoztatott eszközök hello SCCM felügyeleti konzolon.
* **Az egyszeri bejelentkezés toocompany erőforrások**: felhasználók egyszeri bejelentkezésre hello Windows asztali tooapps és erőforrások hello felhőben, például az Office 365 és Azure AD hitelesítésében keresztül üzletialkalmazásokezerélvez[Az azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Vállalati tulajdonú eszközök, amelyek összeillesztett tooAzure AD is rendelkezik SSO tooon helyszíni erőforrások, ha hello eszköz a vállalati hálózaton, és bárhonnan, ha ezeket az erőforrásokat elérhetővé tett tárolókra keresztül hello [az Azure AD-alkalmazásproxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).
* **Az operációs rendszer Állapothordozás**: kisegítő beállítást, webhelyek, Wi-Fi jelszavakat és egyéb beállítások vannak eszközök közötti szinkronizálását. a vállalat által birtokolt személyes Microsoft-fiók nélkül.
* **Vállalati Windows áruház**: hello Windows áruház támogatja az alkalmazás beszerzési és a licencelés az Azure AD-fiókok. A szervezetek mennyiségi licenccel alkalmazásokat is, és hogy azok a szervezetek elérhető toohello felhasználók.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hogyan működik az Azure AD Joint a különböző eszközökön?
| Munkahelyi eszközök (illesztett tooon helyszíni tartomány) | Munkahelyi eszközök (illesztett toohello felhő) | Személyes eszköz |
| --- | --- | --- |
| Felhasználók bejelentkezhetnek a Windows a munkahelyi hitelesítő adatait (mint még ma). |Felhasználó tud egyszerre bejelentkezni a munkahelyi hitelesítő adatait az Azure AD-ban kezelt tooWindows. Ez fontos vállalati eszközök három esetben: <ol><li>hello szervezete nem rendelkezik az Active Directory, a helyszíni (például a kisvállalkozásnak).</li><li>hello szervezet nem hoz létre felhasználói fiókokhoz az Active Directoryban (például a diákok, tanácsadókkal vagy határozza munkavállalók vannak nem hozhatók létre fiókok Active Directory).</li><li>hello szervezet rendelkezik, amely nem lehet illesztett tooan (helyszíni) tartomány, például telefonokon vagy táblagépek egy Mobile Termékváltozat (például egy másodlagos eszköz hozott tooa gyári/kereskedelmi emelet) futtató vállalati eszközök.</li></ol> Az Azure AD Join támogatja a felügyelt és a összevont szervezetek vállalati eszközök csatlakoztatása. |A felhasználók bejelentkeznek a személyes Microsoft-fiók hitelesítő adataival (nincs módosítás) tooWindows. |
| Felhasználó rendelkezik hozzáférési tooroaming beállításokat és hello vállalati Windows áruházban. Ezek a szolgáltatások munkahelyi fiókok dolgozni, és nem igényel személyes Microsoft-fiókkal. Ehhez a szervezetek tooconnect a helyszíni Active Directory tooAzure AD. |A telepítő önkiszolgáló felhasználók teheti meg. Ezek Lépkedjen végig hello kezdőélmény (FRX) keresztül a munkahelyi fiókkal, egy alternatív toohaving informatikai hello eszközök beállításához, bár mindkét módszer támogatja. |Felhasználók könnyedén adhat hozzá a kezelt munkahelyi fiókkal az Active Directoryban vagy az Azure AD. |
| Felhasználók, hogy egyszeri Bejelentkezéses számítógép hello asztali toowork alkalmazások, webhelyek és erőforrások – beleértve a helyi erőforrások és a felhőalkalmazások, amelyeknek az Azure AD használatára a hitelesítéshez. |Eszközök automatikusan regisztrálja az hello vállalati directory (Azure AD) és a mobileszköz-kezelés automatikusan regisztrálva vannak. (Az azure AD Premium-funkció). |A felhasználóknál SSO képes különféle alkalmazásokkal és toowebsites/erőforrások a munkahelyi fiókhoz. |
| A felhasználók hozzáadhatnak a személyes Microsoft-fiókok tooaccess saját egyéni képek és fájlok vállalati adat befolyásolása nélkül. (Központi folytatáshoz munkahelyi fiókkal toowork.) hello Microsoft-fiók lehetővé teszi az egyszeri Bejelentkezést, és már nem fokozza a beállítások központi hello. |Felhasználók teheti a winlogon, önkiszolgáló jelszó-visszaállítási (SSPR) visszaállíthatnák elfelejtett jelszó jelentését. (Az azure AD Premium-funkció). |A felhasználóknál hozzáférés toohello vállalati Windows áruház, hogy szerezzen be és az üzletági alkalmazások használatát a személyes eszközeiket. |

## <a name="additional-information"></a>További információ
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

