# PcapPoisoning Writeup

Another super easy chall

> #### Description
> 
> How about some hide and seek heh?Download this [file](https://artifacts.picoctf.net/c/377/trace.pcap) and find the flag.



So we got this trace.pcap file..



I simple ran:

```bash
strings trace.pcap | pico
```

and get the flag:

`picoCTF{P64P_4N4L7S1S_SU55355FUL_31010c46}`


