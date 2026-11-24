# MAP3121 - Numerical Methods and Applications

Two programming exercises for MAP3121 - Numerical Methods and Applications (2021), an Electrical Engineering course at POLI-USP. Both compute the eigenvalues and eigenvectors of symmetric matrices with the QR algorithm, written in C, and use them to find how mechanical systems vibrate.

Each algorithm was first prototyped as an Octave script (the `.m` files) to check the values and plan the functions, and then written in C for speed.

Main vibration mode of the truss from project 2, magnified 1000 times:

![Truss vibrating in its lowest-frequency mode](Projeto_2/Trelica1000w1.gif)

## Project 1: symmetric tridiagonal matrices

Finds the eigenvalues and eigenvectors of symmetric tridiagonal matrices with the QR algorithm, using Givens rotations, with or without a spectral shift (Wilkinson's heuristic).

- **Test A** compares the number of iterations with and without the shift, for a matrix with 2 on the main diagonal and -1 on the diagonals next to it (precision 10⁻⁶):

  | Matrix size | 4 | 8 | 16 | 32 | 64 |
  |---|---|---|---|---|---|
  | Without shift | 45 | 143 | 473 | 1600 | 5446 |
  | With shift | 7 | 15 | 31 | 59 | 109 |

- **Tests B and C** solve mass-spring systems (5 masses with 6 springs, and 10 masses with 11 springs) over time: the square roots of the eigenvalues are the vibration frequencies, and the eigenvectors are the vibration modes. The program writes each mass's position over time to a text file (`saidaB*.txt`, `saidaC*.txt`).

`Ep1Antigo.c` is the first version, which works on full matrices. `Ep1Otimizado.c` is the optimised version, which keeps only the three diagonals in vectors: in C, a 64x64 matrix went from 20 seconds to less than one second.

## Project 2: real symmetric matrices and a truss

Extends project 1 to any real symmetric matrix: Householder transformations, in a faster O(n²) form that works on vectors instead of multiplying matrices, turn the matrix into a tridiagonal one, and the QR algorithm from project 1 finds its eigenvalues and eigenvectors.

- **Matrix tests:** a 4x4 and a 20x20 matrix, typed in or read from a file (`input-a`, `input-b`).
- **Truss test:** the program reads a truss from a file (`input-c`: nodes, bars, angles, lengths and the material's constants), builds the global stiffness matrix of its free nodes, makes it symmetric with the mass matrix (M^-1/2 K M^-1/2) and finds its eigenvalues and eigenvectors. The five lowest frequencies and their vibration modes are saved in `saidaTesteTrelica.txt`. The animation above was made in GeoGebra from those modes.

## Repository layout

| Folder | Contents |
|---|---|
| `Projeto_1/` | Project 1: C programs, Octave prototypes (QR factorisation, Givens rotation, power method, QR algorithm) and test outputs |
| `Projeto_2/` | Project 2: C program, Octave prototypes (Householder tridiagonalisation, truss tests), input files, output and the GIF |
| `Relatorio_1/`, `Relatorio_2/` | The LaTeX reports of each project, in Portuguese |

## Running

The programs were compiled with gcc 9.2.0 (MinGW):

```
cd Projeto_2
gcc Ep2.c -o ep2 -lm
./ep2
```

Run it from the project's folder so it finds the input files by name. The program asks for the parameters it needs; the accepted values are shown in square brackets. Matrices are limited to 128x128 in project 1 and 256x256 in project 2, because the arrays in C are sized at compile time.
