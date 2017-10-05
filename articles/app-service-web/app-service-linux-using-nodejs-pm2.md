---
title: "PM2 configuration for Linux Azure Web App alkalmazásban Node.js használatával |} Microsoft Docs"
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
ms.openlocfilehash: 5002400a673e2c5cc4290bab488b839fb2282966
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a>A Node.js az Azure Web Apps Linux PM2 konfiguráció használata

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Ha állítja be az alkalmazás verem Node.js webalkalmazás Azure Linux, kap egy Node.js indítási fájlt a következő ábrán látható módon beállítására:

![Node.js a rendszerindító fájlok][1]

Ez a beállítás segítségével hajtsa végre az alábbi műveletek közül:

* Adja meg a indítási parancsfájlt a Node.js-alkalmazás (például: /bin/server.js).
* Adja meg a PM2 konfigurációs fájl használata a Node.js-alkalmazás (például: /foo/process.json).
  
  > [!NOTE]
  > Ha azt szeretné, hogy automatikusan újrainduljon, ha egyes fájlok módosítják a Node.js folyamatok, PM2 konfigurációt használja. Ellenkező esetben az alkalmazás nem indítsa újra a változási értesítéseket (például az alkalmazás kódjában megváltozásakor) fogadásakor.
  > 
  > 

Ellenőrizheti, hogy a Node.js [feldolgozni a fájlt dokumentáció](http://pm2.keymetrics.io/docs/usage/application-declaration/) minden a beállításokat, de az érték egy minta a process.json fájlként is használja:

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

Fontos megjegyezni, ebben a konfigurációban minden olyan:

* A "script" tulajdonság határozza meg az alkalmazás indítási parancsfájlra.
* Az "instances" tulajdonság határozza meg, hogy a csomópont folyamat elindításához hány példánya. Ha az alkalmazás a nagyobb virtuális gépeken, több maggal rendelkező futnak, célszerű maximalizálhatja az erőforrások magasabb értéket itt beállításával.
* A "figyelés" tömb határozza meg az összes fájl, amely újraindítja a csomópont folyamata, amikor változik.
* A "watch_options" meg kell adnia a "usePolling" igaz a tartalmát csatlakoztatva van módjával jelenleg.

## <a name="next-steps"></a>Következő lépések
* [Mi az Azure Web Apps Linux?](app-service-linux-intro.md)
* [Az Azure App Service webalkalmazásba Linux – gyakori kérdések](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
