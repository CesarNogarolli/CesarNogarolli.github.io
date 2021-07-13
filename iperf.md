# Iperf

Iperf é uma ferramenta simples para realização de testes de throughput que permitem medir a largura de banda, Jitter e o total de pacotes perdidos.



## Utilizando o Iperf

De maneira geral você deve ter as mesmas versões do programa Iperf instalado nas máquina cliente e servidor. A versão que está rodando na PCP é a 3.1.3 conforme abaixo:

```powershell
PS C:\iperf> iperf3.exe -v
iperf 3.1.3
CYGWIN_NT-10.0 EIML561429 2.5.1(0.297/5/3) 2016-04-21 22:14 x86_64
Optional features available: None
```

> Nas imagens da PCP o Iperf está na pasta **C:\iperf**.



Para executar o Iperf basta acessar a pasta em que ele está e executar via **cmd** conforme descrito na sequencia. Na PCP, o servidor 10.22.7.10 responde às solicitações de teste do Iperf. Esse servidor está lotado no datacenter da Celepar.




### Executar um teste em conexão TCP

  Você precisa apontar para um servidor que esteja executando o Iperf.

#### Executando o teste

  > **> iperf3.exe -c host**
  >
  > **-c**		Executa o teste como cliente
  >
  > **host**	Endereço do Servidor que está rodando o Iperf

  Ao executar você deve ter um retorno similar ao seguinte:

  ```powershell
  PS C:\iperf> iperf3.exe -c 10.22.7.10
  Connecting to host 10.22.7.10, port 5201
  [  4] local 10.69.234.218 port 10145 connected to 10.22.7.10 port 5201
  [ ID] Interval           Transfer     Bandwidth
  [  4]   0.00-1.00   sec  4.25 MBytes  35.6 Mbits/sec
  [  4]   1.00-2.00   sec  10.9 MBytes  91.4 Mbits/sec
  [  4]   2.00-3.01   sec  10.5 MBytes  87.4 Mbits/sec
  [  4]   3.01-4.00   sec  9.50 MBytes  80.1 Mbits/sec
  [  4]   4.00-5.00   sec  10.6 MBytes  89.0 Mbits/sec
  [  4]   5.00-6.00   sec  11.1 MBytes  93.4 Mbits/sec
  [  4]   6.00-7.00   sec  10.8 MBytes  90.3 Mbits/sec
  [  4]   7.00-8.01   sec  10.5 MBytes  87.5 Mbits/sec
  [  4]   8.01-9.01   sec  11.0 MBytes  92.0 Mbits/sec
  [  4]   9.01-10.00  sec  10.5 MBytes  88.9 Mbits/sec
  - - - - - - - - - - - - - - - - - - - - - - - - -
  [ ID] Interval           Transfer     Bandwidth
  [  4]   0.00-10.00  sec  99.6 MBytes  83.6 Mbits/sec                  sender
  [  4]   0.00-10.00  sec  99.5 MBytes  83.5 Mbits/sec                  receiver
  
  iperf Done.
  ```

  

### Executar um teste com pacotes - VoIP*

 Ao efetuar o teste em UDP é possível identificar melhor a qualidade da rede pois o UDP não reenvia os datagramas quando eles se perdem ou são corrompidos em alguma das etapas do envio. Assim, esse teste pode, além de ajudar a identificar problemas na rede, auxiliar o diagnóstico da qualidade do VoIP. Útil quando reclamações sobre a qualidade das ligações começam a aparecer.

#### Executando o teste

> **iperf3.exe -c host -u**
>
> **-c**		Executa o teste como cliente
>
> **host**	Endereço do Servidor que está rodando o Iperf
>
> **-u**		Executa o teste em UDP

```powershell
PS C:\iperf> iperf3.exe -c 10.22.7.10 -u
Connecting to host 10.22.7.10, port 5201
[  4] local 10.69.234.218 port 61594 connected to 10.22.7.10 port 5201
[ ID] Interval           Transfer     Bandwidth       Total Datagrams
[  4]   0.00-1.01   sec   128 KBytes  1.04 Mbits/sec  16
[  4]   1.01-2.01   sec   128 KBytes  1.04 Mbits/sec  16
[  4]   2.01-3.01   sec   128 KBytes  1.06 Mbits/sec  16
[  4]   3.01-4.01   sec   128 KBytes  1.05 Mbits/sec  16
[  4]   4.01-5.00   sec   128 KBytes  1.06 Mbits/sec  16
[  4]   5.00-6.01   sec   128 KBytes  1.04 Mbits/sec  16
[  4]   6.01-7.00   sec   128 KBytes  1.05 Mbits/sec  16
[  4]   7.00-8.01   sec   128 KBytes  1.04 Mbits/sec  16
[ ID] Interval           Transfer     Bandwidth       Jitter    Lost/Total Datagrams
[  4]   0.00-10.01  sec  1.26 MBytes  1.05 Mbits/sec  0.287 ms  1/160 (0.62%)
[  4] Sent 160 datagrams

iperf Done.
```

Basicamente, quanto menor a perda de melhor mas, em certos cenários de rede é inevitável que ocorram. O problema aparecem quando essas perdas são altas e começam a interferir no trafego de dados.

Redes com alto tráfego de dados ou vários dispositivos conectados num mesmo domínio de broadcast podem gerar uma perda nos datagramas UDP ou aumentar o **Jitter**. Nesse cenário é recomendado o uso de **QoS** para garantir a qualidade das comunicações VoIP.

> **Jitter:**
>
> É a variação de tempo entre os pacotes num fluxo. O jitter alto impacta diretamente o uso do VOIP
>
> https://tools.ietf.org/html/rfc4689#section-3.2.5
>
> 
>
> **QoS:**
>
> Uma forma de fornecer acesso priorizado para diferentes classes de tráfego como vídeo, voz e outros.
>
> https://tools.ietf.org/html/rfc7561#section-2
