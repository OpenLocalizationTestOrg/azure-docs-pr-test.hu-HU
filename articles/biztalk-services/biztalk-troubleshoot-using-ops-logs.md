---
title: "aaaTroubleshoot BizTalk szolgáltatások műveletnaplók használatával |} Microsoft Docs"
description: "Végezzen hibaelhárítást a BizTalk szolgáltatások műveletnaplók használatával. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 102779ed6e29784f190c28e4102a7d9670614914
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk szolgáltatások: Műveletnaplók használata – hibaelhárítás

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-hello-operation-logs"></a>Mik azok a hello műveletnaplók
A műveletnaplók rendszerben érhető el, a klasszikus Azure-portál, amely lehetővé teszi tooview korábbi naplók az Azure-szolgáltatásokkal, beleértve a BizTalk szolgáltatások végre műveletek hello szolgáltatások szolgáltatás. Ez lehetővé teszi, hogy tooview előzményadatokat kapcsolódó toomanagement műveleteket végez a BizTalk szolgáltatás előfizetésének kötött 180 nap.

> [!NOTE]
> Ez a funkció csak rögzíti a naplókat, ha hello szolgáltatás elindult, például a BizTalk szolgáltatások, a felügyeleti műveleteihez biztonsági fel, és így tovább. Függetlenül attól, hogy a klasszikus Azure portálon hello vagy hello segítségével végrehajtandó műveletek követi [BizTalk szolgáltatás REST API-k](http://msdn.microsoft.com/library/azure/dn232347.aspx). Szolgáltatások használatával nyomon követett műveletek teljes listáját lásd: [műveletek nyomon követett Azure felügyeleti szolgáltatások használatával](#bizops).<br/><br/>
> Ez nem rögzíthető lemezkép a hello naplókat a további tevékenységek kapcsolódó tooBizTalk szolgáltatás futásideje (például hidak, és így tovább által feldolgozott üzenet.). tooview ezek a naplók használata hello követési nézet hello BizTalk szolgáltatások portálról. További információkért lásd: [üzenetek nyomon követése](http://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>BizTalk szolgáltatások műveletnaplók megtekintése
1. Hello a klasszikus Azure portálon, válassza ki **szolgáltatások**, majd válassza ki a hello **műveletnaplók** fülre.
2. Hello naplók előfizetés, dátumtartományt, szolgáltatás típusa (pl. BizTalk szolgáltatások), szolgáltatásnevet vagy hello művelet (sikeres, sikertelen) állapotának különböző paraméterek alapján szűrhetők.
3. Jelölje be hello pipa tooview hello szűrt listáról. hello következő kép bemutatja tevékenységek kapcsolódó tootestbiztalkservice: ![műveletnaplók megtekintése][ViewLogs] 
4. További információ az egy adott művelet tooview hello válassza ki, és kattintson a **részletek** hello tálcán hello lap alján.

## <a name="bizops"></a>Az Azure szolgáltatások segítségével nyomon műveletek
hello következő táblázat hello műveletek nyomon követett hello Azure szolgáltatások használatával.

| A művelet neve | Tevékenység |
| --- | --- |
| CreateBizTalkService |A művelet toocreate új BizTalk szolgáltatás |
| DeleteBizTalkService |A művelet toodelete BizTalk szolgáltatás |
| RestartBizTalkService |A művelet toorestart BizTalk szolgáltatás |
| StartBizTalkService |A művelet toostart BizTalk szolgáltatás |
| StopBizTalkService |A művelet toostop BizTalk szolgáltatás |
| DisableBizTalkService |A művelet toodisable BizTalk szolgáltatás |
| EnableBizTalkService |A művelet tooenable BizTalk szolgáltatás |
| BackupBizTalkService |A művelet tooback a BizTalk szolgáltatás |
| RestoreBizTalkService |A művelet toorestore megadott biztonsági másolatból BizTalk szolgáltatás |
| SuspendBizTalkService |A művelet toosuspend BizTalk szolgáltatás |
| ResumeBizTalkService |A művelet tooresume BizTalk szolgáltatás |
| ScaleBizTalkService |A BizTalk szolgáltatása művelet-tooscale feljebb vagy lejjebb |
| ConfigUpdateBizTalkService |A BizTalk szolgáltatás művelet tooupdate hello konfigurációját |
| ServiceUpdateBizTalkService |A művelet tooupgrade vagy alacsonyabb szintre való visszalépést BizTalk szolgáltatás tooa eltérő verziójú |
| PurgeBackupBizTalkService |A művelet toopurge biztonsági mentései hello BizTalk szolgáltatás hello megőrzési időn kívül |

## <a name="see-also"></a>Lásd még:
* [Biztonsági mentési BizTalk szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Állítsa vissza biztonsági másolatból BizTalk szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Kiépítési állapot diagramja](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Irányítópult, Figyelés és Méret lapok](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Szabályozás](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Kiállító neve és kiállító kulcsa](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

