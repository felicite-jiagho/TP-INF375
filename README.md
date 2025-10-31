# TP-INF375
# --------------------------------------------------------------
# TP : Implémentation de quelques méthodes numériques en Python
# Étudiant : (jiagho feukeng chamberline felicite)
# Classe : L2
# --------------------------------------------------------------
# Contenu :
# 1. Méthode du Pivot de Gauss
# 2. Méthode de Crout
# 3. Méthode de Dichotomie
# 4. Méthode de Balayage
# --------------------------------------------------------------

import numpy as np

# ==============================================================
# 1️⃣ MÉTHODE DU PIVOT DE GAUSS
# ==============================================================
def pivot_de_gauss():
    """
    Résout le système :
        x + y = 1
        x - y = 1
        3x + y = 3
    par la méthode du pivot de Gauss.
    """
    mat = [
        [1, 1, 1],
        [1, -1, 1],
        [3, 1, 3]
    ]

    # Élimination de x
    for i in range(1, 3):
        coeff = mat[i][0] / mat[0][0]
        for j in range(3):
            mat[i][j] = mat[i][j] - coeff * mat[0][j]

    # Élimination de y
    coeff = mat[2][1] / mat[1][1]
    for j in range(3):
        mat[2][j] = mat[2][j] - coeff * mat[1][j]

    # Remontée
    y = mat[1][2] / mat[1][1]
    x = (mat[0][2] - mat[0][1] * y) / mat[0][0]

    print("=== Méthode du pivot de Gauss ===")
    print("x =", x)
    print("y =", y)
    print("---------------------------------\n")


# ==============================================================
# 2️⃣ MÉTHODE DE CROUT
# ==============================================================
def methode_de_crout():
    """
    Décompose la matrice A en deux matrices L et U telles que A = L * U
    selon la méthode de Crout.
    """
    A = np.array([
        [4, 2, 0],
        [4, 4, 2],
        [2, 2, 3]
    ], dtype=float)

    n = A.shape[0]
    L = np.zeros((n, n))
    U = np.zeros((n, n))

    for j in range(n):
        for i in range(j, n):
            somme = 0
            for k in range(j):
                somme += L[i][k] * U[k][j]
            L[i][j] = A[i][j] - somme

        U[j][j] = 1
        for i in range(j + 1, n):
            somme = 0
            for k in range(j):
                somme += L[j][k] * U[k][i]
            U[j][i] = (A[j][i] - somme) / L[j][j]

    print("=== Méthode de Crout ===")
    print("Matrice L :\n", L)
    print("Matrice U :\n", U)
    print("Vérification (L * U) =\n", np.dot(L, U))
    print("---------------------------------\n")


# ==============================================================
# 3️⃣ MÉTHODE DE DICHOTOMIE
# ==============================================================
def methode_dichotomie(a=1, b=2, tol=1e-1):
    """
    Approxime sqrt(2) par la méthode de dichotomie avec une tolérance donnée.
    """
    def f(x):
        return x * x - 2

    if f(a) * f(b) > 0:
        raise ValueError("Pas de changement de signe sur [a,b]")

    while (b - a) / 2 > tol:
        c = (a + b) / 2
        if f(c) == 0:
            return c
        elif f(a) * f(c) < 0:
            b = c
        else:
            a = c
    return (a + b) / 2


# ==============================================================
# 4️⃣ MÉTHODE DE BALAYAGE
# ==============================================================
def methode_balayage(a=1, b=2, h=0.1):
    """
    Approxime sqrt(2) par la méthode de balayage avec un pas donné.
    """
    def f(x):
        return x * x - 2

    x = a
    while x + h <= b:
        if f(x) * f(x + h) < 0:
            return (x + (x + h)) / 2
        x += h
    return None


# ==============================================================
# EXÉCUTION DES MÉTHODES
# ==============================================================
if __name__ == "__main__":
    pivot_de_gauss()
    methode_de_crout()

    # Dichotomie
    racine_dicho = methode_dichotomie()
    print("=== Méthode de Dichotomie ===")
    print("Approximation de sqrt(2) :", racine_dicho)
    print("---------------------------------\n")

    # Balayage
    racine_bal = methode_balayage()
    print("=== Méthode de Balayage ===")
    print("Approximation de sqrt(2) :", racine_bal)
    print("---------------------------------\n")
