---

copyright:
  anos: 2017
última atualização: "18-10-2017"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Sincronizar Configuração de HA
Dois Virtual Router Appliances (VRA) em um par de HA devem ter suas configurações sincronizadas suficientemente para que ambos os dispositivos se comportem de maneira semelhante. Isso é feito por meio de `mapas de sincronização de configuração` e é possível escolher qual parte da configuração será sincronizada. Se você fizer uma mudança em uma máquina, enviará por push a configuração marcada para o outro dispositivo.

**NOTA:** isso não significa que a configuração no outro dispositivo foi salva. Somente que foram feitas mudanças na configuração de execução.

A configuração que é exclusiva para um sistema não deve ser sincronizada com o outro. Os endereços IP e MACs reais não devem ser sincronizados, por exemplo. O nó de configuração `system config-sync` em si e o nó `service https` não podem ser sincronizados de modo algum.

O exemplo a seguir ilustra config-sync:

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

As duas primeiras linhas criam o mapa de sincronização real em si. Aqui, a sub-rotina de configuração para `firewall de segurança` será configurada no `sync-map`. Como resultado, quaisquer mudanças feitas dentro do nó de configuração serão enviadas por push para o dispositivo remoto. No entanto, as mudanças feitas no `usuário de segurança` não seriam enviadas, pois não correspondem à regra. É possível tornar o `sync-map` tão específico ou tão geral quanto desejar.

As próximas três linhas designam o usuário e a senha de `config-sync` do roteador remoto, o IP e qual mapa de sincronização enviar por push. Quaisquer mudanças que correspondam às regras para `TEST`, irão para o `remote-router 192.168.1.22`, usando essas informações de login. Observe que uma chamada `REST` é feita para executar isto usando a API do VRA, portanto, o servidor HTTPS deve estar em execução (e ser desbloqueado) no roteador remoto.

O config-sync acontece sempre que você confirma uma mudança, portanto, observe as mensagens de erro que vêm do dispositivo remoto. Se a configuração estiver fora de sincronização, será necessário corrigir isto no dispositivo remoto para torná-lo operacional novamente.

Também é possível ver diferenças de configuração usando o comando `show config-sync difference`.
