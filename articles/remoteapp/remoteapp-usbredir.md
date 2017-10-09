---
title: "aaaHow, átirányítja az USB-eszközöket az Azure Remoteappban? | Microsoft Docs"
description: "Megtudhatja, hogyan toouse átirányítása Azure RemoteApp USB-eszközökhöz."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 191d98af-2f5a-4307-9042-aae0e4049f9f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 661b90c0910167d76ac3886b5af7a32d00b3943f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hogyan átirányítási USB-eszközöket az Azure Remoteappban?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Eszközátirányítás lehetővé teszi, hogy a felhasználóknak a hello USB-eszközök csatolt tootheir számítógép vagy táblagép hello alkalmazásokhoz az Azure Remoteappban. Például, ha megosztott Azure RemoteApp Skype, a felhasználóknak hozzá kell toobe képes toouse az eszköz fényképezőgépek.

Mielőtt továbblépne, továbbá győződjön meg arról található hello USB-átirányítása információk olvasása [átirányítással az Azure Remoteappban](remoteapp-redirection.md). Nusbdevicestoredirect:s hello ajánljuk: * USB Webkamerák az nem fog működni, és nem feltétlenül alkalmas bizonyos USB-nyomtatók vagy többfunkciós USB-eszközök. Úgy lett kialakítva, és a biztonsági okokból hello Azure RemoteApp rendszergazda tooenable átirányítási GUID eszközosztályt vagy eszköz azonosítója a felhasználók az eszközök használatához.

Bár ez a cikk a webes kamera átirányítási beszél, használhatja a hasonló megközelítés tooredirect USB-nyomtatók és más, amely nem a rendszer által hello többfunkciós USB-eszközök **nusbdevicestoredirect:s:*** parancsot.

## <a name="redirection-options-for-usb-devices"></a>Átirányítási USB-eszközök
Az Azure RemoteApp nagyon hasonló mechanizmusok használ USB-eszközök átirányítása, a távoli asztali szolgáltatások hello azokat érhető el. hello alapul szolgáló technológia segítségével adja meg az adott eszköz, mindkettő magas szintű legjobb tooget hello hello megfelelő átirányítás módját és a RemoteFX USB-eszközök átirányítása hello segítségével **usbdevicestoredirect:s:** parancsot. Négy eleme van toothis parancs:

| Feldolgozási sorrendben | Paraméter | Leírás |
| --- | --- | --- |
| 1 |* |Kijelöli az összes eszközről, amelyen a magas szintű átirányítási nem észlelnie. Megjegyzés: úgy lett kialakítva * USB Webkamerák nem megfelelő. |
| {GUID eszközosztályt} |Minden olyan eszköz, amelyek megfelelnek a megadott hello kiválasztja osztályát. | |
| USB\InstanceID |Egy megadott példányazonosító. hello megadott USB-eszközt kiválasztása | |
| 2 |-USB\Instance azonosítója |Eltávolítja a megadott eszköz hello hello Mappaátirányítási beállítások. |

## <a name="redirecting-a-usb-device-by-using-hello-device-class-guid"></a>USB-eszközt átirányítása hello eszközosztályt GUID használatával
Két módon toofind hello eszközosztályt GUID azonosítója, az átirányítás használható. 

hello első lehetőség toouse hello [rendszer általi eszköz telepítő osztályok elérhető tooVendors](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Válassza ki a hello osztály, amely a legjobban illik a hello eszköz csatolt toohello helyi számítógépen. Digitális fényképezőgépek Ez lehet egy lemezképkészítő eszköz vagy videó rögzítése eszköz osztály. Szüksége lesz toodo néhány kísérletezés hello eszköz osztályok toofind hello helyes osztály GUID, amely kompatibilis a hello helyileg csatlakoztatott USB-eszközzel (az az eset hello webkamerát).

Egy fejlettebb módszert, vagy hello második lehetőség, nem toofollow ezen lépések toofind hello eszközre osztály-GUID:

1. Hello Eszközkezelő megnyitásához, keresse meg, hogy a rendszer átirányítja, és kattintson a jobb gombbal az hello eszközt, és ezután nyissa meg a hello tulajdonságait.
   ![Nyissa meg a hello Device Manager](./media/remoteapp-usbredir/ra-devicemanager.png)
2. A hello **részletek** lapra, majd hello tulajdonság **osztály Guid**. hello megjelenő érték hello osztály GUID adott típusú eszköz.
   ![Kamera tulajdonságai](./media/remoteapp-usbredir/ra-classguid.png)
3. Hello osztály Guid érték tooredirect eszközöket, amelyek megfelelnek az azt használni.

Példa:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Több eszköz átirányítások a hello kombinálhatja ugyanazt a parancsmagot. Például: tooredirect helyi tárterület és egy USB webes kamera, a parancsmag néz ki:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Osztály GUID eszközátirányítás beállításakor, amelyek megfelelnek az, hogy osztály GUID hello a megadott gyűjtemény minden eszköz van átirányítva. Például, ha nincsenek rendelkező több számítógép hello helyi hálózaton hello azonos USB Webkamerák, futtathatja egy önálló parancsmag tooredirect hello Webkamerák mindegyikét.

## <a name="redirecting-a-usb-device-by-using-hello-device-instance-id"></a>Irányít át egy USB-eszközt a hello eszköz azonosítója
Ha szeretné, hogy részletesebb vezérlést és toocontrol átirányítási eszközönként, használhatja a hello **USB\InstanceID** átirányítási paraméter.

Ez a módszer nagyon nehéz részét hello keresi hello USB eszközazonosítót példány. Meg kell elérési toohello számítógép és hello megadott USB-eszközzel. Ezután kövesse az alábbi lépéseket:

1. Távoli asztali munkamenetben hello eszköz átirányítás engedélyezéséhez, a [Hogyan használhatok eszközök és erőforrások távoli asztali munkamenetben?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Nyissa meg a távoli asztali kapcsolat, és kattintson a **beállítások megjelenítése**.
3. Kattintson a **Mentés másként** toosave hello aktuális kapcsolati beállítások tooan RDP-fájlt.  
    ![Hello beállítások RDP-fájlba mentése](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Válassza ki a fájl nevét és helyét, például MyConnection.rdp és a PC\Documents, és mentse hello fájlt.
5. Megnyitás hello MyConnection.rdp fájlt egy szövegszerkesztőben, és hello Példányazonosító található hello eszköz tooredirect szeretné.

Most használjon hello azonosítója a következő parancsmag hello:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Segítsen nekünk, hogy segítsünk
Tudta, hogy a hozzáadása toorating ebben a cikkben és a hozzászólások írása az alábbi tehet módosítások toohello cikkbe? Valami hiányzik? Valami nem működik? Írtam valami olyat, ami nem egyértelmű? Görgessen fel, és kattintson a **Szerkesztés a Githubon** toomake módosítások - ezek hozzánk kerülnek jóváhagyásra toous, és majd, miután jóváhagytuk őket, itt fognak megjelenni a módosításokat és fejlesztéseket azonnal itt.

