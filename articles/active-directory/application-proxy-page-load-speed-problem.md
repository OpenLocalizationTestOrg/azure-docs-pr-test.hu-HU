---
title: "Alkalmazásproxy alkalmazás aaaAn túl hosszú tooload fogad |} Microsoft Docs"
description: "Elhárítása lap betöltési teljesítmény hello Azure AD alkalmazásproxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c7a51f96840966a1d88933fa4e30f39479d8a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-application-proxy-application-takes-too-long-tooload"></a>Az alkalmazásproxy alkalmazás túl hosszú tooload vesz igénybe.

Ez a cikk segítséget nyújtanak az Azure AD alkalmazásproxy-alkalmazás miért eltarthat egy hosszú ideig tooload, és milyen toounderstand teheti tooresolve probléma.

## <a name="overview"></a>Áttekintés
Ha az alkalmazások dolgozik, de a hosszú várakozási ideje fogja látni, lehet néhány kisebb beállításokból állnak a hálózati topológia, érdemes lehet a tooimprove hello sebessége. Különböző topológiák értékelése, lásd: hello [hálózati szempontok dokumentum](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Ha ezeket a szempontokat nem segít, sajnos nem rendelkezik jelenleg tudunk teljesítményhangolás további javaslatokat. Hello alkalmazásproxy-szolgáltatás, amely lehet, hogy szorosabb tooyou toomore adatközpontok bővíti, mert közvetlenül megkezdheti továbbfejlesztett toosee késés. toosee hello a teljes listáját az Azure data adatközpontok úgy is, hogy hello [késés tesztoldalt](http://www.azurespeed.com/Azure/Latency). 

hello adatközpontjaiban hello alkalmazásproxy-szolgáltatás található a hello [összekötő portok vizsgálati eszköz](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Alkalmazásproxy data center helyeken visszajelzés 
Előfordulhat, hogy Azure-adatközpont, amelyek még nem jelennek meg a alkalmazásproxy, de átmásolásának tooa nagy késleltetésű fokozása meg. hello adatközpont a(z) < aadapfeedback@microsoft.com > tehát használhatjuk a visszajelzés tooplan, mivel azt a csomópontot.

Jelenleg néhány további képességeket, amelyek a bérlők számára, amelyet jelenleg a hosszú késések hello késés fokozásához dolgozik, és meg arról, hogy tooshare dokumentáció után érhető el.

## <a name="next-steps"></a>Következő lépések
[A meglévő helyszíni proxykiszolgálókkal működik](application-proxy-working-with-proxy-servers.md)
