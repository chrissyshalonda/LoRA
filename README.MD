# Продолжение экспериментов с LoRA

## Результаты предыдущего ноутбука
- Результат: `generations.md`
- Логи обучения: `logs_first.txt`

## Изменения в текущем эксперименте

### 1. Изменение кодировки LoRA для Хармса

В предыдущем эксперименте для LoRA Хармса по ошибке была установлена кодировка `windows-1251`.

В текущем эксперименте меняем её на `KOI8-R1`.

### 2. Подготовка окружения и обучение

```python
from google.colab import drive
drive.mount("/content/drive")
!cp LoRA.zip /content/drive/MyDrive/
!cp /content/drive/MyDrive/LoRA.zip .
!unzip LoRA.zip
%cd LoRA
!pip uninstall -y torchao
!python task/prepare_data.py --author harms --out data/harms.jsonl
!python task/train_lora.py --data data/harms.jsonl --out output/harms
!python task/generate.py --out generations_2.md
```

**Результат текущего ноутбука:** `generations_2.md`

## Дополнительные эксперименты

### 3. Передача title в make_messages

Попробуем передавать `title` в функцию `make_messages`.

Изменения:

```python
def make_messages(story: str, title: str) -> list[dict]:
```

Изменение метода `getitem`:

```python
def getitem(self, index: int) -> dict:
    messages = make_messages(
        self.rows[index]["text"],
        self.rows[index]["title"].strip()
    )
```

### 4. Изменение количества эпох обучения

Для тестирования изменим количество эпох. Также добавим Чехова, у которого большой корпус текстов.

Обучение LoRA для Хармса (2 эпохи):

```python
!python task/train_lora.py --data data/harms.jsonl --out output/harms_title --epochs 2
```

Обучение LoRA для Чехова (1 эпоха):

```python
!python task/train_lora.py --data data/chekhov.jsonl --out output/chekhov_title --epochs 1
```

### 5. Генерация результатов

```python
!python task/generate.py --out generations_title.md
```

**Результат дополнительного эксперимента:** `generations_title.md`

