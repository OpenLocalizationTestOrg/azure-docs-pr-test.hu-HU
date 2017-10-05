---
title: "Az alkalmazások bármely eszközről elérése |} Microsoft Docs"
description: "Ismerje meg, milyen ügyfelek használhatók az Azure RemoteApp és az alkalmazások elérését."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: fb7bd17d-7aa8-43fd-9278-f96e0e9308e4
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 10a5be6251765b59fac92a33120cedcf8091a677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a>Az alkalmazások elérése az Azure RemoteAppban
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp beauties egyike, hogy elérhető alkalmazások bármely eszközén. Még jobban dolgozni egy eszközön, és majd zökkenőmentesen átmenet egy második eszköz és jobb átvételéhez, ahol abbahagyta. Első lépésként töltse le a megfelelő ügyféloldali az eszközhöz, és jelentkezzen be a szolgáltatás kell.

Ebben a témakörben azt áttekinti a jelenleg támogatott ügyfelek és letöltése őket, mielőtt I bemutatják az egyes ügyfelek a RemoteApp bejelentkezni.

## <a name="supported-clients"></a>Támogatott kliensek
Használja az alábbi lépéseket, ha az eszköz fut a következő operációs rendszerek RemoteApp érhető el:

* Windows 10 
* Windows 8.1
* Windows 8
* Windows 7 Service Pack 1
* Windows Phone 8.1
* iOS
* Mac OS X
* Android

 Mi a helyzet a vékony ügyfeleket? A következő Windows Embedded vékony ügyfelek támogatottak:

* Windows Embedded Standard 7
* Windows Embedded 8 Standard
* Windows Embedded 8.1 Industry Pro
* Windows 10 IoT Enterprise

## <a name="downloading-the-client"></a>Az ügyfél letöltése
Függetlenül attól, milyen platformot használ, az ügyfél RemoteApp eléréséhez szükséges található meg a [távoli asztali ügyfél letöltési](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) lap.

A különböző hivatkozásokra kattintva fog vagy közvetlenül az ügyfél letöltése vagy elküld az ügyfélnek letöltése az app Store-ból a platformhoz való lap. A képernyőn megjelenő utasításokat követve telepítse az ügyfelet.

Miután telepítette az ügyfelet az eszközön, és elindítja azt, hogyan RemoteApp, hogy az ügyfél jelentkezzen be az alábbi megfelelő szakaszra ugorhat.

## <a name="android"></a>Android
A Google Play áruházból a Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztal**.

1. Elindítani az alkalmazást számos lehetőséget kínál, egy üres kapcsolat középre, kivéve, ha már használja az alkalmazást. Ismerkedés az Azure RemoteApp, koppintson a Hozzáadás gombra **"" +""** koppintson **Azure RemoteApp**.    
   
     ![Üres csatlakozási központ](./media/remoteapp-clients/Android1.png)
2. Jelentkezzen be az e-mail címét, a szolgáltatás eléréséhez szükség. Koppintson a **Ismerkedés**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Android2.png)
3. A következő oldalon írja be a **e-mail cím** koppintson **Folytatás**. Ezzel megkezdődik a bejelentkezési folyamat az Azure Active Directoryval.
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/Android3.png)
4. Kövesse a képernyőn jelentkezzen be a Microsoft-fiók (korábbi nevén a "Live ID") vagy a szervezeti azonosítóval. Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot. Ha, válassza ki a megbízható, és koppintson a meghívókat **végzett**.    
   
    ![Meghívókat lap](./media/remoteapp-clients/Android4.png)
5. Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center. Koppintson az alkalmazások indításához használja azt.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Android5.png)
6. Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja. Ehhez koppintson **ingyenes Ugrás** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Android6.png)
7. Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a>iOS
Miután telepítette a Microsoft távoli asztal alkalmazás az App store-ból, megtalálhatja az alkalmazáslistában alatt **távoli asztali ügyfél**.

1. Elindítani az alkalmazást számos lehetőséget kínál, egy üres kapcsolat középre, kivéve, ha már használja az alkalmazást. Ismerkedés az Azure RemoteApp, koppintson a Hozzáadás gombra **"" +""** koppintson **adja hozzá az Azure RemoteApp**.
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/IOS1.png)
2. Meg kell bejelentkezni az e-mail címét, a folyamat elindításához a szolgáltatás eléréséhez írja be a **e-mail cím** koppintson **Folytatás**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/picture1.png)
3. Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot. Ha, válassza ki a megbízható, és koppintson a meghívókat **végzett**.
   
    ![Meghívókat lap](./media/remoteapp-clients/IOS3.png)
4. Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center. Koppintson az alkalmazás elindításához és használatba egyikére.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/IOS4.png)
5. Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja. Ehhez koppintson **ingyenes Ugrás** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/IOS5.png)
6. Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a>Mac OS X
Miután telepítette a Microsoft távoli asztal alkalmazás az App store-ból, megtalálhatja az alkalmazáslistában alatt **Microsoft távoli asztal**.

1. Elindítani az alkalmazást számos lehetőséget kínál, egy üres kapcsolat középre, kivéve, ha már használja az alkalmazást. Ismerkedés az Azure RemoteApp, kattintson a **Azure RemoteApp** gombra.
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/Mac1.png)
2. Meg kell bejelentkezni az e-mail címét, a folyamat elindításához a szolgáltatás eléréséhez koppintson **Ismerkedés**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Mac2.png)
3. A következő oldalon írja be a **e-mail cím** koppintson **Folytatás**. Ezzel megkezdődik a bejelentkezési folyamat során az Azure Active Directoryval.
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/picture2.png)
4. Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot. Ha, válassza ki a meghívót, megbízható, és zárja be a párbeszédpanelt.
   
    ![Meghívókat lap](./media/remoteapp-clients/Mac4.png)
5. Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center. Kattintson duplán az alkalmazás elindításához és használatba egyikét.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Mac5.png)
6. Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja. Ehhez kattintson **ingyenes Ugrás** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Mac6.png)
7. Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a>Windows (Windows Phone kivételével az összes támogatott verzió)
Az ügyfél automatikusan elindul a telepítés, azonban ha való hozzáférésre van szüksége később azt található neve alatt az alkalmazáslistában művelet befejeződése után **Azure RemoteApp**.

1. Az ügyfél, az első képernyőn látható elindításához ésőbb Üdvözli az Azure Remoteapphoz. A folytatáshoz kattintson a **Ismerkedés**.
   
    ![Az Azure RemoteApp-ügyfélen kezdőlapja](./media/remoteapp-clients/Windows1.png)
2. A következő oldalon indítja el a bejelentkezési folyamat az Azure RemoteApp az Azure Active Directoryval. Ez a folyamat ismerős, ha van Microsoft-szolgáltatásokkal a múltban kell kinéznie. Beírásával indítsa el a **e-mail cím** kattintson **Folytatás**.
   
    ![Első Azure Active Directory-kérés](./media/remoteapp-clients/Windows2.png)
3. Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot. Ha, válassza ki a megbízható, és kattintson a meghívókat **végzett**.
   
    ![Az Azure RemoteApp-ügyfélen meghívókat lapja](./media/remoteapp-clients/Windows3.png)
4. Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center. Kattintson duplán az alkalmazás elindításához és használatba egyikét.
   
    ![Az Azure RemoteApp ügyfél csatlakozási központ](./media/remoteapp-clients/Windows4.png)
5. Ha nem küldött meghívót még, ne aggódjon van, a kezelt! Ön továbbra is elérheti a bemutató gyűjteményhez, amellyel tesztelheti, a szolgáltatás.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a>Windows Phone 8.1
Miután telepítette a Microsoft távoli asztal alkalmazással a Windows Phone 8.1-tárolóból, megtalálhatja az alkalmazáslistában alatt **távoli asztal**.

1. Elindítani az alkalmazást biztosítható a az Ön közvetlenül egy üres csatlakozási központ, kivéve, ha már használja az alkalmazást. Ismerkedés az Azure RemoteApp, koppintson a Hozzáadás gombra **"" +""** a képernyő alján.
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/WinPhone1.png)
2. A következő koppintson **Azure RemoteApp**.
   
    ![Konfigurációelem-weblap hozzáadása](./media/remoteapp-clients/WinPhone2.png)
3. Meg kell bejelentkezni az e-mail címét, a folyamat elindításához a szolgáltatás eléréséhez koppintson **csatlakozás**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/WinPhone3.png)
4. A következő oldalon írja be a **e-mail cím** koppintson **Folytatás**. Ezzel megkezdődik a bejelentkezési folyamat során az Azure Active Directoryval.
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/WinPhone4.png)
5. Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot. Ha, válassza ki a megbízható, és koppintson a meghívókat **mentése**.
   
    ![Meghívókat lap](./media/remoteapp-clients/WinPhone5.png)
6. Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center. Koppintson az alkalmazás elindításához és használatba egyikére.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/WinPhone6.png)
7. Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja. Ehhez koppintson **Igen** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/WinPhone7.png)
8. Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/WinPhone8.png)

