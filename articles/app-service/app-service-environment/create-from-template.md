---
title: "a Resource Manager-sablon használatával Azure App Service-környezet aaaCreate"
description: "Azt ismerteti, hogyan toocreate külső vagy ILB Azure App Service környezetben a Resource Manager-sablon használatával"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: c8aeedee675a6e931169b725ee916cc7fa8f762f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Hozzon létre egy ASE Azure Resource Manager-sablon használatával

## <a name="overview"></a>Áttekintés
Az Azure App Service-környezetek (ASEs) az internetről elérhető végpontok vagy egy Azure virtuális hálózatot (VNet) belső cím végpontja hozhatók létre. A belső végpont létrehozásakor adott végpontra kerül az Azure által az összetevő egy belső terheléselosztón (ILB) nevű. hello ASE a belső IP-címet egy ILB ASE nevezik. egy nyilvános végponttal ASE hello egy külső ASE nevezik. 

Egy ASE hello Azure-portálon vagy az Azure Resource Manager-sablon használatával is létrehozható. Ez a cikk végigvezeti a hello lépéseket és kell toocreate egy külső ASE vagy ILB ASE Resource Manager-sablonok szintaxisát. Hogyan toocreate egy ASE a hello Azure-portálon: toolearn [egy külső ASE ellenőrizze] [ MakeExternalASE] vagy [egy ILB ASE ellenőrizze][MakeILBASE].

Amikor létrehoz egy ASE hello Azure-portálon, a virtuális hálózat hello azonos időben, vagy válasszon egy már meglévő virtuális hálózat toodeploy a következő hozhat létre. Amikor egy ASE sablon alapján hoz létre, meg kell kezdődnie: 

* Egy erőforrás-kezelő virtuális hálózat.
* A virtuális alhálózat. Azt javasoljuk, hogy egy ASE alhálózat mérete `/25` a 128 címek tooaccomodate jövőbeli növekedésre. Hello ASE létrehozása után nem módosítható hello méretét.
* hello erőforrás-azonosító a vnet. Ezt az információt lekérheti hello Azure-portálon, a virtuális hálózati tulajdonságok alapján.
* azt szeretné, hogy a toodeploy hello előfizetés.
* hello a toodeploy a kívánt helyre.

tooautomate a ASE létrehozása:

1. Hello ASE létrehozása sablonból. Ha létrehoz egy külső ASE, végzett elvégezte ezt a lépést. Ha létrehoz egy ILB ASE, van néhány további dolgot toodo.

2. A ILB ASE létrehozása után, amely megfelel a ILB ASE tartomány SSL-tanúsítvány feltöltése.

