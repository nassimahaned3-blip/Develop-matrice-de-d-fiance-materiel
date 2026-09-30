# Develop-matrice-de-d-fiance-materiel
\documentclass[12pt,a4paper]{article}

\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[french]{babel}
\usepackage{amsmath, amssymb}
\usepackage{geometry}
\usepackage{booktabs}
\usepackage{array}
\usepackage{float}
\usepackage{xcolor}
\usepackage{hyperref}

\geometry{margin=2.5cm}

\title{
    \textbf{Matrice de défaillance matérielle}\\
    \large Version corrigée --- cohérence $P_k$, source réelle pour E1,\\
    \large diagnostic bayésien étendu au scénario toxique
}
\author{
    \textbf{Nassima HANED}\\
    \small Ingénieur en Planification et Statistique
}
\date{\today}

\begin{document}
\maketitle

\noindent\textit{Ce document met à jour la version du 22-09-2026. Il
reste une étude autonome, distincte de la méthode en 9 étapes,
consacrée exclusivement à la matrice de défaillance matérielle
$\lambda_i$ et à son usage diagnostique. Trois corrections
principales par rapport à la version précédente~: (1) résolution
d'une incohérence interne sur $P_{\text{expl}}^{\text{matériel}}$,
(2) remplacement du $\lambda$ générique non sourcé d'E1 par une
valeur HSE PCAG réelle, (3) extension du diagnostic bayésien au
scénario toxique.}

\tableofcontents
\newpage

\section{Principe}

Chaque équipement de la matrice utilise, par défaut, un $\lambda$
générique de catégorie technique (HSE PCAG, OREDA, TNO Purple Book).
Ce $\lambda$ est remplacé par une donnée réelle sourcée dès qu'elle
est trouvée, équipement par équipement --- sans attendre d'avoir
toutes les données pour avancer.

\section{Corrections de $\lambda_i$ effectuées}

\subsection{E12 --- Vannes de régulation}
\begin{table}[H]
\centering
\begin{tabular}{@{}lcc@{}}
\toprule
& Générique OREDA & Réel (exida, 2000 FIT) \\
\midrule
$\lambda$ (/an) & $5\times10^{-3}$ & $0{,}01737$ \\
\bottomrule
\end{tabular}
\end{table}
Écart~: $\times 3{,}47$.

\subsection{E7 --- Compresseur}
\begin{table}[H]
\centering
\begin{tabular}{@{}lcc@{}}
\toprule
& Générique OREDA & Réel (MTBF 83 mois, OREDA 17\,000 unités) \\
\midrule
$\lambda$ (/an) & $1\times10^{-2}$ & $0{,}145$ \\
\bottomrule
\end{tabular}
\end{table}
Écart~: $\times 14{,}5$.

\subsection{E8/E9 --- Pompes NH$_3$ et carbamate}
\begin{table}[H]
\centering
\begin{tabular}{@{}lcc@{}}
\toprule
& Générique (hypothèse initiale) & Réel (Bloch, MTBF chimie 1,5--6 ans) \\
\midrule
$\lambda$ (/an) & $1\times10^{-3}$ & $0{,}267$ (milieu de fourchette) \\
\bottomrule
\end{tabular}
\end{table}
Écart~: $\times 267$. \textit{Réserve~: le carbamate étant fortement
corrosif (jusqu'à 50~mm/an sur inox 316L), la vraie valeur pour E9
est probablement plus proche de la borne basse (1,5~an) que du
milieu retenu ici par prudence.}

\subsection{E10 --- Tuyauterie HP (modèle physique réel établi)}
\textbf{Donnée expérimentale directe~:} un brevet américain rapporte
un essai réel sur tuyauterie 316L (2 pouces) dans les conditions de
boucle de synthèse urée (160~kg/cm$^2$)~: taux de corrosion mesuré
$0{,}3$~mm/an en écoulement normal, montant à $1$~mm/an à haute
vitesse (2--10~m/s, effet érosion-corrosion). Une revue technique
(CRU, 2022) confirme qu'E10 souffre des \textbf{mêmes modes de
défaillance} qu'E1~: corrosion active par perte d'oxygène en phase
liquide, fissuration sous contrainte, corrosion-érosion aux hautes
vitesses.

