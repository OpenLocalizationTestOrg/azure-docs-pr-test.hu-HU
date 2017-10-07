---
title: "Állapot konfigurációs Resource Manager-sablon aaaDesired |} Microsoft Docs"
description: "A célállapot-konfiguráció Azure-példákkal és hibaelhárítási Resource Manager-sablon meghatározása"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: fc47ac149b1179d9305797eaa8ed8a83c0958c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS és célállapot-konfiguráció Azure Resource Manager-sablonok
Ez a cikk ismerteti a hello Resource Manager-sablon hello [célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="template-example-for-a-windows-vm"></a>A Windows virtuális gépek sablon – példa
hello következő kódrészlettel hiányzóra hello hello sablon erőforrás szakasza.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }

```

## <a name="template-example-for-windows-vmss"></a>A Windows VMSS sablon – példa
Egy VMSS fürtcsomópont hello "VirtualMachineProfile", "extensionProfile" attribútum a "Tulajdonságok" szakasz. A DSC-ből "kiterjesztésekkel" alatt jelenik meg. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
        }
```

## <a name="detailed-settings-information"></a>Részletes beállítási információk
hello következő séma egy hello beállítási részén hello Azure Resource Manager-sablon Azure DSC-bővítményt.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Részletek
| Tulajdonság neve | Típus | Leírás |
| --- | --- | --- |
| settings.wmfVersion |Karakterlánc |Megadja, hogy a virtuális Gépre telepíteni kell a Windows Management Framework hello hello verziószámát. Ez a tulajdonság too'latest beállítása "telepíti hello a frissített verzió WMF. Ez a tulajdonság a lehetséges értékek csak az aktuális hello **"4.0", "5.0", "5.0PP', és a"legutóbbi"**. A lehetséges értékek: tulajdonos tooupdates. hello alapértelmezett érték: "legújabb". |
| Settings.Configuration.URL |Karakterlánc |A DSC konfigurációs zip fájlt adja meg a hello URL-cím hely, mely toodownload. Ha hello URL-cím egy SAS-jogkivonat hozzáférést igényel, meg kell tooset toohello hello protectedSettings.configurationUrlSasToken tulajdonság értékét a SAS-jogkivonat. E tulajdonság megadása kötelező, ha settings.configuration.script és/vagy settings.configuration.function vannak meghatározva. |
| Settings.Configuration.Script |Karakterlánc |A DSC-konfiguráció hello definícióját tartalmazó parancsprogram hello hello fájl nevét adja meg. Ezt a parancsfájlt a hello hello configuration.url tulajdonság által megadott URL-CÍMRŐL letöltött zip-fájl hello hello gyökérmappájában kell lennie. E tulajdonság megadása kötelező, ha settings.configuration.url és/vagy settings.configuration.script vannak meghatározva. |
| Settings.Configuration.Function |Karakterlánc |A DSC-konfiguráció hello nevét adja meg. hello-konfiguráció configuration.script által meghatározott hello parancsfájl kell tartoznia. E tulajdonság megadása kötelező, ha settings.configuration.url és/vagy settings.configuration.function vannak meghatározva. |
| settings.configurationArguments |Gyűjtemény |Határozza meg a paramétereket, milyen toopass tooyour DSC-konfiguráció. Ez a tulajdonság nincs titkosítva. |
| settings.configurationData.url |Karakterlánc |Meghatározza a hello URL-címet, a mely toodownload a konfigurációs adatok (.psd1) toouse fájlt a DSC-konfiguráció bemenetként. Ha hello URL-cím egy SAS-jogkivonat hozzáférést igényel, meg kell tooset toohello hello protectedSettings.configurationDataUrlSasToken tulajdonság értékét a SAS-jogkivonat. |
| settings.privacy.dataEnabled |Karakterlánc |Engedélyezheti vagy letilthatja a telemetriai adatok gyűjtése. hello csak lehetséges Ez a tulajdonság értékei **"Engedélyezés", "Letiltás", ", vagy $null**. Így ez a tulajdonság üres vagy null értékű lehetővé teszi, hogy a telemetriai adatokat. hello alapértelmezett érték ". [További információ](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Gyűjtemény |Meghatározza, mely toodownload a WMF hello más helyekre. [További információ](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Gyűjtemény |Határozza meg a paramétereket, milyen toopass tooyour DSC-konfiguráció. Ez a tulajdonság titkosítva van. |
| protectedSettings.configurationUrlSasToken |Karakterlánc |Meghatározza hello SAS-token tooaccess hello URL-címet configuration.url határozzák meg. Ez a tulajdonság titkosítva van. |
| protectedSettings.configurationDataUrlSasToken |Karakterlánc |Meghatározza hello SAS-token tooaccess hello URL-címet configurationData.url határozzák meg. Ez a tulajdonság titkosítva van. |

## <a name="settings-vs-protectedsettings"></a>Vs beállításait. ProtectedSettings
Minden beállítás beállítások szövegfájlba hello VM lesznek mentve.
A "beállítások" olyan nyilvános tulajdonságok mert hello beállításait szöveges fájlban nincsenek titkosítva.
A "protectedSettings" tulajdonságok a tanúsítvánnyal titkosított, és csak akkor jelennek meg ebben a fájlban, a virtuális gép hello egyszerű szöveg.

Ha hello konfigurációs hitelesítő adatokat igényel, protectedSettings is szerepelni:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Példa
hello alábbi példa származik hello "Első lépések" szakasza hello [DSC kiterjesztés kezelője – áttekintés oldalra](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
A példa Resource Manager-sablonok parancsmagok toodeploy hello kiterjesztés helyett. Hello "IisInstall.ps1" konfigurációs elmentéséhez elhelyezéséhez egy. A ZIP-fájl, és fájlfeltöltési hello az elérhető URL-címre. A példában az Azure blob Storage tárolóban, de lehetséges toodownload. A ZIP-fájlok bármilyen tetszőleges helyről.

A hello Azure Resource Manager sablon hello alábbira arra utasítja a hello VM toodownload hello megfelelő fájlt, és futtassa hello megfelelő PowerShell funkció:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-hello-previous-format"></a>Az előző formátum hello frissítése
Hello előző formátum (nyilvános tulajdonságokat hello ModulesUrl, ConfigurationFunction, SasToken vagy tulajdonságait tartalmazó) automatikusan beállításait toohello aktuális formátumot támogató és csak megfelelően előtt.

a következő séma hello milyen hello előző beállítási séma hasonlóan keresni:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points tooprivate Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Ez hogyan hello előző formátum alkalmazkodik toohello aktuális formátuma:

| Tulajdonság neve | Előző séma megfelelője |
| --- | --- |
| settings.wmfVersion |a beállítások. WMFVersion |
| Settings.Configuration.URL |a beállítások. ModulesUrl |
| Settings.Configuration.Script |Beállítások első része. ConfigurationFunction (előtt "\\\\") |
| Settings.Configuration.Function |Második része a beállításokat. ConfigurationFunction (után "\\\\") |
| settings.configurationArguments |a beállítások. Tulajdonságok |
| settings.configurationData.url |protectedSettings.DataBlobUri (nélkül SAS-jogkivonat) |
| settings.privacy.dataEnabled |a beállítások. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |a beállítások. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |a beállítások. SasToken |
| protectedSettings.configurationDataUrlSasToken |A protectedSettings.DataBlobUri SAS-jogkivonat |

## <a name="troubleshooting---error-code-1100"></a>Hibaelhárítás – 1100-as hibakód
1100-as hibakód azt jelzi, hogy felhasználói bevitel toohello DSC-bővítmény hello probléma van.
Ezek a hibák szövegét hello változó, és módosíthatja.
Az alábbiakban néhány hello hibákat tapasztal előfordulhat, hogy és az hogyan oldható meg őket.

### <a name="invalid-values"></a>Érvénytelen értékekkel
"Privacy.dataCollection nem"{0}". hello csak a lehetséges értékek: ","Engedélyezése"és"Disable"" "WmfVersion nem"{0}". Csak a lehetséges értékek a következők... és a "legújabb" "

Probléma: A megadott érték nem megengedett.

Megoldás: Hello érvénytelen érték tooa érvényes értékének módosítása. Hello táblázatban hello Részletek területen találja.

### <a name="invalid-url"></a>Érvénytelen URL-címe
"ConfigurationData.url nem"{0}". Ez nem egy érvényes URL-címet az""DataBlobUri nem "{0}". Ez nem egy érvényes URL-címet az""Configuration.url nem "{0}". Ez nem egy érvényes URL-címet az"

Probléma: A megadott URL-cím érvénytelen.

Megoldás: Ellenőrizze az a megadott URL-címet. Győződjön meg arról, hogy az URL-címet megoldásához toovalid helyek hozzáférő hello bővítmény hello távoli számítógépen.

### <a name="invalid-configurationargument-type"></a>Érvénytelen ConfigurationArgument típusa
"Típus érvénytelen configurationArguments" {0}

Probléma: hello ConfigurationArguments tulajdonság nem oldható fel tooa Hashtable objektum. 

Megoldás: Ellenőrizze a ConfigurationArguments tulajdonság egy kivonattáblát. Hello portáladatbázis előző példában megadott hello formátumot követi. Figyeljen az ajánlatok, vesszővel válassza el egymástól, és kell használni.

### <a name="duplicate-configurationarguments"></a>Ismétlődő ConfigurationArguments
"Található nyilvános és a védett configurationArguments ismétlődő argumentumok"{0}""

Probléma: hello ConfigurationArguments nyilvános beállításai és hello védett beállításai ConfigurationArguments tartalmaz hello értékkel rendelkező tulajdonságok ugyanazzal a névvel.

Megoldás: Távolítsa el az ismétlődő tulajdonságok hello egyik.

### <a name="missing-properties"></a>Hiányzó tulajdonságok
"Megköveteli, hogy configuration.url configuration.module meg van adva vagy Configuration.function"

"Megköveteli, hogy configuration.script megadott Configuration.url"

"Megköveteli, hogy configuration.url megadott Configuration.script"

"Megköveteli, hogy configuration.function megadott Configuration.url"

"Megköveteli, hogy configuration.url megadott ConfigurationUrlSasToken"

"Megköveteli, hogy configurationData.url megadott ConfigurationDataUrlSasToken"

Probléma: Egy meghatározott tulajdonság kell, hogy hiányzik egy másik tulajdonságot.

Megoldások: 

* Adja meg a hiányzó hello tulajdonságot.
* Távolítsa el, amelyet a hello hiányzó tulajdonság hello tulajdonságot.

## <a name="next-steps"></a>Következő lépések
További tudnivalók a DSC-ből és a virtuálisgép-skálázási készletekben [használatával virtuálisgép-skálázási készletekben a hello Azure DSC-bővítményt](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

További részletekért található a következő [DSC biztonságos hitelesítőadat-kezelés](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Hello Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview). 

