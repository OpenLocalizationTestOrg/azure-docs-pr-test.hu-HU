---
title: "az Azure Notification Hubs apns aaaToken-alapú (HTTP/2) hitelesítési |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan tooleverage hello apns új tokent használó hitelesítés"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 3353d7f16033ce0b68edec9ee9aeb98f47faa1fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="token-based-http2-authentication-for-apns"></a>Az APNS-jogkivonatalapú (HTTP/2) hitelesítés
## <a name="overview"></a>Áttekintés
Ez a cikk részletesen, hogyan toouse hello új APNS HTTP/2-protokoll jogkivonat-alapú hitelesítés.

hello fő előnyei a hello új protokoll a következők:
-   Jogkivonatok létrehozásához viszonylag teljesen szabad (összehasonlított toocertificates)
-   Nincs több lejárati dátumokat – jogkörrel rendelkezik arra a hitelesítési tokenek és a visszavont tanúsítványok ellenőrzése
-   Másolatot too4 KB mostantól lehet hasznos adat található
- Szinkron visszajelzés
-   A legújabb protokoll Apple – tanúsítványok továbbra is használhatják a hello bináris protokoll, amely meg van jelölve érvénytelenítése

Ez új mechanizmus használatával is végrehajtható két lépésben néhány perc múlva:
1.  Hello szükséges adatok lekérését hello Apple Developer-fiók portáljára
2.  Az értesítési központ konfigurálása hello új adatokkal

A Notification Hubs mostantól minden set toouse hello új hitelesítési rendszere az apns-sel. 

Vegye figyelembe, hogy a beállítást, ha áttelepítette az APNS tanúsítvány hitelesítő adatait:
- hello token tulajdonságok felülírni a tanúsítványt a rendszerben,
- de az alkalmazás továbbra is tooreceive értesítések zökkenőmentesen.

## <a name="obtaining-authentication-information-from-apple"></a>Hitelesítési adatok beszerzése az Apple-től
tooenable jogkivonat-alapú hitelesítés kell hello Apple Developer fiókjából következő tulajdonságai:
### <a name="key-identifier"></a>Kulcsazonosító
az Apple fejlesztői fiókban hello "Kulcsok" oldalról hello kulcsazonosító érhető el

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Alkalmazásazonosító & alkalmazás nevét
hello alkalmazásnév hello App IDs lapján hello fejlesztői fiókjába keresztül érhető el. 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

hello alkalmazásazonosító hello tagsági részleteit megjelenítő oldalra hello fejlesztői fiókjába keresztül érhető el.
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>Hitelesítési jogkivonata
hello hitelesítésére szolgáló jogkivonat letölthető az alkalmazás jogkivonat létrehozása után. Hogyan toogenerate Ez a token, a részletekért tekintse meg túl[az Apple fejlesztői dokumentációját](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).

## <a name="configuring-your-notification-hub-toouse-token-based-authentication"></a>A notification hub toouse-jogkivonat-alapú hitelesítés konfigurálása
### <a name="configure-via-hello-azure-portal"></a>Hello Azure-portálon keresztül konfigurálása
tooenable jogkivonat-alapú hitelesítés hello portál, Azure-portálon toohello bejelentkezés, és folytassa a tooyour értesítési központ > értesítési szolgáltatások > APNS panel. 

Új tulajdonság – *hitelesítési mód*. Kiválasztása a Token lehetővé teszi a tooupdate a központ az összes hello vonatkozó token tulajdonság.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- Adja meg az Apple developer-fiók lekért hello tulajdonságait 
- Válassza ki az alkalmazás módot (éles vagy védőfal) 
- Kattintson a Mentés tooupdate APN szolgáltatás hitelesítő adatait. 

### <a name="configure-via-management-api-rest"></a>(REST) API Management szolgáltatáson keresztül konfigurálása

Használhatja a [felügyeleti API-k](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate a notification hub toouse-jogkivonat-alapú hitelesítést.
Attól függően, hogy konfigurálja a hello alkalmazás egy védőfal vagy éles alkalmazás (az Apple fejlesztői fiókban megadva) használja a megfelelő végpont hello közül:

- A védőfal végpont: [https://api.development.push.apple.com:443/3/eszköz](https://api.development.push.apple.com:443/3/device)
- Éles végpont: [https://api.push.apple.com:443/3/eszköz](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Jogkivonat-alapú hitelesítéshez, az API-verziót: **2017-04 vagy újabb**.
> 
> 

Íme egy példa a PUT kérés tooupdate jogkivonat-alapú hitelesítéssel hub:


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-hello-net-sdk"></a>Hello .NET SDK használatával konfigurálása
A központ toouse jogkivonat-alapú hitelesítés használatával konfigurálhatja a [legújabb ügyfél SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8). 

Íme egy hello helyes használatát bemutató példát:


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH tooYOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-toousing-certificate-based-authentication"></a>Visszaállítási toousing Tanúsítványalapú hitelesítés
Visszaállíthatja bármely idő toousing tanúsítványalapú hitelesítési helyett hello token tulajdonságok bármelyik előző metódus és sikeres hello tanúsítvány használatával. Korábban, hogy a művelet felülírja a hello tárolt hitelesítő adatokat.
