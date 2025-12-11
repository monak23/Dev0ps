# Лабораторная работа 2. Сравнение сервисов Amazon Web Services и Microsoft Azure. Создание единой кросс-провайдерной сервисной модели.

## 1. Цель работы

Получение навыков аналитики и понимания спектра публичных облачных сервисов без привязки к вендору. Формирование у студентов комплексного видения облака.

## 2. Исходные данные

Для анализа были предоставлены два слепка биллинговых данных:

### Лабораторная работа 1 (AWS)
- **Файл:** `Mapping-Rules-AWS-team-10.csv`
- **Объем:** 47 строк биллинга
- **Провайдер:** Amazon Web Services (AWS)
- **Основные сервисы:** S3, QLDB, Redshift, VPC, SNS, SES

### Лабораторная работа 2 (Azure)
- **Файл:** `Azure-lab-team-10.csv`
- **Объем:** 42 строки биллинга
- **Провайдер:** Microsoft Azure
- **Основные сервисы:** Virtual Machines, Databricks, HDInsight, Data Factory, PostgreSQL, Azure Monitor

## 3. Ход работы

### 3.1. Классификация Azure сервисов

На основе анализа полей `Meter Category`, `Meter Sub-Category`, `Meter Name` и `Consumed Service` выполнена структуризация Azure сервисов по **5-уровневой иерархии**:

```
IT Tower → Service Family → Service Type → Service Sub Type → Service Usage Type
```

#### 🗄️ Database (Базы данных и аналитика) — 20 строк (47.6%)

**Azure Database for PostgreSQL / MySQL**  
Управляемые реляционные базы данных с полной совместимостью с PostgreSQL и MySQL. Обеспечивают автоматическое резервное копирование, масштабирование и высокую доступность без необходимости управления инфраструктурой.

**Azure Databricks**  
Унифицированная аналитическая платформа на базе Apache Spark для обработки больших данных (Big Data). Поддерживает Data Analytics и Data Engineering workloads, используется для машинного обучения и потоковой обработки данных.

**Azure HDInsight**  
Управляемый сервис для кластеров Hadoop, Spark, Kafka и других фреймворков обработки больших данных. Позволяет обрабатывать петабайты данных для аналитики и ETL-процессов.

**Azure Data Factory**  
Сервис интеграции данных для создания ETL/ELT пайплайнов. Поддерживает перемещение данных между облаком и on-premises инфраструктурой, а также оркестрацию данных с различной частотой выполнения (High/Low Frequency).

**Azure Data Lake Store**  
Масштабируемое хранилище для анализа больших данных. Оптимизировано для хранения структурированных и неструктурированных данных с поддержкой транзакций и аналитических запросов.

**Azure Stream Analytics**  
Сервис обработки данных в реальном времени (real-time analytics). Обрабатывает потоковые данные из IoT-устройств, приложений и датчиков с использованием Streaming Units (SU).

**Azure Redis Cache**  
Управляемый in-memory кэш на базе Redis для ускорения доступа к данным. Поддерживает различные размеры инстансов (C-Series) для разных нагрузок.

**Российские аналоги:**
- Yandex Database (YDB) — аналог Azure SQL/PostgreSQL
- Yandex DataProc — аналог Databricks/HDInsight
- VK Cloud Big Data — аналог Data Factory

#### ☁️ Cloud Services (Вычислительные ресурсы) — 17 строк (40.5%)

**Azure Virtual Machines (A/D/F/G/H/N Series)**  
Виртуальные машины различных серий:
- **A-Series:** Basic/Standard вычисления (разработка/тестирование)
- **D-Series:** General Purpose (универсальные рабочие нагрузки)
- **F-Series:** Compute Optimized (высокая производительность CPU)
- **G-Series:** Memory Optimized (работа с большими объемами RAM)
- **H-Series:** High Performance Computing (HPC для научных расчетов)
- **N-Series:** GPU-ускоренные вычисления (машинное обучение, рендеринг)

**Azure Batch**  
Сервис для запуска крупномасштабных параллельных вычислений (batch jobs). Автоматически управляет пулом виртуальных машин для выполнения задач.

**Azure Monitor**  
Мониторинг и управление облачными ресурсами. Отправляет уведомления через Webhook и Email при возникновении событий или превышении порогов метрик.

**Российские аналоги:**
- Yandex Compute Cloud — виртуальные машины
- VK Cloud Compute — аналог Azure VMs
- SberCloud Virtual Machines

