 Index em Python usando POO (Programação Orientada a Objetos), Index Manager

---

📁 Estrutura recomendada

python-index-poo/
│
├── main.py
├── index_manager.py
├── file_index.py
├── utils.py
├── requirements.txt
└── README.md


---

🎯 Objetivo do Projeto

Criar um sistema que:

Indexa ficheiros de uma pasta

Guarda metadados (nome, tamanho, caminho)

Permite pesquisar ficheiros

Guarda índice em JSON

Usa POO corretamente



---

🧠 Conceito POO

Teremos:

1️⃣ Classe FileIndex

Representa um ficheiro individual

2️⃣ Classe IndexManager

Controla o índice completo

3️⃣ Classe utilitária (opcional)


---

🧩 file_index.py

import os
from datetime import datetime

class FileIndex:
    def __init__(self, path):
        self.path = path
        self.name = os.path.basename(path)
        self.size = os.path.getsize(path)
        self.created_at = datetime.fromtimestamp(os.path.getctime(path))

    def to_dict(self):
        return {
            "name": self.name,
            "path": self.path,
            "size": self.size,
            "created_at": self.created_at.strftime("%Y-%m-%d %H:%M:%S")
        }

    def __str__(self):
        return f"{self.name} ({self.size} bytes)"


---

🧩 index_manager.py

import os
import json
from file_index import FileIndex

class IndexManager:
    def __init__(self):
        self.files = []

    def index_directory(self, directory):
        if not os.path.exists(directory):
            raise FileNotFoundError("Diretório não encontrado.")

        for root, _, files in os.walk(directory):
            for file in files:
                full_path = os.path.join(root, file)
                try:
                    indexed_file = FileIndex(full_path)
                    self.files.append(indexed_file)
                except Exception:
                    pass

    def search(self, keyword):
        return [file for file in self.files if keyword.lower() in file.name.lower()]

    def save_to_json(self, filename="index.json"):
        with open(filename, "w", encoding="utf-8") as f:
            json.dump([file.to_dict() for file in self.files], f, indent=4)

    def load_from_json(self, filename="index.json"):
        with open(filename, "r", encoding="utf-8") as f:
            data = json.load(f)
            self.files = data


---

🧩 main.py

from index_manager import IndexManager

def main():
    manager = IndexManager()

    print("=== INDEXADOR DE FICHEIROS ===")
    directory = input("Digite o caminho da pasta: ")

    manager.index_directory(directory)
    manager.save_to_json()

    print("Indexação concluída!")

    while True:
        keyword = input("Pesquisar ficheiro (ou 'sair'): ")

        if keyword.lower() == "sair":
            break

        results = manager.search(keyword)

        if results:
            for file in results:
                print(file)
        else:
            print("Nenhum ficheiro encontrado.")

if __name__ == "__main__":
    main()


---

📄 README.md (Modelo profissional)

# Python Index POO

Sistema de indexação de ficheiros desenvolvido em Python utilizando Programação Orientada a Objetos.

## Funcionalidades

- Indexação de diretórios
- Pesquisa por nome
- Exportação para JSON
- Estrutura modular

## Requisitos

Python 3.10+

## Executar

```bash
python main.py

Estrutura

file_index.py → Representa ficheiro individual

index_manager.py → Gerencia o índice

main.py → Interface CLI


---

# 🚀 Melhorias futuras (podemos desenvolver)

- 🔎 Indexação por extensão
- 🗂️ Filtro por tamanho
- 🌐 Interface Web com Flask
- 🖥️ Interface Qt (como tu costumas fazer)
- 📊 Estatísticas do índice
- 🧵 Threading para indexação rápida
- 🧠 Banco SQLite em vez de JSON
- 🔐 Sistema de autenticação

---

# 🧠 Conceitos POO utilizados

✔ Encapsulamento  
✔ Responsabilidade única  
✔ Modularização  
✔ Separação de camadas  
✔ Classe como modelo de objeto real  

---

atualização:

- 🔥 Transformar isto numa aplicação Qt com GUI
- 🔥 Integrar com Flask
- 🔥 Fazer versão com SQLite profissional
- 🔥 Criar versão para Windows 10 com janela e botões
- 🔥 Criar versão para documentação automática estilo index HTML

