Monedero Bytcoins BYT para entorno gráfico (desktop *sudo apt-get install qt5-default ) en linux

Instalación:

**1. Clonar con git**

```
git clone https://github.com/bytcoins/bytcoins-wallet.git
```

**2. enlace simbolico a "src"**

```
cd bytcoins-wallet

ln -s ../bytcoins bytcoins
```

**3. Construir**

```
mkdir build && cd build && cmake .. && make
```
