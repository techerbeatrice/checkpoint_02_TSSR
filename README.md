# Checkpoint 2 TSSR

# Exercice 1 : Script Powershell

R : **Je n'ai pas assez révisé le script powershell**  

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


____

Q1a : _Sur la VM Client1 :_       
_Entre le PC Client1 et le serveur DC, le ping ne fonctionne pas. Pourquoi ?_   

**R : Le ping ne fonctionne pas car d'une la VM Client n’est pas sur le même réseau que la VM Serveur et de deux le serveur DHCP n'est pas encore configuré pour définir une plage d'adresses ou une adresse statique réservée qui peuvent être allouées à des hôtes qui en font la demande.**   
  
**IP VM serveur en réseau interne : 172.16.0.250 /24**  
**IP VM client en réseau interne : 169.254.213.238 /16** 

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/eaabf549-79ea-4d58-a11c-bb8cae65c633)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/5810dd2c-7931-4147-9c89-71bf35020f30)


Q1b : _Change le paramétrage des machines pour que cela soit possible._     
_Explique ce que tu as fait sur les 2 machines._     

**R1 : J'ai activé le dhcp en attribution automatique sur la VM client**        

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/f41bc0ef-89a4-40d8-8e36-3134e3027c81)

**J'ai créé une plage d'adresses sur la VM serveur pour les clients qui en font la demande, ensuite de la VM client le ping adressé au VM serveur a fonctionné**  

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/684484bd-4156-492a-a49a-11b33e81ba86)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/961b7b34-8b4c-4e3b-adca-6fd14e6b11eb)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/d0d21321-3757-49b8-b139-7cfb18dc0f12)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/d9781174-68e1-43d5-9c2d-a3fc50904b96)
____

Q2 : _Tente de te connecter avec l'utilisateur john.doe/Azerty123! sur Client1._    
Q3 : _Tente de te connecter avec l'utilisateur jane.doe/Azerty123! sur Client1._    
_Quel est la différence entre les deux utilisateurs ?_     

**Ra : John Doe doit changer son mot de passe avant la première connexion** 

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/1727edee-1dd1-4bee-8156-37437cf83ac1)    

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/68f48441-e31d-4680-b873-ebf627170882)

**Rb : Le compte de Jane Doe est désactivé**

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/88cbb0f8-a08b-4592-adbb-6a081b21b1e6)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/0041d28b-7977-4e0d-8a3a-c4d6fe467bc7)

___________

Q4 : _Que dois-tu faire pour pouvoir te connecter avec le compte de John Doe sur le Client1 ?_   
_Explique et fais l'action._  

**R : J'ai changé le mot de passe de John Doe**  

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/1815dabe-706e-4aae-9982-31bdc5eea0e6)

___________

Q5 : _Configure le serveur DHCP pour adresser entre 172.16.0.10 et 172.16.0.199_  

R : **Opération effectuée à la question 1**   

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/ec5f34c6-af79-49f3-ae3c-fd3741fd9ff9)

______________

Q6 : _Fait un enregistrement de type A pointant vers 172.16.0.254 nommé pfsense_   

Q7 : _Fait un alias sur l'enregistrement de type A qui s'appelle firewall (172.16.0.254)_

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/a05734d5-f4b8-4399-a585-2fb64f43d489)

________

Q8 : _Tu vas devoir créer des GPO qui s'appliquera aux utilisateurs.
Les utilisateurs n'ont pas accès à l'invite de commande et au powershell.
Les utilisateurs ont un fond d'écran imposé par l'entreprise   
Les utilisateurs du domaine peuvent se connecter sur le poste Client1 uniquement entre 9h et 17h._   

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/baa86b63-09cd-49ef-9c63-778e93cfa0b7)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/6d2439c9-e9dc-4e16-8ad5-32e8e31b0c5b)


