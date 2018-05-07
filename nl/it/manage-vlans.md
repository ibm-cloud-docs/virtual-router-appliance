---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gestisci le VLAN
Puoi eseguire diverse azioni dalla [Schermata Gateway Appliance Details](access-gateway-details.html).

## Associa una VLAN a un'applicazione gateway

Una VLAN deve essere associata a un'applicazione gateway prima che possa essere instradata. L'associazione della VLAN è il collegamento di una VLAN idonea a un gateway di rete in modo che possa essere instradata a un'applicazione gateway in futuro. Il processo di associazione non instrada automaticamente una VLAN a un'applicazione gateway; la VLAN continua ad utilizzare i router del cliente di backend e frontend finché non viene instradata al gateway. 

Le VLAN possono essere associate soltanto a un gateway alla volta e non devono avere un firewall. Esegui la seguente procedura per associare una VLAN a un gateway di rete.

1. [Accedi alla schermata Gateway Appliance Details](access-gateway-details.html) nel portale del cliente. 
2. Seleziona la VLAN desiderata dall'elenco a discesa **Associate a VLAN**.
3. Fai clic sul pulsante **Associate** per associare la VLAN.

Dopo aver associato una VLAN all'applicazione gateway, viene visualizzata la sezione delle VLAN associate della schermata dei dettagli dell'applicazione gateway. Da questa sezione, la VLAN può essere instradata al gateway o ne può essere annullata l'associazione. Ulteriori VLAN idonee possono essere associate a un'applicazione gateway in qualsiasi momento ripetendo i precedenti passi.

## Instrada una VLAN associata

Le VLAN associate sono collegate a un'applicazione gateway, ma il traffico in entrata ed uscita della VLAN non riguarda il gateway finché non è stata instradata la VLAN. Dopo l'instradamento di una VLAN associata, tutto il traffico di frontend e backend viene instradato tramite l'applicazione gateway invece che mediante i router del cliente. 

Esegui la seguente procedura per instradare una VLAN associata: 

1. [Accedi alla schermata Gateway Appliance Details](access-gateway-details.html) nel portale del cliente. 
2. Individua la VLAN desiderata nella sezione delle VLAN associate.
3. Seleziona **Route VLAN** dal menu a discesa delle azioni.
4. Fai clic su **Yes** per instradare la VLAN. 

Dopo l'instradamento di una VLAN, tutto il traffico di frontend e backend viene spostato dai router del cliente al gateway di rete. Ulteriori controlli correlati al traffico e all'applicazione gateway stessa, possono essere eseguiti accedendo allo strumento di gestione del gateway. L'instradamento tramite il gateway di rete può essere sospeso in qualsiasi momento [escludendo l'applicazione gateway](#bypass-gateway-appliance-routing-for-a-vlan).

## Escludi l'instradamento dell'applicazione gateway per una VLAN

Dopo l'instradamento di una VLAN, tutto il traffico di frontend e backend passa tramite il gateway di rete. In qualsiasi momento, l'applicazione gateway può essere esclusa in modo che il traffico ritorni ai router del cliente di frontend e backend (FCR e BCR). 

L'esclusione di una VLAN le consente di rimanere associata al gateway di rete. Se la VLAN non è più associata all'applicazione gateway, fai riferimento a [Annullare l'associazione di una VLAN a un'applicazione gateway](#disassociate-a-vlan-from-a-gateway-appliance). 

Esegui la seguente procedura per escludere l'instradamento gateway per una VLAN:

1. [Accedi alla schermata Gateway Appliance Details](access-gateway-details.html) nel portale del cliente. 
2. Individua la VLAN desiderata nella sezione delle VLAN associate.
3. Seleziona **Bypass VLAN** dal menu a discesa delle azioni.
4. Fai clic su **Yes** per escludere il gateway. 

Dopo aver escluso il gateway di rete, tutto il traffico di frontend e backend viene instradato tramite i FCR e BCR associati alla VLAN. La VLAN rimarrà associata all'applicazione gateway e può essere instradata nuovamente ad essa in qualsiasi momento.

## Annullare l'associazione di una VLAN a un'applicazione gateway

Le VLAN possono essere collegate a un'applicazione gateway alla volta tramite [associazione](#associate-a-vlan-to-a-gateway-appliance). L'associazione consente alla VLAN di essere instradata all'applicazione gateway in qualsiasi momento. Se una VLAN deve essere associata a un'altra applicazione gateway o se non è più associata al proprio gateway, è necessario un annullamento dell'associazione. L'annullamento dell'associazione rimuove il "link" dalla VLAN all'applicazione gateway, permettendole di essere associata a un altro gateway, se necessario. 

Esegui la seguente procedura per annullare l'associazione di una VLAN a un'applicazione gateway:

1. [Accedi alla schermata Gateway Appliance Details](access-gateway-details.html) nel portale del cliente. 
2. Individua la VLAN desiderata nella sezione delle VLAN associate.
3. Seleziona **Disassociate** dal menu a discesa **Actions**. 
4. Fai clic su **Yes** per annullare l'associazione alla VLAN. 

Dopo aver annullato l'associazione alla VLAN da un'applicazione gateway, la VLAN deve essere associata a un altro gateway. La VLAN può anche essere associata nuovamente all'applicazione gateway in qualsiasi momento. Dopo aver annullato l'associazione alla VLAN da un'applicazione gateway, il traffico della VLAN non può essere instradato al gateway. Le VLAN devono essere associate a un'applicazione gateway prima di poter essere instradate.

## Instrada più VLAN nella stessa interfaccia di rete 
VRA (Virtual Router Appliance) è in grado di instradare più VLAN nella stessa interfaccia di rete (ad esempio, `dp0bond0` o `dp0bond1`). Questo è possibile impostando la porta switch nella modalità trunk e configurando le interfacce virtuali (VIF) sul dispositivo.

Ad esempio: 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

I precedenti comandi creano due interfacce virtuali nell'interfaccia `dp0bond0`. L'interfaccia `dp0bond0.1432` elabora il traffico per la VLAN 1432 mentre `dp0bond0.1693` per la VLAN 1693.
