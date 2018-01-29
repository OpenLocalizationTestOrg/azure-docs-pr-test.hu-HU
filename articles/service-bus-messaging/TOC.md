# Áttekintés
## [Mi az a Service Bus üzenetkezelés?](service-bus-messaging-overview.md)
## [A Service Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
## [Service Bus-architektúra](service-bus-architecture.md)
## [GYIK](service-bus-faq.md)

# Első lépések
## [Névtér létrehozása](service-bus-create-namespace-portal.md)
## Üzenetsorok használata
### [.NET](service-bus-dotnet-get-started-with-queues.md)
### [Java](service-bus-java-how-to-use-queues.md)
### [Node.js](service-bus-nodejs-how-to-use-queues.md)
### [PHP](service-bus-php-how-to-use-queues.md)
### [Python](service-bus-python-how-to-use-queues.md)
### [Ruby](service-bus-ruby-how-to-use-queues.md)
### [REST](/rest/api/servicebus/queues)
## Témakörök és előfizetések használata
### [.NET](service-bus-dotnet-how-to-use-topics-subscriptions.md)
### [Java](service-bus-java-how-to-use-topics-subscriptions.md)
### [Node.js](service-bus-nodejs-how-to-use-topics-subscriptions.md)
### [PHP](service-bus-php-how-to-use-topics-subscriptions.md)
### [Python](service-bus-python-how-to-use-topics-subscriptions.md)
### [Ruby](service-bus-ruby-how-to-use-topics-subscriptions.md)

# Útmutató
## Tervezés és kialakítás
### [Felügyeltszolgáltatás-identitás (előzetes verzió)](service-bus-managed-service-identity.md)
### [Szerepköralapú hozzáférés-vezérlés (előzetes verzió)](service-bus-role-based-access-control.md)
### [Prémium szintű üzenettovábbítás](service-bus-premium-messaging.md)
### [Azure-üzenetsorok és Azure Service Bus-üzenetsorok összehasonlítása](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
### [Teljesítmény optimalizálása](service-bus-performance-improvements.md)
### [Geo-vészhelyreállítás és georeplikáció](service-bus-geo-dr.md)
### [Aszinkron üzenetkezelés és magas rendelkezésre állás](service-bus-async-messaging.md)
### [Leállások és katasztrófák kezelése](service-bus-outages-disasters.md)

## Fejlesztés
### [Többrétegű Service Bus-alkalmazás felépítése](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
### Üzenetkezelés
#### [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md) (Üzenetsorok, témakörök és előfizetések)
#### [Üzenetek, hasznos adatforgalom és szerializáció](service-bus-messages-payloads.md)
#### [Üzenetek átvitele, zárolása és elszámolása](message-transfers-locks-settlement.md)
#### [Üzenetek előkészítése és időbélyegek](message-sequencing.md)
#### [Üzenetek lejárata (élettartama)](message-expiration.md)
### [Hitelesítés és engedélyezés](service-bus-authentication-and-authorization.md)
#### [Migrálás ACS-ről SAS rendszerre](service-bus-migrate-acs-sas.md)
#### [Hitelesítés közös hozzáférésű jogosultságkódokkal](service-bus-sas.md)
### [Témakörszűrők és -műveletek](topic-filters.md)
### [Particionált üzenetsorok és témakörök](service-bus-partitioning.md)
### [Üzenet-munkamenetek](message-sessions.md)
### AMQP
#### [Az AMQP áttekintése](service-bus-amqp-overview.md)
#### [.NET](service-bus-amqp-dotnet.md)
#### [Java](service-bus-amqp-java.md)
#### [Java Message Service és AMQP](service-bus-java-how-to-use-jms-api-amqp.md)
#### [AMQP protokoll-útmutató](service-bus-amqp-protocol-guide.md)
#### [AMQP kérés-válasz alapú műveletek](service-bus-amqp-request-response.md)
### Speciális funkciók
#### [Kézbesíthetetlen levelek sorai](service-bus-dead-letter-queues.md)
#### [Üzenetek előzetes betöltése](service-bus-prefetch.md)
#### [Ismétlődő üzenetek észlelése](duplicate-detection.md)
#### [Üzenetszámlálók](message-counters.md)
#### [Üzenetek halasztása](message-deferral.md)
#### [Üzenetek tallózása](message-browsing.md)
#### [Entitások láncolása automatikus továbbítással](service-bus-auto-forwarding.md)
#### [Tranzakciófeldolgozás](service-bus-transactions.md)
#### [Párosított névterek implementációja](service-bus-paired-namespaces.md)
### [Átfogó nyomkövetés és diagnosztika](service-bus-end-to-end-tracing.md)
## Kezelés
### [Service Bus monitorozása az Azure Monitoring segítségével](service-bus-metrics-azure-monitor.md)
### [Service Bus felügyeleti könyvtárak](service-bus-management-libraries.md)
### [Diagnosztikai naplók](service-bus-diagnostic-logs.md)
### [Üzenetküldési entitások felfüggesztése és újraaktiválása](entity-suspend.md)
### [Használjon Azure Resource Manager-sablonokat](service-bus-resource-manager-overview.md)
#### [Névtér létrehozása](service-bus-resource-manager-namespace.md)
#### [Névtér és üzenetsor létrehozása](service-bus-resource-manager-namespace-queue.md)
#### [Névtér létrehozása témakörrel és előfizetéssel](service-bus-resource-manager-namespace-topic.md)
#### [Engedélyezési szabály létrehozása névtérhez és üzenetsorhoz](service-bus-resource-manager-namespace-auth-rule.md)
#### [Névtér létrehozása témakörrel, előfizetéssel és szabállyal](service-bus-resource-manager-namespace-topic-with-rule.md)
#### 
### [Az Azure PowerShell használata entitások üzembe helyezésére](service-bus-manage-with-ps.md)

# Referencia
## .NET
### [Microsoft.ServiceBus.Messaging (.NET-keretrendszer)](/dotnet/api/microsoft.servicebus.messaging)
### [Microsoft.Azure.ServiceBus (.NET Standard)](/dotnet/api/microsoft.azure.servicebus)
## [Java](/java/api/overview/azure/servicebus)
## [Azure PowerShell](/powershell/module/azurerm.servicebus)
## [REST](/rest/api/servicebus)
## [Kivételek](service-bus-messaging-exceptions.md)
## [Kvóták](service-bus-quotas.md)
## [SQLFilter szintaxis](service-bus-messaging-sql-filter.md)
## [SQLRuleAction szintaxis](service-bus-messaging-sql-rule-action.md)

# További források
## [Azure-ütemterv](https://azure.microsoft.com/roadmap/?category=enterprise-integration)
## [Blog](https://blogs.msdn.microsoft.com/servicebus/)
## [A Microsoft Developer Network fórumai](https://social.msdn.microsoft.com/forums/home?forum=servbus)
## [Díjszabás](https://azure.microsoft.com/pricing/details/service-bus/)
## [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/)
## [Díjszabás részletei](service-bus-pricing-billing.md)
## [Példák](service-bus-samples.md)
## [ServiceBus360](https://www.servicebus360.com/)
## [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)
## [Szolgáltatási hírek](https://azure.microsoft.com/updates/?product=service-bus)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azureservicebus)
## [Videók](https://azure.microsoft.com/documentation/videos/index/?services=service-bus)


