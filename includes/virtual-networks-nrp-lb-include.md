## <a name="load-balancer"></a>Load Balancer
A terheléselosztó az alkalmazások tooscale kívánt használatos. Tipikus telepítési forgatókönyvek tartalmaz, amely több Virtuálisgép-példányok futó alkalmazások. hello Virtuálisgép-példányok által olyan terheléselosztóhoz, amely segít toodistribute hálózati forgalom toohello különböző példányai vannak fronted. 

![A hálózati adapter által a egyetlen virtuális gép](./media/resource-groups-networking/figure8.png)

| Tulajdonság | Leírás |
| --- | --- |
| *frontendipconfiguration osztálya lehet* |a terheléselosztó tartalmazhat egy vagy több előtérbeli IP-címek, más néven a virtuális IP-címek (VIP). A következő IP-címek érkező forgalom hello szolgál, és nyilvános IP-cím vagy a magánhálózati IP-címe |
| *backendaddresspools készletek* |Ezek a virtuális gép hálózati adapterek toowhich terhelés eloszlik hello társított IP-címek |
| *esetén a terheléselosztási szabályok* |egy szabály tulajdonság képezi le egy adott előtérbeli IP-cím és port kombinációja tooa set háttérbeli IP-címek és port kombináció. A terheléselosztó erőforrást egyetlen definícióját meghatározhatja, hogy több terheléselosztási szabályok, minden egyes szabály egy első kombinációja tükröző befejező IP-cím és port és a záró IP-cím és port társított virtuális gépek biztonsági. hello szabály hello előtér készlet toomany virtuális gépeken a hello háttérkészlethez egyetlen port |
| *Mintavétel* |mintavételt lehetővé teszik a Virtuálisgép-példányok állapotát hello tookeep nyomon. Ha nem sikerül egy állapotmintáihoz, hello virtuálisgép-példányt megnyílik kívül Elforgatás automatikusan |
| *inboundNatRules* |NAT-szabályok meghatározása hello bejövő hello front-end IP átfutó forgalom, és az elosztott toohello háttérbeli IP tooa adott virtuálisgép-példányt. NAT-szabály hello előtér készlet tooone virtuális gépen a háttérkészlethez hello egyetlen port |

Példa a terheléselosztó-sablon Json formátumban:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "dnsNameforLBIP": {
          "type": "string",
          "metadata": {
            "description": "Unique DNS name"
          }
        },
        "location": {
          "type": "string",
          "allowedValues": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ],
          "metadata": {
            "description": "Location toodeploy"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address Prefix"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/24",
          "metadata": {
            "description": "Subnet Prefix"
          }
        },
        "publicIPAddressType": {
          "type": "string",
          "defaultValue": "Dynamic",
          "allowedValues": [
            "Dynamic",
            "Static"
          ],
          "metadata": {
            "description": "Public IP type"
          }
        }
      },
      "variables": {
        "virtualNetworkName": "virtualNetwork1",
        "publicIPAddressName": "publicIp1",
        "subnetName": "subnet1",
        "loadBalancerName": "loadBalancer1",
        "nicName": "networkInterface1",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "nicId": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "backEndIPConfigID": "[concat(variables('nicId'),'/ipConfigurations/ipconfig1')]"
      },
      "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[parameters('location')]",
      "properties": {
        "publicIPAllocationMethod": "[parameters('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsNameforLBIP')]"
        }
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[parameters('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[parameters('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(variables('lbID'), '/backendAddressPools/LoadBalancerBackend')]"
                }
              ],
              "loadBalancerInboundNatRules": [
                {
                  "id": "[concat(variables('lbID'),'/inboundNatRules/RDP')]"
                }
              ]
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-05-01-preview",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[variables('publicIPAddressID')]"
              }
            }
          }
        ],
        "backendAddressPools": [
          {
            "name": "loadBalancerBackEnd"
          }
        ],
        "inboundNatRules": [
          {
            "name": "RDP",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPort": 3389,
              "backendPort": 3389,
              "enableFloatingIP": false
            }
          }
        ]
      }
    }
      ]
    }

### <a name="additional-resources"></a>További források
Olvasási [terheléselosztó REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx) további információt.

