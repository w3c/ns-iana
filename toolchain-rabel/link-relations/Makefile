### Link relations

R ?= /devel/github.com/linkeddata/rabel/rabel.js
C ?= /devel/github.com/linkeddata/swap/cwm.py
OUT=../../out

PHONY: all
all: relation.ttl
	cp index.html relation.ttl ${OUT}/link-relations/
	echo ${PWD}

# touch ,gather to trigger:
#   picking up of the XML file from IANA only if changed there since our copy.
link-relations.xml : ,gather
	curl --time-cond $@ --output $@ http://www.iana.org/assignments/link-relations/link-relations.xml

raw-relation.ttl : check-tools link-relations.xml $R
		$R -format=application/xml -in=link-relations.xml -format=text/turtle -out=raw-relation.ttl

relation.ttl : check-tools raw-relation.ttl infer.n3
		python $C  raw-relation.ttl --think=infer.n3 | sed 's!file://${PWD}!.!' > relation.ttl

# we do not spray link relations, they are just a single file ontology.

# Spray data about anything in this director or a sister directory (people) or below
# into linked data files. THIS CREATES A LOT OF FILES!!
#
# link-relations.spray : relation.ttl $R
#		node $R -in=relation.ttl -spray=${OUT}/ -help

@PHONY:check-tools
check-tools:
	@ [ -x "$R" ] || (echo "\nSet env variable R with path to rabel.js\n"; false)
	@ [ -x "$C" ] || (echo "\nSet env variable C with path to cwm.py\n"; false)
	
