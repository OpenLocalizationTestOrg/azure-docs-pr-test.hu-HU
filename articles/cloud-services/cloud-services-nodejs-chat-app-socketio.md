---
title: "Socket.io segítségével aaaNode.js alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse socket.io node.js-alkalmazásokban az Azure-platformon futó."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 47c6c4a748938959315b880340f41f31faab4ea9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>A Node.js csevegőalkalmazás Socket.IO létrehozása az Azure-Felhőszolgáltatás-kiszolgálón
Socket.IO biztosít a valós idejű között a node.js-kiszolgáló és az ügyfelek közötti kommunikáció. Ez az oktatóanyag végigvezeti egy szoftvercsatorna üzemeltetéséhez. IO Csevegés alkalmazás Azure-alapú. A Socket.IO további információkért lásd: <http://socket.io/>.

A képernyőfelvétel a hello befejeződött alkalmazás alatt van:

![Az Azure-on tárolt hello szolgáltatás megjelenítő böngészőablak][completed-app]  

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy a következő termékek hello és verziók telepített toosuccessfully teljes hello példa ebben a cikkben az alábbiak:

* A [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) telepítése
* [Node.js](https://nodejs.org/download/) telepítése
* Telepítés [Python 2.7.10 verziója](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Egy Felhőszolgáltatás-projekt létrehozása
hello lépések létrehozása hello felhőszolgáltatás-projekt, amely hello Socket.IO alkalmazás fogja futtatni.

1. A hello **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**. Végül kattintson a jobb gombbal **Windows PowerShell** válassza **Futtatás rendszergazdaként**.
   
    ![Az Azure PowerShell ikon][powershell-menu]
2. Hozzon létre egy könyvtárat nevű **c:\\csomópont**. 
   
        PS C:\> md node
3. Módosítsa a könyvtárakat toohello **c:\\csomópont** könyvtár
   
        PS C:\> cd node
4. Adja meg a következő parancsok toocreate nevű új megoldás hello **chatapp** és a feldolgozói szerepkör nevű **WorkerRole1**:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Válasz a következő hello jelenik meg:
   
    ![hello kimeneti hello új-azureservice és azurenodeworkerrolecmdlets hozzáadása](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-hello-chat-example"></a>Töltse le a hello Csevegés – példa
Ebben a projektben használjuk hello Csevegés példa hello [Socket.IO GitHub-tárházban]. Hajtsa végre a következő lépéseket toodownload hello példa hello, majd vegye fel azt a korábban létrehozott toohello projekt.

1. Hozzon létre egy helyi példány hello tárház hello segítségével **Klónozás** gombra. Is használhatja hello **ZIP-** gomb toodownload hello projekt.
   
   ![Egy böngészőablakban https://github.com/LearnBoost/socket.io/tree/master/examples/chat, megtekintését hello ZIP letöltési ikonja kiemelve][chat-example-view]
2. Keresse meg a hello könyvtárstruktúrát hello helyi tárház, amíg megérkezik a hello **példák\\Csevegés** könyvtár. A könyvtár toothe hello tartalom másolása **C:\\csomópont\\chatapp\\WorkerRole1** korábban létrehozott könyvtár.
   
   ![Explorer hello példák hello tartalmát megjelenítő\\hello archív kinyert Csevegés könyvtár][chat-contents]
   
   hello hello fenti képernyőfelvételen kiemelt elemeinek olyan hello átmásolva hello fájlok **példák\\Csevegés** könyvtár
3. A hello **C:\\csomópont\\chatapp\\WorkerRole1** directory, a delete hello **server.js** fájlt, és nevezze át a hello **app.js**fájl túl**server.js**. Ez eltávolítja az hello alapértelmezett **server.js** hello által korábban létrehozott fájl **Add-AzureNodeWorkerRole** parancsmag és a hello alkalmazás fájlként cserél hello Csevegés példa.

### <a name="modify-serverjs-and-install-modules"></a>Módosítsa a Server.js és -modulok telepítése
Tesztelési hello alkalmazás hello Azure emulátorban, mielőtt néhány kisebb módosításokat kell azt. Hajtsa végre a következő lépéseket toothe server.js fájl hello:

1. Nyissa meg hello **server.js** fájlt a Visual Studio vagy szövegszerkesztőben.
2. Hello található **modul függőségek** server.js hello elején szakaszt, és módosítsa a hello sort tartalmazó **sio = require('.. //.. lib//Socket.IO ")** túl**sio = require('socket.io')** alább látható módon:
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. tooensure hello alkalmazás hello megfelelő porton figyel, nyissa meg a server.js a Jegyzettömb vagy a kedvenc szerkesztő, és módosítsa a következő sor tartományvezérlőkkel történő lecserélésével **3000** rendelkező **process.env.port** látható alább:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

Túl hello módosítások mentése után**server.js**, hello lépések segítségével telepítse a szükséges modulokat, és tesztelje az Azure emulátorban hello alkalmazás:

1. Használatával **Azure PowerShell**, módosítsa a könyvtárakat toohello **C:\\csomópont\\chatapp\\WorkerRole1** könyvtárra, és használja, a következő parancs tooinstall hello hello Ez az alkalmazás által igényelt modulok:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Ez a hello package.json fájlban felsorolt hello modulok telepíti. Hello parancs után kimeneti hasonló toothe következő kell megjelennie:
   
   ![hello npm hello kimenete telepítési parancs][The-output-of-the-npm-install-command]
2. Mivel ez a példa eredetileg hello Socket.IO GitHub-tárházban része volt, és közvetlenül a relatív elérési útja által hivatkozott hello Socket.IO könyvtár, Socket.IO nem mutat hivatkozás hello package.json fájlban, ezért azt telepítenie kell a következő parancs hello kiállításával:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Tesztelése és telepítése
1. Indítsa el a hello emulátor hello a következő parancs beírásával:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Ha problémák lépnek fel az emulátor, pl. indítása.: Start-AzureEmulator: nem várt hiba történt.  Részletek: Észlelt egy nem várt hiba hello kommunikációs objektum System.ServiceModel.Channels.ServiceChannel, nem használható kommunikációhoz mivel hello Faulted állapotban van.
   
      Telepítse újra a AzureAuthoringTools v 2.7.1-es és AzureComputeEmulator v 2.7 – győződjön meg arról, hogy verziója megegyezik.
   >
   >


2. Nyisson meg egy böngészőt, és keresse meg a túl**http://127.0.0.1**.
3. Hello böngészőablakban nyílik meg, amikor becenév adja meg, és nyomja le írja be.
   Ez lehetővé teszi toopost üzenetek, egy adott becenév. tootest többfelhasználós funkcióit, nyissa meg az azonos URL-cím segítségével további böngészőablakot, és adja meg a különböző becenevet.
   
   ![Két böngészőablakot felhasználó1, mind a Felhasználó2 üzeneteit megjelenítése](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Tesztelési hello alkalmazása után állítsa le a hello emulátor a következő parancsot:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. toodeploy hello alkalmazás tooAzure, használja a **Publish-AzureServiceProject** parancsmag. Példa:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Lehet, hogy toouse egy egyedi nevet, ellenkező esetben hello közzétételi folyamat meghiúsul. Hello központi telepítés befejezése után hello böngésző megnyitásához, és keresse meg a központilag telepített toohello szolgáltatás.
   > 
   > Ha egy adott hello megadott előfizetés neve nem létezik az importált hello hiba szerint közzétételi profil, töltse le és hello a közzétételi profil importálásához tooAzure telepítése előtt az előfizetéshez. Lásd: hello **hello alkalmazás tooAzure telepítése** szakasza [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Az Azure-on tárolt hello szolgáltatás megjelenítő böngészőablak][completed-app]
   
   > [!NOTE]
   > Ha egy adott hello megadott előfizetés neve nem létezik az importált hello hiba szerint közzétételi profil, töltse le és hello a közzétételi profil importálásához tooAzure telepítése előtt az előfizetéshez. Lásd: hello **hello alkalmazás tooAzure telepítése** szakasza [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.

> [!NOTE]
> Az egyszerűség, ez a minta felhasználók csatlakoztatott toohello közötti korlátozott toochatting ugyanezen példányában. Ez azt jelenti, hogy hello felhőszolgáltatás két szerepkör feldolgozópéldányok hoz létre, ha a felhasználók csak tudják másokkal toochat csatlakoztatott toohello azonos feldolgozói szerepkör példánya. tooscale hello alkalmazás toowork több szerepkör osztályt, használhat olyan technológia például a Service Bus tooshare hello Socket.IO tároló állapota példányok között. Tekintse meg a hello Service Bus-üzenetsorok és témakörök használati minták hello [Azure SDK for Node.js GitHub-tárházban](https://github.com/WindowsAzure/azure-sdk-for-node).
> 
> 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta, hogyan toocreate alapvető csevegőalkalmazás tárolt Azure-Felhőszolgáltatásban. toolearn hogyan toohost az Azure-webhely, alkalmazás: [összeállíthat egy Node.js Csevegés alkalmazást a Socket.IO segítségével az Azure webhelyén található][chatwebsite].

További információkért lásd még: hello [Node.js fejlesztői központ](/develop/nodejs/).

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub-tárházban]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting hello Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


