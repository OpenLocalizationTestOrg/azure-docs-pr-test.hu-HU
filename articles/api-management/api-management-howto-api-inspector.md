---
title: "API-Inspector - Azure API Management aaaTrace hívások |} Microsoft Docs"
description: "Ismerje meg, hogyan tootrace hívásokon hello az Azure API Management API-Inspector."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: b0c401caa8da1b789f6cfe5edf97a5f118d78f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-api-inspector-tootrace-calls-in-azure-api-management"></a><span data-ttu-id="16f4c-103">Hogyan toouse hello API Inspector tootrace meghívja az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="16f4c-103">How toouse hello API Inspector tootrace calls in Azure API Management</span></span>
<span data-ttu-id="16f4c-104">Az API Management biztosít egy API-Inspector eszköz toohelp, a Hibakeresés és hibaelhárítás az API-kat.</span><span class="sxs-lookup"><span data-stu-id="16f4c-104">API Management provides an API Inspector tool toohelp you with debugging and troubleshooting your APIs.</span></span> <span data-ttu-id="16f4c-105">hello API Inspector programozott módon használható, és közvetlenül a hello fejlesztői portálján is használható.</span><span class="sxs-lookup"><span data-stu-id="16f4c-105">hello API Inspector can be used programmatically and can also be used directly from hello developer portal.</span></span> 

