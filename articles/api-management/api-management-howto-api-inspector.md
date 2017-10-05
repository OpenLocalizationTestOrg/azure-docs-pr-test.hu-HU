---
title: "Nyomkövetés-hívások API Inspector - Azure API Management |} Microsoft Docs"
description: "Útmutató az Azure API Management az API-Inspector használatával hívások nyomon követéséhez."
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
ms.openlocfilehash: a9d4d3be7f046af975f6dc25670070204848588c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a><span data-ttu-id="58e09-103">A követni kívánt API-Inspector használatával meghívja az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="58e09-103">How to use the API Inspector to trace calls in Azure API Management</span></span>
<span data-ttu-id="58e09-104">Az API Management rendelkezik egy API-Inspector eszközzel segítséget nyújtanak az hibakeresését vagy hibaelhárítását az API-kat.</span><span class="sxs-lookup"><span data-stu-id="58e09-104">API Management provides an API Inspector tool to help you with debugging and troubleshooting your APIs.</span></span> <span data-ttu-id="58e09-105">Az API-Inspector programozott módon használható, és a developer portálról közvetlenül is használható.</span><span class="sxs-lookup"><span data-stu-id="58e09-105">The API Inspector can be used programmatically and can also be used directly from the developer portal.</span></span> 

<span data-ttu-id="58e09-106">Nyomkövetés műveletek mellett is követi az API-Inspector [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx) értékelések.</span><span class="sxs-lookup"><span data-stu-id="58e09-106">In addition to tracing operations, API Inspector also traces [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx) evaluations.</span></span> <span data-ttu-id="58e09-107">Bemutatója, lásd: [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és előretekerés 21:00.</span><span class="sxs-lookup"><span data-stu-id="58e09-107">For a demonstration, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 21:00.</span></span>

<span data-ttu-id="58e09-108">Ez az útmutató egy segédlet API Inspector használatával.</span><span class="sxs-lookup"><span data-stu-id="58e09-108">This guide provides a walk-through of using API Inspector.</span></span>

> [!NOTE]
> <span data-ttu-id="58e09-109">API-Inspector nyomkövetések csak jön létre és tartozó előfizetés kulcsok tartalmazó kérelmek elérhetővé a [rendszergazda](api-management-howto-create-groups.md) fiók.</span><span class="sxs-lookup"><span data-stu-id="58e09-109">API Inspector traces are only generated and made available for requests containing subscription keys that belong to the [administrator](api-management-howto-create-groups.md) account.</span></span>
> 
> 

## <span data-ttu-id="58e09-110"><a name="trace-call"></a> Használata API-Inspector hívás nyomkövetéséhez</span><span class="sxs-lookup"><span data-stu-id="58e09-110"><a name="trace-call"> </a> Use API Inspector to trace a call</span></span>
<span data-ttu-id="58e09-111">API-Inspector, vegye fel egy **ocp-apim-nyomkövetési: true** a művelet hívásához kérelemfejléc, majd letöltését, és vizsgálja meg a nyomkövetési azt az URL-cím segítségével a **ocp-apim-nyomkövetési-hely** válasz fejléc.</span><span class="sxs-lookup"><span data-stu-id="58e09-111">To use API Inspector, add an **ocp-apim-trace: true** request header to your operation call, and then download and inspect the trace using the URL indicated by the **ocp-apim-trace-location** response header.</span></span> <span data-ttu-id="58e09-112">Programozott módon lehet végrehajtani, és közvetlenül a developer portálról is el lehet végezni.</span><span class="sxs-lookup"><span data-stu-id="58e09-112">This can be done programatically, and can also be done directly from the developer portal.</span></span>

<span data-ttu-id="58e09-113">Ez az oktatóanyag bemutatja, hogyan használható az API-Inspector nyomkövetést műveletekhez konfigurált alapvető Számológép API használatával a [kezelése az első API](api-management-get-started.md) használatába bevezető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="58e09-113">This tutorial shows how to use the API Inspector to trace operations using the Basic Calculator API that is configured in the [Manage your first API](api-management-get-started.md) getting started tutorial.</span></span> <span data-ttu-id="58e09-114">Ha még nem fejeződött be, hogy az oktatóanyag csak az alapszintű Számológép API importálása néhány percet vesz igénybe, vagy egy másik API-hoz a például az Echo API kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="58e09-114">If you haven't completed that tutorial it only takes a few moments to import the Basic Calculator API, or you can use another API of your choosing such as the Echo API.</span></span> <span data-ttu-id="58e09-115">Minden API Management szolgáltatáspéldányhoz előre konfigurálva van egy kipróbálható Echo API, amely segít megismerni az API Management szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="58e09-115">Each API Management service instance comes pre-configured with an Echo API that can be used to experiment with and learn about API Management.</span></span> <span data-ttu-id="58e09-116">Az Echo API bármilyen bemeneti által küldött vissza beolvasása.</span><span class="sxs-lookup"><span data-stu-id="58e09-116">The Echo API returns back whatever input is sent to it.</span></span> <span data-ttu-id="58e09-117">A használatához hívhat meg a HTTP-műveletet, és a visszatérési érték egyszerűen lesz az Ön által küldött.</span><span class="sxs-lookup"><span data-stu-id="58e09-117">To use it, you can invoke any HTTP verb, and the return value will simply be what you sent.</span></span> 

<span data-ttu-id="58e09-118">A kezdéshez kattintson **fejlesztői portálján** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="58e09-118">To get started, click **Developer portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="58e09-119">Műveletek hívható közvetlenül a megtekintése, és az API-k működésének teszteléséhez kényelmes megoldást kínál a fejlesztői portálhoz.</span><span class="sxs-lookup"><span data-stu-id="58e09-119">Operations can be called directly from the developer portal which provides a convenient way to view and test the operations of an API.</span></span>

> <span data-ttu-id="58e09-120">Ha még nem hozott az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a a [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="58e09-120">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

![Az API Management fejlesztői portálján][api-management-developer-portal-menu]

<span data-ttu-id="58e09-122">Kattintson a **API-k** a felső menüben, majd kattintson a **alapvető Számológép**.</span><span class="sxs-lookup"><span data-stu-id="58e09-122">Click **APIs** from the top menu, and then click **Basic Calculator**.</span></span>

![Echo API][api-management-api]

<span data-ttu-id="58e09-124">Kattintson a **kipróbálás** próbálkozzon a **két egész számok hozzáadása** műveletet.</span><span class="sxs-lookup"><span data-stu-id="58e09-124">Click **Try it** to try the **Add two integers** operation.</span></span>

![Kipróbálom][api-management-open-console]

<span data-ttu-id="58e09-126">Tartsa meg az alapértelmezett paraméterértékek, és válassza ki a használni kívánt termék előfizetés kulcsot a **előfizetés-kulcs** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="58e09-126">Keep the default parameter values, and select the subscription key for the product you want to use from the **subscription-key** drop-down.</span></span>

<span data-ttu-id="58e09-127">A fejlesztői portálra alapértelmezés szerint a **Ocp-Apim-nyomkövetési** fejléc már be van állítva **igaz**.</span><span class="sxs-lookup"><span data-stu-id="58e09-127">By default in the developer portal the **Ocp-Apim-Trace** header is already set to **true**.</span></span> <span data-ttu-id="58e09-128">Ezt a fejlécet konfigurálja-e a nyomkövetési jön létre.</span><span class="sxs-lookup"><span data-stu-id="58e09-128">This header configures whether or not a trace is generated.</span></span>

![Küldés][api-management-http-get]

<span data-ttu-id="58e09-130">Kattintson a **küldése** meghívni a műveletet.</span><span class="sxs-lookup"><span data-stu-id="58e09-130">Click **Send** to invoke the operation.</span></span>

![Küldés][api-management-send-results]

<span data-ttu-id="58e09-132">A válaszban szereplő fejlécek kerülnek egy **ocp-apim-nyomkövetési-hely** az alábbi példához hasonló értékkel.</span><span class="sxs-lookup"><span data-stu-id="58e09-132">In the response headers will be an **ocp-apim-trace-location** with a value similar to the following example.</span></span>

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

<span data-ttu-id="58e09-133">A nyomkövetés a megadott helyről letölthető, és tekintse át, ahogyan az a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="58e09-133">The trace can be downloaded from the specified location and reviewed as demonstrated in the next step.</span></span> <span data-ttu-id="58e09-134">Ne feledje, hogy csak az utolsó 100 naplóbejegyzések tárolt naplók helye újra felhasználja a rendszer az elforgatástól.</span><span class="sxs-lookup"><span data-stu-id="58e09-134">Note that only the last 100 log entries are stored and log locations are reused in rotation.</span></span> <span data-ttu-id="58e09-135">Igen, ha több mint 100 hívásokat a kérelmek nyomon követése engedélyezve végül hozzákezdhet az első nyomkövetések helyen felülírja.</span><span class="sxs-lookup"><span data-stu-id="58e09-135">So if you make more than 100 calls with tracing enabled you will eventually start overwriting the first traces in place.</span></span>

## <span data-ttu-id="58e09-136"><a name="inspect-trace"></a>Vizsgálhatja meg a nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="58e09-136"><a name="inspect-trace"> </a>Inspect the trace</span></span>
<span data-ttu-id="58e09-137">Tekintse át a nyomkövetési értékei, töltse le a nyomkövetési fájl a **ocp-apim-nyomkövetési-hely** URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="58e09-137">To review the values in the trace, download the trace file from the **ocp-apim-trace-location** URL.</span></span> <span data-ttu-id="58e09-138">Ez JSON formátumban a fájlt, és az alábbi példához hasonló bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="58e09-138">It is a text file in JSON format, and contains entries similar to the following example.</span></span>

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
                    "message": "Request is being forwarded to the backend service.",
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
                    "message": "Response headers have been sent to the caller. Starting to stream the response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming to the caller is complete."
                }
            }
        ]
    }
}
```

## <span data-ttu-id="58e09-139"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58e09-139"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="58e09-140">Tekintse meg a házirend-kifejezések helyesen a nyomkövetés bemutatója [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span><span class="sxs-lookup"><span data-stu-id="58e09-140">Watch a demo of tracing policy expressions in [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span></span> <span data-ttu-id="58e09-141">Előre 21:00 a bemutató megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="58e09-141">Fast-forward to 21:00 to see the demo.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
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




