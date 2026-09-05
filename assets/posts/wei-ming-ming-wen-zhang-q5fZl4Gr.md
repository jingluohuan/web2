# S3存储服务介绍

对象存储是一种用于存储海量非结构化数据的服务，它将数据作为对象（Object）进行管理。下面这个表格汇总了各大主流云服务商的对象存储服务及其核心特性，帮助你快速了解：

云服务商

服务名称

核心特性/存储类别概览

数据持久性

典型应用场景

**Amazon Web Services**

**Amazon S3**[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)

多种存储类：标准、不频繁访问、归档（Glacier系列）、智能分层[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)。高性能S3 Express One Zone[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)。

设计持久性极高（例如99.999999999%）[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)

数据湖、网站、备份恢复、归档、大数据分析[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)

**Microsoft Azure**

**Blob Storage**[-3](https://docs.azure.cn/zh-tw/storage/blobs/storage-blobs-introduction?view=azureml-api-2)

支持块Blob、追加Blob和页Blob[-3](https://docs.azure.cn/zh-tw/storage/blobs/storage-blobs-introduction?view=azureml-api-2)。集成Azure Data Lake Storage Gen2[-3](https://docs.azure.cn/zh-tw/storage/blobs/storage-blobs-introduction?view=azureml-api-2)。

未明确具体数值

直接提供图像/文档、流式处理、备份/归档、大数据分析[-3](https://docs.azure.cn/zh-tw/storage/blobs/storage-blobs-introduction?view=azureml-api-2)

**Google Cloud**

**Cloud Storage**[-2](https://proxy.weglot.com/wg_5ea938e245cc6eb2bdeaf64e79a3d2909/it/en/cloud.google.com/storage/docs/introduction?hl=zh-cn)

统一的API，集成Google大数据和AI服务[-2](https://proxy.weglot.com/wg_5ea938e245cc6eb2bdeaf64e79a3d2909/it/en/cloud.google.com/storage/docs/introduction?hl=zh-cn)。支持数据加密和存储桶锁定[-2](https://proxy.weglot.com/wg_5ea938e245cc6eb2bdeaf64e79a3d2909/it/en/cloud.google.com/storage/docs/introduction?hl=zh-cn)。

未明确具体数值

存储对象以供各种应用访问，支持多种客户端工具和库[-2](https://proxy.weglot.com/wg_5ea938e245cc6eb2bdeaf64e79a3d2909/it/en/cloud.google.com/storage/docs/introduction?hl=zh-cn)

**阿里云**

**对象存储 OSS**[-4](https://www.alibabacloud.com/help/tc/oss/user-guide/object-overview)

多种对象类型（普通、分片、追加、软链接）[-4](https://www.alibabacloud.com/help/tc/oss/user-guide/object-overview)。支持图片处理、视频直播等[-4](https://www.alibabacloud.com/help/tc/oss/user-guide/object-overview)。

未明确具体数值

文件存储与管理、图片和视频处理、大数据分析[-4](https://www.alibabacloud.com/help/tc/oss/user-guide/object-overview)

**腾讯云**

**对象存储 COS**[-7](https://cloud.tencent.com.cn/product/cos?ad_trace=93f671353077465483e28c41e1f7b8e6&amp;from=28056&amp;from_column=28056)

多种存储类型（标准、低频、归档、深度归档、智能分层）[-7](https://cloud.tencent.com.cn/product/cos?ad_trace=93f671353077465483e28c41e1f7b8e6&amp;from=28056&amp;from_column=28056)。与CDN、数据万象等腾讯云产品联动[-7](https://cloud.tencent.com.cn/product/cos?ad_trace=93f671353077465483e28c41e1f7b8e6&amp;from=28056&amp;from_column=28056)。

高达99.9999999999%[-7](https://cloud.tencent.com.cn/product/cos?ad_trace=93f671353077465483e28c41e1f7b8e6&amp;from=28056&amp;from_column=28056)

CDN数据分发、数据万象处理、大数据分析、容灾备份[-7](https://cloud.tencent.com.cn/product/cos?ad_trace=93f671353077465483e28c41e1f7b8e6&amp;from=28056&amp;from_column=28056)

**华为云**

**对象存储服务 OBS**[-8](https://www.huaweicloud.com/guide/productsdesc-bms_3ca192472a5f60bfc9239e18727d0599support0_d)

对象由Key、Data和Metadata组成[-8](https://www.huaweicloud.com/guide/productsdesc-bms_3ca192472a5f60bfc9239e18727d0599support0_d)。通过“/”模拟文件夹[-8](https://www.huaweicloud.com/guide/productsdesc-bms_3ca192472a5f60bfc9239e18727d0599support0_d)。

未明确具体数值

存储各类非结构化数据，如图片、视频、文档等[-8](https://www.huaweicloud.com/guide/productsdesc-bms_3ca192472a5f60bfc9239e18727d0599support0_d)

**Oracle Cloud**

**OCI Object Storage**[-6](https://www.oracle.com/cn/cloud/storage/object-storage/faq/)

跨可用性域或故障域存储[-6](https://www.oracle.com/cn/cloud/storage/object-storage/faq/)。支持元数据标签[-6](https://www.oracle.com/cn/cloud/storage/object-storage/faq/)。

99.999999999%[-6](https://www.oracle.com/cn/cloud/storage/object-storage/faq/)

备份归档、大数据分析、横向扩展的Web应用[-6](https://www.oracle.com/cn/cloud/storage/object-storage/faq/)

**IBM Cloud**

**IBM Cloud Object Storage**[-5](https://dataplatform.cloud.ibm.com/docs/content/wsj/admin/storage-options.html?context=analytics%3Fcontext%3Dwdp)

数据加密和擦除编码[-5](https://dataplatform.cloud.ibm.com/docs/content/wsj/admin/storage-options.html?context=analytics%3Fcontext%3Dwdp)。区域弹性存储[-5](https://dataplatform.cloud.ibm.com/docs/content/wsj/admin/storage-options.html?context=analytics%3Fcontext%3Dwdp)。

未明确具体数值

为watsonx等工作空间提供存储，存放数据文件、模型、日志等[-5](https://dataplatform.cloud.ibm.com/docs/content/wsj/admin/storage-options.html?context=analytics%3Fcontext%3Dwdp)

### 🔑 核心概念与选择考量

尽管各厂商的服务名称和实现细节有所不同，但对象存储服务通常都围绕几个核心概念构建：

- 
**基本结构**：数据以**对象（Object）** 为基本单位，对象通常包含**键（Key，即对象名称）、数据（Data）和元数据（Metadata）**[-8](https://www.huaweicloud.com/guide/productsdesc-bms_3ca192472a5f60bfc9239e18727d0599support0_d)。对象被组织在**存储桶（Bucket** 或 **Container**）中[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)[-3](https://docs.azure.cn/zh-tw/storage/blobs/storage-blobs-introduction?view=azureml-api-2)。

- 
**存储类别**：如表中所见，几乎所有主流厂商都提供了**标准、低频访问和归档**等不同性能和价格的存储类别，以满足不同的访问模式和成本要求[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)[-7](https://cloud.tencent.com.cn/product/cos?ad_trace=93f671353077465483e28c41e1f7b8e6&amp;from=28056&amp;from_column=28056)。你可以利用**生命周期管理**策略，让数据在不同存储类别间自动转换，从而优化成本[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)。

- 
**安全与访问管理**：数据安全是重中之重。这些服务通常提供**默认加密**（静态和传输中）、精细的**访问控制策略**（如IAM策略、存储桶策略）[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)[-2](https://proxy.weglot.com/wg_5ea938e245cc6eb2bdeaf64e79a3d2909/it/en/cloud.google.com/storage/docs/introduction?hl=zh-cn)，并且默认情况下存储桶和对象都是私有的[-1](https://docs.amazonaws.cn/AmazonS3/latest/userguide/Welcome.html)。

在选择对象存储服务时，除了考虑表格中列出的特性和场景，你还可以从以下几个方面进行考量：

- 
**生态集成**：服务与你正在使用或计划使用的其他云服务（如计算实例、大数据分析工具、AI/ML平台）的集成度。

- 
**成本结构**：不同存储类别的定价，以及请求费用、数据传输费用的差异。

- 
**性能和延迟**：对于高性能应用，需要关注服务的访问延迟和吞吐量。

- 
**合规性与数据驻地**：数据存储的物理位置是否符合当地法规或企业政策。
