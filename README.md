# Infra Redes - Cisco Project

## Sobre o Projeto
Esse projeto consiste em um layout e na implatação de rede local (LAN) e Wireless, projetado para interligar 3 salas distintas.

## Topologia da Rede
O escopo do projeto integra conectividade via cabo e rádio (Wi-Fi), dividida em duas faixas principais de endereçamento IP clássicas (Classe C).

* **Sub-rede 1 (192.168.0.0/24):** Atende à Sala 1 (Desktops via Switch) e à Sala Wireless (Laptops via Access Point).
* **Sub-rede 2 (192.168.1.0/24):** Atende à Sala da Direita (Setor de expansão conectado via Switch secundário).

---

## Desafios Técnicos & Soluções Aplicadas

Durante o desenvolvimento passei por algumas dificuldades, elas foram:

1. **Limitação Física de Portas no Roteador:** * *Desafio:* O roteador padrão (Cisco 1841) possuía apenas duas interfaces FastEthernet nativas, impossibilitando a conexão física da terceira sala de expansão.
   * *Solução:* Realizei o desligamento seguro do equipamento e instalei um módulo de expansão de hardware **WIC-1ENET**, adicionando uma interface Ethernet pura (Camada 3) para receber o novo endereçamento IP de gateway.

2. **Roteamento de Sub-redes Distinctas:**
   * *Desafio:* Garantir que os pacotes transitassem corretamente entre a rede `192.168.0.X` e `192.168.1.X`.
     
   * *Solução:* Configuração manual de Gateways e correção de IPs estáticos nos hosts periféricos para evitar dependência de Proxy ARP.

---

## Comandos Utilizados

Abaixo estão os blocos de comandos executados no Roteador (`quico`) para ativação das interfaces:

### Interface da Sala Esquerda & Wireless (Fa0/0)
```text
Router> enable
Router# configure terminal
Router(config)# hostname quico
quico(config)# interface fastEthernet 0/0
quico(config-if)# ip address 192.168.0.1 255.255.255.0
quico(config-if)# description Gateway Principal
quico(config-if)# no shutdown
