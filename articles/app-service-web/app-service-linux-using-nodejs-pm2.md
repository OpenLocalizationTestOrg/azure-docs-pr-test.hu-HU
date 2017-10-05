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
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a><span data-ttu-id="1345e-104">A Node.js az Azure Web Apps Linux PM2 konfiguráció használata</span><span class="sxs-lookup"><span data-stu-id="1345e-104">Use PM2 configuration for Node.js in Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="1345e-105">Ha állítja be az alkalmazás verem Node.js webalkalmazás Azure Linux, kap egy Node.js indítási fájlt a következő ábrán látható módon beállítására:</span><span class="sxs-lookup"><span data-stu-id="1345e-105">If you set the application stack to Node.js for Azure Web App on Linux, you get the option to set a Node.js startup file as shown in the following image:</span></span>

![Node.js a rendszerindító fájlok][1]

<span data-ttu-id="1345e-107">Ez a beállítás segítségével hajtsa végre az alábbi műveletek közül:</span><span class="sxs-lookup"><span data-stu-id="1345e-107">You can use this option to do one of the following tasks:</span></span>

* <span data-ttu-id="1345e-108">Adja meg a indítási parancsfájlt a Node.js-alkalmazás (például: /bin/server.js).</span><span class="sxs-lookup"><span data-stu-id="1345e-108">Specify the startup script for your Node.js app (for example: /bin/server.js).</span></span>
* <span data-ttu-id="1345e-109">Adja meg a PM2 konfigurációs fájl használata a Node.js-alkalmazás (például: /foo/process.json).</span><span class="sxs-lookup"><span data-stu-id="1345e-109">Specify the PM2 configuration file to use for your Node.js app (for example: /foo/process.json).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="1345e-110">Ha azt szeretné, hogy automatikusan újrainduljon, ha egyes fájlok módosítják a Node.js folyamatok, PM2 konfigurációt használja.</span><span class="sxs-lookup"><span data-stu-id="1345e-110">If you want your Node.js processes to restart automatically when certain files are modified, use the PM2 configuration.</span></span> <span data-ttu-id="1345e-111">Ellenkező esetben az alkalmazás nem indítsa újra a változási értesítéseket (például az alkalmazás kódjában megváltozásakor) fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="1345e-111">Otherwise, your application won't restart when it receives change notifications (for example, when your application code changes).</span></span>
  > 
  > 

<span data-ttu-id="1345e-112">Ellenőrizheti, hogy a Node.js [feldolgozni a fájlt dokumentáció](http://pm2.keymetrics.io/docs/usage/application-declaration/) minden a beállításokat, de az érték egy minta a process.json fájlként is használja:</span><span class="sxs-lookup"><span data-stu-id="1345e-112">You can check the Node.js [process file documentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) for all the options, but following is a sample of what you can use as your process.json file:</span></span>

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

<span data-ttu-id="1345e-113">Fontos megjegyezni, ebben a konfigurációban minden olyan:</span><span class="sxs-lookup"><span data-stu-id="1345e-113">Important things to note in this configuration are:</span></span>

* <span data-ttu-id="1345e-114">A "script" tulajdonság határozza meg az alkalmazás indítási parancsfájlra.</span><span class="sxs-lookup"><span data-stu-id="1345e-114">The "script" property specifies your application's start script.</span></span>
* <span data-ttu-id="1345e-115">Az "instances" tulajdonság határozza meg, hogy a csomópont folyamat elindításához hány példánya.</span><span class="sxs-lookup"><span data-stu-id="1345e-115">The "instances" property specifies how many instances of the node process to launch.</span></span> <span data-ttu-id="1345e-116">Ha az alkalmazás a nagyobb virtuális gépeken, több maggal rendelkező futnak, célszerű maximalizálhatja az erőforrások magasabb értéket itt beállításával.</span><span class="sxs-lookup"><span data-stu-id="1345e-116">If you are running your application on larger VMs that have multiple cores, it's a good idea to maximize your resources by setting a higher value here.</span></span>
* <span data-ttu-id="1345e-117">A "figyelés" tömb határozza meg az összes fájl, amely újraindítja a csomópont folyamata, amikor változik.</span><span class="sxs-lookup"><span data-stu-id="1345e-117">The "watch" array specifies all files that you want to restart the node process for when they change.</span></span>
* <span data-ttu-id="1345e-118">A "watch_options" meg kell adnia a "usePolling" igaz a tartalmát csatlakoztatva van módjával jelenleg.</span><span class="sxs-lookup"><span data-stu-id="1345e-118">For the "watch_options", you currently need to specify "usePolling" as true because of the way your application content is mounted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1345e-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1345e-119">Next steps</span></span>
* [<span data-ttu-id="1345e-120">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="1345e-120">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="1345e-121">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="1345e-121">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
