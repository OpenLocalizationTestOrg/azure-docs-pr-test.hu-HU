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
# <a name="use-pm2-configuration-for-nodejs-in-azure-web-app-on-linux"></a><span data-ttu-id="abd0a-104">A Node.js az Azure Web Apps Linux PM2 konfiguráció használata</span><span class="sxs-lookup"><span data-stu-id="abd0a-104">Use PM2 configuration for Node.js in Azure Web App on Linux</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


<span data-ttu-id="abd0a-105">Hello alkalmazás verem tooNode.js Azure Web Apps Linux állítja be, ha kap hello beállítás tooset Node.js indítási fájl látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="abd0a-105">If you set hello application stack tooNode.js for Azure Web App on Linux, you get hello option tooset a Node.js startup file as shown in hello following image:</span></span>

![Node.js a rendszerindító fájlok][1]

<span data-ttu-id="abd0a-107">Ez a beállítás toodo egy a következő feladatok hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="abd0a-107">You can use this option toodo one of hello following tasks:</span></span>

* <span data-ttu-id="abd0a-108">Adja meg a hello indítási parancsfájl a Node.js-alkalmazás (például: /bin/server.js).</span><span class="sxs-lookup"><span data-stu-id="abd0a-108">Specify hello startup script for your Node.js app (for example: /bin/server.js).</span></span>
* <span data-ttu-id="abd0a-109">Adja meg a hello PM2 konfigurációs fájl toouse a Node.js-alkalmazás (például: /foo/process.json).</span><span class="sxs-lookup"><span data-stu-id="abd0a-109">Specify hello PM2 configuration file toouse for your Node.js app (for example: /foo/process.json).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="abd0a-110">Ha azt szeretné, a Node.js folyamatok toorestart automatikusan Ha egyes fájlok módosítják, hello PM2 konfigurációt használja.</span><span class="sxs-lookup"><span data-stu-id="abd0a-110">If you want your Node.js processes toorestart automatically when certain files are modified, use hello PM2 configuration.</span></span> <span data-ttu-id="abd0a-111">Ellenkező esetben az alkalmazás nem indítsa újra a változási értesítéseket (például az alkalmazás kódjában megváltozásakor) fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="abd0a-111">Otherwise, your application won't restart when it receives change notifications (for example, when your application code changes).</span></span>
  > 
  > 

<span data-ttu-id="abd0a-112">Ellenőrizheti a hello Node.js [feldolgozni a fájlt dokumentáció](http://pm2.keymetrics.io/docs/usage/application-declaration/) összes hello beállításokat, de az alábbiakban látható egy minta a process.json fájlként is használja:</span><span class="sxs-lookup"><span data-stu-id="abd0a-112">You can check hello Node.js [process file documentation](http://pm2.keymetrics.io/docs/usage/application-declaration/) for all hello options, but following is a sample of what you can use as your process.json file:</span></span>

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

<span data-ttu-id="abd0a-113">Ebben a konfigurációban a fontos részek toonote a következők:</span><span class="sxs-lookup"><span data-stu-id="abd0a-113">Important things toonote in this configuration are:</span></span>

* <span data-ttu-id="abd0a-114">hello "script" tulajdonság határozza meg az alkalmazás indítási parancsfájlra.</span><span class="sxs-lookup"><span data-stu-id="abd0a-114">hello "script" property specifies your application's start script.</span></span>
* <span data-ttu-id="abd0a-115">hello "instances" tulajdonság hello csomópont folyamat toolaunch hány példánya határozza meg.</span><span class="sxs-lookup"><span data-stu-id="abd0a-115">hello "instances" property specifies how many instances of hello node process toolaunch.</span></span> <span data-ttu-id="abd0a-116">Az alkalmazás a nagyobb virtuális gépeken, több maggal rendelkező futtat, akkor egy jó ötlet toomaximize az erőforrások magasabb értéket itt beállításával.</span><span class="sxs-lookup"><span data-stu-id="abd0a-116">If you are running your application on larger VMs that have multiple cores, it's a good idea toomaximize your resources by setting a higher value here.</span></span>
* <span data-ttu-id="abd0a-117">hello "bemutató" tömb toorestart hello csomópont folyamata szüksége, amikor változnak az összes fájlt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="abd0a-117">hello "watch" array specifies all files that you want toorestart hello node process for when they change.</span></span>
* <span data-ttu-id="abd0a-118">A "watch_options" hello jelenleg kell toospecify "usePolling" igaz miatt hello módja a tartalmát csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="abd0a-118">For hello "watch_options", you currently need toospecify "usePolling" as true because of hello way your application content is mounted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="abd0a-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="abd0a-119">Next steps</span></span>
* [<span data-ttu-id="abd0a-120">Mi az Azure Web Apps Linux?</span><span class="sxs-lookup"><span data-stu-id="abd0a-120">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="abd0a-121">Az Azure App Service webalkalmazásba Linux – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="abd0a-121">Azure App Service Web App on Linux FAQ</span></span>](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png
