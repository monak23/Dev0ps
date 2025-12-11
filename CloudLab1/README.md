# Лабораторная работа 1. Знакомство с IaaS, PaaS, SaaS сервисами в облаке на примере Amazon Web Services (AWS). Создание сервисной модели.

## 1. Цель работы

Знакомство с облачными сервисами. Понимание уровней абстракции над инфраструктурой в облаке. Формирование понимания типов потребления сервисов в сервисной-модели.

## 2. Исходные данные

Для анализа был предоставлен слепок биллинговых данных AWS в формате CSV, содержащий записи о потреблении различных сервисов с полями:
- **Product Code** — код продукта AWS (например, AmazonS3)
- **Usage Type** — тип использования ресурса (например, %DataTransfer%)
- **lineItem/Operation** — операция (зачастую пустая)
- **lineItem/LineItemDescription** — описание строки счета

**Файл данных:** `Mapping-Rules-AWS-team-10.csv` (47 строк биллинга)

## 3. Ход работы

### 3.1. Импорт и анализ данных

В рамках задания был проанализирован предоставленный файл облачного провайдера Amazon Web Services. Наша команда выбрала файл **"Mapping Rules AWS team 10"**, руководствуясь тем, что в списке наша команда занимает 32 место (остаток от деления 32 на 11 = 10).

На основе анализа полей `Product Code`, `Usage Type` и других атрибутов выполнена структуризация потребляемых сервисов и распределение их по **пятиуровневой иерархии**:

```
IT Tower → Service Family → Service Type → Service Sub Type → Service Usage Type
```

### 3.2. Классификация сервисов

В биллинге были выявлены и классифицированы следующие группы сервисов:

#### 📦 Storage (Хранилище данных) — 7 строк (14.9%)

**Amazon S3 (Simple Storage Service)**  
Масштабируемое и надежное объектное хранилище для хранения неструктурированных данных любого объема, таких как фото, видео, резервные копии и данные для аналитики. Предлагает высокую доступность, безопасность и гибкие классы хранения для оптимизации затрат.

**Российские аналоги:**
- Yandex Object Storage
- SberCloud Object Storage
- VK Cloud S3

#### 🗄️ Database (Базы данных) — 8 строк (17.0%)

**Amazon QLDB (Quantum Ledger Database)**  
Управляемая база данных-реестр, создающая неизменяемый и криптографически проверяемый журнал всех изменений данных. Критически важна для приложений, где необходим полный аудит и доверие к истории данных (финансовые системы, цепи поставок, системы записей).

**Amazon Redshift**  
Облачное хранилище данных (Data Warehouse), предназначенное для выполнения высокопроизводительной аналитики на петабайтах структурированных и полуструктурированных данных с помощью SQL. Используется для бизнес-аналитики, создания отчетов, дашбордов и сложной аналитики в режиме, близком к реальному времени.

**Российские аналоги:**
- Yandex Database (YDB)
- Yandex DataProc (аналог Redshift)
- VK Cloud Big Data

#### 🌐 Networking (Сетевая инфраструктура) — 12 строк (25.5%)

**Amazon VPC (Virtual Private Cloud)**  
Сервис для создания логически изолированных виртуальных сетей в облаке AWS. В этой сети можно запускать ресурсы (например, виртуальные машины) с полным контролем над IP-адресацией, таблицами маршрутизации и сетевыми шлюзами. Это основа для обеспечения безопасности и подключения облачных ресурсов.

**Российские аналоги:**
- Yandex Cloud Virtual Private Cloud
- VK Cloud Network
- SberCloud VPC

#### ☁️ Cloud Services (Облачные сервисы) — 20 строк (42.6%)

**Amazon SES (Simple Email Service)**  
Облачный сервис для отправки и получения электронной почты. Используется для массовых рассылок, транзакционных писем (например, пароли, счета), обеспечивает высокую доставляемость.

**Amazon SNS (Simple Notification Service)**  
Полностью управляемый сервис обмена сообщениями для отправки уведомлений (push, SMS, email) от приложений пользователям или для обмена сообщениями между распределенными компонентами приложений.

**Российские аналоги:**
- Yandex Cloud Message Queue
- VK Cloud Notification Service

### 3.3. Примеры классификации

