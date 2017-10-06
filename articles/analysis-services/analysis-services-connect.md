---
title: Analysis Services aaaConnect tooAzure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconnect tooand adatokat lekérni az Analysis Services-kiszolgálóhoz, az Azure-ban."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 5df94492feb48034f156b72e83e1009683988fc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-analysis-services-server"></a>Csatlakozás tooan Azure Analysis Services-kiszolgáló

Ez a cikk ismerteti a kapcsolódó tooa kiszolgáló adatok modellezési és a felügyeleti alkalmazások, például az SQL Server Management Studio (SSMS) vagy SQL Server Data Tools (SSDT) használatával. Vagy az ügyfél jelentés az alkalmazások, például a Microsoft Excel-, Power BI Desktop, vagy egyéni alkalmazások. Kapcsolatok tooAzure Analysis Services HTTPS használatára.

## <a name="client-libraries"></a>Klienskódtárak
[Hello legújabb klienskódtárak beolvasása](analysis-services-data-providers.md)

Minden kapcsolatok tooa kiszolgálók, típusa, függetlenül a frissített AMO ADOMD.NET és OLEDB szalagtárak tooconnect tooand ügyféladapter az Analysis Services kiszolgálóval igényelnek. SSMS, a SSDT, az Excel 2016 és a Power bi-ban hello legújabb klienskódtárak van telepítve vagy havi kiadásainak frissítve. Azonban néhány esetben is lehet az alkalmazás nem rendelkezhet hello legújabb. Például amikor frissíti a házirendek késleltetést vagy az Office 365 frissítések esetén hello késleltetett csatorna.

## <a name="server-name"></a>Kiszolgálónév

Egy Analysis Services-kiszolgálóhoz az Azure-ban létrehozásakor meg kell adnia egy egyedi nevet és hello terület, ahol hello server létrehozott toobe. Hello kiszolgálónév megadása a kapcsolatot, hello server elnevezési rendszer esetén:

```
<protocol>://<region>/<servername>
```
 Ahol-jére protokoll **asazure**, régió hello Uri, amelyen létrehozták hello server (például westus.asazure.windows.net) és kiszolgálónév hello régión belül egyedi kiszolgálóját hello neve.

### <a name="get-hello-server-name"></a>Hello kiszolgáló nevének beolvasása
A **Azure-portálon** > server > **áttekintése** > **kiszolgálónév**, másolása hello teljes kiszolgálónevet. Ha a munkahely más felhasználóinak túl toothis kiszolgáló kapcsolódik, ezt a kiszolgálónevet megoszthatja velük. Ha a kiszolgáló nevét, hello teljes elérési útját kell használni.

![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Kapcsolati karakterlánc

TooAzure kapcsolódás esetén az Analysis Services használata hello táblázatos objektummodell, a következő kapcsolati karakterlánc-formátumairól használata hello:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrált Azure Active Directory-hitelesítés
Integrált hitelesítés szerzi be hello Azure Active Directory hitelesítő adatok gyorsítótárában ha rendelkezésre áll. Ha nem, hello Azure bejelentkezési ablak jelenik meg.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Az Azure Active Directory authentication felhasználónévvel és jelszóval

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows-hitelesítés (integrált biztonsági)
Hello aktuális folyamatának futtatása hello Windows-fiókot használni.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Csatlakozás .odc fájl használatával
Az Excel régebbi verzióival felhasználók Office Data Connection (.odc) fájl használatával is elérheti tooan Azure Analysis Services-kiszolgálóhoz. több, lásd: toolearn [Office Data Connection (.odc) fájl létrehozása](analysis-services-odc.md).


## <a name="next-steps"></a>Következő lépések
[Csatlakozás az Excel használatával](analysis-services-connect-excel.md)    
[Csatlakozás a Power BI](analysis-services-connect-pbi.md)   
[A kiszolgáló kezelése](analysis-services-manage.md)   

