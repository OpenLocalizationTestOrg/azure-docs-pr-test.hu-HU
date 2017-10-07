---
title: "a Node.js az Azure Web Apps Linux aaaUsing PM2 konfigurációs |} Microsoft Docs"
description: "PM2 configuration for Node.js Linux Azure Web App használatával"
keywords: "az Azure app service, webalkalmazás, nodejs, pm2, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: fb420f32-6d74-49c7-992f-0ed5616e66e7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: 923783ffe656e01c43318899d1a656b553ebb5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a>A Node.js az Azure Web Apps Linux PM2 konfiguráció használata

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Hello alkalmazás verem tooNode.js Azure Web Apps Linux állítja be, ha kap hello beállítás tooset Node.js indítási fájl látható hello kép a következő módon:

![Node.js a rendszerindító fájlok][1]

Ez a beállítás toodo egy a következő feladatok hello használhatja:

* Adja meg a hello indítási parancsfájl a Node.js-alkalmazás (például: /bin/server.js).
* Adja meg a hello PM2 konfigurációs fájl toouse a Node.js-alkalmazás (például: /foo/process.json).
  
  > [!NOTE]
  > Ha azt szeretné, a Node.js folyamatok toorestart automatikusan Ha egyes fájlok módosítják, hello PM2 konfigurációt használja. Ellenkező esetben az alkalmazás nem indítsa újra a változási értesítéseket (például az alkalmazás kódjában megváltozásakor) fogadásakor.
  > 
  > 

Ellenőrizheti a hello Node.js [feldolgozni a fájlt dokumentáció](http://pm2.keymetrics.io/docs/usage/application-declaration/) összes hello beállításokat, de az alábbiakban látható egy minta a process.json fájlként is használja:

        {
          "name"        : "worker",
          "script"      : "./bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["./bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Ebben a konfigurációban a fontos részek toonote a következők:

* hello "script" tulajdonság határozza meg az alkalmazás indítási parancsfájlra.
* hello "instances" tulajdonság hello csomópont folyamat toolaunch hány példánya határozza meg. Az alkalmazás a nagyobb virtuális gépeken, több maggal rendelkező futtat, akkor egy jó ötlet toomaximize az erőforrások magasabb értéket itt beállításával.
* hello "bemutató" tömb toorestart hello csomópont folyamata szüksége, amikor változnak az összes fájlt határozza meg.
* A "watch_options" hello jelenleg kell toospecify "usePolling" igaz miatt hello módja a tartalmát csatlakoztatva van.

## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](app-service-linux-intro.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
