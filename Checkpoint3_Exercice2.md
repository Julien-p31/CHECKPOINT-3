# 🎯Partie 1 : Gestion des utilisateurs

# 📊 Q.2.1.1 

- Utilisation de la commande ``adduser``
- Création de l'utilisateur ``optimus`` avec comme mot de passe : ``Azerty1*``
  
![2 1 1](https://github.com/user-attachments/assets/cfd59120-68b6-4a19-a085-41eba264d07b)

---

# 📊 Q.2.1.2 

- Pour mon compte à usage personnel, je lui donnerais le moindre privilège pour qu'il ait accès juste a ce dont il a besoin. Je mettrais également un mot de passe robuste.

---

# 🎯 Partie 2 : Configuration de SSH


# 📊 Q.2.2.1 

- Pour désactiver le compte ``root``, il faut se rendre dans le fichier ``sshd_config``.

- Avec cette commande : ``nano /etc/ssh/sshd_config`` puis modifier la bonne ligne.

![2 2 1](https://github.com/user-attachments/assets/b1faadf0-de98-4d03-a418-98ed0355a072)

---

# 📊 Q.2.2.2 

- Dans le fihcier de configuration ``sshd_config``, ajouter la ligne ``AllowUsers optimus``.

![2 2 2](https://github.com/user-attachments/assets/89570386-79c6-4f90-afe1-8c285898d4a7)

---

# 📊 Q.2.2.3 

![2 2 3](https://github.com/user-attachments/assets/f7858edd-5d87-4e36-9e13-5fc61bb86650)


- Pour mettre en place une authentification par clé valide, il faut faire en sorte que la machine cliente est ``openssh-server`` d'installer, puis sur le serveur créer une clé avec la commande ``ssh-keygen -t ecdsa`` puis de faire un envoi via ssh de la clé public avec la commande ``ssh-copy-id -i <emplacement ou se trouve la clé public "id_ecdsa_pub"> user@ip_client_ubuntu`` (modifier selon nom d'utilisateur et ip de la machine cliente).

- Et modifier également le fichier de configuration ``sshd_config`` pour permettre uniquement la connexion via clé et non par mot de passe.

---


# 🎯 Partie 3 : Analyse du stockage

# 📊 Q.2.3.1 

- ``EXT2, EXT4, LVM2, SWAP``

![2 3 1](https://github.com/user-attachments/assets/16700ea1-5d1c-4c42-bf48-bb2abf28f8be)

---

# 📊 Q.2.3.2 

- On retrouve comme système de stockage du ``HDD``, ``RAID1`` et ``LVM``.
  
---

# 📊 Q.2.3.3 

- Utilisation de la commande : ``mdadm --detail /dev/md0``, pour voir l'état du RAID.

![2 3 3](https://github.com/user-attachments/assets/9f812563-1c25-4020-88e0-4df47ff76117)


- Création et rajout d'un disque de 8G ainsi que la vérification du disque sur la machine avec la commande ``lsblk``.

![2 3 3 1](https://github.com/user-attachments/assets/2a7c7270-6910-47bd-8150-06f035a23d4c)

![2 3 3 2](https://github.com/user-attachments/assets/1d04322b-ceb2-4563-9478-7992f34ea115)


- Créer une nouvelle partition du disque ``SDB``.

![2 3 3 3](https://github.com/user-attachments/assets/a30ea383-095f-4f3c-bdab-6bc81da3c80d)


- Montage du nouveau disque ``SDB`` sur le RAID ``/dev/md0`` via la commande : ``mdadm --manage /dev/md0 --ad /dev/sdb1``.
  
![2 3 3 4](https://github.com/user-attachments/assets/84dd61c1-daf5-4890-bf45-a23f9a761fc2)

---

# 📊 Q.2.3.4 

- Voir les VGs disponible avec la commande : ``vgdisplay``, puis créer un LV sur le groupe disponible via la commande : ``lvcreate -L 2G -n Saves cp3-vg``.

![2 3 4 2](https://github.com/user-attachments/assets/461a4269-89f8-400d-ba6c-115c1a9c2bfd)

![2 3 4 3](https://github.com/user-attachments/assets/34ae0c3a-5e2d-49bb-8b12-c88baf49fbf5)


- On monte le LV via cette commande : ``mount /dev/cp3-vg/Saves /var/lib/bareos/storage/``.
  
![2 3 4 4](https://github.com/user-attachments/assets/65cb540f-f06a-4db9-ab08-bc5844476c80)

- On configure le fichier ``/etc/fstab``.

![2 3 4 5](https://github.com/user-attachments/assets/7598dd49-c2a6-4693-80c2-fab730b095b4)

---

# 📊 Q.2.3.5 

- Espace disponible : ``1.79g``.

![2 3 4 6](https://github.com/user-attachments/assets/72423d74-e3f8-4574-9823-f87f5b2a2e98)

---

# 🎯 Partie 4 : Sauvegardes

# 📊 Q.2.4.1 

- **Bareos-dir :** C'est celui qui est en charge de l'orchestration des sauvegardes (quoi sauvegarder, quand sauvegarder et ou envoyer les sauvegardes).

- **Bareos-sd :** C'est celui qui gère le stockage des sauvegardes.

- **Bareos-fd :** C'est l'agent installé sur les machines à sauvegarder. (c'est lui qui transmet les données sur le "Bareos-sd").

---


# 🎯 Partie 5 : Filtrage et analyse réseau

# 📊 Q.2.5.1 

- Voir la liste des règles actuelles : ``nft list ruleset``.
  
![2 5 1](https://github.com/user-attachments/assets/6722bdf9-48ff-4e3c-8f03-c7cbf83681df)

---

# 📊 Q.2.5.2 

- ```ct state established, related accept``` : les retours de connexions déjà établies.

- ```iifname "lo" accept``` autorise le trafic local.
- ```TCP dport 22 accept``` autorise les connexions TCP destinées au port 22 (port SSH).
- ```IP protocol icmp accept``` autorise les pings IPV4.
- ```IP6 nexthdr icmpv6 accept``` autorise les pings IPV6.

---

# 📊 Q.2.5.3 

- ```ct state invalid drop``` : les paquets ne pouvant pas être identifiés à une requêtes.
- Et tout le reste qui n'est pas en ``accept``.

---

# 📊 Q.2.5.4

- Utilisation de la commande : ``nft add rule inet inet_filter_table in_chain tcp dport { 9101-9103 } ct state new accept``


![2 5 4](https://github.com/user-attachments/assets/54efff9a-dbeb-444c-a635-f7807ab5584f)


---


# 🎯 Partie 6 : Analyse de logs

# 📊 Q.2.6.1

![2 6 1](https://github.com/user-attachments/assets/1c460344-6679-49a7-b45d-7a103d169d5b)

