# Набор интеграционных и end-to-end тестов

## Описание

Набор тестов для проверки корректности работы системы OrchestrPay. Покрывает основной flow, corner cases и компенсационные сценарии.

## Таблица тест-кейсов

| Название | Тип | Компоненты | Предусловия |
|----------|-----|------------|-------------|
| **Happy Path - Успешный платеж** | E2E | Payment Orchestrator, Payment Service, FraudCheck Service, Notification Service | Пользователь с достаточным балансом, валидные данные платежа, fraud проверки проходят |
| **Валидация данных - Неверный формат** | Integration | Payment Orchestrator, Payment Service | Платеж с некорректными данными (неверный IBAN, сумма < 0) |
| **Валидация данных - Отсутствующие поля** | Integration | Payment Orchestrator, Payment Service | Платеж без обязательных полей (получатель, сумма) |
| **Недостаточно средств** | E2E | Payment Orchestrator, Payment Service | Пользователь с балансом меньше суммы платежа |
| **Fraud Internal - Отклонение** | Integration | Payment Orchestrator, FraudCheck Service | Транзакция с подозрительными параметрами (большая сумма, необычное время) |
| **Fraud External - Отклонение** | Integration | Payment Orchestrator, FraudCheck Service, External Fraud API | Транзакция в черном списке внешней системы |
| **Fraud External - Таймаут** | Integration | Payment Orchestrator, FraudCheck Service, External Fraud API | Внешний fraud сервис не отвечает в течение 30 секунд |
| **Manual Review - Одобрение оператором** | E2E | Payment Orchestrator, FraudCheck Service, Manual Review UI | Транзакция требует ручной проверки, оператор одобряет |
| **Manual Review - Отклонение оператором** | E2E | Payment Orchestrator, FraudCheck Service, Manual Review UI | Транзакция требует ручной проверки, оператор отклоняет |
| **Manual Review - Cut-off Time** | E2E | Payment Orchestrator, FraudCheck Service, Timer Service | Транзакция в ручной проверке 20+ минут, автоматическое одобрение |
| **PIVOT POINT - Успешный перевод** | E2E | Payment Orchestrator, Payment Service, Counterparty Service | Все проверки пройдены, перевод контрагенту выполнен |
| **PIVOT POINT - Ошибка перевода** | E2E | Payment Orchestrator, Payment Service, Counterparty Service | Ошибка при переводе контрагенту (недоступен сервис) |
| **Компенсация - Возврат после fraud** | E2E | Payment Orchestrator, Payment Service | Fraud отклонен после списания, автоматический возврат |
| **Компенсация - Возврат после ручной проверки** | E2E | Payment Orchestrator, Payment Service | Ручная проверка отклонена, автоматический возврат |
| **Компенсация - Освобождение резерва** | Integration | Payment Orchestrator, Payment Service | Недостаточно средств, освобождение зарезервированных средств |
| **Уведомления - Успешный платеж** | Integration | Payment Orchestrator, Notification Service | Платеж успешен, отправка уведомления пользователю |
| **Уведомления - Отклонение** | Integration | Payment Orchestrator, Notification Service | Платеж отклонен, отправка уведомления пользователю |
| **Уведомления - Fraud в безопасность** | Integration | Payment Orchestrator, Notification Service, Security Service | Fraud обнаружен, уведомление в службу безопасности |
| **Уведомления - Ошибка системы** | Integration | Payment Orchestrator, Notification Service, Support Service | Системная ошибка, уведомление в службу поддержки |
| **Идемпотентность - Повторный запрос** | Integration | Payment Orchestrator, Payment Service | Повторный запрос с тем же ID платежа |
| **Идемпотентность - Компенсация** | Integration | Payment Orchestrator, Payment Service | Повторный вызов компенсационной операции |
| **Нагрузочное тестирование** | E2E | Все компоненты системы | 1000+ одновременных платежей |
| **Отказоустойчивость - Payment Service недоступен** | Integration | Payment Orchestrator, Payment Service | Payment Service не отвечает, retry механизм |
| **Отказоустойчивость - FraudCheck Service недоступен** | Integration | Payment Orchestrator, FraudCheck Service | FraudCheck Service не отвечает, fallback на ручную проверку |
| **Отказоустойчивость - Notification Service недоступен** | Integration | Payment Orchestrator, Notification Service | Notification Service не отвечает, логирование ошибки |
| **Консистентность данных** | E2E | Payment Orchestrator, Payment Service, Database | Проверка консистентности данных после отката транзакции |
| **Мониторинг и логирование** | Integration | Все компоненты системы | Проверка корректности логов и метрик |
| **Безопасность - SQL Injection** | Integration | Payment Orchestrator, Payment Service | Попытка SQL injection в параметрах платежа |
| **Безопасность - XSS атака** | Integration | Payment Orchestrator, Manual Review UI | Попытка XSS в комментариях ручной проверки |
| **Производительность - Время ответа** | E2E | Все компоненты системы | Время обработки платежа не превышает 30 секунд |
| **Производительность - Пропускная способность** | E2E | Все компоненты системы | Система обрабатывает 100 платежей в минуту |
