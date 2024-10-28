# PR0202: Conexión remota con SSH
## 1. Preparación de la máquina y configuración de la red
### 1. Añadir un segundo adaptador de red en VirtualBox:
#### Abre VirtualBox.
#### Selecciona tu máquina virtual de Ubuntu en la lista y haz clic en Configuración.
#### Ve a la pestaña Red.
#### Ahora habilita el Adaptador 2 seleccionando la opción Red solo anfitrión. Esto conecta tu VM directamente a la red de tu sistema anfitrión sin pasar por internet.
#### Haz clic en Aceptar para guardar los cambios.
### 2 Comprobar la dirección IP
```bash
vagrant@ubuntu2204:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:8c:69:41 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    inet 10.0.2.15/24 metric 100 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 86248sec preferred_lft 86248sec
    inet6 fe80::a00:27ff:fe8c:6941/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 08:00:27:59:52:53 brd ff:ff:ff:ff:ff:ff
    altname enp0s8
```