## <a name="overview-of-azure-resource-manager-templates"></a>Az Azure Resource Manager-sablonok áttekintése
Az Azure Resource Manager-sablonok segítségével toodeclaratively Json nyelvi hello Azure infrastruktúra-szolgáltatási infrastruktúra megadása erőforrások közti függőségeket hello meghatározásával. A részletes áttekintés az Azure Resource Manager-sablonok tekintse meg az alábbi toohello cikk:

[Erőforráscsoport áttekintése](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>Minta sablon részlet Virtuálisgép-bővítmények
Virtuálisgép-bővítmények telepítése Azure Resource Manager-sablon része van szüksége, hogy toodeclaratively adja meg az hello bővítménykonfiguráció hello sablont.
Itt formátuma hello hello bővítménykonfiguráció meghatározásához.

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

Ahogy látja, a fenti hello, hello bővítmény a sablonban két fő részből áll:

1. Bővítmény nevét, kiadóját és verzió
2. Bővítmény konfigurációja.

## <a name="identifying-hello-publisher-type-and-typehandlerversion-for-any-extension"></a>Hello közzétevő, típusa és kiterjesztés typeHandlerVersion azonosítása
Az Azure Virtuálisgép-bővítmények a Microsoft által közzétett és megbízható 3. fél közzétevők és a egyes kiterjesztések egyedileg azonosítja a közzétevő, típusa és hello typeHandlerVersion. A következőképpen lehet meghatározni:  

