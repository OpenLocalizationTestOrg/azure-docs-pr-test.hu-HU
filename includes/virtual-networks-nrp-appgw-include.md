## <a name="application-gateway"></a><span data-ttu-id="b9c2c-101">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b9c2c-101">Application Gateway</span></span>
<span data-ttu-id="b9c2c-102">Alkalmazásátjáró egy Azure által kezelt HTTP terheléselosztási réteg 7 terheléselosztás megoldást biztosít.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-102">Application Gateway provides an Azure-managed HTTP load balancing solution based on layer 7 load balancing.</span></span> <span data-ttu-id="b9c2c-103">Alkalmazás terheléselosztás engedélyezése hello útválasztási szabályokat a HTTP-alapú hálózati forgalma.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-103">Application load balancing allows hello use of routing rules for network traffic based on HTTP.</span></span> 
<BR>

| <span data-ttu-id="b9c2c-104">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b9c2c-104">Property</span></span> | <span data-ttu-id="b9c2c-105">Leírás</span><span class="sxs-lookup"><span data-stu-id="b9c2c-105">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b9c2c-106">**backendaddresspools készletek**</span><span class="sxs-lookup"><span data-stu-id="b9c2c-106">**backendAddressPools**</span></span> |<span data-ttu-id="b9c2c-107">IP-címek hello háttér kiszolgálók hello listáját.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-107">hello list of IP addresses of hello back end servers.</span></span> <span data-ttu-id="b9c2c-108">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP vagy a magánhálózati IP-címe legyen.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-108">hello IP addresses listed should either belong toohello virtual network subnet, or should be a public IP/VIP or private IP</span></span> |
| <span data-ttu-id="b9c2c-109">**backendHttpSettingsCollection**</span><span class="sxs-lookup"><span data-stu-id="b9c2c-109">**backendHttpSettingsCollection**</span></span> |<span data-ttu-id="b9c2c-110">Minden készlethez beállítások, például a portot, a protokoll és a cookie-alapú kapcsolat van.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-110">Every pool has settings like port, protocol, and cookie based affinity.</span></span> <span data-ttu-id="b9c2c-111">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="b9c2c-111">These settings are tied tooa pool and are applied tooall servers within hello pool</span></span> |
| <span data-ttu-id="b9c2c-112">**frontendports portok**</span><span class="sxs-lookup"><span data-stu-id="b9c2c-112">**frontendPorts**</span></span> |<span data-ttu-id="b9c2c-113">Ez a port nem hello nyilvános portot nyit meg hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-113">This port is hello public port opened on hello application gateway.</span></span> <span data-ttu-id="b9c2c-114">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="b9c2c-114">Traffic hits this port, and then gets redirected tooone of hello back end servers</span></span> |
| <span data-ttu-id="b9c2c-115">**httpListeners**</span><span class="sxs-lookup"><span data-stu-id="b9c2c-115">**httpListeners**</span></span> |<span data-ttu-id="b9c2c-116">Figyelő rendelkezik egy elülső rétegbeli portot, a protokollt (Http vagy Https, amelyek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának kiszervezése)</span><span class="sxs-lookup"><span data-stu-id="b9c2c-116">Listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload)</span></span> |
| <span data-ttu-id="b9c2c-117">**requestroutingrules szabályok**</span><span class="sxs-lookup"><span data-stu-id="b9c2c-117">**requestRoutingRules**</span></span> |<span data-ttu-id="b9c2c-118">hello szabály hello figyelő és hello server háttérkészlethez van kötve, és határozza meg, melyik háttér címkészletet hello forgalom legyenek irányítva.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-118">hello rule binds hello listener and hello back end server pool and defines which back end server pool hello traffic should be directed.</span></span> <span data-ttu-id="b9c2c-119">Jelenleg csak a ciklikus multiplexelés szerint működik</span><span class="sxs-lookup"><span data-stu-id="b9c2c-119">Currently works only as Round-robin</span></span> |

