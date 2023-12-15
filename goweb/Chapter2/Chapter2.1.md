# 微服务概览

## 单体架构

最终打包部署为单体式应用，应用国语复杂，任何单个开发者都不可能搞懂，应用无法拓展，可靠性低，敏捷性开发和部署无法完成。

## 微服务的起源

什么是SOA（面向服务的架构模式），微服务是SOA的时间。

- 小即是美：小的服务代码少，bug少，易测试，易维护，更容易不断迭代完善。
- 单一职责：一个服务也只需要做好一件事。
- 尽可能早的创建原型：尽可能早的提供服务API，建立服务契约，达成服务间沟通的一致性约定，至于实现和完善可以后续。
- 可移植性比效率重要：服务间的轻量级交互协议在效率和可移植性二者之间，首先依然考虑兼容性和移植性。

## 微服务定义

围绕业务功能构建，服务关注单一业务，服务间采用轻量级的通信机制，自动独立部署，可以使用不同的编程语言和数据存储技术。微服务架构通过业务拆分实现服务组件化，通过组件组合快速开发系统，业务单一的服务组件又可以独立部署。

- 原子服务
- 独立进程
- 隔离部署
- 去中心化服务治理

缺点：基础设施的建设，复杂度高

## 微服务不足

- 微服务应用是分布式系统，由此会带来固有的复杂性。开发者不得不使用RPC活消息传递来实现进程通信，必须处理消息传递中速度过慢或者服务不可用局部失效等问题。
- 微服务架构应用中需要更新不同服务所使用的不同的数据库，从而对开发者要求更高。
- 对于微服务的测试也是很复杂
- 服务模块间的依赖导致在应用升级会可能波及多个服务模块的修改。

## 组件服务化

传统实现组件的方式是通过库（library)，库是和应用一起运行在进程中，库的局部变化意味着整个应用的重新部署。通过服务来实现组件，意味着将应用拆散为一系列的服务运行在不同的进程中，那么单一服务的局部变化只需重新部署对应的服务进程。

用Go 实施一个微服务:

- kit: 一个微服务的基础库（框架）
- service:业务代码+kit依赖+第三方依赖组成的业务微服务
- RPC + message queue: 轻量级通讯

多个微服务组合（compose） 完成了一个完整的用户场景（usecase）

## 按业务组织服务

按业务能力组织服务的意思是服务提供的能力和业务功能对应，比如：订单服务和数据访问服务，前者反应了真实的订单相关业务，后者是一种技术抽象服务，不反应真实的业务。所以按微服务架构理念来划分服务时，是不应该存在数据访问服务这样一个服务的。

一般为大前端（移动/Web） > 网关接入 > 业务服务 > 平台服务 > 基础设施 （PaaS/Saas）

## 去中心化

- 数据去中心化
- 治理去中心化
- 技术去中心化

每个服务独享自身的数据存储设施（缓存，数据库等），不像传统应用共享一个缓存和数据库，这样有利于服务的独立性，隔离相关干扰。

## 基础设施自动化

无自动化不微服务，自动化包括测试和部署。单一进程的传统应用被拆分为一系列的多进程服务后，意味着开发、调试、测试、监控和部署的复杂度都会相应增大，必须要有合适的自动化设施来支持微服务架构模式，否则开发、运维成本将大大增加

- CI/CD: Gitlab + Gitlab Hooks + Kubernetes
- Testing: 测试环境、单元测试、 API自动化测试
- 在线运行时： Kubernetes, 以及一系列Prometheus、 ELK、 Control Panel

## 可用性&兼容性

- 隔离
- 超时控制
- 负载保护
- 限流
- 降级
- 重试
- 负载均衡

发送时要保守，接收时要开放。按照伯斯塔尔法则的思想来设计和实现服务时，发送的数据要更保守，意味着最小化的传送必要的信息，接收时更开放意味着要最大限度的容忍冗余数据，保证兼容性。