## LLM04: 模型拒绝服务

### 描述

攻击者与LLM（大型语言模型）交互的方式会消耗异常多的资源，导致其自身和其他用户的服务质量下降，可能还会产生高昂的资源成本。此外，一个新兴的主要安全关切是攻击者可能干扰或操纵LLM的上下文窗口。由于LLM在各种应用中的使用越来越广泛，它们对资源的密集利用、用户输入的不可预测性以及开发人员对这一漏洞的普遍不了解，这个问题变得越来越关键。在LLM中，上下文窗口代表模型可以处理的文本的最大长度，包括输入和输出。这是LLM的一个关键特性，因为它决定了模型可以理解的语言模式的复杂性以及它可以在任何给定时间处理的文本的大小。上下文窗口的大小由模型的架构定义，并可能因模型而异。

### 漏洞的常见示例

1. 提出导致通过高容量生成任务队列中的高卷入而导致资源使用量不断重复的查询，例如使用LangChain或AutoGPT。
2. 发送异常消耗资源的查询，使用异常的拼写法或序列。
3. 持续输入溢出：攻击者向LLM发送超出其上下文窗口的输入流，导致模型消耗过多的计算资源。
4. 重复的长输入：攻击者反复向LLM发送超出上下文窗口的长输入。
5. 递归上下文扩展：攻击者构建的输入触发递归上下文扩展，强制LLM反复扩展和处理上下文窗口。
6. 变长输入洪水攻击：攻击者向LLM洪水般发送大量的变长输入，其中每个输入都精心制作，以达到上下文窗口的限制。这种技术旨在利用处理变长输入的任何效率低下之处，使LLM负担过重，可能导致其无法响应。

### 预防和缓解策略

1. 实施输入验证和清理，以确保用户输入符合定义的限制，并过滤掉任何恶意内容。
2. 限制每个请求或步骤的资源使用，以便涉及复杂部分的请求执行更慢。
3. 强制执行API速率限制，限制个体用户或IP地址在特定时间内可以发出的请求数量。
4. 限制排队操作的数量和对LLM响应的系统中的总操作数量。
5. 持续监视LLM的资源利用情况，以识别异常的峰值或模式，可能表明存在拒绝服务攻击。
6. 基于LLM的上下文窗口设置严格的输入限制，以防止超载和资源耗尽。
7. 向开发人员提供有关LLM中可能存在的拒绝服务漏洞的意识，并提供安全LLM实施的指南。

### 示例攻击场景

1. 攻击者反复向托管模型发送多个困难且耗费大量资源的请求，导致其他用户的服务质量下降，托管方的资源费用增加。
2. 在LLM驱动的工具正在收集信息以回应良性查询时，遇到网页上的一段文本。这导致工具发出了更多的网页请求，导致大量资源消耗。
3. 攻击者持续不断地向LLM发送超出其上下文窗口的输入。攻击者可能使用自动化脚本或工具发送大量输入，压倒LLM的处理能力。结果，LLM消耗了过多的计算资源，导致系统明显减慢或完全不响应。
4. 攻击者向LLM发送一系列连续的输入，每个输入都设计为略低于上下文窗口的限制。通过反复提交这些输入，攻击者旨在耗尽可用的上下文窗口容量。由于LLM在其上下文窗口内努力处理每个输入，系统资源变得紧张，可能导致性能下降或完全拒绝服务。
5. 攻击者利用LLM的递归机制反复触发上下文扩展。通过制作利用LLM递归行为的输入，攻击者迫使模型反复扩展和处理上下文窗口，消耗大量计算资源。这种攻击会使系统受到压力，可能导致拒绝服务状况，使LLM无法响应或导致崩溃。
6. 攻击者向LLM洪水般发送大量的变长输入，经过精心制作，以接近或达到上下文窗口的限制。通过用不同长度的输入淹没LLM，攻击者旨在利用处理变长输入的任何低效之处。这些输入的洪水会对LLM的资源造成过多负担，可能导致性能下降，妨碍系统响应合法请求。
7. 尽管拒绝服务攻击通常旨在超载系统资源，但它们也可能利用系统行为的其他方面，例如API限制。例如，在最近的Sourcegraph安全事件中，

恶意行为者使用泄露的管理员访问令牌来更改API速率限制，从而通过启用异常高的请求量可能导致服务中断。 

### 参考链接

1. [LangChain max_iterations](https://twitter.com/hwchase17/status/1608467493877579777): **Twitter上的hwchase17**
2. [海绵示例：对神经网络的能量-延迟攻击](https://arxiv.org/abs/2006.03463): **Arxiv白皮书**
3. [OWASP拒绝服务攻击](https://owasp.org/www-community/attacks/Denial_of_Service): **OWASP**
4. [从机器中学到：了解上下文](https://lukebechtel.com/blog/lfm-know-thy-context): **Luke Bechtel**
5. [关于API限制操纵和拒绝服务攻击的Sourcegraph安全事件](https://about.sourcegraph.com/blog/security-update-august-2023): **Sourcegraph**