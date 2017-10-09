---
title: "az Azure RemoteApp hibrid gyűjtemény aaaHow toocreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate tooyour belső hálózathoz csatlakozó RemoteApp központi telepítését."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 08ea0ce3-3a2c-4ddf-9394-6d75c8030cb1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 3fba29acc676e0af48e995da406f889c532c44c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-hybrid-collection-for-azure-remoteapp"></a>Hogyan toocreate az Azure RemoteApp hibrid gyűjtemény
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Azure RemoteApp-gyűjtemény két fő típusba sorolhatók:

* Felhő: teljesen az Azure-ban található. Választhat toosave minden adat hello felhőben (, egy kizárólag felhőalapú gyűjtemény) vagy tooconnect a gyűjtemény tooa VNET és van az adatok mentése.   
* Hibrid: tartalmazza a virtuális hálózati hozzáférés a helyszíni - ehhez szükséges hello használata az Azure AD és a helyszíni Active Directory-környezetben.

Nem tudható, amely van szüksége? Tekintse meg [gyűjtemény milyen típusú szükséges az Azure RemoteApp](remoteapp-collections.md).

Ez az oktatóanyag bemutatja, hogyan hello egy hibrid gyűjtemény létrehozásának folyamatán. Nyolc lépésből áll:

1. Döntse el, mi [kép](remoteapp-imageoptions.md) toouse a gyűjteményhez. Hozzon létre egy egyéni lemezképet, vagy hello Microsoft rendszerképeket az előfizetéskor valamelyikével.
2. A virtuális hálózat beállítása. Tekintse meg a hello [hálózatok tervezési](remoteapp-planvnet.md) és [méretezési](remoteapp-vnetsizing.md) információkat.
3. Hozzon létre egy gyűjteményt.
4. Csatlakozás a gyűjtemény tooyour helyi tartományhoz.
5. A sablon lemezképének tooyour gyűjtemény hozzáadása.
6. Címtár-szinkronizálás beállítása. Azure követel meg, hogy integrálása az Azure Active Directory által vagy 1) konfigurálása Azure Active Directory szinkronizáló a jelszó-szinkronizálás beállítás hello, vagy 2) konfigurálása Azure Active Directory-szinkronizálás nélkül hello jelszó-szinkronizálás beállítás, de a tartományba, amely összevont tooAD FS. Tekintse meg a hello [konfigurációs adatait az Active Directory RemoteApp](remoteapp-ad.md).
7. A RemoteApp-alkalmazások közzététele.
8. Felhasználói hozzáférés konfigurálása.

**Előkészületek**

Hello gyűjtemény létrehozása előtt toodo hello következőkre lesz szüksége:

