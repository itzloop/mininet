# Internet Engineering Assignment #5 - MiniNet and Multipath TCP

## Prerequisite
- Read instructions in [here](https://github.com/qdeconinck/sigcomm20_mptp_tutorial) and follow the steps

## Creating different toplogies with MiniNet

### A

<p align="center">
  <img alt="figure 1" src="./assets/fig01.png" />
</p>

This is the `minimal` topology and we can create this in one of two ways:
-	`minimal` topology

```bash
$ sudo mn --topo minimal
```

`net` command's output:

```bash
mininet> net
h1 h1-eth0:s1-eth1
h2 h2-eth0:s1-eth2
s1 lo:  s1-eth1:h1-eth0 s1-eth2:h2-eth0
c0
```

- `single` toplogy using 2 hosts

```bash
$ sudo mn --topo signle,2
```

`net` command's output

```bash
mininet> net
h1 h1-eth0:s1-eth1
h2 h2-eth0:s1-eth2
s1 lo:  s1-eth1:h1-eth0 s1-eth2:h2-eth0
c0
```

### B

<p align="center">
  <img alt="figure 2" src="./assets/fig02.png" />
</p>

This topology can be created with `linear`, so:

```bash
$ sudo mn --topo linear,2,2
```

The above command will create 2 siwtches with 2 hosts each.

`net` command's output:

```bash
mininet> net
h1s1 h1s1-eth0:s1-eth1
h1s2 h1s2-eth0:s2-eth1
h2s1 h2s1-eth0:s1-eth2
h2s2 h2s2-eth0:s2-eth2
s1 lo:  s1-eth1:h1s1-eth0 s1-eth2:h2s1-eth0 s1-eth3:s2-eth3
s2 lo:  s2-eth1:h1s2-eth0 s2-eth2:h2s2-eth0 s2-eth3:s1-eth3
c0
```

### C

<p align="center">
  <img alt="figure 3" src="./assets/fig03.png" />
</p>

This is the `tree` topology and can be created as follows:

```bash
$ sudo mn --topo tree,2,3
# or you specify what each number means like below
$ sudo mn --topo,depth=2,fanout=3
```

`net` command's output:

```bash
mininet> net
h1 h1-eth0:s2-eth1
h2 h2-eth0:s2-eth2
h3 h3-eth0:s2-eth3
h4 h4-eth0:s3-eth1
h5 h5-eth0:s3-eth2
h6 h6-eth0:s3-eth3
h7 h7-eth0:s4-eth1
h8 h8-eth0:s4-eth2
h9 h9-eth0:s4-eth3
s1 lo:  s1-eth1:s2-eth4 s1-eth2:s3-eth4 s1-eth3:s4-eth4
s2 lo:  s2-eth1:h1-eth0 s2-eth2:h2-eth0 s2-eth3:h3-eth0 s2-eth4:s1-eth1
s3 lo:  s3-eth1:h4-eth0 s3-eth2:h5-eth0 s3-eth3:h6-eth0 s3-eth4:s1-eth2
s4 lo:  s4-eth1:h7-eth0 s4-eth2:h8-eth0 s4-eth3:h9-eth0 s4-eth4:s1-eth3
c0
```

## Working with Latency and Bandwidth

First fixed bandwidth of `100Mb/s` and variable delays:

- delay of `1ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=1ms
``mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['90.2 Mbits/sec', '93.9 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 4.559/9.866/22.392/7.305 ms
```

- delay of `5ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=5ms
``mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['90.3 Mbits/sec', '93.7 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 20.691/25.943/40.249/8.268 ms
```

- delay of `10ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=10ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['89.4 Mbits/sec', '92.5 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 40.946/63.804/129.676/38.040 ms
```
- delay of `25ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=25ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['81.2 Mbits/sec', '84.2 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 100.905/106.736/123.432/9.645 ms
```

- delay of `50ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=50ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['55.4 Mbits/sec', '56.7 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.

--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 202.970/211.728/235.528/13.753 ms
```
- delay of `100ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['16.4 Mbits/sec', '16.7 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms
rtt min/avg/max/mdev = 401.671/414.333/448.804/19.939 ms
```

- delay of `200ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=200ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['3.15 Mbits/sec', '3.18 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 801.730/813.077/845.843/18.941 ms
```

- delay of `350ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=350ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['463 Kbits/sec', '463 Kbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3023ms
rtt min/avg/max/mdev = 1401.917/1415.633/1455.250/22.926 ms, pipe 2
```

- delay of `500ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=500ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['160 Kbits/sec', '190 Kbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3025ms
rtt min/avg/max/mdev = 2000.979/2009.010/2030.876/12.647 ms, pipe 3
```

- delay of `750ms`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=750ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['19.0 Kbits/sec', '43.4 Kbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3051ms
rtt min/avg/max/mdev = 3001.181/3009.091/3030.704/12.551 ms, pipe 3
```


<p align="center">
  <img alt="Avg. RTT(ms) vs. Delay(ms)" src="./assets/Avg. RTT(ms) vs. Delay(ms).png" />
</p>

<p align="center">
  <img alt="Bandwidth(Mbit_s) between h1 and h2" src="./assets/Bandwidth(Mbit_s) between h1 and h2.png" />
</p>

As you can see with higher delays we will have lower bandwidth and higher RTT. Let's use delay of `1ms` as a reference point. Now if we had a `50x (1ms * 50 = 50ms)` delay, our `RTT` would be `50x` as well.

Now fixed delay of `100ms` and variable bandwidth
- bandwidth of `1Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=1,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['888 Kbits/sec', '1.25 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 402.160/412.736/443.566/17.819 ms
```

- bandwidth of `5Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=5,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['3.95 Mbits/sec', '5.29 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 401.735/411.109/438.408/15.768 ms
```

- bandwidth of `10Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=10,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['7.20 Mbits/sec', '9.36 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 402.050/410.821/433.278/13.026 ms
```

- bandwidth of `25Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=25,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['14.5 Mbits/sec', '16.5 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 400.787/410.053/435.986/14.980 ms
```

- bandwidth of `50Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=50,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['16.9 Mbits/sec', '17.6 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 401.677/411.533/438.658/15.676 ms
```
- bandwidth of `100Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=100,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['15.9 Mbits/sec', '16.1 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms
rtt min/avg/max/mdev = 401.824/409.590/431.902/12.885 ms
```

- bandwidth of `200Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=200,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['15.9 Mbits/sec', '16.1 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms
rtt min/avg/max/mdev = 401.716/409.217/430.548/12.336 ms
```

- bandwidth of `350Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=350,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['16.5 Mbits/sec', '16.8 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 402.492/411.008/431.400/11.828 ms
```

- bandwidth of `500Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=500,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['16.1 Mbits/sec', '16.2 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 402.147/406.026/417.567/6.693 ms
```

- bandwidth of `750Mbit/s`

```bash
$ sudo mn --topo minimal --link tc,bw=750,delay=100ms
mininet> iperf
*** Iperf: testing TCP bandwidth between h1 and h2 
*** Results: ['16.1 Mbits/sec', '16.2 Mbits/sec']
mininet> h1 ping -qc 4 h2
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
--- 10.0.0.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 401.272/410.083/434.787/14.289 ms
```


<p align="center">
  <img alt="Avg. RTT(ms) vs. Delay(ms)" src="./assets/Avg. RTT(ms) vs. Bandwidth(Mbit_s).png" />
</p>

Changing the bandwidth doesn't affect `RTT` when we have introduced fixed delay in our netwrok.

<p align="center">
  <img alt="Bandwidth(Mbit_s) between h1 and h2 vs. Bandwidth(Mbit_s)" src="./assets/Bandwidth(Mbit_s) between h1 and h2 vs. Bandwidth(Mbit_s).png" />
</p>

After a certain point increasing bandwidth would not change the bandwidth between `h1` and `h2` because we have a delay of `100ms`.
