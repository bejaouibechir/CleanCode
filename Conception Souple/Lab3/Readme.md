# Scénario 3 : Application de Gestion des Utilisateurs
**Contexte :** Une application pour gérer les utilisateurs et leurs rôles.

## Avant :
**Classe Utilisateur**
Propriétés : ***Id***, ***Nom***, ***Email***
Méthodes : ***AjouterRole(string role)***, ***RetirerRole(string role)***

## Implémentation Progressive des Principes :
**1. Contour conceptuel :** Utilisation du pattern Factory pour la création des utilisateurs.
``` CSharp
public class UtilisateurFactory
{
    public Utilisateur CreerUtilisateur(int id, string nom, string email)
    {
        return new Utilisateur(id, nom, email);
    }
}
```
2. Interfaces révélatrices d'intention : Nommer les interfaces et méthodes de manière descriptive.
``` CSharp
public interface IUtilisateurService
{
    void AjouterRole(int utilisateurId, string role);
    void RetirerRole(int utilisateurId, string role);
}

public class UtilisateurService : IUtilisateurService
{
    private readonly IUtilisateurRepository _repository;

    public UtilisateurService(IUtilisateurRepository repository)
    {
        _repository = repository;
    }

    public void AjouterRole(int utilisateurId, string role)
    {
        var utilisateur = _repository.GetUtilisateur(utilisateurId);
        utilisateur.AjouterRole(role);
        _repository.UpdateUtilisateur(utilisateur);
    }

    public void RetirerRole(int utilisateurId, string role)
    {
        var utilisateur = _repository.GetUtilisateur(utilisateurId);
        utilisateur.RetirerRole(role);
        _repository.UpdateUtilisateur(utilisateur);
    }
}
```
**3. Fonctions enfermées :** Utilisation de délégués pour encapsuler les opérations sur les rôles.
``` CSharp
public delegate void GestionRoleDelegate(string role);

public class Utilisateur
{
    public int Id { get; set; }
    public string Nom { get; set; }
    public string Email { get; set; }
    private List<string> roles;

    public GestionRoleDelegate AjouterRoleDelegate;
    public GestionRoleDelegate RetirerRoleDelegate;

    public Utilisateur()
    {
        roles = new List<string>();
        AjouterRoleDelegate = AjouterRole;
        RetirerRoleDelegate = RetirerRole;
    }

    private void AjouterRole(string role)
    {
        if (!roles.Contains(role))
            roles.Add(role);
    }

    private void RetirerRole(string role)
    {
        if (roles.Contains(role))
            roles.Remove(role);
    }
}
```
**4. Fonctions à effet secondaire :** Manipulation directe des valeurs de l'état de l'utilisateur en dehors de son environnement local.
``` CSharp
public class Utilisateur
{
    public int Id { get; set; }
    public string Nom { get; set; }
    public string Email { get; set; }
    private List<string> roles;

    public Utilisateur(int id, string nom, string email)
    {
        Id = id;
        Nom = nom;
        Email = email;
        roles = new List<string>();
    }

    public void AjouterRole(string role)
    {
        roles.Add(role);
        Logger.Log($"Rôle ajouté : {role} à l'utilisateur {Nom}");
    }

    public void RetirerRole(string role)
    {
        roles.Remove(role);
        Logger.Log($"Rôle retiré : {role} de l'utilisateur {Nom}");
    }
}

public static class Logger
{
    public static void Log(string message)
    {
        Console.WriteLine(message);
    }
}
```
**5. Style déclaratif :** Utilisation d'attributs pour valider les propriétés de l'utilisateur.
``` CSharp
public class Utilisateur
{
    [Required]
    public int Id { get; set; }
    [Required]
    [StringLength(100)]
    public string Nom { get; set; }
    [Required]
    [EmailAddress]
    public string Email { get; set; }
}
```
**6. Programmation défensive :** Ajout de vérifications pour éviter des états invalides.
``` CSharp
public class Utilisateur
{
    public int Id { get; private set; }
    public string Nom { get; private set; }
    public string Email { get; private set; }
    private List<string> roles;

    public Utilisateur(int id, string nom, string email)
    {
        if (string.IsNullOrWhiteSpace(nom))
            throw new ArgumentException("Nom est requis", nameof(nom));
        if (string.IsNullOrWhiteSpace(email))
            throw new ArgumentException("Email est requis", nameof(email));

        Id = id;
        Nom = nom;
        Email = email;
        roles = new List<string>();
    }

    public void AjouterRole(string role)
    {
        if (string.IsNullOrWhiteSpace(role))
            throw new ArgumentException("Role ne peut pas être vide", nameof(role));
        roles.Add(role);
    }

    public void RetirerRole(string role)
    {
        if (string.IsNullOrWhiteSpace(role))
            throw new ArgumentException("Role ne peut pas être vide", nameof(role));
        roles.Remove(role);
    }
}
```
