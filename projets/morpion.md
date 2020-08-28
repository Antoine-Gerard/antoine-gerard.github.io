---
layout: default
title: Projets
permalink: /projets/morpion
---

{% include mathjax.html %}

# Introduction

L'apprentissage par renforcement est un paradigme de l'apprentissage automatique
dans lequel un **agent** apprend à prendre les bonnes décisions afin de
maximiser sa récompense. C'est notamment grâce à l'apprentissage par
renforcement que nous avons assisté à la première victoire d'un ordinateur face
au champion du monde de Go (voir [ici](https://deepmind.com/research/case-studies/alphago-the-story-so-far)). 

Dans cet article, nous allons apprendre les bases de l'apprentissage par
renforcement en nous concentrant sur un exemple: le jeu du morpion. Pour cela
nous allons diviser la tâche en quatre parties: 

1. Définition du jeu
2. Définition d'un **agent** (joueur de morpion)
3. Entrainement de deux agents jouant l'un contre l'autre
4. Évaluation des résultats

# 1. Définition du jeu de morpion

Dans cette partie, nous allons définir une classe permettant de garder les
informations sur le jeu en cours. Cette classe aura les objectifs suivants:

* Initialiser le plateau de jeu
* Réinitialiser le plateau après une partie finie
* Décider quand une partie est finie: savoir quand un joueur a gagné, ou quand
il n'existe plus de mouvements possibles (match nul)
* Garder en mémoire la liste des mouvements encore possibles sur la partie en
cours. 
* Définir la récompense à donner à chaque joueur. Nous verrons un peu plus loin
ce qu'est exactement la récompense qui est un concept important dans
l'apprentissage par renforcement. 

Voici alors le code source pour la classe `Morpion`:

```python
import numpy as np
import random
import itertools
from MorpionPlayer import MorpionPlayer

class Morpion():

    def __init__(self, p1, p2):
        self.board = np.array([['-']*3]*3)
        self.symbols = {0: '-', 1: 'x', 2:'o'}

        self.available_actions = \
            list(itertools.product(range(0,3), range(0,3)))

        self.p1 = p1
        self.p2 = p2

    def get_combinations(self):
        """
        Compute all combinations in the current tic-tac-toe board. 

        :returns: All combinations
        :rtype: list(str)

        """

        # Line combinations
        combinations = ["".join(row) for row in self.board]

        # Column combinations
        combinations.extend(["".join(row) for row in self.board.transpose()])

        # Diagonal combinations
        combinations.extend(["".join(row) \
                             for row in [self.board.diagonal(),
                                         np.fliplr(self.board).diagonal()]])
        return combinations
        
    def is_finished(self):
        combinations = self.get_combinations()

        if ((np.all(self.board != '-'))
            or ('xxx' in combinations)
            or ('ooo' in combinations)):
            return True
        return False

    def reset(self):
        self.board = np.array([['-']*3]*3)
        self.available_actions = \
            list(itertools.product(range(0,3), range(0,3)))

        return self.board

    def display(self):
        print(self.board)

    def step(self, action, player):

        # Play action
        self.board[action] = self.symbols[player]

        # Hash the new board
        sp = MorpionPlayer.hash_board(self.board)

        # Remove current action from available actions
        self.available_actions.remove(action)
        
        return sp
    def give_reward(self, train=True):
        combinations = self.get_combinations()
        
        if ('xxx' in combinations):
            if (train):
                self.p1.feed_reward(1)
                self.p2.feed_reward(-1)
                
            self.p1.win += 1
            self.p2.lose +=1
        elif ('ooo' in combinations):
            if (train):
                self.p1.feed_reward(-1)
                self.p2.feed_reward(1)

            self.p1.lose += 1
            self.p2.win += 1
        else:
            if (train):
                self.p1.feed_reward(0.1)
                self.p2.feed_reward(0.1)

            self.p1.draw += 1
            self.p2.draw += 1
```

Comme nous pouvons le voir, nous avons fait le choix d'initialiser un plateau de
jeu sous la forme d'un tableau bidimensionnelle pour lequel nous ferons les
choix suivants: 

* '-' indique une place libre
* 'x' est le symbole du joueur 1
* 'o' est le symbole du joueur 2

Nous avons également initialisé une variable d'instance `available_actions`
gardant en mémoire la liste des mouvements possibles sur la partie en cours. À
l'initialisation de la partie nous avons 9 mouvements possibles (combinaisons
possibles entre les 3 indices de lignes et de colonnes). 

# 2. Définition de l'agent

Avant d'aller plus loin dans le développement de notre jeu de morpion, faisons
une petite introduction aux concepts de l'apprentissage par renforcement. Tout
d'abord introduisons un peu de vocabulaire: 

* **Agent**: L'apprenant et le preneur de décision. Pour notre exemple, l'agent
sera un joueur de morpion. 
* **Environnement**: L'endroit où l'agent prend ces décisions et décide des
actions à prendre. Ici l'environnement est la partie de Morpion "virtuelle". 
* **État**: L'état de l'agent dans l'environnement. Pour notre exemple, les
états sont les configurations du plateau de jeu. 
* **Action**: Ensemble des actions que l'agent peut réaliser. Pour notre
exemple, l'ensemble des mouvements possibles. 
* **Récompense**: Pour chaque action entreprise par l'agent, une récompense lui
est attribuée par l'environnement. Généralement, la récompense est une valeur
numérique. 
* **Politique**: Fonction -- généralement notée $\pi$ -- qui à chaque état
préconise une action à effectuer.
* **Fonction valeur**: Attribue une valeur à chaque état représentant ce que
l'agent peut espérer de mieux en moyenne s'il choisit cet état. 

Le but de l'apprentissage par renforcement est d'apprendre par l'expérience une
stratégie "comportementale" (la **politique**) en fonctions des échecs et des
succés rencontrés (les **récompenses**). 

## Exploitation versus Exploration:

Pour expliquer la différence entre **exploitation** et **exploration**, prenons
l'exemple suivant: vous avez décidé d'aller boire une bière avec un.e ami.e après le
travail, et vous cherchez un bar. D'habitude, vous allez dans un bar vendant de
très bonnes bières belges, et vous savez qu'il existe une superbe ambiance dans
ce bar. Cependant, cette fois, votre ami.e vous signifie l'existence d'un
nouveau bar venant d'ouvrir au coin de la rue, et ce bar est bien noté sur
l'Internet. Aucun d'entre vous n'arrive à se décider: devez vous aller dans
votre bar habituel où vous ne serez pas déçu.e, ou devez vous aller dans ce
nouveau bar qui pourrait être mieux, ou alors vous décevoir. 

Dans l'apprentissage par renforcemment, ce dilemme est connu sous le nom de
**compromis exploration-exploitation**. À quel moment devez vous choisir
**d'exploiter** une action qui vous semble la meilleure option plutôt que
**d'explorer** des actions pouvant être meilleures ou pire. Il existe un
algorithme en apprentissage par renforcement permettant de prendre en compte ce
compromis: l'algorithme **$\epsilon$-greedy**. L'algorithme prend en compte le
compromis **exploration-exploitation** de la manière suivante: 

1. Exploration (action choisie au hasard) avec une probabilité $\epsilon$
2. Exploitation ("meilleure" action) le reste du temps. 

Voici alors le code pour la définition de l'agent (`MorpionPlayer.py`): 

```python
import itertools
import random

class MorpionPlayer():
    def __init__(self,
                 is_human,
                 player,
                 trainable = True):

        self.is_human = is_human
        self.player = player
        
        self.history = []
        self.V = {}

        self.win = 0
        self.lose = 0
        self.draw = 0
        
        self.eps = 0.99
        self.greedy_move = 0
        self.move = 0
        self.trainable = trainable

    def reset_stat(self):
        self.win = 0
        self.lose = 0
        self.draw = 0
        self.move = 0
        self.greedy_move = 0
        
    @staticmethod
    def hash_board(board):
        symbol_to_val = {'-': 0, 'x':1, 'o':2}
        
	hash_board = itertools.chain(*list(board))
        hash_board = list(map(lambda s: symbol_to_val[s], hash_board))

        return sum([x * 3**i for i, x in enumerate(hash_board)])

    def greedy_step(self, game):
        available_actions = game.available_actions
        vmax = -1000
        
        for action in available_actions:
            next_board = game.board.copy()
            next_board[action] = game.symbols[self.player]
            next_board_hash = self.hash_board(next_board)
                        
            
            value = 0 if self.V.get(next_board_hash) is None \
                else self.V.get(next_board_hash)
            
            if (value > vmax):
                vmax = value
                v_action = action

        return v_action
                

    def play(self, game):
        available_actions = game.available_actions
        if self.is_human is False:
            if random.uniform(0, 1) < self.eps:
                action = random.choice(available_actions)
                self.move += 1
            else:
                action = self.greedy_step(game)
                self.greedy_move += 1
        else:
            row = int(input("Ligne: "))
            col = int(input("Colonne: "))

            action = (row, col)

        return action

    def add_state(self, sp):
        self.history.append(sp)

    def feed_reward(self, reward):
        if not self.trainable or self.is_human is True:
            return

        for transition in reversed(self.history):
            sp = transition

            if self.V.get(sp) is None:
                self.V[sp] = 0

            self.V[sp] = self.V[sp] + 0.2 * (reward - self.V[sp])
            reward = self.V[sp]
            

        self.history = []
```
