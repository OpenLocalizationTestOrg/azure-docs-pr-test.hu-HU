---
title: "aaaWindows Phone Silverlight SDK frissítési eljárások"
description: "Windows Phone Silverlight SDK frissítési eljárásokat és Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d72e7b8a59ef2c0a95b22efbf1e5257271399ddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK frissítési eljárások
Ha Ön már rendelkezik integrált egy régebbi verzióját az SDK az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban. Például ha telepít át 0.10.1 hello kövesse toofirst rendelkezik too0.11.0 "0.9.0-s a too0.10.1" eljárást akkor hello "0.10.1 a too0.11.0" eljárás.

## <a name="from-200-too330"></a>A 2.0.0 too3.3.0
### <a name="test-logs"></a>Vizsgálati naplók
Hello SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve. toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-too200"></a>1.1.1 a too2.0.0
hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba. 

> [!IMPORTANT]
> Capptain és a Mobile Engagement nem hello azonos szolgáltatások és hello alábbi eljárás csak emel ki, hogyan toomigrate hello ügyfélalkalmazást. Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement kiszolgálókról
> 
> 

Ha egy korábbi verziójáról telepít, adjon hello Capptain webhely toomigrate too1.1.1 először tekintse át, akkor alkalmazza a következő eljárás hello

### <a name="nuget-package"></a>Nuget-csomag
Cserélje le **Capptain.WindowsPhone** által **MicrosoftAzure.MobileEngagement** Nuget-csomagot.

### <a name="applying-mobile-engagement"></a>Mobilmarketing alkalmazása
hello SDK hello kifejezést használja `Engagement`. A projekt toomatch kell tooupdate ezt a módosítást.

Az aktuális Capptain nuget-csomagot kell toouninstall. Vegye figyelembe, hogy eltávolítja-e Capptain erőforrások mappában összes módosítást. Ha azt szeretné, hogy tookeep azokat a fájlokat készítsen egy másolatot kapnak.

Ezt követően telepítse a projekt hello új Microsoft Azure Engagement nuget-csomagot. Megtalálhatja közvetlenül a [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Ez a művelet cserél összes erőforrás fájlok Engagement használják, és hozzáadja a hello új Engagement DLL tooyour projektre hivatkozik.

Hogy tooclean a projekt hivatkozásait Capptain DLL hivatkozások törlésével. Ha ez nem teszi meg, Capptain hello verziója ütközésbe fognak kerülni, és hiba történik.

Ha testreszabott Capptain erőforrások, a régi fájlok tartalmát, és illessze be őket hello új Engagement fájlokat. Vegye figyelembe, hogy xaml-és cs is rendelkezik-e a frissített toobe.

Ha ezek lépéseket csak akkor kell Capptain hivatkozásai tooreplace hello új Engagement hivatkozást.

1. Összes Capptain névtér toobe frissíteni kell.
   
    Mielőtt áttelepítése:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    : Az áttelepítés után
   
        using Microsoft.Azure.Engagement;
2. Minden Capptain osztály, amely tartalmazza a "Capptain" tartalmaznia kell "Engagement".
   
    Mielőtt áttelepítése:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    : Az áttelepítés után
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. Az xaml-fájlokkal Capptain névtér és attribútumok is módosíthatja.
   
    Mielőtt áttelepítése:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    : Az áttelepítés után
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Hello a további erőforrások, például a Capptain képek, ne feledje, hogy azok is történt átnevezett toouse "Engagement".

### <a name="application-id--sdk-key"></a>Alkalmazásazonosító / SDK kulcs
Bevonási egy kapcsolati karakterláncot használ. Egy alkalmazás Azonosítót és egy Mobile Engagement SDK kulcs toospecify nincs, így nem toospecify egy kapcsolati karakterláncot. Állíthat be azt a EngagementConfiguration fájlokat.

hello Engagement konfigurációs állítható be a `Resources\EngagementConfiguration.xml` fájlt a projekt.

A fájl toospecify szerkesztése:

* Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.

Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

hello kapcsolati karakterlánc az alkalmazás hello klasszikus Azure portálon jelenik meg.

### <a name="items-name-change"></a>Elemek kiszolgálónév-változás
Minden elem nevű *capptain* megnevezett *engagement*. Ehhez hasonlóan az *Capptain* túl*Engagement*.

Példák a gyakran használt Capptain elemeket:

* Most nevű EngagementConfiguration CapptainConfiguration
* Most nevű EngagementAgent CapptainAgent
* Most nevű EngagementReach CapptainReach
* Most nevű EngagementHttpConfig CapptainHttpConfig
* Most nevű GetEngagementPageName GetCapptainPageName

Vegye figyelembe, hogy az Átnevezés is hatással van a többszörösen definiált metódusok.