\textbf{Conséquence structurelle~:} E10 suit le \textbf{même modèle à
trois régimes qu'E1} (section 2.5), pas un phénomène indépendant~:

\begin{table}[H]
\centering
\caption{E10 --- régimes de corrosion (316L UG, tuyauterie HP)}
\begin{tabular}{@{}lcl@{}}
\toprule
Régime & Taux de corrosion & Source \\
\midrule
A --- Normal, passivé & 0,3 mm/an (mesuré, brevet) & Expérimental direct \\
A' --- Haute vitesse (érosion) & 1 mm/an & Expérimental direct \\
B --- Passivation perdue & jusqu'à 50 mm/an & Même mécanisme qu'E1 \\
C --- Liner percé, acier carbone exposé & jusqu'à 500 mm/an & JUBCOR 2024 (HPCC) \\
\bottomrule
\end{tabular}
\end{table}

\textbf{Le $p_f=30\%$ de la version précédente reste invalidé et
n'est pas remplacé par une valeur unique} --- comme pour E1, la
défaillance dépend d'un événement discret (perte de passivation, via
E16) plutôt que d'un taux continu. \textbf{E10 rejoint donc CCF-2}
(maintenance/passivation), aux côtés d'E1, plutôt que d'être traité
isolément.

\textit{Point ouvert restant~: l'épaisseur de paroi réelle et la
surépaisseur de corrosion de la tuyauterie HP du site (nécessaires
pour un calcul Barlow complet de temps-à-rupture) ne sont pas
disponibles --- mais le régime de corrosion lui-même est désormais
réel et sourcé, ce qui constitue une avancée substantielle par
rapport à la valeur invalidée précédente.}

\subsection{E1 --- Réacteur (mécanisme physique affiné)}
Le mécanisme de corrosion carbamate suit en réalité \textbf{trois
régimes distincts}, tous sourcés (JUBCOR 2024, brevets urée)~:

\begin{table}[H]
\centering
\caption{E1 --- régimes de corrosion du revêtement 316L UG}
\begin{tabular}{@{}lcl@{}}
\toprule
Régime & Taux de corrosion & Condition \\
\midrule
A --- Normal, passivé & 0,15--0,3~mm/an & Injection O$_2$ continue maintenue \\
B --- Passivation perdue & jusqu'à 50~mm/an & Défaillance injection O$_2$ (SABIC/JUBCOR 2024) \\
C --- Revêtement percé & jusqu'à 500~mm/an & Acier carbone exposé (JUBCOR 2024, Al-Lafi) \\
\bottomrule
\end{tabular}
\end{table}

\textbf{Découverte importante~:} la perte de passivation (régime B)
n'est pas continue dans le temps --- des retours d'expérience
d'exploitants réels (CF Industries, discussions
Stamicarbon/UreaKnowHow) montrent qu'elle survient presque
systématiquement lors des \textbf{arrêts/démarrages}~: au-delà de
48--72h de blocage de la section synthèse sans écoulement, la couche
de passivation se dégrade et ne peut plus être maintenue. E1 est donc
\textbf{reclassé sous CCF-2 (maintenance)}, pas CCF-1 (corrosion
générique) --- la vraie cause commune est l'exposition aux
transitoires d'arrêt/démarrage, pas seulement le contact avec le
fluide corrosif.

\begin{equation}
P(\text{défaillance E1}) \approx P(\text{perte de passivation}) \times P(\text{non-détection/non-intervention à temps})
\end{equation}
\textit{Point ouvert~: $P(\text{perte de passivation})$ non
quantifiée --- aucune fréquence publique trouvée ; un proxy
raisonnable serait la fréquence des arrêts programmés/non programmés
du site, non disponible à ce stade.}

