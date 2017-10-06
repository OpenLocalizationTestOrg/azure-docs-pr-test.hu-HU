---
title: "aaaManaging eszközök hello Azure portál – előzetes verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure portál toomanage eszközök."
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
ms.openlocfilehash: a39d14e4ce8bb79f0223a9de40d5f1259a869927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-devices-using-hello-azure-portal---preview"></a>Eszközök kezelése hello Azure portál – előzetes

>[!NOTE]
>Ez a funkció jelenleg nyilvános előzetes verziójához. Készüljön toorevert, vagy távolítsa el a módosításokat. bármely Azure Active Directory (Azure AD) előfizetés nyilvános előzetes hello szolgáltatás érhető el. Amikor hello szolgáltatás általánosan elérhetővé válik, azonban bizonyos aspektusainak hello szolgáltatást szükség lehet az Azure Active Directory premium előfizetéssel.


Az eszköz kezelése az Azure Active Directory (Azure AD) biztosíthatja, hogy a felhasználók a biztonsági és megfelelőségi szabványoknak megfelelő eszközökről érnek el az erőforrásokat. 

Ez a témakör:

- Feltételezi, hogy Ön ismeri a hello [bemutatása toodevice kezelése az Azure Active Directoryban](device-management-introduction.md)

- Biztosít, akkor használja az eszközök felügyeletével kapcsolatos információkkal hello Azure-portálon


toomanage eszközök hello Azure-portálon, kell tooclick **eszközök** a hello **kezelése** hello hello szakasza **Azure Active Directory** panelen.

