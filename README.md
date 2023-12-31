# Checkpoint 2 TSSR

# Exercice 1 : Script Powershell
Effectué en dehors du timing

# Import des modules nécessaires
Import-Module ActiveDirectory

# Fonction pour générer un mot de passe aléatoire de 12 caractères
function Random-Password {
    $length = 12
    $validChars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_=+[]{}|;:'<>,.?/"
    $password = ""
    1..$length | ForEach-Object {
        $password += $validChars[(Get-Random -Maximum $validChars.Length)]
    }
    return $password
}

# Fichier CSV contenant les nouveaux utilisateurs
$File = "c:\scripts\users.csv"
$FileCsv = Import-Csv -Path $File -Delimiter ";" -Header "prenom","nom","societe","fonction","service","description","mail","mobile","scriptPath","telephoneNumber" -Encoding UTF8

# Nom de la société (modifiable)
$Company = $CompanyName

# Récupération des utilisateurs existants
$ADUsers = Get-ADUser -Filter * -Properties SamAccountName

# Parcours des utilisateurs à créer
Foreach ($User in $FileCsv) {
    $SamAccountName = "$($User.prenom).$($User.nom)"
    $ResCheckADUser = $ADUsers | Where {$_.SamAccountName -eq $SamAccountName}

    # Vérification si l'utilisateur existe déjà
    If ($ResCheckADUser -eq $null) {
        Write-Host "Compte $SamAccountName à Créer"

        # Mot de passe aléatoire
        $Password = Random-Password | ConvertTo-SecureString -AsPlainText -Force
        $Email = $User.mail
        $MobilePhone = $User.mobile
        $Title = $User.fonction
        $Department = $User.service
        $Description = $User.description
        $ScriptPath = $User.scriptPath
        $UserPrincipalName = "$SamAccountName@Sweetcakes.net"
        $Name = "$($User.prenom) $($User.nom)"
        $OfficePhone = $User.telephoneNumber
        $GivenName = $User.prenom
        $Surname = $User.nom
        $DisplayName = $SamAccountName
        $Path = "OU=$($User.service),OU=Utilisateurs,DC=sweetcakes,DC=net"

        # Création de l'utilisateur
        New-ADUser `
            -SamAccountName $SamAccountName `
            -UserPrincipalName $UserPrincipalName `
            -Name $Name `
            -GivenName $GivenName `
            -Surname $Surname `
            -Enabled $True `
            -DisplayName $DisplayName `
            -Path $Path `
            -Company $Company `
            -OfficePhone $OfficePhone `
            -MobilePhone $MobilePhone `
            -Title $Title `
            -Department $Department `
            -AccountPassword $Password `
            -ChangePasswordAtLogon $True `
            -Description $Description `
            -ScriptPath $ScriptPath

        Write-Host "Le compte $Name a été créé`nLe mot de passe est: $([System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($Password)))" -ForegroundColor Green
    }
    Else {
        Write-Host "Le compte $SamAccountName est déjà créé" -ForegroundColor Red
    }
}

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

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/ce9f1594-5554-4db9-ae60-98a934a8b897)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/92cb9af1-7827-4695-8a1a-05268403eb4c)

____

Q1a : _Sur la VM Client1 :_       
_Entre le PC Client1 et le serveur DC, le ping ne fonctionne pas. Pourquoi ?_   

**R : Le ping ne fonctionne pas car d'une la VM Client n’a pas le même masque sous réseau que la VM Serveur et de deux le serveur DHCP n'est pas encore configuré pour définir une plage d'adresses ou une adresse statique réservée qui peuvent être allouées à des hôtes qui en font la demande.**   
  
**IP VM serveur en réseau interne : 172.16.0.250 /24**  
**IP VM client en réseau interne : 169.254.213.238 /16** 

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/0b85e286-acf0-4159-8924-4b74a216abad)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/5289dc6d-12ff-4f9c-bbf3-a846d1b2ee4c)


Q1b : _Change le paramétrage des machines pour que cela soit possible._     
_Explique ce que tu as fait sur les 2 machines._     

**J'ai créé une plage d'adresses sur la VM serveur pour les clients qui en font la demande**  

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/684484bd-4156-492a-a49a-11b33e81ba86)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/961b7b34-8b4c-4e3b-adca-6fd14e6b11eb)

**J'ai fait un ipconfig /release et un ipconfig /renew sur la VM client**

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/c1112355-a463-404d-b1a8-8f98353ccebb)

**puis fait un ping vers la VM serveur qui cette fois a fonctionné**

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/d0d21321-3757-49b8-b139-7cfb18dc0f12)

____

Q2 : _Tente de te connecter avec l'utilisateur john.doe/Azerty123! sur Client1._    
Q3 : _Tente de te connecter avec l'utilisateur jane.doe/Azerty123! sur Client1._    
_Quel est la différence entre les deux utilisateurs ?_     

**R2 : John Doe doit changer son mot de passe avant la première connexion** 

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/1727edee-1dd1-4bee-8156-37437cf83ac1)    

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/68f48441-e31d-4680-b873-ebf627170882)

**R3 : Le compte de Jane Doe est désactivé**

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/88cbb0f8-a08b-4592-adbb-6a081b21b1e6)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/0041d28b-7977-4e0d-8a3a-c4d6fe467bc7)

___________

Q4 : _Que dois-tu faire pour pouvoir te connecter avec le compte de John Doe sur le Client1 ?_   
_Explique et fais l'action._  

**R4 : J'ai changé le mot de passe de John Doe**  

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/1815dabe-706e-4aae-9982-31bdc5eea0e6)

___________

Q5 : _Configure le serveur DHCP pour adresser entre 172.16.0.10 et 172.16.0.199_  

**R5 : Opération effectuée à la question 1**   

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/ec5f34c6-af79-49f3-ae3c-fd3741fd9ff9)

______________

Q6 : _Fait un enregistrement de type A pointant vers 172.16.0.254 nommé pfsense_   

Q7 : _Fait un alias sur l'enregistrement de type A qui s'appelle firewall (172.16.0.254)_

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/a05734d5-f4b8-4399-a585-2fb64f43d489)

________

Q8 : _Tu vas devoir créer des GPO qui s'appliquera aux utilisateurs.  # Effectué en dehors du timing    

_________

_Les utilisateurs n'ont pas accès à l'invite de commande et au powershell._     


![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/836b2885-5863-4fd3-9778-39c7112bb27b)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/68badfba-80d6-4e39-8e9f-e8333c836eda)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/0001be81-90ec-42b5-b989-df1f9222f743)

___________

_Les utilisateurs ont un fond d'écran imposé par l'entreprise._         

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/2474ae30-a574-44fb-a883-170000fd11d7)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/49b1b410-dee6-4b82-9d7c-486103eb9fe1)

_________

_Les utilisateurs du domaine peuvent se connecter sur le poste Client1 uniquement entre 9h et 17h._ 

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/fa412c14-bb02-4c99-ba7b-6d3667e63292)

![image](https://github.com/techerbeatrice/checkpoint_02_TSSR/assets/138071140/be12d3a9-6de6-452f-b55e-086f90885fe0)