#### 📦 Storage (Хранение и резервное копирование) — 3 строки (7.1%)

**Azure Backup**  
Сервис резервного копирования данных с поддержкой Geo-Redundant Storage (GRS). Обеспечивает защиту виртуальных машин (VM Protected Instances) и данных с автоматической репликацией между регионами.

**Российские аналоги:**
- Yandex Cloud Backup
- VK Cloud Backup Service

#### 🌐 Networking (Доставка контента) — 2 строки (4.8%)

**Azure CDN (Content Delivery Network)**  
Глобальная сеть доставки контента для ускорения загрузки статических и динамических файлов. Кэширует данные в географически распределенных точках присутствия (PoP).

**Российские аналоги:**
- Yandex Cloud CDN
- VK Cloud CDN

### 3.2. Примеры классификации Azure

| Meter Category | IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type |
|---------------|----------|----------------|--------------|------------------|-------------------|
| Azure Database for PostgreSQL | Database | Relational Database | Managed Database | Azure Database for PostgreSQL | Database Instance |
| Azure Databricks | Database | Big Data Analytics | Azure Databricks | Unified Analytics Platform | Data Analytics Workload |
| Azure Monitor | Cloud Services | Monitoring & Management | Azure Monitor | Notifications | Webhook Notifications |
| Backup | Storage | Backup & Disaster Recovery | Azure Backup | Cloud Backup | GRS Backup Storage |
| Business Analytics | Database | Big Data & Analytics | Azure HDInsight | Managed Hadoop/Spark | Compute Hours |
| Business Analytics | Database | Big Data & Analytics | Azure Data Factory | Data Integration | Data Movement Cloud |
| Business Analytics | Database | Big Data & Analytics | Azure Stream Analytics | Real-time Analytics | Streaming Units (Hours) |
| Cache | Database | In-Memory Cache | Azure Redis Cache | Managed Redis | Cache Operations |
| CDN | Networking | Content Delivery | Azure CDN | Content Delivery Network | CDN Data Transfer |
| Cloud Services | Cloud Services | Compute | Virtual Machines | A-Series (Basic/Standard) | A-Series Compute Hours |
| Cloud Services | Cloud Services | Compute | Virtual Machines | D-Series (General Purpose) | D-Series Compute Hours |
| Cloud Services | Cloud Services | Compute | Virtual Machines | N-Series (GPU) | N-Series Compute Hours |

### 3.3. Сравнительная таблица AWS vs Azure

| IT Tower | AWS Строк | AWS % | Azure Строк | Azure % | AWS Сервисы | Azure Сервисы |
|----------|-----------|-------|-------------|---------|-------------|---------------|
| **Storage** | 7 | 14.9% | 3 | 7.1% | Amazon S3 (Object Storage) | Azure Backup (Backup & DR) |
| **Database** | 8 | 17.0% | 20 | 47.6% | QLDB, Redshift | PostgreSQL, Databricks, HDInsight, Data Factory, Stream Analytics, Redis Cache |
| **Networking** | 12 | 25.5% | 2 | 4.8% | Amazon VPC (VPN, Transit Gateway, Endpoints) | Azure CDN |
| **Cloud Services** | 20 | 42.6% | 17 | 40.5% | SNS, SES (Messaging/Email) | Virtual Machines (6 серий), Azure Monitor, Batch |
| **ИТОГО** | **47** | **100%** | **42** | **100%** | 6 сервисов | 12+ сервисов |

### 3.4. Кросс-провайдерное сравнение

#### Архитектурные различия

**AWS (Amazon Web Services):**
- Фокус на **коммуникационные сервисы** (SNS/SES) — 42.6% биллинга
- Развитая **сетевая инфраструктура** (VPC с VPN, Transit Gateway) — 25.5%
- Специализированные БД: QLDB (ledger database), Redshift (data warehouse)
- Объектное хранилище S3 с интеграцией CloudFront CDN — 14.9%

**Microsoft Azure:**
- Фокус на **аналитику больших данных** (Databricks, HDInsight, Data Factory) — 47.6%
- Широкий спектр **виртуальных машин** (6 серий: A/D/F/G/H/N) — 40.5%
- Минимальная сетевая инфраструктура (только CDN) — 4.8%
- Резервное копирование (Azure Backup) вместо объектного хранилища — 7.1%

#### Сравнение по моделям IaaS/PaaS/SaaS

