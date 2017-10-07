---
title: "az Azure Active Directoryban aaaIntroduction toodevice felügyeleti |} Microsoft Docs"
description: "Ismerje meg, hogyan Eszközkezelés segíthetnek erőforrásoknak a környezetben elérő eszközök hello tooget vezérelheti."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: e2fc0a3e8d00dc69cf01db9074e34427e396cfcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodevice-management-in-azure-active-directory"></a>Bevezetés toodevice kezelése az Azure Active Directoryban

Mobileszköz-first, a felhő-első világában Azure Active Directory (Azure AD) lehetővé teszi, hogy egyszeri bejelentkezés toodevices, alkalmazásokhoz és szolgáltatásokhoz bárhonnan. Hello elterjedése eszközök – beleértve a saját eszközök használata (Bring BYOD), az informatikai szakemberek számára két másik célt szemben:

- Hello végfelhasználók toobe hatékony építve, amikor és ahol csak
- Bármikor hello vállalati eszközök védelme

Eszközön a felhasználók vannak fog hozzáférni tooyour vállalati eszközöket. tooprotect a vállalati eszközök informatikai rendszergazdaként szeretné toohave felügyelheti. Ez lehetővé teszi toomake meg arról, hogy a felhasználók a biztonsági és megfelelőségi szabványoknak megfelelő eszközökről érnek el az erőforrásokat. 

Eszközkezelés egyben a hello alapját [eszközalapú feltételes hozzáférési](active-directory-conditional-access-policy-connected-applications.md). Az eszközalapú feltételes hozzáférés biztosítható, hogy hozzáférési tooresources a környezetben csak az lehetséges megbízható eszköz.   

Ez a témakör ismerteti, hogyan működik a kezelés az Azure Active Directoryban.

## <a name="getting-devices-under-hello-control-of-azure-ad"></a>Az Azure AD hello vezérlése alatt eszközök

tooget egy eszközt az Azure AD hello ellenőrzése alatt, két lehetősége van:

- Regisztrálása 
- Csatlakozás

**Regisztrálás** egy eszköz tooAzure AD lehetővé teszi, hogy Ön toomanage egy eszközidentitás. Amikor regisztrál egy eszközt, az Azure AD eszközregisztrációval biztosít hello eszköz, amely használt tooauthenticate hello eszközre, ha egy felhasználó bejelentkezik tooAzure AD identitással. Hello identitás tooenable használja, vagy az eszköz letiltása.

Például a Microsoft Intune mobileszköz-lévő eszközattribútumok megoldás együtt, hello eszközattribútumokon az Azure AD frissítődik hello eszközzel kapcsolatos további információk. Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak. A Microsoft Intune-beli regisztrálásának eszközökön további információkért lásd: eszközök regisztrálása felügyeletre a Intune-ban.

**Csatlakozás** egy eszköz egy bővítmény tooregistering eszköz. Ez azt jelenti, az összes hello előnyt biztosít az eszköz regisztrációja és hozzáadását toothis, hello helyi állapota az eszköz is változik. Hello helyi állapotváltás lehetővé teszi, hogy a felhasználók tooa toosign az eszközt egy szervezeti munkahelyi vagy iskolai fiókot egy személyes fiók helyett.

## <a name="azure-ad-registered-devices"></a>Az Azure AD regisztrált eszközök   

hello Azure AD regisztrált eszközök célja való támogatása hello tooprovide **kapcsolja a saját eszközök használata (BYOD)** forgatókönyv. Ebben a forgatókönyvben egy felhasználó hozzáférhessen a szervezet Azure Active Directory szabályozott erőforrások egy személyes eszközt használ.  

![Az Azure AD regisztrált eszközök](./media/device-management-introduction/03.png)

hello hozzáférést a munkahelyi vagy iskolai fiókkal, hello eszközön beírt alapul.  
Például a Windows 10 lehetővé teszi a felhasználók tooadd egy munkahelyi vagy iskolai fiók tooa személyi számítógép, táblagép vagy telefon.  
Amikor a felhasználó hozzáadta a munkahelyi vagy iskolai fiókkal, hello eszköz regisztrálva az Azure AD és opcionálisan regisztrált rendszerben hello mobileszköz-felügyelet (MDM), amely a szervezet van-e beállítva. A szervezet felhasználók adja hozzá a munkahelyi vagy iskolai kényelmesen fiók tooa személyes eszköz:

- A munkahelyi alkalmazás hello első alkalommal való hozzáféréskor
- Manuálisan keresztül hello **beállítások** hello esetben a Windows 10 menü 

Az Azure AD regisztrált eszközöket a Windows 10, iOS, Android és macOS konfigurálhatja.

## <a name="azure-ad-joined-devices"></a>Az Azure AD csatlakoztatott eszközök

hello az Azure AD csatlakoztatott eszközök célja toosimplify:

- A munkahelyi eszközök a Windows központi telepítése 
- Hozzáférés tooorganizational alkalmazásokat és erőforrásokat bármilyen Windows eszközről

![Az Azure AD regisztrált eszközök](./media/device-management-introduction/02.png)


A felhasználók egy önkiszolgáló felhasználói élmény biztosítása számára az Azure AD hello vezérlése alatt munkahelyi által birtokolt eszközök ezen célok lehet elvégezni.  
**Az Azure AD Join** , amelyek a felhő-első / csak felhőalapú szervezetek számára készült. Ezek rendszerint azok a helyszíni Windows Server Active Directory infrastruktúrával nem rendelkező kis - és közepes méretű vállalkozások. 