\textbf{Valeur de repli en attendant~:} le $\lambda_{E1}$ générique
HSE PCAG (Item FR~1.1.4, \textit{Chemical Reactors}) reste utilisé
tant que $P(\text{perte de passivation})$ n'est pas quantifiée~:

\begin{table}[H]
\centering
\caption{$\lambda_{E1}$ --- HSE PCAG, catégorie "réacteurs généraux" (repli)}
\begin{tabular}{@{}lc@{}}
\toprule
Type de défaillance & Taux (par réacteur-an) \\
\midrule
Catastrophique & $1\times10^{-5}$ \\
Trou 50~mm & $5\times10^{-6}$ \\
Trou 25~mm & $5\times10^{-6}$ \\
Trou 13~mm & $1\times10^{-5}$ \\
Trou 6~mm & $4\times10^{-5}$ \\
\midrule
\textbf{Total} & $\mathbf{7\times10^{-5}}$ \\
\bottomrule
\end{tabular}
\end{table}

\subsection{E16 --- Compresseur d'air de passivation (nouvel équipement identifié)}
\textbf{Manque identifié~:} la matrice de 15 équipements ne contient
pas le compresseur d'air/oxygène de passivation, pourtant confirmé
comme un équipement réel et distinct dans les usines urée (sources
UreaKnowHow/CRU) --- séparé du compresseur CO$_2$ (E7). Sa
défaillance est le mécanisme direct de passivation perdue (régime B
d'E1).

\textit{Points ouverts~:} $\lambda_{E16}$ non caractérisé (aucune
donnée de type OREDA/exida trouvée spécifiquement pour ce type de
compresseur).

\textbf{Hypothèse retenue, documentée~:} par analogie avec E7
(compresseur, mécanisme rotatif comparable) :
\begin{equation}
\lambda_{E16}^{\text{hypothèse}} = \lambda_{E7} = 0{,}145\text{/an}
\end{equation}
\textit{Cette valeur est une hypothèse d'analogie, pas une donnée
mesurée pour ce type spécifique de compresseur (généralement plus
petit et moins sollicité qu'un compresseur CO$_2$ de process) ---
probablement conservatrice (surestimée). À corriger si une donnée
réelle devient disponible. Elle ne sert, à ce stade, qu'à documenter
un ordre de grandeur~: $P(\text{perte de passivation})$ reste non
quantifiée tant que le lien précis entre défaillance d'E16 et perte
effective de passivation (délai, redondance éventuelle) n'est pas
établi.}

\section{Correction de cohérence --- $P_k^{\text{matériel}}$}

\subsection{Incohérence identifiée}
La version précédente affichait deux valeurs de
$P_{\text{expl}}^{\text{matériel}}$ non réconciliées~:
\begin{itemize}
    \item $0{,}0847$ --- somme des $\lambda_i$ corrigés, \textbf{sans}
    la correction de composition réelle $c_{\text{mode}}$,
    \item $0{,}00981$ --- même somme, \textbf{avec} $c_{\text{mode}}$
    (dénominateur implicite du tableau de diagnostic bayésien).
\end{itemize}
Écart~: facteur $\approx 8{,}6$, correspondant exactement au facteur
de correction de composition d'E7 (fraction H$_2$ réelle
$\approx 0{,}02\%$ contre une hypothèse implicite de composition
pure).

\subsection{Valeur retenue}
\begin{equation}
\boxed{P_{\text{expl}}^{\text{matériel}} = \sum_i \lambda_i \times c_{\text{mode},i,\text{expl}} \times n_i \approx 0{,}00981}
\end{equation}
C'est la valeur physiquement correcte~: une fuite sans composition
inflammable réelle suffisante ne mène pas au scénario. Le tableau
"impact cumulé" à $0{,}0847$ de la version précédente est
\textbf{obsolète} et ne doit plus être utilisé tel quel.

\textbf{Seconde correction (barrières E14/E15)~:} la section~7
retire en outre E14 et E15 du calcul, ces deux équipements étant des
barrières et non des termes source. La valeur finale, cohérente
avec le diagnostic bayésien corrigé, est~:
\begin{equation}
\boxed{P_{\text{expl}}^{\text{matériel, final}} = 0{,}00981 - 0{,}0033 - 0{,}00033 \approx \mathbf{0{,}00618}}
\end{equation}

\section{Hiérarchisation des causes --- au-delà du seul $\lambda$}

\begin{table}[H]
\centering
\caption{Répartition des causes de défaillance, pompes centrifuges (Bloch, 1996)}
\begin{tabular}{@{}lc@{}}
\toprule
Cause & Part des pannes \\
\midrule
Déficience de maintenance & 30\% \\
Défaut d'assemblage/installation & 25\% \\
Conditions de service hors design & 15\% \\
Erreur d'exploitation & 12\% \\
\midrule
\textbf{Total (causes non aléatoires)} & \textbf{82\%} \\
\bottomrule
\end{tabular}
\end{table}
82\% des pannes de pompes ne sont pas aléatoires --- premier
chiffrage partiel de $X_5$ (facteur organisationnel, méthode en 9
étapes), pour la famille des pompes uniquement.

\section{Diagnostic bayésien inverse}

\subsection{Principe}
\begin{equation}
\boxed{
P(E_i \text{ cause} \mid S_k \text{ observé}) =
\frac{\lambda_i \times c_{\text{mode},i,k} \times n_i}
{\displaystyle\sum_{i'} \lambda_{i'} \times c_{\text{mode},i',k} \times n_{i'}}
}
\end{equation}
Le dénominateur est $P_k^{\text{matériel}}$, cohérent avec la
correction de la section précédente.

\subsection{Scénario explosion (complet --- corrigé)}

\textbf{Correction méthodologique importante~:} la version précédente
incluait E14 (détecteurs) à 33,6\% et implicitement E15 (arrêt
d'urgence) comme termes source. Or ni l'un ni l'autre ne relâchent de
produit --- E14 est un capteur pur, et $\lambda_{E15}$ représente
(convention OREDA/exida retenue) un échec de fermeture à la demande,
pas une fuite de la vanne elle-même. Les deux sont des
\textbf{barrières}, pas des termes source, et leur contribution est
donc ramenée à 0\% --- redistribuée sur les équipements qui relâchent
réellement du produit.

\begin{table}[H]
\centering
\caption{Diagnostic bayésien --- explosion (corrigé)}
\begin{tabular}{@{}lcc@{}}
\toprule
Équipement & $\lambda_i \times c_{\text{mode},i,\text{expl}} \times n_i$ & $P(E_i \mid \text{explosion})$ \\
\midrule
E12 (vannes) & 0,005732 & \textbf{92,8\%} \\
E13 (soupapes --- vrai terme source à l'activation) & 0,000330 & 5,3\% \\
E10 (tuyauterie HP) & 0,000090 & 1,5\% \\
E7 (compresseur) & 0,000029 & 0,47\% \\
E1, E3, E4 & $<0{,}000001$ & $\approx 0{,}05\%$ chacun \\
E2, E5, E6, E8, E9, E11 & $\approx 0$ & $\approx 0\%$ \\
\textbf{E14 (détecteurs)} & --- & \textbf{0\% (barrière, pas de fuite)} \\
\textbf{E15 (arrêt d'urgence)} & --- & \textbf{0\% (échec à la demande, pas de fuite)} \\
\bottomrule
\end{tabular}
\end{table}
E12 domine très largement (92,8\%) --- résultat plus tranché
méthodologiquement que la version initiale (58,4\%), qui confondait
à tort détection/sécurité passive avec relâchement réel de produit.

\subsection{Scénario toxique (en cours --- points ouverts)}

\begin{table}[H]
\centering
\caption{Diagnostic bayésien --- toxique (quasi complet)}
\begin{tabular}{@{}lccl@{}}
\toprule
Équipement & $c_{\text{mode},i,\text{tox}}$ & Contribution & Statut \\
\midrule
E8 (pompe NH$_3$, ×2) & $\approx 1$ (fonction dédiée) & 0,534 & Établi \\
E9 (pompe carbamate, ×2) & 0,436 (stœchiométrie NH$_2$COONH$_4$) & 0,2328 & Établi \\
E3 (décomposeur HP) & 0,436 (même stœchiométrie que E9) & 0,000106 & Estimé \\
E10 (tuyauterie HP) & 0,45 (flux proche réacteur/condenseur) & 0,0000855 & Estimé (repli, $p_f$ invalidé) \\
E4 (décomposeur BP) & 0,40 & 0,000076 & Estimé \\
E2 (condenseur carbamate) & 0,45 & 0,0000855 & Estimé \\
E1 (réacteur) & 0,449 (doc. brevet) & 0,0000314 & Établi (HSE PCAG) \\
E11 (tuyauterie BP) & 0,25 & 0,0000475 & Estimé \\
E5 (évaporateur) & 0,02 (traces résiduelles, source réelle) & 0,000003 & Estimé (négligeable) \\
E6, E7, E13, E15 & 0 (établi ou non pertinent) & $\approx 0$ & Établi \\
E14 (détecteurs) & --- (barrière, jamais terme source) & 0 & Établi \\
E12 (vannes, ×15) & 0,373 (hypothèse répartition 82\%/18\% riche/pauvre) & 0,097 & Estimé --- \textbf{hypothèse assumée} \\
\bottomrule
\end{tabular}
\end{table}

\begin{table}[H]
\centering
\caption{Diagnostic toxique --- posteriors finaux (clôturé)}
\begin{tabular}{@{}lc@{}}
\toprule
Équipement & $P(E_i \mid \text{toxique})$ \\
\midrule
E8 (pompe NH$_3$) & \textbf{61,8\%} \\
E9 (pompe carbamate) & \textbf{26,9\%} \\
E12 (vannes) & \textbf{11,2\%} \\
Reste (E1--E5, E10, E11) & $\approx 0{,}05\%$ (négligeable) \\
\bottomrule
\end{tabular}
\end{table}

E8, E9 et E12 concentrent 99,9\% de la responsabilité probable ---
contraste net avec l'explosion (vannes+détecteurs à 92\%, effet du
nombre d'unités) ~: ici ce sont les équipements en contact direct
avec le NH$_3$/carbamate pur qui dominent, indépendamment de leur
nombre.

\textit{E2, E3, E4, E11 --- base~:} le procédé urée a un profil de
composition NH$_3$/CO$_2$ net~: section synthèse/récupération
(réacteur, décomposeurs, condenseurs) riche en NH$_3$/CO$_2$ libre
par conception (fonction même de ces équipements) ; section
concentration (évaporateur, granulateur) quasi pure en urée, NH$_3$
résiduel seulement en "petites quantités" (sources techniques
Snamprogetti). E3 est le plus solide (même raisonnement
stœchiométrique validé pour E9) ; E2, E4, E11 restent des estimations
qualitatives, pas des données mesurées.

E8 et E9 dominent très largement le diagnostic toxique. Hors E12, ils
pèsent à eux deux environ 99,9\% des contributions établies/estimées
(0,767 sur 0,8642, brut, avant normalisation) --- avec E12 à sa
borne haute (0,2606), leur part descend à environ 75\% --- cohérent
avec le rôle de ces pompes dans le transfert direct de NH$_3$ et
carbamate.

\textit{E9 --- justification~:} le carbamate d'ammonium se dissocie
selon NH$_2$COONH$_4 \to 2\,$NH$_3$ + CO$_2$. Par masse molaire
(78,07~g/mol ; $2\times$NH$_3$ = 34,06~g/mol), la fraction massique
convertie en NH$_3$ libre toxique est $34{,}06/78{,}07 \approx
0{,}436$. Confirmé qualitativement par un accident réel (ARIA, Le
Havre)~: fuite de carbamate sur une vanne de bypass de condenseur de
synthèse, dissociation en NH$_3$+CO$_2$, nuage toxique atteignant la
ville.

\textit{Le dénominateur $P_{\text{tox}}^{\text{matériel}}$ ne peut
pas encore être calculé --- E12 (15 unités, poids probable dominant
comme pour l'explosion) et 8 autres équipements manquent.}

\subsection{Scénario incendie (nouveau)}

\textbf{Choix méthodologique, justifié par la chimie~:} contrairement
au scénario toxique (dominé par NH$_3$/carbamate), le scénario
incendie est dominé par \textbf{H$_2$}, pas par NH$_3$. NH$_3$ a des
limites d'inflammabilité étroites (16--28\% volumique) et une énergie
d'inflammation minimale très élevée (380--680~mJ, contre
$\approx 0{,}02$~mJ pour H$_2$) --- une fiche de sécurité de
référence (CAMEO Chemicals, NOAA) note explicitement que
\textit{"le risque incendie augmente en présence d'huile ou d'autres
matières combustibles"}, NH$_3$ seul brûlant mal sans confinement.
On réutilise donc les $c_{\text{mode},i,\text{expl}}$ (fraction H$_2$)
déjà établis comme base pour $c_{\text{mode},i,\text{inc}}$, avec les
$p_{i,\text{inc}}$ propres à chaque équipement (Étape~4 de la matrice
d'origine).

\begin{table}[H]
\centering
\caption{Diagnostic bayésien --- incendie}
\begin{tabular}{@{}lcc@{}}
\toprule
Équipement & $\lambda_i \times c_{\text{mode},i,\text{inc}} \times n_i$ & $P(E_i \mid \text{incendie})$ \\
\midrule
E12 (vannes) & 0,005732 & \textbf{92,8\%} \\
E13 (soupapes) & 0,000330 & 5,3\% \\
E10 (tuyauterie HP) & 0,000090 & 1,5\% \\
E7 (compresseur) & 0,000029 & 0,47\% \\
E1, E3, E4 & $<0{,}000001$ & $\approx 0{,}05\%$ chacun \\
E14, E15 & --- & 0\% (barrières) \\
\bottomrule
\end{tabular}
\end{table}

\textbf{Limite explicite --- huile de lubrification non quantifiée~:}
E7, E8, E9 (équipements tournants à paliers lubrifiés) ont une
contribution incendie potentiellement plus élevée que ne le montre ce
tableau, via un feu de nappe d'huile (M6, huile lubrifiante, non
incluse dans le calcul H$_2$ ci-dessus) --- \textbf{point ouvert}, la
masse d'huile en circulation par équipement et sa probabilité de fuite
ne sont pas encore caractérisées.

\textit{Résultat identique à l'explosion par construction} (même
$c_{\text{mode}}$ H$_2$ réutilisé) --- une limite du choix
méthodologique, pas une coïncidence physique confirmée
indépendamment.

\section{Diagnostic à deux niveaux --- causes communes puis équipement}

\subsection{Principe --- ordre de la démarche}

Contrairement au diagnostic direct par équipement (section
précédente), la démarche correcte suit deux niveaux, dans cet
ordre~: (1) identifier si une \textbf{cause commune} (groupe CCF) est
responsable, avant même de chercher quel équipement précis ; (2)
seulement si un groupe est identifié, déterminer lequel de ses
membres. C'est la structure qu'un enquêteur suit réellement --- se
demander d'abord si la défaillance est systémique
(maintenance, électrique) ou isolée, pas directement "lequel des 15
équipements ?".

\subsection{Groupes de cause commune réels (E1--E15)}

\begin{table}[H]
\centering
\caption{Groupes CCF --- matrice réelle, distincts de l'exercice illustratif}
\begin{tabular}{@{}llll@{}}
\toprule
Groupe & Cause & Équipements & Justification \\
\midrule
CCF-1 & Corrosion carbamate & E2, E3, E4, E9 & Contact direct
carbamate, corrosion documentée (0,15--50~mm/an selon régime,
316L) \\
CCF-2 & Maintenance/arrêts-démarrages & E7, E8, E9, E1, \textbf{E10} &
Même équipe technique ; \textbf{E10 ajouté} --- même mécanisme à 3
régimes qu'E1 (données expérimentales brevet + CRU 2022), dépendant
de la même passivation \\
CCF-3 & Électrique/instrumentation & E7, E8, E9, E12, E14 & Même
alimentation électrique/pneumatique de contrôle-commande \\
CCF-4 & Utilité vapeur & E3, E4, E5 & Chauffage vapeur commun aux
décomposeurs et évaporateur \\
\bottomrule
\end{tabular}
\end{table}

\textbf{E16 --- statut particulier, pas un membre de groupe~:} E16 ne
relâche pas de produit toxique (air/oxygène uniquement) --- il n'a
donc pas de contribution propre au diagnostic. Son rôle est celui
d'un \textbf{déclencheur} du régime B (perte de passivation)
d'E1 spécifiquement, pas d'un cinquième membre de CCF-2. La
contribution d'E1 se décompose ainsi~:
\begin{equation}
\text{Contribution}_{E1} = \underbrace{\text{Contribution}_{E1}^{\text{régime A}}}_{\lambda_{E1}\text{ HSE PCAG, indépendant}} + \underbrace{\text{Contribution}_{E1}^{\text{régime B, via E16}}}_{\text{conditionnée par défaillance d'E16}}
\end{equation}
E16 apparaît donc en amont d'E1 dans la chaîne causale, jamais comme
terme source parallèle dans la somme du groupe.

\subsection{Niveau 1 --- diagnostic de groupe (scénario toxique)}

\begin{equation}
P(\text{CCF-}w \mid \text{Incident}) = \frac{P(\text{CCF-}w)}{P(\text{Incident})}
\end{equation}
avec $P(\text{CCF-}w) = \beta_w \times \sum_{i \in w} n_i \lambda_i c_{\text{mode},i}$
(méthode BFM modifiée --- sans double comptage même si un équipement
appartient à plusieurs groupes).

\begin{table}[H]
\centering
\caption{Diagnostic de groupe --- toxique}
\begin{tabular}{@{}lc@{}}
\toprule
Cause (niveau 1) & $P(\text{cause} \mid \text{toxique})$ \\
\midrule
Aucune cause commune (indépendant) & \textbf{74,0\%} \\
\textbf{CCF-2 --- Maintenance équip. tournants} & \textbf{13,3\%} \\
CCF-3 --- Électrique/instrumentation & 10,0\% \\
CCF-1 --- Corrosion carbamate & 2,7\% \\
CCF-4 --- Utilité vapeur & $\approx 0\%$ \\
\bottomrule
\end{tabular}
\end{table}

\textbf{Convergence avec Bloch~:} CCF-2 (maintenance) domine les
causes communes --- cohérent indépendamment avec la hiérarchisation
Bloch (section~4, 82\% de causes non aléatoires pour les pompes).
Deux analyses distinctes, à des moments différents, pointent vers la
même conclusion --- signal de cohérence interne pour la publication.

\subsection{Niveau 2 --- équipement, conditionnel au groupe identifié}

\textbf{Si CCF-2 (maintenance) est la cause identifiée}~:
\begin{equation}
P(E_i \mid \text{CCF-2}, \text{toxique}) = \frac{n_i \lambda_i c_{\text{mode},i}}{\sum_{j \in \text{CCF-2}} n_j \lambda_j c_{\text{mode},j}}
\end{equation}

\begin{table}[H]
\centering
\caption{Niveau 2 --- au sein de CCF-2}
\begin{tabular}{@{}lc@{}}
\toprule
Équipement & $P(E_i \mid \text{CCF-2})$ \\
\midrule
E9 (pompe carbamate) & $\approx 68\%$ \\
E8 (pompe NH$_3$) & $\approx 31\%$ \\
E7 (compresseur) & $\approx 1\%$ \\
\bottomrule
\end{tabular}
\end{table}

\textbf{Si "indépendant" est la cause} (74\% des cas)~: on retombe
sur le diagnostic direct par équipement déjà établi (section
précédente, E8 dominant).

\subsection{Niveau 1 --- diagnostic de groupe (explosion et incendie)}

Même méthode (BFM modifié, sans double comptage). E12 dominant très
largement les deux scénarios (92,8\%, \S 6.2 et 6.4) et étant membre
de CCF-3 (électrique/instrumentation) uniquement, le résultat de
groupe est direct~:

\begin{table}[H]
\centering
\caption{Diagnostic de groupe --- explosion et incendie}
\begin{tabular}{@{}lcc@{}}
\toprule
Cause (niveau 1) & Explosion & Incendie \\
\midrule
\textbf{CCF-3 (électrique, via E12)} & \textbf{$\approx 9{,}3\%$} & \textbf{$\approx 9{,}3\%$} \\
Indépendant (reste) & $\approx 90{,}7\%$ & $\approx 90{,}7\%$ \\
\bottomrule
\end{tabular}
\end{table}
\textit{Avec $\beta_{\text{CCF-3}}=0{,}10$ (valeur de littérature déjà
retenue) --- ordre de grandeur seulement, pas une valeur mesurée sur
ce site. Résultat identique explosion/incendie par construction
(même $c_{\text{mode}}$ H$_2$ réutilisé, \S 6.4) --- limite déjà
signalée, pas une confirmation indépendante.}

\textbf{Niveau 2, si CCF-3 identifié~:} au sein d'E7, E8, E9, E12,
E14 --- E12 y reste très largement dominant, E14 restant à 0\%
(barrière, pas de terme source).

\section{Points ouverts --- état final}
\begin{itemize}
    \item \textbf{E12 (répartition vannes)} --- hypothèse assumée
    (82\%/18\% riche/pauvre), pas une donnée P\&ID réelle.
    \item \textbf{E10 (épaisseur/surépaisseur de site)} --- régime de
    corrosion désormais réel et sourcé (\S 2.4), mais le calcul
    Barlow complet de temps-à-rupture attend l'épaisseur de paroi
    réelle du site.
    \item \textbf{$P(\text{perte de passivation})$, via E16} ---
    mécanisme établi et sourcé, mais non quantifié (aucune fréquence
    publique) ; $\lambda_{E16}$ fixé par analogie E7, à valider.
    \item \textbf{Huile de lubrification (M6)} --- contribution
    incendie potentielle d'E7/E8/E9 via feu de nappe, non quantifiée.
    \item \textbf{Diagnostic à deux niveaux explosion/incendie}
    (\S 7.5) --- fait à l'ordre de grandeur, pas raffiné comme le
    niveau toxique.
    \item Catégorie HSE PCAG d'E1 (réacteurs généraux vs
    emballement thermique) --- jugement procédé à trancher.
\end{itemize}

\section{Limites explicites}
Ce diagnostic suppose que la matrice $\lambda_i \times
c_{\text{mode},i,k} \times n_i$ est correcte et à jour --- toute
composition mal caractérisée fausse directement le diagnostic pour
l'équipement concerné, comme l'a montré la correction de la
section~3.

\end{document}
