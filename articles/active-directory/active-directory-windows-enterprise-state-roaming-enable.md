---
title: "Vállalati Állapothordozás engedélyezése az Azure Active Directoryban |} Microsoft Docs"
description: "Gyakori kérdések a Windows-eszközökön a vállalati Állapothordozás beállításait. A vállalati Állapothordozás nyújt a felhasználók számára egy egységes élmény a Windows-eszközön, és csökkenti az új eszköz konfigurálásához szükséges időt."
services: active-directory
keywords: "a vállalati állapothordozás, a windows-felhő, a vállalati állapothordozás engedélyezése"
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
ms.openlocfilehash: 77d75f4a647e44f27cd9ba8db5286f1456c3ac66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>A vállalati állapothordozás engedélyezése az Azure Active Directoryban
A vállalati Állapothordozás bármely szervezet a prémium szintű Azure Active Directory (Azure AD) előfizetést érhető el. Az Azure AD-előfizetés. További részletekért tekintse meg a [az Azure AD, termékoldala](https://azure.microsoft.com/services/active-directory).

Ha engedélyezi a vállalati Állapothordozás, a szervezet automatikusan megkapja ingyenes, korlátozott használatú-előfizetéshez tartozó licencet az Azure Rights Management. Ez az ingyenes előfizetés korlátozódik titkosítása és visszafejtése a vállalati beállítások és az alkalmazásadatok szinkronizálása a vállalati Állapothordozás szolgáltatás; a szolgáltatás fizetős Azure Rights Management szolgáltatás összes funkciójáról használandó kell rendelkeznie.

Miután beszerezte az Azure AD prémium előfizetésével, kövesse az alábbi lépéseket a vállalati Állapothordozás engedélyezése:

1. Bejelentkezés a klasszikus Azure portálra.
2. Válassza ki a bal oldali **ACTIVE DIRECTORY**, majd válassza ki a könyvtárban legyen a vállalati Állapothordozás engedélyezése.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Lépjen a **KONFIGURÁLÁSA** a legfelső lap.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4. Görgessen le a lapot, és válasszon **felhasználók előfordulhat, hogy a beállítások és a vállalati alkalmazások adatainak SZINKRONIZÁLÁSA**, és kattintson a **mentése**.
   ![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Egy Windows 10-es eszköz számára a beállítások a vállalati Állapothordozás szolgáltatással az eszközt kell hitelesítenie, az Azure AD identity használatával. Az Azure AD-tartományhoz csatlakoztatott eszközök esetén a felhasználó elsődleges bejelentkezési azonosító az Azure AD identity, így nincs szükség további konfigurációra. Használó eszközök esetében a hagyományos a helyszíni Active Directory, a rendszergazdának kell [a tartományhoz csatlakozó eszközök csatlakoztatása az Azure AD, a Windows 10 észlel](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Szinkronizálási adattárolás
A vállalati Állapothordozás adatok található egy vagy több [Azure-régiók](https://azure.microsoft.com/regions/) , hogy ajánlott igazodik az ország vagy régió érték beállítása az Azure Active Directory-példányban. A vállalati Állapothordozás adatok particionálása három fő földrajzi régió alapján: Észak-Amerikában, az EMEA és APAC. A bérlői vállalati Állapothordozás adatok helyben található, és a földrajzi régió, és nem replikálódnak régiók.  Például az ország vagy régió értéke, az EMEA országok rendelkező ügyfelek, például "Franciaország" vagy "Zambia" fog rendelkezni az egyik, vagy az az Azure-régiók Európában adataikat.  Állítsa be az ország vagy régió értéket az Azure AD-Észak-Amerika országok, például "Az Amerikai Egyesült Államok" vagy "Kanada" lesz az adatokat egy vagy több Azure-régiókban az Egyesült Államok belül futó felhasználókat.  Az ország vagy régió érték beállítása az Azure AD-APAC országok like "Ausztrália" vagy "Új-Zéland" lesz az adatokat egy vagy több Azure-régiókban Ázsia belül futó felhasználókat.  Dél-amerikai országokból és Antarktisz adatokat egy vagy több Azure-régiók belül az Egyesült Államok fog üzemelni.  Az ország vagy régió érték az Azure Active directory létrehozási folyamat részeként van beállítva, és ezt követően nem módosítható. 

Ha további részleteket a adatok tárolási helye van szüksége, adjon fájl igénylést [az Azure támogatási](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Kezelheti a vállalati Állapothordozás
Az Azure AD globális rendszergazdák engedélyezhető és letiltható a vállalati Állapothordozás a klasszikus Azure portálon.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globális rendszergazdák korlátozhatják, meghatározott biztonsági csoportoknak szinkronizálása.

Globális rendszergazdák is megtekintheti egy felhasználó eszköz szinkronizálási állapotáról készült jelentés egy adott felhasználó az Active Directory-példányban kiválasztásával **felhasználók** lista, és kattintson a **eszközök** fülre, majd válassza a nézet **Eszközök beállításainak és a vállalati alkalmazásadatok szinkronizálása**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

## <a name="data-retention"></a>Adatmegőrzés
Keresztül a vállalati Állapothordozás Azure-bA szinkronizált adatok határozatlan ideig megmaradnak, kivéve, ha a kézi delete művelet végrehajtása vagy a szóban forgó adatok határozza meg, hogy elavult. 

**Közvetlen törlése:** az adatok akkor törlődnek, ha egy Azure rendszergazdát törli a felhasználó vagy egy könyvtár, illetve a rendszergazda kérelmek explicit módon, hogy adatokat a rendszer törli.

* **Felhasználó törlése**: a felhasználó Azure AD-ben a törölt adatok barangoló felhasználói fiók megjelölve törlésre, és 90-180 nap közötti törlődnek. 
* **Directory törlésre**: egy teljes könyvtár törlése az Azure ad-ben egy azonnali művelet. A beállítások adatainak társított vele directory megjelölve törlésre, és 90-180 nap közötti törölve lesz. 
* **Törlési kérés**: Ha az Azure AD admin szeretné egy adott felhasználó adatok vagy beállítások adatainak törölje kézzel, a rendszergazda is fájl igénylést [az Azure támogatási](https://azure.microsoft.com/support/). 

**Régi adatok törlése**: adatok, amelyek nem elérése egy év ("a megőrzési időtartam") elavult tekinteni, törlődhet az Azure-ból. A megőrzési időtartam változhat, de csak akkor 90 napon belül. Az elavult adatokat lehet egy meghatározott Windows-alkalmazás beállításai vagy a felhasználó az összes beállítás. Példa:

* Ha egy adott beállításokkal gyűjteménye egyetlen eszköz sem érhető el (például egy alkalmazást távolítja el az eszközről vagy egy csoport például a "Theme" le van tiltva, az összes a felhasználó eszközeinek), majd a gyűjteménynek a megőrzési időszak letelte után elavult fog válni, és előfordulhat, hogy törölve. 
* Ha a felhasználó képes eszközök szinkronizálása ki van kapcsolva, majd a beállítások adatainak egyike fogják elérni, és az adott felhasználó összes beállítások adatainak elavult fog válni, és előfordulhat, hogy a megőrzési időszak letelte után lehet törölni. 
* Ha az Azure Active directory-rendszergazda kikapcsolja a vállalati Állapothordozás a teljes címtár, majd az összes felhasználót, hogy a könyvtár a beállítások szinkronizálása leáll, az összes felhasználó számára minden beállítások adatainak elavult fog válni, és előfordulhat, hogy a megőrzési időszak letelte után lehet törölni. 

**Adat-helyreállítás törölt**: az adatmegőrzési házirend érték nem módosítható. Ha az adatok véglegesen törölve lett, akkor nem fog helyreállítható. Fontos azonban vegye figyelembe, hogy a beállítások adatainak csak az Azure, a végfelhasználói eszközön nem törlődnek. A vállalati Állapothordozás szolgáltatás később újracsatlakozik bármely olyan eszközről, ha a beállítások lesz ismét szinkronizálva és az Azure-ban tárolt.

## <a name="related-topics"></a>Kapcsolódó témakörök
* [A vállalati Állapothordozás áttekintése](active-directory-windows-enterprise-state-roaming-overview.md)
* [Beállítások és adatok hordozása – gyakori kérdések](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Szinkronizálási beállítások házirendet és az MDM-beállításai](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 központi beállításainak ismertetése](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [hibaelhárítással](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
