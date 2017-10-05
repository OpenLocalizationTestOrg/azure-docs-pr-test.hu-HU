---
title: "Hogyan átirányítási USB-eszközöket az Azure Remoteappban? | Microsoft Docs"
description: "Ismerje meg, átirányítás használata az Azure Remoteappban USB-eszközök."
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
ms.openlocfilehash: 3d7165d2c3dafe87b829e588b9e7f2c377552a35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hogyan átirányítási USB-eszközöket az Azure Remoteappban?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Eszközátirányítás lehetővé teszi, hogy a felhasználóknak az számítógép vagy táblagép az alkalmazások az Azure Remoteappban csatlakoztatott USB-eszközök. Például ha megosztott Azure RemoteApp Skype, a felhasználók kell fogja tudni használni az eszköz fényképezőgépek.

Mielőtt továbblépne, továbbá győződjön meg arról, hogy olvassa el az USB átirányítási információk [átirányítással az Azure Remoteappban](remoteapp-redirection.md). Azonban az ajánlott nusbdevicestoredirect:s: * USB Webkamerák az nem fog működni, és nem feltétlenül alkalmas bizonyos USB-nyomtatók vagy többfunkciós USB-eszközök. Úgy lett kialakítva, és a biztonsági okokból az Azure RemoteApp rendszergazda segítségével engedélyezheti az átirányítást GUID eszközosztályt vagy eszköz azonosítója a felhasználók az eszközök használatához.

Bár ez a cikk a webes kamera átirányítási beszél, használhatja a hasonló megközelítést USB-nyomtatók és egyéb nem által az átirányított többfunkciós USB-eszközök átirányítása a **nusbdevicestoredirect:s:*** parancsot.

## <a name="redirection-options-for-usb-devices"></a>Átirányítási USB-eszközök
Az Azure RemoteApp nagyon hasonló mechanizmusok használ irányít át USB-eszközök a távoli asztali szolgáltatások, a rendelkezésre állók közül. Az alapul szolgáló technológiát kiválaszthatja a megfelelő átirányítási módszer egy adott eszközt, mind a legjobb magas szintű és RemoteFX USB-eszközzel átirányítás használata a **usbdevicestoredirect:s:** parancsot. A parancs négy eleme van:

| Feldolgozási sorrendben | Paraméter | Leírás |
| --- | --- | --- |
| 1 |* |Kijelöli az összes eszközről, amelyen a magas szintű átirányítási nem észlelnie. Megjegyzés: úgy lett kialakítva * USB Webkamerák nem megfelelő. |
| {GUID eszközosztályt} |Minden olyan eszköz, az adott eszköz osztály megfelelő választ. | |
| USB\InstanceID |A megadott példány azonosítóját a megadott USB-eszköz kiválasztása | |
| 2 |-USB\Instance azonosítója |Eltávolítja a megadott eszköz a mappaátirányítási beállítások. |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>A GUID eszközosztályt használatával egy USB-eszközt átirányítása
Kétféleképpen az átirányításhoz használt GUID eszközosztályt kereséséhez. 

Az első lehetőség a [rendszer általi eszköz telepítő osztályok elérhető szállítóknak](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Válassza ki azt az osztályhoz, amely a legjobban illik a helyi számítógéphez csatlakoztatott eszköz. Digitális fényképezőgépek Ez lehet egy lemezképkészítő eszköz vagy videó rögzítése eszköz osztály. Hajtsa végre a megfelelő osztály, amely kompatibilis a helyileg csatlakoztatott USB-eszközzel (a mi esetünkben a webkamera) GUID kereséséhez eszközosztályok néhány kísérletezés lesz szüksége.

Fejlettebb módszert, vagy a második lehetőség az adott eszköz osztály GUID található lépések:

1. Az Eszközkezelő megnyitásához, és keresse meg az eszközre, amelyet a rendszer átirányítja, és kattintson a jobb gombbal, majd nyissa meg a tulajdonságait.
   ![Nyissa meg az Eszközkezelőben](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Az a **részletek** adja meg a tulajdonság **osztály Guid**. A megjelenő érték az adott eszköz osztály GUID.
   ![Kamera tulajdonságai](./media/remoteapp-usbredir/ra-classguid.png)
3. Az osztály Guid-érték használatával eszközöket, amelyek megfelelnek az átirányítási.

Példa:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Több eszköz átirányítások ugyanazt a parancsmagot a kombinálhatja. Például: helyi tárterület és egy USB webkamera átirányításához parancsmag néz ki:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Osztály GUID eszközátirányítás beállításakor, amelyek megfelelnek az adott osztály GUID adott gyűjtemény összes eszköz van átirányítva. Például ha a helyi hálózaton több olyan számítógépek, amelyek az ugyanazon USB Webkamerák, összes a Webkamerák egyetlen parancsmag futtathatja.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Az eszköz-példány azonosítója használatával egy USB-eszközt átirányítása
Ha azt szeretné, további részletesebb vezérlést és átirányítási eszközönként vezérlésére kívánja, használhatja a **USB\InstanceID** átirányítási paraméter.

Ez a módszer nagyon nehéz részét keresi a USB eszközazonosítót példány. A számítógép és a megadott USB-eszközzel elérésére lesz szüksége. Ezután kövesse az alábbi lépéseket:

1. Engedélyezze a távoli asztali munkamenetben eszközátirányítás leírtak [Hogyan használhatok eszközök és erőforrások távoli asztali munkamenetben?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Nyissa meg a távoli asztali kapcsolat, és kattintson a **beállítások megjelenítése**.
3. Kattintson a **Mentés másként** RDP-fájlba menti az aktuális kapcsolatbeállításokkal.  
    ![A beállítások mentéséhez RDP-fájlba](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Válassza ki a fájl nevét és helyét, például MyConnection.rdp és a PC\Documents, és mentse a fájlt.
5. Nyissa meg a MyConnection.rdp fájlt egy szövegszerkesztőben, és keresse az eszköz át szeretné irányítani a Példányazonosítója.

Most használjon Példányazonosítója a következő parancsmagot:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Segítsen nekünk, hogy segítsünk
Tudta, hogy a cikk értékelése és alább hozzászólások írása mellett magát a cikket is módosíthatja? Valami hiányzik? Valami nem működik? Írtam valami olyat, ami nem egyértelmű? Görgessen fel, és kattintson a **Szerkesztés a GitHubon** elemre a módosítások végrehajtásához. Ezek hozzánk kerülnek jóváhagyásra, és miután jóváhagytuk őket, itt fognak megjelenni.

