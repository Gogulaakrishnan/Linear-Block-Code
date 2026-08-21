# Linear-Block-Code
# Aim
Write a simple python program to Generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on received codeword using Linear block code. 
# Tools required
Google Colab
# Program
```
import numpy as np

pb = []
err = []

# Input
col = int(input("Enter the Parity bits : "))
row = int(input("Enter the Message bits : "))

# Enter P matrix
print("Enter the P matrix:")
for i in range(row):
    p = list(map(int, input(
        f"Enter row {i+1} values ({col} values separated by space): "
    ).split()))
    pb.append(p)

p_mat = np.array(pb, dtype=int)

# Identity matrix
Ik = np.eye(row, dtype=int)

# Generator matrix G = [P | I]
g_mat = np.hstack((p_mat, Ik))

# n = codeword length, k = message length
k = row
n = col + row

# Generate all possible message bits
m = np.array([
    [1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)]
    for i in range(2 ** k)
])

# Generate codewords
c = np.mod(np.dot(m, g_mat), 2)

# Calculate Hamming weight
h_dis = []

for r in c:
    h_dis.append(np.sum(r))

# Minimum Hamming distance
d_min = np.min(h_dis[1:])

# Parity Check Matrix
# H = [I | P^T]
I_parity = np.eye(col, dtype=int)
hp = np.hstack((I_parity, p_mat.T))

# H transpose
ht = hp.T

# Display Generator Matrix
print("\n**********")
print("The Generator Matrix G is:")

for r in g_mat:
    print(" ".join(map(str, r)))

# Display messages, codewords and weights
print("\n**********")
print("Message Bits\tCodeword\tHamming Weight")

for i in range(len(m)):
    message = " ".join(map(str, m[i]))
    codeword = " ".join(map(str, c[i]))
    print(f"{message}\t\t{codeword}\t{h_dis[i]}")

# Minimum Hamming distance
print("\n**********")
print("Minimum Hamming Distance :", d_min)

# Display Parity Check Matrix
print("\n**********")
print("Parity Check Matrix H:")

for r in hp:
    print(" ".join(map(str, r)))

# Display H transpose
print("\n**********")
print("Parity Check Matrix Transpose H^T:")

for r in ht:
    print(" ".join(map(str, r)))

# Receive codeword
print("\n**********")
rc = list(map(int, input(
    f"Enter the received codeword ({n} bits): "
).split()))

# Convert to numpy array
rc = np.array(rc)

# Syndrome calculation
e = np.mod(np.dot(rc, ht), 2)

print("\n**********")
print("Syndrome of given received codeword:")
print(" ".join(map(str, e)))

# Find error position
error_found = False

for i in range(n):
    if np.array_equal(e, ht[i]):
        error_position = i + 1
        error_found = True
        break

if error_found:
    print("\nError position :", error_position)

    # Correct error
    corrected_codeword = rc.copy()
    corrected_codeword[error_position - 1] ^= 1

    print("Correct codeword :",
          " ".join(map(str, corrected_codeword)))
else:
    if np.all(e == 0):
        print("\nNo error detected.")
        print("Received codeword is already correct.")
    else:
        print("\nError pattern not identified.")
```
# Output Waveform

<img width="317" height="497" alt="image" src="https://github.com/user-attachments/assets/0625b891-0bcb-40f8-9b88-7bb59363b6e3" />

<img width="330" height="237" alt="image" src="https://github.com/user-attachments/assets/1c90b588-43eb-428f-84dc-6fa71d4e198c" />


# Results
The code is executed with  Generate Matrix, Codeword, Hamming weight, Syndrome matrix and the output is verified.
