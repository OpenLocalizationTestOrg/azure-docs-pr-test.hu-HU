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
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a><span data-ttu-id="6a62b-103">Microsoft Power BI Embedded előzetes verziójával hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="6a62b-103">Microsoft Power BI Embedded Preview troubleshooting</span></span>
<span data-ttu-id="6a62b-104">Ez a cikk választ ad arról, hogyan tootroubleshoot **Power BI Embedded**.</span><span class="sxs-lookup"><span data-stu-id="6a62b-104">This article provides answers for how  tootroubleshoot **Power BI Embedded**.</span></span>

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a><span data-ttu-id="6a62b-105">SQL Server kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="6a62b-105">Setting SQL Server connection strings</span></span>
<span data-ttu-id="6a62b-106">tooset egy SQL Server kapcsolati karakterlánc, adott formátumú toofollow kell.</span><span class="sxs-lookup"><span data-stu-id="6a62b-106">tooset a SQL Server connecting string, you need toofollow a specific format.</span></span> <span data-ttu-id="6a62b-107">Az alábbiakban van egy példa kapcsolati karakterláncot az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6a62b-107">Below is an example connection string for SQL Server.</span></span>

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

<span data-ttu-id="6a62b-108">További információ az SQL Server kapcsolati karakterláncok, toolearn lásd: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="6a62b-108">toolearn more about SQL Server connection strings, see hello following articles:</span></span>

* [<span data-ttu-id="6a62b-109">SQL Server kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="6a62b-109">SQL Server Connection Strings</span></span>](https://msdn.microsoft.com/library/jj653752.aspx)
* [<span data-ttu-id="6a62b-110">SqlConnection.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="6a62b-110">SqlConnection.ConnectionString</span></span>](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a><span data-ttu-id="6a62b-111">Hitelesítő adatok beállítása</span><span class="sxs-lookup"><span data-stu-id="6a62b-111">Setting credentials</span></span>
<span data-ttu-id="6a62b-112">Hitelesítő adatok fejlesztési vagy tesztelési környezetben, például a felhasználónevét és jelszavát, melyekben hello esetében szükség lehet a tooupdate hitelesítő adatokat, amelyek megfelelnek egy éles megoldás.</span><span class="sxs-lookup"><span data-stu-id="6a62b-112">In hello case where you have credentials for a development or staging environment, such as user name and password, you might need tooupdate credentials that match a production solution.</span></span>

## <a name="see-also"></a><span data-ttu-id="6a62b-113">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6a62b-113">See Also</span></span>
* [<span data-ttu-id="6a62b-114">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="6a62b-114">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)
* [<span data-ttu-id="6a62b-115">Mi az a Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="6a62b-115">What is Power BI Embedded</span></span>](power-bi-embedded-what-is-power-bi-embedded.md)

