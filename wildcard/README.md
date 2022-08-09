# Traefik DNS Challenge avec OVH

TUTO : https://www.grottedubarbu.fr/traefik-dns-challenge-ovh/

## Résumé :

- GO to https://eu.api.ovh.com/createApp/
- Créer une nouvelle app et récuperer les cred'
- Créer un fichier .env et remplir les deux premières variables:

```bash
# OVH APP NAME : traefik_lets_dnschallenge_betabillet

OVH_APPLICATION_KEY=""
OVH_APPLICATION_SECRET=""
OVH_CONSUMER_KEY=""
```

- Lancer cette commande curl pour donner les droits de modification à l'app, avec votre propre DNS, bien sur.

```bash
curl -XPOST -H"X-Ovh-Application: <api_key>" -H "Content-type: application/json" \
https://eu.api.ovh.com/1.0/auth/credential  -d '{
    "accessRules": [
        {
            "method": "POST",
            "path": "/domain/zone/betabillet.tech/record"
        },
        {
            "method": "POST",
            "path": "/domain/zone/betabillet.tech/refresh"
        },
        {
            "method": "DELETE",
            "path": "/domain/zone/betabillet.tech/record/*"
        }
    ],
    "redirection":"https://www.betabillet.tech/"
}'
```

- Copiez le consumer_key et le rentrer dans le .env
- Valider la requete avec le lien envoyé dans la réponse. Pensez à mettre unlimited dans les droits car Traefik renouvelle le widlcard tout les trois mois.
- Effacez ( backupez ) le fichier acme.json dans le dossier letsencrypt monté par compose.

- Lancez le conteneur whoami dans example pour génerer les certificats. Attendre.