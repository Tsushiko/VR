# VR - Assignment

## Task Overview 
- Os tuneis são decididos através do ip dos hosts, alterar nos flows a escolha.
  - 0 representa túnel 1 (R1-R2-R3-R4)  ou  (R4-R2-R3-R1)
  - 1 representa túnel 2 (R1-R6-R5-R4)  ou  (R4-R5-R6-R1)
- Encaminhamento através de labels (MSLP).
  
- Estrutura do pacote:
  |dstAdr|srcAdr|Type|NLabels|Label1|....|IPV4|
  |------|------|----|-------|------|----|----|
  - dstAdr, srcAdr e Type fazem parte da header do Ethernet
  - Type:
    - 0x8000 - IPV4
    - 0x88B5 - MSLP
  - Nlabels, é o número de labels no pacote.
  - A nível do código, as labels são representadas por uma stack de labels.

- Os tunéis são bidirecionais, e todos os hosts podem pingar os outros.
  - A distribuição de host por túnel está 50/50.
  - h1 - h4 ---> túnel 1
  - h4 - h1 ---> túnel 2
  - h2 - h4 ---> túnel 2
  - h4 - h2 ---> túnel 1
  - h3 - h4 ---> túnel 2
  - h4 - h3 ---> túnel 1

- Não usamos hash para a decisão do túnel.
- Não implementamos a firewall.
