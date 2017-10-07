---
title: "aaaAzure App Service Environment-környezet felügyeleti címei"
description: "Listák hello felügyeleti címek használt toocommand egy App Service Environment-környezet"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: b34b6266dc3a35915421b14bf34eddc07c2825c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-management-addresses"></a>App Service Environment-környezet felügyeleti címei

App Service Environment(ASE) hello hello Azure App Service egy alhálózatba az Azure Virtual Network (VNet) a központi telepítését.  hello ASE hello Azure App Service elérhetőknek kell lenniük, hogy a fiók kezelhető.  Ez ASE felügyeleti forgalom halad át a felhasználó által vezérelt hálózati hello.  Azure App Service management kiszolgálók toohello nyilvános VIP kapcsolódó hello mértékéig származik.  Hello ASE részleteinek a hálózat függőségek olvasási [hálózati szempontok és az App Service Environment-környezet hello][networking].  Általános információk a hello ASE Kezdésként használhatja az [bemutatása toohello App Service Environment-környezet][intro].

Ez a dokumentum hello forrás IP-címet a felügyeleti forgalom toohello ASE sorolja fel. Használja a címek toocreate hálózati biztonsági csoportok toolock le bejövő forgalmat, vagy azok használatát az útvonaltáblák igény szerint.  toouse toouse kell ezt az információt:

* Minden egyes felsorolt hello IP-címek
* hello IP-címek, amelyek a ASE rendszerbe való egyezés toohello régió.

hello bejövő felügyeleti forgalom származik következő IP-címek tooports 454 és a 455.

| Régió | Címek |
|--------|-----------|
| Minden egyes | 70.37.57.58, 157.55.208.185, 52.174.22.21,13.94.149.179,13.94.143.126,13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141 |
| USA déli középső RÉGIÓJA & északi USA középső RÉGIÓJA | 23.102.188.65, 191.236.154.88 |
| Ausztrália délkeleti & Kelet-Ausztrália | 23.101.234.41, 104.210.90.65 |
| USA nyugati régiója & USA keleti régiója | 104.45.227.37, 191.236.60.72 |
| Nyugat-Európában & Észak-Európa | 191.233.94.45, 191.237.222.191 |
| Központi USA nyugati régiója & 2 USA nyugati régiója | 13.78.148.75, 13.66.225.188 |
| USA középső RÉGIÓJA & USA keleti régiója 2. régiója | 104.43.165.73, 104.46.108.135 |
| Kelet-Ázsia & Délkelet-Ázsia | 23.99.115.5, 104.215.158.33 |
| Kelet-japán & Nyugat-japán | 104.41.185.116, 191.239.104.48 |
| Kanadában központi & Kanada keleti régiója | 40.85.230.101, 40.86.229.100 |
| Egyesült Királyság nyugati régiója & Egyesült Királyság déli régiója | 51.141.8.34, 51.140.185.75 |
| Koreai Dél & Koreai központi | 52.231.200.177, 52.231.32.117 |
| Dél-Brazília & USA déli középső RÉGIÓJA| 104.41.46.178, 23.102.188.65 |
| Közép-Indiában és Dél-India | 104.211.98.24, 104.211.225.66 |
| Nyugat-Indiában és Dél-India | 104.211.160.229, 104.211.225.66 |


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
