## EASY
## question 1
### a

![Screenshot (65)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/58a7b0d2-3d51-4ece-8c9a-289190a07a05)
![Screenshot (67)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/2b93c5d9-bb2b-451b-bcd4-469ff6e67103)
![Screenshot (68)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/99a52e26-e6d1-463c-b85f-8ca1b9355256)
![Screenshot (69)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/fb9accd0-67e9-460a-817b-37ab295e675f)
## b

![Screenshot (70)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/1f359b6a-cf41-4e50-b337-90ef3fa1e16f)
![Screenshot (71)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/ce14e1d9-e263-4c5e-adda-897b03f4a49d)
![Screenshot (72)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/5e48b198-d4b4-4064-8842-9c0e514887e7)
## question 2 

![Screenshot (74)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/dd8056b5-458a-4bf6-a123-47ee859d2203)
![Screenshot (73)](https://github.com/AdwaithaV/bi0s-recruitment-/assets/142923950/e349fc89-45c4-489f-a4d7-13403a8a3ae6)

## question 3
import socket


from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes


from cryptography.hazmat.backends import default_backend


import hashlib


import hmac


import os


def generate_key():


    return os.urandom(32)

    
def encrypt(key, message):


    iv = os.urandom(16)

    
    cipher = Cipher(algorithms.AES(key), modes.CFB(iv), backend=default_backend())
    encryptor = cipher.encryptor()
    ciphertext = encryptor.update(message) + encryptor.finalize()
    return iv + ciphertext
def decrypt(key, ciphertext):
    iv = ciphertext[:16]
    cipher = Cipher(algorithms.AES(key), modes.CFB(iv), backend=default_backend())
    decryptor = cipher.decryptor()
    message = decryptor.update(ciphertext[16:]) + decryptor.finalize()
    return message
def calculate_sha512(data):
    sha512 = hashlib.sha512()
    sha512.update(data)
    return sha512.digest()
def generate_hmac(key, data):
    h = hmac.new(key, data, hashlib.sha256)
    return h.digest()
def server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(('localhost', 12345))
    server_socket.listen(1)
    print("Server listening on port 12345")
    conn, addr = server_socket.accept()
    with conn:
        print(f"Connected by {addr}")
        key = generate_key()
        conn.sendall(key)
        while True:
            ciphertext = conn.recv(1024)
        if not ciphertext:
                break
            decrypted_message = decrypt(key, ciphertext)
            integrity_hash = calculate_sha512(decrypted_message)
            received_hmac = conn.recv(64)    
            calculated_hmac = generate_hmac(key, integrity_hash)
            if received_hmac == calculated_hmac:
                print("Message Received:", decrypted_message.decode())
            else:
                print("Integrity check failed. Message may be tampered.")
def client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect(('localhost', 12345))
    key = client_socket.recv(32)
    while True:
        message = input("Enter your message: ")
        ciphertext = encrypt(key, message.encode()
        integrity_hash = calculate_sha512(message.encode())
        hmac_generated = generate_hmac(key, integrity_hash)
        client_socket.sendall(ciphertext)
        client_socket.sendall(hmac_generated)
if __name__ == "__main__":
    import multiprocessing
 server_process = multiprocessing.Process(target=server)
    client_process = multiprocessing.Process(target=client)
    server_process.start()
    client_process.start()
    server_process.join()
    client_process.join()

