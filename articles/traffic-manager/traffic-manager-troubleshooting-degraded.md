---
title: "az Azure Traffic Manager \"csökkentett teljesítményű\" aaaTroubleshooting állapota"
description: "Hogyan \"tootroubleshoot Traffic Manager-profilok azt mutatja, mint amikor csökkentett teljesítményű\" állapota."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
ms.assetid: 8af0433d-e61b-4761-adcc-7bc9b8142fc6
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: kumud
ms.openlocfilehash: fd95697781472b52e98d856e66beb7b89dfeaf23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Hibaelhárítás "csökkentett teljesítményű" állapota az Azure Traffic Manager

Ez a cikk ismerteti, hogyan tootroubleshoot az Azure Traffic Manager-profilt, amely azt leromlott állapot. Ebben az esetben fontolja meg, hogy konfigurálta a cloudapp.net üzemeltetett szolgáltatások toosome mutató Traffic Manager-profil. Ha a Traffic Manager hello állapotát jeleníti meg a **csökkentett teljesítményű** állapotát, majd egy vagy több végpontot hello állapotának lehet **csökkentett teljesítményű**:

![csökkent végpont állapota](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Ha a Traffic Manager hello állapotát jeleníti meg egy **inaktív** állapot, akkor mindkét végpontok lehetnek **letiltott**:

![Inaktív Traffic Manager-állapot](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Understanding Traffic Manager-mintavétel

* A TRAFFIC Manager úgy véli, hogy egy végpont toobe csak akkor, ha hello mintavételi egy 200-as HTTP-válasz fogadása ONLINE biztonsági hello mintavételi elérési útról. A rendszer hibát bármely más nem 200 választ.
* 30 x átirányítást nem sikerül, akkor is, ha hello átirányított URL-címet adja vissza a 200-as.
* A HTTPs mintavételek menüpontban hitelesítési hibák figyelmen kívül lesznek hagyva.
* hello tényleges tartalmat hello mintavételi elérési út nem számít, mindaddig, amíg egy 200 adja vissza. Egy URL-cím toosome statikus tartalom hasonló probing "/ favicon.ico" egy általános módszer. Dinamikus tartalom, például a hello ASP-lapok, előfordulhat, hogy mindig vissza 200, akkor is, ha hello alkalmazás állapota kifogástalan.
* Az ajánlott eljárás, amely rendelkezik elegendő logikai toodetermine, amely a hely hello elérési toosomething felfelé vagy lefelé mintavételi tooset hello. "Hello az előző példában úgy, hogy elérési too"/favicon.ico hello szövegrészt, hogy csak adott w3wp.exe tesztelés válaszol. Ez a Hálózatfigyelő is jelzi, hogy a webalkalmazás működik megfelelően. Jobb megoldás lehet egy elérési utat tooa tooset valamit, mint "/ Probe.aspx", amely rendelkezik logika toodetermine hello hello hely állapotát. Használhatja például a teljesítmény számlálók tooCPU kihasználtsági vagy mérték hello sikertelen kérések száma. Vagy adatbázis-erőforrások tooaccess vagy munkamenet-állapot toomake, hogy működik-e hello webalkalmazás sikertelen kísérlet.
* Ha egy profil végpontjai állapotromlást, Traffic Manager kezeli az összes végpontok állapota kiváló, és a útvonalak forgalom tooall végpontok. Ez a viselkedés garantálja, hogy mechanizmus probing hello problémákat nem eredményez a szolgáltatás egy teljes leállás.

## <a name="troubleshooting"></a>Hibaelhárítás

mintavételi hiba tootroubleshoot, kell eszközzel hello HTTP-állapotkód visszatérési hello mintavételi URL-címről. Nincsenek elérhető számos eszközt, hogy Ön hello nyers HTTP-válasz megjelenítése.

* [Fiddler](http://www.telerik.com/fiddler)
* [curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Is használhatja az Internet Explorer tooview hello HTTP-válaszok F12 hibakeresési eszközök hello hello hálózati lapján.

Ehhez a példához azt szeretnénk, ha a mintavételi URL-címhez toosee hello válasza: http://watestsdp2008r2.cloudapp.net:80/mintavétel. a következő PowerShell-példa hello hello probléma mutatja be.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Példa a kimenetre:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Figyelje meg, hogy az átirányítási válasz érkezett. Ahogy korábban is hangsúlyoztuk, StatusCode bármely más, mint 200 tekinthető hiba. A TRAFFIC Manager módosítások hello végpont állapota tooOffline. tooresolve hello probléma elhárításához ellenőrizze hello webhely konfigurációs tooensure, amely megfelelő StatusCode hello hello mintavételi elérési útról adhatók vissza. Konfigurálja újra a hello Traffic Manager mintavételi toopoint tooa elérési utat, amely egy 200 adja vissza.

Ha a mintavételi hello HTTPS protokollt használ, szükség lehet a toodisable tanúsítványellenőrzést tooavoid SSL/TLS-hibákat a vizsgálat során. hello következő PowerShell-utasításokat tiltsa le a tanúsítvány érvényesítési hello aktuális PowerShell-munkamenethez:

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Következő lépések

[A Traffic Manager forgalom-útválasztási módszerei](traffic-manager-routing-methods.md)

[Mi az a Traffic Manager](traffic-manager-overview.md)

[Felhőszolgáltatások](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[A Traffic Manager műveletei (REST API-referencia)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-parancsmagok][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