<span data-ttu-id="16f4c-106">Ezenkívül tootracing műveletek API Inspector is hívásláncainak [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx) értékelések.</span><span class="sxs-lookup"><span data-stu-id="16f4c-106">In addition tootracing operations, API Inspector also traces [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx) evaluations.</span></span> <span data-ttu-id="16f4c-107">Bemutatója, lásd: [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too21:00 előre.</span><span class="sxs-lookup"><span data-stu-id="16f4c-107">For a demonstration, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too21:00.</span></span>

<span data-ttu-id="16f4c-108">Ez az útmutató egy segédlet API Inspector használatával.</span><span class="sxs-lookup"><span data-stu-id="16f4c-108">This guide provides a walk-through of using API Inspector.</span></span>

> [!NOTE]
> <span data-ttu-id="16f4c-109">API-Inspector nyomkövetések csak jön létre, és toohello tartozó előfizetés kulcsok tartalmazó kérelmek felhasználhatóvá [rendszergazda](api-management-howto-create-groups.md) fiók.</span><span class="sxs-lookup"><span data-stu-id="16f4c-109">API Inspector traces are only generated and made available for requests containing subscription keys that belong toohello [administrator](api-management-howto-create-groups.md) account.</span></span>
> 
> 

## <span data-ttu-id="16f4c-110"><a name="trace-call"></a> Használata API-Inspector tootrace hívása</span><span class="sxs-lookup"><span data-stu-id="16f4c-110"><a name="trace-call"> </a> Use API Inspector tootrace a call</span></span>
<span data-ttu-id="16f4c-111">toouse API Inspector hozzáadása egy **ocp-apim-nyomkövetési: true** kérelem fejléc tooyour művelet meghívásának, majd töltse le és hello nyomkövetési hello által jelzett hello URL-cím használatával vizsgálhatja **ocp-apim-nyomkövetési-hely** Válaszfejléc.</span><span class="sxs-lookup"><span data-stu-id="16f4c-111">toouse API Inspector, add an **ocp-apim-trace: true** request header tooyour operation call, and then download and inspect hello trace using hello URL indicated by hello **ocp-apim-trace-location** response header.</span></span> <span data-ttu-id="16f4c-112">Programozott módon lehet végrehajtani, és közvetlenül a hello fejlesztői portálján is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="16f4c-112">This can be done programatically, and can also be done directly from hello developer portal.</span></span>

<span data-ttu-id="16f4c-113">Ez az oktatóanyag bemutatja, hogyan toouse hello API Inspector tootrace műveletekbe hello alapvető Számológép API hello konfigurált [kezelése az első API](api-management-get-started.md) használatába bevezető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="16f4c-113">This tutorial shows how toouse hello API Inspector tootrace operations using hello Basic Calculator API that is configured in hello [Manage your first API](api-management-get-started.md) getting started tutorial.</span></span> <span data-ttu-id="16f4c-114">Ha még nem fejeződött be, hogy az oktatóanyag mindössze néhány perc múlva tooimport hello alapvető Számológép API vagy egy másik API-hoz a például hello Echo API kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="16f4c-114">If you haven't completed that tutorial it only takes a few moments tooimport hello Basic Calculator API, or you can use another API of your choosing such as hello Echo API.</span></span> <span data-ttu-id="16f4c-115">Minden API Management service-példány az Echo API-k, melyek a használt tooexperiment és API-kezeléssel kapcsolatos további tudnivalók az előre konfigurált származnak.</span><span class="sxs-lookup"><span data-stu-id="16f4c-115">Each API Management service instance comes pre-configured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="16f4c-116">hello Echo API bármilyen bemeneti tooit küldött vissza visszaadása.</span><span class="sxs-lookup"><span data-stu-id="16f4c-116">hello Echo API returns back whatever input is sent tooit.</span></span> <span data-ttu-id="16f4c-117">toouse, hívhat meg a HTTP-műveletet, és a visszatérési érték hello egyszerűen lesz az Ön által küldött.</span><span class="sxs-lookup"><span data-stu-id="16f4c-117">toouse it, you can invoke any HTTP verb, and hello return value will simply be what you sent.</span></span> 

<span data-ttu-id="16f4c-118">tooget elindítani, kattintson a **fejlesztői portálján** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="16f4c-118">tooget started, click **Developer portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="16f4c-119">Műveletek hívható közvetlenül a hello fejlesztői portálján, amely egy kényelmes módszert arra tooview biztosít, és tesztelje az API-k hello működésére.</span><span class="sxs-lookup"><span data-stu-id="16f4c-119">Operations can be called directly from hello developer portal which provides a convenient way tooview and test hello operations of an API.</span></span>

> <span data-ttu-id="16f4c-120">Ha még nem hozott az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="16f4c-120">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

![Az API Management fejlesztői portálján][api-management-developer-portal-menu]

<span data-ttu-id="16f4c-122">Kattintson a **API-k** hello felső menüben, majd kattintson a **alapvető Számológép**.</span><span class="sxs-lookup"><span data-stu-id="16f4c-122">Click **APIs** from hello top menu, and then click **Basic Calculator**.</span></span>

![Echo API][api-management-api]

<span data-ttu-id="16f4c-124">Kattintson a **kipróbálás** tootry hello **két egész számok hozzáadása** műveletet.</span><span class="sxs-lookup"><span data-stu-id="16f4c-124">Click **Try it** tootry hello **Add two integers** operation.</span></span>

![Kipróbálom][api-management-open-console]

<span data-ttu-id="16f4c-126">Megtartása hello alapértelmezett paraméterértékek, és válassza hello előfizetés kulcs kívánt hello termék toouse hello **előfizetés-kulcs** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="16f4c-126">Keep hello default parameter values, and select hello subscription key for hello product you want toouse from hello **subscription-key** drop-down.</span></span>

<span data-ttu-id="16f4c-127">Alapértelmezés szerint a hello developer portálon hello **Ocp-Apim-nyomkövetési** fejléc már be van állítva túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="16f4c-127">By default in hello developer portal hello **Ocp-Apim-Trace** header is already set too**true**.</span></span> <span data-ttu-id="16f4c-128">Ezt a fejlécet konfigurálja-e a nyomkövetési jön létre.</span><span class="sxs-lookup"><span data-stu-id="16f4c-128">This header configures whether or not a trace is generated.</span></span>

![Küldés][api-management-http-get]

<span data-ttu-id="16f4c-130">Kattintson a **küldése** tooinvoke hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="16f4c-130">Click **Send** tooinvoke hello operation.</span></span>

![Küldés][api-management-send-results]

<span data-ttu-id="16f4c-132">Hello válaszul fejlécek kerülnek egy **ocp-apim-nyomkövetési-hely** egy hasonló toohello érték, a következő példa a.</span><span class="sxs-lookup"><span data-stu-id="16f4c-132">In hello response headers will be an **ocp-apim-trace-location** with a value similar toohello following example.</span></span>

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

<span data-ttu-id="16f4c-133">hello nyomkövetési tölthető le: hello megadott helyre, és tekintse át, ahogyan az következő lépés hello.</span><span class="sxs-lookup"><span data-stu-id="16f4c-133">hello trace can be downloaded from hello specified location and reviewed as demonstrated in hello next step.</span></span> <span data-ttu-id="16f4c-134">Ne feledje, hogy csak hello utolsó 100 naplóbejegyzések tárolt naplók helye újra felhasználja a rendszer az elforgatástól.</span><span class="sxs-lookup"><span data-stu-id="16f4c-134">Note that only hello last 100 log entries are stored and log locations are reused in rotation.</span></span> <span data-ttu-id="16f4c-135">Igen, ha több mint 100 hívásokat a kérelmek nyomon követése engedélyezve végül hozzákezdhet felülírja a hello első nyomkövetések helyen.</span><span class="sxs-lookup"><span data-stu-id="16f4c-135">So if you make more than 100 calls with tracing enabled you will eventually start overwriting hello first traces in place.</span></span>

## <span data-ttu-id="16f4c-136"><a name="inspect-trace"></a>Hello nyomkövetési vizsgálata</span><span class="sxs-lookup"><span data-stu-id="16f4c-136"><a name="inspect-trace"> </a>Inspect hello trace</span></span>
<span data-ttu-id="16f4c-137">hello nyomkövetési tooreview hello értékek hello nyomkövetési fájl letöltését hello **ocp-apim-nyomkövetési-hely** URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="16f4c-137">tooreview hello values in hello trace, download hello trace file from hello **ocp-apim-trace-location** URL.</span></span> <span data-ttu-id="16f4c-138">Ez JSON formátumban a fájlt, és bejegyzéseket a következő példa hasonló toohello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="16f4c-138">It is a text file in JSON format, and contains entries similar toohello following example.</span></span>

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded toohello backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent toohello caller. Starting toostream hello response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming toohello caller is complete."
                }
            }
        ]
    }
}
```

## <span data-ttu-id="16f4c-139"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16f4c-139"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="16f4c-140">Tekintse meg a házirend-kifejezések helyesen a nyomkövetés bemutatója [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span><span class="sxs-lookup"><span data-stu-id="16f4c-140">Watch a demo of tracing policy expressions in [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span></span> <span data-ttu-id="16f4c-141">Előretekerés too21:00 toosee hello bemutató.</span><span class="sxs-lookup"><span data-stu-id="16f4c-141">Fast-forward too21:00 toosee hello demo.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector tootrace a call]: #trace-call
[Inspect hello trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




