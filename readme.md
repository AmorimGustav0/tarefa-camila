# Projeto IoT – Integração MQTT, MySQL e API REST

Este projeto em Python consome mensagens de uma **Bancada Didática 4.0 – Camila** via MQTT, persiste os dados em **MySQL** e disponibiliza consultas através de uma **API REST** usando **FastAPI**.

---

## 🔹 Tecnologias utilizadas

- Python 3.11+
- FastAPI
- Paho-MQTT
- SQLAlchemy / MySQL-Connector
- MySQL
- Uvicorn
- dotenv

---

## 🔹 Estrutura do projeto

app/
├─ api/
│ └─ main.py # FastAPI
├─ mqtt_client.py # Cliente MQTT e atualização de estados
├─ config.py # Configurações do .env
├─ db.py # Conexão MySQL
└─ crud.py # Funções de persistência no banco
.venv/ # Ambiente virtual
.env # Variáveis de ambiente
requirements.txt # Dependências do projeto


---

## 🔹 Configuração do ambiente

### 1. Clonar o projeto
```powershell
git clone <URL_DO_SEU_REPOSITORIO>
cd <PASTA_DO_PROJETO>

2. Criar e ativar o ambiente virtual
python -m venv .venv
.\.venv\Scripts\Activate.ps1

3. Instalar dependências
pip install -r requirements.txt

🔹 Configurar o .env

Exemplo de arquivo .env:

# MQTT
MQTT_BROKER=1ce4694cef1a4e389f05e446b6a52cc6.s1.eu.hivemq.cloud
MQTT_PORT=8883
MQTT_USERNAME=smart40n1
MQTT_PASSWORD=iOTC39Mc1dLg
MQTT_TOPICS=#

# MySQL
DB_HOST=localhost
DB_PORT=3306
DB_USER=iotuser
DB_PASS=secret_password
DB_NAME=iot_db

# API
APP_HOST=0.0.0.0
APP_PORT=8000


Importante: Ajuste as credenciais do MySQL e do MQTT conforme necessário.

🔹 Criar o banco de dados MySQL

No terminal MySQL:

CREATE DATABASE iot_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'iotuser'@'%' IDENTIFIED BY 'secret_password';
GRANT ALL PRIVILEGES ON iot_db.* TO 'iotuser'@'%';
FLUSH PRIVILEGES;


Certifique-se de que o usuário e senha correspondem ao .env.

🔹 Rodar a API e o MQTT juntos

No PowerShell (com o ambiente virtual ativado):

uvicorn app.api.main:app --reload --host 0.0.0.0 --port 8000


Saída esperada:

INFO: Uvicorn running on http://0.0.0.0:8000
[MQTT] conectado com rc=0
[MQTT] inscrito em: #
[MQTT] Cliente MQTT iniciado junto com a API


O MQTT será iniciado automaticamente junto com a API.

O dicionário estados_atualizados armazena o estado mais recente de cada variável recebida.

🔹 Testar a API
1. Usando navegador

Abra:

http://localhost:8000/docs


Interface Swagger para testar rotas:

/estados → retorna todas as variáveis e valores atuais.

/estados/{variavel} → retorna apenas a variável desejada.

2. Usando curl (opcional)
curl http://localhost:8000/estados
curl http://localhost:8000/estados/operacao_manipulacao

🔹 Testar recebimento de mensagens MQTT

Para enviar mensagens de teste para a API (ou para confirmar que o MQTT está recebendo):

mosquitto_pub -h 1ce4694cef1a4e389f05e446b6a52cc6.s1.eu.hivemq.cloud -p 8883 -u smart40n1 -P iOTC39Mc1dLg -t "#" -m '{"variable":"operacao_manipulacao","value":"PRODUZINDO"}'


No terminal da API, você verá algo como:

[MQTT] Atualizado estados_atualizados: {'operacao_manipulacao': 'PRODUZINDO'}


Acesse /estados para confirmar que a variável foi atualizada.