<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "f00aedb7b1d11b7eaacb0618d8791c65",
  "translation_date": "2025-05-29T22:59:37+00:00",
  "source_file": "02-Security/README.md",
  "language_code": "ru"
}
-->
# Лучшие практики безопасности

Внедрение Model Context Protocol (MCP) открывает новые возможности для приложений с искусственным интеллектом, но также приносит уникальные вызовы в области безопасности, выходящие за рамки традиционных рисков программного обеспечения. Помимо устоявшихся вопросов, таких как безопасное программирование, принцип наименьших привилегий и безопасность цепочки поставок, MCP и AI-нагрузки сталкиваются с новыми угрозами, такими как внедрение подсказок (prompt injection), отравление инструментов и динамическое изменение инструментов. Если эти риски не контролировать, это может привести к утечке данных, нарушению конфиденциальности и непредсказуемому поведению системы.

В этом уроке рассматриваются наиболее актуальные риски безопасности, связанные с MCP — включая аутентификацию, авторизацию, чрезмерные права, косвенное внедрение подсказок и уязвимости цепочки поставок — а также предлагаются практические меры и лучшие практики для их снижения. Вы также узнаете, как использовать решения Microsoft, такие как Prompt Shields, Azure Content Safety и GitHub Advanced Security, чтобы усилить безопасность реализации MCP. Понимание и применение этих мер позволит значительно снизить вероятность нарушения безопасности и обеспечить надежность и доверие к вашим AI-системам.

# Цели обучения

К концу этого урока вы сможете:

- Определять и объяснять уникальные риски безопасности, которые приносит Model Context Protocol (MCP), включая внедрение подсказок, отравление инструментов, чрезмерные права и уязвимости цепочки поставок.
- Описывать и применять эффективные меры по снижению рисков безопасности MCP, такие как надежная аутентификация, принцип наименьших привилегий, безопасное управление токенами и проверка цепочки поставок.
- Понимать и использовать решения Microsoft, такие как Prompt Shields, Azure Content Safety и GitHub Advanced Security, для защиты MCP и AI-нагрузок.
- Осознавать важность проверки метаданных инструментов, мониторинга динамических изменений и защиты от косвенных атак внедрения подсказок.
- Интегрировать проверенные лучшие практики безопасности — такие как безопасное программирование, усиление серверов и архитектура с нулевым доверием — в реализацию MCP для снижения вероятности и последствий нарушений безопасности.

# Контроль безопасности MCP

Любая система с доступом к важным ресурсам сталкивается с определёнными проблемами безопасности. Обычно эти проблемы решаются правильным применением базовых мер и концепций безопасности. Поскольку MCP — это относительно новый протокол, его спецификация быстро меняется и развивается. В конечном итоге меры безопасности внутри него станут более зрелыми, что позволит лучше интегрировать MCP с корпоративными и проверенными архитектурами и лучшими практиками безопасности.

