# Commentaires du modèle conceptuel

Ce document contient le commentaire, avec exemples si nécessaire, des éléments du modèle conceptuel.

## Field of activity
Fait référence à un domaine d'activité de manière large dans lequel est active une personne ou une institution. 

### Propriétés
Pour chaque domaine d'activité sont renseignés:
- son nom 
- sa définition

### Relations
Un domaine d'activité est excercé par une personne ou une institution. Le lien est dans les deux cas réalisés au travers d'une table "pursuit", indiquant à chaque fois:
- les clefs secondaires (personne ou institution et champ d'activité)
- les dates de début et de fin
- une définition si pertinent

## Geographical place 
Un endroit géographique fait référence à tout lieu disposant d'un statut juridique au niveau national ou international. Sont donc exclus les continents, dont les questions associées à leur délimination ne sont pas réglées de manière juridique et pouvant entraîner des biais dans l'analyse. 

### Propriétés
Pour chaque lieu géographique sont renseignés:
- son nom officiel - ou reconnu par l'État fédéral suisse si contestation sur le nom il y a. 
- sa définition telle que présentée par Wikipédia. 

### Relations
Un lieu géographique, dans le cadre de ce travail, peut entretenir des relations avec une personne ou une institution.

#### Lieu géographique - institution
Fait référence à l'endroit habritant le siègle principal et reconnu comme tel par Wikipédia d'une institution. 

#### Lieu géographique - personne
Dans ce cadre-ci, le lien est triple:
- Lieu de naissance 
- Lieu de décès
- Question de nationalités: pour ce dernier cas, la nationalité est exprimée par une table intermédiaire, dans laquelle sont renseignées les clefs étrangères (personne et lieu géographique) ainsi que la définition si nécessaire. 

## Institution
Désigne toute communauté reconnue légalement au niveau national ou international. Une institution peut de se fait désigner entre autres un établissement universitaire, une organisation internationale, une organisation non-gouvernementale ou encore un organe politique reconnu au niveau national ou sub-national. 

### Propriétés
Sont renseignées pour toute institution les informations suivantes, si disponibles:
- sa désignation officielle (name)
- sa définition, faisant référence à la brève définition donnée par Wikipédia. 
- ses dates de création et de fin 
- son statut légal

### Relations 
Une institution peut être liée à une personne, un lieu géographique, une formation, une occupation ou un domaine d'activité:

#### Institution - personne 
Une relation personne-institution est exprimée par une table supplémentaire "membership", renseignant:
- les clefs secondaires correspondantes (personne et institution)
- les dates de début et de fin 

#### Institution - lieu géographique
Cette relation fait référence à l'endroit géographique ou est situé le siège d'une institution.

#### Institution - formation
Une relation institution - formation désigne le fait qu'une formation se déroule dans une institution. Il n'a pas été jugé pertinent ou nécessaire de réaliser une table supplémentaire pour désigner cette relation. 

#### Institution - occupation
Cette relation fait référence au fait  qu'une occupation est généralement réalisée dans le cadre d'une structure institutionnelle. Elle lie donc l'occupation - le métier, si l'on veut bien - à l'institution dans lequel elle est exercée. 

#### Institution - domaine d'activité
Cette relation peut faire référence au domaine d'activité dans lequel une institution peut être active, notamment dans le cas d'organisations internationales et non-gouvernementales. 

## Membership
Désigne l'appartenance d'une personne à une institution. 
- cf. Institution

## Nationality
Désigne le lien juridique liant une personne à un endroit géographique, généralement un État tel que définit par le droit international. 
- cf. Geographical place

## Person
Fait référence à tout être humain ayant été nominé ou ayant nominé dans le cadre du prix Nobel de la paix, depuis 1901. 


### Propriétés
Pour toute personne sont précisés:
- Son nom, indiqué sous le format "nom de famille, prénoms" et désignant le nom par lequel il est communément désigné. Ainsi, Henry Dunant est désigné comme tel: "Dunant, Henry" et non par son nom de naissance. 
- Son statut: nominé ou lauréat, en fonction de si la nomination a abouti de manière positive ou non. 
- Son âge à la première nomination, dans le cas où un individu a été nominé plusieurs années, ce qui est parfois le cas.
- Ses dates de naissance et de décès. 

### Relations
En tant qu'élément central de cette analyse, une personne entretient des relations avec la majorité des autres classes de ce modèle. Par conséquent, elles ne seront pas récapitulées dans cette section, mais peuvent être trouvées dans les sections correspondantes. 

## Pursuit
Fait référence à la relation liant une personne et un champ d'activité.
- cf. Field of activity

## Training
Fait référence à tout diplôme obtenu par une personne, tous niveaux d'études confondus. 

### Propriétés
Pour toute formation sont précisés:
-  Sa définition, contenant les informations pertinentes 
- la date à laquelle la formation a été débutée ou achevée, en fonction de la disponibilité des informations. 

### Relations
Une formation est liée à une institution et à une personne, partant du principe que cette dernière s'étant formée dans une institution. 

---

## Political affiliation
Désigne l'appartenance politique, si elle existe et est assumée et précisée, d'une personne. 

Il peut s'agir d'une affiliation directe à un parti politique, une appartenance philosophique ou théorique à une mouvance politique. Ce travail s'intéressant à une période où les formes étatiques étaient variées, l'affiliation politique peut aussi faire référence à l'appartenance à un mouvement de pensée n'étant pas caractérisé comme un parti au sens actuel du terme. 

### Propriétés
Sont renseignés le nom officiel de l'affiliation politique ainsi que sa définition, dans laquelle sont précisés si nécessaire le pays auquel elle est rattachée et d'autres informations pertinentes. 

## Nomination
Désigne l'acte par lequel une personne atteint le statut de nominé ou celui par lequel une personne atteint le statut de nominant. 

### Propriétés
Pour toute nomination sont renseignés:
- sa définition, sous-entendu sa qualité de nomination individuelle ou collective, une nomination collective faisant référence à une nomination issue par plusieurs personnes. 
- la date à laquelle elle a été émise, si disponible.
- la motivation au nom de laquelle elle a été émise. 

## Occupation à la nomination
L'occupation à la nomination désigne l'activité professionnelle qu'exerçait la personne à la date de la nomination. 

### Propriétés
Pour toute occupation est indiqué l'intitulé du poste occupé par la personne et la date depuis laquelle elle occupe ledit poste.