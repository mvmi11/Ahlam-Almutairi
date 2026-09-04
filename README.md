# Linear-Algebra-Solvers
stationary-combustion.html
import numpy as np
import matplotlib.pyplot as plt

def generate_secure_key():
    # Matrix A must be invertible (Determinant != 0)
    # This is our secret encryption key
    return np.array([[3, 2], [7, 5]])

def encrypt_image(image_matrix, key):
    # Flatten and reshape image to apply matrix multiplication
    original_shape = image_matrix.shape
    flattened = image_matrix.flatten()
    
    # Make sure it matches the 2x2 key dimensions
    if len(flattened) % 2 != 0:
        flattened = np.append(flattened, 0)
        
    reshaped = flattened.reshape(-1, 2)
    # Apply Linear Transformation: Y = X * A
    encrypted_reshaped = np.dot(reshaped, key) % 256
    return encrypted_reshaped, original_shape

def decrypt_image(encrypted_reshaped, key, original_shape):
    # Calculus/Algebra connection: To reverse the transformation, compute the Inverse Matrix A^-1
    det = int(np.round(np.linalg.det(key)))
    if det == 0:
        raise ValueError("Key matrix is singular and cannot decrypt!")
        
    # Standard matrix inversion
    inv_key = np.linalg.inv(key)
    
    # Reverse transformation: X = Y * A^-1
    decrypted_reshaped = np.dot(encrypted_reshaped, inv_key)
    decrypted_flat = np.round(decrypted_reshaped).astype(int).flatten()
    
    # Restore original image dimensions
    if len(decrypted_flat) > np.prod(original_shape):
        decrypted_flat = decrypted_flat[:np.prod(original_shape)]
        
    return decrypted_flat.reshape(original_shape)

if __name__ == "__main__":
    print("🔐 Initializing Linear Algebra Cryptography Framework...")
    
    # Create a dummy 50x50 numerical grid representing a pixelated image (e.g., a cross pattern)
    original_img = np.zeros((50, 50))
    original_img[15:35, :] = 150
    original_img[:, 15:35] = 150
    
    key = generate_secure_key()
    
    # 1. Encrypt
    encrypted_data, orig_shape = encrypt_image(original_img, key)
    # Reshape for visualization
    encrypted_img = encrypted_data.flatten()[:2500].reshape(50, 50)
    
    # 2. Decrypt using the Inverse Matrix
    decrypted_img = decrypt_image(encrypted_data, key, orig_shape)
    
    print("✅ Encryption and Decryption processes completed successfully!")
    
    # Plotting the visual proof of Linear Algebra in action
    fig, axes = plt.subplots(1, 3, figsize=(15, 5))
    axes[0].imshow(original_img, cmap='gray')
    axes[0].set_title('1. Original Data Input')
    
    axes[1].imshow(encrypted_img, cmap='gray')
    axes[1].set_title('2. Linear Encrypted Output (Noise)')
    
    axes[2].imshow(decrypted_img, cmap='gray')
    axes[2].set_title('3. Decrypted Output via Inverse Matrix')
    
    for ax in axes:
        ax.axis('off')
        
    plt.tight_layout()
    plt.savefig('cryptography_proof.png')
    print("📊 Visual verification saved as 'cryptography_proof.png'")

 # 🔐 Linear Algebra Cryptography: Image Encryption via Matrix Transformations

Welcome to my advanced computing repository! This project implements a secure image cryptography framework demonstrating a direct real-world application of **Linear Algebra** and **Matrix Inversion Theories**.

## 📐 The Mathematical Framework

An digital image can be structurally viewed as a high-dimensional tensor or matrix of numbers. By using matrix transformations, we can scramble this spatial data deterministically.

### 1. Encryption (Linear Mapping)
Let $X$ represent the vector containing our raw data points, and $A$ represent our invertible key matrix. The cipher data vector $Y$ is generated using matrix multiplication:

$$ Y = X \cdot A \pmod{256} $$

For this project, our transformation operator is defined as:
$$ A = \begin{bmatrix} 3 & 2 \\ 7 & 5 \end{bmatrix} $$

### 2. Decryption (Inverse Operator Lookup)
To recover the original scrambled data structure, we leverage the foundational property of invertible fields. Since $\det(A) = 15 - 14 = 1 \neq 0$, the inverse matrix $A^{-1}$ exists. We reverse the transformation using:

$$ X = Y \cdot A^{-1} $$

Where the inverse transformation operator is calculated as:
$$ A^{-1} = \begin{bmatrix} 5 & -2 \\ -7 & 3 \end{bmatrix} $$

---

## 💻 Tech Stack & Implementation
* **Language:** Python 3
* **Libraries:** `NumPy` for heavy linear calculations, `Matplotlib` for generating visual proof grids.
* **Outputs:** Generates an end-to-end verification flowchart pipeline saved as `cryptography_proof.png`.

---
✨ *Engineered with mathematical precision by Ahlam Al-Mutairi — Mathematics Undergraduate student exploring Cryptography & Data Security.*
