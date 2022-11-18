# Script de Laplace
#
# > Receber uma função Transferência
# > Visualizar diagrama de polos e zeros.
# > Plotar Gráfico de Magnitude da função transferência
# > Gráfico com o corte do plano sigma = 0

# TESTE -> 1 (LIGADO) , 0 (DESLIGADO)

Teste = 1

# TODOS OS IMPORTS

import matplotlib.pyplot as plt
import control as ctl
import numpy as np



# RECEBENDO DADOS DO USUÁRIO E ARMAZENANDO-OS DEVIDAMENTE

linha1 = input('Digite o Numerador da funcao transferencia (A B C) : ')
linha2 = input('Digite o denominador da funcao transferencia (A B C) : ')

if(Teste == 1):
    print('')
    print('linha1 : ',linha1)
    print('linha2 : ',linha2)

Ns = linha1.split()  # Separa os valores do numerador de 'string' para unidades no formato 'char' e anexa em Ns.
Ds = linha2.split()  # Separa os valores do denominador de 'string' para unidades no formato 'char' e anexa em Ds.

if(Teste == 1):
    print('')
    print('Ns : ',Ns)
    print('Ds : ', Ds)

VetNs = list(map(float,Ns)) # Map -> transforma em float de um em um , List -> "Vetoriza"
VetDs = list(map(float,Ds)) # Map -> transforma em float de um em um , List -> "Vetoriza"

if(Teste == 1):
    print('')
    print('VetNs : ',VetNs)
    print('VetDs : ',VetDs)

# CRIANDO A FUNÇÃO TRANSFERÊNCIA A PARTIR DOS DADOS FORNECIDOS PELO USUÁRIO

Fs = ctl.TransferFunction(VetNs, VetDs) # função tf da biblioteca control, recebe Numerador e Denominador vetorizados como parâmetro e os "transforma" em uma função transferância.

if(Teste == 1):
    print('')
    print('Fs : ',Fs)

# DIAGRAMA DE POLOS E ZEROS

polos = ctl.poles(Fs) #calcula os polos da função e entrega no modo complexo (Parte Real + Pare Imaginária)
zeros = ctl.zeros(Fs) #calcula os polos da função e entrega no modo complexo (Parte Real + Pare Imaginária)

if(Teste == 1):

    print('')
    print('Polos : ',polos)
    print('Zeros : ',zeros)

PZ = ctl.pzmap(Fs,1,0,'Diagrama de Polos e Zeros')                  # pzmap é uma função da biblioteca control que recebe como parametros (função, plot, grid, 'titulo')
plt.show()                                                          # função da biblioteca matplotlib.pyplot que mostra o gráfico na tela.

tampole=len(polos)
tamzero=len(zeros)

print('Quantos polos: ',tampole)
print('Quantos Zeros: ',tamzero)


# GRÁFICO 3D

def f(x,y):

    Num = 0
    for i in range(tamzero):
        calc = (X - zeros[i].real) ** 2 + (Y - zeros[i].imag) ** 2
        Num = calc**(1/2) + Num


    if(tampole==0):
        Den = 1
    else:
        Den = 0
        for i in range(tampole):
            calc = (X - polos[i].real) ** 2 + (Y - polos[i].imag) ** 2
            Den = calc**(1/2) + Den

    return Num / Den

x = np.linspace(-6, 6, 30)
y = np.linspace(-6, 6, 30)

X, Y = np.meshgrid(x, y)
Z = f(X,Y)

fig = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap='viridis', edgecolor='none')

ax.set_title('surface')
ax.set_xlabel('real part')
ax.set_ylabel('imaginary part')
ax.set_zlabel('|H(s)|')

plt.show()

# GRÁFICO COM CORTE EM SIGMA = 0 (x = 0)

fig2 = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_surface(0, Y, Z, rstride=1, cstride=1, cmap='viridis', edgecolor='none')

ax.set_title('sigma = 0')
ax.set_xlabel('real part')
ax.set_ylabel('imaginary part')
ax.set_zlabel('|H(s)|')

plt.show()
