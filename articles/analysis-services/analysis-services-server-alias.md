---
title: "Az Azure Analysis Services alias kiszolgálónevekkel |} Microsoft Docs"
description: "Ismerteti, hogyan történő létrehozásáról és használatáról a kiszolgáló neve az alias."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: a947dde1551c653faa54f088c1712c41c7657aa0
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/14/2017
---
# <a name="alias-server-names"></a>Másodlagos kiszolgálók nevei

A kiszolgáló neve alias használatával a felhasználók kapcsolódhatnak az Azure Analysis Services-kiszolgáló egy rövidebb a *alias* helyett a kiszolgáló nevét. Egy ügyfélalkalmazás kapcsolódáskor az alias egy végpont használatával van megadva a **hivatkozás: / /** protokoll formátumban. A végpont használata esetén csatlakozhassanak majd a tényleges kiszolgáló nevét adja vissza.

Kiszolgáló aliasneveket jól használhatók:

- Áttelepítése modellek nem befolyásolja a felhasználók kiszolgálók között. 
- Kiszolgáló felhasználóbarát nevek megkönnyíthető a felhasználóknak ne feledje. 
- Közvetlen felhasználók számára a különböző napszakokban különböző kiszolgálókon. 
- Közvetlen felhasználó különböző régiókban példányokhoz földrajzilag közelebb, például az Azure Traffic Manager használata esetén. 

A HTTP-végpont ad vissza egy érvényes Azure Analysis Services-kiszolgáló neve alias szolgálhatnak.

![A hivatkozási formátumot használó alias](media/analysis-services-alias/aas-alias-browser.png)

Ha egy ügyfél, a másodlagos kiszolgáló nevét is meg kell adni használatával **hivatkozás: / /** protokoll formátumban. Például a Power BI Desktop:

![A Power BI Desktop-kapcsolat](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>-Alias létrehozása

Hozzon létre egy alias végpontot, használhatja bármelyik módszert, amely egy érvényes Azure Analysis Services-kiszolgáló nevét adja vissza. Például egy fájl az Azure Blob Storage valós kiszolgálót tartalmazó mutató hivatkozás neve, vagy hozzon létre és ASP.NET Web Forms keretrendszerre alkalmazás közzététele.

Ebben a példában a Visual Studio egy ASP.NET webes űrlapalkalmazás jön létre. A mesterlap referencia- és felhasználói vezérlő el lesznek távolítva a Default.aspx lap. A default.aspx fájl tartalma egyszerűen a következő lap direktívájában:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

A Page_Load esemény Default.aspx.cs a Response.Write() metódus használatával adja vissza az Azure Analysis Services-kiszolgáló neve.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Lásd még:

[Ügyfél-függvénytárak](analysis-services-data-providers.md)   
[Csatlakoztassa a Power BI Desktop](analysis-services-connect-pbi.md)
