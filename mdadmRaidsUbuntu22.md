## Passos previs
1. Instal·lar mdadm amb apt
2. Utilitzar lsblk per saber quins discs utilitzar:
  !(mdadmImages/image1.png)

## Creació d`un raid
1. Crear un raid (estructura bàsica)

`sudo mdadm --create --verbose /dev/md0`

* Amb `mdadm --create` indiquem que volem crear un array
* Amb `--verbose` indiquem que volem rebre informació del procés de creació
* Amb `/dev/md0` indiquem quin és el dispositiu virtual que crearà el raid

- Raid 0

`sudo mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sda /dev/sdb`

- Raid 1

`sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb`

- Raid 5

`sudo mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc`

Recordem que necessitem com a minim 3 discs

2. Guardar la configuració del array amb:
`sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf`

3. Crear un sistema de fitxers:
`sudo mkfs.ext4 -F /dev/md0`

4. Montar l'array
Primer de tot crear una carpeta on montar el disc:
`sudo mkdir /mnt/md0`

* Métode temporal
`sudo mount /dev/md0 /mnt/md0`

* Métode permanent
`sudo echo '/dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab`

## Utilitzant un raid
1. Parar un array:
`sudo mdadm --stop /dev/md0`

2. Iniciar un array:
`sudo mdadm --run /dev/md0`
