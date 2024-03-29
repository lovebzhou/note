## 1. 开发者账号

### 1.1. Apple的同一个分发证书打包的App，会不会因为其中的一个App出问题，导致其他全部App都不可用？

通常情况下，同一个分发证书打包的多个App不会因为其中一个App出问题而导致其他全部App不可用。每个App都有自己独立的Bundle ID（应用程序包标识符），并且每个App都被视为独立的应用程序，它们之间的运行是相互独立的。

但是，如果某个App使用了共享的库或框架，并且该库或框架出现了问题，那么所有使用该库或框架的App都可能会受到影响。另外，如果某个App存在某种类型的致命错误或者问题，例如占用了过多的系统资源或者导致系统崩溃，那么这个App可能会影响其他应用程序的性能或稳定性。

总的来说，同一个分发证书打包的多个App通常是相互独立的，但在某些情况下，其中一个App可能会影响其他App的运行。因此，当出现问题时，需要仔细检查问题所在，并采取适当的措施来解决问题。

### 1.2. 苹果开发者账号Certificates, Identifiers，Profiles有数量上的限制吗？

1.  Certificates: 每个开发者账号最多可以创建100个证书。
2.  Identifiers: 每个开发者账号最多可以创建100个App IDs和100个Provisioning Profiles。
3.  Profiles: 每个开发者账号最多可以创建100个Provisioning Profiles。