3. hello feltöltött SSL-tanúsítvány hozzá van rendelve toohello ILB ASE "alapértelmezett" SSL-tanúsítványát.  Ezzel a tanúsítvánnyal az SSL-forgalom tooapps a hello ILB ASE hello közös gyökértartomány, amely hozzárendelt toohello ASE (például https://someapp.mycustomrootcomain.com) használata.


## <a name="create-hello-ase"></a>Hello ASE létrehozása
A Resource Manager-sablont, amely létrehoz egy ASE és a kapcsolódó paraméterfájl érhető [egy példa a] [ quickstartasev2create] a Githubon.

Ha azt szeretné, hogy egy ILB ASE toomake, használja a Resource Manager-sablon [példák][quickstartilbasecreate]. Akkor gondoskodjon arról, hogy toothat használati eset. A legtöbb hello hello paraméterek *azuredeploy.parameters.json* fájl ILB ASEs és külső ASEs közös toohello létrehozását. hello alábbi lista hívja meg megjegyzés paraméterei, vagy az egyedi, amikor létrehoz egy ILB ASE:

* *interalLoadBalancingMode*: A legtöbb esetben állítsa be a too3, ami azt jelenti, hogy mindkét HTTP/HTTPS-forgalom 80/443-as porton, és hello vezérlő/adatai portok hello ASE tooby hello FTP szolgáltatást figyel, kötött tooan ILB lefoglalt virtuális hálózat lesz belső címe. Ha ez a tulajdonság értéke too2, csak hello FTP szolgáltatással kapcsolatos (vezérlő és az adatokat egyaránt csatornák) portjait kötött tooan ILB cím. hello HTTP/HTTPS-forgalmat hello nyilvános VIP marad.
* *dnsSuffix*: Ez a paraméter hello alapértelmezett gyökértartomány, amely hozzárendelt toohello ASE határozza meg. Az Azure App Service változata nyilvános hello, hello alapértelmezett gyökértartomány minden webes alkalmazások nem *azurewebsites.net*. Mivel egy ILB ASE belső tooa az ügyfél virtuális hálózat, nem létrehozni, akkor logika toouse hello nyilvános szolgáltatás alapértelmezett legfelső szintű tartomány. Ehelyett egy ILB ASE kell egy alapértelmezett gyökértartomány legjobb a vállalat belső virtuális hálózaton belüli használatra. Például a Contoso Corporation használhatja az alapértelmezett legfelső szintű tartomány *belső contoso.com* tervezett toobe feloldható és csak a Contoso virtuális hálózaton belülről érhetők el alkalmazások. 
* *ipSslAddressCount*: Ez a paraméter alapértelmezett értéke automatikusan tooa értékének 0-ra a hello *azuredeploy.json* fájlhoz, mert ILB ASEs csak egyetlen ILB címmel rendelkezik. Nincsenek a egy ILB ASE explicit IP-SSL címek. Emiatt az SSL-IP-címkészletet egy ILB ASE hello toozero állítható be. Ellenkező esetben a létesítési hiba történik. 

Hello után *azuredeploy.parameters.json* fájl ki van töltve, és hozzon létre hello ASE hello PowerShell kódrészletet használatával. Hello fájl elérési utak toomatch hello Resource Manager sablon-fájl helyének módosítása a számítógépen. Ne feledje toosupply hello erőforrás-kezelő központi telepítés nevét és az erőforráscsoport neve hello a saját értékeit:

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Hello ASE toobe létrehozott egy órát vesz igénybe. Majd hello ASE megjelennek hello portálon hello előfizetés hello telepítési kiváltó hello ASEs listájában.

## <a name="upload-and-configure-hello-default-ssl-certificate"></a>Töltse fel, és hello "alapértelmezett" SSL-tanúsítvány konfigurálása
Az SSL-tanúsítvány hello "alapértelmezett" SSL-tanúsítványt, amely SSL-kapcsolatok tooapps használt tooestablish hello mértékéig társítva kell lennie. Ha hello ASE tartozó alapértelmezett DNS-utótagja *belső contoso.com*, a kapcsolat toohttps://some-random-app.internal-contoso.com igényel, amely érvényes SSL-tanúsítvány **.internal-contoso.com* . 

Szerezzen be egy érvényes SSL-tanúsítvány belső hitelesítésszolgáltatók használatával, egy tanúsítvány beszerzése egy külső kiállítótól érkező, vagy önaláírt tanúsítványt használ. Hello SSL-tanúsítvány hello forrását, függetlenül a következő tanúsítvány attribútumok hello megfelelően kell konfigurálni:

* **Tulajdonos**: Ez az attribútum túl be kell állítani **.your-gyökér-tartományi-here.com*.
* **Tulajdonos alternatív neve**: ennek az attribútumnak kell tartalmaznia a **.your-gyökér-tartományi-here.com* és **.scm.your-gyökér-tartományi-here.com*. Minden alkalmazáshoz kapcsolódó SCM/Kudu webhely SSL-kapcsolatok toohello hello űrlap cím használata *your-app-name.scm.your-root-domain-here.com*.

Egy érvényes SSL-tanúsítvánnyal aktuális két további előkészítő lépések szükségesek. A konvertálás/save hello SSL-tanúsítvány egy .pfx fájlba. Ne feledje, hogy hello .pfx-fájlt kell tartalmazza az összes köztes és a tanúsítványok. A biztonság jelszóval.

hello .pfx fájlnak kell toobe Base64 kódolású karakterlánc alakítja át, mivel a hello SSL-tanúsítvány feltöltése a Resource Manager-sablon használatával. Mivel Resource Manager-sablonok szövegfájlok, hello .pfx fájl konvertálni kell egy Base64 kódolású karakterlánc. Ezzel a módszerrel azt is meg lehet adni hello sablon paraméterként.

A következő kódrészletet PowerShell való hello használata:

* Önaláírt tanúsítvány jön létre.
* Hello tanúsítvány exportálása egy .pfx fájlba.
* Hello .pfx fájl átalakítása base64 kódolású karakterlánc.
* Hello base64 kódolású karakterlánc tooa külön fájlt mentse. 

A PowerShell-kódot az alkalmazás base64 kódolást volt módosítani a hello [PowerShell parancsfájlok blog][examplebase64encoding]:

        $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

        $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
        $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

        $fileName = "exportedcert.pfx"
        Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

        $fileContentBytes = get-content -encoding byte $fileName
        $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
        $fileContentEncoded | set-content ($fileName + ".b64")

Miután hello SSL-tanúsítvány létrehozása sikeresen megtörtént, és konvertálja tooa base64 kódolású karakterlánc, a hello példa Resource Manager-sablon [konfigurálása hello alapértelmezett SSL-tanúsítvány] [ quickstartconfiguressl] a Githubon. 

paramétereket a hello hello *azuredeploy.parameters.json* fájl itt találhatók:

* *appServiceEnvironmentName*: hello konfigurált ILB ASE hello nevét.
* *existingAseLocation*: szöveges karakterláncot tartalmazó hello Azure-régió, ahol hello ILB ASE telepítve lett.  Például: "Déli középső Régiójában".
* *pfxBlobString*: hello hello .pfx fájl based64 kódolású karakterláncos ábrázolása. Használja a korábban bemutatott hello kódrészletet, és másolja hello karakterláncot "exportedcert.pfx.b64" szerepel. Illessze be az hello hello értékként *pfxBlobString* attribútum.
* *jelszó*: hello használt jelszó toosecure hello .pfx fájlt.
* *certificateThumbprint*: hello tanúsítvány ujjlenyomata. Ha ez az érték lekérése PowerShell (például *$certificate. Ujjlenyomat* hello a korábbi kódrészletet), használhatja a hello érték van. Hello érték másolni hello Windows tanúsítvány párbeszédpanel, ha ne felejtse el toostrip kimenő hello idegen szóközöket. Hello *certificateThumbprint* AF3143EB61D43F6727842115BB7F17BBCECAECAE hasonlóan kell kinéznie.
* *certificateName*: egy rövid karakterlánc-azonosító saját tooidentity hello tanúsítványt használja. hello neve hello egyedi erőforrás-kezelő azonosítója részeként használatos hello *Microsoft.Web/certificates* hello SSL-tanúsítvány képviselő entitás. hello neve *kell* végződhet a következő utótag hello: \_yourASENameHere_InternalLoadBalancingASE. azt jelzi, hogy a tanúsítvány hello használt toosecure egy ILB-kompatibilis ASE hello Azure-portálon használja a utótag.

Rövidített például *azuredeploy.parameters.json* itt jelenik meg:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Hello után *azuredeploy.parameters.json* fájl ki van töltve, hello alapértelmezett SSL-tanúsítvány konfigurálása hello PowerShell kódrészletet használatával. Módosítsa a hello fájl elérési utak toomatch hello Resource Manager sablon fájlok hol található a számítógépen. Ne feledje toosupply hello erőforrás-kezelő központi telepítés nevét és az erőforráscsoport neve hello a saját értékeit:

     $templatePath="PATH\azuredeploy.json"
     $parameterPath="PATH\azuredeploy.parameters.json"

     New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

ASE előtér tooapply hello változásonkénti nagyjából 40 percet vesz igénybe. Például az előtér-webkiszolgálóinak két használó alapértelmezett méretű ASE hello sablon vesz igénybe egy óra és 20 perc toocomplete körül. Hello sablon futása közben, a hello ASE nem lehet méretezni.  

Hello sablonon végeztével hello ILB ASE alkalmazások HTTPS-KAPCSOLATON keresztül érhető el. hello-kapcsolatok biztonságáról hello alapértelmezett SSL-tanúsítvány használatával. hello alapértelmezett SSL-tanúsítvány akkor használatos, ha a hello ILB ASE alkalmazások hello alkalmazás nevét, valamint hello alapértelmezett állomásnév együttes használatával tárgyalja. Például a https://mycustomapp.internal-contoso.com hello alapértelmezett SSL-tanúsítványt használ **.internal-contoso.com*.

Hasonlóan hello nyilvános több-bérlős szolgáltatáson futó alkalmazások, azonban fejlesztők konfigurálhatja az egyes alkalmazások egyéni állomásnevek. Egyedi SNI SSL-tanúsítványok kötései egyes alkalmazások esetében is konfigurálhatja.

## <a name="app-service-environment-v1"></a>App Service-környezet v1 ##
App Service Environment-környezet két verziója van: ASEv1 és ASEv2. információk megelőző hello ASEv2 alapján. Ez a szakasz jeleníti meg, akkor hello ASEv1 és ASEv2 közötti különbséget.

Az ASEv1 kezelhetők minden hello erőforrást manuálisan. Amely tartalmazza a hello előtér-webkiszolgálóinak dolgozó munkatársak és az IP-alapú SSL-hez használt IP-címek. Ki lehet terjeszteni a App Service-csomagot, mielőtt kell a horizontális hello munkavégző készletét, amelyet az toohost azt.

ASEv1 ASEv2 a különböző árképzési modellt használ. ASEv1 a minden lefoglalt core fizetnie. Az előtér-webkiszolgálóinak vagy bármilyen számítási feladatot nem futtató munkavállalók használt magok, amely tartalmazza. A ASEv1 hello alapértelmezett maximális méretű egy ASE mérete 55 összes állomás. Amely tartalmazza a dolgozók és első akkor ér véget. Egy előny tooASEv1, hogy központilag telepíthető a klasszikus virtuális hálózatot és egy erőforrás-kezelő virtuális hálózatot. toolearn ASEv1, kapcsolatos további információkért lásd: [App Service Environment-környezet v1 bemutatása][ASEv1Intro].

a Resource Manager-sablon használatával egy ASEv1 toocreate lásd [hozzon létre egy ILB ASE v1 Resource Manager-sablon][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: ../../app-service-web/app-service-app-service-environment-create-ilb-ase-resourcemanager.md
