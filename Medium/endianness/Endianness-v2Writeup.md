# Endianness-v2 Writeup

This is an interesting challenge that teaches us about endianness. I had to did a little research as I already forgot alot about endianness hehe.



> #### Description
> 
> Here's a file that was recovered from a 32-bits system that organized the bytes a weird way. We're not even sure what type of file it is.Download it [here](https://artifacts.picoctf.net/c_titan/113/challengefile) and see what you can get out of it



First of all, I tried to run `file` command on the file and it displays `challengefile: data` . This means that this file is not compiled into a proper file so we need to inspect more.



I ran `xxd` on the file to read the file's header for clue..

![Screenshot 2025-11-24 152308.png](Screenshot%202025-11-24%20152308.png)

Interestingly, that looks like... `JFIF`! Supposedly a header for image files.

Looking back at the question, the chall name is "Endianness" so I look up about endian ordering..

<img title="" src="endiannote.png" alt="endiannote.png" width="405">

Source: [Don't Let Endianness Flip You Around | Hackaday](https://hackaday.com/2020/08/04/dont-let-endianness-flip-you-around/)

After doing research, I am positive that our challenge file now is in Little-endian ordering since it shows `FJ FI` instead of  `JFIF`! So we simply just need to swap every 4 bits into Big-endian ordering!



I prompted ChatGPT to craft me a quick script:

```python
#I named this script as swap.py
#!/usr/bin/env python3
import sys
import binascii

def swap_endian(hex_str, chunk_size):
    """
    Convert hex string from little endian to big endian by
    reversing bytes within each chunk.
    """
    if len(hex_str) % 2 != 0:
        raise ValueError("Hex string length must be even")

    bytes_list = [hex_str[i:i+2] for i in range(0, len(hex_str), 2)]

    # Group bytes by chunk_size (in bytes)
    chunks = [bytes_list[i:i+chunk_size] for i in range(0, len(bytes_list), chunk_size)]

    # Reverse each chunk (little ↔ big endian)
    swapped = ["".join(reversed(chunk)) for chunk in chunks]
    return "".join(swapped)


def file_to_hex(filename):
    with open(filename, "rb") as f:
        return binascii.hexlify(f.read()).decode()


def main():
    if len(sys.argv) < 3:
        print("Usage: python3 hex_little_to_big.py <input_file> <chunk_size>")
        print("Example: python3 hex_little_to_big.py data.bin 4")
        sys.exit(1)

    input_file = sys.argv[1]
    chunk_size = int(sys.argv[2])

    hex_data = file_to_hex(input_file)
    converted = swap_endian(hex_data, chunk_size)

    out_file = input_file + ".png"
    with open(out_file, "wb") as f:
        f.write(binascii.unhexlify(converted))

    print(f"[+] Converted file written to: {out_file}"
)


if __name__ == "__main__":
    main()
```

This script made by ChatGPT is able to swap based on chunk size too, Thankyou ChatGPT! 



so I ran the command

```bash
python3 swap.py challengefile 4 #I use 4 because i want to swap 4 chunks
```

and finally we got this image with flag:

![challengefile.png](challengefile.png)



Flag: `picoCTF{cert!f1Ed_iNd!4n_s0rrY_3nDian_6d3ad08e}`



