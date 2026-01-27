# Commentaires du modèle conceptuel

Ce document contient le commentaire, avec exemples si nécessaire, des éléments du modèle conceptuel.

## Achievement 
Un accomplissement fait référence à tout acte réalisé par une personne et ayant été mobilisé comme justification à une nomination pour le prix Nobel de la paix. 

### Propriétés
Pour tout accomplissement sont indiqués sa définition, permettant de l'expliciter et la date à laquelle il a été réalisé.

### Relations
Un accomplissement est immanquablement lié à la personne qui en est à l'origine ainsi qu'à la nomination pour laquelle il est mobilisé comme justificatif. 

Chaque accomplissement fait donc référence à une personne par un lien de clef secondaire fk_person.

## Activity 
La table activity permet de lier la table person à la table field of activity. Elle relie donc une personne à un ou plusieurs domaines d'activité.

## Field of activity
Fait référence à un domaine d'activité de manière large dans lequel est active une personne ou une institution. 
Ceci peut-être par exemple l'aide aux victimes, le combat pour les droits civiques, la promotion de la paix, etc.

### Propriétés
Pour chaque domaine d'activité sont renseignés:
- son nom 
- sa définition

### Relations
Un domaine d'activité est excercé par une personne ou une institution. Le lien est dans les deux cas réalisés au travers d'une table "pursuit" et "activity", indiquant à chaque fois:
- les clefs secondaires (personne ou institution et champ d'activité),
- les dates de début et de fin si précisées et pertinentes,
- une définition si pertinente.

## Geographical place 
Un endroit géographique fait référence à tout lieu disposant d'un statut juridique au niveau national ou international. Sont donc exclus les continents, dont les questions associées à leur délimination ne sont pas réglées de manière juridique et pouvant entraîner des biais dans l'analyse. 

### Propriétés
Cette table fait la distinction entre des lieux de différentes échelles. Ainsi, les colonnes permettent de renseigner le nom d'un lieu, l'État dans lequel il se trouve et de manière analogue son continent généralement reconnu. 

Pour chaque lieu géographique sont renseignés, si pertinent:
- son nom officiel - ou reconnu par l'État fédéral suisse si contestation sur le nom il y a. 
- sa définition telle que présentée par Wikipédia. 

### Relations
Un lieu géographique, dans le cadre de ce travail, peut entretenir des relations avec une personne ou une institution.

#### Lieu géographique - institution
Fait référence à l'endroit abritant le siègle principal et reconnu comme tel par Wikipédia d'une institution. 

## Institution
Désigne toute communauté reconnue légalement au niveau national ou international. Une institution peut de ce fait désigner entre autres un établissement universitaire, une organisation internationale, une organisation non-gouvernementale ou encore un organe politique reconnu au niveau national ou sub-national. 

### Propriétés
Sont renseignées pour toute institution les informations suivantes, si disponibles:
- sa désignation officielle (name)
- sa définition, faisant référence à la brève définition donnée par Wikipédia. 
- ses dates de création et de fin si pertinentes
- son statut légal, qui peut être Organisation non-gouvernementale, organisation internationale, etc.

### Relations 
Une institution peut être liée à une personne, un lieu géographique, une formation, une occupation ou un domaine d'activité:

#### Institution - personne 
Une relation personne-institution est exprimée par une table supplémentaire "membership", renseignant:
- les clefs secondaires correspondantes (personne et institution)
- les dates de début et de fin 

#### Institution - lieu géographique
Cette relation fait référence à l'endroit géographique ou est situé le siège d'une institution.
Le lien est renseigné par une clef secondaire fk_headquarters au sein de la table institution.

#### Institution - formation
Une relation institution - formation désigne le fait qu'une formation se déroule dans une institution. Cette relation est précisée dans la table "training" par une clef secondaire. 

#### Institution - occupation
Cette relation fait référence au fait  qu'une occupation à la nomination est généralement réalisée dans le cadre d'une structure institutionnelle. Elle lie donc l'occupation - le métier, si l'on veut bien - à l'institution dans lequel elle est exercée. 
Cette relation est exprimée par une clef secondaire dans la table occupation at nomination.

#### Institution - domaine d'activité
Cette relation peut faire référence au domaine d'activité dans lequel une institution peut être active, notamment dans le cas d'organisations internationales et non-gouvernementales. Elle est exprimée par la table "activity".

## Membership
Désigne l'appartenance d'une personne à une institution. 
- cf. Institution

## Nationality
Désigne le lien juridique liant une personne à un endroit géographique, généralement un État tel que définit par le droit international. 

La table nationality permet de lier une personne à un ou plusieurs lieux géographiques.

## Nomination
Fait référence à l'acte par lequel une personne nomine ou est nominée dans le cadre du prix Nobel de la paix. 

### Propriétés
Pour toute nomination sont renseignés:
- sa définition, sous-entendu sa qualité de nomination individuelle ou collective, une nomination collective faisant référence à une nomination issue par plusieurs personnes. 
- la date à laquelle elle a été émise, si disponible.
- la motivation au nom de laquelle elle a été émise. (cf. accomplishment) 

### Relations
Une nomination est attribuée à un accomplissement par une clef secondaire fk_nomination.

#### Nomination - person 
Cette relation est exprimée par la table supplémentaire "role". Y sont spécifiés la date à laquelle la nomination a été émise et la définition du rôle: nominant ou nominé. 

## Occupation at nomination
L'occupation à la nomination désigne l'activité professionnelle qu'exerçait la personne à la date de la nomination. 

### Propriétés
Pour toute occupation est indiqué l'intitulé du poste occupé par la personne et la date depuis laquelle elle occupe ledit poste.

### Relations
L'occupation est irrémédiablement liée à la personne à laquelle elle se réfère ainsi qu'à l'institution au sein de laquelle elle est exercée. 

## Person
Fait référence à tout être humain ayant été nominé ou ayant nominé dans le cadre du prix Nobel de la paix, depuis 1901. 

### Propriétés
Pour toute personne sont précisés:
- Son nom, indiqué sous le format "prénoms, nom de famille" et désignant le nom par lequel il est communément désigné. Ainsi, Henry Dunant est désigné comme tel: "Dunant, Henry" et non par son nom de naissance. 
- Son statut: nominateur, nominé ou lauréat, en fonction de si la nomination a abouti de manière positive ou non. 
- Son âge à la première nomination, dans le cas où un individu a été nominé plusieurs années, ce qui est parfois le cas. Sinon, l'âge à la nomination ayant débouché sur un prix Nobel est indiqué. Dans le cas du nominateur, l'âge à la nomination n'a pas été retenu comme donnée importante dans le cadre de cette analyse.
- Ses dates de naissance et de décès. 

## Political affiliation
Désigne l'appartenance politique, si elle existe et est assumée et précisée, d'une personne. 

Il peut s'agir d'une affiliation directe à un parti politique, une appartenance philosophique ou théorique à une mouvance politique. Ce travail s'intéressant à une période où les formes étatiques étaient variées, l'affiliation politique peut aussi faire référence à l'appartenance à un mouvement de pensée n'étant pas caractérisé comme un parti au sens actuel du terme. 

Cette table lie les tables person et political party.

## Political party
Un parti politique s'entend d'un gorupe de personnes possédant des idées politiques communes réunies en association ayant un statut légal au niveau national. 

### Propriétés
Sont renseignés le nom officiel de l'affiliation politique ainsi que sa définition, dans laquelle sont précisés si nécessaire le pays auquel elle est rattachée et d'autres informations pertinentes.  

Si pertinent, le siège du parti politique est renseigné par une clef secondaire pointant sur un lieu géographique.

### Relation
L'affiliation politique est reliée si nécessaire au parti politique, pour lequel sont précisés son nom officiel et son orientation politique (gauche-droite).

## Pursuit
Fait référence à la relation liant une personne et un champ d'activité.
- cf. Field of activity

## Role
cf. Nomination

## Training
Fait référence à tout diplôme obtenu par une personne, tous niveaux d'études confondus. 

### Propriétés
Pour toute formation sont précisés:
-  Sa définition, contenant les informations pertinentes 
- la date à laquelle la formation a été débutée ou achevée, en fonction de la disponibilité des informations. 

### Relations
Une formation est liée à une institution et à une personne, partant du principe que cette dernière s'étant formée dans une institution. 