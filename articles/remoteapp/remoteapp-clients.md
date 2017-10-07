---
title: "aaaAccessing az alkalmazások bármely eszközről |} Microsoft Docs"
description: "Ismerje meg, hogy mely ügyfelek Azure RemoteApp támogatja, és hogyan tooaccess az alkalmazásokat."
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
ms.openlocfilehash: 15985b40d870e3155d4132063bf5b9677ff9afed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a>Az alkalmazások elérése az Azure RemoteAppban
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp hello beauties egyike, hogy elérhető alkalmazások bármely eszközén. Még jobban dolgozni az eszközöket és zökkenőmentesen átmenet tooa második eszköz és jobb átvételéhez, ahol abbahagyta. tooget lépések akkor toodownload hello megfelelő ügyfél szükséges az eszközt, és jelentkezzen be toohello szolgáltatás.

Ez a témakör azt áttekinti jelenleg támogatott hello ügyfelek és hogyan toodownload őket előtt I mutatja be az egyes hello ügyfelek tooRemoteApp toosign.

## <a name="supported-clients"></a>Támogatott kliensek
Az alábbi hello lépéseket követve, ha az eszköz fut a következő operációs rendszerek RemoteApp érhető el:

* Windows 10 
* Windows 8.1
* Windows 8
* Windows 7 Service Pack 1
* Windows Phone 8.1
* iOS
* Mac OS X
* Android

 Mi a helyzet a vékony ügyfeleket? a következő Windows Embedded vékony ügyfelek támogatottak hello:

* Windows Embedded Standard 7
* Windows Embedded 8 Standard
* Windows Embedded 8.1 Industry Pro
* Windows 10 IoT Enterprise

## <a name="downloading-hello-client"></a>Hello ügyfél letöltése
Függetlenül attól, milyen platformot használ, a RemoteApp hello található tooaccess kell hello ügyfél [távoli asztali ügyfél letöltési](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) lap.

Hello gombra kattintva másik hivatkozások fog vagy közvetlenül hello ügyfél letöltése vagy megküldeni toohello ügyfélprogram letöltési oldaláról hello app Store-ból a platformhoz való. Hello ügyfél telepítése az üdvözlő képernyőt hello utasításokat követve.

Miután hello ügyfél telepített az eszközön, és akkor indul el, jump toohello megfelelő című szakaszt toolearn hogyan tooRemoteApp az, hogy az ügyfél a toosign.

## <a name="android"></a>Android
A Google Play áruház hello hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztal**.

1. Indító hello app biztosít tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást. az Azure RemoteApp lépései tooget, koppintson a hello Hozzáadás gomb **"" +""** koppintson **Azure RemoteApp**.    
   
     ![Üres csatlakozási központ](./media/remoteapp-clients/Android1.png)
2. Az e-mail cím tooaccess hello szolgáltatást be kell toosign. Koppintson a **Ismerkedés**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Android2.png)
3. Hello következő lapon írja be a **e-mail cím** koppintson **Folytatás**. Ezzel megkezdődik a hello bejelentkezési folyamat az Azure Active Directoryval.
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/Android3.png)
4. Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (korábbi nevén a "Live ID") vagy a szervezeti azonosítóval. Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot. Ha, válassza ki a hello meghívókat megbízik, és koppintson **végzett**.    
   
    ![Meghívókat lap](./media/remoteapp-clients/Android4.png)
5. A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello. Koppintson a hello alkalmazások toostart használja azt.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Android5.png)
6. Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást. toodo tehát koppintson **toofree próbaverzió Ugrás** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Android6.png)
7. Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a>iOS
Hello App store-ból hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztali ügyfél**.

1. Indító hello app biztosít tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást. az Azure RemoteApp lépései tooget, koppintson a hello Hozzáadás gomb **"" +""** koppintson **adja hozzá az Azure RemoteApp**.
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/IOS1.png)
2. Szüksége toosign be az e-mail cím tooaccess hello szolgáltatást, toostart folyamat, írja be a **e-mail cím** koppintson **Folytatás**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/picture1.png)
3. Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot. Ha, válassza ki a hello meghívókat megbízik, és koppintson **végzett**.
   
    ![Meghívókat lap](./media/remoteapp-clients/IOS3.png)
