---
title: "a vállalati Állapothordozás az Azure Active Directoryban aaaEnable |} Microsoft Docs"
description: "Gyakori kérdések a Windows-eszközökön a vállalati Állapothordozás beállításait. A vállalati Állapothordozás nyújt a felhasználók számára egy egységes élmény a Windows-eszközön, és csökkenti a szükséges konfigurál egy új eszközt hello idő."
services: active-directory
keywords: "a vállalati állapothordozás, a windows-felhő, hogyan tooenable a vállalati állapothordozás"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: f71d66fd-7f9e-45eb-9cfe-5d989870f8a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: c69a9cfa8055f418a3b81c62939885d5bec8a6f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>A vállalati állapothordozás engedélyezése az Azure Active Directoryban
A vállalati Állapothordozás a rendelkezésre álló tooany szervezet a prémium szintű Azure Active Directory (Azure AD) előfizetést. További részletes információt a tooget az Azure AD-előfizetéssel, lásd: hello [az Azure AD, termékoldala](https://azure.microsoft.com/services/active-directory).

Ha engedélyezi a vállalati Állapothordozás, a szervezet automatikusan megkapja egy ingyenes, korlátozott használatú előfizetés tooAzure Rights Management licencet. Ez az ingyenes előfizetés, korlátozott tooencrypting és a vállalati beállítások és a vállalati Állapothordozás szolgáltatás; hello által szinkronizált alkalmazásadatok visszafejtése a szolgáltatás fizetős toouse hello szolgáltatás összes funkciójáról a Azure Rights Management kell rendelkeznie.

Miután beszerezte az Azure AD prémium előfizetésével, kövesse ezeket a vállalati Állapothordozás lépéseket tooenable:

1. Bejelentkezési toohello a klasszikus Azure portálon.
2. Hello bal oldalon válassza ki a **ACTIVE DIRECTORY**, majd válassza ki a kívánt vállalati Állapothordozás tooenable hello könyvtár.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Nyissa meg toohello **KONFIGURÁLÁSA** hello legfelső lap.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Görgessen lefelé hello lap, és válassza ki **felhasználók előfordulhat, hogy a beállítások és a vállalati alkalmazások adatainak SZINKRONIZÁLÁSA**, és kattintson a **mentése**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

A Windows 10 tooroam eszközbeállítások hello szolgáltatást a vállalati Állapothordozás, a hello eszközt kell hitelesítenie, az Azure AD identity használatával. Illesztett tooAzure AD eszközöket hello felhasználó elsődleges bejelentkezési azonosító hello Azure AD identity, így nincs szükség további konfigurációra. A hagyományos a helyszíni Active Directoryt használó eszközök hello rendszergazdának kell [hello tartományhoz csatlakozó eszközök tooAzure AD connect a Windows 10 észlel](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Szinkronizálási adattárolás
A vállalati Állapothordozás adatok található egy vagy több [Azure-régiók](https://azure.microsoft.com/regions/) , hogy a legjobb igazítja hello ország vagy régió érték hello Azure Active Directory-példányban. A vállalati Állapothordozás adatok particionálása három fő földrajzi régió alapján: Észak-Amerikában, az EMEA és APAC. A vállalati Állapothordozás adatok hello bérlő helyileg elhelyezve hello földrajzi régiót, és nem replikálódnak régiók.  Például felhasználók, akik az ország vagy régió érték beállítása tooone EMEA országok, például "Franciaország" vagy "Zambia" lesz az adatokat egy vagy üzemeltetett hello az Azure-régiók Európa belül.  Az ügyfelek, akik az ország vagy régió érték megadása például "Az Amerikai Egyesült Államok" vagy "Kanada" lesz az adatokat az Azure AD tooone Észak-Amerika országok üzemeltetett egy vagy több hello Azure-régiók hello USA belül.  Az ügyfelek, akik az ország vagy régió érték megadása például "Ausztrália" vagy "Új-Zéland" lesz az adatokat az Azure AD tooone APAC országok üzemeltetett egy vagy több hello Azure-régiók Ázsia belül.  Dél-amerikai országokból és Antarktisz adatokat egy vagy több Azure-régiók belül hello USA fog üzemelni.  hello ország vagy régió érték hello Azure Active directory-létrehozási folyamat részeként van beállítva, és ezt követően nem módosítható. 

Ha további részleteket a adatok tárolási helye van szüksége, adjon fájl igénylést [az Azure támogatási](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Kezelheti a vállalati Állapothordozás
Az Azure AD globális rendszergazdák engedélyezhető és letiltható a vállalati Állapothordozás a hello a klasszikus Azure portálon.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globális rendszergazdák korlátozhatják, beállítások szinkronizálási toospecific biztonsági csoportok.

Globális rendszergazdák is megtekintheti egy felhasználó eszköz szinkronizálási állapotáról készült jelentés egy adott felhasználó Active Directory-példányban hello kiválasztásával **felhasználók** lista, és kattintson a **eszközök** fülre, majd válassza a nézet **Eszközök beállításainak és a vállalati alkalmazásadatok szinkronizálása**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Adatmegőrzés
Szinkronizált adatok tooAzure keresztül a vállalati Állapothordozás határozatlan ideig megmarad, kivéve ha egy manuális delete művelet végrehajtása vagy a szóban forgó adatok hello meghatározott toobe elavult. 

**Közvetlen törlése:** hello adatok akkor törlődnek, ha egy Azure rendszergazdát törli a felhasználó vagy egy könyvtár, illetve a rendszergazda kérelmek explicit módon, hogy adatokat toobe törölve-e.

* **Felhasználó törlése**: a felhasználó Azure AD-ben a törölt adatok központi hello felhasználói fiók megjelölve törlésre, és törli too180 90 nap között. 
* **Directory törlésre**: egy teljes könyvtár törlése az Azure ad-ben egy azonnali művelet. Minden hello beállítások adatainak társított vele directory megjelölve törlésre, és törli too180 90 nap között. 
* **Törlési kérés**: hello Azure AD felügyeleti részlege azt szeretné toomanually egy adott felhasználói adatok vagy beállítások adatainak törlése, ha Üdvözöljük a rendszergazdákat is fájl igénylést [az Azure támogatási](https://azure.microsoft.com/support/). 

**Régi adatok törlése**: adatok, amelyek nem elérése egy év ("hello megőrzési időtartam") elavult tekinteni, törlődhet az Azure-ból. hello megőrzési időszak tulajdonos toochange, de csak akkor 90 napon belül. hello elavult adatai pontatlanok lehetnek egy meghatározott Windows-alkalmazás beállításai vagy a felhasználó az összes beállítás. Példa:

* Ha egy adott beállításokkal gyűjteménye egyetlen eszköz sem érhető el (például egy alkalmazásnak az eltávolításakor hello eszközről vagy egy csoport például a "Theme" le van tiltva, az összes a felhasználó eszközeinek), akkor gyűjteménynek hello megőrzési időszak letelte után lesz elavult, és lehet, hogy törölve. 
* Ha a felhasználó képes eszközök szinkronizálása ki van kapcsolva, majd hello beállítások adatainak egyike sem éri el, és az adott felhasználó összes hello beállítások adatainak elavult fog válni, és előfordulhat, hogy hello megőrzési időszak letelte után lehet törölni. 
* Ha hello Azure Active directory-rendszergazda kikapcsolja a vállalati Állapothordozás hello teljes könyvtár, majd az összes felhasználót, hogy a könyvtár leáll beállítások szinkronizálása, az összes felhasználó számára minden beállítások adatainak fog elavult és hello megőrzési időszak letelte után törölhető. 

**Adat-helyreállítás törölt**: hello adatmegőrzési házirend érték nem módosítható. Hello adat véglegesen törölve lett, nem állnak helyreállítható. Fontos azonban, hogy a beállítások adatainak hello toonote csak az törölve lesz a Azure-ban nem hello végfelhasználói eszközökhöz. Bármely olyan eszközről, később újracsatlakozik toohello a vállalati Állapothordozás szolgáltatást, ha hello-beállítások lesz ismét szinkronizálva és az Azure-ban tárolt.

## <a name="related-topics"></a>Kapcsolódó témakörök
* [A vállalati Állapothordozás áttekintése](active-directory-windows-enterprise-state-roaming-overview.md)
* [Beállítások és adatok hordozása – gyakori kérdések](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Szinkronizálási beállítások házirendet és az MDM-beállításai](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 központi beállításainak ismertetése](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [hibaelhárítással](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
