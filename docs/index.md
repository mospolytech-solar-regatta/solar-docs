# Документация проекта mospolytech-solar

В этой документации описаны принципы работы системы проекта mospolytech-solar.


## Основные компоненты

Проект состоит из трех больших частей:

* Модули лодки
* Модули берега
* Модули тестирования и поддержки

## Модули лодки
Эти модули разворачиваются непосредственно на контроллерах лодки для полноценной работы системы.

На arduino загружается код и репозитория [boat-controller](https://github.com/mospolytech-solar-regatta/boat-controller). 
Arduino собирает данные с датчиков и отправляет на контроллер лодки в виде телеметрии, сериализованной в формате json.

Остальные модули разворачиваются на контроллере (Raspberry Pi) лодки как сервисы Linux.
Обязательное условие для сервисов лодки, работать в среде linux на процессорах ARM архитектуры.

* API лодки [solar-boat-connector](https://github.com/mospolytech-solar-regatta/solar-boat-connector)
> Этот модуль является центральным модулем лодки, он отвечает за связь всех компонентов.
> В этом модуле производится обработка всех данных и их запись в базу данных.
> Также этот модуль отвечает за хранение конфигурации остальных. Это веб сервер, он взаимодействует с solar-connector
> через Redis pub/sub и по протоколу HTTP со всеми остальными модулями.
* Коннектор лодки [solar-connector](https://github.com/mospolytech-solar-regatta/solar-connector)
> Универсальный коннектор для данных, получаемых по USB. Умеет автоматически обнаруживать порт arduino и подключаться к нему.
> Все взаимодействие с этим модулем производится через Redis pub/sub.
* Монитор [solar-monitor](https://github.com/mospolytech-solar-regatta/solar-monitor)
> Интерфейс лодки для отображения телеметрии. Взаимодействует с solar-connector для получения обновлений по HTTP.

### Базовый сценарий работы
``` mermaid
sequenceDiagram
  loop UART
    Arduino->>Connector: Telemetry (json)
  end
  loop Redis pub/sub
    Connector-->>API: Telemetry (json)
    Connector-->>API: Current serial config (json)
  end
  loop HTTP
    Monitor->>API: Request telemetry
    API->>Monitor: Response telemetry 
  end
```

## Модули берега
TBD

## Модули поддержки

