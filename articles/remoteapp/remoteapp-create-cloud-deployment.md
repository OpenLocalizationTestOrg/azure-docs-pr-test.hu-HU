---
title: "az Azure RemoteApp felhőalapú gyűjtemény aaaHow toocreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a központi telepítés menti az adatokat az Azure RemoteApp hello Azure felhőben."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 4d7c6956-7e4a-4a41-b7f2-7e5832bf01e3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a072ad19d8293016382831d48d0af8e0f5e0d458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-cloud-collection-of-azure-remoteapp"></a>Hogyan toocreate az Azure RemoteApp felhőalapú gyűjtemény
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Kétféle [Azure RemoteApp-gyűjtemény](remoteapp-collections.md) létezik: 

* Felhő: teljesen az Azure-ban található. Választhat toosave minden adat hello felhőben (, egy kizárólag felhőalapú gyűjtemény) vagy tooconnect a gyűjtemény tooa VNET és van az adatok mentése.   
* Hibrid: tartalmazza a virtuális hálózati hozzáférés a helyszíni - ehhez szükséges hello használata az Azure AD és a helyszíni Active Directory-környezetben.

Ez az oktatóanyag bemutatja, hogyan hello felhőalapú gyűjtemény létrehozásának folyamatán. Négy lépésben történik: 

1. Hozzon létre egy Azure RemoteApp-gyűjteményt.
2. Választható lehetőségként konfigurálhatja a címtár-szinkronizálás. Az Azure AD használata + Active Directory rendelkezik toosynchronize felhasználók, partnerek és jelszavak a helyszíni Active Directory tooyour az Azure AD bérlőhöz.
3. Alkalmazások közzététele.
4. Felhasználói hozzáférés konfigurálása.

**Előkészületek**

Hello gyűjtemény létrehozása előtt toodo hello következőkre lesz szüksége:

