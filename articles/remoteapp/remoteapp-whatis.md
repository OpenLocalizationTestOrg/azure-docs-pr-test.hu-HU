---
title: aaaWhat az Azure RemoteApp? | Microsoft Docs
description: "Megtudhatja, hogyan tooshare alkalmazásokat és erőforrásokat tooany eszköz Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: d7a8a311-e70a-4463-ac85-c7f62c500921
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e13909f3a5698f58c7e4348f3ce53f43560f9b1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-remoteapp"></a>Mi az az Azure RemoteApp?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp megteremti hello a helyszíni Microsoft RemoteApp program, távoli asztali szolgáltatások segítségével tooAzure által támogatott hello funkcióit. Az Azure RemoteApp segítségével biztonságos távoli elérést tooapplications számos különböző felhasználói eszközről biztosítható. Az Azure RemoteApp hello felhő alapvetően nem állandó terminálkiszolgáló-munkameneteket üzemeltet, és toouse le azokat, és a felhasználókkal való megosztására.

Az Azure RemoteApp segítségével szinte bármilyen eszközön megoszthat alkalmazásokat és erőforrásokat a felhasználókkal. Kezdeményezünk hello felhőben, az alkalmazások, ami azt jelenti, gondoskodunk a Microsoft hello hardver és a méretezésről toomeet felhasználói igényeknek. Mindössze meg kell toodo hello alkalmazások tooshare szeretne, és majd kérje le a felhasználók toouse az alkalmazások feltöltése. [A felhasználónál tookeep a saját eszközeiket](remoteapp-clients.md), míg minden hello Azure-portálon keresztül kezelheti. Lehetősége van arra is hello lehetőség a vállalati hitelesítő adataival, így biztosíthatja az alkalmazások és adatok hello biztonsági.

