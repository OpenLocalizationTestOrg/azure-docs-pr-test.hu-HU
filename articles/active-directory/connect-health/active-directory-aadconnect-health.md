---
title: "a felhő a helyszíni identitás-infrastruktúra hello aaaMonitor."
description: "Ez a hello Azure AD Connect Health lap, amely leírja, mi az, és ezért használhatja azt."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 84d0b00ec800ba98094343731aa4e7317dfb0c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-hello-cloud"></a>A helyszíni identitás infrastruktúra és a szinkronizálási szolgáltatások hello felhő figyelése
Az Azure Active Directory (Azure AD) Connect Health segít a figyelheti, és betekintést nyerhet a helyszíni identitás-infrastruktúra és hello szinkronizálási szolgáltatások. Lehetővé teszi a megbízható kapcsolatot tooOffice 365 toomaintain és a Microsoft Online Services figyelési lehetőségek körét biztosítva a fontos identitás-összetevők, például az Active Directory összevonási szolgáltatások (AD FS) kiszolgálók, az Azure AD Connect-kiszolgálók (más néven a szinkronizálási motor), mint az Active Directory-tartományvezérlők stb. Lesz hello kulcs adatpontok kapcsolatos összetevők könnyen elérhető, így kaphat a használati és egyéb fontos insights toomake küldjenek döntéseket.

