# Liste des noms des ports
portnames = ["PAN", "AMS", "CAS", "NYC", "HEL"]

# Matrice des distances (en kilomètres) entre les ports
D = [
        [0, 8943, 8019, 3652, 10545],  # Distances depuis PAN
        [8943, 0, 2619, 6317, 2078],   # Distances depuis AMS
        [8019, 2619, 0, 5836, 4939],   # Distances depuis CAS
        [3652, 6317, 5836, 0, 7825],   # Distances depuis NYC
        [10545, 2078, 4939, 7825, 0]   # Distances depuis HEL
    ]

# Taux d'émission de CO2 par kilomètre (20 grammes par km)
co2 = 0.020

# Initialisation des variables pour la plus petite distance et la meilleure route
smallest = 1000000
bestroute = None

# Fonction de permutation pour trouver toutes les routes possibles
def permutations(route, ports):
    global smallest, bestroute
    if len(ports) < 1:  # Si tous les ports ont été visités
        # Calculer le coût total en CO2 pour cette route
        score = co2 * sum(D[i][j] for i, j in zip(route[:-1], route[1:]))
        # Si ce coût est inférieur à la plus petite valeur trouvée
        if score < smallest:
            smallest = score  # Mise à jour de la plus petite valeur
            bestroute = route  # Mise à jour de la meilleure route
    else:
        # Générer les permutations restantes en ajoutant les ports non visités
        for i in range(len(ports)):
            permutations(route + [ports[i]], ports[:i] + ports[i+1:])

# Fonction principale pour lancer le calcul
def main():
    # Trouver la meilleure route en commençant par le premier port (PAN)
    permutations([0], list(range(1, len(portnames))))

    # Afficher la meilleure route avec le coût total en CO2
    print(' '.join([portnames[i] for i in bestroute]) + " %.1f kg" % smallest)

# Appeler la fonction principale
main()
