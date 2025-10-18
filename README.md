#  REENTRANCY ATTACK LAB

### Curso: 2º Ano de CTeSP em Cibersegurança — ISTEC Porto  
**Unidade Curricular:** Criptografia Aplicada à Cibersegurança  
**Docente:** Flávio Vaz  
**Ano Letivo:** 2025

---

## Descrição

Este laboratório prático tem como objetivo compreender e explorar a vulnerabilidade **Reentrancy Attack** em contratos inteligentes (*Smart Contracts*) baseados em Ethereum.  
Através da criação de um **contrato vulnerável** (vítima) e um **contrato atacante**, foi possível observar na prática como a falha pode ser explorada para retirar fundos indevidamente, e de seguida aplicar contramedidas para mitigação.

---

## Objetivos

- Analisar e compreender a vulnerabilidade de *Reentrância* em Smart Contracts.  
- Implementar um contrato atacante funcional.  
- Executar e monitorizar o ataque em ambiente controlado SEED Labs.  
- Aplicar técnicas de defesa (contramedidas).  
- Consolidar conhecimentos de segurança em Blockchain e Solidity.

---

##  Ferramentas e Tecnologias

- **Solidity 0.6.8**
- **Remix IDE / solc**
- **Python3 + Web3.py**
- **Ganache / Geth (blockchain local)**
- **Docker (SEED Labs Environment)**
- **Linux Terminal / nano / bash**

---

##  Estrutura do Projeto

Reentrancy_Lab/

├── contract/

│ ├── ReentrancyVictim.sol

│ ├── ReentrancyAttacker.sol

│ ├── ReentrancyVictim.abi / .bin

│ └── ReentrancyAttacker.abi / .bin

├── scripts/

│ ├── deploy_victim.py

│ ├── deploy_attacker.py

│ ├── launch_attack.py

│ ├── get_balance.py

│ └── cashout.py

└── report/

└── CAC - Relatorio Reentrancy Attack LAB.pdf

---

##  Instruções de Execução

1. **Compilar os contratos:**

   ./solc-0.6.8 --overwrite --abi --bin -o . ReentrancyVictim.sol
   ./solc-0.6.8 --overwrite --abi --bin -o . ReentrancyAttacker.sol
   
Fazer deploy do contrato vítima:
python3 deploy_victim.py

Fazer deploy do contrato atacante:
python3 deploy_attacker.py
Nota: passar o endereço da vítima como string e não como lista para evitar erro de codificação ABI.

Executar o ataque:
python3 launch_attack.py

Verificar os saldos e efetuar cashout:
python3 get_balance.py
python3 cashout.py


## Contramedidas e Boas Práticas
Padrão Checks-Effects-Interactions: atualizar o estado antes de enviar fundos.
Utilizar bloqueios (mutex) para evitar chamadas reentrantes.
Evitar uso direto de call.value(), preferindo transfer() ou send().
Utilizar contratos auditados (ex.: OpenZeppelin ReentrancyGuard).
Analisar contratos com ferramentas como Mythril, Slither, Oyente.

## Referências
SEED Labs – Reentrancy Attack
Phil Daian (2016) – Analysis of the DAO Exploit
Antonopoulos & Wood (2018) – Mastering Ethereum
GitHub (2022) – A Historical Collection of Reentrancy Attacks

## Autores
Nome	                                   Nº
Francisco Rafael Carocinho Ribeiro	2024123
José Samuel da Rocha Oliveira	2024172
Tiago Filipe Sousa Carvalho	2024180
Ana Rita da Silva Monteiro	2024041
Vanina Kollen	2024056
