---
title: "aaaAdvanced konfigurációs az univerzális Windows alkalmazások Engagement SDK"
description: "Speciális konfigurációs beállítások, az Azure Mobile Engagement az univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 23bd05012bc25d438d8d4985a112280bed0292b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Speciális Windows univerzális alkalmazások Engagement SDK konfigurációja
> [!div class="op_single_selector"]
> * [Univerzális Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Ez az eljárás ismerteti, hogyan tooconfigure Azure Mobile Engagement Android-alkalmazások különböző konfigurációs beállításait.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Speciális konfiguráció
### <a name="disable-automatic-crash-reporting"></a>Automatikus összeomlási jelentések letiltása
Bármikor letilthatja hello automatikus összeomlási jelentéskészítési funkció bekapcsolása. Majd amikor nem kezelt kivétel történik, a bevonási nincs semmi hatása.

> [!WARNING]
> Ha letiltja ezt a szolgáltatást, akkor az alkalmazás egy nem kezelt összeomlás esetén Engagement nem küld hello összeomlási **és** hello munkamenet és feladatok nem zárja be.
> 
> 

toodisable automatikus összeomlási reporting, attól függően, hogy hello módon deklarált meg azt a konfigurációs testreszabása:

#### <a name="from-engagementconfigurationxml-file"></a>A `EngagementConfiguration.xml` fájl
Állítsa be a jelentés túl összeomlik miattuk`false` közötti `<reportCrash>` és `</reportCrash>` címkék.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>A `EngagementConfiguration` futási időben objektum
Állítsa be a jelentés összeomlási toofalse a EngagementConfiguration objektum használatával.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Tiltsa le a valós idejű reporting
Alapértelmezés szerint hello Engagement service-jelentéseken naplók valós időben. Ha az alkalmazás naplók gyakran, jelenti jobb toobuffer hello naplók és tooreport őket egyszerre rendszeres időközönként alapon. Ennek elnevezése "kapacitásnövelés mód".

toodo tehát hello metódus hívása:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

hello argumentum értéke a **ezredmásodperc**. Bármikor tooreactivate hello valós idejű naplózás, bármilyen paraméter nélkül, vagy hello 0 értékű hello metódus hívható meg.

Kapacitásnövelés mód némileg növeli a hello eszközakkumulátor élettartamának, azonban hatással van hello Engagement figyelő: minden munkamenetek és feladatok időtartama vannak kerekítve toohello kapacitásnövelés küszöbértéket (így munkamenetek és feladatok rövidebb, mint a hello kapacitásnövelés küszöbértéke nem lehet látható). A 30000 (30s) mint kapacitásnövelés küszöbérték használatát javasoljuk. Mentett naplókat még korlátozott too300 elemek. Ha a küldő túl hosszú, néhány naplók elveszhet.

> [!WARNING]
> hello kapacitásnövelés küszöbértéke nem lehet 1-nél kisebb konfigurált tooa időszak második. Ha így tesz, a hello SDK hello hiba: a nyomkövetési jeleníti meg, és automatikusan visszaállítja az alapértelmezett érték toohello, nulla másodperc. Az eseményindítók hello SDK tooreport hello naplózza valós idejű.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
