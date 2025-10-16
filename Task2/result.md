# Таблица переходов состояний платежа

## Описание

Таблица переходов состояний для процесса обработки платежей в системе OrchestrPay. Отражает все возможные состояния платежа и события, которые переводят его из одного состояния в другое.

## Диаграмма

![State Transitions](img/State%20Transitions%20-%20Detailed%20View.png)

## Таблица переходов

| Исходное состояние | Переходное состояние | Событие |
|-------------------|---------------------|---------|
| CREATED | VALIDATING | VALIDATE_PAYMENT_DATA |
| VALIDATING | FUNDS_RESERVED | RESERVE_USER_FUNDS |
| VALIDATING | VALIDATION_FAILED | Ошибка валидации |
| FUNDS_RESERVED | DEBITED | DEBIT_USER_ACCOUNT |
| FUNDS_RESERVED | VALIDATION_FAILED | Недостаточно средств |
| DEBITED | FRAUD_CHECKING | FRAUD_CHECK_INTERNAL |
| FRAUD_CHECKING | FRAUD_APPROVED | Проверка пройдена |
| FRAUD_CHECKING | MANUAL_REVIEW | Требуется ручная проверка |
| FRAUD_CHECKING | FRAUD_REJECTED | Fraud обнаружен |
| FRAUD_CHECKING | FRAUD_ERROR | Ошибка проверки |
| MANUAL_REVIEW | FRAUD_APPROVED | Оператор одобрил |
| MANUAL_REVIEW | FRAUD_REJECTED | Оператор отклонил |
| MANUAL_REVIEW | AUTO_APPROVED | Cut-off time (20 минут) |
| AUTO_APPROVED | FRAUD_APPROVED | APPLY_CUT_OFF_TIME |
| FRAUD_APPROVED | CREDITED | CREDIT_COUNTERPARTY |
| CREDITED | SUCCESS | UPDATE_PAYMENT_STATUS_SUCCESS |
| SUCCESS | COMPLETED | NOTIFY_USER_SUCCESS |
| FRAUD_REJECTED | REFUNDING | REFUND_USER |
| FRAUD_ERROR | REFUNDING | REFUND_USER |
| REFUNDING | REFUNDED | Возврат выполнен |
| REFUNDED | FAILED | UPDATE_PAYMENT_STATUS_FAILED |
| VALIDATION_FAILED | RELEASING_FUNDS | RELEASE_RESERVED_FUNDS |
| RELEASING_FUNDS | FAILED | Резерв освобожден |
| FAILED | COMPLETED | NOTIFY_USER_FAILURE |

## Финальные состояния

- **SUCCESS** - платеж успешно выполнен
- **REFUNDED** - средства возвращены после списания
- **FAILED** - платеж отклонен без списания
- **COMPLETED** - процесс полностью завершен

## Критические переходы

1. **DEBITED → CREDITED** - pivot point, откат невозможен
2. **DEBITED → REFUNDING** - компенсация при fraud/ошибке
3. **MANUAL_REVIEW → AUTO_APPROVED** - автоматическое одобрение по таймауту