Az alábbiakban további információkat talál az Azure RemoteAppról, vagy ha máris meggyőztük, [próbálja ki most](https://azure.microsoft.com/services/remoteapp/).

Kérdései vannak az Azure RemoteAppról? Tekintse meg a [gyakori kérdéseket](remoteapp-faq.md).

Az Azure RemoteApp hello része [Microsoft virtuális asztali infrastruktúra](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Újdonság!** Azure RemoteApp szolgáltatással kapcsolatban további toolearn szeretné? Vagy készen áll az Azure RemoteApp toovalidate léptékű? Csatlakozzon a hetente megtartott [kérje meg a hello szakértői válaszok webes szemináriumhoz](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp-gyűjtemények
Kétféle [Azure RemoteApp-gyűjtemény](remoteapp-collections.md) létezik:

* A **felhőalapú gyűjtemény** üzemel, és tárolja az adatokat a program hello felhő. A felhasználók a Microsoft-fiókjukkal, illetve az Azure Active Directory-val szinkronizált vagy összevont vállalati hitelesítő adataikkal jelentkezhetnek be és érhetik el az alkalmazásokat.
  
    Válassza ki a felhőalapú gyűjtemény Ha azt szeretné, hogy tooshare hello alkalmazás nincs szüksége egy kapcsolati tooany erőforrást a vállalat magánhálózatán (például a VPN-eszközhöz). Ha hello alkalmazás erőforrások hello Internet, OneDrive, vagy Azure, a felhőalapú gyűjtemény meg fog működni. Akkor is hello leggyorsabb toocreate.
* A **hibrid gyűjtemény** üzemel, és tárolja az hello Azure felhőben, de is megadható, hogy felhasználók hozzáférést adatokhoz és erőforrásokhoz, a helyi hálózaton tárolt adatokat. A felhasználók az Azure Active Directoryval szinkronizált vagy összevont vállalati hitelesítő adataikkal jelentkezhetnek be, és érhetik el az alkalmazásokat.
  
    Válasszon egy hibrid gyűjteményt, ha szüksége van egy kapcsolat tooresources személyes a vállalati hálózaton. Például ha hello alkalmazás hello következő tooone kell elérni:
  
  * Az intraneten található fájlkiszolgálók
  * Quicken
  * Tűzfal mögött lévő adatbázisok
    
    Ez általában hasznosabb a nagy vállalatok sok erőforrásuk található a magánhálózatukon, amelyek nem lehet áthelyezni toohello felhő.

hello különböző gyűjteményekhez különböző lehetőségek érhetők, beleértve a hálózatokat, ezért mérje fel, [melyik gyűjtemény](remoteapp-collections.md) legjobb az Ön. 

### <a name="updating-your-collection"></a>A gyűjtemény frissítése
Hello hello hibrid és a felhőalapú gyűjtemények közötti legfőbb különbségek egyike a szoftverfrissítések kezelésének módja. Felhőalapú gyűjtemény hello előre telepített Office 365 ProPlus vagy az Office 2013 lemezképet használó nem rendelkeznek tooworry frissítésekről. hello szolgáltatás karbantartja magát, és folyamatosan, tooboth alkalmazások és operációs rendszer hello bevezeti a frissítéseket.

A hibrid gyűjtemények, valamint egy egyéni sablonrendszerképet használó felhőalapú gyűjtemények esetében áll feladata hello rendszerkép és az alkalmazások karbantartása. A tartományhoz csatlakoztatott rendszerképek esetében olyan eszközökkel vezérelheti a frissítéseket, mint a Windows Update, a Csoportházirend vagy a System Center.

Miután frissítette az egyéni sablon rendszerképet, akkor feltöltése hello új lemezkép toohello Azure-felhőbe, és frissítse a hello gyűjtemény toouse hello új lemezképet. (Ehhez az Azure RemoteApp hello **gyors üzembe helyezés** lapon vagy irányítópult hello.)

További információk: [A gyűjtemény frissítése](remoteapp-update.md).

## <a name="supported-remoteapp-clients"></a>Támogatott RemoteApp-ügyfelek
Az Azure RemoteApp támogatott hello RemoteApp-ügyfélalkalmazásokon Windows és Windows RT, valamint hello Microsoft távoli asztali alkalmazások Mac, iOS és Android rendszerhez. A felhasználók ezen alkalmazások használatához a mobileszközeiken vagy számítási eszközök tooaccess hello új Azure RemoteApp-programok.

Lásd: [fér hozzá az alkalmazásokban az Azure Remoteappban](remoteapp-clients.md) hello ügyfelek kapcsolatos további információk.

## <a name="next-steps"></a>Következő lépések
Rajta! Próbálja ki! A következő cikkek segítenek az Azure RemoteApp első lépéseiben:

* [What kind of collection do you need for Azure RemoteApp?](remoteapp-collections.md) (Milyen típusú gyűjteményre van szüksége az Azure RemoteApphoz?)
* [Create an Azure RemoteApp image](remoteapp-imageoptions.md) (Azure RemoteApp-rendszerkép létrehozása)
* [Hogyan toocreate az Azure RemoteApp felhőalapú gyűjtemény](remoteapp-create-cloud-deployment.md)
* [Hogyan toocreate egy Azure RemoteApp hibrid gyűjteményének](remoteapp-create-hybrid-deployment.md)
* [How does licensing work in Azure RemoteApp?](remoteapp-licensing.md) (Hogyan működik a licenckezelés az Azure RemoteAppban?)
* [Best practices for using Azure RemoteApp](remoteapp-bestpractices.md) (Ajánlott eljárások az Azure RemoteApp használatához)
* [Azure RemoteApp FAQ](remoteapp-faq.md) (Azure RemoteApp – gyakori kérdések)

### <a name="help-us-help-you"></a>Segítsen nekünk, hogy segítsünk
Tudta, hogy a hozzáadása toorating ebben a cikkben és a hozzászólások írása az alábbi tehet módosítások toohello cikkbe? Valami hiányzik? Valami nem működik? Írtam valami olyat, ami nem egyértelmű? Görgessen fel, és kattintson a **Szerkesztés a Githubon** vagy **szerkesztése** toomake módosítások - ezek hozzánk kerülnek jóváhagyásra toous, és majd, miután jóváhagytuk őket, itt fognak megjelenni a módosításokat és fejlesztéseket azonnal itt.