* [Regisztráció](https://azure.microsoft.com/services/remoteapp/) az Azure RemoteApp.
* A felhasználói fiók létrehozása az Active Directory toouse hello Azure RemoteApp szolgáltatás fiókja. Korlátozza a hello engedélyeket ehhez a fiókhoz, így akkor is csak tartományhoz gép toohello.
* A helyszíni hálózattal kapcsolatos adatok gyűjtése: IP-cím információkat és a VPN-eszköz részletei.
* Telepítse a hello [Azure PowerShell](/powershell/azure/overview) modul.
* Hello felhasználók toogrant eléréséhez használni kívánt információt gyűjteni. Akkor lesz szüksége hello Azure Active Directory egyszerű felhasználónév (például name@contoso.com) minden felhasználó számára. Győződjön meg arról, hogy hello egyszerű Felhasználónevük megegyezik az Azure AD között és az Active Directory.
* Válassza ki a sablon lemezképe. Egy Azure RemoteApp-sablonlemezkép tartalmaz hello alkalmazások és programok toopublish kívánt a felhasználók számára. Lásd: [Azure RemoteApp-rendszerképekkel kapcsolatos lehetőségek](remoteapp-imageoptions.md) további információt.
* Szeretné, hogy toouse hello Office 365 ProPlus-rendszerképet? Információkat [Itt](remoteapp-officesubscription.md).
* [Az Active Directory konfigurálása a RemoteApp](remoteapp-ad.md).

## <a name="step-1-set-up-your-virtual-network"></a>1. lépés: A virtuális hálózat beállítása
Telepíthet egy meglévő Azure virtuális hálózat használó hibrid gyűjteményt, vagy létrehozhat egy új virtuális hálózat. A virtuális hálózati lehetővé teszi, hogy a felhasználók hozzáférési adatok a helyi hálózaton keresztül RemoteApp távoli erőforrásokhoz. Az Azure virtuális hálózat használatával biztosítja a gyűjteményhez közvetlen hálózati hozzáférés tooother Azure-szolgáltatások és virtuális gépek virtuális hálózati toothat telepítve.

Ellenőrizze, hogy tekintse át a hello [hálózatok tervezési](remoteapp-planvnet.md) és [VNET mérete](remoteapp-vnetsizing.md) információkat a virtuális hálózat létrehozása előtt.

### <a name="create-an-azure-vnet-and-join-it-tooyour-active-directory-deployment"></a>Hozzon létre egy Azure virtuális Hálózatot, és hozzá tooyour Active Directory központi telepítése
Először hozzon létre egy [virtuális hálózati](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Ez történik, a hello **hálózati** hello Azure-portálon lapján. A virtuális hálózati toohello Active Directory központi telepítés szinkronizált tooyour Azure Active Directory-bérlőt kell tooconnect.

Lásd: [hello Azure-portál virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) további információt.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Ellenőrizze, hogy a virtuális hálózat az Azure RemoteApp készen áll
A gyűjtemény létrehozása előtt most Meggyőződünk arról, hogy készen áll-e az új virtuális hálózat. Ezt ellenőrizheti a hello következő tevékenységek végrehajtásával:

1. Hozzon létre egy Azure virtuális gépen belüli hello alhálózati hello a RemoteApp imént létrehozott virtuális hálózat.
2. A távoli asztal tooconnect toohello virtuális gép használja. (Kattintson **csatlakozás**.)
3. Csatlakozzon hozzá toohello megjeleníteni kívánt toouse a RemoteApp azonos Active Directory-környezetben.

Amely működött? A virtuális hálózati és alhálózati Azure RemoteApp készen áll!

További információ az Azure virtuális gépek létrehozása és toothem összeköti a távoli asztal található [Itt](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>2. lépés: Az Azure RemoteApp-gyűjtemény létrehozása
1. A hello [Azure-portálon](http://manage.windowsazure.com)lépjen toohello Azure RemoteApp lap.
2. Kattintson a **új > hozzon létre virtuális hálózaton**.
3. Adja meg a gyűjtemény nevét.
4. Válassza ki a megjeleníteni kívánt toouse – standard és alapszintű hello terv.
5. Válassza ki a virtuális hálózat hello legördülő listára, majd az alhálózat.
6. Válassza ki a toojoin azt tooyour tartomány.
7. Kattintson a **RemoteApp-gyűjtemény létrehozása**.

Az Azure RemoteApp-gyűjtemény létrehozása után kattintson duplán a hello gyűjtemény hello nevét. Amely megjelenik a hello **gyors üzembe helyezés** lap – Ez az, amikor befejezte a hello gyűjtésének konfigurálása.

Adott valamilyen hiba lép fel? Tekintse meg a hello [hibaelhárítási információkat hibrid gyűjtemény](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-toohello-local-domain"></a>3. lépés: Hivatkozás a gyűjtemény toohello helyi tartományban
1. A hello **gyors üzembe helyezés** kattintson **helyi tartományhoz**.
2. Adja hozzá a hello Azure RemoteApp szolgáltatás fiók tooyour helyi Active Directory-tartományhoz. Szüksége lesz a hello tartománynév, a szervezeti egységhez, a szolgáltatási fiók felhasználónevét és a jelszó.
   
    Ez az, ha hello lépéseket követte összegyűjtött információkat felhasználva hello [Active Directory konfigurálása az Azure RemoteApp](remoteapp-ad.md).

## <a name="step-4-link-tooan-azure-remoteapp-image"></a>4. lépés: Hivatkozás tooan Azure RemoteApp képe
Egy Azure RemoteApp-sablonlemezkép, amelyet meg szeretne tooshare felhasználók hello programokat is tartalmaz. Létrehozhat egy új [sablonlemezkép](remoteapp-imageoptions.md) vagy hivatkozás tooan meglévő image (egy már importált vagy feltöltött tooAzure RemoteApp). Az Azure RemoteApp hello tooone is hozzárendelhet [sablonrendszerképek](remoteapp-images.md) tartalmaznak, Office 2013 (kipróbálás) a Programok telepítése és az Office 365.

Új kép hello meg feltölteni, ha szüksége van-e tooenter hello nevét, és válasszon hello hello kép elérési utat. Hello hello varázsló következő lapján lesz című szakaszban láthat egy PowerShell-parancsmagok - példány, és ezen parancsmagok futtatásához rendszergazda jogú Windows PowerShell Rákérdezés tooupload hello megadott lemezkép.

Ha kapcsolódik-tooan meglévő sablon rendszerképet, egyszerűen adja meg, hello lemezképének nevét, helyét és társított Azure-előfizetés.

## <a name="step-5-configure-active-directory-directory-synchronization"></a>5. lépés: Az Active Directory címtár-szinkronizálás konfigurálása
Azure követel meg, hogy integrálása az Azure Active Directory által vagy 1) konfigurálása Azure Active Directory szinkronizáló a jelszó-szinkronizálás beállítás hello, vagy 2) konfigurálása Azure Active Directory-szinkronizálás nélkül hello jelszó-szinkronizálás beállítás, de a tartományba, amely összevont tooAD FS.

Tekintse meg [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – Ez segít a 4 lépéseket címtár-integráció beállítása a következő cikket.

Lásd: [címtár-szinkronizálási terv](http://msdn.microsoft.com//library/azure/hh967642.aspx) tervezési információk és részletes lépéseit.

## <a name="step-6-publish-apps"></a>6. lépés: Az alkalmazások közzététele
Egy Azure RemoteApp alkalmazás hello alkalmazás vagy a program, hogy megadja a tooyour felhasználók. Hello gyűjtemény feltöltött hello sablonlemezkép található. Amikor egy felhasználó hozzáfér egy alkalmazás, úgy tűnik, hogy a helyi környezetben toorun, de valójában fut. az Azure-ban.

Mielőtt a felhasználók úgy férhetnek hozzá az alkalmazásokhoz, toopublish kell őket – ez lehetővé teszi, hogy a felhasználók hozzáférést hello alkalmazások hello távoli asztali ügyfél használatával.

Több alkalmazások tooyour gyűjtemény is közzéteheti. Hello közzétételi lapon kattintson **közzététel** tooadd egy alkalmazást. Vagy közzéteheti a hello **Start** menüjéből hello sablonlemezkép vagy hello alkalmazás hello sablon rendszerképre hello elérési út megadásával. Ha tooadd választhat hello **Start** menüben válassza ki a hello program tooadd. Ha úgy dönt, hogy tooprovide hello elérési toohello app, nevezze el a hello alkalmazást, és hello elérési toowhere hello sablonlemezkép van telepítve.

## <a name="step-7-configure-user-access"></a>7. lépés: A felhasználói hozzáférés konfigurálása
Most, hogy a gyűjtemény hozott létre, szüksége tooadd hello felhasználók megjeleníteni kívánt toobe képes toouse a távoli erőforrásokhoz. hello meg az hello az előfizetéshez tartozó hozzáférési tooneed tooexist az Active Directory-bérlő hello adni használó felhasználók toocreate az Azure RemoteApp-gyűjteményt.

1. Hello gyors kezdés lapon kattintson **felhasználói hozzáférés konfigurálása**.
2. Adja meg a hello munkahelyi fiókjával (az Active Directory) vagy Microsoft-fiók, amelyet toogrant hozzáférését.
   
   **Megjegyzések:**
   
   Győződjön meg arról, hogy használja-e hello  *user@domain.com*  formátumban.
   
   Ha használ Office 365 ProPlus a gyűjteményben, a hello Active Directory identitások a felhasználók számára kell használnia. Ezzel a megoldással licencelési ellenőrzése.
3. Amikor hello felhasználók érvényesíti, kattintson a **mentése**.

## <a name="next-steps"></a>Következő lépések
Ennyi az egész-sikeresen létrehozta és telepítve az Azure RemoteApp hibrid gyűjteményt. következő lépés hello toohave töltse le és hello távoli asztali ügyfél telepítése a felhasználók. Hello Azure RemoteApp gyors kezdés lapon található hello ügyfél hello URL-címe. Ezt követően, hogy a felhasználók a hello ügyfél, és közzétette hello alkalmazások eléréséhez.

### <a name="help-us-help-you"></a>Segítsen nekünk, hogy segítsünk
Tudta, hogy a hozzáadása toorating ebben a cikkben és a hozzászólások írása az alábbi tehet módosítások toohello cikkbe? Valami hiányzik? Valami nem működik? Írtam valami olyat, ami nem egyértelmű? Görgessen fel, és kattintson a **Szerkesztés a Githubon** toomake módosítások - ezek hozzánk kerülnek jóváhagyásra toous, és majd, miután jóváhagytuk őket, itt fognak megjelenni a módosításokat és fejlesztéseket azonnal itt.

