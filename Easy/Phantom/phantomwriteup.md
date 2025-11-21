# Ph4nt0m 1ntrud3r

> Description
> 
> A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag.To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder!Find the PCAP file here [Network Traffic PCAP file](https://challenge-files.picoctf.net/c_verbal_sleep/3fe089c41615b9413666bedca922e07bf6ad8894a3dabd2737735143ad2396cf/myNetworkTraffic.pcap) and try to get the flag.



> Hints
> 
> - Filter your packets to narrow down your search.
> 
> - Attacks were done in timely manner.
> 
> - Time is essential



This challenge is.. not interesting because it requires patience I dont have much of those. Since its a pcap, we use wireshark to inspect the file.



![Screenshot 2025-11-21 164200.png](Screenshot%202025-11-21%20164200.png)

I discovered that there are multiple packets that ends with base64 so i think thats the flag but encoded with base64.. so I tried to extract all of them using this script:


```python
import subprocess
import re
import sys

if len(sys.argv) < 2:
    print("Usage: python3 thisscript.py file.pcap")
    sys.exit(1)

pcap = sys.argv[1]
cmd = ["tshark", "-r", pcap, "-x"]
out = subprocess.run(cmd, stdout=subprocess.PIPE, text=True).stdout


hex_bytes = re.findall(r"\b[0-9a-fA-F]{2}\b", out)
raw = bytes.fromhex("".join(hex_bytes))
strings = re.findall(rb"[ -~]{4,}", raw)


matches = sorted(set(
    s.decode(errors="ignore") for s in strings if b"=" in s
))

if matches:
    for m in matches:
        print(m)
else:
    print("(no matches)")

```

> The output
> 
> 2d71KZI=E
> 4pcYwTg=E
> 6w46Q70=E
> 7uDCclg=E
> 9DpIbkA=E
> JbG2Q7w=E
> NjZkMGJmYg==E
> OwFeP0M=E
> QKzFX+c=
> RHxhtS4=E
> RMq+wTM=E
> Tclg/3s=RE
> Tclg/3s=dA==
> XzM0c3lfdA==
> YmhfNHJfOQ==E
> Z1Gdyjk=E
> bnRfdGg0dA==
> cGljb0NURg==E
> ezF0X3c0cw==E
> fQ==E
> hKvZKGA=E
> oFpZPG8=E
> qo9qpiY=E

Then I manually decode these base64 and rearrange the parts to get the flag!



> Flag: picoCTF{1t_w4snt_th4t_34sy_tbh_4r_966d0bfb}