![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Eszközök beállításainak konfigurálása

toomanage toobe szükségük az eszközök hello Azure-portál használatával, vagy regisztrálva, vagy csatlakoztatott tooAzure AD. A rendszergazdák úgy finomhangolhatja, regisztrálása, illetve az eszközök csatlakoztatása hello eszközbeállítások konfigurálásával hello folyamatán.

![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/22.png)


hello eszköz beállítások panel tooconfigure lehetővé teszi:

- **Felhasználók csatlakozhat egy eszközök tooAzure AD** – a beállítások lehetővé teszi tooselect hello felhasználók, akik csatlakozhatnak eszközök tooAzure AD. hello alapértelmezett érték a **összes**.

- **További Azure AD a helyi rendszergazdák csatlakoztatott eszközök** -hello adott eszközön helyi rendszergazdai jogosultságokkal rendelkező felhasználók is kiválaszthatja. Hozzá felhasználót adnak hozzá toohello *Eszközadminisztrátorok* szerepkört az Azure ad-ben. Az Azure AD globális rendszergazdák és eszköztulajdonosok alapértelmezés szerint kapnak helyi rendszergazdai jogosultságokkal. Ez a beállítás akkor premium edition-funkció termékek, például az Azure AD Premium vagy hello nagyvállalati mobilitási csomag (EMS) keresztül érhető el. 

- **Felhasználók előfordulhat, hogy regisztrálják az eszközeiket az Azure AD** – Ez a beállítás tooallow eszközök toobe regisztrálva az Azure ad-val tooconfigure van szüksége. Ha **nincs**, eszközök tooregister, amelyek nincsenek tartományhoz az Azure AD vagy az Azure AD-tartományhoz hibrid nem engedélyezettek. Az Office 365 mobileszköz-felügyeleti (MDM) vagy a Microsoft Intune beléptetési regisztráció szükséges. Ha konfigurálta a szolgáltatások valamelyikét **minden** van kiválasztva és **NONE** nem érhető el.

- **A többtényezős hitelesítés toojoin eszközök megkövetelése** -dönthet úgy, hogy a felhasználók a szükséges tooprovide egy második hitelesítési tényező toojoin az eszköz tooAzure AD. hello alapértelmezett érték a **nem**. Azt javasoljuk, hogy többtényezős hitelesítés megkövetelése eszközök regisztrálásakor. Mielőtt engedélyezné ezt a szolgáltatást a többtényezős hitelesítést, győződjön meg róla, hogy a többtényezős hitelesítés beállítása hello felhasználókat, hogy regisztrálják az eszközeiket. A különböző Azure multi-factor authentication szolgáltatások további információkért lásd: [Ismerkedés az Azure multi-factor Authentication hitelesítés](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Eszközök maximális számát** – Ez a beállítás lehetővé teszi tooselect hello eszközök maximális számát, amelyeken a felhasználó Azure AD-ben. Ha a felhasználó eléri ezt a kvótát, azok vannak nem tud tooadd további eszközöket amíg egy vagy több olyan hello meglévő eszközt a rendszer eltávolítja. hello eszköz ajánlat csatlakozott az Azure AD vagy az Azure AD ma regisztrált összes eszköz akkor számít. hello alapértelmezett értéke **20**.

- **Felhasználók is szinkronizálásának beállítások és alkalmazások adatainak eszközök** -alapértelmezés szerint ez a beállítás túl**NONE**. Egyes felhasználók vagy csoportok, vagy az összes kijelölésével hello felhasználói beállítások és adatok toosync alkalmazás a Windows 10-eszközön. További tudnivalókért a Windows 10-es sync működéséről.
Ez a beállítás akkor a termékek, például az Azure AD Premium vagy hello nagyvállalati mobilitási csomag (EMS) keresztül érhető el prémium funkció.
 
    ![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Keresse meg az eszközök

A rendszergazdák az Azure-portálon hello két lehetősége van toolocate regisztrálva, és a csatlakoztatott eszközök:

- **Minden eszköz** a hello **kezelése** hello szakasza **eszközök** panel  

    ![Minden eszköz](./media/device-management-azure-portal/41.png)


- **Eszközök** a hello **kezelése** szakasza egy **felhasználói** panel
 
    ![Minden eszköz](./media/device-management-azure-portal/43.png)



Mindkét lehetőség érhet tooa megtekintése, amelyek:


- Lehetővé teszi a toosearch eszközök szűrő hello megjelenített név használatával.

- Lehetővé teszi a regisztrált és a csatlakoztatott eszközök részletes áttekintése

- Lehetővé teszi a tooperform gyakori feladatok
   

![Minden eszköz](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Eszközfelügyeleti feladatokat

A rendszergazdák kezelése hello regisztrálva, vagy csatlakoztatott eszközök. Ez a szakasz a közös eszköz felügyeleti feladatokkal kapcsolatos információt nyújt.


**Az Intune eszközök kezelésére** – Ha egy Intune-rendszergazda, jelölésű eszközöket kezelheti **a Microsoft Intune**. A rendszergazda további eszköz látható 

![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/31.png)


**Engedélyezi / letiltja az Azure AD-eszközök**

tooenable vagy letilthatja az eszköz, szüksége toobe egy globális rendszergazda Azure AD-ben. Ha letilt egy eszközt megakadályozza, hogy az eszköz az Azure AD-erőforrások hozzáférését.  toodisable hello eszköz, vagy kattintson is *...* Kattintson a további részletekért hello eszközre.

 
![Az Intune-ban eszközök kezelése](./media/device-management-azure-portal/33.png)

Ha letilt egy eszközt állapota hello a hello **engedélyezve** oszlop túl**nem**.

![Az eszköz letiltása](./media/device-management-azure-portal/32.png)


**Egy Azure AD-eszköz törlése** -toodelete egy eszközt, toobe egy globális rendszergazda Azure AD-ben van szüksége.  
Egy eszköz törlése:
 
- Megakadályozza, hogy az eszköz az Azure AD-erőforrások elérése 

- Eltávolítja az összes csatolt toohello eszköz adatok például, a BitLocker-kulcsok Windows-eszközök  

- Nem állíthatók helyre tevékenységet jelent, és nem ajánlott, kivéve, ha szükséges

Ha egy eszköz felügyelete egy másik felügyeleti hatóság (pl. Microsoft Intune), ellenőrizze, hogy hello eszközön tárolt adatok törléséig / már nincs az Azure AD hello eszköz törlése előtt.

Kiválaszthatja "..." toodelete eszköz hello, vagy kattintson a további részletekért hello eszközön
 
![Eszköz törlése](./media/device-management-azure-portal/34.png)


**Megtekintése vagy másolása Eszközazonosító** -használhatja egy eszköz azonosítója tooverify hello azonosító eszközadatok hello eszközön vagy a PowerShell használatával a hibaelhárítás során. tooaccess hello másolása, majd hello eszköz.

![Eszközazonosítót megtekintése](./media/device-management-azure-portal/35.png)
  

**Megtekintése vagy a BitLocker-kulcsok másolása** -rendszergazdák, megtekintheti, és másolási hello BitLocker-kulcsok toohelp felhasználók toorecover a titkosított meghajtón. Ezek a kulcsok csak titkosított Windows-eszközökhöz elérhető, és a kulcsaikat az Azure ad-ben tárolt. Ezek a kulcsok másolhatja hello eszköz részleteit való hozzáféréskor.
 
![A BitLocker-kulcsok megtekintése](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Naplók


hello eszköz tevékenységek hello tevékenységi naplóit keresztül érhetők el. Ez magában foglalja az eszközregisztrációs szolgáltatás hello vagy hello felhasználó által indított tevékenységek:

- Eszköz létrehozása és hozzáadása tulajdonosai és felhasználói számára hello eszközön

- Toodevice beállításainak módosítása

- Eszköz műveletek, például a törlés vagy egy eszköz frissítése
 
A belépési pont toohello adatok naplózása van **naplók** a hello **tevékenység** hello szakasza **eszközök* panelen.

![Naplók](./media/device-management-azure-portal/61.png)


Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:

- hello dátum és idő hello előfordulás

- hello célok

- hello kezdeményező / szereplő (ki) tevékenység

- hello tevékenység (Mit)

![Naplók](./media/device-management-azure-portal/63.png)

Testre szabhatja hello listanézet kattintva **oszlopok** hello eszköztáron.
 
![Naplók](./media/device-management-azure-portal/64.png)


lefelé hello toonarrow jelentette, hogy az Ön működik, végezhet hello naplózási adatok használatával a következő mezők hello tooa szintnek:

- Catergory
- Tevékenység erőforrástípusa
- Tevékenység
- Dátumtartomány
- cél
- (Aktor) által kezdeményezett

Ezenkívül toohello szűrők kereshet adott bejegyzéseket.

![Naplók](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Következő lépések

* [Bevezetés toodevice kezelése az Azure Active Directoryban](device-management-introduction.md)



