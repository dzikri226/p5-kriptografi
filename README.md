# p5-kriptografi
# M.Dzikri Hidayat
# 312210136
# T.I 22 A1
# criptografi
def generate_matrix(kunci):
    kunci = ''.join(sorted(set(kunci), key=lambda k: kunci.index(k)))
    alfabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    matrix = []
    for c in kunci:
        if c not in matrix:
            matrix.append(c)
    for c in alfabet:
        if c not in matrix:
            matrix.append(c)
    return [matrix[i:i + 5] for i in range(0, 25, 5)]

def preprocess_text(teks):
    teks = teks.upper().replace("J", "I")
    processed = []
    i = 0
    while i < len(teks):
        a = teks[i]
        b = 'X' if i + 1 == len(teks) else teks[i + 1]
        if a != b:
            processed.extend([a, b])
            i += 2
        else:
            processed.extend([a, 'X'])
            i += 1
    return ''.join(processed)

def find_position(matrix, char):
    for row in range(5):
        for col in range(5):
            if matrix[row][col] == char:
                return row, col

def playfair_encrypt(plaintext, kunci):
    matrix = generate_matrix(kunci)
    plaintext = preprocess_text(plaintext)
    encrypted_text = []
    for i in range(0, len(plaintext), 2):
        a_row, a_col = find_position(matrix, plaintext[i])
        b_row, b_col = find_position(matrix, plaintext[i + 1])
        if a_row == b_row:
            encrypted_text.extend([matrix[a_row][(a_col + 1) % 5], matrix[b_row][(b_col + 1) % 5]])
        elif a_col == b_col:
            encrypted_text.extend([matrix[(a_row + 1) % 5][a_col], matrix[(b_row + 1) % 5][b_col]])
        else:
            encrypted_text.extend([matrix[a_row][b_col], matrix[b_row][a_col]])
    return ''.join(encrypted_text)

def playfair_decrypt(ciphertext, kunci):
    matrix = generate_matrix(kunci)
    decrypted_text = []
    for i in range(0, len(ciphertext), 2):
        a_row, a_col = find_position(matrix, ciphertext[i])
        b_row, b_col = find_position(matrix, ciphertext[i + 1])
        if a_row == b_row:
            decrypted_text.extend([matrix[a_row][(a_col - 1) % 5], matrix[b_row][(b_col - 1) % 5]])
        elif a_col == b_col:
            decrypted_text.extend([matrix[(a_row - 1) % 5][a_col], matrix[(b_row - 1) % 5][b_col]])
        else:
            decrypted_text.extend([matrix[a_row][b_col], matrix[b_row][a_col]])
    return ''.join(decrypted_text)

# Kunci dan teks
kunci = "TEKNIKINFORMATIKA"
plaintext1 = "GOOD BROOM SWEEP CLEAN REDWOOD NATIONAL STATE PARK"
plaintext2 = "JUNK FOOD"
plaintext3 = "DAN MASALAH KESEHATAN"

# Enkripsi
ciphertext1 = playfair_encrypt(plaintext1.replace(" ", ""), kunci)
ciphertext2 = playfair_encrypt(plaintext2.replace(" ", ""), kunci)
ciphertext3 = playfair_encrypt(plaintext3.replace(" ", ""), kunci)

# Dekripsi
decrypted_text1 = playfair_decrypt(ciphertext1, kunci)
decrypted_text2 = playfair_decrypt(ciphertext2, kunci)
decrypted_text3 = playfair_decrypt(ciphertext3, kunci)

# Menampilkan hasil
print(f"Plaintext 1: {plaintext1}")
print(f"Teks Enkripsi 1: {ciphertext1}")
print(f"Teks Dekripsi 1: {decrypted_text1}")

print(f"\nPlaintext 2: {plaintext2}")
print(f"Teks Enkripsi 2: {ciphertext2}")
print(f"Teks Dekripsi 2: {decrypted_text2}")

print(f"\nPlaintext 3: {plaintext3}")
print(f"Teks Enkripsi 3: {ciphertext3}")
print(f"Teks Dekripsi 3: {decrypted_text3}")

![Screenshot 2024-10-15 102429](https://github.com/user-attachments/assets/113a3dc4-7a60-4817-b45b-98073eb51c18)
