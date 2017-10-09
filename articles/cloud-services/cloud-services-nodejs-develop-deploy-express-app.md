---
title: App (Node.js) expressz aaaWeb |} Microsoft Docs
description: "Az oktatóanyag, amely hello cloud service oktatóanyag épül, és bemutatja, hogyan toouse hello Express modul."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 91921bfbe137eeca9a110d4cb18eb57b46b0060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Express használata Azure Cloud Service a Node.js-webalkalmazás létrehozása
NODE.js hello core runtime a legszükségesebb funkciókat tartalmazza.
A fejlesztők gyakran használnak 3. fél modulok tooprovide további funkciókat, a Node.js-alkalmazások fejlesztése során. Ebben az oktatóanyagban létrehoz egy új alkalmazást, az hello [Express] [ Express] modult, amely egy MVC keretrendszert biztosít a Node.js webalkalmazás létrehozásához.

A képernyőfelvétel a hello befejeződött alkalmazás alatt van:

![A webböngészőben tooExpress Üdvözli az Azure-ban](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Egy Felhőszolgáltatás-projekt létrehozása
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Hajtsa végre a következő hello lépéseket toocreate egy új felhőszolgáltatás-projekt "expressapp" nevű:

1. A hello **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**. Végül kattintson a jobb gombbal **Windows PowerShell** válassza **Futtatás rendszergazdaként**.
   
    ![Az Azure PowerShell ikon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Módosítsa a könyvtárakat toohello **c:\\csomópont** könyvtár és írja be a következő parancsok toocreate nevű új megoldás hello **expressapp** és a webes szerepkör nevű **WebRole1** :
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Alapértelmezés szerint **Add-AzureNodeWebRole** Node.js régebbi verzióját használja. Hello **Set-AzureServiceProjectRole** fenti utasítás arra utasítja az Azure toouse v0.10.21 csomópont.  Megjegyzés: hello paraméterei kis-és nagybetűket.  Ellenőrizheti a Node.js hello verziójának kijelölt hello ellenőrzésével **motorok** tulajdonság **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Express telepítése
1. Telepítse a hello Express generátor hello a következő parancs beírásával:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    hello npm parancs kimenetében hello toohello hasonló, az alábbi eredmény kell kinéznie. 
   
    ![A Windows PowerShell megjelenítését hello kimenete hello npm telepítési expressz parancs.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Módosítsa a könyvtárakat toohello **WebRole1** könyvtárra, és használja hello expressz parancs toogenerate egy új alkalmazást:
   
        PS C:\node\expressapp\WebRole1> express
   
    Akkor lesz felszólító toooverwrite a korábbi alkalmazás. Adja meg **y** vagy **Igen** toocontinue. Express hello app.js fájl- és az alkalmazás felépítése mappaszerkezetét hoz létre.
   
    ![hello hello expressz parancs kimenete](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. tooinstall további függőségek hello package.json fájlban meghatározott adja meg a következő parancs hello:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![hello npm hello kimenete telepítési parancs](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Használjon hello következő parancsot a toocopy hello **bin/www** fájl túl**server.js**. Ez a helyzet hello felhőszolgáltatás hello belépési ponton található ehhez az alkalmazáshoz.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Ez a parancs után rendelkeznie kell egy **server.js** hello WebRole1 fájl.
5. Hello módosítása **server.js** hello egyik tooremove "." karaktert tartalmaz a következő sor hello.
   
       var app = require('../app');
   
   Ez a módosítás elvégzése, után hello sor meg kell jelennie az alábbiak szerint.
   
       var app = require('./app');
   
   Ez a változás azért szükséges, mert hello fájl helyeztük (korábbi nevén **bin/www**,) toohello megadni hello alkalmazásfájl könyvtárába. A módosítás elvégzése után mentse a hello **server.js** fájlt.
6. A következő parancs toorun hello alkalmazás hello Azure emulátorban hello használata:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Üdvözli a tooexpress tartalmazó webhelyet.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-hello-view"></a>Hello nézet módosítása
Most módosítsa a nézet toodisplay hello üdvözlőüzenetére "Üdvözli tooExpress az Azure-ban".

1. Adja meg a következő parancs tooopen hello index.jade fájl hello:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![hello index.jade fájl hello tartalmát.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade hello alapértelmezett megjelenítési motort Express alkalmazások által használt. Hello Jade megjelenítési motort további információkért lásd: [http://jade-lang.com][http://jade-lang.com].
2. Szöveg utolsó sorának hello módosítása hozzáfűzésével **az Azure-ban**.
   
   ![hello index.jade fájl utolsó sora hello beolvassa: p üdvözli túl\#{title} az Azure-ban](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Mentse hello fájlt, és lépjen ki a Jegyzettömbből.
4. Frissítse a böngészőt, és látni fogja a módosításokat.
   
   ![Egy böngészőablakban hello lap tartalmazza-e az Azure-ban üdvözlő tooExpress](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Tesztelési hello alkalmazását követően használja a hello **Stop-AzureEmulator** parancsmag toostop hello emulátor.

## <a name="publishing-hello-application-tooazure"></a>Közzétételi hello alkalmazás tooAzure
Hello Azure PowerShell-ablakban, használja a hello **Publish-AzureServiceProject** parancsmag toodeploy hello tooa felhőalapú alkalmazásszolgáltatás

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Miután hello telepítési művelet befejeződik, a böngészőben nyissa meg, és hello weblap megjelenítéséhez.

![A webböngészőben hello Express lap. hello URL-cím azt jelzi, hogy most már futó Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [Node.js fejlesztői központ](/develop/nodejs/).

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


