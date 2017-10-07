---
title: "aaaMicrosoft Power BI Embedded előzetes verziójával hibaelhárítása"
description: "Microsoft Power BI Embedded előzetes verziójával hibaelhárítása"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: c8aee652-ed8b-4c66-9c63-f798b7a655b4
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: a0a25cd73977c0ea0bd6b7c82e215412245771bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Microsoft Power BI Embedded előzetes verziójával hibaelhárítása
Ez a cikk választ ad arról, hogyan tootroubleshoot **Power BI Embedded**.

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a>SQL Server kapcsolati karakterláncok beállítása
tooset egy SQL Server kapcsolati karakterlánc, adott formátumú toofollow kell. Az alábbiakban van egy példa kapcsolati karakterláncot az SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

További információ az SQL Server kapcsolati karakterláncok, toolearn lásd: a következő cikkek hello:

* [SQL Server kapcsolati karakterláncok](https://msdn.microsoft.com/library/jj653752.aspx)
* [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a>Hitelesítő adatok beállítása
Hitelesítő adatok fejlesztési vagy tesztelési környezetben, például a felhasználónevét és jelszavát, melyekben hello esetében szükség lehet a tooupdate hitelesítő adatokat, amelyek megfelelnek egy éles megoldás.

## <a name="see-also"></a>Lásd még:
* [Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)
* [Mi az a Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)