<span data-ttu-id="b9c2c-120">Példa: egy alkalmazás átjáró Json-sablont:</span><span class="sxs-lookup"><span data-stu-id="b9c2c-120">Example of an application gateway Json template:</span></span>

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "location": {
          "type": "string",
          "metadata": {
            "description": "Location toodeploy to"
          }
        },
        "addressPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/16",
          "metadata": {
            "description": "Address prefix for hello Virtual Network"
          }
        },
        "subnetPrefix": {
          "type": "string",
          "defaultValue": "10.0.0.0/28",
          "metadata": {
            "description": "Subnet prefix"
          }
        },
        "skuName": {
          "type": "string",
          "allowedValues": [
            "Standard_Small",
            "Standard_Medium",
            "Standard_Large"
          ],
          "defaultValue": "Standard_Medium",
          "metadata": {
            "description": "Sku Name"
          }
        },
        "capacity": {
          "type": "int",
          "defaultValue": 2,
          "metadata": {
            "description": "Number of instances"
          }
        },
        "backendIpAddress1": {
          "type": "string",
          "metadata": {
            "description": "IP Address for Backend Server 1"
          }
        },
        "backendIpAddress2": {
          "type": "string",
          "metadata": {
            "description": "IP Address for Backend Server 2"
          }
        }
      },
      "variables": {
        "applicationGatewayName": "applicationGateway1",
        "publicIPAddressName": "publicIp1",
        "virtualNetworkName": "virtualNetwork1",
        "subnetName": "appGatewaySubnet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
        "publicIPRef": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]",
        "applicationGatewayID": "[resourceId('Microsoft.Network/applicationGateways',variables('applicationGatewayName'))]",
        "apiVersion": "2015-05-01-preview"
      },
      "resources": [
        {
          "apiVersion": "[variables('apiVersion')]",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIPAddressName')]",
          "location": "[parameters('location')]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic"
          }
        },
        {
          "apiVersion": "[variables('apiVersion')]",
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
          "apiVersion": "[variables('apiVersion')]",
          "name": "[variables('applicationGatewayName')]",
          "type": "Microsoft.Network/applicationGateways",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/virtualNetworks/', variables    ('virtualNetworkName'))]",
            "[concat('Microsoft.Network/publicIPAddresses/', variables    ('publicIPAddressName'))]"
          ],
          "properties": {
            "sku": {
              "name": "[parameters('skuName')]",
              "tier": "Standard",
              "capacity": "[parameters('capacity')]"
            },
            "gatewayIPConfigurations": [
              {
                "name": "appGatewayIpConfig",
                "properties": {
                  "subnet": {
                    "id": "[variables('subnetRef')]"
                  }
                }
              }
            ],
            "frontendIPConfigurations": [
              {
                "name": "appGatewayFrontendIP",
                "properties": {
                  "PublicIPAddress": {
                    "id": "[variables('publicIPRef')]"
                  }
                }
              }
            ],
            "frontendPorts": [
              {
                "name": "appGatewayFrontendPort",
                "properties": {
                  "Port": 80
                }
              }
            ],
            "backendAddressPools": [
              {
                "name": "appGatewayBackendPool",
                "properties": {
                  "BackendAddresses": [
                    {
                      "IpAddress": "[parameters('backendIpAddress1')]"
                    },
                    {
                      "IpAddress": "[parameters('backendIpAddress2')]"
                    }
                  ]
                }
              }
            ],
            "backendHttpSettingsCollection": [
              {
                "name": "appGatewayBackendHttpSettings",
                "properties": {
                  "Port": 80,
                  "Protocol": "Http",
                  "CookieBasedAffinity": "Disabled"
                }
              }
            ],
            "httpListeners": [
              {
                "name": "appGatewayHttpListener",
                "properties": {
                  "FrontendIPConfiguration": {
                    "Id": "[concat(variables('applicationGatewayID'), '/    frontendIPConfigurations/appGatewayFrontendIP')]"
                  },
                  "FrontendPort": {
                    "Id": "[concat(variables('applicationGatewayID'), '/    frontendPorts/appGatewayFrontendPort')]"
                  },
                  "Protocol": "Http",
                  "SslCertificate": null
                }
              }
            ],
            "requestRoutingRules": [
              {
                "Name": "rule1",
                "properties": {
                  "RuleType": "Basic",
                  "httpListener": {
                    "id": "[concat(variables('applicationGatewayID'), '/    httpListeners/appGatewayHttpListener')]"
                  },
                  "backendAddressPool": {
                    "id": "[concat(variables('applicationGatewayID'), '/    backendAddressPools/appGatewayBackendPool')]"
                  },
                  "backendHttpSettings": {
                    "id": "[concat(variables('applicationGatewayID'), '/    backendHttpSettingsCollection/    appGatewayBackendHttpSettings')]"
                  }
                }
              }
            ]
          }
        }
      ]    
    }


### <a name="additional-resources"></a><span data-ttu-id="b9c2c-121">További források</span><span class="sxs-lookup"><span data-stu-id="b9c2c-121">Additional resources</span></span>
<span data-ttu-id="b9c2c-122">Olvasási [ Alkalmazásátjáró REST API](https://msdn.microsoft.com/library/azure/mt299388.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="b9c2c-122">Read [ application gateway REST API](https://msdn.microsoft.com/library/azure/mt299388.aspx) for more information.</span></span>