Исследование, опубликованное в [Microsoft Digital Defense Report](https://aka.ms/mddr), показывает, что 98% зарегистрированных нарушений можно предотвратить при соблюдении надежной гигиены безопасности. Лучшей защитой от любых видов нарушений является правильное соблюдение базовой гигиены безопасности, лучших практик безопасного программирования и обеспечения безопасности цепочки поставок — именно эти проверенные временем методы дают наибольший эффект в снижении рисков.

Рассмотрим некоторые способы, как начать решать вопросы безопасности при внедрении MCP.

> **Note:** Следующая информация актуальна по состоянию на **29 мая 2025 года**. Протокол MCP постоянно развивается, и будущие реализации могут вводить новые схемы аутентификации и меры контроля. Для получения последних обновлений и рекомендаций всегда обращайтесь к [MCP Specification](https://spec.modelcontextprotocol.io/), официальному [репозиторию MCP на GitHub](https://github.com/modelcontextprotocol) и [странице лучших практик безопасности MCP](https://modelcontextprotocol.io/specification/draft/basic/security_best_practices).

### Постановка задачи  
Изначальная спецификация MCP предполагала, что разработчики сами будут писать сервер аутентификации. Это требовало знаний OAuth и связанных с ним ограничений безопасности. MCP-серверы выступали в роли OAuth 2.0 Authorization Servers, напрямую управляя аутентификацией пользователей, а не делегируя её внешним сервисам, таким как Microsoft Entra ID. С **26 апреля 2025 года** обновление спецификации MCP позволяет MCP-серверам делегировать аутентификацию пользователей внешним сервисам.

### Риски
- Некорректная настройка логики авторизации на MCP-сервере может привести к раскрытию конфиденциальных данных и неправильному применению контроля доступа.
- Кража OAuth-токена на локальном MCP-сервере. Если токен украден, его можно использовать для имитации MCP-сервера и доступа к ресурсам и данным, защищённым этим токеном.

#### Проброс токенов (Token Passthrough)
Проброс токенов явно запрещён в спецификации авторизации, так как он создаёт ряд рисков безопасности, включая:

#### Обход мер безопасности
MCP-сервер или API на нижестоящем уровне могут реализовывать важные меры безопасности, такие как ограничение частоты запросов, проверка запросов или мониторинг трафика, которые зависят от аудитории токена или других ограничений учётных данных. Если клиенты могут напрямую получать и использовать токены с нижестоящих API без правильной проверки MCP-сервером или без подтверждения, что токены выданы для нужного сервиса, эти меры обходятся.

#### Проблемы ответственности и аудита
MCP-сервер не сможет идентифицировать или различать MCP-клиентов, если клиенты вызывают сервис с токеном, выданным на верхнем уровне, который может быть непрозрачным для MCP-сервера. Логи ресурсного сервера могут показывать запросы, которые выглядят исходящими от другого источника с иной идентичностью, а не от MCP-сервера, который фактически пересылает токены. Это усложняет расследование инцидентов, контроль и аудит. Если MCP-сервер передаёт токены без проверки их утверждений (например, ролей, привилегий или аудитории) или другой метадаты, злоумышленник с украденным токеном может использовать сервер как прокси для утечки данных.

#### Проблемы с границами доверия
Ресурсный сервер доверяет конкретным субъектам, что может включать предположения об источнике или поведении клиента. Нарушение этой границы доверия может привести к неожиданным проблемам. Если токен принимается несколькими сервисами без надлежащей проверки, злоумышленник, скомпрометировавший один сервис, сможет использовать токен для доступа к другим связанным сервисам.

#### Риск совместимости в будущем
Даже если MCP-сервер сегодня действует как «чистый прокси», позже ему может потребоваться добавить меры безопасности. Начало с правильного разделения аудитории токенов упрощает развитие модели безопасности.

### Меры по снижению рисков

**MCP-серверы НЕ ДОЛЖНЫ принимать токены, которые явно не были выданы для MCP-сервера**

- **Проверьте и усилите логику авторизации:** Тщательно аудируйте реализацию авторизации на MCP-сервере, чтобы гарантировать доступ к конфиденциальным ресурсам только для назначенных пользователей и клиентов. Для практических рекомендаций смотрите [Azure API Management Your Auth Gateway For MCP Servers | Microsoft Community Hub](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690) и [Using Microsoft Entra ID To Authenticate With MCP Servers Via Sessions - Den Delimarsky](https://den.dev/blog/mcp-server-auth-entra-id-session/).
- **Соблюдайте безопасные практики работы с токенами:** Следуйте [лучшим практикам Microsoft по проверке токенов и срокам их действия](https://learn.microsoft.com/en-us/entra/identity-platform/access-tokens), чтобы предотвратить неправильное использование токенов доступа и снизить риск их повторного использования или кражи.
- **Защищайте хранение токенов:** Всегда храните токены в защищённом виде и используйте шифрование для защиты данных в состоянии покоя и при передаче. Советы по реализации смотрите в [Use secure token storage and encrypt tokens](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2).

# Чрезмерные права у MCP-серверов

### Постановка задачи
MCP-серверам могли быть предоставлены избыточные права на сервис или ресурс, к которому они обращаются. Например, MCP-сервер, являющийся частью AI-приложения для продаж, подключающийся к корпоративному хранилищу данных, должен иметь доступ только к данным по продажам, а не ко всем файлам в хранилище. Обратимся к принципу наименьших привилегий (одному из самых старых принципов безопасности): ни один ресурс не должен иметь прав больше, чем необходимо для выполнения своих задач. AI создаёт дополнительные сложности, так как для обеспечения гибкости порой сложно точно определить необходимые права.

### Риски  
- Предоставление чрезмерных прав может привести к утечке или изменению данных, к которым MCP-сервер не должен иметь доступа. Это также может стать проблемой конфиденциальности, если данные содержат персональную информацию (PII).

### Меры по снижению рисков
- **Применяйте принцип наименьших привилегий:** Предоставляйте MCP-серверу только минимально необходимые права для выполнения его задач. Регулярно пересматривайте и обновляйте эти права, чтобы они не превышали необходимый уровень. Для подробных рекомендаций смотрите [Secure least-privileged access](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access).
- **Используйте ролевой контроль доступа (RBAC):** Назначайте MCP-серверу роли, строго ограниченные конкретными ресурсами и действиями, избегая широких или ненужных прав.
- **Мониторинг и аудит прав:** Постоянно отслеживайте использование прав и проверяйте журналы доступа, чтобы быстро выявлять и устранять чрезмерные или неиспользуемые привилегии.

# Косвенные атаки внедрения подсказок

### Постановка задачи

Зловредные или скомпрометированные MCP-серверы могут создавать значительные риски, раскрывая данные клиентов или вызывая нежелательные действия. Эти риски особенно актуальны для AI и нагрузок на базе MCP, где:

- **Атаки внедрения подсказок (Prompt Injection):** Злоумышленники внедряют вредоносные инструкции в подсказки или внешний контент, заставляя AI-систему выполнять нежелательные действия или раскрывать конфиденциальные данные. Подробнее: [Prompt Injection](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/)
- **Отравление инструментов (Tool Poisoning):** Атакующие манипулируют метаданными инструментов (например, описаниями или параметрами), чтобы повлиять на поведение AI, обходить меры безопасности или выводить данные. Подробнее: [Tool Poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks)
- **Кросс-доменные атаки внедрения подсказок:** Вредоносные инструкции встраиваются в документы, веб-страницы или письма, которые затем обрабатываются AI, что приводит к утечке данных или манипуляциям.
- **Динамическое изменение инструментов (Rug Pulls):** Определения инструментов могут изменяться после одобрения пользователем, вводя новые вредоносные функции без его ведома.

Эти уязвимости подчёркивают необходимость надёжной проверки, мониторинга и мер безопасности при интеграции MCP-серверов и инструментов в вашу среду. Для более глубокого изучения см. ссылки выше.

![prompt-injection-lg-2048x1034](../../../translated_images/prompt-injection.ed9fbfde297ca877c15bc6daa808681cd3c3dc7bf27bbbda342ef1ba5fc4f52d.ru.png)

**Косвенное внедрение подсказок** (также известное как кросс-доменное внедрение подсказок или XPIA) — критическая уязвимость в генеративных AI-системах, включая использующие Model Context Protocol (MCP). В этой атаке вредоносные инструкции скрываются во внешнем контенте — например, документах, веб-страницах или письмах. Когда AI обрабатывает этот контент, он может воспринять встроенные инструкции как легитимные команды пользователя, что приводит к нежелательным действиям: утечке данных, генерации вредоносного контента или манипуляциям с взаимодействием пользователя. Для подробного объяснения и реальных примеров смотрите [Prompt Injection](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/).

Особо опасна форма этой атаки — **Отравление инструментов**. Здесь злоумышленники внедряют вредоносные инструкции в метаданные MCP-инструментов (например, описания или параметры). Поскольку большие языковые модели (LLM) используют эти метаданные для выбора вызываемых инструментов, скомпрометированные описания могут обмануть модель и заставить её выполнить неавторизованные вызовы инструментов или обойти меры безопасности. Эти манипуляции часто незаметны для конечных пользователей, но интерпретируются и выполняются AI-системой. Риск возрастает в хостинговых MCP-средах, где определения инструментов могут обновляться после одобрения пользователем — такой сценарий иногда называют «[rug pull](https://www.wiz.io/blog/mcp-security-research-briefing#remote-servers-22)». В таких случаях инструмент, ранее безопасный, может быть изменён для выполнения вредоносных действий, например, утечки данных или изменения поведения системы без ведома пользователя. Подробнее об этом в [Tool Poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks).

![tool-injection-lg-2048x1239 (1)](../../../translated_images/tool-injection.3b0b4a6b24de6befe7d3afdeae44138ef005881aebcfc84c6f61369ce31e3640.ru.png)

## Риски
Непреднамеренные действия AI несут различные риски безопасности, включая утечку данных и нарушение конфиденциальности.

### Меры по снижению рисков
### Использование Prompt Shields для защиты от косвенных атак внедрения подсказок
-----------------------------------------------------------------------------

**AI Prompt Shields** — решение Microsoft для защиты от прямых и косвенных атак внедрения подсказок. Они помогают за счёт:

1.  **Обнаружения и фильтрации:** Prompt Shields используют продвинутые алгоритмы машинного обучения и обработку естественного языка для выявления и фильтрации вредоносных инструкций, встроенных во внешний контент — документы, веб-страницы или письма.
    
2.  **Spotlighting (выделение):** Эта техника помогает AI отличать легитимные системные инструкции от потенциально ненадёжных внешних данных. Spotlighting преобразует входной текст так, чтобы он стал более релевантным для модели, позволяя лучше распознавать и игнорировать вредоносные инструкции.
    
3.  **Разделители и маркировка данных:** Включение разделителей в системное сообщение явно указывает расположение входного текста, помогая AI отделять пользовательский ввод от потенциально опасного внешнего контента. Маркировка данных расширяет эту концепцию, используя специальные метки для выделения границ доверенных и недоверенных данных.
    
4.  **Постоянный мониторинг и обновления:** Microsoft непрерывно отслеживает и обновляет Prompt Shields, чтобы справляться с новыми и эволюционирующими угрозами. Такой проактивный подход обеспечивает эффективность защиты от современных техник атак.
    
5. **Интеграция с Azure Content Safety:** Prompt Shields являются частью более широкой платформы Azure AI Content Safety, которая предоставляет дополнительные инструменты для обнаружения попыток обхода защиты, вредоносного контента и других рисков безопасности в AI-приложениях.

Подробнее о AI Prompt Shields читайте в [документации Prompt Shields](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection).

![prompt-shield-lg-2048x1328](../../../translated_images/prompt-shield.ff5b95be76e9c78c6ec0888206a4a6a0a5ab4bb787832a9eceef7a62fe0138d1.ru.png)

### Безопасность цепочки поставок

Безопасность цепочки поставок остаётся ключевой в эпоху AI, но теперь её охват расширился. Помимо традиционных пакетов кода, необходимо тщательно проверять и контролировать все AI-компоненты, включая базовые модели, сервисы эмбеддингов, поставщиков контекста и сторонние API. Каждый из этих элементов может внести уязвимости или риски при недостаточном управлении.

**Основные практики безопасности цепочки поставок для AI и MCP:**
- **Проверяйте все компоненты перед интеграцией:** Это касается не только open-source библиотек, но и AI-моделей, источников данных и внешних API. Всегда проверяйте происхождение, лицензии и известные уязвимости.
- **Поддерживайте безопасные процессы деплоя:** Используйте автоматизированные CI/CD-пайплайны с интегрированным сканированием безопасности для раннего выявления проблем. Убедитесь, что в продакшен попадают только проверенные артефакты.
- **Постоянно мониторьте и проводите аудит:** Внедрите непрерывный мониторинг всех зависимостей, включая модели и сервисы данных, чтобы обнаруживать новые уязвимости или атаки на цепочку поставок.
- **Применяйте принцип наименьших привилегий и контроль доступа:** Ограничьте доступ к моделям, данным и сервисам только тем, что необходимо для работы MCP-сервера.
- **Быстро реагируйте на угрозы:** Имейте процессы для патчей или замены скомпрометированных компонентов, а также для ротации секретов и учётных данных в случае обнаружения нарушений.

[GitHub Advanced Security](https://github.com/security/advanced-security) предлагает функции сканирования секретов, зависимостей и анализ CodeQL. Эти инструменты интегрируются с [Azure DevOps](https://azure.microsoft.com/en-us/products/devops) и [Azure Repos](https://azure.microsoft.com/en-us/products/devops/repos/), помог
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP Top 10 for LLMs](https://genai.owasp.org/download/43299/?tmstv=1731900559)
- [GitHub Advanced Security](https://github.com/security/advanced-security)
- [Azure DevOps](https://azure.microsoft.com/products/devops)
- [Azure Repos](https://azure.microsoft.com/products/devops/repos/)
- [Путь к обеспечению безопасности цепочки поставок программного обеспечения в Microsoft](https://devblogs.microsoft.com/engineering-at-microsoft/the-journey-to-secure-the-software-supply-chain-at-microsoft/)
- [Безопасный доступ с минимальными привилегиями (Microsoft)](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access)
- [Лучшие практики проверки токенов и срока их действия](https://learn.microsoft.com/entra/identity-platform/access-tokens)
- [Использование безопасного хранения токенов и их шифрование (YouTube)](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2)
- [Azure API Management как шлюз аутентификации для MCP](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690)
- [Лучшие практики безопасности MCP](https://modelcontextprotocol.io/specification/draft/basic/security_best_practices)
- [Использование Microsoft Entra ID для аутентификации на MCP серверах](https://den.dev/blog/mcp-server-auth-entra-id-session/)

### Далее

Далее: [Глава 3: Начало работы](/03-GettingStarted/README.md)

**Отказ от ответственности**:  
Этот документ был переведен с помощью сервиса автоматического перевода [Co-op Translator](https://github.com/Azure/co-op-translator). Несмотря на наши усилия по обеспечению точности, просим учитывать, что автоматический перевод может содержать ошибки или неточности. Оригинальный документ на его исходном языке следует считать авторитетным источником. Для критически важной информации рекомендуется обращаться к профессиональному переводу, выполненному человеком. Мы не несем ответственности за любые недоразумения или неправильные толкования, возникшие в результате использования данного перевода.