hello információt az hello [Azure AD Connect Health portálon](https://aka.ms/aadconnecthealth). Hello Azure AD Connect Health portálon megtekintheti a riasztásokat, teljesítményfigyelési, használatelemzési információkat és más információkat. Az Azure AD Connect Health lehetővé teszi, hogy a hello egyetlen helyre gyűjti a legfontosabb identitás-összetevők, egy helyen.

![Mi az az Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Az Azure AD Connect Health szolgáltatásai hello növelése érdekében hello portálon keresztül hello fókuszban identitás egyazon irányítópultra biztosít. Ön munkavégzésre egy még inkább robusztus, kifogástalan állapotú és integrált környezetet a a felhasználók tooincrease azok képességét tooget.

## <a name="why-use-azure-ad-connect-health"></a>Miért érdemes az Azure AD Connect Health eszközt használni?
Az Azure AD integrálása a helyszíni címtárak, amikor a felhasználók is hatékonyabban dolgozhasson, mert nincs közös identitás tooaccess mind a felhőalapú és helyszíni erőforrások. Ez az integráció azonban hello kihívást állapotának biztosítása, hogy ebben a környezetben, hogy a felhasználók bármely eszközről megbízható hozzáférése a helyszíni és az olyan hello felhőalapú erőforrások hoz létre. Az Azure AD Connect Health segít a figyelheti, és betekintést nyerhet a helyszíni identitás-infrastruktúra, amely használt tooaccess Office 365 vagy más Azure AD-alkalmazások. Használata éppolyan egyszerű, mintha egy-egy ügynököt telepítene a helyszíni identitás-kiszolgálókra.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[Azure AD Connect Health for AD FS](active-directory-aadconnect-health-adfs.md)
Az AD FS-hez készült Azure AD Connect Health a Windows Server 2008 R2, a Windows Server 2012 és a Windows Server 2012 R2 rendszeren támogatja az AD FS 2.0-s verzióját. Figyelési hello AD FS proxy is támogatja, vagy, hogy hitelesítésre webalkalmazás-proxy kiszolgálók támogatása extranetes elérését. A könnyű és alacsony költségű hello Health-ügynök a telepítést az Azure AD Connect Health AD FS legfontosabb képességei a következő hello biztosítja:

* Figyelés a riasztások tooknow, ha az AD FS és az AD FS-proxykiszolgálók nem megfelelő
* kritikus riasztásokhoz kapcsolódó e-mail értesítések;
* Teljesítményadat-tendenciák, amelyek hasznosak az AD FS kapacitástervezéséhez;
* Használatelemzés az AD FS bejelentkezések a pivots (alkalmazások, felhasználók, hálózati hely stb.), amelyek hasznos toounderstand, hogy az AD FS módjai
* AD FS-jelentések, például a rossz felhasználónév/jelszó párossal legtöbbet próbálkozó 50 felhasználó, az utolsó IP-címükkel együtt

hello következő videó áttekintést nyújt az Azure AD Connect Health AD FS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Azure AD Connect Health szinkronizálási szolgáltatás](active-directory-aadconnect-health-sync.md)
Az Azure AD Connect Health szinkronizálási szolgáltatás figyeli, és hello szinkronizálások között a helyszíni Active Directory és az Azure AD tájékoztatást. Az Azure AD Connect Health for sync legfontosabb képességei a következő hello biztosítja:

* Figyelés a riasztások tooknow, amikor az Azure AD Connect-kiszolgáló, más néven hello szinkronizálási motor állapota nem kifogástalan
* kritikus riasztásokhoz kapcsolódó e-mail értesítések;
* A szinkronizálási műveletek elemzései, beleértve a rájuk vonatkozó késési diagramokat és olyan különböző műveletek tendenciáit, mint például a hozzáadások, a frissítések vagy a törlések;
* Gyors áttekintő információkat a szinkronizálási tulajdonságok és az utolsó sikeres tooAzure AD exportálása
* Az objektumszintű szinkronizációs hibákról való jelentésekhez \(nem szükséges Prémium szintű Microsoft Azure AD\)

hello következő videó áttekintést nyújt az Azure AD Connect Health szinkronizálási szolgáltatás.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-dsactive-directory-aadconnect-health-addsmd"></a>[Azure AD Connect Health for AD DS](active-directory-aadconnect-health-adds.md)
Az Active Directory Domain Serviceshez (AD DS) készült Azure AD Connect Health a Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 vagy Windows Server 2016 rendszeren telepített tartományvezérlők figyelését teszi lehetővé. hello Health-ügynök telepítése lehetővé teszi, hogy Ön toomonitor a helyszíni AD DS-környezet hello felhőből. Az Active Directory tartományi szolgáltatások az Azure AD Connect Health legfontosabb képességei a következő hello biztosítja:

* Figyelési értesítések toodetect tartományvezérlők nem kifogástalan állapotú, és az e-mail értesítések a kritikus riasztások
* hello tartományvezérlők irányítópult, ami hello állapotának gyors áttekintést és működési állapotát a tartományvezérlőket.
* hello replikációs állapota irányítópult hello replikációs szolgáltatáscsomagjaival és tootroubleshooting útmutatók hivatkozásait, amikor a rendszer hibát észlel
* Gyors helyfüggetlen hozzáférés tooperformance adatok diagramjait népszerű teljesítményszámlálók, hibaelhárítási és ellenőrzés céljából szükséges

hello következő videó áttekintést nyújt az Azure AD Connect Health az Active Directory tartományi Szolgáltatásokban.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Az Azure AD Connect Health használatának első lépései
tooget lépések az Azure AD Connect Health, használja hello a következő lépéseket:

1. [Szerezze be a Prémium szintű Azure AD-t](../active-directory-get-started-premium.md) vagy [indítson egy próbaverziót](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Töltse le és telepítse az Azure AD Connect Health-ügynököket](#download-and-install-azure-ad-connect-health-agent) az identitás-kiszolgálókra.
3. Nézet hello Azure AD Connect Health irányítópultját a [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).

> [!NOTE]
> Ne feledje, hogy adatokat az Azure AD Connect Health irányítópultján látja, jóvá kell tooinstall hello Azure AD Connect Health-ügynököket a célkiszolgálókon.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Az Azure AD Connect Health-ügynök letöltése és telepítése
* Győződjön meg arról, hogy Ön [hello megfelelnek](active-directory-aadconnect-health-agent-install.md#requirements) az Azure AD Connect Health.
* Ismerkedés az Azure AD Connect Health for AD FS használatával
    * [Töltse le az Azure AD Connect Health-ügynököt az AD FS szolgáltatáshoz.](http://go.microsoft.com/fwlink/?LinkID=518973)
    * [Hello telepítési utasításokat lásd:](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Ismerkedés az Azure AD Connect Health szinkronizálási szolgáltatás használatával
    * [Töltse le és telepítse az Azure AD Connect legújabb verziójának hello](http://go.microsoft.com/fwlink/?linkid=615771). hello rendszerállapot-ügynöke való szinkronizálás hello Azure AD Connect telepítésének részeként telepíti (1.0.9125.0-s vagy újabb).
* Ismerkedés az Azure AD Connect Health for AD DS használatával
    * [Töltse le az Azure AD Connect Health-ügynököt az AD DS szolgáltatáshoz](http://go.microsoft.com/fwlink/?LinkID=820540).
    * [Hello telepítési utasításokat lásd:](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="azure-ad-connect-health-portal"></a>Az Azure AD Connect Health portál
hello Azure AD Connect Health portálon riasztásokat, teljesítményfigyelési feladatokat és használatelemzési információkat tartalmazza. URL-cím hello https://aka.ms/aadconnecthealth tart az Azure AD Connect Health fő panelje toohello. A panelek az ablakoknak megfelelő funkciót töltenek be. Hello fő paneljén láthatja **gyors üzembe helyezés**, az Azure AD Connect Health, és további konfigurációs lehetőségek belül. Hello lásd a következő képernyőfelvétel és az alábbi képernyőfelvétel a hello rövid magyarázatot. Hello ügynökök telepítése után a hello Állapotfigyelő szolgáltatás automatikusan azonosítja az Azure AD Connect Health által figyelt hello szolgáltatások.

> [!NOTE]
> Licencelési információ: hello [az Azure AD Connect – gyakori kérdések](active-directory-aadconnect-health-faq.md) vagy hello [az Azure AD árazás lap](https://aka.ms/aadpricing).
    
![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health/portal4.png)

* **Gyors üzembe helyezési**: ezt a beállítást, ha hello **gyors üzembe helyezés** panel nyílik meg. Hello Azure AD Connect Health-ügynök kiválasztásával letöltheti **beolvasása eszközök**. Emellett hozzáférhet a dokumentációkhoz, és visszajelzést is küldhet.
* **Active Directory összevonási szolgáltatások**: ezt a lehetőséget az AD FS hello szolgáltatások láthatók, hogy az Azure AD Connect Health által aktuálisan figyelt. Amikor kiválaszt egy példányát, a hello panelt megnyitó adott szolgáltatáspéldány információkat jeleníti meg. Az információk között tulajdonságokat, riasztásokat, megfigyelési adatokat, használatelemzést és egy áttekintést talál. További információk a hello képességekről az [használata az Azure AD Connect Health AD FS-sel](active-directory-aadconnect-health-adfs.md).
* **Azure Active Directory Connect (Sync)** ((Szinkronizálási) Azure Active Directory Connect): Ez a lehetőség az Azure AD Connect Health által aktuálisan monitorozott Azure AD Connect-kiszolgálókat mutatja. Hello bejegyzés kiválasztásakor a hello panel, amely megnyitja az Azure AD Connect-kiszolgálók információkat jeleníti meg. További információk a hello képességekről az [használata Azure AD Connect Health szinkronizálási szolgáltatás](active-directory-aadconnect-health-sync.md).
* **Active Directory tartományi szolgáltatások**: ezt a beállítást jeleníti meg az összes Active Directory tartományi szolgáltatások hello erdők, hogy az Azure AD Connect Health által aktuálisan figyelt. Amikor kiválaszt egy erdőben, a hello panelt megnyitó adott erdőben információkat jeleníti meg. Ezen információk közé tartozik az alapvető információkat, a hello tartományvezérlők irányítópult, a hello replikációs állapota irányítópult, a riasztások és a Figyelés áttekintése. További információk a hello képességekről az [használata Azure AD Connect Health használata az Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md).
* **Konfigurálása**: Ebben a részben ismertetett beállítások tooturn hello következő be- és kikapcsolása:

  - Az automatikus frissítés tooautomatically update hello Azure AD Connect Health agent toohello legújabb verziója: hello Azure AD Connect Health-ügynök legújabb verziója automatikusan frissített toohello lesz, amikor azok elérhetővé válnak. Ez e beállítás alapértelmezés szerint engedélyezve van.
  - Microsoft access tooyour az Azure AD címtár állapotadataihoz engedélyezése hibaelhárítási céllal: Ha ez engedélyezve van, a Microsoft látható hello ugyanazokat az adatokat, amelyek akkor jelennek meg. Ez az információ a hibaelhárítás és a problémakezelés során jelenthet segítséget. A beállítás alapértelmezés szerint le van tiltva.

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)
