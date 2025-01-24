# Projeto de Virtualização de Redes


## Topologia
     h1   h2  h3
       \  | /
        s1
         |
        r1
       /  \
     r6    r2
     |      |
     r5     r3
       \   /
         r4
         |
         h4

### Configuração da Rede

| Device   | Interface/Port        | MAC Address          | IP Address       | Labels |
|----------|-----------------------|----------------------|------------------|--------|
| h1       | h1-eth0              | aa:00:00:00:00:01   | 10.0.1.1/24        |        |
| h2       | h2-eth0              | aa:00:00:00:00:02   | 10.0.1.2/24        |        |
| h3       | h3-eth0              | aa:00:00:00:00:03   | 10.0.1.3/24        |        |
| h4       | h4-eth0              | aa:00:00:00:00:04   | 10.0.2.1/24        |        |
| s1       | s1-eth1 (to h1)      | cc:00:00:00:01:01   | N/A                |        |
| s1       | s1-eth2 (to h2)      | cc:00:00:00:01:02   | N/A                |        |
| s1       | s1-eth2 (to h3)      | cc:00:00:00:01:03   | N/A                |        |
| s1       | s1-eth3 (to r1)      | cc:00:00:00:01:04   | N/A                |        |
| r1       | r1-eth1 (to s1)      | aa:00:00:00:01:01   | 10.0.1.254/24      |        |
| r1       | r1-eth2 (to r2)      | aa:00:00:00:01:02   | N/A                |        |
| r1       | r1-eth3 (to r6)      | aa:00:00:00:01:03   | N/A                |        |
| r2       | r2-eth1 (to r1)      | aa:00:00:00:02:01   | N/A                |        |
| r2       | r2-eth2 (to r3)      | aa:00:00:00:02:02   | N/A                |        |
| r3       | r3-eth1 (to r2)      | aa:00:00:00:03:01   | N/A                |        |
| r3       | r3-eth2 (to r4)      | aa:00:00:00:03:02   | N/A                |        |
| r4       | r4-eth1 (to h4)      | aa:00:00:00:04:01   | 10.0.2.254/24      |        |
| r4       | r4-eth2 (to r5)      | aa:00:00:00:04:02   | N/A                |        |
| r4       | r4-eth3 (to r3)      | aa:00:00:00:04:03   | N/A                |        |
| r6       | r6-eth1 (to r1)      | aa:00:00:00:06:01   | N/A                |        |
| r6       | r6-eth2 (to r5)      | aa:00:00:00:06:02   | N/A                |        |
| r5       | r5-eth1 (to r6)      | aa:00:00:00:05:01   | N/A                |        |
| r5       | r5-eth2 (to r4)      | aa:00:00:00:05:02   | N/A                |        |

### Compilar P4 
```bash
p4c-bm2-ss --std p4-16  p4/l2switch.p4 -o json/l2switch.json
p4c-bm2-ss --std p4-16  p4/l3switchr1.p4 -o json/l3switchr1.json
p4c-bm2-ss --std p4-16  p4/l3switchrmeio.p4 -o json/l3switchrmeio.json
p4c-bm2-ss --std p4-16  p4/l3switchr4.p4 -o json/l3switchr4.json
```

### Executar
```bash
sudo python3 mininet/topo.py --jsonr1 json/l3switchr1.json --jsonS json/l2switch.json --jsonr4 json/l3switchr4.json --jsonrmeio json/l3switchrmeio.json
```

### Testar
No wireshark é possível visualizar as labels a levarem pop.
```bash
mininet> h3 ping h4 -c 1
mininet> pingall
```

Para visualizar as entradas nas tabelas , as ações e outras componentes a serem executadas.
Por exemplo para o router 1  (Port 9090)
```bash
     sudo ./tools/nanomsg_client.py --thrift-port 9090
```