4. A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello. Koppintson a hello alkalmazások toolaunch egyikére, majd újrakezdi a használja azt.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/IOS4.png)
5. Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást. toodo tehát koppintson **toofree próbaverzió Ugrás** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/IOS5.png)
6. Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a>Mac OS X
Hello App store-ból hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **Microsoft távoli asztal**.

1. Indító hello app biztosít tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást. az Azure RemoteApp lépései tooget kattintson hello **Azure RemoteApp** gombra.
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/Mac1.png)
2. Toosign kell be az e-mail cím tooaccess hello szolgáltatás, toostart feldolgozó, koppintson **Ismerkedés**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Mac2.png)
3. Hello következő lapon írja be a **e-mail cím** koppintson **Folytatás**. Ezzel megkezdődik a hello bejelentkezési folyamat során az Azure Active Directoryval.
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/picture2.png)
4. Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot. Ha, válassza ki a megbízható, és zárja be a párbeszédpanelt hello hello meghívókat.
   
    ![Meghívókat lap](./media/remoteapp-clients/Mac4.png)
5. A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello. Kattintson duplán a hello alkalmazások toolaunch majd újrakezdi a használja azt.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Mac5.png)
6. Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást. toodo kattintson **toofree próbaverzió Ugrás** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Mac6.png)
7. Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a>Windows (Windows Phone kivételével az összes támogatott verzió)
hello ügyfél automatikusan elindul telepíti, azonban ha tooaccess kell azt újra később, található az alkalmazáslistában hello néven művelet befejeződése után **Azure RemoteApp**.

1. Hello ügyfél, hello első képernyőn látható elindításához ésőbb üdvözlőlapjának tooAzure RemoteApp. tooproceed, kattintson a **Ismerkedés**.
   
    ![Hello Azure RemoteApp-ügyfélen kezdőlapja](./media/remoteapp-clients/Windows1.png)
2. hello következő elindul a hello bejelentkezési folyamat során az Azure RemoteApp az Azure Active Directoryval. Ez a folyamat ismerős, ha van Microsoft-szolgáltatásokkal a múltbeli hello kell kinéznie. Beírásával indítsa el a **e-mail cím** kattintson **Folytatás**.
   
    ![Első Azure Active Directory-kérés](./media/remoteapp-clients/Windows2.png)
3. Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot. Ha, válassza ki a hello meghívókat megbízik, és kattintson a **végzett**.
   
    ![Hello Azure RemoteApp-ügyfélen meghívókat lapja](./media/remoteapp-clients/Windows3.png)
4. A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello. Kattintson duplán a hello alkalmazások toolaunch majd újrakezdi a használja azt.
   
    ![Kapcsolat Center hello Azure RemoteApp ügyfél](./media/remoteapp-clients/Windows4.png)
5. Ha nem küldött meghívót még, ne aggódjon van, a kezelt! Hozzáférés tooa bemutató gyűjtemény, amellyel tesztelheti, hello szolgáltatás továbbra is kell.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a>Windows Phone 8.1
Windows Phone 8.1 hello áruházból hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztal**.

1. Indító hello alkalmazás számos lehetőséget kínál, közvetlenül tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást. az Azure RemoteApp lépései tooget, koppintson a hello Hozzáadás gomb **"" +""** üdvözlő képernyőt hello alján.
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/WinPhone1.png)
2. A következő koppintson **Azure RemoteApp**.
   
    ![Konfigurációelem-weblap hozzáadása](./media/remoteapp-clients/WinPhone2.png)
3. Toosign kell be az e-mail cím tooaccess hello szolgáltatás, toostart feldolgozó, koppintson **csatlakozás**.
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/WinPhone3.png)
4. Hello következő lapon írja be a **e-mail cím** koppintson **Folytatás**. Ezzel megkezdődik a hello bejelentkezési folyamat során az Azure Active Directoryval.
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/WinPhone4.png)
5. Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot. Ha, válassza ki a hello meghívókat megbízik, és koppintson **mentése**.
   
    ![Meghívókat lap](./media/remoteapp-clients/WinPhone5.png)
6. A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello. Koppintson a hello alkalmazások toolaunch egyikére, majd újrakezdi a használja azt.
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/WinPhone6.png)
7. Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást. Igen, koppintson toodo **Igen** megjelenésekor.
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/WinPhone7.png)
8. Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/WinPhone8.png)

