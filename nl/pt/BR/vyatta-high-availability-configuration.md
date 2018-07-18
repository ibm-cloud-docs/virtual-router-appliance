---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Configuração de alta disponibilidade do Vyatta

A alta disponibilidade do Vyatta é suportada por meio do uso do VRRP, Virtual Routing Redundancy Protocol. Cada grupo de gateway terá dois endereços IP primários do VRRP, um para o lado privado e um para o lado público das redes. 

**NOTA:** os Vyattas somente privados terão apenas o VRRP privado. 

Esses endereços IP são os IPs de destino para que a infraestrutura de rede do Softlayer possa rotear todas as sub-redes em VLANs associadas aos membros do gateway. Apenas um Vyatta terá esses IPs do VRRP em execução de cada vez, os outros membros do grupo de gateway os terão administrativamente.

A configuração pode ser sincronizada entre os dois Vyattas com os comandos de configuração "config-sync". Essa configuração permitirá que um membro envie por push a configuração de opções específicas para o outro Vyatta em um grupo de forma seletiva. É possível enviar por push apenas as regras de firewall, apenas a configuração do IPsec ou qualquer combinação de conjuntos de regras. 

Recomenda-se não tentar enviar por push endereços IP ou outras configurações de rede, pois a sincronização de configuração confirmará instantaneamente suas mudanças nos outros Vyattas, colocando essas interfaces on-line. Se desejar ativar dinamicamente interfaces e serviços, será necessário usar scripts de transição para executar essa ação em failover. Além disso, recomenda-se usar mais endereços IP do VRRP para seus IPs de gateway em VLANs associadas; isso facilitará o gerenciamento do failover.

Sua configuração básica do VRRP em ambas as máquinas será semelhante a esta configuração de amostra:

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

O VRRP requer que um endereço IP real seja ligado a uma interface virtual primeiro antes que quaisquer avisos do VRRP possam ser enviados. Em muitos casos, basta incluir um IP da sub-rede primária, mas isso pode entrar em conflito com futuras provisões ou talvez você queira rotear uma VLAN que já tem cada IP de sub-rede primário alocado para um servidor. Para contornar isso, é possível usar um par de endereços IP fora da banda em ambos os Vyattas que possam ser usados para conversar entre si. Aqui está um exemplo:

No primeiro vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

No segundo vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

Nesse caso, os vyattas terão seus próprios IPs que não entrarão em conflito com a sub-rede alocada. Será possível escolher quase qualquer intervalo privado que você quiser, bastando escolher uma pequena sub-rede que não entre em conflito com nenhuma outra rota que você possa ter, como um intervalo de sub-rede em um túnel IPsec ou um endereço 10.0.0.0/8 que entre em conflito com o do Softlayer e você ficará bem.

É provável que você também queira incluir um nome de "grupo de sincronização". Todos os endereços do VRRP devem ser uma parte do mesmo grupo de sincronização. Isso ocorre de forma que qualquer falha em uma interface também causará failover em todas as interfaces no mesmo grupo de sincronização. Caso contrário, você poderá acabar com alguns sendo MASTER e outros em BACKUP. Use o mesmo nome na configuração de VLAN nativa de bond0 e bond1.

NOTA: a configuração do VRRP de bond0 e bond1 pode ter uma linha para compatibilidade de rfc3768. Isso não será necessário para VRRP em um vif, apenas a VLAN nativa de bond0 e bond1.

Em um par de gateways recém-provisionado, a sincronização de configuração só terá uma configuração mínima:


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

Você precisará incluir regras para determinar quais ramificações de configuração migrar. Por exemplo, se desejar que a configuração de firewall e ipsec seja enviada por push, inclua estes comandos:


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

Depois que forem confirmados, todas as mudanças na configuração de firewall ou ipsec serão enviadas por push para o outro vyatta na confirmação.

**NOTA:** grupo de sincronização e mapa de sincronização são duas coisas diferentes. A configuração do mapa de sincronização é para que as regras enviem por push mudanças na configuração para outro Vyatta. O outro, grupo de sincronização, é para que os IPs do VRRP sofram failover como um grupo, em vez de um de cada vez. A configuração de um não afeta o outro.
