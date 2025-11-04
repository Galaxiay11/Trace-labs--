# Projeto: Laboratórios de Criptografia & Segurança (PKI, TLS e Reentrancy)

**Curso:** 2º Ano do CTeSP em Cibersegurança — ISTEC Porto  
**Unidade curricular:** Criptografia Aplicada à Cibersegurança  
**Ano:** 2025  
**Professor:** Flávio Vaz

---

## Resumo do Repositório

Este repositório reúne os relatórios e os materiais práticos desenvolvidos de acordo com os laboratórios:

- **PKI Lab** — Infraestrutura de Chave Pública: criação de CA, CSR, certificados, deploy em Apache (HTTPS).
- **TLS Lab** — Implementação e teste de cliente/servidor TLS em Python; configuração de SAN e testes com browser.
- **Reentrancy Attack Lab** — Exploração de vulnerabilidade Reentrancy em smart contracts (Ethereum), implementação do contrato atacante e contramedidas.

---

## Autores

- Francisco Rafael Carocinho Ribeiro — 2024123  
- José Samuel da Rocha Oliveira — 2024172  
- Tiago Filipe Sousa Carvalho — 2024180  
- Ana Rita da Silva Monteiro — 2024041  
- Vanina Kollen — 2024056

---

##  Estrutura do Repositório 

.
├── PKI_TLS_Lab/

│ ├── report/

│ │ └── PKI_TLS_Report.md (ou PDF)

│ ├── openssl_configs/

│ ├── apache/

│ └── tls_scripts/

│ ├── handshake.py

│ └── server.py

│
├── Reentrancy_Lab/

│ ├── contract/

│ │ ├── ReentrancyVictim.sol

│ │ └── ReentrancyAttacker.sol

│ ├── scripts/

│ │ ├── deploy_victim.py

│ │ ├── deploy_attacker.py

│ │ ├── launch_attack.py

│ │ ├── get_balance.py

│ │ └── cashout.py

│ └── report/

│ └── Reentrancy_Attack_Report.pdf

│

└── README.md
---

## Ferramentas e Tecnologias

**PKI & TLS**
- OpenSSL, Apache2, Docker, curl, Wireshark
- Python3 (scripts de teste)
- Certificados, CSRs, configuração `openssl.cnf`, SANs
- Navegadores (importar CA no Firefox)

**Reentrancy**
- Solidity 0.6.8, Remix / solc
- Geth / Ganache (nó local)
- Web3.py, Python3
- Docker (ambientes SEED Labs)

---

## Instruções rápidas de execução

### PKI & TLS (resumo)
1. Copiar/editar `openssl.cnf` -> `myopenssl.cnf` e descomentar `unique_subject = no` / `copy_extensions = copy` conforme necessário.  
2. Criar CA e gerar `ca.crt` / `ca.key`.  
3. Gerar CSR e key do servidor (`openssl req -newkey ... -out server.csr -keyout server.key`), incluir SAN via `-addext` ou ficheiro de config.  
4. Assinar CSR com a CA (`openssl ca` ou `openssl x509 -req ...`).  
5. Copiar certificados para o container Apache e configurar `VirtualHost` com `SSLCertificateFile` e `SSLCertificateKeyFile`.  
6. Nos clientes: testar com `curl --cacert ca.crt https://<host>` e/ou importar CA no browser.

### Reentrancy (resumo)
1. Compilar contratos:
   ```bash
   ./solc-0.6.8 --overwrite --abi --bin -o . ReentrancyVictim.sol
   ./solc-0.6.8 --overwrite --abi --bin -o . ReentrancyAttacker.sol
Deploy do contrato vítima:
python3 scripts/deploy_victim.py

Deploy do contrato atacante (certificar que passas o address como string, não lista):
python3 scripts/deploy_attacker.py

Executar ataque:
python3 scripts/launch_attack.py

Verificar saldos / cashout:
python3 scripts/get_balance.py
python3 scripts/cashout.py
Nota: os scripts pressupõem ligação a um nó local (Geth/Ganache) e contas desbloqueadas, bem como caminhos corretos para .abi e .bin.

## Contramedidas e Boas Práticas
PKI & TLS

-Garantir uso de chaves e algoritmos atualizados (p.ex. RSA ≥ 2048, ou ECC).

-Configuração correta de SAN e chain de confiança.

-Automatizar renovação e monitoração (Let’s Encrypt, ACME quando aplicável).


Reentrancy

-Padrão Checks-Effects-Interactions (actualizar estado antes de enviar fundos).

-Usar mutex/reentrancy guard (ex.: bool locked ou ReentrancyGuard da OpenZeppelin).

-Preferir transfer/send (com cautela e conforme a versão do Solidity) ou retirar para pull payments em vez de push payments.

-Revisões/auditorias com ferramentas automáticas (Slither, Mythril, Oyente).

## Referências
SEED Labs – PKI & TLS & Reentrancy Labs (materiais base).

Phil Daian — Analysis of the DAO exploit (2016).

Andreas M. Antonopoulos & Gavin Wood — Mastering Ethereum (2018).

SEED Labs PDFs: Reentrancy_Attack.pdf, Crypto_PKI e Crypto_TLS.



