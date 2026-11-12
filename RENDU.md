# Rendu — Atelier Kubernetes

> Complétez ce fichier au fil de l'atelier, puis faites un commit et un push (voir la section « Rendu » du README).

**Nom / Prénom :**
Axel Janodet--Marty
**Groupe :**
RS2

---

## Exercice 1 — Cluster

1. Combien de nœuds affiche `kubectl get nodes` ? Quel est le rôle de chacun ?
   > 3 noeuds | il y a 2 agent et 1 serveur
2. Que montre `docker ps` ? Qu'est-ce qu'un « nœud » dans k3d, concrètement ?
   > docker ps liste les conteneurs actifs, et dans k3d, un « nœud » est simplement un conteneur Docker qui fait office de serveur Kubernetes.

## Exercice 2 — Premier déploiement

3. Quels objets Kubernetes ont été créés par `kubectl create deployment` ? (indice : `kubectl get all`)
   > kubectl create deployment crée un Deployment, un ReplicaSet et un ou plusieurs Pods pour gérer et exécuter l'application.
4. Vous avez supprimé le pod `web-...` : que s'est-il passé, et pourquoi ?
   > Kubernetes recrée automatiquement le pod supprimé pour respecter l'état souhaité défini par le Deployment (auto-réparation).

## Exercice 3 — Modifier un conteneur en direct

5. Après suppression du pod, votre message « Bienvenue... » a-t-il survécu ? Pourquoi ?
   > Non.
   Pourquoi : Le stockage d'un conteneur est éphémère. Lorsqu'un pod est supprimé, le nouveau pod recréé repart de l'image d'origine (sans vos modifications manuelles)
6. Pourquoi le `port-forward` a-t-il cessé de fonctionner après la suppression du pod ?
   > Le tunnel était directement relié au pod spécifique qui vient d'être détruit. Sa liaison réseau ayant disparu, le tunnel s'est coupé.

## Exercice 4 — Déclaratif

7. Quelle différence entre `kubectl create` et `kubectl apply` ?
   > kubectl create crée une ressource de manière unique (et échoue si elle existe déjà), alors que kubectl apply la crée ou la met à jour de façon déclarative.
8. À quoi sert le `selector` d'un Service ? Que se passe-t-il s'il ne correspond à aucun pod ?
   > Le selector permet d'identifier quels pods reçoivent le trafic du Service ; s'il ne correspond à aucun pod, le Service reste vide et le trafic ne peut pas être acheminé.

## Exercice 5 — Scaling & mises à jour

9. Sur quels nœuds se trouvent vos 3 réplicas ? Collez la sortie de `kubectl get pods -l app=site -o wide`.
    Les trois réplicas se répartissent sur les nœuds du cluster k3d, comme l'indique la colonne NODE de la sortie de commande
   ``` 
   NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE               NOMINATED NODE   READINESS GATES
site-6f59669f8f-qv64k   1/1     Running   0          4m30s   10.42.0.6    k3d-lab-server-0   <none>           <none>
site-6f59669f8f-rlhvq   1/1     Running   0          4m22s   10.42.1.7    k3d-lab-agent-0    <none>           <none>
site-6f59669f8f-t9dft   1/1     Running   0          4m35s   10.42.2.10   k3d-lab-agent-1    <none>           <none>

   ```
   
   ```
10. Avec l'image `nginx:9.99`, quel statut avait le pod ? Quelle commande vous a donné la cause ?
    > Le statut était ImagePullBackOff (ou ErrImagePull), et c'est la commande kubectl describe pod qui a permis de lire la cause exacte de l'erreur d'extraction.
11. Pendant la panne, le site restait-il accessible ? Pourquoi ?
    > Oui, le site restait accessible car la stratégie de mise à jour progressive (rolling update) maintient les anciens pods fonctionnels en service tant que les nouveaux ne sont pas prêts.

## Exercice 6 — Portainer & Mario

12. URL publique de votre Mario :
    > https://zany-space-yodel-jxg5vwpvv9254q7-8585.app.github.dev/
13. Retrouvez le Deployment `mario` en ligne de commande. Portainer et `kubectl` parlent-ils au même composant ? Lequel ?
    > Pour retrouver le deployment, il faut faire kubectl get deployment mario. Oui, Portainer et kubectl parlent au même composant : l'API Server de Kubernetes.

## Bonus (facultatif)

14. Bonus réalisés et remarques :
    >

---

**Score obtenu avec `./scripts/check.sh` :** ... / ...
