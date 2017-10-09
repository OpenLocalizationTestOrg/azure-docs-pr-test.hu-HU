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
# <a name="how-toouse-hello-api-inspector-tootrace-calls-in-azure-api-management"></a>Hogyan toouse hello API Inspector tootrace meghívja az Azure API Management
Az API Management biztosít egy API-Inspector eszköz toohelp, a Hibakeresés és hibaelhárítás az API-kat. hello API Inspector programozott módon használható, és közvetlenül a hello fejlesztői portálján is használható. 

Ezenkívül tootracing műveletek API Inspector is hívásláncainak [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx) értékelések. Bemutatója, lásd: [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too21:00 előre.

Ez az útmutató egy segédlet API Inspector használatával.

> [!NOTE]
> API-Inspector nyomkövetések csak jön létre, és toohello tartozó előfizetés kulcsok tartalmazó kérelmek felhasználhatóvá [rendszergazda](api-management-howto-create-groups.md) fiók.
> 
> 

## <a name="trace-call"></a> Használata API-Inspector tootrace hívása
toouse API Inspector hozzáadása egy **ocp-apim-nyomkövetési: true** kérelem fejléc tooyour művelet meghívásának, majd töltse le és hello nyomkövetési hello által jelzett hello URL-cím használatával vizsgálhatja **ocp-apim-nyomkövetési-hely** Válaszfejléc. Programozott módon lehet végrehajtani, és közvetlenül a hello fejlesztői portálján is elvégezhető.

Ez az oktatóanyag bemutatja, hogyan toouse hello API Inspector tootrace műveletekbe hello alapvető Számológép API hello konfigurált [kezelése az első API](api-management-get-started.md) használatába bevezető oktatóanyagot. Ha még nem fejeződött be, hogy az oktatóanyag mindössze néhány perc múlva tooimport hello alapvető Számológép API vagy egy másik API-hoz a például hello Echo API kiválasztása. Minden API Management service-példány az Echo API-k, melyek a használt tooexperiment és API-kezeléssel kapcsolatos további tudnivalók az előre konfigurált származnak. hello Echo API bármilyen bemeneti tooit küldött vissza visszaadása. toouse, hívhat meg a HTTP-műveletet, és a visszatérési érték hello egyszerűen lesz az Ön által küldött. 

tooget elindítani, kattintson a **fejlesztői portálján** a hello Azure portál, az API Management szolgáltatás. Műveletek hívható közvetlenül a hello fejlesztői portálján, amely egy kényelmes módszert arra tooview biztosít, és tesztelje az API-k hello működésére.

> Ha még nem hozott az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

![Az API Management fejlesztői portálján][api-management-developer-portal-menu]

Kattintson a **API-k** hello felső menüben, majd kattintson a **alapvető Számológép**.

![Echo API][api-management-api]

Kattintson a **kipróbálás** tootry hello **két egész számok hozzáadása** műveletet.

![Kipróbálom][api-management-open-console]

Megtartása hello alapértelmezett paraméterértékek, és válassza hello előfizetés kulcs kívánt hello termék toouse hello **előfizetés-kulcs** legördülő listán.

Alapértelmezés szerint a hello developer portálon hello **Ocp-Apim-nyomkövetési** fejléc már be van állítva túl**igaz**. Ezt a fejlécet konfigurálja-e a nyomkövetési jön létre.

![Küldés][api-management-http-get]

Kattintson a **küldése** tooinvoke hello műveletet.

![Küldés][api-management-send-results]

Hello válaszul fejlécek kerülnek egy **ocp-apim-nyomkövetési-hely** egy hasonló toohello érték, a következő példa a.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

hello nyomkövetési tölthető le: hello megadott helyre, és tekintse át, ahogyan az következő lépés hello. Ne feledje, hogy csak hello utolsó 100 naplóbejegyzések tárolt naplók helye újra felhasználja a rendszer az elforgatástól. Igen, ha több mint 100 hívásokat a kérelmek nyomon követése engedélyezve végül hozzákezdhet felülírja a hello első nyomkövetések helyen.

## <a name="inspect-trace"></a>Hello nyomkövetési vizsgálata
hello nyomkövetési tooreview hello értékek hello nyomkövetési fájl letöltését hello **ocp-apim-nyomkövetési-hely** URL-CÍMÉT. Ez JSON formátumban a fájlt, és bejegyzéseket a következő példa hasonló toohello tartalmazza.

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

## <a name="next-steps"></a>Következő lépések
* Tekintse meg a házirend-kifejezések helyesen a nyomkövetés bemutatója [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Előretekerés too21:00 toosee hello bemutató.

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




