# EXP.NO.10-Simulation-of-Error-Detection-and-Correction-Algorithms
Simulation of Error Detecion and Correction Algorithms

# AIM
Simulation of Error Detecion and Correction Algorithms
# SOFTWARE REQUIRED
Google Collab
# ALGORITHMS
```
1. Parity Check (Even Parity)
 Count 1s in data.

Add 0 if count is even, else add 1 as parity bit.

Transmit data + parity.

Receiver counts 1s including parity.

If total 1s is even → No error; else → Error detected.

2. Checksum
Divide data into equal blocks.

Add all blocks using binary addition.

Compute 1's complement of the sum as checksum.

Transmit data + checksum.

Receiver adds blocks + checksum.

If 1's complement of sum is 0 → No error; else → Error.

3. Hamming Code (7,4)
Input 4 data bits; calculate 3 parity bits.

Form 7-bit code: p1 p2 d1 p3 d2 d3 d4.

Transmit code.

Receiver calculates syndrome bits (s1, s2, s3).

Error position = binary of s3s2s1.

Flip bit if error; extract 4 data bits.
```
# PROGRAM
```
# Parity Check (Even)
def parity_check(data):
    parity = data.count('1') % 2
    return data + str(parity)

def check_parity(received):
    return received.count('1') % 2 == 0


# Checksum (16-bit blocks)
def compute_checksum(blocks):
    checksum = 0
    for block in blocks:
        checksum += int(block, 2)
        checksum = (checksum & 0xFFFF) + (checksum >> 16)  # carry around
    return bin(~checksum & 0xFFFF)[2:].zfill(16)

def verify_checksum(blocks, checksum):
    total = int(checksum, 2)
    for block in blocks:
        total += int(block, 2)
        total = (total & 0xFFFF) + (total >> 16)
    return (bin(~total & 0xFFFF)[2:].zfill(16) == '0000000000000000')


# Hamming Code (7,4)
def hamming_encode(data):  # data = 4 bits: d1 d2 d3 d4
    d = list(map(int, data))
    p1 = d[0] ^ d[1] ^ d[3]
    p2 = d[0] ^ d[2] ^ d[3]
    p3 = d[1] ^ d[2] ^ d[3]
    return f'{p1}{p2}{d[0]}{p3}{d[1]}{d[2]}{d[3]}'

def hamming_decode(code):
    code = list(map(int, code))
    s1 = code[0] ^ code[2] ^ code[4] ^ code[6]
    s2 = code[1] ^ code[2] ^ code[5] ^ code[6]
    s3 = code[3] ^ code[4] ^ code[5] ^ code[6]
    error_pos = s1 + (s2 << 1) + (s3 << 2)
    if error_pos != 0:
        code[error_pos - 1] ^= 1  # Correct the bit
    return ''.join(str(code[i]) for i in [2, 4, 5, 6])  # Return data bits


# ---- RUNNING EXAMPLES ----
print("=== Parity Check ===")
data = "1011001"
encoded = parity_check(data)
print("Encoded:", encoded)
print("Is Valid?", check_parity(encoded))

print("\n=== Checksum ===")
blocks = ['1100110011001100', '1111000011110000']
checksum = compute_checksum(blocks)
print("Checksum:", checksum)
print("Is Valid?", verify_checksum(blocks, checksum))

print("\n=== Hamming Code ===")
data4 = "1011"
encoded_hamming = hamming_encode(data4)
print("Encoded:", encoded_hamming)
# introduce 1-bit error for test
received = list(encoded_hamming)
received[2] = '0' if received[2] == '1' else '1'
received_code = ''.join(received)
print("Received with error:", received_code)
decoded = hamming_decode(received_code)
print("Decoded (corrected):", decoded)
```

# OUTPUT
 ![image](https://github.com/user-attachments/assets/3b7cb2b1-c31b-4693-a1d8-b263fb06eb98)

# RESULT / CONCLUSIONS
The simulation of Error Detecion and Correction Algorithms was successfully verified
