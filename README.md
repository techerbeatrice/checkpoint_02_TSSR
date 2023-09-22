# Checkpoint 2 TSSR

# Exercice 1 : Script Powershell

# Exercice 2 : Réseaux et domaine

Tu travailles dans le service SI de la société SweetCakes.   
Tu dois reprendre en urgence le travail de l'un de tes collègues.   


Serveur DC :   
OS : Windows Server 2019   
RAM : 2 Go   
Processeurs : 2   
Compte d'administration : Administrateur/Azerty123!   
Domaine : checkpoint.lan  
Rôles installés : AD, DHCP, DNS  
Configuration IP : 172.16.0.250 /24  
  
Client   
OS : Windows 10   
Disque : 50 Go (1 partition pour le système de 35 Go et 1 partition Data de 15 Go)    
RAM : 1 Go   
Compte utilisateur : wilder/Azerty123!   
Configuration IP : DHCP   
Hostname : Change le hostname en Client1   
Connecte toi sur la machine cliente et fais la rejoindre le domaine, puis réponds aux questions suivantes   

____________

Sur la VM Client1 :   
Entre le PC Client1 et le serveur DC, le ping ne fonctionne pas. Pourquoi ?   
Change le paramétrage des machines pour que cela soit possible.   
Explique ce que tu as fait sur les 2 machines.   
Tente de te connecter avec l'utilisateur john.doe/Azerty123! sur Client1.   
Tente de te connecter avec l'utilisateur jane.doe/Azerty123! sur Client1.   
Quel est la différence entre les deux utilisateurs ?   
Que dois-tu faire pour pouvoir te connecter avec le comtpe de John Doe sur le Client1 ?   
Explique et fais l'action.   

Sur la VM DC :   
Configure le serveur DHCP pour adresser entre 172.16.0.10 et 172.16.0.199   
Fait un enregistrement de type A pointant vers 172.16.0.254 nommé pfsense.   
Fait un alias sur l'enregistrement de type A qui s'appelle firewall (172.16.0.254).   
Tu vas devoir créer des GPO qui s'appliquera aux utilisateurs.   
Les utilisateurs n'ont pas accès à l'invite de commande et au powershell.   
Les utilisateurs ont un fond d'écran imposé par l'entreprise Télécharge l'image ici      
Les utilisateurs du domaine peuvent se connecter sur le poste Client1 uniquement entre 9h et 17h.   

______________

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/7af02a1c-1500-4be7-9d02-7683d209635a)


![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/92cb9af1-7827-4695-8a1a-05268403eb4c)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/4b5c889e-3952-46bb-bb89-970e831dff32)

____

_Sur la VM Client1 :_       
_Entre le PC Client1 et le serveur DC, le ping ne fonctionne pas. Pourquoi ?_   

**R : Le ping ne fonctionne pas car d'une la VM Client n’est pas sur le même réseau que la VM Serveur et de deux le serveur DHCP n'est pas encore configuré pour définir une plage d'adresses ou une adresse statique réservée qui peuvent être allouées à des hôtes qui en font la demande.**   
  
**IP VM serveur en réseau interne : 172.16.0.250 /24**  
**IP VM client en réseau interne : 169.254.213.238 /16** 

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/eaabf549-79ea-4d58-a11c-bb8cae65c633)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/5810dd2c-7931-4147-9c89-71bf35020f30)


_Change le paramétrage des machines pour que cela soit possible._     
_Explique ce que tu as fait sur les 2 machines._     

**R : J'ai changé manuellement l'IP de la VM client, dans les paramètres Réseau et Internet --> dans l’onglet Ethernet --> Sélectionner IPV4 en mode manuel et configurer le masque de sous réseau avec les données suivantes :   
•	Configuration IP : 172.16.0.10 /24   
•	Passerelle : 172.16.0.254   
•	Masque de sous réseau : 255.255.255.0**  

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/c0226206-b527-422d-af98-b2894186ece2)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/efa7e8f8-b59f-4535-abf2-febb1bd6c5a9)