| Модель | AWS | Azure |
|--------|-----|-------|
| **IaaS** | Amazon VPC (сетевая инфраструктура) | Virtual Machines (A/D/F/G/H/N series), Azure CDN |
| **PaaS** | Amazon S3, QLDB, Redshift | Azure Database (PostgreSQL/MySQL), Databricks, HDInsight, Data Factory, Stream Analytics, Redis Cache, Backup |
| **SaaS** | Amazon SNS, Amazon SES | Azure Monitor |

#### Ключевые выводы из сравнения

1. **AWS** использует **коммуникационную модель** (42.6% на SNS/SES), что указывает на архитектуру с активным обменом сообщениями между микросервисами и уведомлениями пользователей.

2. **Azure** использует **аналитическую модель** (47.6% на Big Data сервисы), что указывает на обработку больших объемов данных, ETL-процессы и machine learning workloads.

3. **Сетевая инфраструктура:** AWS инвестирует в VPC (25.5%), Azure минимизирует сетевые затраты (4.8% только на CDN).

4. **Вычислительные ресурсы:** AWS использует специализированные сервисы (SNS/SES), Azure использует универсальные VM различных серий (40.5%).

5. **Хранение данных:** AWS — объектное хранилище S3 (14.9%), Azure — резервное копирование Azure Backup (7.1%).

### 3.5. Единая кросс-провайдерная модель

На основе анализа обоих провайдеров создана **унифицированная 5-уровневая иерархия**, которая позволяет сопоставить сервисы AWS и Azure:

| AWS Сервис | Azure Сервис | IT Tower | Service Family |
|-----------|--------------|----------|----------------|
| Amazon S3 | Azure Backup | Storage | Object Storage / Backup & DR |
| Amazon QLDB | Azure Database for PostgreSQL | Database | NoSQL / Relational Database |
| Amazon Redshift | Azure Databricks, HDInsight | Database | Data Warehouse / Big Data Analytics |
| Amazon VPC | Azure CDN | Networking | Virtual Network / Content Delivery |
| Amazon SNS/SES | Azure Monitor | Cloud Services | Messaging / Monitoring |
| — | Azure VMs (A/D/F/G/H/N) | Cloud Services | Compute (IaaS) |
| — | Azure Data Factory | Database | Data Integration (ETL) |
| — | Azure Stream Analytics | Database | Real-time Analytics |
| — | Azure Redis Cache | Database | In-Memory Cache |

## 4. Вывод

В результате выполнения лабораторной работы создана **единая кросс-провайдерная сервисная модель**, объединяющая классификации AWS (47 строк) и Azure (42 строки) в общую 5-уровневую иерархию.

### Ключевые выводы:

1. **Различия в архитектурных подходах:**
   - AWS использует **event-driven архитектуру** с активным использованием SNS/SES (42.6%)
   - Azure использует **data-driven архитектуру** с фокусом на Big Data и аналитику (47.6%)

2. **Модели потребления:**
   - AWS: 42.6% SaaS (SNS/SES) + 39.4% PaaS (S3/QLDB/Redshift) + 25.5% IaaS (VPC)
   - Azure: 47.6% PaaS (Databases/Analytics) + 40.5% IaaS (VMs) + 4.8% IaaS (CDN)

3. **Специализация провайдеров:**
   - **AWS** лидирует в сетевой инфраструктуре (VPC: 25.5% vs Azure CDN: 4.8%)
   - **Azure** лидирует в аналитике данных (47.6% vs AWS: 17%)
   - **AWS** использует специализированные сервисы (SNS/SES для коммуникаций)
   - **Azure** использует универсальные VM для различных задач

4. **Практическая ценность модели:**
   - Позволяет сравнивать затраты между провайдерами на уровне IT Tower
   - Упрощает миграцию между облаками (cloud migration)
   - Помогает выбрать оптимального провайдера под конкретную архитектуру (event-driven vs data-driven)

5. **Рекомендации по выбору провайдера:**
   - Для **микросервисной архитектуры с активным обменом сообщениями** → AWS (SNS/SES)
   - Для **Big Data и аналитики** → Azure (Databricks/HDInsight/Data Factory)
   - Для **сложной сетевой топологии** → AWS (VPC с Transit Gateway)
   - Для **разнообразия вычислительных ресурсов** → Azure (6 серий VM: A/D/F/G/H/N)

Созданная модель демонстрирует, что **выбор облачного провайдера должен основываться на архитектурных требованиях проекта**, а не на предпочтениях вендора.

