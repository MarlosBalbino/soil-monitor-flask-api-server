# Soil Monitor Server

O **Soil Monitor Server** é uma API desenvolvida em **Flask** responsável por intermediar a comunicação entre o firmware da **ESP32-WROVER** e o **Dashboard**.

A ESP32 envia periodicamente as medições para o servidor via **HTTP POST**. O Dashboard, por sua vez, consulta essas informações por meio de requisições **HTTP GET** e as apresenta em tempo real ao usuário.

## Arquitetura

```text
ESP32-WROVER
      │
      │ HTTP POST
      ▼
+-------------------+
|   Flask Server    |
+-------------------+
      ▲
      │ HTTP GET
      │
Dashboard (Frontend)
```

## Fluxo de comunicação

1. A ESP32 realiza uma requisição **POST** para `/api/dados`, enviando as medições dos sensores.
2. O servidor armazena os dados mais recentes em memória.
3. O Dashboard realiza requisições **GET** para `/api/dados`.
4. O servidor retorna os dados ao frontend, que atualiza os gráficos e indicadores.

## Exemplo de logs

### Dados enviados pela ESP32

Supondo que a ESP32 possua o endereço IP `192.168.0.6`:

```text
192.168.0.6 - - [06/Jul/2026 00:12:43] "POST /api/dados HTTP/1.1" 200
```

### Dados solicitados pelo Dashboard

Supondo que o servidor possua o endereço IP `192.168.0.5`:

```text
192.168.0.5 - - [06/Jul/2026 00:12:45] "GET /api/dados HTTP/1.1" 200
```

## Configuração do endereço IP

O endereço IP do servidor deve ser configurado em **dois componentes** do sistema:

- **Firmware da ESP32:** para enviar os dados ao servidor via HTTP POST.
- **Frontend (Dashboard):** para consultar os dados disponíveis via HTTP GET.

> **Importante:** todos os dispositivos (ESP32, servidor e frontend) devem estar conectados à mesma rede e o IP configurado (servidor) deve corresponder ao computador onde a API Flask está em execução.

## Resumo

- **ESP32-WROVER:** coleta os dados e envia ao servidor (`POST`).
- **Flask Server:** recebe e disponibiliza os dados (`GET`).
- **Dashboard:** consulta os dados e atualiza a interface.

## Executar servidor

Instale as dependências, executando o comando no terminal:

```
pip install -r requirements.txt
```

Rode o servidor:

```
py server.py
```