* [Regisztráció](https://azure.microsoft.com/services/remoteapp/) az Azure RemoteApp. 
* Hello felhasználók toogrant eléréséhez használni kívánt információt gyűjteni. Ez lehet, vagy a Microsoft-fiók adatainak, vagy az Active Directory munkahelyi fiók adatait, a felhasználók számára.
* Ez az eljárás azt feltételezi, hogy vagy folyamatos toouse egy, az előfizetés részeként hello sablonrendszerképek vagy, hogy már rendelkezik feltölteni kívánt toouse hello sablon rendszerképet. Ha egy másik sablonlemezkép tooupload van szüksége, azt hello sablon rendszerképek lapon teheti meg. Kattintson **töltse fel a sablon lemezképe** hello hello varázsló lépéseit kövesse. 
* Szeretné, hogy toouse hello Office 365 ProPlus-rendszerképet? Információkat [Itt](remoteapp-officesubscription.md).
* Szeretné, hogy LOB programok telepítése és tooprovide egyéni alkalmazásokba? Hozzon létre egy új [kép](remoteapp-imageoptions.md) és a felhőalapú gyűjtemény használatát.
* Mérje fel, hogy mikor szükséges tooconnect tooa virtuális hálózat. Ha úgy dönt, hogy a virtuális hálózat tooconnect tooa, győződjön meg arról, megfelel-e hello [irányelvek méretezése](remoteapp-vnetsizing.md) és, hogy az informatikai [kapcsolódhatnak tooRemoteApp](remoteapp-vnet.md). Tekintse meg a hello [hálózatok tervezési cikk ](remoteapp-planvnet.md)további információt.
* Ha egy virtuális Hálózatot használ, döntse el, hogy kívánja-e toojoin azt tooyour helyi Active Directory-tartományhoz.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>1. lépés: - Felhőalapú gyűjtemény létrehozása, vagy egy virtuális hálózat nélkül
Használjon hello alábbi lépéseit túl**csak felhőalapú gyűjtemény létrehozása**:

1. Hello felügyeleti portál lépjen a toohello RemoteApp lapra.
2. Kattintson a **új > Gyorslétrehozás**.
3. Adja meg a gyűjtemény nevét, majd válassza ki a régiót.
4. Válassza ki a megjeleníteni kívánt toouse – standard és alapszintű hello terv.
5. Válassza ki a sablon toouse hello ehhez a gyűjteményhez. 
   
    **Tipp:** RemoteApp az előfizetés részeként elérhető [sablonrendszerképek](remoteapp-images.md) , amelyik tartalmazza az Office 365 vagy Office 2013 (kipróbálás) a programok, egyes közzétett (például a Word) és mások toopublish kész. Is létrehozhat egy új [kép](remoteapp-imageoptions.md) és a felhőalapú gyűjtemény használatát.
6. Kattintson a **RemoteApp-gyűjtemény létrehozása**.
   
    **Fontos:** is eltarthat, too30 perc tooprovision a gyűjteményben.

Az Azure RemoteApp-gyűjtemény létrehozása után kattintson duplán a hello gyűjtemény hello nevét. Amely megjelenik a hello **gyors üzembe helyezés** lap – Ez az, amikor befejezte a hello gyűjtésének konfigurálása.

Használjon hello alábbi lépéseit toocreate egy **felhő + VNET gyűjtemény**:

1. Hello felügyeleti portál lépjen a toohello Azure RemoteApp lapra.
2. Kattintson a **új** > **hozzon létre virtuális hálózaton**.
3. Adja meg a gyűjtemény nevét.
4. Válassza ki a megjeleníteni kívánt toouse – standard és alapszintű hello terv.
5. Válassza ki a virtuális hálózat már létrehozott hello. Nem tudja, hogyan toodo, amely? Most hello lépésekre a hello [hibrid](remoteapp-create-hybrid-deployment.md) témakör.
6. Döntse el toojoin a gyűjtemény tooyour tartomány. Ha igen, szüksége lesz toouse AD Connect toointegrate az Azure AD és az Active Directory-környezetbe. Amely is tartalmazza az alábbi **2. lépés**.
7. Kattintson a **RemoteApp-gyűjtemény létrehozása**.

## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>2. lépés: (Opcionális) Active Directory címtár-szinkronizálás konfigurálása
Ha azt szeretné, hogy toouse Active Directory, Azure RemoteApp címtár-szinkronizálás Azure Active Directory és a helyszíni Active Directory toosynchronize felhasználók, partnerek és jelszavak tooyour Azure Active Directory-bérlő között van szükség. Lásd: [Active Directory konfigurálása az Azure RemoteApp](remoteapp-ad.md) tervezési információkat. Is elvégezheti közvetlenül túl[AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) információt.

## <a name="step-3-publish-apps"></a>3. lépés: Az alkalmazások közzététele
Egy Azure RemoteApp alkalmazás hello alkalmazás vagy a program, hogy megadja a tooyour felhasználók. Hello gyűjtemény feltöltött hello sablonlemezkép található. Amikor egy felhasználó egy alkalmazást, a hello app hozzáfér-e a helyi környezetben toorun megjelenik, de valóban egy virtuális gép az Azure-ban futó. 

Mielőtt a felhasználók úgy férhetnek hozzá az alkalmazásokhoz, toopublish kell őket – alkalmazások megadható, hogy a felhasználók férhetnek hozzá a hello alkalmazások közzététele hello távoli asztali ügyfél.

Közzéteheti több alkalmazások tooyour Azure RemoteApp-gyűjteményt. Hello közzétételi lapon kattintson **közzététel** tooadd programot. Vagy közzéteheti a hello **Start** menüjéből hello sablonlemezkép vagy hello alkalmazás hello sablon rendszerképre hello elérési út megadásával. Ha tooadd választhat hello **Start** menüben válassza ki a hello app toopublish. Ha úgy dönt, hogy tooprovide hello elérési toohello app, nevezze el a hello alkalmazást, és hello elérési toowhere hello sablonlemezkép van telepítve.

## <a name="step-4-configure-user-access"></a>4. lépés: A felhasználói hozzáférés konfigurálása
Most, hogy a gyűjtemény hozott létre, szüksége tooadd hello felhasználók megjeleníteni kívánt toobe képes toouse a távoli erőforrásokhoz. Active Directory használata, ha hello meg az hello az előfizetéshez tartozó hozzáférési tooneed tooexist az Active Directory-bérlő hello adni használó felhasználók toocreate ebben a gyűjteményben.

1. Hello gyors kezdés lapon kattintson **felhasználói hozzáférés konfigurálása**. 
2. Adja meg a hello munkahelyi fiókjával (az Active Directory) vagy Microsoft-fiók, amelyet toogrant hozzáférését.
   
   **Megjegyzések:** 
   
   Győződjön meg arról, hogy használja-e hello  *user@domain.com*  formátumban.
   
   Ha használ Office 365 ProPlus a gyűjteményben, a hello Active Directory identitások a felhasználók számára kell használnia. Ezzel a megoldással licencelési ellenőrzése. 
3. Hello felhasználók ellenőrzését követően kattintson **mentése**.

## <a name="next-steps"></a>Következő lépések
Ennyi az egész-sikeresen létrehozta és telepítve az Azure RemoteApp felhőalapú gyűjtemény. következő lépés hello toohave töltse le és hello távoli asztali ügyfél telepítése a felhasználók. Hello Azure RemoteApp gyors kezdés lapon található hello ügyfél hello URL-címe. Ezt követően, hogy a felhasználók a hello ügyfél, és közzétette hello alkalmazások eléréséhez.

### <a name="help-us-help-you"></a>Segítsen nekünk, hogy segítsünk
Tudta, hogy a hozzáadása toorating ebben a cikkben és a hozzászólások írása az alábbi tehet módosítások toohello cikkbe? Valami hiányzik? Valami nem működik? Írtam valami olyat, ami nem egyértelmű? Görgessen fel, és kattintson a **Szerkesztés a Githubon** toomake módosítások - ezek hozzánk kerülnek jóváhagyásra toous, és majd, miután jóváhagytuk őket, itt fognak megjelenni a módosításokat és fejlesztéseket azonnal itt.

