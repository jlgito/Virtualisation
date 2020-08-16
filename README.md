#!/bin/bash
echo Bienvenue sur le script de creation des machines créations
vboxmanage list vms 
read -p  'Donner moi le nom de la vm : ' nomvm
VBoxManage createvm --name $nomvm --register
read -p 'Le nom du disque est necessaire' nomdisk
read -p 'taille du disque necessaire' tailledisk
VBoxManage createhd --filename $nomdisk --size $tailledisk
read -p 'Combien de ram avez vous besoin' tailleram                 
VBoxManage storagectl $nomvm --name "SATA Controller" --add sata --controller IntelAhci       
VBoxManage storageattach $nomvm --storagectl "SATA Controller" --port 0 --device 0 --type hdd --medium "/home/jl/Documents/$nomdisk" 
VBoxManage modifyvm $nomvm --memory $tailleram 
VBoxManage storagectl $nomvm --name IDE --add ide --controller PIIX4 --bootable on
read -p 'Ou se trouve votre image iso?' iso
VBoxManage storageattach $nomvm --storagectl IDE --port 0 --device 0 --type dvddrive --medium "/home/jl/Téléchargements/$iso"
read -p 'De combien de vram aurez vous besoin?' vram
read -p 'Auriez vous besoin de laccelaration 3d?' accel
VBoxManage modifyvm $nomvm --vram $vram --accelerate3d $accel 
read -p 'Le cable Eth   ernet doit il rester branché à la carte reséau ( On ou off)' onoff
VBoxManage modifyvm $nomvm --nic1 nat --nictype1 82540EM --cableconnected1 $onoff
VBoxManage modifyvm $nomvm --ioapic on 
VBoxManage startvm $nomvm
