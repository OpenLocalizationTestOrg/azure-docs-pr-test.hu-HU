---
title: "a Microsoft Azure-felhő Foundry elindítva aaaGetting |} Microsoft Docs"
description: "A Microsoft Azure OSS vagy döntő felhő Foundry futtató"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: 56300d5a0a75b5a9f46145a49ed3f5daa774375a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-foundry-on-azure"></a>Cloud Foundry az Azure-ban

Felhő Foundry egy nyílt forráskódú platform,--szolgáltatás (PaaS) létrehozása, telepítése és a különböző nyelv és keretrendszer 12-tényező alkalmazások operációs rendszer. Ez a dokumentum ismerteti a felhő Foundry futó Azure, és hogyan kezdheti rendelkezik hello-beállítások.

## <a name="cloud-foundry-offerings"></a>Foundry felhőajánlatokat

Nincsenek elérhető toorun felhő Foundry Azure kétféle: nyílt forráskódú felhő Foundry (OSS CF) és döntő felhő Foundry (PCF). OSS CF van egy teljes mértékben [nyílt forráskódú](https://github.com/cloudfoundry) hello felhő Foundry Foundation által kezelt felhő Foundry verzióját. Döntő felhő Foundry egy vállalati felhő Foundry döntő szoftver Inc. terjesztése Úgy tekintünk, néhány hello különbségei hello két ajánlatokat.

### <a name="open-source-cloud-foundry"></a>Nyílt forráskódú felhő Foundry

Először telepítése BOSH igazgató, és majd a felhő Foundry hello segítségével telepítésével OSS felhő Foundry Azure telepíthet [utasításokat a Githubon](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md). toolearn több OSS CF használatáról lásd: hello [dokumentáció](https://docs.cloudfoundry.org/) hello felhő Foundry Foundation által biztosított.

Microsoft közösségi csatornák a következő hello keresztül elérhető legjobb OSS CF támogatást nyújt a:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>a bosh-azure-cpi csatorna [felhő Foundry Slackhez](https://slack.cloudfoundry.org/)
- [CF-bosh levelezési lista](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub problémái hello [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) és [service broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> hello szintű támogatást az Azure-erőforrások, például a felhő Foundry futtató hello virtuális gépek az Azure támogatási foglalt alapul. Legjobb közösségi támogatás csak érvényes toohello felhő Foundry vonatkozó összetevőket.

### <a name="pivotal-cloud-foundry"></a>Foundry döntő felhő

Döntő felhő Foundry hello tartalmaz ugyanazon alapvető platformok hello OSS terjesztési, valamint egyéb felügyeleti eszközök és a nagyvállalati támogatással készlete. az Azure-on PCF toorun, be kell szerezni Pivotal licencet. hello PCF ajánlatot hello Azure piactér 90 napos próba licenc is tartozik.

hello eszközök közé tartozik [döntő Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), egy webes alkalmazás, amely egyszerűbbé teszi a központi telepítés és a felhő Foundry Foundation felügyeleti és [kulcsfontosságú alkalmazások Manager](https://docs.pivotal.io/pivotalcf/console/), egy webes alkalmazás kezelése felhasználók és az alkalmazások.

Ezenkívül toohello támogatási csatornáit OSS CF fent felsorolt, PCF licenc felügyeleti lehetőségeket kínál, toocontact Pivotal támogatásához. A Microsoft és Pivotal is engedélyezve van támogatási munkafolyamatok, amelyek lehetővé teszik toocontact bármelyik fél segítségért és a lekérdezés attól függően, hogy hol helyezkedik el hello probléma megfelelően irányított rendelkezik.

## <a name="azure-service-broker"></a>Az Azure Service Broker

Felhő Foundry bátorítja hello ["tizenkét tényezős alkalmazás"](https://12factor.net/) módszertan használatával, amely elősegíti a tiszta választani egymástól a állapot nélküli alkalmazások folyamatokat és az állapotalapú alkalmazások és szolgáltatások biztonsági szolgáltatások. [Szolgáltatás brókerek](https://docs.cloudfoundry.org/services/api.html) egy egységes módon tooprovision kínálnak, és biztonsági szolgáltatások tooapplications kötni. Hello [Azure service broker](https://github.com/Azure/meta-azure-service-broker) biztosít néhány hello kulcs Azure-szolgáltatások díjairól ezt a csatornát, beleértve az Azure storage és az Azure SQL.

Ha döntő felhő Foundry használ, hello service broker is [csempe érhető el](https://docs.pivotal.io/azure-sb/installing.html) a hello döntő hálózati.

## <a name="related-resources"></a>Kapcsolódó források (lehet, hogy a cikkek angol nyelvűek)

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services beépülő modul

Felhő Foundry kiválóan alkalmas tooagile szoftverfejlesztői, beleértve a folyamatos integrációs (CI) és a folyamatos kézbesítési (CD) hello használatát. Ha használja a Visual Studio Team Services (VSTS) toomanage a projekteket, és szeretné tooset célzó felhőalapú Foundry CI/CD adatcsatorna be, használhatja a hello [VSTS felhő Foundry összeállítási kiterjesztés](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension). hello beépülő modul köszönhetően egyszerű tooconfigure és automatizálhatja a központi telepítések tooCloud Foundry, hogy fut az Azure vagy a másik környezetet.

## <a name="next-steps"></a>Következő lépések

- [Hello Azure Piactérről származó döntő felhő Foundry telepítése](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Központi telepítése egy app tooCloud Foundry az Azure-ban](./cloudfoundry-deploy-your-first-app.md)
