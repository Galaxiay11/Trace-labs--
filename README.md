#  PKI & TLS LABS  
**Public Key Infrastructure + Transport Layer Security**

### Curso: 2º Ano de CTeSP em Cibersegurança — ISTEC Porto  
**Unidade Curricular:** Criptografia Aplicada à Cibersegurança  
**Docente:** Flávio Vaz  
**Ano Letivo:** 2025

---

##  Descrição

Este projeto inclui dois laboratórios práticos sobre **criptografia aplicada à cibersegurança**, desenvolvidos no âmbito da UC *Criptografia Aplicada à Cibersegurança*.  
Os laboratórios abordam:

- **PKI Lab (Public Key Infrastructure)** — Criação e gestão de uma Autoridade Certificadora (CA), geração de *Certificate Signing Requests (CSR)* e implementação de HTTPS em Apache com certificados próprios.  
- **TLS Lab (Transport Layer Security)** — Desenvolvimento de cliente e servidor TLS em Python, com suporte a *Subject Alternative Names (SAN)* e ligação segura a partir de browsers e scripts.

---

##  Objetivos

- Compreender a encriptação assimétrica e o papel das infraestruturas de chave pública (PKI).  
- Criar e assinar certificados digitais em ambiente controlado.  
- Configurar servidores Apache com HTTPS e validação de certificados.  
- Implementar programas cliente/servidor TLS e analisar o *handshake*.  
- Entender como a criptografia protege a integridade e confidencialidade das comunicações.

---

##  Ferramentas e Tecnologias

- **OpenSSL**
- **Docker** e **SEED Labs Environment**
- **Apache2**
- **Python3** (scripts de cliente e servidor TLS)
- **Wireshark** (análise de tráfego)
- **curl**, **nano**, **bash**
- **Certificados autoassinados**

---

##  Estrutura do Projeto

PKI_TLS_Lab/

├── report/

│ └── PKI_TLS_Report.pdf

├── apache/

│ ├── grupo4_apache_ssl.conf

│ └── index.html

├── openssl/

│ ├── myCA_openssl.cnf

│ └── myopenssl.cnf

└── tls_scripts/

├── handshake.py

├── server.py

└── client-certs/


---

##  Instruções de Execução

###  PKI LAB

1. **Criar e configurar uma Autoridade Certificadora (CA):**

   cp /usr/lib/ssl/openssl.cnf myCA_openssl.cnf
   nano myCA_openssl.cnf
   mkdir -p demoCA/newcerts
   touch demoCA/index.txt
   echo 1000 > demoCA/serial
   openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
     -keyout ca.key -out ca.crt \
     -subj "/CN=www.modelCA.com/O=Model CA LTD./C=US" \
     -passout pass:dees
   
Gerar um pedido de certificado (CSR):
openssl req -newkey rsa:2048 -sha256 \
  -keyout server.key -out server.csr \
  -subj "/CN=www.bank32.com/O=Bank32 Inc./C=US" \
  -addext "subjectAltName=DNS:www.bank32.com,DNS:www.bank32A.com,DNS:www.bank32B.com" \
  -passout pass:dees
  
Assinar o certificado:
openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key -config myCA_openssl.cnf

Deploy em servidor Apache com HTTPS:
Copiar os ficheiros .crt e .key para o container Apache.

Criar grupo4_apache_ssl.conf com as diretivas:
SSLEngine On
SSLCertificateFile /certs/server.crt
SSLCertificateKeyFile /certs/server.key

Ativar SSL e o site:
a2enmod ssl
a2ensite grupo4_apache_ssl
service apache2 restart

Testar ligação segura:
curl -v https://www.grupo4.com --cacert ca.crt


## TLS LAB
Implementar cliente TLS:
python3 handshake.py www.example.com
Mostra o certificado do servidor e o cipher suite utilizado.

Implementar servidor TLS:
python3 server.py
Servidor escuta na porta 443 e responde a pedidos HTTPS.

Testar com navegador:
Importar o certificado da CA em Firefox.
Aceder a https://www.bank32.com e verificar o cadeado verde.

## Contramedidas e Boas Práticas
Usar copy_extensions = copy no openssl.cnf para SANs.
Renovar certificados antes da expiração.
Utilizar chaves ≥2048 bits (ou ECC).
Testar configuração com SSL Labs.
Automatizar CA e renovação com Let's Encrypt (para ambientes reais).

##  Referências
SEED Labs – PKI Lab
SEED Labs – TLS Lab
Stallings, W. (2017) – Cryptography and Network Security
Rescorla, E. (2018) – The Transport Layer Security (TLS) Protocol 1.3

## Autores
Nome                                    	Nº
Francisco Rafael Carocinho Ribeiro	2024123
José Samuel da Rocha Oliveira	2024172
Tiago Filipe Sousa Carvalho	2024180
Ana Rita da Silva Monteiro	2024041
Vanina Kollen	2024056