| Product Code | IT Tower | Service Family | Service Type | Service Sub Type | Service Usage Type |
|-------------|----------|----------------|--------------|------------------|-------------------|
| AmazonS3 | Storage | Object Storage | Amazon S3 | Data Transfer | Data Transfer Out |
| AmazonS3 | Storage | Object Storage | Amazon S3 | Data Storage | Standard Storage |
| AmazonS3 | Storage | Object Storage | Amazon S3 | Content Delivery | CloudFront Distribution |
| AmazonQLDB | Database | NoSQL Database | Ledger Database | Amazon QLDB | Journal Storage |
| AmazonQLDB | Database | NoSQL Database | Ledger Database | Amazon QLDB | IO Requests |
| AmazonRedshift | Database | Data Warehouse | Amazon Redshift | Compute Node | RA3 Node Usage |
| AmazonRedshift | Database | Data Warehouse | Amazon Redshift | Query Execution | Data Scanned |
| AmazonVPC | Networking | Network Infrastructure | Virtual Private Cloud | Amazon VPC | VPN Connection |
| AmazonVPC | Networking | Network Infrastructure | Virtual Private Cloud | Amazon VPC | Transit Gateway Attachment |
| AmazonVPC | Networking | Network Infrastructure | Virtual Private Cloud | Amazon VPC | VPC Endpoint Hours |
| AmazonSES | Cloud Services | Application Services | Email Service | Amazon SES | Email Recipients |
| AmazonSES | Cloud Services | Application Services | Email Service | Amazon SES | Messages Sent |
| AmazonSNS | Cloud Services | Application Services | Messaging Service | Amazon SNS | SMS Messages |
| AmazonSNS | Cloud Services | Application Services | Messaging Service | Amazon SNS | Mobile Push (APNS) |
| AmazonSNS | Cloud Services | Application Services | Messaging Service | Amazon SNS | Lambda Function Invocation |

### 3.4. Статистика распределения по IT Tower

| IT Tower | Количество строк | Процент от общего |
|----------|-----------------|-------------------|
| **Cloud Services** | 20 | 42.6% |
| **Networking** | 12 | 25.5% |
| **Database** | 8 | 17.0% |
| **Storage** | 7 | 14.9% |
| **Итого** | **47** | **100%** |

### 3.5. Детальная структура по сервисам

#### Amazon S3 (Storage)
- Data Transfer Out (2 типа)
- Standard Storage
- Retrieval Operations (2 типа)
- Replication Transfer
- CloudFront Distribution

#### Amazon QLDB (Database)
- Journal Storage
- IO Requests

#### Amazon Redshift (Database)
- RA3 Node Usage (вычислительные узлы)
- DC2 Node Usage
- DS2 Node Usage
- Query Execution (Data Scanned)
- Reserved Instance Utilization
- Tax (накладные расходы)

#### Amazon VPC (Networking)
- Data Transfer (общий и CloudFront)
- VPN Connection
- Transit Gateway (Attachment и Data Processing)
- VPC Endpoint (Hours и Data Processed)
- Traffic Mirroring
- Direct Connect Connection
- Tax (накладные расходы)

#### Amazon SES (Email Service)
- Email Recipients
- Messages Sent
- Attachment Size
- Inbound Email Chunks
- Data Transfer (In/Out)
- Tax

#### Amazon SNS (Messaging Service)
- API Requests (Tier 1 и Tier 2)
- Mobile Push (GCM, APNS)
- Email Delivery (SMTP)
- HTTP/HTTPS Delivery
- SQS Queue Delivery
- Lambda Function Invocation
- SMS Messages
- Data Transfer (In/Out)
- Tax

## 4. Вывод

В результате выполнения лабораторной работы была создана полная сервисная модель потребления облачных сервисов AWS с классификацией **47 строк биллинга** по **5-уровневой иерархии**.

### Ключевые выводы:

1. **Основная доля расходов (42.6%) приходится на Cloud Services** — коммуникационные сервисы Amazon SNS и Amazon SES, что указывает на активное использование сервисов обмена сообщениями, уведомлений и email-рассылок.

2. **Сетевая инфраструктура VPC составляет 25.5%** — второй по значимости блок затрат, включающий передачу данных, VPN-соединения, Transit Gateway и VPC Endpoints. Это свидетельствует о сложной сетевой архитектуре с изолированными виртуальными сетями.

3. **Базы данных (QLDB + Redshift) занимают 17%** — аналитические и транзакционные нагрузки. QLDB используется для неизменяемого хранения критически важных данных, а Redshift — для аналитики больших объемов данных.

4. **Объектное хранилище S3 составляет 14.9%** — используется для хранения данных, их репликации и доставки через CDN (CloudFront).

5. **Модель IaaS/PaaS/SaaS:**
   - **IaaS**: Amazon VPC (сетевая инфраструктура)
   - **PaaS**: Amazon S3, Amazon Redshift, Amazon QLDB (управляемые сервисы хранения и БД)
   - **SaaS**: Amazon SNS, Amazon SES (полностью управляемые сервисы коммуникаций)

Созданная модель позволяет точно отслеживать затраты на каждом уровне детализации (от общего IT Tower до конкретного типа использования) и проводить cost-анализ по категориям потребления для оптимизации расходов на облачную инфраструктуру.

