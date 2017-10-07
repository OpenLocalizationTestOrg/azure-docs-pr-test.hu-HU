---
title: "aaaClassic portál Azure AD alkalmazás Proxy összekötők |} Microsoft Docs"
description: "Magában foglalja az hogyan toocreate és az Azure AD alkalmazásproxy összekötők csoportok kezelése."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 43559b0f4ffc3c7dbbf00901e89ac276d01796e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Külön hálózatok és helyek összekötő csoportokat használnak az alkalmazások közzététele
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [klasszikus Azure portál](active-directory-application-proxy-connectors.md)
>
>

Összekötő csoportok hasznosak több, különböző esetekre, beleértve:

* A több összekapcsolt adatközpontokkal helyek. Ebben az esetben érdemes tookeep mértékű forgalom hello adatközponton belül a lehető mert kereszt-datacenter hivatkozások drága, és a lassú. Minden egyes datacenter tooserve csak hello alkalmazásokhoz hello adatközponton belül összekötők telepítése. Ez a megközelítés minimálisra csökkenti a kereszt-datacenter hivatkozások, és teljesen átlátszó élményt nyújt akkor tooyour felhasználók.
* Elszigetelt hálózatokban, amelyek nincsenek hello fő vállalati hálózat részei a telepített alkalmazások kezelése. Összekötő csoportok dedikált tooinstall összekötők elkülönített hálózatok tooalso elkülönítheti alkalmazások toohello hálózaton használható.
* Összekötő csoportok felhőalapú férhetnek hozzá az infrastruktúra-szolgáltatási a telepített alkalmazások, adja meg a közös szolgáltatás toosecure hello hozzáférés tooall hello alkalmazások. Összekötő csoportok nem további függőség létrehozása a vállalati hálózaton, vagy darabolható hello felhasználói élményt. Összekötők minden felhőbeli adatközpontot is telepíthető, és csak ezen a hálózaton lévő alkalmazások kiszolgálására. Több összekötők tooachieve magas rendelkezésre állású telepítése.
* Többerdős környezetben, amelyben adott összekötők erdőnként telepítése és beállítása tooserve bizonyos alkalmazások támogatása.
* Összekötő csoportok használhatók a vész-helyreállítási helyeken tooeither feladatátvételi észleli, és mivel a biztonsági mentés hello fő hely.
* Összekötő csoportokat is használt tooserve több vállalatot egyetlen bérlőtől kell.

## <a name="prerequisite-create-your-connectors"></a>Előfeltétel: Az összekötő létrehozása
toogroup az összekötők [több összekötők telepítése](active-directory-application-proxy-enable.md), majd a nevet, és csoportosításához. Végül rendelkezik tooassign toospecific alkalmazások őket.

## <a name="step-1-create-connector-groups"></a>1. lépés: Az összekötő csoportok létrehozása
Összekötő csoportot hozhat létre. Összekötő-csoport létrehozása a klasszikus Azure portálon hello valósítható meg.

1. Válassza ki azt a címtárat, és kattintson a **konfigurálása**.  
    ![Alkalmazásproxy, konfigurálja a képernyőfelvétel - kattintson összekötő csoportok kezelése](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. Az alkalmazásproxy, kattintson **összekötő csoportok kezelése** , és hozzon létre egy összekötő csoport hello csoport adjon egy nevet.  
    ![Application proxy connector csoportok képernyőfelvétel - új csoport neve](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-tooyour-groups"></a>2. lépés: Hozzárendelése összekötők tooyour csoportok
Miután hello összekötő csoportokat hoz létre, helyezze át a hello összekötők toohello megfelelő csoportot.

1. A **alkalmazásproxy**, kattintson a **összekötők kezelése**.
2. A **csoport**, jelölje be minden összekötőhöz kívánt hello csoport. Is telhet, mire hello összekötők too10 perc toobecome be aktív hello új csoportba.  
    ![Application proxy összekötők képernyőfelvétel - legördülő menüből válassza csoport](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-tooyour-connector-groups"></a>3. lépés: Hozzárendelni tooyour összekötő csoportok
hello utolsó lépése tooset minden egyes toohello összekötő alkalmazáscsoport azt látja, hogy van.

1. A klasszikus Azure portálon, a címtárban hello válassza ki, tooassign toohello csoportba, majd kattintson az alkalmazás hello **konfigurálása**.
2. A **összekötő csoport**, jelölje be kívánt alkalmazás toouse hello hello csoport. Ez a változás azonnal lesz alkalmazva.  
    ![Application proxy connector csoport képernyőfelvétel - legördülő menüből válassza csoport](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Lásd még:
* [Alkalmazásproxy engedélyezése](active-directory-application-proxy-enable.md)
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Feltételes hozzáférés engedélyezése](active-directory-application-proxy-conditional-access.md)
* [Az alkalmazásproxy problémák elhárítása](active-directory-application-proxy-troubleshoot.md)

Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
