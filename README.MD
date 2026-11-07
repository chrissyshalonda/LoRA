ПРОДОЛЖЕНИЕ ПРОШЛОГО НОУТБУКА - РЕЗУЛЬАТ В generations.md

РЕЗУЛЬТАТ ЭТОГО НОУТБУКА - generations_2.md

что меняем: LoRA: Хармс - хМЮЛХЙ ХРЭ ЯЪЕДХ БШЬХЙ - по ошибке установил кодировку windows-1251, меняю на KOI8-R1

логи обучения первого ноутбука: logs_first.txt

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
Попробуем передавать title в make_messages, изменения:

def make_messages(story: str, title: str) -> list[dict]:
def getitem(self, index: int) -> dict: messages = make_messages( self.rows[index]["text"] self.rows[index]["title"].strip() )
Также изменим кол-во эпох для теста + у Чехова большой корпус текстов.

!python task/train_lora.py --data data/harms.jsonl --out output/harms_title --epochs 2
!python task/train_lora.py --data data/chekhov.jsonl --out output/chekhov_title --epochs 1
!python task/generate.py --out generations_title.md

