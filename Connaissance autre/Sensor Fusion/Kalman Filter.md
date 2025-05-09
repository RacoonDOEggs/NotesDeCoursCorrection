### Connaissance de base préalable
#### État caché
Un *état caché* représente l'état réel du système qu'on ne peut pas mesurer avec des capteurs. Par exemple, si on calcule la masse d'une personne avec une balance, on obtient plusieurs résultats différent à cause de l'imprécision de la balance. *L'état caché* du système est la masse réelle de la personne. Dans cette exemple, la personne est le *système* et la masse est *l'état du système*.
#### Variance et écart type
La variance représente les écarts entre une différente population et l'écart type représente la racine carré de la variance. L'écart type se note $\sigma$ et la variance $\sigma^2$. On peut estimer la variance sans connaitre la population au complet à l'aide d'un échantillonnage et du [Théorème central limite](../../Collégial/4e%20session/Statistiques/L'estimation.md#Théorème%20central%20limite). Si on calcule la variance d'un échantillon, on ne divise par par le nombre de personne dans l'échantillon mais par le nombre de personne moins un à cause de la [Correction de Bessel](https://en.wikipedia.org/wiki/Bessel%27s_correction).
#### La distribution normale
Encore une fois, la plupart des phénomènes naturels peuvent être représenté par une distribution normal selon le [Théorème central limite](../../Collégial/4e%20session/Statistiques/L'estimation.md#Théorème%20central%20limite). Cette loi normale aura la forme [suivante](../../Collégial/4e%20session/Statistiques/Loi%20normale%20et%20variables%20continues.md#Forme%20générale).

#### Variable aléatoire
L'état caché du système est une *variable aléatoire*, celle-ci est décrit par un fonction de densité de probabilité.
#### Estimation, précision et exactitude
Une *estimation* essaye d'évaluer l'état caché du système. La *précision* est lorsque nos estimés n'ont pas un grande incertitude. L'*exactitude* est lorsque nos estimés sont proches de la valeur réelle. Un système avec une basse exactitude est dit *biaisé*.
### Filtre $\alpha- \beta - \gamma$ à une dimension

La différence entre la mesure et l'état prédit s'appelle *l'innovation*.

Le processus d'estimation avec ce filtre peut être représenté par [ce canvas](Procédé%20d'estimation.canvas).
### FIltre Kalman multidimensionnelle

**Goated video**: https://youtu.be/CaCcOwJPytQ?si=3uGK8-5EqaUreWEU 
#### La matrice d'état
Elle représente l'état actuelle du système, souvent en coordonnées et en vitesse. Elle est souvent représenté comme suit: $$X = 
\begin{bmatrix} 
x \\
y \\
z \\
\dot{x} \\
\dot{y} \\
\dot{z} \\
\end{bmatrix}$$
#### La prédiction de l'état
Pour prédire l'état du système $X_k$, le filtre Kalman utilise un équation: $$X_k=AX_{k-1}+Bu_k+w_k$$
Où A et B sont des matrices déterminées à partir de la dynamique du système, $u_k$ est la matrice de variable de contrôle et $w_k$ du bruit
##### Exemple avec un système une dimension
La matrice d'état est: $$X = 
\begin{bmatrix} 
x \\
\dot{x} \\
\end{bmatrix}$$
On assume que l'objet agit selon un [MRUA](../../Collégial/1ere%20session/Physique/Cinématique.md#MRUA): $$x=x_0+\dot{x}t+\frac{1}{2}\ddot{x}t^2$$
$$\dot{x}=\dot{x_0}+\ddot{x}t$$
Avec cette équation, une forme standard de la matrice $A$ existe: $$A=
\begin{bmatrix}
1 & \Delta t \\
0 & 1 \\
\end{bmatrix}$$
*A est la matrice qui permet de calculer l'effet de l'état précédent sur l'état actuel*. Si on suit le principe de la [multiplication matricielle](../../Collégial/3e%20session/Algèbre%20linéaire/Opérations%20sur%20les%20matrices.md#Multiplication%20de%20matrice), on obtient: $$AX_{k-1}=
\begin{bmatrix}
x+\dot{x}\Delta t \\
\dot{x} \\
\end{bmatrix}$$
L'accélération de l'objet n'est pas gérée par A mais par B.

Dans le cas de cette exemple, la matrice de variable de contrôle $u_k=[a]$ soit l'accélération et la forme standard de B est:  $$B=
\begin{bmatrix}
\frac{1}{2}\Delta t^2 \\
\Delta t \\
\end{bmatrix}$$
*B est la matrice qui permet de calculer l'effet de la matrice de contrôle sur notre état.* On a donc que $$Bu_k=
\begin{bmatrix}
a\frac{1}{2}\Delta t^2 \\
a\Delta t \\
\end{bmatrix}$$Si on résout l'équation de prédiction, on a que: $$X_k=\begin{bmatrix}x\\\dot{x}\end{bmatrix}=\begin{bmatrix}x+\dot{x}\Delta t +\frac{1}{2}a\Delta t^2\\\dot{x}+\ddot{x}\Delta t\end{bmatrix}$$
Cela nous donne la nouvelle position et vitesse de l'objet selon la prédiction. Il faut aussi se fier à l'observation qui est donné par:
$$y_k=Hy_{k_m} + z_k$$
Où $y_{k_m}$ est la mesure du capteur et $H$ permet de transformer cette information dans le même format que notre matrice d'état. $z_k$ est l'erreur dans notre observation (bruit)

#### La matrice de covariance d'état
En d'autres mots, il s'agit de la matrice qui gère l'erreur de notre estimé
- P est la matrice de covariance de notre estimé 
- Q est la matrice de covariance du bruit dans le procédé d'estimation. 
- R est la matrice de covariance de nos mesures
- K est le Kalman gain (comparaison entre l'erreur d'estimation et l'erreur de mesure)

On peut calculer P ainsi:
$$P_k=AP_{k-1}A^T+Q$$
Avec $P_k$, on peut calculer K:
$$K_k=\frac{P_kH^T}{HP_kH^T+R}$$
On voit que si R est grand, on va ignorer les mesures et que inversement si l'erreur sur l'estimation est grand on va ignorer l'estimation.

La matrice H est seulement une matrice de transformation puisque P et K ne sont pas des mêmes dimensions.
$$R = E(z_kz_k^T)$$
$$Q=E(w_kw_k^T)$$
Où $E$ est [l'espérance](../../Collégial/4e%20session/Statistiques/Statistiques%20descriptives%20et%20échantillonnage.md#Tendance%20centrale)
##### Obtention d'une matrice de covariance
La [covariance](https://fr.wikipedia.org/wiki/Covariance) est un concept statistique dérivé de la [variance](../../Collégial/4e%20session/Statistiques/Statistiques%20descriptives%20et%20échantillonnage.md#Mesures%20de%20dispersion) qui évalue la corrélation de la distribution entre deux variables. On note la variance de $X$ $\sigma_x^2$ et la covariance entre $X$ et $Y$ $\sigma_{xy}$.
$$\begin{align}
\sigma_x^2&=\frac{\sum^n_{k=1}{(x_k-\overline{x})^2}}{n} \\
\sigma_{xy}&=\frac{\sum^n_{k=1}{((x_k-\overline{x})(y_k-\overline{y}))}}{n}
\end{align}$$
On peut aussi noter la covariance $\mathrm{cov}(X,Y)$. Une propriété intéressante est que $\mathrm{cov}(X,Y)=\mathrm{cov}(Y,X)$.

Avec cette notation, on défini une matrice de covariance pour trois variables comme suit:
$$Cov =\begin{bmatrix} 
\sigma_{x}^2 & \sigma_{xy} & \sigma_{xz} \\
\sigma_{yx} & \sigma_{y}^2 & \sigma_{yz} \\
\sigma_{zx} & \sigma_{zy} & \sigma_{z}^2
\end{bmatrix}$$
En résumé, les variances des variables sur la diagonale et les covariances (ligne-colonne) ailleurs.

Pour un ordinateur, il est plus simple de faire des opérations matricielles, il y a donc une méthode plus simple pour obtenir la matrice de covariance. On utilise une matrice de déviation $a$:
$$a=X-[1]X\cdot\frac{1}{N}$$
$$Cov = a^Ta\cdot\frac{1}{N}$$
Où $X$ contient les types de données en colonne et les $N$ données associées à ce type en ligne. Aussi $[1]$ est une matrice $N\times N$ remplie de 1.
#### Mise à jour de l'état
Il faut ensuite remettre à jour l'état et l'incertitude sur l'état:
$$X=X_k+K_k(y_k-HX_k)$$
$$P=(I-HK_k)P_k$$
### Filtre Kalman étendu
Il fonctionne selon le même principe que le filtre de Kalman standard mais n'a pas besoin d'un système linéaire. On remplace les matrices A, B et H par une [[matrice jacobienne]].
### Unscented Kalman Filter