Az Azure AD csatlakoztatott eszközök végrehajtási tesz lehetővé a következő előnyöket hello:

- **Egyszeri bejelentkezéses (SSO)** tooyour Azure felügyelt SaaS-alkalmazásokhoz és szolgáltatásokhoz. Nem jelennek meg a felhasználók további hitelesítési kérések munkahelyi erőforrásokhoz való hozzáféréskor. hello SSO funkció még akkor is, ha azok nem csatlakoztatott toohello tartomány elérhető hálózat.

- **Vállalati kompatibilis központi** a felhasználói beállítások csatlakoztatott eszközön. A felhasználóknak nem kell tooconnect egy Microsoft-fiók (például Hotmail) toosee beállításait az eszközön.

- **A vállalati hozzáférés tooWindows áruház** AD fiók használatával. A felhasználók előre kiválasztott hello szervezeti alkalmazások leltárt közül választhatnak.

- **A Windows Hello** biztonságosabb és kényelmesebb hozzáférési toowork erőforrások támogatása.

- **Hozzáférés korlátozása** tooapps csak olyan eszközökön, amelyek megfelelnek a megfelelőségi szabályzatnak.

Az Azure AD join elsődlegesen a helyi Windows Server Active Directory infrastruktúrával nem rendelkező szervezeteknek, amíg is alapértékekkel is helyzetekben használhatja, ahol:

- Nem használhat egy helyszíni tartományhoz való csatlakozást, például ha tooget mobil eszközök, például táblagépekről és mobiltelefonokról vezérlése alatt van szüksége.

- A felhasználók elsősorban tooaccess Office 365 vagy más SaaS-alkalmazásokhoz az Azure ad-vel integrált van szükség.

- Az Azure AD ahelyett, hogy az Active Directory kívánt toomanage a felhasználók egy csoportjánál. Ez is vonatkozik, például tooseasonal munkavállalók, alvállalkozói és a diákok.

- Azt szeretné, hogy tooprovide csatlakozó képességek tooworkers távoli fiókirodák korlátozott a helyszíni infrastruktúrával.

Beállíthatja, hogy csatlakozott az Azure AD-eszközök Windows 10 rendszerű eszközökhöz.


## <a name="hybrid-azure-ad-joined-devices"></a>Az Azure AD hibrid csatlakoztatott eszközök

A több mint egy évtizedben számos szervezet hello tartományhoz csatlakoztatás tootheir a helyszíni Active Directory tooenable használja:

- INFORMATIKAI részlegek toomanage munkahelyi tulajdonban lévő eszközök egy központi helyről.

- Az Active Directory tootheir eszközöket a felhasználók toosign munkahelyi vagy iskolai fiókok. 

Általában egy helyszíni tárhely szervezetek támaszkodnak imaging módszerek tooprovision eszközöket, és gyakran használják **System Center Configuration Manager (SCCM)** vagy **csoportházirend (GP)** toomanage őket.

Ha a környezetben egy helyszíni AD kezdjen, és azt is szeretné előny az Azure Active Directory által biztosított hello képességek, hibrid csatlakozott az Azure AD-eszközöket is létrehozható. Ezek a nyomtatók is, a csatlakoztatott tooyour a helyszíni Active Directory és az Azure Active Directoryban.

![Az Azure AD regisztrált eszközök](./media/device-management-introduction/01.png)


Az Azure AD hibrid csatlakoztatott eszközöket kell használnia, ha:

- Win32 alkalmazásokat telepített toothese eszközt használó NTLM / Kerberos.

- Szüksége van a csoportházirend vagy SCCM / DCM toomanage eszközök.

- Az alkalmazottak toocontinue toouse lemezkép-készítési megoldások tooconfigure eszközök használni szeretne.

Konfigurálhatja a Hybrid Azure AD csatlakoztatott eszközök a Windows 10-es és régebbi eszközök, például a Windows 8 és Windows 7.

## <a name="summary"></a>Összefoglalás

Az eszköz kezelése az Azure ad-ben a következőket teheti: 

- Az Azure AD hello vezérlése alatt eszközükkel hello folyamat leegyszerűsítése érdekében

- Felhőalapú erőforrások tooyour szervezetek könnyen toouse hozzáférést biztosítania a felhasználóknak

A görgetőgomb szabályként kell használnia:

- Az Azure AD regisztrált eszközök személyes eszközök

- Az Azure AD csatlakoztatott eszközök, az eszközök, amelyek nem csatlakoztatott tooan a helyszíni AD 

- Az Azure AD hibrid csatlakoztatott eszközök, az eszközök, amelyek csatlakoztatott tooan a helyszíni AD     




## <a name="next-steps"></a>Következő lépések

- Hogyan toomanage eszköz hello Azure portál, lásd: áttekintést tooget [hello Azure-portál használatával eszközök kezelése](device-management-azure-portal.md)

- További információ az eszközalapú feltételes hozzáférési toolearn lásd [Azure Active Directory eszközalapú feltételes hozzáférési házirendek konfigurálása](active-directory-conditional-access-policy-connected-applications.md).

- toosetup hibrid csatlakozott az Azure AD-eszközök esetén lásd: [hogyan tooconfigure hibrid Azure Active Directoryhoz csatlakoztatott eszközök](device-management-hybrid-azuread-joined-devices-setup.md).


